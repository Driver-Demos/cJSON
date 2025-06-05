# Purpose
The provided Ruby code is a script designed to generate test runners for C projects using the Unity test framework. The primary class, `UnityTestRunnerGenerator`, is responsible for creating a test runner file from a given C source file. This script automates the process of setting up a test environment by parsing the input C file to identify test cases, includes, and mock dependencies, and then generating a corresponding C file that can execute these tests. The script supports configuration through YAML files or direct command-line options, allowing users to customize various aspects of the test runner, such as the names of setup and teardown functions, the main function, and the inclusion of plugins like CException.

The script provides a broad functionality focused on test automation for C projects, encapsulating several key components such as test discovery, mock management, and test execution. It defines a public API through its command-line interface, allowing users to specify input files, output files, and configuration options. The script is designed to be flexible, supporting parameterized tests and custom suite setup and teardown code. It also includes mechanisms to handle different file encodings and integrates with the CMock framework for mock management. Overall, this script is a comprehensive tool for generating test runners, streamlining the process of testing C code with Unity.
# Imports and Dependencies

---
- `File`
- `YAML`
- `TypeSanitizer`


# Classes

---
### UnityTestRunnerGenerator
- **Description**: The `UnityTestRunnerGenerator` class is designed to automate the generation of test runner files for C unit tests using the Unity framework. It initializes with a set of default options, which can be customized through a configuration file or a hash of options. The class provides methods to parse C source files, identify test cases, and generate corresponding test runner files. It handles the inclusion of necessary headers, management of mock objects, and setup and teardown of test suites. The class also supports parameterized tests and can be configured to include additional plugins like CException. The generated test runner files are designed to be compatible with command-line arguments and can be customized to include specific setup and teardown functions.

**Class Methods**

---
#### UnityTestRunnerGenerator\.default\_options
The `default_options` method returns a hash of default configuration options for the UnityTestRunnerGenerator class.
- **Inputs**: None
- **Control Flow**:
    - The method is defined as a class method using `self.default_options`.
    - It returns a hash with predefined keys and values representing default settings for the test runner generator.
- **Output**: A hash containing default configuration options such as `includes`, `defines`, `plugins`, `framework`, `test_prefix`, `mock_prefix`, `setup_name`, `teardown_name`, `main_name`, `main_export_decl`, `cmdline_args`, and `use_param_tests`.


---
#### UnityTestRunnerGenerator\.grab\_config
The `grab_config` method loads and merges configuration options from a YAML file into default options for the UnityTestRunnerGenerator.
- **Inputs**:
    - `config_file`: A string representing the path to a YAML configuration file, which may contain configuration options under the :unity or :cmock keys.
- **Control Flow**:
    - Initialize options with default options by calling `default_options` method.
    - Check if `config_file` is not nil or empty.
    - If `config_file` is valid, require the 'yaml' library and load the YAML file contents into `yaml_guts`.
    - Merge the contents of `yaml_guts[:unity]` or `yaml_guts[:cmock]` into `options`.
    - Raise an error if neither :unity nor :cmock sections are found in the YAML file.
    - Return the merged `options`.
- **Output**: A hash containing the merged configuration options, with defaults overridden by any options found in the specified YAML file.


**Instance Methods**

---
#### UnityTestRunnerGenerator\.create\_externs
The `create_externs` method generates and writes C function declarations for setup, teardown, and test functions to an output stream.
- **Inputs**:
    - `output`: An IO-like object where the generated C function declarations will be written.
    - `tests`: An array of hashes, each containing details about a test function, including its name and call signature.
    - `_mocks`: An unused parameter in this method, typically representing mock objects.
- **Control Flow**:
    - The method begins by writing a comment to the output indicating the start of external function declarations.
    - It writes the declaration for the setup function using the setup name from the options hash.
    - It writes the declaration for the teardown function using the teardown name from the options hash.
    - The method iterates over each test in the tests array, writing a declaration for each test function using its name and call signature.
    - Finally, it writes an empty line to the output.
- **Output**: The method does not return any value; it writes directly to the provided output stream.


---
#### UnityTestRunnerGenerator\.create\_header
The `create_header` method generates and writes a header section for a test runner file, including necessary includes and configuration based on provided options and mocks.
- **Inputs**:
    - `output`: An IO-like object where the header content will be written.
    - `mocks`: An array of mock header file names that need to be included in the generated header.
    - `testfile_includes`: An optional array of additional include files specific to the test file.
