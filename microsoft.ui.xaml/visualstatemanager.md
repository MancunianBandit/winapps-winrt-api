---
-api-id: T:Microsoft.UI.Xaml.VisualStateManager
-api-type: winrt class
---

<!-- Class syntax.
public class VisualStateManager : Microsoft.UI.Xaml.DependencyObject, Microsoft.UI.Xaml.IVisualStateManager, Microsoft.UI.Xaml.IVisualStateManagerOverrides, Microsoft.UI.Xaml.IVisualStateManagerProtected
-->

# Microsoft.UI.Xaml.VisualStateManager

## -description

Manages visual states and the logic for transitions between visual states for controls. Also provides the attached property support for `VisualStateManager.VisualStateGroups`, which is how you define visual states in XAML for a control template.

## -remarks

`VisualStateManager` supports two important features for control authors, and for app developers who are applying a custom template to a control:

+ Control authors or app developers add [VisualStateGroup](visualstategroup.md) object elements to the root element of a control template definition in XAML, using the `VisualStateManager.VisualStateGroups` attached property. Within a `VisualStateGroup` element, each [VisualState](visualstate.md) represents a discrete visual state of a control. Each `VisualState` has a name that is representative of a UI state that can be changed by the user, or changed by control logic. A `VisualState` consists mainly of a [Storyboard](visualstate_storyboard.md). This `Storyboard` targets individual dependency property value changes that should be applied whenever the control is in that visual state.
+ Control authors or app developers transition between these states by calling the static [GoToState](visualstatemanager_gotostate_51722231.md) method of `VisualStateManager`. Control authors do this whenever the control logic handles events that indicate a change of state, or control logic initiates a state change by itself. It's more common for control definition code to do this rather than app code, so that all the possible visual states and their transitions and trigger conditions are there by default for app code, and the logic is encapsulated by the control.

Most developers will use just two of the `VisualStateManager` APIs: `VisualStateManager.VisualStateGroups`, and [GoToState](visualstatemanager_gotostate_51722231.md), as described above. The remaining APIs are all for extension support and creating a custom `VisualStateManager`. For more info see the "Custom VisualStateManager" section in this topic.

When you edit copies of styles as enabled by the XAML design surface of Microsoft Visual Studio, the visual states from the default template are defined in the XAML you are editing. Make sure you don't delete these states or change their names, because the control logic is expecting that these visual states exist in the template.

In addition to the visual states, the visual state model also includes transitions. Transitions are animation actions controlled by a [Storyboard](visualtransition_storyboard.md) that occur between each visual state when the state is changed. The transition can be defined differently for each combination of starting state and ending state as defined by your control's set of visual states. Transitions are defined by the [Transitions](visualstategroup_transitions.md) property of `VisualStateGroup`, in XAML using property element syntax. Most default control templates don't define transitions. In absence of specifically defined transitions, the transitions between states happen instantaneously (zero-duration). For more info, see [VisualTransition](visualtransition.md).

### Custom VisualStateManager

If you want to implement your own logic for transitions between states (an advanced scenario), you can create a class that inherits from `VisualStateManager`. Follow these guidelines:

+ The derived class should override the protected [GoToStateCore](visualstatemanager_gotostatecore_2010136904.md) method. Any instance of the custom `VisualStateManager` uses this **Core** logic when its [GoToState](visualstatemanager_gotostate_51722231.md) method is called.
+ To reference your custom `VisualStateManager` class, set the value of the `VisualStateManager.CustomVisualStateManager` attached property on the root element of a [ControlTemplate](../microsoft.ui.xaml.controls/controltemplate.md) where you want to use the custom `VisualStateManager` class behavior, alongside the [VisualStateManager.VisualStateGroups](visualstatemanager_visualstategroups.md) attached property usage that defines the visual states for the template. You typically create an instance of the custom `VisualStateManager` class through default XAML construction in [Application.Resources](application_resources.md). Then the `VisualStateManager.CustomVisualStateManager` attached property is set using a [{StaticResource} markup extension](/windows/uwp/xaml-platform/staticresource-markup-extension) reference to the key of the custom `VisualStateManager` resource.

That's the basic requirements for creating and using a custom `VisualStateManager`. You also can choose to override a few more behaviors:

