# Text Analyzer

A Java console application demonstrating multithreading, thread-safe counters, and the producer-consumer pattern through text generation and analysis tasks.

The repository contains two independent concurrency exercises:

1. Finding generated texts with the highest number of specific characters.
2. Detecting so-called "beautiful words" using multiple analyzer threads.

## Features

* Generates random strings containing the characters `a`, `b`, and `c`
* Uses multiple threads to analyze generated text concurrently
* Implements the producer-consumer pattern with `BlockingQueue`
* Uses bounded queues to control memory consumption
* Uses `AtomicInteger` for thread-safe counters
* Waits for worker completion using `Thread.join()`
* Handles thread interruption
* Detects:

  * Palindromes
  * Words containing identical characters
  * Words whose characters are in ascending order

## Tech Stack

* Java 21
* Maven
* Java Concurrency API
* `BlockingQueue`
* `AtomicInteger`

### `MaxLetterAnalyzer`

Demonstrates the producer-consumer pattern.

One generator thread creates random strings and places each string into three separate blocking queues:

```text
Generator thread
    ├── Queue A → Analyzer for letter a
    ├── Queue B → Analyzer for letter b
    └── Queue C → Analyzer for letter c
```

Each analyzer thread:

1. Takes strings from its queue.
2. Counts occurrences of its assigned character.
3. Stores the string containing the highest number of that character.
4. Stops after receiving a special end marker.

The application creates:

* 10,000 random strings
* 100,000 characters in each string
* Three analyzer threads for `a`, `b`, and `c`
* Three bounded queues with a capacity of 100 elements

### `ABCChecking`

Generates 100,000 random words with lengths from 3 to 5 characters.

Three threads analyze the same collection concurrently:

* Palindrome analyzer
* Identical-character analyzer
* Ascending-order analyzer

Thread-safe counters store the number of matching words by length:

* Length 3
* Length 4
* Length 5



## Maximum-Letter Analysis Flow

```text
Random text generator
        ↓
Generated text
        ↓
┌──────────────┬──────────────┬──────────────┐
│ Queue for a  │ Queue for b  │ Queue for c  │
└──────┬───────┴──────┬───────┴──────┬───────┘
       ↓              ↓              ↓
Analyzer A       Analyzer B       Analyzer C
       ↓              ↓              ↓
Maximum count    Maximum count    Maximum count
of letter a      of letter b      of letter c
```

## Beautiful-Word Rules

A generated word is considered beautiful when it matches one of the following rules.

### Palindrome

The word reads the same from left to right and from right to left.

Examples:

```text
aba
acca
abcba
```

### Identical Characters

Every character in the word is the same.

Examples:

```text
aaa
bbbb
ccccc
```

### Ascending Character Order

Each character is lexicographically equal to or greater than the previous character.

Examples:

```text
abc
aabc
abbcc
```

## Possible Improvements

* Use `ExecutorService` instead of creating threads manually
* Add unit tests for:

  * Character counting
  * Palindrome detection
  * Identical-character detection
  * Ascending-order detection
* Add integration tests for concurrent processing
* Add structured logging


## License

No license has been specified for this project.
