# C Coding Standards
## Aurora Lighting UK Firmware Department

## Principles
We value habitable code because it is easy to understand, easy to maintain and reduces operational costs.

* Prefer consistency over personal preference

* Everything should be as simple as possible but no simpler

* Value limited resources

* Value re-use
 
* Encapsulate complexity

* Value scalability

* Value testability

***

## Naming Conventions
### Application Names

Should be in the format NPDXXXXNameOfProduct eg)
 `NPD4318DoubleSocket`
This ensures uniqueness and ease of identification

### File Paths
Component source code (\*.c, \*.cpp) should be in one or more folders called “Source”.
Header files (\*.h) should be in one or more folders called “Include”.


### File Names
Module/Class Source and header filenames should differ only by the extension.
eg)  `SerialDriver.c ` and  `SerialDriver.h`


### Module names
Capitalise word initials

e.g. `ADCDriver` or `LEDDriver` 
>This provides clarity of word separation without cluttering nature of underscores… also considered
>general best practise.

### Function names
Capitalise word initials.

e.g. `CreateScreen()` or `Start()` or `WriteSerial()`

### Abbreviations
Do not use abbreviations, acronyms are allowed. This rule applies to everything (File names,
Function names, Variables, etc...)
Examples of abbreviations (that would not be allowed) :- temp, ret, res
Examples of acronyms (that would be allowed) :- ADC, MCU, RAM
>Abbreviations in particular can be very misleading. For example, “temp” could be interpreted as
>“temporary” or “temperature”.

### Constants and #defines
const, #define values and macros should be upper case. Use an underscore to separate words.
> This makes it very clear that they are constant values (or macros in the case where brackets are
> used) as opposed to variables or normal functions.

e.g. 
```C
const u8 PWM_FREQUENCY = 100;
#define PWM_FREQUENCY 100
#define FREQUENCY_TO_INTERVAL(frequency) (1 / (frequency))
```
### Variables

### Global Variables
Prefix with an ‘g’ and an underscore (“g_”) and capitalise word initials apart from the first
character which should be lower case. Globals should generally be avoided, but are sometimes necessary.

```C
u32 g_thisIsAGlobalVariable;
```

#### Static Variables
Prefix with an ‘s’ and an underscore (“s_”) and capitalise word initials apart from the first
character which should be lower case.

```C++
u16 s_thisIsAStaticVariable;
```

>This makes clear the distinction between static, local and global variables, and also assists the
>programmer with intellisense friendly prefixes.

#### Local Variables and Function Parameters
No prefix and capitalise word initials apart from the first character which should be lower case.

```C++
void Function(u8 exampleParameter) 
{ 
   char8 exampleLocalVariable = 0; 
}
```


#### Enums
All `enum`s should be enclosed in a `struct`, and the `enum` should have the name ‘`Value`’.
The name of the `struct` (which represents the name of the `enum`) should capitalise word initials.
The name of the `enum` values should capitalise word initials.

e.g.
```C++
struct EnumName
{
   enum Value
   {
     ValueA,
     ValueB
   };
};
 ```

>The resultant structure solves the “namespace problem” with enums (for
>example, Pin::On and Pin::Off distinguish a pin’s enumerations from another enum requiring the
>same names, such as Light::On and Light::Off).

## Data Types
Adopt these common types as defined in `stdintextended.h`
```C
#pragma once

#include <stddef.h>
#include <stdint.h>
#include <stdbool.h>

// Extended definitions of stdint for shorter more compact form of notation based upon stdint
typedef char     char8;
typedef uint8_t  u8;
typedef int8_t   s8;
typedef uint16_t u16;
typedef int16_t  s16;
typedef uint32_t u32;
typedef uint64_t u64;
typedef int32_t  s32;
typedef int64_t  s64;

#ifndef NULL
#define NULL ((void *)0)
#endif

#ifndef null
#define null NULL
#endif

#ifndef TRUE
#define TRUE 1
#endif

#ifndef FALSE
#define FALSE 0
#endif
```

## Layout

### Indentation Style
All indentation should be done with spaces (not tabs). 3 spaces per indentation level should be used.
Indentation should occur any time braces are used with the exception of braces for namespaces (see
below).
>Tabs allow individual programmers to specify their own “tab width” and so customise the formatting
>of code according to their needs + terminal. Spaces, however, make code reviewing on GitHub
>easier, because you know the view will always be representative of the original code. Generally
>speaking, with indentation guidelines consistency and clarity are key.

Braces associated with a control statement are on the next line, indented to the same level as the
control statement. Statements within the braces are indented to the next level.

