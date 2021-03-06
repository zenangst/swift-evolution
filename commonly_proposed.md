### Commonly Rejected Changes 
 
This is a list of changes to the Swift language that are frequently proposed, but that are unlikely to be accepted.  If you're interested in pursuing something in this space, please familiarize yourself with the discussions that we have already had.  In order to bring one of these topics up, you'll be expected to add new information to the discussion, not just say "I really want this" or "This exists in some other language and I liked it there".

Several of the discussions below refer to "C Family" languages.  This is intended to mean the extended family of languages that resemble C at a syntactic level.  This includes languages like C++, C#, Objective-C, Java, Javascript, etc.

 * [Replace `{}` Brace Syntax with Python-style indentation](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20151214/003656.html): Surely a polarizing issue, but Swift will not change to use indentation for scoping instead of curly braces.

 * [Replace Logical Operators (`&&`, `||`, etc) with words like "and" and "or"](https://lists.swift.org/pipermail/swift-evolution/2015-December/000032.html), [allowing non-punctuation as operators](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20160104/005669.html) and infix functions: The operator and identifier grammars are intentionally partitioned in Swift, which is a key part to how user defined overloaded operators are supported.  Requiring the compiler to see the "operator" declaration to know how to parse a file would break the ability to be able to parse a Swift file without parsing all of its imports.  This has a major negative effect on tooling support.

 * [Replace `?:` Ternary Operator](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20151214/002609.html): Definitely magical, but it serves a very important use-case for terse selection of different values.  Proposals for alternatives have been intensely discussed, but none have been "better enough" for it to make sense to diverge from the precedent established by the C family of languages.

 * [`if/else` and `switch` as expressions](https://lists.swift.org/pipermail/swift-evolution/2015-December/000393.html): These are conceptually interesting things to support, but many of the problems solved by making these into expressions are already solved in Swift in other ways.  Making them expressions introduces significant tradeoffs, and on balance, we haven't found a design that is clearly better than what we have so far.

 * [Rewrite the Swift compiler in Swift](https://github.com/apple/swift/blob/2c7b0b22831159396fe0e98e5944e64a483c356e/www/FAQ.rst): This would be a lot of fun someday, but (unless you include rewriting all of LLVM) requires the ability to import C++ APIs into Swift.  Additionally, there are lots of higher priority ways to make Swift better.

 * [Change closure literal syntax](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20151214/002583.html): Closure syntax in Swift has been carefully debated internally, and aspects of the design have strong motivations.  It is unlikely that we'll find something better, and any proposals to change it should have a very detailed understanding of the Swift grammar.

 * [Single-quotes `''` for Character literals](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20151221/003977.html): Swift takes the approach of highly valuing Unicode.  However, there are multiple concepts of a character that could make sense in Unicode, and none is so much more commonly used than the others that it makes sense to privilege them.  We'd rather save single quoted literals for a greater purpose (e.g. non-escaped string literals).

 * [Replace `continue` keyword with synonyms from other scripting languages (e.g. next, skip, advance, etc)](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20151221/004407.html): Swift is designed to feel like a member of the C family of languages.  Switching keywords away from C precedent without strong motivation is a non-goal.

* [Replace the do/try/repeat keywords with C++-style syntax](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20151228/004630.html): Swift's error handling approach is carefully designed to make it  obvious to maintainers of code when a call can "throw" an error.  It is intentionally designed to be syntactically similar in some ways - but different in other key ways - to exception handling in other languages.  Its design is a careful balance that favors maintainers of code that uses errors, to make sure someone reading the code understands what can throw.  Before proposing a change to this system, please read the [Error Handling Rationale Document](https://github.com/apple/swift/blob/master/docs/ErrorHandlingRationale.rst) in full to understand why the current design is the way it is, and be ready to explain why your changes would be worth unbalancing this design.

* [Rename `guard` to `unless`](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20160104/005534.html): It is a common request to ask that `guard` be renamed `unless`. People requesting this change argue that `guard` is simply a logically-inverted `if` statement, and therefore `unless` is a more obvious keyword. However, such requests stem from a fundamental misunderstanding of the functionality provided by `guard`. Unlike `if`, `guard` *enforces* that the code within its curly braces provides an early exit from the codepath. In other words, a `guard` block **must** `return`, `throw`, `break`, `continue` or call a `@noreturn` function such as `fatalError()`. This differs from `if` quite significantly, and therefore the parallels assumed between `guard` and `if` are not valid.

* [Use pattern-matching in `if let` instead of optional-unwrapping](http://thread.gmane.org/gmane.comp.lang.swift.evolution/5787/): We actually tried this and got a lot of negative feedback, for several reasons: 1) Most developers don't think about things in "pattern matching" terms, they think about "destructuring". 2) The vastly most common use case for "if let" is actually for optional matching, and change made the common case more awkward. 3) this change increases the learning curve of Swift, changing pattern matching from being a concept that can be learned late to something that must be confronted early. 4) The current design of "if case" unifies "pattern matching" around the 'case' keyword.  5) If an developer unfamiliar with "if case" runs into one in some code, they can successfully search for it in a search engine or stack overflow. 

* [Use Garbage Collection instead of ARC](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20160208/009403.html): Mark and sweep garbage collection is a well known technique used in many popular and widely used languages (e.g., Java and JavaScript) and it has the advantage of automatically collecting reference cycles that ARC requires the programmer to reason about.  That said, garbage collection has a [large number of disadvantages](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20160208/009422.html) and using it would prevent Swift from successfully targeting a number of systems programming domains.  For example, real time systems (video or audio processing), deeply embedded controllers, and most kernels find GC to be generally unsuitable.  Further, GC is only efficient when given 3–4× more memory to work with than the process is using at any time, and this tradeoff is not acceptable for Swift.

Here are some other less-commonly proposed changes that have also been rejected:
 
* [Remove `;` Semicolons](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20151214/002421.html): Semicolons within a line are an intentional expressivity feature.  Semicolons at the end of the line should be handled by a "linter", not by the compiler.

* [Remove support for `default:` in `switch`, and just use `case _:`](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20151207/001422.html): `default` is widely used, `case _` is too magical, and `default` is widely precedented in many C family languages.

* [SE-0009: Require self for accessing instance members  ](proposals/0009-require-self-for-accessing-instance-members.md)

* [SE-0024: Optional Value Setter `??=`](proposals/0024-optional-value-setter.md)

* [SE-0027: Expose code unit initializers on String](proposals/0027-string-from-code-units.md)

* [SE-0010: Add StaticString.UnicodeScalarView](proposals/0010-add-staticstring-unicodescalarview.md)

Finally, the readme file includes a list of changes we want to make eventually, but which are [out of scope for the next major release of Swift](README.md#out-of-scope) because we don't have time to do them right before then. Proposals for out-of-scope changes will not be scheduled for review.
