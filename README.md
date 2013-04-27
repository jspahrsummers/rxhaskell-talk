# RxHaskell: Practical FRP for Haskell

This talk will demonstrate the RxHaskell package
(http://hackage.haskell.org/package/RxHaskell) and show how it offers a powerful
– but easy to understand – approach to functional reactive programming, with
a specific focus on simplifying the development of GUI applications.

RxHaskell makes it very easy to interleave imperative logic when desired, which
we'll use to turn foreign function calls into lazy signals, and perform stateful
UI updates when changes occur. And since native platforms typically require the
use of a dedicated UI thread, we'll use Haskell's type system to avoid threading
errors.

With the help of the ReactiveCocoa framework, we'll also take a brief look
at a reactive FFI to Objective-C that can be used to build a completely native
UI with minimal use of the IO monad.
