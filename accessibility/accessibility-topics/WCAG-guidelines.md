#### Web Content Accessibility Guidelines 2.0
[source](https://www.w3.org/TR/WCAG20/)

## POUR principles:

- **Perceivable**: Web content is made available to the senses - sight, hearing, and/or touch
- **Operable**: Interface forms, controls, and navigation are operable
- **Understandable**: Information and the operation of user interface must be understandable/consistent etc.
- **Robust**: Can the content be consumed by a wide variety of user agents (browsers)? Does it work with assistive technology?

## Web Accessibility In Mind: WebAIM
[source](https://webaim.org/)
- checklist: available online at: webaim.org/standards/wcag/checklist

## Principle 1: Perceivable

### 1.1 Text Alternatives
- Provide text alternatives for any non-text content
- Images, form image buttons, and image map hot spots have appropriate, equivalent
alternative text.
- Images that do not convey content, are decorative, or contain content that is already
conveyed in text are given null alt text (alt="") or implemented as CSS backgrounds.
- Linked images have descriptive alternative text.
- Equivalent alternatives to complex images are provided in context or on a separate
linked page.
- Form buttons have a descriptive value.
- Form inputs have associated text labels.
- Embedded multimedia is identified via accessible text.
- Frames and iframes are appropriately titled.

### 1.2 Time-based Media
- Provide alternatives for time-based media
- A transcript of relevant content is provided for non-live audio-only (audio podcasts,
MP3 files, etc.).
- A transcript or audio description of relevant content is provided for non-live video-only, unless the video is decorative.
- Synchronised captions are provided for non-live video (YouTube videos, etc.).
- Synchronised captions are provided for live media that contains audio (audio-only
broadcasts, web casts, video conferences, etc.)
- Audio descriptions are provided for non-live video. NOTE: Only required if there is
relevant visual content that is not presented in the audio.

### 1.3 Adaptable:
- Create content that can be presented in different ways (e.g., simpler layout) without losing information or structure
- Semantic markup is used to designate headings `<h1>`, regions/landmarks, lists `<ul>`, `<ol>`, and `<dl>` emphasised or special text `<strong>`, `<code>`, `<abbr>`, `<blockquote>`, for example, etc. Semantic markup is used appropriately.
- Tables are used for tabular data and data cells are associated with their headers.
- Text labels are associated with form input elements. Related form elements are grouped with fieldset/legend. ARIA labelling may be used when standard HTML is insufficient.
- The reading and navigation order (determined by code order) is logical and intuitive.
- Instructions do not rely upon shape, size, or visual location (e.g., "Click the square icon to continue" or "Instructions are in the right-hand column").
- Instructions do not rely upon sound (e.g., "A beeping sound indicates you may
continue.").
- Orientation of web content is not restricted to only portrait or landscape, unless a
specific orientation is necessary.
- Input fields that collect certain types of user information have an appropriate
autocomplete attribute defined.
- HTML5 regions or ARIA landmarks are used to identify page regions.
- ARIA is used, where appropriate, to enhance HTML semantics to better identify the
purpose of interface components.

### 1.4 Distinguishable:
- Make it easier for users to see and hear content including separating foreground from background
- Colour is not used as the sole method of conveying content or distinguishing visual
elements.
- Colour alone is not used to distinguish links from surrounding text unless the contrast ratio between the link and the surrounding text is at least 3:1 and an additional distinction (e.g., underline) is provided when the link is hovered and receives focus.
- A mechanism is provided to stop, pause, mute, or adjust volume for audio that
automatically plays on a page for more
- Text and images of text have a contrast ratio of at least 4.5:1.
- Large text - at least 18 point (typically 24px) or 14 point (typically 18.66px) and bold - has a contrast ratio of at least 3:1.
- The page is readable and functional when the page is zoomed to 200%.
- If the same visual presentation can be made using text alone, an image is not used to present that text.
- Audio with speech has no or very low background noise so the speech is easily
distinguished.
- Blocks of text over one sentence in length:
 Are no more than 80 characters wide.
 Are NOT fully justified (aligned to both the left and the right margins).
 Have adequate line spacing (at least 1/2 the height of the text) and paragraph spacing (1.5 times line spacing).
 Have a specified foreground and background colour. These can be applied to specific
elements or to the entire page using CSS (and thus inherited by all other elements).
 Do NOT require horizontal scrolling when the text size is doubled.
- Text is used within an image only for decoration (image does not convey content) OR
when the information cannot be presented with text alone.
- No loss of content or functionality occurs and horizontal scrolling is avoided when
content is presented at a width of 320 pixels.
- A contrast ratio of at least 3:1 is present for differentiating graphical objects (such as icons and components of charts or graphs) and author-customised interface
components (such as buttons, form controls, and focus indicators/outlines).
- No loss of content or functionality occurs when the user adapts paragraph spacing to 2 times the font size, text line height/spacing to 1.5 times the font size, word spacing to .16 times the font size, and letter spacing to .12 times the font size.
 This is best supported by avoiding pixel height definitions for elements that contain
text.
- When additional content is presented on hover or keyboard focus:
 The newly revealed content can be dismissed (generally via the Esc key) without
moving the pointer or keyboard focus, unless the content presents an input error or
does not obscure or interfere with other page content.
 The pointer can be moved to the new content without the content disappearing.
 The new content must remain visible until the pointer or keyboard focus is moved
away from the triggering control, the new content is dismissed, or the new content i

## Principle 2: Operable
### 2.1 Keyboard Accessible:
- All page functionality is available using the keyboard, unless the functionality cannot be accomplished in any known way using a keyboard (e.g., free hand drawing).
- Page-specified shortcut keys and accesskeys (accesskey should typically be avoided) do not conflict with existing browser and screen reader shortcuts.
- Keyboard focus is never locked or trapped at one particular page element. The user can navigate to and from all navigable page elements using only a keyboard.
- If a keyboard shortcut uses printable character keys, then the user must be able to
disable the key command, change the defined key to a non-printable key (Ctrl, Alt, etc.),or only activate the shortcut when an associated interface component or button is focused.

### 2.2 Enough Time:
- Provide users enough time to read and use content
- If a page or application has a time limit, the user is given options to turn off, adjust, or extend that time limit. This is not a requirement for real-time events (e.g., an auction), where the time limit is absolutely required, or if the time limit is longer than 20 hours.
- Automatically moving, blinking, or scrolling content (such as carousels, marquees, or animations) that lasts longer than 5 seconds can be paused, stopped, or hidden by the user.
- Automatically updating content (e.g., a dynamically-updating news ticker, chat
messages, etc.) can be paused, stopped, or hidden by the user or the user can manually
control the timing of the updates.
- Interruptions (alerts, page updates, etc.) can be postponed or suppressed by the user.
- If an authentication session expires, the user can re-authenticate and continue the
activity without losing any data from the current page.
- Users must be warned of any timeout that could result in data loss, unless the data is preserved for longer than 20 hours

### 2.3 Seizures:
- Do not design content in a way that is known to cause seizures or physical reactions
Success Criteria Recommendations
- No page content flashes more than 3 times per second unless that flashing content is
sufficiently small and the flashes are of low contrast and do not contain too much red.
- Users can disable non-essential animation and movement that is triggered by user
interaction.

### 2.4 Navigable:
- Provide ways to help users navigate, find content, and determine where they are
- A link is provided to skip navigation and other page elements that are repeated across web pages.
- A proper heading structure and/or identification of page regions/landmarks may be
considered a sufficient technique. Because navigating by headings or regions is not
supported in most browsers, WebAIM recommends a "skip" link (in addition to
headings and regions) to best support sighted keyboard users.
- The web page has a descriptive and informative page title.
- The navigation order of links, form elements, etc. is logical and intuitive.
- The purpose of each link (or form image button or image map hotspot) can be
determined from the link text alone, or from the link text and its context (e.g.,
surrounding text, list item, previous heading, or table headers).
- Links (or form image buttons) with the same text that go to different locations are
readily distinguishable.
- Multiple ways are available to find other web pages on the site - at least two of: a list of related pages, table of contents, site map, site search, or list of all available web pages.
- Page headings and labels for form and interactive controls are informative. Avoid
duplicating heading (e.g., "More Details") or label text (e.g., "First Name") unless the structure provides adequate differentiation between them.
- It is visually apparent which page element has the current keyboard focus (i.e., as you tab through the page, you can see where you are).
- If a web page is part of a sequence of pages or within a complex site structure, an
indication of the current page location is provided, for example, through breadcrumbs
or specifying the current step in a sequence (e.g., "Step 2 of 5 - Shipping Address").
- The purpose of each link (or form image button or image map hotspot) can be
determined from the link text alone.
- There are no links (or form image buttons) with the same text that go to different places.
- Beyond providing an overall document structure, individual sections of content are
designated using headings, where appropriate.

###  2.5 Input Modalities:
- Make it easier for users to operate functionality through various inputs beyond keyboard
- If multipoint or path-based gestures (such as pinching, swiping, or dragging across the screen) are not essential to the functionality, then the functionality can also be
performed with a single point activation (such as activating a button).
- To help avoid inadvertent activation of controls, avoid non-essential down-event (e.g.,onmousedown) activation when clicking, tapping, or long pressing the screen. Use
onclick, onmouseup, or similar instead. If onmouseup (or similar) is used, you must
provide a mechanism to abort or undo the action performed.
- If an interface component (link, button, etc.) presents text (or images of text), the accessible name (label, alternative text, aria-label, etc.) for that component must include the visible text.
- Functionality that is triggered by moving the device (such as shaking or panning a
mobile device) or by user movement (such as waving to a camera) can be disabled and
equivalent functionality is provided via standard controls like buttons.
- Clickable targets are at least 44 by 44 pixels in size unless an alternative target of that size is provided, the target is inline (such as a link within a sentence), the target is not author-modified (such as a default checkbox), or the small target size is essential to the functionality.

## Principle 3: Understandable
### 3.1 Readable:
- Make text content readable and understandable
- The language of the page is identified using the HTML lang attribute (e.g., <html
lang="en">).
- Words that may be ambiguous, unfamiliar, or used in a very specific way are defined
through adjacent text, a definition list, a glossary, or other suitable method.
- The meaning of an unfamiliar abbreviation is provided by expanding it the first time it is used, using the <abbr> element, or linking to a definition
- A more understandable alternative is provided for content that is more advanced than
can be reasonably read by a person with roughly 9 years of primary education.
- If the pronunciation of a word is vital to understanding that word, its pronunciation is provided immediately following the word or via a link or glossary.
- Make Web pages appear and operate in predictable ways

### 3.2 Predictable:
- When a page element receives focus, it does not result in a substantial change to the page, the spawning of a pop-up window, an additional change of keyboard focus, or any other change that could confuse or disorient the user.
- When a user inputs information or interacts with a control, it does not result in a
substantial change to the page, the spawning of a pop-up window, an additional change
of keyboard focus, or any other change that could confuse or disorient the user unless
the user is informed of the change ahead of time.
- Consistent navigation: Navigation links that are repeated on web pages do not change order when navigating
through the site.
- Elements that have the same functionality across multiple web pages are consistently
identified. For example, a search box at the top of the site should always be labeled the same way.
- Substantial changes to the page, the spawning of pop-up windows, uncontrolled
changes of keyboard focus, or any other change that could confuse or disorient the user must be initiated by the user. Alternatively, the user is provided an option to disable such changes.

### 3.3 Input Assistance:
- Required form elements or form elements that require a specific format, value, or
length provide this information within the element's label.
- Form validation errors are efficient, intuitive, and accessible. The error is clearly identified, quick access to the problematic element is provided, and the user can easily fix the error and resubmit the form.
- Sufficient labels, cues, and instructions for required interactive elements are provided via instructions, examples, properly positioned form labels, and/or fieldsets/legends.
- If an input error is detected (via client-side or server-side validation), suggestions are provided for fixing the input in a timely and accessible manner.
- If the user can change or delete legal, financial, or test data, the changes/deletions can be reversed, verified, or confirmed.
- Instructions and cues are provided in context to help in form completion and
submission.
- If the user can submit information, the submission is reversible, verified,

## Principle 4: Robust
### 4.1 Compatible:
- Maximise compatibility with current and future user agents, including assistive technologies
- Significant HTML/XHTML validation/parsing errors are avoided.
- Markup is used in a way that facilitates accessibility. This includes following the
HTML/XHTML specifications and using forms, form labels, frame titles, etc.
appropriately.
- ARIA is used appropriately to enhance accessibility when HTML is not sufficient.
- If an important status message is presented and focus is not set to that message, the message must be announced to screen reader users, typically via an ARIA alert or live region.
