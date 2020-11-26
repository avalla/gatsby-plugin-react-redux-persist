# gatsby-plugin-react-redux-persist

> A [Gatsby](https://github.com/gatsbyjs/gatsby) plugin for
> [react-redux](https://github.com/reduxjs/react-redux) with
> built-in server-side rendering support and persistance.

## Install

`npm install --save gatsby-plugin-react-redux-persist react-redux redux redux-persist`

## How to use

`./src/state/createStore.js` // same path you provided in gatsby-config
```javascript
import { createStore } from 'redux';
import { persistStore, persistReducer } from 'redux-persist';
import storage from 'redux-persist/lib/storage';

function reducer() {
  //...
}

const persistConfig = {
  key: 'root',
  storage,
};
const persistedReducer = persistReducer(persistConfig, reducer);

export default (preloadedState = {}) => {
  const store = createStore(
    persistedReducer,
    preloadedState, // initial state
  );
  const persistor = persistStore(store);
  return { store, persistor };
}

```

`./gatsby-config.js`
```javascript
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-react-redux-persist`,
      options: {
        // [required] - path to your createStore module
        pathToCreateStoreModule: './src/state/createStore',
        // [optional] - options passed to `serialize-javascript`
        // info: https://github.com/yahoo/serialize-javascript#options
        // will be merged with these defaults:
        serialize: {
          space: 0,
          // if `isJSON` is set to `false`, `eval` is used to deserialize redux state,
          // otherwise `JSON.parse` is used
          isJSON: true,
          unsafe: false,
          ignoreFunction: true,
        },
        // [optional] - if true will clean up after itself on the client, default:
        cleanupOnClient: true,
        // [optional] - name of key on `window` where serialized state will be stored, default:
        windowKey: '__PRELOADED_STATE__',
      },
    },
  ],
};
```

For more informations about redux-persist visit their [website](https://github.com/rt2zz/redux-persist)

## Thanks

Thanks to [Leonid Nikiforenko](https://github.com/le0nik/) for original [plugin](https://github.com/le0nik/gatsby-plugin-react-redux/).


## License

MIT
