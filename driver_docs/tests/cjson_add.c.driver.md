# Purpose
This C source code file is a test suite for the cJSON library, which is a lightweight JSON parser and generator in C. The file uses the Unity test framework to define and run a series of unit tests that verify the functionality of various cJSON functions. These tests cover the creation and manipulation of JSON objects, including adding null, boolean, number, string, raw, object, and array types to JSON objects. The tests also check for proper handling of null pointers and allocation failures, ensuring that the cJSON library behaves correctly under these conditions.

The file includes several static functions, each corresponding to a specific test case. These functions create JSON objects, perform operations using cJSON functions, and use Unity's assertion macros to verify the expected outcomes. The test cases are organized to test both successful operations and failure scenarios, such as memory allocation failures simulated by custom memory hooks. The [`main`](#CJSON_CDECLmain) function initializes the Unity test framework, runs all the defined test cases, and returns the test results. This file is intended to be compiled and executed as a standalone test program to validate the robustness and correctness of the cJSON library's functionality.
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
### failing\_hooks
- **Type**: `cJSON_Hooks`
- **Description**: The `failing_hooks` variable is a static instance of the `cJSON_Hooks` structure, which is used to override the default memory allocation functions in the cJSON library. It is initialized with `failing_malloc`, a function that always returns NULL, and `normal_free`, a standard free function.
- **Use**: This variable is used to simulate memory allocation failures in unit tests by setting it as the hooks for cJSON operations.


# Functions

---
### failing\_malloc<!-- {{#callable:CJSON_CDECL::failing_malloc}} -->
The `failing_malloc` function is a mock memory allocation function that always returns `NULL`, simulating a failure in memory allocation.
- **Inputs**:
    - `size`: The size of memory to allocate, which is ignored in this function.
- **Control Flow**:
    - The function takes a single argument `size` of type `size_t`, which represents the size of memory to allocate.
    - The argument `size` is explicitly cast to void to indicate it is unused, preventing compiler warnings about unused parameters.
    - The function immediately returns `NULL`, indicating a failure to allocate memory.
- **Output**: The function returns `NULL`, simulating a memory allocation failure.


---
### normal\_free<!-- {{#callable:CJSON_CDECL::normal_free}} -->
The `normal_free` function deallocates memory previously allocated by freeing the pointer passed to it.
- **Inputs**:
    - `pointer`: A void pointer to the memory block that needs to be deallocated.
- **Control Flow**:
    - The function takes a single argument, a void pointer named `pointer`.
    - It calls the standard library function `free()` with the `pointer` as its argument to deallocate the memory block pointed to by `pointer`.
- **Output**: The function does not return any value.


---
### cjson\_add\_null\_should\_add\_null<!-- {{#callable:cjson_add_null_should_add_null}} -->
The function `cjson_add_null_should_add_null` tests the addition of a null value to a cJSON object and verifies its successful addition.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object `root` using `cJSON_CreateObject()`.
    - Declare a pointer `null` and initialize it to `NULL`.
    - Add a null value to the `root` object with the key "null" using `cJSON_AddNullToObject()`.
    - Retrieve the item with the key "null" from `root` using `cJSON_GetObjectItemCaseSensitive()` and assign it to `null`.
    - Assert that `null` is not `NULL` using `TEST_ASSERT_NOT_NULL()`.
    - Assert that the type of `null` is `cJSON_NULL` using `TEST_ASSERT_EQUAL_INT()`.
    - Delete the `root` object using `cJSON_Delete()`.
- **Output**: The function does not return any value; it performs assertions to verify the correct behavior of adding a null value to a cJSON object.


---
### cjson\_add\_null\_should\_fail\_with\_null\_pointers<!-- {{#callable:cjson_add_null_should_fail_with_null_pointers}} -->
The function `cjson_add_null_should_fail_with_null_pointers` tests that adding a null value to a JSON object fails when either the object or the key is null.
- **Inputs**: None
- **Control Flow**:
    - Create a new JSON object `root` using `cJSON_CreateObject`.
    - Assert that `cJSON_AddNullToObject` returns null when the object is null and the key is "null".
    - Assert that `cJSON_AddNullToObject` returns null when the object is `root` and the key is null.
    - Delete the JSON object `root` using `cJSON_Delete`.
- **Output**: The function does not return any value; it performs assertions to validate behavior.