+ Override [RaiseCurrentStateChanged](visualstatemanager_raisecurrentstatechanged_230937552.md) to control when the [CurrentStateChanged](visualstategroup_currentstatechanged.md) event is fired by a [VisualStateGroup](visualstategroup.md) being managed by the `VisualStateManager`.
+ Override [RaiseCurrentStateChanging](visualstatemanager_raisecurrentstatechanging_901595439.md) to control when the [CurrentStateChanging](visualstategroup_currentstatechanging.md) event is fired by a [VisualStateGroup](visualstategroup.md) being managed by the `VisualStateManager`.
+ Override or overload the constructor if your custom class needs additional information to initialize with.

All the other API ([CustomVisualStateManagerProperty](visualstatemanager_customvisualstatemanagerproperty.md), [GetCustomVisualStateManager](visualstatemanager_getcustomvisualstatemanager_1591248242.md), [GetVisualStateGroups](visualstatemanager_getvisualstategroups_1953173347.md), [SetCustomVisualStateManager](visualstatemanager_setcustomvisualstatemanager_896513060.md)) are infrastructure for attached property support, and you don't need to call them or do anything with them.

### Visual states for elements that aren't controls

Visual states are sometimes useful for scenarios where you want to change the state of some area of UI that's not immediately a [Control](../microsoft.ui.xaml.controls/control.md) subclass. You can't do this directly because the *control* parameter of the [GoToState](visualstatemanager_gotostate_51722231.md) method requires a `Control` subclass, which refers to the object that the VisualStateManager acts upon. [Page](../microsoft.ui.xaml.controls/page.md) is a `Control` subclass, and it's fairly rare that you'd be showing UI in a context where you don't have a `Page`, or your [Window.Content](window_content.md) root isn't a `Control` subclass. We recommend you define a custom [UserControl](../microsoft.ui.xaml.controls/usercontrol.md) to either be the `Window.Content` root or be a container for other content you want to apply states to (such as a [Panel](../microsoft.ui.xaml.controls/panel.md)). Then you can call [GoToState](visualstatemanager_gotostate_51722231.md) on your `UserControl` and apply states regardless of whether the rest of the content is a `Control`. For example you could apply visual states to UI that otherwise consists of just a [SwapChainPanel](../microsoft.ui.xaml.controls/swapchainpanel.md) so long as you placed that within your `UserControl` and declared named states that apply to the properties of the parent `UserControl` or of the named `SwapChainPanel` part of the template.

### XAML attached properties

`VisualStateManager` is the host service class for several [XAML attached properties](/windows/uwp/xaml-platform/attached-properties-overview).

In order to support XAML processor access to the attached properties, and also to expose equivalent _get_ and _set_ operations to code, each XAML attached property has a pair of `Get` and `Set` accessor methods. Another way to get or set the value in code is to use the dependency property system, calling either [GetValue](dependencyobject_getvalue_229640130.md) or [SetValue](dependencyobject_setvalue_1212521140.md) and passing the identifier field as the dependency property identifier.

<table>
  <tr><th> Attached Property</th><th>Description</th></tr>
  <tr><td>VisualStateGroups</td>
  <td>Gets the collection of VisualStateGroup elements that are defined by a root element of a template definition. A control typically defines this as part of its template.
    <ul><li>Type: <a href="/windows/windows-app-sdk/api/winrt/microsoft.ui.xaml.visualstategroup">VisualStateGroup</a></li>
    <li>Identifier field: N/A</li>
    <li>Accessor methods: <a href="/windows/windows-app-sdk/api/winrt/microsoft.ui.xaml.visualstatemanager.getvisualstategroups">GetVisualStateGroups</a></li></ul>
    <p>When getting this property in code, use GetVisualStateGroups. This returns a collection object, which you can add items to. This parallels the XAML processing behavior of any child elements of a VisualStateManager.VisualStateGroups property element usage.</p>
    <p>Because there is no public dependency property identifier for this particular attached property, you cannot use GetValue to get this attached property value, you always have to use GetVisualStateGroups.</p>
  </td></tr>
  <tr><td>CustomVisualStateManager</td>
  <td>Gets or sets the custom VisualStateManager object that handles transitions between the states of a control.
    <ul><li>Type: VisualStateManager</li>
    <li>Identifier field: <a href="/windows/windows-app-sdk/api/winrt/microsoft.ui.xaml.visualstatemanager.customvisualstatemanagerproperty">CustomVisualStateManagerProperty</a></li>
    <li>Accessor methods: <a href="/windows/windows-app-sdk/api/winrt/microsoft.ui.xaml.visualstatemanager.getcustomvisualstatemanager">GetCustomVisualStateManager</a>, <a href="/windows/windows-app-sdk/api/winrt/microsoft.ui.xaml.visualstatemanager.setcustomvisualstatemanager">SetCustomVisualStateManager</a></li></ul>
    <p>This attached property is only needed for cases where you want to use a custom implementation class to handle your app's visual state changes, rather than the default VisualStateManager class implemented by the Windows Runtime. If you don't intend to use a custom implementation, you don't need to set this property.</p>
  </td></tr>
