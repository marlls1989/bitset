bitset-ng  [![Build Status][travis-img]][travis]
======

This is a portable fork of Sergei Lebedev bitset package.
It strips the GHC version-dependent GMP optimisations.

A _bit set_ is a compact data structure, which maintains a set of members
from a type that can be enumerated (i. e. has an `Enum` instance). Current
implementation is abstract with respect to conatiner type, so any
numeric type with `Bits` instance can be used as a container.

Here's a usage example for a dynamic bit set, which uses a tweaked version
of `Integer` for storing bits:

```haskell
import Data.BitSet (BitSet, (\\))
import qualified Data.BitSet as BitSet

data Color = Red | Green | Blue deriving (Show, Enum)

main :: IO ()
main = print $ bs \\ BitSet.singleton Blue where
  bs :: BitSet Color
  bs = BitSet.fromList [Red, Green, Blue]
```

The API is exactly the same for a static bitset, based on native `Word`.
Here's an example from [`hen`] [hen] library, which uses `Data.BitSet` to
store Xen domain status flags:

```haskell
import qualified Data.BitSet.Word as BS

data DomainFlag = Dying
                | Crashed
                | Shutdown
                | Paused
                | Blocked
                | Running
                | HVM
                | Debugged
    deriving (Enum, Show)

isAlive :: DomainFlag -> Bool
isAlive = not . BS.null . BS.intersect (BS.fromList [Crashed, Shutdown])
```

Benchmarks
----------

To run benchmarks, configure `cabal` with benchmarks
and build:

```bash
$ stack bench --ba "-o benchmarks/Benchmarks.html"
```

[travis]: https://travis-ci.org/marlls1989/bitset-ng
[travis-img]: https://secure.travis-ci.org/marlls1989/bitset-ng.png
[hen]: https://github.com/selectel/hen/
