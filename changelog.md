# Changelog

## Changes in upcoming release (`dev` branch) 

### Components changes

- Added new `disableRR` param in the `spades` component that disables repeat
resolution

### New components

- Added component `abyss`.
- Added component `unicycler`.

### Minor/Other changes

- Added removal of duplicate IDs from `reads_download` component input.
- Added seed parameter to `downsample_fastq` component.
- Added default docker option to avoid docker permission errors.

### Bug fixes

- Fixed forks with same source process name.
- Fixed `inspect` issue when tasks took more than a day in duration.

## 1.3.1

### Features

- Added a new `clearInput` parameter to components that change their input.
The aim of this option is to allow the controlled removal of temporary files,
which is particularly useful in very large workflows.

### Components changes

- Updated images for components `mash_dist`, `mash_screen` and 
`mapping_patlas`.

### New components

- Added component `fast_ani`.

### Minor/Other changes

- Added `--export-directives` option to `build` mode to export component's 
directives in JSON format to standard output.
- Added more date information in `inspect` mode, including the year and the
locale of the executing system.

## 1.3.0

### Features
- Added `report` run mode to Flowcraft that displays the report of any given
pipeline in the Flowcraft's web application. The `report` mode can be executed
after a pipeline ended or during the pipeline execution using the `--watch`
option.
- Added standalone report HTML at the end of the pipeline execution.
- Components with support for the new report system:
    - `abricate`
    - `assembly_mapping`
    - `check_coverage`
    - `chewbbaca`
    - `dengue_typing`
    - `fastqc`
    - `fastqc_trimmomatic`
    - `integrity_coverage`
    - `mlst`
    - `patho_typing`
    - `pilon`
    - `process_mapping`
    - `process_newick`
    - `process_skesa`
    - `process_spades`
    - `process_viral_assembly`
    - `seq_typing`
    - `trimmomatic`
    - `true_coverage`

### Minor/Other changes

- Refactored report json for components `mash_dist`, `mash_screen` and 
`mapping_patlas`

### Bug fixes
- Fixed issue where `seq_typing` and `patho_typing` processes were not feeding
report data to report compiler.
- Fixed fail messages for `process_assembly` and `process_viral_assembly` 
components

## 1.2.2

### Components changes

- `mapping_patlas`: refactored to remove temporary files used to create
sam and bam files and added data to .report.json. Updated databases to pATLAS
version 1.5.2.
- `mash_screen` and `mash_dist`: added data to .report.json. Updated databases 
to pATLAS version 1.5.2.
- Added new options to `abricate` componente. Users can now provide custom database
directories, minimum coverage and minimum identity parameters.

### New components

- Added component `fasterq_dump`
- Added component `mash_sketch_fasta`
- Added component `mash_sketch_fastq`
- Added component `downsample_fastq` for FastQ read sub sampling using seqtk
- Added component `momps` for typing of Legionella pneumophila
- Added component `split_assembly`
- Added component `mafft`
- Added component `raxml`
- Added component `viral_assembly`
- Added component `progressive_mauve`
- Added component `dengue_typing`

### Minor/Other changes

- Added check for `params.accessions` that enables to report a proper
error when it is set to `null`.
- Added `build` option to export component parameters information in JSON format. 
- Fixed minor issue preventing the `maxbin2` and `split_assembly` components 
from being used multiples times in a pipeline
- Added a catch to the `filter_poly` process for cases where the input file is empty. 
- spades template now reports the exit code of spades' execution

### Bug fixes

- Removed the need for the nf process templates to have an empty line
at the beginning of the template files.
- Fixed issue when the `inspect` mode was executed on a pipeline directory
with failed processes but with the work directory removed (the log files
where no longer available).
- Fixed issue when the `inspect` mode was executed on a pipeline without the 
memory directory defined.
- Fixed issue in the `inspect` mode, where there is a rare race condition between
tags in the log and trace files.
- Fixed bug on `midas_species` process where the output file was not being 
linked correctly, causing the process to fail
- Fixed bug on `bowtie` where the reference parameter was missing the pid
- Fixed bug on `filter_poly` where the tag was missing

## 1.2.1

### Improvements

- The parameter system has been revamped, and parameters are now component-specific
and independent by default. This allows a better fine-tuning of the parameters
and also the execution of the same component multiple times (for instance in a fork)
with different parameters. The old parameter system that merged identical parameters
is still available by using the `--merge-params` flag when building the pipeline.
- Added a global `--clearAtCheckpoint` parameter that, when set to true, will remove
temporary files that are no longer necessary for downstream steps of the pipeline
from the work directory. This option is currently supported for the `trimmomatic`,
`fastqc_trimmomatic`, `skesa` and `spades` components. 

### New components

- `maxbin2`: An automatic tool for binning metagenomic sequences.
- `bowtie2`: Align short paired-end sequencing reads to long reference
sequences.
- `retrieve_mapped`: Retrieves the mapped reads of a previous bowtie2 mapping process.

### New recipes

- `plasmids`: A recipe to perform mapping, mash screen on reads
and also mash dist for assembly based approaches (all to detect
plasmids). This also includes annotation with abricate for the assembly.
- `plasmids_mapping`: A recipe to perform mapping for plasmids.
- `plasmids_mash`: A recipe to perform mash screen for plasmids.
- `plasmids_assembly`: A recipe to perform mash dist for plasmid
assemblies.

### Minor/Other changes

- Added "smart" check when the user provides a typo in pipeline string
for a given process, outputting some "educated" guesses to the
terminal.
- Added "-cr" option to show current recipe `pipeline_string`.
- Changed the way recipes were being parsed by `proc_collector` for the
usage of `-l` and `-L` options.
- Added check for non-ascii characters in colored_print.
- Fixed log when a file with the pipeline is provided to -t option
instead of a string.

### Bug fixes

- Fixed pipeline names that contain new line characters.
- Fixed pipeline generation when automatic dependencies were added right after a fork
- **Template: sistr.nf**: Fixed comparison that determined process status.
- Fixed issue with `--version` option.

## 1.2.0

### New components

- `card_rgi`: Anti-microbial resistance gene screening for assemblies
- `filter_poly`: Runs PrinSeq on paired-end FastQ files to remove low complexity sequences
- `kraken`: Taxonomic identification on FastQ files
- `megahit`: Metagenomic assembler for paired-end FastQ files
- `metaprob`: Performs read binning on metagenomic FastQ files
- `metamlst`: Checks the Sequence Type of metagenomic FastQ reads using Multilocus Sequence Typing
- `metaspades`: Metagenomic assembler for paired-end FastQ files
- `midas_species`: Taxonomic identification on FastQ files at the species level
- `remove host`: Read mapping with Bowtie2 against the target host genome (default hg19) and removes the mapping reads
- `sistr`: Salmonella *in silico* typing component for assemblies. 

### Features

- Added `inspect` run mode to flowcraft for displaying the progress overview
  during a nextflow run. This run mode has `overview` and `broadcast` options
  for viewing the progress of a pipeline.

### Minor/Other changes

- Changed `mapping_patlas` docker container tag and variable
(PR [#76](https://github.com/assemblerflow/assemblerflow/pull/76)).
- The `env` scope of nextflow.config now extends the `PYTHONPATH`
environmental variable.
- Updated indexes for both `mapping_patlas` and `mash` based processes.
- New logo!

### Bug Fixes

- **Template: fastqc_report.py**: Added fix to trim range evaluation.
- **Script: merge_json.py**: Fixed chewbbaca JSON merge function.
