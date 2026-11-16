# Notebook generation

Before carrying out this workflow, complete the shared **Source discovery and selection** step in `AGENTS.md`. Use only the sources the user confirms there.

Purpose: generate clear, well-documented Jupyter notebooks that answer the user's question with data from the approved data sources.

## Sources
- Include BeaconIHM as a candidate source for notebook requests. Optionally include beaconwod and beaconargo when environmental data is relevant to the question (e.g. water quality, temperature, salinity, tides, weather), starting with WADAR and RWS_CTD. Use only the sources the user confirms in the shared Source discovery and selection step. If the user excludes a source needed to answer the question, explain the limitation and do not silently use it.
- When questions are asked about medegebruik windmolenpark, use the reference material in `references/medegebruik/Medegebruik_windmolens.md`; disclose this reference in notebook section 2.2 and ask before using it if the user has not already authorized it.
- Other external information sources (public datasets, APIs, documents) may be searched or used only after the user explicitly permits external sources. Disclose them in notebook section 2.2 with source, date and limitations, and keep them clearly separate from data lake data.

## Delivery
- Save every generated `.ipynb` notebook in `notebooks/`. Deliver the actual notebook file, not only notebook code or a chat answer. If creating or attaching a notebook is not possible, state that limitation clearly and provide the closest usable alternative.
- Answer the user's question in the notebook, grounded in data actually retrieved or verified through MCP. If the data is unavailable or cannot be verified, still deliver a Dutch notebook that documents what was checked, what could and could not be established, and why. Never invent data, sources, calculations or results.
- **Data in the notebook.** The agent retrieves the data via MCP, saves the extract as data file(s) delivered together with the notebook, and the notebook loads from those files. Document in notebook section 2.3 which MCP tool and query/filter were used and when the extract was made, so that the extract can be reproduced. If the notebook environment can call the MCP data source directly, that is allowed, but the extract must still be documented.
- **Execution.** Where possible, execute the notebook and deliver it with saved outputs. Report honestly whether it was executed successfully, partially executed, or only structurally reviewed (see notebook section 10). Never describe an unexecuted notebook as fully validated.

## Principles
The audience is business users without a technical or data science background. Every notebook must explain what is being done, why, which data is used and how to interpret the results.

- Use clear, accessible Dutch. Explain unavoidable technical terms when they first appear.
- Distinguish facts from the data from assumptions, estimates and interpretations.
- Explain the purpose of each major analytical step before its code or results.
- Prioritize readability, transparency and reproducibility over technical complexity.
- Do not start the analysis or show plots until the business context, data sources, extracted data, external data and assumptions have been explained.
- Never imply that a relationship between variables proves causation.

## Proportionality
Keep all ten notebook sections and their Dutch headings, in the specified order. For simple questions, sections may be short (a few sentences) and the notebook may be compact, but the mandatory statements in sections 2.2 and 4.3, the explanation of every chart, and the limitations and execution status must always be present.

## Mandatory notebook structure
Every notebook contains the following sections, with these Dutch headings, in this order. (Refer to them as "notebook section 1-10", not as chapters of this document.)

### Notebook section 1: "Overzicht en doelstelling"
Explain what the notebook investigates, why it is useful, which business question it addresses, what the reader will learn, which results, comparisons, trends or insights will be produced, and what the analysis can and cannot conclude. Write for a reader who has not seen the notebook before, in a few coherent paragraphs. Do not begin with code, queries, calculations, tables or plots. Avoid generic descriptions: explain what this specific notebook does and why.

### Notebook section 2: "Databronnen en gebruikte data"
Explain all sources before the analysis.

**2.1 "Bronnen in de data lake".** For every source, explain in accessible Dutch: the name or identifier of the source, table, dataset or file; what it contains and represents; why it is relevant; which fields or measures are extracted; the period covered; the filters applied (date ranges, categories, regions, other criteria); and how it contributes to the analysis. Explain each source separately. For environmental data focus first on WADAR and RWS_CTD. If sources are combined, explain why and which relationship connects them. Never guess source names, columns, schemas or definitions; use MCP metadata and identify what could not be verified.

**2.2 "Externe data en informatie".** State explicitly whether information from outside the data lake is used (public APIs, external files or databases, public datasets, manually supplied reference values, websites or documents, third-party data). If so, explain where it comes from, what is obtained, why it is needed, how it is combined with or compared to data lake data, and limitations, dates or differences in coverage. Keep external and internal data clearly separate. If none is used, state exactly:

> "Er zijn geen externe databronnen gebruikt. Deze analyse is gebaseerd op de hierboven beschreven bronnen in de data lake."

Only use this statement if it has been established from the notebook's actual data dependencies and operations.

**2.3 "Data-extractie en afbakening".** Explain what data is actually retrieved and used, not merely what is available: records, entities and periods; filters and exclusions; all records or a subset; aggregation during extraction; limitations on representativeness; the MCP tool and query used and the time of extraction (see Delivery). Distinguish discovery from actual extraction. Report record counts and date ranges only after they were retrieved or calculated successfully.

### Notebook section 3: "Aannames, datakwaliteit en voorbereiding"
**3.1 "Aannames".** Document all assumptions that could influence results (missing values, record uniqueness, unit harmonisation, interpolation, matching of sources, reporting periods, category definitions, representativeness, incomplete documentation). For each important assumption, explain why it is needed and how it might affect the results. Distinguish user-supplied assumptions from assumptions introduced during the analysis. Do not silently make material assumptions; if one could substantially change the conclusions and cannot reasonably be determined, ask the user.