---
### cjson\_add\_null\_should\_fail\_on\_allocation\_failure<!-- {{#callable:cjson_add_null_should_fail_on_allocation_failure}} -->
The function `cjson_add_null_should_fail_on_allocation_failure` tests that adding a null object to a cJSON object fails when memory allocation is deliberately set to fail.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object `root` using `cJSON_CreateObject`.
    - Initialize cJSON hooks to use `failing_hooks`, which simulates memory allocation failure.
    - Attempt to add a null object to `root` with the key "null" using `cJSON_AddNullToObject`, and assert that the result is NULL, indicating failure.
    - Reset cJSON hooks to default by passing NULL to `cJSON_InitHooks`.
    - Delete the cJSON object `root` to free memory.
- **Output**: The function does not return any value; it uses assertions to verify that the operation fails as expected.


---
### cjson\_add\_true\_should\_add\_true<!-- {{#callable:cjson_add_true_should_add_true}} -->
The function `cjson_add_true_should_add_true` tests if a boolean true value can be successfully added to a cJSON object and verifies its type.
- **Inputs**: None
- **Control Flow**:
    - Create a new cJSON object named `root`.
    - Declare a pointer `true_item` and initialize it to NULL.
    - Add a boolean true value to the `root` object with the key "true" using `cJSON_AddTrueToObject`.
    - Retrieve the item with the key "true" from the `root` object using `cJSON_GetObjectItemCaseSensitive` and assign it to `true_item`.
    - Assert that `true_item` is not NULL, indicating the item was successfully retrieved.
    - Assert that the type of `true_item` is `cJSON_True`, confirming it is a boolean true value.
    - Delete the `root` object to free memory.
- **Output**: The function does not return any value; it performs assertions to verify the correct behavior of adding a true value to a cJSON object.


---
### cjson\_add\_true\_should\_fail\_with\_null\_pointers<!-- {{#callable:cjson_add_true_should_fail_with_null_pointers}} -->
The function `cjson_add_true_should_fail_with_null_pointers` tests that adding a true value to a JSON object fails when either the object or the key is NULL.
- **Inputs**: None
- **Control Flow**:
    - Create a new cJSON object `root` using `cJSON_CreateObject()`.
    - Assert that `cJSON_AddTrueToObject(NULL, "true")` returns NULL, indicating failure when the object is NULL.
    - Assert that `cJSON_AddTrueToObject(root, NULL)` returns NULL, indicating failure when the key is NULL.
    - Delete the cJSON object `root` using `cJSON_Delete(root)`.
- **Output**: The function does not return any value; it performs assertions to validate expected behavior.


---
### cjson\_add\_true\_should\_fail\_on\_allocation\_failure<!-- {{#callable:cjson_add_true_should_fail_on_allocation_failure}} -->
The function `cjson_add_true_should_fail_on_allocation_failure` tests that adding a true value to a cJSON object fails when memory allocation is set to always fail.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object named `root` using `cJSON_CreateObject`.
    - Initialize cJSON hooks to use `failing_hooks`, which simulates memory allocation failure.
    - Attempt to add a true value to the `root` object with the key "true" using `cJSON_AddTrueToObject`, and assert that the result is NULL, indicating failure.
    - Reset cJSON hooks to default by passing NULL to `cJSON_InitHooks`.
    - Delete the `root` object using `cJSON_Delete`.
- **Output**: The function does not return any value; it uses assertions to verify behavior.


---
### cjson\_create\_int\_array\_should\_fail\_on\_allocation\_failure<!-- {{#callable:cjson_create_int_array_should_fail_on_allocation_failure}} -->
The function `cjson_create_int_array_should_fail_on_allocation_failure` tests that the `cJSON_CreateIntArray` function returns NULL when memory allocation fails.
- **Inputs**: None
- **Control Flow**:
    - An integer array `numbers` with values {1, 2, 3} is defined.
    - The `cJSON_InitHooks` function is called with `failing_hooks` to simulate memory allocation failure.
    - The `cJSON_CreateIntArray` function is called with `numbers` and its size (3), and it is asserted that the return value is NULL using `TEST_ASSERT_NULL`.
    - The `cJSON_InitHooks` function is called with NULL to reset the hooks to default.
- **Output**: The function does not return any value; it uses assertions to validate behavior.


