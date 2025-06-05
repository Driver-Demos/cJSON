# Purpose
This C source code file is a test suite designed to validate the functionality of the cJSON library, which is used for parsing and handling JSON data in C. The file utilizes the Unity test framework to define and run a series of unit tests that check the correctness of JSON parsing and serialization operations. The tests cover a range of scenarios, including successful parsing and printing of JSON files, handling of incomplete or malformed JSON data, and ensuring that the library does not cause buffer overflows or other memory-related issues. Each test function is designed to verify specific aspects of the cJSON library's behavior, such as parsing JSON strings of varying completeness and structure, and comparing the parsed output against expected results.

The file includes several static functions that encapsulate the logic for individual tests, such as [`do_test`](#do_test) for general parsing and comparison, and specific tests like [`test12_should_not_be_parsed`](#test12_should_not_be_parsed) for handling incomplete JSON. The [`main`](#CJSON_CDECLmain) function orchestrates the execution of these tests using Unity's `RUN_TEST` macro, ensuring that each test is executed and its results are reported. This test suite is crucial for maintaining the reliability and robustness of the cJSON library by systematically identifying and addressing potential issues in JSON processing.
# Imports and Dependencies

---
- `stdio.h`
- `stdlib.h`
- `string.h`
- `unity/examples/unity_config.h`
- `unity/src/unity.h`
- `common.h`


# Functions

---
### parse\_file<!-- {{#callable:parse_file}} -->
The `parse_file` function reads a file and parses its content as a JSON object using the cJSON library.
- **Inputs**:
    - `filename`: A constant character pointer representing the name of the file to be read and parsed.
- **Control Flow**:
    - Initialize a cJSON pointer `parsed` to NULL.
    - Call [`read_file`](common.h.driver.md#read_file) with `filename` to read the file content into a string `content`.
    - Parse the `content` string into a cJSON object using `cJSON_Parse` and assign the result to `parsed`.
    - If `content` is not NULL, free the allocated memory for `content`.
    - Return the parsed cJSON object `parsed`.
- **Output**: A pointer to a cJSON object representing the parsed JSON data from the file, or NULL if parsing fails.
- **Functions called**:
    - [`read_file`](common.h.driver.md#read_file)


---
### do\_test<!-- {{#callable:do_test}} -->
The `do_test` function reads a test JSON file, parses it, and compares the parsed output to an expected output file to verify correctness.
- **Inputs**:
    - `test_name`: A string representing the name of the test, which is used to construct file paths for the test input and expected output.
- **Control Flow**:
    - Calculate the length of the `test_name` string.
    - Allocate memory for `test_path` and `expected_path` using the test name and predefined directory path.
    - Construct the file paths for the test input and expected output using `sprintf`.
    - Read the expected output from the file specified by `expected_path`.
    - Parse the test input file specified by `test_path` into a cJSON object `tree`.
    - Convert the parsed cJSON object `tree` back into a JSON string `actual`.
    - Compare the `actual` JSON string with the `expected` string to ensure they match.
    - Free all allocated resources, including strings and cJSON objects, to prevent memory leaks.
- **Output**: The function does not return a value; it performs assertions to validate the test and will terminate the test with an error message if any assertion fails.
- **Functions called**:
    - [`read_file`](common.h.driver.md#read_file)
    - [`parse_file`](#parse_file)


---
### file\_test1\_should\_be\_parsed\_and\_printed<!-- {{#callable:file_test1_should_be_parsed_and_printed}} -->
The function `file_test1_should_be_parsed_and_printed` executes a test to parse and print the JSON content of a file named 'test1'.
- **Inputs**: None
- **Control Flow**:
    - The function calls [`do_test`](#do_test) with the argument "test1".
    - The [`do_test`](#do_test) function constructs file paths for the test input and expected output using the provided test name.
    - It reads the expected output from the constructed path and parses the test input file into a cJSON object.
    - The parsed JSON object is printed back to a string and compared with the expected output string.
    - Assertions are used to ensure that the file reading, parsing, and printing operations are successful and that the actual output matches the expected output.
    - Resources such as file paths, expected output, parsed JSON tree, and printed JSON string are freed after use.
- **Output**: The function does not return any value; it performs assertions to validate the test and may terminate the program if an assertion fails.
- **Functions called**:
    - [`do_test`](#do_test)


---
### file\_test2\_should\_be\_parsed\_and\_printed<!-- {{#callable:file_test2_should_be_parsed_and_printed}} -->
The function `file_test2_should_be_parsed_and_printed` executes a test to parse and print the JSON content of a file named 'test2'.
- **Inputs**: None
- **Control Flow**:
    - The function calls [`do_test`](#do_test) with the argument "test2".
    - The [`do_test`](#do_test) function constructs file paths for the input and expected output files using the test name.
    - It reads the expected output from the expected file path and parses the input file content into a JSON tree.
    - The parsed JSON tree is printed back to a JSON string and compared with the expected output.
    - Assertions are used to ensure that the file reading, parsing, and printing operations are successful and that the actual output matches the expected output.
    - Resources such as file paths, expected output, parsed JSON tree, and printed JSON string are freed after use.
- **Output**: The function does not return any value; it performs assertions to validate the test.
- **Functions called**:
    - [`do_test`](#do_test)


---
### file\_test3\_should\_be\_parsed\_and\_printed<!-- {{#callable:file_test3_should_be_parsed_and_printed}} -->
The function `file_test3_should_be_parsed_and_printed` executes a test to parse and print the JSON content of a file named 'test3'.
- **Inputs**: None
- **Control Flow**:
    - The function calls [`do_test`](#do_test) with the argument "test3".
    - [`do_test`](#do_test) constructs file paths for the test input and expected output using the provided test name.
    - It reads the expected output from the constructed path and parses the test input file into a cJSON object.
    - The parsed JSON object is then printed back to a JSON string.
    - The function compares the printed JSON string with the expected output to verify correctness.
    - Resources such as file paths, JSON objects, and strings are freed after use.
- **Output**: The function does not return any value; it performs a test and asserts the correctness of JSON parsing and printing.
- **Functions called**:
    - [`do_test`](#do_test)


---
### file\_test4\_should\_be\_parsed\_and\_printed<!-- {{#callable:file_test4_should_be_parsed_and_printed}} -->
The function `file_test4_should_be_parsed_and_printed` executes a test to parse and print the JSON content of a file named 'test4'.
- **Inputs**: None
- **Control Flow**:
    - The function calls [`do_test`](#do_test) with the argument "test4".
    - The [`do_test`](#do_test) function constructs file paths for the input file and expected output file using the test name.
    - It reads the expected output from the expected file path and parses the input file using `parse_file`.
    - The parsed JSON tree is printed back to a JSON string and compared with the expected output.
    - Assertions are used to ensure that the expected and actual outputs match, and resources are cleaned up after the test.
- **Output**: The function does not return any value; it performs assertions to validate the test.
- **Functions called**:
    - [`do_test`](#do_test)


---
### file\_test5\_should\_be\_parsed\_and\_printed<!-- {{#callable:file_test5_should_be_parsed_and_printed}} -->
The function `file_test5_should_be_parsed_and_printed` executes a test to parse and print the JSON content of a file named 'test5'.
- **Inputs**: None
- **Control Flow**:
    - The function calls [`do_test`](#do_test) with the argument "test5".
    - The [`do_test`](#do_test) function handles the parsing and printing of the JSON content from the file named 'test5'.
- **Output**: This function does not return any output as it is a void function.
- **Functions called**:
    - [`do_test`](#do_test)


---
### file\_test6\_should\_not\_be\_parsed<!-- {{#callable:file_test6_should_not_be_parsed}} -->
The function `file_test6_should_not_be_parsed` tests that a file named 'test6' cannot be parsed as JSON and verifies the error handling.
- **Inputs**: None
- **Control Flow**:
    - Initialize `test6` and `tree` to NULL.
    - Read the contents of the file 'inputs/test6' into `test6`.
    - Assert that `test6` is not NULL, indicating the file was read successfully.
    - Attempt to parse `test6` as JSON and store the result in `tree`.
    - Assert that `tree` is NULL, indicating the parsing failed as expected.
    - Assert that the error pointer from `cJSON_GetErrorPtr()` matches `test6`, verifying the error location.
    - Free the memory allocated for `test6` if it is not NULL.
    - Free the memory allocated for `tree` if it is not NULL.
- **Output**: The function does not return any value; it performs assertions to validate the test conditions.
- **Functions called**:
    - [`read_file`](common.h.driver.md#read_file)


---
### file\_test7\_should\_be\_parsed\_and\_printed<!-- {{#callable:file_test7_should_be_parsed_and_printed}} -->
The function `file_test7_should_be_parsed_and_printed` executes a test to parse and print the JSON content of a file named 'test7'.
- **Inputs**: None
- **Control Flow**:
    - The function calls [`do_test`](#do_test) with the argument "test7".
    - The [`do_test`](#do_test) function constructs file paths for the input file and expected output file using the test name.
    - It reads the expected output from the expected file path and parses the input file using `parse_file`.
    - The parsed JSON tree is printed back to a JSON string and compared with the expected output.
    - Assertions are used to ensure that the expected output matches the actual output and that all operations succeed.
    - Resources such as file paths, expected output, parsed tree, and actual output are freed after use.
- **Output**: The function does not return any value; it performs assertions to validate the test.
- **Functions called**:
    - [`do_test`](#do_test)


---
### file\_test8\_should\_be\_parsed\_and\_printed<!-- {{#callable:file_test8_should_be_parsed_and_printed}} -->
The function `file_test8_should_be_parsed_and_printed` executes a test to parse and print the JSON content of a file named 'test8'.
- **Inputs**: None
- **Control Flow**:
    - The function `file_test8_should_be_parsed_and_printed` calls the [`do_test`](#do_test) function with the argument "test8".
    - The [`do_test`](#do_test) function constructs file paths for the input file and expected output file using the provided test name.
    - It reads the expected output from the expected file path and parses the input file using `parse_file`.
    - The parsed JSON tree is printed back to a JSON string and compared with the expected output.
    - Assertions are used to ensure that the expected and actual outputs match, and resources are cleaned up after the test.
- **Output**: The function does not return any value; it performs assertions to validate the test and may output test results through the Unity test framework.
- **Functions called**:
    - [`do_test`](#do_test)


---
### file\_test9\_should\_be\_parsed\_and\_printed<!-- {{#callable:file_test9_should_be_parsed_and_printed}} -->
The function `file_test9_should_be_parsed_and_printed` executes a test to parse and print the JSON content of a file named 'test9'.
- **Inputs**: None
- **Control Flow**:
    - The function calls [`do_test`](#do_test) with the argument "test9".
    - The [`do_test`](#do_test) function constructs file paths for the input and expected output files using the test name.
    - It reads the expected output from the expected file path and parses the input file content into a JSON tree.
    - The parsed JSON tree is printed back to a JSON string and compared with the expected output.
    - If the actual output matches the expected output, the test passes; otherwise, it fails.
    - Resources such as file paths, expected output, parsed tree, and actual output are freed after the test.
- **Output**: The function does not return any value; it performs a test and asserts the results.
- **Functions called**:
    - [`do_test`](#do_test)


---
### file\_test10\_should\_be\_parsed\_and\_printed<!-- {{#callable:file_test10_should_be_parsed_and_printed}} -->
The function `file_test10_should_be_parsed_and_printed` executes a test to parse and print the JSON content of a file named 'test10'.
- **Inputs**: None
- **Control Flow**:
    - The function calls [`do_test`](#do_test) with the argument "test10".
    - The [`do_test`](#do_test) function constructs file paths for the test input and expected output using the provided test name.
    - It reads the expected output from the constructed path and parses the test input file into a cJSON object.
    - The parsed JSON object is printed back to a string and compared with the expected output string.
    - Assertions are used to ensure that the expected and actual outputs match, and resources are cleaned up after the test.
- **Output**: The function does not return any value; it performs assertions to validate the test and relies on the Unity test framework to report results.
- **Functions called**:
    - [`do_test`](#do_test)


---
### file\_test11\_should\_be\_parsed\_and\_printed<!-- {{#callable:file_test11_should_be_parsed_and_printed}} -->
The function `file_test11_should_be_parsed_and_printed` executes a test to parse and print the JSON content of a file named 'test11'.
- **Inputs**: None
- **Control Flow**:
    - The function calls [`do_test`](#do_test) with the argument "test11".
    - The [`do_test`](#do_test) function constructs file paths for the test input and expected output using the provided test name.
    - It reads the expected output from the constructed path and parses the test input file into a cJSON object.
    - The parsed JSON object is then printed back to a string and compared with the expected output string.
    - Assertions are used to ensure that the file paths are allocated, the expected output is read, the test input is parsed, and the printed JSON matches the expected output.
    - Resources such as file paths, expected output, parsed JSON tree, and printed JSON are freed after use.
- **Output**: The function does not return any value; it performs assertions to validate the parsing and printing of the JSON file.
- **Functions called**:
    - [`do_test`](#do_test)


---
### test12\_should\_not\_be\_parsed<!-- {{#callable:test12_should_not_be_parsed}} -->
The function `test12_should_not_be_parsed` tests the failure of parsing an incomplete JSON string and verifies the error pointer location.
- **Inputs**: None
- **Control Flow**:
    - Define a constant character pointer `test12` with an incomplete JSON string.
    - Initialize a `cJSON` pointer `tree` to `NULL`.
    - Parse the JSON string `test12` using `cJSON_Parse` and assign the result to `tree`.
    - Assert that `tree` is `NULL` to confirm parsing failure with `TEST_ASSERT_NULL_MESSAGE`.
    - Assert that the error pointer returned by `cJSON_GetErrorPtr` is at the end of `test12` using `TEST_ASSERT_EQUAL_PTR_MESSAGE`.
    - If `tree` is not `NULL`, delete it using `cJSON_Delete`.
- **Output**: The function does not return any value; it performs assertions to validate JSON parsing behavior.


---
### test13\_should\_be\_parsed\_without\_null\_termination<!-- {{#callable:test13_should_be_parsed_without_null_termination}} -->
The function `test13_should_be_parsed_without_null_termination` tests the parsing of a JSON string without a null terminator using the `cJSON_ParseWithLength` function.
- **Inputs**: None
- **Control Flow**:
    - Declare a pointer `tree` of type `cJSON` and initialize it to `NULL`.
    - Define a constant character array `test_13` containing a JSON string.
    - Create a character array `test_13_wo_null` with a size one less than `test_13` to exclude the null terminator.
    - Copy the contents of `test_13` into `test_13_wo_null` using `memcpy`, excluding the null terminator.
    - Parse `test_13_wo_null` using `cJSON_ParseWithLength`, passing the length of `test_13` minus one, and assign the result to `tree`.
    - Assert that `tree` is not `NULL` to ensure the JSON was parsed successfully.
    - If `tree` is not `NULL`, delete it using `cJSON_Delete` to free the allocated memory.
- **Output**: The function does not return any value; it performs assertions to validate JSON parsing and cleans up resources.


---
### test14\_should\_not\_be\_parsed<!-- {{#callable:test14_should_not_be_parsed}} -->
The function `test14_should_not_be_parsed` tests that a JSON string is not parsed when the buffer length is intentionally set shorter than the actual string length.
- **Inputs**: None
- **Control Flow**:
    - Initialize a `cJSON` pointer `tree` to `NULL`.
    - Define a JSON string `test_14` containing a nested JSON structure.
    - Call `cJSON_ParseWithLength` with `test_14` and a length shorter than the actual string to parse the JSON.
    - Use `TEST_ASSERT_NULL_MESSAGE` to assert that `tree` is `NULL`, indicating parsing should not continue after the buffer length is reached.
    - Check if `tree` is not `NULL` and delete it using `cJSON_Delete` if necessary.
- **Output**: The function does not return any value; it asserts that the JSON parsing fails when the buffer length is insufficient.


---
### test15\_should\_not\_heap\_buffer\_overflow<!-- {{#callable:test15_should_not_heap_buffer_overflow}} -->
The function `test15_should_not_heap_buffer_overflow` tests for heap buffer overflow by parsing JSON strings with exact allocated memory size.
- **Inputs**: None
- **Control Flow**:
    - Define an array of JSON strings `strings` with two elements.
    - Iterate over each element in the `strings` array.
    - For each string, calculate its length using `strlen`.
    - Allocate a heap buffer `exact_size_heap` with the exact size of the string length.
    - Assert that the allocation was successful using `TEST_ASSERT_NOT_NULL`.
    - Copy the JSON string into the allocated buffer using `memcpy`.
    - Parse the JSON string using `cJSON_ParseWithLength` with the exact length.
    - Delete the parsed JSON object using `cJSON_Delete`.
    - Free the allocated heap buffer.
- **Output**: The function does not return any value; it performs assertions to ensure no heap buffer overflow occurs during JSON parsing.


---
### main<!-- {{#callable:CJSON_CDECL::main}} -->
The `main` function initializes a Unity test framework, runs a series of JSON parsing tests, and returns the test results.
- **Inputs**: None
- **Control Flow**:
    - Call `UNITY_BEGIN()` to initialize the Unity test framework.
    - Sequentially call `RUN_TEST()` for each test function, which includes tests for parsing JSON files and handling various edge cases.
    - Return the result of `UNITY_END()`, which concludes the test run and returns the number of failed tests.
- **Output**: The function returns an integer representing the number of failed tests, as provided by `UNITY_END()`.


