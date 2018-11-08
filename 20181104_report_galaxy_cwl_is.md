# Galaxy+CWL: Implementation Study

Creation date: 20181104

## Overview

In order to run a CWL workflow in Galaxy-CWL, some modifications must be made in
the software and in CWL files. This document describes those modifications.

## Repositories

Galaxy-CWL

```
https://github.com/hmenager/galaxy
```

Cwltool

```
https://github.com/hmenager/cwltool
```

ELIXIR Workflow

```
https://github.com/hmenager/workflow-is-cwl
```

## Modifications

### Overview

Main modifications are:

* Adding 'gx:interface' hints in CWL tools files
* Prevent EDAM filetype checking
* Replacing relative-path with absolute-path in  CWL workflow file (in 'run' attributes containing tools links)
* Adding Directory type support using tar file

### Detailed description

#### Galaxy-CWL

```
9e0d85b  Initialize '_tools_by_hash' at galaxy startup.
f2a4645  Add support for cwl workflow import (yaml fmt).
7827974  Rename "Test Dataset".
ad2f92b  Prevent flooding Galaxy left panel with tools description and label.
22cc09f  Map tar file to 'Directory' type.
64f6b95  Add missing mapping between Galaxy type and CWL type.
2e55c1c  Fix 'ValidationException' which occurs when optional file is unset.
dbb5520  Remove duplicate.
51b7031  Set beta_relaxed_fmt_check to true. Add workflow-is-cwl conf entries.
2956b44  Add 'beta_relaxed_fmt_check' to prevent file fmt check.
45e806f  Disable use of cwltool specific version.
6be9cf0  Disable 'md5sum_non_strict.cwl' CWL test.
8acd18c  Disable pre v1.0 CWL tests.
1657c6d  Prevent call to get_size() when dataset is None.
2d2ec56  Prevent call to get_size() when dataset is None.
```

#### Cwltool

```
b02b33f Add '--beta_relaxed_fmt_check' option.
82b0d0c Add 'beta_relaxed_fmt_check' in core modules.
```

#### ELIXIR Workflow

```
d5adeda  Add expected output for cmsearch-multimodel-wf.
a876284  Replace relative paths with absolute paths.
8096119  Modify label to extract tools name from Galaxy-CWL (#1).
bbfa1b0  Add example directory as tar file.
8fcf887  Use "gx:type: data" to select the tar file (i.e. the directory).
18bded8  Fix fmt and type in 'BUSCO' cwl file.
2d96dbc  Add 'gx:interface' hints in 'BUSCO' cwl file.
7d0a7ad  Fix incorrect hint for "genetic_codes" input.
92ee2b9  Add 'gx:interface' hints in 'Diamon.makedb' cwl file.
db248cd  Temporary fix for search_space_size error.
abc3d8f  Add 'gx:interface' hints in 'infernal-cmsearch' cwl file.
d9d1610  Add 'gx:interface' hints + disable Docker.
e5cb9a7  Add 'gx:interface' hints in 'TransDecoder.LongOrfs-v5.cwl' file.
c100095  Remove DockerRequirement class duplicate.
5e11714  Merge 'gx:interface' hints in HMMER cwl file.
fc5ab08  Add 'gx:interface' hints in 'cmsearch-deoverlap' cwl file.
a1dd63a  Add 'gx:interface' hints in HMMER cwl file.
6e402c8  Add InterProScan expected output.
dc013b6  Add Diamond expected output.
```

