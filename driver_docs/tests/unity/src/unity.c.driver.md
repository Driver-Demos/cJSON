# Purpose
The provided C source code is part of the Unity Test Framework, a lightweight testing framework designed for C programming. This file primarily focuses on implementing various assertion functions, test result output handlers, and utility functions that facilitate the testing process. The code includes functions for printing test results, handling different data types, and managing test execution flow. It provides a comprehensive set of assertion functions to compare expected and actual values, including numbers, strings, arrays, and memory blocks. Additionally, it supports floating-point and double precision comparisons, with options to handle special cases like NaN and infinity.

The file also includes mechanisms for managing test execution, such as starting and concluding tests, handling test failures and ignores, and providing detailed output for test results. It supports command-line arguments to filter tests based on inclusion or exclusion criteria, allowing for flexible test execution. The code is structured to be part of a larger test suite, with functions like [`UnityBegin`](#UnityBegin) and [`UnityEnd`](#UnityEnd) managing the overall test lifecycle. The use of macros and conditional compilation ensures that the framework can be customized and extended to suit different testing needs, making it a versatile tool for C developers.
# Imports and Dependencies

---
- `unity.h`
- `stddef.h`


# Global Variables

---
### Unity
- **Type**: `struct UNITY_STORAGE_T`
- **Description**: The `Unity` variable is a global instance of the `UNITY_STORAGE_T` structure, which is used to store the state and results of the test framework. This structure likely contains fields to track the current test name, line number, number of tests, failures, and ignores, among other details.
- **Use**: This variable is used throughout the Unity test framework to manage and report the results of unit tests.


---
### UnityStrOk
- **Type**: ``const char[]``
- **Description**: `UnityStrOk` is a constant character array that holds the string "OK". It is used to represent a successful test result in the Unity test framework.
- **Use**: This variable is used to output the string "OK" when a test passes successfully.


---
### UnityStrPass
- **Type**: ``const char[]``
- **Description**: `UnityStrPass` is a static constant character array that holds the string "PASS". It is used to represent a successful test result in the Unity test framework.
- **Use**: This variable is used to print or display the result of a test when it passes.


---
### UnityStrFail
- **Type**: ``const char[]``
- **Description**: `UnityStrFail` is a global constant character array that holds the string "FAIL". It is used to represent a failed test result in the Unity test framework.
- **Use**: This variable is used in the Unity test framework to output or log a message indicating that a test has failed.


---
### UnityStrIgnore
- **Type**: ``const char[]``
- **Description**: `UnityStrIgnore` is a global constant character array that holds the string "IGNORE". It is used within the Unity test framework to represent a test status where a test is ignored.
- **Use**: This variable is used to output the status of a test as 'IGNORE' when a test is intentionally skipped or ignored during the test execution process.


---
### UnityStrNull
- **Type**: ``const char[]``
- **Description**: `UnityStrNull` is a global constant character array initialized with the string "NULL". It is used within the Unity test framework to represent a null or empty string in test output.
- **Use**: This variable is used to print or compare against null or empty string values in test assertions and results.


---
### UnityStrSpacer
- **Type**: ``const char[]``
- **Description**: `UnityStrSpacer` is a constant character array initialized with the string ". ". It is used as a spacer or separator in the Unity test framework output.
- **Use**: This variable is used to insert a space and a period in the output messages to improve readability.


---
### UnityStrExpected
- **Type**: ``const char[]``
- **Description**: `UnityStrExpected` is a static constant character array that holds the string " Expected ". It is used within the Unity test framework to format output messages, specifically to indicate the expected value in test assertions.
- **Use**: This variable is used in test result output to label the expected value when comparing expected and actual results in assertions.


---
### UnityStrWas
- **Type**: ``const char[]``
- **Description**: `UnityStrWas` is a constant character array that holds the string " Was ". It is used within the Unity test framework to format output messages, particularly when displaying the actual value of a test result.
- **Use**: This variable is used in test result output functions to indicate the actual value in comparison messages.


---
### UnityStrGt
- **Type**: ``const char[]``
- **Description**: `UnityStrGt` is a constant character array that holds the string " to be greater than ". It is used within the Unity test framework to provide descriptive output when comparing numerical values.
- **Use**: This variable is used in test assertions to indicate that a value is expected to be greater than another value.


---
### UnityStrLt
- **Type**: ``const char[]``
- **Description**: `UnityStrLt` is a constant character array that holds the string " to be less than ". It is used within the Unity test framework to provide descriptive output when comparing values in test assertions.
- **Use**: This variable is used in test assertions to indicate that a value was expected to be less than another value.


---
### UnityStrOrEqual
- **Type**: ``const char[]``
- **Description**: `UnityStrOrEqual` is a static constant character array that holds the string "or equal to ". It is used within the Unity test framework to provide descriptive output for test assertions that involve comparisons where equality is also considered.
- **Use**: This variable is used in test assertions to append descriptive text when checking if a value is greater than, less than, or equal to a threshold.


---
### UnityStrElement
- **Type**: ``const char[]``
- **Description**: `UnityStrElement` is a global constant character array that holds the string " Element ". It is defined as a static constant, meaning it is limited to the file scope and cannot be modified.
- **Use**: This variable is used in the Unity test framework to format output messages, particularly when indicating the index of an element in an array during test assertions.


---
### UnityStrByte
- **Type**: ``const char[]``
- **Description**: `UnityStrByte` is a constant character array that holds the string " Byte ". It is defined as a static variable, meaning it is limited to the file scope and cannot be accessed outside of this file.
- **Use**: This variable is used in the Unity test framework to output or compare byte-related information during test assertions.


---
### UnityStrMemory
- **Type**: ``const char[]``
- **Description**: `UnityStrMemory` is a constant character array that holds the string " Memory Mismatch.". It is used within the Unity test framework to provide a specific error message when a memory mismatch is detected during test assertions.
- **Use**: This variable is used to output a specific error message when a memory mismatch occurs in test assertions.


---
### UnityStrDelta
- **Type**: ``const char[]``
- **Description**: `UnityStrDelta` is a constant character array that holds the string " Values Not Within Delta ". This string is used in the Unity test framework to indicate that a test has failed because the values being compared are not within the specified delta range.
- **Use**: This variable is used in test assertions to provide a specific failure message when values are not within the expected delta.


---
### UnityStrPointless
- **Type**: ``const char[]``
- **Description**: `UnityStrPointless` is a constant character array that holds the string " You Asked Me To Compare Nothing, Which Was Pointless.". This string is used as a message in the Unity test framework to indicate that a comparison was attempted with no elements, which is considered a pointless operation.
- **Use**: This variable is used to provide a specific error message when a test case attempts to compare an empty set of elements.


---
### UnityStrNullPointerForExpected
- **Type**: ``const char[]``
- **Description**: `UnityStrNullPointerForExpected` is a constant character array that holds the string " Expected pointer to be NULL". It is used in the Unity test framework to provide a specific error message when a test expects a pointer to be NULL but it is not.
- **Use**: This variable is used in the Unity test framework to output an error message when a test fails due to an expected NULL pointer not being NULL.


---
### UnityStrNullPointerForActual
- **Type**: ``const char[]``
- **Description**: `UnityStrNullPointerForActual` is a constant character array that holds the string " Actual pointer was NULL". It is used in the Unity test framework to provide a specific error message when an actual pointer is found to be NULL during a test.
- **Use**: This variable is used in the Unity test framework to output an error message when a test fails due to an actual pointer being NULL.


---
### UnityStrNot
- **Type**: ``const char[]``
- **Description**: `UnityStrNot` is a constant character array that holds the string "Not ". It is used in the Unity test framework to represent the negation of a condition or state, particularly in the context of floating-point comparisons.
- **Use**: This variable is used to print or log messages indicating the negation of a condition, such as when a test expects a value not to be a certain trait (e.g., not infinite).


---
### UnityStrInf
- **Type**: ``const char[]``
- **Description**: `UnityStrInf` is a constant character array that holds the string "Infinity". It is used within the Unity test framework to represent the concept of positive infinity in floating-point comparisons.
- **Use**: This variable is used in assertions and comparisons involving floating-point numbers to check for positive infinity.


---
### UnityStrNegInf
- **Type**: ``const char[]``
- **Description**: `UnityStrNegInf` is a constant character array that holds the string "Negative Infinity". It is used within the Unity test framework to represent the concept of negative infinity in floating-point comparisons.
- **Use**: This variable is used in assertions and test result outputs to indicate when a floating-point value is expected to be or is actually negative infinity.


---
### UnityStrNaN
- **Type**: ``const char[]``
- **Description**: `UnityStrNaN` is a constant character array that holds the string "NaN", which stands for 'Not a Number'. This is typically used to represent undefined or unrepresentable numerical results, especially in floating-point calculations.
- **Use**: This variable is used in the Unity test framework to identify and handle cases where a floating-point number is not a valid number (NaN).


---
### UnityStrDet
- **Type**: ``const char[]``
- **Description**: `UnityStrDet` is a constant character array that holds the string "Determinate". It is used within the Unity test framework to represent a determinate floating-point number, which is a number that is neither infinite nor NaN.
- **Use**: This variable is used in assertions to check if a floating-point number is determinate.


---
### UnityStrInvalidFloatTrait
- **Type**: ``const char[]``
- **Description**: `UnityStrInvalidFloatTrait` is a constant character array that holds the string "Invalid Float Trait". It is used within the Unity test framework to represent an invalid float trait condition.
- **Use**: This variable is used in the context of floating-point assertions to indicate an invalid float trait scenario.


---
### UnityStrErrFloat
- **Type**: ``const char[]``
- **Description**: `UnityStrErrFloat` is a global constant character array that holds the string "Unity Floating Point Disabled". This string is used to indicate that floating-point support is disabled in the Unity test framework.
- **Use**: This variable is used to provide a specific error message when floating-point operations are attempted in an environment where they are not supported.


---
### UnityStrErrDouble
- **Type**: ``const char[]``
- **Description**: `UnityStrErrDouble` is a constant character array that holds the string "Unity Double Precision Disabled". This string is used to indicate that double precision support is disabled in the Unity test framework.
- **Use**: This variable is used to provide a specific error message when double precision support is not available in the Unity test framework.


---
### UnityStrErr64
- **Type**: ``const char[]``
- **Description**: `UnityStrErr64` is a constant character array that holds the string "Unity 64-bit Support Disabled". This string is used to indicate that 64-bit support is not enabled in the Unity test framework.
- **Use**: This variable is used to provide a specific error message when 64-bit support is not available in the Unity framework.


---
### UnityStrBreaker
- **Type**: ``const char[]``
- **Description**: `UnityStrBreaker` is a static constant character array initialized with a string of dashes. It serves as a visual separator or breaker line in the output of the Unity test framework.
- **Use**: This variable is used to print a line of dashes as a separator in the test results output.


---
### UnityStrResultsTests
- **Type**: ``const char[]``
- **Description**: `UnityStrResultsTests` is a constant character array that holds the string " Tests ". It is used within the Unity test framework to represent the word 'Tests' in the output of test results.
- **Use**: This variable is used to print the number of tests executed in the test results summary.


---
### UnityStrResultsFailures
- **Type**: ``const char[]``
- **Description**: `UnityStrResultsFailures` is a constant character array that holds the string " Failures ". It is used within the Unity test framework to represent the label for test failures in the output.
- **Use**: This variable is used to print the number of test failures in the test results summary.


---
### UnityStrResultsIgnored
- **Type**: ``const char[]``
- **Description**: `UnityStrResultsIgnored` is a constant character array that holds the string " Ignored ". This string is used within the Unity test framework to represent the status of tests that have been ignored during a test run.
- **Use**: This variable is used in the Unity test framework to output the number of tests that were ignored in the test results summary.


---
### UnityStrDetail1Name
- **Type**: ``const char[]``
- **Description**: `UnityStrDetail1Name` is a constant character array that holds a string composed of the macro `UNITY_DETAIL1_NAME` followed by a space character. This string is used within the Unity test framework to label or identify a specific detail in test output.
- **Use**: This variable is used to print or display the first detail name in test result messages when additional details are specified.


---
### UnityStrDetail2Name
- **Type**: `const char[]`
- **Description**: `UnityStrDetail2Name` is a constant character array that holds a string composed of a space, the macro `UNITY_DETAIL2_NAME`, and another space. This string is used in the Unity test framework to format and display detailed test information.
- **Use**: This variable is used to print additional test details when a test fails or is ignored, specifically when the second level of detail is provided.


---
### UnityQuickCompare
- **Type**: `union`
- **Description**: `UnityQuickCompare` is a static union that can store different types of integer and floating-point values, depending on the configuration. It includes fields for 8-bit, 16-bit, 32-bit, and optionally 64-bit integers, as well as optional fields for float and double types. This union is used to facilitate quick comparisons of different numeric types within the Unity test framework.
- **Use**: This variable is used to temporarily store numeric values of various types for comparison operations in the Unity test framework.


---
### UnityOptionIncludeNamed
- **Type**: `char*`
- **Description**: `UnityOptionIncludeNamed` is a global variable of type `char*` that is initialized to `NULL`. It is used to store a string that specifies which test names should be included when running tests in the Unity test framework.
- **Use**: This variable is used to filter and include specific tests based on their names when executing tests via command line arguments.


---
### UnityOptionExcludeNamed
- **Type**: `char*`
- **Description**: `UnityOptionExcludeNamed` is a global variable of type `char*` that is initialized to `NULL`. It is used to store a string that specifies test names to be excluded during test execution.
- **Use**: This variable is used to hold the name of tests that should be excluded from running, based on command line arguments.


---
### UnityVerbosity
- **Type**: `int`
- **Description**: `UnityVerbosity` is a global integer variable initialized to 1, which is used to control the verbosity level of the Unity test framework's output.
- **Use**: This variable is used to adjust the amount of detail provided in the test output, with different integer values representing different verbosity levels.


# Functions

---
### UnityPrint<!-- {{#callable:UnityPrint}} -->
The `UnityPrint` function outputs a given string, handling special characters and escape sequences appropriately for display.
- **Inputs**:
    - `string`: A pointer to a constant character string that needs to be printed.
- **Control Flow**:
    - Initialize a pointer `pch` to the input string.
    - Check if `pch` is not NULL to proceed with printing.
    - Iterate over each character in the string until the null terminator is reached.
    - For each character, check if it is a printable ASCII character (between 32 and 126 inclusive) and print it directly using `UNITY_OUTPUT_CHAR`.
    - If the character is a carriage return (ASCII 13), print it as an escaped sequence '\r'.
    - If the character is a line feed (ASCII 10), print it as an escaped sequence '\n'.
    - If `UNITY_OUTPUT_COLOR` is defined, handle ANSI escape codes by printing them until the 'm' character is reached.
    - For other non-printable characters, print them as hexadecimal codes prefixed by '\x' using [`UnityPrintNumberHex`](#UnityPrintNumberHex).
    - Increment the pointer `pch` to process the next character.
- **Output**: The function does not return a value; it outputs the processed string to the designated output using `UNITY_OUTPUT_CHAR`.
- **Functions called**:
    - [`UnityPrintNumberHex`](#UnityPrintNumberHex)


---
### UnityPrintLen<!-- {{#callable:UnityPrintLen}} -->
The `UnityPrintLen` function prints a specified number of characters from a string, handling special characters by escaping them or printing their hexadecimal representation.
- **Inputs**:
    - `string`: A pointer to the string to be printed.
    - `length`: The maximum number of characters to print from the string.
- **Control Flow**:
    - Initialize a pointer `pch` to the start of the input string.
    - Check if the string pointer `pch` is not NULL.
    - Enter a loop that continues as long as the current character is not null and the number of characters printed is less than the specified length.
    - Within the loop, check if the current character is a printable ASCII character (between 32 and 126 inclusive) and print it directly using `UNITY_OUTPUT_CHAR`.
    - If the character is a carriage return (ASCII 13), print '\r' using `UNITY_OUTPUT_CHAR`.
    - If the character is a line feed (ASCII 10), print '\n' using `UNITY_OUTPUT_CHAR`.
    - For other non-printable characters, print '\x' followed by the character's hexadecimal value using [`UnityPrintNumberHex`](#UnityPrintNumberHex).
    - Increment the pointer `pch` to move to the next character in the string.
- **Output**: The function does not return a value; it outputs characters to a specified output stream or device.
- **Functions called**:
    - [`UnityPrintNumberHex`](#UnityPrintNumberHex)


---
### UnityPrintNumberByStyle<!-- {{#callable:UnityPrintNumberByStyle}} -->
The function `UnityPrintNumberByStyle` prints a number in a specific format based on the provided display style.
- **Inputs**:
    - `number`: An integer value to be printed.
    - `style`: A display style flag that determines how the number should be printed (e.g., as an integer, unsigned integer, or hexadecimal).
- **Control Flow**:
    - Check if the style indicates an integer range using a bitwise AND operation with `UNITY_DISPLAY_RANGE_INT`.
    - If true, call [`UnityPrintNumber`](#UnityPrintNumber) to print the number as an integer.
    - Otherwise, check if the style indicates an unsigned integer range using a bitwise AND operation with `UNITY_DISPLAY_RANGE_UINT`.
    - If true, call [`UnityPrintNumberUnsigned`](#UnityPrintNumberUnsigned) to print the number as an unsigned integer.
    - If neither condition is met, print the number as a hexadecimal by first outputting '0x' and then calling [`UnityPrintNumberHex`](#UnityPrintNumberHex) with the number and the number of nibbles to print, determined by the style.
- **Output**: The function does not return a value; it outputs the formatted number to the designated output stream.
- **Functions called**:
    - [`UnityPrintNumber`](#UnityPrintNumber)
    - [`UnityPrintNumberUnsigned`](#UnityPrintNumberUnsigned)
    - [`UnityPrintNumberHex`](#UnityPrintNumberHex)


---
### UnityPrintNumber<!-- {{#callable:UnityPrintNumber}} -->
The `UnityPrintNumber` function prints an integer number, handling negative values by printing a '-' sign and converting the number to an unsigned integer before printing.
- **Inputs**:
    - `number_to_print`: An integer (`UNITY_INT`) that represents the number to be printed.
- **Control Flow**:
    - The function casts the input integer to an unsigned integer.
    - It checks if the input number is negative.
    - If negative, it prints a '-' character and converts the number to a positive unsigned integer.
    - It calls [`UnityPrintNumberUnsigned`](#UnityPrintNumberUnsigned) to print the unsigned integer.
- **Output**: The function does not return a value; it outputs the number to a character output function, typically for display or logging purposes.
- **Functions called**:
    - [`UnityPrintNumberUnsigned`](#UnityPrintNumberUnsigned)


---
### UnityPrintNumberUnsigned<!-- {{#callable:UnityPrintNumberUnsigned}} -->
The `UnityPrintNumberUnsigned` function prints an unsigned integer to the output character by character.
- **Inputs**:
    - `number`: An unsigned integer (`UNITY_UINT`) that is to be printed.
- **Control Flow**:
    - Initialize a divisor to 1.
    - Determine the initial divisor by multiplying it by 10 until the number divided by the divisor is less than or equal to 9.
    - In a loop, calculate the current digit by dividing the number by the divisor and taking the modulus 10, then print the corresponding character by adding '0'.
    - Divide the divisor by 10 and repeat the loop until the divisor is greater than 0.
- **Output**: The function does not return a value; it outputs each digit of the number as a character using the `UNITY_OUTPUT_CHAR` function.


---
### UnityPrintNumberHex<!-- {{#callable:UnityPrintNumberHex}} -->
The function `UnityPrintNumberHex` prints a given unsigned integer in hexadecimal format, using a specified number of nibbles.
- **Inputs**:
    - `number`: An unsigned integer (`UNITY_UINT`) that is to be printed in hexadecimal format.
    - `nibbles_to_print`: A character indicating the number of nibbles to print from the number.
- **Control Flow**:
    - The function first checks if the number of nibbles to print exceeds the maximum possible for the given number size, and adjusts it if necessary.
    - A loop iterates over each nibble, starting from the most significant nibble, and extracts it by shifting the number right by the appropriate number of bits and masking with 0x0F.
    - Each extracted nibble is converted to its corresponding hexadecimal character ('0'-'9' or 'A'-'F') and printed using `UNITY_OUTPUT_CHAR`.
- **Output**: The function does not return a value; it outputs the hexadecimal representation of the number directly through the `UNITY_OUTPUT_CHAR` function.


---
### UnityPrintMask<!-- {{#callable:UnityPrintMask}} -->
The `UnityPrintMask` function prints a binary representation of a number, using a mask to determine which bits to display as '1', '0', or 'X'.
- **Inputs**:
    - `mask`: A `UNITY_UINT` value used to determine which bits of the number should be printed as '1' or '0', and which should be printed as 'X'.
    - `number`: A `UNITY_UINT` value whose binary representation is printed, with bits masked by the `mask` parameter.
- **Control Flow**:
    - Initialize `current_bit` to the highest bit position of a `UNITY_UINT` by left-shifting 1 by `UNITY_INT_WIDTH - 1` bits.
    - Iterate over each bit position from the most significant to the least significant (total `UNITY_INT_WIDTH` iterations).
    - For each bit position, check if the corresponding bit in `mask` is set.
    - If the bit in `mask` is set, check if the corresponding bit in `number` is set and print '1' if true, otherwise print '0'.
    - If the bit in `mask` is not set, print 'X'.
    - Right-shift `current_bit` by one to move to the next lower bit position.
- **Output**: The function outputs characters ('1', '0', or 'X') to represent the masked binary form of the `number`, using the `UNITY_OUTPUT_CHAR` function to print each character.


---
### UnityPrintFloat<!-- {{#callable:UnityPrintFloat}} -->
The `UnityPrintFloat` function prints a floating-point number in a format similar to `printf("%.6g")`, handling special cases like zero, NaN, and infinity, and adjusting for significant digits and scientific notation as needed.
- **Inputs**:
    - `input_number`: A floating-point number of type `UNITY_DOUBLE` to be printed.
- **Control Flow**:
    - The function first checks if the number is negative or negative zero and prints a minus sign if true, then makes the number positive.
    - It handles special cases: prints '0' for zero, 'nan' for NaN, and 'inf' for infinity.
    - For other numbers, it scales the number to fit within a specific range by multiplying or dividing by powers of 10, adjusting the exponent accordingly.
    - The number is rounded to the nearest integer, and trailing zeros are truncated after determining the decimal point position.
    - The digits are stored in a buffer in reverse order and printed, inserting a decimal point at the correct position.
    - If the exponent is non-zero, it prints the exponent in scientific notation format.
- **Output**: The function outputs the formatted floating-point number to the standard output using the `UNITY_OUTPUT_CHAR` function.
- **Functions called**:
    - [`UnityPrint`](#UnityPrint)


---
### UnityTestResultsBegin<!-- {{#callable:UnityTestResultsBegin}} -->
The `UnityTestResultsBegin` function initializes the output of test results by printing the file name, line number, and current test name in a specific format.
- **Inputs**:
    - `file`: A constant character pointer representing the name of the file where the test is located.
    - `line`: A constant of type `UNITY_LINE_TYPE` representing the line number in the file where the test is located.
- **Control Flow**:
    - The function begins by calling [`UnityPrint`](#UnityPrint) to print the file name.
    - It then outputs a colon character using `UNITY_OUTPUT_CHAR`.
    - Next, it prints the line number by converting it to an integer and calling [`UnityPrintNumber`](#UnityPrintNumber).
    - Another colon character is outputted using `UNITY_OUTPUT_CHAR`.
    - Finally, it prints the current test name stored in `Unity.CurrentTestName` and outputs a colon character.
- **Output**: The function does not return any value; it outputs formatted test result information to the console or specified output.
- **Functions called**:
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintNumber`](#UnityPrintNumber)


---
### UnityTestResultsFailBegin<!-- {{#callable:UnityTestResultsFailBegin}} -->
The function `UnityTestResultsFailBegin` initializes the test result output for a failed test case by printing the test file, line number, and a 'FAIL' message.
- **Inputs**:
    - `line`: A `UNITY_LINE_TYPE` value representing the line number where the test failure occurred.
- **Control Flow**:
    - Calls [`UnityTestResultsBegin`](#UnityTestResultsBegin) with the current test file and line number to start the test result output.
    - Prints the string 'FAIL' to indicate a test failure.
    - Outputs a colon character ':' to separate the failure message from subsequent output.
- **Output**: This function does not return a value; it outputs formatted test failure information to the test result output stream.
- **Functions called**:
    - [`UnityTestResultsBegin`](#UnityTestResultsBegin)
    - [`UnityPrint`](#UnityPrint)


---
### UnityConcludeTest<!-- {{#callable:UnityConcludeTest}} -->
The `UnityConcludeTest` function finalizes the results of a test by updating counters for ignored or failed tests and printing the test result.
- **Inputs**: None
- **Control Flow**:
    - Check if the current test was ignored; if so, increment the `TestIgnores` counter.
    - If the test was not ignored and did not fail, print the test result as 'PASS'.
    - If the test failed, increment the `TestFailures` counter.
    - Reset the `CurrentTestFailed` and `CurrentTestIgnored` flags to 0.
    - Print a newline and flush the output buffer.
- **Output**: The function does not return a value; it updates global state and outputs test results.
- **Functions called**:
    - [`UnityTestResultsBegin`](#UnityTestResultsBegin)
    - [`UnityPrint`](#UnityPrint)


---
### UnityAddMsgIfSpecified<!-- {{#callable:UnityAddMsgIfSpecified}} -->
The `UnityAddMsgIfSpecified` function conditionally prints a message and additional details if specified.
- **Inputs**:
    - `msg`: A constant character pointer to the message string that may be printed.
- **Control Flow**:
    - Check if the `msg` is not NULL.
    - If `msg` is not NULL, print a spacer string using [`UnityPrint`](#UnityPrint).
    - If `UNITY_EXCLUDE_DETAILS` is not defined, check if `Unity.CurrentDetail1` is not NULL.
    - If `Unity.CurrentDetail1` is not NULL, print `UnityStrDetail1Name` and `Unity.CurrentDetail1`.
    - Check if `Unity.CurrentDetail2` is not NULL and if so, print `UnityStrDetail2Name` and `Unity.CurrentDetail2`.
    - Print another spacer string using [`UnityPrint`](#UnityPrint).
    - Finally, print the `msg` using [`UnityPrint`](#UnityPrint).
- **Output**: The function does not return any value; it performs printing operations based on the input and conditions.
- **Functions called**:
    - [`UnityPrint`](#UnityPrint)


---
### UnityPrintExpectedAndActualStrings<!-- {{#callable:UnityPrintExpectedAndActualStrings}} -->
The function `UnityPrintExpectedAndActualStrings` prints the expected and actual string values, handling NULL pointers by printing 'NULL' instead.
- **Inputs**:
    - `expected`: A pointer to the expected string value, which can be NULL.
    - `actual`: A pointer to the actual string value, which can also be NULL.
- **Control Flow**:
    - The function starts by printing the string ' Expected '.
    - It checks if the 'expected' pointer is not NULL; if true, it prints the expected string enclosed in single quotes, otherwise it prints 'NULL'.
    - It then prints the string ' Was '.
    - Similarly, it checks if the 'actual' pointer is not NULL; if true, it prints the actual string enclosed in single quotes, otherwise it prints 'NULL'.
- **Output**: The function does not return a value; it outputs the formatted expected and actual strings to the Unity output stream.
- **Functions called**:
    - [`UnityPrint`](#UnityPrint)


---
### UnityPrintExpectedAndActualStringsLen<!-- {{#callable:UnityPrintExpectedAndActualStringsLen}} -->
The function `UnityPrintExpectedAndActualStringsLen` prints the expected and actual strings up to a specified length, handling NULL pointers by printing 'NULL' instead.
- **Inputs**:
    - `expected`: A pointer to the expected string to be printed.
    - `actual`: A pointer to the actual string to be printed.
    - `length`: The maximum number of characters to print from each string.
- **Control Flow**:
    - Prints the string 'Expected '.
    - Checks if the 'expected' string is not NULL; if not, it prints the string enclosed in single quotes up to the specified length using [`UnityPrintLen`](#UnityPrintLen); otherwise, it prints 'NULL'.
    - Prints the string ' Was '.
    - Checks if the 'actual' string is not NULL; if not, it prints the string enclosed in single quotes up to the specified length using [`UnityPrintLen`](#UnityPrintLen); otherwise, it prints 'NULL'.
- **Output**: The function does not return a value; it outputs the formatted strings to the Unity test output.
- **Functions called**:
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintLen`](#UnityPrintLen)


---
### UnityIsOneArrayNull<!-- {{#callable:UnityIsOneArrayNull}} -->
The function `UnityIsOneArrayNull` checks if either of two pointers is NULL and reports a failure if so.
- **Inputs**:
    - `expected`: A pointer that is expected to be non-NULL.
    - `actual`: A pointer that is expected to be non-NULL.
    - `lineNumber`: The line number in the source code where the check is being performed.
    - `msg`: An optional message to be printed if a failure is detected.
- **Control Flow**:
    - Check if `expected` and `actual` are the same; if so, return 0 indicating no NULL pointers.
    - If `expected` is NULL, begin failure reporting, print a message indicating the expected pointer is NULL, add any specified message, and return 1.
    - If `actual` is NULL, begin failure reporting, print a message indicating the actual pointer is NULL, add any specified message, and return 1.
    - If neither pointer is NULL, return 0 indicating no NULL pointers.
- **Output**: Returns 1 if either `expected` or `actual` is NULL, otherwise returns 0.
- **Functions called**:
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrint`](#UnityPrint)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityAssertBits<!-- {{#callable:UnityAssertBits}} -->
The `UnityAssertBits` function checks if the bits specified by a mask in two integers are equal, and reports a test failure if they are not.
- **Inputs**:
    - `mask`: A bitmask used to select specific bits for comparison.
    - `expected`: The expected integer value whose bits are to be compared.
    - `actual`: The actual integer value whose bits are to be compared.
    - `msg`: An optional message to be printed if the assertion fails.
    - `lineNumber`: The line number in the source code where the assertion is being made.
- **Control Flow**:
    - The function first checks if the current test has already failed or been ignored using the `RETURN_IF_FAIL_OR_IGNORE` macro, and returns immediately if so.
    - It then compares the bits of `expected` and `actual` that are selected by `mask`.
    - If the masked bits of `expected` and `actual` are not equal, it begins the process of reporting a test failure.
    - The function calls [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin) to start the failure report, passing the line number.
    - It prints the expected and actual values using [`UnityPrintMask`](#UnityPrintMask), which displays the bits according to the mask.
    - If a message is provided, it is printed using [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified).
    - Finally, the function uses the `UNITY_FAIL_AND_BAIL` macro to mark the test as failed and abort further execution.
- **Output**: The function does not return a value; it either completes successfully or aborts the test with a failure report.
- **Functions called**:
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintMask`](#UnityPrintMask)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityAssertEqualNumber<!-- {{#callable:UnityAssertEqualNumber}} -->
The function `UnityAssertEqualNumber` checks if two integer values are equal and reports a test failure if they are not.
- **Inputs**:
    - `expected`: The expected integer value to compare against.
    - `actual`: The actual integer value to be compared.
    - `msg`: An optional message to include in the failure report.
    - `lineNumber`: The line number in the source code where the assertion is made.
    - `style`: The display style for printing the numbers, which determines how the numbers are formatted in the output.
- **Control Flow**:
    - The function first checks if the current test has already failed or been ignored using the macro `RETURN_IF_FAIL_OR_IGNORE` and returns immediately if so.
    - It then compares the `expected` and `actual` values for equality.
    - If the values are not equal, it begins the failure report by calling [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin) with the `lineNumber`.
    - It prints the expected value using [`UnityPrintNumberByStyle`](#UnityPrintNumberByStyle) with the `style` parameter.
    - It prints the actual value using [`UnityPrintNumberByStyle`](#UnityPrintNumberByStyle) with the `style` parameter.
    - If a message `msg` is provided, it is added to the output using [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified).
    - Finally, it marks the test as failed and aborts further execution using the macro `UNITY_FAIL_AND_BAIL`.
- **Output**: The function does not return a value; it outputs test results and may abort the test execution if the assertion fails.
- **Functions called**:
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintNumberByStyle`](#UnityPrintNumberByStyle)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityAssertGreaterOrLessOrEqualNumber<!-- {{#callable:UnityAssertGreaterOrLessOrEqualNumber}} -->
The function `UnityAssertGreaterOrLessOrEqualNumber` checks if a given number meets a specified comparison condition against a threshold and reports a test failure if it does not.
- **Inputs**:
    - `threshold`: The reference value against which the actual value is compared.
    - `actual`: The actual value to be compared with the threshold.
    - `compare`: A bitmask indicating the type of comparison to perform (e.g., greater than, less than, or equal to).
    - `msg`: An optional message to include in the test output if the assertion fails.
    - `lineNumber`: The line number in the source code where the assertion is being made, used for reporting.
    - `style`: The display style for the numbers, indicating whether they are signed, unsigned, or hexadecimal.
- **Control Flow**:
    - Initialize a failure flag to 0 and return immediately if the current test has already failed or been ignored.
    - Check if the threshold equals the actual value and the comparison includes equality; if so, return without failure.
    - If the threshold equals the actual value but equality is not part of the comparison, set the failure flag to 1.
    - Determine if the style indicates signed integers; if so, perform signed comparisons to set the failure flag based on the comparison type.
    - If the style indicates unsigned integers or hexadecimal, cast the values to unsigned and perform the comparisons accordingly.
    - If the failure flag is set, begin the test failure report, print the expected and actual values, the comparison type, and any specified message, then mark the test as failed and abort.
- **Output**: The function does not return a value but will report a test failure and abort the test if the comparison condition is not met.
- **Functions called**:
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintNumberByStyle`](#UnityPrintNumberByStyle)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityAssertEqualIntArray<!-- {{#callable:UnityAssertEqualIntArray}} -->
The function `UnityAssertEqualIntArray` compares two integer arrays for equality and reports any discrepancies in a test framework.
- **Inputs**:
    - `expected`: A pointer to the expected integer array.
    - `actual`: A pointer to the actual integer array to compare against the expected array.
    - `num_elements`: The number of elements in each array to compare.
    - `msg`: An optional message to display if the assertion fails.
    - `lineNumber`: The line number in the test file where the assertion is made.
    - `style`: The display style for the numbers, which includes information about the size of the integers.
    - `flags`: Flags that modify the behavior of the comparison, such as whether to treat the arrays as arrays of arrays.
- **Control Flow**:
    - Check if the current test has already failed or been ignored, and return if so.
    - If `num_elements` is zero, print a message indicating a pointless comparison and bail out.
    - If `expected` and `actual` are the same pointer, return as they are trivially equal.
    - Check if either `expected` or `actual` is NULL and handle accordingly, failing the test if one is NULL.
    - Iterate over each element in the arrays, comparing them based on the specified integer size (1, 2, 4, or 8 bytes).
    - If a mismatch is found, handle unsigned integer display by masking sign extension if necessary, then print the failure details and bail out.
    - Adjust the pointers for the next element based on the specified flags.
- **Output**: The function does not return a value; it reports test results through the Unity test framework, marking the test as failed if the arrays are not equal.
- **Functions called**:
    - [`UnityIsOneArrayNull`](#UnityIsOneArrayNull)
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintNumberUnsigned`](#UnityPrintNumberUnsigned)
    - [`UnityPrintNumberByStyle`](#UnityPrintNumberByStyle)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityFloatsWithin<!-- {{#callable:UnityFloatsWithin}} -->
The `UnityFloatsWithin` function checks if the absolute difference between two floating-point numbers is within a specified delta.
- **Inputs**:
    - `delta`: The maximum allowable difference between the expected and actual floating-point numbers.
    - `expected`: The expected floating-point value.
    - `actual`: The actual floating-point value to compare against the expected value.
- **Control Flow**:
    - Calculate the difference between the actual and expected values.
    - Take the absolute value of the difference and the delta.
    - Return true if the difference is not NaN or infinite and is less than or equal to the delta, otherwise return false.
- **Output**: Returns an integer indicating whether the actual value is within the delta of the expected value (1 for true, 0 for false).


---
### UnityAssertEqualFloatArray<!-- {{#callable:UnityAssertEqualFloatArray}} -->
The `UnityAssertEqualFloatArray` function checks if two arrays of floating-point numbers are equal within a specified precision and reports any discrepancies.
- **Inputs**:
    - `expected`: A pointer to the array of expected floating-point values.
    - `actual`: A pointer to the array of actual floating-point values to compare against the expected values.
    - `num_elements`: The number of elements in each of the arrays to compare.
    - `msg`: An optional message to include in the test results if the assertion fails.
    - `lineNumber`: The line number in the source code where the assertion is being made, used for reporting.
    - `flags`: Flags that determine the behavior of the comparison, such as whether to treat the arrays as array-to-array or array-to-value.
- **Control Flow**:
    - Check if the current test has already failed or been ignored, and return if so.
    - If the number of elements is zero, print a message indicating a pointless comparison and abort the test.
    - If the expected and actual pointers are the same, return as they are either both NULL or point to the same data.
    - Check if one of the arrays is NULL and handle the failure if so.
    - Iterate over each element in the arrays, comparing them using the [`UnityFloatsWithin`](#UnityFloatsWithin) function to check if they are within a specified precision.
    - If a discrepancy is found, report the failure with details about the element index and the expected and actual values, then abort the test.
    - If the `flags` parameter indicates array-to-array comparison, increment the expected pointer to the next element.
    - Always increment the actual pointer to the next element.
- **Output**: The function does not return a value; it reports test results and may abort the test if the arrays are not equal.
- **Functions called**:
    - [`UnityIsOneArrayNull`](#UnityIsOneArrayNull)
    - [`UnityFloatsWithin`](#UnityFloatsWithin)
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintNumberUnsigned`](#UnityPrintNumberUnsigned)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityAssertFloatsWithin<!-- {{#callable:UnityAssertFloatsWithin}} -->
The `UnityAssertFloatsWithin` function checks if two floating-point numbers are within a specified delta of each other and handles test failure if they are not.
- **Inputs**:
    - `delta`: The maximum allowable difference between the expected and actual floating-point values for the test to pass.
    - `expected`: The expected floating-point value in the test.
    - `actual`: The actual floating-point value obtained in the test.
    - `msg`: An optional message to be printed if the test fails.
    - `lineNumber`: The line number in the source code where the test is being performed, used for reporting test results.
- **Control Flow**:
    - The function first checks if the current test has already failed or been ignored using the `RETURN_IF_FAIL_OR_IGNORE` macro, and returns immediately if so.
    - It calls the [`UnityFloatsWithin`](#UnityFloatsWithin) function to determine if the `actual` value is within `delta` of the `expected` value.
    - If [`UnityFloatsWithin`](#UnityFloatsWithin) returns false, indicating the values are not within the specified delta, the function begins the process of reporting a test failure.
    - It calls [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin) to start the failure report, passing the line number where the failure occurred.
    - The function then prints the expected and actual float values using `UNITY_PRINT_EXPECTED_AND_ACTUAL_FLOAT`.
    - If a message is specified, it is added to the failure report using [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified).
    - Finally, the function uses the `UNITY_FAIL_AND_BAIL` macro to mark the test as failed and abort further execution.
- **Output**: The function does not return a value; it either completes successfully if the test passes or aborts the test execution if the test fails.
- **Functions called**:
    - [`UnityFloatsWithin`](#UnityFloatsWithin)
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityAssertFloatSpecial<!-- {{#callable:UnityAssertFloatSpecial}} -->
The function `UnityAssertFloatSpecial` checks if a floating-point number matches a specified trait (e.g., infinity, NaN, determinate) and reports a test failure if it does not.
- **Inputs**:
    - `actual`: The floating-point number to be checked.
    - `msg`: An optional message to be printed if the assertion fails.
    - `lineNumber`: The line number in the test file where the assertion is made.
    - `style`: The trait style to check against, indicating the expected characteristic of the floating-point number (e.g., infinity, NaN).
- **Control Flow**:
    - Initialize an array `trait_names` with string representations of float traits (Infinity, Negative Infinity, NaN, Determinate).
    - Determine `should_be_trait` based on the least significant bit of `style`, indicating if the trait should be present or not.
    - Initialize `is_trait` to the opposite of `should_be_trait` and `trait_index` to the index derived from `style` shifted right by one bit.
    - Check if the current test has already failed or been ignored using `RETURN_IF_FAIL_OR_IGNORE` macro, and return if so.
    - Use a switch statement on `style` to determine if `actual` matches the specified trait, updating `is_trait` accordingly.
    - If `is_trait` does not match `should_be_trait`, begin test failure reporting with [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin).
    - Print the expected trait using [`UnityPrint`](#UnityPrint), including 'Not' if `should_be_trait` is false.
    - Print the actual value of `actual` using [`UnityPrintFloat`](#UnityPrintFloat) if float printing is not excluded, otherwise print the trait name again.
    - Add any specified message using [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified) and conclude with `UNITY_FAIL_AND_BAIL` to mark the test as failed.
- **Output**: The function does not return a value but will report a test failure if the `actual` value does not match the expected trait, potentially printing a message and the actual value.
- **Functions called**:
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintFloat`](#UnityPrintFloat)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityDoublesWithin<!-- {{#callable:UnityDoublesWithin}} -->
The function `UnityDoublesWithin` checks if two double precision floating-point numbers are within a specified delta of each other.
- **Inputs**:
    - `delta`: The maximum allowable difference between the expected and actual double values.
    - `expected`: The expected double value.
    - `actual`: The actual double value to compare against the expected value.
- **Control Flow**:
    - Calculate the difference between the actual and expected values.
    - Check if both expected and actual are infinite with the same sign, returning true if so.
    - Check if both expected and actual are NaN, returning true if so.
    - Convert the difference to its absolute value.
    - Convert the delta to its absolute value.
    - Return true if the absolute difference is not NaN, not infinite, and less than or equal to the delta; otherwise, return false.
- **Output**: Returns an integer (1 or 0) indicating whether the actual value is within the delta of the expected value.


---
### UnityAssertEqualDoubleArray<!-- {{#callable:UnityAssertEqualDoubleArray}} -->
The function `UnityAssertEqualDoubleArray` checks if two arrays of double-precision floating-point numbers are equal within a specified precision and reports any discrepancies.
- **Inputs**:
    - `expected`: A pointer to the expected array of double-precision floating-point numbers.
    - `actual`: A pointer to the actual array of double-precision floating-point numbers to compare against the expected array.
    - `num_elements`: The number of elements in each of the arrays to be compared.
    - `msg`: An optional message to include in the test results if the arrays are not equal.
    - `lineNumber`: The line number in the source code where the test is being performed, used for reporting purposes.
    - `flags`: Flags that determine the behavior of the comparison, such as whether to treat the arrays as array-to-array or array-to-value.
- **Control Flow**:
    - Check if the current test has already failed or been ignored, and return immediately if so.
    - If the number of elements is zero, print a message indicating a pointless comparison and abort the test.
    - If the expected and actual pointers are the same, return immediately as they are considered equal.
    - Check if either the expected or actual array is NULL, and if so, report a failure and abort the test.
    - Iterate over each element in the arrays, comparing the corresponding elements from the expected and actual arrays.
    - For each element, check if the actual value is within a specified precision of the expected value using [`UnityDoublesWithin`](#UnityDoublesWithin).
    - If any element is not within the specified precision, report a failure with details of the discrepancy and abort the test.
    - If the `flags` parameter indicates array-to-array comparison, increment the expected pointer to the next element.
    - Always increment the actual pointer to the next element.
- **Output**: The function does not return a value; it reports test results and may abort the test if the arrays are not equal.
- **Functions called**:
    - [`UnityIsOneArrayNull`](#UnityIsOneArrayNull)
    - [`UnityDoublesWithin`](#UnityDoublesWithin)
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintNumberUnsigned`](#UnityPrintNumberUnsigned)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityAssertDoublesWithin<!-- {{#callable:UnityAssertDoublesWithin}} -->
The function `UnityAssertDoublesWithin` checks if two double values are within a specified delta and handles test failure if they are not.
- **Inputs**:
    - `delta`: The maximum allowable difference between the expected and actual double values.
    - `expected`: The expected double value in the test.
    - `actual`: The actual double value obtained in the test.
    - `msg`: An optional message to display if the assertion fails.
    - `lineNumber`: The line number in the source code where the assertion is being made.
- **Control Flow**:
    - The function first checks if the current test has already failed or been ignored using the macro `RETURN_IF_FAIL_OR_IGNORE` and returns immediately if so.
    - It calls [`UnityDoublesWithin`](#UnityDoublesWithin) to determine if the `actual` value is within `delta` of the `expected` value.
    - If [`UnityDoublesWithin`](#UnityDoublesWithin) returns false, indicating the values are not within the specified delta, the function begins handling the test failure.
    - It calls [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin) to start recording the failure, passing the `lineNumber`.
    - It prints the expected and actual values using `UNITY_PRINT_EXPECTED_AND_ACTUAL_FLOAT`.
    - If a message `msg` is provided, it is added to the output using [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified).
    - Finally, it marks the test as failed and aborts further execution using `UNITY_FAIL_AND_BAIL`.
- **Output**: The function does not return a value; it either completes successfully or aborts the test execution if the assertion fails.
- **Functions called**:
    - [`UnityDoublesWithin`](#UnityDoublesWithin)
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityAssertDoubleSpecial<!-- {{#callable:UnityAssertDoubleSpecial}} -->
The function `UnityAssertDoubleSpecial` checks if a given double value matches a specified floating-point trait (such as infinity, negative infinity, NaN, or determinate) and reports a test failure if it does not match the expected trait.
- **Inputs**:
    - `actual`: The double value to be checked against the specified trait.
    - `msg`: An optional message to be printed if the assertion fails.
    - `lineNumber`: The line number in the test file where the assertion is made.
    - `style`: The expected floating-point trait, specified as a `UNITY_FLOAT_TRAIT_T` enum value, indicating the trait to check for (e.g., infinity, NaN).
- **Control Flow**:
    - Initialize an array `trait_names` with string representations of possible traits (Infinity, Negative Infinity, NaN, Determinate).
    - Determine `should_be_trait` and `is_trait` based on the `style` parameter, where `should_be_trait` indicates if the trait should be present or not, and `is_trait` is initially set to the opposite of `should_be_trait`.
    - Determine `trait_index` by shifting the `style` parameter right by one bit to identify the specific trait index.
    - Check if the current test has already failed or been ignored using `RETURN_IF_FAIL_OR_IGNORE` macro, and return if so.
    - Use a switch statement on `style` to set `is_trait` based on whether `actual` matches the specified trait (e.g., using `isinf` and `isnan` functions).
    - If `is_trait` does not match `should_be_trait`, begin a test failure report using [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin).
    - Print the expected trait using [`UnityPrint`](#UnityPrint), and if `should_be_trait` is false, print 'Not' before the trait name.
    - Print the actual value using [`UnityPrintFloat`](#UnityPrintFloat) if floating-point printing is not excluded, otherwise print the trait name again.
    - Add any specified message using [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified).
    - Invoke `UNITY_FAIL_AND_BAIL` to mark the test as failed and abort further execution.
- **Output**: The function does not return a value; it either completes successfully or triggers a test failure and aborts execution if the assertion fails.
- **Functions called**:
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintFloat`](#UnityPrintFloat)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityAssertNumbersWithin<!-- {{#callable:UnityAssertNumbersWithin}} -->
The `UnityAssertNumbersWithin` function checks if two numbers are within a specified delta and records a test failure if they are not.
- **Inputs**:
    - `delta`: The maximum allowable difference between the expected and actual values for the test to pass.
    - `expected`: The expected value in the test.
    - `actual`: The actual value obtained in the test.
    - `msg`: An optional message to display if the test fails.
    - `lineNumber`: The line number in the source code where the test is being performed.
    - `style`: The display style for the numbers, indicating whether they are signed, unsigned, or hexadecimal.
- **Control Flow**:
    - The function first checks if the current test has already failed or been ignored using the `RETURN_IF_FAIL_OR_IGNORE` macro.
    - It determines if the numbers are signed integers by checking the `style` parameter.
    - If the numbers are signed, it calculates the difference between `actual` and `expected` and checks if it exceeds `delta`.
    - If the numbers are unsigned, it performs a similar check using unsigned arithmetic.
    - If the difference exceeds `delta`, it sets `Unity.CurrentTestFailed` to true.
    - If the test fails, it begins recording the failure with [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin), prints the delta, expected, and actual values, and adds any specified message.
    - Finally, it calls `UNITY_FAIL_AND_BAIL` to handle the failure.
- **Output**: The function does not return a value but sets `Unity.CurrentTestFailed` to indicate if the test failed and outputs failure details if applicable.
- **Functions called**:
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintNumberByStyle`](#UnityPrintNumberByStyle)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityAssertEqualString<!-- {{#callable:UnityAssertEqualString}} -->
The `UnityAssertEqualString` function compares two strings for equality and handles test failure reporting if they are not equal.
- **Inputs**:
    - `expected`: A pointer to the expected string that the actual string is compared against.
    - `actual`: A pointer to the actual string that is being tested.
    - `msg`: An optional message to include in the test results if the strings are not equal.
    - `lineNumber`: The line number in the test file where the assertion is made, used for reporting purposes.
- **Control Flow**:
    - The function begins by checking if the current test has already failed or been ignored, returning immediately if so.
    - If both `expected` and `actual` pointers are not null, it enters a loop to compare each character of the strings until a difference is found or the end of either string is reached.
    - If a difference is found, it sets `Unity.CurrentTestFailed` to 1 and breaks out of the loop.
    - If either `expected` or `actual` is null, it checks if they are not equal, setting `Unity.CurrentTestFailed` to 1 if they differ.
    - If `Unity.CurrentTestFailed` is set, it calls [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin) to start failure reporting, prints the expected and actual strings, adds any specified message, and then calls `UNITY_FAIL_AND_BAIL` to handle the failure.
- **Output**: The function does not return a value; it modifies the global state to indicate test failure and handles reporting if the strings are not equal.
- **Functions called**:
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrintExpectedAndActualStrings`](#UnityPrintExpectedAndActualStrings)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityAssertEqualStringLen<!-- {{#callable:UnityAssertEqualStringLen}} -->
The function `UnityAssertEqualStringLen` compares two strings up to a specified length and reports a test failure if they are not equal.
- **Inputs**:
    - `expected`: A pointer to the expected string to compare.
    - `actual`: A pointer to the actual string to compare.
    - `length`: The maximum number of characters to compare between the two strings.
    - `msg`: An optional message to include in the test results if the strings are not equal.
    - `lineNumber`: The line number in the test file where the assertion is made, used for reporting purposes.
- **Control Flow**:
    - The function first checks if the current test has already failed or been ignored using the macro `RETURN_IF_FAIL_OR_IGNORE` and returns immediately if so.
    - If both `expected` and `actual` pointers are not null, it enters a loop to compare each character of the strings up to the specified `length` or until a null character is encountered in either string.
    - If a mismatch is found during the comparison, it sets `Unity.CurrentTestFailed` to 1 and breaks out of the loop.
    - If either `expected` or `actual` is null, it checks if they are not equal and sets `Unity.CurrentTestFailed` to 1 if they differ.
    - If `Unity.CurrentTestFailed` is set, it calls [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin) to start reporting the failure, prints the expected and actual strings using [`UnityPrintExpectedAndActualStringsLen`](#UnityPrintExpectedAndActualStringsLen), adds any specified message, and then calls `UNITY_FAIL_AND_BAIL` to handle the failure.
- **Output**: The function does not return a value; it sets the `Unity.CurrentTestFailed` flag and reports the failure if the strings are not equal up to the specified length.
- **Functions called**:
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrintExpectedAndActualStringsLen`](#UnityPrintExpectedAndActualStringsLen)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityAssertEqualStringArray<!-- {{#callable:UnityAssertEqualStringArray}} -->
The function `UnityAssertEqualStringArray` compares two arrays of strings for equality and reports any mismatches as test failures.
- **Inputs**:
    - `expected`: A pointer to the expected array of strings.
    - `actual`: A pointer to the actual array of strings to be compared against the expected array.
    - `num_elements`: The number of elements (strings) in the arrays to be compared.
    - `msg`: An optional message to be printed if the test fails.
    - `lineNumber`: The line number in the test file where the assertion is made.
    - `flags`: Flags indicating the comparison mode, such as whether to compare array-to-array or array-to-value.
- **Control Flow**:
    - Check if the test should be ignored or has already failed using `RETURN_IF_FAIL_OR_IGNORE` macro.
    - If `num_elements` is zero, print an error message and bail out using `UnityPrintPointlessAndBail`.
    - If `expected` and `actual` point to the same memory location, return as they are considered equal.
    - Check if either `expected` or `actual` is NULL using [`UnityIsOneArrayNull`](#UnityIsOneArrayNull); if so, fail the test and bail out.
    - If `flags` is not `UNITY_ARRAY_TO_ARRAY`, treat `expected` as a single string rather than an array of strings.
    - Iterate over each element in the `actual` array, comparing it to the corresponding element in the `expected` array.
    - For each pair of strings, compare them character by character; if a mismatch is found, mark the test as failed.
    - If a mismatch is detected, print the failure details including the index of the mismatched element and the expected and actual strings, then bail out.
- **Output**: The function does not return a value; it reports test results by setting the `Unity.CurrentTestFailed` flag and printing messages.
- **Functions called**:
    - [`UnityIsOneArrayNull`](#UnityIsOneArrayNull)
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintNumberUnsigned`](#UnityPrintNumberUnsigned)
    - [`UnityPrintExpectedAndActualStrings`](#UnityPrintExpectedAndActualStrings)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityAssertEqualMemory<!-- {{#callable:UnityAssertEqualMemory}} -->
The `UnityAssertEqualMemory` function compares two memory blocks for equality and reports a failure if they differ.
- **Inputs**:
    - `expected`: A pointer to the expected memory block.
    - `actual`: A pointer to the actual memory block to compare against the expected.
    - `length`: The number of bytes to compare in each element.
    - `num_elements`: The number of elements to compare.
    - `msg`: An optional message to print if the assertion fails.
    - `lineNumber`: The line number in the source code where the assertion is being made.
    - `flags`: Flags that modify the behavior of the comparison, such as whether to reset the expected pointer for each element.
- **Control Flow**:
    - Check if the current test has already failed or been ignored, and return if so.
    - If either `elements` or `length` is zero, print a message and bail out.
    - If `expected` and `actual` are the same, return as they are either both NULL or the same pointer.
    - Check if one of the arrays is NULL and handle it by failing the test if so.
    - Iterate over each element, and for each element, iterate over each byte to compare the memory blocks.
    - If a mismatch is found, print detailed information about the mismatch, including the element and byte index, expected and actual values, and fail the test.
    - If the `flags` indicate `UNITY_ARRAY_TO_VAL`, reset the `expected` pointer to the start of the memory block for each element.
- **Output**: The function does not return a value but will report a test failure if the memory blocks do not match.
- **Functions called**:
    - [`UnityIsOneArrayNull`](#UnityIsOneArrayNull)
    - [`UnityTestResultsFailBegin`](#UnityTestResultsFailBegin)
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintNumberUnsigned`](#UnityPrintNumberUnsigned)
    - [`UnityPrintNumberByStyle`](#UnityPrintNumberByStyle)
    - [`UnityAddMsgIfSpecified`](#UnityAddMsgIfSpecified)


---
### UnityNumToPtr<!-- {{#callable:UnityNumToPtr}} -->
The `UnityNumToPtr` function converts a numeric value to a pointer of a specified size, storing the value in a union for quick comparison.
- **Inputs**:
    - `num`: An integer value to be converted to a pointer.
    - `size`: An unsigned 8-bit integer specifying the size of the integer type to convert to (1, 2, 4, or 8 bytes).
- **Control Flow**:
    - The function uses a switch statement to determine the size of the integer type to convert the input number to.
    - If the size is 1, the number is cast to an 8-bit integer and stored in the union's `i8` field, and a pointer to this field is returned.
    - If the size is 2, the number is cast to a 16-bit integer and stored in the union's `i16` field, and a pointer to this field is returned.
    - If the size is 8 and `UNITY_SUPPORT_64` is defined, the number is cast to a 64-bit integer and stored in the union's `i64` field, and a pointer to this field is returned.
    - For any other size (default case), the number is cast to a 32-bit integer and stored in the union's `i32` field, and a pointer to this field is returned.
- **Output**: A pointer to the union field where the converted integer is stored, cast to `UNITY_INTERNAL_PTR`.


---
### UnityFloatToPtr<!-- {{#callable:UnityFloatToPtr}} -->
The `UnityFloatToPtr` function converts a float value to a pointer of type `UNITY_INTERNAL_PTR` by storing the float in a union and returning the address of the float within the union.
- **Inputs**:
    - `num`: A float value that needs to be converted to a pointer.
- **Control Flow**:
    - The function takes a float `num` as input.
    - It assigns the float `num` to the `f` member of the `UnityQuickCompare` union.
    - The function returns the address of the `f` member, cast to `UNITY_INTERNAL_PTR`.
- **Output**: A pointer of type `UNITY_INTERNAL_PTR` pointing to the float value stored in the union.


---
### UnityDoubleToPtr<!-- {{#callable:UnityDoubleToPtr}} -->
The `UnityDoubleToPtr` function converts a double precision floating-point number to a pointer of type `UNITY_INTERNAL_PTR`.
- **Inputs**:
    - `num`: A double precision floating-point number to be converted to a pointer.
- **Control Flow**:
    - The function assigns the input double `num` to the `d` field of the `UnityQuickCompare` union.
    - It then returns the address of the `d` field cast to `UNITY_INTERNAL_PTR`.
- **Output**: A pointer of type `UNITY_INTERNAL_PTR` pointing to the location where the double is stored.


---
### UnityFail<!-- {{#callable:UnityFail}} -->
The `UnityFail` function logs a test failure message and details, then aborts the test execution.
- **Inputs**:
    - `msg`: A constant character pointer to the failure message to be printed.
    - `line`: A constant of type `UNITY_LINE_TYPE` representing the line number where the failure occurred.
- **Control Flow**:
    - Check if the current test has already failed or been ignored using `RETURN_IF_FAIL_OR_IGNORE`; if so, return immediately.
    - Begin logging test results with [`UnityTestResultsBegin`](#UnityTestResultsBegin), using the current test file and line number.
    - Print the failure message prefix using [`UnityPrint`](#UnityPrint) with `UnityStrFail`.
    - If a message (`msg`) is provided, print a colon and additional details if available, followed by the message itself.
    - Use `UNITY_FAIL_AND_BAIL` to set the test as failed and abort further execution.
- **Output**: The function does not return a value; it logs the failure and aborts the test execution.
- **Functions called**:
    - [`UnityTestResultsBegin`](#UnityTestResultsBegin)
    - [`UnityPrint`](#UnityPrint)


---
### UnityIgnore<!-- {{#callable:UnityIgnore}} -->
The `UnityIgnore` function marks a test as ignored and outputs a message if provided.
- **Inputs**:
    - `msg`: A constant character pointer to a message string that can be printed if the test is ignored.
    - `line`: A constant of type `UNITY_LINE_TYPE` representing the line number where the ignore action is invoked.
- **Control Flow**:
    - Check if the current test has already failed or been ignored using the macro `RETURN_IF_FAIL_OR_IGNORE`; if so, return immediately.
    - Begin the test result output by calling [`UnityTestResultsBegin`](#UnityTestResultsBegin) with the current test file and line number.
    - Print the string 'IGNORE' using [`UnityPrint`](#UnityPrint).
    - If a message is provided (i.e., `msg` is not NULL), print a colon and space, followed by the message using [`UnityPrint`](#UnityPrint).
    - Mark the test as ignored and abort further execution using the macro `UNITY_IGNORE_AND_BAIL`.
- **Output**: The function does not return any value; it outputs the ignore status and message to the test result output.
- **Functions called**:
    - [`UnityTestResultsBegin`](#UnityTestResultsBegin)
    - [`UnityPrint`](#UnityPrint)


---
### UnityDefaultTestRun<!-- {{#callable:UnityDefaultTestRun}} -->
The `UnityDefaultTestRun` function executes a test function within a protected environment, handling setup and teardown, and concludes the test with a result.
- **Inputs**:
    - `Func`: A function pointer to the test function that will be executed.
    - `FuncName`: A constant character pointer representing the name of the test function.
    - `FuncLineNum`: An integer representing the line number where the test function is defined.
- **Control Flow**:
    - Set the current test name and line number in the Unity framework using `FuncName` and `FuncLineNum`.
    - Increment the total number of tests executed.
    - Clear any existing test details using `UNITY_CLR_DETAILS()`.
    - Use `TEST_PROTECT()` to create a protected environment and call `setUp()` followed by the test function `Func()`.
    - Use `TEST_PROTECT()` again to ensure `tearDown()` is called after the test function execution.
    - Conclude the test by calling `UnityConcludeTest()`, which handles the test result output and resets test status flags.
- **Output**: The function does not return a value; it updates the Unity framework's internal state to reflect the test execution and results.
- **Functions called**:
    - [`setUp`](unity.h.driver.md#setUp)
    - [`tearDown`](unity.h.driver.md#tearDown)
    - [`UnityConcludeTest`](#UnityConcludeTest)


---
### UnityBegin<!-- {{#callable:UnityBegin}} -->
The `UnityBegin` function initializes the Unity test framework by setting up the test environment and preparing it for running tests.
- **Inputs**:
    - `filename`: A constant character pointer representing the name of the test file to be used in the test framework.
- **Control Flow**:
    - Assigns the provided filename to `Unity.TestFile`.
    - Initializes various Unity test framework variables such as `CurrentTestName`, `CurrentTestLineNumber`, `NumberOfTests`, `TestFailures`, `TestIgnores`, `CurrentTestFailed`, and `CurrentTestIgnored` to their default values.
    - Calls `UNITY_CLR_DETAILS()` to clear any existing test details.
    - Calls `UNITY_OUTPUT_START()` to begin the output process for the test framework.
- **Output**: The function does not return any value; it sets up the initial state for the Unity test framework.


---
### UnityEnd<!-- {{#callable:UnityEnd}} -->
The `UnityEnd` function concludes the test run by printing a summary of the test results and returning the number of test failures.
- **Inputs**: None
- **Control Flow**:
    - Prints a newline and a separator line using `UNITY_PRINT_EOL` and [`UnityPrint`](#UnityPrint) functions.
    - Prints the total number of tests, failures, and ignored tests using [`UnityPrintNumber`](#UnityPrintNumber) and [`UnityPrint`](#UnityPrint) functions.
    - Checks if there are any test failures; if none, prints 'OK', otherwise prints 'FAIL'.
    - If `UNITY_DIFFERENTIATE_FINAL_FAIL` is defined, outputs 'ED' to differentiate final failure.
    - Prints a newline, flushes the output buffer with `UNITY_FLUSH_CALL`, and completes the output with `UNITY_OUTPUT_COMPLETE`.
    - Returns the number of test failures as an integer.
- **Output**: Returns the number of test failures as an integer.
- **Functions called**:
    - [`UnityPrint`](#UnityPrint)
    - [`UnityPrintNumber`](#UnityPrintNumber)


---
### UnityParseOptions<!-- {{#callable:UnityParseOptions}} -->
The `UnityParseOptions` function processes command-line arguments to configure test execution options for the Unity test framework.
- **Inputs**:
    - `argc`: The number of command-line arguments passed to the program.
    - `argv`: An array of strings representing the command-line arguments.
- **Control Flow**:
    - Initialize `UnityOptionIncludeNamed` and `UnityOptionExcludeNamed` to NULL.
    - Iterate over each command-line argument starting from index 1.
    - Check if the argument starts with a '-' character, indicating an option.
    - Use a switch statement to handle different options based on the second character of the argument:
    - If the option is 'l', return -1 to list tests.
    - If the option is 'n' or 'f', set `UnityOptionIncludeNamed` to the specified string or the next argument, or print an error and return 1 if no string is provided.
    - If the option is 'q', set `UnityVerbosity` to 0 for quiet mode.
    - If the option is 'v', set `UnityVerbosity` to 2 for verbose mode.
    - If the option is 'x', set `UnityOptionExcludeNamed` to the specified string or the next argument, or print an error and return 1 if no string is provided.
    - For any unknown option, print an error message and return 1.
- **Output**: Returns 0 on successful parsing of options, -1 if the 'list tests' option is encountered, or 1 if an error occurs.
- **Functions called**:
    - [`UnityPrint`](#UnityPrint)


---
### IsStringInBiggerString<!-- {{#callable:IsStringInBiggerString}} -->
The function `IsStringInBiggerString` checks if a `shortstring` is present within a `longstring`, considering certain special characters as delimiters or wildcards.
- **Inputs**:
    - `longstring`: A pointer to a constant character array representing the larger string in which to search.
    - `shortstring`: A pointer to a constant character array representing the smaller string to search for within the `longstring`.
- **Control Flow**:
    - Initialize pointers `lptr` to `longstring`, `sptr` to `shortstring`, and `lnext` to `lptr`.
    - If the first character of `shortstring` is '*', return 1 immediately as it acts as a wildcard.
    - Enter a loop that continues as long as `lptr` points to a non-null character in `longstring`.
    - Set `lnext` to point to the next character after `lptr`.
    - Enter a nested loop that continues as long as both `lptr` and `sptr` point to non-null characters and the characters they point to are equal.
    - Increment both `lptr` and `sptr` to compare the next characters.
    - If `sptr` points to '*', ',', '"', '\'', or a null character, return 1, indicating a match or wildcard condition.
    - If `sptr` points to ':', return 2, indicating a special match condition.
    - If the nested loop exits without returning, reset `lptr` to `lnext` and `sptr` to the start of `shortstring` to try matching from the next position in `longstring`.
    - If the outer loop exits without finding a match, return 0.
- **Output**: Returns 1 if `shortstring` is found in `longstring` or matches up to a wildcard or delimiter, 2 if a special ':' condition is met, and 0 if no match is found.


---
### UnityStringArgumentMatches<!-- {{#callable:UnityStringArgumentMatches}} -->
The `UnityStringArgumentMatches` function checks if a given string matches certain patterns related to test file and test name in the Unity test framework.
- **Inputs**:
    - `str`: A constant character pointer representing the string to be checked for matches against test file and test name patterns.
- **Control Flow**:
    - Initialize pointers `ptr1`, `ptr2`, and `ptrf` to traverse the input string `str`.
    - Iterate through the string `str` using `ptr1` until the end of the string is reached.
    - Skip any initial quote characters in `str` by incrementing `ptr1`.
    - Set `ptr2` to `ptr1` and initialize `ptrf` to `0`.
    - Use a `do-while` loop to find the start of the next partial string, updating `ptrf` if a colon is found followed by a non-quote, non-comma character.
    - Skip any trailing colon, quote, or comma characters by incrementing `ptr2`.
    - Check for a complete filename match using [`IsStringInBiggerString`](#IsStringInBiggerString) with `Unity.TestFile` and `ptr1`; return `1` if matched.
    - If a partial filename match is found (`retval == 2`) and `ptrf` is not `0`, check for a test name match using [`IsStringInBiggerString`](#IsStringInBiggerString) with `Unity.CurrentTestName` and `ptrf`; return `1` if matched.
    - Check for a complete test name match using [`IsStringInBiggerString`](#IsStringInBiggerString) with `Unity.CurrentTestName` and `ptr1`; return `1` if matched.
    - Move `ptr1` to `ptr2` to continue checking the next substring.
    - Return `0` if no matches are found after checking all substrings.
- **Output**: An integer value, `1` if a match is found, otherwise `0`.
- **Functions called**:
    - [`IsStringInBiggerString`](#IsStringInBiggerString)


---
### UnityTestMatches<!-- {{#callable:UnityTestMatches}} -->
The `UnityTestMatches` function determines if the current test name matches specified inclusion or exclusion patterns for test execution.
- **Inputs**:
    - `None`: The function does not take any direct input parameters, but it uses global variables `UnityOptionIncludeNamed` and `UnityOptionExcludeNamed` to determine inclusion and exclusion patterns.
- **Control Flow**:
    - Initialize `retval` to the result of `UnityStringArgumentMatches(UnityOptionIncludeNamed)` if `UnityOptionIncludeNamed` is set, otherwise set `retval` to 1.
    - Check if `UnityOptionExcludeNamed` is set; if so, call `UnityStringArgumentMatches(UnityOptionExcludeNamed)` and set `retval` to 0 if it returns true.
    - Return the value of `retval`.
- **Output**: The function returns an integer, where 1 indicates the test matches the inclusion criteria and does not match the exclusion criteria, and 0 indicates it does not match the criteria for execution.
- **Functions called**:
    - [`UnityStringArgumentMatches`](#UnityStringArgumentMatches)


