# Focus
- determines where keyboard events go in the page
Tab = moves focus forwards through 'tab order'
Shift + Tab = moves focus backwards
Arrow keys = move focus around within a component e.g. dropdown

WebAIM 2.1.1 - all functionality should be available via the keyboard
https://www.w3.org/TR/wai-aria-practices/#aria_ex

Interactive HTML elements like input etc => implicitly focusable
BUT not all elements are focusable

DOM order matters!
tabindex attribute = applicable to any html element (but really only interactive elements should be focusable!)
- takes a range of values:
  - '-1' = not in natural tab order, but can be inserted via JS (and calling focus() on it), useful for modals
  - '0' = default
  - anti-pattern to use numbers > 0
### Managing focus:
  - e.g. w animated scroll on a click etc.
  - e.g. click on navigation link shifts focus to relevant heading: give heading a tabindex of -1 (so it's not in usual flow) then on nav click, call `.focus()`;

### Skip links:
- gives access to main content, skipping navbar etc.
- anchor tag on link above nav => main content section
- use css to initially place it off screen then move it on with the `:focus` pseudo-class

### Roving tabindex/focus:
- currently focused element has tabindex of '0', rest have -1
- component then uses a keyboard event listener to determine which key the user presses; when this happens, it sets the previously focused child's tabindex to -1, sets the to-be-focused child's tabindex to 0, and calls the `.focus()` method on it.
- when it reaches the end, loops back round again
- example at https://www.w3.org/TR/wai-aria-practices-1.1/examples/radio/radio-1/radio-1.html

<div role="radiogroup"
    aria-labelledby="group_label_1"
    id="rg1">
  <div role="radio"
      checked
      tabindex="0">
    Coffee
  </div>
  <div role="radio"
      tabindex="-1">
    Tea
  </div>
  <div role="radio"
      tabindex="-1">
    Hot Chocolate
  </div>
</div>
```html
<div role="radiogroup"
     aria-labelledby="group_label_1"
     id="rg1">
  <div role="radio"
       aria-checked="false"
       tabindex="0">
    Coffee
  </div>
  <div role="radio"
       aria-checked="false"
       tabindex="-1">
    Tea
  </div>
  <div role="radio"
       aria-checked="false"
       tabindex="-1">
    Hot Chocolate
  </div>
</div>
```
--
### Losing focus:
- if you 'lose' where the focus is (e.g. could be off screen), use DevTools & log document active element: `document.activeElement`
- or use Chrome extension - accessibility audit
- e.g. sliding drawer menu: set visibility to hidden/ or display none when offscreen

### Keyboard traps:
- if you can't 'progress' via Tab (WebAIM 2.1.2 in checklist)
- BUT necessary for modals = 'temporary trap'! (identify focusable elements and tab through)
