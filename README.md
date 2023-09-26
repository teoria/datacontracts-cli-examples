# cli-examples
Usage examples for the data contract CLI

## Find breaking changes on pull request
In this example the `breaking` command is used in a [simple GitHub action](.github/workflows/breaking.yml) to find breaking changes on pull requests. Take a look at [the failing pull request](https://github.com/datacontract/cli-examples/pull/2).

### Action
```yaml
name: Breaking Changes

on: [pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Get CLI
        run: |
          curl -L https://github.com/datacontract/cli/releases/download/v0.2.0/datacontract-v0.2.0-linux-amd64.tar.gz -o datacontract.tar.gz
          tar -xf datacontract.tar.gz
      - name: Check backwards compatibility 
        run: ./datacontract breaking --with https://raw.githubusercontent.com/datacontract/cli-examples/main/datacontract.yaml
```

### Result Log
```yaml
Run ./datacontract breaking --with https://raw.githubusercontent.com/datacontract/cli-examples/main/datacontract.yaml
Found 1 differences between the data contracts!

🔴 Difference 1:
Description:  field 'my_table.my_column_2' was removed
Type:         field-removed
Severity:     breaking
Level:        field
Model:        my_table
Field:        my_column_2
Exiting application with error: found breaking differences between the data contracts 
Error: Process completed with exit code 1.
```
