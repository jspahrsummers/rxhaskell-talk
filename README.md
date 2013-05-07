# Effectful Functional Reactive Programming with RxHaskell

Functional reactive programming (FRP) is a powerful paradigm for writing
code that reacts to change, and has been gaining significant traction as a way
to implement GUI applications with minimal state.

Haskell has a few existing FRP libraries that are commonly used for GUI
programming; however, all of them currently target cross-platform widget
toolkits, like GTK+ or wxWidgets. Native frameworks (like Cocoa or WPF) have
remained highly imperative, and out of place in the FRP model.

This talk introduces RxHaskell (http://hackage.haskell.org/package/RxHaskell) –
a library for functional reactive programming inspired by Microsoft's Reactive Extensions for .NET
(http://msdn.microsoft.com/en-us/library/hh242985(v=VS.103).aspx) and designed
specifically for integration with native frameworks.

Rx and RxHaskell simplify the traditional FRP model by omitting continuous behaviors, and
discarding values after they've been processed (which enables switching without time
leaks). These changes make it easier to interleave impure code.

And because native frameworks often require UI changes to be made on a dedicated
OS thread, RxHaskell provides a "scheduler" abstraction which uses Haskell's
type system to offer static guarantees about concurrency. Functions can be
annotated as running on the main thread or in the background, and an action of
one type cannot be directly executed from the other, preventing some classes of
threading error.

We'll also take a brief look at how RxHaskell can be bridged to ReactiveCocoa,
an Rx implementation in Objective-C, to break down the barriers between two very
different programming languages, and support amazing native development without
giving way to completely imperative programming.
