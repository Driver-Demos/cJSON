# Purpose
This C header file, `cJSON_Utils.h`, is an extension of the cJSON library, providing additional utilities for handling JSON data structures in C. It primarily implements functionalities based on several RFC specifications, including RFC6901 (JSON Pointer), RFC6902 (JSON Patch), and RFC7396 (JSON Merge Patch). These utilities allow for advanced manipulation and querying of JSON objects, such as retrieving values using JSON Pointers, generating and applying JSON Patches to modify JSON objects, and creating JSON Merge Patches to update JSON structures. The functions are designed to handle both case-sensitive and case-insensitive operations, offering flexibility in JSON data processing.

The file defines a set of public APIs that can be used by other C programs to perform complex JSON operations. It includes functions for generating and applying patches, sorting JSON objects, and constructing JSON Pointers. The header file is intended to be included in other C source files, providing a broad range of JSON manipulation capabilities. The use of `CJSON_PUBLIC` indicates that these functions are part of the public API, making them accessible for external use. Additionally, the file includes conditional compilation directives to ensure compatibility with C++ compilers, allowing the utilities to be used in both C and C++ projects.
# Imports and Dependencies

---
- `cJSON.h`


