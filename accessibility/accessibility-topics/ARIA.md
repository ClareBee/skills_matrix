# Web Accessibility Initiative Accessible Rich Internet Applications (WAI-ARIA)
- attributes which modify how an element is translated into the accessibility tree
- [Accessible Rich Internet Applications (WAI-ARIA)](https://www.w3.org/TR/wai-aria-1.1/)
- can add semantics to an element where no native semantics exist e.g. dropdown
- ARIA attributes **always** need to have explicit values (i.e. aria-checked="true")
- NB ARIA attributes ONLY influence the accessibility tree - they DON'T modify element behaviour/appearance/focusability/event-handling
- can add extra label info for screen readers
- can express different relationships between elements other than parent/child/sibling
- can inform users of change = 'live update'

### Role
- https://www.w3.org/TR/wai-aria-1.0/roles
- shorthand for a UI pattern from defined taxonomy, e.g. tablist
- role, superclass role, subclass role, inherited states & properties, name from, accessible name required y/n, implicit value etc.

- https://www.w3.org/TR/wai-aria-practices-1.1/

- **aria-label** - e.g. for hamburger menu = label FOR
- **aria-labelledby** - using id (or list of ids) to link to any element (even those that don't take labels), e.g. for both radiogroup and radio buttons = label BY
- **aria-describedby** - additional info e.g. password criteria

- DON'T redefine default semantics e.g. `<input type="checkbox"/>` doesn't need role!
- which HTML elements take which roles etc. https://www.w3.org/TR/html-aria/
- BUT `<main role="main"></main>` as not fully supported!

### Relationship attributes
- e.g. **aria-labelledby**
- https://www.w3.org/TR/wai-aria-1.1/#attrs_relationships
- creates semantic relationship between elements
- **aria-owns** = separate element in DOM should be owned by another e.g. in a popup sub-menu
- **aria-activedescendent** = should be presented as focused element e.g. listbox
- **aria-posinset** = 'position in set' & **aria-setsize** (on the item, not the container) = 'set size': defining relationship between sibling elements in a set, e.g. list, esp with lazy-rendering/pagination

### Hidden content
- https://webaim.org/techniques/css/invisiblecontent/
- visibility: hidden/display: none/ html5 hidden attribute
- use absolute positioning to remove from screen?
- **aria-labelledby**, **aria-describedby** = screen-readers only
- **aria-hidden**: removes it unless aria-labelledby/describedby


### aria-live
- updates: 'off', 'polite' (when next do-able), 'assertive' (interrupts)
- aria-live region should ideally be in initial page load
- also work with **aria-atomic** (e.g. date widget), **aria-relevant** (what types of changes should be communicated e.g. additions, removals, text, all), **aria-busy** (ignore changes temporarily e.g. during load, then set to false when finished)
