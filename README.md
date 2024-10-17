# WeiDU Mod Packager
*A GitHub action that automates creation of WeiDU mod packages.*

## Overview

A customizable GitHub action that automates creation of WeiDU mod packages. It provides options to create "iemod" packages for Project Infinity or platform-specific zip archives for Windows, Linux and macOS. Moreover, it is possible to control the WeiDU binary architecture (e.g. `amd64` or `x86`) as well as generation of a version suffix for the mod packages.

*See README of the [`all-version`](../../tree/all-version) branch for more detailed information about how to include this action in your own projects.*

## Configuration

This branch builds WeiDU mod packages for
- iemod
- Multi-platform (`amd64`)

It uses the tp2 filename as package names and uses the mod's own tp2 `VERSION` string as version suffix for the package filenames.
