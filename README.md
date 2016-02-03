_These guidelines are written with Zurb Foundation in mind, but are good practices regardless of front-end framework._

1. [Formatting](#formatting)
2. [Naming](#naming)
3. [Rules](#rules)
4. [Scoping](#scoping)
5. [Vendor Specific Rules](#vendor-specific-rules)
6. [Right To Left](#right-to-left)
7. [Variables](#variables)
8. [Commenting](#commenting)
9. [Mixins](#mixins)
10. [Headings](#headings)

###<a name="formatting"></a>1. Formatting
* Always use a single space between property and value (but no space between property and colon) for consistency reasons.
* End every declaration with a semicolon for consistency and extensibility reasons.
* Indent all block content - rules within rules - as well as declarations, so to reflect hierarchy and improve understanding.
* Always use a single space between the last selector and the opening brace (but no space after the brace) that begins the declaration block.
* The opening brace should be on the same line as the last selector in a given rule. Always put a blank line between rules.

```css
.element‐card {
  position: relative;
  width: 100%;

  &.is‐selected {
    background‐color: $background‐color‐inverted;
    color: $text‐color‐inverted;
  }

  &.is‐blank {
    display: none;
  }
}
```

###<a name="naming"></a>2. Naming
* CSS class names must be clear and contextual. Child element names must remain clear and contextual and should be specific to their parent context.
* Classes must always use `spinal-case` for word separation.
* Names that are specific and reflect the purpose of the element should be preferred as these are most understandable and the least likely to change.
* For the most part, avoid using IDs as a CSS rule and add a class instead.

```html
<div class="progress">
  <div class="meter"></div>
</div>
```

```css
.progress {
  /* Some styles */

  /* Modifier */
  &.is‐complete {
    /* Some styles */
  }

  > meter {
    /* Some styles */
  }
}
```

###<a name="rules"></a>3. Rules
Use the built in `rem-calc()` function in place of `px` values. This applies to all pixel values except visual properties such as `border-width`, `box-shadow`, `text-shadow`, `border-radius`, etc.

```css
.element {
  height: rem‐calc(20);
  padding: rem‐calc(5 10);
}

// output
.element {
  height: 1.25rem;
  padding: 0.3125rem 0.625rem;
}
```

###<a name="scoping"></a>4. Scoping
When writing SCSS styles, use scoping to avoid potential conflicts with classes that share the same name. In the example below, `.base-courses` and `.base-messages` use panels slightly different than how it is defined globally. Without proper scoping, the `.panel` would be overridden.

```css
.base‐courses {
  .panel {
    padding‐top: rem‐calc(20);
  }
}

.base‐messages {
  .panel {
    padding‐top: rem‐calc(40);
  }
}

/*
* Global component
*/
.panel {
  padding: rem‐calc(10);
}
```

Following the above scoping practices will lead to organized, self contained, modular code that's easier to move and maintain.

A case where you might consider breaking away from the modular style would be when handling keyframe animations. `@keyframes` are required to be placed at the root of your document — which would conflict with the scoping practices. You can still do this using the @at‐root directive within SCSS. In the following example the `@keyframes` will be placed at the same level as `.color‐swap`.

```css
.color‐swap {
  width: rem‐calc(100);
  height: rem‐calc(100);
  background‐color: $red;

  &:hover {
    animation: color‐swap 300ms ease-in;
  }

  @at‐root {
    @keyframes color‐swap {
      from { background‐color: $red; }
      to { background‐color: $green; }
    }
  }
}
```

###<a name="vendor-specific-rules"></a>5. Vendor Specific Rules
It's recommended to use Autoprefixer, a plugin that parses the CSS and adds specific vendor prefixes to the rules and values. If it's not used, vendor specific rules need to be added, this is easier done creating mixins to handle it. An example:

```css
@mixin display-flex() {
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
}

@mixin flex-justify-content($justify-content) {
  -webkit-justify-content: $justify-content;
  -ms-justify-content: $justify-content;
  justify-content: $justify-content;
}

.some-element {
  @include display-flex();
  @include flex-justify-content(center);
}

//output
.some-element {
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-justify-content: center;
  -ms-justify-content: center;
  justify-content: center;
}
```

Avoid writing vendor specific rules manually, it can lead to unforseen issues.

###<a name="right-to-left"></a>6. Right To Left
Only applicable for Zurb Foundation.

* Be sure to use the `$default‐float` and `$opposite‐direction` variables when applying left/right values that need to be reversed for RTL.
* This must be used on the following properties: `float`, `text‐align` as well as any others that use `left` or `right` as a value.
* Replace properties that use a `‐left` or `‐right` modifier with the variables. (`margin‐right`, `border‐right`, `padding‐right`, etc.)
* Avoid using shorthand values if they don't translate well in right-to-left. Example, don't use `padding: rem-calc(5, 10, 20, 30);`, use `padding: rem-calc(5, 10, 20);` and `padding-#{$default-float}: rem-calc(30);`

```css
.element {
  margin‐#{$default‐float}: rem‐calc(5);
}

.element‐container {
  float: $opposite‐direction;
  text‐align: $default‐float;
}

// output
.element {
  margin‐left: 0.3125rem;
}

.element‐container {
  float: right;
}
```

###<a name="variables"></a>7. Variables
Always prefer the use of SCSS variables instead of typing the values all the time. It's easier to refactor later on. Most Front End frameworks like Foundation or Bootstrap come bundled with variables. Also, create variables when a certain value will be repeated, here are some examples of common variables.

```css
$global-milliseconds: 300ms;
$global-easing: ease-in;

$background-color-inverse: #fff;
$background-color-normal: #ccc;
$background-color-dark: #222;

$font-size-large: rem-calc(18);
$font-size-lead: rem-calc(14);
$font-size-small: rem-calc(10);

$text-color: #222;
$text-color-inverse: #eee;

$weight-light: 300;
$weight-normal: 500;
$weight-semi-bold: 700;
$weight-bold: 900;
```

* Use the `$background‐x` variables when setting a `background‐color`.
* Use the `$font‐size‐x` variables when setting a `font‐size`.
* Use the `$text‐color‐x` variables when setting a `color`.
* Use the `$weight‐x` variables when setting a `font‐weight`.

```css
.element {
  transition: all $global-milliseconds $global-easing;
  background-color: $background-color-normal;
  color: $text-color;
  font-size: $font-size-lead;
  font-weight: $weight-semi-bold;
}
```

###<a name="commenting"></a>8. Commenting

* Comment frequently.
* Use a table of contents for large files with indexes mixed with appropriate section titles.
* Comments about selectors live right above the selector.
* Comments about an attribute resides on the same line as the attribute.
* Comments that separate a subsection will have an index and a short description on a single line.

```css
/**
 * Table of Contents
 *
 * 1.0 ‐ Reset
 * 2.0 ‐ Genericons
 * 3.0 ‐ Typography
 * 4.0 ‐ Elements
*/

/**
 * #.# Section title
 *
 * Description of section, whether or not it has media queries, etc.
 */

.selector {
  float: #{$default‐float};
}

// This is a comment about this selector
.another‐selector {
  position: absolute;
  top: 0 !important; // I should explain why this is so !important
}

// #.# This is a comment about this sub‐section
.a‐different‐selector {
  position: relative;
}
```

###<a name="mixins"></a>9. Mixins

###<a name="headings"></a>10. Headings
Each application layer that is opened (like a panel or modal) needs to start it's headings with ```<h1>```. This way screen reader users will have a clear understanding of where they are on the application.
Heading tags should only be used to communicate their level of importance to screenreader and not to apply styles. To apply styles to a heading tag you should create a class that applies regardless of heading level.

```css
h1, h2, h3, h4, h5, h6 {
  &.my‐heading {
    font‐family: $sans‐serif‐font‐family;
    font‐size: $font‐size‐medium;
  }
}
```
