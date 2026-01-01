# Complete Standard Operating Procedure (SOP) for Data Extraction from Literature

## Project: Chitosan/Metal Nanoparticle Synthesis - Systematic Data Mining

---

## **1. PREPARATION PHASE**

### 1.1 Define Search Strategy
**Objective:** Identify all relevant papers on chitosan-based nanoparticles with TPP, metal ions (Co, Zn, Ag, Cu), or other crosslinkers.

**Databases to Search:**
- PubMed/MEDLINE
- Scopus
- Web of Science
- Google Scholar
- ScienceDirect
- IEEE Xplore (for engineering applications)

**Search Terms (Boolean):**
```
("chitosan nanoparticle*" OR "chitosan nanogel*" OR "chitosan-TPP" OR "chitosan-metal") 
AND 
("synthesis" OR "preparation" OR "formulation" OR "fabrication")
AND
("particle size" OR "zeta potential" OR "characterization")
```

**Filters:**
- Publication Year: 2010-2025 (adjust as needed)
- Language: English
- Article Types: Original Research, Short Communications (exclude reviews initially)

**Target:** 100-200 papers minimum for robust ML models

---

### 1.2 Paper Screening Criteria

**Inclusion Criteria:**
- Contains chitosan-based nanoparticle synthesis
- Reports at least 2 of: particle size, PDI, zeta potential
- Clearly states synthesis parameters (concentration, pH, temperature, etc.)
- Full-text available or accessible via institutional access

**Exclusion Criteria:**
- Pure theoretical/computational studies (no experimental data)
- Composite materials where chitosan is minor component (<30%)
- Insufficient synthesis details
- Duplicate publications

**Initial Screening:**
1. Read title and abstract
2. Check if methods section describes synthesis protocol
3. Verify if results include characterization data (TEM, DLS, zeta potential)
4. Mark as "Include," "Exclude," or "Maybe" (re-review)

---

### 1.3 Create Data Extraction Template

**Prepare Excel/CSV with these columns:**

| Column Name | Data Type | Unit | Notes |
|-------------|-----------|------|-------|
| Paper_ID | Text | - | Unique identifier (e.g., P001, P002) |
| DOI | Text | - | Digital Object Identifier |
| Authors | Text | - | First author surname et al. |
| Year | Integer | - | Publication year |
| Journal | Text | - | Journal name |
| Application | Categorical | - | Biomedical/Catalytic/Environmental/Other |
| **CHITOSAN PARAMETERS** |
| Chitosan_Conc_mgmL | Numeric | mg/mL | Exact concentration used |
| Chitosan_MW_kDa | Numeric | kDa | Molecular weight |
| Chitosan_DD_percent | Numeric | % | Degree of deacetylation |
| Chitosan_Source | Text | - | Commercial/Lab-prepared, company name |
| **CROSSLINKER/METAL PARAMETERS** |
| Crosslinker_Type | Categorical | - | TPP/Metal ion (Co/Zn/Ag/Cu)/Other |
| Crosslinker_Conc_mgmL | Numeric | mg/mL or mM | Concentration |
| Chitosan_Crosslinker_Ratio | Numeric | - | Mass or molar ratio |
| Metal_Ion_Type | Text | - | If metal: Co²⁺, Zn²⁺, Ag⁺, etc. |
| **SYNTHESIS CONDITIONS** |
| Solution_pH | Numeric | - | pH of chitosan solution |
| Temperature_C | Numeric | °C | Synthesis temperature |
| Stirring_Speed_rpm | Numeric | rpm | Magnetic stirring rate |
| Reaction_Time_min | Numeric | min | Total reaction duration |
| Crosslinker_Addition_Rate_mL_min | Numeric | mL/min | Dropwise addition rate |
| Ultrasonication_Amplitude_percent | Numeric | % | If used |
| Ultrasonication_Time_min | Numeric | min | Duration of sonication |
| Ionic_Strength_mM | Numeric | mM | Background salt concentration |
| Acid_Solvent_Type | Categorical | - | Acetic acid/HCl/Citric acid |
| Acid_Concentration_percent | Numeric | % (v/v or w/v) | Acid used to dissolve chitosan |
| **CROSSLINKING PROPERTIES** |
| Crosslinking_Degree_percent | Numeric | % | If reported (FTIR, titration) |
| **OUTPUT PROPERTIES** |
| Particle_Size_nm | Numeric | nm | Mean diameter (DLS/TEM) |
| Particle_Size_Method | Categorical | - | DLS/TEM/SEM/AFM |
| PDI | Numeric | - | Polydispersity index (0-1) |
| Zeta_Potential_mV | Numeric | mV | Surface charge |
| Zeta_Measurement_pH | Numeric | - | pH at which zeta was measured |
| Encapsulation_Efficiency_percent | Numeric | % | If drug/metal loading studied |
| Yield_percent | Numeric | % | Synthesis yield |
| **STABILITY DATA** |
| Storage_Temperature_C | Numeric | °C | Storage condition |
| Stability_Duration_days | Numeric | days | How long stable |
| **ANTIMICROBIAL/BIOACTIVITY** |
| Bacterial_Strain | Text | - | E. coli, S. aureus, etc. |
| ZOI_mm | Numeric | mm | Zone of inhibition |
| MIC_ugmL | Numeric | µg/mL | Minimum inhibitory concentration |
| Cell_Viability_percent | Numeric | % | If cytotoxicity tested |
| Cell_Line | Text | - | NIH3T3, HeLa, etc. |
| Test_Concentration_ugmL | Numeric | µg/mL | Concentration for cell viability |
| **DATA QUALITY** |
| Data_Completeness | Categorical | - | Complete/Partial/Minimal |
| Notes | Text | - | Any special conditions or remarks |

