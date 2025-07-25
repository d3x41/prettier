# 3.6.2

[diff](https://github.com/prettier/prettier/compare/3.6.1...3.6.2)

#### Markdown: Add missing blank line around code block ([#17675](https://github.com/prettier/prettier/pull/17675) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
````md
<!-- Input -->
1. Some text, and code block below, with newline after code block

   ```yaml
   ---
   foo: bar
   ```

   1. Another
   2. List

<!-- Prettier 3.6.1 -->
1. Some text, and code block below, with newline after code block

   ```yaml
   ---
   foo: bar
   ```
   1. Another
   2. List

<!-- Prettier 3.6.2 -->
1. Some text, and code block below, with newline after code block

   ```yaml
   ---
   foo: bar
   ```

   1. Another
   2. List
````

# 3.6.1

[diff](https://github.com/prettier/prettier/compare/3.6.0...3.6.1)

#### TypeScript: Allow const without initializer ([#17650](https://github.com/prettier/prettier/pull/17650), [#17654](https://github.com/prettier/prettier/pull/17654) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
export const version: string;

// Prettier 3.6.0 (--parser=babel-ts)
SyntaxError: Unexpected token (1:21)
> 1 | export const version: string;
    |                     ^

// Prettier 3.6.0 (--parser=oxc-ts)
SyntaxError: Missing initializer in const declaration (1:14)
> 1 | export const version: string;
    |              ^^^^^^^^^^^^^^^

// Prettier 3.6.1
export const version: string;
```

#### Miscellaneous: Avoid closing files multiple times ([#17665](https://github.com/prettier/prettier/pull/17665) by [@43081j](https://github.com/43081j))

When reading a file to infer the interpreter from a shebang, we use the
`n-readlines` library to read the first line in order to get the shebang.

This library closes files when it reaches EOF, and we later try close the same
files again. We now close files only if `n-readlines` did not already close
them.

# 3.6.0

[diff](https://github.com/prettier/prettier/compare/3.5.3...3.6.0)

🔗 [Release Notes](https://prettier.io/blog/2025/06/23/3.6.0)

# 3.5.3

[diff](https://github.com/prettier/prettier/compare/3.5.2...3.5.3)

#### Flow: Fix missing parentheses in `ConditionalTypeAnnotation` ([#17196](https://github.com/prettier/prettier/pull/17196) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
type T<U> = 'a' | ('b' extends U ? 'c' : empty);
type T<U> = 'a' & ('b' extends U ? 'c' : empty);

// Prettier 3.5.2
type T<U> = "a" | "b" extends U ? "c" : empty;
type T<U> = "a" & "b" extends U ? "c" : empty;

// Prettier 3.5.3
type T<U> = "a" | ("b" extends U ? "c" : empty);
type T<U> = "a" & ("b" extends U ? "c" : empty);
```

# 3.5.2

[diff](https://github.com/prettier/prettier/compare/3.5.1...3.5.2)

#### Remove `module-sync` condition ([#17156](https://github.com/prettier/prettier/pull/17156) by [@fisker](https://github.com/fisker))

In Prettier 3.5.0, [we added `module-sync` condition to `package.json`](https://prettier.io/blog/2025/02/09/3.5.0#use-esm-entrypoint-for-requireesm-16958-by-tats-u), so that `require("prettier")` can use ESM version, but turns out it doesn't work if CommonJS and ESM plugins both imports builtin plugins. To solve this problem, we decide simply remove the `module-sync` condition, so `require("prettier")` will still use the CommonJS version, we'll revisit until `require(ESM)` feature is more stable.

# 3.5.1

[diff](https://github.com/prettier/prettier/compare/3.5.0...3.5.1)

#### Fix CLI crash when cache for old version exists ([#17100](https://github.com/prettier/prettier/pull/17100) by [@sosukesuzuki](https://github.com/sosukesuzuki))

Prettier 3.5 uses a different cache format than previous versions, Prettier 3.5.0 crashes when reading existing cache file, Prettier 3.5.1 fixed the problem.

#### Support dockercompose and github-actions-workflow in VSCode ([#17101](https://github.com/prettier/prettier/pull/17101) by [@remcohaszing](https://github.com/remcohaszing))

Prettier now supports the `dockercompose` and `github-actions-workflow` languages in Visual Studio Code.

# 3.5.0

[diff](https://github.com/prettier/prettier/compare/3.4.2...3.5.0)

🔗 [Release Notes](https://prettier.io/blog/2025/02/09/3.5.0.html)

# 3.4.2

[diff](https://github.com/prettier/prettier/compare/3.4.1...3.4.2)

#### Treat U+30A0 & U+30FB in Katakana Block as CJK ([#16796](https://github.com/prettier/prettier/pull/16796) by [@tats-u](https://github.com/tats-u))

Prettier doesn't treat U+30A0 & U+30FB as Japanese. U+30FB is commonly used in Japanese to represent the delimitation of first and last names of non-Japanese people or “and”. The following “C言語・C++・Go・Rust” means “C language & C++ & Go & Rust” in Japanese.

<!-- prettier-ignore -->
```md
<!-- Input (--prose-wrap=never) -->

C言
語
・
C++
・
Go
・
Rust

<!-- Prettier 3.4.1 -->
C言語・ C++ ・ Go ・ Rust

<!-- Prettier 3.4.2 -->
C言語・C++・Go・Rust
```

U+30A0 can be used as the replacement of the `-` in non-Japanese names (e.g. “Saint-Saëns” (Charles Camille Saint-Saëns) can be represented as “サン゠サーンス” in Japanese), but substituted by ASCII hyphen (U+002D) or U+FF1D (full width hyphen) in many cases (e.g. “サン=サーンス” or “サン＝サーンス”).

#### Fix comments print on class methods with decorators ([#16891](https://github.com/prettier/prettier/pull/16891) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
class A {
  @decorator
  /** 
   * The method description
   *
  */
  async method(foo: Foo, bar: Bar) {
    console.log(foo);
  }
}

// Prettier 3.4.1
class A {
  @decorator
  async /**
   * The method description
   *
   */
  method(foo: Foo, bar: Bar) {
    console.log(foo);
  }
}

// Prettier 3.4.2
class A {
  @decorator
  /**
   * The method description
   *
   */
  async method(foo: Foo, bar: Bar) {
    console.log(foo);
  }
}
```

#### Fix non-idempotent formatting ([#16899](https://github.com/prettier/prettier/pull/16899) by [@seiyab](https://github.com/seiyab))

This bug fix is not language-specific. You may see similar change in any languages. This fixes regression in 3.4.0 so change caused by it should yield same formatting as 3.3.3.

<!-- prettier-ignore -->
```jsx
// Input
<div>
  foo
  <span>longlonglonglonglonglonglonglonglonglonglonglonglonglonglongl foo</span>
  , abc
</div>;

// Prettier 3.4.1 (first)
<div>
  foo
  <span>
    longlonglonglonglonglonglonglonglonglonglonglonglonglonglongl foo
  </span>, abc
</div>;

// Prettier 3.4.1 (second)
<div>
  foo
  <span>longlonglonglonglonglonglonglonglonglonglonglonglonglonglongl foo</span>
  , abc
</div>;

// Prettier 3.4.2
<div>
  foo
  <span>longlonglonglonglonglonglonglonglonglonglonglonglonglonglongl foo</span>
  , abc
</div>;
```

# 3.4.1

[diff](https://github.com/prettier/prettier/compare/3.4.0...3.4.1)

#### Remove unnecessary parentheses around assignment in `v-on` ([#16887](https://github.com/prettier/prettier/pull/16887) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```vue
<!-- Input -->
<template>
  <button @click="foo += 2">Click</button>
</template>

<!-- Prettier 3.4.0 -->
<template>
  <button @click="(foo += 2)">Click</button>
</template>

<!-- Prettier 3.4.1 -->
<template>
  <button @click="foo += 2">Click</button>
</template>
```

# 3.4.0

[diff](https://github.com/prettier/prettier/compare/3.3.3...3.4.0)

🔗 [Release Notes](https://prettier.io/blog/2024/11/26/3.4.0.html)

# 3.3.3

[diff](https://github.com/prettier/prettier/compare/3.3.2...3.3.3)

#### Add parentheses for nullish coalescing in ternary ([#16391](https://github.com/prettier/prettier/pull/16391) by [@cdignam-segment](https://github.com/cdignam-segment))

This change adds clarity to operator precedence.

<!-- prettier-ignore -->
```js
// Input
foo ? bar ?? foo : baz;
foo ?? bar ? a : b;
a ? b : foo ?? bar;

// Prettier 3.3.2
foo ? bar ?? foo : baz;
foo ?? bar ? a : b;
a ? b : foo ?? bar;

// Prettier 3.3.3
foo ? (bar ?? foo) : baz;
(foo ?? bar) ? a : b;
a ? b : (foo ?? bar);
```

#### Add parentheses for decorator expressions ([#16458](https://github.com/prettier/prettier/pull/16458) by [@y-schneider](https://github.com/y-schneider))

Prevent parentheses around member expressions or tagged template literals from being removed to follow the stricter parsing rules of TypeScript 5.5.

<!-- prettier-ignore -->
```ts
// Input
@(foo`tagged template`)
class X {}

// Prettier 3.3.2
@foo`tagged template`
class X {}

// Prettier 3.3.3
@(foo`tagged template`)
class X {}
```

#### Support `@let` declaration syntax ([#16474](https://github.com/prettier/prettier/pull/16474) by [@sosukesuzuki](https://github.com/sosukesuzuki))

Adds support for Angular v18 `@let` declaration syntax.

Please see the following code example. The `@let` declaration allows you to define local variables within the template:

<!-- prettier-ignore -->
```html
@let name = 'Frodo';

<h1>Dashboard for {{name}}</h1>
Hello, {{name}}
```

For more details, please refer to the excellent blog post by the Angular Team: [Introducing @let in Angular](https://blog.angular.dev/introducing-let-in-angular-686f9f383f0f).

We also appreciate the Angular Team for kindly answering our questions to implement this feature.

# 3.3.2

[diff](https://github.com/prettier/prettier/compare/3.3.1...3.3.2)

#### Fix handlebars path expressions starts with `@` ([#16358](https://github.com/prettier/prettier/pull/16358) by [@Princeyadav05](https://github.com/Princeyadav05))

<!-- prettier-ignore -->
```hbs
{{! Input }}
<div>{{@x.y.z}}</div>

{{! Prettier 3.3.1 }}
<div>{{@x}}</div>

{{! Prettier 3.3.2 }}
<div>{{@x.y.z}}</div>
```

# 3.3.1

[diff](https://github.com/prettier/prettier/compare/3.3.0...3.3.1)

#### Preserve empty lines in front matter ([#16347](https://github.com/prettier/prettier/pull/16347) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```markdown
<!-- Input -->
---
foo:
  - bar1

  - bar2

  - bar3
---
Markdown

<!-- Prettier 3.3.0 -->

---
foo:
  - bar1
  - bar2
  - bar3
---

Markdown


<!-- Prettier 3.3.1 -->
---
foo:
  - bar1

  - bar2

  - bar3
---

Markdown
```

#### Preserve explicit language in front matter ([#16348](https://github.com/prettier/prettier/pull/16348) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```markdown
<!-- Input -->
---yaml
title: Hello
slug: home
---

<!-- Prettier 3.3.0 -->
---
title: Hello
slug: home
---

<!-- Prettier 3.3.1 -->
---yaml
title: Hello
slug: home
---
```

#### Avoid line breaks in import attributes ([#16349](https://github.com/prettier/prettier/pull/16349) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
import something from "./some-very-very-very-very-very-very-very-very-long-path.json" with { type: "json" };

// Prettier 3.3.0
import something from "./some-very-very-very-very-very-very-very-very-long-path.json" with { type:
  "json" };

// Prettier 3.3.1
import something from "./some-very-very-very-very-very-very-very-very-long-path.json" with { type: "json" };
```

# 3.3.0

[diff](https://github.com/prettier/prettier/compare/3.2.5...3.3.0)

🔗 [Release Notes](https://prettier.io/blog/2024/06/01/3.3.0.html)

# 3.2.5

[diff](https://github.com/prettier/prettier/compare/3.2.4...3.2.5)

#### Support Angular inline styles as single template literal ([#15968](https://github.com/prettier/prettier/pull/15968) by [@sosukesuzuki](https://github.com/sosukesuzuki))

[Angular v17](https://blog.angular.io/introducing-angular-v17-4d7033312e4b) supports single string inline styles.

<!-- prettier-ignore -->
```ts
// Input
@Component({
  template: `<div>...</div>`,
  styles: `h1 { color: blue; }`,
})
export class AppComponent {}

// Prettier 3.2.4
@Component({
  template: `<div>...</div>`,
  styles: `h1 { color: blue; }`,
})
export class AppComponent {}

// Prettier 3.2.5
@Component({
  template: `<div>...</div>`,
  styles: `
    h1 {
      color: blue;
    }
  `,
})
export class AppComponent {}

```

#### Unexpected embedded formatting for Angular template ([#15969](https://github.com/prettier/prettier/pull/15969) by [@JounQin](https://github.com/JounQin))

Computed template should not be considered as Angular component template

<!-- prettier-ignore -->
```ts
// Input
const template = "foobar";

@Component({
  [template]: `<h1>{{       hello }}</h1>`,
})
export class AppComponent {}

// Prettier 3.2.4
const template = "foobar";

@Component({
  [template]: `<h1>{{ hello }}</h1>`,
})
export class AppComponent {}

// Prettier 3.2.5
const template = "foobar";

@Component({
  [template]: `<h1>{{       hello }}</h1>`,
})
export class AppComponent {}
```

#### Use `"json"` parser for `tsconfig.json` by default ([#16012](https://github.com/prettier/prettier/pull/16012) by [@sosukesuzuki](https://github.com/sosukesuzuki))

In [v3.2.0](https://prettier.io/blog/2024/01/12/3.2.0#new-jsonc-parser-added-15831httpsgithubcomprettierprettierpull15831-by-fiskerhttpsgithubcomfisker), we introduced `"jsonc"` parser which adds trailing comma **by default**.

When adding a new parser we also define how it will be used based on the [`linguist-languages`](https://www.npmjs.com/package/linguist-languages) data.

`tsconfig.json` is a special file used by [TypeScript](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html#using-tsconfigjson-or-jsconfigjson), it uses `.json` file extension, but it actually uses the [JSON with Comments](https://code.visualstudio.com/docs/languages/json#_json-with-comments) syntax. However, we found that there are many third-party tools not recognize it correctly because of the confusing `.json` file extension.

We decide to treat it as a JSON file for now to avoid the extra configuration step.

To keep using the `"jsonc"` parser for your `tsconfig.json` files, add the following to your `.prettierrc` file

```json
{
  "overrides": [
    {
      "files": ["tsconfig.json", "jsconfig.json"],
      "options": {
        "parser": "jsonc"
      }
    }
  ]
}
```

<!-- prettier-ignore -->
```
# Prettier 3.2.4
prettier --file-info tsconfig.json
{ "ignored": false, "inferredParser": "jsonc" }

# Prettier 3.2.5
prettier --file-info tsconfig.json
{ "ignored": false, "inferredParser": "json" }
```

# 3.2.4

[diff](https://github.com/prettier/prettier/compare/3.2.3...3.2.4)

#### Fix incorrect parser inference ([#15947](https://github.com/prettier/prettier/pull/15947) by [@fisker](https://github.com/fisker))

Files like `.eslintrc.json` were incorrectly formatted as JSONC files.

<!-- prettier-ignore -->
```jsx
// Input
prettier --file-info .eslintrc.json
{ "ignored": false, "inferredParser": "jsonc" }

// Prettier 3.2.4
prettier --file-info .eslintrc.json
{ "ignored": false, "inferredParser": "json" }
```

# 3.2.3

[diff](https://github.com/prettier/prettier/compare/3.2.2...3.2.3)

#### Throw errors for invalid code ([#15881](https://github.com/prettier/prettier/pull/15881) by [@fisker](https://github.com/fisker), [@Josh-Cena](https://github.com/Josh-Cena), [@auvred](https://github.com/auvred))

<!-- prettier-ignore -->
```ts
// Input
1++;

// Prettier 3.2.2
1++;

// Prettier 3.2.3
SyntaxError: Invalid left-hand side expression in unary operation (1:1)
> 1 | 1++;
    | ^
```

<!-- prettier-ignore -->
```ts
// Input
try {} catch (error = 1){}

// Prettier 3.2.2
try {
} catch (error) {}

// Prettier 3.2.3
SyntaxError: Catch clause variable cannot have an initializer. (1:23)
> 1 | try {} catch (error = 1){}
    |                       ^
```

#### Fix parser inference ([#15927](https://github.com/prettier/prettier/pull/15927) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```console
// Prettier 3.2.2
prettier --file-info tsconfig.json
{ "ignored": false, "inferredParser": "json" }

// Prettier 3.2.3
prettier --file-info tsconfig.json
{ "ignored": false, "inferredParser": "jsonc" }
```

# 3.2.2

[diff](https://github.com/prettier/prettier/compare/3.2.1...3.2.2)

#### Fix crash when parsing template literal CSS in a JSX style tag using a spread attribute ([#15896](https://github.com/prettier/prettier/pull/15896) by [@eelco](https://github.com/eelco))

For example this code would crash before:

<!-- prettier-ignore -->
```jsx
<style {...spread}>{`.{}`}</style>
```

#### Fix formatting error on optional call expression and member chain ([#15920](https://github.com/prettier/prettier/pull/15920) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```jsx
// Input
a(() => {}, c?.d());

// Prettier 3.2.1
TypeError: Cannot read properties of undefined (reading 'type')

// Prettier 3.2.2
a(() => {}, c?.d());
```

# 3.2.1

[diff](https://github.com/prettier/prettier/compare/3.2.0...3.2.1)

#### Fix formatting error on member chain ([#15915](https://github.com/prettier/prettier/pull/15915) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```jsx
// Input
test().test2().test2(thing?.something);

// Prettier 3.2.0
TypeError: Cannot read properties of undefined (reading 'type')

// Prettier 3.2.1
test().test2().test2(thing?.something);

```

# 3.2.0

[diff](https://github.com/prettier/prettier/compare/3.1.1...3.2.0)

🔗 [Release Notes](https://prettier.io/blog/2024/01/12/3.2.0.html)

# 3.1.1

[diff](https://github.com/prettier/prettier/compare/3.1.0...3.1.1)

#### Fix config file search ([#15363](https://github.com/prettier/prettier/pull/15363) by [@fisker](https://github.com/fisker))

Previously, we start search for config files from the filePath as a directory, if it happened to be a directory and contains config file, it will be used by mistake.

```text
├─ .prettierrc
└─ test.js         (A directory)
  └─ .prettierrc
```

```js
// Prettier 3.1.0
await prettier.resolveConfigFile(new URL("./test.js", import.meta.url));
// <CWD>/test.js/.prettierrc

// Prettier 3.1.1
await prettier.resolveConfigFile(new URL("./test.js", import.meta.url));
// <CWD>/.prettierrc
```

#### Skip explicitly passed symbolic links with `--no-error-on-unmatched-pattern` ([#15533](https://github.com/prettier/prettier/pull/15533) by [@sanmai-NL](https://github.com/sanmai-NL))

Since Prettier v3, we stopped following symbolic links, however in some use cases, the symbolic link patterns can't be filtered out, and there is no way to prevent Prettier from throwing errors.

In Prettier 3.1.1, you can use `--no-error-on-unmatched-pattern` to simply skip symbolic links.

#### Consistently use tabs in ternaries when `useTabs` is `true` ([#15662](https://github.com/prettier/prettier/pull/15662) by [@auvred](https://github.com/auvred))

<!-- prettier-ignore -->
```jsx
// Input
aaaaaaaaaaaaaaa
	? bbbbbbbbbbbbbbbbbb
	: ccccccccccccccc
	  ? ddddddddddddddd
	  : eeeeeeeeeeeeeee
	    ? fffffffffffffff
	    : gggggggggggggggg;

// Prettier 3.1.0
aaaaaaaaaaaaaaa
	? bbbbbbbbbbbbbbbbbb
	: ccccccccccccccc
	  ? ddddddddddddddd
	  : eeeeeeeeeeeeeee
	    ? fffffffffffffff
	    : gggggggggggggggg;

// Prettier 3.1.1
aaaaaaaaaaaaaaa
	? bbbbbbbbbbbbbbbbbb
	: ccccccccccccccc
		? ddddddddddddddd
		: eeeeeeeeeeeeeee
			? fffffffffffffff
			: gggggggggggggggg;
```

#### Improve config file search ([#15663](https://github.com/prettier/prettier/pull/15663) by [@fisker](https://github.com/fisker))

The Prettier config file search performance has been improved by more effective cache strategy.

#### Fix unstable and ugly formatting for comments in destructuring patterns ([#15708](https://github.com/prettier/prettier/pull/15708) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```tsx
// Input
const {
  foo,
  // bar
  // baz
}: Foo = expr;

// Prettier 3.1.0
const {
  foo1,
} // bar
// baz
: Foo = expr;

// Prettier 3.1.0 second output
const {
  foo1, // bar
} // baz
: Foo = expr;

// Prettier 3.1.1
const {
  foo1,
  // bar
  // baz
}: Foo = expr;
```

#### Support "Import Attributes" ([#15718](https://github.com/prettier/prettier/pull/15718) by [@fisker](https://github.com/fisker))

[TypeScript 5.3](https://devblogs.microsoft.com/typescript/announcing-typescript-5-3/#import-attributes) supports the latest updates to the [import attributes](https://github.com/tc39/proposal-import-attributes) proposal.

```tsx
import something from "./something.json" with { type: "json" };
```

#### Fix false claim in docs that cursorOffset is incompatible with rangeStart/rangeEnd ([#15750](https://github.com/prettier/prettier/pull/15750) by [@ExplodingCabbage](https://github.com/ExplodingCabbage))

The cursorOffset option has in fact been compatible with rangeStart/rangeEnd for over 5 years, thanks to work by @ds300. However, Prettier's documentation (including the CLI `--help` text) continued to claim otherwise, falsely. The documentation is now fixed.

#### Keep curly braces and `from` keyword in empty `import` statements ([#15756](https://github.com/prettier/prettier/pull/15756) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```js
// Input
import { } from 'foo';
import { /* comment */ } from 'bar';

// Prettier 3.1.0
import {} from "foo";
import /* comment */ "bar";

// Prettier 3.1.1
import {} from "foo";
import {} from /* comment */ "bar";
```

#### Keep empty import attributes and assertions ([#15757](https://github.com/prettier/prettier/pull/15757) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```js
// Input
import foo from "foo" with {};
import bar from "bar" assert {};

// Prettier 3.1.0
import foo from "foo";
import bar from "bar";

// Prettier 3.1.1
import foo from "foo" with {};
import bar from "bar" assert {};
```

# 3.1.0

[diff](https://github.com/prettier/prettier/compare/3.0.3...3.1.0)

🔗 [Release Notes](https://prettier.io/blog/2023/11/13/3.1.0.html)

# 3.0.3

[diff](https://github.com/prettier/prettier/compare/3.0.2...3.0.3)

#### Add `preferUnplugged: true` to `package.json` ([#15169](https://github.com/prettier/prettier/pull/15169) by [@fisker](https://github.com/fisker) and [@so1ve](https://github.com/so1ve))

Prettier v3 uses dynamic imports, user [will need to unplug Prettier](https://github.com/yarnpkg/berry/pull/5411#issuecomment-1523502224) when Yarn's PnP mode is enabled, add [`preferUnplugged: true`](https://yarnpkg.com/configuration/manifest#preferUnplugged) to `package.json`, so Yarn will install Prettier as unplug by default.

#### Support shared config that forbids `require()` ([#15233](https://github.com/prettier/prettier/pull/15233) by [@fisker](https://github.com/fisker))

If an external shared config package is used, and the package `exports` don't have `require` or `default` export.

In Prettier 3.0.2 Prettier fails when attempt to `require()` the package, and throws an error.

```text
Error [ERR_PACKAGE_PATH_NOT_EXPORTED]: No "exports" main defined in <packageName>/package.json
```

#### Allow argument of `require()` to break ([#15256](https://github.com/prettier/prettier/pull/15256) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
const plugin = require(
  global.STANDALONE
    ? path.join(__dirname, "../standalone.js")
    : path.join(__dirname, "..")
);

// Prettier 3.0.2
const plugin = require(global.STANDALONE
  ? path.join(__dirname, "../standalone.js")
  : path.join(__dirname, ".."));

// Prettier 3.0.3
const plugin = require(
  global.STANDALONE
    ? path.join(__dirname, "../standalone.js")
    : path.join(__dirname, "..")
);
```

#### Do not print trailing commas in arrow function type parameter lists in `ts` code blocks ([#15286](https://github.com/prettier/prettier/pull/15286) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
````md
<!-- Input -->
```ts
const foo = <T>() => {}
```

<!-- Prettier 3.0.2 -->
```ts
const foo = <T,>() => {}
```

<!-- Prettier 3.0.3 -->
```ts
const foo = <T>() => {}
```
````

#### Support TypeScript 5.2 `using` / `await using` declaration ([#15321](https://github.com/prettier/prettier/pull/15321) by [@sosukesuzuki](https://github.com/sosukesuzuki))

Support for the upcoming Explicit Resource Management feature in ECMAScript. [`using` / `await using` declaration](https://devblogs.microsoft.com/typescript/announcing-typescript-5-2/#using-declarations-and-explicit-resource-management)

<!-- prettier-ignore -->
```tsx
{
   using foo = new Foo();
   await using bar = new Bar();
}
```

# 3.0.2

[diff](https://github.com/prettier/prettier/compare/3.0.1...3.0.2)

#### Break after `=` of assignment if RHS is poorly breakable AwaitExpression or YieldExpression ([#15204](https://github.com/prettier/prettier/pull/15204) by [@seiyab](https://github.com/seiyab))

<!-- prettier-ignore -->
```js
// Input
const { section, rubric, authors, tags } = await utils.upsertCommonData(mainData);

// Prettier 3.0.1
const { section, rubric, authors, tags } = await utils.upsertCommonData(
  mainData,
);

// Prettier 3.0.2
const { section, rubric, authors, tags } =
  await utils.upsertCommonData(mainData);
```

#### Do not add trailing comma for grouped scss comments ([#15217](https://github.com/prettier/prettier/pull/15217) by [@auvred](https://github.com/auvred))

<!-- prettier-ignore -->
```scss
/* Input */
$foo: (
	'property': (),
	// comment 1
	// comment 2
)

/* Prettier 3.0.1 */
$foo: (
  "property": (),
  // comment 1
  // comment 2,
);

/* Prettier 3.0.2 */
$foo: (
  "property": (),
  // comment 1
  // comment 2
);
```

#### Print `declare` and `export` keywords for nested namespace ([#15249](https://github.com/prettier/prettier/pull/15249) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```tsx
// Input
declare namespace abc1.def {}
export namespace abc2.def {}

// Prettier 3.0.1
namespace abc1.def {}
namespace abc2.def {}

// Prettier 3.0.2
declare namespace abc1.def {}
export namespace abc2.def {}
```

# 3.0.1

[diff](https://github.com/prettier/prettier/compare/3.0.0...3.0.1)

#### Fix cursor positioning for a special case ([#14812](https://github.com/prettier/prettier/pull/14812) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```js
// <|> is the cursor position

/* Input */
// All messages are represented in JSON.
// So, the prettier.py controls a subprocess which spawns "node {this_file}".
import {<|>  } from "fs"

/* Prettier 3.0.0 */
// All messages are represented in JSON.
// So, the prettier.py <|>controls a subprocess which spawns "node {this_file}".
import {} from "fs"

/* Prettier 3.0.1 */
// All messages are represented in JSON.
// So, the prettier.py controls a subprocess which spawns "node {this_file}".
import {<|>} from "fs"
```

#### Fix plugins/estree.d.ts to make it a module ([#15018](https://github.com/prettier/prettier/pull/15018) by [@kingyue737](https://github.com/kingyue737))

Add `export {}` in `plugins/estree.d.ts` to fix the "File is not a module" error

#### Add parenthesis around leading multiline comment in return statement ([#15037](https://github.com/prettier/prettier/pull/15037) by [@auvred](https://github.com/auvred))

<!-- prettier-ignore -->
```jsx
// Input
function fn() {
  return (
    /**
     * @type {...}
     */ expression
  )
}

// Prettier 3.0.0
function fn() {
  return /**
   * @type {...}
   */ expression;
}

// Prettier 3.0.1
function fn() {
  return (
    /**
     * @type {...}
     */ expression
  );
}
```

#### Add support for Vue "Generic Components" ([#15066](https://github.com/prettier/prettier/pull/15066) by [@auvred](https://github.com/auvred))

https://blog.vuejs.org/posts/vue-3-3#generic-components

<!-- prettier-ignore -->
```vue
<!-- Input -->
<script setup lang="ts" generic="T extends Type1 & Type2 & (Type3 | Type4), U extends string | number | boolean"></script>

<!-- Prettier 3.0.0 -->
<script
  setup
  lang="ts"
  generic="T extends Type1 & Type2 & (Type3 | Type4), U extends string | number | boolean"
></script>

<!-- Prettier 3.0.1 -->
<script
  setup
  lang="ts"
  generic="
    T extends Type1 & Type2 & (Type3 | Type4),
    U extends string | number | boolean
  "
></script>
```

#### Fix comments print in `IfStatement` ([#15076](https://github.com/prettier/prettier/pull/15076) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```js
function a(b) {
  if (b) return 1; // comment
  else return 2;
}

/* Prettier 3.0.0 */
Error: Comment "comment" was not printed. Please report this error!

/* Prettier 3.0.1 */
function a(b) {
  if (b) return 1; // comment
  else return 2;
}
```

#### Add missing type definition for `printer.preprocess` ([#15123](https://github.com/prettier/prettier/pull/15123) by [@so1ve](https://github.com/so1ve))

```diff
export interface Printer<T = any> {
  // ...
+ preprocess?:
+   | ((ast: T, options: ParserOptions<T>) => T | Promise<T>)
+   | undefined;
}
```

#### Add missing `getVisitorKeys` method type definition for `Printer` ([#15125](https://github.com/prettier/prettier/pull/15125) by [@auvred](https://github.com/auvred))

```tsx
const printer: Printer = {
  print: () => [],
  getVisitorKeys(node, nonTraversableKeys) {
    return ["body"];
  },
};
```

#### Add typing to support `readonly` array properties of AST Node ([#15127](https://github.com/prettier/prettier/pull/15127) by [@auvred](https://github.com/auvred))

<!-- prettier-ignore -->
```tsx
// Input
interface TestNode {
  readonlyArray: readonly string[];
}

declare const path: AstPath<TestNode>;

path.map(() => "", "readonlyArray");

// Prettier 3.0.0
interface TestNode {
  readonlyArray: readonly string[];
}

declare const path: AstPath<TestNode>;

path.map(() => "", "readonlyArray");
//                  ^ Argument of type '"readonlyArray"' is not assignable to parameter of type '"regularArray"'. ts(2345)

// Prettier 3.0.1
interface TestNode {
  readonlyArray: readonly string[];
}

declare const path: AstPath<TestNode>;

path.map(() => "", "readonlyArray");
```

#### Add space before unary minus followed by a function call ([#15129](https://github.com/prettier/prettier/pull/15129) by [@pamelalozano](https://github.com/pamelalozano))

<!-- prettier-ignore -->
```less
// Input
div {
  margin: - func();
}

// Prettier 3.0.0
div {
  margin: -func();
}

// Prettier 3.0.1
div {
  margin: - func();
}
```

# 3.0.0

[diff](https://github.com/prettier/prettier/compare/3.0.0-alpha.6...3.0.0)

🔗 [Release Notes](https://prettier.io/blog/2023/07/05/3.0.0.html)

# 2.8.8

This version is a republished version of v2.8.7.
A bad version was accidentally published and [it can't be unpublished](https://github.com/npm/cli/issues/1686), apologies for the churn.

# 2.8.7

[diff](https://github.com/prettier/prettier/compare/2.8.6...2.8.7)

#### Allow multiple decorators on same getter/setter ([#14584](https://github.com/prettier/prettier/pull/14584) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```ts
// Input
class A {
  @decorator()
  get foo () {}
  
  @decorator()
  set foo (value) {}
}

// Prettier 2.8.6
SyntaxError: Decorators cannot be applied to multiple get/set accessors of the same name. (5:3)
  3 |   get foo () {}
  4 |   
> 5 |   @decorator()
    |   ^^^^^^^^^^^^
  6 |   set foo (value) {}
  7 | }

// Prettier 2.8.7
class A {
  @decorator()
  get foo() {}

  @decorator()
  set foo(value) {}
}
```

# 2.8.6

[diff](https://github.com/prettier/prettier/compare/2.8.5...2.8.6)

#### Allow decorators on private members and class expressions ([#14548](https://github.com/prettier/prettier/pull/14548) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```ts
// Input
class A {
  @decorator()
  #privateMethod () {}
}

// Prettier 2.8.5
SyntaxError: Decorators are not valid here. (2:3)
  1 | class A {
> 2 |   @decorator()
    |   ^^^^^^^^^^^^
  3 |   #privateMethod () {}
  4 | }

// Prettier 2.8.6
class A {
  @decorator()
  #privateMethod() {}
}
```

# 2.8.5

[diff](https://github.com/prettier/prettier/compare/2.8.4...2.8.5)

#### Support TypeScript 5.0 ([#14391](https://github.com/prettier/prettier/pull/14391) by [@fisker](https://github.com/fisker), [#13819](https://github.com/prettier/prettier/pull/13819) by [@fisker](https://github.com/fisker), [@sosukesuzuki](https://github.com/sosukesuzuki))

TypeScript 5.0 introduces two new syntactic features:

- `const` modifiers for type parameters
- `export type *` declarations

#### Add missing parentheses for decorator ([#14393](https://github.com/prettier/prettier/pull/14393) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
class Person {
  @(myDecoratorArray[0])
  greet() {}
}

// Prettier 2.8.4
class Person {
  @myDecoratorArray[0]
  greet() {}
}

// Prettier 2.8.5
class Person {
  @(myDecoratorArray[0])
  greet() {}
}
```

#### Add parentheses for `TypeofTypeAnnotation` to improve readability ([#14458](https://github.com/prettier/prettier/pull/14458) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```tsx
// Input
type A = (typeof node.children)[];

// Prettier 2.8.4
type A = typeof node.children[];

// Prettier 2.8.5
type A = (typeof node.children)[];
```

#### Support `max_line_length=off` when parsing `.editorconfig` ([#14516](https://github.com/prettier/prettier/pull/14516) by [@josephfrazier](https://github.com/josephfrazier))

If an .editorconfig file is in your project and it sets `max_line_length=off` for the file you're formatting,
it will be interpreted as a `printWidth` of `Infinity` rather than being ignored
(which previously resulted in the default `printWidth` of 80 being applied, if not overridden by Prettier-specific configuration).

<!-- prettier-ignore -->
```html
<!-- Input -->
<div className='HelloWorld' title={`You are visitor number ${ num }`} onMouseOver={onMouseOver}/>

<!-- Prettier 2.8.4 -->
<div
  className="HelloWorld"
  title={`You are visitor number ${num}`}
  onMouseOver={onMouseOver}
/>;

<!-- Prettier 2.8.5 -->
<div className="HelloWorld" title={`You are visitor number ${num}`} onMouseOver={onMouseOver} />;
```

# 2.8.4

[diff](https://github.com/prettier/prettier/compare/2.8.3...2.8.4)

#### Fix leading comments in mapped types with `readonly` ([#13427](https://github.com/prettier/prettier/pull/13427) by [@thorn0](https://github.com/thorn0), [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```tsx
// Input
type Type = {
  // comment
  readonly [key in Foo];
};

// Prettier 2.8.3
type Type = {
  readonly // comment
  [key in Foo];
};

// Prettier 2.8.4
type Type = {
  // comment
  readonly [key in Foo];
};
```

#### Group params in opening block statements ([#14067](https://github.com/prettier/prettier/pull/14067) by [@jamescdavis](https://github.com/jamescdavis))

This is a follow-up to #13930 to establish wrapping consistency between opening block statements and else blocks by
grouping params in opening blocks. This causes params to break to a new line together and not be split across lines
unless the length of params exceeds the print width. This also updates the else block wrapping to behave exactly the
same as opening blocks.

<!-- prettier-ignore -->
```hbs
{{! Input }}
{{#block param param param param param param param param param param as |blockParam|}}
  Hello
{{else block param param param param param param param param param param as |blockParam|}}
  There
{{/block}}

{{! Prettier 2.8.3 }}
{{#block
  param
  param
  param
  param
  param
  param
  param
  param
  param
  param
  as |blockParam|
}}
  Hello
{{else block param
param
param
param
param
param
param
param
param
param}}
  There
{{/block}}

{{! Prettier 2.8.4 }}
{{#block
  param param param param param param param param param param
  as |blockParam|
}}
  Hello
{{else block
  param param param param param param param param param param
  as |blockParam|
}}
  There
{{/block}}
```

#### Ignore files in `.sl/` ([#14206](https://github.com/prettier/prettier/pull/14206) by [@bolinfest](https://github.com/bolinfest))

In [Sapling SCM](https://sapling-scm.com/), `.sl/` is the folder where it stores its state, analogous to `.git/` in Git. It should be ignored in Prettier like the other SCM folders.

#### Recognize `@satisfies` in Closure-style type casts ([#14262](https://github.com/prettier/prettier/pull/14262) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
const a = /** @satisfies {Record<string, string>} */ ({hello: 1337});
const b = /** @type {Record<string, string>} */ ({hello: 1337});

// Prettier 2.8.3
const a = /** @satisfies {Record<string, string>} */ { hello: 1337 };
const b = /** @type {Record<string, string>} */ ({ hello: 1337 });

// Prettier 2.8.4
const a = /** @satisfies {Record<string, string>} */ ({hello: 1337});
const b = /** @type {Record<string, string>} */ ({hello: 1337});
```

#### Fix parens in inferred function return types with `extends` ([#14279](https://github.com/prettier/prettier/pull/14279) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```ts
// Input
type Foo<T> = T extends ((a) => a is infer R extends string) ? R : never;

// Prettier 2.8.3 (First format)
type Foo<T> = T extends (a) => a is infer R extends string ? R : never;

// Prettier 2.8.3 (Second format)
SyntaxError: '?' expected. 

// Prettier 2.8.4
type Foo<T> = T extends ((a) => a is infer R extends string) ? R : never;
```

# 2.8.3

[diff](https://github.com/prettier/prettier/compare/2.8.2...2.8.3)

#### Allow self-closing tags on custom elements ([#14170](https://github.com/prettier/prettier/pull/14170) by [@fisker](https://github.com/fisker))

See [Angular v15.1.0 release note](https://github.com/angular/angular/releases/tag/15.1.0) for details.

<!-- prettier-ignore -->
```html
// Input
<app-test/>

// Prettier 2.8.2
SyntaxError: Only void and foreign elements can be self closed "app-test" (1:1)
> 1 | <app-test/>
    | ^^^^^^^^^
  2 |

// Prettier 2.8.3
<app-test />
```

# 2.8.2

[diff](https://github.com/prettier/prettier/compare/2.8.1...2.8.2)

#### Don't lowercase link references ([#13155](https://github.com/prettier/prettier/pull/13155) by [@DerekNonGeneric](https://github.com/DerekNonGeneric) & [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```markdown
<!-- Input -->
We now don't strictly follow the release notes format suggested by [Keep a Changelog].

[Keep a Changelog]: https://example.com/

<!-- Prettier 2.8.1 -->
We now don't strictly follow the release notes format suggested by [Keep a Changelog].

[keep a changelog]: https://example.com/
<!--
^^^^^^^^^^^^^^^^^^ lowercased
-->

<!-- Prettier 2.8.2 -->
<Same as input>
```

#### Preserve self-closing tags ([#13691](https://github.com/prettier/prettier/pull/13691) by [@dcyriller](https://github.com/dcyriller))

<!-- prettier-ignore -->
```hbs
{{! Input }}
<div />
<div></div>
<custom-component />
<custom-component></custom-component>
<i />
<i></i>
<Component />
<Component></Component>

{{! Prettier 2.8.1 }}
<div></div>
<div></div>
<custom-component></custom-component>
<custom-component></custom-component>
<i></i>
<i></i>
<Component />
<Component />

{{! Prettier 2.8.2 }}
<div />
<div></div>
<custom-component />
<custom-component></custom-component>
<i />
<i></i>
<Component />
<Component />
```

#### Allow custom "else if"-like blocks with block params ([#13930](https://github.com/prettier/prettier/pull/13930) by [@jamescdavis](https://github.com/jamescdavis))

#13507 added support for custom block keywords used with `else`, but failed to allow block params. This updates printer-glimmer to allow block params with custom "else if"-like blocks.

<!-- prettier-ignore -->
```hbs
{{! Input }}
{{#when isAtWork as |work|}}
  Ship that
  {{work}}!
{{else when isReading as |book|}}
  You can finish
  {{book}}
  eventually...
{{else}}
  Go to bed!
{{/when}}

{{! Prettier 2.8.1 }}
{{#when isAtWork as |work|}}
  Ship that
  {{work}}!
{{else when isReading}}
  You can finish
  {{book}}
  eventually...
{{else}}
  Go to bed!
{{/when}}

{{! Prettier 2.8.2 }}
{{#when isAtWork as |work|}}
  Ship that
  {{work}}!
{{else when isReading as |book|}}
  You can finish
  {{book}}
  eventually...
{{else}}
  Go to bed!
{{/when}}
```

#### Preserve empty lines between nested SCSS maps ([#13931](https://github.com/prettier/prettier/pull/13931) by [@jneander](https://github.com/jneander))

<!-- prettier-ignore -->
```scss
/* Input */
$map: (
  'one': (
     'key': 'value',
  ),

  'two': (
     'key': 'value',
  ),
)

/* Prettier 2.8.1 */
$map: (
  'one': (
     'key': 'value',
  ),
  'two': (
     'key': 'value',
  ),
)

/* Prettier 2.8.2 */
$map: (
  'one': (
     'key': 'value',
  ),

  'two': (
     'key': 'value',
  ),
)
```

#### Fix missing parentheses when an expression statement starts with `let[` ([#14000](https://github.com/prettier/prettier/pull/14000), [#14044](https://github.com/prettier/prettier/pull/14044) by [@fisker](https://github.com/fisker), [@thorn0](https://github.com/thorn0))

<!-- prettier-ignore -->
```jsx
// Input
(let[0] = 2);

// Prettier 2.8.1
let[0] = 2;

// Prettier 2.8.1 (second format)
SyntaxError: Unexpected token (1:5)
> 1 | let[0] = 2;
    |     ^
  2 |

// Prettier 2.8.2
(let)[0] = 2;
```

#### Fix semicolon duplicated at the end of LESS file ([#14007](https://github.com/prettier/prettier/pull/14007) by [@mvorisek](https://github.com/mvorisek))

<!-- prettier-ignore -->
```less
// Input
@variable: {
  field: something;
};

// Prettier 2.8.1
@variable: {
  field: something;
}; ;

// Prettier 2.8.2
@variable: {
  field: something;
};
```

#### Fix no space after unary minus when followed by opening parenthesis in LESS ([#14008](https://github.com/prettier/prettier/pull/14008) by [@mvorisek](https://github.com/mvorisek))

<!-- prettier-ignore -->
```less
// Input
.unary_minus_single {
  margin: -(@a);
}

.unary_minus_multi {
  margin: 0 -(@a);
}

.binary_minus {
  margin: 0 - (@a);
}

// Prettier 2.8.1
.unary_minus_single {
  margin: - (@a);
}

.unary_minus_multi {
  margin: 0 - (@a);
}

.binary_minus {
  margin: 0 - (@a);
}

// Prettier 2.8.2
.unary_minus_single {
  margin: -(@a);
}

.unary_minus_multi {
  margin: 0 -(@a);
}

.binary_minus {
  margin: 0 - (@a);
}
```

#### Do not change case of property name if inside a variable declaration in LESS ([#14034](https://github.com/prettier/prettier/pull/14034) by [@mvorisek](https://github.com/mvorisek))

<!-- prettier-ignore -->
```less
// Input
@var: {
  preserveCase: 0;
};

// Prettier 2.8.1
@var: {
  preservecase: 0;
};

// Prettier 2.8.2
@var: {
  preserveCase: 0;
};
```

#### Fix formatting for auto-accessors with comments ([#14038](https://github.com/prettier/prettier/pull/14038) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
class A {
  @dec()
  // comment
  accessor b;
}

// Prettier 2.8.1
class A {
  @dec()
  accessor // comment
  b;
}

// Prettier 2.8.1 (second format)
class A {
  @dec()
  accessor; // comment
  b;
}

// Prettier 2.8.2
class A {
  @dec()
  // comment
  accessor b;
}
```

#### Add parentheses for TSTypeQuery to improve readability ([#14042](https://github.com/prettier/prettier/pull/14042) by [@onishi-kohei](https://github.com/onishi-kohei))

<!-- prettier-ignore -->
```tsx
// Input
a as (typeof node.children)[number]
a as (typeof node.children)[]
a as ((typeof node.children)[number])[]

// Prettier 2.8.1
a as typeof node.children[number];
a as typeof node.children[];
a as typeof node.children[number][];

// Prettier 2.8.2
a as (typeof node.children)[number];
a as (typeof node.children)[];
a as (typeof node.children)[number][];
```

#### Fix displacing of comments in default switch case ([#14047](https://github.com/prettier/prettier/pull/14047) by [@thorn0](https://github.com/thorn0))

It was a regression in Prettier 2.6.0.

<!-- prettier-ignore -->
```jsx
// Input
switch (state) {
  default:
    result = state; // no change
    break;
}

// Prettier 2.8.1
switch (state) {
  default: // no change
    result = state;
    break;
}

// Prettier 2.8.2
switch (state) {
  default:
    result = state; // no change
    break;
}
```

#### Support type annotations on auto accessors via `babel-ts` ([#14049](https://github.com/prettier/prettier/pull/14049) by [@sosukesuzuki](https://github.com/sosukesuzuki))

[The bug that `@babel/parser` cannot parse auto accessors with type annotations](https://github.com/babel/babel/issues/15205) has been fixed. So we now support it via `babel-ts` parser.

<!-- prettier-ignore -->
```tsx
class Foo {
  accessor prop: number;
}
```

#### Fix formatting of empty type parameters ([#14073](https://github.com/prettier/prettier/pull/14073) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
const foo: bar</* comment */> = () => baz;

// Prettier 2.8.1
Error: Comment "comment" was not printed. Please report this error!

// Prettier 2.8.2
const foo: bar</* comment */> = () => baz;
```

#### Add parentheses to head of `ExpressionStatement` instead of the whole statement ([#14077](https://github.com/prettier/prettier/pull/14077) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
({}).toString.call(foo) === "[object Array]"
  ? foo.forEach(iterateArray)
  : iterateObject(foo);

// Prettier 2.8.1
({}.toString.call(foo) === "[object Array]"
  ? foo.forEach(iterateArray)
  : iterateObject(foo));

// Prettier 2.8.2
({}).toString.call(foo.forEach) === "[object Array]"
  ? foo.forEach(iterateArray)
  : iterateObject(foo);
```

#### Fix comments after directive ([#14081](https://github.com/prettier/prettier/pull/14081) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
"use strict" /* comment */;

// Prettier 2.8.1 (with other js parsers except `babel`)
Error: Comment "comment" was not printed. Please report this error!

// Prettier 2.8.2
<Same as input>
```

#### Fix formatting for comments inside JSX attribute ([#14082](https://github.com/prettier/prettier/pull/14082) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
function MyFunctionComponent() {
  <button label=/*old*/"new">button</button>
}

// Prettier 2.8.1
Error: Comment "old" was not printed. Please report this error!

// Prettier 2.8.2
function MyFunctionComponent() {
  <button label=/*old*/ "new">button</button>;
}
```

#### Quote numeric keys for json-stringify parser ([#14083](https://github.com/prettier/prettier/pull/14083) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
{0: 'value'}

// Prettier 2.8.1
{
  0: "value"
}

// Prettier 2.8.2
{
  "0": "value"
}
```

#### Fix removing commas from function arguments in maps ([#14089](https://github.com/prettier/prettier/pull/14089) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```scss
/* Input */
$foo: map-fn(
  (
    "#{prop}": inner-fn($first, $second),
  )
);

/* Prettier 2.8.1 */
$foo: map-fn(("#{prop}": inner-fn($first $second)));

/* Prettier 2.8.2 */
$foo: map-fn(
  (
    "#{prop}": inner-fn($first, $second),
  )
);

```

#### Do not insert space in LESS property access ([#14103](https://github.com/prettier/prettier/pull/14103) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```less
// Input
a {
  color: @colors[@white];
}

// Prettier 2.8.1
a {
  color: @colors[ @white];
}

// Prettier 2.8.2
<Same as input>
```

# 2.8.1

[diff](https://github.com/prettier/prettier/compare/2.8.0...2.8.1)

#### Fix SCSS map in arguments ([#9184](https://github.com/prettier/prettier/pull/9184) by [@agamkrbit](https://github.com/agamkrbit))

<!-- prettier-ignore -->
```scss
// Input
$display-breakpoints: map-deep-merge(
  (
    "print-only": "only print",
    "screen-only": "only screen",
    "xs-only": "only screen and (max-width: #{map-get($grid-breakpoints, "sm")-1})",
  ),
  $display-breakpoints
);

// Prettier 2.8.0
$display-breakpoints: map-deep-merge(
  (
    "print-only": "only print",
    "screen-only": "only screen",
    "xs-only": "only screen and (max-width: #{map-get($grid-breakpoints, " sm
      ")-1})",
  ),
  $display-breakpoints
);

// Prettier 2.8.1
$display-breakpoints: map-deep-merge(
  (
    "print-only": "only print",
    "screen-only": "only screen",
    "xs-only": "only screen and (max-width: #{map-get($grid-breakpoints, "sm")-1})",
  ),
  $display-breakpoints
);
```

#### Support auto accessors syntax ([#13919](https://github.com/prettier/prettier/pull/13919) by [@sosukesuzuki](https://github.com/sosukesuzuki))

Support for [Auto Accessors Syntax](https://devblogs.microsoft.com/typescript/announcing-typescript-4-9/#auto-accessors-in-classes) landed in TypeScript 4.9.

(Doesn't work well with `babel-ts` parser)

<!-- prettier-ignore -->
```tsx
class Foo {
  accessor foo: number = 3;
}
```

# 2.8.0

[diff](https://github.com/prettier/prettier/compare/2.7.1...2.8.0)

🔗 [Release Notes](https://prettier.io/blog/2022/11/23/2.8.0.html)

# 2.7.1

[diff](https://github.com/prettier/prettier/compare/2.7.0...2.7.1)

#### Keep useful empty lines in description ([#13013](https://github.com/prettier/prettier/pull/13013) by [@chimurai](https://github.com/chimurai))

<!-- prettier-ignore -->
```graphql
# Input
"""
First line

Second Line
"""
type Person {
  name: String
}

# Prettier 2.7.0
"""
First line
Second Line
"""
type Person {
  name: String
}


# Prettier 2.7.1
"""
First line

Second Line
"""
type Person {
  name: String
}
```

# 2.7.0

[diff](https://github.com/prettier/prettier/compare/2.6.2...2.7.0)

🔗 [Release Notes](https://prettier.io/blog/2022/06/14/2.7.0.html)

# 2.6.2

[diff](https://github.com/prettier/prettier/compare/2.6.1...2.6.2)

#### Fix LESS/SCSS format error ([#12536](https://github.com/prettier/prettier/pull/12536) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```less
// Input
.background-gradient(@cut) {
    background: linear-gradient(
        to right,
        @white 0%,
        @white (@cut - 0.01%),
        @portal-background @cut,
        @portal-background 100%
    );
}

// Prettier 2.6.1
TypeError: Cannot read properties of undefined (reading 'endOffset')

// Prettier 2.6.2
.background-gradient(@cut) {
  background: linear-gradient(
    to right,
    @white 0%,
    @white (@cut - 0.01%),
    @portal-background @cut,
    @portal-background 100%
  );
}
```

#### Update `meriyah` to fix several bugs ([#12567](https://github.com/prettier/prettier/pull/12567) by [@fisker](https://github.com/fisker), fixes in [`meriyah`](https://github.com/meriyah/meriyah/) by [@3cp](https://github.com/3cp))

Fixes bugs when parsing following valid code:

```js
foo(await bar());
```

```js
const regex = /.*/ms;
```

```js
const element = <p>{/w/.test(s)}</p>;
```

```js
class A extends B {
  #privateMethod() {
    super.method();
  }
}
```

# 2.6.1

[diff](https://github.com/prettier/prettier/compare/2.6.0...2.6.1)

#### Ignore `loglevel` when printing information ([#12477](https://github.com/prettier/prettier/pull/12477) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```bash
# Prettier 2.6.0
prettier --loglevel silent --find-config-path index.js
# Nothing printed

# Prettier 2.6.1
prettier --loglevel silent --help no-color
# .prettierrc
```

#### Make artifact friendly for webpack ([#12485](https://github.com/prettier/prettier/pull/12485), [#12511](https://github.com/prettier/prettier/pull/12511) by [@fisker](https://github.com/fisker))

Fixes two problems when bundling our UMD files with webpack:

- A error ``"`....__exportStar` is not a function"`` throws when running the bundles.
- Some files cause warning about `"Critical dependency: the request of a dependency is an expression"`.

#### Fix non-idempotent formatting of function calls with complex type arguments ([#12508](https://github.com/prettier/prettier/pull/12508) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```tsx
// Input
const foo =
  doSomething<{ foo1: "foo1", foo2: "foo2", foo3: "foo3", foo4: "foo4", foo5: "foo5" }>();

// Prettier 2.6.0 (first)
const foo =
  doSomething<{
    foo1: "foo1";
    foo2: "foo2";
    foo3: "foo3";
    foo4: "foo4";
    foo5: "foo5";
  }>();

// Prettier 2.6.0 (second)
const foo = doSomething<{
  foo1: "foo1";
  foo2: "foo2";
  foo3: "foo3";
  foo4: "foo4";
  foo5: "foo5";
}>();

// Prettier 2.6.1
const foo = doSomething<{
  foo1: "foo1";
  foo2: "foo2";
  foo3: "foo3";
  foo4: "foo4";
  foo5: "foo5";
}>();

```

#### Fix minimist security issue ([#12513](https://github.com/prettier/prettier/pull/12513) by [@dependabot](https://github.com/dependabot))

Details: [Prototype Pollution](https://security.snyk.io/vuln/SNYK-JS-MINIMIST-559764)

# 2.6.0

[diff](https://github.com/prettier/prettier/compare/2.5.1...2.6.0)

🔗 [Release Notes](https://prettier.io/blog/2022/03/16/2.6.0.html)

# 2.5.1

[diff](https://github.com/prettier/prettier/compare/2.5.0...2.5.1)

#### Improve formatting for empty tuple types ([#11884](https://github.com/prettier/prettier/pull/11884) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```tsx
// Input
type Foo =
  Foooooooooooooooooooooooooooooooooooooooooooooooooooooooooo extends []
    ? Foo3
    : Foo4;

// Prettier 2.5.0
type Foo = Foooooooooooooooooooooooooooooooooooooooooooooooooooooooooo extends [

]
  ? Foo3
  : Foo4;

// Prettier 2.5.0 (tailingCommma = all)
// Invalid TypeScript code
type Foo = Foooooooooooooooooooooooooooooooooooooooooooooooooooooooooo extends [
  ,
]
  ? Foo3
  : Foo4;

// Prettier 2.5.1
type Foo =
  Foooooooooooooooooooooooooooooooooooooooooooooooooooooooooo extends []
    ? Foo3
    : Foo4;

```

#### Fix compatibility with Jest inline snapshot test ([#11892](https://github.com/prettier/prettier/pull/11892) by [@fisker](https://github.com/fisker))

A internal change in Prettier@v2.5.0 accidentally breaks the Jest inline snapshot test.

#### Support Glimmer's named blocks ([#11899](https://github.com/prettier/prettier/pull/11899) by [@duailibe](https://github.com/duailibe))

Prettier already supported this feature, but it converted empty named blocks to self-closing, which is not supported by the Glimmer compiler.

See: [Glimmer's named blocks](https://emberjs.github.io/rfcs/0460-yieldable-named-blocks.html).

<!-- prettier-ignore -->
```hbs
// Input
<Component>
  <:named></:named>
</Component>

// Prettier 2.5.0
<Component>
  <:named />
</Component>

// Prettier 2.5.1
<Component>
  <:named></:named>
</Component>
```

# 2.5.0

[diff](https://github.com/prettier/prettier/compare/2.4.1...2.5.0)

🔗 [Release Notes](https://prettier.io/blog/2021/11/25/2.5.0.html)

# 2.4.1

[diff](https://github.com/prettier/prettier/compare/2.4.0...2.4.1)

#### Fix wildcard syntax in `@forward` ([#11482](https://github.com/prettier/prettier/pull/11482)) ([#11487](https://github.com/prettier/prettier/pull/11487) by [@niksy](https://github.com/niksy))

<!-- prettier-ignore -->
```scss
// Input
@forward "library" as btn-*;

// Prettier 2.4.0
@forward "library" as btn- *;

// Prettier 2.4.1
@forward "library" as btn-*;
```

#### Add new CLI option `debug-print-ast` ([#11514](https://github.com/prettier/prettier/pull/11514) by [@sosukesuzuki](https://github.com/sosukesuzuki))

A new `--debug-print-ast` CLI flag for debugging.

# 2.4.0

[diff](https://github.com/prettier/prettier/compare/2.3.2...2.4.0)

🔗 [Release Notes](https://prettier.io/blog/2021/09/09/2.4.0.html)

# 2.3.2

[diff](https://github.com/prettier/prettier/compare/2.3.1...2.3.2)

#### Fix failure on dir with trailing slash ([#11000](https://github.com/prettier/prettier/pull/11000) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```console
$ ls
1.js  1.unknown

# Prettier 2.3.1
$ prettier . -l
1.js
$ prettier ./ -l
[error] No supported files were found in the directory: "./".

# Prettier 2.3.2
$ prettier ./ -l
1.js
```

#### Fix handling of parenthesis with Flow's {Optional}IndexedAccess ([#11051](https://github.com/prettier/prettier/pull/11051) by [@gkz](https://github.com/gkz))

Add parens when required.

<!-- prettier-ignore -->
```jsx
// Input
type A = (T & S)['bar'];
type B = (T | S)['bar'];
type C = (?T)['bar'];
type D = (typeof x)['bar'];
type E = (string => void)['bar'];

// Prettier 2.3.1
type A = T & S["bar"];
type B = T | S["bar"];
type C = ?T["bar"];
type D = typeof x["bar"];
type E = (string) => void["bar"];

// Prettier 2.3.2
type A = (T & S)["bar"];
type B = (T | S)["bar"];
type C = (?T)["bar"];
type D = (typeof x)["bar"];
type E = ((string) => void)["bar"];
```

#### Print override modifiers for parameter property ([#11074](https://github.com/prettier/prettier/pull/11074) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```ts
// Input
class D extends B {
  constructor(override foo: string) {
    super();
  }
}

// Prettier 2.3.1
class D extends B {
  constructor(foo: string) {
    super();
  }
}

// Prettier 2.3.2
class D extends B {
  constructor(override foo: string) {
    super();
  }
}

```

# 2.3.1

[diff](https://github.com/prettier/prettier/compare/2.3.0...2.3.1)

#### Support TypeScript 4.3 ([#10945](https://github.com/prettier/prettier/pull/10945) by [@sosukesuzuki](https://github.com/sosukesuzuki))

##### [`override` modifiers in class elements](https://devblogs.microsoft.com/typescript/announcing-typescript-4-3/#override)

```ts
class Foo extends  {
  override method() {}
}
```

##### [static index signatures (`[key: KeyType]: ValueType`) in classes](https://devblogs.microsoft.com/typescript/announcing-typescript-4-3/#static-index-signatures)

```ts
class Foo {
  static [key: string]: Bar;
}
```

##### [`get` / `set` in type declarations](https://devblogs.microsoft.com/typescript/announcing-typescript-4-3/#separate-write-types)

```ts
interface Foo {
  set foo(value);
  get foo(): string;
}
```

#### Preserve attributes order for element node ([#10958](https://github.com/prettier/prettier/pull/10958) by [@dcyriller](https://github.comdcyriller))

<!-- prettier-ignore -->
```handlebars
{{!-- Input --}}
<MyComponent
  {{! this is a comment for arg 1}}
  @arg1="hello"
  {{on "click" this.modify}}
  @arg2="hello"
  {{! this is a comment for arg 3}}
  @arg3="hello"
  @arg4="hello"
  {{! this is a comment for arg 5}}
  @arg5="hello"
  ...arguments
/>
{{!-- Prettier stable --}}
<MyComponent
  @arg1="hello"
  @arg2="hello"
  @arg3="hello"
  @arg4="hello"
  @arg5="hello"
  ...arguments
  {{on "click" this.modify}}
  {{! this is a comment for arg 1}}
  {{! this is a comment for arg 3}}
  {{! this is a comment for arg 5}}
/>
{{!-- Prettier main --}}
<MyComponent
  {{! this is a comment for arg 1}}
  @arg1="hello"
  {{on "click" this.modify}}
  @arg2="hello"
  {{! this is a comment for arg 3}}
  @arg3="hello"
  @arg4="hello"
  {{! this is a comment for arg 5}}
  @arg5="hello"
  ...arguments
/>
```

#### Track cursor position properly when it’s at the end of the range to format ([#10938](https://github.com/prettier/prettier/pull/10938) by [@j-f1](https://github.com/j-f1))

Previously, if the cursor was at the end of the range to format, it would simply be placed back at the end of the updated range.
Now, it will be repositioned if Prettier decides to add additional code to the end of the range (such as a semicolon).

<!-- prettier-ignore -->
```jsx
// Input (<|> represents the cursor)
const someVariable = myOtherVariable<|>
// range to format:  ^^^^^^^^^^^^^^^

// Prettier stable
const someVariable = myOtherVariable;<|>
// range to format:  ^^^^^^^^^^^^^^^

// Prettier main
const someVariable = myOtherVariable<|>;
// range to format:  ^^^^^^^^^^^^^^^
```

#### Break the LHS of type alias that has complex type parameters ([#10901](https://github.com/prettier/prettier/pull/10901) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```ts
// Input
type FieldLayoutWith<
  T extends string,
  S extends unknown = { width: string }
> = {
  type: T;
  code: string;
  size: S;
};

// Prettier stable
type FieldLayoutWith<T extends string, S extends unknown = { width: string }> =
  {
    type: T;
    code: string;
    size: S;
  };

// Prettier main
type FieldLayoutWith<
  T extends string,
  S extends unknown = { width: string }
> = {
  type: T;
  code: string;
  size: S;
};

```

#### Break the LHS of assignments that has complex type parameters ([#10916](https://github.com/prettier/prettier/pull/10916) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```ts
// Input
const map: Map<
  Function,
  Map<string | void, { value: UnloadedDescriptor }>
> = new Map();

// Prettier stable
const map: Map<Function, Map<string | void, { value: UnloadedDescriptor }>> =
  new Map();

// Prettier main
const map: Map<
  Function,
  Map<string | void, { value: UnloadedDescriptor }>
> = new Map();

```

#### Fix incorrectly wrapped arrow functions with return types ([#10940](https://github.com/prettier/prettier/pull/10940) by [@thorn0](https://github.com/thorn0))

<!-- prettier-ignore -->
```ts
// Input
longfunctionWithCall12("bla", foo, (thing: string): complex<type<something>> => {
  code();
});

// Prettier stable
longfunctionWithCall12("bla", foo, (thing: string): complex<
  type<something>
> => {
  code();
});

// Prettier main
longfunctionWithCall12(
  "bla",
  foo,
  (thing: string): complex<type<something>> => {
    code();
  }
);
```

#### Avoid breaking call expressions after assignments with complex type arguments ([#10949](https://github.com/prettier/prettier/pull/10949) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```ts
// Input
const foo = call<{
  prop1: string;
  prop2: string;
  prop3: string;
}>();

// Prettier stable
const foo =
  call<{
    prop1: string;
    prop2: string;
    prop3: string;
  }>();

// Prettier main
const foo = call<{
  prop1: string;
  prop2: string;
  prop3: string;
}>();

```

#### Fix order of `override` modifiers ([#10961](https://github.com/prettier/prettier/pull/10961) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```ts
// Input
class Foo extends Bar {
  abstract override foo: string;
}

// Prettier stable
class Foo extends Bar {
  override abstract foo: string;
}

// Prettier main
class Foo extends Bar {
  abstract override foo: string;
}
```

# 2.3.0

[diff](https://github.com/prettier/prettier/compare/2.2.1...2.3.0)

🔗 [Release Notes](https://prettier.io/blog/2021/05/09/2.3.0.html)

# 2.2.1

[diff](https://github.com/prettier/prettier/compare/2.2.0...2.2.1)

#### Fix formatting for AssignmentExpression with ClassExpression ([#9741](https://github.com/prettier/prettier/pull/9741) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```js
// Input
module.exports = class A extends B {
  method() {
    console.log("foo");
  }
};

// Prettier 2.2.0
module.exports = class A extends (
  B
) {
  method() {
    console.log("foo");
  }
};

// Prettier 2.2.1
module.exports = class A extends B {
  method() {
    console.log("foo");
  }
};
```

# 2.2.0

[diff](https://github.com/prettier/prettier/compare/2.1.2...2.2.0)

🔗 [Release Notes](https://prettier.io/blog/2020/11/20/2.2.0.html)

# 2.1.2

[diff](https://github.com/prettier/prettier/compare/2.1.1...2.1.2)

#### Fix formatting for directives in fields ([#9116](https://github.com/prettier/prettier/pull/9116) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```graphql
# Input
type Query {
  someQuery(id: ID!, someOtherData: String!): String! @deprecated @isAuthenticated
  versions: Versions!
}


# Prettier stable
type Query {
  someQuery(id: ID!, someOtherData: String!): String!
  @deprecated
  @isAuthenticated
  versions: Versions!
}

# Prettier master
type Query {
  someQuery(id: ID!, someOtherData: String!): String!
    @deprecated
    @isAuthenticated
  versions: Versions!
}

```

#### Fix line breaks for CSS in JS ([#9136](https://github.com/prettier/prettier/pull/9136) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```js
// Input
styled.div`
  // prettier-ignore
  @media (aaaaaaaaaaaaa) {
	z-index: ${(props) => (props.isComplete ? '1' : '0')};
  }
`;
styled.div`
  ${props => getSize(props.$size.xs)}
  ${props => getSize(props.$size.sm, 'sm')}
  ${props => getSize(props.$size.md, 'md')}
`;

// Prettier stable
styled.div`
  // prettier-ignore
  @media (aaaaaaaaaaaaa) {
	z-index: ${(props) =>
    props.isComplete ? "1" : "0"};
  }
`;
styled.div`
  ${(props) => getSize(props.$size.xs)}
  ${(props) => getSize(props.$size.sm, "sm")}
  ${(props) =>
    getSize(props.$size.md, "md")}
`;

// Prettier master
styled.div`
  // prettier-ignore
  @media (aaaaaaaaaaaaa) {
        z-index: ${(props) => (props.isComplete ? "1" : "0")};
  }
`;
styled.div`
  ${(props) => getSize(props.$size.xs)}
  ${(props) => getSize(props.$size.sm, "sm")}
  ${(props) => getSize(props.$size.md, "md")}
`;

```

#### Fix comment printing in mapping and sequence ([#9143](https://github.com/prettier/prettier/pull/9143), [#9169](https://github.com/prettier/prettier/pull/9169) by [@sosukesuzuki](https://github.com/sosukesuzuki), [@fisker](https://github.com/fisker), fix in `yaml-unist-parser` by [@ikatyang](https://github.com/ikatyang))

<!-- prettier-ignore -->
```yaml
# Input
- a
  # Should indent
- bb

---
- a: a
  b: b

  # Should print one empty line before
- another

# Prettier stable
- a
# Should indent
- bb

---
- a: a
  b: b


  # Should print one empty line before
- another

# Prettier master
- a
  # Should indent
- bb

---
- a: a
  b: b

  # Should print one empty line before
- another
```

# 2.1.1

[diff](https://github.com/prettier/prettier/compare/2.1.0...2.1.1)

#### Fix format on html with frontMatter ([#9043](https://github.com/prettier/prettier/pull/9043) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```html
<!-- Input -->
---
layout: foo
---

Test <a
href="https://prettier.io">abc</a>.

<!-- Prettier stable -->
TypeError: Cannot read property 'end' of undefined
  ...

<!-- Prettier master -->
---
layout: foo
---

Test <a href="https://prettier.io">abc</a>.
```

#### Fix broken format for `...infer T` ([#9044](https://github.com/prettier/prettier/pull/9044) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```ts
// Input
type Tail<T extends any[]> = T extends [infer U, ...infer R] ? R : never;

// Prettier stable
type Tail<T extends any[]> = T extends [infer U, ...(infer R)] ? R : never;

// Prettier master
type Tail<T extends any[]> = T extends [infer U, ...infer R] ? R : never;
```

#### Fix format on `style[lang="sass"]` ([#9051](https://github.com/prettier/prettier/pull/9051) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
<!-- Input -->
<style lang="sass">
.hero
  @include background-centered
</style>

<!-- Prettier stable -->
<style lang="sass">
.hero @include background-centered;
</style>

<!-- Prettier master -->
<style lang="sass">
  .hero
    @include background-centered
</style>
```

#### Fix self-closing blocks and blocks with `src` attribute format ([#9052](https://github.com/prettier/prettier/pull/9052), [#9055](https://github.com/prettier/prettier/pull/9055) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```vue
<!-- Input -->
<custom lang="markdown" src="./foo.md"></custom>
<custom lang="markdown" src="./foo.md" />
<custom lang="markdown" />

<!-- Prettier stable -->
<custom lang="markdown" src="./foo.md">

</custom>
<custom lang="markdown" src="./foo.md"

/>
<custom lang="markdown"

/>

<!-- Prettier master -->
<custom lang="markdown" src="./foo.md"></custom>
<custom lang="markdown" src="./foo.md" />
<custom lang="markdown" />
```

# 2.1.0

[diff](https://github.com/prettier/prettier/compare/2.0.5...2.1.0)

🔗 [Release Notes](https://prettier.io/blog/2020/08/24/2.1.0.html)

# 2.0.5

[diff](https://github.com/prettier/prettier/compare/2.0.4...2.0.5)

#### Less: Fix formatting of `:extend` ([#7984](https://github.com/prettier/prettier/pull/7984) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```less
// Input
.class {
  &:extend(.some-class .some-other-class .some-very-loooooooooooooong-class all);
}

// Prettier 2.0.4
.class {
  &:extend(
    .some-class .some-other-class .some-very-loooooooooooooong-class all
  );
}

// Prettier 2.0.4 (Second format)
.class {
  &: extend(
    .some-class .some-other-class .some-very-loooooooooooooong-class all
  );
}

// Prettier 2.0.5
.class {
  &:extend(
    .some-class .some-other-class .some-very-loooooooooooooong-class all
  );
}
```

#### Editor integration: Use [`resolve`](https://www.npmjs.com/package/resolve) if builtin `require.resolve` is overridden ([#8072](https://github.com/prettier/prettier/pull/8072) by [@fisker](https://github.com/fisker))

This fixes issues that the users of Atom and WebStorm faced with 2.0.4.

Prettier now switches to using the `resolve` module for resolving configuration files and plugins if it detects that `require.resolve` isn't Node's builtin function (doesn't support the second argument), which happens in environments like editor extensions. To force the fallback, set the `PRETTIER_FALLBACK_RESOLVE` environment variable to `true`.

# 2.0.4

[diff](https://github.com/prettier/prettier/compare/2.0.3...2.0.4)

#### Revert [#7869](https://github.com/prettier/prettier/pull/7869), "[TypeScript] format TSAsExpression with same logic as BinaryExpression" ([#7958](https://github.com/prettier/prettier/pull/7958))

# 2.0.3

[diff](https://github.com/prettier/prettier/compare/2.0.2...2.0.3)

### JavaScript

#### Fix `prettier-ignore` inside JSX ([#7877](https://github.com/prettier/prettier/pull/7877) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```jsx
// Input
<div>
{
  /* prettier-ignore */
  x     ?   <Y/> : <Z/>
}
</div>;

// Prettier 2.0.2 (first output)
<div>
  {/* prettier-ignore */
  x     ?   <Y/> : <Z/>}
</div>;

// Prettier 2.0.2 (second output)
<div>{/* prettier-ignore */ x     ?   <Y/> : <Z/>}</div>;

// Prettier 2.0.3
<div>
  {
    /* prettier-ignore */
    x     ?   <Y/> : <Z/>
  }
</div>;
```

#### Fix regressions in styled-components template literals ([#7883](https://github.com/prettier/prettier/pull/7883) by [@thorn0](https://github.com/thorn0))

<!-- prettier-ignore -->
```js
// Input
const Icon = styled.div`
  background:   var(--${background});
  ${Link}:not(:first-child) {
      fill:    rebeccapurple;
  }
`;

// Prettier 2.0.2
const Icon = styled.div`
  background: var(-- ${background});
  ${Link}:not (:first-child) {
    fill: rebeccapurple;
  }
`;

// Prettier 2.0.3
const Icon = styled.div`
  background: var(--${background});
  ${Link}:not(:first-child) {
    fill: rebeccapurple;
  }
`;
```

#### Fix: line endings were not always converted properly in multiline strings and comments ([#7891](https://github.com/prettier/prettier/pull/7891) by [@sidharthv96](https://github.com/sidharthv96))

<!-- prettier-ignore -->
```
// Input
export const IAmIncredibleLongFunctionName = IAmAnotherFunctionName(<CRLF>
  (_0: IAmIncredibleLongParameterType) => {<CRLF>
    setTimeout(() => {<CRLF>
      /*<CRLF>
        Multiline comment<CRLF>
        Multiline comment<CRLF>
        Multiline comment<CRLF>
      */<CRLF>
      console.log(<CRLF>
        "Multiline string\<CRLF>
         Multiline string\<CRLF>
         Multiline string"<CRLF>
      );<CRLF>
    });<CRLF>
  }<CRLF>
);<CRLF>

// Prettier 2.0.2
export const IAmIncredibleLongFunctionName = IAmAnotherFunctionName(<CRLF>
  (_0: IAmIncredibleLongParameterType) => {<CRLF>
    setTimeout(() => {<CRLF>
      /*<LF>
        Multiline comment<LF>
        Multiline comment<LF>
        Multiline comment<LF>
      */<CRLF>
      console.log(<CRLF>
        "Multiline string\<LF>
         Multiline string\<LF>
         Multiline string"<CRLF>
      );<CRLF>
    });<CRLF>
  }<CRLF>
);<CRLF>

// Prettier 2.0.3: same as input
```

#### Fix bug with holes in array literals ([#7911](https://github.com/prettier/prettier/pull/7911) by [@bakkot](https://github.com/bakkot))

<!-- prettier-ignore -->
```jsx
// Input
new Test()
  .test()
  .test([, 0])
  .test();

// Prettier 2.0.2
[error] in.js: TypeError: Cannot read property 'type' of null

// Prettier 2.0.3
new Test().test().test([, 0]).test();
```

### TypeScript

#### Wrap TSAsExpression ([#7869](https://github.com/prettier/prettier/pull/7869) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```ts
// Input
const value = thisIsAnIdentifier as ThisIsAReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyLongInterface;

// Prettier 2.0.2
const value = thisIsAnIdentifier as ThisIsAReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyLongInterface;

// Prettier 2.0.3
const value =
  thisIsAnIdentifier as
  ThisIsAReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyLongInterface;
```

### Flow

#### Print dangling comments for inexact object type ([#7892](https://github.com/prettier/prettier/pull/7892) by [@sosukesuzuki](https://github.com/sosukesuzuki))

<!-- prettier-ignore -->
```js
// Input
type Foo = {
  // comment
  ...,
};

// Prettier 2.0.2
Error: Comment "comment" was not printed. Please report this error!

// Prettier 2.0.3
type Foo = {
  // comment
  ...,
};
```

#### Do not add comma for explicit inexact object with indexer property or no properties ([#7923](https://github.com/prettier/prettier/pull/7923) by [@DmitryGonchar](https://github.com/DmitryGonchar))

<!-- prettier-ignore -->
```jsx
// Input
type T = {
  [string]: number,
  ...,
}

type T = {
  // comment
  ...,
}

// Prettier 2.0.2
type T = {
  [string]: number,
  ...,
}

type T = {
  // comment
  ...,
}

// Prettier 2.0.3
type T = {
  [string]: number,
  ...
}

type T = {
  // comment
  ...
}
```

### HTML

#### Fix printing of ignored empty inline elements ([#7867](https://github.com/prettier/prettier/pull/7867) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```html
<!-- Input-->
<!--prettier-ignore--><span></span>
<!--prettier-ignore--><span>_</span>

<!-- Prettier 2.0.2 (first output) -->
<!--prettier-ignore--><span
></span>
<!--prettier-ignore--><span>_</span>

<!-- Prettier 2.0.2 (second output) -->
<!--prettier-ignore--><span

></span>
<!--prettier-ignore--><span>_</span>

<!-- Prettier 2.0.3 -->
<!--prettier-ignore--><span></span>
<!--prettier-ignore--><span>_</span>
```

#### Format `script` and `style` inside tags with a colon in the name ([#7916](https://github.com/prettier/prettier/pull/7916) by [@fisker](https://github.com/fisker))

<!-- prettier-ignore -->
```html
<!-- Input -->
<with:colon>
<script>function foo(){      return 1}</script>
<style>a         {color:         #f00}</style>
</with:colon>

<!-- Prettier 2.0.2 -->
<with:colon>
  <script>
    function foo(){ return 1}
  </script>
  <style>
    a {color: #f00}
  </style>
</with:colon>

<!-- Prettier 2.0.3 -->
<with:colon>
  <script>
    function foo() {
      return 1;
    }
  </script>
  <style>
    a {
      color: #f00;
    }
  </style>
</with:colon>
```

### Other changes

- Workaround for `require.resolve` in prettier-vscode ([#7951](https://github.com/prettier/prettier/pull/7951) by [@thorn0](https://github.com/thorn0))
- Fix unstable Angular expression binding ([#7924](https://github.com/prettier/prettier/pull/7924) by [@fisker](https://github.com/fisker))
- Update `isSCSS` regex ([#7922](https://github.com/prettier/prettier/pull/7922) by [@fisker](https://github.com/fisker))
- Fix formatting of empty files ([#7921](https://github.com/prettier/prettier/pull/7921) by [@fisker](https://github.com/fisker))

# 2.0.2

[diff](https://github.com/prettier/prettier/compare/2.0.1...2.0.2)

### 2.0 regressions

#### JavaScript: Fix formatting of pseudo-elements and pseudo-classes in styled-components template literals ([#7842](https://github.com/prettier/prettier/pull/7842) by [@thorn0](https://github.com/thorn0))

<!-- prettier-ignore -->
```jsx
// Input
const Foo = styled.div`
  ${media.smallDown}::before {}
`;

// Prettier 2.0.0
const Foo = styled.div`
  ${media.smallDown}: : before{
  }
`;

// Prettier 2.0.2
const Foo = styled.div`
  ${media.smallDown}::before {
  }
`;
```

#### TypeScript: Avoid trailing commas on index signatures with only one parameter ([#7836](https://github.com/prettier/prettier/pull/7836) by [@bakkot](https://github.com/bakkot))

TypeScript index signatures technically allow multiple parameters and trailing commas, but it's an error to have multiple parameters there, and Babel's TypeScript parser does not accept them. So Prettier now avoids putting a trailing comma there when you have only one parameter.

<!-- prettier-ignore -->
```ts
// Input
export type A = {
  a?: {
    [
      x: string
    ]: typeof SomeLongLongLongTypeName[keyof typeof SomeLongLongLongTypeName];
  } | null;
};

// Prettier 2.0.0
export type A = {
  a?: {
    [
      x: string,
    ]: typeof SomeLongLongLongTypeName[keyof typeof SomeLongLongLongTypeName];
  } | null;
};

// Prettier 2.0.2
export type A = {
  a?: {
    [
      x: string
    ]: typeof SomeLongLongLongTypeName[keyof typeof SomeLongLongLongTypeName];
  } | null;
};
```

#### Revert "markdown: fix redundant leading spaces in markdown list" ([#7847](https://github.com/prettier/prettier/pull/7847))

See [#7846](https://github.com/prettier/prettier/issues/7846)

### Other changes

#### TypeScript: Fix `prettier-ignore` in union types ([#7798](https://github.com/prettier/prettier/pull/7798) by [@thorn0](https://github.com/thorn0))

<!-- prettier-ignore -->
```ts
// Input
export type a =
  // foo
  | foo1&foo2
  // prettier-ignore
  | bar1&bar2
  // baz
  | baz1&baz2;

// Prettier 2.0.0
export type a =
  // foo
  | foo1&foo2
    // prettier-ignore
  // prettier-ignore
  | (bar1 & bar2)
  // baz
  | (baz1 & baz2);

// Prettier 2.0.2
export type a =
  // foo
  | (foo1 & foo2)
  // prettier-ignore
  | bar1&bar2
  // baz
  | (baz1 & baz2);
```

# 2.0.1

[diff](https://github.com/prettier/prettier/compare/2.0.0...2.0.1)

#### API: Fix build script to not corrupt `import-fresh` module ([#7820](https://github.com/prettier/prettier/pull/7820) by [@thorn0](https://github.com/thorn0))

# 2.0.0

[diff](https://github.com/prettier/prettier/compare/1.19.1...2.0.0)

🔗 [Release Notes](https://prettier.io/blog/2020/03/21/2.0.0.html)

# 1.19.1

[diff](https://github.com/prettier/prettier/compare/1.19.0...1.19.1)

### CLI

#### Fix `--stdin` regression in 1.19.0 ([#6894](https://github.com/prettier/prettier/pull/6894) by [@lydell](https://github.com/lydell))

<!-- prettier-ignore -->
```
// Prettier stable
$ echo "test" | prettier --stdin --parser babel
[error] regeneratorRuntime is not defined

// Prettier master
$ echo "test" | prettier --stdin --parser babel
test;
```

### TypeScript

#### Fix formatting of union type as arrow function return type ([#6896](https://github.com/prettier/prettier/pull/6896) by [@thorn0](https://github.com/thorn0))

<!-- prettier-ignore -->
```jsx
// Input
export const getVehicleDescriptor = async (
  vehicleId: string,
): Promise<Collections.Parts.PrintedCircuitBoardAssembly['attributes'] | undefined> => {}

// Prettier stable
export const getVehicleDescriptor = async (
  vehicleId: string
): Promise<| Collections.Parts.PrintedCircuitBoardAssembly["attributes"]
| undefined> => {};

// Prettier master
export const getVehicleDescriptor = async (
  vehicleId: string
): Promise<
  Collections.Parts.PrintedCircuitBoardAssembly["attributes"] | undefined
> => {};
```

# 1.19.0

[diff](https://github.com/prettier/prettier/compare/1.18.2...1.19.0)

🔗 [Release Notes](https://prettier.io/blog/2019/11/09/1.19.0.html)

# 1.18.2

[diff](https://github.com/prettier/prettier/compare/1.18.1...1.18.2)

- TypeScript: only add trailing commas in tuples for `--trailing-comma=all` ([#6199] by [@duailibe])

  In Prettier 1.18 we added trailing commas in tuples when `--trailing-comma=all`, but it was also adding for `--trailing-comma=es5`.

  [#6199]: https://github.com/prettier/prettier/pull/6199
  [@duailibe]: https://github.com/duailibe

# 1.18.1

[diff](https://github.com/prettier/prettier/compare/1.18.0...1.18.1)

- TypeScript: Add trailing comma in tsx, only for arrow function ([#6190] by [@sosukesuzuki])

  Prettier inserts a trailing comma to single type parameter for arrow functions in tsx, since v 1.18. But, this feature inserts a trailing comma to type parameter for besides arrow functions too (e.g, function , interface). This change fix it.

  <!-- prettier-ignore -->
  ```tsx
  // Input
  interface Interface1<T> {
    one: "one";
  }
  function function1<T>() {
    return "one";
  }

  // Output (Prettier 1.18.0)
  interface Interface1<T,> {
    one: "one";
  }
  function function1<T,>() {
    return "one";
  }

  // Output (Prettier 1.18.1)
  interface Interface1<T> {
    one: "one";
  }
  function function1<T>() {
    return "one";
  }
  ```

- Config: Match dotfiles in config overrides ([#6194] by [@duailibe])

  When using [`overrides`](https://prettier.io/docs/configuration#configuration-overrides) in the config file, Prettier was not matching dotfiles (files that start with `.`). This was fixed in 1.18.1

[#6190]: https://github.com/prettier/prettier/pull/6190
[#6194]: https://github.com/prettier/prettier/pull/6194
[@duailibe]: https://github.com/duailibe
[@sosukesuzuki]: https://github.com/sosukesuzuki

# 1.18.0

[diff](https://github.com/prettier/prettier/compare/1.17.1...1.18.0)

🔗 [Release Notes](https://prettier.io/blog/2019/06/06/1.18.0.html)

# 1.17.1

[diff](https://github.com/prettier/prettier/compare/1.17.0...1.17.1)

- Range: Fix ranged formatting not using the correct line width ([#6050] by [@mathieulj])

  <!-- prettier-ignore -->
  ```js
  // Input
  function f() {
    if (true) {
      call("this line is 79 chars", "long", "it should", "stay as single line");
    }
  }

  // Output (Prettier 1.17.0 run with --range-start 30 --range-end 110)
  function f() {
    if (true) {
      call(
        "this line is 79 chars",
        "long",
        "it should",
        "stay as single line"
      );
    }
  }

  // Output (Prettier 1.17.0 run without range)
  function f() {
    if (true) {
      call("this line is 79 chars", "long", "it should", "stay as single line");
    }
  }

  // Output (Prettier 1.17.1 with and without range)
  function f() {
    if (true) {
      call("this line is 79 chars", "long", "it should", "stay as single line");
    }
  }
  ```

- JavaScript: Fix closure compiler typecasts ([#5947] by [@jridgewell])

  If a closing parenthesis follows after a typecast in an inner expression, the typecast would wrap everything to the that following parenthesis.

  <!-- prettier-ignore -->
  ```js
  // Input
  test(/** @type {!Array} */(arrOrString).length);
  test(/** @type {!Array} */((arrOrString)).length + 1);

  // Output (Prettier 1.17.0)
  test(/** @type {!Array} */ (arrOrString.length));
  test(/** @type {!Array} */ (arrOrString.length + 1));

  // Output (Prettier 1.17.1)
  test(/** @type {!Array} */ (arrOrString).length);
  test(/** @type {!Array} */ (arrOrString).length + 1);
  ```

- JavaScript: respect parenthesis around optional chaining before await ([#6087] by [@evilebottnawi])

  <!-- prettier-ignore -->
  ```js
  // Input
  async function myFunction() {
    var x = (await foo.bar.blah)?.hi;
  }

  // Output (Prettier 1.17.0)
  async function myFunction() {
    var x = await foo.bar.blah?.hi;
  }

  // Output (Prettier 1.17.1)
  async function myFunction() {
    var x = (await foo.bar.blah)?.hi;
  }
  ```

- Handlebars: Fix {{else}}{{#if}} into {{else if}} merging ([#6080] by [@dcyriller])

  <!-- prettier-ignore -->
  ```
  // Input
  {{#if a}}
    a
  {{else}}
    {{#if c}}
      c
    {{/if}}
    e
  {{/if}}

  // Output (Prettier 1.17.0)
  {{#if a}}
    a
  {{else if c}}
    c
  e
  {{/if}}

  // Output (Prettier 1.17.1)
  Code Sample
  {{#if a}}
    a
  {{else}}
    {{#if c}}
      c
    {{/if}}
    e
  {{/if}}
  ```

- JavaScript: Improved multiline closure compiler typecast comment detection ([#6070] by [@yangsu])

  Previously, multiline closure compiler typecast comments with lines that
  start with \* weren't flagged correctly and the subsequent parenthesis were
  stripped. Prettier 1.17.1 fixes this issue.

  <!-- prettier-ignore -->
  ```js
  // Input
  const style =/**
   * @type {{
   *   width: number,
   * }}
  */({
    width,
  });

  // Output (Prettier 1.17.0)
  const style =/**
   * @type {{
   *   width: number,
   * }}
  */ {
    width,
  };

  // Output (Prettier 1.17.1)
  const style =/**
   * @type {{
   *   width: number,
   * }}
  */({
    width,
  });
  ```

[@mathieulj]: https://github.com/mathieulj
[@yangsu]: https://github.com/yangsu
[@dcyriller]: https://github.com/dcyriller
[@jridgewell]: https://github.com/jridgewell
[@evilebottnawi]: https://github.com/evilebottnawi
[#6050]: https://github.com/prettier/prettier/pull/6050
[#6070]: https://github.com/prettier/prettier/pull/6070
[#6080]: https://github.com/prettier/prettier/pull/6080
[#6087]: https://github.com/prettier/prettier/pull/6087

# 1.17.0

[diff](https://github.com/prettier/prettier/compare/1.16.2...1.17.0)

🔗 [Release Notes](https://prettier.io/blog/2019/04/12/1.17.0.html)

# 1.16.4

[diff](https://github.com/prettier/prettier/compare/1.16.3...1.16.4)

- API: Fix `prettier.getSupportInfo()` reporting babel parser for older versions of Prettier. ([#5826] by [@azz])

  In version `1.16.0` of Prettier, the `babylon` parser was renamed to `babel`. Unfortunately this lead to a minor breaking change: `prettier.getSupportInfo('1.15.0')` would report that it supported `babel`, not `babylon`, which breaks text-editor integrations. This has now been fixed.

[@azz]: https://github.com/azz
[#5826]: https://github.com/prettier/prettier/pull/5826

# 1.16.3

[diff](https://github.com/prettier/prettier/compare/1.16.2...1.16.3)

- TypeScript: Revert "Update typescript-estree to new package name" ([#5818] by [@ikatyang])

  There's an internal change introduced in Prettier 1.16.2,
  which updated `typescript-estree` to its new package name,
  but unfortunately it broke the output
  so we reverted it as a temporary workaround for now.

  <!-- prettier-ignore -->
  ```ts
  // Input
  export default {
    load<K, T>(k: K, t: T) {
      return {k, t};
    }
  }

  // Output (Prettier 1.16.2)
  export default {
    load(k: K, t: T) {
      return { k, t };
    }
  };

  // Output (Prettier 1.16.3)
  export default {
    load<K, T>(k: K, t: T) {
      return { k, t };
    }
  };
  ```

[@ikatyang]: https://github.com/ikatyang
[#5818]: https://github.com/prettier/prettier/pull/5818

# 1.16.2

[diff](https://github.com/prettier/prettier/compare/1.16.1...1.16.2)

- CLI: Fix CI detection to avoid unwanted TTY behavior ([#5804] by [@kachkaev])

  In Prettier 1.16.0 and 1.16.1, `--list-different` and `--check` logged every file in some CI environments, instead of just unformatted files.
  This unwanted behavior is now fixed.

- HTML: Do not format non-normal whitespace as normal whitespace ([#5797] by [@ikatyang])

  Previously, only non-breaking whitespaces (U+00A0) are marked as non-normal whitespace,
  which means other non-normal whitespaces such as non-breaking narrow whitespaces (U+202F)
  could be formatted as normal whitespaces, which breaks the output. We now follow the spec to
  exclude all non-[ASCII whitespace](https://infra.spec.whatwg.org/#ascii-whitespace) from whitespace normalization.

  (`·` represents a non-breaking narrow whitespace)

  <!-- prettier-ignore -->
  ```html
  <!-- Input -->
  Prix·:·32·€

  <!-- Output (Prettier 1.16.1) -->
  Prix : 32 €

  <!-- Output (Prettier 1.16.2) -->
  Prix·:·32·€
  ```

- JavaScript: Fix record type cast comment detection ([#5793] by [@yangsu])

  Previously, type cast comments with record types were ignored and prettier
  stripped the subsequent parens. Prettier 1.16.2 handles these cases correctly.

  <!-- prettier-ignore -->
  ```js
  // Input
  const v = /** @type {{key: number}} */ (value);

  // Output (Prettier 1.16.1)
  const v = /** @type {{key: number}} */ value;

  // Output (Prettier 1.16.2)
  const v = /** @type {{key: number}} */ (value);
  ```

[@ikatyang]: https://github.com/ikatyang
[@kachkaev]: https://github.com/kachkaev
[@yangsu]: https://github.com/yangsu
[#5793]: https://github.com/prettier/prettier/pull/5793
[#5797]: https://github.com/prettier/prettier/pull/5797
[#5804]: https://github.com/prettier/prettier/pull/5804

# 1.16.1

[diff](https://github.com/prettier/prettier/compare/1.16.0...1.16.1)

- JavaScript: Do not format functions with arguments as react hooks ([#5778] by [@SimenB])

  The formatting added in Prettier 1.16 would format any function receiving an
  arrow function and an array literal to match React Hook's documentation.
  Prettier will now format this the same as before that change if the arrow
  function receives any arguments.

  <!-- prettier-ignore -->
  ```js
  // Input
  ["red", "white", "blue", "black", "hotpink", "rebeccapurple"].reduce(
    (allColors, color) => {
      return allColors.concat(color);
    },
    []
  );

  // Output (Prettier 1.16.0)
  ["red", "white", "blue", "black", "hotpink", "rebeccapurple"].reduce((
    allColors,
    color
  ) => {
    return allColors.concat(color);
  }, []);

  // Output (Prettier 1.16.1)
  ["red", "white", "blue", "black", "hotpink", "rebeccapurple"].reduce(
    (allColors, color) => {
      return allColors.concat(color);
    },
    []
  );
  ```

- JavaScript: Add necessary parentheses for decorators ([#5785] by [@ikatyang])

  Parentheses for decorators with nested call expressions are optional for legacy decorators
  but they're required for decorators in the current [proposal](https://tc39.github.io/proposal-decorators/#sec-syntax).

  <!-- prettier-ignore -->
  ```js
  // Input
  class X {
    @(computed().volatile())
    prop
  }

  // Output (Prettier 1.16.0)
  class X {
    @computed().volatile()
    prop
  }

  // Output (Prettier 1.16.1)
  class X {
    @(computed().volatile())
    prop
  }
  ```

- TypeScript: Stable parentheses for function type in the return type of arrow function ([#5790] by [@ikatyang])

  There's a regression introduced in 1.16 that
  parentheses for function type in the return type of arrow function were kept adding/removing.
  Their parentheses are always printed now.

  <!-- prettier-ignore -->
  ```ts
  // Input
  const foo = (): (() => void) => (): void => null;
  const bar = (): () => void => (): void => null;

  // First Output (Prettier 1.16.0)
  const foo = (): () => void => (): void => null;
  const bar = (): (() => void) => (): void => null;

  // Second Output (Prettier 1.16.0)
  const foo = (): (() => void) => (): void => null;
  const bar = (): () => void => (): void => null;

  // Output (Prettier 1.16.1)
  const foo = (): (() => void) => (): void => null;
  const bar = (): (() => void) => (): void => null;
  ```

- MDX: Correctly recognize inline JSX ([#5783] by [@ikatyang])

  Previously, some inline JSXs are wrongly recognized as block HTML/JSX,
  which causes unexpected behaviors. This issue is now fixed.

  <!-- prettier-ignore -->
  ```md
  <!-- Input -->
  _foo <InlineJSX /> bar_

  <!-- Output (Prettier 1.16.0) -->
  _foo

  <InlineJSX /> bar_

  <!-- Output (Prettier 1.16.1) -->
  _foo <InlineJSX /> bar_
  ```

[@ikatyang]: https://github.com/ikatyang
[@simenb]: https://github.com/SimenB
[#5778]: https://github.com/prettier/prettier/pull/5778
[#5783]: https://github.com/prettier/prettier/pull/5783
[#5785]: https://github.com/prettier/prettier/pull/5785
[#5790]: https://github.com/prettier/prettier/pull/5790

# 1.16.0

[diff](https://github.com/prettier/prettier/compare/1.15.3...1.16.0)

🔗 [Release Notes](https://prettier.io/blog/2019/01/20/1.16.0.html)

# 1.15.3

[diff](https://github.com/prettier/prettier/compare/1.15.2...1.15.3)

- JavaScript: support `htm` ([#5565](https://github.com/prettier/prettier/pull/5565))
- JavaScript: support logical assignment operator ([#5489](https://github.com/prettier/prettier/pull/5489))
- JavaScript: do not add quotes for interpolation-only attributes in `html` templates ([#5544](https://github.com/prettier/prettier/pull/5544))
- JavaScript: add missing parenthesis for binary in optional member ([#5543](https://github.com/prettier/prettier/pull/5543))
- JavaScript: fix a parser regression ([#5530](https://github.com/prettier/prettier/pull/5530))
- JavaScript: improve union types with leading comments ([#5575](https://github.com/prettier/prettier/pull/5575))
- TypeScript: support BigInt ([#5546](https://github.com/prettier/prettier/pull/5546), [#5577](https://github.com/prettier/prettier/pull/5577))
- TypeScript: inline method decorators should stay inlined ([#5444](https://github.com/prettier/prettier/pull/5444))
- TypeScript: do not change `module` into `namespace` and break/hug their body correctly ([#5551](https://github.com/prettier/prettier/pull/5551))
- TypeScript: do not add invalid semicolon for construct in interface with `// prettier-ignore` ([#5469](https://github.com/prettier/prettier/pull/5469))
- HTML: do not touch comments ([#5525](https://github.com/prettier/prettier/pull/5525))
- HTML: preserve bogus comments `<! ... >`/`<? ... >` ([#5565](https://github.com/prettier/prettier/pull/5565))
- HTML: support IE conditional start/end comment ([#5470](https://github.com/prettier/prettier/pull/5470))
- HTML: do not add extra indentation for js template in `<script>` ([#5527](https://github.com/prettier/prettier/pull/5527))
- HTML: leading spaces for the first interpolation in `<textarea>` are sensitive ([#5468](https://github.com/prettier/prettier/pull/5468))
- HTML: preserve content for element in `<pre>` correctly ([#5473](https://github.com/prettier/prettier/pull/5473))
- HTML: correct column for error code frame ([#5553](https://github.com/prettier/prettier/pull/5553))
- Angular: support interpolation in attributes ([#5573](https://github.com/prettier/prettier/pull/5573))
- Angular: do not print colon for `then` and `else` in `*ngIf` ([#5542](https://github.com/prettier/prettier/pull/5542))
- Angular/Vue: do not normalize tag/attribute names ([#5526](https://github.com/prettier/prettier/pull/5526), [#5549](https://github.com/prettier/prettier/pull/5549))
- Vue: preserve custom block ([#5458](https://github.com/prettier/prettier/pull/5458))
- Vue: remove unnecessary semicolon and preserve necessary semicolon for single expression in event bindings ([#5519](https://github.com/prettier/prettier/pull/5519))
- Vue: group `slot-scope` correctly ([#5563](https://github.com/prettier/prettier/pull/5563))
- Markdown: do not trim content in inline-math ([#5485](https://github.com/prettier/prettier/pull/5485))
- Markdown: add more category to CJK regex ([#5480](https://github.com/prettier/prettier/pull/5480))
- SCSS: update parser for performance improvements ([#5481](https://github.com/prettier/prettier/pull/5481))
- YAML: preserve the first document head end marker `---` ([#5502](https://github.com/prettier/prettier/pull/5502))
- API: resolve `ignored` field correctly in `.getFileInfo()` with absolute filePath ([#5570](https://github.com/prettier/prettier/pull/5570))
- API/CLI: fix a bug that caches for `.js` config files did not respect `.clearConfigCache()` ([#5558](https://github.com/prettier/prettier/pull/5558))
- API/CLI: ignore `unset` in `.editorconfig` ([#5550](https://github.com/prettier/prettier/pull/5550))
- CLI: report status code `0` for `--list-different` + `--write` ([#5512](https://github.com/prettier/prettier/pull/5512))
- Standalone: fix a regression for browser compatibility ([#5560](https://github.com/prettier/prettier/pull/5560))

# 1.15.2

[diff](https://github.com/prettier/prettier/compare/1.15.1...1.15.2)

- CLI: allow flag overriding ([#5390](https://github.com/prettier/prettier/pull/5390))
- JavaScript: do not apply test call formatting to arrow function without body ([#5366](https://github.com/prettier/prettier/pull/5366))
- JavaScript: do not duplicate comments in styled-components ([#5416](https://github.com/prettier/prettier/pull/5416))
- JavaScript: do not indent comments behind variable declarations ([#5434](https://github.com/prettier/prettier/pull/5434))
- JavaScript: inline property decorator should stay inline ([#5364](https://github.com/prettier/prettier/pull/5364), [#5423](https://github.com/prettier/prettier/pull/5423))
- JavaScript: treat `createSelector` as function composition ([#5430](https://github.com/prettier/prettier/pull/5430))
- Flow: do not move flow comment for function argument to its body ([#5435](https://github.com/prettier/prettier/pull/5435))
- Flow: force-break interface body to be consistent with TypeScript interface ([#5432](https://github.com/prettier/prettier/pull/5432))
- Flow/TypeScript: remove extra indentation for `extends` ([#5432](https://github.com/prettier/prettier/pull/5432))
- TypeScript: distinguish `module` and `namespace` correctly ([#5432](https://github.com/prettier/prettier/pull/5432))
- HTML: handle CRLF correctly ([#5393](https://github.com/prettier/prettier/pull/5393))
- HTML: handle `<pre>` with interpolation ([#5400](https://github.com/prettier/prettier/pull/5400))
- HTML: preserve content for `<template>` with unknown `lang` ([#5388](https://github.com/prettier/prettier/pull/5388))
- HTML: preserve incomplete IE conditional comments ([#5429](https://github.com/prettier/prettier/pull/5429))
- HTML: preserve unterminated IE conditional comments ([#5424](https://github.com/prettier/prettier/pull/5424))
- HTML: treat capital element as custom element ([#5395](https://github.com/prettier/prettier/pull/5395))
- Angular: add missing parens for pipe in ternary ([#5397](https://github.com/prettier/prettier/pull/5397))
- Angular: correctly print unary expression with operator `+` ([#5405](https://github.com/prettier/prettier/pull/5405))
- Angular: correctly handle parens ([#5387](https://github.com/prettier/prettier/pull/5387))
- Angular/Vue: whitespaces between interpolation and text are sensitive ([#5396](https://github.com/prettier/prettier/pull/5396))
- Vue: do not add invalid semicolon for `v-on` attribute value ([#5418](https://github.com/prettier/prettier/pull/5418))
- SCSS: do not crash on grid value ([#5394](https://github.com/prettier/prettier/pull/5394))
- Markdown: handle CRLF correctly ([#5414](https://github.com/prettier/prettier/pull/5414))
- Markdown: identify CJK correctly ([#5402](https://github.com/prettier/prettier/pull/5402))
- MDX: treat JSX code block same as in Markdown ([#5391](https://github.com/prettier/prettier/pull/5391))

# 1.15.1

[diff](https://github.com/prettier/prettier/compare/1.15.0...1.15.1)

- Markdown: do not keep increasing backslashes for dollar sign ([#5358](https://github.com/prettier/prettier/pull/5358))

# 1.15.0

[diff](https://github.com/prettier/prettier/compare/1.14.3...1.15.0)

🔗 [Release Notes](https://prettier.io/blog/2018/11/07/1.15.0.html)

# 1.14.3

[diff](https://github.com/prettier/prettier/compare/1.14.2...1.14.3)

- Chore: add missing LICENSE ([#5114](https://github.com/prettier/prettier/pull/5114))

# 1.14.2

[diff](https://github.com/prettier/prettier/compare/1.14.1...1.14.2)

- YAML: fix the line ending issue on Windows ([#4957](https://github.com/prettier/prettier/pull/4957))
- TypeScript: better error message ([#4947](https://github.com/prettier/prettier/pull/4947))

# 1.14.1

[diff](https://github.com/prettier/prettier/compare/1.14.0...1.14.1)

- JavaScript: add parens for unary in bind ([#4950](https://github.com/prettier/prettier/pull/4950))
- JavaScript: format angular jasmine `it("should ...", fakeAsync(() => { ...` correctly. ([#4954](https://github.com/prettier/prettier/pull/4954))
- JavaScript: Revert this/super blacklist for function composition heuristic ([#4936](https://github.com/prettier/prettier/pull/4936))
- JavaScript: no extra space on Flow interface method named `static` ([#4910](https://github.com/prettier/prettier/pull/4910))
- JavaScript: no extra line break in destructed assignment of ternary ([#4932](https://github.com/prettier/prettier/pull/4932))
- Flow: print ObjectTypeInternalSlot with both flow/babel parsers ([#4869](https://github.com/prettier/prettier/pull/4869))
- TypeScript: no invalid output for ImportType in TypeReference ([#4939](https://github.com/prettier/prettier/pull/4939))
- YAML: do not throw on duplicate merge key ([#4931](https://github.com/prettier/prettier/pull/4931))
- YAML: no duplicate comments in mappingValue ([#4931](https://github.com/prettier/prettier/pull/4931))
- YAML: print end comment in nested mapping correctly ([#4918](https://github.com/prettier/prettier/pull/4918))
- YAML: do not put singleline values on a separate line from the key ([#4916](https://github.com/prettier/prettier/pull/4916))
- YAML: prefer dash as document separator ([#4921](https://github.com/prettier/prettier/pull/4921))
- API: update support info for Flow ([#4943](https://github.com/prettier/prettier/pull/4943))
- CLI: ignore .git, .svn and .hg directories ([#4906](https://github.com/prettier/prettier/pull/4906))
- CLI: support TOML configuration files ([#4877](https://github.com/prettier/prettier/pull/4877))

# 1.14.0

[diff](https://github.com/prettier/prettier/compare/1.13.7...1.14.0)

🔗 [Release Notes](https://prettier.io/blog/2018/07/29/1.14.0.html)

# 1.13.7

[diff](https://github.com/prettier/prettier/compare/1.13.6...1.13.7)

- Remove calls to `eval("require")` in the distributed code ([#4766](https://github.com/prettier/prettier/pull/4766))

# 1.13.6

[diff](https://github.com/prettier/prettier/compare/1.13.5...1.13.6)

- Upgrade Flow parser to 0.75.0 ([#4649](https://github.com/prettier/prettier/pull/4649) and [#4727](https://github.com/prettier/prettier/pull/4727))
- Preserve type parameters of import-types in TypeScript ([#4662](https://github.com/prettier/prettier/pull/4662))
- Preserve parens for type casting for sub-item ([#4648](https://github.com/prettier/prettier/pull/4648))

# 1.13.5

[diff](https://github.com/prettier/prettier/compare/1.13.4...1.13.5)

- Better handling of trailing spaces in Markdown ([#4593](https://github.com/prettier/prettier/pull/4593))
- Fix empty file error in JSON and GraphQL ([#4553](https://github.com/prettier/prettier/pull/4553))
- Preserve decorator on TypeScript interfaces ([#4632](https://github.com/prettier/prettier/pull/4632))
- Inline \_ or \$ in the root of a method chain ([#4621](https://github.com/prettier/prettier/pull/4621))

# 1.13.4

[diff](https://github.com/prettier/prettier/compare/1.13.3...1.13.4)

- Fix a regression when printing graphql-in-js ([#4616](https://github.com/prettier/prettier/pull/4616))

# 1.13.3

[diff](https://github.com/prettier/prettier/compare/1.13.2...1.13.3)

- Fix a regression when printing `hasOwnProperty` and other functions in `Object`'s prototype ([#4603](https://github.com/prettier/prettier/pull/4603))
- Fix a regression in exit status when using `--debug-check` and `--list-different` ([#4600](https://github.com/prettier/prettier/pull/4600))

# 1.13.2

[diff](https://github.com/prettier/prettier/compare/1.13.1...1.13.2)

- Republished 1.13.1 with missing README included this time

# 1.13.1

[diff](https://github.com/prettier/prettier/compare/1.13.0...1.13.1)

- Revert default parser change in API (still present in CLI)

# 1.13.0

[diff](https://github.com/prettier/prettier/compare/1.12.1...1.13.0)

🔗 [Release Notes](https://prettier.io/blog/2018/05/23/1.13.0.html)

# 1.12.1

[diff](https://github.com/prettier/prettier/compare/1.12.0...1.12.1)

- Fix for tag being removed from CSS with embedded expressions ([#4302](https://github.com/prettier/prettier/pull/4302))
- Wrap awaits in unary expressions with parens ([#4315](https://github.com/prettier/prettier/pull/4315))
- Fix style regression on flow union types ([#4325](https://github.com/prettier/prettier/pull/4325))

# 1.12.0

[diff](https://github.com/prettier/prettier/compare/1.11.1...1.12.0)

🔗 [Release Notes](https://prettier.io/blog/2018/04/11/1.12.0.html)

# 1.11.1

[diff](https://github.com/prettier/prettier/compare/1.11.0...1.11.1)

- 1.11.0 was incorrectly shipped with the wrong version of the TypeScript parser, which broke conditional types. This release fixes it.
- Fixed an issue relating to deprecated parsers ([#4072](https://github.com/prettier/prettier/pull/4072))

# 1.11.0

[diff](https://github.com/prettier/prettier/compare/1.10.2...1.11.0)

🔗 [Release Notes](https://prettier.io/blog/2018/02/26/1.11.0.html)

# 1.10.2

[diff](https://github.com/prettier/prettier/compare/1.10.1...1.10.2)

- Fixed an issue printing .vue files with self-closing tags. (#3705 by duailibe)

# 1.10.1

[diff](https://github.com/prettier/prettier/compare/1.10.0...1.10.1)

- Fixed an issue where the CLI fails to resolve a file.

# 1.10.0

[diff](https://github.com/prettier/prettier/compare/1.9.2...1.10.0)

🔗 [Release Notes](https://prettier.io/blog/2018/01/10/1.10.0.html)

# 1.9.2

[diff](https://github.com/prettier/prettier/compare/1.9.1...1.9.2)

- Fixed trailing comma not being printed in function calls if the last arg was an arrow (#3428 by duailibe)
- Ignore whitespace after the `/**` in docblocks (#3430 by duailibe)
- Fixed a bug where `get` and `set` class properties arrows would print an unnecessary semicolon with `--no-semi` (#3434 by duailibe)
- Fixed a bug for missing `.editorconfig` files (#3439 by josephfrazier)
- Fix comments being moved in class methods and object properties with the babylon parser (#3441 by duailibe)
- Better printing of member chains with a TSNonNullExpression (`!` character) (#3442 by duailibe)
- Fix missing commas in object properties when a `prettier-ignore` comment is present (#3448 by duailibe)
- Fix printing union types inside a function param type (#3446 by duailibe)
- Fix closing parens on multi-line intersection/union type (#3436 by josephfrazier)
- Don't break single argument destructuring arguments (for arrays and with simple default values) (#3443 by duailibe)

# 1.9.1

[diff](https://github.com/prettier/prettier/compare/1.9.0...1.9.1)

- Fixed a bug of comments with JSX fragments being duplicated in output (#3398 by duailibe)
- Fixed a bug that Prettier would fail when using tabs (#3401 by ikatyang)
- Fixed a regression that removed trailing commas when printing JS code blocks in Markdown (#3405 by azz)
- Fixed a bug when using glob `**/*` which would try to format directories (#3411 by duailibe)
- Fixed a bug when `.editorconfig` had `max_line_length = "off"` (#3412 by duailibe)

# 1.9.0

[diff](https://github.com/prettier/prettier/compare/1.8.2...1.9.0)

🔗 [Release Notes](https://prettier.io/blog/2017/12/05/1.9.0.html)

# 1.8.2

[diff](https://github.com/prettier/prettier/compare/1.8.1...1.8.2)

- Markdown: Don't break on links (#3204 by ikatyang)
- Markdown: Add `--no-prose-wrap` option (#3199 by ikatyang)
- TypeScript: Parenthesis around TSAsExpression inside TSAbstractClassDeclaration (#3191 by duailibe)
- JSON: Print JSON top comments as leading comments of root node (#3187 by duailibe)

# 1.8.1

[diff](https://github.com/prettier/prettier/compare/1.8.0...1.8.1)

- Force JSON to no trailing comma in multiparser (#3182 by azz)
- Don't add trailing commas in JSXAttribute for arrow functions (#3181 by duailibe)
- Markdown: Allow more cases that `_`-style emphasis is available (#3186 by ikatyang)
- Markdown: Handle additional spaces before `code` (#3180 by ikatyang)
- Markdown: Do not break on unbreakable place (#3177 by ikatyang)
- Markdown: Do not break before special prefix (#3172 by ikatyang)

# 1.8.0

[diff](https://github.com/prettier/prettier/compare/1.7.4...1.8.0)

🔗 [Release Notes](https://prettier.io/blog/2017/11/07/1.8.0.html)

# 1.7.4

[diff](https://github.com/prettier/prettier/compare/1.7.3...1.7.4)

- Force template literals to break after \` for styled-components (#2926 by duailibe)
- Update cosmiconfig to v3.1.0 (#2952 by ikatyang)
- Respect --stdin-filepath, regardless of config source (#2948 by azz)

# 1.7.3

[diff](https://github.com/prettier/prettier/compare/1.7.2...1.7.3)

- Fix cosmiconfig in the built version of Prettier (#2930 by lydell)
- Fix: ignore and show warning for unknown option from config file (#2929 by ikatyang)
- Don't use parens with optional chaining member expressions (#2921 by azz)

# 1.7.2

[diff](https://github.com/prettier/prettier/compare/1.7.1...1.7.2)

- Revert "Fix line break in test declarations with a single argument function declaration" (#2912)

# 1.7.1

[diff](https://github.com/prettier/prettier/compare/1.7.0...1.7.1)

- Enable cosmiconfig rcExtensions (#2749 by elektronik2k5)
- Keep original empty lines in argument list (#2763 by jackyho112)
- Upgrade prettier dependency to 1.7.0, fix lint (#2821 by josephfrazier)
- Fix different precedence binary expression when inlining (#2827 by duailibe)
- Bump Babylon (#2831 by existentialism)
- Don't lowercase Less variables when parsed with SCSS parser (#2833 by lydell)
- Don't lowercase `&class` in SCSS/Less selectors (#2834 by lydell)
- Add support for ClassPrivateProperty (#2837 by existentialism)
- Upgrade cosmiconfig to v3, remove hardcoded combinatorial problem (#2843 by azz)
- Split Less and SCSS parsing into different parsers (#2844 by lydell)
- feat: support detailed `--help` (#2847 by ikatyang)
- Update cosmiconfig to 3.0.1 to avoid memory leak (#2848 by danez)
- chore: add prettier-stylelint to the related projects (#2859 by hugomrdias)
- Don't lowercase SCSS placeholder selectors (#2876 by lydell)
- Fix line break in test declarations with 2nd argument as a function (#2877 by duailibe)
- Use semicolons in Flow interface-like bodies (#2593) (#2888 by motiz88)
- We do not need to have a reference to the toolbox-companion since we (#2892 by mitermayer)
- fix(cli): validate options for every `config-precedence` (#2894 by ikatyang)
- fix: do not print stack trace for invalid option (#2895 by ikatyang)
- refactor: use custom error (#2896 by ikatyang)
- fix(typescript): allow symbol type (#2899 by ikatyang)
- Support fit(), xit(), it.only(), etc (#2900 by azz)
- Fix editor styling on empty editors (#2904 by jakegavin)
- Fix printing of comments between decorators and method names (#2906 by azz)

# 1.7.0

[diff](https://github.com/prettier/prettier/compare/1.6.1...1.7.0)

🔗 [Release Notes](https://prettier.io/blog/2017/09/15/1.7.0.html)

# 1.6.1

[diff](https://github.com/prettier/prettier/compare/1.6.0...1.6.1)

- Fix CLI option parsing with no arguments (#2684)
- Fix config file finding when using stdin (#2692)
- Fix union type with type params regression (#2688)
- Fix flow parenthesis regression (#2687)

# 1.6.0

[diff](https://github.com/prettier/prettier/compare/1.5.3...1.6.0)

🔗 [Release Notes](https://prettier.io/blog/2017/08/29/1.6.0.html)

# 1.5.3

[diff](https://github.com/prettier/prettier/compare/1.5.2...1.5.3)

- Force trailingComma option to "none" when parser is JSON (#2335)

# 1.5.2

[diff](https://github.com/prettier/prettier/compare/1.5.1...1.5.2)

- Full printing support for GraphQL and various bug fixes
- Fixes for range formatting for JSON and CSS (#2295, #2298)

# 1.5.1

[diff](https://github.com/prettier/prettier/compare/1.5.0...1.5.1)

- Go back to babylon beta 13 (#2289)
- Inline import('x') to avoid having trailing comma (#2288)

# 1.5.0

[diff](https://github.com/prettier/prettier/compare/1.4.4...1.5.0)

🔗 [Release Notes](https://prettier.io/blog/2017/06/28/1.5.0.html)

# 1.4.4

🔗 Fix postcss, I forgot to re-run the build script :(

# 1.4.3

[diff](https://github.com/prettier/prettier/compare/1.4.2...1.4.3)

- Fix support for node 4 (#1988)
- Fix website on iOS Safari (#1970)

Formatting change:

- Position JSX whitespace (`{" "}`) at the end of lines (#1964)

Lots of small fixes, mainly for TypeScript.

# 1.4.2

[diff](https://github.com/prettier/prettier/compare/1.4.1...1.4.2)

- fix(decorators): do not inline methods with decorators with babylon (#1934)
- fix(typescript): print semi with inline interfaces/types (#1936)
- fix(typescript): no semi after export default abstract class, fixes (#1937)
- TypeScript: fix trailing comma in enum (#1938)

# 1.4.1

[diff](https://github.com/prettier/prettier/compare/1.4.0...1.4.1)

- Lots of fixes for TypeScript and regressions from 1.4.0. If you are using 1.4.0, you should migrate to 1.4.1 asap ;)

# 1.4.0

[diff](https://github.com/prettier/prettier/compare/1.3.1...1.4.0)

🔗 [Release Notes](https://prettier.io/blog/2017/06/03/1.4.0.html)

# 1.3.1

[diff](https://github.com/prettier/prettier/compare/1.3.0...1.3.1)

- Respect template inline-ness (#1497)

# 1.3.0

[diff](https://github.com/prettier/prettier/compare/1.2.2...1.3.0)

🔗 [Release Notes](https://prettier.io/blog/2017/05/03/1.3.0.html)

- add printer branches for some TypeScript nodes (#1331)
- Skip trailing commas with FlowShorthandWithOneArg (#1364)
- add TSLastTypeNode and TSIndexedAccessType (#1370)
- add TSConstructorType (#1367)
- fix do-while break (#1373)
- Fix missing trailing commas on flow generics (#1381)
- Add example of using yarn test with arguments (#1383)
- Have --debug-check also run ast verification (#1337)
- Fix empty line in block with EmptyStatement (#1375)
- parent decides how to print type annotations (#1391)
- add TSTypeOperator (#1396)
- fix TSTypeReference not printing typeArguments (#1392)
- add TSMappedType and TSTypeParameter (#1393)
- fix TSFunctionType failing on TypeParameters (#1394)
- add TSIntersectionType (#1395)
- fix typeParameters printing TSFunctionType w/o breaking flow (#1397)
- Fix optional flow parenthesis (#1357)
- [experimental] Add linting step in test pipeline (#1172)
- fix VariableDeclarator not printing type parameters (#1415)
- add TSMethodSignature (#1416)
- Add TSParameterProperty, TSAbstractClassDeclaration and TSAbstractMethodDefinition (#1410)
- Inline nullable in flow generics (#1426)
- fixed method 'check' error 'format' of undefined (#1424)
- feat(typescript): add declare modifier support for vars, classes and functions (#1436)
- Allow flow declarations to break on StringLiteralTypeAnnotations (#1437)
- Require '::a.b' to have a preceding ; in no-semi style (#1442)
- Require '(a || b).c++' to have a preceding ; in no-semi style (#1443)
- Upgrade flow parser to 0.45 (#1447)
- Add supertype tests and add TSAbstractClassProperty (#1467)
- Break class expression returned by arrow call (#1464)
- update typescript snapshots to account for #1464 (#1470)
- [WIP] add TSNamespaceExportDeclaration (#1459)
- update yarn.lock (#1471)
- [RFC] Do not indent calls with a single template literal argument (#873)
- Proper indentation for template literals (#1385)
- Add parenthesis for unusual nested ternaries (#1386)
- Preserve inline comment as last argument (#1390)
- Only add parenthesis on ternaries inside of arrow functions if doesn't break (#1450)
- Fix windows line ending on template literals (#1439)
- Add space around `=` for flow generics default arguments (#1476)
- Don't break for unparenthesised single argument flow function (#1452)
- Don't break on empty arrays and objects (#1440)
- Do not break on `[0]` (#1441)
- Reorder flow object props (#1451)
- Break inline object first in function arguments (#1453)
- Break inline object first in function arguments (#1453) (#1173)
- Inline template literals as arrow body (#1485)

# 1.2.2

[diff](https://github.com/prettier/prettier/compare/1.2.1...1.2.2)

- Only break for conditionals (#1350)

# 1.2.1

[diff](https://github.com/prettier/prettier/compare/1.2.0...1.2.1)

- Fix duplicate comments in classes (#1349)

# 1.2.0

[diff](https://github.com/prettier/prettier/compare/1.1.0...1.2.0)

🔗 [Release Notes](https://prettier.io/blog/2017/04/20/1.2.0.html)

- match jsx files in pre-commit hook (#1276)
- Fix isPreviousLineEmpty on Windows (#1263)
- Add --dev option to suggested install cmd (#1289)
- Write out change CLI changes synchronously. Fixes #1287. (#1292)
- Remove emoji part from lint-staged's name (#1302)
- omit 'doc' key from options object before passing it to format() (#1299)
- Skip globbing filenames with node-glob when the filename is not a glob (#1307)
- FIX: more documentation for jetbrains (#1265)
- Fix template literal comments (#1296)
- Double quotes for option values in Readme file (#1314)
- Do not print the sub-tree when using prettier-ignore (#1286)
- Bail when traversing === groups (#1294)
- Avoid breaking arguments for last arg expansion (#1305)
- Add typescript as a valid parser value (#1318)
- Add JetBrains File Watcher docs (#1310)
- Add prettier_d to Related Projects (#1328)
- Add parentheses for assignment as body of arrow (#1326)
- Add information about Vim's other autocmd events (#1333)
- add printer branch for TSFirstTypeNode (#1332)
- Optimize `prettier --help` for humans (#1340)
- Update link to @vjeux's React London presentation (#1330)
- Improve regex printing (#1341)
- Fix arrow function parenthesis with comments in flow (#1339)
- Break if () if conditional inside breaks (#1344)
- Don't inline paren at right of arguments (#1345)

# 1.1.0

[diff](https://github.com/prettier/prettier/compare/1.0.0...1.1.0)

- Prettier 1.0 is the stabler release we've been waiting for (#1242)
- fix small typo (#1255)
- Fix : ReferenceError: err is not defined (#1254)
- Document debugging strategies (#1253)
- Do not inline member expressions as part of assignments (#1256)
- Fix flow union params (#1251)
- Use a whitelist instead of blacklist for member breaking (#1261)
- Remove trailing whitespace (#1259)
- Get rid of fixFaultyLocations code (#1252)
- Fixing n.comments check in printer (#1239)
- [WIP] no-semi comments (#1257)

# 1.0.1

- change semi default

# 1.0.0

[diff](https://github.com/prettier/prettier/compare/0.22.0...1.0.0)

🔗 [Release Notes](https://prettier.io/blog/2017/04/13/1.0.0.html)

# 0.22.0

[diff](https://github.com/prettier/prettier/compare/0.21.0...0.22.0)

- Run 0.21.0 (#876)
- Fix paren removal on UnionTypeAnnotation (#878)
- Fix typo (#891)
- Ensure no parens for JSX inside of an ArrayExpression (#895)
- Fix object expression in arrow function expression (#897)
- Fix unprinted comments in destructuring (#898)
- Fix bug with importing empty type (#904)
- Fix broken export comment (#899)
- Add CLI Example to Readme (#909)
- Fix 0.5e0 (#911)
- Fix binary expression instanceof in arrow function expression (#913)
- Improve readme CLI usage example (#910)
- Do not break long it/test calls when template literal (#893)
- Update lint-staged docs to use husky for less config. (#923)
- Fix files with comments only (#813)
- Update README.md (#928)
- Fix binary op as body in arrow expression (#921)
- cleanup needsParens (#935)
- [JSX] Break if opening element breaks (#942)
- Parenthesize function expressions in expression position (#941)
- update the README to add a pre-commit hook (#944)
- Fix #951: properly parenthesize \*\* expressions (#952)
- [WIP] TypeScript Parser (#915)
- Do not break long `describe` calls (#953)
- Recursively find leading comments inside ReturnStatements (#955)
- Fix `in` inside of a for in a nested way (#954)
- Make comments around empty parenthesis be inside (#957)
- Stabilize comment after object label (#958)
- Inline BinaryExpressions inside JSXExpression (#965)
- Only allow same-line arrow-less body for explicit nodes (#966)

# 0.21.0

[diff](https://github.com/prettier/prettier/compare/0.20.0...0.21.0)

- [JSX] Break before and after jsx whitespace (#836)
- re-run snapshot tests
- Run prettier 0.20.0 (#835)
- [JSX] Don't wrap JSX Elements in parentheses in {} (#845)
- Fix comment after the last argument of a function (#856)
- Fix travis build image
- Do not break require calls (#841)
- Stabilize import as comments (#855)
- Fix jsx expression comment that break (#852)
- Inline last arg function arguments (#847)
- Keep parenthesis on export default function (#844)
- Inline short expressions for boolean operators too (#826)
- Introduce -l option (#854)
- Add parenthesis around assignments (#862)
- Do not put \n after label (#860)
- Fix comment for `call( // comment` (#858)
- Do not break long it calls (#842)
- Fix flow union comments (#853)

# 0.20.0

[diff](https://github.com/prettier/prettier/compare/0.19.0...0.20.0)

- Fix extra parens for update expressions (#796)
- Fix empty options (#803)
- Eagerly evaluate `ifBreak` when processing template literals (fixes #795
- Fix function declaration params comments (#802)
- Update flow to 0.40 (#808)
- Correct link for travis
- Fix function call args (#809)
- Properly support `do` (#811)
- Do not put parenthesis around not named default export (#819)
- Adds another preset to related projects (#820)
- Fix trailing commas in docs (#825)
- Put short body of arrow functions on the same line (#829)
- Preserve new lines for comments after `=` (#830)
- Fix indentation of a merged group (#828)
- Migrate class comments to the beginning (#831)
- Update list of related projects (#833)
- Allow breaking for logical expressions in member chains (#827)

# 0.19.0

[diff](https://github.com/prettier/prettier/compare/0.18.0...0.19.0)

- docs(README): use yarn add for consistency (#734)
- Make trailing-comma option support 2 different modes (#641)
- Update README with valid trailingComma options
- Fix await ternary parenthesis (#740)
- Fix missing dangling comment in exports (#741)
- Fix missing dangling comments in arrays (#744)
- Remove extra parenthesis around await inside of unary expression (#745)
- Fix missing dangling comments in for loop (#742)
- Add note about trailingComma option in versions 0.18.0 and below
- Add missing explanatory comment in ForStatement case (#748)
- Fix leading & operators in flow types (#738)
- Fix missing comments in assignment pattern (#704)
- Correctly place trailing comments in conditionals (#754)
- Use double quotes in script wildcards to support windows `cmd.exe`. (#761)
- Upgrade to Jest 19 (#762)
- Upgrade to Jest 19.0.1 (#779)
- Remove extra parens around ternary arguments of a new call (#776)
- Do not attach comments to EmptyStatements in try/catch (#763)
- Bump babylon & add test for async func decl (#790)
- Add `this` for Member factory whitelist and remove softline (#782)
- Do not expand empty catch (#783)
- Group [0] at the end of the previous chain instead of beginning of next one (#784)
- Do not format template literals (#749)
- Revert babylon bump (#792)
- Do not put trailing commas for function declaration in es5 mode (#791)
- [WIP] Fix comments in template literals (#643)
- Introduce line-suffix-boundary (#750)
- [RFC] Add parenthesis around && inside of || (#780)
- Fix tests on node 4

# 0.18.0

[diff](https://github.com/prettier/prettier/compare/0.17.1...0.18.0)

- fix --debug-check
- [JSX] Don't add newline following newline (#690)
- [Docs] Use replaceState API when demo code changes (#710)
- Do not inline new as last argument (#705)
- Inline objects & arrays as right part of a boolean expression (#692)
- [RFC] Remove parenthesis object special case (#689)
- Ensure importKind is printed (#718)
- [Docs]: update Readme to reference VS extension (#720)
- docs: Add pre-commit hook with 🚫💩 lint-staged section to the README (#714)
- [RFC] Preserve new lines between array elements (#707)
- Do not put \n inside of empty object method bodies (#706)
- Align boolean inside of arrow functions (#691)
- Fix trailing new lines preservation (#724)
- Unified Split

# 0.17.1

[diff](https://github.com/prettier/prettier/compare/0.17.0...0.17.1)

- Use `readline` api to manipulate `process.stdout` output. (#687)

# 0.17.0

[diff](https://github.com/prettier/prettier/compare/0.16.0...0.17.0)

- [JSX] Fix spurious newline (fixes #614) (#628)
- Add --debug-check cli option (#627)
- Remove last trailing line for directives-only files (#609)
- Expand vim instructions
- Fix formatting in readme
- Update snapshots
- Preserve empty line before last comment (#594)
- test on current release of node.js (#595)
- [JSX] jsx-whitespace breaks with parent (fixes #622) (#626)
- Log filename with [update] or [ignore] flags during `--write` process. (#584)
- Do not indent binary expressions inside of if (#604)
- Put short elements at right of single binary expression on same line (#605)
- Run prettier 0.16.0 on the codebase (#631)
- Preserve blank lines inside of objects (#606)
- fix typo in JetBrains External Tool config readme (#679)
- Fix dangling comments for arrays (#665)
- Print line-suffix in --debug-print-doc (#676)
- Avoid unneeded parenthesis for colon with comments (#673)
- Stabilize comments inside of if/then/else before { (#672)
- Soft break the first member of a chain (#667)
- Stabilize comments inside of ternaries (#666)
- Fix trailing commas with a trailing comment (#664)
- Fix Flow union type annotations indentation (#650)
- Ensure that all comments are printed (#571)
- Properly support member chains comments (#668)
- [WIP] Fix Flow DeclareTypeAlias (#669)
- Add option for putting > on the last line in jsx (#661)
- Always put a hardline before dangling comment (#675)
- Fix comments in return statement argument (#657)
- [RFC] Introduce prettier-ignore-next (#671)

# 0.16.0

[diff](https://github.com/prettier/prettier/compare/0.15.0...0.16.0)

- Revert "Print \x and \u escapes in strings and regexes lowercase (#522)
- Fix ternary indent bug (#577)
- jsx parentheses fix (#580)
- Run prettier on 0.15.0 (#558)
- Add parenthesis around single argument arrow if comments (#573)
- Use breakParent inside of last arrow expansion (#559)
- Support dangling comments in ClassBody (#570)
- Make all the member expressions but the last one part of the first group (#589)
- Break long imports (#590)
- Remove the concept of globalPrecedingNode (#561)
- Remove test.js and put it back in the gitignore
- Fix use strict as expression statement (#602)
- Use arrow function when inputted that way for flow objects (#608)
- Better support try/catch comments (#607)
- Print CallExpression comments inside of memberChain (#600)
- Do not attach comments to EmptyStatement (#599)
- Fix files with only comments on the flow parser (#598)
- Remove first line special case (#597)
- Fix single indented JSX comment (#596)
- Print dangling on ast on all the paths

# 0.15.0

[diff](https://github.com/prettier/prettier/compare/0.14.1...0.15.0)

- Fix syntax error in empty object with dangling comment (#533)
- Fix closing call expression commented out (#530)
- Update `bracketSpacing` comment to say it's about {} (#529)
- Add 0.14.1 to CHANGELOG (#525)
- Print \x and \u escapes in strings and regexes lowercase (#522)
- Fix Jetbrains example screenshot url. (#534)
- Preserve next line with trailing comment (#535)
- Break nested calls (#517)
- Update snapshot tests from conflicting PRs
- Reimplement MemberExpression printing (#469)
- Remove spurious test.js
- Fix small typo on Jetbrains section (#552)
- [JSX] Handle non-breaking space (#557)
- Make comments between if & else to look good (#544)
- Whitelist UnaryExpression for parentless objects (#545)
- Make comments inside of MemberExpression look good (#556)

# 0.14.1

[diff](https://github.com/prettier/prettier/compare/0.14.0...0.14.1)

- Fix range for object newline detection (#520)
  - a bugfix for "Keep expanded objects expanded" (#495)

# 0.14.0

[diff](https://github.com/prettier/prettier/compare/0.13.0...0.14.0)

- Only write to files if the change (#511)
- Remove extra group when printing object values (#502)
- Add documentation for JetBrains products. (#509)
- Don't print trailing commas for object destructuring and rest (#512)
- Mention eslint-config-prettier (#516)
- [RFC] Keep expanded objects expanded (#495)
- Do not always put an empty lines after directives (#505)
- Print numbers in a uniform way (#498)

# 0.13.0

[diff](https://github.com/prettier/prettier/compare/0.12.0...0.13.0)

- Simplify arrow functions that use blocks (#496)
- Properly print comments for BinaryExpression (#494)
- Preserve empty line after comment (#493)
- [JSX] Handle each line of text separately (#455)
- Proper support for dangling comments (#492)

# 0.12.0

[diff](https://github.com/prettier/prettier/compare/0.11.0...0.12.0)

- [WIP] Add rationale document (#372)
- Proper parenthesis for yield and await in conditional (#436)
- Don't print double newlines at EOF to stdout (#437)
- Explain the `--color` option in a comment (#434)
- Validate user-provided config with jest-validate (#301)
- Propagate breaks upwards automatically, introduce `breakParent` (#440)
- Fix typo in variable name (#441)
- Refactor traversal (#442)
- Do not put a newline on empty `{}` for functions (#447)
- Better error message for assertDoc (#449)
- Remove `multilineGroup` (#450)
- Ability to break on `:` for objects (#314)
- Update snapshots
- [RFC] Do not put spacing inside of arrays with bracketSpacing (#446)
- Fix integer CLI arguments (#452)
- Move tests around (#454)
- Update package.json, use ast-types 0.9.4 (#453)
- Update lockfile
- Support printing import("a") (#458)
- Explain that you can pass options to the spec runner (#460)
- Fix spurious whitespace (#463)
- Preserve new lines after directives (#464)
- Put decorators on the same line (#459)
- docs: add related projects (#456)
- Add break points for class declaration (#466)
- Added parens around in operator in for loops 🚀. (#468)
- CLI improvements (#478)
- [RFC] Hug Conditionals in JSX (#473)
- Refactor comment algorithm and improve newline/spaces detection (#482)
- Indent ternaries (#484)
- Indent computed member (#471)
- Maintain windows line ending (#472)
- Don't break up JSXOpeningElement if it only has a single text (#488)
- Add CallExpression to the last argument expansion whitelist (#470)
- Mention eslint-plugin-prettier in Related Projects (#490)
- Stop using conditionalGroup inside of UnionTypeAnnotation (#491)

# 0.11.0

[diff](https://github.com/prettier/prettier/compare/0.0.10...0.11.0)

Now using minor versions instead of patch versions for the releases.

- Swap quotes (#355)
- Drop jsesc (#357)
- Use a Symbol instead of the private dep (#359)
- Add parens for default export FunctionExpressions (#345)
- Fix export extension output (#361)
- Exit with an error if an unknown CLI option is passed (#365)
- Warn if using deprecated CLI options (#364)
- s/jscodefmt/prettier/ (#370)
- Fix CLI options (#369)
- Fix some parens cases for UpdateExpressions (#381)
- Output strings with the minimum amount of escaped quotes (#390)
- Ignore EmptyStatement inside of switch case (#391)
- Support multiple standalones in import (#389)
- Fix missing semi-colon in for loop and var body (#388)
- Fix empty labels (#383)
- Fix forced trailing comma (#382)
- Empty switch should not have an empty line (#384)
- add formatAST() for formatting ASTs directly (#393)
- Fix class extends parenthesis (#396)
- Fix class inside of binary expression missing parenthesis (#397)
- Fix parenthesis in object as left-hand-side of template (#398)
- Remove unneeded parens for FunctionExpression inside LogicalExpression (#399)
- Remove trailing comma for array destructuring with rest (#400)
- Fix +++x (#401)
- Also do the class extend parenthesis for class expressions (#403)
- Fix various parenthesis issues on the left side of template (#404)
- Fix in inside of the first group of a for (#406)
- Add parenthesis for arrow function inside of ternary (#408)
- Add parenthesis around class expression when left side of call expression (#409)
- Ensure computed method names don't lose quotes (#419)
- Add parenthesis for yield inside of a conditional (#418)
- Add parenthesis around assignment for arrow function body (#422)
- Add parenthesis around export default assignments (#423)
- Add parenthesis for class expression on left of member expression (#421)
- Fix missing parens around object in MemberExpression (#424)
- Re-run snapshot tests
- Workaround flow bug around trailing comma (#427)
- Add parenthesis when class expressions are left of a ternary (#428)
- Revert "Workaround flow bug around trailing comma" (#429)
- Update commands.md (#430)
- Improve vim integration section (#416)
- Add glob support to the CLI (#363)
- Use babel-code-frame for syntax errors (#367)
- Update yarn.lock

# 0.0.10

[diff](https://github.com/prettier/prettier/compare/0.0.9...0.0.10)

- Add description to package.json (#320)
- Fix crash for single rest on class declaration (#315)
- Add canonical link to Prettier SublimeText package. (#318)
- Properly escape JSXText (#329)
- Hug objects and arrays inside of JSXExpressionContainer (#213)
- Add quotes around unicode keys in flow parser (#328)
- Add tests for comments (#330)
- Print dangling comments in blocks (#331)
- Remove Printer module in favor of single function (#333)
- Split pp.js into doc-{printer,builders,utils}.js (#334)
- Fix node 4 (#336)
- Remove unused functions from recast (#337)
- Kill fromString (#335)
- Extract parser.js (#338)
- Normalize exports (#339)
- Refactor index.js (#340)
- Add semicolon to more default exports (#343)
- Introduce --parser/parser option and deprecate --flow-parser/useFlowParser (#342)
- Remove parens around AwaitExpression in ternary (#346)
- Indent while test the same way as if test (#352)
- Add debugging support for doc IR (#347)

# 0.0.9

[diff](https://github.com/prettier/prettier/compare/0.0.8...0.0.9)

- Workaround flow bug parsing astral unicode characters (#277)
- Allow specifying the major mode that `defun-before-save` will use. (#276
- Fix space missing before `,` on export with bracket spacing off (#278)
- Fix space missing before `,` on import with bracket spacing off (#279)
- Add newline after shebang if necessary. (#215)
- Remove +1 from newline detection (#261)
- Fix path when printing member chains so parens work properly (fixes #243
- Ensure parens on NewExpression with function callee (#282)
- Fix missing semi when default exporting CallExpression (#287)
- Workaround flow parser bug with spread in arrays (#285)
- Update flow-parser to 0.38 (#290)
- Allow customizing args sent to prettier-command (#289)
- Do not output trailing commas with rest arguments (#283)
- Use exact versions in package.json (#291)
- Use js native String.repeat() (#293)
- Handle additional export default parens cases (#298)
- Fix parens around anonymous functions (#297)
- Introduce second argument to ifBreak (#302)
- Fix bracketSpacing typo in tests (#299)
- Remove unused variable (#304)
- Fix trailing whitespace (#300)
- add version flag (#294)
- Add --run-in-band to travis (#306)
- [JSX] Split elements on newlines and preserve whitespace (w/@yamafaktory) (#234)
- Print binary and logical expressions in a nicer format (#262)

# 0.0.8

[diff](https://github.com/prettier/prettier/compare/e447971...0192d58)

- Fix await parenthesis (#185)
- Add note about Sublime Test github issue in readme
- Remove legacy Recast code and simplify API. (#191)
- Don't break to new line if logical/loop statements are without brackets. (#194)
- Fix parenthesis for UpdateExpression (#198)
- Fix directives printing for empty functions (#199)
- Fix key quotes omission for flow parser (#203)
- Fix comma when an arrow function with no arguments breaks (#210)
- Last argument expansion works for arrow functions that return JSX (#211)
- Remove faulty location check on template literals that throws in Nuclide (#218)
- Add flow parser experimental options (#221)
- Fix empty exports (#225)
- Fix cases of missing parens with NewExpression (#230)
- Fix issue with ArrowFunctionExpression parens (#236)
- Add npm version badge (#240)
- Consolidate badges in readme
- Fix parens issue with nested UnaryExpressions (#237)
- Escape strings using jsesc (#229)
- Add newline for empty blocks {} (#205)
- Fix empty export with from clause (#248)
- Fix missing parenthesis for typeof and arrow functions (#249)
- Fix FunctionExpression parens issues (#250)
- Fix last element of an array being null (#232)
- Make sure empty for loops generate valid code (#224)
- Fix parens for functions inside TaggedTemplateExpression (#259)
- Preserve the way numbers were written (#257)

# 0.0.7

[diff](https://github.com/prettier/prettier/compare/7e31610...6f5df0e)

- Update live editor to 0.0.6
- Adds various prettier-browser changes (#175)
- Fix `[(0)]` (#179)
- Do not advance for forward skipSpaces (#176)
- Fix windows line-endings (#177)
- add license to package.json (#178)
- Fix exponent in babylon (#181)
- Make `declare type` consistent between babylon and flow (#183)
- Fix DeclareInterface (#182)
- Change test to workaround babylon bug (#184)

# 0.0.6

[diff](https://github.com/prettier/prettier/compare/faed09c...3af7da5)

- Format property names consistently
- remove node 0.10 from travis config, add travis badge to readme
- Update snapshots
- chore: link prettier package to its github project
- add gitter badge to readme
- add instructions for Visual Studio plugin
- Do not unquote string properties
- Add prettier-browser
- v0.0.5 -- Accidentally didn't push this commit out before others landed; 0.0.5 is actually based on commit faed09c
- update yarn.lock
- remove recast (not used)
- Always use double quotes for JSX and properly escape
- remove unused recast ref
- Fix typo in README.
- Support type annotation for rest argument on babylon parser
- Use `setq` instead of `infc` and `decf`
- Add title and encoding to the REPL
- Fix old name reference in tests_config
- Minimize string escapes
- Support method generics on babylon parser
- Break long `exports` into multiple lines.
- Use group instead of conditionalGroup
- Fix misprinting of computed properties in method chains. (#157)
- treat shebang outside of parsing (#137)
- Break multiline imports (#167)
- Do not put spaces on empty for loop (#169)
- Add trailing comma support for multiline exports (#168)
- Update run_spec to support options
- Add tests for bracketSpacing option
- Add tests for quotes option
- Add tests for tabWidth option
- Add tests for trailingComma option
- Fix for Node 4
- Add test for shebang and move to index.js (#170)
- Numeric literal callees should keep parens (#141)
- Remove leftover `arrowParensAlways` option (#171)
- Wrap Stateless JSX Arrow Functions and Assignment in Parens (fixes part of #73)
- Break JSXOpeningElement between attributes (fixes #15)
- JSX maintains spaces that matter (fixes #30 and thus part of #73)
- Multiline JSX opening tag breaks children out too (for #73)
- Add regression tests for long JSX Expression contents
- include index.js in format:all script (#132)
- Wrap ForStatement in a block for const decls (#172)
- Reprint all the files!
