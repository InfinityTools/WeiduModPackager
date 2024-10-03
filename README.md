# WeiDU Mod Package Builder
*A GitHub action that automates creation of WeiDU mod packages.*

## Overview

A customizable GitHub action that automates creation of WeiDU mod packages. It provides options to create "iemod" packages for Project Infinity or platform-specific zip archives for Windows, Linux and macOS. Moreover, it is possible to control the WeiDU binary architecture (e.g. `amd64` or `x86`) as well as generation of a version suffix for the mod packages.

*See README of branch [`all-version`](../all-version/README.md) for more detailed information about how to include this action in your own projects.*

## Configuration

This branch builds WeiDU mod packages for
- iemod
- Windows (`x86`)
- Linux (`amd64`)
- macOS (`amd64`)

and uses the mod's own tp2 `VERSION` string as version suffix for the package filenames.