---

## **2. DATA EXTRACTION PHASE**

### 2.1 Systematic Extraction Process

**For Each Paper:**

**Step 1: Read Methods Section**
- Locate "Materials and Methods" or "Experimental" section
- Identify all synthesis parameters
- Note any variations (e.g., multiple formulations tested)

**Step 2: Extract Synthesis Parameters**
- **Chitosan:** Concentration, MW, DD%, source/supplier
- **Crosslinker:** Type (TPP/metal), concentration, addition method
- **Conditions:** pH, temperature, stirring, time, sonication
- **If multiple experiments:** Create separate rows for each unique formulation

**Step 3: Extract Characterization Data**
- **Primary Outputs:** Particle size, PDI, zeta potential
- **Check figures/tables:** Often data is in graphs or supplementary materials
- **Note measurement methods:** DLS vs. TEM (different techniques give different values)

**Step 4: Extract Application-Specific Data**
- If biomedical: antimicrobial data, cytotoxicity, drug release
- If environmental: adsorption capacity, catalytic activity
- If catalytic: conversion efficiency, selectivity

**Step 5: Data Quality Assessment**
```
Complete = All key parameters reported
Partial = Some parameters missing (e.g., no stirring speed)
Minimal = Only size and zeta reported, synthesis poorly described
```

---

### 2.2 Handling Common Issues

#### **Issue 1: Missing Parameters**
**Solution:**
- Mark as "NaN" or "Not Reported" in Excel
- In Notes column, write: "Parameter not specified in paper"
- If crucial parameter missing, consider excluding paper

#### **Issue 2: Conflicting Units**
**Solution:**
- **Standardize all units in template:**
  - Concentration: mg/mL (convert from µg/mL, g/L, etc.)
  - MW: kDa (convert from Da)
  - Time: minutes (convert from hours, seconds)
- Create unit conversion reference sheet

**Common Conversions:**
```
1 g/L = 1 mg/mL
1 wt% = 10 mg/mL (assuming density ≈ 1 g/mL)
1 mM = Molecular Weight (mg/L)
```

#### **Issue 3: Multiple Formulations in One Paper**
**Solution:**
- Create multiple rows, one per formulation
- Use Paper_ID with suffix: P001a, P001b, P001c
- Keep DOI and authors same for all rows

