# Bem class mixins

Minimalistic core. Supports mixins: `+m(<list of modifiers>)` and `+e(<list of elements>)` with nest-in-state support.

## Usage

### Install

`npm i -S bem-sass-mixins`

### Include

*Webpack*

`@import '~bem-sass-mixins/bem-sass-mixins'` (Sass 4.0 will give us [improved resolving](https://github.com/sass/sass/issues/690))

*Gulp*

See [gulp-sass options](https://github.com/dlmanning/gulp-sass#options). You'll need `includePaths` option to resolve `@import 'bem-sass-mixins'` (from node_modules/bemsass-mixins) or `@import 'bem-sass-mixins/bem-sass-mixins'` (from node_modules/). Looks simple enough to configure.

### Configure

Define preferrable separators in your variables file like `$bemSeparators: (element: '-', modifier: '--')` (these are mine for .MyBlock-myElement--myModifier style) or rely on defaults `__` and `--`.

### Use

I came to conclusion that it was ALWAYS better to write blocks as pure classes. Problems happen when composing mixins which represent blocks. E.g. we cannot have two block mixins each under another, so we may think about list of blocks passed to mixin. Ok, but what if we also want to specify namespaces, which may also be different per block or even absent for some blocks?

Nevermind... Looks like unnecessary work at all.

```
.Block
  color: red
  +e(element1 element2)
    color: green
    +m(elementModifier1 elementModifier2)
      color: blue
  +m(blockModifier1 blockModifier2)
    color: red
    +e(element)
      color: green
      +m(elementModifier)
        @extend %TestPlaceholder // Stylus can't do this without ugly prefix :)
        color: blue
```

Will produce:

```
.Block {
  color: red;
}
.Block-element1, .Block-element2 {
  color: green;
}
.Block-element1--elementModifier1, .Block-element1--elementModifier2, .Block-element2--elementModifier1, .Block-element2--elementModifier2 {
  color: blue;
}
.Block--blockModifier1, .Block--blockModifier2 {
  color: red;
}
.Block--blockModifier1 .Block-element, .Block--blockModifier2 .Block-element {
  color: green;
}
.Block--blockModifier1 .Block-element--elementModifier, .Block--blockModifier2 .Block-element--elementModifier {
  color: blue;
}

.Block--blockModifier1 .Block-element--elementModifier, .Block--blockModifier2 .Block-element--elementModifier {
  font-weight: bold;
}
```
