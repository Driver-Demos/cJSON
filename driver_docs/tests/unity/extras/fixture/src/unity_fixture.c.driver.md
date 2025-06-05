# Purpose
This C source code file is part of the Unity Test Framework, a unit testing framework for C, and it provides essential functionality for running and managing test cases. The file defines several key components, including the [`UnityMain`](#UnityMain) function, which serves as the entry point for executing tests. It processes command-line arguments to configure test execution, such as verbosity and filtering by test name or group. The file also includes functions for managing test setup and teardown, as well as functions for running individual tests ([`UnityTestRunner`](#UnityTestRunner)) and handling ignored tests ([`UnityIgnoreTest`](#UnityIgnoreTest)). These functions ensure that tests are executed in a controlled environment, with proper setup and teardown procedures, and they provide feedback on test results.

Additionally, the file includes custom memory management functions, such as [`unity_malloc`](#unity_malloc), [`unity_free`](#unity_free), [`unity_calloc`](#unity_calloc), and [`unity_realloc`](#unity_realloc), which are designed to track memory allocations and detect memory leaks or buffer overruns during test execution. This is crucial for ensuring that tests do not inadvertently introduce memory-related issues. The file also implements a mechanism for managing and restoring pointer values, which is useful for tests that need to temporarily modify pointers. Overall, this file is a comprehensive component of the Unity Test Framework, providing both test execution control and memory management capabilities to facilitate reliable and efficient unit testing in C.
# Imports and Dependencies

---
- `unity_fixture.h`
- `unity_internals.h`
- `string.h`
- `stdlib.h`


# Global Variables

---
### UnityFixture
- **Type**: `struct UNITY_FIXTURE_T`
- **Description**: The `UnityFixture` is a global variable of type `struct UNITY_FIXTURE_T`, which is part of the Unity Test Framework for C. This structure is used to manage and control the execution of test fixtures, including options for verbosity, filtering by group or name, and repeating test runs.
- **Use**: This variable is used to store configuration and state information for running test fixtures, such as verbosity level, group and name filters, and repeat count.


---
### malloc\_count
- **Type**: `int`
- **Description**: `malloc_count` is a static integer variable used to track the number of active memory allocations made by the custom memory management functions in the Unity test framework. It is incremented each time a memory allocation is successfully made and decremented when memory is freed.
- **Use**: This variable is used to detect memory leaks by ensuring that all allocated memory is properly freed by the end of a test.


---
### malloc\_fail\_countdown
- **Type**: `int`
- **Description**: The `malloc_fail_countdown` is a static integer variable initialized to `MALLOC_DONT_FAIL`, which is defined as -1. It is used to control the behavior of memory allocation failures in the Unity test framework.
- **Use**: This variable is used to simulate memory allocation failures after a specified number of successful allocations, aiding in testing the robustness of code against such failures.


---
### unity\_heap
- **Type**: `unsigned char array`
- **Description**: The `unity_heap` is a static array of unsigned characters used as an internal heap for memory allocation when the standard library's malloc is excluded. Its size is determined by the `UNITY_INTERNAL_HEAP_SIZE_BYTES` macro.
- **Use**: This variable is used to manage memory allocation within the Unity test framework when standard library functions are not available.


---
### heap\_index
- **Type**: `size_t`
- **Description**: The `heap_index` is a static variable of type `size_t` used to track the current position in the `unity_heap` array, which is a simulated heap for memory allocation when the standard library's malloc is excluded. It is used to manage memory allocation and deallocation within the Unity test framework.
- **Use**: `heap_index` is used to keep track of the next available position in the `unity_heap` for memory allocation operations.


---
### end
- **Type**: ``const char[]``
- **Description**: The `end` variable is a static constant character array initialized with the string "END". It is used as a marker or sentinel value in memory management functions to detect buffer overruns.
- **Use**: This variable is used in memory allocation functions to check for buffer overruns by comparing it with the end of allocated memory blocks.


---
### pointer\_store
- **Type**: `struct PointerPair[]`
- **Description**: The `pointer_store` is a static array of `PointerPair` structures, which is used to store pointers and their original values. This array is used to manage and restore pointers during test execution in the Unity test framework.
- **Use**: This variable is used to keep track of pointers that have been modified during a test, allowing them to be restored to their original values after the test completes.


---
### pointer\_index
- **Type**: `int`
- **Description**: The `pointer_index` is a static integer variable initialized to 0. It is used to track the current index in the `pointer_store` array, which stores pairs of pointers and their old values for automatic restoration.
- **Use**: `pointer_index` is used to manage the number of pointers currently stored in `pointer_store` and to iterate over them for restoration.


# Data Structures

---
### Guard
- **Type**: `struct`
- **Members**:
    - `size`: Represents the size of the memory block being guarded.
    - `guard_space`: Used to store additional space for guarding against memory overrun.
- **Description**: The `Guard` structure is used to manage memory allocation in a way that helps detect buffer overruns. It contains two members: `size`, which indicates the size of the allocated memory block, and `guard_space`, which is used to store extra space for guarding purposes. This structure is part of a custom memory management system that checks for memory overruns by appending a known pattern at the end of the allocated memory and verifying it upon deallocation.


---
### PointerPair
- **Type**: `struct`
- **Members**:
    - `pointer`: A double pointer to a void type, used to store the address of a pointer.
    - `old_value`: A void pointer to store the previous value of the pointer.
- **Description**: The `PointerPair` structure is designed to manage and restore pointer values in a testing environment. It holds a reference to a pointer and its previous value, allowing for the temporary modification of the pointer's value during a test and the ability to revert it back to its original state afterwards. This is particularly useful in scenarios where pointer values need to be manipulated and then restored to ensure test isolation and integrity.


# Functions

---
### setUp<!-- {{#callable:setUp}} -->
The `setUp` function is a placeholder function that performs no operations.
- **Inputs**: None
- **Control Flow**:
    - The function is defined with the `void` return type and takes no parameters.
    - The function body contains a comment indicating that it does nothing.
- **Output**: The function does not produce any output or perform any operations.


---
### tearDown<!-- {{#callable:tearDown}} -->
The `tearDown` function is a placeholder function that performs no operations.
- **Inputs**: None
- **Control Flow**:
    - The function is defined with no parameters and an empty body, indicating it performs no actions.
    - It is conditionally compiled only if `UNITY_WEAK_ATTRIBUTE` and `UNITY_WEAK_PRAGMA` are not defined.
- **Output**: The function does not return any value or perform any operations.


---
### announceTestRun<!-- {{#callable:announceTestRun}} -->
The `announceTestRun` function prints the current test run number and the total number of test runs to the console.
- **Inputs**:
    - `runNumber`: An unsigned integer representing the current test run number, starting from 0.
- **Control Flow**:
    - The function begins by printing the string 'Unity test run ' to the console.
    - It then prints the current test run number, which is `runNumber + 1`, to account for zero-based indexing.
    - Next, it prints the string ' of ' to the console.
    - It prints the total number of test runs, which is stored in `UnityFixture.RepeatCount`.
    - Finally, it calls `UNITY_PRINT_EOL()` to print a newline character, ending the output.
- **Output**: The function does not return any value; it outputs information to the console.
- **Functions called**:
    - [`UnityPrint`](../../../src/unity.c.driver.md#UnityPrint)
    - [`UnityPrintNumberUnsigned`](../../../src/unity.c.driver.md#UnityPrintNumberUnsigned)


---
### UnityMain<!-- {{#callable:UnityMain}} -->
The `UnityMain` function initializes and executes a series of test runs based on command line options and a provided test execution function.
- **Inputs**:
    - `argc`: The number of command line arguments.
    - `argv`: An array of command line argument strings.
    - `runAllTests`: A function pointer to the function that runs all the tests.
- **Control Flow**:
    - Call [`UnityGetCommandLineOptions`](#UnityGetCommandLineOptions) to parse command line arguments and set test options.
    - If the result from [`UnityGetCommandLineOptions`](#UnityGetCommandLineOptions) is non-zero, return this result immediately.
    - Iterate over the number of test repetitions specified by `UnityFixture.RepeatCount`.
    - For each repetition, call [`UnityBegin`](../../../src/unity.c.driver.md#UnityBegin) to initialize the test run.
    - Announce the current test run using [`announceTestRun`](#announceTestRun).
    - Execute the tests by calling the function pointed to by `runAllTests`.
    - If verbosity is not enabled, print a newline character after the test run.
    - Call [`UnityEnd`](../../../src/unity.c.driver.md#UnityEnd) to finalize the test run.
    - Return the number of test failures recorded in `Unity.TestFailures`.
- **Output**: The function returns an integer representing the number of test failures, or an error code if command line options parsing fails.
- **Functions called**:
    - [`UnityGetCommandLineOptions`](#UnityGetCommandLineOptions)
    - [`UnityBegin`](../../../src/unity.c.driver.md#UnityBegin)
    - [`announceTestRun`](#announceTestRun)
    - [`UnityEnd`](../../../src/unity.c.driver.md#UnityEnd)


---
### selected<!-- {{#callable:selected}} -->
The `selected` function checks if a given name contains a specified filter string, returning true if it does or if no filter is provided.
- **Inputs**:
    - `filter`: A pointer to a constant character string representing the filter to search for within the name.
    - `name`: A pointer to a constant character string representing the name to be checked against the filter.
- **Control Flow**:
    - Check if the `filter` is `NULL` (or zero), and if so, return 1 indicating the name is selected by default.
    - If the `filter` is not `NULL`, use `strstr` to check if the `name` contains the `filter` string.
    - Return 1 if `strstr` finds the `filter` in `name`, otherwise return 0.
- **Output**: Returns an integer value: 1 if the `name` contains the `filter` or if the `filter` is `NULL`, otherwise 0.


---
### testSelected<!-- {{#callable:testSelected}} -->
The `testSelected` function checks if a given test name matches the current name filter set in the Unity test framework.
- **Inputs**:
    - `test`: A constant character pointer representing the name of the test to be checked against the name filter.
- **Control Flow**:
    - The function calls the [`selected`](#selected) function, passing `UnityFixture.NameFilter` and the `test` argument.
    - The [`selected`](#selected) function checks if the `NameFilter` is null, returning 1 if it is, indicating all tests are selected.
    - If `NameFilter` is not null, it uses `strstr` to check if the `test` name contains the `NameFilter` as a substring, returning 1 if true, otherwise 0.
- **Output**: An integer value, 1 if the test name matches the name filter or if no filter is set, otherwise 0.
- **Functions called**:
    - [`selected`](#selected)


---
### groupSelected<!-- {{#callable:groupSelected}} -->
The `groupSelected` function checks if a given group name matches the current group filter set in the Unity test framework.
- **Inputs**:
    - `group`: A constant character pointer representing the name of the group to be checked against the group filter.
- **Control Flow**:
    - The function calls the [`selected`](#selected) function, passing `UnityFixture.GroupFilter` and the `group` argument.
    - The [`selected`](#selected) function checks if the `group` name contains the `GroupFilter` string using `strstr`.
    - If `GroupFilter` is `NULL`, [`selected`](#selected) returns 1, indicating a match.
    - If `strstr` finds the `GroupFilter` in `group`, [`selected`](#selected) returns 1, otherwise it returns 0.
- **Output**: The function returns an integer, 1 if the group matches the filter or if no filter is set, and 0 otherwise.
- **Functions called**:
    - [`selected`](#selected)


---
### UnityTestRunner<!-- {{#callable:UnityTestRunner}} -->
The `UnityTestRunner` function executes a test case by running setup, test body, and teardown functions, while managing test selection, memory allocation, and pointer restoration.
- **Inputs**:
    - `setup`: A pointer to a function that sets up the test environment before the test body is executed.
    - `testBody`: A pointer to the function that contains the actual test logic to be executed.
    - `teardown`: A pointer to a function that cleans up the test environment after the test body has been executed.
    - `printableName`: A string representing the name of the test, used for display purposes.
    - `group`: A string representing the group to which the test belongs, used for filtering tests.
    - `name`: A string representing the specific name of the test, used for filtering tests.
    - `file`: A string representing the file name where the test is located, used for reporting.
    - `line`: An unsigned integer representing the line number in the file where the test is defined, used for reporting.
- **Control Flow**:
    - Check if the test is selected based on its name and group using [`testSelected`](#testSelected) and [`groupSelected`](#groupSelected) functions.
    - If the test is selected, set the current test file, name, and line number in the Unity framework.
    - If verbose mode is off, output a dot character; otherwise, print the test name.
    - Increment the number of tests counter.
    - Initialize memory management and pointer tracking for the test.
    - Use `TEST_PROTECT` to safely execute the setup function, followed by the test body function.
    - Use `TEST_PROTECT` to safely execute the teardown function.
    - Use `TEST_PROTECT` to undo all pointer sets and end memory management if the test did not fail.
    - Conclude the test fixture by updating test results and outputting the test status.
- **Output**: The function does not return a value; it manages the execution and reporting of a test case within the Unity test framework.
- **Functions called**:
    - [`testSelected`](#testSelected)
    - [`groupSelected`](#groupSelected)
    - [`UnityPrint`](../../../src/unity.c.driver.md#UnityPrint)
    - [`UnityMalloc_StartTest`](#UnityMalloc_StartTest)
    - [`UnityPointer_Init`](#UnityPointer_Init)
    - [`UnityPointer_UndoAllSets`](#UnityPointer_UndoAllSets)
    - [`UnityMalloc_EndTest`](#UnityMalloc_EndTest)
    - [`UnityConcludeFixtureTest`](#UnityConcludeFixtureTest)


---
### UnityIgnoreTest<!-- {{#callable:UnityIgnoreTest}} -->
The `UnityIgnoreTest` function increments the count of ignored tests and optionally prints the test name if the test and group are selected.
- **Inputs**:
    - `printableName`: A string representing the name of the test to be printed if verbose mode is enabled.
    - `group`: A string representing the group name to which the test belongs.
    - `name`: A string representing the specific name of the test to be ignored.
- **Control Flow**:
    - Check if the test is selected using `testSelected(name)` and if the group is selected using `groupSelected(group)`.
    - If both conditions are true, increment `Unity.NumberOfTests` and `Unity.TestIgnores`.
    - If `UnityFixture.Verbose` is false, output a '!' character to indicate an ignored test.
    - If `UnityFixture.Verbose` is true, print the `printableName` and a newline.
- **Output**: The function does not return a value; it modifies global state by incrementing test counters and optionally outputs to the console.
- **Functions called**:
    - [`testSelected`](#testSelected)
    - [`groupSelected`](#groupSelected)
    - [`UnityPrint`](../../../src/unity.c.driver.md#UnityPrint)


---
### UnityMalloc\_StartTest<!-- {{#callable:UnityMalloc_StartTest}} -->
The `UnityMalloc_StartTest` function initializes the memory allocation tracking variables for a test by resetting the allocation count and setting the failure countdown to a default non-failing state.
- **Inputs**: None
- **Control Flow**:
    - Set the global variable `malloc_count` to 0, indicating no memory allocations have been made yet.
    - Set the global variable `malloc_fail_countdown` to `MALLOC_DONT_FAIL`, indicating that memory allocations should not fail by default.
- **Output**: This function does not return any value.


---
### UnityMalloc\_EndTest<!-- {{#callable:UnityMalloc_EndTest}} -->
The `UnityMalloc_EndTest` function checks for memory leaks by verifying if the `malloc_count` is zero at the end of a test and fails the test if any memory leaks are detected.
- **Inputs**: None
- **Control Flow**:
    - Set `malloc_fail_countdown` to `MALLOC_DONT_FAIL` to reset the failure countdown for memory allocations.
    - Check if `malloc_count` is not zero, indicating that there are unfreed memory allocations.
    - If there are unfreed allocations, call `UNITY_TEST_FAIL` with the current test line number and a message indicating a memory leak.
- **Output**: The function does not return a value; it performs a check and potentially fails the test if a memory leak is detected.


---
### UnityMalloc\_MakeMallocFailAfterCount<!-- {{#callable:UnityMalloc_MakeMallocFailAfterCount}} -->
The function `UnityMalloc_MakeMallocFailAfterCount` sets a countdown for when malloc operations should start failing.
- **Inputs**:
    - `countdown`: An integer specifying the number of successful malloc operations before malloc should start failing.
- **Control Flow**:
    - The function assigns the input parameter `countdown` to the global variable `malloc_fail_countdown`.
- **Output**: The function does not return any value.


---
### unity\_malloc<!-- {{#callable:unity_malloc}} -->
The `unity_malloc` function allocates memory with additional guard bytes for buffer overrun detection and handles custom failure conditions for testing purposes.
- **Inputs**:
    - `size`: The size in bytes of the memory block to allocate.
- **Control Flow**:
    - Calculate the total size needed, including guard bytes and an end marker.
    - Check if the malloc failure countdown is active and return NULL if it reaches zero, decrementing the countdown otherwise.
    - Return NULL if the requested size is zero.
    - If UNITY_EXCLUDE_STDLIB_MALLOC is defined, check if there is enough space in the internal heap and allocate memory from it; otherwise, use standard malloc.
    - Return NULL if memory allocation fails.
    - Increment the malloc count to track allocations.
    - Initialize the guard structure with the requested size and zero guard space.
    - Set the memory pointer to the space after the guard structure and copy the end marker to the end of the allocated block.
    - Return the pointer to the allocated memory block.
- **Output**: A pointer to the allocated memory block, or NULL if allocation fails.


---
### isOverrun<!-- {{#callable:isOverrun}} -->
The `isOverrun` function checks if a memory buffer has been overrun by examining guard bytes and a sentinel string.
- **Inputs**:
    - `mem`: A pointer to the memory block that needs to be checked for overruns.
- **Control Flow**:
    - Cast the input `mem` to a `Guard` pointer and decrement it to access the guard structure preceding the memory block.
    - Cast the input `mem` to a `char` pointer for byte-level operations.
    - Check if the `guard_space` field of the `Guard` structure is non-zero, indicating a potential overrun.
    - Check if the string at the end of the allocated memory block matches the sentinel string `end`, indicating no overrun if they match.
    - Return true (non-zero) if either the `guard_space` is non-zero or the sentinel string does not match, indicating an overrun; otherwise, return false (zero).
- **Output**: An integer value, where non-zero indicates a buffer overrun and zero indicates no overrun.


---
### release\_memory<!-- {{#callable:release_memory}} -->
The `release_memory` function deallocates memory previously allocated by `unity_malloc` and updates the memory management state accordingly.
- **Inputs**:
    - `mem`: A pointer to the memory block that needs to be released.
- **Control Flow**:
    - The function casts the input pointer `mem` to a `Guard` pointer and decrements it to access the `Guard` structure associated with the allocated memory.
    - It decrements the global `malloc_count` to reflect the release of a memory block.
    - If `UNITY_EXCLUDE_STDLIB_MALLOC` is defined, it checks if the memory block is at the end of the heap and adjusts the `heap_index` to effectively release the memory.
    - If `UNITY_EXCLUDE_STDLIB_MALLOC` is not defined, it uses `UNITY_FIXTURE_FREE` to deallocate the memory block.
- **Output**: The function does not return any value.


---
### unity\_free<!-- {{#callable:unity_free}} -->
The `unity_free` function releases allocated memory and checks for buffer overruns before freeing the memory.
- **Inputs**:
    - `mem`: A pointer to the memory block that needs to be freed.
- **Control Flow**:
    - Check if the input pointer `mem` is NULL; if so, return immediately without doing anything.
    - Call [`isOverrun`](#isOverrun) to check if the memory block has been overrun and store the result in `overrun`.
    - Call [`release_memory`](#release_memory) to free the memory block pointed to by `mem`.
    - If `overrun` is true, call `UNITY_TEST_FAIL` to report a buffer overrun error.
- **Output**: The function does not return a value; it performs memory management and error reporting as side effects.
- **Functions called**:
    - [`isOverrun`](#isOverrun)
    - [`release_memory`](#release_memory)


---
### unity\_calloc<!-- {{#callable:unity_calloc}} -->
The `unity_calloc` function allocates memory for an array of elements, initializes all bytes to zero, and returns a pointer to the allocated memory.
- **Inputs**:
    - `num`: The number of elements to allocate memory for.
    - `size`: The size of each element in bytes.
- **Control Flow**:
    - Call [`unity_malloc`](#unity_malloc) to allocate memory for `num * size` bytes.
    - Check if the memory allocation was successful; if not, return `NULL`.
    - Use `memset` to initialize all allocated bytes to zero.
    - Return the pointer to the allocated and initialized memory.
- **Output**: A pointer to the allocated memory block, or `NULL` if the allocation fails.
- **Functions called**:
    - [`unity_malloc`](#unity_malloc)


---
### unity\_realloc<!-- {{#callable:unity_realloc}} -->
The `unity_realloc` function reallocates a block of memory, handling buffer overruns and optimizing memory usage when possible.
- **Inputs**:
    - `oldMem`: A pointer to the previously allocated memory block that needs to be reallocated.
    - `size`: The new size in bytes for the memory block.
- **Control Flow**:
    - Check if `oldMem` is NULL; if so, allocate new memory of the given size using [`unity_malloc`](#unity_malloc) and return it.
    - Decrement the guard pointer to access the metadata of the memory block.
    - Check for buffer overrun using [`isOverrun`](#isOverrun); if detected, release the memory and fail the test with an error message.
    - If the requested size is zero, release the memory and return NULL.
    - If the current memory block is already large enough, return the original memory block.
    - If `UNITY_EXCLUDE_STDLIB_MALLOC` is defined, check if the memory can be expanded in place; if so, release the old memory and allocate new memory without copying data.
    - Allocate new memory of the requested size using [`unity_malloc`](#unity_malloc); if allocation fails, return NULL without releasing the old memory.
    - Copy the contents of the old memory block to the new memory block using `memcpy`.
    - Release the old memory block using [`release_memory`](#release_memory).
    - Return the pointer to the newly allocated memory block.
- **Output**: A pointer to the newly allocated memory block, or NULL if the allocation fails or if the requested size is zero.
- **Functions called**:
    - [`unity_malloc`](#unity_malloc)
    - [`isOverrun`](#isOverrun)
    - [`release_memory`](#release_memory)


---
### UnityPointer\_Init<!-- {{#callable:UnityPointer_Init}} -->
The `UnityPointer_Init` function initializes the pointer index to zero, effectively resetting the pointer tracking system.
- **Inputs**: None
- **Control Flow**:
    - The function sets the global variable `pointer_index` to 0.
- **Output**: The function does not return any value (void function).


---
### UnityPointer\_Set<!-- {{#callable:UnityPointer_Set}} -->
The `UnityPointer_Set` function updates a pointer to a new value while storing its old value for potential restoration, ensuring the number of pointers does not exceed a predefined limit.
- **Inputs**:
    - `pointer`: A double pointer (void**) to the memory location that needs to be updated.
    - `newValue`: A void pointer to the new value that the pointer should point to.
    - `line`: A UNITY_LINE_TYPE variable representing the line number in the test file, used for error reporting.
- **Control Flow**:
    - Check if the current pointer index exceeds the maximum allowed pointers (UNITY_MAX_POINTERS).
    - If the limit is exceeded, call UNITY_TEST_FAIL with the line number and an error message indicating too many pointers are set.
    - If the limit is not exceeded, store the current pointer and its old value in the pointer_store array at the current pointer index.
    - Update the pointer to point to the new value.
    - Increment the pointer index to reflect the addition of a new pointer.
- **Output**: The function does not return a value; it modifies the pointer in place and updates internal state for pointer tracking.


---
### UnityPointer\_UndoAllSets<!-- {{#callable:UnityPointer_UndoAllSets}} -->
The `UnityPointer_UndoAllSets` function restores all pointers in the `pointer_store` array to their original values by iterating backwards through the stored pointers.
- **Inputs**: None
- **Control Flow**:
    - The function enters a while loop that continues as long as `pointer_index` is greater than 0.
    - Within the loop, `pointer_index` is decremented by 1.
    - The pointer at the current `pointer_index` in `pointer_store` is set to its `old_value`.
- **Output**: The function does not return any value; it modifies the pointers in `pointer_store` to their original values.


---
### UnityGetCommandLineOptions<!-- {{#callable:UnityGetCommandLineOptions}} -->
The `UnityGetCommandLineOptions` function parses command-line arguments to configure test execution settings such as verbosity, group and name filters, and repeat count for the Unity test framework.
- **Inputs**:
    - `argc`: The number of command-line arguments passed to the program.
    - `argv`: An array of strings representing the command-line arguments.
- **Control Flow**:
    - Initialize UnityFixture settings: Verbose to 0, GroupFilter to 0, NameFilter to 0, and RepeatCount to 1.
    - Check if only one argument is provided (the program name), and return 0 if true, indicating no options to process.
    - Iterate over the command-line arguments starting from the second argument (index 1).
    - If '-v' is found, set UnityFixture.Verbose to 1 and move to the next argument.
    - If '-g' is found, check if the next argument exists; if not, return 1. Otherwise, set UnityFixture.GroupFilter to the next argument and move to the next argument.
    - If '-n' is found, check if the next argument exists; if not, return 1. Otherwise, set UnityFixture.NameFilter to the next argument and move to the next argument.
    - If '-r' is found, set UnityFixture.RepeatCount to 2 and check if the next argument is a number. If it is, parse the number and set UnityFixture.RepeatCount to this value, then move to the next argument.
    - Ignore any unknown parameters and continue to the next argument.
    - Return 0 after processing all arguments.
- **Output**: Returns 0 if command-line options are successfully parsed, or 1 if an expected argument is missing after '-g' or '-n'.


---
### UnityConcludeFixtureTest<!-- {{#callable:UnityConcludeFixtureTest}} -->
The `UnityConcludeFixtureTest` function finalizes the result of a test by updating counters for ignored or failed tests and printing the appropriate status message.
- **Inputs**: None
- **Control Flow**:
    - Check if the current test is ignored; if so, increment the ignore counter and print a newline.
    - If the test is not ignored and not failed, check if verbose mode is enabled; if so, print 'PASS' and a newline.
    - If the test has failed, increment the failure counter and print a newline.
    - Reset the `CurrentTestFailed` and `CurrentTestIgnored` flags to 0.
- **Output**: The function does not return any value; it updates global state variables related to test results.
- **Functions called**:
    - [`UnityPrint`](../../../src/unity.c.driver.md#UnityPrint)


