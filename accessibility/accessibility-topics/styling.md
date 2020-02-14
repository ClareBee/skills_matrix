# Styling

## Focus styles
- WebAIM 2.4.7 focus visible
- native elements but also custom via `:focus` pseudo class in css
- if you insist on removing built-in focus (with outline: 0), use same styles for hover & focus, and also add focus-specific box-shadow for cross-browser support
- style radio buttons w `:focus::before` to target the circle
- mouse and keyboard focus treated differently by browser - focus ring only for keyboard
- if creating custom controls, you'll need to add :moz-focusring (Firefox) or shim: https://github.com/WICG/focus-visible

## ARIA
- use aria attributes for your css selectors  = verifies that you've set your aria properly!
e.g. `button[aria-pressed="true"] {}`

## Responsive design
- e.g. allow zoom - text resizing WebAIM 1.4.4
- always use meta viewport tag:
```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```
- design with responsive grid esp w relative units (%, em, rem)
- correctly-sized buttons (touch targets at least 48 x 48 dp, pointer targets at least 44 x 44dp), spaced at least 8 dp


## Mobile screen readers
- e.g. TalkBack on Android, double-tab to select; swipe through headings

## Colour & Contrast
- WebAIM 1.4.3/1.4.6
- contrast ratio: min of 4.5:1, or larger text 3:1
- BUT for users w low vision impairment or colour deficiencies: 7:1 or large text 4.5:1
- Chrome accessibility dev tools extension - audit
- c 320 million users w some form of colour blindness
- High Contrast Mode - enabled at system level in Mac/Windows, or Chrome High Contrast extension
- Silktide extension on Chrome
 
