# ember-cli-jss [![Build Status][buildstat-image]][buildstat-url]

> JSS integration for Ember

## Install

```bash
$ npm install --save ember-cli-jss
```

## Usage

The property `stylesheet` must be an instance of the `StyleSheet`.

Properties `jssNames` and `jssNameBindings` work like `classNames` and `classNameBindings`, respectively.

When you update properties listed in the `jssObservedProps`, dynamic styles will be updated.

```js
// ...awesome-component/component.js

import Component from '@ember/component';
import { JSS, TaglessJSS } from 'ember-cli-jss';
import stylesheet from './stylesheet';

export default Component.extend(JSS, {
  stylesheet,
  jssNames: ['wrapper'],
  jssNameBindings: ['isShow:show'],
  jssObservedProps: ['color'],

  color: 'blue',
  isShow: true,

  actions: {
    changeColor(color) {
      this.set('color', color);
    },
  },
});

// For tag-less component use TaglessJSS
export default Component.extend(TaglessJSS, {
  stylesheet,
  jssObservedProps: ['color'],
  tagName: '',
  color: 'blue',

  actions: {
    changeColor(color) {
      this.set('color', color);
    },
  },
});
```

Constructor `StyleSheet` accepts the same arguments as [`jss.createStyleSheet`](http://cssinjs.org/js-api?v=v8.0.0#create-style-sheet).

```js
// ...awesome-component/stylesheet.js

import { StyleSheet } from 'ember-cli-jss';

export default new StyleSheet({
  wrapper: {
    width: 600,
    display: 'none',
  },

  show: {
    display: 'block',
  },

  content: {
    color: data => data.color,
  },
});
```

```hbs
{{!-- ...awesome-component/template.hbs --}}

<button type="button" {{action "changeColor" "green"}}>
  Green
</button>

<div class="{{jss 'content'}}">
  Lorem ipsum...
</div>
```

## Helper

```hbs
<button class="{{jss 'large' 'primary' disabled=true}}">
  Submit
</button>
```

## Configuration

Plugin [`jss-preset-default`](https://github.com/cssinjs/jss-preset-default) applied by default. Please note that the work of the dynamic properties depends on [`jss-compose`](https://github.com/cssinjs/jss-compose) plugin.

You can override the `app/initializers/ember-cli-jss.js`.

```js
// ...app/initializers/ember-cli-jss.js

import jss from 'jss';
import preset from 'jss-preset-default';

export function initialize() {
  jss.setup(preset());
}

export default {
  name: 'ember-cli-jss',
  initialize,
};
```

## License

[MIT](LICENSE.md) © [Timofey Dergachev](https://exeto.me/)

[buildstat-url]: https://travis-ci.org/exeto/ember-cli-jss?branch=master
[buildstat-image]: https://img.shields.io/travis/exeto/ember-cli-jss/master.svg?style=flat-square
