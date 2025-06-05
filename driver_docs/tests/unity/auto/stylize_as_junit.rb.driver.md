# Purpose
The provided Ruby script, `unity_to_junit.rb`, is designed to convert test results from Unity test framework files into a JUnit-compatible XML format. This script is particularly useful for developers who need to integrate Unity test results into systems that consume JUnit XML, such as continuous integration (CI) tools. The script processes test result files, extracting details about test successes, failures, and ignored tests, and then formats this information into an XML structure that adheres to the JUnit schema. This allows for seamless integration with tools that expect JUnit output, facilitating better test reporting and analysis.

The script is composed of two main classes: `ArgvParser` and `UnityToJUnit`. The `ArgvParser` class handles command-line argument parsing, allowing users to specify directories for test results, root paths, and output filenames. The `UnityToJUnit` class is responsible for reading the test result files, parsing the test outcomes, and generating the XML output. It includes methods for writing various parts of the XML document, such as headers, test cases, and footers. The script also includes error handling to ensure that users are informed of any issues, such as missing test result files. Overall, this script provides a focused functionality aimed at bridging the gap between Unity test results and JUnit-compatible systems.
# Imports and Dependencies

---
- `fileutils`
- `optparse`
- `ostruct`
- `set`
- `pp`


# Classes

---
### ArgvParser
- **Description**: The `ArgvParser` class is designed to handle command-line argument parsing for a Ruby script, specifically for the `unity_to_junit.rb` script. It utilizes the `OptionParser` library to define and parse various command-line options, such as specifying directories for results, setting a root path, and defining an output file name. The class provides a `parse` class method that returns an `OpenStruct` object containing the parsed options with default values set for each option. This class is essential for configuring the behavior of the script based on user input from the command line.

**Class Methods**

---
#### ArgvParser\.parse
The `parse` method processes command-line arguments to configure options for a script, returning an OpenStruct with the specified or default values.
- **Inputs**:
    - `args`: An array of command-line arguments passed to the script.
- **Control Flow**:
    - Initialize an OpenStruct `options` with default values for `results_dir`, `root_path`, and `out_file`.
    - Create an OptionParser instance `opts` to define command-line options and their behavior.
    - Define specific options for results directory, root path, and output file, updating `options` accordingly when these options are provided.
    - Define common options for displaying help and version information, which terminate the program when invoked.
    - Parse the provided `args` using the `opts` parser, updating `options` based on the parsed arguments.
    - Return the `options` OpenStruct containing the configured options.
- **Output**: An OpenStruct object containing the parsed command-line options with their respective values.



---
### UnityToJUnit
- **Description**: The `UnityToJUnit` class is designed to convert test results from Unity test framework format into JUnit XML format. It processes test result files, extracting details about test cases, including their pass, fail, and ignore statuses, and then writes this information into a structured XML file that conforms to the JUnit format. This class includes methods for parsing test summaries, handling file operations, and generating XML output. It also provides a command-line interface for specifying input directories, root paths, and output files, making it a versatile tool for integrating Unity test results into systems that utilize JUnit XML for reporting.
- **Includes**:
    - FileUtils::Verbose

**Attributes**

---
#### UnityToJUnit\.failures
- **Type**: `Integer`
- **Description**: The `failures` attribute is an integer that represents the number of test cases that have failed during the execution of a test suite. It is part of the test summary that is parsed from the test result files.
- **Use**: This attribute is used to keep track of the number of failed tests and is included in the XML output to provide a summary of the test results.


---
#### UnityToJUnit\.ignored
- **Type**: `Integer`
- **Description**: The `ignored` attribute is an integer that represents the number of tests that were ignored during the test run. It is part of the test summary that includes total tests, failures, and ignored tests.
- **Use**: This attribute is used to track and report the count of ignored tests in the test results summary.


---
#### UnityToJUnit\.out\_file=
- **Type**: `String`
- **Description**: The `out_file` attribute is a writable attribute that specifies the name of the output file where the XML results will be written. It is used in the `UnityToJUnit` class to create and write the test results in XML format.
- **Use**: This attribute is used to set the filename for the output XML file that stores the test results.


