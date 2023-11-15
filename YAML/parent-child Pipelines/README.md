# Parent-Child Pipeline Example
In this example I wanted to show how the parameters you configure in an Azure DevOps Pipeline appears when you try to run it. 

This is how Azure DevOps show the parameters before running the pipeline:
![Pipeline](https://github.com/fergavgo/Azure-DevOps/blob/39e3b2e3cc0396853726505e736ce12a0ba618ed/YAML/parent-child%20Pipelines/RunPipeline.png)


This is for radiobutton-like options:

```
parameters:
- name: Environment
  displayName: 'Environment to Deploy'
  type: string
  default: Development
  values:
  - Development
  - UAT
  - Production
```


For text fields:

```
- name: numberParameter
  displayName: "Write a number"
  type: string
  default: 1
```

For a list:
```
- name: listParameter
  displayName: "listParameter"
  type: string
  default: Item 1
  values:
  - Item 1
  - Item 2
  - Item 3
  - Item 4
  - Item ...
```

For a checkbox:
```
- name: booleanParameter
  displayName: "This can be false or true"
  type: boolean
  default: false
```