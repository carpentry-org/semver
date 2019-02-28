# Semver

Semantic versioning for Carp.

## Installation

You can obtain this library like so:

```clojure
(load "git@github.com:carpentry-org/semver@0.0.1")
```

## Usage

Semantic versions can be parsed using `Semver.from-string`. This will return a
`Maybe`—a `Just` on a successful parse, and `Nothing` if the parser fails.

```clojure
(Semver.from-string "1.2.3-mytag") ; => will return a (Just Semver)
; or, alternatively
(Semver.init 1 2 3 "-mytag")
```

You can then compare them using normal arithmetic comparison. The tags are
disregarded everywhere, except when checking equality.

<hr/>

Have fun!
