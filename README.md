# EUDIW - iOS Library for Presentation Exchange V2.0.0

:heavy_exclamation_mark: **Important!** Before you proceed, please read
the [EUDI Wallet Reference Implementation project description](https://github.com/eu-digital-identity-wallet/.github/blob/main/profile/reference-implementation.md)

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)

## In a nutshell

[Presentation Exchange v2](https://identity.foundation/presentation-exchange/spec/v2.0.0/) is a specification that defines:


* A way for the `Verifier` to describe proof requirements in terms of `PresentationDefintion` object
* A way for the `Holder` to describe submissions of proofs that allign with those requirements in terms of a `PresentationSubmission`

The use of this specification is mandatory by OpenID4VP

## Library functionality

* As a `Verifier` be able to
  * produce a valid `PresentationDefinition` in order to be communicated to a `Holder` using a protocol like `OpenID4VP`
  * decide whether  a given `PresentationSubmission` satisfies a specific `PresentationDefinition`

* As a `Holder/Wallet` be able to
  * parse/validate a `PresentationDefition`
  * to check if a claim stored in the wallet satisfies a `PresentationDefinition`
  * to produce a `PresentationSubmission` given a valid `PresentationDefintion` and a matching `Claim`

### Presentation Exchange optional features supported

| Feature                      | Status |
|------------------------------|--------|
| Submission requirement       | ❌      |
| Predicate                    | ❌      |
| Relational constraint        | ❌      |
| Credential status constraint | ❌      |
| JSON-LD framing              | ❌      |
| Retention                    | ❌      |

## Usage

```swift
import PresentationExchange

let matcher = PresentationMatcher()
let presentationDefinition = ...
let claims = ...
    
let match = matcher.match(
  claims: claims,
  with: presentationDefinition
)

switch matched {
case .matched(let matches):
  ...
case .notMatched:
  ...
}
```

In the resources folder there are several
examples of `PresentationDefintion` JSON objects taken from directly from the
[specification](https://github.com/decentralized-identity/presentation-exchange/tree/main/test/v2.0.0/presentation-definition)

### Verifier: Produce a valid `PresentationDefinition`

Precondition:

* Verifier should know the data model of the claim(s)  that wants to be presented by the holder
* Verifier should be able to describe which formats (jwt, jwt_vc, ldp_vc etc.) and which algorithms is able to process

Library should offer a factory/builder to produce the `PresentationDefinition`.
The resulting `PresentationDefinition` should

* Adhere to the data model defined in the spec (JSON Schema validation)
* Contain valid JSONPath expressions

In order to create a presentation definition just instantiate the
[PresentationDefinition](src/main/kotlin/eu/europa/ec/euidw/prex/types.kt) data class
which enforces the syntactic a conditional rules as defined in the specification.

### Holder: Parse/Validate a `PresentationDefintion`

The holder should be able to verify that a JSON object is a syntactically valid `PresentationDefintion`:

* Adheres to the data model defined in the spec (JSON Schema validation)
* Contain valid JSONPath expressions

### Dependencies (to other libs)

* JSONSchema support: [JSON Schema](https://github.com/kylef/JSONSchema.swift)
* JSONPath support: [Sextant](https://github.com/KittyMac/Sextant.git)
* Lint support: [SwiftLint](https://github.com/realm/SwiftLint.git)
* JWS, JWE, and JWK support: [JOSESwift](https://github.com/airsidemobile/JOSESwift.git)
* Testing support: [Mockingbird](https://github.com/birdrides/mockingbird.git)

### References

* [Presentation Exchange v2](https://identity.foundation/presentation-exchange/spec/v2.0.0/)
* [JSON Schema of data model](https://github.com/decentralized-identity/presentation-exchange/tree/main/schemas/v2.0.0)
