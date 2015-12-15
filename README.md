# Using Sass & Foundation in Rails

## Sass

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

That is all I'm going to show you for today. You can check out these resources for more:

- [Sass for Web Designers](http://esdwebtestlink.com/PDFs/ABookApart/Sass-for-Web-Designers.pdf) - great book on Sass
- [Previous clinic notes of mine](https://gist.github.com/cmkoller/fadb5e1d177aa209d0c2)
- [Sassmeister](http://sassmeister.com/) - Tool for messing around w/ Sass and seeing it turn into CSS
- Alex's future clinic on more Sass stuff

## Foundation

You should be using Foundation in pretty much all your projects.

### Installing Foundation in Rails

You do NOT copy and paste files anymore! Instead, do the following:

- Add `gem 'foundation-rails'` to your Gemfile
- Run `bundle install`
- In the command line, run `rails g foundation:install` (aka `rails generate foundation:install` - `g` is short for `generate`)
- Type `y` to authorize Foundation to override your `application.html.erb` file

Bam! Foundation installed.

You'll now see the following changes:

- Your `application.html.erb` file now has some extra asset links inside
- Your `application.js` file now loads and initializes Foundation's js
- Your `application.css` file now has the line `require foundation_and_overrides`
- You now have a fancy new file called `foundation_and_overrides.scss`!

### IMPORTANT (temporary) NOTE!

Foundation 6 is tragically NOT available as a gem yet! **You must use [THESE DOCS](http://foundation.zurb.com/sites/docs/v/5.5.3/) for reference with your Rails apps,** or else

![](https://cessnachick.files.wordpress.com/2015/08/youre-going-to-have-a-bad-time.png)

### Using `foundation_and_overrides`

`foundation_and_overrides.scss` looks like this:

1. A table of contents
2. A ton of comments
3. The line `@import 'foundation';`

So how do we use it?

These comments are all variables that Foundation uses to determine how things are going to look. In this file, you can both **see what Foundation's default values are** for each of these variables, and **override any value** by uncommenting the line and putting your own value in instead.

## Using Foundation

Resources:

- [Foundation clinic notes](https://gist.github.com/cmkoller/7ccb2a2f9ec48ee71471) (<-- These notes are for Foundation 5, like you'll be using!)
- [Foundation docs](http://foundation.zurb.com/sites/docs/v/5.5.3/)
