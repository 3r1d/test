# Galaxy+CWL: Implementation Study

Date: 20181104

## Overview

In order to run a CWL workflow with Galaxy-CWL, some modifications must be made in
Galaxy-CWL, Cwltool and CWL workflow files.

This document describes those modifications.

## Repositories used

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

Project documentation

```
https://github.com/hmenager/workflow-is-galaxy-hall
```

## Modifications

### Overview

Main modifications are:

* Adding 'gx:interface' hints in CWL tools files
* Preventing EDAM filetype checking
* Replacing relative-path with absolute-path in 'run' attributes of CWL workflow files (tools links).
* Adding Directory type support using tar file

### Detailed description

#### Galaxy-CWL

##### Enable CWL workflow execution through GUI
9e0d85b  

##### Enable CWL workflow import through GUI
f2a4645

##### Rename "Test Dataset".
7827974

##### Prevent flooding Galaxy left panel with tools description and label.
ad2f92b  

##### Map tar file to 'Directory' type.
22cc09f  

##### Add missing mapping between Galaxy type and CWL type.
64f6b95  

##### Fix 'ValidationException' which occurs when optional file is unset.
2e55c1c  

##### Add 'beta_relaxed_fmt_check' to prevent file fmt check.
2956b44  

##### Prevent call to get_size() when dataset is None.
1657c6d 
2d2ec56  

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

## Problems that remain

* Tools default values not set when running a workflow (must be set manually)
* Tools need to be created in galaxy before importing and running a CWL workflow
(if dynamic tools are created on-the-fly when importing the workflow, it should work. FIXME: this need further investigation)
* Find alternative to replacing relative-path with absolute-path in CWL workflow files (CWL-pack ?)
* Find alternative to adding Directory type support using tar file

## DEMO server

* Url: http://sd-104052.dedibox.fr:8083
* Authentication login: democwl
* Galaxy demo user: demo-g-cwl-is.test
