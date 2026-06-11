# ET3 Designer Library – Initial Implementation Report

## Overview

Instead of each developer creating his own colors.ts or any other constants files shared between the developers and designers, they use this.

The main objectives of the ET3 Library were:

- Create a centralized repository for design tokens.
- Allow developers to install shared design assets through npm.
- Avoid recreating files such as `colors.ts` in every project.
- Establish a scalable foundation for future design system growth.
- Explore secure package distribution mechanisms using GitHub Packages.

---

# Technologies Used

## Language

- TypeScript

## Bundler

- tsup
  Used to build the package and generate:

- ECMAScript modules (ESM)
- CommonJS modules (CJS)
- TypeScript declaration files (`.d.ts`)

## Package Registry

- GitHub Packages

Chosen because it supports:

- Private packages
- GitHub Organization permissions
- Version control integration
- Controlled access to internal libraries

---

# Library Structure

The initial structure was intentionally kept simple.

```text
ET3-Library/
├── dist/
├── src/
│   ├── colors.ts
│   └── index.ts
├── package.json
├── tsconfig.json
├── tsup.config.ts
└── README.md
```

---

# Implementation Steps to create this library.

---

## Step 1 – Create the Repository

A dedicated repository was created to isolate the designer assets from application-specific code.

Repository:

```text
ET3-Library
```

---

## Step 2 – Initialize npm

The project was initialized using:

```bash
npm init -y
```

---

## Step 3 – Install Development Dependencies

The following dependencies were installed:

```bash
npm install -D typescript tsup
```

---

## Step 4 – Configure TypeScript

A `tsconfig.json` file was created.

Configuration included:

- ES2020 target
- strict mode
- declaration generation
- module interoperability

---

## Step 5 – Implement Design Tokens

The first version focused exclusively on color definitions.

Example:

```typescript
export const Colors = {
  PRIMARY: "#0055FF",
  SECONDARY: "#FF7A00",
  SUCCESS: "#22C55E",
  ERROR: "#EF4444",
} as const;
```

---

## Step 6 – Create Public Exports

An `index.ts` file was created to expose the library API.

Example:

```typescript
export * from "./colors";
```

This allowed consumers to import using:

```typescript
import { Colors } from "@scope/designer-library";
```

---

## Step 7 – Configure tsup

A `tsup.config.ts` file was added.

Configuration:

```typescript
import { defineConfig } from "tsup";

export default defineConfig({
  entry: ["src/index.ts"],
  format: ["esm", "cjs"],
  dts: true,
  clean: true,
});
```

---

## Step 8 – Build the Library

The library was built using:

```bash
npm run build
```

Generated artifacts included:

```text
dist/
├── index.js
├── index.cjs
├── index.d.ts
└── index.d.cts
```

---

## Step 9 – Local Validation

The package was validated locally using npm linking to ensure imports worked correctly before publishing.

This verified that applications could consume the package successfully.

---

## Step 10 – Configure GitHub Packages

GitHub Packages was selected for package distribution.

The package configuration was updated:

```json
"publishConfig": {
    "registry": "https://npm.pkg.github.com"
}
```

---

## Step 11 – Authenticate npm with GitHub

A GitHub Personal Access Token (PAT) was generated.

Required scopes included:

- repo
- read:packages
- write:packages

npm authentication was configured through:

```text
~/.npmrc
```

Example:

```text
@scope:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=TOKEN
```

---

## Step 12 – Publish the Package

The package was published using:

```bash
npm publish
```

A successful publication was achieved under the personal GitHub scope:

```text
@mahmoudalhajj/designer-library@1.0.0
```

# How to Run the Library

## Install Dependencies

```bash
npm install
```

---

## Build the Package

```bash
npm run build
```

Publish:

```bash
npm publish
```

---

# How to Use the Library

Configure authentication:

```text
@scope:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=TOKEN
```

Install:

```bash
npm install @scope/designer-library
```

Use:

```typescript
import { Colors } from "@scope/designer-library";

console.log(Colors.PRIMARY);
```

---

# Future Improvements

Planned additions include:

- Typography tokens
- Spacing tokens
- Border radius tokens
- Theme configuration
- Shared UI components
- Automated publishing using GitHub Actions
- Migration from personal scope to ET3 organizational scope

---

# Conclusion

The proof of concept successfully demonstrated that a centralized designer library can be created, versioned, published, and consumed across applications.

The implementation established a scalable foundation for a future ET3 design system while validating GitHub Packages as a secure distribution mechanism for internal assets.
