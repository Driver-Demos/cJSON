# Purpose
The provided Ruby code defines a module named `RakefileHelpers`, which serves as a utility toolkit for managing the build and test processes of C projects. This module provides a collection of methods that facilitate the configuration, compilation, linking, and testing of C source files. It leverages YAML configuration files to dynamically load and apply settings for compilers, linkers, and simulators, ensuring that the build process is adaptable to different environments and requirements. The module also includes methods for cleaning build directories, extracting header dependencies, and generating test runners, which are essential for maintaining a clean and efficient build process.

The code integrates with external tools and libraries, such as Unity for unit testing, and uses file manipulation utilities to handle source and object files. It defines public APIs for configuring toolchains, running tests, and building applications, making it a versatile component in a larger build system. The module's design emphasizes modularity and reusability, allowing it to be easily integrated into Rake-based build systems. By abstracting common build tasks into reusable methods, `RakefileHelpers` streamlines the development workflow, enabling developers to focus on writing and testing code rather than managing the intricacies of the build process.
# Imports and Dependencies

---
- `yaml`
- `fileutils`
- `UNITY_ROOT + '/auto/unity_test_summary'`
- `UNITY_ROOT + '/auto/generate_test_runner'`
- `UNITY_ROOT + '/auto/colour_reporter'`


# Modules

---
### RakefileHelpers
- **Description**: The `RakefileHelpers` module provides a comprehensive set of methods to facilitate the configuration, compilation, linking, and testing of C projects using Rake. It includes functionality for loading configuration files, managing clean-up tasks, configuring toolchains, and handling unit test files. The module also provides methods to extract headers from source files, find corresponding source files, and build compiler and linker command fields. Additionally, it supports executing commands, reporting test summaries, running tests, and building applications. The module is designed to integrate with Unity for test summary reporting and test runner generation, and it handles various aspects of the build process, including dependency management and simulator configuration.

**Instance Methods**

---
#### RakefileHelpers\.build\_application
The `build_application` method compiles and links a main source file and its dependencies to create an executable.
- **Inputs**:
    - `main`: The name of the main source file (without extension) to be compiled and linked into an executable.
- **Control Flow**:
    - Report the start of the application building process.
    - Initialize an empty list `obj_list` to store object files.
    - Load the configuration from a global configuration file `$cfg_file`.
    - Construct the full path to the main source file using the source path from the configuration and the provided `main` argument.
    - Retrieve local include directories using `get_local_include_dirs`.
    - Extract headers from the main source file and iterate over each header.
    - For each header, find the corresponding source file in the include directories and compile it if found, adding the resulting object file to `obj_list`.
    - Compile the main source file and add the resulting object file to `obj_list`.
    - Link all object files in `obj_list` to create the final executable.
- **Output**: The method does not return a value; it produces an executable file as a side effect.


---
#### RakefileHelpers\.build\_compiler\_fields
The `build_compiler_fields` method constructs a hash containing the command, defines, options, and includes for a compiler configuration.
- **Inputs**: None
- **Control Flow**:
    - Retrieve the compiler command path from the global configuration and process it using the `tackit` method.
    - Check if the compiler defines items are nil; if so, set defines to an empty string, otherwise use the `squash` method to concatenate the prefix and items.
    - Use the `squash` method to concatenate the compiler options with an empty prefix.
    - Use the `squash` method to concatenate the compiler includes with the specified prefix and items.
    - Remove any trailing slashes and escape characters from the includes string using `gsub`.
    - Return a hash containing the processed command, defines, options, and includes.
- **Output**: A hash with keys `:command`, `:defines`, `:options`, and `:includes`, each containing processed strings based on the compiler configuration.


