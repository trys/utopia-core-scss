# Utopia Core SCSS

The calculations behind [Utopia.fyi](https://utopia.fyi). Written in SCSS. Also available in [JS/TS here](https://github.com/trys/utopia-core).

## Installation

```bash
npm install utopia-core-scss
```

```scss
@use 'node_modules/utopia-core-scss/src/utopia' as utopia;
```

## Documentation

Utopia Core SCSS outputs in two formats: mixins & functions. Mixins generate actual (slightly) opinionated CSS custom properties. Functions sit beneath mixins doing the real work, returning calculated values for you to write your own custom CSS. The both use identical APIs and only differ by name (`generateX` for mixins, `calculateX` for functions).

All mixins/functions default `relativeTo` to `viewport` (`vi`), but can be overriden to `container` (`cqi`), or `viewport-width` (`vw`).

## Mixins

### `generateTypeScale()`

Generate a fluid type scale between two widths, sizes and scales. Set the number of positive and negative steps. Custom property `prefix` can be overriden from `step-`.

#### Example

```scss
:root {
  // Calling this:
  @include utopia.generateTypeScale((
    "minWidth": 320,
    "maxWidth": 1240,
    "minFontSize": 18,
    "maxFontSize": 20,
    "minTypeScale": 1.2,
    "maxTypeScale": 1.25,
    "positiveSteps": 2,
    "negativeSteps": 2,
    /* Optional params */
    "relativeTo": "viewport",
    "prefix": "step-",
  ));

  // Generates this:
  --step--2: clamp(...);
  --step--1: clamp(...);
  --step-0: clamp(...);
  --step-1: clamp(...);
  --step-2: clamp(...);
}
```

### `generateSpaceScale()`

Generate a set of fluid spaces from min/max width/base sizes, and a number of positive/negative multipliers. Fluid spaces & one-up pairs are automatically created, and custom pairs can be created by supplying the keys you wish to interpolate between. Pass `usePx: true` to separate space from browser text zoom preferences. Custom property `prefix` can be overriden from `space-`.

#### Example

```scss
:root {
  // Calling this:
  @include utopia.generateSpaceScale((
    "minWidth": 320,
    "maxWidth": 1240,
    "minSize": 18,
    "maxSize": 20,
    "positiveSteps": (1.5, 2),
    "negativeSteps": (0.75),
    /* Optional params */
    "customSizes": ("s-l",),
    "usePx": true,
    "relativeTo": "container",
    "prefix": "space-",
    "allPairs": false,
  ));

  // Generates this:
  --space-xs: clamp(...);
  --space-s: clamp(...);
  --space-m: clamp(...);
  --space-l: clamp(...);
  // One-up pairs
  --space-xs-s: clamp(...);
  --space-s-m: clamp(...);
  --space-m-l: clamp(...);
  // Custom pairs
  --space-s-l: clamp(...);
}
```

#### Note

Passing `allPairs: true` will generate all possible pairs of the positive/negative steps, overriding any custom pairs you specify. This is useful for generating a full set of fluid spaces, but may not be necessary if you are only interested in a few specific pairs, and will result in a larger CSS file. Use a tool such as [PurgeCSS](https://purgecss.com/) to remove unused custom properties at build time.

### `generateClamps()`

Generate multiple clamps from a single set of min/max widths. Supply an array of number pairs to interpolate between. Pass `usePx: true` to separate space from browser text zoom preferences. Custom property `prefix` can be overriden from `space-`.

#### Example

```scss
:root {
  // Calling this:
  @include utopia.generateClamps((
    "minWidth": 320,
    "maxWidth": 1240,
    "pairs": (
      (16, 48),
      (40, 18)
    ),
    /* Optional params */
    "usePx": true,
    "relativeTo": "container",
    "prefix": "space-",
  ))

  // Generates this:
  --space-16-48: clamp(...);
  --space-40-18: clamp(...);
}
```

### `generateClamp()`

Generate a single clamp custom property from a min/max width & size. Default to using `rem` and `vi` but this can be overriden to use `px` and `cqi`. Pass `usePx: true` to separate space from browser text zoom preferences. Custom property `prefix` can be overriden from `space-`.

#### Example

```scss
:root {
  // Calling this:
  @include utopia.generateClamp((
    "minWidth": 320,
    "maxWidth": 1240,
    "minSize": 16,
    "maxSize": 20,
    /* Optional params */
    "usePx": true,
    "relativeTo": "container",
    "prefix": "space-",
  ))

  // Generates this:
  --space-16-20: clamp(...);
}
```

## Functions

### `calculateTypeScale()`

Calculate a fluid type scale between two widths, sizes and scales. Set the number of positive and negative steps, and whether you want the scale to be relative to the `viewport` or the `container`.

#### Example

```scss
$typeScale: utopia.calculateTypeScale((
  "minWidth": 320,
  "maxWidth": 1240,
  "minFontSize": 18,
  "maxFontSize": 20,
  "minTypeScale": 1.2,
  "maxTypeScale": 1.25,
  "positiveSteps": 5,
  "negativeSteps": 2,
  /* Optional params */
  "relativeTo": "container",
))

// $typeScale == (
//    (
//      "step": 0,
//      "minFontSize": 18,
//      "maxFontSize": 20,
//      "clamp": clamp(...),
//    )
//  )
```

### `calculateSpaceScale()`

Calculate a set of fluid spaces from min/max width/base sizes, and a number of positive/negative multipliers. Fluid spaces & one-up pairs are automatically created, and custom pairs can be created by supplying the keys you wish to interpolate between. Clamp provided in `rem` and `px`.

#### Example

```scss
$spaceScales: utopia.calculateSpaceScale((
  "minWidth": 320,
  "maxWidth": 1240,
  "minSize": 18,
  "maxSize": 20,
  "positiveSteps": (1.5, 2, 3, 4, 6),
  "negativeSteps": (0.75, 0.5, 0.25),
  "customSizes": ("s-l", "l-s",),
  /* Optional params */
  "relativeTo": "container",
))

// $spaceScales == (
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
//  "allPairs": (),
// )
```

### `calculateClamps()`

Calculate multiple clamps from a single set of min/max widths. Supply an array of number pairs to interpolate between.

#### Example

```scss
$clamps: utopia.calculateClamps((
  "minWidth": 320,
  "maxWidth": 1240,
  "pairs": (
    (16, 48),
    (40, 18)
  ),
  /* Optional params */
  "usePx": true,
  "relativeTo": "container"
))

// $clamps == (
//    (
//      "label": "16-48",
//      "clamp": clamp(...),
//    ),
//    (
//      "label": "40-16",
//      "clamp": clamp(...),
//    )
//  )
```

### `calculateClamp()`

Calculate a single clamp calculation from a min/max width & size. Default to using `rem` and `vi` but this can be overriden to use `px` and `cqi`.

#### Example

```scss
$clamp: utopia.calculateClamp((
  "minWidth": 320,
  "maxWidth": 1240,
  "minSize": 16,
  "maxSize": 48,
  /* Optional params */
  "usePx": true,
  "relativeTo": "container"
))

// clamp(1rem, 0.3043rem + 3.4783vi, 3rem);
```


