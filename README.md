## README.md — Third-Wave Coffee Project 
A global visual and statistical analysis of specialty cafés and their urban geographies

### Project Overview
This project explores the visual branding and spatial distribution of third-wave coffee shops around the world. It combines:
- Web scraping
- Image captioning with AI models
- Optical character recognition (OCR)
- Theme clustering
- Census-based regression analysis
- Interactive visual storytelling

The aim was to map the “aesthetic logic” of third-wave cafés and analyze where, and why, they appear in urban environments.

### Data Sources
- **Primary source**: [ThirdWaveNearMe.com](https://thirdwavenearme.com), a curated global map of specialty cafés.
- **Instagram content**: Screenshots from verified café pages (5,276 images), OCR-extracted bios and captions.
- **Websites**: 200+ homepage screenshots; metadata scraped
- **Census data**: American Community Survey (ACS 5-Year Estimates), tract-level data for New York City, Dallas, and Miami.
- **Geospatial data**: U.S. Census TIGER/Line shapefiles + KML exports from Google My Maps.

### Process Summary
**1.Web Scraping**
- Scraped nearly 100 city pages using Playwright
- Parsed 100 KML files from Google My Maps (5,204 café locations)
- Manually refined Bing search to gather website and social links from Bing
- USed Playwright to automate social links scraping process and manual checks of 5,000+ links afterwards
- Extracted social links (Instagram, websites) and saved to CSV

**2.Instagram & Website Content** 
- Captured 5,191 full-page Instagram screenshots using automated browser scripting
- Took 3-part screenshots (top/middle/bottom) of 200+ websites, automated browser scripting
- Ran OCR on images using Tesseract; matched to café metadata via fuzzy logic (RapidFuzz)

**3.AI Vision + Captioning** 
- Ran BLIP (Salesforce) and OWL-ViT to generate captions and object tags on 6,000+ images
- Rejected generic or inaccurate outputs manually

**4.Theme Clustering** 
Used a hybrid method:
- Ran FASTopic on ~500 bios
- Manually classified outputs into buckets: minimalist design, dog-friendly, plant-heavy, laptop zones, vegan, kid-friendly, vintage aesthetic, and more
- The AI model provided raw clusters; these were interpreted, validated, and labeled by hand
- Labeled ~5,000 café bios into these predefined theme categories

**5.Color Analysis** 
To quantify the visual identity of each city’s cafés:
- Used Playwright screenshots from Instagram and websites
- For each image:
    Applied K-Means clustering (k=5) on RGB values (converted to CIELAB for perceptual accuracy)
    Extracted the top 1–2 dominant colors per image
- Aggregated all café colors within each city and computed mean RGB color
- Visualized results as city-level swatches, revealing dominant aesthetic tones (e.g., warm beiges, tropical greens, icy neutrals)

**6. Data merging** 
- Joined café coordinates back to csv with themes extracted files to map. 
- Joined café coordinates to census tracts in NYC, Dallas, and Miami; ran logistic regression models on tract-level variables to estimate the probability of café presence.

### Technical Highlights
- **Scraping**: Playwright, BeautifulSoup; scraped JS-heavy websites and manually reviewed tab automation flows.
- **AI captioning**: BLIP model used locally for ~4 hours (CPU-bound); rejected >5% of generic outputs.
- **Manual supervision** Less than 5% of incorrect Instagram links and websites after manually reviewed thousands of links and ran fact-check with Perplexity and ChatGPT.
- **Topic modeling**: FASTopic clustered bios into ~30 themes; top words per cluster manually validated.
- **Regression**: ACS data joined to café points by census tract; modeled effects of log income, rent, home value, Gini index, and education levels.
- **Visualization**: Created horizontal bar charts (matplotlib) and probability maps using geopandas.

### Regression Coefficients Summary
| Variable              | NYC     | Dallas | Miami  |
|-----------------------|---------|--------|--------|
| log_income            | -0.3649 | -1.2013| -1.9067|
| pct_bachelor_plus     | 0.0008  | 0.0003 | 0.0004 |
| log_rent              | 3.4188  | 1.7494 | 1.7683 |
| log_home_value        | 1.1426  | 1.6046 | 2.0776 |
| gini_index            | 8.0569  | 5.4155 | 1.5188 |

🗺️ These models helped produce city-level maps predicting future café locations based on underlying socioeconomic traits.

### Limitations
- Google My Maps KML export is not publicly accessible via API, requiring semi-manual tab interaction.
- OCR quality varied by font style and screenshot clarity.
- BLIP sometimes failed on image contrast or dark palettes.
- ACS-based regression only applies where census data and café density overlap.
- Student life and no money to support the project led to no chatGPT-vision model success since $90 was too expensive, so very sad. 

### Reflections
This was the most technically ambitious project I’ve built to date. It taught me:
- How to manage fuzzy, multi-source datasets.
- How to chain scraping, AI, geospatial joins, and modeling together.
- The limits of AI-generated captions and the value of manual validation (eye-drops donation encouraged).
- How to convert regression outputs into geographic storytelling.

I would love to expand this work to cluster cafés globally by aesthetic similarity (using computer vision embedding models) or examine how neighborhood change precedes café appearance over time.

### Files
- `scrape_thirdwaveneame.ipynb`: Scraping map data.
- `analyze_and_map.ipynb`: KML verification, café geojoin, and city visuals.
- `usingBLIP.ipynb`: Image captioning + topic modeling.
- `clean_and_merge.ipynb`: OCR matching, fuzzy logic, and dataset consolidation.
- `coffee_logit_coeff_horizontal.svg`: Regression coefficient chart.
- `README.md` (this file): Project narrative + reproducibility notes.
To replicate the analysis, begin with scrape_thirdwaveneame.ipynb to gather base data, then proceed to clean_and_merge.ipynb for merging and text processing, followed by usingBLIP.ipynb for caption generation and theme detection. Use analyze_and_map.ipynb for regression and visualizations.

To replicate:
1. Run scrape_thirdwaveneame.ipynb
2. Clean + join data in clean_and_merge.ipynb
3. Generate image captions + assign themes via usingBLIP.ipynb
4. Run modeling and mapping in analyze_and_map.ipynb

### Data Volume Summary
- 90+ cities scraped
- 5,204 cafés geolocated
- 5,191 Instagram screenshots processed
- 5,000+ bios modeled
- 6,000+ data points cleaned, matched, and visualized


