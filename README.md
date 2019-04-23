
# powershell_task_helper

A PowerShell helper library for use by [Puppet Tasks](https://puppet.com/docs/bolt/1.x/writing_tasks.html). It provides a class that handles error generation, simplifies JSON input and output, and makes testing your task easier. It requires Bolt >= 1.1 and Puppet Enterprise >= 2019.0.

#### Table of Contents

1. [Description](#description)
2. [Setup - The basics of getting started with powershell_task_helper](#setup)
    * [Beginning with powershell_task_helper](#beginning-with-powershell_task_helper)
3. [Usage - Configuration options and additional functionality](#usage)
4. [Limitations - OS compatibility, etc.](#limitations)
5. [Development - Guide for contributing to the module](#development)

## Description

This library handles parsing JSON input, serializing the result as JSON output, and producing a formatted error message for errors.

## Setup

### Beginning with powershell_task_helper

To use this library, include this module in a `Puppetfile`
```ruby
mod 'puppetlabs-powershell_task_helper'
```

Add it to your [task metadata](https://puppet.com/docs/bolt/1.x/writing_tasks.html#concept-677)
```json
{
  "files": ["powershell_task_helper/files/task_helper.ps1"],
  "input_method": "powershell"
}
```

## Usage

When writing your task include the library in your script by dot-sourcing the `task_helepr.ps1` file

```powershell
#!/usr/bin/env pwsh
[CmdletBinding()]
Param(
  [Parameter(Mandatory = $True)]
  [String]
  $Name
)

function Get-CurrentDirectory
{
  $thisName = $MyInvocation.MyCommand.Name
  [IO.Path]::GetDirectoryName((Get-Content function:$thisName).File)
}

$taskHelper = Join-Path (Get-CurrentDirectory) '../../powershell_task_helper/files/task_helper.ps1'
. $taskHelper

# TODO: consume functions from ps1
```

You can then run the task like any other Bolt task
```shell
bolt task run mymodule::task -n target.example.com name='Robert'
```

You can find this example in [examples](examples), as well as an example test in [tests](tests). For a real task, `examples` would be renamed to `tasks`.

You can additionally provide detailed errors by raising a `TaskError`, such as

```powershell
# TODO: ps equivalent of raising an error
class MyTask(TaskHelper):
    def task(self, args):
        raise TaskError('my task errored', 'mytask/error_kind', {'location': 'task entry'})
```

## Limitations

## Development

Puppet modules on the Puppet Forge are open projects, and community contributions are essential for keeping them great. We can’t access the huge number of platforms and myriad of hardware, software, and deployment configurations that Puppet is intended to serve.

We want to keep it as easy as possible to contribute changes so that our modules work in your environment. There are a few guidelines that we need contributors to follow so that we can have a chance of keeping on top of things.

For more information, see our [module contribution guide.](https://docs.puppet.com/forge/contributing.html)

### Contributors

To see who's already involved, see the [list of contributors.](https://github.com/puppetlabs/puppetlabs-powershell_task_helper/graphs/contributors)
