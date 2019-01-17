clappr-rtmp-plugin
==================

RTMP support for [Clappr player](http://github.com/globocom/clappr). Supports both RTMP direct and SMIL (dynamic streaming). Updated to work more natually with npm environments like ReactJS.

## How to use

Install dependencies

```javascript
yarn add clappr
yarn add visorgg/clappr-rtmp-plugin
```

Render like this:

```javascript
// @flow
"use strict";

import React from "react";
import Clappr from "clappr";
import ClapprRtmp from "clappr-rtmp";

export default class MyPlayer extends React.PureComponent {
  player = null;
  componentDidMount() {
    this.player = new Clappr.Player({
      source: "rtmp://<url>",
      parentId: "#player",
      plugins: {"playback": [ClapprRtmp]},
      rtmpConfig: {
        scaling:"stretch",
        playbackType: "live",
        bufferTime: 1,
        startLevel: 0,
        switchRules: {
          "SufficientBandwidthRule": {
            "bandwidthSafetyMultiple": 1.15,
            "minDroppedFps": 2,
          },
          "InsufficientBufferRule": {
            "minBufferLength": 2,
          },
          "DroppedFramesRule": {
            "downSwitchByOne": 10,
            "downSwitchByTwo": 20,
            "downSwitchToZero": 24,
          },
          "InsufficientBandwidthRule": {
            "bitrateMultiplier": 1.15,
          },
        },
      },
    });
  }
  render() {
    return (
      <div id="player"></div>
    );
  }
}

```

## Configuration

The plugin accepts several **optional** configuration options, such as:

  - `swfPath` (default **//cdn.jsdelivr.net/clappr.rtmp/latest/assets/RTMP.swf**) - Path to the SWF responsible for the playback.
  - `scaling` (default **letterbox**) - Scaling behavior.
  - `playbackType` (default **live** if the source contains live on its URL, **vod** otherwise).
  - `bufferTime` (default **0.1**) - How long to buffer before start playing the media.
  - `startLevel` (default **-1**) - Initial quality level index.
  - `autoSwitch` (default **false**) - Whether video should autoSwitch quality
  - `useAppInstance` (default **false**) - Set it to `true` if your source url contains the app instance (not required if the app instance is `_definst_`).
  - `proxyType` (default **none**) - Determines which fallback methods are tried if an initial connection attempt to Flash Media Server fails.
  - `switchRules` (default **system defined**) - Rules defined to autoSwitch video quality based in some conditions.

## Building

#### Requirements

1. [AirSDK](http://www.adobe.com/devnet/air/air-sdk-download.html)
2. [Webpack](https://www.npmjs.com/package/webpack)

#### Compile SWF

```sh
cd src
sh build_player.sh
```

#### Download JS dependencies

```sh
npm install
```

#### Build the final dist

```sh
webpack
```
