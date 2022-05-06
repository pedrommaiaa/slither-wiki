## Usage

- [How to run Slither](#how-to-run-slither)
  - [Truffle/Dapp/Etherlime](#truffledappetherlime) 
  - [Embark](#embark) 
  - [solc](#solc)
  - [Etherscan](#etherscan)
  - [AST input](#ast-file)
- [Options](#options)
  - [Detector selection](#detector-selection)
  - [Printer selection](#printer-selection)
  - [Path Filtering](#path-filtering)
  - [Triage mode](#triage-mode)
  - [Configuration file](#configuration-file)
- [IDE integrations](#ide-integration)

## How to run Slither

All the [`crytic-compile`](https://github.com/crytic/crytic-compile/wiki/Configuration) options are available through Slither.

### Truffle/Dapp/Etherlime

To run Slither on a Truffle/Dapp/Etherlime directory:
```
slither .
```

### Embark

To run Slither on a Embark directory, on the first run, use:
```
slither . --embark-overwrite-config
```

It will:
- install [@trailofbits/embark-contract-plugin](https://github.com/crytic/embark-contract-info).
- add `@trailofbits/embark-contract-plugin` to `embark.json` plugin list.

Following runs will not need the `--embark-overwrite-config` flag, you can run Slither with `slither .`.

Alternatively, you can do those two steps manually, and then directly run `slither .`

Ensure that you have [embark-contract-info](https://github.com/crytic/embark-contract-info) >= 1.1.0

### solc

To run Slither from a Solidity file:

```
slither file.sol
```

### Etherscan

To run Slither from a contract hosted on Etherscan, run

```
slither 0x7F37f78cBD74481E593F9C737776F7113d76B315
```

We recommend installing [solc-select](https://github.com/crytic/solc-select/) so Slither can switch to the expected solc version automatically.

### AST file

To run Slither on a AST file generated by solc, run:
```
slither file.ast.json
```

## Options

- To disable the solc warnings: `--solc-disable-warnings`
- To disable the output colorization: `--disable-color`
- To export the result to a json file: `--json file.json`
  - To export to stdout instead of a file, simply replace the filename with `-`

### Detector selection

Slither runs all its detectors by default.

To run only selected detectors, use `--detect detector1,detector2`. For example:
```
slither file.sol --detect arbitrary-send,pragma
```

To exclude detectors, use `--exclude detector1,detector2`. For example:
```
slither file.sol --exclude naming-convention,unused-state,suicidal
```

To exclude detectors with an informational or low severity, use `--exclude-informational` or `--exclude-low`.

`--list-detectors` lists [available detectors](https://github.com/trailofbits/slither/wiki/Detectors-Documentation).

### Printer selection

By default, no printers are run.

To run selected printers, use `--print printer1,printer2`. For example:
```
slither file.sol --print inheritance-graph
```

`--list-printers` lists [available printers](https://github.com/trailofbits/slither/wiki/Printers-Documentation).

### Path filtering

`--filter-paths path1` will exclude all the results that are only related to `path1`. The path specified can be a path directory or a filename. Direct string comparison and [Python regular expression](https://docs.python.org/3/library/re.html) are used.

Examples:
```
slither . --filter-paths "openzeppelin"
```
Filter all the results only related to openzeppelin.
```
slither . --filter-paths "Migrations.sol|ConvertLib.sol"
```
Filter all the results only related to the file `SafeMath.sol` or `ConvertLib.sol`.


### Triage mode

Slither offers two ways to remove results:
- By adding `//slither-disable-next-line DETECTOR_NAME` before the issue
- By running the triage mode (see below)

### Triage mode

`--triage-mode` runs Slither in its triage mode. For every finding, Slither will ask if the result should be shown for the next run. Results are saved in `slither.db.json`.

Examples:
```
slither . --triage-mode
[...]
0: C.destination (test.sol#3) is never initialized. It is used in:
	- f (test.sol#5-7)
Reference: https://github.com/trailofbits/slither/wiki/Vulnerabilities-Description#uninitialized-state-variables
Results to hide during next runs: "0,1,..." or "All" (enter to not hide results):  0
[...]
```

The second run of Slither will hide the above result.

To show the hidden results again, delete `slither.db.json`.

### Configuration File

Some options can be set through a json configuration file. By default,  `slither.config.json` is used if present (it can be changed through `--config-file file.config.json`).

Options passed via the CLI have priority over options set in the configuration file.

The following flags are supported:

```
{
    "detectors_to_run": "detector1,detector2",
    "printers_to_run": "printer1,printer2",
    "detectors_to_exclude": "detector1,detector2",
    "exclude_informational": false,
    "exclude_low": false,
    "exclude_medium": false,
    "exclude_high": false,
    "json": "",
    "disable_color": false,
    "filter_paths": "file1.sol,file2.sol",
    "legacy_ast": false
}
```

For flags related to the compilation, see the [`crytic-compile` configuration](https://github.com/crytic/crytic-compile/blob/master/crytic_compile/cryticparser/defaults.py)

## IDE integrations

* Remix: https://github.com/samparsky/remix-plugin-slither (http://remix.ethereum.org/ integration in progress)
* Embark: https://github.com/embark-framework/embark-slither (Work in progress)
* Visual Studio: https://github.com/samparsky/slither-vscode (Work in progress)