---
#### UnityToJUnit\.report
- **Type**: `String`
- **Description**: The `report` attribute is a string that is initialized as an empty string in the `initialize` method of the `UnityToJUnit` class. It is intended to store or represent some form of report data, although its specific use is not detailed in the provided code.
- **Use**: The `report` attribute is used to hold report data, but its specific usage is not shown in the provided code.


---
#### UnityToJUnit\.root=
- **Type**: `attr_writer`
- **Description**: The `root=` attribute is a writer method for setting the `@root` instance variable in the `UnityToJUnit` class. This attribute allows the user to specify a root path that will be prepended to file paths in the results. It is used to configure the base directory for file operations within the class.
- **Use**: This attribute is used to set the root path for file operations, ensuring that file paths are correctly resolved relative to a specified base directory.


---
#### UnityToJUnit\.targets=
- **Type**: `Array<String>`
- **Description**: The `targets` attribute is an array of strings, where each string represents a file path to a test result file. These file paths are used to read and process test results for conversion into a JUnit-compatible XML format.
- **Use**: This attribute is used to store and iterate over the list of test result files that need to be processed and converted into XML format.


---
#### UnityToJUnit\.total\_tests
- **Type**: `Integer`
- **Description**: The `total_tests` attribute is an integer that represents the total number of tests that have been executed. It is part of the `UnityToJUnit` class, which processes test results and generates a JUnit-compatible XML report.
- **Use**: This attribute is used to store and provide access to the total count of tests processed during the execution of the `run` method.


**Instance Methods**

---
#### UnityToJUnit\.get\_details
The `get_details` method processes lines of test result data, categorizing them into successes, failures, and ignores based on their status, and returns a structured hash of these results.
- **Inputs**:
    - `_result_file`: A placeholder argument representing the result file name, which is not used in the method.
    - `lines`: An array of strings, each representing a line from a test result file, containing test details separated by colons.
- **Control Flow**:
    - Initialize a results hash using the `results_structure` method.
    - Iterate over each line in the `lines` array.
    - For each line, replace backslashes with forward slashes.
    - Split the line into components: `_src_file`, `src_line`, `test_name`, `status`, and `msg` using a colon as the delimiter.
    - Use a case statement to check the `status` of the test.
    - If the status is 'IGNORE', append a hash with test details to the `:ignores` array in the results hash.
    - If the status is 'FAIL', append a hash with test details to the `:failures` array in the results hash.
    - If the status is 'PASS', append a hash with test details to the `:successes` array in the results hash.
    - Return the populated results hash.
- **Output**: A hash containing categorized test results with keys `:successes`, `:failures`, `:ignores`, and other metadata initialized by `results_structure`.


---
#### UnityToJUnit\.here
The `here` method returns the absolute path of the directory where the current file is located.
- **Inputs**: None
- **Control Flow**:
    - The method calls `File.dirname(__FILE__)` to get the directory name of the current file.
    - It then calls `File.expand_path` with the directory name to convert it to an absolute path.
    - The method returns the absolute path.
- **Output**: The method outputs a string representing the absolute path of the directory containing the current file.


---
#### UnityToJUnit\.initialize
The `initialize` method sets up a new instance of the `UnityToJUnit` class by initializing the `@report` and `@unit_name` instance variables to empty strings.
- **Inputs**: None
- **Control Flow**:
    - The method initializes the `@report` instance variable to an empty string.
    - The method initializes the `@unit_name` instance variable to an empty string.
- **Output**: The method does not return any value; it initializes instance variables for a new object.


---
#### UnityToJUnit\.parse\_test\_summary
The `parse_test_summary` method extracts the number of tests, failures, and ignored tests from a summary string.
- **Inputs**:
    - `summary`: An array of strings representing the test summary, where each string may contain information about the number of tests, failures, and ignored tests.
