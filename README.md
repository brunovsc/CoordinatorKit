# CoordinatorKit 
[![codecov](https://codecov.io/gh/bocato/CoordinatorKit/branch/master/graph/badge.svg)](https://codecov.io/gh/bocato/CoordinatorKit)
[![CodeFactor](https://www.codefactor.io/repository/github/bocato/coordinatorkit/badge)](https://www.codefactor.io/repository/github/bocato/coordinatorkit)

Coordinator pattern extended.

## Instalation

Add this to Cartfile:

`git "https://github.com/bocato/CoordinatorKit.git" ~> 1.0`

Then:

`$ carthage update`

## Usage

### Without Events

- Simply conform with `CoordinatorType`.

Example:
```swift
final class MyCoordinator: CoordinatorType {

    // MARK: - Properties
    
    // Don't forget to define the delegates as weak!
    weak var coordinatorDelegate: CoordinatorDelegate? 
    var parent: CoordinatorType?
    var children: [CoordinatorType]? = []
    
    // MARK: - Methods
    
    func start() {
      // Do something...
    }
    
}
```

### Event Driven

- Conform with `CoordinatorType` and `EventReceivingCoordinator`, implementing `receiveEventFromParent(:)` and `receiveEvent(:from:)`.

Example:

```swift
final class MyCoordinator: CoordinatorType, EventReceivingCoordinator {

    // MARK: - Properties
    
    // Don't forget to define the delegates as weak!
    weak var coordinatorDelegate: CoordinatorDelegate? 
    var parent: CoordinatorType?
    var children: [CoordinatorType]? = []
    
    // MARK: - Methods
    
    func start() {
      // Do something...
    }
    
    // MARK: - Event Handlers
    
    func receiveEvent(_ event: CoordinatorEvent, from child: CoordinatorType) {
      switch (event, child) {
        case let (event as SomeCoordinator.Event, child as SomeCoordinator):
          switch event {
            case .someEvent:
              // Do something with the event...
              sendEventToParent(event) // Pass it on if needed...
          }
          default: 
            return
      }
    }
    
    func receiveEventFromParent(_ event: CoordinatorEvent) {
      switch (event) {
      case .someEvent:
        // Do something with the event...
      }
    }
    
}
