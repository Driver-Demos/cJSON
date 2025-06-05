# Purpose
This C source code file is a test suite designed to validate the functionality of the `cJSON_ParseWithOpts` function from the cJSON library, which is a popular C library for parsing and generating JSON. The file uses the Unity Test Framework, a unit testing framework for C, to define and run a series of tests that check the robustness and correctness of the `cJSON_ParseWithOpts` function under various conditions. The tests cover scenarios such as handling null inputs, empty strings, incomplete JSON data, and JSON data with UTF-8 Byte Order Marks (BOM). Each test function is designed to assert expected outcomes and ensure that the function behaves correctly in edge cases, such as returning null pointers or specific error pointers when parsing fails.

The file includes several static test functions, each targeting a specific aspect of the `cJSON_ParseWithOpts` function's behavior. These functions utilize Unity's assertion macros to verify that the function's output matches expected results, such as returning null for invalid inputs or correctly identifying the end of a parsed JSON string. The [`main`](#CJSON_CDECLmain) function orchestrates the execution of these tests using Unity's `RUN_TEST` macro, and the test suite is initiated and concluded with `UNITY_BEGIN` and `UNITY_END` macros, respectively. This file is integral to ensuring the reliability and stability of the cJSON library by systematically testing its JSON parsing capabilities.
# Imports and Dependencies

---
- `unity/examples/unity_config.h`
- `unity/src/unity.h`
- `common.h`


# Functions

---
### parse\_with\_opts\_should\_handle\_null<!-- {{#callable:parse_with_opts_should_handle_null}} -->
The function `parse_with_opts_should_handle_null` tests the `cJSON_ParseWithOpts` function's ability to handle NULL inputs and error pointers.
- **Inputs**: None
- **Control Flow**:
    - Initialize a NULL pointer `error_pointer` and a NULL `cJSON` pointer `item`.
    - Call `cJSON_ParseWithOpts` with a NULL input string and a non-NULL error pointer, asserting that the result is NULL and outputs a specific error message if not.
    - Call `cJSON_ParseWithOpts` with an empty JSON object string and a NULL error pointer, asserting that the result is not NULL and outputs a specific error message if it is.
    - Delete the `cJSON` item to free memory.
    - Call `cJSON_ParseWithOpts` with both a NULL input string and a NULL error pointer, asserting that the result is NULL and outputs a specific error message if not.
    - Call `cJSON_ParseWithOpts` with an invalid JSON string and a NULL error pointer, asserting that the result is NULL and outputs a specific error message if not.
- **Output**: The function does not return any value; it uses assertions to validate the behavior of `cJSON_ParseWithOpts` under various NULL input conditions.


---
### parse\_with\_opts\_should\_handle\_empty\_strings<!-- {{#callable:parse_with_opts_should_handle_empty_strings}} -->
The function `parse_with_opts_should_handle_empty_strings` tests the behavior of `cJSON_ParseWithOpts` when parsing empty strings, ensuring it returns NULL and sets the error pointer correctly.
- **Inputs**: None
- **Control Flow**:
    - Define an empty string `empty_string` and a NULL pointer `error_pointer`.
    - Call `cJSON_ParseWithOpts` with `empty_string` and NULL for the error pointer, expecting a NULL return value.
    - Assert that the error pointer returned by `cJSON_GetErrorPtr` matches `empty_string`.
    - Call `cJSON_ParseWithOpts` again with `empty_string` and `error_pointer`, expecting a NULL return value.
    - Assert that `error_pointer` and the error pointer from `cJSON_GetErrorPtr` both match `empty_string`.
- **Output**: The function does not return a value; it uses assertions to verify that `cJSON_ParseWithOpts` handles empty strings correctly by returning NULL and setting the error pointer to the empty string.


---
### parse\_with\_opts\_should\_handle\_incomplete\_json<!-- {{#callable:parse_with_opts_should_handle_incomplete_json}} -->
The function `parse_with_opts_should_handle_incomplete_json` tests the behavior of `cJSON_ParseWithOpts` when given an incomplete JSON string.
- **Inputs**: None
- **Control Flow**:
    - Define a constant character array `json` containing an incomplete JSON string "{ \"name\": ".
    - Declare a pointer `parse_end` initialized to `NULL`.
    - Call `cJSON_ParseWithOpts` with `json`, `&parse_end`, and `false`, and assert that it returns `NULL`, indicating a parse failure.
    - Assert that `parse_end` points to the end of the `json` string, which is `json + strlen(json)`.
    - Assert that `cJSON_GetErrorPtr()` also points to the end of the `json` string, indicating the location of the parse error.
