# WeiDU Mod Packager
*A GitHub action that automates creation of WeiDU mod packages.*

## Overview

A customizable GitHub action that automates creation of WeiDU mod packages. It provides options to create "iemod" packages for Project Infinity or platform-specific zip archives for Windows, Linux and macOS. Moreover, it is possible to control the WeiDU binary architecture (e.g. `amd64` or `x86`) as well as generation of a version suffix for the mod packages.

## Configuration

This branch (`all-version`) builds WeiDU mod packages for
- iemod
- Windows (`amd64`)
- Linux (`amd64`)
- macOS (`amd64`)

It uses the tp2 filename as package names and uses the mod's own tp2 `VERSION` string as version suffix for the package filenames.

## Available templates:

| Template | Description |
| -------- | ----------- |
| `all-version` | This GitHub action template. It includes 64-bit setup binaries in the platform-specific mod packages for Windows, Linux and macOS, and uses the mod's own tp2 `VERSION` string as version suffix for mod package names. |
| [`all-version-x86`](../../tree/all-version-x86) | GitHub action template which includes a 32-bit setup binary in the Windows mod package and 64-bit setup binaries in Linux and macOS packages, and uses the mod's own tp2 `VERSION` string as version suffix for mod package names. |
| [`all-version-x86-legacy`](../../tree/all-version-x86-legacy) | GitHub action template which includes a 32-bit legacy setup binary in the Windows mod package and 64-bit setup binaries in Linux and macOS packages, and uses the mod's own tp2 `VERSION` string as version suffix for mod package names. (*Note: This template should only be used for mods that contain filenames with non-ASCII characters, such as Infinity Animations.*) |
| [`all-tag`](../../tree/all-tag) | GitHub action template which includes 64-bit setup binaries in the platform-specific mod packages for Windows, Linux and macOS, and uses the release tag name as version suffix for mod package names. |
| [`all-tag-x86`](../../tree/all-tag-x86) | GitHub action template which includes a 32-bit setup binary in the Windows mod package and 64-bit setup binaries in Linux and macOS packages, and uses the release tag name as version suffix for mod package names. |
| [`all-tag-x86-legacy`](../../tree/all-tag-x86-legacy) | GitHub action template which includes a 32-bit legacy setup binary in the Windows mod package and 64-bit setup binaries in Linux and macOS packages, and uses the release tag name as version suffix for mod package names. |
| [`multi-version`](../../tree/multi-version) | GitHub action template which includes setup binaries and scripts for all supported platforms (Windows, Linux and macOS) into a single mod package. It includes 64-bit setup binaries and uses the mod's own tp2 `VERSION` string as version suffix for mod package names. |
| [`multi-version-x86`](../../tree/multi-version-x86) | GitHub action template which includes setup binaries and scripts for all supported platforms (Windows, Linux and macOS) into a single mod package. It includes a 32-bit setup binary for Windows and uses the mod's own tp2 `VERSION` string as version suffix for mod package names. |
| [`multi-version-x86-legacy`](../../tree/multi-version-x86-legacy) | GitHub action template which includes setup binaries and scripts for all supported platforms (Windows, Linux and macOS) into a single mod package. It includes a 32-bit legacy setup binary for Windows and uses the mod's own tp2 `VERSION` string as version suffix for mod package names. |
| [`multi-tag`](../../tree/multi-tag) | GitHub action template which includes setup binaries and scripts for all supported platforms (Windows, Linux and macOS) into a single mod package. It includes 64-bit setup binaries and uses the release tag name as version suffix for mod package names. |
| [`multi-tag-x86`](../../tree/multi-tag-x86) | GitHub action template which includes setup binaries and scripts for all supported platforms (Windows, Linux and macOS) into a single mod package. It includes a 32-bit setup binary for Windows and uses the release tag name as version suffix for mod package names. |
| [`multi-tag-x86-legacy`](../../tree/multi-tag-x86-legacy) | GitHub action template which includes setup binaries and scripts for all supported platforms (Windows, Linux and macOS) into a single mod package. It includes a 32-bit legacy setup binary for Windows and uses the release tag name as version suffix for mod package names. |
| [`win-version`](../../tree/win-version) | GitHub action template which only creates iemod and Windows mod packages (with a 64-bit setup binary), and uses the mod's own tp2 `VERSION` string as version suffix for mod package names. |
| [`win-version-x86`](../../tree/win-version-x86) | GitHub action template which only creates iemod and Windows mod packages (with a 32-bit setup binary), and uses the mod's own tp2 `VERSION` string as version suffix for mod package names. |
| [`win-version-x86-legacy`](../../tree/win-version-x86-legacy) | GitHub action template which only creates iemod and Windows mod packages (with a 32-bit legacy setup binary), and uses the mod's own tp2 `VERSION` string as version suffix for mod package names.|
| [`win-tag`](../../tree/win-tag) | GitHub action template which only creates iemod and Windows mod packages (with a 64-bit setup binary), and uses the release tag name as version suffix for mod package names. |
| [`win-tag-x86`](../../tree/win-tag-x86) | GitHub action template which only creates iemod and Windows mod packages (with a 32-bit setup binary), and uses the release tag name as version suffix for mod package names. |
| [`win-tag-x86-legacy`](../../tree/win-tag-x86-legacy) | GitHub action template which only creates iemod and Windows mod packages (with a 32-bit legacy setup binary), and uses the release tag name as version suffix for mod package names. |

## How to use

This repository contains several preconfigured workflows for use in your own mods. To do this select the desired branch from the dropdown list and download the code by clicking on "Code" > Download ZIP. Unpack the downloaded zip file and copy the `.github` folder to the root folder of your own mod project.