**Example:**
```
Paper_ID  | Chitosan_Conc | TPP_Conc | Size_nm
P001a     | 2.0           | 1.0      | 150
P001b     | 4.0           | 1.0      | 220
P001c     | 2.0           | 2.0      | 180
```

#### **Issue 4: Data Only in Graphs**
**Solution:**
- Use graph digitization software (WebPlotDigitizer, GraphClick)
- Estimate values from error bars (use midpoint)
- Note in "Notes" column: "Data extracted from Figure 2"

#### **Issue 5: Ranges Instead of Single Values**
**Solution:**
- If paper reports "pH 5-6," record as 5.5 (midpoint)
- If "particle size 100-150 nm," record 125 nm
- Note in "Notes" column: "Range reported, using midpoint"

#### **Issue 6: Unreported pH or Temperature**
**Solution:**
- If not stated, mark as "NaN"
- Common assumptions (use cautiously):
  - Room temperature = 25°C (only if stated "room temp")
  - pH of acetic acid solution ≈ 4.5 (if "1% acetic acid" mentioned without pH)
- **Always note assumptions in "Notes" column**

---

### 2.3 Quality Control During Extraction

**Daily Checklist:**
- [ ] All units consistent with template
- [ ] No empty cells in mandatory columns (Paper_ID, Year, Size)
- [ ] Numeric values are actually numbers (not text like "~150")
- [ ] Categorical values match predefined options
- [ ] DOI is correct and clickable

**Weekly Review:**
- Re-check 10% of extracted papers for accuracy
- Compare your extracted values with original paper
- Correct any transcription errors

---

## **3. POST-EXTRACTION PHASE**

### 3.1 Data Validation

**Check for Outliers:**
```python
import pandas as pd
df = pd.read_csv('extracted_data.csv')

# Identify unrealistic values
print(df[df['Particle_Size_nm'] > 1000])  # Suspiciously large
print(df[df['pH'] > 14])  # Impossible pH
print(df[df['Zeta_Potential_mV'].abs() > 100])  # Unusual zeta
```

**Correct Outliers:**
- Re-check original paper
- If confirmed error, correct it
- If paper has typo, note in "Notes" column or exclude

### 3.2 Data Cleaning

**Step 1: Remove Duplicates**
```python
df = df.drop_duplicates(subset=['DOI', 'Chitosan_Conc_mgmL', 'Particle_Size_nm'])
```

**Step 2: Standardize Text Entries**
```python
# Standardize categorical values
df['Application'] = df['Application'].str.title()
df['Crosslinker_Type'] = df['Crosslinker_Type'].replace({'tpp': 'TPP', 'TPP/Tripolyphosphate': 'TPP'})
```

**Step 3: Handle Missing Data**
- Count missing values per column: `df.isnull().sum()`
- If >50% missing for a parameter, consider excluding that parameter
- If <20% missing, consider imputation (but not for ML targets!)

### 3.3 Final Dataset Preparation

**Create Summary Statistics:**
```python
print(df.describe())
print(f"Total papers: {df['DOI'].nunique()}")
print(f"Total formulations: {len(df)}")
print(f"Years covered: {df['Year'].min()}-{df['Year'].max()}")
```

**Save Clean Dataset:**
```python
df.to_csv('chitosan_nanoparticles_final_dataset.csv', index=False)
```

---

## **4. DOCUMENTATION**

### 4.1 Maintain Extraction Log

**Create a separate file: "Extraction_Log.txt"**

```
Data Extraction Log
Project: Chitosan Nanoparticle ML
Start Date: [Date]
Extractor: [Your Name]

Papers Screened: 250
Papers Included: 120
Papers Excluded: 130
  - Reason: No synthesis details (45)
  - Reason: Not chitosan-based (30)
  - Reason: Review articles (25)
  - Reason: Full text unavailable (20)
  - Reason: No characterization data (10)

Date       Papers Extracted   Notes
---------- -----------------  ---------------------------
2025-10-20  15                Focus on TPP papers
2025-10-21  20                Added metal ion studies
...
```

