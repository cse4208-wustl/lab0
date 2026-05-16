# Lab 0

## Cards and Decks

This lab gives you hands-on practice with procedural, functional, object-oriented, and generic programming styles while refreshing core C++ concepts such as basic data types, containers, output streams, functions, operator overloading, classes, inheritance, and templates.

You will implement abstractions for decks and cards in two games with different rules for rank ordering and card multiplicity: [Pinochle](https://en.wikipedia.org/wiki/Pinochle) and [Texas hold 'em Poker](https://en.wikipedia.org/wiki/Texas_hold_%27em).

You will then write a program that constructs decks for each game and prints their contents so you can verify that the abstractions behave correctly.

## Reference

Useful references for this lab:

- C++ variables and basic data types: Lippman Chapters 1 and 2
- C++ enumerations: Lippman Chapter 19.3
- C++ strings, vectors, arrays, and I/O: Lippman Chapter 3, plus a skim of Chapter 8
- C++ class and function templates: Lippman Chapter 16.1
- C++ increment operators: Lippman Chapter 4.5
- C++ shift operators: Lippman Chapter 4.8
- [C++ Reference](http://www.cppreference.com)
- [Studio 0](https://github.com/cse4208-wustl/studio0) for environment setup review

## Programming Guidelines

Review the [Programming Guidelines](docs/programming-guidelines.md) before you begin implementing the lab, and keep them in mind as you develop and test your solution.

## Details

Record your observations, design decisions, compile warnings or errors, and any other written responses in `ANSWERS.md` as you work.

Some parts of this lab are intentionally somewhat open-ended. When details are under-specified, choose a reasonable design and document that choice clearly in `ANSWERS.md` and comments where helpful.

1. Log into one of the Linux Lab machines via `qlogin`, and confirm that the correct version of `g++` (`8.3.0`) is installed in your environment, as you did in [Studio 0](https://github.com/cse4208-wustl/studio0).

2. Clone your `lab0` repo and work inside that cloned directory.

   The repo already includes the provided `Makefile`. It assumes specific names for all the files you will develop as part of your solution, so you may want to adjust those names if your implementation uses a different structure.

   In the interest of working incrementally, you may also want to comment out some of the `Makefile` details at first and then add them back in as you go.

3. Use `ANSWERS.md` to record your observations, design decisions, and any information about your implementation as you develop your solution.

4. Add a new C++ header file and a new C++ source file. In them, declare and define a suit enumeration and related operators for playing cards used in both Pinochle and Texas hold 'em Poker.

   In the header file:

   - declare an `enum class` for the suits `clubs`, `diamonds`, `hearts`, and `spades`
   - add a highest-valued `undefined` suit to support iteration and out-of-band states
   - keep the enumeration values monotonically increasing in that order

   In the same header and source file:

   - declare and define `operator<<` so it prints `"C"`, `"D"`, `"H"`, `"S"`, or `"?"` as appropriate
   - declare and define a prefix increment operator that advances to the next suit unless the value is already `undefined`

5. Add a new C++ header file and source file that declare and define a `Card` struct template parameterized by rank and suit types.

   Your solution should:

   - store the rank and suit as member variables
   - provide a constructor that initializes those members
   - include the template source file from the template header inside `#ifdef TEMPLATE_HEADERS_INCLUDE_SOURCE`
   - ensure the `Makefile` provides `-DTEMPLATE_HEADERS_INCLUDE_SOURCE`
   - declare and define a template `operator<<` that inserts the card's rank and suit into an `ostream`
   - keep the `operator<<` declaration and definition outside the `Card` struct template itself

   Each template declaration or definition should begin with:

   ```cpp
   template <typename R, typename S>
   ```

6. Add a new C++ header file declaring an abstract base class `Deck` with a single public pure virtual `print` method that takes an `ostream&` and returns `void`.

7. Add a new C++ header file and source file for a Pinochle rank enumeration and a `PinochleDeck` class derived from `Deck`. Note that using an enum class style declaration and giving the enumerated type a distinct name (like `PinochleRank` for example) can be helpful for both (1) readability and (2) disambiguating labels for ranks that appear in both Pinochle and Texas hold 'em poker, because scoping is explicit.

   Your solution should:

   - declare the ranks in increasing order as `nine`, `jack`, `queen`, `king`, `ten`, `ace`, then `undefined`
   - provide `operator<<` for the rank values
   - provide a prefix increment operator for the rank enumeration
   - define a `PinochleDeck` class with a private `vector` of cards parameterized with the rank enumeration for Pinochle and the suit enumeration
   - implement a default constructor that inserts two of each valid rank and suit combination into the deck
   - implement `print` so it outputs the cards in a readable format

   The default constructor should use the prefix increment operators to traverse valid rank and suit values and should not insert cards with `undefined` rank or suit.

   Document any important design decisions for the `print` formatting in `ANSWERS.md`.

   Note that when working with templates, using enough whitespace (and especially putting whitespace between distinct types and the symbols that surround them where necessary) is often essential in order for your code to compile. For example, vector< Foo<T> >::iterator may compile just fine as long as the appropriate header files are included, while vector<Foo<T>>::iterator may not compile because the compiler sees >> as a shift operator.

8. Add a new C++ header file and source file for a Texas hold 'em rank enumeration and a `HoldEmDeck` class derived from `Deck`.

   Your solution should:

   - declare the ranks in increasing order as `two` through `ace`, then `undefined`
   - provide `operator<<` for the rank values
   - provide a prefix increment operator for the rank enumeration
   - define a `HoldEmDeck` class with a private `vector` of `Card<HoldEmRank, Suit>`
   - implement a default constructor that inserts one of each valid rank and suit combination
   - implement `print` so it outputs the cards in a readable format

   The default constructor should use the prefix increment operators to traverse valid rank and suit values and should not insert cards with `undefined` rank or suit.

   Document any important design decisions for formatting or ordering in `ANSWERS.md`.

9. Add a new C++ source file defining `main`.

   The `main` function should:

   - declare stack variables of the Pinochle and Hold 'em deck types
   - pass `cout` into each deck's `print` method
   - return `0` on success

10. Run `make` and fix any errors or warnings that occur. Record the kinds of errors or warnings you encountered, even if you do not list every single instance.

11. Run the executable and confirm that the right number of cards of each suit and rank are printed for each deck:

   - two of each valid card for Pinochle
   - one of each valid card for Texas hold 'em Poker

   In `ANSWERS.md`, document:

   - any runs where the output was incorrect
   - what caused the incorrect output
   - how you fixed it
   - the output that demonstrates correct behavior

12. At the top of `ANSWERS.md`, include:

   - your names
   - your email addresses
   - the lab number (`lab0`)

   Also make sure `ANSWERS.md` records:

   - whether you encountered warnings or errors while developing the solution
   - what the executable did for each trial you ran

   If you did not encounter any warnings or errors, note that explicitly.

13. Continue developing the remaining parts of the assignment from the provided instructions using the same conventions above:

   - preserve the intended abstractions and iteration rules
   - use `ANSWERS.md` in place of the old readme-file workflow
   - document deviations, assumptions, and noteworthy implementation details as you go