**3.2 "Datakwaliteit".** Explain relevant quality issues (missing or incomplete data, duplicates, unexpected values, inconsistent formats or categories, gaps in time series, inconsistent definitions) and what was checked and done about them. Do not imply the data is complete or correct merely because it was retrieved. If no material issues are found, describe the checks performed rather than claiming the data is perfect.

**3.3 "Dataverwerking en transformaties".** Explain each material change made to the original data (combining, de-duplicating, filtering, converting dates/units/categories, aggregating, calculating new measures, excluding records), what changed and why, with the focus on business meaning rather than implementation.

### Notebook section 4: "Interpolatie, schatting en omgang met ontbrekende data"
Mandatory, even when nothing is estimated.

**4.1 "Interpolatie".** If values are interpolated (estimated between known observations), explain which data, why, which method, which periods are affected, what pattern the method assumes (gradual, linear, other) and how it might affect conclusions. Label interpolated values as estimates, never as observations.

**4.2 "Overige schattingen en invulmethoden".** Explain any other filling or estimation (averages, forward/backward fill, extrapolation, model-based estimates) in plain Dutch, with limitations. Do not use these only to make a dataset look complete.

**4.3 "Geen interpolatie of schatting".** If nothing is interpolated or estimated, state exactly:

> "Er is geen interpolatie of schatting toegepast. De analyse gebruikt waargenomen waarden uit de genoemde bronnen, met inachtneming van de hierboven beschreven filters en bewerkingen."

Only use this statement if it is accurate. Explain separately how missing values were excluded, retained or otherwise handled without estimation.

### Notebook section 5: "Analyseaanpak"
Only after sections 1-4. Explain what will be calculated, compared or investigated; why this approach fits the business question; which measures or indicators are used; how results will be interpreted; and the method's limitations. Break complex analyses into logical stages and add a short Markdown explanation before each major step. No code without context.

### Notebook section 6: "Analyse en resultaten"
Perform the analysis on the prepared data. Use logical subsections; introduce each major calculation or comparison in Dutch; use descriptive names for measures and outputs; explain important results immediately after presenting them; distinguish observed results from interpretations or hypotheses; use appropriate units, labels and periods; explain joins, aggregations and statistics by their practical meaning. Never fabricate results or claim code was executed unless it was.

### Notebook section 7: "Visualisaties en interpretatie"
Create plots only after data, assumptions and approach are explained. Every visualization has: a descriptive title; labelled axes with units; a legend when there are multiple series; dates, filters or scope where needed; a short Markdown explanation of what it shows; an interpretation of the key pattern, difference or trend; and caveats where it could mislead. Prefer simple, readable charts (line for change over time, bar for comparisons, scatter for relationships). Avoid decoration, overcrowded charts, unexplained abbreviations and too many plots. Clearly distinguish estimated or interpolated values from observed values.

### Notebook section 8: "Validatie en betrouwbaarheidscontroles"
Validate data and results before concluding. Where relevant check: whether the extract matches the intended scope; whether record counts and totals are plausible; whether joins create duplicates or lose records; whether measures follow their definitions; whether assumptions hold; whether estimated values are identified correctly; whether results reconcile with source data or independent calculations. Explain material outcomes in plain Dutch. Report failed checks and unresolved issues transparently; never change assumptions silently to force a check to pass.

### Notebook section 9: "Belangrijkste bevindingen en betekenis voor de organisatie"
Summarize the most important findings for a non-technical audience: what was found, which patterns matter most, why they may be relevant, what the data supports, and what remains uncertain. Distinguish evidence from interpretation. Give recommendations only when supported by the analysis and appropriate to the objective. Do not repeat every result.

### Notebook section 10: "Beperkingen en vervolgstappen"
Explain limitations (missing or incomplete data, restricted time coverage, influential assumptions, external dependencies, estimated values, unverified interpretations, no proof of causation, checks that could not be completed) and suggest useful next steps. State whether the notebook was executed successfully, partially executed, or only structurally reviewed.

## Writing style
Applies to every Markdown cell, table, chart explanation and conclusion.
- Write accessible Dutch for business users; short paragraphs with clear headings; familiar Dutch words over jargon.
- Explain why a step matters, not just what the code does. Keep explanations informative but concise and do not repeat the same explanation in several sections.
- Use tables where they make sources, assumptions or findings easier to compare.
- Put implementation details in code comments only when needed.
- Never make the reader infer where data came from, what was changed or what a chart means.

## Final review before delivering a notebook
Verify that:
1. All ten sections are present, with the Dutch headings, in order.
2. The overview explains purpose and approach before any technical analysis.
3. Data lake sources and extracted information are documented, including the MCP tool, query and extraction time.
4. External information is disclosed, or its absence is accurately stated (section 2.2).
5. Assumptions, data quality and preparation are explained.
6. Interpolation, imputation, estimation and missing-data treatment are explicitly addressed (section 4).
7. Every major analytical step has a plain-Dutch introduction.
8. Every visualization has an explanation and interpretation.
9. Results and conclusions are supported by actual data and calculations.
10. Limitations and validation outcomes are documented.
11. The notebook is readable by a non-technical business user.
12. Execution and validation status are reported honestly.

If a requirement is unmet, correct the notebook before delivering it where possible; otherwise identify the outstanding issue clearly.

The final notebook must tell a coherent story: what the analysis is about, where the data comes from, what assumptions and changes were made, what was analyzed, what was found, and what the reader should take away.
