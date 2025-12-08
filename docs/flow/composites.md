# Composite Nodes

Composite nodes allow you to group multiple nodes into a single, reusable component. This simplifies complex graphs and enables logic reuse.

## Creating a Composite
1.  **Select Nodes**: Shift-click or drag-select a group of connected nodes on the canvas.
2.  **Create**: Click the "Create Composite" button (or use the context menu).
3.  **Configure**:
    *   **Name**: Give your composite a descriptive name.
    *   **Category**: Choose where it appears in the Add Node menu.
    *   **Exposed I/O**: Select which internal inputs and outputs should be accessible from the outside.
4.  **Save**: The new composite is saved to the library.

## Using Composites
Once created, your composite appears in the **Add Node** menu under the designated category (default: "Custom"). You can add it to the canvas like any standard node.

## Editing Composites
- **Properties**: Click a composite node to view its inputs and outputs.
- **Internal Graph**: Currently, composites are edited by re-creating them or managing them via the **Composite Manager**.

## Composite Manager
Access the Composite Manager from the top bar to:
-   **View** all saved composites.
-   **Delete** old composites.
-   **Export**: Download a composite definition as a JSON file.
-   **Import**: Upload a composite JSON file to your library.
