# HTTP File Runner Github Action

GitHub Action for running .http files using the [httprunner](https://github.com/christianhelle/httprunner) CLI tool.

## Features

- **Cross-platform support**: Works on Linux, macOS, and Windows runners
- **Discovery mode**: Automatically finds and runs all .http files in your repository
- **Selective execution**: Run specific .http files
- **Automatic installation**: Handles httprunner installation on all supported platforms

## Usage

### Discovery Mode

Run all .http files found recursively in the current directory:

```yaml
- name: Run HTTP tests
  uses: christianhelle/httprunner-action@v1
  with:
    discover: true
```

### Specific Files

Run one or more specific .http files:

```yaml
- name: Run specific HTTP tests
  uses: christianhelle/httprunner-action@v1
  with:
    files: 'api-tests.http auth-tests.http'
```

### Complete Workflow Example

```yaml
name: API Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    - name: Run HTTP tests
      uses: christianhelle/httprunner-action@v1
      with:
        discover: true
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `discover` | Enable discovery mode to recursively scan for .http files | No | `false` |
| `files` | One or more .http files to execute (space-separated) | No | - |

## Requirements

- Either `discover: true` OR `files` must be specified, but not both
- The action will fail if both options are used simultaneously
- The action will fail if neither option is specified

## Platform Support

### Linux
- Uses `snap install httprunner` for installation
- Requires sudo access (available in GitHub hosted runners)

### macOS
- Downloads appropriate binary from GitHub releases based on architecture
- Supports both Intel (x86_64) and Apple Silicon (ARM64) runners

### Windows
- Downloads Windows binary from GitHub releases
- Works on both x86 and x64 Windows runners

## Examples

### Test API endpoints in a monorepo

```yaml
- name: Test user API
  uses: christianhelle/httprunner-action@v1
  with:
    files: 'tests/user-api.http'

- name: Test payment API  
  uses: christianhelle/httprunner-action@v1
  with:
    files: 'tests/payment-api.http'
```

### Run all HTTP tests

```yaml
- name: Run all HTTP tests
  uses: christianhelle/httprunner-action@v1
  with:
    discover: true
```

### Matrix testing across platforms

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest, macos-latest]
    
runs-on: ${{ matrix.os }}

steps:
- name: Checkout
  uses: actions/checkout@v4
  
- name: Run HTTP tests
  uses: christianhelle/httprunner-action@v1
  with:
    discover: true
```

## License

MIT License - see [LICENSE](LICENSE) file for details.
