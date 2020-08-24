# sge

This profile configures [Snakemake](https://snakemake.readthedocs.io/en/stable/) to run on (Sun) Grid Engine.

## Setup

### Deploy profile

To deploy this profile, run

	mkdir -p ~/.config/snakemake
	cd ~/.config/snakemake
	cookiecutter https://github.com/drjbarker/snakemake-gridengine.git
  
  
Then, you can run Snakemake with

	snakemake --profile sge ...
  
### Cookiecutter options

* `profile_name` : A name to address the profile via the `--profile` Snakemake option.
* `cluster_config` : Path to a YAML or JSON configuration file analogues to the
  Snakemake [`--cluster-config` option](https://snakemake.readthedocs.io/en/stable/snakefiles/configuration.html#cluster-configuration-deprecated).
  This is also used to define custom resources on the SGE cluster.
  
### Default snakemake arguments
Default arguments to ``snakemake`` maybe adjusted in the ``<profile path>/config.yaml`` file.

## Parsing arguments to SGE (qsub)
Arguments are overridden in the following order, aliases are also defined and can be defined :

1) `QSUB_DEFAULTS` in `sge-submit.py`
2) Profile `cluster_config` file `__default__` entries
3) Snakefile threads and resources (time, mem)
4) Profile `cluster_config` file <rulename> entries
5) `--cluster-config` parsed to Snakemake (deprecated since Snakemake 5.10)

## Resource mapping

To allow more expressive resource requests we map some simple names to the SGE options and resources. These can be used for example in `cluster.yaml` to make the configuration simpler to read.

### Notes

Custom SGE resources can be specified in `__resources__` only in the profile folder (i.e. any `__resources__` in a local `--cluster-config cluster.yaml` will be ignored, but you can request the resources defined in the global profile). Custom resources are specified as a YAML dictionary where the key is the resource name as defined in SGE and the values are any aliases you want to use for this resource. The key will always be avaiable as a name even if you don't specifiy it as an alias. If a key already exists in the resource list the the aliases are just appended to that resource. 

For example:

```
__resources__:
  coproc_v100: 
    - "gpu"
    - "nvidia_gpu"
```

Allows you to request with `coproc_v100=1`, `gpu=1` or `nvidia_gpu=1` in the cluster config files or snakemake rule resources all of which will actually set `-j coproc_v100=1` for qsub.

Memory (`s_vmem`, `h_vmem` and aliases) must be given in gigabytes.

A full list of the default supported SGE options and resource requests with their aliases is:


| SGE Option       | Accepted aliases                   |
| -----------------|-------------------------------------------| 
| binding          | binding                                   |
| cwd              | cwd,                                      |
| e                | e, error                                  |
| hard             | hard,                                     |
| j                | j, join                                   |
| m                | m, mail_options                           |
| M                | M, email                                  |
| notify           | notify,                                   |
| now              | now,                                      |
| N                | N, name                                   |
| o                | o, output                                 |
| P                | P, project                                |
| p                | p, priority                               |
| pe               | pe, parallel_environment                  |
| pty              | pty,                                      |
| q                | q, queue                                  |
| R                | R, reservation                            |
| r                | r, rerun                                  |
| soft             | soft,                                     |
| v                | v, variable                               | 
| V                | V, export_env                             |
| qname            | qname,                                    |
| hostname         | hostname,                                 |
| calendar         | calendar,                                 |
| min_cpu_interval | min_cpu_interval,                         |
| tmpdir           | tmpdir,                                   |
| seq_no           | seq_no,                                   |
| s_rt             | s_rt, soft_runtime, soft_walltime         |
| h_rt             | h_rt, time, runtime, walltime             |
| s_cpu            | s_cpu, soft_cpu                           |
| h_cpu            | h_cpu, cpu                                |
| s_data           | s_data, soft_data                         |
| h_data           | h_data, data                              |
| s_stack          | s_stack, soft_stack                       |
| h_stack          | h_stack, stack                            |           
| s_core           | s_core, soft_core                         |
| h_core           | h_core, core                              |
| s_rss            | s_rss, soft_resident_set_size             |
| h_rss            | h_rss, resident_set_size                  |
| slots            | slots,                                    |
| s_vmem           | s_vmem, soft_memory,  soft_virtual_memory | 
| h_vmem           | h_vmem, mem, memory,  virtual_memory      | 
| s_fsize          | s_fsize, soft_file_size                   |
| h_fsize          | h_fsize, file_size                        |