### 4.2 Create Data Dictionary

**File: "Data_Dictionary.txt"**

```
VARIABLE NAME: Chitosan_Conc_mgmL
DESCRIPTION: Concentration of chitosan in aqueous solution
UNIT: mg/mL
DATA TYPE: Numeric (continuous)
TYPICAL RANGE: 0.5 - 20 mg/mL
MISSING VALUES: Allowed (will be imputed)
NOTES: If reported as % (w/v), assume density = 1 g/mL for conversion

VARIABLE NAME: Crosslinker_Type
DESCRIPTION: Type of crosslinking agent used
DATA TYPE: Categorical
ALLOWED VALUES: TPP, Co2+, Zn2+, Ag+, Cu2+, Other
MISSING VALUES: Not allowed
NOTES: If multiple crosslinkers, separate into different rows
...
```

---

## **5. TROUBLESHOOTING GUIDE**

### Problem: "Paper doesn't report MW or DD%"
**Solution:** 
- Check if commercial chitosan source is cited (e.g., Sigma-Aldrich)
- Look up supplier product specifications online
- If unavailable, use literature average (MW: 150 kDa, DD: 85%) and note assumption

### Problem: "Particle size varies across different measurements"
**Solution:**
- Prioritize DLS over TEM (DLS is hydrodynamic diameter, more relevant for colloidal stability)
- If both reported, use DLS for "Particle_Size_nm" and note TEM value in "Notes"

### Problem: "Multiple pH values mentioned (chitosan solution pH vs. final pH)"
**Solution:**
- Extract the **initial chitosan solution pH** (this affects synthesis)
- Note final pH in "Notes" column if different

### Problem: "Can't access full text"
**Solution:**
1. Try Sci-Hub (use cautiously, legal gray area)
2. Email corresponding author directly
3. Check university library interlibrary loan
4. If unavailable after 1 week, exclude and move to next paper

---

## **6. FINAL CHECKLIST BEFORE ML MODELING**

- [ ] All numeric columns are numeric (use `df.dtypes` to verify)
- [ ] All categorical columns have consistent spelling
- [ ] No single missing value in Paper_ID, Year, or primary target (Size/Zeta)
- [ ] Outliers identified and verified/corrected
- [ ] Dataset has ≥80 unique formulations for ML
- [ ] Data dictionary and extraction log completed
- [ ] Dataset saved in CSV format with UTF-8 encoding

---

## **7. RECOMMENDED TOOLS**

**Literature Management:**
- Zotero (free, open-source)
- Mendeley
- EndNote

**Data Extraction:**
- Microsoft Excel / Google Sheets (for small datasets <500 rows)
- Python + pandas (for large datasets)

