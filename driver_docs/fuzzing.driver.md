## Folders
- **[inputs](fuzzing/inputs.driver.md)**: The `inputs` folder in the `cJSON` codebase contains various JSON and JSON-like files used for fuzz testing, representing diverse data structures such as glossary entries, menu configurations, widget settings, geographical data, and more.

## Files
- **[afl-prepare-linux.sh](fuzzing/afl-prepare-linux.sh.driver.md)**: The `afl-prepare-linux.sh` file is a shell script that configures the Linux system for fuzzing by setting the core dump pattern and CPU frequency scaling governor.
- **[afl.c](fuzzing/afl.c.driver.md)**: The `afl.c` file in the `cJSON` codebase is a fuzzing utility that reads a JSON file, parses it using cJSON, and optionally prints the parsed JSON, supporting both formatted and unformatted output.
- **[afl.sh](fuzzing/afl.sh.driver.md)**: The `afl.sh` file is a shell script for setting up and building the cJSON project with AFL fuzzing and sanitizers enabled.
- **[cjson_read_fuzzer.c](fuzzing/cjson_read_fuzzer.c.driver.md)**: The `cjson_read_fuzzer.c` file implements a fuzzing test for the cJSON library, which parses JSON data and tests various configurations of JSON printing and minification.
- **[CMakeLists.txt](fuzzing/CMakeLists.txt.driver.md)**: The `CMakeLists.txt` file in the `cJSON/fuzzing` directory configures the build system to create executables and targets for fuzzing the cJSON library using afl-fuzz, with options for enabling fuzzing and sanitizers.
- **[fuzz_main.c](fuzzing/fuzz_main.c.driver.md)**: The `fuzz_main.c` file in the `cJSON` codebase implements a fuzzing target entry point that reads input from a file and passes it to the `LLVMFuzzerTestOneInput` function for testing.
- **[json.dict](fuzzing/json.dict.driver.md)**: The `json.dict` file in the `cJSON` codebase provides an AFL dictionary for JSON, defining various JSON components and escape sequences for fuzz testing.
- **[ossfuzz.sh](fuzzing/ossfuzz.sh.driver.md)**: The `ossfuzz.sh` file is a script for building and setting up the cJSON project for fuzz testing with OSS-Fuzz, including compiling the fuzzer and preparing input seeds and dictionary files.
