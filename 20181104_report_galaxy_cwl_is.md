# Galaxy+CWL: Implementation Study

Date: 20181104

## Overview

In order to run a CWL workflow with Galaxy-CWL, some modifications must be made in
Galaxy-CWL, Cwltool, CWL tools files and CWL workflow files.

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

* Preventing EDAM filetype checking in Cwltool.
* Adding 'Directory' type support in Galaxy using tar file
* Adding 'gx:interface' hints in CWL tools files
* Replacing relative-path with absolute-path in 'run' attributes of CWL workflow files (tools links).

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

##### Add 'beta_relaxed_fmt_check' to prevent file fmt check.

b02b33f
82b0d0c

#### ELIXIR Workflow

8fcf887  Use "gx:type: data" to select the tar file (i.e. the directory).
db248cd  Temporary fix for search_space_size error.

##### Replace relative paths with absolute paths.

a876284  

##### Modify label to extract tools name from Galaxy-CWL (#1).

8096119  

##### Add 'gx:interface' hints in tools CWL files

92ee2b9 
2d96dbc
7d0a7ad
abc3d8f 
d9d1610 
e5cb9a7 
5e11714 
fc5ab08 
a1dd63a 

## Problems that remain

* Tools default values not set when running a workflow (must be set manually)
* Tools need to be created in galaxy before importing and running a CWL workflow
(this is strange as dynamic tools are created on-the-fly when importing the workflow. This need further investigation.)
* Find alternative to replacing relative-path with absolute-path in CWL workflow files (CWL-pack ?)
* Find alternative to adding Directory type support using tar file (as it prevents tar files from being used for another purpose in Galaxy)

## DEMO server

* Url: http://sd-104052.dedibox.fr:8083
* Authentication login: democwl
* Galaxy demo user: demo-g-cwl-is.test

The DEMO server contains all the modifications described in this document and can successfully import and run the following workflow:

https://github.com/hmenager/workflow-is-cwl/workflows/cmsearch-multimodel-wf.cwl