**Graph Digitization:**
- WebPlotDigitizer (https://automeris.io/WebPlotDigitizer/)

**Unit Conversion:**
- Online calculators
- Create custom Excel formulas

**Quality Control:**
- Double-entry (two people extract same papers independently, compare)
- Random audits (re-extract 10% of papers)

---

## **APPENDIX A: Example Extraction Workflow**

**Paper:** "Chitosan-TPP nanoparticles for drug delivery" (2023)

**Step 1: Scan Methods**
- Found: Chitosan 2 mg/mL, TPP 1 mg/mL, pH 5.0, 25°C, stirring 700 rpm, 30 min

**Step 2: Scan Results**
- Found: Size = 180 nm (DLS), PDI = 0.25, Zeta = +35 mV

**Step 3: Fill Template**
```
Paper_ID: P045
DOI: 10.xxxx/xxxxx
Year: 2023
Chitosan_Conc_mgmL: 2.0
Crosslinker_Type: TPP
Crosslinker_Conc_mgmL: 1.0
Solution_pH: 5.0
Temperature_C: 25
Stirring_Speed_rpm: 700
Reaction_Time_min: 30
Particle_Size_nm: 180
PDI: 0.25
Zeta_Potential_mV: 35
Data_Completeness: Complete
Notes: High-quality paper, all parameters clearly stated
```

**Time per paper:** 15-30 minutes (varies with complexity)

---

## **APPENDIX B: Common Abbreviations**

- **DD%:** Degree of Deacetylation
- **MW:** Molecular Weight
- **TPP:** Tripolyphosphate (sodium tripolyphosphate)
- **DLS:** Dynamic Light Scattering
- **TEM:** Transmission Electron Microscopy
- **PDI:** Polydispersity Index
- **rpm:** Revolutions per minute
- **ZOI:** Zone of Inhibition
- **MIC:** Minimum Inhibitory Concentration

---

## **END OF SOP**

**Version:** 1.0  
**Last Updated:** October 27, 2025  
**Contact:** [Your Email]  

**This SOP should be updated as you encounter new scenarios or challenges during extraction.**
Of course. I will provide a comprehensive report detailing the systematic process I have followed so far for extracting and predicting the data according to your specified schema.

This report is designed to give you full transparency into my methodology.

Report on Data Extraction & Prediction Methodology

Project Goal: To systematically extract a detailed, 25-column dataset from scientific articles involving chitosan and cobalt nanoparticle synthesis, with the ultimate aim of creating a complete database for an AI/ML project.

My Role: To act as a scientific data-extraction assistant, adhering strictly to the provided schema and rules, and to fill all data fields through direct extraction, calculation, or scientifically-informed prediction.

Part 1: The Data Extraction Workflow

For each PDF provided, I follow a multi-step process to locate and process the required information.

Step 1: Document Ingestion and Text Recognition
The PDF is first processed to make its entire text content machine-readable. This includes text in paragraphs, tables, figure captions, and footnotes. This step is equivalent to a human reading the entire paper from start to finish.

Step 2: Schema-Guided Keyword and Context Search
I use the 25-column schema you provided as a "search template". For each column, I scan the document for specific keywords and numerical values within relevant contexts.

Bibliographic Data (DOI, Year, Journal): Extracted directly from the paper's header, footer, or metadata. This is the most straightforward step.

Application & Method (Synthesis_Application, Synthesis_Method): I scan the Abstract, Introduction, and Conclusion sections for keywords like "biomedical," "catalytic," "environmental," "adsorption," "co-precipitation," "hydrothermal," etc., to categorize the paper's focus and synthesis technique.

Reactant Information (Chitosan_Conc, Cobalt_Source, Cobalt_Conc): I focus on the "Materials and Methods" or "Experimental" section.

I search for "chitosan," followed by numbers and units like "mg/mL," "wt%," or "%".

For Cobalt, I look for chemical formulas (e.g., CoCl₂, Co(NO₃)₂·6H₂O, CoFe₂O₄) and associated concentrations (mM, M, g, wt%).

Synthesis Conditions (pH, Temperature, Time, Stirring_Speed): I again search the "Experimental" section for phrases like "adjusted to pH," "heated at," "stirred for," "rpm," "room temperature," etc.

Characterization Data (Particle_Size, PDI, Zeta_Potential): I primarily search the "Results and Discussion" and "Characterization" sections, as well as figure captions (e.g., "TEM image showing particles of...", "DLS results indicated a PDI of...").

Step 3: Unit Normalization and Calculation
This is a critical step to ensure database consistency.

Concentration: If a paper states "2% chitosan solution," I convert it to 20 mg/mL (assuming a solvent density of 1 g/mL). If a cobalt salt concentration is given in Molarity (M), I convert it to millimolar (mM) (e.g., 0.1 M becomes 100 mM).

Time: If time is given in minutes (e.g., "120 min"), I convert it to hours (2 hr).

Chitosan-Cobalt Ratio: This is almost never directly reported. I calculate it from the stated weights or molar amounts of chitosan and the cobalt source used in the synthesis.

Particle Size: If a range is given (e.g., "40-50 nm"), I take the average (45 nm).

Part 2: The Prediction and Inference Methodology

When a value is not explicitly reported, I am programmed to infer it based on scientific principles, standard laboratory practices, and contextual clues from the paper. Every predicted value is marked with (pred) for transparency.

Here is a step-by-step breakdown of my prediction logic for each type of parameter:

1. Chitosan Properties (Chitosan_MW_kDa, Chitosan_DD_percent)

Reasoning: Authors often purchase commercial chitosan and may only state "low molecular weight" or "medium molecular weight" without giving exact numbers.

Prediction Logic:

MW: Standard commercial "low molecular weight" chitosan falls in the 50–190 kDa range. I use 150 kDa (pred) as a standard representative value that balances viscosity and reactivity for nanoparticle synthesis.

DD: For biomedical or chemical synthesis applications, a high degree of deacetylation is required for solubility and functional groups. The common commercial grade is ≥75% or ≥85%. I use 85% (pred) as a default for high-quality synthesis.

2. Synthesis Conditions (pH, Temperature_C, Stirring_Speed_rpm)

Reasoning: These are often considered "standard practice" and are sometimes omitted.

Prediction Logic:

pH: The chemistry of chitosan dictates that it is only soluble in acidic solutions. If a paper mentions dissolving chitosan in acetic or hydrochloric acid, the pH must be acidic. A pH of 5.0 (pred) is a standard, optimal value for this process.

Temperature: If not mentioned, the default assumption is "room temperature," which I standardize as 25 °C (pred). If a method like "reflux" is mentioned with a solvent like ethanol, I predict a temperature just below the boiling point (e.g., 80 °C (pred)).

Stirring Speed: This is very rarely reported. For a standard lab-scale synthesis in a beaker, vigorous stirring is needed for homogeneity. I use 400-600 rpm (pred) as a typical speed for a magnetic stirrer.

3. Characterization Properties (Size_SD_nm, PDI, Zeta_Potential_mV)

Reasoning: These depend on the quality of the synthesis but often follow predictable patterns based on the chemistry involved.

Prediction Logic:

Size Standard Deviation (SD): If a paper shows a TEM image that appears uniform or gives a tight size range, I predict a small SD, such as 3 nm (pred) or 5 nm (pred).

Polydispersity Index (PDI): This is a direct measure of size uniformity from DLS. A value < 0.5 is considered relatively monodisperse. For syntheses that report uniform particles in TEM/SEM images but omit the PDI, I predict a value in the range of 0.2-0.4 (pred).

Zeta Potential: This is predicted based on the surface chemistry.

Chitosan-coated particles at acidic/neutral pH: The amine groups (-NH₂) on chitosan become protonated (-NH₃⁺), creating a positive surface charge. This is fundamental. Therefore, I will always predict a positive value, typically +30 mV (pred) or +35 mV (pred), which indicates good colloidal stability driven by electrostatic repulsion.

Negatively charged particles: For a material I know to be negative (like bare cobalt ferrite in some conditions), I would predict a negative value (e.g., -20 mV (pred)).

4. Reactant Details (Crosslinker_Conc_mM, Ionic_Strength_mM)

Reasoning: These are derived from other reported values.

Prediction Logic:

Crosslinker: If a crosslinker like glutaraldehyde is mentioned but its concentration is not, I can sometimes calculate it if the volume and molarity of the stock solution are provided. If not, a prediction is made based on common literature ratios relative to the polymer. If no crosslinker is mentioned, the value is 0.

Ionic Strength: This is rarely reported. It is primarily influenced by the acid used to dissolve the chitosan and any salts present. For a typical 1% acetic acid solution, the ionic strength is low. I predict a value like 10-50 mM (pred). If high concentrations of precursor salts are used (e.g., 0.1 M CoCl₂), the ionic strength will be much higher, and I predict accordingly (e.g., 500+ (pred)).

By combining these direct extraction, calculation, and inference steps, I can successfully populate all 25 fields of your data schema, ensuring a complete and logically consistent dataset ready for your analysis.
