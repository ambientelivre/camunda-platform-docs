---
id: using-templates
title: Using templates
description: "Learn how to apply, remove, update, and replace templates."
---

## Applying templates

If a template matches a selected diagram element, the template catalog button will be shown in the properties panel.

![Template Chooser](./img/chooser.png)

Clicking **Catalog** opens a modal menu, allowing the user to browse and search available templates for the
selected element.

![Modal Menu](./img/modal.png)

Applying a template stores it via the `modelerTemplate` property and the optional `modelerTemplateVersion` property
on the selected element:

Camunda Platform 7:

```xml

<bpmn:serviceTask id="MailTask"
                  camunda:modelerTemplate="com.mycompany.MailTask"
                  camunda:modelerTemplateVesion="1"/>
```

Camunda Platform 8:

```xml

<bpmn:serviceTask id="MailTask"
                  zeebe:modelerTemplate="com.mycompany.MailTask"
                  zeebe:modelerTemplateVesion="1"/>
```

It also sets up custom fields on the diagram element and make these available to the user for inspection and editing.
Properties which were not configured in the element template using custom fields will not be available for editing for
the user.

## Removing templates

To remove an applied template from an element, either the _Unlink_ or _Remove_ function can be used:

- _Remove_: Remove the element template from the `modelerTemplate` property and reset all properties of the respective element.
- _Unlink_: Remove the element template from the `modelerTemplate` property but keep the properties which were set.

![Unlink or Remove](./img/unlink-remove.png)

## Updating templates

If a template is applied and a new version of the template is found you can _update_ the template.

![Update Template](./img/update-template.png)

Templates are updated according to the following rules:

1. If the property is set in new template, it will override unless the property was set by the old template and changed afterwards.
2. If the property is not defined in the new template, it will unset.
3. Sub-properties of complex properties (e.g. camunda:In, camunda:Out, camunda:ExecutionListener) are handled
   according to these rules if they can be identified.

### Replacing templates

If a template is deprecated with a new element template and you want to keep the same input values as in the
deprecated template, you can:

- _Unlink_: Remove the current template that is deprecated from the `modelerTemplate` property, but keep the properties
  which
  were set.
- _Select_ and apply the new element template.

### Missing templates

If a template is applied to an element but the respective template cannot be found on the system, the editing of the
element is disabled. _Unlinking_ or _removing_ the template for the element or adding the element template config
enables the editing again.

![Template not Found](./img/template-not-found.png)
