# Enhanced Input Creation Toolkit

The Enhanced Input Creation Toolkit plugin streamlines the process of migrating, templating, and generating Enhanced Input assets in Unreal Engine. All features are accessible directly from the Unreal Editor UI.

## Features
- Migrate Legacy Input Actions: Convert legacy input setups to the Enhanced Input system.
- Enhanced Input Templates: Quickly create input templates for common game types.
- Create Mapping Context from Input Actions Folder: Batch-generate a new Input Mapping Context from all Input Actions in a folder.

---
## Getting Started
1. Open the Plugin Menu

   After enabling the plugin and restarting the editor:
   - Go to the Tools menu in the main Unreal Editor menu bar.
   - Find the Enhanced Input Migration section.

   You will see the following options:
   - Migrate Legacy Input Actions
   - Enhanced Input Templates (submenu)
   - Create Mapping Context from Input Actions Folder

---
2. Migrate Legacy Input Actions

   1.	Click Migrate Legacy Input Actions.
   2.	Select the output folder for the migrated Enhanced Input assets.
   3.	The plugin will process and migrate your legacy input actions, displaying a success or error message when complete.
---
3. Create Enhanced Input Templates
   1.	Hover over Enhanced Input Templates to open the submenu.
   2.	Select a template (e.g., "First Person", "Third Person", etc.).
   3.	Choose the output folder for the template assets.
   4.	The plugin will generate the selected template in the chosen folder.
---
4. Create Mapping Context from Input Actions Folder
   1.	Click Create Mapping Context from Input Actions Folder.
   2.	Select the folder containing your existing Input Actions.
   3.	Select the output folder where the new Input Mapping Context should be saved.
   4.	Enter a name for the new Mapping Context asset.
   5.	The plugin will create a new Mapping Context asset that includes all Input Actions found in the source folder.
---
Notes
•	All actions are performed via modal dialogs for folder and asset name selection.
•	The plugin uses Unreal’s Content Browser and Asset Tools for asset creation and management.
•	If you encounter warnings about asset registry filters, ensure you are using Unreal Engine 5.2 or later and that the plugin is up to date.
---
Support
For issues or feature requests... TODO
