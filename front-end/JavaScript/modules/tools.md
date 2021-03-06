# Tools - Linting & Webpack & NPM


## Things to know

### Helpful tools when compiling your code

| **Tool Purpose** | **Tool Name** | **What it does & Why** | 
| ------------ |---------- | ------------------ |
| A Development | **webpack-dev-server** or **serve** | Serve under (more) realistic circumstances, auto-reload |
| Building Tool | **webpack** | Combine multiple files into bundled code (less files) |
| Code Optimization Tool | **webpack optimizer** | Optimize code(shorten funciton names, remove white spaces, ...) |
| Code Compilation Tool | **Babel** | Write modern code, get "older" browser support as an ouput |
| Code Quality Checker | **ESLint** | Check code quality, check for conventions & patterns |

### Workflow

We need NodeJS & npm installed globally to utilize in order to work with the above tools.
The desired workflow is the following,

#### During Development (upon "Save")
Linting (EsLint) -> Bundling (Webpack) -> Reload Dev Server

#### For Production (upon Command)
Linting (ESLint) -> Bundling (Webpack) -> Compilation (Babel) -> Optimization -> Production-ready Code