# CSS "scrollbar-gutter" explainer

* Spec: https://www.w3.org/TR/css-overflow-4/
* CSSWG Discussions:
  * https://github.com/w3c/csswg-drafts/issues/92
  * https://github.com/w3c/csswg-drafts/issues/4674#issuecomment-577662037

## Background

The scrollbar gutter is the space between the inner border edge and the outer padding edge, where user agents may display a scrollbar.

“Classic” scrollbars are always placed in a gutter, consuming space when present, and are usually opaque.

“Overlay” scrollbars are placed over the content, not in a gutter, and are usually partially transparent. Their appearance may vary depending on whether and how the user is interacting with them.

The user agent determines whether classic scrollbars or overlay scrollbars are used. Likewise, the user agent also defines the appearance and size of scrollbars and whether they appear on the start or end edge of the box.

## The “scrollbar-gutter” CSS property

### Motivation

The scrollbar-gutter CSS property gives control to Web authors over the presence of scrollbar gutters separately from the ability to control the presence of scrollbars provided by the “overflow” property.

This allows authors to avoid layout changes when the content size changes, while also avoiding ugly visuals when scrolling isn't needed.

### Values

The values of this property have the following meaning:

* auto: Classic scrollbars consume space by creating a scrollbar gutter when overflow is 'scroll, or when overflow is auto and the box is overflowing. Overlay scrollbars do not consume space.
* stable: The scrollbar gutter is present when overflow is scroll or auto and the scrollbar is a classic scrollbar even if the box is not overflowing, but not when the scrollbar is an overlay scrollbar. ("zero space if overlay, always space if intrusive")
* always: The scrollbar gutter is always present when overflow is scroll or auto, regardless of the type of scrollbar or of whether the box is overflowing. ("always space as if it's intrusive, even if you have overlay")
  * Use case: fix cases where an overlay scrollbar obscures content. See https://github.com/w3c/csswg-drafts/issues/4674#issuecomment-577662037
  * Use case: more predictable Web engine layout tests across platforms. See: https://github.com/web-platform-tests/wpt/issues/10972
* both: If a scrollbar gutter would be present on one of the inline start edge or the inline end edge of the box, another scrollbar gutter must be present on the opposite edge as well.
* force: When the force keyword is present stable and always take effect when overflow is visible, hidden or clip in addition auto or scroll. This does not cause a scrollbar to be displayed, only a scrollbar gutter.
  * Use case: reserve the same amount of space on the edges of an element that is adjacent to a stroller as is being reserved in the stroller itself, so that things would visually line up. See https://github.com/w3c/csswg-drafts/issues/4674#issuecomment-577662037

### Layout and painting

For classic scrollbars, the width of the scrollbar gutter is the same as the width of the scrollbar.

For overlay scrollbars, the width of the scrollbar gutter is UA defined. However, it must not be 0, and it must not change based on user interactions with the page or the scrollbar even if the scrollbar itself changes. Also, it must be the same for all elements in the page.

When the scrollbar gutter is present but the scrollbar is not, or the scrollbar is transparent or otherwise does not fully obscure the scrollbar gutter, the background of the scrollbar gutter must be painted as an extension of the padding.
