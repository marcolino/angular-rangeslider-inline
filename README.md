angular-rangeslider
===================
_Current version: 0.0.14_

Angular RangeSlider is a directive that creates an interactive slider that allows a user to change model values.

It has been styled to match form elements styled by [Twitter's Bootstrap CSS framework](http://twitter.github.io/bootstrap/).

#### Requirements

- Angular (v1.0.8+)
- jQuery (v1.7+)

Description
-----------

This directive (originally by [danielcrisp](https://github.com/danielcrisp)) was written for a project completed way back in 2013.

This directive has been improved (or just changed, you will tell ..) from the original one to be usable "inline", i.e., the minimum and maximum values are displayed on the cursors, insted of on elements external to the slider.
This is a behavoir useful when you need to use the slider, say, in a compact form.

You should be aware, though, that values will have to be of a reasonable length (say from values -999 to 999). if they are longer (say floats with some digits), teir valu will overflow the cursors borders, resulting in a sub-optimal visual effect.

The changes have been currently tested only for the horizontal version.

Installation
------------

Download the files from Github or use Bower:

    $ bower install angular-rangeslider-inline

Add the JS and CSS to your page:

    <script src="bower_components/angular-rangeslider-inline/angular.rangeSliderInline.js"></script>
    <link rel="stylesheet" href="bower_components/angular-rangeslider-inline/angular.rangeSliderInline.css">

Add the `ui-rangeSliderInline` module as a dependency for your app: `angular.module('myApp', ['ui-rangeSliderInline']);`

Bootstrap is not required.

Demo
----

 - [Demo - Angular v.1.1.*](http://marcolino.github.io/angular-rangeslider-inline/demo/)
 - [Legacy Demo - Angular v.1.0.*](http://marcolino.github.io/angular-rangeslider-inline/demo/legacy.html)

GitHub Pages
------------

[Project page](http://marcolino.github.io/angular-rangeslider-inline/)

Quick example
-------------

A basic slider with a range of 0 to 100:

    <div range-slider-inline min="0" max="100" model-min="min" model-max="max"></div>

As the handles are moved the model values `min` and `max` will be updated in the parent controller.

![Default example](screenshots/slider.png)

Options
-------

Options are set as attributes on the `<div range-slider-inline>`

### `=` two-way bindings

`min` - the minimum value the user can select (must be a number, can be a model property)

`max` - the maximum value the user can select (must be a number, can be a model property)

`model-min` - the model property for the min value, represents the position of the min handle

`model-max` - the model property for the max value, represents the position of the max handle

`disabled` - model property or boolean, disables the slider when `true`

### `@` attributes

`orientation` - slider orientation, default: 'horizontal' - options: 'horizontal' | 'vertical' | 'vertical left' | 'vertical right'

`step` - amount to change the value by when moving a handle, default: 0

`decimal-places` - the number of decimal places to round to, default: 0

`filter` - a built-in filter to apply to the displayed values, for example `currency` or `currency:'$'`

`filter-options` - options to pass to the filter

`pin-handle` - disable/hide one handle, default: null - options: 'min' | 'max'

`prevent-equal-min-max` - prevent the `min` and `max` values from being equal. The `step` value is used to set the minimum difference, otherwise a value of `1` is used.

`attach-handle-values` - move the value labels in sync with the slider handles when `true`, default: `false`

Some more examples
------------------

### Using model properties

The following properties are present in the scope:

    // set available range
    $scope.minPrice = 100;
    $scope.maxPrice = 999;
    
    // default the user's values to the available range
    $scope.userMinPrice = $scope.minPrice;
    $scope.userMaxPrice = $scope.maxPrice;
    
So we can include the directive in the HTML like this:

    <div range-slider-inline min="minPrice" max="maxPrice" model-min="userMinPrice" model-max="userMaxPrice" step="5"></div>

As the user moves the min and max handles the `userMinPrice` and `userMaxPrice` will be updated in increments of `5` in real-time in the model.

### Using filters

Continuing from the example above we can format the values displayed to the user as currency.

    <div range-slider-inline min="minPrice" max="maxPrice" model-min="userMinPrice" model-max="userMaxPrice" step="5" filter="currency"></div>

This will automatically be localised by Angular, but we can force it to be USD by passing this as an option:

    <div range-slider-inline min="minPrice" max="maxPrice" model-min="userMinPrice" model-max="userMaxPrice" step="5" filter="currency" filter-options="USD$"></div>

Alternatively you can use Angular's filter notation directly in the `filter` attribute, such as `filter="currency:'GBP £'"`, which would result in values like this: `GBP £7,500.00`.

    <div range-slider-inline min="minPrice" max="maxPrice" model-min="userMinPrice" model-max="userMaxPrice" step="5" filter="currency:'GBP £'"></div>

**NOTE:** If the `filter-options` attribute is defined you **cannot** use Angular filter notation. You must only use the filter name in the `filter` attribute.

![Currency example](screenshots/currency.png)

### Making the slider vertical

Simply add one of the following values to the `orientation` attribute: 'vertical', 'vertical left' or 'vertical right'.

This will create a vertical slider that is centred in it's parent element:

    <div range-slider-inline min="0" max="100" model-min="min" model-max="max" orientation="vertical"></div>

![Vertical example](screenshots/vertical.png)

To left-align the slider use 'vertical left':

    <div range-slider-inline min="0" max="100" model-min="min" model-max="max" orientation="vertical left"></div>
    
And to right-align the slider use 'vertical right':

    <div range-slider-inline min="0" max="100" model-min="min" model-max="max" orientation="vertical right"></div>

### Disabling the slider

If you have a boolean property in your scope you can simply change this value to `true` to disable the slider:

    $scope.sliderDisabled = false;

And then specify the property using the disabled attribute:

    <div range-slider min="0" max="100" model-min="min" model-max="max" disabled="sliderDisabled"></div>
    
    // clicking this button will toggle the sliderDisabled value between true and false
    <button ng-click="sliderDisabled=!sliderDisabled">Toggle slider disabled status</button>
    
![Disabled example](screenshots/disabled.png)

### Pinning a handle

If you would like only allow setting one value, effectively creating a single-value silder, set the pin-handle attribute to 'min' or 'max'. You may then omit the corresponding model-xxx property:

    <div range-slider-inline min="0" max="100" model-max="max" pin-handle="min></div>
    
![Pinned example](screenshots/pinned.png)

### Move values with handles

Set the attach-handle-values attribute to true to have the values move with the slider handles. This works for either horizontal:

    <div range-slider-inline min="0" max="100" model-min="min" model-max="max" attach-handle-values="true"></div>
![Attached handles horizontal example](screenshots/attached-handles-horizontal.png)

or vertical:

    <div range-slider-inline min="0" max="100" model-min="min" model-max="max" attach-handle-values="true" orientation="vertical"></div>
![Attached handles vertical example](screenshots/attached-handles-vertical.png)

Angular 1.0.* Support
---------------------

If you use this directive with an older version of Angular (e.g. v1.0.*) then the directive automatically detects this and switches on legacy support.

This basically changes the optional isolate scope properties `disabled`, `modelMin` and `modelMax` so that they are no longer optional and must always be defined on the directive element.

So, this would give an error (`Error: Non-assignable model expression: undefined (directive: rangeSliderInline)`) if you were using Angular v1.0.8:

    <div range-slider-inline min="0" max="100" model-min="demo1.min" model-max="demo1.max"></div>

However this would work correctly:

    <div range-slider-inline min="0" max="100" model-min="demo1.min" model-max="demo1.max" disabled="false"></div>

Note, only tested in v1.0.8

To Do
-----

- Remove full jQuery dependency
- Improve behaviour when model values are not valid (e.g. min is greater than max)
- Improve the dev architecture (add jshint, tests, minification, auto-versioning etc)


Known Issues
------------

 - The slider restricts the model value when editing outside the slider (e.g. in an <input>) but the only notification is made to the `console`
 - The min slider handle disappears behind the max slider handle


Credits
-------

This was originally forked from [Léon Gersen's](http://refreshless.com/) brilliant noUiSlider:

https://github.com/leongersen/noUiSlider

Licence
-------

This code is released under the [MIT Licence](http://opensource.org/licenses/MIT)

Copyright (c) 2013 Daniel Crisp

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.



