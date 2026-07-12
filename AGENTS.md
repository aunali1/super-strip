# AGENTS.md

## Cursor Cloud specific instructions

`sstrip` is a single C command-line utility (with a helper static library under `elfrw/`) that removes non-essential data from ELF executables. There are no runtime services, databases, or network dependencies — development is a plain compile-and-run cycle.

- Build: `make` (from the repo root). It recurses into `elfrw/` to build `elfrw/libelfrw.a` first, then links `sstrip`. See `Makefile` and `elfrw/Makefile`.
- Lint: there is no separate linter; the Makefiles compile with `-Wall -Wextra`, so a clean `make` is the lint signal. Run `make clean && make` to force a full recompile.
- Test: there is no automated test suite. Test end-to-end by running the binary against a real ELF file, e.g. `./sstrip <elf-file>`, then confirm the file is smaller and still executes.
- Toolchain (`gcc`, `make`, `ar`, libc headers) is provided by the system image, so the startup update script is a no-op. Nothing needs installing to build.
- Note: `elfrw/libelfrw.a` is not rebuilt automatically when the `elfrw/*.c` sources change (the top-level `Makefile` only rebuilds `sstrip`). After editing anything under `elfrw/`, run `make -C elfrw clean` (or `make clean` there) before rebuilding, or the stale archive will be relinked.