- **Control Flow**:
    - The method checks if any string in the `summary` array matches the regular expression pattern `/\(\d+\) Tests \(\d+\) Failures \(\d+\) Ignored/` to find a line containing test results.
    - If no match is found, the method raises an exception with a message indicating that the test results could not be parsed.
    - If a match is found, the method uses `Regexp.last_match` to extract the number of tests, failures, and ignored tests from the matched string.
    - The extracted numbers are converted to integers and returned as an array.
- **Output**: An array of three integers representing the number of tests, failures, and ignored tests, respectively.


---
#### UnityToJUnit\.results\_structure
The `results_structure` method initializes and returns a hash structure to store test results, including source information, test outcomes, and counts.
- **Inputs**: None
- **Control Flow**:
    - The method defines a hash with keys for `source`, `successes`, `failures`, `ignores`, `counts`, and `stdout`.
    - The `source` key is associated with a nested hash containing empty strings for `path` and `file`.
    - The `successes`, `failures`, and `ignores` keys are associated with empty arrays.
    - The `counts` key is associated with a nested hash initialized with zero values for `total`, `passed`, `failed`, and `ignored`.
    - The `stdout` key is associated with an empty array.
- **Output**: A hash structure with initialized keys for storing test results and related metadata.


---
#### UnityToJUnit\.run
The `run` method processes test result files, extracts test details, and writes them into an XML file in JUnit format.
- **Inputs**:
    - `@targets`: An array of file paths representing the test result files to be processed.
    - `@out_file`: The file path where the output XML file will be written.
- **Control Flow**:
    - Convert backslashes to forward slashes in each target file path.
    - Open the output file for writing and write the XML header and suites header.
    - Iterate over each result file path in the `results` array.
    - Read all lines from the current result file and remove trailing newline characters.
    - Raise an error if the result file is empty.
    - Call `get_details` to extract test details from the lines and store them in `result_output`.
    - Call `parse_test_summary` to extract test summary counts and update `result_output` with these counts.
    - Determine the test file path and name from the first line of the result file and update `result_output` with this information.
    - Set `@unit_name` to the base name of the test file without extension.
    - Write the suite header, failures, tests, ignored tests, and suite footer to the output file using the data in `result_output`.
    - After processing all result files, write the suites footer and close the output file.
- **Output**: The method writes an XML file in JUnit format containing the test results, including test counts, failures, successes, and ignored tests.


---
#### UnityToJUnit\.usage
The `usage` method displays an error message and usage instructions for the script, then exits the program.
- **Inputs**:
    - `err_msg`: An optional error message to be displayed before the usage instructions.
- **Control Flow**:
    - Prints a newline followed by 'ERROR: '.
    - If `err_msg` is provided, it prints the error message.
    - Prints the usage instructions for the script, detailing specific and common options available.
    - Exits the program with a status code of 1.
- **Output**: The method does not return a value; it exits the program after printing the error message and usage instructions.


---
#### UnityToJUnit\.write\_failures
The `write_failures` method writes XML-formatted failure details of test cases to a given output stream.
- **Inputs**:
    - `results`: A hash containing test results, specifically a key `:failures` which is an array of failure details, and a key `:source` which contains the path and file information of the test source.
    - `stream`: An output stream object where the XML-formatted failure details will be written.
- **Control Flow**:
    - Extract the `:failures` array from the `results` hash.
    - Iterate over each failure item in the `:failures` array.
    - For each failure item, construct the filename using the `:source` path and file from the `results` hash.
    - Write an XML `<testcase>` element to the `stream` with attributes for the class name, test name, and time.
    - Within the `<testcase>` element, write a `<failure>` element with a message and type attribute.
    - Write a `<system-err>` element containing the file and line number information of the failure.
    - Close the `<testcase>` element.
- **Output**: The method outputs XML-formatted strings to the provided `stream`, detailing each test failure with its associated message, file, and line number.


