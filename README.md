# Utopia Core SCSS

The calculations behind [Utopia.fyi](https://utopia.fyi). Written in SCSS.

## Documentation

> Note: Complete documentation to follow in the coming weeks.

## Mixins



## Functions

### `calculateTypeScale()`

Create a fluid type scale between two widths, sizes and scales. Set the number of positive and negative steps, and whether you want the scale to be relative to the `viewport` or the `container`.

#### Example
```ts
generateTypeScale((
  "minWidth": 320,
  "maxWidth": 1240,
  "minFontSize": 18,
  "maxFontSize": 20,
  "minTypeScale": 1.2,
  "maxTypeScale": 1.25,
  "positiveSteps": 5,
  "negativeSteps": 2,
  // Optional
  "relativeTo": "container",
))
```

### `calculateSpaceScale()`

Calculate a set of fluid spaces from min/max width/base sizes, and a number of positive/negative multipliers. Fluid spaces & one-up pairs are automatically created, and custom pairs can be created by supplying the keys you wish to interpolate between. Clamp provided in `rem` and `px`.

#### Example

```ts
calculateSpaceScale((
  "minWidth": 320,
  "maxWidth": 1240,
  "minSize": 18,
  "maxSize": 20,
  "positiveSteps": (1.5, 2, 3, 4, 6),
  "negativeSteps": (0.75, 0.5, 0.25),
  "customSizes": ("s-l", "l-s"),
  // Optional
  "relativeTo": "container",
))

// (
//  "sizes": (
//    (
//      "label": "s",
//      "minSize": 18,
//      "maxSize": 20,
//      "clamp": clamp(),
//      "clampPx": clamp(),
//    ),
//  ),
//  "oneUpPairs": (),
//  "customPairs": (),
// )
```

### `calculateClamp`

Calculate a single clamp calculation from a min/max width & size. Default to using `rem` and `vi` but this can be overriden to use `px` and `cqi`.

#### Example

```ts
calculateClamp({
  "minWidth": 320,
  "maxWidth": 1240,
  "minSize": 16,
  "maxSize": 48,
  // Optional
  "usePx": true,
  "relativeTo": "container"
})
```


### `calculateClamps`

Calculate multiple clamps from a single set of min/max widths. Supply an array of number pairs to interpolate between.

#### Example

```ts
calculateClamps((
  "minWidth": 320,
  "maxWidth": 1240,
  "pairs": (
    (16, 48),
    (40, 18)
  ),
  // Optional
  "usePx": true,
  "relativeTo": "container"
))
```


