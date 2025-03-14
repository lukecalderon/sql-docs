---
title: "Export a paginated report to an image file (Report Builder)"
description: In Report Builder, the Image rendering extension renders a paginated report to a bitmap or metafile. The default is a TIFF file viewable in multiple pages.
author: maggiesMSFT
ms.author: maggies
ms.date: 06/07/2023
ms.service: reporting-services
ms.subservice: report-builder
ms.topic: conceptual
ms.custom: updatefrequency5
---
# Export a paginated report to an image file (Report Builder)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-ssrs-rb](../../includes/ssrs-appliesto-ssrs-rb.md)] [!INCLUDE [ssrs-appliesto-pbi-rb](../../includes/ssrs-appliesto-pbi-rb.md)] [!INCLUDE [ssrb-applies-to-ssdt-yes](../../includes/ssrb-applies-to-ssdt-yes.md)]

  The Image rendering extension renders a paginated report to a bitmap or metafile. By default, the Image rendering extension produces a TIFF file of the report, which can be viewed in multiple pages. When the client receives the image, it can be displayed in an image viewer and printed. This article provides Image renderer-specific information and describes exceptions to the rendering rules.

The Image rendering extension can generate files in any of the formats supported by GDI+: BMP, EMF, EMFPlus, GIF, JPEG, PNG, and TIFF. For TIFF format, the file name of the primary stream is `ReportName.tif`. For all other formats, which render as a single page per file, the file name is `ReportName_Page.ext` where `.ext` is the file extension for the chosen format. To produce a file in another Image-supported format, specify any of the above listed strings in the **OutputFormatDeviceInfo** setting.

> [!NOTE]  
> [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]

## <a id="SupportedImageFormats"></a> Supported image formats

The following table shows the file extension and MimeType for each Image renderer format.

| **Type** | **Extension** | **MIMEType** |
| --- | --- | --- |
| BMP | bmp | image/bmp |
| GIF | gif | image/gif |
| JPEG | jpeg | image/jpeg |
| PNG | png | image/png |
| TIFF | tif | image/tiff |
| EMF | emf | image/emf |
| EMFPlus | emf | image/emf |

## <a id="RenderingMultiplePages"></a> Render multiple pages

TIFF is the only format that supports multiple page documents in a single file. Other formats, such as JPG or PNG, output one page at a time and require a separate call to the rendering extension for each page.

## <a id="Interactivity"></a> Interactivity

Interactivity isn't supported in any Image formats generated by this renderer. The following interactive elements aren't rendered:

- Hyperlinks

- Show or Hide

- Document Map

- Drillthrough or clickthrough links

- End user sort

- Fixed headers

- Bookmarks

## <a id="DeviceInfo"></a> Device information settings

You can change some default settings for this renderer by changing the device information settings. For more information, see [Image device information settings](../../reporting-services/image-device-information-settings.md).

## Related content

- [Pagination in Reporting Services (Report Builder  and SSRS)](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)
- [Renderer behaviors (Report Builder  and SSRS)](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)
- [Interactive functionality for different report rendering extensions (Report Builder and SSRS)](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)
- [Render report items (Report Builder and SSRS)](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)
- [Tables, matrices, and lists (Report Builder and SSRS)](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)
