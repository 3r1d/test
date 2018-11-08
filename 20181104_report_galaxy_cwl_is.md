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

## Modifications description

### Overview

Main modifications are:

* Prevent EDAM filetype checking in Cwltool
* Add 'Directory' type support in Galaxy (using tar file)
* Add 'gx:interface' hints in CWL tools files
* Replace relative-path with absolute-path in 'run' attributes of CWL workflow files (tools links).
* Call '_init_dynamic_tools' method at Galaxy startup to populate '_tools_by_hash' variable.
* Assume yaml CWL workflow if exception occurs during json deserialization (when user click on 'import')

### Detailed description

#### Galaxy-CWL

##### Enable CWL workflow execution with GUI

'_tools_by_hash' must be initialized at galaxy startup to prevent error below
(which occurs when running a CWL workflow):

```
Traceback (most recent call last):
  File "lib/galaxy/web/framework/decorators.py", line 285, in decorator
    rval = func(self, trans, *args, **kwargs)
  File "lib/galaxy/webapps/galaxy/api/workflows.py", line 417, in workflow_dict
    ret_dict = self.workflow_contents_manager.workflow_to_dict(trans, stored_workflow, style=style)
  File "lib/galaxy/managers/workflows.py", line 384, in workflow_to_dict
    return self._workflow_to_dict_run(trans, stored)
  File "lib/galaxy/managers/workflows.py", line 408, in _workflow_to_dict_run
    module_injector.inject(step, steps=workflow.steps, exact_tools=False)
  File "lib/galaxy/workflow/modules.py", line 1505, in inject
    module = step.module = module_factory.from_workflow_step(self.trans, step, **kwargs)
  File "lib/galaxy/workflow/modules.py", line 1382, in from_workflow_step
    return self.module_types[type].from_workflow_step(trans, step, **kwargs)
  File "lib/galaxy/workflow/modules.py", line 815, in from_workflow_step
    module = super(ToolModule, Class).from_workflow_step(trans, step, tool_id=tool_id, tool_version=tool_version, tool_hash=tool_hash, **kwds)
  File "lib/galaxy/workflow/modules.py", line 88, in from_workflow_step
    module = Class(trans, **kwds)
  File "lib/galaxy/workflow/modules.py", line 772, in __init__
    self.tool = trans.app.toolbox.get_tool(tool_id, tool_version=tool_version, exact=exact_tools, tool_hash=tool_hash)
  File "lib/galaxy/tools/toolbox/base.py", line 439, in get_tool
    tool_id = self._tools_by_hash[tool_hash].id
KeyError: u'e4de79296ec91ab9b8d8d9d71f94044a2561c01b9fc708e5197d432b453fa297'
```

'filter' has been replaced by 'filter_by' to prevent error below:

```
  File "lib/galaxy/workflow/modules.py", line 771, in __init__
    trans.app.toolbox._init_dynamic_tools()
  File "lib/galaxy/tools/toolbox/base.py", line 183, in _init_dynamic_tools
    for dynamic_tool in self.app.dynamic_tool_manager.list_tools():
  File "lib/galaxy/managers/tools.py", line 117, in list_tools
    return self.query().filter(active=active)
TypeError: filter() got an unexpected keyword argument 'active'
```

9e0d85b  

##### Enable CWL workflow import with GUI

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
* At Galaxy startup, tools are loaded twice in order to populate '_tools_by_hash' variable.
* After the workflow run, the browser sends requests to Galaxy server without interruption (every second).
* Workflow import take a long time (maybe caused by the creation of each
  dynamic tools on-the-fly). Maybe a progress bar is needed to inform the user
  that the import process is running.
* Find alternative to assuming CWL workflow if exception occurs during json
  deserialization (when user click on 'import'). This is problematic as Galaxy
  native workflow may use yaml too in the future.


## DEMO server

* Url: http://sd-104052.dedibox.fr:8083
* Authentication login: democwl
* Galaxy demo user: demo-g-cwl-is.test

The DEMO server contains all the modifications described in this document and
can successfully import and run the following workflow: https://github.com/hmenager/workflow-is-cwl/workflows/cmsearch-multimodel-wf.cwl
