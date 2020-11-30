# Steam Filtered Image Data

This repository provides details about image data:
-   downloaded from Steam,
-   **filtered** following parts of the procedure in [`download-steam-banners-data`][download-steam-banners-data].

A dataset, called Steam-OneFace, is shared [in this section][steam-oneface-section].

## Data

Data downloaded from Steam consists of:
-   PNG logos with transparency,
-   JPG vertical banners.

Filtered image data is shared [on Google Drive][filtered-data-on-gdrive].

The *up-to-date* list of appIDs for which I have *tried* to download image data is available in:
> `data/download-queries/app_ids.txt`. Most of these ~48k appIDs do not feature any image data.

Here is an example of a vertical banner:

![Example of a vertical Banner][vertical-banner-example]

Here is an example of a logo:

![Example of a logo][logo-example]

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
    - strictly less than 100% image space for banners, which happens with [vignetting][vignetting-wiki].

## Suggestions of filtering

Suggestions of filtering include:
-   removal of duplicate images with [`imagededup`][imagededup],
-   filtering based on the number of detected faces, as in [`steam-face-detection`][steam-face-detection].

The enforcement of such filtering is left to the reader.
Otherwise, it would be difficult to keep **filtered** data up-to-date.

## Steam-OneFace dataset

The notebook [`build_steam_oneface_dataset.ipynb`][steam-oneface-notebook] shows an application of the filters suggested above.
[![Open In Colab][colab-badge]][steam-oneface-notebook]

This allows to build a dataset, called `Steam-OneFace`, of 1688 images which should all feature exactly one face.

This dataset is shared [on Google Drive][steam-oneface-gdrive] in:
-   the original resolution (300x450): [`steam-oneface-hr.tar.gz`][steam-oneface-hr] (94 MB)
-   a lower resolution (256x256): [`steam-oneface-lr.tar.gz`][steam-oneface-lr] (52 MB)

[![Thumbnails of Steam OneFace][steam-oneface-cover-small]][steam-oneface-cover-big]

To use this dataset on Google Colab, run the following:
```bash
!gdown --id 1-AdMe7AKyhtkulcfvazxlb0B52iCVDt2
!tar xf steam-oneface-lr.tar.gz
```
```python
import glob
from pathlib import Path

file_names = glob.glob('steam-oneface-lr/*.jpg')
app_ids = [int(Path(fname).stem) for fname in file_names]
```

<!-- Definitions -->

[download-steam-banners-data]: <https://github.com/woctezuma/download-steam-banners-data>
[steam-oneface-section]: <https://github.com/woctezuma/steam-filtered-image-data#steam-oneface-dataset>

[logo-example]: <https://cdn.cloudflare.steamstatic.com/steam/apps/546560/logo.png>
[vertical-banner-example]: <https://cdn.cloudflare.steamstatic.com/steam/apps/546560/library_600x900.jpg>

[filtered-data-on-gdrive]: <https://drive.google.com/drive/folders/1SHb7u_mZZ0fDy2lDQ7d94E79os_OYH2z>

[vignetting-wiki]: <https://en.wikipedia.org/wiki/Vignetting>

[imagededup]: <https://idealo.github.io/imagededup/>
[steam-face-detection]: <https://github.com/woctezuma/steam-face-detection>

[steam-oneface-notebook]: <https://colab.research.google.com/github/woctezuma/steam-filtered-image-data/blob/main/build_steam_oneface_dataset.ipynb>
[steam-oneface-gdrive]: <https://drive.google.com/drive/folders/1MlpNk6PwYZWhJegMjuukcYCNFSnXR3wg>
[steam-oneface-hr]: <https://drive.google.com/file/d/1dmm1W8kPINVQrG8NbxXw_KEgU2Nkeksu>
[steam-oneface-lr]: <https://drive.google.com/file/d/1QptHrW9vloTtP--YJsxMY8PZWI2D8NJt>
[steam-oneface-cover-small]: <https://raw.githubusercontent.com/wiki/woctezuma/steam-filtered-image-data/img/oneface-cover-small.jpg>
[steam-oneface-cover-big]: <https://raw.githubusercontent.com/wiki/woctezuma/steam-filtered-image-data/img/oneface-cover.jpg>

[colab-badge]: <https://colab.research.google.com/assets/colab-badge.svg>
