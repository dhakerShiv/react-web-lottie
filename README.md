# Lottie Animation View for React

[![npm version](https://badge.fury.io/js/react-web-lottie.svg)](http://badge.fury.io/js/react-web-lottie)

## What's new
Added ```lottiRef``` as new param to fully control lottie animation, you can use any [lottie-web](https://github.com/airbnb/lottie-web) methods on it.

## Wapper of bodymovin.js

[bodymovin](https://github.com/bodymovin/bodymovin) is [Adobe After Effects](http://www.adobe.com/products/aftereffects.html) plugin for exporting animations as JSON, also it provide bodymovin.js for render them as svg/canvas/html.

## Why Lottie?

#### Flexible After Effects features
We currently support solids, shape layers, masks, alpha mattes, trim paths, and dash patterns. And we’ll be adding new features on a regular basis.

#### Manipulate your animation any way you like
You can go forward, backward, and most importantly you can program your animation to respond to any interaction.

#### Small file sizes
Bundle vector animations within your app without having to worry about multiple dimensions or large file sizes. Alternatively, you can decouple animation files from your app’s code entirely by loading them from a JSON API.

[Learn more](http://airbnb.design/introducing-lottie/) › http://airbnb.design/lottie/

Looking for lottie files › https://www.lottiefiles.com/

## Installation

Install through npm:
```
npm install --save react-web-lottie
```

## Usage

Import animation.json as animation data

```jsx
import React from 'react'
import Lottie from 'react-web-lottie';
import * as animationData from './animation.json'

export default class LottieControl extends React.Component {

  constructor(props) {
    super(props);
    this.state = {isStopped: false, isPaused: false};
    this.animationRef = React.createRef();
  }

  /*
    Update text dynamically
    to make it work,
    you need to deselect glyphs option while exporting JSON from After effects
  */
  setInputData = (text) => {
    // Play a particular segment form animation by providing starting and ending frame number
    this.animationRef.current.playSegments([[140, 145]], true);

    let 
      element,
      layerIndex = 4, // Layer number from JSON file
      frameIndex = 0; // frame in a layer

    element = this.animationRef.current.renderer.elements[layerIndex];
    element.updateDocumentData({ t: text, s: 40 }, frameIndex); // s is font size
  };

  render() {
    const buttonStyle = {
      display: 'block',
      margin: '10px auto'
    };

    const defaultOptions = {
      loop: true,
      autoplay: true, 
      animationData: animationData,
      rendererSettings: {
        preserveAspectRatio: 'xMidYMid slice'
      }
    };

    return <div>
      <Lottie 
        options={defaultOptions}
        height={400}
        width={400}
        isStopped={this.state.isStopped}
        isPaused={this.state.isPaused}
        isClickToPauseDisabled={true}
        lottiRef={animationRef}
      />
      <button style={buttonStyle} onClick={() => this.setState({isStopped: true})}>stop</button>
      <button style={buttonStyle} onClick={() => this.setState({isStopped: false})}>play</button>
      <button style={buttonStyle} onClick={() => this.setState({isPaused: !this.state.isPaused})}>pause</button>

      <div>
        <input
          type="text"
          height="100"
          onKeyUp={(event) => setInputData(event.target.value)} // you need to deselect glyphs option while exporting JSON from After effects
        />
      </div>
    </div>
  }
}

```

### props
The `<Lottie />` Component supports the following components:

**options** *required*

the object representing the animation settings that will be instantiated by bodymovin. Currently a subset of the bodymovin options are supported:

>**loop** *options* [default: `false`]
>
>**autoplay** *options* [default: `false`]
>
>**animationData** *required*
>
>**rendererSettings** *required* 

**width** *optional* [default: `100%`]

pixel value for containers width.

**height** *optional* [default: `100%`]

pixel value for containers height.

**eventListeners** *optional* [default: `[]`]

This is an array of objects containing a `eventName` and `callback` function that will be registered as  eventlisteners on the animation object. refer to [bodymovin#events](https://github.com/bodymovin/bodymovin#events) where the mention using addEventListener, for a list of available custom events.

example:
```jsx
eventListeners=[
  {
    eventName: 'complete',
    callback: () => console.log('the animation completed:'),
  },
]
```

## Related Projects

* [Bodymovin](https://github.com/bodymovin/bodymovin) react-web-lottie is a wrapper of bodymovin

## Contribution
Your contributions and suggestions are heartily welcome.

## License
MIT