Alternatively, select "Add file" > Create new file, enter `.github/workflows/WeiduModPackager.yml` into the filename input field (do not skip the leading dot `.`) and copy-paste the content of ".github/workflows/WeiduModPackager.yml" from the unpacked zip file. Finally, commit the changes to your project.

The action is invoked automatically whenever a new release is published. It can take a few moments for the mod packages to appear on the release page. Details about the mod package generation process can be inquired on the "Actions" page.

**Note:** Make sure the project has the required permissions in the project's Settings > Actions > General. Actions permissions should be set to "Allow all actions and reusable workflows".

## Create your own workflow

To create your own workflow you can either use one of preconfigured templates and adjust it accordingly to your needs, or create a workflow script yourself in the `.github/workflows` folder of your git project.

This is a basic workflow file as yaml script with detailed information about available parameters:
```yaml
name: 'My own WeiDU Mod Packager'

on:
  # Trigger this action whenever a release is published.
  release:
    types: [published]
  # Can be omitted if you don't want to call the script manually.
  workflow_dispatch:

jobs:
  call_workflow:
    # Important: The action requires write access to work properly
    permissions:
      contents: write

    # This directive executes the mod package build script.
    uses: InfinityTools/WeiduModPackagerLibrary/.github/workflows/WeiduModPackagerLibrary.yml@master
    # The "with" directive allows you to customize mod package generation.
    with:
      # "type" determines the resulting archive format.
      # Supported keywords: iemod, windows, linux, macos, multi
      # "iemod" creates a .iemod package for Project Infinity.
      # "windows", "linux" and "macos" create a zip file which includes a compatible setup binary.
      # "macos" also contains a .command script file.
      # Platform-specific package names will be prefixed by "win-", "lin-" and "mac-" respectively.
      # "multi" creates a zip file that includes setup binaries and scripts for all supported platforms
      # (Windows, Linux, macOS).
      # Notes about multi-platform archives:
      # 1) Size of the mod package will increase by about 10 MB compared to "iemod".
      # 2) On Windows platforms users will run setup-*.exe directly.
      #    For macOS and Linux interactive mod installation will be invoked by a script
      #    ("setup-*.command" for macOS, "setup-*.sh" for Linux), and the WeiDU binaries
      #    are placed into the "weidu_external/tools/weidu/{platform}" folder structure.
      # "iemod" is used if this parameter is omitted.
      type: windows

      # "architecture" defines the architecture of the included setup binary.
      # This is currently only relevant if "type" is "windows". Other platforms provide architecture-
      # specific binaries only for WeiDU version 246.
      # Supported keywords: amd64, x86, x86-legacy
      # Specify "x86-legacy" to include a setup binary that is still compatible with older Windows
      # versions and does not mangle non-ASCII characters in filenames. This can be useful for
      # specific mods, such as Generalized Biffing in combination with Infinity Animations.
      # "amd64" is used if this parameter is omitted.
      architecture: amd64

      # "suffix" defines the version string that is appended to the mod package filename.
      # Specify one of the predefined constants:
      # - version: Attempts to fetch the VERSION string from the mod's own tp2 file.
      # - none:    Indicates that no version suffix is added.
      # Everything else is treated as a literal string that is added to the mod package name.
      # Important: Do not include spaces in the suffix string. The build script may not handle it
      #            properly.
      # Note: Special characters (such as :, <, >, or ?) are replaced by underscores.
      # "version" is used if this parameter is omitted.
      suffix: ${{ github.event.release.tag_name }}

      # An arbitrary string that will be appended after the package base name but before the version
      # suffix. This can be used to further specialize a package name (e.g. when providing packages
      # for different architectures for the same platform).
      # Unused if this parameter is omitted.
      extra: ''

      # This parameter defines the mod package base name.
      # Supported types: tp2, ini. Anything else is treated as a literal string.
      # - tp2: Uses the tp2 filename as base for generating the mod package base name.
      # - ini: Fetches the "Name" definition from the associated Project Infinity metadata ini file.
      #        Falls back to "tp2" if not available.
      # "tp2" is used if this parameter is omitted.
      naming: ini

      # This parameter defines the tp2 filename of the mod to include in the mod package.
      # Specifying this option is only useful if a project contains multiple mods
      # (e.g. EET, EET_end, EET_gui).
      # All available mods are included if this parameter is omitted.
      tp2_name: ''

      # WeiDU version to use for the setup binaries for platform-specific zip archives.
      # Specify "latest" to use the latest WeiDU version, or a specific WeiDU version.
      # Currently supported versions: 246 or later.
      # "latest" version is used if this parameter is omitted.
      weidu_version: latest

      # Defines the prefix string to use for Windows-specific zip archive names.
      # "win" is used if this parameter is omitted.
      prefix_windows: win

      # Defines the prefix string to use for Linux-specific zip archive names.
      # "lin" is used if this parameter is omitted.
      prefix_linux: lin

      # Defines the prefix string to use for macOS-specific zip archive names.
      # "mac" is used if this parameter is omitted.
      prefix_macos: mac

      # Specifies whether to enable or disable the WeiDU auto-update feature in setup scripts of
      # multi-platform zip archives (see "type" parameter above).
      # "true" is used if this parameter is omitted.
      multi_autoupdate: true

      # Indices whether duplicate files which only differ in case should be preserved when found in
      # the same folder of the mod.
      # Enabling this option can be useful on Linux where filesystems are case-sensitive by default.
      # Otherwise, duplicate files with the oldest modification date are removed.
      # "false" is used if this parameter is omitted.
      case_sensitive: false
```
