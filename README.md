# Design Tokens

The Open Inwoner project has the goal to follow the
[NL Design System](https://github.com/nl-design-system). We organize
the design tokens in JSON files and use them within the Open Inwoner
backend project.

## How it works

Specify the design tokens in JSON files, which are picked up and merged using the
[style-dictionary](https://www.npmjs.com/package/style-dictionary) library. The resulting packages
include various build targets, such as ES6 modules, CSS variables files, SASS vars... to be consumed
in downstream projects.

The draft [Design Token Format](https://design-tokens.github.io/community-group/format/) drives the
structure of these design tokens.

## Add this as a submodule

In root folder of your project, create a dir or directly add the submodule in its own directory:

```bash
cd open-inwoner-design-tokens

git submodule add git@github.com:maykinmedia/open-inwoner-design-tokens.git

git status

git submodule update --init
```

Do not forget to commit these changes to your repository.

If you are using Github actions, add these to your script:

```bash
      - uses: actions/checkout@v3
        with:
          submodules: 'true'
```


## Usage

Install the necessary modules (from inside the `open-inwoner-design-tokens` directory):

```bash
npm ci --legacy-peer-deps
```

Generate the CSS files

```bash
npm run build
```

In order to update an existing Git submodule, you need to execute the “git submodule update” with the “–remote” and the “–merge” option.

`$ git submodule update --remote --merge`

**Using tokens**

If you are only _consuming_ the design tokens, the easiest integration path is adding this
package in your own project.

Then, import the desired build target artifact and run your usual build chain.

**Developing and using tokens**

If you actively need to add or change design tokens, we recommend installing the package locally and
using npm workspaces or `npm link` for the least-friction experience. You can include the package as
a git-submodule and leverage npm workspaces with instructions in the downstream projects.

This allows you to create atomic PRs with design token changes, while being able to develop against
the newest changes.

Run:

```bash
npm start
```

to start the watcher which will re-build on changes.

To prettify files:

Run:

```bash
npm run format
```

## Naming pattern

Because of the way style-dictionary works, you have to pay close attention to the structure of the
tokens. E.g. if you have two tokens definition files like:

```json
{
  "oip": {
    "color": {
      "fg": {"value": "#000000"}
    }
  }
}
```

```json
{
  "oip": {
    "color": {
      "fg": {
        "muted": {"value": "#000000"}
      }
    }
  }
}
```

Then only `--oip-color-fg` will be emitted since the merged object sees a `value` key at the
`oip.color.fg` path.

You can usually avoid this by sticking to a structure adhering to:

```
<prefix>.<component>.<modifier>.<UIState>.<CSSProperty>
```

Where `UIState` can be blank or a value like `hover`, `active`...

Alternatively, if the structure is not that important, you can put the tokens on the same level,
e.g.:

```json
{
  "oip": {
    "color": {
      "fg-muted": {"value": "#000000"}
    }
  }
}
```

The latter form is harder to keep track off across files though.