---
### cjson\_create\_float\_array\_should\_fail\_on\_allocation\_failure<!-- {{#callable:cjson_create_float_array_should_fail_on_allocation_failure}} -->
The function `cjson_create_float_array_should_fail_on_allocation_failure` tests that the creation of a float array using `cJSON_CreateFloatArray` fails when memory allocation is deliberately set to fail.
- **Inputs**: None
- **Control Flow**:
    - Define a float array `numbers` with values {1.0f, 2.0f, 3.0f}.
    - Initialize cJSON hooks to use `failing_hooks`, which simulates memory allocation failure.
    - Assert that `cJSON_CreateFloatArray(numbers, 3)` returns NULL, indicating failure due to allocation issues.
    - Reset cJSON hooks to default by passing NULL.
- **Output**: The function does not return any value; it uses assertions to verify behavior.


---
### cjson\_create\_double\_array\_should\_fail\_on\_allocation\_failure<!-- {{#callable:cjson_create_double_array_should_fail_on_allocation_failure}} -->
The function `cjson_create_double_array_should_fail_on_allocation_failure` tests that the creation of a double array using `cJSON_CreateDoubleArray` fails when memory allocation is set to always fail.
- **Inputs**: None
- **Control Flow**:
    - Initialize a double array `numbers` with values {1.0, 2.0, 3.0}.
    - Set the cJSON hooks to `failing_hooks` which simulates memory allocation failure.
    - Assert that `cJSON_CreateDoubleArray` returns NULL when trying to create a double array from `numbers`, indicating a failure due to allocation issues.
    - Reset the cJSON hooks to default by passing NULL.
- **Output**: The function does not return any value; it uses assertions to verify behavior.


---
### cjson\_create\_string\_array\_should\_fail\_on\_allocation\_failure<!-- {{#callable:cjson_create_string_array_should_fail_on_allocation_failure}} -->
The function `cjson_create_string_array_should_fail_on_allocation_failure` tests that the `cJSON_CreateStringArray` function returns NULL when memory allocation fails.
- **Inputs**: None
- **Control Flow**:
    - An array of strings `{"1", "2", "3"}` is defined.
    - The `cJSON_InitHooks` function is called with `failing_hooks` to simulate memory allocation failure.
    - The `cJSON_CreateStringArray` function is called with the string array and its size (3), and it is asserted that the return value is NULL using `TEST_ASSERT_NULL`.
    - The `cJSON_InitHooks` function is called with `NULL` to reset the hooks to default.
- **Output**: The function does not return any value; it uses assertions to validate behavior.


---
### cjson\_add\_false\_should\_add\_false<!-- {{#callable:cjson_add_false_should_add_false}} -->
The function `cjson_add_false_should_add_false` tests the addition of a boolean false value to a cJSON object and verifies its correctness.
- **Inputs**: None
- **Control Flow**:
    - Create a new cJSON object named `root`.
    - Declare a pointer `false_item` and initialize it to NULL.
    - Add a boolean false value to the `root` object with the key "false" using `cJSON_AddFalseToObject`.
    - Retrieve the item with the key "false" from the `root` object using `cJSON_GetObjectItemCaseSensitive` and assign it to `false_item`.
    - Assert that `false_item` is not NULL, indicating the item was successfully retrieved.
    - Assert that the type of `false_item` is `cJSON_False`, confirming the correct type was added.
    - Delete the `root` object to free memory.
- **Output**: The function does not return any value; it performs assertions to verify the correct behavior of adding a false value to a cJSON object.


---
### cjson\_add\_false\_should\_fail\_with\_null\_pointers<!-- {{#callable:cjson_add_false_should_fail_with_null_pointers}} -->
The function `cjson_add_false_should_fail_with_null_pointers` tests that adding a false value to a JSON object fails when either the object or the key is NULL.
- **Inputs**: None
- **Control Flow**:
    - Create a new cJSON object named `root` using `cJSON_CreateObject()`.
    - Assert that `cJSON_AddFalseToObject(NULL, "false")` returns NULL, indicating failure when the object is NULL.
    - Assert that `cJSON_AddFalseToObject(root, NULL)` returns NULL, indicating failure when the key is NULL.
    - Delete the `root` object using `cJSON_Delete(root)` to free memory.
- **Output**: The function does not return any value; it uses assertions to validate expected failures.