---
#### RakefileHelpers\.build\_linker\_fields
The `build_linker_fields` method constructs a hash containing the command, options, and includes for the linker based on the configuration settings.
- **Inputs**: None
- **Control Flow**:
    - Retrieve the linker command path from the configuration and pass it to the `tackit` method to ensure it is properly formatted.
    - Check if linker options are present in the configuration; if not, set options to an empty string, otherwise use the `squash` method to concatenate them into a single string.
    - Check if linker includes are present in the configuration; if not, set includes to an empty string, otherwise use the `squash` method to concatenate them into a single string with a prefix, and then remove any unwanted escape characters and trailing slashes.
    - Return a hash containing the formatted command, options, and includes.
- **Output**: A hash with keys `:command`, `:options`, and `:includes`, each containing strings that represent the respective linker fields.


---
#### RakefileHelpers\.build\_simulator\_fields
The `build_simulator_fields` method constructs a hash containing command, pre_support, and post_support fields for a simulator configuration.
- **Inputs**: None
- **Control Flow**:
    - Check if the global configuration `$cfg['simulator']` is nil, and return nil if true.
    - Determine the `command` by checking if `$cfg['simulator']['path']` is nil; if not, use `tackit` to format the path and append a space.
    - Determine the `pre_support` by checking if `$cfg['simulator']['pre_support']` is nil; if not, use `squash` to format the pre_support items.
    - Determine the `post_support` by checking if `$cfg['simulator']['post_support']` is nil; if not, use `squash` to format the post_support items.
    - Return a hash with keys `:command`, `:pre_support`, and `:post_support` containing the respective values.
- **Output**: A hash with keys `:command`, `:pre_support`, and `:post_support`, each containing a string value based on the simulator configuration.


---
#### RakefileHelpers\.compile
The `compile` method constructs and executes a command to compile a C source file into an object file using a configured compiler.
- **Inputs**:
    - `file`: The path to the C source file that needs to be compiled.
    - `_defines`: An optional array of defines to be used during compilation, though it is not utilized in the method.
- **Control Flow**:
    - The method begins by calling `build_compiler_fields` to retrieve a hash containing the compiler command, defines, options, and includes.
    - It constructs a command string (`cmd_str`) by concatenating the compiler command, defines, options, includes, the source file path, and the object file destination prefix and path from the configuration.
    - The method determines the object file name by replacing the C file extension with the configured object file extension.
    - It calls the `execute` method with the complete command string to perform the compilation.
    - Finally, it returns the name of the object file.
- **Output**: The method returns the name of the compiled object file as a string.


---
#### RakefileHelpers\.configure\_clean
The `configure_clean` method adds the build path to the CLEAN list for file cleanup, if the build path is defined in the configuration.
- **Inputs**: None
- **Control Flow**:
    - Checks if the build path is not nil in the global configuration `$cfg`.
    - If the build path is defined, it appends the build path with a wildcard pattern '*.*' to the CLEAN list.
- **Output**: The method does not return any value; it modifies the CLEAN list as a side effect.


---
#### RakefileHelpers\.configure\_toolchain
The `configure_toolchain` method sets up the toolchain configuration by loading a specified configuration file and configuring the clean task.
- **Inputs**:
    - `config_file`: An optional string representing the path to the configuration file, defaulting to `DEFAULT_CONFIG_FILE` if not provided.
- **Control Flow**:
    - Check if the `config_file` string ends with '.yml'; if not, append '.yml' to it.
    - Call the `load_configuration` method with the `config_file` to load the configuration settings.
    - Call the `configure_clean` method to set up the clean task based on the loaded configuration.
- **Output**: The method does not return any value; it performs configuration setup as a side effect.


---
#### RakefileHelpers\.execute
The `execute` method runs a shell command, optionally reports its output, and raises an error if the command fails.
- **Inputs**:
    - `command_string`: A string representing the shell command to be executed.
    - `verbose`: A boolean flag indicating whether to report the command's output; defaults to true.
    - `raise_on_fail`: A boolean flag indicating whether to raise an error if the command fails; defaults to true.
