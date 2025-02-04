---
-api-id: P:Microsoft.UI.Xaml.Controls.MediaPlayerElement.PosterSource
-api-type: winrt property
---

# Microsoft.UI.Xaml.Controls.MediaPlayerElement.PosterSource

<!--
public Microsoft.UI.Xaml.Media.ImageSource PosterSource { get; set; }
-->


## -description
Gets or sets the image source that is used for a placeholder image during [MediaPlayerElement](mediaplayerelement.md) loading transition states.


## -xaml-syntax
```xaml
<MediaPlayerElement PosterSource="imageUri" />
```


## -property-value
An image source for a transition [ImageBrush](../windows.ui.xaml.media/imagebrush.md) that is applied to the [MediaPlayerElement](mediaplayerelement.md) content area.

## -remarks
A PosterSource is an image, such as a album cover or movie poster, that is displayed in place of video. It provides your [MediaPlayerElement](mediaplayerelement.md) with a visual representation before the media is loaded, or when the media is audio only.


The PosterSource is displayed in the following situations:

+ When a valid source is not set. For example, [Source](mediaplayerelement_source.md) is not set, [Source](mediaplayerelement_source.md) is set to **Null**, or the source is invalid (as is the case when a [MediaFailed](/uwp/api/windows.media.playback.mediaplayer.mediafailed) event fires).
+ While media is loading. For example, a valid source is set, but the [MediaOpened](/uwp/api/windows.media.playback.mediaplayer.mediaopened) event has not fired yet.
+ While media is streaming to another device.
+ When the media is audio only.


## -examples

## -see-also
