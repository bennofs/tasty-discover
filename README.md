[![Build Status](https://travis-ci.org/lwm/tasty-discover.svg?branch=master)](https://travis-ci.org/lwm/tasty-discover)

# tasty-discover
Automatic test discovery and runner for the [tasty framework][tasty-framework].

### Status
So far, this package is only a hackage candidate. I'd like to see that change
shortly but for now, to use this package in your projects, you'll need to get
it from Github. See the [installation section][install-discover] for more
details.

### Example project
See the [example project](https://github.com/lwm/tasty-discover/tree/master/tasty-discover-example).

### Integration
If you are using [stack][stack-haskell], you can add the following to your `stack.yaml`:

```
packages:
- '.'
- location:
    git: "https://github.com/lwm/tasty-discover.git"
    commit: "HEAD"
```

Note. `tasty-discover` relies on `tasty-th` version `0.1.4`, so you may need to
explicitly set this in your `stack.yaml` like so:

```
extra-deps:
- "tasty-th-0.1.4"
```

### Getting started
`tasty-discover` expects the following conventions:

  - Cabal test suite `main-is` must point to a file with the necessary GHC preprocessor line. ([example](https://github.com/lwm/tasty-discover/blob/master/tasty-discover-example/test/Tasty.hs))
  - Test files ending with `Test.hs`
  - Test cases starting with either `prop_`, `case_` (related to [tasty-th usage](https://github.com/bennofs/tasty-th#usage))
    - It is also possible to pass `test_` for test groups

In order to get started, you need to set up your test suite with `Cabal`.
Add the following to your cabal file:

```
test-suite your-test-suite-name
  type:                exitcode-stdio-1.0
  hs-source-dirs:      test  -- your test directory
  main-is:             Tasty.hs  -- your `tasty-discover` pre processor
  other-modules:       XTest, YTest, Folder.ZTest -- your test files
  build-depends:       base, tasty-discover  -- your dependencies
  default-language:    Haskell2010
```

Create a `test` folder, and place a file in it called `Tasty.hs`.
Inside this file, add the following:

```
{-# OPTIONS_GHC -F -pgmF tasty-discover #-}
```

Create a test file inside `test/`. `tasty-discover` can detect test files in
whatever directory structure you choose (as long as they sit under your
`hs-source-dirs`), so there is no restrictions on directory organization.
However, you must end your test file names with `Test.hs`. So, for example,
`EatYourLandlordTest.hs` would be a perfectly acceptable test file name.

`tasty-discover` exports the `tasty-quickcheck` and `tasty-hunit` modules, so
we can simple write the following in `test/EatYourLandlordTest.hs`:

```
module EatYourLandlordTest where

import Test.Tasty.Discover

prop_length_append :: [Int] -> [Int] -> Bool
prop_length_append as bs = length (as ++ bs) == length as + length bs

case_length_of_one :: Assertion
case_length_of_one = 1 @=? length [()]
```

Now, we can run our tests with:

```
$ stack test
```

### Contributing
Pull requests are very welcome! Please submit an issue so we can discuss what
you would like to do. I test any changes within the `tasty-discover-example`
directory, which has its own `.cabal` and `stack.yaml` file. Please add some
tests there if you want to add functionality. Also, see below for some
things that could be worked on immediately.

### TODO
  - [ ] Solve current issues
  - [ ] Make a full release to Hackage/Stackage (currently candidate)

### Other Haskellers made this possible
This project borrows / is heavily influenced from the awesome work of the
people involved with these projects:

  - [tasty][tasty-framework]
  - [tasty-th][tasty-th]
  - [hspec][hspec]

### Related documentation
  - [hspec-discover documentation][hspec-discover]
  - [tasty-th documentation][tasty-th-docs]

[issues]: https://github.com/lwm/tasty-discover/issues
[stack-haskell]: https://github.com/commercialhaskell/stack
[tasty-th-docs]: https://github.com/bennofs/tasty-th#usage
[tasty-th]: https://github.com/bennofs/tasty-th
[hspec]: https://github.com/hspec/hspec
[hspec-discover]: https://hspec.github.io/hspec-discover.html
[install-discover]: https://github.com/lwm/tasty-discover#installation
[tasty-framework]: https://github.com/feuerbach/tasty
