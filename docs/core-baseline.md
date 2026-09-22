# Core baseline status

The Linux baseline is intentionally limited to the application build and the
read/parser regression lanes used by the upstream continuous build.

The upstream DWG writer suite currently reports 20 failing test cases on the
latest snapshot. It is not used as a blocking Android-port gate until those
pre-existing writer failures are isolated and triaged separately. The suite
remains available as the explicit CMake target `librecad_dwg_write_fast_tests`.

The corpus smoke lane excludes `ordinary_enc_AC1021.dwg`, which currently
returns `BAD_READ_FILE_HEADER` in the standalone reader. This is kept explicit
in the workflow rather than silently treating the whole corpus as healthy.
