# Explainer: the `scrollbar-gutter` CSS property

* Spec: https://www.w3.org/TR/css-overflow-4/
* CSSWG Discussions:
  * https://github.com/w3c/csswg-drafts/issues/92
  * https://github.com/w3c/csswg-drafts/issues/4674#issuecomment-577662037

## Motivation

The `scrollbar-gutter` CSS property gives control to Web authors over the presence of scrollbar gutters separately from the ability to control the presence of scrollbars provided by the `overflow` property.

This allows authors to avoid layout changes when the content size changes, while also avoiding ugly visuals when scrolling isn't needed.

## Background

The *scrollbar gutter* is the space between the inner border edge and the outer padding edge, where user agents may display a scrollbar.

*“Classic” scrollbars* are always placed in a gutter, consuming space when present, and are usually opaque.

*“Overlay” scrollbars* are placed over the content, not in a gutter, and are usually partially transparent. Their appearance may vary depending on whether and how the user is interacting with them.

The user agent determines whether classic scrollbars or overlay scrollbars are used. Likewise, the user agent also defines the appearance and size of scrollbars and whether they appear on the start or end edge of the box.

## Definition and values

The syntax for the `scrollbar-gutter` property is:

```
auto | [ stable | always ] && both? && force?
```

These values have the following meaning:

* `auto`: No changes from current behaviour. Default value.
* `stable`: When using classic scrollbars, the gutter will be present if the *overflow* is *scroll* or `auto` even if the box is not overflowing. When using overlay scrollbars, the gutter will not be present.
  * Use case: Prevent layout changes when the content starts/stops overflowing.
* `always`: The scrollbar gutter is always present when overflow is scroll or auto, regardless of the type of scrollbar or of whether the box is overflowing.
  * Use case: fix cases where an overlay scrollbar obscures content. See https://github.com/w3c/csswg-drafts/issues/4674#issuecomment-577662037
  * Use case: more predictable Web engine layout tests across platforms. See: https://github.com/web-platform-tests/wpt/issues/10972

Additionally, these can only be used in combination with `stable` and `always`:

* `both`: If a gutter would be present on one of the inline start/end edges of the box, another gutter must be present on the opposite edge as well.
  * Use case: simmetry between padding on both sides of the box.
  * Use case: keep the layout stable regardless of the edge where the user agent decides to place the scrollbar.
* `force`: the keywords `stable` and `always` are also in effect when `overflow` is `visible`, `hidden` or `clip`; only the gutter is displayed, not a scrollbar.
  * Use case: reserve the same amount of space on the edges of an element that is adjacent to a stroller as is being reserved in the stroller itself, so that things would visually line up. See https://github.com/w3c/csswg-drafts/issues/4674#issuecomment-577662037

## Layout

For classic scrollbars, the width of the scrollbar gutter is the same as the width of the scrollbar.

For overlay scrollbars, the width of the scrollbar gutter is UA defined. However, it must not be 0, and it must not change based on user interactions with the page or the scrollbar even if the scrollbar itself changes. Also, it must be the same for all elements in the page.

## Painting

This property does not influence how the scrollbars themselves are painted.

When the gutter is present but the scrollbar is not, or the scrollbar is transparent or otherwise does not fully obscure the gutter, the background of the gutter must be painted as an extension of the padding.