- **Control Flow**:
    - The method starts by reporting the command string using the `report` method.
    - It executes the command using backticks and stores the output, removing any trailing newline characters with `chomp`.
    - If `verbose` is true and the output is not nil or empty, it reports the output using the `report` method.
    - It checks the exit status of the command using `$?.exitstatus`. If the exit status is non-zero and `raise_on_fail` is true, it raises an error with a message indicating the command failed and includes the exit status.
    - Finally, it returns the output of the command.
- **Output**: The method returns the output of the executed command as a string.


---
#### RakefileHelpers\.extract\_headers
The `extract_headers` method reads a file and extracts all the header file names included in it.
- **Inputs**:
    - `filename`: The path to the file from which header file names are to be extracted.
- **Control Flow**:
    - Initialize an empty array `includes` to store the extracted header file names.
    - Read all lines from the file specified by `filename`.
    - Iterate over each line in the file.
    - For each line, use a regular expression to check if it matches the pattern of a C/C++ include directive for header files (e.g., `#include "header.h"`).
    - If a match is found, extract the header file name and add it to the `includes` array.
    - Return the `includes` array containing all extracted header file names.
- **Output**: An array of strings, each representing a header file name extracted from the file.


---
#### RakefileHelpers\.fail\_out
The `fail_out` method prints a message and a note about not returning an exit code to allow continuous integration to pass.
- **Inputs**:
    - `msg`: A string message that will be printed to the console.
- **Control Flow**:
    - Prints the input message `msg` to the console using `puts`.
    - Prints a fixed message 'Not returning exit code so continuous integration can pass' to the console using `puts`.
    - The method does not exit the program or return an exit code, as the `exit(-1)` line is commented out.
- **Output**: The method does not return any value or output, as it only performs print operations.


---
#### RakefileHelpers\.find\_source\_file
The `find_source_file` method searches for a source file corresponding to a given header file within specified directories.
- **Inputs**:
    - `header`: A header file object that has a method `ext` to append a file extension.
    - `paths`: An array of directory paths where the method will search for the source file.
- **Control Flow**:
    - Iterates over each directory in the `paths` array.
    - For each directory, constructs a potential source file path by appending the C_EXTENSION to the header file name.
    - Checks if the constructed source file path exists using `File.exist?`.
    - Returns the source file path if it exists.
    - Returns `nil` if no source file is found in any of the directories.
- **Output**: The method returns the path of the source file if found, otherwise it returns `nil`.


---
#### RakefileHelpers\.link\_it
The `link_it` method constructs and executes a command to link object files into an executable.
- **Inputs**:
    - `exe_name`: The name of the executable file to be created.
    - `obj_list`: An array of object file names to be linked into the executable.
- **Control Flow**:
    - Call `build_linker_fields` to retrieve linker command, options, and includes.
    - Construct a command string (`cmd_str`) by concatenating the linker command, options, includes, object file paths, binary file prefix, destination, executable name, and binary file extension.
    - Use the `execute` method to run the constructed command string.
- **Output**: The method does not return a value; it executes a system command to link object files into an executable.


---
#### RakefileHelpers\.load\_configuration
The `load_configuration` method loads a YAML configuration file and assigns its content to a global variable.
- **Inputs**:
    - `config_file`: A string representing the path to the configuration file to be loaded.
- **Control Flow**:
    - Assigns the input `config_file` to a global variable `$cfg_file`.
    - Reads the content of the file at the path specified by `$cfg_file`.
    - Parses the file content as YAML and assigns the resulting data structure to a global variable `$cfg`.
- **Output**: The method does not return any value; it sets global variables `$cfg_file` and `$cfg`.


---
#### RakefileHelpers\.local\_include\_dirs
The `local_include_dirs` method retrieves a list of local include directories from the global configuration, excluding any entries that are arrays.
- **Inputs**: None
- **Control Flow**:
    - Duplicate the list of include directories from the global configuration `$cfg['compiler']['includes']['items']`.
    - Remove any entries from the list that are arrays using `delete_if`.
    - Return the modified list of include directories.
- **Output**: An array of include directory paths, excluding any that were arrays.


