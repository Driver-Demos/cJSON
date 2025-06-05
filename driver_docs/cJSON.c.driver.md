# Purpose
The provided C source code is part of the cJSON library, which is a lightweight JSON parser and printer written in C. This file implements the core functionality of the cJSON library, including parsing JSON strings into cJSON objects, printing cJSON objects back into JSON strings, and managing memory for these operations. The code defines various functions to handle JSON data types such as objects, arrays, strings, numbers, booleans, and null values. It also includes utility functions for creating, deleting, and manipulating cJSON objects, as well as functions for comparing JSON objects and handling JSON arrays and objects.

The file includes public API functions that are exposed for use by other programs, such as [`cJSON_Parse`](#cJSON_Parse), [`cJSON_Print`](#cJSON_Print), [`cJSON_GetObjectItem`](#cJSON_GetObjectItem), and [`cJSON_AddItemToObject`](#cJSON_AddItemToObject). These functions allow users to parse JSON data, print JSON data, access elements within JSON objects, and modify JSON objects, respectively. The code also includes internal utility functions for memory management, string handling, and JSON data type checking. The file is designed to be included in a larger project, providing a comprehensive set of tools for working with JSON data in C applications.
# Imports and Dependencies

---
- `string.h`
- `stdio.h`
- `math.h`
- `stdlib.h`
- `limits.h`
- `ctype.h`
- `float.h`
- `locale.h`
- `cJSON.h`


# Global Variables

---
### global\_error
- **Type**: `error`
- **Description**: The `global_error` variable is a static instance of the `error` struct, which contains two fields: a pointer to a JSON string (`json`) and a position index (`position`). It is initialized with `NULL` for the `json` pointer and `0` for the `position`. This struct is used to track the position of errors encountered during JSON parsing.
- **Use**: `global_error` is used to store and retrieve the position of errors in JSON parsing operations, allowing functions to report where parsing issues occur.


---
### global\_hooks
- **Type**: `internal_hooks`
- **Description**: The `global_hooks` variable is a static instance of the `internal_hooks` structure, which contains function pointers for memory allocation, deallocation, and reallocation. It is initialized with default functions `internal_malloc`, `internal_free`, and `internal_realloc`, which are typically mapped to the standard C library functions `malloc`, `free`, and `realloc`.
- **Use**: This variable is used to manage memory operations throughout the cJSON library, allowing for custom memory management strategies if needed.


---
### cJSON\_Duplicate\_rec
- **Type**: `cJSON *`
- **Description**: The `cJSON_Duplicate_rec` function is a recursive function that duplicates a cJSON item, including its children, if specified. It takes a pointer to a cJSON item, a depth counter, and a boolean indicating whether to recurse into child items.
- **Use**: This function is used to create a deep copy of a cJSON item, optionally including all nested child items.


# Data Structures

---
### error
- **Type**: `struct`
- **Members**:
    - `json`: A pointer to an unsigned char representing the JSON data associated with the error.
    - `position`: A size_t value indicating the position in the JSON data where the error occurred.
- **Description**: The `error` structure is used to represent an error state in the JSON parsing process. It contains a pointer to the JSON data and a position index, which together indicate where in the JSON data the error was encountered. This structure is used to track and report errors during JSON parsing, allowing developers to identify and address issues in the JSON input.


---
### internal\_hooks
- **Type**: `struct`
- **Members**:
    - `allocate`: A function pointer for memory allocation, taking a size_t argument and returning a void pointer.
    - `deallocate`: A function pointer for memory deallocation, taking a void pointer argument.
    - `reallocate`: A function pointer for memory reallocation, taking a void pointer and a size_t argument, returning a void pointer.
- **Description**: The `internal_hooks` structure is designed to encapsulate custom memory management functions for allocation, deallocation, and reallocation of memory. This allows the cJSON library to use user-defined memory management routines instead of the default `malloc`, `free`, and `realloc` functions, providing flexibility for different memory management strategies or environments.


---
### parse\_buffer
- **Type**: `struct`
- **Members**:
    - `content`: A pointer to the unsigned char array representing the content to be parsed.
    - `length`: The total length of the content array.
    - `offset`: The current position within the content array being parsed.
    - `depth`: Indicates the current level of nesting within arrays or objects at the current offset.
    - `hooks`: A structure containing function pointers for memory allocation, deallocation, and reallocation.
- **Description**: The `parse_buffer` structure is used in the context of parsing JSON data. It holds the content to be parsed, tracks the current position within that content, and maintains the depth of nested structures to manage parsing of complex JSON objects and arrays. The `internal_hooks` member provides custom memory management functions, allowing for flexible allocation strategies during parsing operations.


---
### printbuffer
- **Type**: `struct`
- **Members**:
    - `buffer`: A pointer to an unsigned char array that holds the data to be printed.
    - `length`: The total size of the buffer in bytes.
    - `offset`: The current position in the buffer where the next data will be written.
    - `depth`: The current nesting depth for formatted printing.
    - `noalloc`: A boolean indicating if the buffer should not be reallocated.
    - `format`: A boolean indicating if the print should be formatted.
    - `hooks`: A structure containing function pointers for memory allocation, deallocation, and reallocation.
- **Description**: The `printbuffer` structure is used in the cJSON library to manage the buffer used for printing JSON data. It contains information about the buffer's size, current offset, and formatting options. The structure also includes a depth field to handle nested JSON objects and arrays, and a set of hooks for custom memory management. This allows for efficient and flexible printing of JSON data, either formatted or unformatted, depending on the `format` flag.


# Functions

---
### cJSON\_GetErrorPtr<!-- {{#callable:cJSON_GetErrorPtr}} -->
Returns a pointer to the error message in the global error state.
- **Inputs**: None
- **Control Flow**:
    - The function accesses the global error state structure `global_error`.
    - It calculates the address of the error message by adding the `position` offset to the base pointer of the `json` string.
    - The function returns the computed pointer to the error message.
- **Output**: A pointer to a constant character string representing the error message, or NULL if no error has occurred.


---
### cJSON\_GetStringValue<!-- {{#callable:cJSON_GetStringValue}} -->
Retrieves the string value from a `cJSON` item if it is of type string.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that is expected to be of type string.
- **Control Flow**:
    - The function first checks if the `item` is a string using the [`cJSON_IsString`](#cJSON_IsString) function.
    - If `item` is not a string, the function returns NULL.
    - If `item` is a string, the function returns the `valuestring` member of the `item`.
- **Output**: Returns a pointer to the string value of the `cJSON` item if it is a string; otherwise, it returns NULL.
- **Functions called**:
    - [`cJSON_IsString`](#cJSON_IsString)


---
### cJSON\_GetNumberValue<!-- {{#callable:cJSON_GetNumberValue}} -->
Retrieves the numeric value of a `cJSON` item if it is a number, otherwise returns NaN.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that is expected to represent a number.
- **Control Flow**:
    - Checks if the `item` is a number using the [`cJSON_IsNumber`](#cJSON_IsNumber) function.
    - If `item` is not a number, it returns NaN (Not a Number).
    - If `item` is a number, it retrieves and returns the value stored in `item->valuedouble`.
- **Output**: Returns the numeric value of the `cJSON` item as a double, or NaN if the item is not a number.
- **Functions called**:
    - [`cJSON_IsNumber`](#cJSON_IsNumber)


---
### cJSON\_Version<!-- {{#callable:cJSON_Version}} -->
Returns the version of the cJSON library as a string.
- **Inputs**: None
- **Control Flow**:
    - A static character array `version` of size 15 is declared to hold the version string.
    - The `sprintf` function is used to format the version string using the major, minor, and patch version numbers defined by `CJSON_VERSION_MAJOR`, `CJSON_VERSION_MINOR`, and `CJSON_VERSION_PATCH`.
    - The formatted version string is returned.
- **Output**: A pointer to a string containing the version of the cJSON library.


---
### case\_insensitive\_strcmp<!-- {{#callable:case_insensitive_strcmp}} -->
Compares two strings in a case-insensitive manner.
- **Inputs**:
    - `string1`: A pointer to the first string to compare.
    - `string2`: A pointer to the second string to compare.
- **Control Flow**:
    - Checks if either `string1` or `string2` is NULL, returning 1 if so.
    - If both strings point to the same memory location, returns 0.
    - Iterates through both strings, comparing their characters in a case-insensitive manner using `tolower`.
    - If a null terminator is reached in both strings simultaneously, returns 0.
    - Returns the difference between the first non-matching characters after converting them to lowercase.
- **Output**: Returns 0 if the strings are equal, a negative value if `string1` is less than `string2`, and a positive value if `string1` is greater than `string2.


---
### internal\_malloc<!-- {{#callable:CJSON_CDECL::internal_malloc}} -->
Allocates memory of the specified size using `malloc`.
- **Inputs**:
    - `size`: The number of bytes to allocate.
- **Control Flow**:
    - Calls `malloc` with the specified size.
    - Returns the pointer to the allocated memory.
- **Output**: A pointer to the allocated memory block, or NULL if the allocation fails.


---
### internal\_free<!-- {{#callable:CJSON_CDECL::internal_free}} -->
The `internal_free` function deallocates memory previously allocated for a given pointer.
- **Inputs**:
    - `pointer`: A pointer to the memory block that needs to be freed.
- **Control Flow**:
    - The function takes a single argument, `pointer`, which is expected to be a valid memory address.
    - It calls the standard library function `free` to deallocate the memory pointed to by `pointer`.
- **Output**: This function does not return a value; it simply frees the memory associated with the provided pointer.


---
### internal\_realloc<!-- {{#callable:CJSON_CDECL::internal_realloc}} -->
The `internal_realloc` function reallocates memory for a given pointer to a specified size.
- **Inputs**:
    - `pointer`: A pointer to the memory block that needs to be reallocated.
    - `size`: The new size in bytes for the memory block.
- **Control Flow**:
    - The function calls the standard library function `realloc` with the provided `pointer` and `size`.
    - It returns the pointer returned by `realloc`, which may be the same as the original pointer or a new pointer to a different memory location.
- **Output**: Returns a pointer to the reallocated memory block, or NULL if the reallocation fails.


---
### cJSON\_InitHooks<!-- {{#callable:cJSON_InitHooks}} -->
Initializes custom memory allocation hooks for the cJSON library.
- **Inputs**:
    - `hooks`: A pointer to a `cJSON_Hooks` structure that contains custom allocation, deallocation, and reallocation functions.
- **Control Flow**:
    - If the `hooks` pointer is NULL, the function resets the global memory allocation hooks to use the default `malloc`, `free`, and `realloc` functions.
    - If `hooks` is not NULL, it checks if the custom allocation function (`malloc_fn`) is provided; if so, it sets the global allocation hook to this function.
    - It then checks if a custom deallocation function (`free_fn`) is provided and sets the global deallocation hook accordingly.
    - Finally, it sets the global reallocation hook to `realloc` only if both the allocation and deallocation hooks are set to their default values.
- **Output**: The function does not return a value; it modifies the global memory allocation hooks used by the cJSON library.


---
### cJSON\_New\_Item<!-- {{#callable:cJSON_New_Item}} -->
Creates a new `cJSON` item by allocating memory and initializing it.
- **Inputs**:
    - `hooks`: A pointer to an `internal_hooks` structure that contains memory allocation functions.
- **Control Flow**:
    - Allocates memory for a new `cJSON` item using the provided allocation function from `hooks`.
    - If the allocation is successful, it initializes the allocated memory to zero using `memset`.
    - Returns the pointer to the newly created `cJSON` item or NULL if allocation fails.
- **Output**: Returns a pointer to the newly created `cJSON` item, or NULL if memory allocation fails.


---
### cJSON\_Delete<!-- {{#callable:cJSON_Delete}} -->
The `cJSON_Delete` function recursively frees a `cJSON` structure and all its children.
- **Inputs**:
    - `item`: A pointer to the `cJSON` structure to be deleted.
- **Control Flow**:
    - The function initializes a pointer `next` to NULL.
    - It enters a while loop that continues as long as `item` is not NULL.
    - Inside the loop, it stores the next item in `next` before processing the current item.
    - If the current item is not a reference and has children, it recursively calls `cJSON_Delete` on the child.
    - If the current item is not a reference and has a non-NULL `valuestring`, it deallocates the `valuestring` memory.
    - If the current item is not a constant string and has a non-NULL `string`, it deallocates the `string` memory.
    - Finally, it deallocates the current item itself and moves to the next item.
- **Output**: This function does not return a value; it frees the memory allocated for the `cJSON` structure and its associated data.


---
### get\_decimal\_point<!-- {{#callable:get_decimal_point}} -->
The `get_decimal_point` function retrieves the decimal point character based on the current locale settings.
- **Inputs**: None
- **Control Flow**:
    - If `ENABLE_LOCALES` is defined, the function calls `localeconv()` to obtain the current locale's formatting information.
    - It returns the first character of the `decimal_point` string from the `lconv` structure.
    - If `ENABLE_LOCALES` is not defined, it simply returns the character '.' as the decimal point.
- **Output**: The function returns an `unsigned char` representing the decimal point character, which is either the locale-specific character or '.'.


---
### parse\_number<!-- {{#callable:parse_number}} -->
Parses a number from a JSON input buffer and populates a cJSON item with the parsed value.
- **Inputs**:
    - `item`: A pointer to a `cJSON` structure where the parsed number will be stored.
    - `input_buffer`: A pointer to a `parse_buffer` structure containing the JSON input data to be parsed.
- **Control Flow**:
    - Checks if the `input_buffer` or its content is NULL, returning false if so.
    - Iterates through the `input_buffer` to count valid characters for the number, including digits, signs, and scientific notation.
    - Allocates a temporary buffer to hold the number string, replacing '.' with the locale-specific decimal point if necessary.
    - Uses `strtod` to convert the string to a double, checking for parsing errors.
    - Stores the parsed double in the `item` structure, handling potential overflow by saturating the integer value.
    - Updates the `input_buffer` offset to reflect the number of characters consumed and frees the temporary buffer.
- **Output**: Returns true if the number was successfully parsed and stored; otherwise, returns false.
- **Functions called**:
    - [`get_decimal_point`](#get_decimal_point)


---
### cJSON\_SetNumberHelper<!-- {{#callable:cJSON_SetNumberHelper}} -->
Sets the integer and double values of a `cJSON` object based on a given double input.
- **Inputs**:
    - `object`: A pointer to a `cJSON` object that will have its values set.
    - `number`: A double value that will be used to set the integer and double values of the `cJSON` object.
- **Control Flow**:
    - Checks if the input `number` is greater than or equal to `INT_MAX` and sets `object->valueint` to `INT_MAX` if true.
    - Checks if the input `number` is less than or equal to `INT_MIN` and sets `object->valueint` to `INT_MIN` if true.
    - If the input `number` is within the range of `INT_MIN` and `INT_MAX`, it casts the number to an integer and assigns it to `object->valueint`.
    - Finally, it sets `object->valuedouble` to the input `number` and returns this value.
- **Output**: Returns the double value that was set in `object->valuedouble`.


---
### cJSON\_SetValuestring<!-- {{#callable:cJSON_SetValuestring}} -->
Sets the `valuestring` of a `cJSON` object to a new string.
- **Inputs**:
    - `object`: A pointer to a `cJSON` object that is expected to be of type `cJSON_String`.
    - `valuestring`: A pointer to a null-terminated string that will be set as the new value for the `valuestring` of the `cJSON` object.
- **Control Flow**:
    - Check if the `object` is NULL or not of type `cJSON_String`, or if it is a reference type; if so, return NULL.
    - Check if the current `valuestring` of the object or the new `valuestring` is NULL; if so, return NULL.
    - Calculate the lengths of the current and new strings.
    - If the new string is shorter than or equal to the current string, check for overlapping memory regions and copy the new string directly.
    - If the new string is longer, duplicate the new string, free the old string if it exists, and set the new string as the object's `valuestring`.
- **Output**: Returns a pointer to the new `valuestring` if successful, or NULL if an error occurred.
- **Functions called**:
    - [`cJSON_strdup`](#cJSON_strdup)
    - [`cJSON_free`](#cJSON_free)


---
### ensure<!-- {{#callable:ensure}} -->
Reallocates the buffer in a `printbuffer` structure to ensure it has enough space for additional data.
- **Inputs**:
    - `p`: A pointer to a `printbuffer` structure that contains the current buffer, its length, and offset.
    - `needed`: The amount of additional space needed in the buffer.
- **Control Flow**:
    - Checks if the `printbuffer` pointer `p` or its `buffer` is NULL, returning NULL if so.
    - Validates that the `offset` is within the bounds of the current `length` of the buffer.
    - Returns a pointer to the current buffer plus the `offset` if the needed space is already available.
    - Calculates the new size for the buffer, ensuring it does not exceed `INT_MAX`.
    - Attempts to reallocate the buffer using the provided reallocation hook, or allocates a new buffer if no hook is available.
    - Copies the existing data from the old buffer to the new buffer if a new allocation is made.
    - Updates the `length` and `buffer` fields of the `printbuffer` structure before returning a pointer to the newly allocated space.
- **Output**: Returns a pointer to the buffer at the current offset, or NULL if allocation fails or if the input conditions are not met.


---
### update\_offset<!-- {{#callable:update_offset}} -->
Updates the offset of a `printbuffer` by the length of the string at the current offset.
- **Inputs**:
    - `buffer`: A pointer to a `printbuffer` structure that contains the current offset and the buffer to be updated.
- **Control Flow**:
    - Checks if the `buffer` or its internal `buffer` pointer is NULL; if so, the function returns immediately.
    - Calculates the pointer to the current position in the buffer using the `offset` field.
    - Updates the `offset` field of the `buffer` by adding the length of the string starting from the current position.
- **Output**: The function does not return a value; it modifies the `offset` field of the provided `printbuffer` directly.


---
### compare\_double<!-- {{#callable:compare_double}} -->
Compares two double precision floating-point numbers for equality within a relative tolerance.
- **Inputs**:
    - `a`: The first double value to compare.
    - `b`: The second double value to compare.
- **Control Flow**:
    - Calculates the maximum absolute value between `a` and `b` using `fabs`.
    - Checks if the absolute difference between `a` and `b` is less than or equal to the product of the maximum value and `DBL_EPSILON`.
- **Output**: Returns `cJSON_bool` indicating whether the two double values are considered equal within the defined tolerance.


---
### print\_number<!-- {{#callable:print_number}} -->
The `print_number` function formats a `cJSON` number item into a string representation and writes it to a specified output buffer.
- **Inputs**:
    - `item`: A pointer to a `cJSON` structure representing the number to be printed.
    - `output_buffer`: A pointer to a `printbuffer` structure where the formatted number will be written.
- **Control Flow**:
    - Checks if the `output_buffer` is NULL and returns false if it is.
    - Checks if the number is NaN or Infinity, and if so, formats it as 'null'.
    - If the number is an integer, it formats it as an integer string.
    - If the number is a floating-point, it attempts to format it with 15 decimal places.
    - If the formatted string cannot accurately represent the original number, it tries again with 17 decimal places.
    - Checks for buffer overrun or formatting errors and returns false if any occur.
    - Ensures there is enough space in the `output_buffer` for the formatted number.
    - Copies the formatted number into the `output_buffer`, replacing locale-specific decimal points with '.'
    - Updates the offset of the `output_buffer` to reflect the new data added.
- **Output**: Returns true if the number was successfully formatted and written to the output buffer; otherwise, returns false.
- **Functions called**:
    - [`get_decimal_point`](#get_decimal_point)
    - [`compare_double`](#compare_double)
    - [`ensure`](#ensure)


---
### parse\_hex4<!-- {{#callable:parse_hex4}} -->
Parses a 4-digit hexadecimal string and returns its integer value.
- **Inputs**:
    - `input`: A pointer to an array of unsigned characters representing a 4-character hexadecimal string.
- **Control Flow**:
    - Initializes an unsigned integer `h` to 0 and a size_t variable `i` to 0.
    - Iterates over the first four characters of the input string.
    - For each character, checks if it is a valid hexadecimal digit (0-9, A-F, a-f).
    - If valid, converts the character to its integer value and adds it to `h`, shifting `h` left by 4 bits for each character except the last.
    - If an invalid character is encountered, returns 0 immediately.
    - After processing all four characters, returns the accumulated integer value.
- **Output**: Returns the integer value represented by the hexadecimal string, or 0 if the input contains invalid characters.


---
### utf16\_literal\_to\_utf8<!-- {{#callable:utf16_literal_to_utf8}} -->
Converts a UTF-16 literal to UTF-8 encoding.
- **Inputs**:
    - `input_pointer`: Pointer to the start of the UTF-16 literal input.
    - `input_end`: Pointer to the end of the input buffer.
    - `output_pointer`: Pointer to a pointer where the UTF-8 output will be written.
- **Control Flow**:
    - Checks if the input length is less than 6 bytes, indicating an unexpected end of input.
    - Parses the first UTF-16 sequence to obtain the code point.
    - Validates the first code point to ensure it is not a surrogate.
    - If the first code point indicates a surrogate pair, it checks for a second sequence and validates it.
    - Calculates the Unicode code point from the surrogate pair if applicable.
    - Determines the UTF-8 length and first byte mark based on the code point value.
    - Encodes the code point into UTF-8 format, writing the result to the output pointer.
    - Returns the length of the sequence processed or 0 in case of failure.
- **Output**: Returns the length of the UTF-16 sequence processed, or 0 if an error occurred.
- **Functions called**:
    - [`parse_hex4`](#parse_hex4)


---
### parse\_string<!-- {{#callable:parse_string}} -->
Parses a JSON string from the input buffer and populates a cJSON item.
- **Inputs**:
    - `item`: A pointer to a `cJSON` structure where the parsed string will be stored.
    - `input_buffer`: A pointer to a `parse_buffer` structure containing the JSON string to be parsed.
- **Control Flow**:
    - Checks if the first character in the input buffer is a double quote ('"'). If not, it jumps to the fail label.
    - Calculates the approximate size of the output string by iterating through the input buffer until the closing double quote is found, accounting for escape sequences.
    - Allocates memory for the output string based on the calculated size.
    - Iterates through the input string, copying characters to the output while handling escape sequences appropriately.
    - Handles UTF-16 escape sequences by converting them to UTF-8.
    - Zero-terminates the output string and assigns it to the `valuestring` field of the `item`.
    - Updates the offset in the input buffer to point to the next character after the closing double quote.
    - Returns true if parsing is successful; otherwise, it jumps to the fail label.
- **Output**: Returns true if the string was successfully parsed; otherwise, returns false.
- **Functions called**:
    - [`utf16_literal_to_utf8`](#utf16_literal_to_utf8)


---
### print\_string\_ptr<!-- {{#callable:print_string_ptr}} -->
The `print_string_ptr` function formats a string for JSON output, escaping necessary characters.
- **Inputs**:
    - `input`: A pointer to the input string that needs to be formatted.
    - `output_buffer`: A pointer to a `printbuffer` structure where the formatted string will be stored.
- **Control Flow**:
    - Check if the `output_buffer` is NULL; if so, return false.
    - If the `input` string is NULL, allocate space for an empty JSON string and return true.
    - Iterate through the `input` string to count characters that need escaping.
    - Calculate the total length of the output string, including escape characters.
    - Ensure the `output_buffer` has enough space for the formatted string.
    - If no characters need escaping, copy the input string directly into the output buffer.
    - If there are characters to escape, copy the input string to the output buffer while escaping necessary characters.
- **Output**: Returns true if the string was successfully formatted and stored in the output buffer; otherwise, returns false.
- **Functions called**:
    - [`ensure`](#ensure)


---
### print\_string<!-- {{#callable:print_string}} -->
The `print_string` function renders a JSON string item into a formatted output buffer.
- **Inputs**:
    - `item`: A pointer to a `cJSON` structure representing the JSON item to be printed, specifically expected to be of type string.
    - `p`: A pointer to a `printbuffer` structure that holds the output buffer where the rendered string will be stored.
- **Control Flow**:
    - The function calls [`print_string_ptr`](#print_string_ptr), passing the `valuestring` of the `item` cast to an `unsigned char*` along with the output buffer `p`.
    - The [`print_string_ptr`](#print_string_ptr) function handles the actual rendering of the string, including any necessary escaping.
- **Output**: Returns a boolean value indicating success (true) or failure (false) of the printing operation.
- **Functions called**:
    - [`print_string_ptr`](#print_string_ptr)


---
### buffer\_skip\_whitespace<!-- {{#callable:buffer_skip_whitespace}} -->
The `buffer_skip_whitespace` function advances the offset of a `parse_buffer` structure to skip over any leading whitespace characters.
- **Inputs**:
    - `buffer`: A pointer to a `parse_buffer` structure that contains the content to be parsed.
- **Control Flow**:
    - The function first checks if the `buffer` or its `content` is NULL, returning NULL if either is the case.
    - It then checks if the first index of the buffer can be accessed; if not, it returns the buffer as is.
    - A while loop is used to increment the `offset` of the buffer as long as the current character is a whitespace character (ASCII value <= 32).
    - After the loop, if the `offset` equals the `length` of the buffer, it decrements the `offset` by one to avoid going out of bounds.
    - Finally, the function returns the updated `buffer`.
- **Output**: Returns a pointer to the updated `parse_buffer` with the offset adjusted to skip leading whitespace.


---
### skip\_utf8\_bom<!-- {{#callable:skip_utf8_bom}} -->
The `skip_utf8_bom` function skips the UTF-8 Byte Order Mark (BOM) in a given parse buffer if it is present at the start.
- **Inputs**:
    - `buffer`: A pointer to a `parse_buffer` structure that contains the content to be checked for a UTF-8 BOM.
- **Control Flow**:
    - The function first checks if the `buffer` is NULL, if its `content` is NULL, or if the `offset` is not zero; if any of these conditions are true, it returns NULL.
    - It then checks if there are at least 4 bytes available to read in the buffer and if the first three bytes match the UTF-8 BOM (0xEF, 0xBB, 0xBF).
    - If the BOM is found, it increments the `offset` of the buffer by 3 to skip over the BOM.
- **Output**: Returns the updated `parse_buffer` pointer, or NULL if the input conditions were not met.


---
### cJSON\_ParseWithOpts<!-- {{#callable:cJSON_ParseWithOpts}} -->
Parses a JSON string with options for null termination.
- **Inputs**:
    - `value`: A pointer to the JSON string to be parsed.
    - `return_parse_end`: A pointer to a pointer that will be set to the end of the parsed JSON string.
    - `require_null_terminated`: A boolean flag indicating whether the JSON string must be null-terminated.
- **Control Flow**:
    - Checks if the input string `value` is NULL; if so, returns NULL.
    - Calculates the length of the input string, adding space for a null terminator if required.
    - Calls [`cJSON_ParseWithLengthOpts`](#cJSON_ParseWithLengthOpts) with the calculated length and other parameters.
- **Output**: Returns a pointer to a `cJSON` object representing the parsed JSON, or NULL on failure.
- **Functions called**:
    - [`cJSON_ParseWithLengthOpts`](#cJSON_ParseWithLengthOpts)


---
### cJSON\_ParseWithLengthOpts<!-- {{#callable:cJSON_ParseWithLengthOpts}} -->
Parses a JSON string with specified length options and returns a cJSON object.
- **Inputs**:
    - `value`: A pointer to the JSON string to be parsed.
    - `buffer_length`: The length of the JSON string to be parsed.
    - `return_parse_end`: A pointer to a string pointer that will be set to the end of the parsed JSON.
    - `require_null_terminated`: A boolean indicating whether the JSON string must be null-terminated.
- **Control Flow**:
    - Initializes a parse buffer and resets the global error state.
    - Checks if the input string is NULL or if the buffer length is zero, and jumps to the fail label if true.
    - Sets up the parse buffer with the input string and its length.
    - Attempts to create a new cJSON item to hold the parsed data.
    - Calls the [`parse_value`](#parse_value) function to parse the JSON value from the buffer.
    - If `require_null_terminated` is true, checks for a null terminator after parsing.
    - If `return_parse_end` is provided, sets it to the end of the parsed JSON.
    - Returns the parsed cJSON item or NULL if parsing fails, cleaning up any allocated memory.
- **Output**: Returns a pointer to a cJSON object representing the parsed JSON, or NULL if parsing fails.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)
    - [`parse_value`](#parse_value)
    - [`buffer_skip_whitespace`](#buffer_skip_whitespace)
    - [`skip_utf8_bom`](#skip_utf8_bom)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### cJSON\_Parse<!-- {{#callable:cJSON_Parse}} -->
Parses a JSON string and returns a `cJSON` object.
- **Inputs**:
    - `value`: A pointer to a null-terminated string containing the JSON data to be parsed.
- **Control Flow**:
    - Calls [`cJSON_ParseWithOpts`](#cJSON_ParseWithOpts) with the provided JSON string and default options (0 for both options).
- **Output**: Returns a pointer to a `cJSON` object representing the parsed JSON data, or NULL if parsing fails.
- **Functions called**:
    - [`cJSON_ParseWithOpts`](#cJSON_ParseWithOpts)


---
### cJSON\_ParseWithLength<!-- {{#callable:cJSON_ParseWithLength}} -->
Parses a JSON string with a specified buffer length.
- **Inputs**:
    - `value`: A pointer to the JSON string to be parsed.
    - `buffer_length`: The length of the JSON string to be parsed.
- **Control Flow**:
    - The function calls [`cJSON_ParseWithLengthOpts`](#cJSON_ParseWithLengthOpts) with the provided `value` and `buffer_length`, along with two additional parameters set to 0.
    - The [`cJSON_ParseWithLengthOpts`](#cJSON_ParseWithLengthOpts) function handles the actual parsing of the JSON string.
- **Output**: Returns a pointer to a `cJSON` object representing the parsed JSON structure, or NULL if parsing fails.
- **Functions called**:
    - [`cJSON_ParseWithLengthOpts`](#cJSON_ParseWithLengthOpts)


---
### print<!-- {{#callable:print}} -->
The `print` function serializes a `cJSON` object into a JSON formatted string.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that needs to be serialized.
    - `format`: A boolean indicating whether the output should be formatted (pretty-printed) or not.
    - `hooks`: A pointer to an `internal_hooks` structure that provides custom memory allocation functions.
- **Control Flow**:
    - Initializes a static buffer for printing and allocates memory for it using the provided hooks.
    - Checks if the buffer allocation was successful; if not, it jumps to the failure handling.
    - Calls [`print_value`](#print_value) to serialize the `cJSON` object into the buffer.
    - Updates the buffer's offset after printing the value.
    - Checks if reallocation is needed; if so, attempts to reallocate the buffer to fit the serialized string.
    - If reallocation is not available, it copies the contents of the buffer to a new memory location and frees the original buffer.
    - Handles any failures by deallocating memory and returning NULL.
- **Output**: Returns a pointer to a dynamically allocated string containing the serialized JSON representation of the `cJSON` object, or NULL if an error occurred.
- **Functions called**:
    - [`print_value`](#print_value)
    - [`update_offset`](#update_offset)


---
### cJSON\_Print<!-- {{#callable:cJSON_Print}} -->
The `cJSON_Print` function converts a `cJSON` object into a formatted JSON string.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that needs to be printed as a JSON string.
- **Control Flow**:
    - The function calls the [`print`](#print) function with the provided `item`, a boolean value `true` for formatting, and a pointer to `global_hooks` for memory management.
    - The [`print`](#print) function handles the actual conversion of the `cJSON` object to a string, managing memory allocation and formatting.
- **Output**: Returns a pointer to a dynamically allocated string containing the JSON representation of the `cJSON` object, or NULL if an error occurs.
- **Functions called**:
    - [`print`](#print)


---
### cJSON\_PrintUnformatted<!-- {{#callable:cJSON_PrintUnformatted}} -->
The `cJSON_PrintUnformatted` function serializes a `cJSON` object into a JSON string without formatting.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that needs to be serialized.
- **Control Flow**:
    - The function calls the [`print`](#print) function with the `item`, a `false` flag indicating no formatting, and a pointer to `global_hooks`.
    - The [`print`](#print) function handles the serialization process and returns a pointer to the resulting string.
- **Output**: Returns a pointer to a string containing the serialized JSON representation of the `cJSON` object, or NULL if an error occurs.
- **Functions called**:
    - [`print`](#print)


---
### cJSON\_PrintBuffered<!-- {{#callable:cJSON_PrintBuffered}} -->
The `cJSON_PrintBuffered` function generates a JSON string representation of a `cJSON` item using a pre-allocated buffer.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that represents the JSON structure to be printed.
    - `prebuffer`: An integer specifying the size of the pre-allocated buffer to use for the output string.
    - `fmt`: A boolean value indicating whether the output should be formatted (pretty-printed) or not.
- **Control Flow**:
    - The function first checks if the `prebuffer` is negative; if so, it returns NULL.
    - It allocates a buffer of size `prebuffer` using the global memory allocation hooks.
    - If the buffer allocation fails, it returns NULL.
    - The function initializes the `printbuffer` structure with the allocated buffer and other parameters.
    - It calls the [`print_value`](#print_value) function to generate the JSON string representation of the `item` into the buffer.
    - If [`print_value`](#print_value) fails, it deallocates the buffer and returns NULL.
    - Finally, it returns the pointer to the buffer containing the JSON string.
- **Output**: Returns a pointer to the generated JSON string if successful, or NULL if an error occurs.
- **Functions called**:
    - [`print_value`](#print_value)


---
### cJSON\_PrintPreallocated<!-- {{#callable:cJSON_PrintPreallocated}} -->
The `cJSON_PrintPreallocated` function formats a `cJSON` object into a preallocated buffer.
- **Inputs**:
    - `item`: A pointer to the `cJSON` object that needs to be printed.
    - `buffer`: A pointer to a preallocated character buffer where the formatted JSON string will be stored.
    - `length`: An integer representing the size of the preallocated buffer.
    - `format`: A boolean indicating whether the output should be formatted (pretty-printed) or not.
- **Control Flow**:
    - The function initializes a `printbuffer` structure to hold the output details.
    - It checks if the provided buffer is valid and if the length is non-negative.
    - If the checks pass, it sets up the `printbuffer` with the provided buffer and its length.
    - Finally, it calls the [`print_value`](#print_value) function to format the `cJSON` object into the buffer.
- **Output**: Returns a boolean indicating success (true) or failure (false) of the printing operation.
- **Functions called**:
    - [`print_value`](#print_value)


---
### parse\_value<!-- {{#callable:parse_value}} -->
Parses a JSON value from the input buffer and populates the provided cJSON item.
- **Inputs**:
    - `item`: A pointer to a `cJSON` structure where the parsed value will be stored.
    - `input_buffer`: A pointer to a `parse_buffer` structure containing the JSON input data to be parsed.
- **Control Flow**:
    - First, the function checks if the `input_buffer` or its content is NULL, returning false if so.
    - It then attempts to parse the input as different JSON types: null, false, true, string, number, array, or object.
    - For each type, it checks if the input buffer has enough data to read and matches the expected format.
    - If a match is found, the corresponding type is set in the `item`, and the input buffer's offset is updated.
    - If none of the types match, the function returns false.
- **Output**: Returns true if a value was successfully parsed and stored in the `item`, otherwise returns false.
- **Functions called**:
    - [`parse_string`](#parse_string)
    - [`parse_number`](#parse_number)
    - [`parse_array`](#parse_array)
    - [`parse_object`](#parse_object)


---
### print\_value<!-- {{#callable:print_value}} -->
The `print_value` function serializes a `cJSON` item into a string representation and writes it to a specified output buffer.
- **Inputs**:
    - `item`: A pointer to a `cJSON` structure representing the JSON item to be printed.
    - `output_buffer`: A pointer to a `printbuffer` structure that holds the output buffer where the serialized string will be written.
- **Control Flow**:
    - The function first checks if either `item` or `output_buffer` is NULL, returning false if so.
    - It then uses a switch statement to determine the type of the `cJSON` item.
    - For `cJSON_NULL`, `cJSON_False`, and `cJSON_True`, it allocates space in the output buffer and copies the corresponding string representation.
    - For `cJSON_Number`, it calls the [`print_number`](#print_number) function to handle the serialization.
    - For `cJSON_Raw`, it checks if the `valuestring` is NULL, allocates space, and copies the raw string.
    - For `cJSON_String`, `cJSON_Array`, and `cJSON_Object`, it calls their respective print functions to handle serialization.
    - If the item type does not match any known type, it returns false.
- **Output**: The function returns a boolean value indicating success (true) or failure (false) of the serialization process.
- **Functions called**:
    - [`ensure`](#ensure)
    - [`print_number`](#print_number)
    - [`print_string`](#print_string)
    - [`print_array`](#print_array)
    - [`print_object`](#print_object)


---
### parse\_array<!-- {{#callable:parse_array}} -->
Parses a JSON array from the input buffer and populates a `cJSON` item.
- **Inputs**:
    - `item`: A pointer to a `cJSON` structure where the parsed array will be stored.
    - `input_buffer`: A pointer to a `parse_buffer` structure that contains the JSON input data and its parsing state.
- **Control Flow**:
    - Checks if the current depth of parsing exceeds the nesting limit; if so, returns false.
    - Increments the depth of the input buffer.
    - Validates that the current character in the buffer is the opening bracket '['; if not, it jumps to the fail label.
    - Increments the offset to skip the opening bracket and skips any whitespace.
    - Checks if the next character is a closing bracket ']', indicating an empty array; if so, it jumps to the success label.
    - Enters a loop to parse each element of the array, allocating a new `cJSON` item for each element.
    - Parses each value in the array and links the items in a doubly linked list.
    - Checks for commas to continue parsing additional elements until the closing bracket ']' is found.
    - If the closing bracket is not found, it jumps to the fail label.
    - On success, decrements the depth and sets the type of the item to `cJSON_Array`, linking the head of the list to the item.
- **Output**: Returns true if the array was successfully parsed; otherwise, it returns false and cleans up any allocated items.
- **Functions called**:
    - [`buffer_skip_whitespace`](#buffer_skip_whitespace)
    - [`cJSON_New_Item`](#cJSON_New_Item)
    - [`parse_value`](#parse_value)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### print\_array<!-- {{#callable:print_array}} -->
The `print_array` function renders a JSON array to a string format.
- **Inputs**:
    - `item`: A pointer to a `cJSON` structure representing the array to be printed.
    - `output_buffer`: A pointer to a `printbuffer` structure where the output string will be stored.
- **Control Flow**:
    - The function first checks if the `output_buffer` is NULL and returns false if it is.
    - It ensures there is space in the `output_buffer` for the opening bracket '[' and increments the offset.
    - It iterates through each element in the array, calling [`print_value`](#print_value) to render each element into the output buffer.
    - After each element, if there is a next element, it adds a comma (and possibly a space) to separate the elements.
    - Finally, it ensures there is space for the closing bracket ']' and decrements the depth before returning true.
- **Output**: Returns true if the array was successfully printed; otherwise, returns false.
- **Functions called**:
    - [`ensure`](#ensure)
    - [`print_value`](#print_value)
    - [`update_offset`](#update_offset)


---
### parse\_object<!-- {{#callable:parse_object}} -->
Parses a JSON object from the input buffer and populates the provided cJSON item.
- **Inputs**:
    - `item`: A pointer to a `cJSON` structure where the parsed object will be stored.
    - `input_buffer`: A pointer to a `parse_buffer` structure that contains the JSON input data and its parsing state.
- **Control Flow**:
    - Checks if the current depth of parsing exceeds the nesting limit; if so, returns false.
    - Increments the depth of the input buffer.
    - Validates that the current character in the buffer is '{', indicating the start of an object.
    - Handles the case of an empty object by checking for '}'.
    - Enters a loop to parse key-value pairs separated by commas.
    - For each key-value pair, allocates a new `cJSON` item and attaches it to a linked list.
    - Parses the key as a string and the value using the [`parse_value`](#parse_value) function.
    - Checks for the presence of a closing '}' to ensure the object is properly terminated.
    - If parsing is successful, updates the item type to `cJSON_Object` and sets its child to the head of the linked list.
- **Output**: Returns true if the object was successfully parsed; otherwise, it returns false and cleans up any allocated memory.
- **Functions called**:
    - [`buffer_skip_whitespace`](#buffer_skip_whitespace)
    - [`cJSON_New_Item`](#cJSON_New_Item)
    - [`parse_string`](#parse_string)
    - [`parse_value`](#parse_value)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### print\_object<!-- {{#callable:print_object}} -->
The `print_object` function formats and outputs a `cJSON` object as a JSON string.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that represents the JSON object to be printed.
    - `output_buffer`: A pointer to a `printbuffer` structure that holds the output string and its properties.
- **Control Flow**:
    - The function first checks if the `output_buffer` is NULL and returns false if it is.
    - It initializes the output by allocating space for the opening brace '{' and possibly a newline character based on formatting options.
    - The function then iterates over each child item of the `cJSON` object.
    - For each child, it prints the key and value, ensuring proper formatting and indentation if specified.
    - After processing all child items, it appends the closing brace '}' to the output buffer.
- **Output**: Returns true if the object was successfully printed to the output buffer; otherwise, it returns false.
- **Functions called**:
    - [`ensure`](#ensure)
    - [`print_string_ptr`](#print_string_ptr)
    - [`update_offset`](#update_offset)
    - [`print_value`](#print_value)


---
### cJSON\_GetArraySize<!-- {{#callable:cJSON_GetArraySize}} -->
The `cJSON_GetArraySize` function returns the number of elements in a given JSON array.
- **Inputs**:
    - `array`: A pointer to a `cJSON` structure representing a JSON array.
- **Control Flow**:
    - The function first checks if the `array` pointer is NULL; if it is, it returns 0.
    - It initializes a `child` pointer to the first child of the array.
    - A while loop iterates through the linked list of child elements, incrementing a `size` counter for each child until there are no more children.
    - Finally, it returns the size as an integer.
- **Output**: The function outputs an integer representing the number of elements in the array, or 0 if the array is NULL.


---
### get\_array\_item<!-- {{#callable:get_array_item}} -->
Retrieves an item from a JSON array at a specified index.
- **Inputs**:
    - `array`: A pointer to a `cJSON` structure representing the JSON array from which to retrieve the item.
    - `index`: A size_t value representing the zero-based index of the item to retrieve from the array.
- **Control Flow**:
    - The function first checks if the `array` pointer is NULL; if it is, it returns NULL.
    - It initializes a pointer `current_child` to the first child of the array.
    - It enters a while loop that continues as long as `current_child` is not NULL and `index` is greater than 0.
    - Within the loop, it decrements `index` and moves `current_child` to the next sibling in the array.
    - Once the loop exits, it returns the `current_child`, which is either the item at the specified index or NULL if the index is out of bounds.
- **Output**: Returns a pointer to the `cJSON` item at the specified index in the array, or NULL if the index is out of bounds or the array is NULL.


---
### cJSON\_GetArrayItem<!-- {{#callable:cJSON_GetArrayItem}} -->
Retrieves an item from a JSON array at a specified index.
- **Inputs**:
    - `array`: A pointer to a `cJSON` object representing the JSON array from which to retrieve the item.
    - `index`: An integer representing the index of the item to retrieve from the array.
- **Control Flow**:
    - Checks if the provided `index` is less than 0; if so, returns NULL.
    - Calls the [`get_array_item`](#get_array_item) function with the `array` and the `index` cast to a size_t to retrieve the item.
- **Output**: Returns a pointer to the `cJSON` object at the specified index in the array, or NULL if the index is invalid.
- **Functions called**:
    - [`get_array_item`](#get_array_item)


---
### get\_object\_item<!-- {{#callable:get_object_item}} -->
Retrieves an item from a JSON object by its name, with an option for case sensitivity.
- **Inputs**:
    - `object`: A pointer to a `cJSON` object from which the item is to be retrieved.
    - `name`: A string representing the name of the item to retrieve.
    - `case_sensitive`: A boolean indicating whether the name comparison should be case sensitive.
- **Control Flow**:
    - The function first checks if either `object` or `name` is NULL, returning NULL if so.
    - It initializes `current_element` to the first child of the `object`.
    - If `case_sensitive` is true, it iterates through the children of the object, comparing `name` with each child's string using `strcmp`.
    - If `case_sensitive` is false, it uses [`case_insensitive_strcmp`](#case_insensitive_strcmp) for the comparison.
    - If a matching child is found, it is returned; otherwise, NULL is returned.
- **Output**: Returns a pointer to the `cJSON` item if found, or NULL if the item does not exist or if the input parameters are invalid.
- **Functions called**:
    - [`case_insensitive_strcmp`](#case_insensitive_strcmp)


---
### cJSON\_GetObjectItem<!-- {{#callable:cJSON_GetObjectItem}} -->
Retrieves an item from a JSON object by its key.
- **Inputs**:
    - `object`: A pointer to a `cJSON` object from which the item is to be retrieved.
    - `string`: A string representing the key of the item to retrieve.
- **Control Flow**:
    - Calls the [`get_object_item`](#get_object_item) function with the provided `object`, `string`, and a case sensitivity flag set to false.
    - The [`get_object_item`](#get_object_item) function iterates through the children of the `object` to find a match for the provided key.
- **Output**: Returns a pointer to the `cJSON` item associated with the specified key, or NULL if the key does not exist.
- **Functions called**:
    - [`get_object_item`](#get_object_item)


---
### cJSON\_GetObjectItemCaseSensitive<!-- {{#callable:cJSON_GetObjectItemCaseSensitive}} -->
Retrieves a cJSON object item by its name in a case-sensitive manner.
- **Inputs**:
    - `object`: A pointer to a `cJSON` object from which the item is to be retrieved.
    - `string`: A pointer to a string representing the name of the item to retrieve.
- **Control Flow**:
    - The function calls [`get_object_item`](#get_object_item) with the provided `object`, `string`, and a boolean value `true` to indicate case sensitivity.
    - The [`get_object_item`](#get_object_item) function iterates through the children of the `object`, comparing each child's string name to the provided `string` using case-sensitive comparison.
- **Output**: Returns a pointer to the `cJSON` item if found; otherwise, returns NULL.
- **Functions called**:
    - [`get_object_item`](#get_object_item)


---
### cJSON\_HasObjectItem<!-- {{#callable:cJSON_HasObjectItem}} -->
The `cJSON_HasObjectItem` function checks if a specified item exists in a JSON object.
- **Inputs**:
    - `object`: A pointer to a `cJSON` object that represents the JSON object to be checked.
    - `string`: A pointer to a string that represents the key of the item to check for in the JSON object.
- **Control Flow**:
    - The function calls [`cJSON_GetObjectItem`](#cJSON_GetObjectItem) with the provided `object` and `string` to retrieve the item associated with the key.
    - If the item exists (i.e., the return value of [`cJSON_GetObjectItem`](#cJSON_GetObjectItem) is not NULL), the function returns 1 (true).
    - If the item does not exist, the function returns 0 (false).
- **Output**: Returns 1 if the item exists in the object, otherwise returns 0.
- **Functions called**:
    - [`cJSON_GetObjectItem`](#cJSON_GetObjectItem)


---
### suffix\_object<!-- {{#callable:suffix_object}} -->
Links two `cJSON` objects in a doubly linked list.
- **Inputs**:
    - `prev`: A pointer to the previous `cJSON` object in the linked list.
    - `item`: A pointer to the current `cJSON` object to be linked.
- **Control Flow**:
    - Sets the `next` pointer of the `prev` object to point to the `item` object.
    - Sets the `prev` pointer of the `item` object to point back to the `prev` object.
- **Output**: This function does not return a value; it modifies the linked list structure of the `cJSON` objects.


---
### create\_reference<!-- {{#callable:create_reference}} -->
Creates a reference to a given `cJSON` item.
- **Inputs**:
    - `item`: A pointer to the `cJSON` item to be referenced.
    - `hooks`: A pointer to `internal_hooks` structure used for memory management.
- **Control Flow**:
    - Check if the `item` is NULL; if so, return NULL.
    - Allocate a new `cJSON` item using [`cJSON_New_Item`](#cJSON_New_Item).
    - If allocation fails, return NULL.
    - Copy the contents of the `item` into the new reference using `memcpy`.
    - Set the `string` field of the new reference to NULL.
    - Set the `type` field of the new reference to indicate it is a reference.
    - Set the `next` and `prev` pointers of the new reference to NULL.
    - Return the new reference.
- **Output**: Returns a pointer to the newly created reference `cJSON` item, or NULL if the input item is NULL or memory allocation fails.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)


---
### add\_item\_to\_array<!-- {{#callable:add_item_to_array}} -->
Adds an item to a `cJSON` array.
- **Inputs**:
    - `array`: A pointer to the `cJSON` array to which the item will be added.
    - `item`: A pointer to the `cJSON` item that is to be added to the array.
- **Control Flow**:
    - Checks if the `item` or `array` is NULL or if they are the same, returning false if any condition is met.
    - If the array is empty, the item is set as the first child of the array.
    - If the array is not empty, the item is appended to the end of the array using the [`suffix_object`](#suffix_object) function.
- **Output**: Returns true if the item was successfully added to the array; otherwise, returns false.
- **Functions called**:
    - [`suffix_object`](#suffix_object)


---
### cJSON\_AddItemToArray<!-- {{#callable:cJSON_AddItemToArray}} -->
Adds an item to a cJSON array.
- **Inputs**:
    - `array`: A pointer to the `cJSON` array to which the item will be added.
    - `item`: A pointer to the `cJSON` item that will be added to the array.
- **Control Flow**:
    - The function calls [`add_item_to_array`](#add_item_to_array) with the provided `array` and `item`.
    - The [`add_item_to_array`](#add_item_to_array) function handles the logic of adding the item to the array.
- **Output**: Returns a boolean indicating success (true) or failure (false) of the operation.
- **Functions called**:
    - [`add_item_to_array`](#add_item_to_array)


---
### cast\_away\_const<!-- {{#callable:cast_away_const}} -->
The `cast_away_const` function casts a pointer from a constant type to a non-constant type.
- **Inputs**:
    - `string`: A pointer to a constant void type that is to be cast away.
- **Control Flow**:
    - The function takes a single input parameter of type `const void*`.
    - It directly casts the input pointer to a `void*` type without any checks or modifications.
    - The casted pointer is then returned as the output.
- **Output**: Returns a pointer of type `void*`, which is the input pointer cast to a non-constant type.


---
### add\_item\_to\_object<!-- {{#callable:add_item_to_object}} -->
Adds an item to a JSON object with a specified key.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object to which the item will be added.
    - `string`: A constant character pointer representing the key under which the item will be stored.
    - `item`: A pointer to the `cJSON` item that is to be added to the object.
    - `hooks`: A pointer to `internal_hooks` structure for memory management.
    - `constant_key`: A boolean indicating whether the key is constant.
- **Control Flow**:
    - Check if any of the input parameters are NULL or if the object is the same as the item; if so, return false.
    - If `constant_key` is true, cast away the constness of the string and set the item type to include `cJSON_StringIsConst`.
    - If `constant_key` is false, duplicate the string and check for allocation failure.
    - If the item is not constant and has a string, deallocate the existing string.
    - Assign the new key and type to the item.
    - Call [`add_item_to_array`](#add_item_to_array) to add the item to the object.
- **Output**: Returns true if the item was successfully added to the object; otherwise, returns false.
- **Functions called**:
    - [`cast_away_const`](#cast_away_const)
    - [`cJSON_strdup`](#cJSON_strdup)
    - [`add_item_to_array`](#add_item_to_array)


---
### cJSON\_AddItemToObject<!-- {{#callable:cJSON_AddItemToObject}} -->
Adds an item to a JSON object with a specified key.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object to which the item will be added.
    - `string`: A string representing the key under which the item will be stored.
    - `item`: A pointer to the `cJSON` item that will be added to the object.
- **Control Flow**:
    - The function checks if the `object`, `string`, and `item` are not NULL and that they are not the same.
    - It calls the [`add_item_to_object`](#add_item_to_object) function, passing the `object`, `string`, `item`, global memory hooks, and a boolean indicating that the key is not constant.
    - The [`add_item_to_object`](#add_item_to_object) function handles the actual addition of the item to the object.
- **Output**: Returns a boolean value indicating whether the item was successfully added to the object.
- **Functions called**:
    - [`add_item_to_object`](#add_item_to_object)


---
### cJSON\_AddItemToObjectCS<!-- {{#callable:cJSON_AddItemToObjectCS}} -->
Adds an item to a cJSON object with a constant string key.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object to which the item will be added.
    - `string`: A constant string key for the item being added.
    - `item`: A pointer to the `cJSON` item that is to be added to the object.
- **Control Flow**:
    - Checks if the `object`, `string`, or `item` is NULL; if so, returns false.
    - If `constant_key` is true, casts the `string` to a non-const type and sets the item type to include `cJSON_StringIsConst`.
    - If `constant_key` is false, duplicates the `string` and assigns it to the item.
    - Deallocates the previous string of the item if it exists.
    - Sets the item's string to the new key and adds the item to the object using `add_item_to_array`.
- **Output**: Returns true if the item was successfully added; otherwise, returns false.
- **Functions called**:
    - [`add_item_to_object`](#add_item_to_object)


---
### cJSON\_AddItemReferenceToArray<!-- {{#callable:cJSON_AddItemReferenceToArray}} -->
Adds a reference to an item in a cJSON array.
- **Inputs**:
    - `array`: A pointer to a `cJSON` object representing the array to which the item will be added.
    - `item`: A pointer to a `cJSON` object representing the item to be added as a reference.
- **Control Flow**:
    - Check if the `array` pointer is NULL; if it is, return false.
    - Call [`create_reference`](#create_reference) to create a reference to the `item`.
    - Pass the created reference to [`add_item_to_array`](#add_item_to_array) to add it to the `array`.
- **Output**: Returns true if the item was successfully added to the array, otherwise returns false.
- **Functions called**:
    - [`add_item_to_array`](#add_item_to_array)
    - [`create_reference`](#create_reference)


---
### cJSON\_AddItemReferenceToObject<!-- {{#callable:cJSON_AddItemReferenceToObject}} -->
Adds a reference to a `cJSON` item in a `cJSON` object using a specified string key.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object to which the item will be added.
    - `string`: A string key that will be used to reference the item in the object.
    - `item`: A pointer to the `cJSON` item that is to be added as a reference.
- **Control Flow**:
    - Checks if the `object` or `string` is NULL; if so, returns false.
    - Calls [`create_reference`](#create_reference) to create a reference to the `item`.
    - Calls [`add_item_to_object`](#add_item_to_object) to add the created reference to the `object` using the provided `string` key.
- **Output**: Returns true if the item was successfully added, otherwise returns false.
- **Functions called**:
    - [`add_item_to_object`](#add_item_to_object)
    - [`create_reference`](#create_reference)


---
### cJSON\_AddNullToObject<!-- {{#callable:cJSON_AddNullToObject}} -->
Adds a null value to a specified key in a cJSON object.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object to which the null value will be added.
    - `name`: A string representing the key under which the null value will be stored.
- **Control Flow**:
    - Creates a new `cJSON` null item using `cJSON_CreateNull()`.
    - Attempts to add the null item to the specified object using `add_item_to_object()`.
    - If the addition is successful, returns the created null item.
    - If the addition fails, deletes the null item and returns NULL.
- **Output**: Returns a pointer to the newly created null item if successful, or NULL if the addition failed.
- **Functions called**:
    - [`cJSON_CreateNull`](#cJSON_CreateNull)
    - [`add_item_to_object`](#add_item_to_object)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### cJSON\_AddTrueToObject<!-- {{#callable:cJSON_AddTrueToObject}} -->
Adds a `true` value to a specified key in a `cJSON` object.
- **Inputs**:
    - `object`: A pointer to a `cJSON` object where the `true` value will be added.
    - `name`: A string representing the key under which the `true` value will be stored.
- **Control Flow**:
    - Creates a new `cJSON` item representing the boolean value `true` using `cJSON_CreateTrue()`.
    - Attempts to add the created `true` item to the specified `object` using the `add_item_to_object()` function.
    - If the addition is successful, the function returns the pointer to the `true_item`.
    - If the addition fails, the created `true_item` is deleted using `cJSON_Delete()`, and the function returns NULL.
- **Output**: Returns a pointer to the newly created `true` item if added successfully, otherwise returns NULL.
- **Functions called**:
    - [`cJSON_CreateTrue`](#cJSON_CreateTrue)
    - [`add_item_to_object`](#add_item_to_object)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### cJSON\_AddFalseToObject<!-- {{#callable:cJSON_AddFalseToObject}} -->
Adds a `false` value to a specified key in a `cJSON` object.
- **Inputs**:
    - `object`: A pointer to a `cJSON` object where the false value will be added.
    - `name`: A string representing the key under which the false value will be stored.
- **Control Flow**:
    - Creates a new `cJSON` item representing the boolean value `false` using `cJSON_CreateFalse()`.
    - Attempts to add the created false item to the specified object using `add_item_to_object()`.
    - If the addition is successful, returns the created false item.
    - If the addition fails, deletes the created false item and returns NULL.
- **Output**: Returns a pointer to the created `cJSON` false item if successful, or NULL if the addition fails.
- **Functions called**:
    - [`cJSON_CreateFalse`](#cJSON_CreateFalse)
    - [`add_item_to_object`](#add_item_to_object)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### cJSON\_AddBoolToObject<!-- {{#callable:cJSON_AddBoolToObject}} -->
Adds a boolean value to a cJSON object.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object to which the boolean will be added.
    - `name`: A string representing the key under which the boolean value will be stored.
    - `boolean`: The boolean value to be added to the object.
- **Control Flow**:
    - Creates a new `cJSON` item representing the boolean value using [`cJSON_CreateBool`](#cJSON_CreateBool).
    - Attempts to add the newly created boolean item to the specified object using [`add_item_to_object`](#add_item_to_object).
    - If the addition is successful, returns the created boolean item.
    - If the addition fails, deletes the boolean item and returns NULL.
- **Output**: Returns a pointer to the newly created boolean item if successful, or NULL if the addition fails.
- **Functions called**:
    - [`cJSON_CreateBool`](#cJSON_CreateBool)
    - [`add_item_to_object`](#add_item_to_object)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### cJSON\_AddNumberToObject<!-- {{#callable:cJSON_AddNumberToObject}} -->
Adds a number to a specified object in a cJSON structure.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object to which the number will be added.
    - `name`: A string representing the key under which the number will be stored.
    - `number`: The double value that will be added to the object.
- **Control Flow**:
    - Creates a new `cJSON` item representing the number using [`cJSON_CreateNumber`](#cJSON_CreateNumber).
    - Attempts to add the newly created number item to the specified object using [`add_item_to_object`](#add_item_to_object).
    - If the addition is successful, returns the created number item.
    - If the addition fails, deletes the created number item and returns NULL.
- **Output**: Returns a pointer to the created `cJSON` number item if successful, or NULL if the addition fails.
- **Functions called**:
    - [`cJSON_CreateNumber`](#cJSON_CreateNumber)
    - [`add_item_to_object`](#add_item_to_object)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### cJSON\_AddStringToObject<!-- {{#callable:cJSON_AddStringToObject}} -->
Adds a string to a JSON object.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object to which the string will be added.
    - `name`: A constant character pointer representing the name (key) for the string in the JSON object.
    - `string`: A constant character pointer representing the string value to be added.
- **Control Flow**:
    - Creates a new `cJSON` string item using [`cJSON_CreateString`](#cJSON_CreateString) with the provided `string`.
    - Checks if the item was successfully added to the `object` using [`add_item_to_object`](#add_item_to_object).
    - If the addition is successful, returns the created string item.
    - If the addition fails, deletes the created string item and returns NULL.
- **Output**: Returns a pointer to the created string item if successful, or NULL if the addition fails.
- **Functions called**:
    - [`cJSON_CreateString`](#cJSON_CreateString)
    - [`add_item_to_object`](#add_item_to_object)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### cJSON\_AddRawToObject<!-- {{#callable:cJSON_AddRawToObject}} -->
Adds a raw JSON string to a specified object in the cJSON structure.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object to which the raw string will be added.
    - `name`: A string representing the key under which the raw string will be stored in the object.
    - `raw`: A string containing the raw JSON data to be added.
- **Control Flow**:
    - Creates a new `cJSON` item from the raw string using [`cJSON_CreateRaw`](#cJSON_CreateRaw).
    - Attempts to add the newly created raw item to the specified object using [`add_item_to_object`](#add_item_to_object).
    - If the addition is successful, returns the created raw item.
    - If the addition fails, deletes the created raw item and returns NULL.
- **Output**: Returns a pointer to the created `cJSON` item if successful, or NULL if the addition fails.
- **Functions called**:
    - [`cJSON_CreateRaw`](#cJSON_CreateRaw)
    - [`add_item_to_object`](#add_item_to_object)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### cJSON\_AddObjectToObject<!-- {{#callable:cJSON_AddObjectToObject}} -->
Adds a new object to an existing JSON object.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object to which the new object will be added.
    - `name`: A string representing the key under which the new object will be stored.
- **Control Flow**:
    - Creates a new `cJSON` object using `cJSON_CreateObject()`.
    - Attempts to add the newly created object to the specified parent object using `add_item_to_object()`.
    - If the addition is successful, returns the newly created object.
    - If the addition fails, deletes the newly created object and returns NULL.
- **Output**: Returns a pointer to the newly added `cJSON` object if successful, or NULL if the addition fails.
- **Functions called**:
    - [`cJSON_CreateObject`](#cJSON_CreateObject)
    - [`add_item_to_object`](#add_item_to_object)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### cJSON\_AddArrayToObject<!-- {{#callable:cJSON_AddArrayToObject}} -->
Adds a new array to a JSON object with a specified name.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object to which the new array will be added.
    - `name`: A string representing the name under which the new array will be stored in the object.
- **Control Flow**:
    - Creates a new array using `cJSON_CreateArray()`.
    - Attempts to add the newly created array to the specified object using `add_item_to_object()`.
    - If the addition is successful, returns a pointer to the new array.
    - If the addition fails, deletes the created array and returns NULL.
- **Output**: Returns a pointer to the newly created array if successful, or NULL if the addition fails.
- **Functions called**:
    - [`cJSON_CreateArray`](#cJSON_CreateArray)
    - [`add_item_to_object`](#add_item_to_object)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### cJSON\_DetachItemViaPointer<!-- {{#callable:cJSON_DetachItemViaPointer}} -->
Detaches a specified `cJSON` item from its parent, updating the linked list accordingly.
- **Inputs**:
    - `parent`: A pointer to the `cJSON` object that serves as the parent of the item to be detached.
    - `item`: A pointer to the `cJSON` item that is to be detached from the parent.
- **Control Flow**:
    - Checks if either `parent` or `item` is NULL, or if `item` is not a child of `parent` and has no previous item; if any condition is true, returns NULL.
    - If `item` is not the first child of `parent`, updates the `next` pointer of `item`'s previous sibling to skip over `item`.
    - If `item` is not the last child, updates the `prev` pointer of `item`'s next sibling to skip over `item`.
    - If `item` is the first child, updates `parent`'s `child` pointer to point to `item`'s next sibling.
    - If `item` is the last child, updates the `prev` pointer of `parent`'s child to point to `item`'s previous sibling.
    - Sets `item`'s `prev` and `next` pointers to NULL to detach it completely from the list.
    - Returns the detached `item`.
- **Output**: Returns a pointer to the detached `cJSON` item, or NULL if the detachment was not successful.


---
### cJSON\_DetachItemFromArray<!-- {{#callable:cJSON_DetachItemFromArray}} -->
Detaches an item from a JSON array at a specified index.
- **Inputs**:
    - `array`: A pointer to the `cJSON` array from which an item will be detached.
    - `which`: An integer index specifying the position of the item to detach from the array.
- **Control Flow**:
    - The function first checks if the provided index `which` is less than 0; if so, it returns NULL.
    - If the index is valid, it retrieves the item at the specified index using the [`get_array_item`](#get_array_item) function.
    - The retrieved item is then passed to [`cJSON_DetachItemViaPointer`](#cJSON_DetachItemViaPointer) along with the original array to perform the detachment.
- **Output**: Returns a pointer to the detached `cJSON` item if successful, or NULL if the index was invalid.
- **Functions called**:
    - [`cJSON_DetachItemViaPointer`](#cJSON_DetachItemViaPointer)
    - [`get_array_item`](#get_array_item)


---
### cJSON\_DeleteItemFromArray<!-- {{#callable:cJSON_DeleteItemFromArray}} -->
Deletes an item from a `cJSON` array at a specified index.
- **Inputs**:
    - `array`: A pointer to the `cJSON` array from which an item will be deleted.
    - `which`: An integer index specifying the position of the item to be deleted from the array.
- **Control Flow**:
    - Calls [`cJSON_DetachItemFromArray`](#cJSON_DetachItemFromArray) to detach the item at the specified index from the array.
    - Passes the detached item to [`cJSON_Delete`](#cJSON_Delete) to free its memory.
- **Output**: This function does not return a value; it modifies the array by removing the specified item.
- **Functions called**:
    - [`cJSON_Delete`](#cJSON_Delete)
    - [`cJSON_DetachItemFromArray`](#cJSON_DetachItemFromArray)


---
### cJSON\_DetachItemFromObject<!-- {{#callable:cJSON_DetachItemFromObject}} -->
Detaches an item from a JSON object using its string key.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object from which an item is to be detached.
    - `string`: A string representing the key of the item to be detached from the object.
- **Control Flow**:
    - Calls [`cJSON_GetObjectItem`](#cJSON_GetObjectItem) to retrieve the item associated with the provided key from the object.
    - If the item is found, it calls [`cJSON_DetachItemViaPointer`](#cJSON_DetachItemViaPointer) to detach the item from the object.
    - Returns the detached item or NULL if the item was not found.
- **Output**: Returns a pointer to the detached `cJSON` item, or NULL if the item does not exist.
- **Functions called**:
    - [`cJSON_GetObjectItem`](#cJSON_GetObjectItem)
    - [`cJSON_DetachItemViaPointer`](#cJSON_DetachItemViaPointer)


---
### cJSON\_DetachItemFromObjectCaseSensitive<!-- {{#callable:cJSON_DetachItemFromObjectCaseSensitive}} -->
Detaches an item from a JSON object using a case-sensitive key.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object from which an item is to be detached.
    - `string`: A pointer to a string representing the key of the item to be detached.
- **Control Flow**:
    - Calls [`cJSON_GetObjectItemCaseSensitive`](#cJSON_GetObjectItemCaseSensitive) to retrieve the item associated with the provided key from the object.
    - If the item is found, it calls [`cJSON_DetachItemViaPointer`](#cJSON_DetachItemViaPointer) to detach the item from the object and return it.
- **Output**: Returns a pointer to the detached `cJSON` item if found; otherwise, returns NULL.
- **Functions called**:
    - [`cJSON_GetObjectItemCaseSensitive`](#cJSON_GetObjectItemCaseSensitive)
    - [`cJSON_DetachItemViaPointer`](#cJSON_DetachItemViaPointer)


---
### cJSON\_DeleteItemFromObject<!-- {{#callable:cJSON_DeleteItemFromObject}} -->
Deletes an item from a `cJSON` object based on the specified key.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object from which the item will be deleted.
    - `string`: A pointer to a string representing the key of the item to be deleted.
- **Control Flow**:
    - Calls [`cJSON_DetachItemFromObject`](#cJSON_DetachItemFromObject) to detach the item associated with the specified key from the object.
    - Passes the detached item to [`cJSON_Delete`](#cJSON_Delete) to free its memory.
- **Output**: This function does not return a value; it performs the deletion operation directly on the provided `cJSON` object.
- **Functions called**:
    - [`cJSON_Delete`](#cJSON_Delete)
    - [`cJSON_DetachItemFromObject`](#cJSON_DetachItemFromObject)


---
### cJSON\_DeleteItemFromObjectCaseSensitive<!-- {{#callable:cJSON_DeleteItemFromObjectCaseSensitive}} -->
Deletes an item from a cJSON object in a case-sensitive manner.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object from which the item will be deleted.
    - `string`: A pointer to the string key of the item to be deleted.
- **Control Flow**:
    - Calls [`cJSON_DetachItemFromObjectCaseSensitive`](#cJSON_DetachItemFromObjectCaseSensitive) to detach the item associated with the provided key from the object.
    - Passes the detached item to [`cJSON_Delete`](#cJSON_Delete) to free its memory.
- **Output**: This function does not return a value; it modifies the `cJSON` object by removing the specified item.
- **Functions called**:
    - [`cJSON_Delete`](#cJSON_Delete)
    - [`cJSON_DetachItemFromObjectCaseSensitive`](#cJSON_DetachItemFromObjectCaseSensitive)


---
### cJSON\_InsertItemInArray<!-- {{#callable:cJSON_InsertItemInArray}} -->
Inserts a new item into a specified position in a cJSON array.
- **Inputs**:
    - `array`: A pointer to the `cJSON` array where the new item will be inserted.
    - `which`: An integer index specifying the position in the array to insert the new item.
    - `newitem`: A pointer to the `cJSON` item that is to be inserted into the array.
- **Control Flow**:
    - Check if the index 'which' is negative or if 'newitem' is NULL; if so, return false.
    - Retrieve the item currently at the specified index using [`get_array_item`](#get_array_item).
    - If the item at the specified index is NULL, add the new item to the end of the array using [`add_item_to_array`](#add_item_to_array).
    - Check if the item at the specified index is not the first child and if its previous pointer is NULL; if so, return false.
    - Set the next and previous pointers of 'newitem' to insert it before the item at the specified index.
    - If the item at the specified index is the first child, update the array's child pointer to 'newitem'.
    - Otherwise, update the previous item's next pointer to point to 'newitem'.
    - Return true to indicate successful insertion.
- **Output**: Returns true if the insertion was successful, otherwise returns false.
- **Functions called**:
    - [`get_array_item`](#get_array_item)
    - [`add_item_to_array`](#add_item_to_array)


---
### cJSON\_ReplaceItemViaPointer<!-- {{#callable:cJSON_ReplaceItemViaPointer}} -->
Replaces an item in a cJSON structure with a new item.
- **Inputs**:
    - `parent`: A pointer to the `cJSON` structure that contains the item to be replaced.
    - `item`: A pointer to the `cJSON` item that is to be replaced.
    - `replacement`: A pointer to the `cJSON` item that will replace the existing item.
- **Control Flow**:
    - Checks if any of the input pointers (`parent`, `item`, or `replacement`) are NULL; if so, returns false.
    - If the `replacement` is the same as `item`, it returns true immediately.
    - Updates the `next` and `prev` pointers of the `replacement` item to maintain the linked list structure.
    - If the `item` being replaced is the first child of `parent`, updates the `parent->child` pointer to point to `replacement`.
    - If the `item` is not the first child, adjusts the `next` and `prev` pointers of surrounding items accordingly.
    - Finally, it deletes the `item` and returns true.
- **Output**: Returns true if the replacement was successful, otherwise returns false.
- **Functions called**:
    - [`cJSON_Delete`](#cJSON_Delete)


---
### cJSON\_ReplaceItemInArray<!-- {{#callable:cJSON_ReplaceItemInArray}} -->
Replaces an item in a `cJSON` array at a specified index with a new item.
- **Inputs**:
    - `array`: A pointer to the `cJSON` array in which the item will be replaced.
    - `which`: An integer index specifying the position of the item to be replaced.
    - `newitem`: A pointer to the new `cJSON` item that will replace the existing item.
- **Control Flow**:
    - Checks if the index `which` is less than 0; if so, returns false.
    - Calls [`cJSON_ReplaceItemViaPointer`](#cJSON_ReplaceItemViaPointer) to replace the item at the specified index with the new item.
- **Output**: Returns true if the replacement was successful, otherwise returns false.
- **Functions called**:
    - [`cJSON_ReplaceItemViaPointer`](#cJSON_ReplaceItemViaPointer)
    - [`get_array_item`](#get_array_item)


---
### replace\_item\_in\_object<!-- {{#callable:replace_item_in_object}} -->
Replaces an item in a JSON object with a new item.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object that contains the item to be replaced.
    - `string`: A string representing the key of the item to be replaced.
    - `replacement`: A pointer to the `cJSON` item that will replace the existing item.
    - `case_sensitive`: A boolean indicating whether the key comparison should be case-sensitive.
- **Control Flow**:
    - Checks if the `replacement` or `string` is NULL; if so, returns false.
    - If the `replacement` item is not constant and has a string, it frees the existing string.
    - Duplicates the `string` into the `replacement` item.
    - If the duplication fails, returns false.
    - Clears the constant flag from the `replacement` type.
    - Calls [`cJSON_ReplaceItemViaPointer`](#cJSON_ReplaceItemViaPointer) to replace the item in the object.
- **Output**: Returns true if the replacement was successful, otherwise false.
- **Functions called**:
    - [`cJSON_free`](#cJSON_free)
    - [`cJSON_strdup`](#cJSON_strdup)
    - [`cJSON_ReplaceItemViaPointer`](#cJSON_ReplaceItemViaPointer)
    - [`get_object_item`](#get_object_item)


---
### cJSON\_ReplaceItemInObject<!-- {{#callable:cJSON_ReplaceItemInObject}} -->
Replaces an existing item in a JSON object with a new item.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object that contains the item to be replaced.
    - `string`: A string representing the key of the item to be replaced in the object.
    - `newitem`: A pointer to the new `cJSON` item that will replace the existing item.
- **Control Flow**:
    - The function calls [`replace_item_in_object`](#replace_item_in_object) with the provided parameters and a case sensitivity flag set to false.
    - The [`replace_item_in_object`](#replace_item_in_object) function handles the actual replacement logic, including memory management for the item being replaced.
- **Output**: Returns a boolean value indicating whether the replacement was successful.
- **Functions called**:
    - [`replace_item_in_object`](#replace_item_in_object)


---
### cJSON\_ReplaceItemInObjectCaseSensitive<!-- {{#callable:cJSON_ReplaceItemInObjectCaseSensitive}} -->
Replaces an item in a JSON object with a new item, considering case sensitivity.
- **Inputs**:
    - `object`: A pointer to the `cJSON` object that contains the item to be replaced.
    - `string`: A string representing the key of the item to be replaced in the object.
    - `newitem`: A pointer to the new `cJSON` item that will replace the existing item.
- **Control Flow**:
    - The function first checks if the `newitem` and `string` are not NULL.
    - It then calls the [`replace_item_in_object`](#replace_item_in_object) function with the `object`, `string`, `newitem`, and a boolean value `true` to indicate case sensitivity.
    - The result of the replacement operation is returned.
- **Output**: Returns a boolean value indicating whether the replacement was successful.
- **Functions called**:
    - [`replace_item_in_object`](#replace_item_in_object)


---
### cJSON\_CreateNull<!-- {{#callable:cJSON_CreateNull}} -->
Creates a new `cJSON` item representing a null value.
- **Inputs**: None
- **Control Flow**:
    - Calls [`cJSON_New_Item`](#cJSON_New_Item) to allocate and initialize a new `cJSON` item.
    - If the item is successfully created, it sets the item's type to `cJSON_NULL`.
    - Returns the newly created item.
- **Output**: Returns a pointer to the newly created `cJSON` item representing null, or NULL if the allocation fails.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)


---
### cJSON\_CreateTrue<!-- {{#callable:cJSON_CreateTrue}} -->
Creates a new `cJSON` item representing a JSON true value.
- **Inputs**: None
- **Control Flow**:
    - Calls [`cJSON_New_Item`](#cJSON_New_Item) to allocate and initialize a new `cJSON` item.
    - If the item is successfully created, sets its type to `cJSON_True`.
    - Returns the created item.
- **Output**: Returns a pointer to the newly created `cJSON` item representing true, or NULL if the allocation fails.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)


---
### cJSON\_CreateFalse<!-- {{#callable:cJSON_CreateFalse}} -->
Creates a new `cJSON` item representing a JSON false value.
- **Inputs**: None
- **Control Flow**:
    - Calls [`cJSON_New_Item`](#cJSON_New_Item) to allocate and initialize a new `cJSON` item.
    - If the item is successfully created, it sets the item's type to `cJSON_False`.
    - Returns the created item, which may be NULL if allocation failed.
- **Output**: Returns a pointer to the newly created `cJSON` item representing false, or NULL if the allocation failed.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)


---
### cJSON\_CreateBool<!-- {{#callable:cJSON_CreateBool}} -->
Creates a new `cJSON` item representing a boolean value.
- **Inputs**:
    - `boolean`: A `cJSON_bool` value indicating the boolean state to be represented (true or false).
- **Control Flow**:
    - Calls [`cJSON_New_Item`](#cJSON_New_Item) to allocate a new `cJSON` item.
    - If the item is successfully created, it sets the item's type to `cJSON_True` if the boolean is true, or `cJSON_False` if it is false.
    - Returns the created item.
- **Output**: Returns a pointer to the newly created `cJSON` item representing the boolean value, or NULL if the item could not be created.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)


---
### cJSON\_CreateNumber<!-- {{#callable:cJSON_CreateNumber}} -->
Creates a new `cJSON` item representing a number.
- **Inputs**:
    - `num`: A double precision floating-point number to be represented as a `cJSON` number.
- **Control Flow**:
    - Calls [`cJSON_New_Item`](#cJSON_New_Item) to allocate a new `cJSON` item.
    - If the item is successfully created, sets its type to `cJSON_Number`.
    - Assigns the input number to the `valuedouble` field of the item.
    - Checks for overflow conditions and assigns the appropriate value to `valueint` based on the input number.
    - Returns the created `cJSON` item.
- **Output**: Returns a pointer to the newly created `cJSON` item representing the number, or NULL if the item could not be created.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)


---
### cJSON\_CreateString<!-- {{#callable:cJSON_CreateString}} -->
Creates a new `cJSON` string item from a given C string.
- **Inputs**:
    - `string`: A pointer to a null-terminated C string that will be used as the value of the new `cJSON` string item.
- **Control Flow**:
    - Calls [`cJSON_New_Item`](#cJSON_New_Item) to allocate and initialize a new `cJSON` item.
    - If the item is successfully created, it sets the item's type to `cJSON_String`.
    - Attempts to duplicate the input string using [`cJSON_strdup`](#cJSON_strdup) and assigns it to the item's `valuestring` field.
    - If string duplication fails, it deletes the item and returns NULL.
    - Finally, returns the created `cJSON` string item.
- **Output**: Returns a pointer to the newly created `cJSON` string item, or NULL if the creation or string duplication fails.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)
    - [`cJSON_strdup`](#cJSON_strdup)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### cJSON\_CreateStringReference<!-- {{#callable:cJSON_CreateStringReference}} -->
Creates a `cJSON` string reference from a given string.
- **Inputs**:
    - `string`: A pointer to a constant character string that will be referenced by the created `cJSON` object.
- **Control Flow**:
    - Calls [`cJSON_New_Item`](#cJSON_New_Item) to allocate a new `cJSON` item.
    - If the item is successfully created, it sets the type of the item to `cJSON_String | cJSON_IsReference`.
    - The `valuestring` of the item is set to the input string after casting away its const qualifier.
    - Returns the created `cJSON` item, or NULL if the item could not be created.
- **Output**: Returns a pointer to the newly created `cJSON` item that references the input string, or NULL if the creation failed.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)
    - [`cast_away_const`](#cast_away_const)


---
### cJSON\_CreateObjectReference<!-- {{#callable:cJSON_CreateObjectReference}} -->
Creates a new `cJSON` object that references an existing `cJSON` object.
- **Inputs**:
    - `child`: A pointer to a constant `cJSON` object that will be referenced by the new object.
- **Control Flow**:
    - Calls [`cJSON_New_Item`](#cJSON_New_Item) to allocate a new `cJSON` object.
    - If allocation is successful, sets the type of the new object to `cJSON_Object | cJSON_IsReference`.
    - Assigns the `child` parameter to the `child` field of the new object after casting away its const qualifier.
    - Returns the newly created object.
- **Output**: Returns a pointer to the newly created `cJSON` object that references the provided `child` object, or NULL if allocation fails.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)
    - [`cast_away_const`](#cast_away_const)


---
### cJSON\_CreateArrayReference<!-- {{#callable:cJSON_CreateArrayReference}} -->
Creates a reference to an existing cJSON array.
- **Inputs**:
    - `child`: A pointer to a constant `cJSON` object that represents the array to be referenced.
- **Control Flow**:
    - Calls [`cJSON_New_Item`](#cJSON_New_Item) to allocate a new `cJSON` item.
    - If the allocation is successful, sets the type of the item to `cJSON_Array` and marks it as a reference.
    - Assigns the `child` parameter to the `child` field of the new item after casting away its const qualifier.
    - Returns the newly created item.
- **Output**: Returns a pointer to the newly created `cJSON` item that references the provided array, or NULL if the allocation fails.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)
    - [`cast_away_const`](#cast_away_const)


---
### cJSON\_CreateRaw<!-- {{#callable:cJSON_CreateRaw}} -->
Creates a new `cJSON` item of type `cJSON_Raw` from a raw string.
- **Inputs**:
    - `raw`: A pointer to a null-terminated string that represents the raw JSON data.
- **Control Flow**:
    - Calls [`cJSON_New_Item`](#cJSON_New_Item) to allocate and initialize a new `cJSON` item.
    - If the item is successfully created, sets its type to `cJSON_Raw`.
    - Uses [`cJSON_strdup`](#cJSON_strdup) to duplicate the input string and assign it to the `valuestring` field of the item.
    - If string duplication fails, deletes the item and returns NULL.
    - Finally, returns the created `cJSON` item.
- **Output**: Returns a pointer to the newly created `cJSON` item if successful, or NULL if an error occurs during creation or string duplication.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)
    - [`cJSON_strdup`](#cJSON_strdup)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### cJSON\_CreateArray<!-- {{#callable:cJSON_CreateArray}} -->
Creates a new `cJSON` array object.
- **Inputs**: None
- **Control Flow**:
    - Calls [`cJSON_New_Item`](#cJSON_New_Item) to allocate and initialize a new `cJSON` item.
    - If the item is successfully created, sets its type to `cJSON_Array`.
    - Returns the newly created item.
- **Output**: Returns a pointer to the newly created `cJSON` array object, or NULL if the allocation fails.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)


---
### cJSON\_CreateObject<!-- {{#callable:cJSON_CreateObject}} -->
Creates a new `cJSON` object.
- **Inputs**: None
- **Control Flow**:
    - Calls [`cJSON_New_Item`](#cJSON_New_Item) to allocate and initialize a new `cJSON` item.
    - If the item is successfully created, it sets the item's type to `cJSON_Object`.
    - Returns the newly created item.
- **Output**: Returns a pointer to the newly created `cJSON` object, or NULL if the creation failed.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)


---
### cJSON\_CreateIntArray<!-- {{#callable:cJSON_CreateIntArray}} -->
Creates a `cJSON` array from a given array of integers.
- **Inputs**:
    - `numbers`: A pointer to an array of integers that will be converted into a JSON array.
    - `count`: The number of integers in the `numbers` array.
- **Control Flow**:
    - Checks if the `count` is negative or if `numbers` is NULL; if so, returns NULL.
    - Creates a new `cJSON` array using `cJSON_CreateArray()`.
    - Iterates over the `numbers` array up to `count`, creating a `cJSON` number for each integer.
    - If the first number is added, it sets it as the first child of the array.
    - For subsequent numbers, it links them to the previous number using the `suffix_object()` function.
    - If any number creation fails, it deletes the created array and returns NULL.
    - Finally, it returns the created `cJSON` array.
- **Output**: Returns a pointer to a `cJSON` array containing the integers from the input array, or NULL if an error occurs.
- **Functions called**:
    - [`cJSON_CreateArray`](#cJSON_CreateArray)
    - [`cJSON_CreateNumber`](#cJSON_CreateNumber)
    - [`cJSON_Delete`](#cJSON_Delete)
    - [`suffix_object`](#suffix_object)


---
### cJSON\_CreateFloatArray<!-- {{#callable:cJSON_CreateFloatArray}} -->
Creates a `cJSON` array from an array of floats.
- **Inputs**:
    - `numbers`: A pointer to an array of `float` values.
    - `count`: The number of elements in the `numbers` array.
- **Control Flow**:
    - Checks if `count` is negative or if `numbers` is NULL; if so, returns NULL.
    - Creates a new `cJSON` array using `cJSON_CreateArray()`.
    - Iterates over the `numbers` array up to `count` times.
    - For each float in `numbers`, creates a `cJSON` number using `cJSON_CreateNumber()`.
    - If the creation of a number fails, deletes the array and returns NULL.
    - Links the created numbers into the array as children nodes.
    - Sets the previous pointer of the last child to the last created number.
- **Output**: Returns a pointer to the newly created `cJSON` array containing the float values, or NULL if an error occurs.
- **Functions called**:
    - [`cJSON_CreateArray`](#cJSON_CreateArray)
    - [`cJSON_CreateNumber`](#cJSON_CreateNumber)
    - [`cJSON_Delete`](#cJSON_Delete)
    - [`suffix_object`](#suffix_object)


---
### cJSON\_CreateDoubleArray<!-- {{#callable:cJSON_CreateDoubleArray}} -->
Creates a `cJSON` array from an array of double values.
- **Inputs**:
    - `numbers`: A pointer to an array of double values.
    - `count`: The number of elements in the `numbers` array.
- **Control Flow**:
    - Checks if `count` is negative or if `numbers` is NULL; if so, returns NULL.
    - Creates a new `cJSON` array using `cJSON_CreateArray()`.
    - Iterates over the `numbers` array, creating a `cJSON` number for each element.
    - If the creation of a number fails, deletes the array and returns NULL.
    - Links each newly created number to the previous one in the array.
    - Sets the `prev` pointer of the first child to the last created number.
- **Output**: Returns a pointer to the newly created `cJSON` array, or NULL if an error occurred.
- **Functions called**:
    - [`cJSON_CreateArray`](#cJSON_CreateArray)
    - [`cJSON_CreateNumber`](#cJSON_CreateNumber)
    - [`cJSON_Delete`](#cJSON_Delete)
    - [`suffix_object`](#suffix_object)


---
### cJSON\_CreateStringArray<!-- {{#callable:cJSON_CreateStringArray}} -->
Creates a cJSON array from an array of strings.
- **Inputs**:
    - `strings`: A pointer to an array of string literals.
    - `count`: The number of strings in the array.
- **Control Flow**:
    - Checks if the count is negative or if the strings pointer is NULL, returning NULL if true.
    - Creates a new cJSON array using `cJSON_CreateArray()`.
    - Iterates over the input strings up to the specified count.
    - For each string, creates a cJSON string item using `cJSON_CreateString()`.
    - If the creation of a string item fails, deletes the array and returns NULL.
    - Links the created string items into the array.
    - Sets the previous pointer of the first child to the last created string item.
- **Output**: Returns a pointer to the newly created cJSON array containing the strings, or NULL on failure.
- **Functions called**:
    - [`cJSON_CreateArray`](#cJSON_CreateArray)
    - [`cJSON_CreateString`](#cJSON_CreateString)
    - [`cJSON_Delete`](#cJSON_Delete)
    - [`suffix_object`](#suffix_object)


---
### cJSON\_Duplicate<!-- {{#callable:cJSON_Duplicate}} -->
Duplicates a `cJSON` item, optionally recursively.
- **Inputs**:
    - `item`: A pointer to the `cJSON` item to be duplicated.
    - `recurse`: A boolean flag indicating whether to duplicate child items recursively.
- **Control Flow**:
    - Calls the [`cJSON_Duplicate_rec`](#cJSON_Duplicate_rec) function with the provided `item`, a depth of 0, and the `recurse` flag.
    - The [`cJSON_Duplicate_rec`](#cJSON_Duplicate_rec) function handles the actual duplication process, including copying the item and its children if `recurse` is true.
- **Output**: Returns a pointer to the newly duplicated `cJSON` item, or NULL if the duplication fails.
- **Functions called**:
    - [`cJSON_Duplicate_rec`](#cJSON_Duplicate_rec)


---
### cJSON\_Duplicate\_rec<!-- {{#callable:cJSON_Duplicate_rec}} -->
Duplicates a `cJSON` item recursively up to a specified depth.
- **Inputs**:
    - `item`: A pointer to the `cJSON` item to be duplicated.
    - `depth`: The current depth of recursion, used to prevent circular references.
    - `recurse`: A boolean flag indicating whether to duplicate child items recursively.
- **Control Flow**:
    - Check if the input `item` is NULL; if so, jump to the fail label.
    - Allocate a new `cJSON` item and check for allocation failure.
    - Copy the type, integer value, double value, and string values from the original item to the new item.
    - If `recurse` is false, return the newly created item.
    - If `recurse` is true, iterate through the child items of the original item.
    - For each child, check for circular reference by comparing the current depth with `CJSON_CIRCULAR_LIMIT`.
    - Recursively call [`cJSON_Duplicate_rec`](#cJSON_Duplicate_rec) for each child item and link them to the new item.
    - Return the newly created item if all operations succeed; otherwise, handle failures by deleting the new item.
- **Output**: Returns a pointer to the newly duplicated `cJSON` item, or NULL if duplication fails.
- **Functions called**:
    - [`cJSON_New_Item`](#cJSON_New_Item)
    - [`cJSON_strdup`](#cJSON_strdup)
    - [`cJSON_Duplicate_rec`](#cJSON_Duplicate_rec)
    - [`cJSON_Delete`](#cJSON_Delete)


---
### skip\_oneline\_comment<!-- {{#callable:skip_oneline_comment}} -->
The `skip_oneline_comment` function advances a given input pointer past a single-line comment in the format '//'.
- **Inputs**:
    - `input`: A pointer to a character pointer that points to the current position in the input string.
- **Control Flow**:
    - The function first increments the `input` pointer by the length of the string '//'.
    - It then enters a loop that continues until the end of the string is reached.
    - Within the loop, it checks each character; if a newline character ('\n') is encountered, it increments the `input` pointer by the length of the newline and exits the function.
- **Output**: The function does not return a value; it modifies the input pointer to skip over the comment.


---
### skip\_multiline\_comment<!-- {{#callable:skip_multiline_comment}} -->
The `skip_multiline_comment` function advances a pointer past a multiline comment in a C-style syntax.
- **Inputs**:
    - `input`: A pointer to a pointer to a character string, which represents the input text being processed.
- **Control Flow**:
    - The function first increments the `input` pointer to skip the initial '/*' of the comment.
    - It enters a loop that continues until the end of the string is reached.
    - Within the loop, it checks for the closing '*/' sequence.
    - If the closing sequence is found, it increments the `input` pointer to skip past it and returns from the function.
- **Output**: The function does not return a value; it modifies the input pointer to point to the character immediately following the end of the multiline comment.


---
### minify\_string<!-- {{#callable:minify_string}} -->
The `minify_string` function processes a JSON string to remove unnecessary whitespace and escape sequences.
- **Inputs**:
    - `input`: A pointer to a pointer to a character array representing the input JSON string.
    - `output`: A pointer to a pointer to a character array where the minified output will be stored.
- **Control Flow**:
    - The function initializes the first character of the output with the first character of the input.
    - It then advances both input and output pointers past the initial double quote character.
    - A loop iterates through the input string until a null terminator is encountered.
    - Within the loop, it copies characters from input to output, checking for double quotes and escape sequences.
    - If a double quote is encountered, it copies it to the output and terminates the function.
    - If an escape sequence for a double quote is found, it copies the next character to the output.
- **Output**: The function does not return a value but modifies the output pointer to contain the minified JSON string.


---
### cJSON\_Minify<!-- {{#callable:cJSON_Minify}} -->
The `cJSON_Minify` function removes whitespace and comments from a JSON string.
- **Inputs**:
    - `json`: A pointer to a null-terminated string containing the JSON data to be minified.
- **Control Flow**:
    - Check if the input `json` pointer is NULL; if so, return immediately.
    - Iterate through each character in the `json` string until the null terminator is reached.
    - For each character, determine its type: whitespace, comment, string, or other.
    - If the character is whitespace (space, tab, carriage return, or newline), skip it.
    - If the character is a '/', check if it starts a single-line or multi-line comment and skip the entire comment.
    - If the character is a double quote ('"'), call [`minify_string`](#minify_string) to handle the string content.
    - For any other character, copy it to the output position and advance the output pointer.
    - Finally, null-terminate the resulting minified string.
- **Output**: The function modifies the input string in place, resulting in a minified version of the JSON without whitespace and comments.
- **Functions called**:
    - [`skip_oneline_comment`](#skip_oneline_comment)
    - [`skip_multiline_comment`](#skip_multiline_comment)
    - [`minify_string`](#minify_string)


---
### cJSON\_IsInvalid<!-- {{#callable:cJSON_IsInvalid}} -->
The `cJSON_IsInvalid` function checks if a given `cJSON` item is of type invalid.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that is to be checked for its type.
- **Control Flow**:
    - The function first checks if the `item` pointer is NULL; if it is, the function returns false.
    - If the `item` is not NULL, it checks if the type of the item is equal to `cJSON_Invalid` by performing a bitwise AND operation with 0xFF.
    - The result of the type check is returned as a boolean value.
- **Output**: Returns true if the item is of type invalid, otherwise returns false.


---
### cJSON\_IsFalse<!-- {{#callable:cJSON_IsFalse}} -->
The `cJSON_IsFalse` function checks if a given `cJSON` item represents a JSON false value.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that is to be checked.
- **Control Flow**:
    - The function first checks if the `item` pointer is NULL; if it is, it returns false.
    - If `item` is not NULL, it checks if the type of the item is equal to `cJSON_False` by performing a bitwise AND operation with 0xFF.
- **Output**: Returns true if the item is of type `cJSON_False`, otherwise returns false.


---
### cJSON\_IsTrue<!-- {{#callable:cJSON_IsTrue}} -->
The `cJSON_IsTrue` function checks if a given `cJSON` item represents a true value.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that is to be checked for its type.
- **Control Flow**:
    - The function first checks if the `item` pointer is NULL; if it is, it returns false.
    - If the `item` is not NULL, it checks if the type of the item matches `cJSON_True` by performing a bitwise AND operation with 0xff.
    - The result of the comparison is returned as a boolean value.
- **Output**: Returns true if the `item` is of type `cJSON_True`, otherwise returns false.


---
### cJSON\_IsBool<!-- {{#callable:cJSON_IsBool}} -->
Determines if a given `cJSON` item is of boolean type.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that is to be checked for boolean type.
- **Control Flow**:
    - The function first checks if the `item` pointer is NULL; if it is, the function returns false.
    - If the `item` is not NULL, it checks if the `type` of the item is either `cJSON_True` or `cJSON_False` by performing a bitwise AND operation.
    - The function returns true if the item is of boolean type, otherwise it returns false.
- **Output**: Returns a boolean value indicating whether the `item` is a boolean type (true or false).


---
### cJSON\_IsNull<!-- {{#callable:cJSON_IsNull}} -->
The `cJSON_IsNull` function checks if a given `cJSON` item is of type null.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that is to be checked for null type.
- **Control Flow**:
    - The function first checks if the `item` pointer is NULL; if it is, the function returns false.
    - If the `item` is not NULL, it checks if the type of the item is equal to `cJSON_NULL` by performing a bitwise AND operation with 0xFF.
    - The result of the type check is returned as a boolean value.
- **Output**: Returns true if the `item` is of type null, otherwise returns false.


---
### cJSON\_IsNumber<!-- {{#callable:cJSON_IsNumber}} -->
The `cJSON_IsNumber` function checks if a given `cJSON` item is of type number.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that is to be checked for its type.
- **Control Flow**:
    - The function first checks if the `item` pointer is NULL; if it is, the function returns false.
    - If the `item` is not NULL, it checks if the type of the item matches `cJSON_Number` by performing a bitwise AND operation with 0xFF.
    - The result of the type check is returned as a boolean value.
- **Output**: Returns true if the item is of type number, otherwise returns false.


---
### cJSON\_IsString<!-- {{#callable:cJSON_IsString}} -->
The `cJSON_IsString` function checks if a given `cJSON` item is of type string.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that is to be checked for its type.
- **Control Flow**:
    - The function first checks if the `item` pointer is NULL; if it is, it returns false.
    - If `item` is not NULL, it checks if the type of the item matches `cJSON_String` by performing a bitwise AND operation with 0xFF.
    - The result of the comparison is returned as a boolean value.
- **Output**: Returns true if the item is a string, otherwise returns false.


---
### cJSON\_IsArray<!-- {{#callable:cJSON_IsArray}} -->
The `cJSON_IsArray` function checks if a given `cJSON` item is of type array.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that is to be checked if it is an array.
- **Control Flow**:
    - The function first checks if the `item` pointer is NULL; if it is, the function returns false.
    - If `item` is not NULL, it checks if the type of `item` is equal to `cJSON_Array` by performing a bitwise AND operation with 0xFF.
    - The result of the type check is returned as a boolean value.
- **Output**: Returns true if the `item` is an array, otherwise returns false.


---
### cJSON\_IsObject<!-- {{#callable:cJSON_IsObject}} -->
The `cJSON_IsObject` function checks if a given `cJSON` item is of type object.
- **Inputs**:
    - `item`: A pointer to a `cJSON` structure that represents the item to be checked.
- **Control Flow**:
    - The function first checks if the `item` pointer is NULL; if it is, the function returns false.
    - If the `item` is not NULL, it checks if the type of the item matches `cJSON_Object` by performing a bitwise AND operation with 0xFF.
    - The result of the comparison is returned as a boolean value.
- **Output**: Returns true if the item is an object, otherwise returns false.


---
### cJSON\_IsRaw<!-- {{#callable:cJSON_IsRaw}} -->
The `cJSON_IsRaw` function checks if a given `cJSON` item is of type raw.
- **Inputs**:
    - `item`: A pointer to a `cJSON` object that is to be checked for the raw type.
- **Control Flow**:
    - The function first checks if the `item` pointer is NULL; if it is, it returns false.
    - If `item` is not NULL, it checks if the type of the item matches `cJSON_Raw` by performing a bitwise AND operation with 0xFF.
    - The result of the comparison is returned as a boolean value.
- **Output**: Returns true if the item is of type raw, otherwise returns false.


---
### cJSON\_Compare<!-- {{#callable:cJSON_Compare}} -->
Compares two `cJSON` objects for equality.
- **Inputs**:
    - `a`: A pointer to the first `cJSON` object to compare.
    - `b`: A pointer to the second `cJSON` object to compare.
    - `case_sensitive`: A boolean indicating whether the comparison should be case-sensitive.
- **Control Flow**:
    - Checks if either `a` or `b` is NULL or if their types differ, returning false if so.
    - Validates that the type of `a` is one of the supported types.
    - If `a` and `b` are the same pointer, returns true.
    - For boolean and null types, returns true if they are of the same type.
    - For numbers, compares their values using a helper function for floating-point precision.
    - For strings and raw types, compares their string values for equality.
    - For arrays, iterates through each element, recursively comparing corresponding elements.
    - For objects, checks each key-value pair in both objects for equality, ensuring all keys match.
- **Output**: Returns true if the two `cJSON` objects are equal, false otherwise.
- **Functions called**:
    - [`compare_double`](#compare_double)
    - [`get_object_item`](#get_object_item)


---
### cJSON\_ArrayForEach<!-- {{#callable:cJSON_Compare::cJSON_ArrayForEach}} -->
Iterates over each element in a JSON array and compares it with corresponding elements in another JSON object.
- **Inputs**:
    - `b_element`: A pointer to the current element in the JSON array being iterated over.
    - `b`: A pointer to the JSON array that is being traversed.
    - `a`: A pointer to the JSON object that contains elements to be compared against.
    - `case_sensitive`: A boolean flag indicating whether the comparison should be case-sensitive.
- **Control Flow**:
    - The function starts by iterating over each element in the JSON array `b` using `cJSON_ArrayForEach`.
    - For each element `b_element`, it retrieves the corresponding element from the JSON object `a` using [`get_object_item`](#get_object_item).
    - If the retrieved element `a_element` is NULL, the function returns false, indicating a mismatch.
    - Next, it compares `b_element` with `a_element` using `cJSON_Compare`.
    - If the comparison fails, the function returns false.
    - If all elements match, the function completes without returning false.
- **Output**: Returns true if all elements in the JSON array `b` match the corresponding elements in the JSON object `a`, otherwise returns false.
- **Functions called**:
    - [`get_object_item`](#get_object_item)


---
### cJSON\_malloc<!-- {{#callable:cJSON_malloc}} -->
Allocates memory of a specified size using a global allocation hook.
- **Inputs**:
    - `size`: The size in bytes of the memory to allocate.
- **Control Flow**:
    - Calls the `allocate` function from the `global_hooks` structure with the specified size.
    - Returns the pointer to the allocated memory.
- **Output**: A pointer to the allocated memory block, or NULL if the allocation fails.


---
### cJSON\_free<!-- {{#callable:cJSON_free}} -->
Frees the memory allocated for a cJSON object.
- **Inputs**:
    - `object`: A pointer to the cJSON object that needs to be deallocated.
- **Control Flow**:
    - Calls the `deallocate` function from `global_hooks` to free the memory associated with the `object`.
    - Sets the `object` pointer to NULL to avoid dangling references.
- **Output**: This function does not return a value; it performs memory deallocation.


# Function Declarations (Public API)

---
### parse\_value<!-- {{#callable_declaration:parse_value}} -->
Parses a JSON value from the input buffer.
- **Description**: Use this function to parse a JSON value from a given input buffer and populate the provided cJSON item with the parsed data. It handles various JSON data types such as null, boolean, string, number, array, and object. This function should be called with a valid parse buffer containing JSON content. It returns a boolean indicating success or failure, allowing the caller to handle parsing errors appropriately.
- **Inputs**:
    - `item`: A pointer to a cJSON structure where the parsed JSON value will be stored. The caller must ensure this is a valid, non-null pointer.
    - `input_buffer`: A pointer to a parse_buffer structure containing the JSON content to be parsed. The content and offset fields must be properly initialized. If the buffer is null or its content is null, the function returns false.
- **Output**: Returns a cJSON_bool indicating success (true) if a value was successfully parsed, or false if parsing failed or the input was invalid.
- **See also**: [`parse_value`](#parse_value)  (Implementation)


---
### print\_value<!-- {{#callable_declaration:print_value}} -->
Renders a cJSON item into a JSON string and writes it to a print buffer.
- **Description**: Use this function to convert a cJSON item into its JSON string representation and store it in a provided print buffer. This function is essential when you need to serialize a cJSON object for output or storage. It must be called with valid cJSON and printbuffer pointers. The function handles various JSON data types, including null, boolean, number, string, array, and object. It returns a boolean indicating success or failure, which can occur if the input parameters are null or if memory allocation fails during the process.
- **Inputs**:
    - `item`: A pointer to a cJSON object to be serialized. Must not be null.
    - `output_buffer`: A pointer to a printbuffer where the JSON string will be written. Must not be null.
- **Output**: Returns a cJSON_bool indicating success (true) or failure (false).
- **See also**: [`print_value`](#print_value)  (Implementation)


---
### parse\_array<!-- {{#callable_declaration:parse_array}} -->
Parses a JSON array from the input buffer.
- **Description**: Use this function to parse a JSON array from a given input buffer and populate a cJSON item with the parsed data. It should be called when you expect the input buffer to contain a JSON array. The function handles nested arrays up to a predefined limit and skips whitespace as needed. It returns a boolean indicating success or failure, where failure can occur due to invalid input, memory allocation issues, or exceeding the nesting limit.
- **Inputs**:
    - `item`: A pointer to a cJSON structure where the parsed array will be stored. Must not be null.
    - `input_buffer`: A pointer to a parse_buffer structure containing the JSON data to be parsed. Must not be null and should be properly initialized with the JSON content and length.
- **Output**: Returns a cJSON_bool indicating success (true) or failure (false).
- **See also**: [`parse_array`](#parse_array)  (Implementation)


---
### print\_array<!-- {{#callable_declaration:print_array}} -->
Renders a cJSON array to a text buffer.
- **Description**: Use this function to convert a cJSON array into its string representation, writing the result into a provided print buffer. This function is typically called internally by other cJSON functions that handle JSON serialization. It requires a valid cJSON array and a non-null print buffer. The function will return false if the output buffer is null or if memory allocation fails during the process.
- **Inputs**:
    - `item`: A pointer to a cJSON structure representing an array. It must not be null and should point to a valid cJSON array object.
    - `output_buffer`: A pointer to a printbuffer structure where the serialized JSON array will be written. Must not be null. The buffer is expected to be properly initialized and capable of being resized if necessary.
- **Output**: Returns a cJSON_bool indicating success (true) or failure (false).
- **See also**: [`print_array`](#print_array)  (Implementation)


---
### parse\_object<!-- {{#callable_declaration:parse_object}} -->
Parses a JSON object from a buffer.
- **Description**: Use this function to parse a JSON object from a given input buffer into a cJSON structure. It should be called when you have a JSON object represented as a string and want to convert it into a cJSON object for further manipulation. The function expects the input buffer to contain a valid JSON object starting with '{'. It handles nested objects up to a predefined depth limit and returns false if the nesting is too deep or if any parsing error occurs. The function modifies the input buffer's offset to reflect the parsing progress and updates the cJSON item with the parsed object data.
- **Inputs**:
    - `item`: A pointer to a cJSON structure where the parsed JSON object will be stored. The caller must ensure this is a valid, non-null pointer.
    - `input_buffer`: A pointer to a parse_buffer structure containing the JSON data to be parsed. The buffer must be properly initialized and must not be null. The function will update the buffer's offset as it parses the JSON object.
- **Output**: Returns a cJSON_bool indicating success (true) or failure (false) of the parsing operation. On success, the item is populated with the parsed JSON object.
- **See also**: [`parse_object`](#parse_object)  (Implementation)


---
### print\_object<!-- {{#callable_declaration:print_object}} -->
Formats a cJSON object into a JSON string and writes it to a print buffer.
- **Description**: Use this function to convert a cJSON object into a JSON formatted string, which can be either pretty-printed or compact, depending on the format setting of the print buffer. This function must be called with a valid cJSON object and a non-null print buffer. It returns a boolean indicating success or failure, which can occur if memory allocation fails or if the input parameters are invalid.
- **Inputs**:
    - `item`: A pointer to a cJSON object to be formatted. Must not be null and should represent a valid JSON object.
    - `output_buffer`: A pointer to a printbuffer structure where the formatted JSON string will be written. Must not be null. The buffer's format field determines if the output is pretty-printed or compact.
- **Output**: Returns a cJSON_bool (true or false) indicating whether the operation was successful. Returns false if the output buffer is null or if memory allocation fails.
- **See also**: [`print_object`](#print_object)  (Implementation)


---
### cJSON\_Duplicate\_rec<!-- {{#callable_declaration:cJSON_Duplicate_rec}} -->
Creates a duplicate of a cJSON item.
- **Description**: Use this function to create a duplicate of a given cJSON item. It can perform a deep copy if the recurse parameter is set to true, duplicating all child elements recursively. This function is useful when you need to manipulate or store a copy of a cJSON structure without affecting the original. Ensure that the item parameter is not null before calling this function. If the depth exceeds a predefined limit, the function will fail and return null.
- **Inputs**:
    - `item`: A pointer to the cJSON item to be duplicated. Must not be null.
    - `depth`: The current depth of recursion. Used internally to prevent excessive recursion.
    - `recurse`: A boolean indicating whether to perform a deep copy. If true, all child elements are duplicated recursively.
- **Output**: Returns a pointer to the duplicated cJSON item, or null if duplication fails.
- **See also**: [`cJSON_Duplicate_rec`](#cJSON_Duplicate_rec)  (Implementation)


