# Technical Explanation: Enhanced Input Migration System

## Overview

The Enhanced Input Migration feature is a comprehensive system designed to automatically convert legacy Unreal Engine input systems (based on `InputSettings`) to the newer Enhanced Input system introduced in more recent engine versions. This feature analyzes existing input mappings, creates appropriate Enhanced Input assets, and configures them to match the original behavior as closely as possible.

## End-to-End Migration Process

### 1. Workflow Initiation

The migration process begins when a user selects the "Migrate Legacy Input Actions" option from the Tools menu. This triggers the `OnMigrateLegacyInputClicked()` method in `FEnhancedInputCreationToolkitModule`, which presents a path picker dialog for the user to choose where to save the migrated input assets.

### 2. Directory Structure Creation

Once the user selects an output path, the system:

1. Calls `MigrateLegacyInput(const FString& OutputPath)` which:
   - Uses `FAssetUtilityHelper::CreateEnhancedInputDirectories()` to create appropriate subdirectories:
     - `/Actions` - For Input Action assets
     - `/Mappings` - For Input Mapping Context assets

### 3. Legacy Input Extraction

The `FLegacyInputExtractor` class handles retrieving existing input configurations:

1. `GetLegacyInputMappings()` loads the project's `UInputSettings` and extracts:
   - Action mappings (button presses)
   - Axis mappings (analog inputs like joysticks)
   
2. For each mapping, it captures:
   - Name (e.g., "Jump", "MoveForward")
   - Key bindings
   - Modifiers (Shift, Ctrl, Alt, Cmd)
   - Scale values (for axis inputs)

### 4. Enhanced Input Asset Creation

The `FLegacyInputMigrator` class orchestrates the migration process:

1. **Modifier Action Creation**: First creates special Input Actions for modifier keys (Shift, Ctrl, Alt, Cmd) via `CreateModifierActions()`, which are necessary for creating chorded inputs.

2. **Input Action Processing**: Uses `ProcessLegacyActions()` to analyze legacy actions and determine which Enhanced Input actions to create:
   - Special handling for common patterns like "MoveForward"/"MoveRight" (combined into a 2D vector)
   - Special handling for "LookUp"/"Turn" (combined for mouse/stick look)
   - Regular actions are converted one-to-one

3. **Asset Creation**:
   - Uses `FInputActionFactory` to create UInputAction assets
   - Configures appropriate value types (Boolean, Axis1D, Axis2D, etc.)
   - Sets appropriate triggers (Pressed, Released, etc.)

### 5. Mapping Context Creation

After creating Input Actions, the system creates Mapping Contexts to bind keys to actions:

1. Creates separate contexts for different input devices:
   - Keyboard & Mouse context
   - Gamepad context

2. Maps each input to the appropriate context using `MapActionsToContexts()`:
   - Regular key mappings are added to the appropriate context
   - Special handling for movement (WASD) via `AddWASDMappingsToMovement()`
   - Special handling for look (mouse XY) via `AddMouseXYMappingToLook()`
   - Chorded inputs (key combinations) are handled via `FChordedActionFactory`

### 6. Asset Saving

Each created asset is:

1. Added to the asset registry via `FAssetRegistryModule::AssetCreated()`
2. Packaged and saved to disk using `UPackage::SavePackage()` with the appropriate `FSavePackageArgs`

### 7. User Feedback

Upon completion:

1. Success or failure is logged via `UE_LOG()`
2. A message dialog informs the user of the result
3. If successful, users can immediately begin using the new Enhanced Input system

## Key Classes and Their Responsibilities

1. **FEnhancedInputCreationToolkitModule** - Main module that integrates with the editor UI and initiates migration

2. **FLegacyInputExtractor** - Extracts legacy input configurations from project settings

3. **FLegacyInputMigrator** - Core migration logic that analyzes inputs and creates Enhanced Input assets

4. **FInputActionFactory** - Creates and configures UInputAction assets

5. **FMappingContextFactory** - Creates and configures UInputMappingContext assets

6. **FChordedActionFactory** - Handles creation of chorded actions (key combinations)

7. **FAssetUtilityHelper** - Provides utility functions for asset and directory operations

## Technical Challenges and Solutions

1. **Handling Axis Combinations**:
   - Legacy systems often use separate axes for movement (MoveForward, MoveRight)
   - Enhanced Input combines these into 2D vectors
   - The migrator detects common patterns and combines them appropriately

2. **Modifiers and Chorded Inputs**:
   - Legacy inputs can use Shift/Ctrl/Alt modifiers
   - Enhanced Input requires explicit actions for these
   - The migrator creates modifier actions first, then uses them in chorded configurations

3. **Preserving Scale Values**:
   - Legacy axis mappings have scale values (e.g., -1.0 for reverse direction)
   - Enhanced Input handles this differently
   - The migrator applies appropriate modifiers to maintain the correct behavior

4. **Asset Naming and Organization**:
   - Enhanced Input uses a different naming convention
   - The migrator applies "IA_" prefixes for actions and organizes assets into appropriate directories

## Technical Benefits of Enhanced Input

The migration provides several technical advantages:

1. Better support for multiple input devices with dedicated mapping contexts
2. More flexible input processing with modifiers and triggers
3. Runtime rebinding capabilities
4. Support for different input behaviors based on context
5. Cleaner, more maintainable input code in gameplay classes

This migration system allows developers to leverage these benefits without manually recreating their entire input system.
