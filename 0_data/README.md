# Topic 1 · Data centers: who lives next door?

## Files in this folder

| File | One row is | Rows × columns |
| --- | --- | --- |
| **1_data_centers_combined.csv** | A census tract (neighborhood) in NC, SC, GA, TN or VA | 10,599 × 39 |
| original/1_data_centers_southeast_tracts.csv | A census tract | 10,599 × 32 |
| original/1b_data_center_facilities_southeast.csv | A data center facility | 377 × 20 |
| data_dictionary.csv | The meaning of every variable in all three files | |

**Start with the combined file.** It is the original tracts file plus 7 new columns that summarize the facilities in each tract.

## How the combined file was made (Join 2)

1. The facilities file has one row per data center. It was summarized to **one row per tract**: how many campuses, buildings and points; how many are run by Amazon, Google, Meta or Microsoft; how many have no operator listed; and the list of operators and facility names.
2. That summary was joined onto the tracts file using **geoid**, the census tract ID both files share.
3. Tracts with no data center get 0 in the count columns and a blank in the list columns.

The result keeps one row per tract (10,599 rows), so tract averages are not distorted by tracts that contain many facilities.

Operator names were standardized before counting (for example, "Amazon Web Serivces", "Amazon IAD75" and "Amazon Web Services" all count as Amazon; "Google LLC" as Google). The original files are unchanged.

**3 facilities are not in the combined file** because they sit in special-use census tracts with no residents, which are not in the tracts file: Fairfax County, VA, Loudoun County, VA, Roane County, TN. They are still in the original facilities file. This is why n_data_centers adds up to 374, not 377.

## What is a geoid?

An 11-digit census tract ID: 2 digits for the state, 3 for the county, 6 for the tract. Example: 37119980300 = North Carolina (37), Mecklenburg County (119), tract 9803.00. Always read it as text so the leading zero (e.g., Georgia = 13, Alabama = 01) is not lost.

## New columns in the combined file

| Variable | Type | Meaning | Values / range | Missing |
| --- | --- | --- | --- | --- |
| dc_n_campus | integer | Number of the tract's data centers mapped as a campus (several buildings) | 0 to 6 | 0 |
| dc_n_building | integer | Number of the tract's data centers mapped as a single building | 0 to 54 | 0 |
| dc_n_point | integer | Number of the tract's data centers mapped only as a point (no size known) | 0 to 2 | 0 |
| dc_n_big_tech | integer | Number of the tract's data centers run by Amazon, Google, Meta or Microsoft (operator names standardized) | 0 to 17 | 0 |
| dc_n_operator_unknown | integer | Number of the tract's data centers with no operator listed | 0 to 15 | 0 |
| dc_operators | text | Companies operating data centers in the tract, separated by ; (standardized names; blank = none or unknown) | 33 distinct values, e.g. Datasite | 10,540 |
| dc_named_facilities | text | Names of the tract's data centers, separated by ; (blank = none or unnamed) | 81 distinct values, e.g. DataSite Atlanta | 10,518 |

All other columns are described in the full table below and in data_dictionary.csv.

## All variables in the combined file

