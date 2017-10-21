# ultimate-guitar-scraper

[![npm version](https://badge.fury.io/js/ultimate-guitar-scraper.svg)](https://badge.fury.io/js/ultimate-guitar-scraper)
[![Dependency Status](https://gemnasium.com/masterT/ultimate-guitar-scraper.svg)](https://gemnasium.com/masterT/ultimate-guitar-scraper)
[![TravisCI Status](https://travis-ci.org/masterT/ultimate-guitar-scraper.svg)](https://travis-ci.org/masterT/ultimate-guitar-scraper)
[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

> A scraper for http://www.ultimate-guitar.com

> Rock and roll! 🎸 🎶 🤘🏻

The scraper allow you to:
- Search TAB by song name and band name.
- Get TAB from its url.
- Get suggestions for artist or album.

## requirements

- nodejs `>= 6.5`
- npm


## installation

`npm i ultimate-guitar-scraper --save`


## usage

### `search(query, callback [, requestOptions])`

#### query

Type: `Object`

| Name     | Type            | Require | Default              |
|----------|-----------------|---------|----------------------|
| bandName | string          | yes     |                      |
| songName | string          | no      |                      |
| page     | number          | no      | `1`                  |
| type     | string or array | no      | `['tabs', 'chords']` |

Available TAB types:
- `'video lessons'`
- `'tabs'`
- `'chords'`
- `'bass tabs'`
- `'guitar pro tabs'`
- `'power tabs'`
- `'drum tabs'`
- `'ukulele chords'`

#### callback

Type: `Function (error, tabs, requestResponse, requestBody)`

- **error**: Error object. `null` if no error.
- **tabs**: array of TAB (see TAB structure below) `null` if error.
- **requestResponse**: the original response returned by [request](https://www.npmjs.com/package/request).
- **requestBody**: the original body returned by [request](https://www.npmjs.com/package/request).


#### requestOptions

Type: `Object`

Options of the HTTP request, made with package [request](https://www.npmjs.com/package/request).


### examples

Basic usage.

```js
const ugs = require('ultimate-guitar-scraper')

ugs.search({
  bandName: 'Pink Floyd',
  songName: 'Wish You Were Here',
  page: 1,
  type: ['tabs', 'chords', 'guitar pro tabs']
}, (error, tabs) => {
  if (error) {
    console.log(error)
  } else {
    console.log(tabs)
  }
})
```

Using [request](https://www.npmjs.com/package/request) options to pass a custom header.

```js
const ugs = require('ultimate-guitar-scraper')

var query = {
  bandName: 'Half Moon Run'
}

function callback (error, tabs, response, body) {
  if (error) {
    console.log(error)
  } else {
    console.log(tabs)
    console.log('Utlimate Guitar server: ' + response.headers['server'])
  }
}

var options = {
  headers: {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/46.0.2490.86 Safari/537.36'
  }
}

ugs.search(query, callback, options)
```

### tabs

An `Array` of TAB object that looks like this:

```js
{
  artist: 'Pink Floyd',
  name: 'Wish You Were Here Live',
  difficulty: 'intermediate',       // can be null
  rating: 5,                        // can be null
  numberRates: 2,                   // can be null
  type: 'tab'
  url: 'http://tabs.ultimate-guitar.com/p/pink_floyd/wish_you_were_here_live_tab.htm'
}
```


### `get(tabUrl, callback [, requestOptions])`

#### tabUrl

Type: `String`

The url of the TAB.

#### callback

Type: `Function(error, tab, requestResponse, requestBody)`

- **error**: Error object. `null` if no error.
- **tab**: the TAB (see TAB structure below) `null` if error.
- **requestResponse**: the original response returned by [request](https://www.npmjs.com/package/request).
- **requestBody**: the original body returned by [request](https://www.npmjs.com/package/request).

#### requestOptions

Type: `Object`

Options of the HTTP request, made with package [request](https://www.npmjs.com/package/request).

#### exemple

Basic usage.

```js
let tabUrl = 'https://tabs.ultimate-guitar.com/n/nirvana/smells_like_teen_spirit_ver2_crd.htm'
ugs.get(tabUrl, (error, tab) => {
  if (error) {
    console.log(error)
  } else {
    console.log(tab)
  }
})
```

#### tab

```js
{
  name: 'Smells Like Teen Spirit',
  type: 'chords',
  artist: 'Nirvana',
  rating: 4,
  numberRates: 28,
  difficulty: null,
  contentText: '[Intro]\n\nFsus2  Bbsus2  Ab  Db (x4)\n\n\n[Verse Intro]\n\nFsus2  Bbsus2  Ab  Db (x2)\n\n\n...',
  contentHTML: '[Intro]\n\n<span>Fsus2</span>  <span>Bbsus2</span>  <span>Ab</span>  <span>Db</span> (x4)\n\n\n[Verse Intro]\n\n<span>Fsus2</span>  <span>Bbsus2...'
}
```

Content attributes depend on the type.

| Type             | Content attributes                  |
|------------------|-------------------------------------|
| `tabs`           | `contentText`, `contentHTML`        |
| `chords`         | `contentText`, `contentHTML`        |
| `ukulele chords` | `contentText`, `contentHTML`        |
| `drum tabs`      | `contentText`, `contentHTML`        |
| `bass tabs`      | `contentText`, `contentHTML`        |
| `guitar pro tabs`| `downloadUrl`                       |
| `power tabs`     | `downloadUrl`                       |
| `video lessons`  | `contentUrl`                        |



### `autocomplete(query, callback [, requestOptions])`

#### query

Type: `String`

#### callback

Type: `Function(error, suggestions, requestResponse, requestBody)`

- **error**: Error object. `null` if no error.
- **suggestions**: array of String that represent `'song'` or `'artist'`.
- **requestResponse**: the original response returned by [request](https://www.npmjs.com/package/request).
- **requestBody**: the original body returned by [request](https://www.npmjs.com/package/request).


#### requestOptions

Type: `Object`

Options of the HTTP request, made with package [request](https://www.npmjs.com/package/request).


### example

```js
const ugs = require('ultimate-guitar-scraper')
var query = 'Ozzy'
ugs.autocomplete(query, (error, suggestions) => {
  if (error) {
    console.log(error)
  } else {
    console.log(suggestions)
  }
})
```

## test

Feature tests are run _daily_, thank to Travis CI new feature [CRON Jobs](https://docs.travis-ci.com/user/cron-jobs/). This way we know if the scraper is ever broken.

Run the test:

```bash
npm test
```


## contributing

Contribution is welcome! Open an issue first.


## license

MIT.
