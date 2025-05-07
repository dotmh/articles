---
title: "My Typescript Stack in 2025"
datePublished: Wed May 07 2025 23:00:36 GMT+0000 (Coordinated Universal Time)
cuid: cmaejknxv000b0aihh894h2cj
slug: my-typescript-stack-in-2025
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/Y376h7VN27c/upload/69ffe6f3b73d8b0e839775ce812fa8e0.jpeg
tags: github, nodejs, stack, typescript, eslint, istanbul, nvm, prettier, pnpm, github-actions-1, vitest, conventional-commits, devcontainers, commitlint, secretlint

---

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">TL;DR: See my Typescript <a target="_self" rel="noopener noreferrer nofollow" href="https://github.com/dotmh/ts" style="pointer-events: none">GitHub template</a>, the <a target="_self" rel="noopener noreferrer nofollow" href="https://github.com/dotmh/ts/blob/main/README.md" style="pointer-events: none">Readme</a> includes how to get started</div>
</div>

If you have read any of my other posts, you are likely aware that my primary coding stack is based on TypeScript. Working in modern JavaScript can be complex; a modern JavaScript project often involves numerous moving parts to be productive and comfortable. Setting up these moving parts can be a time-consuming process. I tend to create numerous projects, whether a library, a whole project, or an experiment; I want all my standard tooling. I therefore decided to make a [GitHub template](https://github.com/dotmh/ts) repository that I can use to generate new repositories when I want to work with TypeScript.

This post will walk through the template's stack and explain the rationale behind choosing each part. This doesn’t mean you have to use this technology, but for me, these are the best technologies currently available in 2025. The [Readme](https://github.com/dotmh/ts/blob/main/README.md) covers the setup and other helpful information if you want to use the template.

We recently adopted this stack at work, which means it is also used professionally.

## Node

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746026088273/6b9a071f-2a3c-4094-9bd3-5547bfa6749c.png align="center")

To manage nodes, I use [NVM.](https://github.com/nvm-sh/nvm) I have used it for a long time and am comfortable using it. I also like that you can switch node versions when entering a node project by detecting the `nvmrc` file. It is also well-supported by other tooling and [VSCode,](https://code.visualstudio.com/) my primary IDE.

## Package Manager

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746025836923/efedee8f-5dd3-43ea-941b-a0b07ba30cea.png align="center")

Nowadays, I almost exclusively use [PNPM](https://pnpm.io/) to manage my dependencies in Node projects. There are two main reasons for that.

The first is that PNPM provides excellent tooling for managing a monorepo through its workspace tooling. You can easily link packages under the repo, build and package code for deployment and scope commands to a package within a workspace.

The other significant benefit of PNPM is that it reduces the space required for your packages on a disk. We have all, by now, seen the meme of `node_modules` Having a density greater than that of a black hole, being able to reduce it is a massive benefit.

You can read more about the motivation behind PNPM and how it reduces install sizes on the [PNPM Website](https://pnpm.io/motivation).

## Typescript

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746025911937/467f89b1-a9b3-4602-b6c6-3ac8c758eea0.png align="center")

It shouldn’t come as a shock now, but the central part of my stack is [Typescript](https://www.typescriptlang.org/). I use TypeScript because it makes JavaScript easier and safer to use. For anyone who hasn’t encountered TypeScript yet, it is essentially a superset of JavaScript, meaning that all valid JavaScript is also valid TypeScript. However, TypeScript supports type annotations. This adds safety to working with JavaScript, reducing bugs and edge cases. It also reduces the number of tests you write, as the TypeScript compiler can do the work.

Like many influential and essential tools, Typescript requires a commitment to learn how to use it effectively and maximise its benefits. However, it makes a big difference in building reliable and robust codebases that run anywhere JavaScript can.

## Linting

### ESLint

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746110925678/18979ba3-cb45-40d9-942a-7b3a23d7dc62.png align="center")

I use ESLint partly out of habit but primarily because it allows me the most flexibility when defining the code style I want. Although opinionated, pre-configured tooling is available to enforce a set of sensible defaults, I prefer to manage code style rules myself.

I bundled a shared configuration under the GitHub repository [**shared-typescript-configuration**](https://github.com/dotmh/shared-typescript-configuration)**,** which allows me to share my ESLint settings across projects. I then distribute these settings as an [NPM module](https://www.npmjs.com/package/@dotmh/eslint-config-ts), which I can consume in the project. This is also how the template is set up, and it creates its own `eslint.config.mjs` in the root, exports the config from the node module.

```javascript
import base from '@dotmh/eslint-config-ts';

export default [
  ...base,
  {
    ignores: ['dist/', '/coverage', 'node_modules/', '**/*.js', '**/*.mjs'],
  },
];
```

### Prettier

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746111011779/9458fbcb-d3f9-4f3f-8c2d-b0c2784cab02.png align="center")

Whereas ESLint primarily targets code smells and enforces good practices, Prettier enforces the actual physical style of the code, including aspects such as which quotes to use. I prefer Prettier for many of the same reasons I prefer ESLint; it allows me to control my code style. Both tools require a little time to set up and tune to your liking, but once done, they’re set.

Again, like with ESLint, I bundle my settings into an [NPM module](https://www.npmjs.com/package/@dotmh/prettier-config) whose source comes from the same [shared-typescript-configuration](https://github.com/dotmh/shared-typescript-configuration) repository on GitHub.

### Secret Lint

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746111428533/64004514-22bb-4a3b-a8a1-ddf082417baf.png align="center")

Secret Lint scans code for secrets and a [Husky](https://typicode.github.io/husky/) hook that triggers when anyone attempts to commit, thereby reducing the risk that a Secret will be included in the repository. Due to the way GitHub operates to facilitate collaboration, it is complicated to remove something once it is added to a repository. Secret scanning tools that work at the CI level or as a GitHub app are great, but they don’t prevent a secret from being introduced in the first place. This lack of prevention means that once these tools detect a secret being committed, you must rotate all your secrets, which is not a small amount of work.

### Commit Lint

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746116098888/ab26f007-4c9a-49c1-8857-8bafdbf8fa22.png align="center")

Commit lint instead of linting code; it lints the commit messages. I have it set up to enforce [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/). I prefer conventional commits because they provide a structure for writing clear and concise commit messages. I am terrible at writing commit messages, let alone meaningful commit messages, but with conventional commit messages as a guide, the situation is greatly improved. I also know myself and if nothing is enforcing me to use conventional commits I will end, when I am rushed, tired or excited falling back to my typical bullshit, hence the need for a linter.

## Testing

### Vitest

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746518664312/6b82cede-b095-437c-ad12-829e18ed83a3.png align="center")