- **Control Flow**:
    - Writes a comment indicating the file is autogenerated.
    - Calls `create_runtest` to generate the test runner function.
    - Writes a section for automatically detected include files.
    - Includes platform-specific setup stubs for Windows.
    - Includes the main framework header file specified in options.
    - Conditionally includes 'cmock.h' if there are mocks.
    - Includes standard C headers like 'setjmp.h' and 'stdio.h'.
    - Defines any macros specified in the options.
    - Includes a specific header file if specified in options, otherwise includes headers from options and testfile_includes.
    - Includes each mock header file.
    - Includes 'CException.h' if the cexception plugin is enabled.
    - If strict ordering is enforced, declares global variables for test order verification.
- **Output**: The method does not return any value; it writes the generated header content directly to the provided output object.


---
#### UnityTestRunnerGenerator\.create\_main
The `create_main` method generates the main function for a test runner, handling command-line arguments and executing test cases.
- **Inputs**:
    - `output`: An IO-like object where the generated main function code will be written.
    - `filename`: The name of the C file for which the test runner is being generated.
    - `tests`: An array of hashes, each representing a test case with details like test name, arguments, and line number.
    - `used_mocks`: An array of mock headers used in the test, which affects memory management in the generated code.
- **Control Flow**:
    - The method starts by writing a comment header for the main function to the output.
    - It determines the main function's name based on the `@options[:main_name]` setting, defaulting to 'main' or a filename-based name if set to :auto.
    - If command-line arguments are enabled (`@options[:cmdline_args]` is true), it writes a function declaration for the main function with `argc` and `argv` parameters.
    - The method writes the main function definition, handling command-line argument parsing with `UnityParseOptions`.
    - If parsing fails, it prints test names and returns 0 if the status is negative, or returns the parse status otherwise.
    - If command-line arguments are not used, it writes a simpler main function definition without parameters.
    - The method writes code to set up the test suite and begin Unity testing with `UnityBegin`.
    - It iterates over the `tests` array, writing code to run each test with `RUN_TEST`, handling parameterized tests if enabled.
    - If mocks are used, it writes a call to `CMock_Guts_MemFreeFinal` to free memory.
    - Finally, it writes code to return the result of `suite_teardown` after calling `UnityEnd`.
- **Output**: The method outputs C code for a main function that sets up and runs a series of tests, handling command-line arguments and memory management for mocks if applicable.


---
#### UnityTestRunnerGenerator\.create\_mock\_management
The `create_mock_management` method generates C code for initializing, verifying, and destroying mock objects based on provided mock headers.
- **Inputs**:
    - `output`: An IO-like object where the generated C code will be written.
    - `mock_headers`: An array of strings representing the file paths of mock header files.
- **Control Flow**:
    - The method first checks if `mock_headers` is empty and returns immediately if it is.
    - It writes a comment header for mock management to the `output`.
    - It defines a static C function `CMock_Init` and writes its opening brace to the `output`.
    - If the `@options[:enforce_strict_ordering]` is true, it initializes global variables for order enforcement.
    - It maps `mock_headers` to their base filenames and iterates over them.
    - For each mock, it sanitizes the filename to a valid C identifier and writes an initialization call to the `output`.
    - It closes the `CMock_Init` function definition.
    - It defines and writes the `CMock_Verify` function, iterating over mocks to write verification calls.
    - It defines and writes the `CMock_Destroy` function, iterating over mocks to write destruction calls.
- **Output**: The method outputs C code to the `output` object, which includes functions for initializing, verifying, and destroying mock objects.


---
#### UnityTestRunnerGenerator\.create\_reset
The `create_reset` method generates C code for a test reset function, which includes mock verification, destruction, initialization, and setup/teardown calls.
- **Inputs**:
    - `output`: An IO-like object where the generated C code will be written.
    - `used_mocks`: An array of mock objects that are used in the test, which determines whether certain mock-related code is included in the output.
- **Control Flow**:
    - The method begins by writing a comment and function declaration for `resetTest` to the output.
    - It writes the function definition for `resetTest`, opening with a curly brace.
    - If `used_mocks` is not empty, it writes calls to `CMock_Verify` and `CMock_Destroy` to the output.
    - It writes a call to the teardown function, using the name from `@options[:teardown_name]`.
    - If `used_mocks` is not empty, it writes a call to `CMock_Init`.
    - It writes a call to the setup function, using the name from `@options[:setup_name]`.
    - The function definition is closed with a curly brace.
- **Output**: The method outputs C code for a `resetTest` function, which includes conditional mock management and setup/teardown calls.


---
#### UnityTestRunnerGenerator\.create\_runtest
The `create_runtest` method generates a C macro definition for running unit tests, handling setup, teardown, and exception management based on provided options and used mocks.
- **Inputs**:
    - `output`: An IO-like object where the generated test runner code will be written.
    - `used_mocks`: An array of strings representing the mock headers used in the test file.