---
#### UnityToJUnit\.write\_ignored
The `write_ignored` method writes XML entries for ignored test cases to a given output stream.
- **Inputs**:
    - `results`: A hash containing test results, specifically the ignored tests under the key `:ignores`, and source file information under `:source`.
    - `stream`: An output stream object where the XML entries for ignored tests will be written.
- **Control Flow**:
    - Extracts the ignored test cases from the `results` hash using the key `:ignores`.
    - Iterates over each ignored test case item.
    - For each item, constructs the filename by joining the source path and the base name of the source file without its extension.
    - Prints a message to the console indicating the filename for which ignored tests are being written.
    - Writes an XML `<testcase>` entry to the `stream` for each ignored test, including the test name, a `<skipped>` tag with a message, and a `<system-err>` tag with file and line information.
- **Output**: The method does not return a value; it writes XML formatted data to the provided `stream`.


---
#### UnityToJUnit\.write\_suite\_footer
The `write_suite_footer` method writes the closing tag for a test suite in an XML format to a given output stream.
- **Inputs**:
    - `stream`: An output stream object where the XML closing tag for a test suite will be written.
- **Control Flow**:
    - The method takes a single argument, `stream`, which is expected to be an output stream object.
    - It writes the string "\t</testsuite>" to the provided stream, which represents the closing tag of a test suite in XML format.
- **Output**: The method does not return any value; it performs an action by writing to the provided stream.


---
#### UnityToJUnit\.write\_suite\_header
The `write_suite_header` method writes an XML header for a test suite to a given stream, using test count data.
- **Inputs**:
    - `counts`: A hash containing test count data with keys :ignored, :failed, and :total, representing the number of ignored, failed, and total tests respectively.
    - `stream`: An IO-like object where the XML header for the test suite will be written.
- **Control Flow**:
    - The method constructs an XML string for a test suite header using the values from the counts hash.
    - It writes this XML string to the provided stream using the `puts` method.
- **Output**: The method does not return a value; it writes an XML-formatted string to the provided stream.


---
#### UnityToJUnit\.write\_suites\_footer
The `write_suites_footer` method writes the closing XML tag for test suites to a given output stream.
- **Inputs**:
    - `stream`: An output stream object where the closing XML tag for test suites will be written.
- **Control Flow**:
    - The method takes a single argument, `stream`, which is expected to be an output stream object.
    - It writes the string '</testsuites>' to the provided stream using the `puts` method.
- **Output**: The method does not return any value; it performs an action by writing to the provided stream.


---
#### UnityToJUnit\.write\_suites\_header
The `write_suites_header` method writes the opening XML tag for test suites to a given output stream.
- **Inputs**:
    - `stream`: An output stream object where the XML header for test suites will be written.
- **Control Flow**:
    - The method takes a single argument, `stream`, which is expected to be an output stream.
    - It writes the string '<testsuites>' to the provided stream using the `puts` method.
- **Output**: The method does not return any value; it performs an action by writing to the provided stream.


---
#### UnityToJUnit\.write\_tests
The `write_tests` method writes XML elements for successful test cases to a given output stream.
- **Inputs**:
    - `results`: A hash containing test results, specifically the key `:successes` which holds an array of successful test case details.
    - `stream`: An output stream object where the XML elements for successful test cases will be written.
- **Control Flow**:
    - Extracts the array of successful test cases from the `results` hash using the key `:successes`.
    - Iterates over each successful test case in the array.
    - For each test case, writes an XML `<testcase>` element to the `stream`, including the class name, test name, and a fixed time attribute.
- **Output**: The method does not return any value; it writes XML elements directly to the provided `stream`.


---
#### UnityToJUnit\.write\_xml\_header
The `write_xml_header` method writes an XML declaration to a given output stream.
- **Inputs**:
    - `stream`: An output stream object where the XML header will be written.
- **Control Flow**:
    - The method takes a single argument, `stream`, which is expected to be an object that responds to the `puts` method.
    - It writes the XML declaration `<?xml version='1.0' encoding='utf-8' ?>` to the provided stream using the `puts` method.
- **Output**: The method does not return any value; it performs an action by writing to the provided stream.



