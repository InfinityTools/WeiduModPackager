# WeiDU Mod Package Builder
*A GitHub action that automates creation of WeiDU mod packages.*

## Overview

A customizable GitHub action that automates creation of WeiDU mod packages. It provides options to create "iemod" packages for Project Infinity or platform-specific zip archives for Windows, Linux and macOS. Moreover, it is possible to control the WeiDU binary architecture (e.g. `amd64` or `x86`) as well as generation of a version suffix for the mod packages.

## Configuration

This branch (`all-version`) builds WeiDU mod packages for
- iemod
- Windows (`amd64`)
- Linux (`amd64`)
- macOS (`amd64`)

and uses the mod's own tp2 `VERSION` string as version suffix for the package filenames.

## Available templates:

| Template | Description |
| -------- | ----------- |
| `all-version` | This GitHub action template. It includes a 64-bit setup binary in the platform-specific mod packages and uses the mod's own tp2 `VERSION` string as version suffix for mod package names. |
| [`all-version-x86`](../../tree/all-version-x86) | GitHub action template which includes a 32-bit setup binary in the Windows mod package and uses the mod's own tp2 `VERSION` string as version suffix for mod package names. |
| [`all-version-x86-legacy`](../../tree/all-version-x86-legacy) | GitHub action template which includes a 32-bit legacy setup binary in the Windows mod package and uses the mod's own tp2 `VERSION` string as version suffix for mod package names. (*Note: This template should only be used for mods that contain filenames with non-ASCII characters, such as Infinity Animations.*) |
| [`all-tag`](../../tree/all-tag) | GitHub action template which includes a 64-bit setup binary in the platform-specific mod packages and uses the release tag name as version suffix for mod package names. |
| [`all-tag-x86`](../../tree/all-tag-x86) | GitHub action template which includes a 32-bit setup binary in the Windows mod package and uses the release tag name as version suffix for mod package names. |
| [`all-tag-x86-legacy`](../../tree/all-tag-x86-legacy) | GitHub action template which includes a 32-bit legacy setup binary in the Windows mod package and uses the release tag name as version suffix for mod package names. |
| [`win-version`](../../tree/win-version) | GitHub action template which only creates iemod and Windows mod packages (with a 64-bit setup binary), and uses the mod's own tp2 `VERSION` string as version suffix for mod package names. |
| [`win-version-x86`](../../tree/win-version-x86) | GitHub action template which only creates iemod and Windows mod packages (with a 32-bit setup binary), and uses the mod's own tp2 `VERSION` string as version suffix for mod package names. |
| [`win-version-x86-legacy`](../../tree/win-version-x86-legacy) | GitHub action template which only creates iemod and Windows mod packages (with a 32-bit legacy setup binary), and uses the mod's own tp2 `VERSION` string as version suffix for mod package names.|
| [`win-tag`](../../tree/win-tag) | GitHub action template which only creates iemod and Windows mod packages (with a 64-bit setup binary), and uses the release tag name as version suffix for mod package names. |
| [`win-tag-x86`](../../tree/win-tag-x86) | GitHub action template which only creates iemod and Windows mod packages (with a 32-bit setup binary), and uses the release tag name as version suffix for mod package names. |
| [`win-tag-x86-legacy`](../../tree/win-tag-x86-legacy) | GitHub action template which only creates iemod and Windows mod packages (with a 32-bit legacy setup binary), and uses the release tag name as version suffix for mod package names. |

## How to use

This repository contains several preconfigured workflows for use in your own mods. To do this select the desired branch from the dropdown list and download the code by clicking on "Code" > Download ZIP. Unpack the downloaded zip file and copy the `.github` folder to the root folder of your own mod project.

Alternatively, select "Add file" > Create new file, enter `.github/workflows/WeiduPackageBuilder.yml` into the filename input field (do not skip the leading dot `.`) and copy-paste the content of ".github/workflows/WeiduPackageBuilder.yml" from the unpacked zip file. Finally, commit the changes to your project.

The action is invoked automatically whenever a new release is published. It can take a few moments for the mod packages to appear on the release page. Details about the mod package generation process can be inquired on the "Actions" page.

**Note:** Make sure the project has the required permissions in the project's Settings > Actions > General. Actions permissions should be set to "Allow all actions and reusable workflows".

## Create your own workflow

To create your own workflow you can either use one of preconfigured templates and adjust it accordingly to your needs, or create a workflow script yourself in the `.github/workflows` folder of your git project.

This is a basic workflow file as yaml script:
```yaml
name: 'My own WeiDU Mod Package Builder'

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
      # Supported keywords: iemod, windows, linux, macos
      # "iemod" creates a .iemod package for Project Infinity.
      # "windows", "linux" and "macos" create a zip file which includes a compatible setup binary.
      # "macos" also contains a .command script file.
      # Platform-specific package names will be prefixed by "win-", "lin-" and "mac-" respectively.
      # "iemod" is used if this parameter is omitted.
      type: windows

      # "architecture" defines the architecture of the included setup binary.
      # This is currently only relevant if "type" is "windows".
      # Supported keywords: amd64, x86, x86-legacy
      # Specify "x86-legacy" to include a setup binary that is still compatible with older Windows versions
      # and does not mangle non-ASCII characters in filenames. (Useful for mods such as Infinity Animations.)
      # "amd64" is used if this parameter is omitted.
      architecture: amd64

      # "suffix" defines the version string that is appended to the mod package filename.
      # Specify one of the predefined constants:
      # - version: Attempts to fetch the VERSION string from the mod's own tp2 file.
      # - none:    Indicates that no version suffix is added.
      # Everything else is treated as a literal string that is added to the mod package name.
      # Important: Do not include spaces in the suffix string. The build script may not handle it properly.
      # Note: Special characters (such as :, <, >, or ?) are replaced by underscores.
      # "version" is used if this parameter is omitted.
      suffix: ${{ github.event.release.tag_name }}
```
