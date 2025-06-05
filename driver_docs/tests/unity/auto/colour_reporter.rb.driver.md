# Purpose
This Ruby code file is part of the Unity Project, a test framework designed for C programming. The primary purpose of this file is to handle the reporting of test results with color-coded output to enhance readability and quick interpretation of test outcomes. The code achieves this by defining a `report` method that processes messages, which can be either strings or arrays, and applies color formatting based on the content of each line. The color coding is determined by specific keywords or patterns within the lines, such as "PASS" for green, "FAIL" or "ERROR" for red, and "IGNORE" for yellow. This visual feedback is particularly useful in continuous integration environments or when running tests in a terminal, as it allows developers to quickly identify the status of their tests.

The file includes a requirement for a `colour_prompt` module, which is likely responsible for the actual implementation of the `colour_puts` method used to print colored text to the console. The global variable `$colour_output` is used to toggle the color output feature, defaulting to true, indicating that color output is enabled by default. This file does not define a broad range of functionalities but focuses specifically on enhancing the test reporting aspect of the Unity Project. It does not define public APIs or external interfaces but rather serves as an internal utility to improve the user experience when interpreting test results.
# Imports and Dependencies

---
- `colour_prompt`


