# Purpose
The `TypeSanitizer` module provides a narrow, specific functionality aimed at transforming strings into valid C identifiers. It includes a single class method, `sanitize_c_identifier`, which takes an input string (`unsanitized`) and replaces characters that are invalid in C identifiers—such as hyphens, slashes, backslashes, periods, commas, and whitespace—with underscores. This ensures that the resulting string conforms to the naming conventions required for identifiers in the C programming language, which can be particularly useful when dynamically generating C code or interfacing with C libraries.
# Modules

---
### TypeSanitizer
- **Description**: The TypeSanitizer module provides a utility method for converting strings into valid C identifiers by replacing characters that are not allowed in C identifiers, such as hyphens, slashes, backslashes, periods, commas, and whitespace, with underscores. This is particularly useful for ensuring that dynamically generated or user-provided strings can be safely used as identifiers in C code.

**Module Methods**

---
#### TypeSanitizer\.sanitize\_c\_identifier
The `sanitize_c_identifier` method converts a given string into a valid C identifier by replacing invalid characters with underscores.
- **Inputs**:
    - `unsanitized`: A string that potentially contains characters invalid for a C identifier, such as hyphens, slashes, backslashes, periods, commas, and whitespace.
- **Control Flow**:
    - The method uses the `gsub` function to search for any characters in the input string that match the regular expression pattern `[-\/\\\.\,\s]`, which includes hyphens, slashes, backslashes, periods, commas, and whitespace.
    - Each occurrence of these characters is replaced with an underscore ('_').
- **Output**: A new string where all invalid characters have been replaced with underscores, making it a valid C identifier.



