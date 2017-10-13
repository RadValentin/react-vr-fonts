# react-vr-fonts

> Popular fonts in SDF format for React VR apps

## How to use

### Load a single font

```js
import {VRInstance} from 'react-vr-web';
import * as OVRUI from 'ovrui';

function init(bundle, parent, options) {
  OVRUI.loadFont(
    '../static_assets/Lobster.fnt',
    '../static_assets/Lobster_sdf.png'
  ).then(font => {
    const vr = new VRInstance(bundle, 'CustomFont', parent, {
      font: font,
      ...options
    });
    vr.render = function() {};
    vr.start();
  });
}

window.ReactVR = {init};
```

### Load muliple fonts as fallbacks

```js
import {VRInstance} from 'react-vr-web';
import * as OVRUI from 'ovrui';

function init(bundle, parent, options) {
  Promise.all([
    OVRUI.loadFont(
      '../static_assets/Lobster.fnt',
      '../static_assets/Lobster_sdf.png'
    ),
    OVRUI.loadFont(
      '../static_assets/FontAwesome.fnt',
      '../static_assets/FontAwesome_sdf.png'
    )
  ]).then(([lobsterFont, fontAwesome]) => {
    // If a glyph isn't in FontAwesome then we fallback to Lobster
    OVRUI.addFontFallback(lobsterFont, fontAwesome);

    const vr = new VRInstance(bundle, 'CustomFontFallback', parent, {
      font: lobsterFont,
      ...options
    });
    vr.render = function() {};
    vr.start();
  });
}

window.ReactVR = {init};
```

## How to generate a font on macOS

1. Clone the [react-vr](https://github.com/facebook/react-vr) repo, you'll need to work in its `/tools/fontue` directory
    ```
    $ git clone git@github.com:facebook/react-vr.git
    $ cd react-vr/tools/fontue/
    ```
1. Get [homebrew](https://brew.sh/)
1. Install cmake and freetype
    ```
    $ brew install cmake
    $ brew install freetype
    ```
1. Build the fontue tool
    ```
    $ cmake .
    $ make
    ```
1. Run the tool it takes arguments in the format below
    ```
    ./fontue <in font file> <out font file > [[-option], ... ]
    ```
    Most fonts in this repo are generated with the following options:
    ```
    ./fontue ~/fonts/Lobster-1.4.otf ~/fonts/Lobster -co -0.01 -ts 1.0 -hpad 128 -vpad 128 -sdf 512 -1 -1 -ur 0x0010 0x00ff -ur 0x0370 0x03FF -ur 0x0100 0x017F -ur 0x0180 0x024F
    ```