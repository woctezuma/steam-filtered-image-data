# Steam Filtered Image Data

This repository provides details about image data:
-   downloaded from Steam,
-   **filtered** following parts of the procedure in [`download-steam-banners-data`][download-steam-banners-data].

## Data

Data downloaded from Steam consists of:
-   PNG logos with transparency,
-   JPG vertical banners.

Here is an example of a logo:

![Example of a logo][logo-example]

Here is an example of a vertical banner:

![Example of a vertical Banner][vertical-banner-example]

The *up-to-date* list of appIDs for which I have *tried* to download image data is available in:
> `data/download-queries/app_ids.txt`.
Most of these ~48k appIDs do not feature any image data.

Filtered image data is shared [on Google Drive][filtered-data-on-gdrive].

## Filtering

The filtering consist in the removal of:
-   blank images, i.e. images for which grayscale image intensity extrema are equal,
-   images of uncommon resolution:
    - anything but 640x360 for logos,
    - anything but 300x450 for vertical banners,
-   images with uncommon bands:
    - anything but RGBA for logos,
    - anything but RGB for banners,
-   images for which the bounding box of the non-zero regions covers:
    - 100% of the image space for transparent logos,
    - strictly less than 100% image space for banners, which happens with [vignetting][vignetting-wiki]).

## Filtering suggestions

Suggestions of filtering include:
-   removal of duplicate images with [`imagededup`][imagededup],
-   filtering based on the number of detected faces, as in [`steam-face-detection`][steam-face-detection].

The enforcement of such filtering is left to the reader.
Otherwise, it would be difficult to keep **filtered** data up-to-date.

<!-- Definitions -->

[download-steam-banners-data]: <https://github.com/woctezuma/download-steam-banners-data>

[logo-example]: <https://cdn.cloudflare.steamstatic.com/steam/apps/546560/logo.png>
[vertical-banner-example]: <https://cdn.cloudflare.steamstatic.com/steam/apps/546560/library_600x900.jpg>

[filtered-data-on-gdrive]: <https://drive.google.com/drive/folders/1SHb7u_mZZ0fDy2lDQ7d94E79os_OYH2z>

[vignetting-wiki]: <https://en.wikipedia.org/wiki/Vignetting>

[imagededup]: <https://idealo.github.io/imagededup/>
[steam-face-detection]: <https://github.com/woctezuma/steam-face-detection>

[colab-badge]: <https://colab.research.google.com/assets/colab-badge.svg>
