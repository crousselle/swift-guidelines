# Purpose 

Provide a usefull ressource to any Swift coders in the team, that will allow our Swift codebase to be consistent, easy to read and ultimately easy to maintain.
Swift is a very permissive language, and the objective of this document is to provide a sense of rigor, clarity to our codebase. 


# Variables

## Always prefer `let` to `var`.
We should only use `var` if we know for sure that the value might change. This results in safer and clearer code, as a `let` binding clearly states that its value will never change.


## Use type infering as much as possible for constants
Whenever possible, `let myObject = "A String"` rather than `let myObject: String = "A String".
We should still explicitely declare `var` objects

## Avoid using implicitely unwrapped optionals

`let foo: Foo?` is always gonna be safer than `let foo: Foo!` and will not limit the use of the object in any case.

## Use optional binding and avoid force unwrapping

If an object `foo` is of type `BOXType?` or `BOXType!`, we should avoid as much as possible to force unwrap the underlying value (`foo!`).
Instead, prefer using 
```
if let unwrappedFoo = foo {
	// I can proceed with my unwrapped value
} else {
	// I can handle the case where foo is nil
}
```

In some cases, using optional chaining is also valid : `foo?.doSomething()` is safe

Force unwrapping can quickly lead to crashes throughout the codebase and can be easily avoided.

**Note**: Avoid nested optional bindings and prefer using mutiple per lines when necessary :

```
// Don't 
if let unwrappedFoo = foo {
	if let unwrappedBar = bar {
		if let unwrappedBox = box {
			// my code
		}
	}
}

// Do
if let unwrappedFoo = foo, unwrappedBar = bar, unwrappedbox = box {
	// my code
}

```

## Type Arrays whenever possible

Using `private var points: [CGPoints]?` will always result in cleaner code than `private var points: Array?`

## Variables and parameters declarations should be followed by a whitespace
`private var foo: Foo?`

#Classes structure and properties

## Access properties and values with `self`

It's all about readability. Self make intent more explicit, and distinction between class properties an local variables easier. 
Additionally, using `self` is mandatory in a closure, which causes confusion :

```
private var foo: Foo?

private func myMethod() {
	
	// This works but access to the property is not consistent and goes against readability
	foo?.use()
	let myClosure = {
		self.foo?.useAgain()
	}

	// This works and is more readable.
	self.foo?.use()
	let myClosure = {
		self.foo?.useAgain()
	}	
}

```

## Use inferred return types whenever possible

`private func myFunction()` does not need to be `private func myFunction() -> Void`

## Use explicitely specify access control for properties and methods

Since by default any method of property will be exposed in the whole current Swift module, access control needs to be done manually : 
`private var foo: Foo?` vs `var foo: Foo?`
`private func myMethod()` vs `func myMethod`

## Protocol implementation should be made via extensions

```
protocol MyProtocol  {
	// Protocol declaration
}

class Foo {
	// Class Implementation
}

extension Foo: MyProtocol {
	// Protocol implementation
}
```

This makes protocol implementations more explicit, easy to find and grouped after the class implementation.

## Overall class structure

Classes should be organized as follow whenever possible : 

- Properties (`var`)
- Lazy properties (`lazy var`)
- Initializers
- `public` Methods 
- `private` Methods 
- Protocol extensions


## Prefer `Struct` over `Class`

Unless you're in need functionality that can only be provided by a class, like idendity or deinitializers, implement a struc instead.
Value types are usually easier to deal with and simpler to use.

## Do not use global access modifiers on Struct

Swift's behavior is confusing and not consistent between `Class` and `Struct` for global access modifiers
- In a `public Class`, all methods will default to `internal`
- In a `public Struct`, all methods will default to `public`

By no using global access modifiers in `Struct`, we get the same default behavior as `Class`.

## Don't include `@objc` if implicit

Situations such as Objective C class inheritance.

# Code Style

## Break / indent

In swift, there is no colon align. We should not fight the formatter, but should still break long lines even if not indented.

## Function bracket 

Should always be on the same line as the method declaration 
```
private func myMethod() {
	// Stuff happens
}
```

## `guard` and `defer`

`guard` and `defer` should always be used in early method implementation. 
If a `guard` or `defer` needs to be used mid method implementation, this is probably a sign that the method needs to be simplified and broken down.

`guard` and `defer` can use early `return` whenever required.

## Early returns

Our experience with Swift is that the way coding is done is slightly different than in Objective C and that early `return` do not feel very natural in Swift. 
We should not enforce `return` location.

*// Probably need a bit more here, but I'm having a hard time wording what we expressed in our meeting.*

## Documentation

Still as important as in Objective-C and should be used as much as possible.

```
/** 
	Some Documentation
	- Note: Some note
	- Warning: Some warning 
	- Parameter: 
	- Return: 
*/
private func myMethod() {
	// Stuff happens
}
```




























