# Volar

> ⚡ Fast Vue Language Support Extension

https://marketplace.visualstudio.com/items?itemName=johnsoncodehk.volar

Volar is a Language Support plugin built specifically for Vue 3. It's based on [`@vue/reactivity`](https://www.npmjs.com/package/@vue/reactivity) to calculate TypeScript on-demand to optimize performance similar to the native TypeScript language service.

🛠️ This project is still in refactoring to make contributing easier.

[[Roadmap](https://github.com/johnsoncodehk/volar/issues/28)] [[Tips](https://github.com/johnsoncodehk/volar/issues/53)] [[Discord](https://discord.gg/5bnSSSSBbK)]

Volar does not include ESLint and Prettier, but the official [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) and [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) extensions support Vue, so you could install these yourself if needed.

If using Vetur's [Customizable Scaffold Snippets](https://vuejs.github.io/vetur/guide/snippet.html#customizable-scaffold-snippets), recommend use [vscode-snippet-generator](https://github.com/wenfangdu/vscode-snippet-generator) convert to VSCode Snippets.

## By-product

- [vue-tsc](https://github.com/johnsoncodehk/vue-tsc): Type-Checking on command line
- [typescript-vue-plugin](https://www.npmjs.com/package/typescript-vue-plugin): See [#169](https://github.com/johnsoncodehk/volar/issues/169#issuecomment-832377254).
- [vscode-vue-languageservice](https://www.npmjs.com/package/vscode-vue-languageservice) : Generic language server module for Vue
- [vscode-pug-languageservice](https://www.npmjs.com/package/vscode-pug-languageservice): Generic language server module for Pug
- [vscode-typescript-languageservice](https://www.npmjs.com/package/vscode-typescript-languageservice): Generic language server module for TypeScript

## Sponsors

If you like this extension, please consider becoming a [Sponsor](https://github.com/sponsors/johnsoncodehk). Thank you.

## Quick Start

- [vite/create-app](https://github.com/vitejs/vite/tree/main/packages/create-app/template-vue-ts)
- [Vitesse](https://github.com/antfu/vitesse)

## Using

<!-- Global components support -->
<details>
<summary>Global components support (Updated at 5/4/2021)</summary>

See: https://github.com/vuejs/vue-next/pull/3399

By default, Local components, Built-in components, native HTML elements Type-Checking are active.

For Global components, you need to have Vue 3 `GlobalComponents` interface definition, for example:

```typescript
// components.d.ts
declare module 'vue' {
  export interface GlobalComponents {
    RouterLink: typeof import('vue-router')['RouterLink']
    RouterView: typeof import('vue-router')['RouterView']
  }
}

export {}
```

</details>

<!-- v-slot support -->
<details>
<summary>v-slot support</summary>

v-slot Type-Checking will auto service all .vue files under the project, but for third party libraries, you need to define the slot types, for example:

```typescript
// components.d.ts
import { RouterLink, RouterView, useLink, RouteLocationNormalized } from 'vue-router'
import { UnwrapRef, VNode } from 'vue'

declare module 'vue' {
  export interface GlobalComponents {
    RouterLink: typeof RouterLink & {
      __VLS_slots: {
        default: UnwrapRef<ReturnType<typeof useLink>>
      }
    }
    RouterView: typeof RouterView & {
      __VLS_slots: {
        default: {
          Component: VNode
          route: RouteLocationNormalized & { href: string }
        }
      }
    }
  }
}

export {}
```

</details>

<!-- Work with Vue 2? -->
<details>
<summary>Work with Vue 2?</summary>

This tool required Vue 3 types from the `@vue/runtime-dom` module.

Vue 3 in itself includes the package `@vue/runtime-dom`. For Vue 2 you will have to install this package yourself:

```json
{
  "devDependencies": {
    "@vue/runtime-dom": "latest"
  }
}
```

</details>

## Note

> You need to disable Vetur to avoid conflicts.

> Recommended use css / less / scss as `<style>` language, because these base on [vscode-css-languageservice](https://github.com/microsoft/vscode-css-languageservice) to provide reliable language support.
>
> If use postcss / stylus / sass, you need to install additional extension for syntax highlighting. I tried these and it works, you can also choose others.
>
> - postcss: [language-postcss](https://marketplace.visualstudio.com/items?itemName=cpylua.language-postcss).
> - stylus: [language-stylus](https://marketplace.visualstudio.com/items?itemName=sysoev.language-stylus)
> - sass: [Sass](https://marketplace.visualstudio.com/items?itemName=Syler.sass-indented)

> tsconfig.json / jsconfig.json is required.
>
> Also required `"strict": true` and `"moduleResolution": "node"`.

> `__VLS_slots` will change in future, see: [#40](https://github.com/johnsoncodehk/volar/discussions/40)