e.g.
```C++
unsigned u16 DoOperation(u16 value, bool operation)
{
   if(operation)
   {
      value &= 0xFF00;
   }
   else
   {
      value &= 0x00FF;
   }
   return value;
}
```

Always adopts braces even if the control statement is one line.

### Indentation for Switch statements
Within a switch statement all case statements and the default statement should be indented to the
same level as the opening brace, all other statements should follow the normal indentation rule.

e.g.
```C++
switch(currentState)
{
case PossibleStates::StateA:
   break;
case PossibleStates::StateB:
   break;
default:
   break;
}
```

### Line Length
Lines should not exceed 100 characters in length. Lines that exceed the limit should be split across
two or more lines with the additional lines indented to the next indentation level or the first
parameter level from the first line.

e.g.

```C++
void ExampleFunctionWithSplitParameterList(u16 valueA, u16 valueB,
   u16 valueC, u16 valueD)
{
   ...
}
```
or
```C++
void ExampleFunctionWithSplitParameterList(u16 valueA,
                                           u16 valueB,
                                           u16 valueC,
                                           u16 valueD)
{
   ...
}
```

### Function Parameter Lists
Function Parameters should all be on the same line (unless the line length rule is broken), separated
by spaces.

e.g.
 
```C++
void ExampleFunction(s8 exampleParameter1, s8 exampleParameter2)
{
   ...
}
```

### Variable and Member Variable Definitions
Variable definitions should be at the current indentation level with the type followed by a space and
then the variable name. Spaces and/or tabs should not be used to line up variable names.

e.g.
```C
void ExampleFunction()
{
   s16 exampleVariable;
}
```

### Operators
Use spaces around operators

e.g. `if ((length + 5) > MAXIMUM_LENGTH)`

### Parentheses
Liberally use parentheses to explicitly show precedence

### Hex
Use lower case ‘x’ and uppercase A-F

e.g. `0x3F`

***

## Miscellaneous

### Comments
To be included but not to overwhelm the code. And to be in the double forward slash format 
 ```C
 // this is a comment
```

### Return Paths
Functions to have 1 return path unless there is a very, very good reason why it cannot. 

e.g.

```C
bool DoSomething()
{
   bool success = false; 
   
   if(SomethingElse() > THE_TEST_VALUE)
   {
      success = true;
   }
   return success;
}
```


### Yoda Comparisons

Good practice to have the const as the lvalue as this prevents to = / == mixup as the compiler will error on a non modifiable lvalue.  

```C
if(CRITICAL_ERROR == testCondition)
{
}
```

### Switch Statements
Must include a default case.

### #defines
Parameters in a macro type #define must be in parentheses as should function like expressions,
and should be capitalised. Avoid post/prefix operators such as ++ in macros which are likely to have
unwanted side effects.

e.g. `#define MULTIPLY(a,b) ((a) * (b))`

Complex macros should be avoided.
>Macros are difficult to debug and can be obfuscating vs. the use of a function, so should be used
>sparingly.

### Magic numbers
Magic numbers are to be avoided, use constants or defines. The only exceptions are numbers used
in only one place.
> This is primarily to avoid burying numbers in code which makes it harder to maintain. It encourages
> re-use of values without relying on duplication.

### #includes
Includes of header files within headers should be avoided, whenever practical. There are some
situations where this is not practical, for C++ this includes the header declaring the base class which
needs to be included before the derived class can be declared.


### Code in Header Files
The only code (implementations) in header files should be code in-lined for performance

### Guards/Sentinels
All header files should have guards around their contents.

e.g. `For Keys.h`

```C++
#ifndef _KEYS_H
#define _KEYS_H
#include "abc.h"
#include "xyz.h"
const u8 REPEAT_TIME = 100; // milliseconds
#endif // #ifndef _KEYS_H
```

or if supported by the compiler
```C++
#pragma once
```

To get `_KEYS_H` use the filename (upper case) replacing the ‘.’ with an underscore ‘_’ and prefix
with an underscore ‘_’.


### C++ Externs
Good practice to wrap any C header file with following c++ extern

```C
#pragma once

#include "stdintextended.h"

#ifdef __cplusplus
extern "C" {
#endif

#define UART_SUCCESS		 0
#define UART_WRITE_ERROR	-1
#define UART_READ_ERROR		-2

s16 uartWrite(u8 port, u8 *data, u8 length);
s16 uartRead(u8 port, u8 *data, u8 length);
s16 uartReadPending(u8 port);

#ifdef __cplusplus
}
#endif
```



### Long
Use capital L for a long not l which looks like a one

e.g. `index = 10L` not `index = 10l`


### Dynamic Allocation
Allocate all memory required at initialisation to avoid the need for dynamic allocation at run time. 

