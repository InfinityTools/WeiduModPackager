# WeiDU Mod Package Builder
*A GitHub action that automates creation of WeiDU mod packages.*

## Overview

A customizable GitHub action that automates creation of WeiDU mod packages. It provides options to create "iemod" packages for Project Infinity or platform-specific zip archives for Windows, Linux and macOS. Moreover, it is possible to control the WeiDU binary architecture (e.g. `amd64` or `x86`) as well as generation of a version suffix for the mod packages.

*See README of the [`all-version`](../../tree/all-version) branch for more detailed information about how to include this action in your own projects.*

## Configuration

This branch builds WeiDU mod packages for
- iemod
- Windows (`x86-legacy`)
- Linux (`amd64`)
- macOS (`amd64`)

It uses the tp2 filename as package names and uses the git release tag name as version suffix for the package filenames.
