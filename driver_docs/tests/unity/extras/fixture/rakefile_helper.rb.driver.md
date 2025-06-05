# Purpose
This Ruby code file is part of a test framework for C, specifically designed to facilitate the building, compiling, linking, and execution of unit tests using the Unity testing framework. The file defines a module, `RakefileHelpers`, which provides a collection of methods to automate the configuration and execution of C tests. The module includes methods for loading configuration files, setting up the build environment, compiling source files, linking object files, and executing tests. It also handles the generation of test summaries and reports. The code relies on external YAML configuration files to define compiler and linker settings, such as paths, options, and file extensions, which are crucial for the build process.

The module is structured to support a streamlined workflow for testing C code, with functions that encapsulate specific tasks like `compile`, `link_it`, and `run_tests`. These functions use helper methods like `build_compiler_fields` and `build_linker_fields` to construct command strings for the compiler and linker based on the configuration settings. The `execute` method is used to run these command strings, capturing and reporting output, and raising errors if commands fail. The code also integrates with external scripts for generating test runners and color reports, enhancing the test framework's functionality. Overall, this file provides a robust and automated approach to managing the lifecycle of C unit tests within the Unity framework.
# Imports and Dependencies

---
- `yaml`
- `fileutils`
- `unity_test_summary`
- `generate_test_runner`
- `colour_reporter`


# Modules

---
### RakefileHelpers
- **Description**: The `RakefileHelpers` module is designed to facilitate the configuration and execution of build and test processes for C projects using the Unity Test Framework. It provides methods to load configuration files, set up cleaning tasks, configure toolchains, and manage the compilation and linking of source files. The module also includes functionality to execute commands, report summaries, and run tests, integrating with simulators if necessary. It handles various aspects of the build process, such as defining compiler and linker fields, managing include paths, and executing the build and test commands, ensuring a streamlined workflow for testing C code.

**Instance Methods**

---
#### RakefileHelpers\.build\_compiler\_fields
The `build_compiler_fields` method constructs a hash containing the command, defines, options, and includes for a compiler configuration.
- **Inputs**: None
- **Control Flow**:
    - Retrieve the compiler path from the global configuration `$cfg` and process it using the `tackit` method to form the `command`.
    - Check if the `defines` items are nil in the configuration; if so, set `defines` to an empty string, otherwise use the `squash` method to concatenate the defines prefix with the items and additional Unity output char definitions.
    - Use the `squash` method to concatenate the compiler options into a single string `options`.
    - Use the `squash` method to concatenate the includes prefix with the items into a single string `includes`.
    - Perform a series of `gsub` operations on `includes` to clean up the string by removing escaped spaces, quotes, and trailing slashes.
    - Return a hash containing the `command`, `defines`, `options`, and `includes` as keys with their respective processed values.
- **Output**: A hash with keys `:command`, `:defines`, `:options`, and `:includes`, each containing strings representing the respective compiler configuration fields.


---
#### RakefileHelpers\.build\_linker\_fields
The `build_linker_fields` method constructs a hash containing the command, options, and includes for the linker based on the configuration settings.
- **Inputs**: None
- **Control Flow**:
    - Retrieve the linker command path from the configuration and process it using the `tackit` method.
    - Check if linker options are present in the configuration; if not, set options to an empty string, otherwise process them using the `squash` method.
    - Check if linker includes are present in the configuration; if not, set includes to an empty string, otherwise process them using the `squash` method and perform additional string replacements to clean up the includes.
    - Return a hash containing the processed command, options, and includes.
- **Output**: A hash with keys `:command`, `:options`, and `:includes`, each containing the respective processed string values for the linker.


