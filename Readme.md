*This project has been created as part of the 42 curriculum by bilyas.*

# Get Next Line

## Description

The **Get Next Line (GNL)** project consists of implementing a function in C that reads a file descriptor and returns one line at a time.

The main goal of this project is to deeply understand how low-level input/output works in Unix systems using the `read()` system call, while also managing memory efficiently and handling edge cases.

The function must:
- Read from a file descriptor
- Return a single line per call
- Preserve the remaining data for the next call
- Work regardless of the buffer size

This project introduces key concepts such as **static variables**, **dynamic memory allocation**, and **buffer management**, which are essential for writing efficient and reliable C programs.

---

## Instructions

### Compilation

To compile the mandatory part:

```bash
gcc -Wall -Wextra -Werror -c get_next_line.c get_next_line_utils.c
```

To compile with a custom buffer size:

```bash
gcc -D BUFFER_SIZE=1024 -Wall -Wextra -Werror -c get_next_line.c get_next_line_utils.c
```

### Usage

Include the header in your program:

```c
#include "get_next_line.h"
```

Function prototype:

```c
char *get_next_line(int fd);
```

### Example

```c
#include "get_next_line.h"
#include <fcntl.h>
#include <stdio.h>

int main(void)
{
    int fd = open("file.txt", O_RDONLY);
    char *line;

    if (fd < 0)
        return (1);
    while ((line = get_next_line(fd)) != NULL)
    {
        printf("%s", line);
        free(line);
    }
    close(fd);
    return (0);
}
```

---

## Detailed Explanation

The implementation of **get_next_line** relies on a persistent memory mechanism using a static variable to store data between function calls.

### 1. Reading from the file descriptor

The function uses the `read()` system call to read chunks of data of size `BUFFER_SIZE`.  
This continues until a newline (`\n`) is found or the end of file is reached.

### 2. Buffer storage

A **static buffer** is used to store leftover data that was read but not yet returned.  
This allows the function to resume exactly where it left off in the next call.

### 3. Line extraction

Once a newline is detected:
- The function extracts the line (including `\n` if present)
- Allocates memory dynamically for that line
- Returns it to the caller

### 4. Buffer update

After extracting the line:
- The remaining data is kept in the static buffer
- The used portion is removed

### 5. Memory management

- Each returned line is dynamically allocated
- The caller is responsible for freeing it
- Care is taken to avoid memory leaks

---

## Project Structure

```
.
├── get_next_line.c
├── get_next_line_utils.c
├── get_next_line.h
├── get_next_line_bonus.c
├── get_next_line_utils_bonus.c
├── get_next_line_bonus.h
```

---

## Key Concepts

- File descriptors and low-level I/O
- Static variables
- Dynamic memory allocation
- String manipulation
- Buffer management

---

## Edge Cases Handled

- Empty files
- Files without a newline at the end
- Large files
- Multiple file descriptors (bonus)
- Invalid file descriptors

---

## Resources

### Documentation

- POSIX `read()` manual
- Unix file descriptor documentation
- C standard library documentation

### Learning Materials

- Memory management in C
- Static variables behavior
- Buffer handling techniques

### AI Usage

AI tools were used in the following ways:

- **Debugging**: identifying edge cases and fixing memory leaks  
- **Code Review**: improving buffer handling and function structure  
- **Documentation**: helping structure and clarify explanations  

AI was not used to generate the full solution but as a support tool to improve understanding and code quality.

---

For more information about 42 School, visit [42school.com](https://www.42school.com/).