</table>

## -examples

This example shows how to use the `VisualStateManager.VisualStateGroups` XAML attached property. Note how there is otherwise no "VisualStateManager" tag defined. Conceptually, `VisualStateManager.VisualStateGroups` contains the visual states for a control, as an immediate child tag of the template root in a control template.

The particular set of visual states contains one [VisualStateGroup](visualstategroup.md), called "CommonStates", which defines the "PointerOver" and "Normal" [VisualState](visualstate.md) objects. When the user puts the pointer over the [Button](../microsoft.ui.xaml.controls/button.md), the [Grid](../microsoft.ui.xaml.controls/grid.md) changes from green to red in .5 seconds. When the user moves the pointer away from the button, the [Grid](../microsoft.ui.xaml.controls/grid.md) immediately changes back to green.

[!code-xaml[11](../microsoft.ui.xaml.data/code/StylingTemplatingOverview/csharp/ButtonStages.xaml#Snippet11)]

```xaml
<common:LayoutAwarePage>
  <Grid>
...
    <VisualStateManager.VisualStateGroups>
    <!-- Visual states reflect the application's window size -->
      <VisualStateGroup>
        <VisualState x:Name="DefaultLayout">
           <Storyboard>
           </Storyboard>
        </VisualState>
        <VisualState x:Name="Below768Layout">
           <Storyboard>
             <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="(FrameworkElement.Margin)"
               Storyboard.TargetName="ContentRoot">
               <DiscreteObjectKeyFrame KeyTime="0">
                 <DiscreteObjectKeyFrame.Value>
                   <Thickness>20,20,20,20</Thickness>
                 </DiscreteObjectKeyFrame.Value>
               </DiscreteObjectKeyFrame>
             </ObjectAnimationUsingKeyFrames>
             <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="(FrameworkElement.HorizontalAlignment)"
               Storyboard.TargetName="FooterPanel">
               <DiscreteObjectKeyFrame KeyTime="0">
                 <DiscreteObjectKeyFrame.Value>
                    <HorizontalAlignment>Left</HorizontalAlignment>
                 </DiscreteObjectKeyFrame.Value>
               </DiscreteObjectKeyFrame>
             </ObjectAnimationUsingKeyFrames>
           </Storyboard>
         </VisualState>
       </VisualStateGroup>
     </VisualStateManager.VisualStateGroups>
   </Grid>
</common:LayoutAwarePage>
```

The next snippet is code to go along with the XAML, showing how an app might detect the app window width and use that info to call the appropriate visual state.

```csharp
String state = (Window.Current.Bounds.Width > 768) ? "DefaultLayout" : "Below768Layout";
VisualStateManager.GoToState(this, state, false); // 'this' is the LayoutAwarePage, scope is page code-behind
```

## -see-also

[DependencyObject](dependencyobject.md), [VisualStateGroup](visualstategroup.md), [VisualState](visualstate.md), [Storyboard](../microsoft.ui.xaml.media.animation/storyboard.md), [XAML Control templates](/windows/apps/design/style/xaml-control-templates), [Storyboarded animations](/windows/apps/design/motion/storyboarded-animations), [Attached properties overview](/windows/uwp/xaml-platform/attached-properties-overview),  [XAML control and app styling sample](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208.1%20Store%20app%20samples/99866-Windows%208.1%20Store%20app%20samples/XAML%20control%20and%20app%20styling%20sample/C%23)
