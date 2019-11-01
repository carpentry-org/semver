# Semver

Semantic versioning for Carp.

## Installation

You can obtain this library like so:

```clojure
(load "git@github.com:carpentry-org/semver@0.0.3")
```

## Usage

Semantic versions can be parsed using `Semver.from-string`.

```clojure
(Semver.from-string "1.2.3-mytag") ; returns a Maybe
; or, alternatively
(Semver.init 1 2 3 "-mytag")
```

You can then compare them using normal arithmetic comparison. The tags are
disregarded everywhere, except when checking equality.

<hr/>

Have fun!
