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

I came to conclusion that it is ALWAYS better to write blocks as pure classes. Problems matter when composing mixins which represent blocks. E.g. we cannot have two block mixins each under other, so we may think about list of blocks passed to mixin. Ok, but what if we also want specify namespaces, which may also be different per block or even absent for some blocks?

Nevermind... Looks like unnecessary work at all.

```
%TestPlaceholder
  font-weight: bold

.Block
  color: red
  +e(elementA elementB)
    color: green
    +m(modifierA modifierB)
      color: blue
  +m(modifierA modifierB)
    color: red
    +e(element)
      color: green
      +m(modifier)
        @extend %TestPlaceholder // Stylus can't do this without ugly prefix :)
        color: blud
```

Will produce:

```
.Block--modifierA .Block-element--modifier, .Block--modifierB .Block-element--modifier {
  font-weight: bold;
}

.Block {
  color: green;
}
.Block-elementA, .Block-elementB {
  color: green;
}
.Block-elementA--modifierA, .Block-elementA--modifierB, .Block-elementB--modifierA, .Block-elementB--modifierB {
  color: green;
}
.Block--modifierA, .Block--modifierB {
  color: green;
}
.Block--modifierA .Block-element, .Block--modifierB .Block-element {
  color: green;
}
.Block--modifierA .Block-element--modifier, .Block--modifierB .Block-element--modifier {
  color: green;
}
```
