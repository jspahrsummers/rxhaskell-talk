**Why FRP**

    1. Developing a complex GUI application in a pure FP style is difficult
    1. "Events over time" is a great model for a GUI application
    1. FRP provides many of the same advantages as mutability
    1. FRP can effectively replace imperative programming at many of the same tasks

**Why Rx**

    1. RxHaskell is meant for easy integration with native frameworks, which are predominantly imperative
        - Want to develop 100% native GUI applications, not cross-platform GUIs
    1. Rx was originally built for .NET, alongside mostly impure code
        - Idioms have to consider the impact of side effects everywhere
        - Has to be easy to perform side effecting actions
    1. RxHaskell follows a similar design because of the success of Rx in that environment

**Events**

    1. Signal grammar: `NextEvent* (ErrorEvent | CompletedEvent)?`
    1. Past events are not retained
    1. Events are not tagged with time
        - Without past events, the information is useless
    1. No Behaviors
        - They map poorly to imperative code

**Signals**

    1. `Signal s v` sends nexts with a value of type v
        - Only `NextEvent`s are in the monad
    1. Signal events are received using a subscription
    1. "Cold" signals (the most common) only perform work upon subscription
        - The work may include side effects, like a foreign function call
    1. "Hot" signals are unaffected by subscription
    1. Subscriptions run until error or completed, but can also be disposed manually
        - Disposing of a subscription to a cold signal cancels the associated work

**Concurrency**

    1. A Scheduler is a FIFO serial queue of side-effecting actions
        - Similar to message passing, but just a work queue
    1. `BackgroundScheduler`s are associated with a dedicated background thread
    1. The `MainScheduler` should be associated with the native UI thread
    1. `SchedulerIO s v` is an `IO v` action parameterized by the kind of scheduler it must run on
        - Intensive processing work can be restricted to `BackgroundScheduler`s
        - UI changes can be restricted to the `MainScheduler`
    1. Most signal and subscription actions use `SchedulerIO` to restrict thread-hopping
        - Prevents background threads from subscribing to a `MainScheduler` signal (which could cause UI changes to run in the background)
        - Since multiple `BackgroundScheduler`s can be created, it's still possible for a single signal to be multithreaded, but events are always delivered serially

**Operators**

    1. Some familiar stream and list operators: `map`, `filter`, `take`, `drop`
    1. `materialize` brings all events into the monad
    1. `switch` keeps only one subscription to the signal last sent
        - Useful to completely change the behavior of a signal while terminating previous work
    1. `combine` joins the latest values from two input signals
        - Sorta like an uneven zip
        - Useful for algorithms that take multiple changing inputs (ex: form validation)
    1. `do…` and `finally` operators inject arbitrary side effects into subscriptions
        - Useful for performing UI updates as values are sent
    1. `subscribeOn` and `deliverOn` affect the concurrency of a signal
        - Start work on a `BackgroundScheduler`, deliver the results to the `MainScheduler` for a UI update

**Other signal types**

    1. Channels include a subscriber, and are like "mutable" signals
        - Channels are "hot" because new subscriptions have no side effects
    1. Replay channels can be used to preserve an event history for future subscribers
        - Useful when subscriptions could occur after all work has already finished
    1. Connections share one subscription to many subscribers
        - Can subscribe to the base signal lazily or eagerly
        - Built using some kind of channel, so the underlying subscription can be replayed if desired
    1. Commands are triggered by UI actions
        - Can also be conditionally disabled
        - A button bound to a command would be automatically enabled/disabled, and the signal would represent button presses
        - Background work (e.g. network requests) can also be attached to commands

**Bridging to ReactiveCocoa**

    1. ReactiveCocoa is another implementation of Rx, so its semantics are similar to RxHaskell's
    1. `RACSignal` can be treated like a `Signal` and vice-versa
    1. AppKit properties can be modeled as `Signal`s (via RAC)
    1. `Signal`s can be bound to AppKit properties (via RAC)
    1. Tasks that usually require `IO` glue code can be written purely instead
