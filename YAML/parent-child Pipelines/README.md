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

All these options are in the mainPipeline.yml in this project.
Now you can "convert" them into variables:

```
variables:
  - group: ${{ parameters.Environment }}

  - name: numberParameter
    value: ${{ parameters.numberParameter }}
  - name: listParameter
    value: ${{ parameters.listParameter }}
  - name: booleanParameter
    value: ${{ parameters.booleanParameter }}
```
In this block, you can see the option `group`. Here we take a parameter (the one called Environment, the radiobutton-like one) and pass it as a variable group. We're telling Azure DevOps to look in the Library for a variable group called whatever ${{ parameters.Environment }} is:


![VariableGroup](https://github.com/fergavgo/Azure-DevOps/blob/2a6cd62838dd5ce7920d86055f0f98f78d4b382d/YAML/parent-child%20Pipelines/VariableGroup.png)


Finally, we take all the those options and pass them as parameters to the child pipeline:

```
jobs:
  - template: ../child/Pipeline.yaml
    parameters:
      numberParameter: $(numberParameter)
      listParameter: $(listParameter)
      booleanParameter: $(booleanParameter)
      Something_In_Group: $(Something_In_Group)
```


Now, in the `child/Pipeline.yaml` you'll see some task that are using the parameters/variables we passed from the Parent Pipeline:

```
Write-host $(booleanParameter) $(listParameter)
Write-host $(numberParameter) $(Something_In_Group)
```

We can even push further and pass this variables to ANOTHER in a Task. For instance, a Powershell script that execute a JMeter test plan ;)