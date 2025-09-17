<!-- default badges list -->
![](https://img.shields.io/endpoint?url=https://codecentral.devexpress.com/api/v1/VersionRange/128585309/24.2.1%2B)
[![](https://img.shields.io/badge/Open_in_DevExpress_Support_Center-FF7200?style=flat-square&logo=DevExpress&logoColor=white)](https://supportcenter.devexpress.com/ticket/details/T394199)
[![](https://img.shields.io/badge/📖_How_to_use_DevExpress_Examples-e9f6fc?style=flat-square)](https://docs.devexpress.com/GeneralInformation/403183)
[![](https://img.shields.io/badge/💬_Leave_Feedback-feecdd?style=flat-square)](#does-this-example-address-your-development-requirementsobjectives)

# WPF Diagram Control - Microsoft Automatic Graph Layout (MSAGL) Algorithms

This example connects the [Microsoft Automatic Graph Layout (MSAGL)](https://github.com/Microsoft/automatic-graph-layout) library to [`DiagramControl`](https://docs.devexpress.com/WPF/DevExpress.Xpf.Diagram.DiagramControl). This technic allows you apply advanced algorithms such as  **Sugiyama**, **Ranking**, **PhyloTree**, **MDS**, or **Disconnected Graphs** with one click.

![Diagram Control - Microsoft Automatic Graph Layout (MSAGL) Algorithms](./Images/diagram-layouts.jpg)

## Implementation Details

### Load Sample Graph

The application loads a diagram from an XML file before applying a layout. This example includes five datasets: **Sugiyama**, **Ranking**, **PhyloTree**, **MDS**, and **Disconnected Graphs**.

```csharp
void LoadSugiyama(object sender, DevExpress.Xpf.Bars.ItemClickEventArgs e) {
    diagramControl.LoadDocument("Data/SugiyamaLayout.xml");
}
void LoadMDS(object sender, DevExpress.Xpf.Bars.ItemClickEventArgs e) {
    diagramControl.LoadDocument("Data/MDSLayout.xml");
}
// … similar for Ranking, PhyloTree, DisconnectedGraphs
```

### Extract and Arrange Nodes

The `GraphOperations.GetDiagramGraph` extracts nodes and edges from the diagram. The selected MSAGL calculator computes positions, which are then applied to the diagram:

```csharp
void ApplyLayout(GraphLayout layout) {
    try {
        diagramControl.RelayoutDiagramItems(
            layout.RelayoutGraphNodesPosition(GraphOperations.GetDiagramGraph(diagramControl))
        );
        diagramControl.Items.OfType<IDiagramConnector>().ForEach(connector => { 
            connector.Type = layout.GetDiagramConnectorType(); 
            connector.UpdateRoute(); 
        });
        diagramControl.FitToDrawing();
    } catch(Exception e) {
        DXMessageBox.Show(string.Format("Error message: '{0}'", e.Message), "Error has been occurred");
    }
}
```

### Update Connectors

After shapes are repositioned, connectors update routs. The code example sets connector types and updates their routes:

```csharp
diagramControl.Items.OfType<IDiagramConnector>().ForEach(connector => {
    connector.Type = layout.GetDiagramConnectorType();
    connector.UpdateRoute();
});
```

The controller also registers a routing strategy:

```csharp
diagramControl.Controller.RegisterRoutingStrategy(
    layout.GetDiagramConnectorType(), 
    layout.GetDiagramRoutingStrategy()
);
```

### Display Entire Diagram

The `DiagramControl` adjusts its viewport to display the entire diagram:

```csharp
diagramControl.FitToDrawing();
```

### Ribbon commands

Ribbon buttons load documents and apply the corresponding algorithm:

```csharp
void ApplySugiyama(object s, ItemClickEventArgs e) {
    ApplyLayout(new GraphLayout(new SugiyamaLayoutCalculator()));
}
void ApplyRanking(object s, ItemClickEventArgs e) {
    ApplyLayout(new GraphLayout(new RankingLayoutCalculator()));
}
void ApplyPhyloTree(object s, ItemClickEventArgs e) {
    ApplyLayout(new PhyloTreeLayout(new PhyloTreeLayoutCalculator()));
}
void ApplyMDS(object s, ItemClickEventArgs e) {
    ApplyLayout(new GraphLayout(new MDSLayoutCalculator()));
}
void ApplyDisconnectedGraphs(object s, ItemClickEventArgs e) {
    ApplyLayout(new GraphLayout(new DisconnectedGraphsLayoutCalculator()));
}
```

## Files to Review

* [LayoutExampleWindow.xaml](./CS/DXDiagram.CustomLayoutAlgorithms/LayoutExampleWindow.xaml) (VB: [LayoutExampleWindow.xaml](./VB/DXDiagram.CustomLayoutAlgorithms/LayoutExampleWindow.xaml))
* [LayoutExampleWindow.xaml.cs](./CS/DXDiagram.CustomLayoutAlgorithms/LayoutExampleWindow.xaml.cs) (VB: [LayoutExampleWindow.xaml.vb](./VB/DXDiagram.CustomLayoutAlgorithms/LayoutExampleWindow.xaml.vb))
* [MainWindow.xaml](./CS/DXDiagram.CustomLayoutAlgorithms/MainWindow.xaml) (VB: [MainWindow.xaml](./VB/DXDiagram.CustomLayoutAlgorithms/MainWindow.xaml))
* [MainWindow.xaml.cs](./CS/DXDiagram.CustomLayoutAlgorithms/MainWindow.xaml.cs) (VB: [MainWindow.xaml.vb](./VB/DXDiagram.CustomLayoutAlgorithms/MainWindow.xaml.vb))
* [Converter.cs](./CS/MsaglHelpers/Converter.cs) (VB: [Converter.vb](./VB/MsaglHelpers/Converter.vb))
* [GraphLayout.cs](./CS/MsaglHelpers/Layout/GraphLayout.cs) (VB: [GraphLayout.vb](./VB/MsaglHelpers/Layout/GraphLayout.vb))
* [PhyloTreeLayout.cs](./CS/MsaglHelpers/Layout/PhyloTreeLayout.cs) (VB: [PhyloTreeLayout.vb](./VB/MsaglHelpers/Layout/PhyloTreeLayout.vb))
* [DisconnectedGraphsLayoutCalculator.cs](./CS/MsaglHelpers/LayoutCalculators/DisconnectedGraphsLayoutCalculator.cs) (VB: [DisconnectedGraphsLayoutCalculator.vb](./VB/MsaglHelpers/LayoutCalculators/DisconnectedGraphsLayoutCalculator.vb))
* [ILayoutCalculator.cs](./CS/MsaglHelpers/LayoutCalculators/ILayoutCalculator.cs) (VB: [ILayoutCalculator.vb](./VB/MsaglHelpers/LayoutCalculators/ILayoutCalculator.vb))
* [MDSLayoutCalculator.cs](./CS/MsaglHelpers/LayoutCalculators/MDSLayoutCalculator.cs) (VB: [MDSLayoutCalculator.vb](./VB/MsaglHelpers/LayoutCalculators/MDSLayoutCalculator.vb))
* [PhyloTreeLayoutCalculator.cs](./CS/MsaglHelpers/LayoutCalculators/PhyloTreeLayoutCalculator.cs) (VB: [PhyloTreeLayoutCalculator.vb](./VB/MsaglHelpers/LayoutCalculators/PhyloTreeLayoutCalculator.vb))
* [RankingLayoutCalculator.cs](./CS/MsaglHelpers/LayoutCalculators/RankingLayoutCalculator.cs) (VB: [RankingLayoutCalculator.vb](./VB/MsaglHelpers/LayoutCalculators/RankingLayoutCalculator.vb))
* [SugiyamaLayoutCalculator.cs](./CS/MsaglHelpers/LayoutCalculators/SugiyamaLayoutCalculator.cs) (VB: [SugiyamaLayoutCalculator.vb](./VB/MsaglHelpers/LayoutCalculators/SugiyamaLayoutCalculator.vb))
* [MsaglGeometryGraphHelpers.cs](./CS/MsaglHelpers/MsaglGeometryGraphHelpers.cs) (VB: [MsaglGeometryGraphHelpers.vb](./VB/MsaglHelpers/MsaglGeometryGraphHelpers.vb))
* [RoutingHelper.cs](./CS/MsaglHelpers/RoutingHelper.cs) (VB: [RoutingHelper.vb](./VB/MsaglHelpers/RoutingHelper.vb))

## Documentation

* [DiagramControl](https://docs.devexpress.com/WPF/DevExpress.Xpf.Diagram.DiagramControl)
* [Diagram Items](https://docs.devexpress.com/WPF/116675/controls-and-libraries/diagram-control/diagram-items)
* [DiagramDesignerControl](https://docs.devexpress.com/WPF/DevExpress.Xpf.Diagram.DiagramDesignerControl)
* [RibbonControl](https://docs.devexpress.com/WPF/DevExpress.Xpf.Ribbon.RibbonControl)
* [RibbonPageCategory](https://docs.devexpress.com/WPF/DevExpress.Xpf.Ribbon.RibbonPageCategory)
* [RibbonPage](https://docs.devexpress.com/WPF/DevExpress.Xpf.Ribbon.RibbonPage)

## More Examples

* [WPF DiagramControl - Create Custom Shapes with Connection Points](https://github.com/DevExpress-Examples/wpf-diagramdesigner-create-custom-shapes-with-connection-points)
* [WPF DiagramControl - Create Custom Context Menus](https://github.com/DevExpress-Examples/wpf-diagram-custom-context-menu)
* [WPF Diagram Control - Track and Restrict Drag Actions](https://github.com/DevExpress-Examples/wpf-diagram-track-and-restrict-drag-actions)

<!-- feedback -->
## Does this example address your development requirements/objectives?

[<img src="https://www.devexpress.com/support/examples/i/yes-button.svg"/>](https://www.devexpress.com/support/examples/survey.xml?utm_source=github&utm_campaign=wpf-diagram-ms-automatic-graph-layout-algorithms&~~~was_helpful=yes) [<img src="https://www.devexpress.com/support/examples/i/no-button.svg"/>](https://www.devexpress.com/support/examples/survey.xml?utm_source=github&utm_campaign=wpf-diagram-ms-automatic-graph-layout-algorithms&~~~was_helpful=no)

(you will be redirected to DevExpress.com to submit your response)
<!-- feedback end -->
