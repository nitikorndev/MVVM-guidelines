
# MVVM Guidelines

* [Inputs - Outputs](#inputs---outputs)
  * [Principles](#principles)
  * [How-to](#how-to)
  * [Why Inputs - Outputs?](#why-inputs--outputs)
  * [Example](#example)
  * [Reference](#reference)

## Inputs - Outputs

#### Principles

inputs is a set of actions and events that have impacts on viewModel such as the tap action on a button, or the viewDidLoad event.
outputs represents changes that views should reflect.
Since ouputs may change over time, it’s best to return an Observable (in RxSwift context) for each ouput.
Behaviors defined in inputs should not be expressed as Variable because we don’t need the inputs to be obseravable.

#### How to

```swift
protocol LoginViewModelInputs {
	var viewDidLoad: PublishRelay<Void> { get }
	var tapSubmit: PublishRelay<Void> { get }
}

protocol LoginViewModelOutputs {
	var validInput: Driver<Bool> { get }
	var isLoading: Driver<Bool> { get }
}

protocol LoginViewModelType {
	var inputs: LoginViewModelInputs { get }
	var ouputs: LoginViewModelOutputs { get }
}
```

This is what LoginViewModel looks like:

```swift
final class LoginViewModel: LoginViewModelType, LoginViewModelInputs, LoginViewModelOutputs {
	var inputs: LoginViewModelInputs { return self }
	var ouputs: LoginViewModelOutputs { return self }

	// MARK: - Inputs
	var viewDidLoad: PublishRelay<Void>
	var tapSubmit: PublishRelay<Void>
	
	// MARK: - Ouputs
	var validInput: Driver<Bool>
	var isLoading: Driver<Bool>
	init() {

	}
}
```

#### Why Inputs / Outputs?

First of all, by using protocols like this, we achieve higher level of abstraction. Therefore, our code is more behavior-oriented and easier to test.

Another advantage of this protocol-based convention is readability in unit tests. 
By looking at the codes related to inputs calls, we quickly have a sense of the scenarios we are trying to simulate. Similarly, what we expect to see are reflected upon outputs.

## Naming

Descriptive and consistent naming makes software easier to read and understand. 

#### Inputs use events name instead of command sentences

**Preferred:**
```swift
protocol LoginViewModelInputs {
	var viewDidLoad: PublishRelay<Void> { get }
	var tapSubmit: PublishRelay<Void> { get }
}
```

**Not Preferred:**
```swift
	var fetchItems: PublishRelay<Void> { get }
	var submitItem: PublishRelay<Void> { get }
```

#### Outputs use status that represent data for instead of what is  data meaning

**Preferred:**
```swift
protocol LoginViewModelOutputs {
	var isHideLeftMenu: Driver<Bool> { get }
}
```

**Not Preferred:**
```swift
	var isGuest: Driver<Bool> { get }
```

#### After binding/subscribe alway add dispose to dispose bag

**Preferred:**
```swift
...
.bind(to: viewModel.input.didBecomeActive)
.disposed(by: bag)
```

**Not Preferred:**
```swift
...
.bind(to: viewModel.input.didBecomeActive)
```

#### Binding method

**Preferred:**
```swift
private func bindInputs() {
private func bindOutputs() {
```

**Not Preferred:**
```swift
private func configure() {
```

#### Use Dependency Injection for use-cases to view models

**Preferred:**
```swift
init(provider: Domain.UseCaseProvider) {
	let useCase = provider.makeHomeScreenUseCase()
}
	OR
init(provider: Domain.UseCaseProvider) {
	let useCase = provider.makeHomeScreenUseCase()
	let network = provider.network
}
	PS. Depends on Project and Team
```

**Not Preferred:**
```swift
init() {
	let useCase = UseCase()
}

init(network: Network) {
}
```

#### Use Enum to represent Image named / color

**Preferred:**
```swift

enum LeftImage: String {
	case discount = "image1"
	case BOGO = "image2"
}

protocol LoginViewModelOutputs {
	var leftImage: Driver<LeftImage> { get }
}
```

**Not Preferred:**
```swift
protocol LoginViewModelOutputs {
	var leftImage: Driver<String> { get }
	var headerBarColor: Driver<BarColor> { get }
}
```

#### NOT provine the hold model / ONLY necessary data

**Preferred:**
```swift
protocol LoginViewModelOutputs {
	var fullName: Driver<String> { get }
}
```

**Not Preferred:**
```swift
protocol LoginViewModelOutputs {
	var person: Driver<Person> { get }
}
```

#### Present screens with coordinator

**Preferred:**
```swift
init(coordinator: Coordinable, useCaseProvider: Domain.UseCaseProvider){
	let viewModel = SignUpViewModel(coordinator: coordinator)
	let scene: OnboardScene = .signUp(viewModel)
	coordinator.transition(.push(scene: scene, animated: true))

```

#### Example Code
https://bitbucket.org/appsynth/day1/src/master/

#### Clean architecture with  [RxSwift](https://github.com/ReactiveX/RxSwift)
https://github.com/sergdort/CleanArchitectureRxSwift

#### Reference

[1] https://github.com/kickstarter/native-docs/blob/master/vm-structure.md
[2] https://github.com/kickstarter/native-docs/blob/master/inputs-outputs.md

