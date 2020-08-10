Spotify Objective-C Coding Style
================================

Version: 0.9.0

Our general coding conventions at Spotify are documented on an internal wiki, but specifics for Objective-C and
Objective-C++ code in the iOS client are documented here.

License
-------
Copyright (c) 2015-2016 Spotify AB. 

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
9.  [Headers](#headers)
10. [Nullability](#nullability)
11. [Strings](#strings)
12. [Dot Notation](#dot-notation)
13. [Categories](#categories)

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
 This is a multi-line comment. The opening and closing markers are on their
 own lines.

 This is a new paragraph in the same block comment.
 */

stop(); // Hammer-time!

// this is a very brief comment.
```

* Document all methods and properties in the headers using Xcode’s documentation style (which is `///` at the time of writing). 
You can select a property or method and use `Option`+`Cmd`+`/` to generate the documentation template for it. Make sure to use the available 
markup tags like `@param`, `@return`, etc. (these will be auto-generated for you by Xcode).

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

Return Early
------------
* Return early on errors and failed pre-conditions to avoid unnecessary nested brackets and / or unnecessary
computations.

**Example:**

```objc
- (void)setFireToTheRain:(id)rain
{
    if ([_rain isEqualTo:rain]) {
        return;
    }

    _rain = rain;
}
```

Initializers
------------
* Follow [Apple's typical init format](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html#//apple_ref/doc/uid/TP40011210-CH5-SW11) to initialize objects.

**Example:**

```objc
- (instancetype)init 
{
    self = [super init];
 
    if (self) {
        // initialize instance variables here
    }
 
    return self;
}
```

Headers
-------
* The use of prefix headers has been deprecated. Do not add new code to an existing prefix header.
* When importing a module use the hash-import variant instead of at-import.
  * Yes: `#import <Foundation/Foundation.h>`
  * No: `@import Foundation;`

Nullability
-----------
* Use `NS_ASSUME_NONNULL_BEGIN` and `NS_ASSUME_NONNULL_END` in header files, and explicitly add `nullable` when needed. Example:
```objc
#import <Foundation/Foundation.h>

@protocol SPTPlaylist;

NS_ASSUME_NONNULL_BEGIN

typedef void(^SPTSomeBlock)(NSData * _Nullable data, NSError * _Nullable error);

@interface SPTYourClass : NSObject

@property (nonatomic, copy, readonly, nullable) NSString *customTitle;
@property (nonatomic, strong, readonly) id<SPTPlaylist> playlist;

- (nullable instancetype)initWithPlaylist:(id<SPTPlaylist>)playlist
                              customTitle:(nullable NSString *)customTitle;

@end

NS_ASSUME_NONNULL_END
```

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
