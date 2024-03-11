{{cookiecutter.project_name}}
{{ "=" * cookiecutter.project_name|length }}

{{cookiecutter.description}}

Description
-----------

This repository is intended to host one or more analysis scripts.

Instructions
------------

Each script should live in a separate directory under ``src``.
The script directory should contain at least the following files:

- ``run.py`` (required): the main python script that's going to be called to run the analysis.
  To avoid repeating the same code in multiple scripts, it's possible to decorate the main function with the decorator ``run_analysis`` found in ``common``.
  The decorated function should then accept a configuration dict in input, and return a metadata dict as output.
  The content of the input and output dictionaries may depend on the type of the analysis.
  Example::

    from common.utils import run_analysis

    @run_analysis
    def main(analysis_config: dict) -> dict:
        L.info("analysis_config:\n%s", analysis_config)
        ...
        return {"outputs": []}

- ``requirements.in`` (required): list of required packages to be installed in a virtualenv.
- ``requirements.txt`` (required): frozen list of required packages, automatically generated from ``requirements.in`` with ``tox -e freeze`` (see below for more details).
- ``modules.cfg`` (optional): module archive and list of modules to load.
  Example::

    module-archive: archive/2024-02
    modules: hpe-mpi/2.27.p1.hmpt



Automation
----------

To generate or update ``requirements.txt`` for a specific analysis, replace ``analysis_00`` with the desired subdirectory in ``src`` and run::

    PACKAGE=analysis_00 tox -e freeze

To run tests for a specific analysis, replace ``analysis_00`` with the desired subdirectory in ``src`` and run::

    PACKAGE=analysis_00 tox -e test

To format the code with ``isort`` and ``black``, run::

    tox -e format

To check the code with ``isort`` and ``black``, run::

    tox -e lint

To run a specific analysis, replace ``analysis_00`` with the desired subdirectory in ``src`` and run::

    PACKAGE=analysis_00 tox -e run analysis_config.json analysis_output.json

Alternatively, if you already have setup a virtualenv, you can run::

   /path/to/bin/run.sh analysis_00 analysis_config.json analysis_output.json

or::

    PYTHONPATH=/path/to/src python /path/to/src/analysis_00/run.py analysis_config.json analysis_output.json
