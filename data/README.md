# Data

The directory structure is as follows:
```
data/
├ banners/
| ├ app_ids_banners.txt             # appIDs (including duplicates)
| ├ duplicate_banners.txt           # ALL of the duplicates
| ├ duplicate_banners_to_remove.txt # it is sufficient to remove these
| └ duplicates_0.json               # matches between duplicates
├ download-queries/
| ├ app_ids.txt                     # all the queried appIDs
| └ delta_20201125.txt              # newly queried appIDs on Nov 25, 2020
├ logos/
| ├ app_ids_logos.txt
| ├ duplicate_logos.txt
| ├ duplicate_logos_to_remove.txt
| └ duplicates_0.json
├ merge/
  ├ app_ids.txt                     # intersection of sets (cf. below)
  └ app_ids_strict.txt              # ALL of the duplicates are removed
```

The file `app_ids_banners.txt` contains all the appIDs left after applying filters mentioned in the README.
Duplicates have not yet been filtered out.

The file `duplicate_banners_to_remove.txt` contains a list of appIDs which:
-   are duplicates,
-   and can be safely removed.
Indeed, the selection was performed by `imagededup` so that at least one duplicate would be kept.

The file `duplicates_0.json` contains all the matches between the duplicates, as returned by `imagededup`.

The file `duplicate_banners.txt` is obtained from `duplicates_0.json`, as such:

```python
import json

with open('duplicates_0.json', 'r') as f:
  d = json.load(f)

duplicate_banners = [
                     int(fname.split('.')[0])
                     for fname in d
                     if len(d[fname])>0
                     ]

with open('duplicate_banners.txt', 'w') as f:
  for app_id in duplicate_banners:
    f.write('{}\n'.format(app_id))
```

## Merge of filtered results

Filters are independently applied to banners and logos.

Then filtered results can be:
-   merged with the following script,
-   saved to the folder called `merge/`.

```python
from pathlib import Path

with open('banners/app_ids_banners.txt', 'r') as f:
    banners = [int(i.strip()) for i in f.readlines()]

with open('logos/app_ids_logos.txt', 'r') as f:
    logos = [int(i.strip()) for i in f.readlines()]

with open('banners/duplicate_banners.txt', 'r') as f:
    dup_banners = [int(i.strip()) for i in f.readlines()]

with open('logos/duplicate_logos.txt', 'r') as f:
    dup_logos = [int(i.strip()) for i in f.readlines()]

l = set(banners).intersection(logos)

l_dup = set(dup_banners).union(dup_logos)

l_strict = l.difference(l_dup)

folder_name = 'merge'
Path(folder_name).mkdir(exist_ok=True)

with open('{}/app_ids.txt'.format(folder_name), 'w') as f:
  for i in sorted(l):
    f.write(str(i) + '\n')

with open('{}/app_ids_strict.txt'.format(folder_name), 'w') as f:
  for i in sorted(l_strict):
    f.write(str(i) + '\n')
```
