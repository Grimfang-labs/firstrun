# MVP specification

Status: proposed v0.1 contract. Changes should be discussed before implementation.

## Scope

Test selected Python Markdown examples against either a local wheel or an exactly pinned public package. One invocation uses one installed Python interpreter on one OS; CI supplies the matrix. Support Windows and Linux, Python 3.11 and the latest stable supported Python at implementation time.

No shell execution, remote recipes, notebook support, automatic README rewriting, private registry credentials, external services, telemetry, or model API dependency in v0.1. A venv isolates dependencies; it does not sandbox untrusted code.

## Example selection

An HTML comment immediately preceding a Python fenced block identifies it:

````markdown
<!-- firstrun: hello -->
```python
from example import greet
print(greet())
```
````

Proposed manifest:

```toml
schema_version = 1
readme = "README.md"

[[examples]]
id = "hello"
stdout = "Hello\n"
timeout_seconds = 60
```

Resolve `readme` relative to the manifest directory and require its resolved path to remain inside that directory. The first version supports top-level fenced Python blocks only; reject marked unsupported blocks rather than silently skipping them. IDs must be unique. Unknown manifest keys, duplicate selections, missing blocks, unsupported schema/language, empty selection, and invalid limits are preflight errors. Unmarked blocks are ignored.

## CLI contract

Exactly one of `--artifact PATH` and `--package NAME==VERSION` is required. v0.1 accepts wheels only in artifact mode. Reject URL, VCS, editable, and unpinned package targets. Extras can be introduced in a later schema revision after pilot evidence; no hidden development extras are installed.

`--json PATH` writes a versioned result. The manifest is required; no heuristic discovery. The CLI uses the interpreter running FirstRun to create the venv. Validate selected examples before creating environments or installing anything.

Exit codes: 0 all selected examples passed; 1 installation, execution, assertion, timeout, or output-limit failure; 2 invalid input/configuration; 3 internal tool/environment failure; 130 user interruption. JSON includes a more precise stage and reason.

## Execution

Resolve the wheel path before changing directories. Create a fresh venv without system site packages and a temporary working directory outside the checkout. Install only the selected target and its declared runtime dependencies. Clear PYTHONPATH and inherited pip configuration. Examples use the venv interpreter directly, never a shell.

Each example runs in a fresh process and working directory; the installed venv is shared. Python variables and generated files do not carry between examples. Do not import source-tree modules implicitly. Continue after ordinary example failures to report all results; abort on installation/environment failure or cancellation.

Stdout assertions are optional and exact except for CRLF-to-LF normalization on both expected and actual text. Preserve other whitespace. No regex assertions. Default example timeout is 60 seconds; manifest overrides must be positive and at most 300 seconds. Installation timeout is 300 seconds. Bound each captured stream to one MiB, terminate on overflow, and retain a truncated diagnostic.

Terminate descendants on timeout, overflow, and interruption. Always clean up temporary resources; report cleanup failure without hiding the original failure. Environment allowlisting and termination behavior must be tested separately on Windows and Linux.

## Result model

JSON schema version 1: tool version, run status, stage/reason, Python/OS, package target, wheel digest when applicable, resolved dependency versions, selected example count, and per-example ID, README path/line, status, exit code, duration, stdout/stderr, truncation flag. Durations use milliseconds. No environment values or credentials are included. Report an install failure even when no example ran. Preflight errors produce a structured result when a writable `--json` destination was supplied.

## Release acceptance criteria

- A correct wheel passes from outside its checkout on Windows and Linux.
- A missing data file is detected even when present in the checkout.
- An undeclared dependency fails even when installed in the developer environment.
- Duplicate/missing markers, invalid manifests, and empty selections fail before execution.
- Timeouts and output floods terminate descendants on both systems.
- Install failure and example failure remain distinguishable in console and JSON.
- Spaces, non-ASCII paths, and CRLF behave as specified.
- A parent-environment sentinel secret is absent from child processes.
- Cleanup runs after success, failure, and interruption.
- Internal tests use locally built fixtures and dependencies; registry/network behavior has a separate integration check.

Each criterion requires a regression test or explicit integration evidence before v0.1. Planning documents are not evidence that these behaviors exist.
