# Investigate

### Struct

- ppsb-web ( monorepo )
 /apps/
  /web/
 /packages/
  /store/
  /services/

- ppsb-platform
 /modules/routing ( to be done )
 /modules/utils
 /modules/catalogue
 /modules/analytics
  
- ppsb-abacus

#### What to do
- Move store and services to a monorepo with web
- Migrate abacus to github ( already been done )

### Nx

Nx is a powerful open-source build system that provides tools and techniques for enhancing developer productivity, optimizing CI performance, and maintaining code quality

Nx helps to speed up computation (e.g. builds, tests etc), locally and on CI and to integrate and automate your tooling via its plugins. All of this can be adopted incrementally.

The Nx package provides fundamental technology-agnostic capabilities such as: workspace analysis, task running, caching, distribution, code generation and automated code migrations.


├── apps/
│   └── SMS
│       └── package.json
├── libs
│   └── store
│       ├── src
│       ├── tsconfig.json
│       └── package.json
│   └── services
│       ├── src
│       ├── tsconfig.json
│       └── package.json
├── nx.json
├── package.json
└── tsconfig.base.json

With NX VS Code pluggin we can see graphically the dependencies of each package.

We can add a task pipeline to configure the builds of each package that depends on. 

{
  ...
  "targetDefaults": {
    "build": {
      "dependsOn": ["^build"]
    }
  }
}

^^ This tells Nx to run the build target of all the dependent projects first, before the build target of the package itself is being run

Nx enable to use cache when we want to run our scripts of build and test! 
However nx generates the hash to the cash looking for all the scr files, including e.g README.md

When build the nx.json file we can tell to nx to not look to that custom files, e.g: 

{
  ...
  "targetDefaults": {
    "build": {
      "inputs": ["!{projectRoot}/**/*.md"]
    },
    "test": {
      "inputs": ["!{projectRoot/**/*.md"]
    },

  }
}

and to not repeat the inputs that we want to avoid, we can define before, e.g:

{
  ...,
  "namedInputs":{
    "noMarkdown": ["!{projectRoot}/**/*.md"]
  },
  "targetDefaults": {
    "build": {
      "inputs": ["noMarkdown"]
    },
    "test": {
      "inputs": ["noMarkdown"]
    },

  }
}


#### CI
- Nx provides CI configurations like [Nx CI config](https://nx.dev/recipes/ci/monorepo-ci-github-actions#configuring-ci-using-github-actions-and-nx)
  - Jenkins
  - Github Actions

#### Adding Nx to NPM/Yarn/PNPM Workspace

Nx has first-class support for package-based monorepos. 

As a result, if you have an existing NPM/Yarn or PNPM-based monorepo setup, you can easily add Nx to get:
- fast task scheduling
- support for task pipelines
- caching
- optionally remote caching with Nx Cloud
- optionally distributed task execution with Nx Cloud

This is a low-impact operation because all that needs to be done is to install the nx package at the root level and add an nx.json for configuring caching and task pipelines.

[Nx instalation on npm workspaces](https://nx.dev/recipes/adopting-nx/adding-to-monorepo#installing-nx) 

 npm add store -w my-app --> command to add packages 
           |           |   
           ^           ^
    package to add     package destiny


### npm workspaces

Workspaces is a generic term that refers to the set of features in the npm cli that provides support to managing multiple packages from your local files system from within a singular top-level, root package.

This set of features makes up for a much more streamlined workflow handling linked packages from the local file system. Automating the linking process as part of npm install and avoiding manually having to use npm link in order to add references to packages that should be symlinked into the current node_modules folder.

We also refer to these packages being auto-symlinked during npm install as a single workspace, meaning it's a nested package within the current local file system that is explicitly defined in the package.json workspaces configuration.

We also have this tooling on plaform


# SCRIPTS

npx nx build store ---> build package store 