---
#### RakefileHelpers\.report\_summary
The `report_summary` method generates a summary of test results and checks for any failures.
- **Inputs**: None
- **Control Flow**:
    - Create a new instance of `UnityTestSummary` and assign it to the variable `summary`.
    - Set the `root` attribute of `summary` to the constant `HERE`.
    - Construct a file path pattern `results_glob` using the build path from the global configuration `$cfg` and replace backslashes with forward slashes.
    - Retrieve a list of files matching the `results_glob` pattern using `Dir[]` and assign it to `results`.
    - Assign the `results` array to the `targets` attribute of `summary`.
    - Invoke the `run` method on `summary` to process the test results.
    - Check if the `failures` attribute of `summary` is greater than 0, and if so, call `fail_out` with a failure message.
- **Output**: The method does not return a value but may output a failure message if there are test failures.


---
#### RakefileHelpers\.run\_tests
The `run_tests` method compiles, links, and executes a series of unit tests, generating result files for each test.
- **Inputs**:
    - `test_files`: An array of file paths representing the unit test source files to be compiled and executed.
- **Control Flow**:
    - Report the start of the system tests.
    - Load the configuration from a global configuration file.
    - Ensure the 'TEST' define is included in the compiler configuration.
    - Retrieve local include directories from the configuration.
    - Iterate over each test file in the `test_files` array.
    - For each test file, initialize an empty list for object files (`obj_list`).
    - Extract headers from the test file to determine dependencies.
    - For each header, find the corresponding source file and compile it if it exists, adding the resulting object file to `obj_list`.
    - Determine the test runner file name and path, generating it if necessary, and compile it, adding the resulting object file to `obj_list`.
    - Compile the test file itself and add the resulting object file to `obj_list`.
    - Link all object files in `obj_list` to create the test executable.
    - Build the command string to execute the test, considering any simulator configuration.
    - Execute the test command and capture the output.
    - Determine the test result based on the output and write the result to a file with a '.testpass' or '.testfail' extension.
- **Output**: The method does not return a value; instead, it generates result files for each test, indicating whether the test passed or failed.


---
#### RakefileHelpers\.squash
The `squash` method concatenates a prefix with each item in a list, using a helper method to format each item, and returns the resulting string.
- **Inputs**:
    - `prefix`: A string that will be prepended to each item in the `items` list.
    - `items`: An array of items that will be concatenated into a single string with the `prefix`.
- **Control Flow**:
    - Initialize an empty string `result` to accumulate the final output.
    - Iterate over each `item` in the `items` array.
    - For each `item`, use the `tackit` method to format the item and concatenate it with the `prefix`, then append the result to the `result` string with a leading space.
    - Return the accumulated `result` string after processing all items.
- **Output**: A single string that is the concatenation of the `prefix` and each formatted item from the `items` array, separated by spaces.


---
#### RakefileHelpers\.tackit
The `tackit` method converts an array of strings into a single quoted string or returns the input as is if it's not an array.
- **Inputs**:
    - `strings`: An input that can be either an array of strings or a single string.
- **Control Flow**:
    - Check if the input `strings` is an instance of Array.
    - If `strings` is an Array, join all elements into a single string and wrap it in double quotes.
    - If `strings` is not an Array, return it as is.
    - Assign the result to the variable `result`.
    - Return the `result`.
- **Output**: A string that is either the joined and quoted version of the input array or the original input if it is not an array.


---
#### RakefileHelpers\.unit\_test\_files
The `unit_test_files` method generates a list of unit test files with a specific naming pattern and file extension from a configured directory.
- **Inputs**: None
- **Control Flow**:
    - Retrieve the unit tests path from the global configuration `$cfg` and append the pattern 'Test*' followed by the C file extension to form a search path.
    - Replace any backslashes in the path with forward slashes to ensure compatibility across different operating systems.
    - Create and return a new `FileList` object using the constructed path, which will include all files matching the pattern.
- **Output**: A `FileList` object containing paths to unit test files that match the specified pattern and extension.



