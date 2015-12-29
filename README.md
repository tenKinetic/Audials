# Audials

> Alternative to HTML5 range (slider) inspired by DAWs, kinda like dials. Audials.

> Some very easy to achieve styles (the third is not wrong, its range is -50..60). You'll find the HTML / JavaScript required to achieve these in usage.html:

> ![Audials Styles](styles.png?raw=true "Audials Styles")

## Usage

There are four usage types corresponding to two functionality styles and two display styles.

Gain functionality style:
> This dial goes from the range minimum to the range maximum, left to right. The range would typically be 0..100.

Balance functionality style:
> This dial goes from the range minimum to the range maximum, left to right, The range would typically be -50..50.

Notch display style:
> This style shows the current value as a single notch or line on the control.

Fill display style:
> This style Shows the current value as a fill from left to right on a gain control or, for a balance control, a fill from the centre-top to either the left or right.

```
<script src="es5/tk-audials.min.js"></script>
<link rel="stylesheet" media="all" href="build/tk-audials.min.css">
...
<div id="gain-notch" class="tk-audial"></div>
<div id="balance-notch" class="tk-audial"></div>
<div id="gain-fill" class="tk-audial"></div>
<div id="balance-fill" class="tk-audial"></div>
<script>
  var gainNotch = document.querySelector('#gain-notch');
  var balanceNotch = document.querySelector('#balance-notch');
  var gainFill = document.querySelector('#gain-fill');
  var balanceFill = document.querySelector('#balance-fill');
  var gainNotchButton = new TkAudial(gainNotch);
  var balanceNotchButton = new TkAudial(balanceNotch, {type:'balance',display:'notch',min:-50,max:50,value:12});
  var gainFillButton = new TkAudial(gainFill, {type:'gain',display:'fill'});
  var balanceFillButton = new TkAudial(balanceFill, {type:'balance',display:'fill',min:-50,max:50,value:12,inputId:'unique-id'});
</script>
```

Will result in something like this:
> ![Audials Layout](audials.png?raw=true "Audials Layout")

A working implementation is provided in usage.html.

The dials will scale to fill their container which should have the class *tk-audial*.

You can use the ES6 version from the *build* folder or the ES5 version from the *es5* folder.

## Options

The first argument when instantiating TkAudial is its HTML container element. The second is an optional options object of optional options:
```
{
  type: (String) Either 'gain' or 'balance'. Default is gain,
  display: (String) Either 'notch' or 'fill'. Default is notch,
  min: (Number) Range start. Default is 0,
  max: (Integer) Range end. Default is 100,
  step: (Integer) Legal interval. 0.01..n (values are rounded to two decimal places) Default is 1,
  value: (Integer) Initial value. Default is 50,
  borderColour: (String) Style value (EG white, #, rgb, rgba). Default is #545454,
  borderWidth: (Integer) Default is 8,
  indicatorBackgroundColour: (String) Style value (EG white, #, rgb, rgba). Default is white,
  indicatorColour: (String) Style value (EG white, #, rgb, rgba). Default is #888888,
  indicatorWidth: (Integer) Default is 15,
  valueBackgroundColour:  (String) Style value (EG white, #, rgb, rgba). Default is black,
  valueColour: (String) Style value (EG white, #, rgb, rgba). Default is white,
  valueFontSize: (String) Style value (EG 1.2em, 22px, 12pt, 90%). Default is 1em,
  inputId: (String) used for name and id of a hidden form input. Input is not generated if this option is omitted,
  zeroModifiers: (Boolean) Set touch modifier count to zero when releasing a dial. Default is false,
  enableClipboard: (Boolean) Add the control value to the clipboard when releasing a dial. Default is false
}
```
See [Known Issues](#known-issues) for details regarding the *zeroModifiers* option.

Number type options are rounded to two decimal places. The initial value is rounded to the nearest valid step (and also the same number of decimal places as the step provided). If your minimum and maximum values don't match the step the final step will be reduced.

## Events

> When the value is changed the *changed* event is triggered by the container (that is passed as he first argument to TkAudial).

## Methods

> All methods are in the prototype so you can take a look and call anything you need to. The only methods that would typically be needed though are setValue(value) where *value* is a number (which will be rounded to the same number of decimal places as the step option) and getValue() which returns a number whose precision will also match that of the step option.

## Modifiers

> For large ranges it may be useful to use the alt and shift modifiers which double and treble the sensitivity of the dial. They are also cumulative so using both would sextuple the sensitivity. Using the modifiers will reduce accuracy but they can be triggered mid-drag so you can use them to quickly get close to where you need to be then release them to achieve the precise value you need.

<!-- -->

> Touch modifiers are slightly different. The sensitivity of the dial is multiplied by the number of touches detected. Modifier touches can be added while moving the dial by using your other hand for example. Touch modifiers are detected anywhere on the screen.

## Pipeline

> The modifiers still may not be sensitive enough in some cases. A control for Hz for example will typically range from 20..20000. The current modifiers seem ideal for smaller ranges so an additional option will soon be added so that all modifiers can be amplified to suit the range option and also the UI. This will also be implemented so that the dials can also be made less sensitive.

<!-- -->

> Looking into an intuitive way to copy and paste values between dials. Copy is done by the inclusion of clipboard.js. An intuitive way to indicate you want to paste a value to a control is required. Particularly on touch, a key modifier will be easy enough for desktop.

## Known issues

> Selection of the control value as text when the control is not in use proved flaky. Especially in Safari. To select the text value of the control, start the selection outside the control. This works well enough on desktop. On iOS text selection causes a slight issue. Just touch and hold to select the text, this is fine. Starting the dial manipulation from the text though doesn't seem to set the text as non-selectable as well as desktop does. In this case the dial needs to be manipulated by its edges. Text selection is probably not particularly useful and this may soon all be avoided by have the control non-selectable at all times.

<!-- -->

> Safari on both desktop and iOS doesn't seem to completely get along with the modifiers. It's not fatal but it seems to get jumpy when triggering them sometimes.

<!-- -->

> For touch devices, touch mashing, or perhaps a cpu struggling device, can result in the touch count getting out of sync. This seems to be rare though. This may be dealt with by setting the touch count for a dial to zero when it is released. The *zeroModifiers* options has been added for this purpose. If this option is set to true, modifiers would have to be re-applied before or during manipulation of a dial. If you suspect modifier count sync is proving to be an issue, this can be applied.

<!-- -->

> The clipboard library is [clipboard.js](https://zenorocha.github.io/clipboard.js/). Safari is not supported.
