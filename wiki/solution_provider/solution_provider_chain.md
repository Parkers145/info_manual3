# How to use the solution provider with your deployments?

After following the step for creating your solution provider as mentioned [here](solution_provider.md).

You can now add the solutionProviderId to your deployment as follows:

- ### Grid3_client
```bash
vm.solutionProviderID = solutionProviderID;
```
This should be added to your deployments as shown below
![add_solutionProviderID](./img/grid3_solution.png)

After deploying the machine, The solutionProviderID will be shown in the machine specs
![specs](./img/machine_solution_provider.png)