I was recently introduced to vitest by a friend and colleague as an alternative to [Jest](https://jestjs.io/). It labels itself as a fast, Jest-compatible testing framework that integrates with [Vite](https://vite.dev/). This seemed ideal for me; until recently, Jest was my preferred testing framework, Vite is my preferred build tool, and it had native support for both TypeScript and code coverage reporting with Istanbul. Vitest also had good support for working within a monorepo, which I extensively utilise (although Jest may have had that support too, I never looked into it). The template mentioned in the article is set up as a monorepo. I used Vitest on a number of both backend and frontend projects, and found it easy to use, integrate and have a nice set of functions and keywords; I like the “describe”, “it” and “expect” way of writing tests.

### Istanbul

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746518681306/b7e13e7e-33f8-4c41-97d0-9cf32999d3d9.png align="center")

Code coverage seems to have a lot of baggage in the developer world. I like it, as it is a valuable guide to making sure that I am writing code tests covering the functionality. I don’t think it is a metric that should have much enforcement. It should be a guide. There are several downsides to relying on code coverage because, like all metrics, it can lead to a false sense of security and wasted effort without understanding what it tells you. I use Istanbul out of habit; its HTML report is easy to use and read.

## GitHub Actions

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746518702992/e78badc2-e232-42ff-a36a-cede2b60660f.png align="center")

Continuous integration and continuous deployment are potent tools for software development. Although there has been some interesting [discussion](https://world.hey.com/dhh/we-re-moving-continuous-integration-back-to-developer-machines-3ac6c611) lately about whether it is worth using a service, it is still considered valid and a requirement by many organisations. I have used many CI systems, including JetBrains [TeamCity](https://www.jetbrains.com/teamcity/), [Semaphore](https://semaphore.io/) and [Circle CI](https://circleci.com/). However, my favourite CI platform was [GitLab CI](https://docs.gitlab.com/ci/). Its ease of use and deep integration with the code repositories made it pleasant to work with.

For several reasons, though, I am now mostly using GitHub, so when they launched GitHub Actions, I was interested in what it offered. GitHub Actions is more than just a plain CI/CD tool; it is an event-based system that can trigger actions based on a load of events in GitHub. This allows you to build powerful workflows using it. GitHub Actions has an ecosystem; you can use first- and third-party actions in your workflows. This allows for rapid workflow development, though it exposes you to supply chain attacks like any package-based ecosystem.

I chose to use GitHub Actions in my template for these reasons. This means users don’t have to sign up for third-party services. If they are using the GitHub template, then they are already on GitHub.

## Dev Containers and GitHub Codespace

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746549071410/8a465a87-9e58-4662-b514-8e50fc8b5a82.png align="center")

Docker changed the development world. With a single command, you can have a reproducible, lightweight runtime environment that matches production to run your app. Imagine having that for your development environment. Well, that is Dev Containers. Dev Containers integrate with [VS Code](https://code.visualstudio.com/) and most [JetBrains IDES](https://www.jetbrains.com/ides/), allowing you to work seamlessly within the dev container as if it were your file system while having all the tools you need in an environment close to your production environment.

I also like Dev Containers because I love to explore other languages and tools. They allow me to test and play with those without the overhead of installing them into my main machine. This benefits from testing in a virtual machine, but with the lightweight overhead of containers.

Github Codespaces builds on top of Dev Containers, allowing you to run a dev environment in the cloud that you can either attach to through a browser with VS Code, or directly in the VS Code app or JetBrains IDE through [JetBrains Gateway](https://www.jetbrains.com/remote-development/gateway/). This is great if you want to work on a project without downloading it or using someone else’s or an underpowered machine. Codespaces cost money and aren’t the end-all solution for daily development, but can be really useful in certain situations.

I want to write a full post on dev containers soon, but if you wanna learn more, I have a [repository](https://github.com/dotmh/devcontainer) that contains several dev containers that are also deployed as [packages](https://github.com/dotmh?tab=packages&repo_name=devcontainer) in GitHub, ready for use.

%%[cc-license]