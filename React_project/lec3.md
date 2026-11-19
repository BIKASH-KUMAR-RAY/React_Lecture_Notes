# NPM and React Setup

## 1. What is NPM?

**NPM (Node Package Manager)** is a central package registry and package manager where packages like React, React DOM, Babel, and other JavaScript libraries are available.

It helps us install, manage, and use packages in our JavaScript projects.

## 2. How to Use NPM?

### Step 1: Initialize the Project

```bash
npm init
```

After that, give information like the project name and description. You can skip whatever you like initially.

You can also use:

```bash
npm init -y
```

This creates a `package.json` file with default information without asking questions.

### Step 2: Install React and React DOM

```bash
npm install react react-dom
```

All the installed packages get stored in the `node_modules` directory.

- **React:** Used to build user interfaces.
- **React DOM:** Used to render React components into the browser's DOM.

### Step 3: Install Vite

```bash
npm install -D vite
```

Vite is a development tool that provides a development server and builds our application for production.

---

## 3. Dev Dependencies vs Dependencies

### Dependencies

Dependencies are packages required by the application to run in production.

For example:

```bash
npm install react react-dom
```

React and React DOM are dependencies because our application needs them to run.

### Dev Dependencies

Dev dependencies are packages primarily required during development or the build process.

For example:

```bash
npm install -D vite
```

**Why is Vite a dev dependency?**

- Vite is a dev dependency since it is primarily required for development and building the application, not for running the already-built website in production.
- Vite is a bundler and build tool that processes and bundles files and optimizes the output for production.
- Once the bundling is done, Vite itself is not required to serve the built static files in a typical production deployment.
- Babel is often used as a development dependency when it is needed to transform JavaScript or JSX.

**Note:** We don't require Babel separately in a typical Vite React project because Vite's React tooling handles JSX transformation. Vite uses tools such as esbuild for many transformations, while production builds use Rollup-based bundling. The exact tools depend on the configuration.

We don't necessarily need to install Babel manually when using Vite.

### Why Does `node_modules` Contain So Many Folders?

The `node_modules` directory consists of many folders because Vite and other packages depend on additional packages.

These dependencies may have their own dependencies, which are called **transitive dependencies**.

For example:

```text
my-project/
├── node_modules/
│   ├── react/
│   ├── react-dom/
│   ├── vite/
│   └── other dependencies...
├── package.json
└── package-lock.json
```

Vite does not necessarily take other packages' code and make changes to it. Instead, packages declare dependencies on other packages, which NPM installs as needed.

---

## 4. Understanding Version Numbers

Example:

```text
^19.3.0
```

The three parts of a version number are:

```text
19 . 3 . 0
│   │   │
│   │   └── Patch
│   └────── Minor
└────────── Major
```

This is known as **Semantic Versioning (SemVer)**.

### Major Version

A major update may introduce breaking changes that affect existing code.

For example:

```text
1.0.0 → 2.0.0
```

When we get a major update, existing functions or APIs may change, which can cause previous code to stop working.

### Minor Version

A minor update adds new features while maintaining backward compatibility.

For example:

```text
1.2.0 → 1.3.0
```

When new functionality or features are added without breaking existing functionality, a minor update is given.

### Patch Version

A patch update usually fixes bugs or security issues without introducing breaking changes.

For example:

```text
1.2.0 → 1.2.1
```

We get a patch update when bugs are fixed, and then the patch value changes.

### What Does the `^` Symbol Mean?

Example:

```json
{
  "dependencies": {
    "some-package": "^1.2.3"
  }
}
```

The `^` symbol indicates that NPM can accept compatible updates within the same major version.

For example, with `^1.2.3`, compatible versions such as `1.2.4` and `1.3.0` may be accepted, but `2.0.0` is not accepted by that version range.

**Important:** The `^` symbol does not automatically update the package. It specifies which versions are allowed when NPM resolves or updates dependencies.

### What Is `package-lock.json`?

`package-lock.json` stores the exact resolved versions of dependencies and their dependency tree.

It helps ensure that the project installs consistent dependency versions across different machines.

For example:

