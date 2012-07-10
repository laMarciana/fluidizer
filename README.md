# Fluidizer
Fluidizer is a Sass tool intended to help in the development of 100% fluid web sites, including fluid grid and fluid vertical rhythm designs. It allows developers to think in pixels or grid and baseline units, and to get automatically percentage values for horizontal sizes and em values for vertical ones.

## Features
* Provide pixel size and containing block's width, get percentage size.
* For grids, provide the amount of columns and gutters a size takes up and its containing block width, get percentage size.
* Provide pixel size and the relevant font size, get em size.
* For baseline rhythm, provide the amount of baseline increments a size takes up and its relevant font size, get em size.

## Setup
Fluidizer requires you are working with [Sass](http://sass-lang.com/ "Sass homepage").

With git, to get latest stable version:

    git clone https://github.com/laMarciana/fluidizer.git

Otherwise, download it from:

    https://github.com/laMarciana/fluidizer/zipball/master

Put the file `sass/_fluidizer.scss` in your sass directory and import it from your sass style-sheet:

    @import fluidizer;

## How does it work?
### Horizontal layout
#### Configuring design properties
For grid based layouts, set:

    $grid-strategy: true;
    $column-width: 60px; //your grid's column width
    $gutter-width: 20px; //your grid's gutter width
    $columns: 12; //your grid's number of columns

For non-grid based layouts, set:

    $grid-strategy: false;
    $design-width: 960px; //your design's width

#### The overall container
Optionally, set the wrapper element that will contain your whole layout. That way, when the screen resolution is equal or higher than this value, all the horizontal dimensions set with fluidizer will have percentage dimensions but, visually, they exactly will take up the amount of pixels that you indicated. For lower resolutions, all the horizontal dimensions set with fluidizer will scale.

    body {
      @include layout-max-width();
    }

*This will set your layout width as the `max-width` for the body element. By default, it is set in em units with the value of the `$initial-rfs` variable (16px if not manually changed) as the relevant font size. If you want to change the relevant font size, change the `$rfs` parameter like in `layout-max-width($rfs: 21px)`. If you want to use pixels, set the `$em` parameter to false, like in `layout-max-width($em: false)`.*

#### Getting percentage values
To get percentage values, you can use the `percent()` function, which takes two arguments: the value to be converted and the containing block width.

    percent($value, $cbw)

You can provide both the value and the containing block width arguments in two different units:

* Pixel units: Like in *10px* or *23px*.

* Grid units: A list of two unitless numbers. Firs one telling the amount of column width's the size takes up (without the gutters), and second one telling the amount of gutters the size takes up. Ex: *3 2* means 3 column width's plus 2 gutters.

Here they are some examples:

    margin-left: percent(100px, 400px); //it outputs 'margin-left: 25%'

    margin-left: percent(3 2, 400px); //for a 60*20*12 grid, it outputs 'margin-left: 55%'

    margin-left: percent(3 3, 6 6); //it outputs, 'margin-left: 50%'

### Mixins

You can use as well two mixins to work it quicker:

    fluid-x($width, $padding, $border, $margin, $cbw)

and

    fluid-column($width, $padding, $border, $margin, $cbw)

The only difference is that `fluid-column()` will add `float:left;` to make the box appear as a column.

As in the `percent()` function, you can provide each argument in pixel or grid units, and they can be as well a string to manually force something or `nil` to don't set them. For `$padding`, `$border` and `$margin`, they can be just one value that will be used both for left and right values, or a list of two values, being the first one used for the left property and the second one for the right property.

Here they are some examples:

    @include fluid-x($width: 400px, $padding: 20px, $border: 0, $margin: 60px 100px, $cbw: 600px);

    @include fluid-column($width: 4 3, $padding: (2 2) (2 2), $border: 0, $margin: auto, $cbw: 12 12);

###Vertical layout
#### Configuring design properties
Set initial relevant font size, usually browser default:

    $initial-rfs: 16px;

If using baseline rhythm, set baseline height:

    $baseline: 18px;

#### Getting em values
To get em values you can use the `em()` function, which takes two arguments: the value to be converted and the relevant font size.

    em($value, $rfs)

You can provide both the value and the relevant font size arguments in two different units:

* Pixel units: Like in *21px* or *14px*.

* Baseline units: A unitless number telling the amount of baseline heights the size takes up. Ex: *3* means 3 baseline height's.

Here they are some examples:

    font-size: em(18px, 16px); //it outputs `font-size: 1.125em`

    font-size: em(1, 18px); //for a 18px baseline, it outputs `font-size: 1em`

    font-size: em(1, 1); //it outputs `font-size: 1em`

### Mixins

You can use as well a mixin to work it quicker:

    fluid-y($line-height, $height, $padding, $border, $margin, $rfs)

As in the `em()` function, you can provide each argument in pixel or baseline units, and they can be as well a string to manually force something or `nil` to don't set them. For `$padding`, `$border` and `$margin`, they can be just one value that will be used both for top and bottom values, or a list of two values, being the first one used for the top property and the second one for the bottom property.

Here they are some examples:

    @include fluid-y($line-height: 21px, $height: 21px, $border: 0, $margin: 5px 7px, $rfs: 30px);

    @include fluid-y($line-height: 1, $height: 0.5, $rfs: 2);

## Examples
You can find code examples in the `test` directory.

## Reference
Variables, functions and mixins are fully documented in the source code.

___

Copyright 2012, Marc Busqué Pérez, under GNU LESSER GENERAL PUBLIC LICENSE
marc@lamarciana.com - http://www.lamarciana.com