---
### cjson\_add\_false\_should\_fail\_on\_allocation\_failure<!-- {{#callable:cjson_add_false_should_fail_on_allocation_failure}} -->
The function `cjson_add_false_should_fail_on_allocation_failure` tests that adding a false value to a cJSON object fails when memory allocation fails.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object named `root` using `cJSON_CreateObject`.
    - Initialize cJSON hooks to use `failing_hooks`, which simulates memory allocation failure.
    - Attempt to add a false value to the `root` object with the key "false" using `cJSON_AddFalseToObject`, and assert that the result is NULL, indicating failure.
    - Reset cJSON hooks to default by passing NULL to `cJSON_InitHooks`.
    - Delete the `root` object using `cJSON_Delete`.
- **Output**: The function does not return any value; it uses assertions to verify behavior.


---
### cjson\_add\_bool\_should\_add\_bool<!-- {{#callable:cjson_add_bool_should_add_bool}} -->
The function `cjson_add_bool_should_add_bool` tests the addition of boolean values to a cJSON object and verifies their correctness.
- **Inputs**: None
- **Control Flow**:
    - Create a new cJSON object `root`.
    - Add a boolean value `true` to the `root` object with the key "true" using `cJSON_AddBoolToObject`.
    - Retrieve the item with the key "true" from `root` and assert it is not null and its type is `cJSON_True`.
    - Add a boolean value `false` to the `root` object with the key "false" using `cJSON_AddBoolToObject`.
    - Retrieve the item with the key "false" from `root` and assert it is not null and its type is `cJSON_False`.
    - Delete the `root` object to free memory.
- **Output**: The function does not return any value; it performs assertions to verify the correct addition of boolean values to a cJSON object.


---
### cjson\_add\_bool\_should\_fail\_with\_null\_pointers<!-- {{#callable:cjson_add_bool_should_fail_with_null_pointers}} -->
The function `cjson_add_bool_should_fail_with_null_pointers` tests that adding a boolean to a cJSON object fails when either the object or the key is NULL.
- **Inputs**: None
- **Control Flow**:
    - Create a new cJSON object named `root` using `cJSON_CreateObject()`.
    - Call `cJSON_AddBoolToObject` with a NULL object and a valid key, expecting it to return NULL.
    - Call `cJSON_AddBoolToObject` with a valid object and a NULL key, expecting it to return NULL.
    - Delete the `root` object using `cJSON_Delete()`.
- **Output**: The function does not return any value; it uses assertions to verify that the operations fail as expected.


---
### cjson\_add\_bool\_should\_fail\_on\_allocation\_failure<!-- {{#callable:cjson_add_bool_should_fail_on_allocation_failure}} -->
The function `cjson_add_bool_should_fail_on_allocation_failure` tests that adding a boolean to a cJSON object fails when memory allocation is set to always fail.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object `root` using `cJSON_CreateObject`.
    - Initialize cJSON hooks to use `failing_hooks`, which simulates memory allocation failure.
    - Attempt to add a boolean value `false` to the `root` object with the key "false" using `cJSON_AddBoolToObject`, expecting it to return NULL due to allocation failure.
    - Reset cJSON hooks to default by passing `NULL` to `cJSON_InitHooks`.
    - Delete the `root` object using `cJSON_Delete`.
- **Output**: The function does not return any value; it uses assertions to verify that the operation fails as expected.


---
### cjson\_add\_number\_should\_add\_number<!-- {{#callable:cjson_add_number_should_add_number}} -->
The function `cjson_add_number_should_add_number` tests the addition of a number to a cJSON object and verifies its correctness.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object `root` using `cJSON_CreateObject`.
    - Declare a pointer `number` and initialize it to NULL.
    - Add a number with the key "number" and value 42 to the `root` object using `cJSON_AddNumberToObject`.
    - Retrieve the item with the key "number" from `root` using `cJSON_GetObjectItemCaseSensitive` and assign it to `number`.
    - Assert that `number` is not NULL using `TEST_ASSERT_NOT_NULL`.
    - Assert that the type of `number` is `cJSON_Number` using `TEST_ASSERT_EQUAL_INT`.
    - Assert that the double value of `number` is 42 using `TEST_ASSERT_EQUAL_DOUBLE`.
    - Assert that the integer value of `number` is 42 using `TEST_ASSERT_EQUAL_INT`.
    - Delete the `root` object using `cJSON_Delete`.
- **Output**: The function does not return any value; it performs assertions to verify the correct addition of a number to a cJSON object.


