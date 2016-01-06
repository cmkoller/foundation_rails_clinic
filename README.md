# All About Sass

*Sass == "Syntactically Awesome Stylesheets"*

Sass is a CSS extension language. It builds on top of CSS, fixes some of its problems, and generally makes it way better.

###What's wrong with CSS?

It's so, so not DRY:
- You often need to repeat big lines of selectors as you target more and more deeply nested items
- You can't store things as variables, so if you want to keep multiple things the same color, you need to go change them one-by-one everywhere in the file
- Big chunks of style often end up getting repeated in different places
- And on, and on...

###Enter Sass!

Which will solve all your problems in life (okay, many of them) and also make you look cool in the process for using a CSS preprocessor.

##Setting it up

Luckily for you, if you're in a Rails app, it's already set up! Here's what you do to get started:

1. Go to your `/app/assets/stylesheets` folder
2. Make a file called `<your-name>.scss`
3. Write CSS in it

You're done! Congrats, you have written valid Sass.

##How Sass is read

When Sass is processed by your app, it gets *compiled* into a normal CSS file, which is then read by your browser. It's important to understand how Sass syntax gets translated into normal CSS syntax, so most of these code examples will show both the Sass and then the CSS it compiles to.

##Some Resources

- [Sassmeister](http://www.sassmeister.com/)
- [Awesome Sass Book](http://esdwebtestlink.com/PDFs/ABookApart/Sass-for-Web-Designers.pdf)

##Things you can do with Sass

First off, all CSS is also valid SCSS. So you can start out by writing CSS like you normally would, and add these pieces in slowly!

###Variable Definition

```scss
$main-color: #ff0000;
$font-sans: "Proxima Nova", "Helvetica Neue", Helvetica, Arial, sans-serif;

body {
  background-color: $main-color;
  font-family: $font-sans;
}
```

This will compile to the following CSS:

```scss
body {
  background-color: #ff0000;
  font-family:  "Proxima Nova", "Helvetica Neue", Helvetica, Arial, sans-serif;
}
```

###Loading Files Differently

By default, Rails uses something like this to load in its files:

```scss
/*
 *= require foundation_and_overrides
 *= require_tree .
 *= require_self
*/
```

This loads in each file separately in a specific order - first `foundation_and_overrides`, then all the other files in the stylesheets folder, then the file we're currently looking at (`application.scss`).

This is fine for the most part, but unfortunately means that since each file is getting treated as separate, any variables you declare in one file will *not* be visible in any other file. Let's change that!

Sass allows us to use `@import 'filename';` to load one Sass file into another Sass file. We can update our `application.scss` file to look like so:

```scss
@import 'foundation_and_overrides';
@import 'any_other_file_we_have';
```

This syntax will basically copy-and-paste the entire contents of `foundation_and_overrides`, and place it in `application.scss` where we have our `@import` command. Then it'll do the same with the next file that we `@import`, and the next, etc. Now all variables defined in `foundation_and_overrides` will be visible in all files that get `@import`ed after.

**Important note**: This method is **instead** of using those `require` comments. This also requires that you have an `@import` for **each stylesheet** you have in your project. Nothing will be automatically loaded any more.

###Nesting Selectors

If you're trying to reference one HTML object within another, you can do it like this:

```scss
ul {
  margin: 1em 0;
  border: 1px solid black;

  li {
    padding: 3em 1em;
  }
}
```

This piece of code will get compiled to this CSS:

```scss
ul {
  margin: 1em 0;
  border: 1px solid black;
}

ul li {
  padding: 3em 1em;
}
```

**Important: it's considered bad practice to nest more than 3 layers deep.**

###Referencing Parent Selectors

Sometimes, you'll be in some nested code like above and you'll want to reference the parent selector that you're currently nested inside. To do this, use the `&` symbol! `&` will get replaced with the parent selector you're currently nested within.

This can be useful when putting some styling on multiple things at once, including your parent selector:

```scss
ul {
 ...

  li, & {
    padding: 3em 1em;
  }
}
```

This will compile to:

```scss
ul {
  ...
}

li, ul {
  padding: 3em 1em;
}
```

**Another example -** it can be useful for referencing the state of your objects in a concise way:

```scss
a {
  color: blue;
  text-decoration: none;

  &:hover {
    text-decoration: underline;
  }
}
```

This will compile to:

```scss
a {
  color: blue;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
```

**Another example -** it can be useful when defining a special context within which you want different styling for your object. Here, most of the time your paragraphs will be blue, but if you're on the `store` page, they'll be red:

```scss
p {
  color: blue;

  .store & {
    color: red;
  }
}
```

This will compile to:

```scss
p {
  color: blue;
}

.store p {
  color: red;
}
```

####Quick Interlude - Darkening and Lightening Colors

Let's bring together the pieces so far with an example, that also demonstrates a fun little color-manipulation method you can use!

```scss
$button-color: #ea4c89;

button {
  background-color: $button-color;

  &:hover {
    background-color: darken($button-color, 30%);
  }
}
```

This will compile to:

```scss
button {
  background-color: #ea4c89;
}

button:hover {
  background-color: #8d1040;
}
```

###Mixins

Variables allow you to define and reuse values - **mixins** allow you to define and reuse chunks of code! (Yay, DRYness!)

```scss
@mixin title-style {
  margin: 0 0 20px 0;
  font-family: $font-serif;
  font-size: 20px;
  font-weight: bold;
  text-transform: uppercase;
}

.header h1 {
  @include title-style;
  color: #999;
}

.article h2 {
  @include title-style;
}
```

This will compile to:

```scss
.header h1 {
  margin: 0 0 20px 0;
  font-family: $font-serif;
  font-size: 20px;
  font-weight: bold;
  text-transform: uppercase;
  color: #999;
}

.article h2 {
  margin: 0 0 20px 0;
  font-family: $font-serif;
  font-size: 20px;
  font-weight: bold;
  text-transform: uppercase;
}
```

### Mixins with Arguments

You can also feed your mixins arguments! Wow, this is almost like a programming language!

```scss
@mixin title-style($color) {
  margin: 0 0 20px 0;
  font-family: $font-serif;
  font-size: 20px;
  font-weight: bold;
  text-transform: uppercase;
  color: $color;
  border: 3px solid $color;
}
```

Multiple arguments work too:

```scss
@mixin title-style($color, $background) {
  margin: 0 0 20px 0;
  font-family: $font-serif;
  font-size: 20px;
  font-weight: bold;
  text-transform: uppercase;
  color: $color;
  border: 3px solid $color;
  background: $background;
}
```

Setting an argument's defualt value:

```scss
@mixin title-style($color, $background: #eee) {
  margin: 0 0 20px 0;
  font-family: $font-serif;
  font-size: 20px;
  font-weight: bold;
  text-transform: uppercase;
  color: $color;
  border: 3px solid $color;
  background: $background;
}

section.main h2 {
  @include title-style(#c63);
}
section.secondary h3 {
  @include title-style(#39c, #333);
}
```

### Extending other classes

You can take all the styles that get applied to one class, and apply it to another class! For example, Foundation has a lot of styles that they put onto their `button` class. We can nab those and apply them to a new, custom class, with some of our own tweaks:

```scss
.green-button {
  @extend .button;
  background-color: lime;

  &:hover,
  &:focus,
  &:active {
    background-color: darken(lime, 10%);
  }
}
