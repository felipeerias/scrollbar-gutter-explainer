# CSS "scrollbar-gutter" explainer

* Spec: https://www.w3.org/TR/css-overflow-4/
* Discussion: https://github.com/w3c/csswg-drafts/issues/92#issuecomment-272567887
* More discussion: https://github.com/w3c/csswg-drafts/issues/4674#issuecomment-577662037
* Web engine layout testing: https://github.com/web-platform-tests/wpt/issues/10972

The space between the inner border edge and the outer padding edge which user agents may reserve to display the scrollbar is called the scrollbar gutter.

The scrollbar-gutter CSS property gives control to Web authors over the presence of scrollbar gutters separately from the ability to control the presence of scrollbars provided by the “overflow” property.

This allows authors to avoid layout changes when the content size changes, while also avoiding ugly visuals when scrolling isn't needed.

Scrollbars which by default are placed over the content box and do not cause scrollbar gutters to be created are called overlay scrollbars. Such scrollbars are usually partially transparent, revealing the content behind them if any. Their appearance and size may vary based on whether and how the user is interacting with them.

Scrollbars which are always placed in a scrollbar gutter, consuming space when present, are called classic scrollbars. Such scrollbars are usually opaque.

Whether classic scrollbars or overlay scrollbars are used is UA defined.

The appearance and size of the scrollbar is UA defined.

Whether scrollbars appear on the start or end edge of the box is UA defined.

For classic scrollbars, the width of the scrollbar gutter is the same as the width of the scrollbar. For overlay scrollbars, the width of the scrollbar gutter is UA defined. However, it must not be 0, and it must not change based on user interactions with the page or the scrollbar even if the scrollbar itself changes. Also, it must be the same for all elements in the page.

When the scrollbar gutter is present but the scrollbar is not, or the scrollbar is transparent or otherwise does not fully obscure the scrollbar gutter, the background of the scrollbar gutter must be painted as an extension of the padding.

The values of this property have the following meaning:

* auto: Classic scrollbars consume space by creating a scrollbar gutter when overflow is 'scroll, or when overflow is auto and the box is overflowing. Overlay scrollbars do not consume space.
* stable: The scrollbar gutter is present when overflow is scroll or auto and the scrollbar is a classic scrollbar even if the box is not overflowing, but not when the scrollbar is an overlay scrollbar. ("zero space if overlay, always space if intrusive")
* always: The scrollbar gutter is always present when overflow is scroll or auto, regardless of the type of scrollbar or of whether the box is overflowing. ("always space as if it's intrusive, even if you have overlay")
  * Use case: fix cases where an overlay scrollbar obscures content.
  * Use case: more predictable Web engine layout tests across platforms.
* both: If a scrollbar gutter would be present on one of the inline start edge or the inline end edge of the box, another scrollbar gutter must be present on the opposite edge as well.
* force: When the force keyword is present stable and always take effect when overflow is visible, hidden or clip in addition auto or scroll. This does not cause a scrollbar to be displayed, only a scrollbar gutter.
  * Use case: reserve the same amount of space on the edges of an element that is adjacent to a stroller as is being reserved in the stroller itself, so that things would visually line up.


