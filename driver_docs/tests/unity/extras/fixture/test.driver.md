## Folders
- **[main](test/main.driver.md)**: The `main` folder in the `cJSON` codebase contains the `AllTests.c` file, which is responsible for setting up and running all test groups using the Unity test framework.

## Files
- **[Makefile](test/Makefile.driver.md)**: The `Makefile` in the `cJSON` codebase at `test/Makefile.driver.md` is used to compile and build test executables for the Unity testing framework with various configurations and compiler flags, including support for different architectures and exclusion of standard library malloc.
- **[template_fixture_tests.c](test/template_fixture_tests.c.driver.md)**: The `template_fixture_tests.c` file contains unit tests for a test group named "mygroup" using the Unity Test Framework, demonstrating setup, teardown, and assertions on a static integer variable.
- **[unity_fixture_Test.c](test/unity_fixture_Test.c.driver.md)**: The `unity_fixture_Test.c` file contains a series of unit tests for the Unity Test Framework, focusing on memory management functions like malloc, realloc, and calloc, as well as command line options and leak detection.
- **[unity_fixture_TestRunner.c](test/unity_fixture_TestRunner.c.driver.md)**: The `unity_fixture_TestRunner.c` file defines test runners for various test groups, including UnityFixture, UnityCommandOptions, LeakDetection, and InternalMalloc, as part of the Unity test framework in the cJSON codebase.
- **[unity_output_Spy.c](test/unity_output_Spy.c.driver.md)**: The `unity_output_Spy.c` file implements a spy for capturing and managing character output in the Unity test framework, allowing for controlled output and retrieval of captured data.
- **[unity_output_Spy.h](test/unity_output_Spy.h.driver.md)**: The `unity_output_Spy.h` file declares functions for creating, destroying, and managing a character output spy for the Unity Test Framework in C.
