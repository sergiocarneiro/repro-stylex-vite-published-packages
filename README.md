# StyleX Vite Published Packages Issue Reproduction

A minimal reproduction project to demonstrate the issue with StyleX and Vite not compiling definitions in published packages.

When running the app, only a blank page is displayed, and the console shows the error:

```
Unexpected 'stylex.create' call at runtime. Styles must be compiled by '@stylexjs/babel-plugin'.
```

Removing the use of StyleX definitions from the library, leaving only the local StyleX definitions in `App.tsx`, resolves the issue, and the app displays correctly.

## Steps to Reproduce

1. Run `npm install` in the root directory to install dependencies for both packages.
2. Run `npm run dev` in the `app/` directory to start the Vite development server.
3. Visit `http://localhost:5173` in your browser.
4. Observe that the app is displaying correctly at this point.
5. Uncomment line 14 in `app/src/App.tsx` which uses the library's StyleX definitions (`color: colors.green`).
6. Refresh the browser to see a blank page and the console error.

## Project Structure

This project consists of *fully isolated* packages, that consume `library` via the `file:` protocol for simulating the package being published:

- [`app/`](./app/) - A minimal React + Vite application
- [`library/`](./library/) - A minimal library that exports StyleX definitions

### Additional Experiments

- [`react-router-app/`](./react-router-app/) - A minimal React Router + Vite application — _this one works correctly_
- [`react-router-cloudflare-app/`](./react-router-cloudflare-app/) - A minimal React Router + Vite + Cloudflare Workers application — _fails with the same error_

## Key Files

- [`app/src/App.tsx`](./app/src/App.tsx) - The main React component that uses StyleX both locally and from the library
- [`app/vite.config.ts`](./app/vite.config.ts) - Vite configuration that compiles StyleX using Babel and PostCSS
- [`library/src/index.ts`](./library/src/index.ts) - A `stylex.create` definition
- [`library/src/tokens.stylex.ts`](./library/src/tokens.stylex.ts) - A `stylex.defineVars` definition