---
### cjson\_add\_number\_should\_fail\_with\_null\_pointers<!-- {{#callable:cjson_add_number_should_fail_with_null_pointers}} -->
The function `cjson_add_number_should_fail_with_null_pointers` tests that adding a number to a cJSON object fails when either the object or the key is NULL.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object `root` using `cJSON_CreateObject()`.
    - Call `cJSON_AddNumberToObject` with a NULL object and a valid key, expecting it to return NULL.
    - Call `cJSON_AddNumberToObject` with a valid object and a NULL key, expecting it to return NULL.
    - Delete the cJSON object `root` using `cJSON_Delete()`.
- **Output**: The function does not return any value; it uses assertions to verify that the function calls return NULL as expected.


---
### cjson\_add\_number\_should\_fail\_on\_allocation\_failure<!-- {{#callable:cjson_add_number_should_fail_on_allocation_failure}} -->
The function `cjson_add_number_should_fail_on_allocation_failure` tests that adding a number to a cJSON object fails when memory allocation is deliberately set to fail.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object named `root` using `cJSON_CreateObject`.
    - Initialize cJSON hooks to use `failing_hooks`, which simulates memory allocation failure.
    - Attempt to add a number to the `root` object with `cJSON_AddNumberToObject`, expecting it to return NULL due to allocation failure.
    - Revert cJSON hooks to default by passing NULL to `cJSON_InitHooks`.
    - Delete the `root` object using `cJSON_Delete`.
- **Output**: The function does not return any value; it uses assertions to verify that the number addition fails as expected.


---
### cjson\_add\_string\_should\_add\_string<!-- {{#callable:cjson_add_string_should_add_string}} -->
The function `cjson_add_string_should_add_string` tests the addition of a string to a cJSON object and verifies its correctness.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object `root` using `cJSON_CreateObject`.
    - Declare a pointer `string` and initialize it to NULL.
    - Add a string with key "string" and value "Hello World!" to the `root` object using `cJSON_AddStringToObject`.
    - Retrieve the string item from `root` using `cJSON_GetObjectItemCaseSensitive` and assign it to `string`.
    - Assert that `string` is not NULL using `TEST_ASSERT_NOT_NULL`.
    - Assert that the type of `string` is `cJSON_String` using `TEST_ASSERT_EQUAL_INT`.
    - Assert that the value of `string` is "Hello World!" using `TEST_ASSERT_EQUAL_STRING`.
    - Delete the `root` object using `cJSON_Delete`.
- **Output**: The function does not return any value; it performs assertions to verify the correct addition of a string to a cJSON object.


---
### cjson\_add\_string\_should\_fail\_with\_null\_pointers<!-- {{#callable:cjson_add_string_should_fail_with_null_pointers}} -->
The function `cjson_add_string_should_fail_with_null_pointers` tests that adding a string to a cJSON object fails when either the object or the key is NULL.
- **Inputs**: None
- **Control Flow**:
    - Create a new cJSON object `root` using `cJSON_CreateObject()`.
    - Assert that `cJSON_AddStringToObject(NULL, "string", "string")` returns NULL, indicating failure when the object is NULL.
    - Assert that `cJSON_AddStringToObject(root, NULL, "string")` returns NULL, indicating failure when the key is NULL.
    - Delete the cJSON object `root` using `cJSON_Delete(root)`.
- **Output**: The function does not return any value; it uses assertions to verify expected behavior.


---
### cjson\_add\_string\_should\_fail\_on\_allocation\_failure<!-- {{#callable:cjson_add_string_should_fail_on_allocation_failure}} -->
The function `cjson_add_string_should_fail_on_allocation_failure` tests that adding a string to a cJSON object fails when memory allocation is deliberately set to fail.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object `root` using `cJSON_CreateObject`.
    - Initialize cJSON hooks to use `failing_hooks`, which simulates memory allocation failure.
    - Attempt to add a string to the `root` object using `cJSON_AddStringToObject` and assert that the result is `NULL`, indicating failure due to allocation issues.
    - Reset cJSON hooks to default by passing `NULL` to `cJSON_InitHooks`.
    - Delete the `root` object using `cJSON_Delete`.
- **Output**: The function does not return any value; it uses assertions to verify behavior.


