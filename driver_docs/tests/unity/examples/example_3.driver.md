## Folders
- **[helper](example_3/helper.driver.md)**: The `helper` folder in the `cJSON` codebase contains files that provide functions and macros for asserting the equality of `EXAMPLE_STRUCT_T` structures using Unity's testing framework.
- **[src](example_3/src.driver.md)**: The `src` folder in the `cJSON` codebase contains source and header files for production code, including functions for searching arrays, returning local variables, and a placeholder function that lacks tests.
- **[test](example_3/test.driver.md)**: The `test` folder in the `cJSON` codebase contains unit test files, `TestProductionCode.c` and `TestProductionCode2.c`, which utilize the Unity test framework to verify and plan future tests for functions in the `ProductionCode` and `ProductionCode2` modules.

## Files
- **[rakefile.rb](example_3/rakefile.rb.driver.md)**: The `rakefile.rb` file in the `cJSON` codebase sets up Rake tasks for preparing, building, and testing the Unity framework, including configuration management and test summary generation.
- **[rakefile_helper.rb](example_3/rakefile_helper.rb.driver.md)**: The `rakefile_helper.rb` file in the `cJSON` codebase provides a set of helper methods for configuring, compiling, linking, and running unit tests and applications using a specified toolchain and configuration file.
- **[readme.txt](example_3/readme.txt.driver.md)**: The `readme.txt` file provides instructions and information about Example 3, which demonstrates passing, ignored, and failing tests using Unity's advanced features, and explains how to build and test using rake with Ruby.
- **[target_gcc_32.yml](example_3/target_gcc_32.yml.driver.md)**: The `target_gcc_32.yml` file configures the build settings for compiling and linking 32-bit unit tests using GCC in the `cJSON` codebase.
