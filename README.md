#Fluidizer
Fluidizer is a Sass framework which purpose is to help in the development of fluid web pages. It tries to make life easier for developers and allow them to think in pixels and get percentage horizontal and em's vertical layouts. It simplifies as well the process of dealing with grids and baselines, allowing developers to think in "grid units" and "baseline units" and get corresponding fluid values.

##Features
* Set pixel size and containing block reference, get percentage size.
* For grids, set number of columns and gutters a box spread and its containing block reference, get percentage size.
* Optionally, keep record of containing block width's in a visual way, taking advantage of sass nested rules.
* Set pixel size and relevant font size in pixels, get em size.
* For baseline rhythm, set number of baselines a box spread and its relevant font size, get em size.
* Optionally, keep record of relevant font sizes in a visual way, taking advantage of sass nested rules.

##Setup
Fluidizer framework requires you are working with [Sass](http://sass-lang.com/ "Sass homepage").

With git, to get latest stable version:

    git clone https://github.com/laMarciana/fluidizer.git

Otherwise, download it from:

    https://github.com/laMarciana/fluidizer/zipball/master

Put the files contained in the sass folder in your sass directory and import them from your sass stylesheet:

    //For horizontal layout
    @import fluidizer-horizontal;
    //For vertical layout
    @import fluidizer-vertical;

##How it works?
###Horizontal layout
For grid based layouts, set:

    $grid-strategy: true;
    $column-width: 60px; //your grid's column width
    $gutter-width: 20px; //your grid's gutter width
    $columns: 12; //your grid's number of columns

For non-grid based layouts, set:

    $grid-strategy: false;
    $layout-max-width: 960px; //your layout max-width

Set the wrapper element which will contain your page layout:

    body {
      @include set-layout-max-width();
    }

This will set your grid or layout max-width to body element in em's, having 16px as the relevant font size. If you want to change the relevant font size, change $rfs parameter like in `set-layout-max-width($rfs: 21px)`. If you want to use pixels, set the $em parameter to false, like in `set-layout-max-width($em: false)`.

You can use the `percent($h-size, $cbw-ref)` function to calculate percentage. Its first argument is the size you want to be transformed, the second one is a reference of containing block width.

You can get percentage sizes from two kinds of units:

**Pixel units:** i.e. 100px.

    margin-left: percent(100px, 400px);

**Grid units:** i.e. 3 2, meaning 3 times column width plus 2 times gutter width.

    margin-left: percent(3 2, 400px);

You can provide containing block width in two ways:

**Manually**, in pixel or grid units.

    margin-left: percent(100px, 4 4);

**As a visual reference**. 0 means parent element is root element (which width is grid or page width), 1 means parent element is first descendant of root element you have already setted and so on. -1 means new element is descendant of last element setted. This is specially visual when used in combination with sass nested rules feature.

    margin-left: percent(100px, 1);

**IMPORTANT**: when using the visual reference notation, width sizes must me setted with the `percent-width()` function.

    body {
      @include set-layout-max-width();
        div.one {
          width: percent-width(3 3, 0); //Descendant of root element. Set new width
          margin: 0 percent(0 0.5, 0); //Descendant of root element
          div.two {
            padding-left: percent(10px, 1); //Descendant of first root descendant
          }
          div.three {
            padding-right: percent(10px, 1); //Descendant of first root descendant
          }
        }
    }

You can use as well two mixins to work it quicker:

    fluid-horizontal($width, $padding, $border, $margin, $cbw-ref)

and

    fluid-column($width, $padding, $border, $margin, $cbw-ref)

The only difference is that `fluid-column()` will add `float:left;` to make the box appear as a column.

As with percent functions, you can indicate sizes in pixels or grid units. `$padding`, `$border` and `$margin` can be one value to represent same value for left and right directions, or two values first one for left and second one for right. Their value can be as well a string to manually force a value or false if you want to not be setted. `$cbw-ref`, as in functions, can be assigned manually or by a visual reference.

    div.one {
      @include fluid-column($width: 3 3, $padding: 0 2, $border: "1px", $margin: (0 1) auto, $cbw-ref: 0);
        div.two {
          @include fluid-horizontal($width: 50px, $margin: auto, $cbw-ref: 1);
        }
    }

###Vertical layout
Set initial relevant font size, usually browser default:

    $initial-rfs: 16px;

If using baseline rhythm, set baseline height:

    $baseline: 18px;

You can use the `em($v-size, $rfs-ref)` function to calculate em's. Its first argument is the size you want to be transformed, the second one is a reference of relevant font size.

You can get em sizes from two kinds of units:

**Pixel units:** i.e. 21px

    height: em(21px, 16px);

**Baseline units:** i.e. 2, meaning 2 times baseline height.

    height: em(2, 16px);

You can provide relevant font size in two ways:

**Manually**, in pixels

    height: em(21px, 16px);

**As a visual reference**. 0 means relevant font size is `$initial-rfs`, 1 means relevant font size is the second relevant font size setted and so on. -1 means relevant font size is last one setted. This is specially visual when used in combination with sass nested rules feature.

    height: em(21px, 0);

**IMPORTANT**: when usign the visual reference notation, new font sizes must be setted with the `em-font-size()`function.

    $initial-rfs: 16px;
    body {
      div.one {
        font-size: em-font-size(18px, 0); //Set a new relevant font size
        height: em(12, 1); //Relevant font size is first one setted after the $initial-rfs
          div.two {
            line-height: em(29px, 1); //Relevant font size is first one setted after the $initial-rfs
          }
      }
      div.three {
        padding-top: em(29px, 0); //Relevant font size is $initial-rfs
      }
    }

You can use as well a mixin to work it quicker:

    fluid-vertical($line-height, $height, $padding, $border, $margin, $rfs-ref)

As with the em functions, you can indicate sizes in pixels or baseline units. `$padding`, `$border` and `$margin` can be one value to represent same value for left and right directions, or two values first one for left and second one for right. Their value can be as well a string to manually force a value or false if you want to not be setted. `$rfs-ref`, as in functions, can be assigned manually or by a visual reference.

    div.one {
      @include fluid-vertical($line-height: 2, $padding: 1, $margin: auto, 0);
    }

##Reference

Variables, functions and mixins are full documented in source code.

___

Copyright 2011, Marc Busqué Pérez, under GNU LESSER GENERAL PUBLIC LICENSE
marc@lamarciana.com - http://www.lamarciana.com
