# Screening-Extreme-Rainfall-and-Persistent-Dryness-Across-728-Indian-Districts-Using-Apache-Spark

Presented at LAKE 2026: 15th Biennial Lake Conference (Bengaluru, 17–20 December 2026).

Author: Tejas S., BCA (4th Year), School of Engineering, Chanakya University, Bengaluru

A reproducible Apache Spark (PySpark) pipeline that screens India Meteorological Department (IMD) district-wise daily rainfall to show where rainfall was extreme and where districts stayed persistently dry, between 19 August and 15 October 2026. It is a screening tool: it shows which districts to examine first. It is not a flood or drought assessment.

Research question

Between 19 Aug and 15 Oct 2026, which districts saw heavy to extremely heavy rain, and which stayed persistently dry against their daily normal?

Objectives
Clean and validate the IMD district-wise daily file
Classify records into IMD intensity classes
Rank districts by peak rain, departure from normal and dry runs
Judge what the results can and cannot support
Data
Item	Detail
Source	IMD, "Rainfall Daily IMD: Rainfall DistrictWise Daily", National Water Informatics Centre, Govt. of India
Link	https://nwdp.nwic.gov.in/en/dataset/rainfall-daily-imd (accessed 20 Oct 2026)
Size	42,190 rows, 22 columns
Coverage	728 districts, 58 days
Method
Source. Load the IMD district-wise daily rainfall CSV.
Clean (Spark). Parse dates, strip % signs, check ranges. No row was invalid or duplicated.
Exclude no data. 496 rows (1.2%, 51 districts) carry IMD category ND with a placeholder 0 mm. They are left out of all means, departures and dry runs. 41,694 valid records remain.
Classify. IMD daily bands:
Class	Daily rain (mm)	Records	Mean (mm)
Below heavy	under 64.5	41,372	4.11
Heavy	64.5 to 115.5	273	81.5
Very heavy	115.6 to 204.4	47	135.9
Extremely heavy	204.5 and above	2	221.5
Aggregate. Per district: peak, mean, heavy records, departure from normal (mm and %), and longest run of days under 1 mm. Departure rankings need 30 or more valid days.
Data-quality notes
Dibang Valley, Lower Dibang Valley and North Delhi have no data on all 58 days. Counting ND as 0 mm makes them look dry; they are not dry districts.
Spark's default CSV reader dropped 13 of 22 columns. Read the file with the multiLine option to keep them.
Key results
322 records (0.77%) reached heavy rain or more: 47 very heavy and 2 extremely heavy.
Peak daily total: 231.9 mm in Nabarangapur, Odisha.
Mean rainfall per record fell from 6.60 mm in August to 1.40 mm in October.
Extreme rain clustered in southern Chhattisgarh and Odisha and in Uttar Pradesh.
Longest dry runs (days under 1 mm): 55 in Leh and Ladakh, 50 in Firozpur, 37 in Mahendragarh.
The biggest mm shortfalls are in the Meghalaya hills (for example West Jaintia Hills, 21.78 mm per day below normal).
Per cent departure is capped at 100: Kargil's 100% is only 0.08 mm per day.
Limits
Indicators only: no flood, crop or loss data.
58 days cannot show a trend.
Dry runs are not an official drought index.
Per cent departure is unstable when the normal is small.
Recommendations
Treat IMD ND as missing, never 0 mm; fix the feeds for the three no-data districts.
Put the top-10 peak districts on an early-warning and drainage watchlist.
Rank dry districts by longest dry run and mm deficit, not per cent alone.
Build multi-year district baselines before drawing climate conclusions.
Run the Spark pipeline on every daily IMD release.
Validate results with IMD experts before policy action.
Tech stack

Python, Apache Spark (PySpark), Matplotlib, Google Colab.

Reproducing the analysis
Download the district-wise daily rainfall CSV from the IMD link above.
Open the notebook in Google Colab (or any environment with PySpark installed).
Read the CSV with multiLine=True, run the cleaning, ND exclusion and classification steps, then the per-district aggregation.
Figures in the poster are produced from the author's notebook outputs.
References
IMD, "Rainfall Daily IMD: Rainfall DistrictWise Daily", National Water Informatics Centre, Govt. of India. nwdp.nwic.gov.in/en/dataset/rainfall-daily-imd
IMD daily rainfall intensity classes: heavy 64.5–115.5 mm, very heavy 115.6–204.4 mm, extremely heavy 204.5 mm and above.
M. Zaharia et al., "Apache Spark: a unified engine for big data processing", Commun. ACM 59(11), 2016.
J. D. Hunter, "Matplotlib: a 2D graphics environment", Comput. Sci. Eng. 9(3), 2007.
Acknowledgements

Rainfall data: India Meteorological Department (IMD) via the National Water Informatics Centre, Ministry of Jal Shakti. Thanks to Chanakya University for the opportunity.
