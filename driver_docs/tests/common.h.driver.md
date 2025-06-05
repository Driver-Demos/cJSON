# Purpose
This C header file, `cJSON_tests_common.h`, is part of a testing framework for the cJSON library, which is a lightweight JSON parser in C. The file provides utility functions and macros to facilitate testing of cJSON objects. It includes a function [`reset`](#reset) that clears a cJSON item, ensuring that any dynamically allocated memory associated with the item is properly deallocated, and the item is reset to a default state. This is crucial for maintaining memory integrity during tests. Another function, [`read_file`](#read_file), is provided to read the contents of a file into a string, which can be useful for loading JSON data from files for testing purposes.

Additionally, the file defines a series of assertion helper macros that are used to verify the state and properties of cJSON objects during tests. These macros check for specific conditions, such as whether an item has a particular type, whether it has a reference or a constant string, and whether it is part of a linked list. By using these macros, developers can write concise and readable test cases that ensure the cJSON library behaves as expected. The inclusion of the cJSON source file (`cJSON.c`) suggests that this header is intended for internal testing purposes, rather than being part of the public API.
# Imports and Dependencies

---
- `../cJSON.c`


# Global Variables

---
### read\_file
- **Type**: `function`
- **Description**: The `read_file` function is designed to read the contents of a file specified by its filename and return the content as a dynamically allocated string. It opens the file in binary read mode, determines the file's length, allocates memory for the content, reads the file into memory, and ensures the string is null-terminated.
- **Use**: This function is used to read the entire content of a file into a string for further processing or analysis.


# Functions

---
### reset<!-- {{#callable:reset}} -->
The `reset` function clears and deallocates memory associated with a `cJSON` item, effectively resetting it to an initial state.
- **Inputs**:
    - `item`: A pointer to a `cJSON` structure that is to be reset.
- **Control Flow**:
    - Check if `item` is not NULL and has a child; if so, delete the child using `cJSON_Delete`.
    - Check if `item` has a non-NULL `valuestring` and is not a reference; if so, deallocate `valuestring` using `global_hooks.deallocate`.
    - Check if `item` has a non-NULL `string` and is not a constant string; if so, deallocate `string` using `global_hooks.deallocate`.
    - Use `memset` to zero out the memory of the `cJSON` structure, effectively resetting it.
- **Output**: The function does not return a value; it modifies the `cJSON` structure pointed to by `item` in place.


---
### read\_file<!-- {{#callable:read_file}} -->
The `read_file` function reads the entire content of a file specified by its filename into a dynamically allocated string and returns it.
- **Inputs**:
    - `filename`: A constant character pointer representing the name of the file to be read.
- **Control Flow**:
    - Open the file in binary read mode using `fopen`; if it fails, proceed to cleanup.
    - Use `fseek` to move to the end of the file to determine its length; if it fails, proceed to cleanup.
    - Retrieve the file length using `ftell`; if it fails or returns a negative value, proceed to cleanup.
    - Use `fseek` to return to the start of the file; if it fails, proceed to cleanup.
    - Allocate memory for the file content plus a null terminator; if allocation fails, proceed to cleanup.
    - Read the file content into the allocated memory using `fread`; if the number of characters read does not match the file length, free the memory and proceed to cleanup.
    - Null-terminate the string after the last character read.
    - In the cleanup section, close the file if it was opened.
- **Output**: A pointer to a dynamically allocated string containing the file's content, or NULL if an error occurred during the process.


# Function Declarations (Public API)

---
### reset<!-- {{#callable_declaration:reset}} -->
Resets a cJSON item to its default state.
- **Description**: Use this function to reset a cJSON item, clearing its contents and deallocating any associated memory. This function should be called when you want to reuse a cJSON item or ensure it is in a clean state before further use. It safely handles null pointers and checks for reference and constant string flags to avoid deallocating memory that should not be freed. After calling this function, the cJSON item will be in a default, uninitialized state.
- **Inputs**:
    - `item`: A pointer to a cJSON structure that will be reset. Must not be null. If the item has children or dynamically allocated strings, they will be deallocated unless marked as references or constants.
- **Output**: None
- **See also**: [`reset`](#reset)  (Implementation)


