# Semantics

## Assistive Technology
e.g. screen-reader, braille display, magnification, eye-tracking, [sip and puff](https://en.wikipedia.org/wiki/Sip-and-puff), [switch access](https://en.wikipedia.org/wiki/Switch_access), voice control

## Affordances
>Affordances are an object’s properties that show the possible actions users can take with it, thereby suggesting how they may interact with that object. For instance, a button can look as if it needs to be turned or pushed. The characteristics of the button which make it look “turnable” or “pushable” together form its affordances.
[source](https://www.interaction-design.org/literature/topics/affordances)
- Originally coined James Gibson, 1977.
- 'cues' for usage
- WebAIM Checklist 4.1.2: Name, role, value = markup to facilitate accessibility


## Screen Reader
e.g. Mac's VoiceOver (Cmd + F5); NVDA OS/Windows-compatible
- provides auditory interface based on programmatically expressed semantics
- role = what type of element it is
- name
- value
- info about state

## The Accessibility Tree
- modified version of the DOM tree w/o visual info
- much of DOM has implicit semantic meaning thanks to native HTML elements

## Writing semantic HTML
- use native HTML where possible - i.e. a button instead of a clickable div!
- provide text alternatives for non-text content (WebAIM 1.1)
- labels = a) visible b) text alternative (e.g. form buttons have descriptive text)
- wrap the element with the label or use 'for' attribute
- images with 'alt' text: only if necessary, not just decorative/redundant (use an empty alt text instead so the screen reader can skip it)

## Headings
- screen-readers can navigate by headings (as well as by other elements, e.g. links, formfields, landmarks etc)
- WebAIM 1.3.2 - Meaningful Sequence i.e. DOM order matters!
- 2.4.10 - Section Headings
- headings are NOT for appearance but for structuring content
```javascript
for (var i = 0, headings = $$('h1,h2,h3,h4,h5,h6');
     i < headings.length; i++) {
   console.log(headings[i].textContent.trim() + " " +  
               headings[i].tagName,
               headings[i]);
}
```
- some headings offscreen - for screen-reader-only content (use sparingly!)

## Other nav options
- CTRL + Option + U = Web Rotor
- links:
  - anti-patterns:
    - using a span instead of an anchor tag
    - using an anchor tag without a href
    - using an anchor tag instead of a button
    - using an image as link content - needs descriptive alt text at least!
    - ambiguous link text e.g. 'click here'/'learn more' - needs more context

- `accesskey`: https://webaim.org/techniques/keyboard/accesskey (use carefully)
- landmarks (HTML5)
  - main
  - header
  - footer
  - section (generic)
  - article (self-contained e.g. blog post)
  - aside (tangentially related)
  - nav
