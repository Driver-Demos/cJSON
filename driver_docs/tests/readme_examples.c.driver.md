# Purpose
This C source code file is a test suite for validating the functionality of JSON creation and parsing using the cJSON library. It is structured to test the creation of a JSON object representing a monitor with various resolutions and to verify if a monitor supports Full HD resolution. The file includes functions to create JSON objects both manually and with helper functions, and it uses the Unity testing framework to assert the correctness of these operations. The [`create_monitor`](#create_monitor) and [`create_monitor_with_helpers`](#create_monitor_with_helpers) functions generate JSON strings for a monitor with predefined resolutions, while the [`supports_full_hd`](#supports_full_hd) function checks if a given JSON string includes a Full HD resolution (1920x1080).

The file is designed to be executed as a standalone program, as indicated by the presence of a [`main`](#CJSON_CDECLmain) function that initializes the Unity test framework and runs a series of tests. These tests include verifying the correct creation of the JSON object and checking the Full HD support functionality. The code is organized to facilitate testing and debugging of JSON-related operations, leveraging the cJSON library for JSON manipulation and the Unity framework for test assertions. This file does not define public APIs or external interfaces but rather serves as an internal testing mechanism for ensuring the reliability of JSON handling functions.
# Imports and Dependencies

---
- `stdio.h`
- `stdlib.h`
- `string.h`
- `unity/examples/unity_config.h`
- `unity/src/unity.h`
- `common.h`


# Global Variables

---
### json
- **Type**: `const char*`
- **Description**: The `json` variable is a static constant character pointer that holds a JSON formatted string. This string represents a monitor with the name "Awesome 4K" and a list of resolutions, including 1280x720, 1920x1080, and 3840x2160.
- **Use**: This variable is used as a reference JSON string to validate the output of functions that create or manipulate JSON objects representing monitor specifications.


# Functions

---
### create\_monitor<!-- {{#callable:create_monitor}} -->
The `create_monitor` function constructs a JSON representation of a monitor with a name and a list of supported resolutions, and returns it as a string.
- **Inputs**: None
- **Control Flow**:
    - Initialize a 2D array `resolution_numbers` with predefined resolution values.
    - Declare and initialize several `cJSON` pointers and a `string` pointer to `NULL`.
    - Create a `cJSON` object `monitor` and check for successful creation; if unsuccessful, jump to cleanup.
    - Create a `cJSON` string `name` with the value "Awesome 4K" and add it to the `monitor` object.
    - Create a `cJSON` array `resolutions` and add it to the `monitor` object.
    - Iterate over the `resolution_numbers` array to create `cJSON` objects for each resolution, adding `width` and `height` as properties, and append them to the `resolutions` array.
    - Convert the `monitor` object to a JSON string using `cJSON_Print` and store it in `string`.
    - Perform cleanup by deleting the `monitor` object and return the JSON string.
- **Output**: A JSON string representing a monitor with a name and a list of resolutions, or `NULL` if an error occurs during creation.


---
### create\_monitor\_with\_helpers<!-- {{#callable:create_monitor_with_helpers}} -->
The function `create_monitor_with_helpers` creates a JSON representation of a monitor with predefined resolutions using cJSON helper functions.
- **Inputs**: None
- **Control Flow**:
    - Initialize a 2D array `resolution_numbers` with predefined width and height pairs for different resolutions.
    - Declare and initialize variables for JSON string and cJSON objects.
    - Create a cJSON object `monitor` to represent the monitor.
    - Add a string property 'name' with value 'Awesome 4K' to the `monitor` object.
    - Add an array property 'resolutions' to the `monitor` object.
    - Iterate over the `resolution_numbers` array to create and populate resolution objects with 'width' and 'height' properties, adding each to the 'resolutions' array.
    - Convert the `monitor` cJSON object to a JSON string using `cJSON_Print`.
    - Handle errors by checking for NULL returns from cJSON functions and using a `goto end` for cleanup.
    - Delete the `monitor` cJSON object to free memory before returning the JSON string.
- **Output**: A JSON string representing a monitor with a name and an array of resolution objects.


---
### supports\_full\_hd<!-- {{#callable:supports_full_hd}} -->
The function `supports_full_hd` checks if a given monitor JSON string supports a resolution of 1920x1080 (Full HD).
- **Inputs**:
    - `monitor`: A JSON string representing a monitor, which includes its name and a list of supported resolutions.
- **Control Flow**:
    - Parse the input JSON string into a cJSON object `monitor_json`.
    - If parsing fails, print an error message and set `status` to 0, then exit the function.
    - Retrieve the 'name' field from the JSON and print it if it is a valid string.
    - Retrieve the 'resolutions' array from the JSON.
    - Iterate over each resolution in the 'resolutions' array.
    - For each resolution, retrieve the 'width' and 'height' fields.
    - If either 'width' or 'height' is not a number, set `status` to 0 and exit the function.
    - Check if the resolution is 1920x1080 using `compare_double` for both width and height.
    - If a 1920x1080 resolution is found, set `status` to 1 and exit the function.
    - Delete the `monitor_json` object to free memory.
- **Output**: Returns 1 if the monitor supports a resolution of 1920x1080, otherwise returns 0.


---
### cJSON\_ArrayForEach<!-- {{#callable:supports_full_hd::cJSON_ArrayForEach}} -->
The `cJSON_ArrayForEach` function iterates over each element in a JSON array and checks if any resolution matches 1920x1080, setting a status flag accordingly.
- **Inputs**:
    - `resolution`: A variable representing each element in the 'resolutions' JSON array during iteration.
    - `resolutions`: A JSON array containing resolution objects, each with 'width' and 'height' properties.
- **Control Flow**:
    - Iterate over each 'resolution' object in the 'resolutions' JSON array using the `cJSON_ArrayForEach` macro.
    - For each 'resolution', retrieve the 'width' and 'height' properties using `cJSON_GetObjectItemCaseSensitive`.
    - Check if both 'width' and 'height' are numbers using `cJSON_IsNumber`. If not, set `status` to 0 and exit the loop.
    - If both are numbers, compare 'width' to 1920 and 'height' to 1080 using `compare_double`.
    - If both comparisons are true, set `status` to 1 and exit the loop.
- **Output**: The function sets a status flag to 1 if a resolution of 1920x1080 is found, otherwise it remains 0.


---
### create\_monitor\_should\_create\_a\_monitor<!-- {{#callable:create_monitor_should_create_a_monitor}} -->
The function `create_monitor_should_create_a_monitor` tests if the [`create_monitor`](#create_monitor) function correctly creates a JSON string representation of a monitor and matches it with a predefined JSON string.
- **Inputs**: None
- **Control Flow**:
    - Call the [`create_monitor`](#create_monitor) function to generate a JSON string representation of a monitor.
    - Use `TEST_ASSERT_EQUAL_STRING` to compare the generated JSON string with a predefined JSON string `json`.
    - Free the allocated memory for the `monitor` string.
- **Output**: This function does not return any value; it performs a unit test to validate the output of [`create_monitor`](#create_monitor).
- **Functions called**:
    - [`create_monitor`](#create_monitor)


---
### create\_monitor\_with\_helpers\_should\_create\_a\_monitor<!-- {{#callable:create_monitor_with_helpers_should_create_a_monitor}} -->
The function `create_monitor_with_helpers_should_create_a_monitor` tests if the [`create_monitor_with_helpers`](#create_monitor_with_helpers) function correctly creates a JSON string representing a monitor with specific resolutions.
- **Inputs**: None
- **Control Flow**:
    - Call the function [`create_monitor_with_helpers`](#create_monitor_with_helpers) to create a JSON string representing a monitor.
    - Use `TEST_ASSERT_EQUAL_STRING` to compare the generated JSON string with a predefined JSON string `json` to ensure they are identical.
    - Free the allocated memory for the monitor string.
- **Output**: This function does not return any value; it performs a unit test to validate the output of [`create_monitor_with_helpers`](#create_monitor_with_helpers).
- **Functions called**:
    - [`create_monitor_with_helpers`](#create_monitor_with_helpers)


---
### supports\_full\_hd\_should\_check\_for\_full\_hd\_support<!-- {{#callable:supports_full_hd_should_check_for_full_hd_support}} -->
The function `supports_full_hd_should_check_for_full_hd_support` tests whether the [`supports_full_hd`](#supports_full_hd) function correctly identifies monitors that support full HD resolution.
- **Inputs**: None
- **Control Flow**:
    - Defines a static JSON string `monitor_without_hd` representing a monitor with a resolution of 640x480.
    - Calls `TEST_ASSERT` to verify that `supports_full_hd(json)` returns true, indicating the monitor supports full HD.
    - Calls `TEST_ASSERT_FALSE` to verify that `supports_full_hd(monitor_without_hd)` returns false, indicating the monitor does not support full HD.
- **Output**: The function does not return any value; it performs assertions to validate the behavior of the [`supports_full_hd`](#supports_full_hd) function.
- **Functions called**:
    - [`supports_full_hd`](#supports_full_hd)


---
### main<!-- {{#callable:CJSON_CDECL::main}} -->
The `main` function initializes and runs a series of unit tests using the Unity test framework.
- **Inputs**: None
- **Control Flow**:
    - The function begins by calling `UNITY_BEGIN()` to initialize the Unity test framework.
    - It then runs three test functions using `RUN_TEST()`: `create_monitor_should_create_a_monitor`, `create_monitor_with_helpers_should_create_a_monitor`, and `supports_full_hd_should_check_for_full_hd_support`.
    - Finally, it calls `UNITY_END()` to conclude the test run and returns the result of `UNITY_END()`.
- **Output**: The function returns an integer status code from `UNITY_END()`, indicating the result of the test execution.


