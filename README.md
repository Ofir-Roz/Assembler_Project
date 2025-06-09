# Assembly Language Assembler

A comprehensive two-pass assembler implementation for a custom assembly language, written in C. This project translates assembly source code into machine code through a sophisticated multi-stage process including macro preprocessing, symbol table management, and binary code generation.

## Overview

This assembler processes assembly language files (`.as`) and generates corresponding object files (`.ob`), entry files (`.ent`), and external files (`.ext`). The implementation follows the classic two-pass assembly algorithm with an additional preprocessing stage for macro expansion.

## Features

### Core Functionality
- **Two-Pass Assembly Algorithm**: Complete implementation of the standard two-pass assembler
- **Macro Preprocessing**: Support for macro definitions and expansions
- **Symbol Table Management**: Handles labels, entries, and external symbols
- **Multiple Output Formats**: Generates object, entry, and external files
- **Error Handling**: Comprehensive error detection and reporting with line numbers
- **Memory Management**: Efficient memory allocation and deallocation

### Supported Instructions

#### Arithmetic and Logic Operations
- `mov` - Move data between operands
- `cmp` - Compare two operands
- `add` - Addition operation
- `sub` - Subtraction operation
- `not` - Bitwise NOT operation
- `clr` - Clear operand
- `lea` - Load effective address

#### Control Flow
- `jmp` - Unconditional jump
- `bne` - Branch if not equal
- `jsr` - Jump to subroutine
- `rts` - Return from subroutine
- `stop` - Halt execution

#### I/O Operations
- `prn` - Print operand value
- `red` - Read input
- `inc` - Increment operand
- `dec` - Decrement operand

### Addressing Methods
1. **Immediate Addressing** (`#value`) - Direct value
2. **Direct Addressing** (`label`) - Memory address by label
3. **Jump with Parameters** (`label(param1,param2)`) - Parameterized jumps
4. **Register Addressing** (`r0-r7`) - 8 general-purpose registers

### Directives
- `.data` - Define data constants
- `.string` - Define string literals
- `.entry` - Mark symbols for export
- `.extern` - Declare external symbols

## Project Structure

```
Assembler_Project/
├── main.c                    # Main program entry point
├── main_header.h            # Main header with all includes
├── processor.c              # Preprocessing and main assembly logic
├── first_pass.c             # First pass implementation
├── second_pass.c            # Second pass implementation
├── memory_map.c             # Memory management and word encoding
├── tables.c                 # Symbol table operations
├── errors.c                 # Error handling and validation
├── files_handler.c          # File I/O operations
├── string_manipulations.c   # String processing utilities
├── assistance_funcs.c       # Helper functions
├── Makefile                 # Build configuration
├── headers/                 # Header files directory
│   ├── types_macros.h       # Type definitions and macros
│   ├── processor.h          # Preprocessing functions
│   ├── first_pass.h         # First pass declarations
│   ├── second_pass.h        # Second pass declarations
│   ├── memory_map.h         # Memory management
│   ├── tables.h             # Symbol table structures
│   ├── errors.h             # Error handling
│   ├── files_handler.h      # File operations
│   ├── string_manipulations.h # String utilities
│   └── assistance_funcs.h   # Helper function declarations
└── test*.as                 # Test assembly files
```

## Building the Project

### Prerequisites
- GCC compiler
- Make utility
- ANSI C compliant environment

### Compilation
```bash
make
```

This will compile all source files and create the `Assembler` executable.

### Manual Compilation
```bash
gcc -ansi -Wall -g main_header.h main.c processor.c first_pass.c second_pass.c \
    assistance_funcs.c errors.c files_handler.c memory_map.c \
    string_manipulations.c tables.c -o Assembler -lm
```

## Usage

```bash
./Assembler file1 file2 [file3 ...]
```

The assembler automatically appends `.as` extension to input filenames.

### Example
```bash
./Assembler test1 test2
```

This processes `test1.as` and `test2.as` files.

## Assembly Language Syntax

### Basic Program Structure
```assembly
; Comments start with semicolon

MAIN:       mov r3, LENGTH     ; Move LENGTH to register 3
LOOP:       jmp L1(#-1,r6)     ; Jump with parameters
            prn #-5            ; Print immediate value
            bne LOOP(r4,r5)    ; Branch if not equal
            sub r1, r4         ; Subtract r4 from r1
L1:         inc K              ; Increment K
            stop               ; Halt execution

LENGTH:     .data 6,-9,15      ; Data definition
K:          .data 22           ; Single data value
STR:        .string "hello"    ; String definition

.entry LENGTH                  ; Export symbol
.extern EXTERNAL_LABEL         ; Import symbol
```

### Macro Support
```assembly
mcr macro_name
    mov r1, r2
    add r1, #5
endmcr

; Usage
macro_name    ; Expands to the macro content
```

## Output Files

### Object File (`.ob`)
Contains the machine code in binary format with memory addresses:
```
    12 5
0100        00100000110000
0101        00001000000000
...
```

### Entry File (`.ent`)
Lists exported symbols with their addresses:
```
LENGTH      0104
LOOP        0102
```

### External File (`.ext`)
Lists external symbol references with their usage addresses:
```
EXTERNAL    0105
EXTERNAL    0108
```

## Memory Organization

- **Memory Size**: 256 words (14-bit each)
- **Starting Address**: 100 (decimal)
- **Word Structure**: 14-bit words with A/R/E encoding
- **Registers**: 8 general-purpose registers (r0-r7)

### Memory Word Types
1. **First Memory Word**: Contains opcode and addressing information
2. **Additional Memory Words**: For operands and parameters
3. **Data Words**: For `.data` and `.string` directives

## Error Handling

The assembler provides comprehensive error detection including:
- **Syntax Errors**: Invalid instruction format
- **Semantic Errors**: Illegal addressing modes
- **Symbol Errors**: Undefined labels, duplicate definitions
- **Macro Errors**: Invalid macro syntax
- **Memory Errors**: Address overflow, invalid memory access

Error messages include line numbers and descriptive text in color-coded format.

## Technical Implementation

### Two-Pass Algorithm
1. **Preprocessing**: Macro expansion and `.am` file generation
2. **First Pass**: Symbol table construction and instruction validation
3. **Second Pass**: Code generation and address resolution

### Memory Management
- Dynamic memory allocation for symbols and macros
- Efficient linked list structures for tables
- Proper memory cleanup and error handling

### File Processing
- Robust file I/O with error checking
- Support for multiple input files
- Automatic file extension handling

## Testing

The project includes several test files:
- `test1.as` - Basic functionality test
- `test2.as` - Symbol table test
- `test3.as` - Error handling test
- `test4.as` - Advanced features test

## Author

**Ofir Rozanes**

## License

This project is part of an academic assignment for The Open University of Israel.

## Architecture Details

### Data Structures
- **Symbol Table**: Linked list with labels, types, and addresses
- **Memory Image**: Array representing the program's memory
- **Macro Table**: Dynamic storage for macro definitions
- **Command Words**: Union structures for different instruction formats

### Bit-Level Encoding
- **A/R/E Bits**: Absolute/Relocatable/External encoding
- **Opcode Fields**: 4-bit operation codes
- **Addressing Modes**: 2-bit addressing method indicators
- **Register Fields**: 3-bit register specifications

This assembler demonstrates advanced concepts in systems programming, including:
- Multi-pass compilation techniques
- Symbol table management
- Memory organization
- Error recovery
- Modular software design
