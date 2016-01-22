_These guidelines are written with Zurb Foundation in mind, but are good practices regardless of front-end framework._

1. [Formatting](#formatting)
2. [Naming](#naming)
3. [Rules](#rules)
4. [Scoping](#scoping)
5. [Browser Specific Rules](#browser-specific-rules)
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

###<a name="browser-specific-rules"></a>5. Browser Specific Rules

###<a name="right-to-left"></a>6. Right To Left

###<a name="variables"></a>7. Variables

###<a name="commenting"></a>8. Commenting

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
