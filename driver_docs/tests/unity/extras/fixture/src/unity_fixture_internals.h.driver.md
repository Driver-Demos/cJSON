# Purpose
The provided code is a C header file, `unity_fixture_internals.h`, which is part of the Unity Test Framework, a unit testing framework for C. This file defines internal structures and functions that are essential for managing and executing test fixtures within the Unity framework. The primary structure defined here is `UNITY_FIXTURE_T`, which holds configuration options such as verbosity, repeat count, and filters for test names and groups. This structure is used to control the execution of test suites and individual tests.

The file also declares several functions that facilitate the setup, execution, and teardown of tests. Key functions include [`UnityTestRunner`](#UnityTestRunner), which orchestrates the execution of a test by calling setup, body, and teardown functions, and [`UnityIgnoreTest`](#UnityIgnoreTest), which allows tests to be marked as ignored. Memory management during tests is handled by [`UnityMalloc_StartTest`](#UnityMalloc_StartTest) and [`UnityMalloc_EndTest`](#UnityMalloc_EndTest), ensuring that dynamic memory allocations are properly managed. Additionally, the file provides functionality for managing pointers during tests, with functions like [`UnityPointer_Set`](#UnityPointer_Set) and [`UnityPointer_UndoAllSets`](#UnityPointer_UndoAllSets). This header file is intended to be included in other parts of the Unity framework, providing essential internal APIs for test management and execution.
# Data Structures

---
### UNITY\_FIXTURE\_T
- **Type**: `struct`
- **Members**:
    - `Verbose`: An integer flag indicating whether verbose output is enabled.
    - `RepeatCount`: An unsigned integer specifying how many times a test should be repeated.
    - `NameFilter`: A pointer to a constant character string used to filter tests by name.
    - `GroupFilter`: A pointer to a constant character string used to filter tests by group.
- **Description**: The `UNITY_FIXTURE_T` structure is part of the Unity Test Framework for C, designed to manage test execution settings. It includes fields for controlling verbosity, repeating tests, and filtering tests by name or group, allowing for flexible and targeted test execution.


# Function Declarations (Public API)

---
### UnityTestRunner<!-- {{#callable_declaration:UnityTestRunner}} -->
Executes a unit test with setup and teardown functions.
- **Description**: This function is used to run a unit test within the Unity test framework, allowing for setup and teardown operations to be performed before and after the test body, respectively. It should be called when a specific test needs to be executed, and it handles the test's lifecycle, including setup, execution, and teardown. The function also manages test selection based on the provided group and name filters, and it outputs test results according to the verbosity setting. It is essential to ensure that the setup, test body, and teardown functions are correctly defined and that the test is intended to be run based on the current filter settings.
- **Inputs**:
    - `setup`: A pointer to a function that performs any necessary setup before the test body is executed. Must not be null.
    - `testBody`: A pointer to the function containing the test logic. Must not be null.
    - `teardown`: A pointer to a function that performs cleanup after the test body has been executed. Must not be null.
    - `printableName`: A string representing the human-readable name of the test. Must not be null.
    - `group`: A string representing the group to which the test belongs. Must not be null.
    - `name`: A string representing the unique name of the test. Must not be null.
    - `file`: A string representing the file name where the test is located. Must not be null.
    - `line`: An unsigned integer representing the line number in the file where the test is defined.
- **Output**: None
- **See also**: [`UnityTestRunner`](unity_fixture.c.driver.md#UnityTestRunner)  (Implementation)


---
### UnityIgnoreTest<!-- {{#callable_declaration:UnityIgnoreTest}} -->
Marks a test as ignored in the Unity test framework.
- **Description**: Use this function to mark a specific test as ignored during the test execution process. It should be called when a test is selected by name and group, but you want to skip its execution. This function increments the count of ignored tests and, depending on the verbosity setting, either outputs a character or prints the test's name. It is typically used within the Unity test framework to manage test execution flow and reporting.
- **Inputs**:
    - `printableName`: A string representing the human-readable name of the test. Must not be null.
    - `group`: A string representing the group to which the test belongs. Must not be null.
    - `name`: A string representing the unique name of the test. Must not be null.
- **Output**: None
- **See also**: [`UnityIgnoreTest`](unity_fixture.c.driver.md#UnityIgnoreTest)  (Implementation)


---
### UnityMalloc\_StartTest<!-- {{#callable_declaration:UnityMalloc_StartTest}} -->
Initialize memory allocation tracking for a test.
- **Description**: This function is used to prepare the memory allocation tracking system at the start of a test. It resets the internal counters that track the number of allocations and any conditions for forced allocation failures. This function should be called before any test that involves dynamic memory allocation to ensure accurate tracking and failure simulation. It is typically used in conjunction with UnityMalloc_EndTest to finalize the tracking after the test completes.
- **Inputs**: None
- **Output**: None
- **See also**: [`UnityMalloc_StartTest`](unity_fixture.c.driver.md#UnityMalloc_StartTest)  (Implementation)


---
### UnityMalloc\_EndTest<!-- {{#callable_declaration:UnityMalloc_EndTest}} -->
Ends a test and checks for memory leaks.
- **Description**: This function should be called at the end of a test to ensure that no memory leaks have occurred during the test execution. It resets the internal memory allocation failure countdown and checks if the number of active memory allocations is zero. If there are any active allocations, it triggers a test failure, indicating a memory leak. This function is typically used in conjunction with UnityMalloc_StartTest to bracket test code that involves dynamic memory allocation.
- **Inputs**: None
- **Output**: None
- **See also**: [`UnityMalloc_EndTest`](unity_fixture.c.driver.md#UnityMalloc_EndTest)  (Implementation)


---
### UnityGetCommandLineOptions<!-- {{#callable_declaration:UnityGetCommandLineOptions}} -->
Parses command line options for Unity test framework configuration.
- **Description**: This function processes command line arguments to configure the Unity test framework's behavior. It should be called with the argument count and argument vector typically provided to the main function of a C program. The function recognizes specific options: '-v' for verbose output, '-g' followed by a group name to filter tests by group, '-n' followed by a test name to filter tests by name, and '-r' followed by a number to set the repeat count for test execution. If '-r' is provided without a number, the repeat count defaults to 2. Unknown options are ignored. The function returns 0 on success and 1 if a required argument for an option is missing.
- **Inputs**:
    - `argc`: The number of command line arguments, including the program name. Must be at least 1.
    - `argv`: An array of strings representing the command line arguments. The first element is typically the program name, and subsequent elements are the options and their arguments. Must not be null.
- **Output**: Returns 0 on successful parsing of options, or 1 if a required argument for an option is missing.
- **See also**: [`UnityGetCommandLineOptions`](unity_fixture.c.driver.md#UnityGetCommandLineOptions)  (Implementation)


---
### UnityConcludeFixtureTest<!-- {{#callable_declaration:UnityConcludeFixtureTest}} -->
Concludes the current fixture test and updates test results.
- **Description**: This function should be called at the end of a fixture test to finalize the test results. It checks whether the current test was ignored or failed and updates the respective counters for ignored or failed tests. If the test passed and verbose mode is enabled, it prints a 'PASS' message. After processing, it resets the test status flags for the next test. This function assumes that the test status flags have been appropriately set during the test execution.
- **Inputs**: None
- **Output**: None
- **See also**: [`UnityConcludeFixtureTest`](unity_fixture.c.driver.md#UnityConcludeFixtureTest)  (Implementation)


---
### UnityPointer\_Set<!-- {{#callable_declaration:UnityPointer_Set}} -->
Sets a pointer to a new value and records the change for potential undo.
- **Description**: This function is used to change the value of a pointer and record the original value for potential restoration later. It is typically used in testing scenarios where pointer values need to be temporarily altered. The function must be called with a valid pointer to a pointer, and it will fail if the maximum number of pointers that can be set is exceeded. This function is part of a test framework and should be used in conjunction with other Unity test functions.
- **Inputs**:
    - `ptr`: A double pointer to the memory location that will be updated. Must not be null, and the caller retains ownership.
    - `newValue`: The new value to set the pointer to. Can be null if setting the pointer to null is desired.
    - `line`: The line number in the test file where this function is called, used for error reporting.
- **Output**: None
- **See also**: [`UnityPointer_Set`](unity_fixture.c.driver.md#UnityPointer_Set)  (Implementation)


---
### UnityPointer\_UndoAllSets<!-- {{#callable_declaration:UnityPointer_UndoAllSets}} -->
Restores all pointers to their previous values.
- **Description**: Use this function to revert all pointer modifications made by UnityPointer_Set during a test. It is typically called at the end of a test to ensure that any changes to pointers are undone, maintaining test isolation and preventing side effects from affecting subsequent tests. This function should be used only after pointers have been set using UnityPointer_Set, and it assumes that the pointer index is greater than zero, indicating that there are pointers to undo.
- **Inputs**: None
- **Output**: None
- **See also**: [`UnityPointer_UndoAllSets`](unity_fixture.c.driver.md#UnityPointer_UndoAllSets)  (Implementation)


---
### UnityPointer\_Init<!-- {{#callable_declaration:UnityPointer_Init}} -->
Initialize the pointer tracking system.
- **Description**: This function initializes the pointer tracking system used by the Unity test framework. It should be called before any pointer operations are performed to ensure the system is in a known state. This is typically done at the start of a test suite or before any tests that involve pointer manipulation. The function does not take any parameters and does not return a value, ensuring that the pointer tracking index is reset to its initial state.
- **Inputs**: None
- **Output**: None
- **See also**: [`UnityPointer_Init`](unity_fixture.c.driver.md#UnityPointer_Init)  (Implementation)


