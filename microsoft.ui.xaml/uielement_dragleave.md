---
-api-id: E:Microsoft.UI.Xaml.UIElement.DragLeave
-api-type: winrt event
---

<!-- Event syntax
public event Microsoft.UI.Xaml.DragEventHandler DragLeave
-->

# Microsoft.UI.Xaml.UIElement.DragLeave

## -description

Occurs when the input system reports an underlying drag event with this element as the origin.

## -xaml-syntax

```xaml
<uiElement DragLeave="eventhandler"/>
```

## -remarks

For `DragLeave` to occur, the value of [AllowDrop](uielement_allowdrop.md) on the current [UIElement](uielement.md) and on the event source must be `true`. Otherwise, consider using [PointerExited](uielement_pointerexited.md).

You can initiate a drag-drop action on any [UIElement](uielement.md) by calling the [StartDragAsync](uielement_startdragasync_369751260.md) method. Once the action is initiated, any `UIElement` in the app can potentially be a drop target so long as [AllowDrop](uielement_allowdrop.md) is `true` on that element. Any elements that the drag-drop action passes over can handle [DragEnter](uielement_dragenter.md), DragLeave or [DragOver](uielement_dragover.md).

`DragLeave` is a routed event. For more info on the routed event concept, see [Events and routed events overview](/windows/uwp/xaml-platform/events-and-routed-events-overview).

For touch actions, drag-drop actions, and also for interaction-specific or manipulation events that are consequences of a touch action, an element must be hit-test visible in order to be the event source and fire the event that is associated with the action. [UIElement.Visibility](uielement_visibility.md) must be [Visible](visibility.md). Other properties of derived types also affect hit-test visibility, for example [IsEnabled](../microsoft.ui.xaml.controls/control_isenabled.md). For more info, see [Events and routed events overview](/windows/uwp/xaml-platform/events-and-routed-events-overview).

`DragLeave` supports the ability to attach event handlers to the route that will be invoked even if the event data for the event is marked [Handled](drageventargs_handled.md). See [AddHandler](uielement_addhandler_1350394113.md).

Specific Windows Runtime controls may have class-based handling for the `DragLeave` event. If so, the control probably has an override for the method [OnDragLeave](../microsoft.ui.xaml.controls/control_ondragleave_1870191881.md). Typically the event is marked handled by the class handler, and the `DragLeave` event is not raised for handling by any user code handlers on that control. For more info, see [Events and routed events overview](/windows/uwp/xaml-platform/events-and-routed-events-overview).

Independent of the event occurrence, some controls may use theme animations such as [DragItemThemeAnimation](../microsoft.ui.xaml.media.animation/dragitemthemeanimation.md) to visually indicate a drag behavior to the user.

## -examples

## -see-also

[DragEventArgs](drageventargs.md), [DragEnter](uielement_dragenter.md), [AllowDrop](uielement_allowdrop.md), [Events and routed events overview](/windows/uwp/xaml-platform/events-and-routed-events-overview), [Drag and drop sample (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop)