---
#### RakefileHelpers\.build\_simulator\_fields
The `build_simulator_fields` method constructs a hash containing command, pre_support, and post_support fields for a simulator configuration.
- **Inputs**: None
- **Control Flow**:
    - Check if the global configuration variable `$cfg['simulator']` is `nil`, and return `nil` if true.
    - Determine the `command` by checking if `$cfg['simulator']['path']` is `nil`; if not, use the `tackit` method to format the path and append a space.
    - Determine the `pre_support` by checking if `$cfg['simulator']['pre_support']` is `nil`; if not, use the `squash` method to format the pre_support items.
    - Determine the `post_support` by checking if `$cfg['simulator']['post_support']` is `nil`; if not, use the `squash` method to format the post_support items.
    - Return a hash containing the `command`, `pre_support`, and `post_support` values.
- **Output**: A hash with keys `:command`, `:pre_support`, and `:post_support`, each containing a string value based on the simulator configuration.


---
#### RakefileHelpers\.compile
The `compile` method constructs and executes a command string to compile a C source file using configuration settings.
- **Inputs**:
    - `file`: The path to the C source file that needs to be compiled.
    - `_defines`: An optional array of additional defines for the compiler, defaulting to an empty array.
- **Control Flow**:
    - The method begins by calling `build_compiler_fields` to retrieve a hash of compiler command components.
    - It constructs a string `unity_include` by appending '../../src' to the compiler's include prefix from the configuration.
    - A command string `cmd_str` is constructed by concatenating the compiler command, defines, options, includes, the `unity_include`, the file to be compiled, and the destination and extension for the object file.
    - The `execute` method is called with `cmd_str` to run the compilation command.
- **Output**: The method does not return a value; it executes a system command to compile the file and raises an error if the command fails.


---
#### RakefileHelpers\.configure\_clean
The `configure_clean` method adds all files in the compiler's build path to the CLEAN list, unless the build path is nil.
- **Inputs**: None
- **Control Flow**:
    - Check if the build path in the configuration is not nil.
    - If the build path is not nil, append all files in the build path to the CLEAN list using a wildcard pattern.
- **Output**: The method does not return any value; it modifies the CLEAN list in place.


---
#### RakefileHelpers\.configure\_toolchain
The `configure_toolchain` method sets up the toolchain configuration by ensuring the configuration file has the correct extension and path, then loads the configuration and sets up cleaning rules.
- **Inputs**:
    - `config_file`: An optional string representing the name of the configuration file to be used, defaulting to `DEFAULT_CONFIG_FILE` if not provided.
- **Control Flow**:
    - Check if the `config_file` does not end with '.yml', and if so, append '.yml' to it.
    - Ensure the `config_file` does not contain directory separators, indicating it is a relative path.
    - Call `load_configuration` with the `config_file` to load the configuration settings.
    - Call `configure_clean` to set up cleaning rules based on the loaded configuration.
- **Output**: The method does not return any value; it performs configuration setup as a side effect.


---
#### RakefileHelpers\.execute
The `execute` method runs a shell command, optionally reports its output, and raises an error if the command fails.
- **Inputs**:
    - `command_string`: A string representing the shell command to be executed.
    - `verbose`: A boolean flag indicating whether to report the command's output; defaults to true.
- **Control Flow**:
    - The method starts by reporting the command string using the `report` method.
    - It executes the command string in a subshell and captures the output, removing any trailing newline characters.
    - If the `verbose` flag is true and the output is not nil or empty, it reports the output using the `report` method.
    - The method checks the exit status of the command; if it is not zero, it raises an error indicating the command failed.
    - Finally, it returns the output of the command.
- **Output**: The method returns the output of the executed command as a string.


---
#### RakefileHelpers\.link\_it
The `link_it` method constructs and executes a command string to link object files into an executable using specified linker configurations.
- **Inputs**:
    - `exe_name`: The name of the executable file to be created.
    - `obj_list`: An array of object file names to be linked into the executable.
- **Control Flow**:
    - Call `build_linker_fields` to retrieve linker command, options, and includes.
    - Construct a command string (`cmd_str`) by concatenating the linker command, options, includes, object file paths, binary file prefix, destination, executable name, and binary file extension.
    - Use `execute` method to run the constructed command string.
