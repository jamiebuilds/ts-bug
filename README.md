# ts-bug

Trying to compile the same module using `--esModuleInterop` with both
`commonjs` and `esnext` modules

---

With the import:

```ts
import fn from 'external';
```

And the library definition:

```ts
declare function fn(): void;
declare module "external" {
  export = fn;
}
```

This works:

```sh
$ tsc --esModuleInterop --module commonjs
```

This fails:

```sh
$ tsc --esModuleInterop --module esnext
```

With this error:

> ***error TS1192: Module '"external"' has no default export.***

---

You also cannot use `import fn = require('external')`:

> ***error TS1202: Import assignment cannot be used when targeting ECMAScript
> modules. Consider using 'import * as ns from "mod"', 'import {a} from "mod"',
> 'import d from "mod"', or another module format instead.***

Or `const fn = require('external')`:

> ***error TS2304: Cannot find name 'require'.***

---

But really the first should work because if you specify `esModuleInterop` and
esnext, TypeScript should assume your compiler will take care of it. Otherwise
why would you specify that option?
