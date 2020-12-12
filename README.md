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

The filtering consists in the removal of:
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

There are three options for the face detector:
-   [`dlib`][dlib-github]
-   [`face-alignment`][python-face-alignment]
-   [`retinaface_pytorch`][retinaface]

### With the `face_alignment` module

This allows to build a dataset, called `Steam-OneFace`, of 1688 images which should all feature exactly one face.

This dataset is shared [on Google Drive][steam-oneface-gdrive] in:
-   the original resolution (300x450): [`steam-oneface-hr.tar.gz`][steam-oneface-hr] (94 MB)
-   a lower resolution (256x256): [`steam-oneface-lr.tar.gz`][steam-oneface-lr] (52 MB)

[![Thumbnails of Steam OneFace][steam-oneface-cover-small]][steam-oneface-cover-big]

To use this dataset on Google Colab, run the following:
```bash
!gdown --id 1QptHrW9vloTtP--YJsxMY8PZWI2D8NJt
!tar xf steam-oneface-lr.tar.gz
```
```python
import glob
from pathlib import Path

file_names = glob.glob('steam-oneface-lr/*.jpg')
app_ids = [int(Path(fname).stem) for fname in file_names]
```

### With the `retinaface` module

The dataset consists of 2472 images, shared in:
-   the original resolution (300x450): [`steam-oneface-hr_with_retinaface.tar.gz`][steam-oneface-hr-retinaface] (133 MB)
-   a lower resolution (256x256): [`steam-oneface-lr_with_retinaface.tar.gz`][steam-oneface-lr-retinaface] (74 MB)

To use this dataset on Google Colab, run the following:
```bash
!gdown --id 1-0Nk7H6Cn3Nt60EdHG_NWSA8ohi2oBqr
!tar xf steam-oneface-lr_with_retinaface.tar.gz
```

### With the `dlib` module

The dataset consists of 305 images, shared in:
-   the original resolution (300x450): [`steam-oneface-hr_with_dlib.tar.gz`][steam-oneface-hr-dlib] (16 MB)
-   a lower resolution (256x256): [`steam-oneface-lr_with_dlib.tar.gz`][steam-oneface-lr-dlib] (9 MB)

To use this dataset on Google Colab, run the following:
```bash
!gdown --id 1-4RIn9G9Bee2JZ1bK1gkkgkLocHuWJJ4
!tar xf steam-oneface-lr_with_retinaface.tar.gz
```

NB: I would no

### With several detection modules

The notebook [`trim_steam_oneface_dataset.ipynb`][steam-oneface-notebook-trim] trims the dataset by intersecting the results of different detectors.
[![Open In Colab][colab-badge]][steam-oneface-notebook-trim]

The trimmed datasets are:

-   [`Steam-OneFace-small`][steam-oneface-small-gdrive]:
    - 993 images,
    - obtained with modules `face_alignment` and `retinaface`,

To use this dataset on Google Colab, run the following:
```bash
!gdown --id 1-1V5fDhPo75iDtAbrD18rppV-lf51bPW
!tar xf steam-oneface-small-lr.tar.gz
```

-   [`Steam-OneFace-tiny`][steam-oneface-tiny-gdrive]:
    - 168 images,
    - obtained with modules `dlib`, `face_alignment` and `retinaface`.

To use this dataset on Google Colab, run the following:
```bash
!gdown --id 1-2sCVgBUmu6LFug1pzBfmL8zNFFBq27F
!tar xf steam-oneface-tiny-lr.tar.gz
```

## References

-   To download images: [`download_steam_banners.ipynb`][download_steam_banners] in [`woctezuma/google-colab`][code]
-   To filter out duplicates, etc.:
    - for PNG logos: [`remove_duplicates.ipynb`][filter_steam_logos] in [`woctezuma/google-colab`][code]
    - for JPG banners: [`remove_duplicates.ipynb`][filter_steam_banners] in [`woctezuma/steam-stylegan2-ada`][code-ada]
-   To detect faces: [`detect_faces_on_steam_banners.ipynb`][colab-notebook-face-detection] in [`woctezuma/steam-face-detection`][steam-face-detection]


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
[dlib-github]: <https://github.com/davisking/dlib>
[python-face-alignment]: <https://github.com/1adrianb/face-alignment>
[retinaface]: <https://github.com/ternaus/retinaface>
[steam-oneface-gdrive]: <https://drive.google.com/drive/folders/1MlpNk6PwYZWhJegMjuukcYCNFSnXR3wg>
[steam-oneface-hr]: <https://drive.google.com/file/d/1dmm1W8kPINVQrG8NbxXw_KEgU2Nkeksu>
[steam-oneface-lr]: <https://drive.google.com/file/d/1QptHrW9vloTtP--YJsxMY8PZWI2D8NJt>
[steam-oneface-cover-small]: <https://raw.githubusercontent.com/wiki/woctezuma/steam-filtered-image-data/img/oneface-cover-small.jpg>
[steam-oneface-cover-big]: <https://raw.githubusercontent.com/wiki/woctezuma/steam-filtered-image-data/img/oneface-cover.jpg>
[steam-oneface-hr-retinaface]: <https://drive.google.com/file/d/1-04pq-vVnEU5T083DkeLmdRxP2dnQ4Vb>
[steam-oneface-lr-retinaface]: <https://drive.google.com/file/d/1-0Nk7H6Cn3Nt60EdHG_NWSA8ohi2oBqr>
[steam-oneface-hr-dlib]: <https://drive.google.com/file/d/1-4_-d8ClSCRzuULEVgV8gvjQHKkz0WuK>
[steam-oneface-lr-dlib]: <https://drive.google.com/file/d/1-4RIn9G9Bee2JZ1bK1gkkgkLocHuWJJ4>

[steam-oneface-notebook-trim]: <https://colab.research.google.com/github/woctezuma/steam-filtered-image-data/blob/main/trim_steam_oneface_dataset.ipynb>
[steam-oneface-small-gdrive]: <https://drive.google.com/drive/folders/1jSCyxmthOoFQjUEfwRXNUT7Ap9oU8kY_>
[steam-oneface-tiny-gdrive]: <https://drive.google.com/drive/folders/1Godpyq52rfrs8CXdSFzSt_CLErucYf6Z>

[colab-badge]: <https://colab.research.google.com/assets/colab-badge.svg>

[code]: <https://github.com/woctezuma/google-colab>
[code-ada]: <https://github.com/woctezuma/steam-stylegan2-ada>
[download_steam_banners]: <https://colab.research.google.com/github/woctezuma/google-colab/blob/master/download_steam_banners.ipynb>
[filter_steam_logos]: <https://colab.research.google.com/github/woctezuma/google-colab/blob/master/remove_duplicates.ipynb>
[filter_steam_banners]: <https://colab.research.google.com/github/woctezuma/steam-stylegan2-ada/blob/main/remove_duplicates.ipynb>
[colab-notebook-face-detection]: <https://colab.research.google.com/github/woctezuma/steam-face-detection/blob/main/detect_faces_on_steam_banners.ipynb>
