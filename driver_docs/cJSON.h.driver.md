# Purpose
This C header file is part of the cJSON library, which provides functionality for parsing, creating, and manipulating JSON data in C. The file defines the core structures, types, and functions necessary for working with JSON objects, arrays, and values. It includes definitions for various JSON data types such as numbers, strings, arrays, and objects, and provides a comprehensive API for parsing JSON strings into cJSON objects, printing cJSON objects back into JSON strings, and manipulating JSON data structures. The file also includes macros and functions for memory management, allowing users to customize memory allocation and deallocation through hooks.

The header file is designed to be cross-platform, with specific considerations for Windows and Unix-like systems, including handling of symbol visibility and calling conventions. It defines a public API that can be used by other C programs to integrate JSON parsing and manipulation capabilities. The API includes functions for creating JSON objects, adding and removing items, and checking item types, among others. Additionally, the file provides mechanisms to handle deeply nested JSON structures and circular references, ensuring robust parsing and manipulation of complex JSON data. Overall, this header file serves as a foundational component of the cJSON library, enabling efficient and flexible JSON handling in C applications.
# Imports and Dependencies

---
- `stddef.h`


# Data Structures

---
### cJSON
- **Type**: `struct`
- **Members**:
    - `next`: Pointer to the next cJSON item in a linked list, used for traversing arrays or objects.
    - `prev`: Pointer to the previous cJSON item in a linked list, used for traversing arrays or objects.
    - `child`: Pointer to the first child of a cJSON item, used for arrays or objects containing other items.
    - `type`: Integer representing the type of the cJSON item, such as string, number, array, or object.
    - `valuestring`: Pointer to a string value if the cJSON item is of type string or raw.
    - `valueint`: Deprecated integer value of the cJSON item, use cJSON_SetNumberValue instead.
    - `valuedouble`: Double value of the cJSON item if it is of type number.
    - `string`: Pointer to the name string of the cJSON item if it is a child or part of an object.
- **Description**: The cJSON structure is a fundamental component of the cJSON library, representing a JSON item in a tree-like structure. Each cJSON item can be part of a linked list, allowing traversal of JSON arrays and objects. The structure supports various JSON data types, including strings, numbers, arrays, and objects, and provides pointers to manage hierarchical relationships between items. The 'type' field indicates the specific JSON type, while 'valuestring', 'valueint', and 'valuedouble' store the actual data values. The 'string' field is used for naming items within objects, facilitating JSON parsing and manipulation.


---
### cJSON\_Hooks
- **Type**: `struct`
- **Members**:
    - `malloc_fn`: A function pointer to a custom memory allocation function.
    - `free_fn`: A function pointer to a custom memory deallocation function.
- **Description**: The `cJSON_Hooks` structure is designed to allow users to specify custom memory management functions for the cJSON library. It contains two function pointers, `malloc_fn` and `free_fn`, which can be set to user-defined functions for allocating and freeing memory, respectively. This is particularly useful for integrating cJSON with custom memory management systems or for ensuring compatibility with different calling conventions, especially on Windows platforms.


