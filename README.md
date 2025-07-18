### Why Fork?
To switch to v6
### result (exe at 2025-07-17)
#### succeed?
before(INSANE!!!:O_O:) 
```swift
Path: easy -> easy -> easy -> easy -> easy
        0日 -> +289.0日 -> +2078.0日 -> +12663.0日 -> +36500.0日
        -> Final State: S: 62591.37, D: 1.00, nextReview: 2166-09-22 09:20
```
after
```swift
Path: easy -> easy -> easy -> easy -> easy
        0日 -> +35.0日 -> +96.0日 -> +237.0日 -> +536.0日
        -> Final State: S: 536.19, D: 1.00, nextReview: 2028-01-20 11:29
```
# SwiftFSRS

[![](https://img.shields.io/endpoint?url=https%3A%2F%2Fswiftpackageindex.com%2Fapi%2Fpackages%2F4rays%2Fswift-fsrs%2Fbadge%3Ftype%3Dswift-versions)](https://swiftpackageindex.com/4rays/swift-fsrs) [![](https://img.shields.io/endpoint?url=https%3A%2F%2Fswiftpackageindex.com%2Fapi%2Fpackages%2F4rays%2Fswift-fsrs%2Fbadge%3Ftype%3Dplatforms)](https://swiftpackageindex.com/4rays/swift-fsrs)

An idiomatic and configurable Swift implementation of the [FSRS spaced repetition algorithm](https://github.com/open-spaced-repetition/fsrs4anki/wiki/The-Algorithm).

## Installation

Add the following to your `Package.swift` file:

```swift
dependencies: [
    .package(url: "https://github.com/4rays/swift-fsrs")
]
```

## Usage

### The Scheduler

The workhorse of the algorithm is the `Scheduler` protocol. It takes a card and a review and returns a new card and review log object.

Out of the box, the library ships with a short-term and a long-term scheduler.
Use the short-term scheduler when you want to support multiple reviews of the same card in a single day. Use the long-term scheduler otherwise.

Here is how you can create your own scheduler:

```swift
import SwiftFSRS

struct CustomScheduler: Scheduler {
  func schedule(
    card: Card,
    algorithm: FSRSAlgorithm,
    reviewRating: ReviewRating,
    reviewTime: Date
  ) -> CardReview {
    // Implement your custom algorithm here
  }
}
```

### The Algorithm

The library implements `v5` of the FSRS algorithm out of the box. It can also support custom implementations.

```swift
import SwiftFSRS

let customAlgorithm = FSRSAlgorithm(
  decay: -0.5,
  factor: 19 / 81,
  requestRetention: 0.9,
  maximumInterval: 36500,
  parameters: [/* ... */]
)

scheduler.schedule(
  card: card,
  algorithm: customAlgorithm,
  reviewRating: .good,
  reviewTime: Date()
)
```

### Scheduling a Review

To schedule a review for a given card:

```swift
import SwiftFSRS

let scheduler = LongTermScheduler()
let card = Card()

let review = scheduler.schedule(
  card: card,
  algorithm: .v5,
  reviewRating: .good, // or .easy, .hard, .again
  reviewTime: Date()
)

print(review.card)
print(review.log)
```

Cards don't have any content properties and are meant to be properties of your own type.

```swift
import SwiftFSRS

struct MyFlashCard {
  let question: String
  let answer: String
  let fsrsCard: Card
}
```

## License

SwiftFSRS is available under the MIT license. See the LICENSE file for more info.