| Variable | Type | Meaning | Values / range | Missing |
| --- | --- | --- | --- | --- |
| geoid | text | Census tract ID (11 digits, 2020 tracts). Unique key; keep as text | 10,599 distinct values, e.g. 13001950100 | 0 |
| state | text | State abbreviation | GA; NC; SC; TN; VA | 0 |
| county_fips | text | County FIPS code (5 digits); keep as text | 533 distinct values, e.g. 13001 | 0 |
| county_name | text | County name (repeats across states; use with state) | 431 distinct values, e.g. Appling County | 0 |
| total_pop | integer | Total population (ACS 5-yr 2020–2024) | 2 to 18,754 | 0 |
| median_hh_income | integer | Median household income, $ (ACS 5-yr 2020–2024). 250,001 = $250,000 or more | 2,499 to 250,001 | 74 |
| poverty_rate | decimal | % of people below the poverty line | 0 to 100 | 32 |
| pct_white_nh | decimal | % non-Hispanic White | 0 to 100 | 0 |
| pct_black_nh | decimal | % non-Hispanic Black | 0 to 100 | 0 |
| pct_asian_nh | decimal | % non-Hispanic Asian | 0 to 67.9 | 0 |
| pct_hispanic | decimal | % Hispanic or Latino (any race) | 0 to 98.9 | 0 |
| pct_nonwhite | decimal | 100 − pct_white_nh | 0 to 100 | 0 |
| pct_bachelors_plus | decimal | % of adults 25+ with a bachelor's degree or higher | 0 to 100 | 1 |
| unemployment_rate | decimal | % of the labor force unemployed | 0 to 52.8 | 22 |
| pct_owner_occupied | decimal | % of occupied homes that are owner-occupied | 0 to 100 | 37 |
| pct_no_vehicle | decimal | % of households with no vehicle | 0 to 100 | 37 |
| pct_rent_burdened | decimal | % of renter households paying 30%+ of income on rent | 0 to 100 | 50 |
| median_gross_rent | integer | Median gross rent, $/month. 3,501 = $3,500 or more | 99 to 3,501 | 546 |
| median_home_value | integer | Median owner-occupied home value, $. 2,000,001 = $2M or more | 13,500 to 2,000,001 | 229 |
| land_sqmi | decimal | Land area, square miles | 0.03 to 515.98 | 0 |
| lat | decimal | Latitude of the tract's internal point | 30.58 to 39.34 | 0 |
| lon | decimal | Longitude of the tract's internal point (negative = west) | -90.11 to -75.29 | 0 |
| pop_density_sqmi | decimal | People per square mile (total_pop ÷ land_sqmi) | 0 to 103,384.6 | 0 |
| n_data_centers | integer | Number of data center facilities located in the tract | 0 to 60 | 0 |
| dc_total_sqft | integer | Total data center footprint in the tract, sq ft (0 = none) | 0 to 27,106,734 | 0 |
| dc_largest_sqft | integer | Largest single facility in the tract, sq ft (blank = no facility) | 1,232 to 26,597,517 | 10,518 |
| has_data_center | 0/1 flag | 1 if the tract has at least one data center | 1 = yes (87); 0 = no (10,512) | 0 |
| dist_nearest_dc_km | decimal | Straight-line distance from the tract point to the nearest data center, km | 0.05 to 337.84 | 0 |
| n_dc_within_10km | integer | Number of data centers within 10 km of the tract point | 0 to 202 | 0 |
| nearest_metro | text | Closest of 12 regional metro centers | 12 distinct values, e.g. Charleston SC | 0 |
| dist_nearest_metro_km | decimal | Straight-line distance to that metro's city hall, km | 0.2 to 359.4 | 0 |
| in_charlotte_region | 0/1 flag | 1 if the tract is in the 14-county Charlotte region | 1 = yes (750); 0 = no (9,849) | 0 |
| dc_n_campus | integer | Number of the tract's data centers mapped as a campus (several buildings) | 0 to 6 | 0 |
| dc_n_building | integer | Number of the tract's data centers mapped as a single building | 0 to 54 | 0 |
| dc_n_point | integer | Number of the tract's data centers mapped only as a point (no size known) | 0 to 2 | 0 |
| dc_n_big_tech | integer | Number of the tract's data centers run by Amazon, Google, Meta or Microsoft (operator names standardized) | 0 to 17 | 0 |
| dc_n_operator_unknown | integer | Number of the tract's data centers with no operator listed | 0 to 15 | 0 |
| dc_operators | text | Companies operating data centers in the tract, separated by ; (standardized names; blank = none or unknown) | 33 distinct values, e.g. Datasite | 10,540 |
| dc_named_facilities | text | Names of the tract's data centers, separated by ; (blank = none or unnamed) | 81 distinct values, e.g. DataSite Atlanta | 10,518 |

## Things to watch for

- **Capped values:** median_hh_income = 250,001 means $250,000 or more (75 tracts); 2,499 means under $2,500 (1 tract). median_gross_rent = 3,501 means $3,500 or more (85 tracts). median_home_value = 2,000,001 means $2M or more (4 tracts).
- **Suspicious values:** tract 51153901412 (Prince William County, VA) has median rent of $99; tract 47093000902 (Knox County, TN) has median income of $2,499 with 85% poverty, likely student housing.
- **Tiny tracts:** 8 tracts have fewer than 100 people. Their percentages are unreliable.
- **Data centers are rare:** only 87 of 10,599 tracts (0.8%) have one, and 292 of 377 facilities are in Virginia. Results about data center tracts are driven by Northern Virginia.
- **Compare like with like:** data centers are built near metro areas and power lines. When comparing tracts with and without data centers, account for urban vs. rural (dist_nearest_metro_km, pop_density_sqmi).
- Distances are straight-line, not driving distance.

## Sources

- IM3 Open Source Data Center Atlas, Pacific Northwest National Laboratory (2026 update). https://doi.org/10.57931/2550666
- U.S. Census Bureau, American Community Survey 5-year 2020–2024. https://www2.census.gov/programs-surveys/acs/summary_file/2024/
- U.S. Census Bureau 2024 Gazetteer (tract area and location).
- U.S. Census Geocoder (matched each facility to its tract).
