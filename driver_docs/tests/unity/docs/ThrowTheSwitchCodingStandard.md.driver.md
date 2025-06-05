# Purpose
The provided content is a coding standard document from ThrowTheSwitch.org, which outlines the conventions and philosophies for writing code within their projects. This document serves as a guideline for contributors to ensure consistency and readability across the codebase, particularly for C and Ruby programming languages. It covers various aspects such as naming conventions, whitespace usage, brace placement, and commenting styles, emphasizing the importance of readable, descriptive, consistent, and memorable code. The document also highlights the organization's philosophy of supporting a wide range of compilers, especially for embedded systems, and the aim to make tools work seamlessly with minimal configuration. This coding standard is crucial for maintaining a cohesive and understandable codebase, facilitating collaboration among developers, and ensuring that the software is robust and maintainable.
# Content Summary
The provided document is a comprehensive coding standard guide for contributors to ThrowTheSwitch.org, focusing on C and Ruby programming languages. It aims to unify code contributions into a cohesive unit by establishing clear guidelines and philosophies for code consistency and readability.

### Key Philosophies and Standards:

1. **Purpose of a Coding Standard**: The document emphasizes the importance of consistency in code to enhance understandability. It aims to keep the standards simple and comprehensible, encouraging contributors to adhere to them.

2. **Philosophy**: The primary philosophy is to support as many compilers as possible, especially those used in embedded systems, by writing standards-compliant C code (often C89). The tools are designed to be tolerant of non-standard compiler quirks, with configurable options to accommodate various needs. The secondary philosophy is to ensure that configuration options have logical defaults, minimizing the need for user intervention.

3. **Naming Conventions**: The guide outlines a hierarchy for naming that prioritizes readability, descriptiveness, consistency, and memorability. It discourages cryptic abbreviations and Hungarian notation, advocating for clear and descriptive names, especially for functions and variables. Exceptions are made for loop counters and local variables where simplicity is preferred.

4. **C and C++ Style Details**:
   - **Whitespace**: Uses spaces with 4 spaces per indent level.
   - **Case**: Different conventions for files, variables, macros, typedefs, functions, constants, and globals, with a mix of lower case, underscores, and camel case.
   - **Braces**: Left braces are placed on the next line after declarations, with consistent indentation.
   - **Comments**: Despite a preference against them, old-school C block comments are used for compatibility with older compilers.

5. **Ruby Style Details**:
   - **Whitespace**: Uses spaces with 2 spaces per indent level.
   - **Case**: Specific conventions for files, variables, classes, modules, functions, and constants, using a mix of lower case, underscores, and camel case.

6. **Documentation**: The project uses markdown for documentation and prefers PDF files for their aesthetic and portability benefits.

Overall, the document serves as a detailed guide for contributors to maintain a consistent and readable codebase, accommodating a wide range of compilers and development environments, particularly in the context of embedded systems.