---
### cjson\_add\_raw\_should\_add\_raw<!-- {{#callable:cjson_add_raw_should_add_raw}} -->
The function `cjson_add_raw_should_add_raw` tests the addition of a raw JSON string to a cJSON object and verifies its correctness.
- **Inputs**: None
- **Control Flow**:
    - Create a new cJSON object `root` using `cJSON_CreateObject()`.
    - Declare a pointer `raw` and initialize it to NULL.
    - Add a raw JSON string "{}" to the `root` object with the key "raw" using `cJSON_AddRawToObject()`.
    - Retrieve the item with the key "raw" from the `root` object using `cJSON_GetObjectItemCaseSensitive()` and assign it to `raw`.
    - Assert that `raw` is not NULL using `TEST_ASSERT_NOT_NULL()`.
    - Assert that the type of `raw` is `cJSON_Raw` using `TEST_ASSERT_EQUAL_INT()`.
    - Assert that the value string of `raw` is "{}" using `TEST_ASSERT_EQUAL_STRING()`.
    - Delete the `root` object using `cJSON_Delete()`.
- **Output**: The function does not return any value; it performs assertions to verify the correct addition of a raw JSON string to a cJSON object.


---
### cjson\_add\_raw\_should\_fail\_with\_null\_pointers<!-- {{#callable:cjson_add_raw_should_fail_with_null_pointers}} -->
The function `cjson_add_raw_should_fail_with_null_pointers` tests that adding a raw JSON string to a cJSON object fails when either the object or the key is NULL.
- **Inputs**: None
- **Control Flow**:
    - Create a new cJSON object `root` using `cJSON_CreateObject()`.
    - Call `cJSON_AddRawToObject` with a NULL object and a valid key, expecting it to return NULL.
    - Call `cJSON_AddRawToObject` with a valid object and a NULL key, expecting it to return NULL.
    - Delete the cJSON object `root` using `cJSON_Delete()`.
- **Output**: The function does not return any value; it uses assertions to verify that the operations fail as expected.


---
### cjson\_add\_raw\_should\_fail\_on\_allocation\_failure<!-- {{#callable:cjson_add_raw_should_fail_on_allocation_failure}} -->
The function `cjson_add_raw_should_fail_on_allocation_failure` tests that adding a raw JSON string to a cJSON object fails when memory allocation is deliberately made to fail.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object `root` using `cJSON_CreateObject`.
    - Initialize cJSON hooks to use `failing_hooks`, which simulates memory allocation failure.
    - Attempt to add a raw JSON string "{}" to the `root` object with the key "raw" using `cJSON_AddRawToObject`, expecting it to return NULL due to allocation failure.
    - Revert cJSON hooks to default by passing NULL to `cJSON_InitHooks`.
    - Delete the `root` object using `cJSON_Delete`.
- **Output**: The function does not return any value; it uses assertions to verify that the operation fails as expected.


---
### cJSON\_add\_object\_should\_add\_object<!-- {{#callable:cJSON_add_object_should_add_object}} -->
The function `cJSON_add_object_should_add_object` tests the functionality of adding an object to a cJSON object and verifies its successful addition.
- **Inputs**: None
- **Control Flow**:
    - Create a new cJSON object `root` using `cJSON_CreateObject`.
    - Declare a pointer `object` and initialize it to NULL.
    - Add an object to `root` with the key "object" using `cJSON_AddObjectToObject`.
    - Retrieve the added object using `cJSON_GetObjectItemCaseSensitive` and assign it to `object`.
    - Assert that `object` is not NULL using `TEST_ASSERT_NOT_NULL`.
    - Assert that the type of `object` is `cJSON_Object` using `TEST_ASSERT_EQUAL_INT`.
    - Delete the `root` object using `cJSON_Delete`.
- **Output**: The function does not return any value; it performs assertions to verify the correct behavior of adding an object to a cJSON object.


---
### cjson\_add\_object\_should\_fail\_with\_null\_pointers<!-- {{#callable:cjson_add_object_should_fail_with_null_pointers}} -->
The function `cjson_add_object_should_fail_with_null_pointers` tests that adding an object to a cJSON object fails when either the parent object or the key is NULL.
- **Inputs**: None
- **Control Flow**:
    - Create a new cJSON object named `root` using `cJSON_CreateObject`.
    - Call `cJSON_AddObjectToObject` with a NULL parent and a valid key, and assert that the result is NULL using `TEST_ASSERT_NULL`.
    - Call `cJSON_AddObjectToObject` with a valid parent and a NULL key, and assert that the result is NULL using `TEST_ASSERT_NULL`.
    - Delete the `root` object using `cJSON_Delete`.
