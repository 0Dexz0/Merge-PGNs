# Merge PGNs

**Merge PGNs** is a small standalone console program that merges multiple PGN move lines into a single PGN while preserving variations.

## What it does

* Reads chess move lines from files placed in a `PGNs` folder next to the executable.
* Accepts **`.pgn`** and **`.txt`** files.
* Merges lines and variations into a single combined PGN string.
* The merged output **removes move numbers** (e.g. `1.`, `2.` are stripped), but the result still works if pasted into chess websites or programs that accept PGN without move numbers.
* Writes results to `Merged PGNs/<original-filename>.pgn` next to the executable.

## Important input rules

* **Each PGN must be a single line** in the file (one game per line).
* `.pgn` and `.txt` files are accepted.
* The program accepts comments (`{...}`), NAG markers (`$n`) and the `*` character in input lines is automatically removed as it is of no use.
* If the `PGNs` or `Merged PGNs` folders are missing, the program will create them next to the executable.

## Merge behavior

* **Comments (`{...}`)**: comments are preserved and **merged** when applicable. If the same move has different comments across PGNs, the resulting comment for that move will join them separated by `---` (space, three dashes). Example:

  Input A: `1. e4 {comment 1}`
  Input B: `1. e4 {comment 2}`

  Result (move comment): `{comment 1 --- comment 2}`

  If several PGNs contain the exact same comment for a move, that comment will appear only once (no unnecessary duplication).

* **NAGs (`$n`)**: NAG markers are **merged** for the same move. If a move has `$1` in one PGN and `$2` in another, the result will include both NAGs separated by a space: `$1 $2`.

  Example:

  Input A: `1. e4 $1`
  Input B: `1. e4 $2`

  Result: `1. e4 $1 $2`

  **Note:** the collected NAGs are kept in the final result for context and traceability that can be usefull for the user (e.g., to preserve all annotations or metadata), although boards or programs that display a single NAG visually will typically use the **last** NAG applied during the merge (in the example above, `$2`). This means the final move will include all found NAGs, but a UI that can show only one will usually show the last one.

* **The `*` character**: it no longer matters if lines contain `*` (game termination marker). The program will handle it and it will not prevent merging.

* **Variations**: variations from the PGNs are preserved and combined into a single PGN with variations; that is, the different continuation lines present in the input files are incorporated into the combined output so that all found continuations are reflected.

## Quick usage

1. Place the prebuilt executable in a folder.
2. Add your `.pgn` or `.txt` files into the `PGNs` subfolder (the program will create it if missing).
3. Run the executable (double-click or run from a terminal).
4. Select the file you want to merge.
5. Find merged files in the `Merged PGNs` folder next to the executable.

## Examples

**Example 1 — different comments**

File 1 (single-line):

```
1. e4 {Good opening} 1... e5
```

File 2 (single-line):

```
1. e4 {Aggressive play} 1... e5
```

Combined output (relevant fragment):

```
e4 {Good opening --- Aggressive play} e5
```

**Example 2 — different NAGs**

File 1:

```
1. e4 $1 e5
```

File 2:

```
1. e4 $2 e5
```

Combined output (relevant fragment):

```
e4 $1 $2 e5
```

(If a board can only display a single NAG, it will typically show `$2`, the last one applied during the merge.)

## Notes

* The tool is console-based and intended for batch merging of PGN move lines (with variations).
* It preserves variations and now also preserves and merges comments and NAGs.

## Upcoming

* Soon there will be an option to choose whether the merged PGN **includes move numbers** (for example `1.`, `1...`) or **omits them**. This will be configurable and optional, since most boards and programs interpret both forms correctly.

## Troubleshooting

* If no files are detected, make sure the `PGNs` folder is in the same location as the executable and that the files are `.pgn` or `.txt`.
* Ensure each line of the file contains a single PGN (one game per line).

## Feedback and Feature Requests

If you have any suggestions, ideas, or features you would like to see added to Merge PGNs, feel free to reach out or open an issue in this repository. Your feedback is welcome and appreciated!


## License and Usage Terms
This software and its source code are © 2025 0Dexz0. All rights reserved.

By downloading the program you accept the terms of use Read the [LICENSE](./LICENSE) file included in this repository.
