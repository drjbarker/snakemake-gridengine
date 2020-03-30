# gridengine

This profile configures [Snakemake](https://snakemake.readthedocs.io/en/stable/) to run on (Sun) Grid Engine.

- `config.yaml` contains default arguments for the profile
- `cluster.yaml` contains default options for grid engine’s qsub

Additional cluster options (including complex resources) can be given in a local yaml file using Snakemake’s `--cluster-config` flag. This also allows per-rule options.

## qsub options

The options are read and overwritten in the following order:

1. Default options in `sge-submit.py`
2. Default options in the profile’s `cluster.yaml` file
3. Resources specified for the rule in the Snakemake file
4. Rule specific options in the profile’s `cluster.yaml` file
5. Default and rule specific options in the cluster config pass with `--cluster-config`


