# Style Guide

1. [SCSS](#scss)
2. [Vue](#vue)

------

## <a href="#scss" name="scss">#</a> SCSS

1. [Nomenclature](#scss-nomenclature)
   1. [Classname Anatomy](#scss-nomenclature-anatomy)
   2. [Class Types](#scss-nomenclature-types)
2. [Composition](#scss-composition)
   1. [Note](#scss-composition-note)
   2. [Rules](#scss-composition-rules)
   3. [Class Types](#scss-composition-class-types)
   4. [Examples](#scss-composition-examples)
3. [File Structure](#scss-file-structure)
   1. [Rules](#scss-file-structure-rules)
   2. [General Structure and Formatting](#scss-file-structure-general)
   3. [Properties](#scss-file-structure-properties)
4. [Token Maps](#scss-tokens)
   1. [Declaration Syntax](#scss-tokens-syntax)
   2. [Invoking Tokens](#scss-tokens-invokation)
   3. [Inheritance](#scss-tokens-inheritance)
   4. [Note about `$color` map](#scss-tokens-color-note)
   5. [File Structure](#scss-tokens-file-structure)
5. [Directory Structure](#scss-directory-structure)
   1. [Organization](#scss-directory-structure-organization)
   2. [Import Order](#scss-directory-structure-import-order)
   3. [Notes on specific files](#scss-directory-structure-files)

------

### <a href="#scss-nomenclature" name="scss-nomenclature">#</a> Nomenclature

1. [Classname Anatomy](#scss-nomenclature-anatomy)
2. [Class Types](#scss-nomenclature-types)

#### <a href="#scss-nomenclature-anatomy" name="scss-nomenclature-anatomy">#</a> Classname Anatomy

```scss
.cn-x-class-name {...}
```
1. `cn-` company name name-spacing prefix
2. `x-` Hungarian notation (HN) type infix
3. `class-name`

##### <a name="scss-nomenclature-blocks"></a>BEM Blocks

```scss
.cn-x-block {...}
```

##### <a name="scss-nomenclature-elements"></a>BEM Elements

```scss
.cn-x-block__element {...}
```

##### <a name="scss-nomenclature-modifiers"></a>BEM Modifiers

```scss
.cn-x-block--modifier {...}
```

##### <a name="scss-nomenclature-modified-elements"></a>BEM-Modified Elements

```scss
.cn-x-block--modifier .cn-x-block__element {...}
```

##### Object/Component State

```scss
.cn-x-block.cn-is-state {...}
```

##### Object/Component Element State

```scss
.cn-x-block.cn-is-state .cn-x-block__element {...}
```

##### Media Breakpoint Modifiers

```scss
.cn-x-block@bp {...}
```

##### <a name="scss-nomenclature-anatomy-note"></a>Note

After this point in the document, the term *class structure* refers to any set of classes defined under a particular block, including the block itself, its elements and any compound selectors, such as those involving modifiers, pseudo-selectors, State Classes, etc.

<a name="scss-nomenclature-anatomy-note-orders"></a>A class-structure consisting of only one class selector, i.e. its [block](#scss-nomenclature-blocks), is considered to be *lower-order*; all other class structures are considered *higher-order*. For orders of Utilities, see [§ClassTypes.Utilities.Orders](#scss-nomenclature-types-utilities-orders).

#### <a href="#scss-nomenclature-types" name="scss-nomenclature-types">#</a> Class Types

##### <a name="scss-nomenclature-types-objects"></a>Objects

HN infix: `o`, `l` (layout), `t` (transition)

###### Notes

Objects are recurring, often multi-class patterns.

An ideal architecture would consist entirely of Objects. Any refactors of the architecture ought to consider abstracting code into Objects while at the same time reducing their number (with attention paid to future flexibility); in practice, however, Objects are often rather simple constructs.

For more information, see [§Composition.ClassTypes.Objects](#scss-composition-objects).

##### <a name="scss-nomenclature-types-components"></a>Components

HN infix: `c`

###### Notes

Simply put, Components are parallel representations of Vue files. A Component's [block](#scss-nomenclature-blocks) is most often named essentially the same as the `.vue` file.

Components should consist of only pattern instance-specific styles, all other styles being relegated first to Objects, and then to Utilities. [Static props](#scss-file-structure-properties-classes-static) should be replaced by Utilities, or Objects where possible.

For more information, see [§Composition.ClassTypes.Components](#scss-composition-components).

##### <a name="scss-nomenclature-types-utilities"></a>Utilities

HN infix: `o`, `l` (layout)

###### Anatomy

```scss
.cn-u-pn-v {...}
```

1. `pn-` abbreviated property name prefix
2. `v` abbreviated property value

###### Notes

Often consist of a single property with a common value.

A Utility's [class structure](#scss-nomenclature-anatomy-note) should consist of no more than one class (itself).

[Higher-order Utilities](#scss-nomenclature-types-utilities-orders) should exist only when there is no discernable or meaningful pattern to be found, or if the combination of props is sufficiently simple and/or obvious.

In general, all Utility class properties have the `!important` flag. This can often provide a distinction between higher-order Utilities and lower-order Objects.

For more information, see [§Composition.ClassTypes.Utilities](#scss-composition-utilities).

###### <a name="scss-nomenclature-types-utilities-orders"></a>Orders

Whereas the order of other [class-structures](#scss-nomenclature-anatomy-note) depends upon the number of class selectors present, the distinction between 'higher-' and 'lower-order' Utilities depends, by analogy, upon the number of properties present, with lower-order Utilities containing only one property, and higher- more than one.

Classifying Utilities in this way also hints at a soft but useful benchmark helping to delineate between Utilities and lower-order Objects.

##### <a name="scss-nomenclature-types-state-classes"></a>State Classes

HN infixes: `is`, `has`, etc.

###### Notes

State Classes are glorified, free-standing [modifiers](#scss-nomenclature-modifiers) denoting differences in a [block's](#scss-nomenclature-blocks) state, such as being hovered, toggled, disabled, etc. Separating them from the classes they modify aims to make their usage more generic. It also allows them to be used across a [class structure](#scss-nomenclature-anatomy-note) without breaking the [proscription against directly modifying](#scss-composition-rules-modifying-elements) a [block's](#scss-nomenclature-blocks) [elements](#scss-nomenclature-elements). Importantly, in the case of naturally occurring states, such as hovered, disabled, etc., they provide an interface for applying, and framework for definitively grappling with, styles *not to be* bound to the :pseudo-selectors whose presence, in practice, may or may not actually conform to an element's intended presentational state, or for that matter even occur on the element at all as in the case of complex, artificial input controls, etc.

State Classes are generally programmatically applied, with rare exception.

###### Examples

* `cn-is-disabled`
* `cn-is-hovered`
* `cn-is-open`
* `cn-has-content`

For more information, see [§Composition.ClassTypes.StateClasses](#scss-composition-state-classes).

------

### <a href="#scss-composition" name="scss-composition">#</a> Composition

1. [Note](#scss-composition-note)
2. [Rules](#scss-composition-rules)
3. [Class Types](#scss-composition-class-types)
4. [Examples](#scss-composition-examples)

#### <a href="#scss-composition-note" name="scss-composition-note">#</a> Note

*Composition* refers the use of multiple classes on a single DOM element, or multiple class structures through a DOM element structure, to the end of creating some synergetic effect that none would have accomplished on its own.

(For information about the listing of classes on a component instance, see [§Vue.ComponentInstanceStructure.ClassAttributeOrder](#vue-component-instance-class-attribute).)

#### <a href="#scss-composition-rules" name="scss-composition-rules">#</a> Rules

Selectors may not contain #id's.

Selectors may not contain tag-names (excluding [base styles](#scss-directory-structure-files-base-styles)).

A classname may have at most two levels: [block](#scss-nomenclature-blocks) and \[sub-][element](#scss-nomenclature-elements)[s-set]. This is primarily to encourage the decoupling of styles and DOM.

Selectors must be as flat as possible, with the exception of :pseudo-selectors and [State Classes](#scss-nomenclature-types-state-classes): an [element's](#scss-nomenclature-elements) declaration should not need to be nested within, or a [modifier's](#scss-nomenclature-modifiers) declaration appended to, its [block](#scss-nomenclature-blocks) to apply its properties, even if this would render its styles incomplete; elements and modifiers should not be used outside of the context of their block.

```scss
// Valid
.cn-x-block__element {...}
.cn-x-block--modifier {...}
```

```scss
// Invalid
.cn-x-block .cn-x-block__element {...}
.cn-x-block.cn-x-block--modifier {...}
```

<a name="scss-composition-rules-modifying-elements"></a>[Modifiers](#scss-nomenclature-modifiers), including those for variants and themes, may be applied **only** to [blocks](#scss-nomenclature-blocks), i.e. **never** to [elements](#scss-nomenclature-elements).
For element modification, see [§Nomenclature.BEMModifiedElements](#scss-nomenclature-modified-elements) and [§FileStructure.VariantAndThemeModifierClasses](#scss-file-structure-variant-theme-modifiers).

[State Classes](#scss-nomenclature-types-state-classes) may directly modify [elements](#scss-nomenclature-elements); however, this should be avoided, and the state description simplified to the [block](#scss-nomenclature-blocks) level, wherever possible.

#### <a href="#scss-composition-class-types" name="scss-composition-class-types">#</a> Class Types

##### <a name="scss-composition-objects"></a>Objects

In principle, [Object](#scss-nomenclature-types-objects) declarations should not overlap each other, and Object composition should have as much overlap as possible.

##### <a name="scss-composition-components"></a>Components

Unlike Objects, [Components](#scss-nomenclature-types-components) are strictly forbidden from overlapping in composition; however, like Objects, Component declarations should also not overlap each other; anywhere Components overlap is an opportunity to relegate styles to an Object.

##### <a name="scss-composition-utilities"></a>Utilities

[Utilities](#scss-nomenclature-types-utilities) are meant to shoulder the burden of a Component [class structure's](#scss-nomenclature-anatomy-note) [Static props](#scss-file-structure-properties-classes-static) if and only if these props are, or are likely to be, used across multiple Components and no Object exists or can be declared to simplify their implementation across the architecture.

An Object's [Static props](#scss-file-structure-properties-classes-static) **may not** be replaced by Utilities.

##### <a name="scss-composition-state-classes"></a>State Classes

[State Classes](#scss-nomenclature-types-state-classes) should not be assigned styles except in composition.

```scss
.cn-x-block.cn-is-disabled {...} // Valid
```

```scss
.cn-is-disabled {...} // Invalid
```

#### <a href="#scss-composition-examples" name="scss-composition-examples">#</a> Examples

```html
<tag class="cn-x-block cn-is-disabled">
  <tag class="cn-x-block__element"></tag>
</tag>
```

```html
<tag class="cn-o-btn cn-o-btn--v-primary cn-o-btn--t-primary  cn-is-toggled">
```

```html
<tag class="cn-c-extended-btn__actions-list  cn-o-semantic-list  cn-l-p-a cn-l-r-0 cn-l-ta-sa">
```

```html
<tag class="cn-c-switch__label  cn-o-label cn-o-label--t-primary">
```

------

### <a href="#scss-file-structure" name="scss-file-structure">#</a> File Structure

1. [Rules](#scss-file-structure-rules)
2. [General Structure and Formatting](#scss-file-structure-general)
3. [Properties](#scss-file-structure-properties)

#### <a href="#scss-file-structure-rules" name="scss-file-structure-rules">#</a> Rules

There may be only one [Component](#scss-nomenclature-types-components) per file. [Objects](#scss-nomenclature-types-objects) and [Utilities](#scss-nomenclature-types-utilities) may be grouped where it makes sense to do so.

All styles under a [block](#scss-nomenclature-blocks) are nested with the SASS `&`. There is no increase in specificity.

```scss
// SCSS
.block {
  &__element {
    color: #000;
  }
}
```

```css
// Compiled CSS
.block__element { color: #000; }
```

All [Config](#scss-file-structure-properties-classes-config) and [Theme props](#scss-file-structure-properties-classes-theme) should be defined in respective [maps](#scss-tokens) associated with the [block](#scss-nomenclature-blocks), and should themselves be consolidated into the [config and theme files](#scss-directory-structure-configuration).

<a name="scss-file-structure-rules-state-declarations"></a>State declarations should be ordered from lowest to highest in terms of priority to override others, with the disabled state generally being last.

<a name="scss-file-structure-rules-property-classes"></a>[Property Classes](#scss-file-structure-properties-classes) should be set within a selector in the following order, and marked (with the exception of the [Static](#scss-file-structure-properties-classes-static) or only Class):

1. Static
2. Config
3. Theme
4. State

(For information about ordering collections of individual properties within classes, see [§Properties.PropertyOrder](#scss-file-structure-properties-order).)



#### <a href="#scss-file-structure-general" name="scss-file-structure-general">#</a> General Structure and Formatting

```scss
//**
// [File and class(es) type]
//**



.cn-x-block {
  // [Static props]
  
  // Config
  // [Config props]

  // Theme
  // [Theme props]
  
  // State [state props, state pseudo-selectors, state selectors]
  // [(neutral) State props]
  // [transition props]
  &:state-pseudo-selector {
    // [all Prop Classes, same as above]
  }
  // [...]
  &.cn-is-state {
    // [all Prop Classes, same as above]
  }
  // [...]
  
  
  &__element { // [elements]
    // [all Prop Classes, same as above, incl. neutral State props]
  }
  // State [block state pseudo-selectors, block state classes]
  &:state-pseudo-selector &__element {
    // [all Prop Classes, same as above]
  }
  // [...]
  &.cn-is-state &__element {
    // [all Prop Classes, same as above]
  }
  // [...]
  
}

```

White space between selectors belongs to the following selectors. So, for example, if a [block](#scss-nomenclature-blocks) had only [elements](#scss-nomenclature-elements), there would still be 2 whitespace lines between the block selector and the first element.

For information about Static, Config, Theme and State property classes, see [§Properties.PropertyClasses](#scss-file-structure-properties-classes).

#### <a href="#scss-file-structure-variant-theme-modifiers" name="scss-file-structure-variant-theme-modifiers">#</a> Variant and Theme modifier classes (extending to other configurations)

```scss
.cn-x-block {
  
  // Config
  &--v-variant-name {...}
  &--v-variant-name:state-pseudo-selector {...}
  &--v-variant-name.cn-is-state {...}
  // [...]
  // Theme
  &--t-theme-name {...}
  &--t-theme-name:state-pseudo-selector {...}
  &--t-theme-name.cn-is-state {...}
  // [...]


  &__element {...}
  &:state-pseudo-selector &__element {...}
  &.cn-is-state &__element {...}
  // Config
  &--v-variant-name &__element {...}
  &--v-variant-name:state-pseudo-selector &__element {...}
  &--v-variant-name.cn-is-state &__element {...}
  // [...]
  // Theme
  &--t-theme-name &__element {...}
  &--t-theme-name:state-pseudo-selector &__element {...}
  &--t-theme-name.cn-is-state &__element {...}
  // [...]

}
```

#### <a href="#scss-file-structure-properties" name="scss-file-structure-properties">#</a> Properties

##### Rules

One property per line.

White space between lines may be added to improve legibility (e.g. around a `position` + [`top`/`bottom`/`left`/`right`] + `margin` group), but should span only one line.

Quoted property values should use only double-quotes `"`.

##### <a name="scss-file-structure-properties-classes"></a>Property Classes

###### <a name="scss-file-structure-properties-classes-static"></a>Static

Static props never change, no matter the config, theme or state. **On [Components](#scss-nomenclature-types-components)** they should be replaced by [Utilities](#scss-nomenclature-types-utilities), or [Objects](#scss-nomenclature-types-objects) where it makes sense to do so.  
For more information, see [§Composition.ClassTypes.Utilities](#scss-composition-utilities).

Static props often include [**abstract** properties](#scss-file-structure-properties-order-abstract), such as `overflow`, `display`, etc; however, any prop is technically configurable. Whether a prop is configurable depends upon whether the [class structure](#scss-nomenclature-anatomy-note) using it requires it to change at all across variants, themes or states; in practice, however, an unchanging prop is assumed to be configurable unless it has a generic value, such as `margin: 0`, or `top: 100%`; but, ultimately, whether a value is generic must be determined on a case-by-case basis.

###### <a name="scss-file-structure-properties-classes-config"></a>Config

Config props are typically any **spatial** properties configurable across a [class structure's](#scss-nomenclature-anatomy-note) (potential) [variants](#scss-file-structure-variant-theme-modifiers).

Config props often include `margin`, `padding`, `font-size`, etc.

###### <a name="scss-file-structure-properties-classes-theme"></a>Theme

Theme props are typically any **visual** properties, configurable across a [class structure's](#scss-nomenclature-anatomy-note) (potential) [themes](#scss-file-structure-variant-theme-modifiers).

Unlike Config props, something considered a Theme prop is *always* a Theme prop, with rare exception, regardless of whether the [class structure](#scss-nomenclature-anatomy-note) would otherwise configure it. <a name="scss-file-structure-properties-classes-theme-inheritance"></a>Spatial prop inheritance is generally flat; whereas visual prop definitions derive through chains of successively more lower-level theme [maps](#scss-tokens), to allow for more nuanced and powerful control over the theme's expression in the UI. In short, visual props tend toward being universal, and propagate overtop the more often instance-specific spatial props.

Theme props include `border`, `line-height`, `color`, etc.

###### State

State props are any non-Config, non-Theme props configurable across a [class structure's](#scss-nomenclature-anatomy-note) states.

State props are mostly a formal categorization, crossing Static props with an analogy to Config/Theme props.

These props may or may not be implemented in a selector statically, but, if not, their configuration, by definition, will come from somewhere beyond the [class structure's](#scss-nomenclature-anatomy-note) own config or theme [maps](#scss-tokens). Consider the following map:

```scss
$cursor: (
  disabled: not-allowed,
);
```

State props often include `cursor`, `top`, etc.

##### <a name="scss-file-structure-properties-order"></a>Property Order

The headings *Abstract*, *Spatial*, *Visual* and *State* are to help illustrate a natural scheme to order properties by.

The list of properties included is certainly not exhaustive, but it should suffice to direct any future additions.

This order is to be applied per implemented [Property Class](#scss-file-structure-properties-classes). For information on implementing Property Classes, see [§FileStructure.Rules, p.5](#scss-file-structure-rules-property-classes) and [§FileStructure.GeneralStructureAndFormatting](#scss-file-structure-general).

###### <a name="scss-file-structure-properties-order-abstract"></a>Abstract

1. * `cursor`
   * `user-select`
   * `pointer-events`
2. `overflow`
3. `display`
4. etc.

###### <a name="scss-file-structure-properties-order-spatial"></a>Spatial

1. `position`
2. 1. `top`
   2. `bottom`
   3. `left`
   4. `right`
3. 1. `margin` / `margin-top`
   2. `margin-bottom`
   3. `margin-left`
   4. `margin-right`
4. 1. `padding` / `padding-top`
   2. `padding-bottom`
   3. `padding-left`
   4. `padding-right`
5. `font-size`
6. etc.

###### <a name="scss-file-structure-properties-order-visual"></a>Visual

1. 1. `border` / `border-width`
   2. `border-style`
   3. `border-color`
2. `line-height`
3. * `font-weight`
   * `font-style`
   * `text-decoration`
4. `background-color`
5. `color`
6. etc.

###### State

1. 1. `transtion` / `transition-property`
   2. `transition-duration`
   3. `transition-timing-function`
2. etc.

------

### <a href="#scss-tokens" name="scss-tokens">#</a> Token Maps

1. [Declaration Syntax](#scss-tokens-syntax)
2. [Invoking Tokens](#scss-tokens-invokation)
3. [Inheritance](#scss-tokens-inheritance)
4. [Note about `$color` map](#scss-tokens-color-note)
5. [File Structure](#scss-tokens-file-structure)

#### <a href="#scss-tokens-syntax" name="scss-tokens-syntax">#</a> Declaration Syntax

Token maps use the SASS map syntax:

```scss
$map: (
  key-1: val,
  key-2: (
    key-1: val,
  ),
);
```

#### <a href="#scss-tokens-invokation" name="scss-tokens-invokation">#</a> Invoking Tokens

Tokens are invoked through `token($map [, $token-chain])`.

##### `$token-chain`

A `$token-chain` consists of a list of strings corresponding to a continuous or discontinuous, well ordered sequence of [keys](#scss-tokens-syntax) descending levels of the `$map` parameter and pointing to some value the map may or may not hold.

The parameter is optional, or the sequence may be discontinuous, given the map make appropriate use of the `default` keyword.

###### Examples

```scss
background-color: token($theme-main);
```

```scss
color: token($theme-btn, hover type);
```

##### `default` Keyword

The `default` keyword may be used in a map declaration, at any level, to allow `token()` invokations access to values without them having specified a [key](#scss-tokens-syntax) for that level in the map. It allows simplicity in map declaration, and ease of maintainence of both inheriting maps and implementing [class structures](#scss-nomenclature-anatomy-note), as it allows for invokations to make assumptions about a map's implementation details.

There may be only one `default` keyword per set of [keys](#scss-tokens-syntax), which should be placed before any named keys.

###### Example

```scss
// SCSS
$theme-btn: (
  default: (
    fill: #00f,
  ),
  hover: (
    fill: #0f0,
  ),
);

.cn-x-btn {
  background-color: token($theme-btn, fill);
}
```

```css
// Compiled CSS
.cn-x-btn { background-color: #00f; }
```

##### Assimilation

`token()` invokations can progressively delete leading `$token-chain` links if no token is found at the end of the chain.

In combination with a map's use of the `default` keyword, assimilation allows a [class structure](#scss-nomenclature-anatomy-note) to anticipate its own needs, rather than having to wait to implement selectors until the design, or design tokens, have been fully implemented.

###### Example

```scss
// SCSS

// map with 2/3 states implemented
$theme-btn: (
  default: (
    fill: #00f,
  ),
  hover: (
    fill: #0f0,
  ),
);

// 3/3 implementing selectors
.cn-x-btn {
  background-color: token($theme-btn, fill);
  &:hover {
    background-color: token($theme-btn, hover fill);
  }
  &:active {
    background-color: token($theme-bth, active fill);
  }
}
```

```css
// Compiled CSS
.cn-x-btn { background-color: #00f; }
.cn-x-btn:hover { background-color: #7f7fff; }
.cn-x-btn:active { background-color: #00f; } // gracefully fails to value at 'ø fill'
```

In the preceding example, `active fill` is effectively equivalent to `fill`.

###### Note

If a `$token-chain` is thought to require progressive deletion not from its start but from some arbitrary point in the middle, the map has likely become too big and/or its tree does not organize properties correctly and should be refactored. Consider the following malformed, and corrected, examples:

Malformed Assimilation example

```scss
// SCSS
$theme-btn: (
  default: (
    default: (
      fill: #00f,
    ),
    hover: (
      fill: magenta,
    ),
  ),
  secondary: (
    default: (
      fill: #0f0,
    ),
  ),
);

.cn-x-btn {
  &--t-primary {
    background-color: token($theme-btn, fill);
    &:hover {
      background-color: token($theme-btn, hover fill);
    }
  }
  &--t-secondary {
    background-color: token($theme-btn, secondary fill);
    &:hover {
      background-color: token($theme-btn, secondary hover fill);
    }
  }
}
```

```css
// Compiled CSS
.cn-x-btn--t-primary { background-color: #00f; }
.cn-x-btn--t-primary:hover { background-color: magenta; }

.cn-x-btn--t-secondary { background-color: #0f0; }
.cn-x-btn--t-secondary:hover { background-color: magenta; } // Uses primary btn token
```

Corrected Assimilation example

```scss
// SCSS
$theme-btn-primary: (
  default: (
    fill: #00f,
  ),
  hover: (
    fill: magenta,
  ),
);
$theme-btn-secondary: (
  default: (
    fill: #0f0,
  ),
);

.cn-x-btn {
  &--t-primary {
    background-color: token($theme-btn-primary, fill);
    &:hover {
      background-color: token($theme-btn-primary, hover fill);
    }
  }
  &--t-secondary {
    background-color: token($theme-btn-secondary, secondary fill);
    &:hover {
      background-color: token($theme-btn-secondary, secondary hover fill);
    }
  }
}
```

```css
// Compiled CSS
.cn-x-btn--t-primary { background-color: #00f; }
.cn-x-btn--t-primary:hover { background-color: magenta; }

.cn-x-btn--t-secondary { background-color: #0f0; }
.cn-x-btn--t-secondary:hover { background-color: #0f0; } // Uses own fallback
```

#### <a href="#scss-tokens-inheritance" name="scss-tokens-inheritance">#</a> Inheritance

Maps should be made to use other maps as a tree branches, with higher-level maps (higher with respect to configuration specificity - lower with respect to being the interface between tokens and implementing [class structures](#scss-nomenclature-anatomy-note)) referencing lower-levels, where it makes sense to do so.

Inheritance is **not arbitrary** but should be an explicit representation of underlying patterns in the design of the UI.

For a note contrasting typical [spatial](#scss-file-structure-properties-order-spatial) and [visual](#scss-file-structure-properties-order-visual) prop inheritance, see [§FileStructure.Properties.PropertyClasses.Theme, p.2](#scss-file-structure-properties-classes-theme-inheritance).

##### Example

```scss
// SCSS
$color: (
  blue: (
    default: #00f,
    50: lighten(#00f, 0.5),
	),
  green: (
    default: #0f0,
    50: lighten(#0f0, 0.5),
  ),
);

$theme-main: (
  default: (
    default: token($color, blue),
    highlight: token($color, blue 50),
  ),
  secondary: (
    default: token($color, green),
    highlight: token($color, green 50),
  ),
);

$theme-btn: (
  default: (
    fill: token($theme-main),
  ),
  hover: (
    fill: token($theme-main, highlight),
  ),
);

//...

.cn-x-btn {
  background-color: token($theme-btn, fill);
  &:hover {
    background-color: token($theme-btn, hover fill);
  }
}
```

```css
// Compiled CSS
.cn-x-btn { background-color: #00f; }
.cn-x-btn:hover { background-color: #7f7fff; }
```

#### <a href="#scss-tokens-color-note" name="scss-tokens-color-note">#</a>  Note about `$color` map

The `$color` map is the most widely inherited map and most often the ultimate root. It does not aim to make sense of the colours it holds but merely indexes them for use by other maps, leaving semantic concerns to any descendants.

Only in rare and exceptional cases is this map ever accessed by a higher-level map directly, and this should be avoided at all costs, and [inheritance](#scss-tokens-inheritance) via lower-level maps leveraged instead.

#### <a href="#scss-tokens-file-structure" name="scss-tokens-file-structure">#</a> File Structure

##### General Structure and Formatting

Theme maps should be prefixed with the word "theme": `$theme-map`.

Maps should have trailing commas (the last key-value pair is ended with a comma).

Maps should be organized within sections reflecting the [directory structure](#scss-directory-structure), including Principals, Objects, Layout, Components and Templates.

Sections should be separated by 3 lines of whitespace.

All maps and their section header should be separated by 1 line of whitespace.

Within each section, loosely related, or unrelated, maps should be given extra whitespace (3 spaces to the regular 1).

```scss
//**
// [File and map type]
//**



//**
// Principals
//**

$first-map: (
  default: (
    default: val,
    key: val,
  ),
  key: val,
);

$second-map: (
  default: val,
);



$third-unrelated-map: (
  default: val,
);



//**
// Objects
//**

//...

```

##### Order

Maps should be ordered from low-level (abstract) to high-level (concrete), and should generally end up reflecting the order of the directory `main.scss` [import order](#scss-directory-structure-import-order). Assuming inheritance is well implemented, any deviated order would fail to compile.

------

### <a href="#scss-directory-structure" name="scss-directory-structure">#</a> Directory Structure

1. [Organization](#scss-directory-structure-organization)
2. [Import Order](#scss-directory-structure-import-order)
3. [Notes on specific files](#scss-directory-structure-files)

#### <a href="#scss-directory-structure-organization" name="scss-directory-structure-organization">#</a> Organization

```
- [root]/
|- main.scss
|- principals/
|  `- _mixins.scss
|- objects/
|- components/
|- templates/
|- supplemental/
`- utilities/
```

All files are imported into `main.scss`. See [§ImportOrder](#scss-directory-structure-import-order) below.

All files except the `main.scss` should be prefixed with an underscore.

Filename words should be hyphenated, and all characters made lowercase.

If using sub-directories, each should contain its own `_main.scss` partial to be imported into the main file proper, into which it would import its own 'sub-partials'.

#### <a href="#scss-directory-structure-import-order" name="scss-directory-structure-import-order">#</a> Import Order

##### General `main.scss` File Structure

```scss
// resets and third-party resources, incl. external fonts

// Principals
@import "principals/mixins";
@import "principals/theme";
@import "principals/config";

@import "principals/transitions";
@import "principals/base-styles";


// Objects
@import ...


// Layout
@import ...

  
// Components
@import ...


// Templates
@import ...


// Supplemental
@import "supplemental/print";
@import "supplemental/hacks";

  
// Utilities
@import ...

```

When importing, do not include any underscores `_` or file extensions (e.g. `.scss`, `.css`, etc.).

For information on Objects, Components and Utilities, see [§Nomenclature.ClassTypes](#scss-nomenclature-types).

For information on Layout and Templates, see §Notes below.

##### Notes

###### Layout

Layout contains any [Objects](#scss-nomenclature-types-objects) designed for organizing other [class structures](#scss-nomenclature-anatomy-note) on the page.

###### Templates

Templates contains any page-level [Components](#scss-nomenclature-types-components).

###### Utilities

[Utilities](#scss-nomenclature-types-utilities) are added in reverse order; whereas the rest of the file is ordered from low to high specificity, utilities must be ordered high to low. This may seem counter-intuitive, but does make sense as general utilities should have a higher priority over specific ones. If in some apparently problematic use-case this seems not to be the case, the use-case ought to be reconsidered.

#### <a href="#scss-directory-structure-files" name="scss-directory-structure-files">#</a> Notes on specific files

##### <a name="scss-directory-structure-files-base-styles"></a>Base styles

The base styles file is intended to include any tag-specific resets or initializations. Consider the following:

```scss
* {
  box-sizing: border-box;
}
body {
  font-family: token($theme-font);
}
i {
  font-style: italic;
}
img,
video {
  max-width: 100%;
}
```

##### Theme and Config

These files consolidate all [token maps](#scss-tokens). For information about organization of maps within these files, see [§TokenMaps.FileStructure](#scss-tokens-file-structure).

##### Hacks

Hacks are last-minute additions which are not taken into account by the rest of the architecture and should be refactored with due consideration at the first opportunity. It is important to separate these selectors to avoid losing, first, their individual occurrences and, ultimately, at the extreme, control over the architecture.







------

## <a href="#vue" name="vue">#</a> Vue

1. [Component Instance Structure and Formatting](#vue-component-instance)
   1. [Rules](#vue-component-instance-rules)
   2. [Attribute Order](#vue-component-instance-attribute-order)
   3. [Class Attribute](#vue-component-instance-class-attribute)

------

### <a href="#vue-component-instance" name="vue-component-instance">#</a> Component Instance Structure and Formatting

1. [Rules](#vue-component-instance-rules)
2. [Attribute Order](#vue-component-instance-attribute-order)
3. [Class Attribute](#vue-component-instance-class-attribute)

#### <a href="#vue-component-instance-rules" name="vue-component-instance-rules">#</a> Rules

Attributes should be listed along the length of the file, with each attribute on its own line, and a line of white space separating each [attribute class](#vue-component-instance-attribute-order-classes).

Listing attributes ([illustrated](#vue-component-instance-illustration) below) applies unless the attribute list is sufficiently small as to warrant no white-space separation between attribute groups. In some cases an attribute list may be so small as even to warrant no line breaks; however in such cases the list consists typically of no more than about 3 attributes. It **always** applies, however, when a tag's [class list](#vue-component-instance-class-attribute) has multiple [class groups](#vue-component-instance-class-attribute-groups).

Additional whitespace may be used sparingly to punctuate large or diverse groups of attributes.  

Directives should be placed specially on the same line as the tag name.

```vue
<tag v-if="condition"
	...
```

For `<transition>` tags, the name attribute may be placed similarly to directives.

```vue
<transition name="name"
	...
```

All attribute values should be surrounded by quotes.

Attributes whose values are strings should not use `v-bind:` or its short-hand `:` with the value quoted as a string.

The closing bracket of the opening tag should be placed on the line following the attribute list, and at the same indentation depth as the tag name itself, rather than immediately after the last attribute or at some other indentation. This is critical for at-a-glance legibility, and provides text-editors the option to collapse attribute lists and contained content independently of each other. See the following malformed, and corrected, examples:

Malformed formatting example

```vue
<div
	attribute="value">
  ...
</div>
```

Corrected formatting example

```vue
<div
  attribute="value"
>
  ...
</div>
```

Tags with only textual content should open and close on the same line.

```vue
<span
  attribute="value"
>Textual Content</span>
```

#### <a href="#vue-component-instance-attribute-order" name="vue-component-instance-attribute-order">#</a> Attribute Order

Attributes, like [properties](#scss-file-structure-properties-order), are ordered according to class, but in some cases supporting properties are grouped as with `v-model`; whereas [properties are organized](#scss-file-structure-properties-order) from abstract to concrete to aesthetic, attributes are organized analogously, by functionality, from crucial to noncrucial, by which `class` is categorized as *technically* functional but *exceptionally* noncrucial.

##### <a name="vue-component-instance-attribute-order-classes"></a>Attribute Classes

###### Directives

1. 1. `v-for`
   2. `key`
2. `v-if`
3. `v-show`
4. etc.

###### `is`

###### `ref`

###### Input Events and Supporting Attributes

1. `v-model`
2. `value`
3. `@change`
4. `@input`
5. `disabled`
6. `aria-disabled`
7. etc.

###### Active Events

1. any custom active events
2. `click`
3. * `mousedown`
   * `mouseover`
   * `mouseout`
   * etc.
4. `hover`
5. * `keydown`
   * `keyup`
   * etc.
6. etc.

###### Passive Events

1. any custom passive events
2. `focus`
3. `blur`
4. etc.

###### Component Props

Any props essential for the component to function, e.g. a table row's `:row` data.

###### Essential Accessbility Implementations

1. `id`
2. `for`
3. `name`
4. `type`
5. any attributes setting constraints upon an input, e.g. `min`, `rows`, etc.
6. `required`
7. `autocomplete`
8. `placeholder`
9. etc.

###### Provisional Accessibility Implementations

1. `tabindex`
2. `role`
3. `title`
4. `alt`
5. `aria-hidden`
6. 1. `aria-labelledby`
   2. `aria-describedby`
   3. etc.
7. 1. `aria-haspopup`
   2. `aria-owns`
   3. `aria-expanded`
   4. `aria-activedescendant`
   5. etc.
8. etc.

###### Aesthetic

1. any props controlling internal class states, including variants and themes
2. any sub-component class list props
3. etc.
4. `class`

##### <a name="vue-component-instance-illustration"></a>Illustration

```vue
<tag v-for="(item, index) in items" :key="index" v-if="condition"
  :is="tag"
  ref="ref"
  v-model="model"
  @change="onChange"
  :disabled="disabled"

  @click="onClick"
	@keydown.enter="onEnter"
	...

	@focus="onFocus"
	@blur="onBlur"
	...

	:componentProp="componentProp"
	...

	:id="id"
	:type="type"
	:autocomplete="autocomplete"
	:placeholder="placeholder"
	...

	:tabindex="tabindex"
	:aria-labelledby="label"
	aria-haspopup="true"
	...

	:componentClassList="componentClassList"
	:class="['static class list',
          componentPropClassList,
          { 'conditional class list': classCondition }]"
>
  ...
</tag>
```

#### <a href="#vue-component-instance-class-attribute" name="vue-component-instance-class-attribute">#</a> Class Attribute

##### <a name="vue-component-instance-class-attribute-groups"></a>Class Groups

###### Class Group Order

1. Static Classes
2. Dynamic [Modifiers](#scss-nomenclature-modifiers)
3. Prop Class Lists
4. Conditional Class Lists

###### Static Classes

Static classes are always present on a tag.

###### Dynamic Modifiers

Dynamic [modifiers](#scss-nomenclature-modifiers), classes constructed in part from values passed into the component as props, such as those used for accessing a [class structure's](#scss-nomenclature-anatomy-note) [variants or themes](#scss-file-structure-variant-theme-modifiers), are best treated the same as Prop Class Lists after being stored in variables in the component's `data()` or `computed` objects.

###### Prop Class Lists

Prop class lists are any class lists passed into the component as props and used as is.

###### Conditional Class Lists

Conditional class lists should be ordered sensibly, from most to least crucial in its effect on a component's presentation; however, this must be determined on a component-by-component basis.

For legibility, [State Classes](#scss-nomenclature-types-state-classes) should come last, and be ordered similarly to the note in [§SCSS.FileStructure.Rules, p.4](#scss-file-structure-rules-state-declarations) on SCSS state declarations, but for organizational, rather than functional, purposes.

###### Note

When using more than one class group, the class attribute should be moved to its own line if it hasn't already, and subsequent groups should indent to the same level as within the opening quotation mark (see the [illustration](#vue-component-instance-class-attribute-illustration) below).

##### Class Order

In general, within each [class group](#vue-component-instance-class-attribute-groups), classes should be ordered by [class type](#scss-nomenclature-types), and, within each type, from highest implementation specificity to lowest:

1. [Components](#scss-nomenclature-types-components)
2. [Objects](#scss-nomenclature-types-objects)
   1. Higher-order
   2. Lower-order
3. [Utilities](#scss-nomenclature-types-utilities)
   1. Higher-order
   2. Lower-order

(For the distinction between higher- and lower-order [class structures](#scss-nomenclature-anatomy-note), see [§Nomenclature.Anatomy.Note, p.2](#scss-nomenclature-anatomy-note-orders).)

[Modifiers](#scss-nomenclature-modifiers) should be listed immediately after the [class structure's](#scss-nomenclature-anatomy-note) block class.

An extra space may be added between each [class type](#scss-nomenclature-types) if the number of classes belonging to any type is greater than 1.

##### <a name="vue-component-instance-class-attribute-illustration"></a>Illustration

```vue
<tag
  :class="['cn-c-component  cn-o-object cn-o-object--modifier  cn-u-p-v cn-u-p2-v',
          variantClassList,
          componentPropClassList,
          { 'cn-is-open': isOpenCondition },
          { 'cn-is-disabled': isDisabledCondition }]"
>
  ...
```