- **Output**: The function does not return any value; it performs assertions to validate expected behavior.


---
### cjson\_add\_object\_should\_fail\_on\_allocation\_failure<!-- {{#callable:cjson_add_object_should_fail_on_allocation_failure}} -->
The function `cjson_add_object_should_fail_on_allocation_failure` tests that adding an object to a cJSON object fails when memory allocation is deliberately set to fail.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object `root` using `cJSON_CreateObject`.
    - Initialize cJSON hooks to use `failing_hooks`, which simulates memory allocation failure.
    - Attempt to add an object to `root` with the key "object" using `cJSON_AddObjectToObject`, expecting it to return NULL due to allocation failure.
    - Reinitialize cJSON hooks to default by passing NULL to `cJSON_InitHooks`.
    - Delete the `root` object using `cJSON_Delete`.
- **Output**: The function does not return any value; it uses assertions to verify that the operation fails as expected.


---
### cJSON\_add\_array\_should\_add\_array<!-- {{#callable:cJSON_add_array_should_add_array}} -->
The function `cJSON_add_array_should_add_array` tests if an array can be successfully added to a cJSON object and verifies its type.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object named `root` using `cJSON_CreateObject`.
    - Declare a pointer `array` and initialize it to NULL.
    - Add an array to the `root` object with the key "array" using `cJSON_AddArrayToObject`.
    - Retrieve the item with the key "array" from `root` using `cJSON_GetObjectItemCaseSensitive` and assign it to `array`.
    - Assert that `array` is not NULL using `TEST_ASSERT_NOT_NULL`.
    - Assert that the type of `array` is `cJSON_Array` using `TEST_ASSERT_EQUAL_INT`.
    - Delete the `root` object using `cJSON_Delete`.
- **Output**: The function does not return any value; it performs assertions to validate the behavior of adding an array to a cJSON object.


---
### cjson\_add\_array\_should\_fail\_with\_null\_pointers<!-- {{#callable:cjson_add_array_should_fail_with_null_pointers}} -->
The function `cjson_add_array_should_fail_with_null_pointers` tests that adding an array to a JSON object fails when either the object or the key is NULL.
- **Inputs**: None
- **Control Flow**:
    - Create a new cJSON object named `root` using `cJSON_CreateObject()`.
    - Assert that `cJSON_AddArrayToObject(NULL, "array")` returns NULL, indicating failure when the object is NULL.
    - Assert that `cJSON_AddArrayToObject(root, NULL)` returns NULL, indicating failure when the key is NULL.
    - Delete the `root` object using `cJSON_Delete(root)` to free memory.
- **Output**: The function does not return any value; it uses assertions to validate expected behavior.


---
### cjson\_add\_array\_should\_fail\_on\_allocation\_failure<!-- {{#callable:cjson_add_array_should_fail_on_allocation_failure}} -->
The function `cjson_add_array_should_fail_on_allocation_failure` tests that adding an array to a cJSON object fails when memory allocation is deliberately set to fail.
- **Inputs**: None
- **Control Flow**:
    - Create a cJSON object `root` using `cJSON_CreateObject`.
    - Initialize cJSON hooks to use `failing_hooks`, which simulates memory allocation failure.
    - Attempt to add an array to the `root` object with the key "array" using `cJSON_AddArrayToObject`, expecting it to return NULL due to allocation failure.
    - Reset cJSON hooks to default by passing NULL to `cJSON_InitHooks`.
    - Delete the `root` object using `cJSON_Delete`.
- **Output**: The function does not return any value; it uses assertions to verify that the array addition fails as expected.


---
### main<!-- {{#callable:CJSON_CDECL::main}} -->
The `main` function initializes and runs a series of unit tests for various cJSON operations using the Unity test framework.
- **Inputs**: None
- **Control Flow**:
    - The function begins by calling `UNITY_BEGIN()` to initialize the Unity test framework.
    - It then sequentially calls `RUN_TEST()` for each test function, which includes tests for adding null, true, false, boolean, number, string, raw, object, and array to a cJSON object, as well as tests for handling null pointers and allocation failures for these operations.
    - Finally, the function calls `UNITY_END()` to conclude the test run and return the result of the tests.
- **Output**: The function returns the result of `UNITY_END()`, which indicates the success or failure of the test suite.


