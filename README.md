# GE-Man - Glorious Eggroll version manager

![Demo gif](geman-demo.gif)

GE-Man is a version manager for GE Proton and Wine GE versions.

Currently, it has the following features:

* Manage Proton GE or Wine GE versions
  * Download versions by their Github release tag
  * Remove downloaded versions
  * Set the default Proton version for Steam
  * Set the default Wine version for Lutris
  * Check for the latest releases
  * List managed releases
* Copy a Proton user-settings file from one version to another

And these features are planned:

* Add labels to a version
* Apply a version by manually specifying its directory path
* Remove versions that are before release X to free disk space
* More functionality for the user-settings command
  * Apply a user-settings.py file to a version
  * Add a user-settings.py file to GE-Man to make it applicable

GE-Man consists of two Rust workspaces:

* A binary crate providing the application
* A library crate containing common functionality to deal with downloading GE versions, providing tag information and
  extracting downloaded archives.

# Building

This project is build by using [Cargo](https://doc.rust-lang.org/cargo/) - Rust's package manager.
Additionally, [cross](https://github.com/cross-rs/cross) can be used to verify builds for other architectures.

For local development the library crate [GE-Man-Lib] (https://github.com/Compile-Time/GE-Man-Lib) needs to be present on
the same directory root as the `GE-Man` directory (`../GE-Man-Lib`).

The library crate requires a version of OpenSSL to be present for linking. By default, the installed OpenSSL version of
the system is used. When building with cross the `vendored-openssl` feature must be used, so the build does not fail due
to no OpenSSL library being present.

# Installation

GE-Man can be installed with cargo or by downloading the precompiled binaries from the [release page]().

`cargo install ge-man`

When installing with cargo the resulting binary is placed into `$HOME/.cargo`. To make the binary accessible from
everywhere in a terminal add the `$HOME/.cargo` path to the `PATH` environment variable.

# Changelog

See [CHANGELOG.md](./CHANGELOG.md) for all changes.

# Usage

GE-Man provides the following commands:

* `add` - Add a GE Proton or Wine GE version
* `remove` (`rm`) - Remove a GE Proton version or Wine GE version
* `check` (`ck`) - Display the latest GE Proton, Wine GE and Wine GE LoL version
* `apply` - Set the default compatibility tool for Steam or Lutris
* `list` - List versions managed by ge-man
* `migrate` - (`mg`) - Make an existing GE version manageable by ge-man
* `user-settings` (`us`) - Commands that relate to Proton user-settings.py files
  * `copy` - Copy a user-settings.py file from on Proton version to another

Every command supports a `--help` argument to view possible parameters and general usage information.

## How do I add a new version?

```sh
# Proton GE
ge-man add -p GE-Proton7-8

# Wine GE
ge-man add -w GE-Proton7-6

# Wine GE for LoL
ge-man add -l 7.0-GE-1-LoL
```

You can also directly apply the downloaded version by using the `--apply` option.<br>
If no release is provided to the `-p`, `-w` and `-l` options, the latest release is downloaded.

## How do I remove a version?

```sh
# Proton GE
ge-man rm -p GE-Proton7-8

# Wine GE
ge-man rm -w GE-Proton7-6

# Wine GE for LoL
ge-man rm -l 7.0-GE-1-LoL
```

This operation will delete the versions file from the hard drive. If you wish to keep the files and only "forget"
the version in ge-man then use the `forget` command.

## How can I view the latest releases?

```sh
# All GE kinds
ge-man check

# Proton GE
ge-man check -p

# Wine GE
ge-man check -w

# Wine GE for LoL
ge-man check -l
```

## How can I remove a version without deleting its files?

```sh
# Proton GE
ge-man forget -p GE-Proton7-8

# Wine GE
ge-man forget -w GE-Proton7-6

# Wine GE for LoL
ge-man forget -l 7.0-GE-1-LoL
```

## How can I list the ge-man managed versions?

```sh
# All GE kinds
ge-man list

# Proton GE
ge-man list -p

# Wine GE
ge-man list -w

# Wine GE for LoL
ge-man list -l
```

## How can I make my existing GE versions manageable by GE-Man?

To make an existing version manageable by ge-man you need to use the `migrate` command. The `migrate` command takes a
path to a directory containing a GE version and the kind of GE version.

```sh
# Proton GE
ge-man migrate -s $HOME/.local/share/Steam/compatibilitytools.d/GE-Proton7-8 -p GE-Proton7-8

# Wine GE
ge-man migrate -s $HOME/.local/share/lutris/runners/wine/lutris-GE-Proton7-6-x86_64/ -w GE-Proton7-6

# Wine GE for LoL
ge-man migrate -s $HOME/.local/share/lutris/runners/wine/lutris-ge-7.0-1-lol-x86_64 -l 7.0-GE-1-LoL
```
