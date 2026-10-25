---
id: getting-started
title: Getting Started
sidebar_position: 2
---

# Getting Started

This guide demonstrates how to install Cyanotype and generate a Bill of Materials (BoM) from a real mechanical assembly.

The commands run locally. No server or account is required.

## 1. Install Cyanotype

Download the latest release for your operating system from [GitHub Releases](https://github.com/tychonis/cyanotype/releases).

Cyanotype provides command-line binaries for Linux, macOS, and Windows.

Extract the archive and place the `cyanotype` executable in your `PATH`.

Verify the installation:

```bash
cyanotype version
```

### Build from source

Alternatively, install [Go](https://go.dev/dl/) 1.26 or later and build Cyanotype yourself:

```bash
git clone https://github.com/tychonis/cyanotype.git
cd cyanotype
go build -o cyanotype .
```

## 2. Clone the Example

We'll use an open-source pick-and-place machine as our example.

The [tychonis/pnp](https://github.com/tychonis/pnp) repository contains Cyanotype product definitions (`.bpo` files) generated from a mechanical CAD assembly and subsequently organized for readability.

```bash
git clone https://github.com/tychonis/pnp.git
cd pnp
```

## 3. Generate a BoM

Run:

```bash
cyanotype bom .pnp
```

Cyanotype reads the BPO files in the current directory, resolves the assembly rooted at `.pnp`, and generates a Bill of Materials.

By default, the output is CSV, which can be redirected to a file:

```bash
cyanotype bom .pnp > bom.csv
```

For a human-readable list:

```bash
cyanotype bom .pnp --format list
```

This command performs the computation in memory without connecting to a remote service.

## 4. Explore the Product Definition

Open `pnp.bpo` in your editor.

Cyanotype uses a declarative language based on HCL. Product definitions describe Items, CoItems, Processes, and CoProcesses, including their relationships and referenced artifacts.

Unlike a conventional BoM spreadsheet, the source captures how the product is structured and can be reviewed, versioned, and modified using Git.

## 5. Generate an Assembly Tree

To generate the assembly's structured representation:

```bash
cyanotype tree .pnp
```

This writes a `.pnp.bpc` file containing the resolved assembly tree in JSON format.

## 6. Explore More Commands

Cyanotype also supports committing product definitions into catalogs and other operations.

```bash
cyanotype --help
cyanotype bom --help
cyanotype tree --help
cyanotype commit --help
```

Some functionality, including remote publishing and CAD integration, is still evolving.

## Next Steps

- Explore the [Pick and Place Machine repository](https://github.com/tychonis/pnp).
- Try the [interactive demo](https://demo.tychonis.com) to explore a product structure visually.
- Browse the [Cyanotype source code](https://github.com/tychonis/cyanotype).

:::caution Early Development
Cyanotype is experimental software. Commands, product definitions, and data formats may change between releases.
:::