```text
package.json
    ↓
Specifies the allowed dependency versions

package-lock.json
    ↓
Records the exact resolved versions
```

---

## 5. Running a React Project Using Vite

While using Vite, you can run the development server using:

```bash
npx vite
```

Change your `app.js` extension to `app.jsx` because we are using JSX syntax in the file.

**Note:** JSX can be used with different file extensions when the tooling supports it, but `.jsx` makes it clear that the file contains JSX.

### Import React and React DOM

For using React and React DOM, we need to import them from the installed packages in `node_modules`.

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
```

- `React` provides React functionality.
- `ReactDOM` provides browser DOM rendering functionality.
- `react-dom/client` provides the `createRoot()` API.

In modern React projects using the automatic JSX transform, importing `React` is not always necessary just to write JSX. However, your original import is valid.

### Add `type="module"` to the Script

Write `type="module"` because our JavaScript file uses ES module syntax, such as `import`.

```html
<script type="module" src="./app.jsx"></script>
```

This tells the browser to treat the script as a JavaScript module.

**Important:** In a Vite project, the development server processes the JSX file before serving it to the browser. You should run the project through Vite rather than opening the HTML file directly.

### Configure the `package.json` File

Go to `package.json`, then inside the `"scripts"` section, write:

```json
"scripts": {
  "dev": "vite"
}
```

Now you can run the development server using:

```bash
npm run dev
```

Vite will provide a local URL, usually something like:

```text
http://localhost:5173/
```

Open that URL in your browser to see your application.

### Add the Build Script

Inside the `"scripts"` section, also write:

```json
"scripts": {
  "dev": "vite",
  "build": "vite build"
}
```

The `build` command processes and optimizes the files and creates a production-ready build.

Run:

```bash
npm run build
```

By default, Vite generates the production build inside the `dist` directory.

```text
my-project/
├── src/
├── node_modules/
├── dist/
├── package.json
├── package-lock.json
└── index.html
```

The `dist` directory contains the generated files that can be deployed to a static hosting service or served by a production web server.

**Development vs Production**

| Command | Purpose |
|---|---|
| `npm run dev` | Starts the development server |
| `npm run build` | Creates the production build |
| `npm run preview` | Locally previews the production build |

To preview the production build, you can add or use this script:

```json
"preview": "vite preview"
```

Then run:

```bash
npm run preview
```

---

## 6. The Easier Way to Create a React Project

Instead of manually installing React, React DOM, and Vite and configuring everything, write:

```bash
npm create vite@latest
```

Then choose the project name, framework, and variant when prompted.

For a React project:

1. Enter your project name.
2. Choose **React** as the framework.
3. Choose **JavaScript** or **TypeScript** as the variant.
4. Move into the project directory.
5. Install the dependencies.
6. Start the development server.

Commands:

```bash
npm create vite@latest
cd my-project
npm install
npm run dev
```

Replace `my-project` with the actual project directory name you selected.

That's it! Your React project is ready to run.

---

## 7. Quick Revision

| Concept | Meaning |
|---|---|
| NPM | Package manager for JavaScript |
| `npm init` | Initializes a project and creates `package.json` |
| `npm install` | Installs dependencies |
| `npm install -D vite` | Installs Vite as a development dependency |
| `node_modules` | Stores installed packages and their dependencies |
| `package.json` | Stores project metadata, scripts, and dependency ranges |
| `package-lock.json` | Records exact resolved dependency versions |
| React | Library for building user interfaces |
| React DOM | Connects React to the browser DOM |
| Vite | Development server and build tool |
| `.jsx` | File extension commonly used for JavaScript with JSX |
| `type="module"` | Enables JavaScript module syntax in browser scripts |
| `npm run dev` | Starts the development server |
| `npm run build` | Builds the application for production |
| `npm run preview` | Previews the production build locally |
| `^` | Allows compatible updates within the same major version |

### Important Commands to Remember

```bash
npm init
npm install react react-dom
npm install -D vite
npx vite
npm run dev
npm run build
npm run preview
npm create vite@latest
```

**Remember:** For a new React project, `npm create vite@latest` is generally the easiest starting point because it sets up the project structure and configuration for you.