# Purpose
This C source code file is a demonstration program that utilizes the cJSON library to create and manipulate JSON objects. The primary purpose of this file is to showcase how to construct various JSON data structures, such as objects and arrays, and to demonstrate the use of cJSON functions for JSON serialization and memory management. The code includes examples of creating JSON objects that represent different data types, such as strings, numbers, and arrays, and it demonstrates how to print these JSON structures in a formatted manner using both `cJSON_Print` and `cJSON_PrintPreallocated` functions. The file also includes error handling for memory allocation failures and demonstrates how to handle JSON serialization errors.

The program begins by printing the version of the cJSON library being used, and then it proceeds to create several JSON objects and arrays, including a "Video" object, a "days of the week" array, a matrix, a "gallery" item, and an array of "records." Each of these JSON structures is printed to the console using a preallocated buffer to demonstrate the formatted output. The code also includes a `struct record` definition to illustrate how complex data types can be represented in JSON. This file serves as an educational tool for developers looking to understand how to work with JSON data in C using the cJSON library, providing practical examples of JSON creation, manipulation, and output formatting.
# Imports and Dependencies

---
- `stdio.h`
- `stdlib.h`
- `string.h`
- `cJSON.h`


# Data Structures

---
### record
- **Type**: `struct`
- **Members**:
    - `precision`: A pointer to a constant character string representing the precision of the location data.
    - `lat`: A double representing the latitude of the location.
    - `lon`: A double representing the longitude of the location.
    - `address`: A pointer to a constant character string representing the address of the location.
    - `city`: A pointer to a constant character string representing the city of the location.
    - `state`: A pointer to a constant character string representing the state of the location.
    - `zip`: A pointer to a constant character string representing the ZIP code of the location.
    - `country`: A pointer to a constant character string representing the country of the location.
- **Description**: The `record` structure is designed to encapsulate geographical location data, including latitude and longitude coordinates, as well as address details such as city, state, ZIP code, and country. It also includes a precision field to specify the accuracy of the location data. This structure is useful for applications that require detailed location information, such as mapping or geolocation services.


# Functions

---
### print\_preallocated<!-- {{#callable:print_preallocated}} -->
The `print_preallocated` function attempts to print a cJSON object to a preallocated buffer, testing both successful and failed memory allocation scenarios.
- **Inputs**:
    - `root`: A pointer to a cJSON object that represents the JSON data to be printed.
- **Control Flow**:
    - Declare and initialize pointers for output strings and buffers, and size variables for buffer lengths.
    - Use `cJSON_Print` to convert the JSON object `root` to a formatted string `out`.
    - Calculate the required buffer size `len` for successful printing, adding 5 extra bytes for memory inaccuracies, and allocate memory for `buf`.
    - Check if memory allocation for `buf` fails, print an error message, and exit if it does.
    - Calculate the buffer size `len_fail` for a failed scenario (without extra bytes) and allocate memory for `buf_fail`.
    - Check if memory allocation for `buf_fail` fails, print an error message, and exit if it does.
    - Attempt to print the JSON object to `buf` using `cJSON_PrintPreallocated`; if it fails, compare `buf` with `out` and print discrepancies, then free allocated memory and return -1.
    - If successful, print the contents of `buf`.
    - Attempt to print the JSON object to `buf_fail` to force a failure due to insufficient memory; if it succeeds, print an error message, then free allocated memory and return -1.
    - Free all allocated memory and return 0 on success.
- **Output**: Returns 0 on successful printing to the preallocated buffer, or -1 if any printing operation fails.


---
### create\_objects<!-- {{#callable:create_objects}} -->
The `create_objects` function constructs various JSON objects and arrays, prints them using a preallocated buffer, and handles memory cleanup.
- **Inputs**: None
- **Control Flow**:
    - Initialize several cJSON pointers and other variables for JSON object creation.
    - Define arrays and structures for days of the week, a matrix, gallery item IDs, and record fields.
    - Create a JSON object representing a video format and add various properties to it.
    - Print the JSON object using [`print_preallocated`](#print_preallocated), check for errors, and delete the object.
    - Create a JSON string array for days of the week, print it, check for errors, and delete the object.
    - Create a JSON array for the matrix, populate it with integer arrays, print it, check for errors, and delete the object.
    - Create a JSON object for a gallery item, add nested objects and arrays, print it, check for errors, and delete the object.
    - Create a JSON array for records, populate it with objects containing field data, print it, check for errors, and delete the object.
    - Create a JSON object with a division by zero to demonstrate error handling, print it, check for errors, and delete the object.
- **Output**: The function does not return any value; it performs operations for demonstration purposes, printing JSON objects and handling errors.
- **Functions called**:
    - [`print_preallocated`](#print_preallocated)


---
### main<!-- {{#callable:CJSON_CDECL::main}} -->
The `main` function prints the cJSON library version and demonstrates object creation using the [`create_objects`](#create_objects) function.
- **Inputs**: None
- **Control Flow**:
    - Prints the cJSON library version using `printf` and `cJSON_Version()`.
    - Calls the [`create_objects`](#create_objects) function to demonstrate JSON object creation.
    - Returns 0 to indicate successful execution.
- **Output**: The function returns an integer value of 0, indicating successful execution.
- **Functions called**:
    - [`create_objects`](#create_objects)


