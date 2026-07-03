# Semver

Semantic versioning for Carp.

## Installation

You can obtain this library like so:

```clojure
(load "git@github.com:carpentry-org/semver@0.0.6")
```

## Usage

Semantic versions can be parsed using `Semver.from-string`.

```clojure
(Semver.from-string "1.2.3-mytag") ; returns a Maybe
; or, alternatively
(Semver.init 1 2 3 (Maybe.Just @"-mytag"))
```

You can then compare them using normal arithmetic comparison (`<`, `>`, `<=`,
`>=`), which follows [SemVer 2.0.0](https://semver.org) precedence: major,
minor, and patch are compared numerically, and when they are equal the
pre-release tag decides. A pre-release such as `1.0.0-rc.1` ranks below the
corresponding release `1.0.0`; pre-release identifiers are compared field by
field (numeric identifiers numerically, and below alphanumeric ones); and build
metadata is ignored. Equality (`=`) remains structural and compares the tag
verbatim.

## Version ranges

A `Range` is a version requirement of the kind found in package manifests. Parse
one with `Range.from-string` and test a version with `Range.satisfies?`.

```clojure
(match (Range.from-string "^1.2.3")
  (Maybe.Just r)
    (Range.satisfies? &r &(Semver.init 1 4 0 (Maybe.Nothing))) ; => true
  (Maybe.Nothing) false)
```

`Range.from-string` understands a practical subset of the npm/node-semver
grammar:

| Form                | Example            | Meaning                          |
|---------------------|--------------------|----------------------------------|
| comparators         | `>=1.2.3 <2.0.0`   | whitespace is `AND`              |
| exact               | `1.2.3`            | same as `=1.2.3`                 |
| caret               | `^1.2.3`           | `>=1.2.3 <2.0.0`                 |
| tilde               | `~1.2.3`           | `>=1.2.3 <1.3.0`                 |
| x-ranges / partials | `1.2.x`, `1.x`, `*`| wildcard components              |
| hyphen              | `1.2.3 - 2.3.4`    | inclusive on both ends           |
| alternatives        | `^1.0.0 \|\| ^2.0.0` | `\|\|` is `OR`                 |

Comparisons follow SemVer 2.0.0 precedence. Following node-semver's default, a
pre-release version such as `1.2.3-beta` only satisfies a range when some
comparator explicitly opts into a pre-release at the same `major.minor.patch`,
so `2.0.0-beta` does not satisfy `^1.2.3`. `Range.str` renders a range back to a
normalized comparator form.

## Resolving versions

Given a set of available versions, `Range.max-satisfying` and
`Range.min-satisfying` pick the highest and lowest version that satisfies a
range — the core operation a dependency resolver performs. Both return a
`Maybe`, since nothing may match.

```clojure
(let [available [(Semver.init 1 2 0 (Maybe.Nothing))
                 (Semver.init 1 4 2 (Maybe.Nothing))
                 (Semver.init 2 0 0 (Maybe.Nothing))]]
  (match (Range.from-string "^1.2.0")
    (Maybe.Just r) (Range.max-satisfying &r &available) ; => (Just 1.4.2)
    (Maybe.Nothing) (Maybe.Nothing)))
```

`Semver.compare` gives an explicit three-way comparison (`-1`/`0`/`1`), and
because `Semver` implements `<`/`>` an array of versions can be ordered directly
with `Array.sorted`.

<hr/>

Have fun!