- **Control Flow**:
    - Check if the :cexception plugin is included in the options to determine if exception handling code should be generated.
    - Determine if parameterized tests are used by checking the :use_param_tests option, and set variable argument strings accordingly.
    - Write the initial comment and macro definition for the test runner to the output.
    - If parameterized tests are used, define a macro for tests with no arguments.
    - Write the macro definition for RUN_TEST, including the test function name and line number, and handle variable arguments if applicable.
    - If command line arguments are enabled, wrap the test execution in a conditional block that checks if the test should be run.
    - Increment the test count and initialize mock management if mocks are used.
    - Clear Unity test details if mocks are used.
    - Wrap the test execution in a TEST_PROTECT block to handle setup, test execution, and teardown safely.
    - If :cexception is enabled, wrap the test execution in a Try-Catch block to handle exceptions.
    - Call the setup function, execute the test function, and handle any exceptions if :cexception is enabled.
    - Call the teardown function and verify mocks if used, within a TEST_PROTECT block.
    - Destroy mock objects if used after the test execution.
    - Conclude the test execution with UnityConcludeTest.
    - Close the conditional block for command line argument checks if applicable.
- **Output**: The method outputs C code to the provided output object, defining a macro for running unit tests with setup, teardown, and optional exception handling.


---
#### UnityTestRunnerGenerator\.create\_suite\_setup
The `create_suite_setup` method generates C code for a suite setup function in a test runner file.
- **Inputs**:
    - `output`: An IO-like object where the generated C code for the suite setup will be written.
- **Control Flow**:
    - The method begins by writing a comment and the function signature for the suite setup to the output.
    - It checks if the `:suite_setup` option is nil in the `@options` hash.
    - If `:suite_setup` is nil, it writes a conditional compilation block to call `suiteSetUp()` if weak symbols are supported.
    - If `:suite_setup` is not nil, it writes the C code provided in the `:suite_setup` option directly to the output.
    - The method concludes by writing the closing brace for the function.
- **Output**: The method outputs C code for a suite setup function to the provided `output` object.


---
#### UnityTestRunnerGenerator\.create\_suite\_teardown
The `create_suite_teardown` method generates C code for a suite teardown function, handling both new and old styles based on configuration options.
- **Inputs**:
    - `output`: An IO-like object where the generated C code for the suite teardown function will be written.
- **Control Flow**:
    - The method begins by writing a comment and the function signature for the suite teardown to the output.
    - It checks if the `:suite_teardown` option is nil to determine whether to use new or old style teardown.
    - If the `:suite_teardown` option is nil, it writes conditional compilation directives to call `suiteTearDown(num_failures)` if weak symbols are supported, otherwise it returns `num_failures`.
    - If the `:suite_teardown` option is not nil, it writes the C code embedded in the `:suite_teardown` option directly to the output.
    - The method concludes by writing the closing brace for the function.
- **Output**: The method outputs C code for a suite teardown function to the provided output object, which can be a file or any IO-like object.


---
#### UnityTestRunnerGenerator\.find\_includes
The `find_includes` method processes a source code string to remove comments and extract include directives, categorizing them into local, system, and link-only includes.
- **Inputs**:
    - `source`: A string containing the source code to be processed.
- **Control Flow**:
    - Remove line comments that might comment out the start of block comments using a regular expression.
    - Remove block comments using a regular expression that matches multi-line block comments.
    - Remove remaining line comments using a regular expression.
    - Extract local include directives using a regular expression that matches `#include` with double quotes and a `.h` or `.H` extension, and store them in the `local` key of the `includes` hash.
    - Extract system include directives using a regular expression that matches `#include` with angle brackets, and store them in the `system` key of the `includes` hash, wrapping each match in angle brackets.
    - Extract link-only include directives using a regular expression that matches `TEST_FILE` with double quotes and a `.c` or `.C` extension, and store them in the `linkonly` key of the `includes` hash.
    - Return the `includes` hash containing categorized include directives.
- **Output**: A hash with keys `:local`, `:system`, and `:linkonly`, each containing an array of strings representing the respective include directives found in the source code.


---
#### UnityTestRunnerGenerator\.find\_mocks
The `find_mocks` method filters a list of include paths to identify and return those that match a specified mock prefix.
- **Inputs**:
    - `includes`: An array of file paths representing include files.
- **Control Flow**:
    - Initialize an empty array `mock_headers` to store paths of mock headers.
    - Iterate over each `include_path` in the `includes` array.
    - For each `include_path`, extract the file name using `File.basename`.
    - Check if the file name matches the mock prefix pattern defined in `@options[:mock_prefix]` using a case-insensitive regular expression.
    - If the file name matches the pattern, add the `include_path` to the `mock_headers` array.
    - Return the `mock_headers` array containing paths of mock headers.
