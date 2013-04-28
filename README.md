# RxHaskell: Practical FRP for Haskell

Functional reactive programming (FRP) is an powerful paradigm for writing
code that reacts to change, and has been gaining significant traction as a way
to implement GUI applications with minimal state. Although Haskell has several
existing FRP libraries, they can be overly complex, and difficult to integrate
with native UI frameworks.

This talk introduces RxHaskell (http://hackage.haskell.org/package/RxHaskell),
an FRP library inspired by Microsoft's Reactive Extensions for .NET
(http://msdn.microsoft.com/en-us/library/hh242985(v=VS.103).aspx) and designed
specifically with GUI programming in mind.

Compared to existing solutions, RxHaskell focuses more on ease-of-use and
practicality than absolute conceptual purity. We'll see that side effects are
particularly easy to capture, making it straightforward to interleave UI updates
and foreign function calls in otherwise pure code.

With the help of the ReactiveCocoa framework in Objective-C, we'll also take
a brief look at how a parallel Rx implementation in foreign code can be used to
build an FFI that lives outside of the IO monad, even for side-effecting calls.
