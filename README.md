# TypeScript Composite Project Build Bug Repro

TypeScript version: 4.0.5

Run `yarn tsc -b --verbose` multiple times, despite no file change, `b` project will be always be rebuilt.

## Overview

Three projects in this repo: `a`, `b`, and `c`. They are configured to only emit `d.ts` files to
`a_out`, `b_out`, and `c_out` respectively. With project reference, `c` depends on `b` depends on `a`.

`b/doubly_included.ts` is included in both `a` project and `b` project. To allow that, `a`'s rootDir
is set to `..` to allow referencing source files in its sibling folder `b`.

Run `yarn tsc -b` to populate `a_out`, `b_out`, and `c_out`. Notice that there is no `b_out/b.d.ts`,
but there is `a_out/b/doubly_included.d.ts`. `b` is also rebuilt despite no file change with the
reason reported in the verbose mode:

```
Project 'b/tsconfig.json' is out of date because output file 'b_out/doubly_included.d.ts' does not exist
```
