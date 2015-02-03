Spotify Objective-C Coding Style
================================

Version: 0.9.0

Our general coding conventions at Spotify are documented on an internal wiki, but specifics for Objective C and
Objective C++ code in the iOS client are documented here.

License
-------
Copyright (c) 2015 Spotify AB. 

This work is licensed under a [Creative Commons Attribution 4.0 International License]( http://creativecommons.org/licenses/by/4.0/).

Table of Contents
-----------------

1.  [Spacing, Lines and Formatting](#spacing-lines-and-formatting)
2.  [Brackets](#brackets)
3.  [Naming](#naming)
4.  [Comments](#comments)
5.  [Pragma Marks](#pragma-marks)
6.  [Constants](#constants)
7.  [Return Early](#return-early)
8.  [Initializers](#initializers)
9.  [Prefix Headers](#prefix-headers)
10. [Strings](#strings)
11. [Dot Notation](#dot-notation)
12. [Categories](#categories)

Spacing, Lines and Formatting
-----------------------------

### Line length
* Keep your lines within **120** characters width when possible.
  * In Xcode, you can set a page guide in Text Editing in Preferences.

### Whitespace
* Use **4** spaces for indentation and alignment. Do not use tabs.
* Trailing whitespace is acceptable only on blank lines, but discouraged even there.
  * In Xcode, select "Automatically trim whitespace" and "Including whitespace-only lines" in Text Editing preferences
    to handle this automatically.
* Put spaces after commas, and before and after operators.
* Do not put spaces between parentheses and what they are enclosing.

**Example:**
```objc
foo("bar") 
```

**Not:**

```objc
foo( "bar" )
```

### Containers
* Array one-liners are acceptable unless they have too many items or their values are too long.

```objc
NSArray *array = @[@"uno", @"dos", @"tres", @"cuatro"];
```

* In that case, break them in several lines:

```objc
NSArray *array = @[
    @"This is how we do, yeah, chilling, laid back",
    @"Straight stuntin’ yeah we do it like that",
    @"This is how we do, do do do do, this is how we do",
];
```

* Dictionary one-liners are reserved for single pairs only:

```objc
NSDictionary *dict = @{@"key" : @"highway"};
```

* Format it pretty otherwise, leaving a trailing comma after the last item:
```objc
NSDictionary *dict = @{
    @"key1" : @"highway",
    @"key2" : @"heart",
};
```

Brackets
--------
* Always use brackets for `if` / `for` / `while` / `do`-`while` statements. Even if its a one-liner:

```objc
if (itsMagic) {
    [self makeItEverlasting];
}
```

* Write follow up `else` clauses after the previous closing bracket on the same line.

```objc
if (hasClue) {
    knowExactlyWhatToDo();
} else if (seeItAllSoClear) {
    writeItDown();
} else {
    sing();
    dance();
}
```

Naming
------
* Follow Apple’s [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
on naming.

Comments
--------
* Use the `//` style for single line comments or when appending comments in the same line of code.
* Use the `/* */` style for multi-line comments.

**Example:**
```objc
/*
 * This is a multi-line comment. The opening and closing markers are on their
 * own lines, and each other line is preceded by a * that is indented by one
 * space.
 *
 * This is a new paragraph in the same block comment.
 */

stop(); // Hammer-time!

// this is a very brief comment.
```

* Use [doxygen](http://www.stack.nl/~dimitri/doxygen/manual/docblocks.html)-style comments for all the headers. Make
sure to use the available markup tags like `@param`, `@returns`, etc. The `///` form is preferred for single line
comments, and `/** */` for multi-line comments. Use the same formatting as in the example block above.

Pragma Marks
------------
* Use the pre-processor instruction `#pragma mark` to mark related groups of methods.

Constants
---------
* Do not define constants using `#define`.
* Publicly (or privately) exposed variables should be constant (trying to assign values will result in compilation
error).

```objc
extern NSString * const SPTCodeStandardErrorDomain;
```

* You can use the `SPTNSStringConstant` (defined in "SPTTypeUtils.h") for your constant `NSString`s.

```objc
extern SPTNSStringConstant SPTCodeStandardDidPassLintingNotification;
```

Return Early
------------
* Return early on errors and failed pre-conditions to avoid unnecessary nested brackets and / or unnecessary
computations.

**Example:**

```objc
- (void)setFireToThe:(id)rain
{
    if ([_rain isEqualTo:rain]) {
        return;
    }

    _rain = rain;
}
```

Initializers
------------
* Always do assignment of `self`, call `super` (or the designated initializer) and return early on failure.

**Example:**

```objc
- (instancetype)initWithName:(NSString *)name
{
    if (!(self = [super init])) {
        return nil;
    }

    _name = name;

    return self;
}
```

Prefix Headers
--------------
* The use of prefix headers has been deprecated. Do not add new code to an existing prefix header.

Strings
-------
* All strings presented to the user should be localized.

Dot Notation
------------
* Use bracket notation when calling non-accessor methods:

```objc
[self doSomething];
```

* Use bracket notation when accessing globals through a class method:

```objc
[MyClass sharedInstance];
```

* Set and access properties using dot notation:

```objc
self.myString = @"A string";
```

* Except in the `init` or `dealloc` methods, always use the ivar directly there:

```objc
_myString = nil;
```

Categories
----------
* Methods in categories on non-Spotify classes must be prefixed `spt_` to avoid name clashes.
