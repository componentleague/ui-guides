# Dialogs

Dialogs are content areas that appear, as a modal, above the main content area.

Use a dialog to focus a user on a specific task. Dialog tasks should be relatively quick, like settings panels.
DO NOT use a dialog if the user will need to be looking at some other content or cross-referencing to perform the task.

## Links

- [A11y Dialog Specifications](https://www.w3.org/TR/wai-aria-practices/#dialog_modal)
- [Generic Dialog](https://genericcomponents.netlify.app/generic-dialog/demo/index.html)
- [GitHub Details Dialog](https://github.com/github/details-dialog-element)
- [Material Dialog](https://github.com/material-components/material-components-web-components/tree/master/packages/dialog)

## User Considerations

For a user, the purpose of a dialog is to focus a specific task.

Expectations:

- The dialog MUST open when I need it.
  - Often opens after clicking a button
  - MUST open quickly (~ 300 milliseconds)
- The dialog MUST close when I'm done with it.
  - MUST close very quickly (~ 150 milliseconds)
  - MUST close when I click a close button
  - MUST indicate clearly how to close it, often with a X in the top right, and/or a [cancel] button
  - MUST close when I press ESC
  - SHOULD NOT close when clicking outside of it / common anti-pattern
- Dialogs MUST be modal, otherwise it is simply an Overlay.
- The dialog MUST trap focus; it has it's own focus land.
  - MUST focus first focusable element when opened
- The dialog SHOULD hide background information - this is a feature.
  - It MAY give a faint background of the main content to hint context of where the user is
- The dialog SHOULD have a header with a title.

## Style Considerations

- The Dialog SHOULD have a "scrim" or background to delineate itself from the main content and focus the user.
- On mobile, the dialog SHOULD act like a screen-stack instead of a pure dialog.
  - Push the main content left while the dialog comes in from the right as its own page
  - This means no scrim
  - SHOULD have a back button at the top left
- Buttons SHOULD be in their own area in the footer.
- If the content is larger than the dialog, the content SHOULD have its own scroll space, separate from the header and footer.

## Technical Considerations

### Shadow DOM

As all overlays, the dialog is sometimes defined in a Shadow Root which makes it impossible to take up the entire screen. There are two strategies for this:

Keep dialogs in the light dom: This means we must layout our pages to ensure dialogs or parents of dialogs are never put into a Shadow DOM. 
If we can ensure a page architecture where we keep page-level elements in the Light DOM, which is perferable anyway, then this strategy works fine.

So called "hoisting" the dialog, which is when the element is taken out of the ShadowRoot of its host, and is moved up into the document body. 
This can be a simple solution, but necessitates duplicating the element, so updating will not work as planned. Also styles are no longer encapsulated. This
strategy is perfered if we can't ensure the dialog is in the Light DOM.

### Details / Summary

GitHub tends to use the `<details>` element in many surprising ways, and has a very nice pattern for using it for dialogs, which is implemented in the [GitHub Details Dialog](https://github.com/github/details-dialog-element).

This has an added benefit of working (mostly) without javascript as the content can be styled as a dialog, allowing progressive enhancement.

The trade off is that extra boilerplate must be added around the dialog, and that it must live where its button is. In the end, it seems worth it as long as the tradeoffs are not prohibitively complex.
