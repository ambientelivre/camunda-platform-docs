---
id: call-activities
title: "Call activities"
---

A call activity (aka reusable subprocess) allows to call/invoke another workflow as part of this workflow. It is similar to an [embedded subprocess](../embedded-subprocesses/embedded-subprocesses.md) but the workflow is externalized (i.e. stored as separated BPMN) and can be invoked by different workflows.

![call-activity](assets/call-activities-example.png)

When a call activity is entered then a new workflow instance of the referenced workflow is created. The new workflow instance gets activated at the **none start event**. The workflow can have start events of other types but they are ignored.

When the created workflow instance is completed then the call activity is left and the outgoing sequence flow is taken.

## Defining the called workflow

A call activity must define the BPMN process id of the called workflow as `processId`.

The new instance of the defined workflow will be created of its **latest version** - at the point when the call activity is activated.

Usually, the `processId` is defined as a static value (e.g. `shipping-process`) but it can also be defined as [expression](/components/concepts/expressions.md) (e.g. `= "shipping-" + tenantId`). The expression is evaluated on activating the call activity and must result in a `string`.

## Boundary events

![call-activity-boundary-event](assets/call-activities-boundary-events.png)

Interrupting and non-interrupting boundary events can be attached to a call activity.

When an interrupting boundary event is triggered then the call activity **and** the created workflow instance are terminated. The variables of the created workflow instance are not propagated to the call activity.

When an non-interrupting boundary event is triggered then the created workflow instance is not affected. The activities at the outgoing path have no access to the variables of the created workflow instance since they are bounded to the other workflow instance.

## Variable mappings

When the call activity is activated then **all variables** of the call activity scope are copied to the created workflow instance.

Input mappings can be used to create new local variables in the scope of the call activity. These variables are also copied to the created workflow instance.

If the attribute `propagateAllChildVariables` is set (default: `true`) then all variables of the created workflow instance are propagated to the call activity. This behavior can be customized by defining output mappings at the call activity. The output mappings are applied on completing the call activity and only those variables that are defined in the output mappings are propagated.

It is recommended to disable the attribute `propagateAllChildVariables` or define output mappings if the call activity is in a parallel flow (e.g. when it is marked as [parallel multi-instance](../multi-instance/multi-instance.md#variable-mappings)). Otherwise, it can happen that variables are overridden accidentally when they are changed in the parallel flow.

## Additional resources

<details>
  <summary>XML representation</summary>
  <p>A call activity with static process id:

```xml
<bpmn:callActivity id="task-A" name="A">
  <bpmn:extensionElements>
    <zeebe:calledElement processId="child-process-id" />
  </bpmn:extensionElements>
</bpmn:callActivity>
```

  </p>
</details>

<details>
  <summary>Using the BPMN modeler</summary>
  <p>Adding a call activity with static process id:

![call-activity](assets/bpmn-modeler-call-activity.gif)

  </p>
</details>

<details>
  <summary>Workflow lifecycle</summary>
  <p>Workflow instance records of a call activity:

<table>
    <tr>
        <th>Intent</th>
        <th>Element Id</th>
        <th>Element Type</th>
    </tr>
    <tr>
        <td>ELEMENT_ACTIVATING</td>
        <td>task-a</td>
        <td>CALL_ACTIVITY</td>
    </tr>
    <tr>
        <td>ELEMENT_ACTIVATED</td>
        <td>task-a</td>
        <td>CALL_ACTIVITY</td>
    </tr>
    <tr>
        <td>ELEMENT_ACTIVATING</td>
        <td>child-process-id</td>
        <td>PROCESS</td>
    </tr>
    <tr>
        <td>ELEMENT_ACTIVATED</td>
        <td>child-process-id</td>
        <td>PROCESS</td>
    </tr>
    <tr>
        <td>...</td>
        <td>...</td>
        <td>...</td>
    </tr>
    <tr>
        <td>ELEMENT_COMPLETED</td>
        <td>child-process-id</td>
        <td>PROCESS</td>
    </tr>
    <tr>
        <td>ELEMENT_COMPLETING</td>
        <td>task-a</td>
        <td>CALL_ACTIVITY</td>
    </tr>
    <tr>
        <td>ELEMENT_COMPLETED</td>
        <td>task-a</td>
        <td>CALL_ACTIVITY</td>
    </tr>
</table>

The workflow instance records of the created workflow instance have a reference to its parent workflow instance (`parentWorkflowInstanceKey`) and the element instance of the call activity (`parentElementInstanceKey`).

  </p>
</details>

References:

- [Expressions](/components/concepts/expressions.md)
- [Variable scopes](/components/concepts/variables.md#variable-scopes)
- [Variable mappings](/components/concepts/variables.md#inputoutput-variable-mappings)
