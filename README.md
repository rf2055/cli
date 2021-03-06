<h1 align="center">
  <br>
      <img src="assets/screen-1.png">
  <br>
</h1>

# Akamai CLI
[![Go Report Card](https://goreportcard.com/badge/github.com/akamai/cli)](https://goreportcard.com/report/github.com/akamai/cli) [![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fakamai%2Fcli.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fakamai%2Fcli?ref=badge_shield)


Akamai CLI is an ever-growing CLI toolkit for working with Akamai's API from the command line.

## Goals

- Simplicity
- Feature-full
- Consistent UX

## Available Packages

- [Akamai CLI for Property Manager](https://github.com/akamai/cli-property)
- [Akamai CLI for Purge](https://github.com/akamai/cli-purge)
- [Akamai CLI for Visitor Prioritization](https://github.com/akamai/cli-visitor-prioritization)

## Installation

Akamai CLI itself has no dependencies, but may rely on packages that can be written using any language and may require additional runtimes.

### Download a Release Binary

The easiest way to install Akamai CLI is to download a [release binary](https://github.com/akamai/cli/releases) for your platform and follow the instructions for your platform below. There are _no additional requirements_.

#### Linux and macOS

Once you have downloaded the appropriate binary for your system, you must make it executable, and optionally move it somewhere within your path.

```sh
$ chmod +x ~/Downloads/akamai-<VERSION>-<PLATFORM>
$ mv ~/Downloads/akamai-<VERSION>-<PLATFORM> /usr/local/bin/akamai
```

#### Windows

Once you have downloaded the appropriate binary for your system, no further actions
are required on your part, simply execute the binary from the command line.

### Using Homebrew

If you are using macOS, you can also install using the [Homebrew](https://brew.sh) package manager:

```sh
$ brew install akamai
```

This will install all necessary dependencies, compile, and install the binary — which will then be available globally.

### Using Docker

If you use (or want to use) [docker](http://docker.com), you can get a fully installed CLI instance by running:

```sh
$ docker run -ti akamaiopen/cli
```

The container contains Akamai CLI, as well as the `purge` and `property` subcommands pre-installed.  

> **Note**: When setting up your `.edgerc`, the `purge` subcommand defaults to the `ccu` credentials section, while the `property` subcommand uses the `papi` section. These can be changed using the `--section` flag.

### Compiling from Source

If you want to compile it from source, you will need Go 1.7 or later, and the [Glide](https://glide.sh) package manager installed:

1. Fetch the package:  
  `go get github.com/akamai/cli`
2. Change to the package directory:  
  `cd $GOPATH/src/github.com/akamai/cli`
3. Install dependencies using Glide:  
  `glide install`
4. Compile the binary:  
  - Linux/macOS/*nix: `go build -o akamai`
  - Windows: `go build -o akamai.exe`
5. Move the binary (`akamai` or `akamai.exe`) in to your `PATH`

### Credentials

Akamai CLI uses the standard Akamai OPEN credentials file, `.edgerc`. By default, it will look for credentials in your `HOME` directory.

You can override both the credentials file location, or the section, by passing the the `--edgerc` or `--section` flags to each command.

To set up your credential file, see the [authorization](https://developer.akamai.com/introduction/Prov_Creds.html) and [credentials](https://developer.akamai.com/introduction/Conf_Client.html) sections of the Get Started guide.

## Upgrading

Akamai CLI can automatically check for newer versions (at most, once per day). You will be prompted to enable this feature the first time you run Akamai CLI v0.3.0 or later.

If a new version is found, you will be prompted to upgrade. Choosing to do so will download the latest version in-place, and your original command will then be executed using the _new_ version.

Akamai CLI _automatically_ checks the SHA256 signature of the new version to verify it's validity.

To manually upgrade, see `akamai upgrade`

## Usage

All commands start with the `akamai` binary, followed by a `command`, and optionally an action or other arguments.

```
akamai [command] [action] [arguments...]
```

### Built-in commands

#### Help

Calling `akamai help` will show basic usage info, and available commands. To learn more about a specific command, use `akamai help <command> [sub-command]`.

#### List

Calling `akamai list` will show you a list of available commands. If a command is not shown, ensure that the binary is executable, and in your `PATH`.

#### Install

The `install` command allows you to easily install new packages from a git repository.

Calling `akamai install <package name or repository URL>` will download and install the command repository to the `$HOME/.akamai-cli` directory.

For Github repositories, you can pass in `user/repo` or `organization/repo`. For official Akamai packages, you can  omit the `akamai/cli-` prefix, so to install `akamai/cli-property` you can specify `property`.

For example, all of the following will install Akamai CLI for Property Manager from Github using various aliases:

```
akamai install property
akamai install akamai/cli-property
akamai install https://github.com/akamai/cli-property.git
```

You can specify _multiple_ packages to install at once.

#### Uninstall

To uninstall a package installed with `akamai install`, you call `akamai uninstall <command>`, where `<command>` is any command within that package.

You can specify _multiple_ packages to uninstall at once.

#### Update

To update a package installed with `akamai install`, you call `akamai update <command>`, where `<command>` is any command within that package.

You can specify _multiple_ packages to update at once.

Calling `akamai update` with no arguments will update _all_ packages installed using `akamai install`

#### Upgrade

Manually upgrade Akamai CLI to the latest version.

### Installed Commands

To call an installed command, use `akamai <command> [args]`, e.g.

```sh
akamai property create example.org
```

### Custom commands

Akamai CLI also provides a framework for writing custom CLI commands. These commands are contained in packages, which may have one or more commands within it.

There are a few requirements:

1. The package must be available via a Git repository (standard SSH public key authentication is supported)
2. The executable must be named `akamai-<command>` or `akamai<Command>`
3. Help must be visible when you run: `akamai-command help` and ideally, should allow for `akamai-command help <sub-command>`
4. If using OPEN APIs, it must support the `.edgerc` format, and must support both `--edgerc` and `--section` flags
5. If an action fails to complete, the executable should exit with a non-zero status code (however, `akamai` will only return `0` on success or `1` on failure)

You can use _any_ language to build commands, so long as the result is executable — this includes PHP, Python, Ruby, Perl, Java, Golang, JavaScript, and C#.

### Dependencies

Currently Akamai CLI supports automatically installing package dependencies using the following package managers:

- PHP: composer
- Python: pip (using requirements.txt)
- Ruby: bundler
- Golang: Glide
- JavaScript: npm and yarn

For other languages or package managers, all dependencies must be included in the package repository (i.e. by vendoring).

### Command Package Metadata

You *must* include a `cli.json` file to inform Akamai CLI about the command package and it's included commands.

`cli.json` allows you specify the command language runtime version, as well as define all commands included in package.

##### Example

```json
{
  "requirements": {
    "go": "1.8.0"
  },
  "commands": [
    {
      "name": "purge",
      "version": "0.1.0",
      "description": "Purge content from the Edge",
      "bin": "https://github.com/akamai/cli-purge/releases/download/{{.Version}}/akamai-{{.Name}}-{{.OS}}{{.Arch}}{{.BinSuffix}}"
    }
  ]
}
```

##### Format

- `requirements` — specify runtime requirements. You may specify a minimum version number or use `*` for any version. Possible requirements are:
  - `go`
  - `php`
  - `ruby`
  - `node`
  - `python`
- `commands` — A list of commands included in the package
  - `name` — The command name (used as the executable name)
  - `aliases` - An array of aliases that can be used to invoke the command
  - `version` — The command version
  - `description` - A short description of the command
  - `bin` — A url to fetch a binary package from if it cannot be installed from source

The `bin` URL may contain the following placeholders:

- `{{.Version}}` — The command version
- `{{.Name}}` — The command name
- `{{.OS}}` — The current operating system
  - Possible values are: `windows`, `mac`, or `linux`
- `{{.Arch}}` — The current OS architecture
  - Possible values are: `386`, `amd64`
- `{{.BinSuffix}}` — The binary suffix for the current OS
  - Possible values are: `.exe` for windows


## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fakamai%2Fcli.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fakamai%2Fcli?ref=badge_large)
