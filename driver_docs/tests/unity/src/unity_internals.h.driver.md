# Purpose
The provided C header file, `unity_internals.h`, is part of the Unity Test Framework, which is a unit testing framework for C. This file is crucial for defining the internal structures, macros, and functions that facilitate the testing of C code. It includes configurations for handling different data types, such as integers, floats, and doubles, and provides mechanisms for determining the size of these types based on the platform's architecture. The file also defines various macros for assertions, which are used to verify the correctness of code during testing. These assertions cover a wide range of checks, including equality, inequality, and boundary conditions for different data types.

Additionally, the file includes support for handling test output, managing test suites, and providing detailed test results. It defines structures and functions for managing test execution, such as [`UnityBegin`](#UnityBegin), [`UnityEnd`](#UnityEnd), and [`UnityConcludeTest`](#UnityConcludeTest), which are used to initialize, finalize, and conclude test runs, respectively. The file also includes provisions for handling floating-point operations and special conditions like infinity and NaN (Not a Number). Overall, `unity_internals.h` serves as a comprehensive internal API for the Unity Test Framework, enabling developers to write and execute unit tests efficiently in C.
# Imports and Dependencies

---
- `../examples/unity_config.h`
- `setjmp.h`
- `math.h`
- `stdint.h`
- `limits.h`
- `stdio.h`


# Global Variables

---
### UNITY\_OUTPUT\_CHAR\_HEADER\_DECLARATION
- **Type**: `void`
- **Description**: `UNITY_OUTPUT_CHAR_HEADER_DECLARATION` is an external declaration for a function that is intended to be used for character output in the Unity test framework. It is defined as a void function, indicating it does not return a value.
- **Use**: This variable is used to declare a custom character output function if `UNITY_OUTPUT_CHAR` is defined as something other than the default `putchar`.


---
### UNITY\_OMIT\_OUTPUT\_FLUSH\_HEADER\_DECLARATION
- **Type**: `extern void`
- **Description**: `UNITY_OMIT_OUTPUT_FLUSH_HEADER_DECLARATION` is an external declaration for a function or variable related to output flushing in the Unity test framework. It is conditionally declared based on the presence of `UNITY_OUTPUT_FLUSH_HEADER_DECLARATION`. This suggests it is used to manage or control the flushing of output streams, potentially to ensure test results are properly outputted.
- **Use**: This variable is used to declare an external function or variable for output flushing, which can be defined elsewhere in the program.


---
### Unity
- **Type**: `struct UNITY_STORAGE_T`
- **Description**: The `Unity` variable is an external instance of the `UNITY_STORAGE_T` structure, which is used to store information about the current state of a test being executed in the Unity test framework. This structure includes details such as the current test file, test name, line number, and counters for the number of tests, failures, and ignores.
- **Use**: The `Unity` variable is used to track and manage the state and results of tests during the execution of a test suite in the Unity framework.


---
### UnityStrErrFloat
- **Type**: ``const char[]``
- **Description**: `UnityStrErrFloat` is a global constant character array used to store error messages related to floating-point operations in the Unity test framework. It is declared as an external variable, indicating that its definition is located in another source file. This variable is likely used to provide error messages when floating-point assertions fail.
- **Use**: This variable is used to store and provide error messages for floating-point related errors in the Unity test framework.


---
### UnityStrErrDouble
- **Type**: ``const char[]``
- **Description**: `UnityStrErrDouble` is a constant character array that is declared as an external variable. It is intended to store an error message related to double precision floating-point operations in the Unity test framework.
- **Use**: This variable is used to provide a specific error message when double precision floating-point operations are not supported or fail in the Unity test framework.


---
### UnityStrErr64
- **Type**: ``const char[]``
- **Description**: `UnityStrErr64` is a constant character array that is declared as an external variable. It is likely used to store an error message related to 64-bit support in the Unity test framework.
- **Use**: This variable is used to provide a specific error message when 64-bit support is not available or an error occurs related to 64-bit operations.


# Data Structures

---
### UNITY\_DISPLAY\_STYLE\_T
- **Type**: `enum`
- **Members**:
    - `UNITY_DISPLAY_STYLE_INT`: Represents the display style for an integer with a size of 'int' plus a range offset for integers.
    - `UNITY_DISPLAY_STYLE_INT8`: Represents the display style for an 8-bit integer plus a range offset for integers.
    - `UNITY_DISPLAY_STYLE_INT16`: Represents the display style for a 16-bit integer plus a range offset for integers.
    - `UNITY_DISPLAY_STYLE_INT32`: Represents the display style for a 32-bit integer plus a range offset for integers.
    - `UNITY_DISPLAY_STYLE_INT64`: Represents the display style for a 64-bit integer plus a range offset for integers, available if 64-bit support is enabled.
    - `UNITY_DISPLAY_STYLE_UINT`: Represents the display style for an unsigned integer with a size of 'unsigned' plus a range offset for unsigned integers.
    - `UNITY_DISPLAY_STYLE_UINT8`: Represents the display style for an 8-bit unsigned integer plus a range offset for unsigned integers.
    - `UNITY_DISPLAY_STYLE_UINT16`: Represents the display style for a 16-bit unsigned integer plus a range offset for unsigned integers.
    - `UNITY_DISPLAY_STYLE_UINT32`: Represents the display style for a 32-bit unsigned integer plus a range offset for unsigned integers.
    - `UNITY_DISPLAY_STYLE_UINT64`: Represents the display style for a 64-bit unsigned integer plus a range offset for unsigned integers, available if 64-bit support is enabled.
    - `UNITY_DISPLAY_STYLE_HEX8`: Represents the display style for an 8-bit hexadecimal number plus a range offset for hexadecimal numbers.
    - `UNITY_DISPLAY_STYLE_HEX16`: Represents the display style for a 16-bit hexadecimal number plus a range offset for hexadecimal numbers.
    - `UNITY_DISPLAY_STYLE_HEX32`: Represents the display style for a 32-bit hexadecimal number plus a range offset for hexadecimal numbers.
    - `UNITY_DISPLAY_STYLE_HEX64`: Represents the display style for a 64-bit hexadecimal number plus a range offset for hexadecimal numbers, available if 64-bit support is enabled.
    - `UNITY_DISPLAY_STYLE_UNKNOWN`: Represents an unknown display style.
- **Description**: The `UNITY_DISPLAY_STYLE_T` is an enumeration used in the Unity Test Framework to define various display styles for integers and hexadecimal numbers. It includes styles for different bit-widths (8, 16, 32, and optionally 64 bits) and distinguishes between signed and unsigned integers, as well as hexadecimal representations. Each style is associated with a specific range offset to differentiate between integer, unsigned, and hexadecimal types. This enumeration is crucial for formatting and displaying test results in a consistent manner across different data types.


---
### UNITY\_COMPARISON\_T
- **Type**: `enum`
- **Members**:
    - `UNITY_EQUAL_TO`: Represents the equality comparison with a value of 1.
    - `UNITY_GREATER_THAN`: Represents the greater than comparison with a value of 2.
    - `UNITY_GREATER_OR_EQUAL`: Represents the greater than or equal to comparison with a value of 3.
    - `UNITY_SMALLER_THAN`: Represents the smaller than comparison with a value of 4.
    - `UNITY_SMALLER_OR_EQUAL`: Represents the smaller than or equal to comparison with a value of 5.
- **Description**: The `UNITY_COMPARISON_T` is an enumeration used in the Unity Test Framework for C to define various comparison operations that can be performed during testing. It includes constants for equality, greater than, greater than or equal to, smaller than, and smaller than or equal to comparisons. These constants are used to specify the type of comparison to be made when asserting conditions in test cases, facilitating the validation of expected outcomes against actual results.


---
### UNITY\_FLOAT\_TRAIT\_T
- **Type**: `enum`
- **Members**:
    - `UNITY_FLOAT_IS_NOT_INF`: Represents a float that is not infinite.
    - `UNITY_FLOAT_IS_INF`: Represents a float that is infinite.
    - `UNITY_FLOAT_IS_NOT_NEG_INF`: Represents a float that is not negative infinite.
    - `UNITY_FLOAT_IS_NEG_INF`: Represents a float that is negative infinite.
    - `UNITY_FLOAT_IS_NOT_NAN`: Represents a float that is not NaN (Not a Number).
    - `UNITY_FLOAT_IS_NAN`: Represents a float that is NaN (Not a Number).
    - `UNITY_FLOAT_IS_NOT_DET`: Represents a float that is not determinate.
    - `UNITY_FLOAT_IS_DET`: Represents a float that is determinate.
    - `UNITY_FLOAT_INVALID_TRAIT`: Represents an invalid float trait.
- **Description**: The `UNITY_FLOAT_TRAIT_T` is an enumeration used in the Unity Test Framework for C to categorize different traits of floating-point numbers. It provides a set of constants that describe whether a float is infinite, negative infinite, NaN (Not a Number), or determinate, among other traits. This enumeration is useful for asserting specific properties of floating-point numbers during unit testing, allowing developers to verify the behavior of their code when handling special float values.


---
### UNITY\_FLAGS\_T
- **Type**: `enum`
- **Members**:
    - `UNITY_ARRAY_TO_VAL`: Represents a flag value of 0, indicating an array to value operation.
    - `UNITY_ARRAY_TO_ARRAY`: Represents a flag value of 1, indicating an array to array operation.
- **Description**: The `UNITY_FLAGS_T` is an enumeration used within the Unity Test Framework to specify the type of operation being performed on arrays during test assertions. It defines two possible values: `UNITY_ARRAY_TO_VAL` for operations where an array is compared to a single value, and `UNITY_ARRAY_TO_ARRAY` for operations where two arrays are compared to each other. This enumeration helps in managing and differentiating the context of array operations in test cases.


---
### UNITY\_STORAGE\_T
- **Type**: `struct`
- **Members**:
    - `TestFile`: A pointer to a string representing the name of the test file.
    - `CurrentTestName`: A pointer to a string representing the name of the current test.
    - `CurrentDetail1`: A pointer to a string for additional detail about the current test (optional).
    - `CurrentDetail2`: A pointer to a string for more detail about the current test (optional).
    - `CurrentTestLineNumber`: The line number in the test file where the current test is defined.
    - `NumberOfTests`: A counter for the total number of tests executed.
    - `TestFailures`: A counter for the number of test failures.
    - `TestIgnores`: A counter for the number of tests that were ignored.
    - `CurrentTestFailed`: A flag indicating if the current test has failed.
    - `CurrentTestIgnored`: A flag indicating if the current test was ignored.
    - `AbortFrame`: A buffer used for non-local jumps to handle test aborts (optional).
- **Description**: The `UNITY_STORAGE_T` structure is a central data structure used in the Unity Test Framework for C to manage and track the state of test execution. It holds information about the current test being executed, such as its name, file, and line number, as well as counters for the total number of tests, failures, and ignored tests. Additionally, it can store optional details about the test and manage test aborts using a jump buffer. This structure is essential for maintaining the context and results of test runs within the framework.


# Function Declarations (Public API)

---
### UnityBegin<!-- {{#callable_declaration:UnityBegin}} -->
Initializes the Unity test framework for a new test file.
- **Description**: Use this function to initialize the Unity test framework before running any tests in a new test file. It sets up the necessary internal state, including resetting test counters and clearing any previous test details. This function should be called at the beginning of a test file to ensure that the test environment is properly prepared. It is important to provide a valid filename to associate with the test results.
- **Inputs**:
    - `filename`: A string representing the name of the test file. This parameter must not be null, as it is used to identify the test file in the test results.
- **Output**: None
- **See also**: [`UnityBegin`](unity.c.driver.md#UnityBegin)  (Implementation)


---
### UnityEnd<!-- {{#callable_declaration:UnityEnd}} -->
Concludes the Unity test run and outputs the results.
- **Description**: This function should be called at the end of a Unity test suite to finalize the test run and print a summary of the results. It outputs the total number of tests, the number of failures, and the number of ignored tests. Additionally, it indicates whether all tests passed or if there were any failures. This function must be called after all tests have been executed to ensure accurate reporting. It returns the number of test failures, which can be used to determine the success of the test suite.
- **Inputs**: None
- **Output**: Returns the number of test failures as an integer.
- **See also**: [`UnityEnd`](unity.c.driver.md#UnityEnd)  (Implementation)


---
### UnityConcludeTest<!-- {{#callable_declaration:UnityConcludeTest}} -->
Concludes the current test and updates test results.
- **Description**: This function should be called at the end of each test to finalize the test results. It checks if the current test was ignored or failed and updates the respective counters accordingly. If the test passed, it prints a success message. It resets the test status flags for failure and ignore, and ensures that the output is properly flushed and a newline is printed. This function must be called after each test execution to ensure accurate test reporting.
- **Inputs**: None
- **Output**: None
- **See also**: [`UnityConcludeTest`](unity.c.driver.md#UnityConcludeTest)  (Implementation)


---
### UnityDefaultTestRun<!-- {{#callable_declaration:UnityDefaultTestRun}} -->
Executes a test function within the Unity test framework.
- **Description**: This function is used to run a single test function within the Unity test framework, managing the setup and teardown processes. It should be called with a test function, its name, and the line number where the test is defined. The function increments the test count, clears any previous test details, and executes the test function between setup and teardown calls. It handles any exceptions that occur during the test execution and concludes the test, updating the test results accordingly. This function is typically used internally by the Unity framework to manage test execution.
- **Inputs**:
    - `Func`: A function pointer to the test function to be executed. The function must not take any parameters and must return void.
    - `FuncName`: A string representing the name of the test function. This must not be null and should be a valid C string.
    - `FuncLineNum`: An integer representing the line number where the test function is defined. This is used for reporting purposes.
- **Output**: None
- **See also**: [`UnityDefaultTestRun`](unity.c.driver.md#UnityDefaultTestRun)  (Implementation)


---
### UnityPrint<!-- {{#callable_declaration:UnityPrint}} -->
Prints a string with special character handling.
- **Description**: This function outputs a given string to the Unity test framework's output mechanism, handling special characters by escaping them or converting them to a readable format. It is useful for displaying test results or messages where special characters like carriage returns, line feeds, and non-printable characters need to be represented in a human-readable form. The function should be called with a valid string pointer, and it will safely handle a null pointer by performing no action.
- **Inputs**:
    - `string`: A pointer to a null-terminated string to be printed. The string may contain printable characters, carriage returns, line feeds, and other non-printable characters. The pointer can be null, in which case the function does nothing. The caller retains ownership of the string.
- **Output**: None
- **See also**: [`UnityPrint`](unity.c.driver.md#UnityPrint)  (Implementation)


---
### UnityPrintLen<!-- {{#callable_declaration:UnityPrintLen}} -->
Prints a specified length of a string with special character handling.
- **Description**: This function is used to print a portion of a string up to a specified length, handling special characters such as carriage returns and line feeds by escaping them, and representing non-printable characters in hexadecimal format. It is useful when you need to output a controlled segment of a string, ensuring that special characters are displayed in a readable format. The function expects a non-null string pointer and a length that specifies how many characters to process. If the string is null, the function does nothing.
- **Inputs**:
    - `string`: A pointer to the string to be printed. Must not be null. The function will process characters up to the specified length or until a null terminator is encountered.
    - `length`: The maximum number of characters to print from the string. It is of type UNITY_UINT32, and the function will not process more characters than this length.
- **Output**: None
- **See also**: [`UnityPrintLen`](unity.c.driver.md#UnityPrintLen)  (Implementation)


---
### UnityPrintMask<!-- {{#callable_declaration:UnityPrintMask}} -->
Prints a masked binary representation of a number.
- **Description**: This function outputs the binary representation of a given number, applying a mask to determine which bits are displayed. For each bit position, if the mask bit is set, the corresponding bit from the number is printed as '1' or '0'. If the mask bit is not set, 'X' is printed instead. This function is useful for debugging or testing scenarios where only certain bits of a number are relevant. The function assumes that the mask and number are of the same bit width as defined by UNITY_INT_WIDTH.
- **Inputs**:
    - `mask`: A bitmask of type UNITY_UINT that determines which bits of the number are displayed. Each bit set to 1 in the mask will result in the corresponding bit of the number being printed as '1' or '0'. Must be of the same bit width as UNITY_INT_WIDTH.
    - `number`: The number of type UNITY_UINT whose binary representation is to be printed, subject to the mask. Must be of the same bit width as UNITY_INT_WIDTH.
- **Output**: None
- **See also**: [`UnityPrintMask`](unity.c.driver.md#UnityPrintMask)  (Implementation)


---
### UnityPrintNumberByStyle<!-- {{#callable_declaration:UnityPrintNumberByStyle}} -->
Prints a number in a specified display style.
- **Description**: This function is used to print a number in a specific format based on the provided display style. It supports printing as signed integers, unsigned integers, or hexadecimal values. The function should be called when you need to output a number in a format that matches the style specified by the `style` parameter. The function does not return a value and is intended for use in environments where the output is directed to a character output function, such as a console or log.
- **Inputs**:
    - `number`: The integer value to be printed. It is of type `UNITY_INT`, which is a signed integer type.
    - `style`: Specifies the display style for the number. It is of type `UNITY_DISPLAY_STYLE_T`, which can be a style for signed integers, unsigned integers, or hexadecimal. The style determines how the number is formatted and printed.
- **Output**: None
- **See also**: [`UnityPrintNumberByStyle`](unity.c.driver.md#UnityPrintNumberByStyle)  (Implementation)


---
### UnityPrintNumber<!-- {{#callable_declaration:UnityPrintNumber}} -->
Prints an integer to the output.
- **Description**: This function is used to print an integer value to the output, handling both positive and negative numbers. It is suitable for use in test frameworks where displaying integer values is necessary. The function will output a '-' character for negative numbers before printing the absolute value. It is expected that the output mechanism is properly configured to handle character output.
- **Inputs**:
    - `number_to_print`: The integer value to be printed. It can be any valid integer within the range of UNITY_INT, including negative values. The caller retains ownership of the value.
- **Output**: None
- **See also**: [`UnityPrintNumber`](unity.c.driver.md#UnityPrintNumber)  (Implementation)


---
### UnityPrintNumberUnsigned<!-- {{#callable_declaration:UnityPrintNumberUnsigned}} -->
Prints an unsigned integer to the output.
- **Description**: Use this function to print an unsigned integer to the output character by character. It is typically used within the Unity test framework to display numeric values during test execution. The function does not handle negative numbers, as it is designed for unsigned integers. Ensure that the output mechanism (e.g., putchar) is properly configured before calling this function, as it relies on the UNITY_OUTPUT_CHAR macro to output each digit.
- **Inputs**:
    - `number`: An unsigned integer of type UNITY_UINT to be printed. The value must be non-negative, as it represents an unsigned integer. The function does not perform any checks for invalid values, as it assumes a valid unsigned integer is provided.
- **Output**: None
- **See also**: [`UnityPrintNumberUnsigned`](unity.c.driver.md#UnityPrintNumberUnsigned)  (Implementation)


---
### UnityPrintNumberHex<!-- {{#callable_declaration:UnityPrintNumberHex}} -->
Prints a hexadecimal representation of a number.
- **Description**: This function outputs the hexadecimal representation of a given unsigned integer to the configured output method, printing a specified number of nibbles. It is useful for debugging or logging purposes where hexadecimal format is preferred. The function ensures that the number of nibbles printed does not exceed twice the size of the input number in bytes. It should be called when a hexadecimal display of a number is needed, and the output is directed to the method defined by UNITY_OUTPUT_CHAR.
- **Inputs**:
    - `number`: The unsigned integer to be printed in hexadecimal format. It is expected to be a valid UNITY_UINT value.
    - `nibbles_to_print`: Specifies the number of nibbles to print. If this value exceeds twice the size of the number in bytes, it will be clamped to that maximum. Negative values are treated as large unsigned values, effectively clamping them as well.
- **Output**: None
- **See also**: [`UnityPrintNumberHex`](unity.c.driver.md#UnityPrintNumberHex)  (Implementation)


---
### UnityPrintFloat<!-- {{#callable_declaration:UnityPrintFloat}} -->
Prints a floating-point number to the Unity output.
- **Description**: This function is used to print a floating-point number, represented as a UNITY_DOUBLE, to the Unity test framework's output. It handles special cases such as negative zero, NaN (Not a Number), and positive or negative infinity, printing them as "0", "nan", and "inf" respectively. The function also formats the number in scientific notation if necessary, ensuring that the output is human-readable. This function should be used when you need to display floating-point numbers in test results or logs within the Unity framework.
- **Inputs**:
    - `input_number`: A floating-point number of type UNITY_DOUBLE to be printed. It can represent any valid double value, including special cases like NaN and infinity. The caller retains ownership of the value.
- **Output**: None
- **See also**: [`UnityPrintFloat`](unity.c.driver.md#UnityPrintFloat)  (Implementation)


---
### UnityAssertEqualNumber<!-- {{#callable_declaration:UnityAssertEqualNumber}} -->
Asserts that two numbers are equal and reports a failure if they are not.
- **Description**: Use this function to verify that two integer values are equal during a test. It is typically called within a test case to assert that the actual value matches the expected value. If the values are not equal, the function will report a failure, including the line number and an optional message. The function requires that the test framework is properly initialized and that the test is not already marked as failed or ignored. The style parameter determines how the numbers are displayed in the failure message.
- **Inputs**:
    - `expected`: The expected integer value to compare against. It should be a valid integer of type UNITY_INT.
    - `actual`: The actual integer value obtained from the test. It should be a valid integer of type UNITY_INT.
    - `msg`: An optional message to include in the failure report. Can be null if no additional message is needed.
    - `lineNumber`: The line number in the test file where the assertion is made. It should be a valid line number of type UNITY_LINE_TYPE.
    - `style`: The display style for the numbers in the failure message. It should be a valid UNITY_DISPLAY_STYLE_T value, such as UNITY_DISPLAY_STYLE_INT or UNITY_DISPLAY_STYLE_HEX.
- **Output**: None
- **See also**: [`UnityAssertEqualNumber`](unity.c.driver.md#UnityAssertEqualNumber)  (Implementation)


---
### UnityAssertGreaterOrLessOrEqualNumber<!-- {{#callable_declaration:UnityAssertGreaterOrLessOrEqualNumber}} -->
Asserts a comparison between a threshold and an actual value.
- **Description**: This function is used to assert that a given actual value meets a specified comparison condition relative to a threshold value. It is typically used in unit tests to verify that numerical values are greater than, less than, or equal to a threshold, based on the comparison type provided. The function requires a valid comparison type and display style to be specified. If the assertion fails, it logs the failure message and the line number where the failure occurred. This function should be called within a test framework context where assertions are expected to be handled.
- **Inputs**:
    - `threshold`: The reference value against which the actual value is compared. It must be a valid integer.
    - `actual`: The value to be compared against the threshold. It must be a valid integer.
    - `compare`: Specifies the type of comparison to perform, such as greater than, less than, or equal to. Must be a valid UNITY_COMPARISON_T value.
    - `msg`: An optional message to include in the failure output if the assertion fails. Can be null.
    - `lineNumber`: The line number in the source code where the assertion is made. Used for reporting purposes.
    - `style`: Specifies how the numbers should be displayed in the output, such as integer, unsigned integer, or hexadecimal. Must be a valid UNITY_DISPLAY_STYLE_T value.
- **Output**: None
- **See also**: [`UnityAssertGreaterOrLessOrEqualNumber`](unity.c.driver.md#UnityAssertGreaterOrLessOrEqualNumber)  (Implementation)


---
### UnityAssertEqualIntArray<!-- {{#callable_declaration:UnityAssertEqualIntArray}} -->
Compares two integer arrays for equality.
- **Description**: Use this function to assert that two integer arrays are equal in a unit test. It compares each element of the arrays based on the specified number of elements and the display style, which determines the integer size. The function should be called with valid pointers to the expected and actual arrays, and it will handle cases where either pointer is null. If the arrays differ, the function will report a failure with the provided message and line number. This function is typically used within a testing framework to validate that the actual output of a function matches the expected output.
- **Inputs**:
    - `expected`: A pointer to the expected array of integers. Must not be null unless 'actual' is also null.
    - `actual`: A pointer to the actual array of integers to compare against the expected array. Must not be null unless 'expected' is also null.
    - `num_elements`: The number of elements to compare in the arrays. Must be greater than zero.
    - `msg`: A custom message to display if the assertion fails. Can be null if no custom message is needed.
    - `lineNumber`: The line number in the test file where the assertion is made. Used for reporting purposes.
    - `style`: Specifies the display style and size of the integers in the arrays (e.g., 8, 16, 32, or 64 bits). Must be a valid UNITY_DISPLAY_STYLE_T value.
    - `flags`: Determines the comparison mode, typically UNITY_ARRAY_TO_ARRAY for array comparisons. Must be a valid UNITY_FLAGS_T value.
- **Output**: None
- **See also**: [`UnityAssertEqualIntArray`](unity.c.driver.md#UnityAssertEqualIntArray)  (Implementation)


---
### UnityAssertBits<!-- {{#callable_declaration:UnityAssertBits}} -->
Asserts that specific bits in two integers are equal.
- **Description**: Use this function to verify that the bits specified by a mask in two integers, `expected` and `actual`, are equal during a test. This function is typically called within a test case to ensure that the masked bits of the `actual` value match the `expected` value. If the assertion fails, it will report the failure, including the line number and an optional message, and then abort the test. This function should be used when you need to compare specific bits rather than entire integers.
- **Inputs**:
    - `mask`: A bitmask indicating which bits to compare between `expected` and `actual`. It must be a valid integer.
    - `expected`: The integer value representing the expected state of the bits specified by `mask`. It must be a valid integer.
    - `actual`: The integer value representing the actual state of the bits specified by `mask`. It must be a valid integer.
    - `msg`: An optional message to include in the test output if the assertion fails. It can be null if no message is desired.
    - `lineNumber`: The line number in the test file where this assertion is made. It must be a valid line number, typically provided by the `__LINE__` macro.
- **Output**: None
- **See also**: [`UnityAssertBits`](unity.c.driver.md#UnityAssertBits)  (Implementation)


---
### UnityAssertEqualString<!-- {{#callable_declaration:UnityAssertEqualString}} -->
Compares two strings for equality and reports a test failure if they differ.
- **Description**: Use this function to assert that two strings are equal in a unit test. It compares the expected and actual strings character by character. If the strings differ, or if one is null while the other is not, the current test is marked as failed. If both strings are null, the test passes. This function should be called within a test case, and it will report the failure along with the specified message and line number if the strings are not equal.
- **Inputs**:
    - `expected`: A pointer to the expected string. It can be null, in which case the actual string must also be null for the test to pass.
    - `actual`: A pointer to the actual string to compare against the expected string. It can be null, in which case the expected string must also be null for the test to pass.
    - `msg`: An optional message to include in the test output if the assertion fails. The caller retains ownership of this string.
    - `lineNumber`: The line number in the test file where this assertion is made. This is used for reporting the location of the failure.
- **Output**: None
- **See also**: [`UnityAssertEqualString`](unity.c.driver.md#UnityAssertEqualString)  (Implementation)


---
### UnityAssertEqualStringLen<!-- {{#callable_declaration:UnityAssertEqualStringLen}} -->
Compares two strings up to a specified length and reports a test failure if they differ.
- **Description**: Use this function to assert that two strings are equal up to a specified length during a unit test. It is particularly useful when you want to compare only a portion of the strings or when the strings may not be null-terminated. The function will report a test failure if the strings differ within the specified length or if one string is null and the other is not. If both strings are null, the test will pass. This function should be called within a test case, and it will handle reporting the failure message and line number if the assertion fails.
- **Inputs**:
    - `expected`: A pointer to the expected string. Can be null, in which case the actual string must also be null for the test to pass.
    - `actual`: A pointer to the actual string. Can be null, in which case the expected string must also be null for the test to pass.
    - `length`: The maximum number of characters to compare between the two strings. Must be a non-negative integer.
    - `msg`: An optional message to include in the test failure report. Can be null if no additional message is needed.
    - `lineNumber`: The line number in the test file where this assertion is made. Used for reporting purposes.
- **Output**: None
- **See also**: [`UnityAssertEqualStringLen`](unity.c.driver.md#UnityAssertEqualStringLen)  (Implementation)


---
### UnityAssertEqualStringArray<!-- {{#callable_declaration:UnityAssertEqualStringArray}} -->
Compares two arrays of strings for equality.
- **Description**: Use this function to assert that two arrays of strings are equal in a unit test. It compares each string in the expected array with the corresponding string in the actual array, up to the specified number of elements. If any string pair differs, the test fails, and an optional message is printed. The function handles null pointers and empty arrays, treating them as errors. It should be called within a test framework that handles test failures and line numbers.
- **Inputs**:
    - `expected`: A pointer to the expected array of strings. Must not be null unless 'actual' is also null.
    - `actual`: A pointer to the actual array of strings to compare against the expected array. Must not be null unless 'expected' is also null.
    - `num_elements`: The number of elements to compare in the arrays. Must be greater than zero; otherwise, it is treated as an error.
    - `msg`: An optional message to print if the assertion fails. Can be null.
    - `lineNumber`: The line number in the test file where this assertion is made. Used for reporting purposes.
    - `flags`: A flag indicating the comparison mode. Typically set to UNITY_ARRAY_TO_ARRAY for array-to-array comparison.
- **Output**: None
- **See also**: [`UnityAssertEqualStringArray`](unity.c.driver.md#UnityAssertEqualStringArray)  (Implementation)


---
### UnityAssertEqualMemory<!-- {{#callable_declaration:UnityAssertEqualMemory}} -->
Compares two memory blocks for equality.
- **Description**: Use this function to assert that two memory blocks are equal in a unit test. It compares the memory content of the expected and actual pointers for a specified length and number of elements. This function should be called when you need to verify that two memory regions are identical. It handles cases where either pointer is null and provides detailed failure messages if the memory blocks differ. Ensure that the expected and actual pointers are valid and that the length and number of elements are greater than zero before calling this function.
- **Inputs**:
    - `expected`: A pointer to the expected memory block. Must not be null unless actual is also null.
    - `actual`: A pointer to the actual memory block. Must not be null unless expected is also null.
    - `length`: The number of bytes to compare in each element. Must be greater than zero.
    - `num_elements`: The number of elements to compare. Must be greater than zero.
    - `msg`: An optional message to include in the failure output. Can be null.
    - `lineNumber`: The line number to report in case of a failure. Typically the line where the assertion is made.
    - `flags`: Specifies comparison behavior, such as UNITY_ARRAY_TO_VAL or UNITY_ARRAY_TO_ARRAY.
- **Output**: None
- **See also**: [`UnityAssertEqualMemory`](unity.c.driver.md#UnityAssertEqualMemory)  (Implementation)


---
### UnityAssertNumbersWithin<!-- {{#callable_declaration:UnityAssertNumbersWithin}} -->
Asserts that two numbers are within a specified delta of each other.
- **Description**: Use this function to verify that the difference between two numbers, `expected` and `actual`, does not exceed a given `delta`. This is useful in test cases where exact equality is not required, but the values should be close within a certain tolerance. The function will fail the current test if the numbers are not within the specified range, and it will output a failure message including the expected and actual values, the delta, and any additional message provided. This function should be called within a test context where the Unity test framework is managing test results.
- **Inputs**:
    - `delta`: The maximum allowable difference between `expected` and `actual`. Must be a non-negative integer.
    - `expected`: The expected value in the comparison. Can be any integer value.
    - `actual`: The actual value to compare against the expected value. Can be any integer value.
    - `msg`: An optional message to include in the test output if the assertion fails. Can be null.
    - `lineNumber`: The line number in the test file where this assertion is made. Typically provided by the UNITY_LINE_TYPE macro.
    - `style`: Specifies the display style for the numbers, such as integer or unsigned integer. Must be a valid UNITY_DISPLAY_STYLE_T value.
- **Output**: None
- **See also**: [`UnityAssertNumbersWithin`](unity.c.driver.md#UnityAssertNumbersWithin)  (Implementation)


---
### UnityFail<!-- {{#callable_declaration:UnityFail}} -->
Reports a test failure with an optional message and line number.
- **Description**: Use this function to explicitly mark a test as failed, providing an optional message for additional context and specifying the line number where the failure occurred. This function is typically called within a test case when a specific condition is not met, and it will output the failure details to the test results. The function should be used in conjunction with the Unity testing framework and is expected to be called when a test condition fails. It is important to ensure that the Unity framework is properly initialized before calling this function.
- **Inputs**:
    - `msg`: A pointer to a constant character string containing the failure message. This parameter can be NULL if no message is desired. The caller retains ownership of the string.
    - `line`: The line number of the test where the failure occurred. This should be of type UNITY_LINE_TYPE, which is typically an unsigned integer type.
- **Output**: None
- **See also**: [`UnityFail`](unity.c.driver.md#UnityFail)  (Implementation)


---
### UnityIgnore<!-- {{#callable_declaration:UnityIgnore}} -->
Marks a test as ignored with an optional message.
- **Description**: Use this function to mark a test as ignored during a test run, optionally providing a message to explain the reason for ignoring the test. This function should be called within a test case when a condition arises that warrants skipping the test, such as an unimplemented feature or a known issue. The function outputs the ignore message and the line number where the ignore was triggered, and then halts further execution of the test.
- **Inputs**:
    - `msg`: An optional message explaining why the test is ignored. Can be NULL if no message is needed. The caller retains ownership of the string.
    - `line`: The line number where the ignore is triggered. This is typically provided using the __LINE__ macro to indicate the exact location in the source code.
- **Output**: None
- **See also**: [`UnityIgnore`](unity.c.driver.md#UnityIgnore)  (Implementation)


---
### UnityAssertFloatsWithin<!-- {{#callable_declaration:UnityAssertFloatsWithin}} -->
Asserts that two floating-point numbers are within a specified delta.
- **Description**: Use this function to verify that two floating-point numbers, `expected` and `actual`, are within a specified range defined by `delta`. This is useful in unit tests where floating-point precision errors might occur. The function should be called within a test case, and it will report a failure if the numbers are not within the specified delta. The `msg` parameter allows you to specify a custom message that will be included in the test output if the assertion fails. The `lineNumber` parameter is used to indicate the line number in the test file where the assertion is made, aiding in debugging.
- **Inputs**:
    - `delta`: The maximum allowable difference between `expected` and `actual` for the test to pass. Must be a non-negative floating-point number.
    - `expected`: The expected floating-point value in the test.
    - `actual`: The actual floating-point value obtained in the test.
    - `msg`: A custom message to include in the test output if the assertion fails. Can be null if no custom message is desired.
    - `lineNumber`: The line number in the test file where this assertion is made. Used for reporting purposes.
- **Output**: None
- **See also**: [`UnityAssertFloatsWithin`](unity.c.driver.md#UnityAssertFloatsWithin)  (Implementation)


---
### UnityAssertEqualFloatArray<!-- {{#callable_declaration:UnityAssertEqualFloatArray}} -->
Asserts that two arrays of floats are equal within a specified precision.
- **Description**: Use this function to verify that two arrays of floating-point numbers are equal within a defined precision. This function is typically used in unit tests to ensure that the actual output of a function matches the expected output. The function requires both arrays to be non-null and of the same length, specified by `num_elements`. If the arrays are not equal, the function will fail the test and provide a message indicating the first differing element. The function can handle cases where both arrays are null or point to the same memory location, treating them as equal. It is important to ensure that the function is called with valid pointers and a non-zero number of elements to avoid unnecessary test failures.
- **Inputs**:
    - `expected`: A pointer to the array of expected float values. Must not be null unless `actual` is also null.
    - `actual`: A pointer to the array of actual float values. Must not be null unless `expected` is also null.
    - `num_elements`: The number of elements in each array to compare. Must be greater than zero.
    - `msg`: An optional message to include in the test output if the assertion fails. Can be null.
    - `lineNumber`: The line number in the test file where the assertion is made. Used for reporting purposes.
    - `flags`: Flags to control the comparison behavior, such as whether to treat the arrays as array-to-array or array-to-value.
- **Output**: None
- **See also**: [`UnityAssertEqualFloatArray`](unity.c.driver.md#UnityAssertEqualFloatArray)  (Implementation)


---
### UnityAssertFloatSpecial<!-- {{#callable_declaration:UnityAssertFloatSpecial}} -->
Asserts that a floating-point number has a specific special trait.
- **Description**: Use this function to verify that a floating-point number exhibits a specific special trait, such as being infinite, negative infinite, NaN, or determinate. This function is typically used in unit tests to ensure that floating-point values behave as expected under certain conditions. It requires a valid line number for error reporting and a style parameter to specify the trait to check. If the actual value does not match the expected trait, the function will report a failure and halt the test execution. Ensure that the Unity test framework is properly initialized before calling this function.
- **Inputs**:
    - `actual`: The floating-point number to be checked. It must be a valid floating-point value.
    - `msg`: An optional message to include in the test output if the assertion fails. Can be null if no message is desired.
    - `lineNumber`: The line number in the test file where this assertion is made. It is used for reporting purposes and must be a valid line number.
    - `style`: Specifies the trait to check for in the floating-point number. It must be one of the predefined UNITY_FLOAT_TRAIT_T values, such as UNITY_FLOAT_IS_INF or UNITY_FLOAT_IS_NAN.
- **Output**: None
- **See also**: [`UnityAssertFloatSpecial`](unity.c.driver.md#UnityAssertFloatSpecial)  (Implementation)


---
### UnityAssertDoublesWithin<!-- {{#callable_declaration:UnityAssertDoublesWithin}} -->
Asserts that two double values are within a specified delta.
- **Description**: Use this function to verify that two double precision floating-point numbers are within a specified range of each other during a test. This is useful for validating calculations that may have small floating-point errors. The function should be called within a test case, and it will report a failure if the actual value is not within the delta range of the expected value. The function also allows for an optional message to be specified, which will be included in the test output if the assertion fails. The line number parameter is used to indicate where in the test the assertion is being made.
- **Inputs**:
    - `delta`: The maximum allowable difference between the expected and actual values. Must be a non-negative double.
    - `expected`: The expected double value in the comparison.
    - `actual`: The actual double value to compare against the expected value.
    - `msg`: An optional message to include in the test output if the assertion fails. Can be null.
    - `lineNumber`: The line number in the test file where the assertion is made. Typically provided by a macro.
- **Output**: None
- **See also**: [`UnityAssertDoublesWithin`](unity.c.driver.md#UnityAssertDoublesWithin)  (Implementation)


---
### UnityAssertEqualDoubleArray<!-- {{#callable_declaration:UnityAssertEqualDoubleArray}} -->
Compares two arrays of double precision floating-point numbers for equality.
- **Description**: Use this function to assert that two arrays of double precision floating-point numbers are equal within a specified precision. It is typically used in unit tests to verify that the actual output of a function matches the expected output. The function requires both arrays to be non-null and of the same length, specified by `num_elements`. If the arrays are not equal, the function will fail the test and print a message, including the line number and an optional user message. The function handles cases where either array is null and will ignore the test if both arrays are null or point to the same memory location.
- **Inputs**:
    - `expected`: A pointer to the first element of the expected array of double precision numbers. Must not be null unless `actual` is also null.
    - `actual`: A pointer to the first element of the actual array of double precision numbers. Must not be null unless `expected` is also null.
    - `num_elements`: The number of elements in each array to compare. Must be greater than zero.
    - `msg`: An optional message to print if the assertion fails. Can be null.
    - `lineNumber`: The line number in the test file where this function is called. Used for reporting purposes.
    - `flags`: Flags to control the comparison behavior. Typically set to `UNITY_ARRAY_TO_ARRAY` to compare array elements.
- **Output**: None
- **See also**: [`UnityAssertEqualDoubleArray`](unity.c.driver.md#UnityAssertEqualDoubleArray)  (Implementation)


---
### UnityAssertDoubleSpecial<!-- {{#callable_declaration:UnityAssertDoubleSpecial}} -->
Asserts that a double value has a specific floating-point trait.
- **Description**: Use this function to verify that a given double precision floating-point number exhibits a specific trait, such as being infinite, NaN, or determinate. This function is typically used in unit tests to ensure that floating-point calculations yield expected special values. It must be called with a valid floating-point trait style, and it will report a test failure if the actual value does not match the expected trait. The function also allows for an optional message to be included in the test output, providing additional context for the assertion.
- **Inputs**:
    - `actual`: The double precision floating-point number to be checked. It should be a valid double value.
    - `msg`: An optional message string that provides additional context for the assertion. Can be null if no message is desired. The caller retains ownership of this string.
    - `lineNumber`: The line number in the source code where the assertion is made. This is used for reporting purposes and should be a valid line number.
    - `style`: A value of type UNITY_FLOAT_TRAIT_T that specifies the expected trait of the floating-point number. Valid values include UNITY_FLOAT_IS_INF, UNITY_FLOAT_IS_NEG_INF, UNITY_FLOAT_IS_NAN, UNITY_FLOAT_IS_DET, and their negations. Invalid values will result in a test failure.
- **Output**: None
- **See also**: [`UnityAssertDoubleSpecial`](unity.c.driver.md#UnityAssertDoubleSpecial)  (Implementation)


---
### UnityNumToPtr<!-- {{#callable_declaration:UnityNumToPtr}} -->
Converts an integer to a pointer of a specified size.
- **Description**: This function is used to convert an integer value into a pointer of a specified size, which can be 1, 2, 4, or 8 bytes. It is useful in scenarios where you need to perform operations on integers as if they were pointers, such as in testing frameworks. The function must be called with a valid size parameter, and it returns a pointer to the integer value cast to the specified size. If the size is not 1, 2, or 8 (when 64-bit support is enabled), it defaults to 4 bytes.
- **Inputs**:
    - `num`: The integer value to be converted to a pointer. It is expected to be a valid integer within the range of the specified size.
    - `size`: Specifies the size in bytes of the integer representation. Valid values are 1, 2, 4, and 8 (if 64-bit support is enabled). If an unsupported size is provided, it defaults to 4 bytes.
- **Output**: Returns a pointer to the integer value cast to the specified size.
- **See also**: [`UnityNumToPtr`](unity.c.driver.md#UnityNumToPtr)  (Implementation)


---
### UnityFloatToPtr<!-- {{#callable_declaration:UnityFloatToPtr}} -->
Converts a float to a pointer representation.
- **Description**: Use this function to obtain a pointer representation of a float value, which can be useful for certain testing scenarios where pointer comparison is required. This function is part of the Unity test framework and is intended for internal use within the framework. It should be used when you need to pass a float value as a pointer, typically for assertions or comparisons in tests. Ensure that the float value is valid and within the representable range of the float type.
- **Inputs**:
    - `num`: A float value to be converted to a pointer representation. The value should be a valid float and within the typical range of float values. The caller retains ownership of the float value.
- **Output**: Returns a pointer to the float value, cast to UNITY_INTERNAL_PTR, which can be used for pointer-based operations or comparisons.
- **See also**: [`UnityFloatToPtr`](unity.c.driver.md#UnityFloatToPtr)  (Implementation)


---
### UnityDoubleToPtr<!-- {{#callable_declaration:UnityDoubleToPtr}} -->
Converts a double to a pointer representation.
- **Description**: This function is used to convert a double precision floating-point number into a pointer representation, which can be useful for certain testing or comparison operations where a pointer is required. It should be used when you need to handle a double as a pointer within the Unity test framework. The function assumes that the input is a valid double and does not perform any error checking on the input value.
- **Inputs**:
    - `num`: A double precision floating-point number to be converted to a pointer. The input must be a valid double, and the function does not handle invalid or special floating-point values like NaN or infinity.
- **Output**: Returns a pointer representation of the input double, which can be used in pointer-based operations.
- **See also**: [`UnityDoubleToPtr`](unity.c.driver.md#UnityDoubleToPtr)  (Implementation)


---
### UnityParseOptions<!-- {{#callable_declaration:UnityParseOptions}} -->
Parses command-line options for Unity test framework.
- **Description**: This function processes command-line arguments to configure the Unity test framework's behavior. It supports options for listing tests, including or excluding tests by name, and setting verbosity levels. The function should be called with the command-line argument count and argument vector. It returns specific codes based on the options parsed, such as -1 for listing tests, 1 for errors, and 0 for successful parsing. Ensure that the argument vector is properly formatted and contains valid options to avoid errors.
- **Inputs**:
    - `argc`: The number of command-line arguments, including the program name. Must be greater than zero.
    - `argv`: An array of strings representing the command-line arguments. Must not be null, and each string should be properly formatted as a command-line option.
- **Output**: Returns -1 if the '-l' option is found, 1 if an error occurs, and 0 if options are parsed successfully.
- **See also**: [`UnityParseOptions`](unity.c.driver.md#UnityParseOptions)  (Implementation)


---
### UnityTestMatches<!-- {{#callable_declaration:UnityTestMatches}} -->
Determines if a test name matches specified include and exclude patterns.
- **Description**: This function checks if the current test name matches the patterns specified by the include and exclude options. It should be used when you need to determine if a test should be executed based on these patterns. The function first checks if the test name matches the include pattern, if specified, and defaults to a match if no include pattern is set. It then checks against the exclude pattern, if specified, and will return a non-match if the test name matches the exclude pattern. This function is typically used in test frameworks to filter which tests to run based on user-specified criteria.
- **Inputs**: None
- **Output**: Returns 1 if the test name matches the include pattern and does not match the exclude pattern, otherwise returns 0.
- **See also**: [`UnityTestMatches`](unity.c.driver.md#UnityTestMatches)  (Implementation)


