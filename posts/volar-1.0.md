---
title: Volar 1.0 "Nika" Released!
date: 2022-10-10
author: Johnson Chu
avatar: https://avatars.githubusercontent.com/u/16279759?v=4
twitter: '@johnsoncodehk'
---

We are happy to announce that we have released v1.0 of Volar, the official IDE/TS tooling support for Vue! 🎉

This major version ships with tons of improvements across the board. In addition to improving UX, performance, and package size, we also released Plugin API v1 and refactored the core code to be framework-agnostic.

注：結尾附有中文版本 (There is Chinese version of this post at the end)。

---

Earlier this year in March, Vue creator Evan You started sponsoring me ([@johnsoncodehk](https://twitter.com/johnsoncodehk)) to work full-time on the development of Volar, with the goal of pushing it towards 1.0. After 7 months of hard work, we have finally achieved this goal!

There are so many updates that you most likely missed some. Below we have summarized the most significant changes from the past 7 months:

### Feature Updates

- Implemented `Goto Code` and `Highlight Selection Dom Elements` for Vite and Nuxt 3 Preview (highly recommend you try it out!)
- Implemented [Component Preview](https://github.com/johnsoncodehk/volar/discussions/1511)
- Added setting `format.initialIndent` to specify the initial indent of SFC blocks
- Implemented support for Web IDE
- No longer built-in support for `<template lang="pug">` (In v1.0 you need to install [@volar/vue-language-plugin-pug](https://www.npmjs.com/package/@volar/ vue-language-plugin-pug))

### Out-of-the-Box Usage Improvements

- No longer needs `"jsx": "preserve"` by default, and also will no longer conflict with `@types/react` (unless explicitly enabled in `vueCompilerOptions.jsxTemplates`)
- Always wraps component options in `defineComponent()` or `Vue.extend` by default so that you can simply export an object literal.
- Unknown component tags is not in warning color anymore.
- TypeScript support is improved in `<template>` block for JS components
- Reduce the need to read the README for new users. Common project setup problems will be automatically detected and prompted in status bar (such as incorrectly setting `vueCompilerOptions.target`)

### UX / DX Improvements

- Takeover mode no longer shows a pop-up when enabled
- There is a new faster command to use when user needs to restart server: `Reload Project`. This command won't restart the server but will clear the cache. `Restart Vue Server` is still available to hard-reload the server.
- "Show Virtual Scripts" command is added. Check out how it works [here](https://twitter.com/johnsoncodehk/status/1577046525490126848).
- Incrementally calculate error range for old diagnostic results to prevent annoying flickering problem
- Added support for adding and switching workspaces without restarting the server
- Simplified display of name casing tool in the status bar, and also supports prop name casing conversion

### TypeScript Improvements

- vue-tsc supports `--watch`
- vue-tsc supports `--declaration --emitDeclarationOnly` to emit dts for components
- Support for `Inlay Hints`
- Support `Find File References`
- Support for [JavaScript and TypeScript Nightly](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-typescript-next)
- Fixed `<script>` types could not be used in `<template>`
- Added `vueCompilerOptions.strictTemplates` option to support stricter template type checking for report unknown component tag and props

### Performance Improvements

- Much faster SourceMap mapping
- Implemented incremental update for `SFC AST` and `Template AST`
- Template codegen simplified to reduce the memory footprint for each Vue file
- When a Monorepo has multiple TS projects, TS SourceFile instance is reused to reduce memory usage.
- Implemented auto import cache of tsserver in LSP server to speedup autocomplete
- Implemented named pipe base cancellation token of tsserver in LSP server
- Optimized Bundle to reduce the package size, and extension startup faster

In addition, some performance issues with larger projects may be due to tsconfig including too many unneeded files, we also have a new [VSCode plugin](https://marketplace.visualstudio.com/items?itemName=johnsoncodehk.vscode-tsconfig-helper) to check what files your tsconfig contains.

### Generic Language Server Framework

Volar core modules is now framework-agnostic, and you can use Volar to implement language servers other than Vue framework.

In this [example repo](https://github.com/johnsoncodehk/volar/tree/master/examples), we have implemented `svelte-tsc`, `svelte-langauge-server` and other examples based on svelte2tsx.

There is also a `vue-and-svelte-language-server` example, which supports both Vue and Svelte in a single Language Server, avoiding the establishment of multiple Language Servers to establish expensive TypeScript LanguageService instances. It may helpful for framework like Astro which supports multiple languages.

### VueLanguagePlugin API

Now supported `vueCompilerOptions.plugins` option to specify additional plugins to alter virtual code generation.

The codegen API of `VueLanguagePlugin` uses `muggle-string` instead of `magic-string`, `muggle-string` is still lacking major features in earlier versions, but it is decoupled from Volar so it can be developed independently, if you need to develop in `VueLanguagePlugin`, please track for https://github.com/johnsoncodehk/muggle-string update.

### LanguageServicePlugin API

You can setup plugin to `volar.config.js` to change the language server behavior, for example you can change the formatter used by `<template>` to Prettier.

We have a single repo for maintaining common plugins: https://github.com/johnsoncodehk/volar-plugins

### External Tools Support

- `@volar/vue-typescript` exposed organizeImports API for `prettier-plugin-organize-imports`
- `@volar/vue-language-core` exposed `vue-tsconfig.schema.json` for IDEs other than VSCode
- Implemented `vue-component-meta` for UI Library documentation generation

## What's Next

Volar has been developed for more than two years so far. The development cost is huge for it just a VSCode plugin at beginning, and its project scope may also frighten some people who planned to make tools for other languages, so I hope to improve the core framework in v2.0 to make it easier for other languages that need to implement tooling to take advantage of the efforts made by Volar.

In addition there are some other things planned to do:

- Documentation website
- Improve the bug report process
- Support global installation of LangaugeServicePlugin
- Bun base Language Server
- Incremental update template codegen
- Explore performance improvements in TypeScript and LSP source code

After v1.0 is released I will go back to working full time and starting some other personal projects, while continuing to work on these in my spare time.

Thank you for reading this blog. 🙌

---

很高興宣布在今天完成了所有主要功能並發佈了v1.0版本。🎉

這個主要版本全方面改進了工具，除了改進 UX、性能、包大小，我們還發佈了 Plugin API v1，以及將項目重構為與框架無關的工具。

今年初我與 Vue 的作者 Evan 達成協議，他在 3 月開始資助我全職開發 Volar 直至完成 1.0 版本，經過7個月的努力我們終於做到這點！

如果你沒有追蹤每個版本的 changelog，你可能不知道發生了什麼，我會簡單總結在這半年間可能對你有影響，相對較主要的更改：

### 功能更新

- 為 Vite 和 Nuxt 3 Preview 實現了 Goto Code 和 Highlight Selection Dom Elements（非常推薦你試一試！）
- 實現了 [Component Preview](https://github.com/johnsoncodehk/volar/discussions/1511)
- 新增 `format.initialIndent` 設置以指定 SFC blocks 的初始縮進
- 實現了 Web IDE 支持
- 不再內置支持 `<template lang="pug">`（在v1.0需要安裝[@volar/vue-language-plugin-pug](https://www.npmjs.com/package/@volar/vue-language-plugin-pug)）

### 開箱即用改進

- 預設不再需要 `"jsx": "preserve"`，並且不會與 `@types/react` 衝突（除非明確啟用 `vueCompilerOptions.jsxTemplates`）
- 預設總是以 `defineComponent()` 包裝Component Options
- 不再以警告色顯示未知 Component Tag
- 改進 JS 組件中的 Template TypeScript 支持
- 減少對閱讀 README 的依賴，現在會自動檢測常見的項目設置問題（例如錯誤地設置 `vueCompilerOptions.target`），發現問題時會在狀態欄提示

### UX / DX 改進

- 啟用 Takeover mode 不再彈出提示
- 更快的 "Reload Project" 命令代替 "Restart Vue server"
- 新的 "Show Virtual Scripts" 命令代替 "Write Virtual Files"
- 增量更新過去的診斷結果防止煩人的閃爍問題
- 支持在不重啟服務器的情況下添加或切換工作區
- 簡化 Name casing tool 在狀態欄的顯示方式，並且現在也支持 Prop name casing 轉換

### TypeScript 改進

- 支持了 `vue-tsc --watch`
- 支持了 Inlay Hints
- 支持 Find File References
- 支持了 [JavaScript and TypeScript Nightly](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-typescript-next)
- 解決了無法在 Template 引用類型的問題
- 新增 `vueCompilerOptions.strictTemplates` 選項支持更嚴格的Template type checking，在使用未知的Component Tag和Props時報告錯誤

### 性能改進

- 大量改善 SourceMap mapping 性能
- 實現 SFC AST 和 Template AST 的增量更新
- 簡化 Template 生成的代碼以降低了每個 Vue 文件產生的內存佔用
- 為 Monorepo 多個 TS project 共用 TS SourceFile 實例
- 移植了 tsserver 自動導入的緩存邏輯加快自動完成
- 移植了 tsserver 基於 Named Pipe 的 Cancellation Token 實現以解決了 LSP 請求阻塞
- 優化 Bundle 降低包大小，並且插件啟動速度更快

另外一些大型項目的性能問題可能是由於 tsconfig 包含了太多不需要的文件，我們還有一個新的[VSCode插件](https://marketplace.visualstudio.com/items?itemName=johnsoncodehk.vscode-tsconfig-helper)用來檢查你的tsconfig包含的文件。

### 通用的 Language Server 框架

Volar 的核心代碼現在與框架無關，你可以使用 Volar 為 Vue 以外的語言實現語言服務器。我們有一個[Svelte Language Server 例子](https://github.com/johnsoncodehk/volar/tree/master/examples/svelte)。

### VueLanguagePlugin API

現在支持 `vueCompilerOptions.plugins` 選項指定額外 plugin 來更改 virtual code 的生成方式。

VueLanguagePlugin的 codegen API 使用 `muggle-string` 而不是 `magic-string`，`muggle-string` 仍然是早期版本缺乏主要功能，但是與 Volar 解耦因此可以獨立發展，如果你需要使用請關注 https://github.com/johnsoncodehk/muggle-string 更新。

### LanguageServicePlugin API

你可以在 `volar.config.js` 添加 plugin 來更改 language server 的行為，例如將 `<template>` 使用的 formatter 改為 Prettier。

我們有一個 repo 用來維護常用的 plugins: https://github.com/johnsoncodehk/volar-plugins

### 外部工具支持

- `@volar/vue-typescript` 公開了 organizeImports API，因此 `prettier-plugin-organize-imports` 可以使用它
- 為 VSCode 以外的 Vue Language Client 從 `@volar/vue-language-core` 公開了 `vue-tsconfig.schema.json` 檔案
- 實現了 `vue-component-meta` 用於 UI Librarys docgen

## What's Next

Volar 至今開發了兩年多時間，我有點自豪因為我幾乎做到了所有承諾。對於原本只是一個 VSCode Plugin 來說投入的開發成本是巨大的，同時它的項目 scope 也可能嚇怕一些原本打算為語言實現 Tooling 的人，因此我希望在 v2.0 改進核心框架，讓其他需要實現 Tooling 的語言更容易地利用 Volar 所做的努力。

此外還有一些計劃做的事情：

- 文檔網站
- 改進 Bug report 流程
- 支持全局安裝 LangaugeServicePlugin
- 基於 Bun 的 Language Server
- 增量更新 template codegen
- 探索 TypeScript 和 LSP 源代碼中的性能改進

發佈v1.0之後我將會回到全職工作和開始一些其他個人項目，同時在業餘時間繼續做這些工作。

感謝你閱讀這篇文章！ 🙌
