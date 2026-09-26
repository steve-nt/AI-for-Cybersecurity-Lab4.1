# Dataset

`dataset_phishing.csv` is the **Web page phishing detection** dataset, version 3:

> Hannousse, A. & Yahiouche, S. (2021). *Web page phishing detection* (Version 3) [Data set].
> Mendeley Data. https://doi.org/10.17632/c2gw7fy2j4.3

- **Licence:** Creative Commons Attribution 4.0 International (CC BY 4.0),
  http://creativecommons.org/licenses/by/4.0
- **Source file:** `dataset_B_05_2020.csv` from https://data.mendeley.com/datasets/c2gw7fy2j4/3,
  downloaded on 2026-09-26.
- **Changes:** renamed to `dataset_phishing.csv` (the name used in the lab instructions and on Kaggle).
  The content is unchanged.
- **SHA-256:** `21093e2902e5441c86a6daf95e86e7c332046e477fdf109a579d7bd81e586d6c`
  (the same hash Mendeley publishes for the original file)

Contents as checked on download: 11,430 rows × 89 columns (`url`, 87 features, `status`),
5,715 `phishing` and 5,715 `legitimate`, no missing values.

To download it again:

```bash
curl -L -o data/dataset_phishing.csv \
  "https://data.mendeley.com/public-files/datasets/c2gw7fy2j4/files/575316f4-ee1d-453e-a04f-7b950915b61b/file_downloaded"
sha256sum data/dataset_phishing.csv
```