- **Output**: The function does not return a value; it uses assertions to verify the expected behavior of the `cJSON_ParseWithOpts` function when handling incomplete JSON.


---
### parse\_with\_opts\_should\_require\_null\_if\_requested<!-- {{#callable:parse_with_opts_should_require_null_if_requested}} -->
The function `parse_with_opts_should_require_null_if_requested` tests the behavior of `cJSON_ParseWithOpts` when the `require_null_terminated` option is set to true.
- **Inputs**: None
- **Control Flow**:
    - Call `cJSON_ParseWithOpts` with a JSON string "{}" and `require_null_terminated` set to true, then assert that the result is not NULL.
    - Delete the parsed JSON item to free memory.
    - Call `cJSON_ParseWithOpts` with a JSON string "{} \n" and `require_null_terminated` set to true, then assert that the result is not NULL.
    - Delete the parsed JSON item to free memory.
    - Call `cJSON_ParseWithOpts` with a JSON string "{}x" and `require_null_terminated` set to true, then assert that the result is NULL.
- **Output**: The function does not return a value; it uses assertions to validate the behavior of `cJSON_ParseWithOpts`.


---
### parse\_with\_opts\_should\_return\_parse\_end<!-- {{#callable:parse_with_opts_should_return_parse_end}} -->
The function `parse_with_opts_should_return_parse_end` tests if the `cJSON_ParseWithOpts` function correctly identifies the end of a JSON array in a string.
- **Inputs**: None
- **Control Flow**:
    - Define a JSON string `json` containing an empty array followed by text.
    - Initialize a pointer `parse_end` to NULL.
    - Call `cJSON_ParseWithOpts` with `json`, `parse_end`, and `false` to parse the JSON.
    - Assert that the returned `cJSON` item is not NULL, indicating successful parsing.
    - Assert that `parse_end` points to the character after the JSON array, verifying correct parsing end position.
    - Delete the parsed `cJSON` item to free memory.
- **Output**: The function does not return a value; it uses assertions to validate the behavior of `cJSON_ParseWithOpts`.


---
### parse\_with\_opts\_should\_parse\_utf8\_bom<!-- {{#callable:parse_with_opts_should_parse_utf8_bom}} -->
The function `parse_with_opts_should_parse_utf8_bom` tests the ability of the `cJSON_ParseWithOpts` function to correctly parse JSON strings with and without a UTF-8 Byte Order Mark (BOM).
- **Inputs**: None
- **Control Flow**:
    - Initialize two `cJSON` pointers, `with_bom` and `without_bom`, to `NULL`.
    - Parse a JSON string with a UTF-8 BOM using `cJSON_ParseWithOpts` and assign the result to `with_bom`.
    - Assert that `with_bom` is not `NULL` to ensure parsing was successful.
    - Parse a JSON string without a BOM using `cJSON_ParseWithOpts` and assign the result to `without_bom`.
    - Assert that `without_bom` is not `NULL` to ensure parsing was successful.
    - Compare the two parsed JSON objects using `cJSON_Compare` to ensure they are equivalent, and assert that the comparison is true.
    - Delete the `cJSON` objects `with_bom` and `without_bom` to free memory.
- **Output**: The function does not return any value; it uses assertions to validate the parsing and comparison of JSON objects.


---
### main<!-- {{#callable:CJSON_CDECL::main}} -->
The `main` function initializes and runs a series of unit tests for JSON parsing using the Unity test framework.
- **Inputs**: None
- **Control Flow**:
    - Call `UNITY_BEGIN()` to initialize the Unity test framework.
    - Execute a series of test functions using `RUN_TEST()`, each of which tests different aspects of JSON parsing.
    - Call `UNITY_END()` to finalize the test run and return the result.
- **Output**: The function returns an integer status code from `UNITY_END()`, indicating the success or failure of the test suite.


