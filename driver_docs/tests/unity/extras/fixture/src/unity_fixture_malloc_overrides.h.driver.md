# Purpose
This C header file, `UNITY_FIXTURE_MALLOC_OVERRIDES_H_`, is part of the Unity Test Framework, which is designed for unit testing in C. It provides a mechanism to override the standard memory allocation functions (`malloc`, `calloc`, `realloc`, and `free`) with custom implementations, particularly useful for environments where the standard library is unavailable or unsuitable, such as embedded systems. The file includes conditional compilation directives to either use the standard library's memory functions or define custom ones, allowing for deterministic memory allocation from a fixed-size internal heap. This flexibility is crucial for testing on platforms with limited resources or specific memory management requirements, such as those using FreeRTOS.
# Imports and Dependencies

---
- `stddef.h`
- `stdlib.h`


# Global Variables

---
### UNITY\_FIXTURE\_MALLOC
- **Type**: `function pointer`
- **Description**: `UNITY_FIXTURE_MALLOC` is a function pointer that is used to allocate memory on the heap. It is defined as an external function, allowing it to be overridden with platform-specific implementations, such as using `pvPortMalloc()` in FreeRTOS environments.
- **Use**: This variable is used to allocate memory dynamically in the Unity Test Framework, with the possibility of being customized for different platforms.


---
### unity\_malloc
- **Type**: `function pointer`
- **Description**: `unity_malloc` is a function pointer that is used to allocate memory dynamically. It is defined to replace the standard `malloc` function when the `UNITY_FIXTURE_MALLOC` macro is not defined, allowing for custom memory allocation strategies in environments where the standard library is not available or desired.
- **Use**: `unity_malloc` is used to allocate a specified amount of memory on the heap, potentially using a custom implementation for environments without standard library support.


---
### unity\_calloc
- **Type**: `function pointer`
- **Description**: `unity_calloc` is a function pointer that is used to allocate memory for an array of elements, initializing all bytes in the allocated storage to zero. It is part of a set of memory management functions that can be overridden to use platform-specific implementations, particularly useful in environments where the standard library is not available or desired.
- **Use**: `unity_calloc` is used to allocate and zero-initialize memory for an array of elements, serving as a replacement for the standard `calloc` function in the Unity Test Framework.


---
### unity\_realloc
- **Type**: `function pointer`
- **Description**: `unity_realloc` is a function pointer that points to a function used to reallocate memory blocks. It is defined to replace the standard `realloc` function with a custom implementation in the Unity Test Framework.
- **Use**: This function is used to adjust the size of a previously allocated memory block, allowing for dynamic memory management within the Unity Test Framework.


# Function Declarations (Public API)

---
### unity\_malloc<!-- {{#callable_declaration:unity_malloc}} -->
Allocates memory of a specified size using Unity's custom allocator.
- **Description**: This function allocates a block of memory of the specified size using Unity's custom memory allocation mechanism. It is designed to be used in environments where standard library functions like malloc are not available or desired, such as in embedded systems. The function returns a pointer to the allocated memory block, or NULL if the allocation fails. It should be used when dynamic memory allocation is needed within the Unity test framework, especially when UNITY_EXCLUDE_STDLIB_MALLOC is defined. The function handles a special case where a size of zero results in a NULL return, indicating no memory allocation. Additionally, it respects a fail countdown mechanism that can simulate allocation failures for testing purposes.
- **Inputs**:
    - `size`: Specifies the number of bytes to allocate. Must be greater than zero; otherwise, the function returns NULL. The caller does not own the memory and must not pass a zero size.
- **Output**: Returns a pointer to the allocated memory block, or NULL if the allocation fails due to size being zero, insufficient memory, or a simulated failure.
- **See also**: [`unity_malloc`](unity_fixture.c.driver.md#unity_malloc)  (Implementation)


---
### unity\_calloc<!-- {{#callable_declaration:unity_calloc}} -->
Allocates and zeroes memory for an array.
- **Description**: This function allocates memory for an array of elements, each of a specified size, and initializes all bytes in the allocated storage to zero. It is typically used when you need a block of memory initialized to zero, such as when creating arrays or structures that require zero-initialization. The function returns a pointer to the allocated memory, or NULL if the allocation fails. It is important to check the return value to ensure that the memory allocation was successful before using the allocated memory.
- **Inputs**:
    - `num`: The number of elements to allocate memory for. Must be a non-zero value. If zero, the behavior is implementation-defined and may result in a NULL return.
    - `size`: The size of each element to allocate. Must be a non-zero value. If zero, the behavior is implementation-defined and may result in a NULL return.
- **Output**: Returns a pointer to the allocated memory block, which is initialized to zero. If the allocation fails, returns NULL.
- **See also**: [`unity_calloc`](unity_fixture.c.driver.md#unity_calloc)  (Implementation)


---
### unity\_realloc<!-- {{#callable_declaration:unity_realloc}} -->
Reallocate a memory block to a new size.
- **Description**: This function attempts to resize a previously allocated memory block to a new size, returning a pointer to the new memory block. It should be used when you need to change the size of an existing memory allocation. If the old memory pointer is NULL, it behaves like a standard allocation. If the new size is zero, the memory is freed and NULL is returned. The function handles buffer overruns by releasing the memory and failing the test. It is important to ensure that the old memory pointer is valid and was allocated by a compatible allocator.
- **Inputs**:
    - `oldMem`: A pointer to the previously allocated memory block. It must be a valid pointer returned by a compatible allocator or NULL. If NULL, the function allocates a new block of the specified size.
    - `size`: The new size for the memory block in bytes. If zero, the memory is freed and NULL is returned. Must be a non-negative value.
- **Output**: Returns a pointer to the newly allocated memory block of the specified size, or NULL if the allocation fails or if the size is zero.
- **See also**: [`unity_realloc`](unity_fixture.c.driver.md#unity_realloc)  (Implementation)


---
### unity\_free<!-- {{#callable_declaration:unity_free}} -->
Releases allocated memory and checks for buffer overruns.
- **Description**: This function is used to release memory that was previously allocated using Unity's memory management functions. It should be called when the memory is no longer needed to prevent memory leaks. The function also checks for buffer overruns, which can occur if the memory boundaries are exceeded during usage. If a buffer overrun is detected, the function will trigger a test failure. It is important to ensure that the memory pointer passed to this function is valid and was allocated by Unity's memory functions.
- **Inputs**:
    - `mem`: A pointer to the memory block to be freed. It must not be null, and it should point to memory allocated by Unity's memory management functions. If the pointer is null, the function does nothing. The caller retains ownership of the pointer, but it should not be used after calling this function.
- **Output**: None
- **See also**: [`unity_free`](unity_fixture.c.driver.md#unity_free)  (Implementation)


