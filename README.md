# Explainer: the `scrollbar-gutter` CSS property

- Contents:
  - [Author](#author)
  - [Participate](#participate)
  - [Introduction](#introduction)
  - [Background](#background)
  - [Definition and values](#definition-and-values)
  - [Interaction between `overflow` and `scrollbar-gutter`](#interaction-between-overflow-and-scrollbar-gutter)
  - [Layout](#layout)
  - [Painting](#painting)
  - [Illustrations 🌅](#illustrations-)
  - [References](#references)


## Author

* [Felipe Erias](felipeerias)

## Participate

* Chromium intent-to-prototype (coming soon)
* [TAG design review](https://github.com/w3ctag/design-reviews/issues/520)

## Introduction

The `scrollbar-gutter` CSS property (defined in [CSS Overflow L4](https://www.w3.org/TR/css-overflow-4/)) gives control to Web authors over the presence of scrollbar gutters separately from the ability to control the presence of scrollbars provided by the `overflow` property.

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
* `stable`: When using classic scrollbars, the gutter will be present if `overflow` is `scroll` or `auto` even if the box is not overflowing. When using overlay scrollbars, the gutter will not be present.
  * Use case: Prevent layout changes when the content grows or shrinks.
* `always`: The scrollbar gutter is always present when `overflow` is `scroll` or `auto`, regardless of the type of scrollbar or of whether the box is overflowing.
  * Use case: fix instances where an overlay scrollbar would obscure content. See https://github.com/w3c/csswg-drafts/issues/4674#issuecomment-577662037
  * Use case: more predictable Web engine layout tests across platforms. See: https://github.com/web-platform-tests/wpt/issues/10972

These can only be used in combination with `stable` and `always`:

* `both`: If a gutter would be present on one of the inline start/end edges of the box, another must be present on the opposite edge as well.
  * Use case: simmetry between padding on both sides of the box.
  * Use case: keep the layout stable regardless of the edge where the user agent decides to place the scrollbar.
* `force`: the keywords `stable` and `always` are also in effect when `overflow` is `visible`, `hidden` or `clip`; only the gutter is displayed, not a scrollbar.
  * Use case: reserve the same amount of space on the edges of an element that is adjacent to a scrolling element as is being reserved in the latter, so that both elements line up visually. See https://github.com/w3c/csswg-drafts/issues/4674#issuecomment-577662037

## Interaction between `overflow` and `scrollbar-gutter`

This list summarizes the cases when space will be reserved for the scrollbar gutter:

* Using **classic** scrollbars:
  * when `overflow: scroll`
  * when `overflow: auto` and
    * `scrollbar-gutter: stable`
    * `scrollbar-gutter: always`
    * `scrollbar-gutter: auto` and the box is overflowing
  * when `overflow` is `visible`, `hidden` or `clip`, and
    * `scrollbar-gutter stable force`
    * `scrollbar-gutter always force`
* Using **overlay** scrollbars:
  * when `overflow:scroll` or `auto` and
    * `scrollbar-gutter: always`
  * when `overflow: ` `visible`, `hidden` or `clip`, and
    * `scrollbar-gutter always force`

## Layout

For classic scrollbars, the width of the gutter is the same as the width of the scrollbar.

For overlay scrollbars, the width of the gutter is defined by the user agent, with the following constraints:

* the width must not be 0
* it must not change based on user interactions with the page or the scrollbar (even if the scrollbar itself changes)
* it must be the same for all elements in the page

## Painting

This property does not influence how the scrollbars themselves are painted.

When the gutter is present but the scrollbar is not, or the scrollbar is transparent or otherwise does not fully obscure the gutter, the background of the gutter must be painted as an extension of the padding.

## Illustrations 🌅

Effect of different values of `scrollbar-gutter` with ***classic*** scrollbars (using `overflow: auto;`):

![Classic scrollbars](images/classic.png)

Effect of different values of `scrollbar-gutter` with ***overlay*** scrollbars (using `overflow: auto;`):

![Overlay scrollbars](images/overlay.png)

## References

* Spec: https://www.w3.org/TR/css-overflow-4/
* CSSWG Discussions:
  * https://github.com/w3c/csswg-drafts/issues/92
  * https://github.com/w3c/csswg-drafts/issues/4674#issuecomment-577662037
