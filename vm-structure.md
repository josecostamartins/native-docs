There is a standard template we use for our view models in both Java and Swift. This helps
use not worry about stylistc considerations, so as where certain methods are placed.

## Java

## Swift

```swift
// Alphabetized list of imports
import Models
import ReactiveCocoa
import Result

internal protocol ViewModelInputs {
  // Alphabetized list of input functions with documentation
  
  /// Call with the project supplied to the view.
  func configureWith(project project: Project)
  
  /// Call when the view loads.
  func viewDidLoad()
}

internal protocol ViewModelOutputs {
  // Alphabetized list of output signals with documentation
  
  /// Emits the creator's name.
  var creatorName: Signal<String, NoError> { get }
}

internal protocol ViewModelType {
  var inputs: ViewModelInputs { get }
  var outputs: ViewModelOutputs { get }
}

internal final class ViewModel: ViewModelType, ViewModelInputs, ViewModelOutputs {

  // Constructor of the view model at the top.
  
  internal init() {
    // Assign all outputs in terms of the inputs
   
    self.creatorName = self.projectProperty.ignoreNil().map { $0.creator.name }
  }
  
  // Implementation of interfaces at the bottom of the view model.
  
  // Decleration of all input functions and the `MutableProperty`s that back them.
  
  private let projectProperty = MutableProperty<Project?>(nil)
  internal func configureWith(project project: Project) {
    self.projectProperty.value = project
  }
  private let viewDidLoadProperty = MutableProperty()
  internal func viewDidLoad() {
    self.viewDidLoadProperty.value = ()
  }
  
  // Decleration of all output signals
  
  internal let creatorName: Signal<String, NoError> { get }
  
  // Decleration of inputs/outputs
  
  internal var inputs: ViewModelInputs { return self }
  internal var outputs: ViewModelOutputs { return self }
}

// Private helper methods (optional)
private func helper() -> String {
  return "Hello"
}
```

Some notes:

* It is acceptable to omit spaces between input functions and mutable properties.
* 