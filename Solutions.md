# What is Solutions?

Solutions are containers where developers package and maintain list of components that extend Microsoft Dataverse. For example, Dynamics 365 for Sales, Marketing, Customer Service apps are composed of solutions. Customizers and developers distribute solutions so that organizations can use Dataverse to install and uninstall the business functionality defined by the solution.

## Types Of Solutions:

- **Unmanaged solutions** are developed. Unmanaged solutions are used in development environments while you make changes to your application. Unmanaged solutions can be exported either as unmanaged or managed. Exported unmanaged versions of your solutions should be checked into your source control system. Unmanaged solutions should be considered your source for Microsoft Power Platform assets. When an unmanaged solution is deleted, only the solution container of any customizations included in it is deleted. All the unmanaged customizations remain in effect and belong to the default solution.

- **Managed solutions** are deployed. Managed solutions are deployed to any environment that isn't a development environment for that solution. This includes test, UAT, SIT, and production environments. Managed solutions can be serviced independently from other managed solutions in an environment. As an ALM best practice, managed solutions should be generated by exporting an unmanaged solution as managed. When a managed solution is deleted, the following data is lost, data stored in custom entities that are part of the managed solution and data stored in custom attributes that are part of the managed solution on other entities that are not part of the managed solution.

Additionally:

   - You can't edit components directly within a managed solution. To edit managed components, first add them to an unmanaged solution.
   - When you do this, you create a dependency between your unmanaged customizations and the managed solution. When a dependency exists, the managed solution can't be uninstalled until you remove the dependency.
   - Some managed components can’t be edited. To verify whether a component can be edited, view the Managed properties.
   - You can't export a managed solution.
   - When a managed solution is deleted (uninstalled), all the customizations and extensions included with it are removed.

When developing solutions, it is best practice to select your preferred solution to ensure that all componets created as part of the build are included. further information about preferred solutions can be found at [Set the preferred solution - Power Apps | Microsoft Learn](https://learn.microsoft.com/en-us/power-apps/maker/data-platform/preferred-solution)

## Solution layers:

Solution layering is implemented at a component level. Managed and unmanaged solutions exist at different layers within a Microsoft Dataverse environment. In Dataverse, there are two distinct layers:

- **Unmanaged layer** All imported unmanaged solutions and ad-hoc customizations exist at this layer. All unmanaged solutions share a single unmanaged layer. In managed environments best practice is to avoid having unmanaged layers. [Block unmanaged customizations in Dataverse](https://learn.microsoft.com/en-us/power-platform/alm/block-unmanaged-customizations)

- **Managed layer** All imported, managed solutions and the system solution exist at this level. When multiple managed solutions are installed, the last one installed is above the managed solution installed previously. This means that the second solution installed can customize the one installed before it. When two managed solutions have conflicting definitions, the runtime behavior is either "Last one wins" or a merge logic is implemented. If you uninstall a managed solution, the managed solution below it takes effect. If you uninstall all managed solutions, the default behavior defined within the system solution is applied. At the base of the managed layers level is the system layer. The system layer contains the entities and components that are required for the platform to function.

## Solution update and upgrade:

An upgrade involves replacing an existing solution with a new version. Updates, on the other hand, are designed to add components and update existing components. Updates are what we used to have when we did not have a choice of doing an upgrade or an update.

- **Update** If no components were deleted in your solution you may opt for an update. The update of your imported solution will occur **faster** than an upgrade because only the changes are imported.

- **Upgrade** If you delete an entities or fields, an upgrade means the deleted components will no longer exist in the environment you are importing the solution into. In the old way of doing things, deletes never occurred when you moved a solution into a new environment. With an upgrade, you are doing a full removal of the old solution and replacing with the new solution.

## Segmented solutions:

If you use segemented solutions, only include entity components that are updated when you distribute solution updates. With solution segmentation, you export solution updates with selected entity assets, such as entity fields, forms, and views, rather than entire entities with all the assets.

When dividing a solution into segments (**multiple solutions**), ensure that the segments are arranged in a sequence or order that prevents any issues with dependencies.

## Healthy ALM:

Transition from manual deployment to an automated approach, ensuring adherence to the following guidelines:

1. Consider adopting a source code control system like Azure DevOps or GitHub. Azure DevOps provides developer services for support teams to plan work, collaborate on code development, and build and deploy applications.

2. Environments that are managed ought to be centered around source control, which is beneficial for upholding a branching strategy in agile project workflows.

3. Export a solution from your development environment containing your apps and customizations, unpack your solution, and store the components in your source control system. Use Azure Pipelines to manage your components, and then deploy them to the target environment for testing. Finally, deploy to the production environment for user consumption.
