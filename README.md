# meteor-webpack-react

This is a port of the Meteor sample-todos tutorial to a React UI built by Webpack.

## Advantages over serving the UI straight out of the Meteor project

* `require`/ES6 `import` let you avoid Meteor global variables/load order issues
* `react-hot-loader` reloads React components without reloading the entire page
  when you make changes
* If you `require` your styles with Webpack, it will also reload them without
  reloading the entire page when you make changes to them
* Using an npm module in the browser is as simple as `npm install` and `require`
  * This puts a large part of the React ecosystem (which revolves around Webpack/npm)
    at your fingertips
* Other Webpack loaders are great too, for example:
  * you can break up your CSS into one file per React component, and then `require`
    them in your JSX files
  * or if you want to use Sass, you can `require` the Sass files
  * or you can use `url-loader` to `require` an image file and get a URL to stick in
    an `<img>` tag

## How it works

The `meteor` directory contains a `.prod` folder that is hidden for dev mode and unhidden (renamed to `prod`) for prod mode.

In prod mode, only meteor is running, and it gets the webpack bundle via the soft link `meteor/prod/client/main.js`.  The React prod build is also in that folder, and the bundle gets it via a Webpack alias to a script that simply does `export default window.React`.  (I should change this soon, since I figured out how to recreate the prod build from the npm package using UglifyJS and Webpack's `DefinePlugin`.

In dev mode, both webpack-dev-server and meteor run simultaneously on different ports (9090 and 3000, respectively).  A script in `index.html` detects if the prod bundle has been loaded, and if not, inserts a `<script>` tag linking to the bundle from webpack-dev-server via port 9090 on the page's host.  React from npm is built into the bundle in dev mode.

### Windows note

`meteor/.prod/client/main.js` is a soft link to `../../../webpack/dist/bundle.js`.  I don't know
if the soft link will work on Windows.  If not, you can just copy the bundle in, but *make sure
to rename it to `main.js`* so that Meteor loads it after everything else.

## Running (dev mode)

```
> cd webpack
> npm install
> npm run dev
```
## Running (prod mode)

```
> cd webpack
> npm install
> npm run prod
```
Make sure to wait for Meteor to say it's listening, and for webpack-dev-server to print out the list of modules.  The site won't work until both are ready.

## Production build

```
> cd webpack
> npm install
> npm run build
```
