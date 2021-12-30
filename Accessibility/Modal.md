
# Modals

## Example

```html
<div id="dialog" role="dialog" aria-labelledby="title" aria-describedby="description" aria-modal="true">
  <h1 id="title">Title of the dialog</h1>
  <p id="description">Information provided by the dialog.</p>
  <button id="close" aria-label="close">×</button>
</div>
```

## Key accessibility points
- role = “dialog”
- aria-labelledby=”modal-header”
- aria-describedby=”modal-content”
- add `aria-label` for close button
- aria-modal="true"
- it is conventional for `modals` to be able to escape on `ESC` button press. Do not use `disableEscapeKeyDown`

## talstrap Accessibility Assessment

- [x] role = “dialog”
- [ ] id for `modal-header` exist
- [ ] id for `modal-content` exist
- [ ] `aria-labelledby` is defined
- [ ] `aria-describedby` is defined
- [ ] `aria-label` is defined for close button
- [ ] `aria-modal` is set to true

## References:
- [Basic Accessible Modal Example](https://accessibility.huit.harvard.edu/practice-basic-modal-dialog)
- [MDN ARIA: dialog role](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/dialog_role)
- [w3 Modal Example](https://www.w3.org/TR/wai-aria-practices-1.1/examples/dialog-modal/dialog.html)
