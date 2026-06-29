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

<hr/>

Have fun!
