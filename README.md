# BOINC BUDA Sample Application

## Overview

This repository contains a minimal but complete example of a BOINC application that follows the BUDA framework conventions. It demonstrates key concepts including progress reporting, proper application structure, and cross-platform compatibility.

## What It Does

The application runs a simple simulation that:
- Executes a 10-step process with short delays
- Reports progress through the standard BOINC `fraction_done` mechanism
- Provides console output for monitoring
- Demonstrates proper BOINC application lifecycle

## Features

- **Cross-platform compatibility**: Builds on Linux (x64 and ARM64)
- **Progress reporting**: Uses BOINC's standard fraction_done file for progress tracking
- **Minimal dependencies**: Only requires standard C++ libraries
- **Clean architecture**: Follows BOINC application best practices
- **Automated builds**: GitHub Actions workflow for continuous integration

## Building

### Prerequisites

- CMake 3.16 or higher
- C++ compiler with C++11 support
- Make or compatible build system

### Local Build

```bash
# Create build directory
mkdir build
cd build

# Configure the project
cmake ..

# Build the application
make
```

### Cross-compilation for ARM64

```bash
# Install cross-compilation tools (on Ubuntu/Debian)
sudo apt-get install g++-aarch64-linux-gnu gcc-aarch64-linux-gnu

# Configure for ARM64
cmake -E env CC="aarch64-linux-gnu-gcc" CXX="aarch64-linux-gnu-g++" \
  CFLAGS="-march=armv8-a" CXXFLAGS="-march=armv8-a" \
  cmake -B build -S .

# Build
cmake --build build --config Release
```

## Running

Execute the built application:

```bash
./build/boinc-buda-app
```

Expected output:
```
Running test...
1/10
2/10
...
10/10
Test completed.
```

The application will also create a `fraction_done` file containing the completion percentage (0.0 to 1.0).

## Key Components

### Progress Reporting (`fraction_done`)

The application demonstrates BOINC's standard progress reporting mechanism:

```cpp
void fraction_done(double fraction) {
    std::fstream outfile("fraction_done", std::ios::trunc | std::ios::out);
    if(outfile.is_open()) {
        outfile << fraction << std::endl;
        outfile.close();
    }
}
```

This creates a file that BOINC clients read to track computational progress.

## Development Guidelines

When extending this application:

1. **Maintain Progress Reporting**: Always update fraction_done appropriately
2. **Handle Interruption**: BOINC applications should handle graceful shutdown
3. **Minimize Output**: Excessive console output can impact performance
4. **Cross-platform Code**: Ensure compatibility across target platforms
5. **Resource Awareness**: Be mindful of CPU, memory, and disk usage

## License

This project is part of BOINC and is licensed under the GNU General Public License v3.0. See the [LICENSE](LICENSE) file for full details.

## Contributing

This is a sample application maintained by the BOINC project. For contributions or issues:

1. Fork the repository
2. Create a feature branch
3. Submit a pull request with detailed description
4. Ensure all CI checks pass

## Support

For questions about BOINC application development:
- Visit the [BOINC Website](https://boinc.berkeley.edu/)
- Review existing [BOINC Applications](https://boinc.berkeley.edu/projects.php)
