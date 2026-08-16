---
name: ag-grid
description: Conventions for AG Grid column definitions, cell styling, and addressing a grid from tests. Use when working with AG Grid.
---

# AG Grid

## Every column carries an explicit `colId`

Set `colId` on every column definition, including field-backed columns where AG Grid would otherwise derive the same id from `field`. Column groups carry an explicit `groupId` the same way.

Page objects can address cells by `col-id`, so an explicit id decouples selector stability from field naming. It also survives a later switch to a `valueGetter`, which could otherwise change the derived id and break every selector at once.

Don't strip either as redundant during a review sweep. Duplicating `field` is the point.

## Style cells with `cellStyle`, not Tailwind classes in `cellClass`

The Theming API injects its stylesheet at runtime, after the app's CSS. A Tailwind utility in `cellClass` can therefore lose to the theme.

Use `cellStyle` for anything the theme also controls. Reserve `cellClass` for utilities the theme doesn't touch, where it works fine.

This is specificity-by-load-order, so it won't reproduce in a style debugger that shows the class as present. The class is present. It lost.

## Address grids by role and id in tests

Prefer locating cells by their `gridcell` role together with `col-id`, rather than by position or by text alone. That pairs with the explicit-`colId` rule above and keeps a page object stable when columns are reordered.
