## Migrating to `v3.0`

Breaking changes are in 🔥 __bold and on fire__.

### Some player providers are not supported yet

Since `v3.0` is a new architecture not all providers have been updated.
It is recommended to keep using `v2` and vote to add this provider to `v3` in [discussions](https://github.com/cookpete/react-player/discussions).
These include:

  - `Dailymotion`
  - `SoundCloud`
  - `Streamable`
  - `Twitch`
  - `Facebook`
  - `Mixcloud`
  - `Kaltura`

### Lazy players

As of `v3.0` all the players are lazy loaded by default. 
Due to the use of `lazy` and `Suspense`, 🔥 __React 16.6 or later is now required__.

### Player props

As of `v3.0` some player props are renamed to be closer to the native
[HTMLMediaElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement) naming.

- 🔥 __`url` => `src`__
- 🔥 __`playsinline` => `playsInline`__
- 🔥 __`progressInterval`__ deprecated
- 🔥 __`stopOnUnmount`__ deprecated
- 🔥 __`wrapper`__ is `undefined` by default. Set to `div` if you want a wrapper element.

### Player instance methods

As of `v3.0` use [`ref`](https://react.dev/learn/manipulating-the-dom-with-refs) to call instance methods on the player. See [the demo app](examples/react/src/App.js) for an example of this. Since `v3`, the instance methods aim to be 🔥 __compatible 
with the [HTMLMediaElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement) interface__.

### Player callback props

As of `v3.0` some player callback props are renamed to be closer to the native
[HTMLMediaElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement) event naming.

- 🔥 __`onProgress` => `onTimeUpdate` and `onProgress`__
- 🔥 __`onDuration` => `onDurationChange`__
- 🔥 __`onPlaybackRateChange` => `onRateChange`__
- 🔥 __`onSeek` => `onSeeking` and `onSeeked`__
- 🔥 __`onBuffer` => `onWaiting`__
- 🔥 __`onBufferEnd` => `onPlaying`__
- 🔥 __`onEnablePIP` => `onEnterPictureInPicture`__
- 🔥 __`onDisablePIP` => `onLeavePictureInPicture`__


## Migrating to `v2.0`

Breaking changes are in 🔥 __bold and on fire__.

### Lazy players

As of `v2.2`, if your build system supports `import()` statements, use `react-player/lazy` to [lazy load](https://reactjs.org/docs/code-splitting.html#reactlazy) the appropriate player for the `url` you pass in. This adds several `reactPlayer` chunks to your output, but reduces your main bundle size.

Due to the use of `lazy` and `Suspense`, 🔥 __React 16.6 or later is now required__.

```jsx
// Before
import ReactPlayer from 'react-player'

// After
import ReactPlayer from 'react-player/lazy'
```

Lazy players were the default import in `v2.1`, but moved to `react-player/lazy` in `v2.2` to avoid causing problems with common build systems.

### Single player imports

As of `v2.2`, the 🔥 __location of single player imports has changed__. Single players are not available in `v2.0` and `v2.1`.

```jsx
// Before
import ReactPlayer from 'react-player/lib/players/YouTube'

// After
import ReactPlayer from 'react-player/youtube'
```

### Preloading

The `preload` config option was originally added to solve a [very specific use case](https://github.com/CookPete/react-player/issues/7) a very long time ago. Modern browsers are trending towards disabling autoplay by default, which makes the preload behaviour quite useless. The implementation was also quite hacky, and added to the bundle size for a feature that seems to be very rarely used. For this reason, 🔥 __the `preload` option has been removed__.

### The `config` prop

🔥 __Deprecated config props have been removed.__ Previously these props still worked, but with a console warning.

```jsx
// Before
<ReactPlayer 
  youtubeConfig={{ playerVars: { showinfo: 1 } }} 
/>

// After
<ReactPlayer 
  config={{ youtube: { playerVars: { showinfo: 1 } }}} 
/>
```

It is also worth noting that you no longer need to use separate config keys for different players. For example, if you are only ever using one type of `url` you can put player-specific options directly inside `config`.

```jsx
// Before
<ReactPlayer 
  youtubeConfig={{ playerVars: { showinfo: 1 } }} 
/>

// After
<ReactPlayer 
  config={{ playerVars: { showinfo: 1 } }} 
/>
```

### `onReady` is invoked with the player instance

Previously, instance methods would be called using [refs](https://reactjs.org/docs/refs-and-the-dom.html). They still can, but in v2.0, `onReady` is called with the ReactPlayer instance, giving you the option of storing the instance and calling methods on it. This is especially useful when using `getInternalPlayer`.

```jsx
// Before
class Player extends Component {
  ref = player => {
    this.player = player            // Store a player that may not be ready for methods
    this.player.getInternalPlayer() // Returns null if player is not ready
  }
  handleReady = () => {
    this.player.getInternalPlayer() // Internal player now ready
  }
  render () {
    return (
      <ReactPlayer ref={this.ref} onReady={this.handleReady} />
    )
  }
}

// After
class Player extends Component {
  handleReady = player => {
    this.player = player            // Store a player that is ready for methods
    this.player.getInternalPlayer() // Internal player now ready
  }
  render () {
    return (
      <ReactPlayer onReady={this.handleReady} />
    )
  }
}
