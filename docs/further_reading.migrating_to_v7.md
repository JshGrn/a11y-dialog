---
title: Migrating to v7
slug: /migrating-to-v7
---

Versions 6 and 7 represent a significant gap in the way a11y-dialog should be use. Mainly, the [expected HTML](./usage.markup.md) is quite different (for the better), which also has an impact of the styles. The JavaScript API remains untouched.

## No more `<dialog>`

The support for the native `<dialog>` HTML element has been discontinued. The rational behind this choice is explained more in detail in the page [about the `<dialog>` element](./advanced.dialog_element.md).

Before migrating to v7, it is recommended you update your usage to no longer use the `<dialog>` element as this will make the actual migration easier. To do so, replace any usage of the `<dialog>` element with a `<div role="dialog">`.

```diff
- <dialog>
+ <div role="dialog">
```

The styles will also need to be updated, as you will no longer benefit from the native display handling from the `<dialog>` element. Refer to the [styling section](./usage.styling.md) and the [demo](https://codesandbox.io/s/a11y-dialog-cp3rz) for a set of styles to get you started.

## New markup

The main difference between v6 and v7 is that a lot of the logic moved onto the container instead of the dialog itself.

- The `aria-labelledby` attribute move from the former dialog element (or its `<div>` equivalent) to the container.
- If wanting an [alert dialog](./advanced.alert_dialog.md), the `role="alertdialog"` attribute should be applied to the container.
- The backdrop no longers needs the `tabindex="-1"` attribute.
- The dialog element itself no longer exists. Its inner `<div>` with the `role="document"` are the new dialog. This is what should be styled as such.

```diff
<div
  id="your-dialog-id"
  aria-hidden="true"
+ aria-labelledby="your-dialog-title-id"
  >
  <div
    data-a11y-dialog-hide
-   tabindex="-1"
  ></div>
- <div role="dialog" aria-labelledby="your-dialog-title-id">
    <div role="document">
      <button type="button" data-a11y-dialog-hide aria-label="Close dialog">
        &times;
      </button>
      <h1 id="your-dialog-title-id">Your dialog title</h1>
      <!-- Your content here -->
    </div>
- </div>
</div>
```

If you are not using JavaScript to instantiate your dialog and are relying on the `data-a11y-dialog` attribute for [automatic instantiation on script load](./usage.instantiation.md), its value is now the unique name for the dialog, just like an `id`.

```diff
<div
- id="your-dialog-id"
- data-a11y-dialog="#root"
+ data-a11y-dialog="your-dialog-id"
  aria-hidden="true"
  >
```
