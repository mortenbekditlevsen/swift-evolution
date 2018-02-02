# Make Standard Library Types Conditionally Hashable

* Proposal: [SE-NNNN](NNNN-stdlib-types-conditionally-hashable.md)
* Authors: [Morten Bek Ditlevsen](https://github.com/mortenbekditlevsen), [Karoy Lorentey](https://github.com/lorentey)
* Review Manager: TBD
* Status: **Awaiting implementation** <- Should this be 'implemented' when we have a proposed implementation?
* Implementation: [apple/swift#14247](https://github.com/apple/swift/pull/14247)

## Introduction

In Swift 4.1 conformance to `Hashable` can be synthesized by the compiler in case all of a type's members are `Hashable`. This synthesis removes a lot of boilerplate code and ensures that a correct implementation is used. But when types have members of generic types `Optional`, `Array` or `Dictionary` from the Standard Library, then the conformance can no longer be synthesized since currently these types do not conditionally conform to `Hashable`. To provide extra utility for the synthesis of `Hashable`, these Standard Library generics should conditionally conform to `Hashable`.

Swift-evolution thread: [Let Optional, Dictionary and Array conditionally conform to Hashable](https://forums.swift.org/t/let-optional-dictionary-and-array-conditionally-conform-to-hashable/)

## Motivation

The synthesis of `Hashable` conformance in Swift 4.1 removes both boilerplate code and the cognitive load of having to know how to implement a good hash value. Generating good hash values is not a trivial excersise - and it is easy to do it wrong. 

The synthesis can of course only be performed if all members of a type already conform to `Hashable`. So for instance if a type has an `Optional` member of another `Hashable` type, then it's `Hashable` conformance can only be synthesized in case `Optional` conditionally conforms to `Hashable`.

Having the need to implement conditional conformance for types like `Optional`, `Array` and `Dictionary` still requires the developer to know how to create good hash values, so the task of implementing this is just shifted from one place to another.

If, however, a number of Standard Library generic types already implemented conditional `Hashable` conformance, this would mean that in many common situations, the developer would not need to know the least bit about implementing good hash values.

Having these `Hashable` conformances implemented 'correctly' by the Standard Library would thus decrease the risk of implementing them wrong, which in turn would help Swift developers create better Swift code.

## Proposed solution

This proposal would add `Hashable` conformance to the following generic types in the Standard Library:

* `Optional`
* `Array`
* `ArraySlice`
* `ContiguousArray`
* `Dictionary`
* `Range`
* `Slice`
* `DictionaryLiteral`
* `CollectionOfOne`
* `EmptyCollection`


## Detailed design

Describe the design of the solution in detail. If it involves new
syntax in the language, show the additions and changes to the Swift
grammar. If it's a new API, show the full API and its documentation
comments detailing what it does. The detail in this section should be
sufficient for someone who is *not* one of the authors to be able to
reasonably implement the feature.

## Source compatibility

This is an additive change in the behavior of standard library types, so it should pose no source compatibility burden. 

## Effect on ABI stability and API resilience

Beyond an additional conformance for the types mentioned above, this proposal has no effect on ABI stability or API resilience.

## Alternatives considered

None.
