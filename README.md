# gridengine

This is written for the arc4 cluster at the University of Leeds, but should be general enough to work in most SGE setups.

This profile configures [Snakemake](https://snakemake.readthedocs.io/en/stable/) to run on (Sun) Grid Engine.

- `config.yaml` contains default arguments for the profile
- `cluster.yaml` contains default options for grid engine’s qsub

Additional cluster options (including complex resources) can be given in a local yaml file using Snakemake’s `--cluster-config` flag. This also allows per-rule options. Example use cases would be when a certain rule needs to run in a certain queue or under a certain project or you want to use a complex resource which may not exist in other SGE configurations.

## Installation

On Linux, place the files into `~/.config/snakemake/sge` for snakemake to automatically find the profile.

## Usage

To use the profile (i.e. to submit tasks as jobs on in an SGE queue) use:

`snakemake --profile sge`

To also use a local `cluster.yaml` file in your working directory use;

`snakemake --profile sge --cluster-config cluster.yaml`

## qsub options

The options are read and overwritten in the following order:

1. Default options in `sge-submit.py`
2. Default options in the profile’s `cluster.yaml` file
3. Resources specified for the rule in the Snakemake file
4. Rule specific options in the profile’s `cluster.yaml` file
5. Default and rule specific options in the cluster config pass with `--cluster-config`

## Resource mapping

To allow more expressive resource requests we map some simple names to the SGE options and resources. These can be used for example in `cluster.yaml` to make the configuration simpler to read.


| SGE Option       | Accepted YAML key names                   |
| -----------------|-------------------------------------------| 
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
| nodes            | nodes, np                                 |
| coproc_v100      | coproc_v100, gpu                          |

