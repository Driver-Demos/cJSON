# Purpose
This Ruby source code file defines a utility for managing colored text output in the command line, specifically tailored for both Windows and POSIX-compliant systems. The primary class, `ColourCommandLine`, encapsulates the functionality required to change text color in the console. It uses the `Win32API` library to interface with Windows system calls for setting console text attributes, while it employs ANSI escape codes for POSIX systems. The class provides methods to convert symbolic color names to their respective system-specific codes, ensuring compatibility across different platforms. The `change_to` method is central to this functionality, determining the appropriate color code based on the operating system and applying it to the console output.

The file also includes two utility methods, `colour_puts` and `colour_print`, which serve as public interfaces for printing colored text to the console. These methods instantiate the `ColourCommandLine` class and utilize its `out_c` method to handle the actual output, allowing users to specify the text role (such as `:success`, `:failure`, etc.) and the string to be printed. This code provides a focused functionality aimed at enhancing the readability of console output by using color coding, which can be particularly useful in test frameworks or any command-line applications that benefit from visual differentiation of output.
# Imports and Dependencies

---
- `Win32API`


# Classes

---
### ColourCommandLine
- **Description**: The `ColourCommandLine` class is designed to facilitate the output of colored text to the command line, with specific support for both Windows and POSIX-compliant systems. It utilizes the `Win32API` for Windows platforms to change console text attributes, allowing for a variety of colors to be displayed in the console. The class provides methods to map symbolic color names to their respective console codes for both Windows and POSIX systems, enabling consistent color output across different environments. The `out_c` method is used to print or put strings to the console in the specified color, reverting back to the default color after the operation. This class is particularly useful in scenarios where colored output is needed for better readability or emphasis in command-line applications.

**Instance Methods**

---
#### ColourCommandLine\.change\_to
The `change_to` method changes the console text color based on the platform and the specified color.
- **Inputs**:
    - `new_colour`: A symbol representing the desired color to change the console text to, such as :red, :green, :blue, etc.
- **Control Flow**:
    - Check if the current platform is Windows by matching `RUBY_PLATFORM` against the regex `/(win|w)32$/`.
    - If on Windows, call the `@set_console_txt_attrb` method with the handle `@hout` and the result of `win32_colour(new_colour)` to change the console text color.
    - If not on Windows, return a string with ANSI escape codes to change the console text color using `posix_colour(new_colour)`.
- **Output**: On Windows, it changes the console text color directly; on other platforms, it returns a string with ANSI escape codes to change the console text color.


---
#### ColourCommandLine\.initialize
The `initialize` method sets up the necessary Windows API calls for changing console text color on Windows platforms.
- **Inputs**: None
- **Control Flow**:
    - Check if the current platform is Windows by matching `RUBY_PLATFORM` against a regex for Windows platforms.
    - If the platform is not Windows, the method returns immediately without executing further.
    - If the platform is Windows, create a `Win32API` object for the `GetStdHandle` function from `kernel32` to retrieve the standard output handle.
    - Create another `Win32API` object for the `SetConsoleTextAttribute` function from `kernel32` to change console text attributes.
    - Call `GetStdHandle` with the argument `-11` to get the handle for standard output and store it in the instance variable `@hout`.
- **Output**: The method does not return any value; it initializes instance variables for Windows API calls if on a Windows platform.


---
#### ColourCommandLine\.out\_c
The `out_c` method outputs a string to the console with a specified color and mode, adapting its behavior based on the operating system platform.
- **Inputs**:
    - `mode`: A symbol indicating the output method, either `:puts` for a newline-terminated output or `:print` for a non-terminated output.
    - `colour`: A symbol representing the desired text color, which is mapped to specific color codes for Windows or POSIX systems.
    - `str`: The string to be output to the console.
- **Control Flow**:
    - Check the current platform using `RUBY_PLATFORM` to determine if it is a Windows environment.
    - If on Windows, change the console text color using `change_to(colour)`, then output the string using either `puts` or `print` based on the `mode`, and finally reset the color to `:default_white`.
    - If not on Windows, output the string with ANSI escape codes for color, appending `\033[0m` to reset the color after the string, using either `puts` or `print` based on the `mode`.
- **Output**: The method outputs the given string to the console in the specified color and mode, with no return value.


---
#### ColourCommandLine\.posix\_colour
The `posix_colour` method maps symbolic color names to their corresponding ANSI escape code for foreground colors.
- **Inputs**:
    - `colour`: A symbol representing the color name or role, such as :red, :success, :default, etc.
- **Control Flow**:
    - The method uses a `case` statement to match the input `colour` symbol against predefined color names or roles.
    - For each matched color or role, it returns the corresponding ANSI escape code for the foreground color.
    - If the input `colour` does not match any predefined symbol, it defaults to returning the ANSI code for the default foreground color (39).
- **Output**: An integer representing the ANSI escape code for the specified foreground color.


---
#### ColourCommandLine\.win32\_colour
The `win32_colour` method maps symbolic colour names to their corresponding Windows console colour codes.
- **Inputs**:
    - `colour`: A symbol representing the colour name, such as :black, :dark_blue, :green, etc.
- **Control Flow**:
    - The method uses a case statement to match the input symbol `colour` against predefined colour symbols.
    - For each matched colour symbol, it returns a corresponding integer value representing the Windows console colour code.
    - If the input symbol does not match any predefined colour, the method defaults to returning 0, which corresponds to the colour black.
- **Output**: An integer representing the Windows console colour code for the given symbolic colour name.



