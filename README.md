# pytest-conda-solvers

```{image} https://img.shields.io/pypi/v/pytest-conda-solvers.svg
:alt: PyPI version
:target: https://pypi.org/project/pytest-conda-solvers
```

```{image} https://img.shields.io/pypi/pyversions/pytest-conda-solvers.svg
:alt: Python versions
:target: https://pypi.org/project/pytest-conda-solvers
```

```{image} https://github.com/zklaus/pytest-conda-solvers/actions/workflows/main.yml/badge.svg
:alt: See Build Status on GitHub Actions
:target: https://github.com/zklaus/pytest-conda-solvers/actions/workflows/main.yml
```

A pytest plugin to run conda solver tests

---

This [pytest] plugin was generated with [Cookiecutter] along with [@hackebrot]'s [cookiecutter-pytest-plugin] template.

## Features

- TODO

## Error assertion semantics

For unsatisfiable tests, this suite deliberately asserts more than conda's own
test suite does when running under conda-libmamba-solver. Upstream's
`assert_unsatisfiable` helper only checks the exception is an
`UnsatisfiableError` subclass there, because its entries comparison is gated on
the exact `UnsatisfiableError` type. Our runner additionally checks that the
endpoint package names of each expected conflict chain appear in the libmamba
error message, and `message_includes`/`message_excludes` fields in the YAML add
further content checks. This strengthening is intentional: cross-solver
consistency of error reporting is part of what this plugin exists to verify.
The endpoint-name check applies to libmamba only. Other solvers word their
messages differently (rattler may omit the requested package entirely), so
for them only the explicit `message_includes`/`message_excludes` fields
apply, matching upstream's type-only check.

## Solver applicability under rattler

conda-rattler-solver skips the feature-dependent tests of conda's shared
SolverTests suite ("conda-rattler-solver does not support features", see
https://github.com/conda-incubator/conda-rattler-solver/blob/main/tests/test_solver.py).
Those tests are already restricted to the classic solver in this suite, so
they are skipped under `--conda-solver=rattler` automatically. Known rattler
limitations beyond that (flexible channel priority) are carried as strict
per-entry `xfail_solvers: rattler` marks, so CI notices when an upstream fix
lands.

Tests with `add_pip: true` need no such marks. For them the channel server
serves pip-injected repodata under a parallel `/pip` route, where every
python 2.x/3.x record gains a pip dependency at index level, exactly like
upstream's test fixtures. The injection therefore reaches solvers that read
repodata directly, such as rattler, without relying on conda's
add_pip_as_python_dependency setting at solve time.

## Requirements

- TODO

## Installation

You can install "pytest-conda-solvers" via [pip] from [PyPI]:

```
$ pip install pytest-conda-solvers
```

## Usage

- TODO

## Contributing

Contributions are very welcome. Tests can be run with [tox], please ensure
the coverage at least stays the same before you submit a pull request.

## License

Distributed under the terms of the [BSD-3] license, "pytest-conda-solvers" is free and open source software

## Issues

If you encounter any problems, please [file an issue] along with a detailed description.

[@hackebrot]: https://github.com/hackebrot
[apache software license 2.0]: https://www.apache.org/licenses/LICENSE-2.0
[bsd-3]: https://opensource.org/licenses/BSD-3-Clause
[cookiecutter]: https://github.com/audreyr/cookiecutter
[cookiecutter-pytest-plugin]: https://github.com/pytest-dev/cookiecutter-pytest-plugin
[file an issue]: https://github.com/zklaus/pytest-conda-solvers/issues
[gnu gpl v3.0]: https://www.gnu.org/licenses/gpl-3.0.txt
[mit]: https://opensource.org/licenses/MIT
[pip]: https://pypi.org/project/pip/
[pypi]: https://pypi.org/project
[pytest]: https://github.com/pytest-dev/pytest
[tox]: https://tox.readthedocs.io/en/latest/
