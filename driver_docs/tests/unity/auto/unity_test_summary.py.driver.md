# Purpose
This Python script is designed to serve as a test summary generator for Unity, a test framework for C. The script processes test result files, which are expected to be in a specific format, and generates a summary report detailing the number of tests run, the number of failures, and the number of ignored tests. The script is structured as a command-line utility, allowing users to specify the directory containing the test result files and an optional root path for more verbose output. The main class, `UnityTestSummary`, encapsulates the functionality needed to parse the test results, aggregate the data, and produce a formatted report.

The script is intended to be executed directly and not as an importable module, as indicated by the `if __name__ == '__main__':` block. It uses standard Python libraries such as `sys`, `os`, `re`, and `glob` to handle file operations and regular expression parsing. The script reads test result files, extracts relevant information about test outcomes, and compiles this data into a human-readable summary. This summary includes sections for ignored and failed tests, as well as an overall test summary. The script is particularly useful for developers using the Unity test framework to quickly assess the results of their test suites and identify areas that require attention.
# Imports and Dependencies

---
- `sys`
- `os`
- `re`
- `glob.glob`


# Classes

---
### UnityTestSummary<!-- {{#class:cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary}} -->
- **Members**:
    - `report`: A string that accumulates the summary report of the test results.
    - `total_tests`: An integer that tracks the total number of tests processed.
    - `failures`: An integer that counts the total number of failed tests.
    - `ignored`: An integer that counts the total number of ignored tests.
