# Data Files

The Yelp dataset is **not included** in this repository. It is governed by Yelp's licensing terms and cannot be redistributed.

## How to obtain the data

1. Download the **Yelp Open Dataset** from [yelp.com/dataset](https://www.yelp.com/dataset/download). You will need to agree to Yelp's terms of use.

2. Extract the archive. You will get several `.json` files. This project uses two:
   - `yelp_academic_dataset_business.json`
   - `yelp_academic_dataset_review.json`

## How to filter the data

The full review file is too large to upload to Colab directly (over 5 GB). The project uses a local pre-filtering step to keep only martial arts businesses and their associated reviews.

A reference filtering script is included in the **Appendix** of the main notebook. Run it locally on your machine before uploading to Colab. It produces two project-ready files:

- `business_filtered.csv` — martial arts businesses only
- `reviews_filtered.csv` — reviews associated with those businesses

Place both files in this `data/` directory (or in your Colab runtime's working directory) before running the main notebook.

## Expected file properties after filtering

| File | Approximate size | Rows |
|------|-----------------:|-----:|
| `business_filtered.csv` | < 1 MB | 265 |
| `reviews_filtered.csv` | ~ 8 MB | 3,212 (after dropping 3-star and null text) |

## Privacy note

Reviews in the Yelp dataset are public-facing and contain customer first names and business identifiers. Do not include extracted reviews in screenshots, blog posts, or other public-facing material without adhering to Yelp's terms of use.
