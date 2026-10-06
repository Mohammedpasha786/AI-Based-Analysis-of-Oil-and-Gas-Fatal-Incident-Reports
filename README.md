# AI-Based-Analysis-of-Oil-and-Gas-Fatal-Incident-Reports

A MATLAB workflow that turns a published IOGP fatal incident report from PDF into a structured table, visualizations, a keyword classification measured against the report's own CAUSE field, and a count of People (acts) versus Process (conditions) causal factors.
Submission for the MathWorks Challenge Project Hub: AI-Based Analysis of Oil and Gas Fatal Incident Reports.
Workflow
```
report PDF -> extractFileText -> split at DATE -> regexp fields + region/onshore/offshore headings
-> check totals -> tokenizedDocument cleaning -> bagOfWords -> figures
-> keyword theme vs CAUSE (confusionchart) -> PEOPLE/PROCESS factor counts -> CSV + report
```
Repository structure
```
main.m                      single entry point (one-click run)
src/                        pipeline functions
data/sample/                synthetic sample report and country coordinates (no download needed)
data/reports/               place the real IOGP PDF here
models/                     reserved for trained models (none used in the basic workflow)
tests/IOGPPipelineTest.m    unit and end-to-end tests
docs/usage.md               configuration, real-report run, outputs
results/                    generated table, figures, and report
```
Setup
MATLAB R2024a or later with the toolboxes listed below. No add-ons are required.
Optional, to analyse the real report: download Safety performance indicators - 2025 data: fatal incident reports from the IOGP bookstore, save the PDF in `data/reports/`, and follow `docs/usage.md`.
Run
```matlab
main
```
This reads the bundled sample report, extracts the incidents, checks the totals, builds nine captioned figures, and writes `results/incidents.csv`, `results/keyword_errors.csv`, `results/figure_captions.csv`, `results/fig01...fig09.png`, and `results/report.md`.
To submit as a Live Script, open `main.m`, Save As `main.mlx`, run it, and export to PDF or HTML.
Test
```matlab
runtests("tests")
```
Expected output on the sample data
Check	Expected
Incidents / fatalities parsed	14 / 17, matching the report's stated totals
Keyword classification vs reported CAUSE	13 of 14 (93%); the miss is the Australia dropped-basket lift, where "supply vessel" triggers the Confined space keyword
People / Process factors	23 / 35
Attribution labels	3 People-dominant, 8 Process-dominant, 2 Mixed, 1 Not allocated
Most frequent People factor	Worker positioned in the line of fire (3)
Most frequent Process factor	Inadequate supervision (3)
Each figure has a one-sentence claim in `results/figure_captions.csv` and `results/report.md`. The two dropped-load fatalities in Africa and Australia, and the vehicle-and-fire collision in Saudi Arabia, are included to exercise duplicate-incident and multi-theme cases.
What the figures show
Word cloud of the cleaned narratives.
Bar chart of the most frequent narrative words.
Word clouds for the four deadliest causes in a tiled layout.
Fatalities by cause and by activity, sorted.
Heatmap of cause against activity.
Geobubble map of fatalities by country.
Stacked bars of onshore against offshore fatalities by region.
Confusion chart of keyword theme against reported cause.
Stacked bars of People and Process factors by cause and by activity.
Dependencies
Item	Needed for
MATLAB R2024a+	Everything
Text Analytics Toolbox	`extractFileText`, `tokenizedDocument`, `bagOfWords`, `wordcloud`, `topkwords`
Statistics and Machine Learning Toolbox	`groupsummary`, `confusionchart`
Limitations
The bundled report is synthetic text written to match the field labels in the project brief. It is not an IOGP document, and the numbers above describe it, not real incidents.
The parser has not yet been run on the real IOGP PDF. `extractFileText` can alter headings and bullets, so adjust the lists in `src/iogpConfig.m` if the totals check fails. Layout also varies between years.
Keyword rules are deliberately simple. Substring matching confuses words such as "vessel" (process vessel versus supply vessel).
Factor counts show how investigators wrote up each case. They do not prove cause, which `results/report.md` discusses.
The advanced tracks and the multi-year extension are not implemented.
Data sources
IOGP bookstore: https://www.iogp.org/bookstore/. Aggregate indicators: https://data.iogp.org.
License
MIT, see `LICENSE`.
Contact
MD. Afreed Pasha, SR University, Warangal. GitHub: Mohammedpasha786
