# Flask + Svelte + TS + Vite

This template should help get you started developing with Svelte and TypeScript in Vite served via Flask.

## How it was prepared

Create a new project:
```shell
mkdir flasvelte
```

Create a new Flask project:
```shell
cd flasvelte
pip install flask
touch server.py
```
Then, add a minimum code to start the flask server:
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def base():
    return "Hello, World!"

if __name__ == '__main__':
    app.run(debug=True)
```

Prepare client:
```shell
mkdir client
cd client
npm init vite
# select Svelte then finish it
```

Let Flask serve the static files of Svelte:
```python
from flask import Flask, send_from_directory
import random

app = Flask(__name__)

@app.route('/')
def base():
    return send_from_directory('client/dist/', 'index.html')

# Path for all the static files (compiled JS/CSS, etc.)
@app.route('/<path:path>')
def static_proxy(path):
    return send_from_directory('client/dist/', path)

@app.route('/random')
def random_number():
    return str(random.randint(1, 100))

if __name__ == '__main__':
    app.run(debug=True)
```

## Recommended IDE Setup

[VS Code](https://code.visualstudio.com/) + [Svelte](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode).

## Technical considerations

**Why not using SvelteKit?**

- It brings its own routing solution which might not be preferable for some users.
- It is first and foremost a framework that just happens to use Vite under the hood, not a Vite app.

This template contains as little as possible to get started with Vite + TypeScript + Svelte, while taking into account the developer experience with regards to HMR and intellisense. It demonstrates capabilities on par with the other `create-vite` templates and is a good starting point for beginners dipping their toes into a Vite + Svelte project.

Should you later need the extended capabilities and extensibility provided by SvelteKit, the template has been structured similarly to SvelteKit so that it is easy to migrate.

**Why `global.d.ts` instead of `compilerOptions.types` inside `jsconfig.json` or `tsconfig.json`?**

Setting `compilerOptions.types` shuts out all other types not explicitly listed in the configuration. Using triple-slash references keeps the default TypeScript setting of accepting type information from the entire workspace, while also adding `svelte` and `vite/client` type information.

**Why include `.vscode/extensions.json`?**

Other templates indirectly recommend extensions via the README, but this file allows VS Code to prompt the user to install the recommended extension upon opening the project.

**Why enable `allowJs` in the TS template?**

While `allowJs: false` would indeed prevent the use of `.js` files in the project, it does not prevent the use of JavaScript syntax in `.svelte` files. In addition, it would force `checkJs: false`, bringing the worst of both worlds: not being able to guarantee the entire codebase is TypeScript, and also having worse typechecking for the existing JavaScript. In addition, there are valid use cases in which a mixed codebase may be relevant.

**Why is HMR not preserving my local component state?**

HMR state preservation comes with a number of gotchas! It has been disabled by default in both `svelte-hmr` and `@sveltejs/vite-plugin-svelte` due to its often surprising behavior. You can read the details [here](https://github.com/rixo/svelte-hmr#svelte-hmr).

If you have state that's important to retain within a component, consider creating an external store which would not be replaced by HMR.

```ts
// store.ts
// An extremely simple external store
import { writable } from 'svelte/store'
export default writable(0)
```
