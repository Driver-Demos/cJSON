# Purpose
This C header file, `unity_fixture.h`, is part of the Unity Test Framework, which is designed to facilitate unit testing in C. It provides macros and function declarations to define and manage test groups, setup and teardown routines, and individual test cases. The file includes essential headers and defines macros for creating and running tests, ignoring tests, and running test groups, which are crucial for organizing and executing tests systematically. Additionally, it offers compatibility macros for CppUTest, allowing for easier integration with existing test codebases. The file also includes a function declaration for controlling memory allocation failures, which is useful for testing memory management in C programs. Overall, this header file is a key component in structuring and executing unit tests using the Unity framework.
# Imports and Dependencies

---
- `unity.h`
- `unity_internals.h`
- `unity_fixture_malloc_overrides.h`
- `unity_fixture_internals.h`


# Function Declarations (Public API)

---
### UnityMain<!-- {{#callable_declaration:UnityMain}} -->
Executes all registered unit tests with specified command line options.
- **Description**: This function is used to run all unit tests defined in a test suite, utilizing command line arguments to configure the test execution environment. It should be called from the main function of a test application. The function processes command line options to configure the test framework, executes the tests the specified number of times, and returns the number of test failures. It is essential to ensure that the `runAllTests` function pointer is correctly set to a function that executes all desired tests. The function handles command line parsing errors by returning a non-zero result, and it requires that the Unity test framework is properly initialized before calling.
- **Inputs**:
    - `argc`: The number of command line arguments. It must be non-negative and should match the number of elements in `argv`.
    - `argv`: An array of command line argument strings. The first element is typically the program name. It must not be null, and the array should contain `argc` elements.
    - `runAllTests`: A function pointer to a function that runs all the tests. This function must be defined by the user and should not be null.
- **Output**: Returns an integer representing the number of test failures. If command line options are invalid, it returns a non-zero error code.
- **See also**: [`UnityMain`](unity_fixture.c.driver.md#UnityMain)  (Implementation)


---
### UnityMalloc\_MakeMallocFailAfterCount<!-- {{#callable_declaration:UnityMalloc_MakeMallocFailAfterCount}} -->
Sets a countdown for malloc failures in tests.
- **Description**: Use this function to simulate memory allocation failures in a controlled manner during testing. It sets a countdown after which subsequent calls to malloc will fail, allowing you to test how your code handles memory allocation errors. This function is useful for testing error handling and robustness in scenarios where memory allocation might fail. It should be called before the test cases that require malloc failure simulation.
- **Inputs**:
    - `count`: Specifies the number of successful malloc calls before failures begin. Must be a non-negative integer. If set to zero, malloc will fail immediately on the next call.
- **Output**: None
- **See also**: [`UnityMalloc_MakeMallocFailAfterCount`](unity_fixture.c.driver.md#UnityMalloc_MakeMallocFailAfterCount)  (Implementation)


