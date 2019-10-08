release-task
===================

A package to automate releasing for git projects. Out of the box this package does the following:

- `bump`: Increment the `package.json` file version property, to add more files see the options below. 
- `changelog`: Updates the changelog with the latest commits that has not been pushed to origin. 
- `commit`: Commit those files which had version constraint bumped i.e: `package.json`, `changelog.md`.
- `tag`: Create a new annotated tag for the new release version. 


### Install

```bash
$ npm i release-task --save-dev
```

### Usage

Create a script file that could be run via npm scripts like: 
`./scripts/release.js`
```js
const releaseTask = require(release-task);
tasks.init();
```

Add a new script within `package.json`

```json
{
  "scripts": {
   "release": "node ./scripts/release.js"
  }
}
```

```bash
$ npm run release
```

> To see how this package handle releasing commits, changelog and tags take a look 
> in its own releasing [commit](https://github.com/adrianorsouza/release-task/commit/e383f9535130379b1f78d249745a565f22e4e2e9)
> and [tag releases](https://github.com/adrianorsouza/release-task/releases/tag/v0.1.0)

### Adding custom tasks

Tasks are function that do something and return a Promise, by default there are 4 tasks setup, but it's 
possible to create your own task to run after the default tasks are completed.

Tasks are queued, in order to executed each step without missing something on the way, 
i.e commit a file without changelog content being updated. So because of this we run 
tasks as an array of functions in series using [run-series](https://github.com/feross/run-series).

### Default Options

option            | type     | description
------------------|----------|-------------
`currentVersion`  |`string`  | the current project version defined in your `package.json`.
`bumpFiles`       |`array`   | files that should have version property bumped.
`commitFiles`     |`array`   | files that should be commit after changes.
`commitMessage`   |`string`  | the format of the commit message for the release.
`changelog`       |`object`  | default options for the changelog.
-`gitChangeLog`  |`string`  | the git command used to list and format the changelog.
-`gitChangeLogArguments`| `array` | list of arguments that git should run to generate the changelog.
-`filename`      |`string` | the change log file name.
-`header`        |`string` | the default header for changelog.
-`template`      |`string` | the default format for the list of the latest commit.
`tagFormatCmd`    |`string` | the git command to format the tag message commit.
`tagName`         |`string` | the default tag format i.e: `v1.2.3`.
`tagMessage`      |`string` | the default title for the annotated tag message.


### Function Tasks

Every task receives two params, the incremented version string and the options

```js
function task(version, options){ 
   return new Promise((resolve, reject) => {
      //
   })
}
```

When build a custom task the function will always receive the bumbed version and the options of the prompt.

## Author

**Adriano Rosa** [@adrianorosa](https://twitter.com/adrianorosa)  
https://adrianorosa.com

## Licence

Copyright © 2019, Adriano Rosa  <info@adrianorosa.com>
All rights reserved.

For the full copyright and license information, please view the LICENSE 
file that was distributed within the source root of this project.