- **Description**: The UnityTestSummary class is designed to process and summarize test results from Unity test framework output files. It accumulates data on the total number of tests, failures, and ignored tests, and generates a comprehensive report detailing the results. The class provides methods to set the target files and root path, parse test summaries, and extract detailed information from test result files. It is intended to be used as a command-line tool to facilitate the analysis of test outcomes in a structured and readable format.
- **Methods**:
    - [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.__init__`](#UnityTestSummary__init__)
    - [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.run`](#UnityTestSummaryrun)
    - [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.set_targets`](#UnityTestSummaryset_targets)
    - [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.set_root_path`](#UnityTestSummaryset_root_path)
    - [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.usage`](#UnityTestSummaryusage)
    - [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.get_details`](#UnityTestSummaryget_details)
    - [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.parse_test_summary`](#UnityTestSummaryparse_test_summary)

**Methods**

---
#### UnityTestSummary\.\_\_init\_\_<!-- {{#callable:cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.__init__}} -->
The `__init__` method initializes a `UnityTestSummary` object with default values for report, total tests, failures, and ignored tests.
- **Inputs**: None
- **Control Flow**:
    - The method initializes the `report` attribute as an empty string.
    - The method sets `total_tests`, `failures`, and `ignored` attributes to 0.
- **Output**: The method does not return any value; it initializes the object's state.
- **See also**: [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary`](#cJSON/tests/unity/auto/unity_test_summaryUnityTestSummary)  (Base Class)


---
#### UnityTestSummary\.run<!-- {{#callable:cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.run}} -->
The `run` method processes test result files to generate a summary report of total tests, failures, and ignored tests.
- **Inputs**: None
- **Control Flow**:
    - Initialize an empty list `results` to store cleaned-up file paths.
    - Iterate over `self.targets` to replace backslashes with forward slashes in file paths and append to `results`.
    - Initialize `failure_output` and `ignore_output` lists to store failure and ignore details respectively.
    - Iterate over each `result_file` in `results`, read and clean up lines from the file.
    - Raise an exception if a result file is empty.
    - Call [`get_details`](#UnityTestSummaryget_details) to extract failures and ignores from the file lines and append them to `failure_output` and `ignore_output` if they exist.
    - Call [`parse_test_summary`](#UnityTestSummaryparse_test_summary) to extract the number of tests, failures, and ignored tests from the file lines and update the respective counters.
    - If there are ignored tests, append an ignored test summary to `self.report`.
    - If there are failures, append a failed test summary to `self.report`.
    - Append an overall test summary to `self.report`.
    - Return the complete `self.report`.
- **Output**: A string containing the formatted test summary report, including details on total tests, failures, and ignored tests.
- **Functions called**:
    - [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.get_details`](#UnityTestSummaryget_details)
    - [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.parse_test_summary`](#UnityTestSummaryparse_test_summary)
- **See also**: [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary`](#cJSON/tests/unity/auto/unity_test_summaryUnityTestSummary)  (Base Class)


---
#### UnityTestSummary\.set\_targets<!-- {{#callable:cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.set_targets}} -->
The `set_targets` method assigns a list of target file paths to the `targets` attribute of the `UnityTestSummary` class.
- **Inputs**:
    - `target_array`: A list of file paths that are intended to be used as targets for processing within the `UnityTestSummary` class.
- **Control Flow**:
    - The method takes a single argument, `target_array`, which is expected to be a list of file paths.
    - It assigns the provided `target_array` to the `targets` attribute of the `UnityTestSummary` instance.
- **Output**: The method does not return any value; it simply sets the `targets` attribute.
- **See also**: [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary`](#cJSON/tests/unity/auto/unity_test_summaryUnityTestSummary)  (Base Class)


---
#### UnityTestSummary\.set\_root\_path<!-- {{#callable:cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.set_root_path}} -->
The `set_root_path` method sets the root path for the `UnityTestSummary` instance.
- **Inputs**:
    - `path`: The path to be set as the root for the `UnityTestSummary` instance.
- **Control Flow**:
    - Assigns the provided `path` argument to the `root` attribute of the `UnityTestSummary` instance.
- **Output**: This method does not return any value.
- **See also**: [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary`](#cJSON/tests/unity/auto/unity_test_summaryUnityTestSummary)  (Base Class)


---
#### UnityTestSummary\.usage<!-- {{#callable:cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.usage}} -->
The `usage` method prints an error message and usage instructions for the UnityTestSummary script, then exits the program.
- **Inputs**:
    - `err_msg`: An optional error message to be printed before the usage instructions.
- **Control Flow**:
    - Prints a newline followed by 'ERROR: '.
    - If `err_msg` is provided, it prints the error message.
    - Prints the usage instructions for the script, detailing the expected arguments and their descriptions.
    - Calls `sys.exit(1)` to terminate the program with a status code of 1, indicating an error.
- **Output**: The method does not return any value; it exits the program with a status code of 1.
- **See also**: [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary`](#cJSON/tests/unity/auto/unity_test_summaryUnityTestSummary)  (Base Class)


---
#### UnityTestSummary\.get\_details<!-- {{#callable:cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.get_details}} -->
The `get_details` method processes lines from a test result file to categorize them into failures, ignores, and successes based on their status.
- **Inputs**:
    - `result_file`: The name of the result file being processed, though it is not directly used in the method.
    - `lines`: A list of strings, each representing a line from the result file, which contains test details separated by colons.
- **Control Flow**:
    - Initialize a dictionary `results` with keys 'failures', 'ignores', and 'successes', each associated with an empty list.
    - Iterate over each line in the `lines` list.
    - Split each line by colons into a list `parts`.
    - Check the length of `parts`; if it is 5, unpack into `src_file`, `src_line`, `test_name`, `status`, and `msg`. If it is 4, unpack into `src_file`, `src_line`, `test_name`, and `status`, setting `msg` to an empty string. If neither, skip the line.
    - Construct `line_out` by prepending `self.root` to the line if `self.root` is non-empty, otherwise use the line as is.
    - Append `line_out` to the appropriate list in `results` based on the `status` value ('IGNORE', 'FAIL', or 'PASS').
- **Output**: A dictionary with keys 'failures', 'ignores', and 'successes', each containing a list of lines categorized by their test status.
- **See also**: [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary`](#cJSON/tests/unity/auto/unity_test_summaryUnityTestSummary)  (Base Class)


---
#### UnityTestSummary\.parse\_test\_summary<!-- {{#callable:cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary.parse_test_summary}} -->
The `parse_test_summary` method extracts and returns the number of tests, failures, and ignored tests from a summary string using regular expressions.
- **Inputs**:
    - `summary`: A string containing the test summary, expected to match the pattern of 'X Tests Y Failures Z Ignored' where X, Y, and Z are integers.
- **Control Flow**:
    - Use a regular expression to search for a pattern in the input string that matches 'X Tests Y Failures Z Ignored'.
    - If the pattern is not found, raise an exception indicating the summary could not be parsed.
    - If the pattern is found, extract the numbers of tests, failures, and ignored tests from the matched groups.
    - Convert the extracted numbers from strings to integers.
- **Output**: A tuple containing three integers: the number of tests, the number of failures, and the number of ignored tests.
- **See also**: [`cJSON/tests/unity/auto/unity_test_summary.UnityTestSummary`](#cJSON/tests/unity/auto/unity_test_summaryUnityTestSummary)  (Base Class)



