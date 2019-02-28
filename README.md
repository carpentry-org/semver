# Semver

Semantic versioning for Carp.

## Installation

You can obtain this library like so:

```clojure
(load "git@github.com:carpentry-org/semver@0.0.1")
```

## Usage

Semantic versions can be parsed using `Semver.from-string`.

```clojure
(Semver.from-string "1.2.3-mytag")
; or, alternatively
(Semver.init 1 2 3 "-mytag")
```

You can then compare them using normal arithmetic comparison. The tags are
disregarded everywhere, except when checking equality.

For the time being, errors will be caught by `assert` errors. As soon as
sumtypes hit the Carp master branch, this library will use the `Maybe` type for
`from-string`.

<hr/>

Have fun!
