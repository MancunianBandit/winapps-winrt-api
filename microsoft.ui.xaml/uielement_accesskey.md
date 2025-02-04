---
-api-id: P:Microsoft.UI.Xaml.UIElement.AccessKey
-api-type: winrt property
---

<!-- Property syntax
public string AccessKey { get;  set; }
-->

# Microsoft.UI.Xaml.UIElement.AccessKey

## -description

Gets or sets the access key (mnemonic) for this element.

## -property-value

The access key (mnemonic) for this element.

## -remarks

Setting this property enables the [AccessKeyDisplayRequested](uielement_accesskeydisplayrequested.md) event to be raised.

If the [AutomationProperties.AccessKey](/windows/windows-app-sdk/api/winrt/microsoft.ui.xaml.automation.automationproperties#xaml-attached-properties) attached property is not set, this property is used by the Automation framework instead. The value is used as case-insensitive, using the user language. It is used as text, so if an Input Method Editor (IME) is active the composed text is used.

## -examples

## -see-also

[Access keys](/windows/apps/design/input/access-keys)
