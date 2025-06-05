# Purpose
The provided C source code file is part of the cJSON library, specifically focusing on utilities for handling JSON data structures. This file extends the core functionality of cJSON by implementing utilities for JSON Pointer and JSON Patch operations, which are standardized methods for referencing and modifying parts of a JSON document. The code includes functions for comparing JSON data, encoding and decoding JSON pointers, and applying JSON patches to modify JSON objects. It also provides functions for generating patches that describe the differences between two JSON objects, allowing for efficient updates and synchronization of JSON data.

Key technical components of this file include functions for string manipulation, JSON pointer encoding/decoding, and JSON patch application. The file defines several public APIs, such as [`cJSONUtils_ApplyPatches`](#cJSONUtils_ApplyPatches), [`cJSONUtils_GeneratePatches`](#cJSONUtils_GeneratePatches), and [`cJSONUtils_MergePatch`](#cJSONUtils_MergePatch), which are intended to be used by other parts of the cJSON library or by external applications that need to manipulate JSON data. The code is structured to handle both case-sensitive and case-insensitive operations, providing flexibility in how JSON data is processed. Additionally, the file includes error handling and memory management to ensure robust and efficient operation. Overall, this file provides specialized utilities that enhance the capabilities of the cJSON library in dealing with complex JSON data manipulation tasks.
# Imports and Dependencies

---
- `ctype.h`
- `string.h`
- `stdlib.h`
- `stdio.h`
- `limits.h`
- `math.h`
- `float.h`
- `cJSON_Utils.h`


# Data Structures

---
### patch\_operation
- **Type**: `enum`
- **Members**:
    - `INVALID`: Represents an invalid or unrecognized operation.
    - `ADD`: Represents the addition of a new element.
    - `REMOVE`: Represents the removal of an existing element.
    - `REPLACE`: Represents the replacement of an existing element with a new one.
    - `MOVE`: Represents moving an element from one location to another.
    - `COPY`: Represents copying an element from one location to another.
    - `TEST`: Represents testing whether a specified condition is met.
- **Description**: The `patch_operation` enum defines a set of operations that can be performed in a JSON patch context, such as adding, removing, replacing, moving, copying, and testing elements. These operations are used to modify JSON documents according to the JSON Patch standard, allowing for dynamic and flexible updates to JSON data structures.


# Functions

---
### cJSONUtils\_strdup<!-- {{#callable:cJSONUtils_strdup}} -->
The `cJSONUtils_strdup` function duplicates a given string by allocating memory and copying the string into it.
- **Inputs**:
    - `string`: A constant pointer to an unsigned char array representing the string to be duplicated.
- **Control Flow**:
    - Initialize a variable `length` to 0 and a pointer `copy` to NULL.
    - Calculate the length of the input string using `strlen` and add the size of an empty string to it, storing the result in `length`.
    - Allocate memory for the new string using `cJSON_malloc` with the calculated `length` and assign the result to `copy`.
    - Check if `copy` is NULL, indicating memory allocation failure, and return NULL if so.
    - Use `memcpy` to copy the input string into the allocated memory pointed to by `copy`.
    - Return the pointer `copy` which now contains the duplicated string.
- **Output**: A pointer to the newly allocated and duplicated string, or NULL if memory allocation fails.


---
### compare\_strings<!-- {{#callable:compare_strings}} -->
The `compare_strings` function compares two strings, either case-sensitively or case-insensitively, and returns an integer indicating their relative order or equality.
- **Inputs**:
    - `string1`: A pointer to the first string to be compared, represented as an unsigned char array.
    - `string2`: A pointer to the second string to be compared, represented as an unsigned char array.
    - `case_sensitive`: A boolean value indicating whether the comparison should be case-sensitive (true) or case-insensitive (false).
- **Control Flow**:
    - Check if either `string1` or `string2` is NULL; if so, return 1 indicating they are not equal.
    - Check if `string1` and `string2` point to the same memory location; if so, return 0 indicating they are equal.
    - If `case_sensitive` is true, use `strcmp` to compare the strings directly and return the result.
    - If `case_sensitive` is false, iterate through each character of the strings, comparing them using `tolower` to ignore case differences.
    - If a null terminator is reached in both strings simultaneously, return 0 indicating they are equal.
    - If a difference is found, return the difference between the first non-matching characters, converted to lowercase.
- **Output**: An integer that is 0 if the strings are equal, a positive value if `string1` is greater than `string2`, or a negative value if `string1` is less than `string2`.


---
### compare\_double<!-- {{#callable:compare_double}} -->
The `compare_double` function checks if two double precision floating-point numbers are approximately equal within a tolerance defined by the machine epsilon.
- **Inputs**:
    - `a`: The first double precision floating-point number to compare.
    - `b`: The second double precision floating-point number to compare.
- **Control Flow**:
    - Calculate the absolute value of both input numbers, `a` and `b`.
    - Determine the maximum of the absolute values of `a` and `b` and store it in `maxVal`.
    - Calculate the absolute difference between `a` and `b`.
    - Compare the absolute difference to `maxVal` multiplied by `DBL_EPSILON` (the smallest difference between two representable double values).
    - Return `true` if the absolute difference is less than or equal to the calculated tolerance, otherwise return `false`.
- **Output**: A boolean value (`cJSON_bool`) indicating whether the two doubles are approximately equal.


---
### compare\_pointers<!-- {{#callable:compare_pointers}} -->
The `compare_pointers` function compares two strings, `name` and `pointer`, up to the next '/' character, considering case sensitivity and handling escape sequences, to determine if they match.
- **Inputs**:
    - `name`: A pointer to an unsigned char array representing the first string to compare.
    - `pointer`: A pointer to an unsigned char array representing the second string to compare, which may contain escape sequences.
    - `case_sensitive`: A boolean value indicating whether the comparison should be case-sensitive.
- **Control Flow**:
    - Check if either `name` or `pointer` is NULL, returning false if so.
    - Iterate through both strings until a null character or '/' is encountered in `pointer`.
    - If a '~' is found in `pointer`, check for valid escape sequences '~0' for '~' and '~1' for '/', returning false if invalid.
    - Compare characters from `name` and `pointer`, considering case sensitivity, and return false if they do not match.
    - After the loop, check if one string has ended while the other has not, returning false if so.
    - Return true if all characters matched up to the termination conditions.
- **Output**: A boolean value indicating whether the two strings match according to the specified conditions.


---
### pointer\_encoded\_length<!-- {{#callable:pointer_encoded_length}} -->
The `pointer_encoded_length` function calculates the length of a string when encoded as a JSON pointer, accounting for escape sequences for '~' and '/'.
- **Inputs**:
    - `string`: A pointer to an unsigned char array representing the input string to be encoded as a JSON pointer.
- **Control Flow**:
    - Initialize a size_t variable `length` to zero.
    - Iterate over each character in the input string until the null terminator is reached.
    - For each character, increment the `length` counter.
    - If the character is '~' or '/', increment the `length` counter again to account for the escape sequence.
    - Return the final value of `length`.
- **Output**: The function returns a `size_t` value representing the length of the input string when encoded as a JSON pointer, including additional length for escape sequences.


---
### encode\_string\_as\_pointer<!-- {{#callable:encode_string_as_pointer}} -->
The `encode_string_as_pointer` function encodes a source string into a destination buffer by replacing '/' with '~1' and '~' with '~0', following JSON pointer encoding rules.
- **Inputs**:
    - `destination`: A pointer to an unsigned char array where the encoded string will be stored.
    - `source`: A pointer to an unsigned char array containing the source string to be encoded.
- **Control Flow**:
    - Iterate over each character in the source string until the null terminator is reached.
    - For each character, check if it is a '/' or '~'.
    - If the character is '/', write '~1' to the destination and increment the destination pointer by two.
    - If the character is '~', write '~0' to the destination and increment the destination pointer by two.
    - If the character is neither '/' nor '~', copy it directly to the destination.
    - After processing all characters, append a null terminator to the destination string.
- **Output**: The function does not return a value; it modifies the destination buffer in place to contain the encoded string.


---
### cJSONUtils\_FindPointerFromObjectTo<!-- {{#callable:cJSONUtils_FindPointerFromObjectTo}} -->
The function `cJSONUtils_FindPointerFromObjectTo` recursively searches for a target cJSON object within a given cJSON object and returns a JSON pointer string representing the path to the target if found.
- **Inputs**:
    - `object`: A pointer to the cJSON object from which the search for the target begins.
    - `target`: A pointer to the cJSON object that is being searched for within the object.
- **Control Flow**:
    - Check if either the object or target is NULL, returning NULL if so.
    - If the object is the same as the target, return an empty string as the path.
    - Iterate over each child of the object, recursively calling `cJSONUtils_FindPointerFromObjectTo` on each child to search for the target.
    - If the target is found in a child, construct the JSON pointer path based on whether the object is an array or an object.
    - For arrays, append the index of the child to the path; for objects, append the encoded key of the child to the path.
    - Return the constructed path if the target is found, or NULL if the target is not found in any children.
- **Output**: A dynamically allocated string representing the JSON pointer path to the target object, or NULL if the target is not found.
- **Functions called**:
    - [`cJSONUtils_strdup`](#cJSONUtils_strdup)
    - [`pointer_encoded_length`](#pointer_encoded_length)
    - [`encode_string_as_pointer`](#encode_string_as_pointer)


---
### get\_array\_item<!-- {{#callable:get_array_item}} -->
The `get_array_item` function retrieves a specific item from a cJSON array based on its index.
- **Inputs**:
    - `array`: A pointer to a cJSON object representing the array from which an item is to be retrieved.
    - `item`: The index of the item to retrieve from the array, specified as a size_t type.
- **Control Flow**:
    - Initialize `child` to the first child of `array` if `array` is not NULL, otherwise set `child` to NULL.
    - Enter a loop that continues as long as `child` is not NULL and `item` is greater than 0.
    - In each iteration, decrement `item` and move `child` to the next sibling in the list.
    - Exit the loop when `item` reaches 0 or `child` becomes NULL.
    - Return the current `child`, which is the desired array item or NULL if the index was out of bounds.
- **Output**: A pointer to the cJSON object at the specified index in the array, or NULL if the index is out of bounds or the array is NULL.


---
### decode\_array\_index\_from\_pointer<!-- {{#callable:decode_array_index_from_pointer}} -->
The function `decode_array_index_from_pointer` parses a string representing an array index and converts it into a numerical index, ensuring the string is a valid representation of a non-negative integer without leading zeros.
- **Inputs**:
    - `pointer`: A constant pointer to an unsigned char array representing the string to be parsed as an array index.
    - `index`: A pointer to a size_t variable where the parsed index will be stored if the parsing is successful.
- **Control Flow**:
    - Initialize `parsed_index` and `position` to zero.
    - Check if the first character of `pointer` is '0' and the next character is not '\0' or '/', returning false if true to disallow leading zeros.
    - Iterate over the characters in `pointer` as long as they are digits ('0' to '9'), updating `parsed_index` by multiplying it by 10 and adding the numeric value of the current character.
    - After the loop, check if the current character is not '\0' or '/', returning false if true to ensure the string ends correctly.
    - Store the parsed index in the variable pointed to by `index`.
    - Return true to indicate successful parsing.
- **Output**: Returns a boolean value (`cJSON_bool`), true if the string was successfully parsed as a valid array index, false otherwise.


---
### get\_item\_from\_pointer<!-- {{#callable:get_item_from_pointer}} -->
The `get_item_from_pointer` function retrieves a specific item from a JSON object or array based on a JSON Pointer string, with an option for case sensitivity.
- **Inputs**:
    - `object`: A pointer to the root cJSON object from which the item is to be retrieved.
    - `pointer`: A string representing the JSON Pointer path to the desired item within the JSON structure.
    - `case_sensitive`: A boolean flag indicating whether the pointer comparison should be case-sensitive.
- **Control Flow**:
    - Initialize `current_element` to the input `object`.
    - Check if `pointer` is NULL; if so, return NULL.
    - Enter a loop that continues as long as the `pointer` starts with '/' and `current_element` is not NULL.
    - Increment the `pointer` to skip the initial '/'.
    - If `current_element` is an array, decode the array index from the `pointer` and retrieve the corresponding array item using [`get_array_item`](#get_array_item).
    - If `current_element` is an object, iterate through its children to find a child whose name matches the `pointer` using [`compare_pointers`](#compare_pointers).
    - If `current_element` is neither an array nor an object, return NULL.
    - Advance the `pointer` to the next '/' or end of string.
    - Return the `current_element` found at the end of the pointer path.
- **Output**: A pointer to the cJSON item located at the specified JSON Pointer path, or NULL if the path is invalid or the item cannot be found.
- **Functions called**:
    - [`decode_array_index_from_pointer`](#decode_array_index_from_pointer)
    - [`get_array_item`](#get_array_item)
    - [`compare_pointers`](#compare_pointers)


---
### cJSONUtils\_GetPointer<!-- {{#callable:cJSONUtils_GetPointer}} -->
The `cJSONUtils_GetPointer` function retrieves a JSON item from a JSON object using a JSON Pointer string.
- **Inputs**:
    - `object`: A pointer to the cJSON object from which an item is to be retrieved.
    - `pointer`: A string representing the JSON Pointer path to the desired item within the JSON object.
- **Control Flow**:
    - The function calls [`get_item_from_pointer`](#get_item_from_pointer) with the provided `object`, `pointer`, and `false` for case insensitivity.
    - The [`get_item_from_pointer`](#get_item_from_pointer) function navigates through the JSON object following the path specified by the `pointer`.
    - If the `pointer` path is valid and the item exists, it returns the corresponding cJSON item.
    - If the `pointer` path is invalid or the item does not exist, it returns NULL.
- **Output**: A pointer to the cJSON item located at the specified JSON Pointer path, or NULL if the path is invalid or the item does not exist.
- **Functions called**:
    - [`get_item_from_pointer`](#get_item_from_pointer)


---
### cJSONUtils\_GetPointerCaseSensitive<!-- {{#callable:cJSONUtils_GetPointerCaseSensitive}} -->
The `cJSONUtils_GetPointerCaseSensitive` function retrieves a JSON item from a given JSON object using a JSON pointer, with case sensitivity enforced.
- **Inputs**:
    - `object`: A pointer to the cJSON object from which an item is to be retrieved.
    - `pointer`: A string representing the JSON pointer path to the desired item within the JSON object.
- **Control Flow**:
    - The function calls [`get_item_from_pointer`](#get_item_from_pointer) with the provided `object`, `pointer`, and `true` for case sensitivity.
    - The [`get_item_from_pointer`](#get_item_from_pointer) function navigates through the JSON object following the path specified by the `pointer`, respecting case sensitivity.
    - If the path is valid and the item is found, it returns the corresponding cJSON item; otherwise, it returns NULL.
- **Output**: A pointer to the cJSON item located at the specified JSON pointer path, or NULL if the path is invalid or the item is not found.
- **Functions called**:
    - [`get_item_from_pointer`](#get_item_from_pointer)


---
### decode\_pointer\_inplace<!-- {{#callable:decode_pointer_inplace}} -->
The `decode_pointer_inplace` function decodes a JSON pointer string in place by replacing escape sequences with their corresponding characters.
- **Inputs**:
    - `string`: A pointer to an unsigned char array (string) that contains the JSON pointer to be decoded.
- **Control Flow**:
    - Check if the input string is NULL, and return immediately if it is.
    - Initialize a pointer `decoded_string` to the start of the input string.
    - Iterate over each character in the input string until the null terminator is reached.
    - If a tilde ('~') is encountered, check the next character to determine the escape sequence.
    - If the escape sequence is '~0', replace it with a tilde ('~') in the decoded string.
    - If the escape sequence is '~1', replace it with a forward slash ('/') in the decoded string.
    - If an invalid escape sequence is encountered, return immediately without further processing.
    - Continue iterating and copying characters to the decoded string.
    - Terminate the decoded string with a null character.
- **Output**: The function modifies the input string in place, decoding any escape sequences, and does not return a value.


---
### detach\_item\_from\_array<!-- {{#callable:detach_item_from_array}} -->
The `detach_item_from_array` function removes a specified item from a cJSON array and returns the detached item.
- **Inputs**:
    - `array`: A pointer to the cJSON array from which an item is to be detached.
    - `which`: The index of the item to be detached from the array.
- **Control Flow**:
    - Initialize a pointer `c` to the first child of the array.
    - Iterate through the array's children until the specified index `which` is reached or the end of the list is encountered.
    - If the item at the specified index does not exist, return NULL.
    - If the item is not the first element, adjust the previous item's `next` pointer to skip the current item.
    - If the item has a next element, adjust the next item's `prev` pointer to skip the current item.
    - If the item is the first child, update the array's child pointer to the next item.
    - If the item is the last element, update the previous item's `next` pointer to NULL.
    - Set the detached item's `prev` and `next` pointers to NULL to fully detach it from the list.
    - Return the detached item.
- **Output**: A pointer to the detached cJSON item, or NULL if the item does not exist.


---
### detach\_path<!-- {{#callable:detach_path}} -->
The `detach_path` function detaches a JSON item from a given path within a JSON object, handling both arrays and objects, and returns the detached item.
- **Inputs**:
    - `object`: A pointer to the cJSON object from which an item is to be detached.
    - `path`: A string representing the path to the item to be detached, using '/' as a separator.
    - `case_sensitive`: A boolean indicating whether the path matching should be case-sensitive.
- **Control Flow**:
    - Allocate memory and duplicate the path string using [`cJSONUtils_strdup`](#cJSONUtils_strdup).
    - Find the last '/' in the path to separate the parent path from the child name.
    - If no '/' is found, or memory allocation fails, clean up and return NULL.
    - Use [`get_item_from_pointer`](#get_item_from_pointer) to find the parent JSON object or array using the parent path.
    - Decode the child pointer in place to handle JSON pointer escape sequences.
    - If the parent is an array, decode the child pointer as an index and detach the item using [`detach_item_from_array`](#detach_item_from_array).
    - If the parent is an object, detach the item using `cJSON_DetachItemFromObject`.
    - If the parent is neither an array nor an object, clean up and return NULL.
    - Free the allocated memory for the parent pointer before returning the detached item.
- **Output**: Returns a pointer to the detached cJSON item, or NULL if the item could not be detached.
- **Functions called**:
    - [`cJSONUtils_strdup`](#cJSONUtils_strdup)
    - [`get_item_from_pointer`](#get_item_from_pointer)
    - [`decode_pointer_inplace`](#decode_pointer_inplace)
    - [`decode_array_index_from_pointer`](#decode_array_index_from_pointer)
    - [`detach_item_from_array`](#detach_item_from_array)


---
### sort\_list<!-- {{#callable:sort_list}} -->
The `sort_list` function sorts a linked list of cJSON objects using a merge sort algorithm, with an option for case-sensitive string comparison.
- **Inputs**:
    - `list`: A pointer to the head of a linked list of cJSON objects to be sorted.
    - `case_sensitive`: A boolean flag indicating whether the string comparison should be case-sensitive.
- **Control Flow**:
    - Check if the list is empty or has only one element, returning it as already sorted.
    - Traverse the list to check if it is already sorted; if so, return it unmodified.
    - Use two pointers to find the middle of the list and split it into two halves.
    - Recursively sort each half of the list.
    - Merge the two sorted halves back together, comparing strings with the specified case sensitivity.
    - Append any remaining elements from either half to the merged list.
- **Output**: A pointer to the head of the newly sorted linked list of cJSON objects.
- **Functions called**:
    - [`compare_strings`](#compare_strings)


---
### sort\_object<!-- {{#callable:sort_object}} -->
The `sort_object` function sorts the children of a cJSON object using a specified case sensitivity.
- **Inputs**:
    - `object`: A pointer to a cJSON object whose children are to be sorted.
    - `case_sensitive`: A boolean value indicating whether the sorting should be case-sensitive or not.
- **Control Flow**:
    - Check if the input object is NULL; if so, return immediately.
    - Call the [`sort_list`](#sort_list) function to sort the children of the object based on the specified case sensitivity.
- **Output**: The function does not return a value; it modifies the input object in place by sorting its children.
- **Functions called**:
    - [`sort_list`](#sort_list)


---
### compare\_json<!-- {{#callable:compare_json}} -->
The `compare_json` function compares two cJSON objects for equality, considering their types and values, with an option for case sensitivity.
- **Inputs**:
    - `a`: A pointer to the first cJSON object to be compared.
    - `b`: A pointer to the second cJSON object to be compared.
    - `case_sensitive`: A boolean flag indicating whether the comparison should be case-sensitive.
- **Control Flow**:
    - Check if either input is NULL or if their types do not match; return false if any condition is true.
    - Use a switch statement to handle different cJSON types: Number, String, Array, and Object.
    - For cJSON_Number, compare both integer and double values; return false if they do not match.
    - For cJSON_String, use strcmp to compare the strings; return false if they do not match.
    - For cJSON_Array, recursively compare each child element; return false if any pair of elements do not match or if the arrays have different lengths.
    - For cJSON_Object, sort both objects, then recursively compare each child element and their keys; return false if any pair of elements do not match or if the objects have different lengths.
    - Return true if all checks pass, indicating the JSON objects are equivalent.
- **Output**: A boolean value indicating whether the two cJSON objects are equivalent.
- **Functions called**:
    - [`compare_double`](#compare_double)
    - [`sort_object`](#sort_object)
    - [`compare_strings`](#compare_strings)


---
### insert\_item\_in\_array<!-- {{#callable:insert_item_in_array}} -->
The `insert_item_in_array` function inserts a new cJSON item into a specified position within a cJSON array.
- **Inputs**:
    - `array`: A pointer to the cJSON array where the new item will be inserted.
    - `which`: The index position in the array where the new item should be inserted.
    - `newitem`: A pointer to the cJSON item that is to be inserted into the array.
- **Control Flow**:
    - Initialize a pointer `child` to the first child of the array.
    - Iterate through the array's children until the desired index `which` is reached or the end of the list is encountered.
    - If `which` is greater than 0 after the loop, return 0 indicating the index is out of bounds.
    - If `child` is NULL, append the new item to the end of the array using `cJSON_AddItemToArray` and return 1.
    - Otherwise, insert `newitem` into the linked list before `child`, adjusting the `next` and `prev` pointers accordingly.
    - If `child` was the first item in the array, update the array's `child` pointer to `newitem`.
    - Return 1 to indicate successful insertion.
- **Output**: Returns a cJSON_bool (1 for success, 0 for failure) indicating whether the insertion was successful.


---
### get\_object\_item<!-- {{#callable:get_object_item}} -->
The `get_object_item` function retrieves a JSON object item by its name, with an option for case-sensitive or case-insensitive search.
- **Inputs**:
    - `object`: A constant pointer to a cJSON object from which an item is to be retrieved.
    - `name`: A constant character pointer representing the name of the item to be retrieved from the JSON object.
    - `case_sensitive`: A cJSON_bool indicating whether the search should be case-sensitive (true) or case-insensitive (false).
- **Control Flow**:
    - The function checks if the search should be case-sensitive by evaluating the `case_sensitive` parameter.
    - If `case_sensitive` is true, it calls `cJSON_GetObjectItemCaseSensitive` to retrieve the item with a case-sensitive search.
    - If `case_sensitive` is false, it calls `cJSON_GetObjectItem` to retrieve the item with a case-insensitive search.
- **Output**: Returns a pointer to the cJSON object item if found, or NULL if the item is not found.


---
### decode\_patch\_operation<!-- {{#callable:decode_patch_operation}} -->
The `decode_patch_operation` function determines the type of JSON Patch operation specified in a given JSON object.
- **Inputs**:
    - `patch`: A pointer to a cJSON object representing the JSON Patch operation.
    - `case_sensitive`: A boolean flag indicating whether the operation name comparison should be case-sensitive.
- **Control Flow**:
    - Retrieve the 'op' field from the JSON object using the [`get_object_item`](#get_object_item) function, considering case sensitivity.
    - Check if the retrieved 'op' field is a string; if not, return `INVALID`.
    - Compare the string value of the 'op' field against known operation names ('add', 'remove', 'replace', 'move', 'copy', 'test').
    - Return the corresponding enum value for the matched operation name.
    - If no match is found, return `INVALID`.
- **Output**: Returns an enum value of type `patch_operation` representing the decoded operation, or `INVALID` if the operation is not recognized.
- **Functions called**:
    - [`get_object_item`](#get_object_item)


---
### overwrite\_item<!-- {{#callable:overwrite_item}} -->
The `overwrite_item` function replaces the contents of a given cJSON object with another cJSON object, freeing any existing resources in the process.
- **Inputs**:
    - `root`: A pointer to the cJSON object that will be overwritten.
    - `replacement`: A cJSON object that contains the new data to overwrite the existing object.
- **Control Flow**:
    - Check if the `root` is NULL and return immediately if it is.
    - Free the memory allocated for the `string` field of the `root` if it is not NULL.
    - Free the memory allocated for the `valuestring` field of the `root` if it is not NULL.
    - Delete the `child` of the `root` if it is not NULL, freeing its resources.
    - Copy the contents of the `replacement` object into the `root` object using `memcpy`.
- **Output**: The function does not return a value; it modifies the `root` object in place.


---
### apply\_patch<!-- {{#callable:apply_patch}} -->
The `apply_patch` function applies a JSON patch operation to a given JSON object, modifying it according to the specified patch instructions.
- **Inputs**:
    - `object`: A pointer to the cJSON object that will be modified by the patch.
    - `patch`: A pointer to the cJSON object representing the patch to be applied, containing the operation and path details.
    - `case_sensitive`: A boolean flag indicating whether the patch application should be case-sensitive.
- **Control Flow**:
    - Initialize variables for path, value, parent, opcode, and pointers.
    - Retrieve the 'path' from the patch and check if it is a valid string; if not, set status to 2 and exit.
    - Decode the patch operation from the patch; if invalid, set status to 3 and exit.
    - Handle the 'TEST' operation by comparing the JSON value at the path with the patch value; set status accordingly and exit.
    - Handle special case for replacing the root object for 'REMOVE', 'REPLACE', and 'ADD' operations.
    - For 'REMOVE' and 'REPLACE', detach the item at the path and delete it; if 'REMOVE', exit with status 0.
    - For 'MOVE' and 'COPY', retrieve the 'from' path, detach or duplicate the item, and handle errors.
    - For 'ADD' and 'REPLACE', retrieve and duplicate the 'value' from the patch, handling memory errors.
    - Split the path into parent and child pointers, retrieve the parent object, and decode the child pointer.
    - Depending on whether the parent is an array or object, add or replace the item at the child pointer.
    - Handle cleanup by freeing allocated memory and deleting temporary values.
    - Return the status indicating success or the type of error encountered.
- **Output**: An integer status code indicating the result of the patch application, where 0 means success and non-zero values indicate specific errors.
- **Functions called**:
    - [`get_object_item`](#get_object_item)
    - [`decode_patch_operation`](#decode_patch_operation)
    - [`compare_json`](#compare_json)
    - [`get_item_from_pointer`](#get_item_from_pointer)
    - [`overwrite_item`](#overwrite_item)
    - [`detach_path`](#detach_path)
    - [`cJSONUtils_strdup`](#cJSONUtils_strdup)
    - [`decode_pointer_inplace`](#decode_pointer_inplace)
    - [`decode_array_index_from_pointer`](#decode_array_index_from_pointer)
    - [`insert_item_in_array`](#insert_item_in_array)


---
### cJSONUtils\_ApplyPatches<!-- {{#callable:cJSONUtils_ApplyPatches}} -->
The `cJSONUtils_ApplyPatches` function applies a series of JSON patches to a given JSON object.
- **Inputs**:
    - `object`: A pointer to the cJSON object to which the patches will be applied.
    - `patches`: A pointer to a cJSON array containing the patches to be applied.
- **Control Flow**:
    - Check if the 'patches' input is a valid JSON array; if not, return 1 indicating malformed patches.
    - Initialize 'current_patch' to the first child of the 'patches' array if 'patches' is not NULL.
    - Iterate over each patch in the 'patches' array using a while loop.
    - For each patch, call the 'apply_patch' function with the 'object', 'current_patch', and 'false' for case sensitivity.
    - If 'apply_patch' returns a non-zero status, return that status immediately.
    - Move to the next patch in the array by setting 'current_patch' to 'current_patch->next'.
    - If all patches are applied successfully, return 0.
- **Output**: Returns an integer status code: 0 if all patches are applied successfully, or a non-zero error code if an error occurs during patch application.
- **Functions called**:
    - [`apply_patch`](#apply_patch)


---
### cJSONUtils\_ApplyPatchesCaseSensitive<!-- {{#callable:cJSONUtils_ApplyPatchesCaseSensitive}} -->
The `cJSONUtils_ApplyPatchesCaseSensitive` function applies a series of JSON patches to a given JSON object, considering case sensitivity in the process.
- **Inputs**:
    - `object`: A pointer to the cJSON object to which the patches will be applied.
    - `patches`: A pointer to a cJSON array containing the patches to be applied.
- **Control Flow**:
    - Check if the `patches` input is an array; if not, return 1 indicating malformed patches.
    - Initialize `current_patch` to the first child of `patches` if `patches` is not NULL.
    - Iterate over each `current_patch` in the `patches` array.
    - For each `current_patch`, call [`apply_patch`](#apply_patch) with the `object`, `current_patch`, and `true` for case sensitivity.
    - If [`apply_patch`](#apply_patch) returns a non-zero status, return that status immediately.
    - Continue to the next patch until all patches are processed.
    - Return 0 if all patches are applied successfully without errors.
- **Output**: Returns an integer status code: 0 for success, 1 for malformed patches, or any non-zero status from [`apply_patch`](#apply_patch) indicating an error.
- **Functions called**:
    - [`apply_patch`](#apply_patch)


---
### compose\_patch<!-- {{#callable:compose_patch}} -->
The `compose_patch` function creates a JSON patch object and adds it to a given array of patches.
- **Inputs**:
    - `patches`: A cJSON array where the newly created patch object will be added.
    - `operation`: A string representing the operation type (e.g., 'add', 'remove', etc.) for the patch.
    - `path`: A string representing the JSON pointer path where the operation will be applied.
    - `suffix`: An optional string that, if provided, will be appended to the path after encoding.
    - `value`: An optional cJSON object representing the value to be used in the patch operation.
- **Control Flow**:
    - Check if `patches`, `operation`, or `path` is NULL; if any are NULL, return immediately.
    - Create a new cJSON object `patch` to represent the patch; if creation fails, return immediately.
    - Add the operation type to the `patch` object under the key 'op'.
    - If `suffix` is NULL, add the `path` directly to the `patch` object under the key 'path'.
    - If `suffix` is not NULL, calculate the length of the encoded `suffix` and allocate memory for the full path, then encode the `suffix` and append it to the `path`, adding the result to the `patch` object under the key 'path'.
    - If `value` is not NULL, duplicate it and add it to the `patch` object under the key 'value'.
    - Add the `patch` object to the `patches` array.
- **Output**: The function does not return a value; it modifies the `patches` array by adding a new patch object to it.
- **Functions called**:
    - [`pointer_encoded_length`](#pointer_encoded_length)
    - [`encode_string_as_pointer`](#encode_string_as_pointer)


---
### cJSONUtils\_AddPatchToArray<!-- {{#callable:cJSONUtils_AddPatchToArray}} -->
The function `cJSONUtils_AddPatchToArray` adds a JSON patch operation to a given array.
- **Inputs**:
    - `array`: A pointer to a cJSON array where the patch operation will be added.
    - `operation`: A string representing the type of operation (e.g., "add", "remove", "replace").
    - `path`: A string representing the JSON pointer path where the operation will be applied.
    - `value`: A pointer to a cJSON object representing the value associated with the operation, if applicable.
- **Control Flow**:
    - The function calls [`compose_patch`](#compose_patch) with the provided `array`, `operation`, `path`, and `value` arguments.
    - The [`compose_patch`](#compose_patch) function constructs a JSON patch object and adds it to the `array`.
- **Output**: The function does not return a value; it modifies the `array` in place by adding a new patch operation.
- **Functions called**:
    - [`compose_patch`](#compose_patch)


---
### create\_patches<!-- {{#callable:create_patches}} -->
The `create_patches` function generates a series of JSON patch operations to transform a JSON object 'from' into another JSON object 'to', considering differences in structure and content.
- **Inputs**:
    - `patches`: A cJSON array where the generated patch operations will be stored.
    - `path`: A string representing the current JSON pointer path being evaluated.
    - `from`: The source cJSON object to compare.
    - `to`: The target cJSON object to compare against.
    - `case_sensitive`: A boolean indicating whether string comparisons should be case-sensitive.
- **Control Flow**:
    - Check if either 'from' or 'to' is NULL and return immediately if so.
    - Compare the types of 'from' and 'to'; if they differ, create a 'replace' patch operation and return.
    - For cJSON_Number type, compare integer and double values; if they differ, create a 'replace' patch operation.
    - For cJSON_String type, compare strings; if they differ, create a 'replace' patch operation.
    - For cJSON_Array type, iterate over elements in both 'from' and 'to', recursively calling create_patches for each element, and handle leftover elements by creating 'remove' or 'add' patch operations.
    - For cJSON_Object type, sort both objects, iterate over their children, and create 'remove', 'add', or recursive patch operations based on key differences.
- **Output**: The function does not return a value; it modifies the 'patches' array in place by adding patch operations.
- **Functions called**:
    - [`compose_patch`](#compose_patch)
    - [`compare_double`](#compare_double)
    - [`sort_object`](#sort_object)
    - [`compare_strings`](#compare_strings)
    - [`pointer_encoded_length`](#pointer_encoded_length)
    - [`encode_string_as_pointer`](#encode_string_as_pointer)


---
### cJSONUtils\_GeneratePatches<!-- {{#callable:cJSONUtils_GeneratePatches}} -->
The `cJSONUtils_GeneratePatches` function generates a JSON patch array that describes the differences between two JSON objects.
- **Inputs**:
    - `from`: A pointer to the cJSON object representing the original JSON structure.
    - `to`: A pointer to the cJSON object representing the target JSON structure to compare against.
- **Control Flow**:
    - Check if either 'from' or 'to' is NULL, and return NULL if so.
    - Create a new cJSON array to hold the patches.
    - Call the helper function [`create_patches`](#create_patches) to populate the patches array with differences between 'from' and 'to'.
    - Return the populated patches array.
- **Output**: A cJSON array containing the patches that describe the differences between the 'from' and 'to' JSON objects, or NULL if either input is NULL.
- **Functions called**:
    - [`create_patches`](#create_patches)


---
### cJSONUtils\_GeneratePatchesCaseSensitive<!-- {{#callable:cJSONUtils_GeneratePatchesCaseSensitive}} -->
The function `cJSONUtils_GeneratePatchesCaseSensitive` generates a JSON patch array that represents the differences between two JSON objects, considering case sensitivity.
- **Inputs**:
    - `from`: A pointer to the source cJSON object from which differences are calculated.
    - `to`: A pointer to the target cJSON object to which differences are calculated.
- **Control Flow**:
    - Check if either 'from' or 'to' is NULL, and return NULL if so.
    - Create a new cJSON array to hold the patches.
    - Call the [`create_patches`](#create_patches) function with the newly created array, an empty string as the path, the 'from' and 'to' objects, and a boolean indicating case sensitivity.
    - Return the array of patches.
- **Output**: A cJSON array containing the patches that represent the differences between the 'from' and 'to' JSON objects, or NULL if either input is NULL.
- **Functions called**:
    - [`create_patches`](#create_patches)


---
### cJSONUtils\_SortObject<!-- {{#callable:cJSONUtils_SortObject}} -->
The `cJSONUtils_SortObject` function sorts the children of a given cJSON object in a non-case-sensitive manner.
- **Inputs**:
    - `object`: A pointer to a cJSON object whose children are to be sorted.
- **Control Flow**:
    - The function calls the helper function [`sort_object`](#sort_object) with the provided `object` and a `false` flag indicating non-case-sensitive sorting.
    - The [`sort_object`](#sort_object) function sorts the children of the `object` using a merge sort algorithm implemented in `sort_list`.
    - The `sort_list` function recursively divides the list of children into sublists, sorts them, and merges them back together in sorted order.
- **Output**: The function does not return a value; it modifies the input cJSON object in place by sorting its children.
- **Functions called**:
    - [`sort_object`](#sort_object)


---
### cJSONUtils\_SortObjectCaseSensitive<!-- {{#callable:cJSONUtils_SortObjectCaseSensitive}} -->
The function `cJSONUtils_SortObjectCaseSensitive` sorts the children of a cJSON object in a case-sensitive manner.
- **Inputs**:
    - `object`: A pointer to a cJSON object whose children are to be sorted.
- **Control Flow**:
    - The function calls [`sort_object`](#sort_object) with the provided `object` and a `true` flag indicating case-sensitive sorting.
    - The [`sort_object`](#sort_object) function sorts the children of the `object` using a merge sort algorithm implemented in `sort_list`, which compares strings case-sensitively.
- **Output**: The function does not return a value; it modifies the input cJSON object in place.
- **Functions called**:
    - [`sort_object`](#sort_object)


---
### merge\_patch<!-- {{#callable:merge_patch}} -->
The `merge_patch` function applies a JSON Merge Patch to a target JSON object, modifying it according to the patch's instructions, with an option for case sensitivity.
- **Inputs**:
    - `target`: A pointer to the cJSON object that is to be modified by the patch.
    - `patch`: A constant pointer to the cJSON object representing the patch to be applied.
    - `case_sensitive`: A cJSON_bool indicating whether the patch application should be case-sensitive.
- **Control Flow**:
    - Check if the patch is not an object; if so, delete the target and duplicate the patch to return.
    - If the target is not an object, delete it and create a new empty object for the target.
    - Iterate over each child of the patch object.
    - If a patch child is NULL, remove the corresponding item from the target object, considering case sensitivity.
    - If a patch child is not NULL, detach the corresponding item from the target object, apply the patch recursively, and add the result back to the target object.
    - Return the modified target object.
- **Output**: Returns a pointer to the modified target cJSON object after applying the patch, or NULL if an error occurs during the process.


---
### cJSONUtils\_MergePatch<!-- {{#callable:cJSONUtils_MergePatch}} -->
The `cJSONUtils_MergePatch` function applies a JSON Merge Patch to a target JSON object, modifying it according to the patch instructions.
- **Inputs**:
    - `target`: A pointer to the cJSON object that will be modified by the merge patch.
    - `patch`: A constant pointer to the cJSON object representing the merge patch to be applied.
- **Control Flow**:
    - The function calls the [`merge_patch`](#merge_patch) function with the `target`, `patch`, and `false` for case insensitivity.
    - The [`merge_patch`](#merge_patch) function checks if the `patch` is an object; if not, it duplicates the `patch` and deletes the `target`.
    - If the `patch` is an object, it iterates over its children.
    - For each child, if it is `NULL`, it removes the corresponding item from the `target`.
    - If not `NULL`, it detaches the item from the `target`, applies the [`merge_patch`](#merge_patch) recursively, and adds the result back to the `target`.
    - The modified `target` is returned.
- **Output**: A pointer to the modified cJSON object after applying the merge patch.
- **Functions called**:
    - [`merge_patch`](#merge_patch)


---
### cJSONUtils\_MergePatchCaseSensitive<!-- {{#callable:cJSONUtils_MergePatchCaseSensitive}} -->
The function `cJSONUtils_MergePatchCaseSensitive` applies a JSON Merge Patch to a target JSON object with case sensitivity.
- **Inputs**:
    - `target`: A pointer to the cJSON object that will be modified by the merge patch.
    - `patch`: A constant pointer to the cJSON object representing the patch to be applied.
- **Control Flow**:
    - The function calls [`merge_patch`](#merge_patch) with the `target`, `patch`, and `true` for case sensitivity.
    - The [`merge_patch`](#merge_patch) function checks if the `patch` is an object; if not, it duplicates the `patch` and deletes the `target`.
    - If the `patch` is an object, it iterates over its children.
    - For each child, if it is `NULL`, it removes the corresponding item from the `target`.
    - If not `NULL`, it detaches the item from the `target`, applies the patch recursively, and adds the result back to the `target`.
- **Output**: A pointer to the modified cJSON object after applying the merge patch.
- **Functions called**:
    - [`merge_patch`](#merge_patch)


---
### generate\_merge\_patch<!-- {{#callable:generate_merge_patch}} -->
The `generate_merge_patch` function creates a JSON merge patch that represents the differences between two JSON objects, `from` and `to`, considering case sensitivity.
- **Inputs**:
    - `from`: A pointer to a cJSON object representing the original JSON structure.
    - `to`: A pointer to a cJSON object representing the target JSON structure to compare against.
    - `case_sensitive`: A boolean flag indicating whether the comparison should be case-sensitive.
- **Control Flow**:
    - If `to` is NULL, return a JSON null object to indicate deletion of everything.
    - If either `from` or `to` is not a JSON object, return a duplicate of `to`.
    - Sort both `from` and `to` objects based on their keys, considering case sensitivity.
    - Initialize `from_child` and `to_child` to iterate over the children of `from` and `to`, respectively.
    - Create a new JSON object `patch` to store the differences.
    - Iterate over the children of `from` and `to` using `from_child` and `to_child`.
    - Compare the keys of `from_child` and `to_child` to determine differences.
    - If a key exists in `from` but not in `to`, add a null entry to `patch` to indicate removal.
    - If a key exists in `to` but not in `from`, add a duplicate of the `to_child` to `patch`.
    - If a key exists in both, compare their values; if they differ, recursively generate a merge patch for the differing values.
    - If no differences are found, delete the `patch` and return NULL.
- **Output**: A cJSON object representing the merge patch, or NULL if no differences are found.
- **Functions called**:
    - [`sort_object`](#sort_object)
    - [`compare_json`](#compare_json)
    - [`cJSONUtils_GenerateMergePatch`](#cJSONUtils_GenerateMergePatch)


---
### cJSONUtils\_GenerateMergePatch<!-- {{#callable:cJSONUtils_GenerateMergePatch}} -->
The `cJSONUtils_GenerateMergePatch` function generates a JSON merge patch that describes the differences between two JSON objects.
- **Inputs**:
    - `from`: A pointer to the cJSON object representing the original JSON structure.
    - `to`: A pointer to the cJSON object representing the target JSON structure to compare against.
- **Control Flow**:
    - The function calls [`generate_merge_patch`](#generate_merge_patch) with the `from` and `to` JSON objects and a `false` flag for case sensitivity.
    - The [`generate_merge_patch`](#generate_merge_patch) function checks if the `to` object is NULL, returning a JSON null if true, indicating a patch to delete everything.
    - If either `from` or `to` is not an object, it duplicates the `to` object and returns it as the patch.
    - Both `from` and `to` objects are sorted to ensure consistent comparison order.
    - The function iterates over the children of both `from` and `to` objects, comparing keys and values.
    - If a key exists in `from` but not in `to`, a null is added to the patch to indicate removal.
    - If a key exists in `to` but not in `from`, the value is added to the patch to indicate addition.
    - If a key exists in both but values differ, a recursive call to `cJSONUtils_GenerateMergePatch` is made to generate a patch for that key.
    - If no differences are found, the patch object is deleted and NULL is returned.
- **Output**: A cJSON object representing the merge patch, or NULL if no differences are found.
- **Functions called**:
    - [`generate_merge_patch`](#generate_merge_patch)


---
### cJSONUtils\_GenerateMergePatchCaseSensitive<!-- {{#callable:cJSONUtils_GenerateMergePatchCaseSensitive}} -->
The function `cJSONUtils_GenerateMergePatchCaseSensitive` generates a JSON merge patch that is case-sensitive, comparing two JSON objects to determine the differences.
- **Inputs**:
    - `from`: A pointer to a cJSON object representing the original JSON structure.
    - `to`: A pointer to a cJSON object representing the target JSON structure to compare against the original.
- **Control Flow**:
    - The function calls [`generate_merge_patch`](#generate_merge_patch) with the `from` and `to` JSON objects and a boolean flag set to `true` to indicate case sensitivity.
    - The [`generate_merge_patch`](#generate_merge_patch) function compares the `from` and `to` JSON objects, generating a patch object that represents the differences.
    - If the `to` object is `NULL`, a JSON `null` is returned to indicate deletion of everything.
    - If either `from` or `to` is not an object, the `to` object is duplicated and returned.
    - The function sorts both `from` and `to` objects to ensure consistent comparison.
    - It iterates over the children of both `from` and `to`, adding or removing items in the patch as necessary based on their presence or absence in each object.
    - If a key exists in both objects but the values differ, a recursive call to `cJSONUtils_GenerateMergePatch` is made to generate a patch for that specific key.
    - If no differences are found, the function returns `NULL`.
- **Output**: A cJSON object representing the merge patch, or `NULL` if no differences are found.
- **Functions called**:
    - [`generate_merge_patch`](#generate_merge_patch)