- **Output**: The method does not return a value; it executes a system command to link object files into an executable.


---
#### RakefileHelpers\.load\_configuration
The `load_configuration` method loads a YAML configuration file and sets global variables based on its contents.
- **Inputs**:
    - `config_file`: The name of the configuration file to be loaded, which may or may not include a path.
- **Control Flow**:
    - Check if the global variable `$configured` is true; if so, exit the method immediately.
    - If `config_file` does not contain a path separator, construct the full path to the configuration file by appending it to a base directory path.
    - Read the contents of the configuration file and parse it as YAML, storing the result in the global variable `$cfg`.
    - Set the global variable `$colour_output` to false if the configuration does not specify a 'colour' setting.
    - Set the global variable `$configured` to true if the `config_file` is not the default configuration file.
- **Output**: The method does not return any value, but it sets several global variables: `$cfg_file`, `$cfg`, `$colour_output`, and `$configured`.


---
#### RakefileHelpers\.report\_summary
The `report_summary` method generates a summary report of Unity test results by collecting test result files and executing a summary operation.
- **Inputs**: None
- **Control Flow**:
    - Instantiate a new `UnityTestSummary` object and assign it to the variable `summary`.
    - Set the `root` attribute of `summary` to the constant `HERE`.
    - Construct a file path pattern `results_glob` using the build path from the global configuration `$cfg` and replace backslashes with forward slashes.
    - Use the `Dir` class to find all files matching the `results_glob` pattern and assign them to the `results` variable.
    - Assign the `results` array to the `targets` attribute of `summary`.
    - Invoke the `run` method on the `summary` object to process the test results.
- **Output**: The method does not return a value; it performs operations to generate a test summary report.


---
#### RakefileHelpers\.run\_tests
The `run_tests` method compiles, links, and executes Unity system tests, then writes the test results to a file.
- **Inputs**: None
- **Control Flow**:
    - The method starts by reporting that Unity system tests are running.
    - It loads the configuration from a global configuration file.
    - It ensures that the 'TEST' define is included in the compiler defines.
    - It gathers all source files from specified directories and adds a specific Unity source file to the list.
    - Each source file is compiled into an object file using the `compile` method.
    - The object files are linked into a test executable named 'framework_test' using the `link_it` method.
    - The method builds a command string to execute the test executable, optionally using a simulator if configured.
    - The test executable is run using the `execute` method, and its output is captured.
    - The method determines the test result based on the output and writes the result to a file with a '.testpass' or '.testfail' extension.
- **Output**: The method outputs a file containing the test results, with a filename indicating pass or fail status based on the test execution output.


---
#### RakefileHelpers\.squash
The `squash` method concatenates a prefix with each item in a list, using a helper method to format each item, and returns the resulting string.
- **Inputs**:
    - `prefix`: A string that will be prefixed to each item in the items list.
    - `items`: An array of items that will be concatenated with the prefix.
- **Control Flow**:
    - Initialize an empty string `result` to accumulate the final output.
    - Iterate over each `item` in the `items` array.
    - For each `item`, call the `tackit` method to format the item appropriately.
    - Concatenate the `prefix` and the formatted `item` to the `result` string, separated by a space.
    - Return the accumulated `result` string after processing all items.
- **Output**: A single string that is the concatenation of the prefix and each formatted item from the items list, separated by spaces.


---
#### RakefileHelpers\.tackit
The `tackit` method converts an array of strings into a single quoted string or returns the input as is if it's not an array.
- **Inputs**:
    - `strings`: An input that can be either an array of strings or a single string.
- **Control Flow**:
    - Check if the input `strings` is an instance of Array.
    - If `strings` is an Array, join the elements into a single string and wrap it in double quotes.
    - If `strings` is not an Array, return it as is.
- **Output**: A string that is either the joined and quoted version of the input array or the original input if it is not an array.



