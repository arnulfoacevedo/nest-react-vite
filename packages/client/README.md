# Nest React boilerplate client

## Client dependencies

As this boilerplate aims to be a minimal shell, it only ships 1 dependency on top of the React / ReactDOM pair and the internal dependencies: [Debug](https://github.com/visionmedia/debug) (^4.1.1).

### Debug library

This simple package allows you to log messages to the browser's console without using the synchronous and greedy `console`'s methods.

The boilerplate already ships a basic [`Logger`](./packages/client/src/utils/logger.ts) class which exposes basic static methods:

- `info`: for debug information
- `log`: alias for `info`
- `warn`: for warnings
- `error`: for error reporting. If you provide an instance of the `Error` type to this method's first argument, you will see its stack trace exposed in a similar way as the `console.error` would expose it. See the `useEffect` hook in the [`App`](./src/components/App/App.tsx) component for an example.

Once you have successfully adapted the boilerplate to your project, as explained in main README's section [How to adapt the boilerplate](../../README.md#how-to-adapt-the-boilerplate), to see the debug messages in your browser, you just need to set up the localstorage `debug` variable. To do so, run `localStorage.debug = '<LOGGER_PREFIX>:*'` in your browser console.

To learn more about the debug library usage in the browser, you can check out [its documentation](https://github.com/visionmedia/debug#browser-support).

### Styling options

The boilerplate doesn't include any styling solution. Nevertheless, if you opt for a solution requiring CSS files, you could easily adapt the webpack configuration to include them:

```js
// close to the end of in the config.module.rules array
{
  test: /\.css$/,
  use: ["style-loader", "css-loader"],
},
```

You would then need to add these two loaders to your client `devDependencies`:

```sh
# from the packages/client directory
yarn add -D style-loader css-loader
```

For more information about these loaders, please refer to their [respective documentation](https://webpack.js.org/loaders/#styling).

## Running the app

By default, the webpack dev server is listening to the [http://localhost:8000](http://localhost:8000) port. This can be configured in the [webpack.config.js](./webpack.config.js) file, changing the `WEBPACK_DEV_SERVER_PORT` constant.

If your client imports assets (e.g. images), they will be passed as base64 inline HTML element if they are under `5000` bytes. Otherwise they will be passed as separated files in the final bundle. To change this limit, you can change the `ASSETS_MAX_INLINE_SIZE` constant. For more information about this, see the [`url-loader` documentation](https://webpack.js.org/loaders/url-loader).

### In development

```sh
# Runs the webpack dev server (using HMR)
yarn start:dev
```

As the `webpack` loader used for both JavaScript and TypeScript files is the `babel-loader`, it simply strips away all TypeScript-specific features in the source code when building both the development server and the final production build. This dramatically speeds up the hot replacement process when locally developing and offers a number of improvements with libraries which don't support tree-shaking natively. It also avoids many useless disrupting errors occurring during prototyping while the types aren't completely respected yet.

But the disadvantage is that the bundler won't detect any TypeScript error unless it is a JavaScript error.

To work around this limitation, the repository exposes a command to check your code is TypeScript compliant:

```sh
yarn check-types
```

#### Debug

The powerful debug feature allows any Node.js debugger tool to connect to the running process in order to define breakpoints, log points, and more. Any Chromium based browser comes with the Node inspector feature built-in. To learn more about the Node.js debugging tools, the [Node.js documentation](https://nodejs.org/de/docs/guides/debugging-getting-started/) is a nice starting point.

If you use the VS Code IDE, this repository already ships 2 configurations to `launch` a Chrome debugging session or `attach` to an existing one. See the [.vscode/launch.json](../../.vscode/launch.json) file for more information.

### In production

Once you are happy with your code, you can run a production version following these steps:

1. Build a production bundle:

   ```sh
   yarn build
   ```

2. Then you can serve the `dist` folder from any webserver application like [NGINX](https://nginx.org/) for example. The [nginx.conf](./nginx.conf) can be used as an example of a working NGINX configuration.

### Docker image

The client package has a [Dockerfile](./Dockerfile) which builds a lightweight (based on the [alpine](https://alpinelinux.org/) project) production ready Docker image.

For more information about the Docker images, see the [main README.md](../../README.md#docker-images).