- **Output**: An array of file paths that match the mock prefix pattern.


---
#### UnityTestRunnerGenerator\.find\_tests
The `find_tests` method identifies and extracts test functions from a given C source code, along with their line numbers and associated test case arguments.
- **Inputs**:
    - `source`: A string containing the C source code from which test functions are to be extracted.
- **Control Flow**:
    - Initialize an empty array `tests_and_line_numbers` to store test details.
    - Clone the `source` and remove string literals, line comments, and block comments to create `source_scrubbed`.
    - Split `source_scrubbed` into logical lines using a regular expression that treats preprocessor directives and certain symbols as line delimiters.
    - Iterate over each line to find test functions using a regular expression that matches the test function signature and optional test case arguments.
    - For each matched test function, extract the test name, arguments, call signature, and parameters, and store them in `tests_and_line_numbers`.
    - Remove duplicate test entries based on the test name.
    - Split the original `source` into lines and iterate over them to find the line number of each test function by matching the test name.
    - Update the `line_number` for each test in `tests_and_line_numbers` with the correct line number from the source.
    - Return the `tests_and_line_numbers` array containing details of all identified test functions.
- **Output**: An array of hashes, each containing details of a test function: `test` (name), `args` (test case arguments), `call` (call signature), `params` (parameters), and `line_number` (line number in the source code).


---
#### UnityTestRunnerGenerator\.generate
The `generate` method creates a test runner file and optionally a header file based on the provided input file, tests, mocks, and includes.
- **Inputs**:
    - `input_file`: The path to the input C file for which the test runner is being generated.
    - `output_file`: The path to the output file where the generated test runner will be written.
    - `tests`: An array of test definitions extracted from the input file, each containing details like test name, arguments, and line number.
    - `used_mocks`: An array of mock headers that are used in the test runner.
    - `testfile_includes`: An array of include files that are required by the test file, excluding mocks.
- **Control Flow**:
    - Opens the output file for writing and passes the file handle to a block.
    - Calls `create_header` to write the header section of the test runner file.
    - Calls `create_externs` to declare external functions used by the test runner.
    - Calls `create_mock_management` to handle mock initialization, verification, and destruction if mocks are used.
    - Calls `create_suite_setup` and `create_suite_teardown` to define setup and teardown functions for the test suite.
    - Calls `create_reset` to define a function for resetting the test environment.
    - Calls `create_main` to define the main function of the test runner, which executes the tests.
    - Checks if a header file option is specified and not empty, then opens the header file for writing and calls `create_h_file` to generate it.
- **Output**: The method does not return any value; it generates files as a side effect.


---
#### UnityTestRunnerGenerator\.initialize
The `initialize` method sets up a new instance of the `UnityTestRunnerGenerator` class with default options, optionally merging in additional configuration from a file or hash.
- **Inputs**:
    - `options`: An optional parameter that can be a string representing a filename, a hash of options, or nil.
- **Control Flow**:
    - Initialize the `@options` instance variable with default options from `UnityTestRunnerGenerator.default_options`.
    - Check the type of `options` parameter using a case statement.
    - If `options` is `NilClass`, do nothing further as `@options` is already set to default.
    - If `options` is a `String`, treat it as a filename and merge the configuration from the file into `@options` using `UnityTestRunnerGenerator.grab_config`.
    - If `options` is a `Hash`, merge it directly into `@options`.
    - If `options` is of any other type, raise an error indicating that the argument should be a filename or a hash.
    - Require the `type_sanitizer` file from the current directory.
- **Output**: The method does not return any value; it initializes the instance with the appropriate configuration options.


---
#### UnityTestRunnerGenerator\.run
The `run` method processes a C source file to generate a test runner file and returns a list of all files used in the process.
- **Inputs**:
    - `input_file`: The path to the C source file that needs to be processed to generate a test runner.
    - `output_file`: The path where the generated test runner file will be saved.
    - `options`: An optional hash of configuration options that can override the default settings.
- **Control Flow**:
    - Merge the provided options with the existing options if any are provided.
    - Read the content of the input C source file and convert its encoding to UTF-8.
    - Extract test cases and include headers from the source file using `find_tests` and `find_includes` methods.
    - Identify and separate mock headers from the include headers using `find_mocks`.
    - Remove any Unity or CMock related headers from the list of includes.
    - Generate the test runner file using the `generate` method with the extracted data.
    - Compile a list of all files used, including the input file, output file, and any additional includes or link-only headers.
    - Return a unique list of all files used in the process.
- **Output**: A unique array of file paths representing all files used during the test runner generation process.



