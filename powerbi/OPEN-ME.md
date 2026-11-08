# Opening the Power BI report

`SchoolsNSWSA.pbip` is a Power BI Project: the report (`SchoolsNSWSA.Report`, PBIR format) and the semantic model (`SchoolsNSWSA.SemanticModel`, TMDL) saved as text, so the repo shows every change. Power BI Desktop opens it like a `.pbix`.

The report has three pages, in this order:

1. **NSW**: inner Sydney government schools on their own.
2. **SA**: inner Adelaide government schools on their own, with the same visuals in the same places as the NSW page.
3. **NSW and SA**: the two side by side, with the state as the legend (NSW blue, SA ochre).

Every page has an **Enrolment year** slicer, a dropdown that takes one year at a time and starts on 2024; it sets the cards and the school tables, and the measures always count one year (the latest, 2024, if the slicer is ever cleared). The NSW page is filtered to NSW and the SA page to SA (a page filter). The NSW and SA page also has a **State** dropdown that starts with nothing picked, which shows both; pick one to see it alone. The maps always show 2024, and the state-wide lines (spend, attendance, retention) show every year.

## What you need

- Power BI Desktop for Windows, a recent version (2026). Projects and the PBIR report format are standard in current versions. If Desktop is older than about mid 2025, turn on **File, Options and settings, Options, Preview features, Power BI Project (.pbip) save option** and **Store reports using enhanced metadata format (PBIR)**, then restart Desktop.
- The repo folder on the Windows machine. In Parallels the Mac home folder is shared, usually as `\\Mac\Home`, so a clone in your home folder is at `\\Mac\Home\plotlay-schools-nsw-sa` (or wherever you cloned it).

## Open it

1. In Windows, open File Explorer and go to the repo's `powerbi` folder.
2. Double-click `SchoolsNSWSA.pbip`. If Windows asks which app, choose Power BI Desktop. Or in Desktop: **File, Open report, Browse reports**, set the file type to *Power BI project files (\*.pbip)* and pick it.
3. The report opens with empty visuals the first time, because nothing has been loaded yet. Select **Refresh** on the Home ribbon.
   - By default the data is read from the repo's public copy on GitHub (`DataFolder` is `https://raw.githubusercontent.com/cjmuldoon/plotlay-schools-nsw-sa/main/data/`). If Desktop asks how to connect to that address, choose **Anonymous** and **Connect**.
   - To read the files from the folder on disk instead (no internet, or before the repo is public): **Home, Transform data, Edit parameters**, set `DataFolder` to the full path of the repo's `data` folder **ending with a backslash**, for example `\\Mac\Home\plotlay-schools-nsw-sa\data\`, then **OK** and **Apply changes**. Power BI can't use a path relative to the project file, so this one parameter is the only place a folder is named.
   - If Desktop warns about privacy levels, choose **Ignore privacy levels** for this file (all the sources are public open data) or set the sources to Public.
4. Check the numbers (below).

## Check the numbers after Refresh

After **Refresh**, with the **Enrolment year** slicer on **2024** (as the report opens; it takes one year at a time), the cards should read:

| Page | Schools compared | Median m² per student, primary | Median m² per student, secondary | Schools under 15 m² per student |
|---|---|---|---|---|
| NSW | 79 | 28.1 | 29.4 | 10 |
| SA | 51 | 42.4 | 34.4 | 3 |

These are the figures in `data/comparison/10_inner_city_summary.csv`. Pick **2019** in the year slicer and they change to:

| Page | Schools compared | Median, primary | Median, secondary | Under 15 m² |
|---|---|---|---|---|
| NSW | 79 | 21.9 | 29.5 | 12 |
| SA | 51 | 38.7 | 38.4 | 2 |

On the **NSW and SA** page (both states, 2024) the table reads a pooled 31.0 m² per student for NSW and 43.5 for SA, with 38,386 and 29,886 students in the compared schools. If a card is blank, Refresh has not finished or the data folder is wrong (step 3).

## How the model fits together

- `Schools` has one row per school for both states (`comparison/07_inner_city_schools.csv`). School codes are numbers that can be the same in NSW and SA, so every join uses `school_id`, made in Power Query as state, a hyphen and the code (`NSW-3305`, `SA-120`). The two NSW-only columns (`nsw_only_icsea`, `nsw_only_ram_per_student_2024`) are joined in from `nsw/07_inner_city_schools.csv` and are blank for SA schools.
- `Enrolments` (`comparison/08_...`) joins to `Schools` on `school_id` and to `Years` on `year`.
- `Spend`, `Outcomes` and `NAPLAN` come from the wide RoGS and ACARA tables (`01_...`, `05_...`, `06c_...`): Power Query keeps the NSW and SA columns and unpivots them into a `state` column. They are not joined to `Years`, so the year slicer leaves those lines alone.
- `States` (NSW, SA) filters everything: the slicers and legends use `States[state]`.
- Measures: `Students`, `Area m2`, `Students compared`, `Pooled m2 per student`, `m2 per student`, `Schools compared`, `Median m2 per student` (and its primary and secondary versions) and `Schools under 15 m2`. A school counts in the comparison when `in_comparison` is true and it has students in the year shown; its figure is `area_m2 / students`.

## The two maps

Each state page has a Shape Map of its schools. The custom map files are saved inside the report (`SchoolsNSWSA.Report/StaticResources/RegisteredResources/`: `sydney_outlines_from_plotlay.topojson` and `adelaide_outlines_from_plotlay.topojson`, copies of the files in `data/nsw/` and `data/sa/` that keep only the school code), so the outlines draw on open with no step by hand. The map matches `Schools[school_key]` to the code in each outline. The Shape Map has no base map of its own, so each file also carries the context: the 7 km study circle, suburb boundaries (ABS, CC BY 4.0) in a pale ground tone, the harbour and sea showing as the map's pale blue background, and a coloured ring round each school too small to see at city scale. Loading the plain file from `data/` by hand (below) gives the schools without that context.

If a map shows the United States instead (an older Desktop can ignore a saved map): select it, then **Format visual** (the paintbrush), **Visual**, **Map settings**, **Map type: Custom map**, **Add map type** (or **Browse**), and choose `..\data\nsw\sydney_outlines_from_plotlay.topojson` on the NSW page or `..\data\sa\adelaide_outlines_from_plotlay.topojson` on the SA page. Under **View map keys** the school codes (3305, 3935... and 120, 1881...) should be matched. Projection: Mercator.

The colours are fixed bins of m² of land per student in 2024: under 15, 15 to 30, 30 to 60, 60 or more, and grey for schools left out of the comparison. They are set per legend value, so both pages use the same colour for the same bin. If the Shape Map is not in the Visualizations pane at all, turn it on in **Options, Preview features, Shape map visual**. If a map comes up empty after opening (Power BI sometimes drops the fields of a map it can't draw yet), drag them back: Location `Schools[school_key]`, Legend `Schools[m2_band_label]`.

The NSW and SA page has no map: the two cities are about 1,160 km apart, so one map would show two dots. Its charts compare the same measures instead.

## Save a .pbix too

A `.pbix` is a single file anyone can open, but only Power BI Desktop can write one. After the refresh: **File, Save as**, set the file type to **Power BI file (\*.pbix)**, and save it as `powerbi\SchoolsNSWSA.pbix` next to the project. It includes the data, so it opens without a refresh.

## Publish (only if you choose to)

1. **Home, Publish**, sign in, pick a workspace.
2. In the Power BI service, open the semantic model's **Settings, Data source credentials**: the GitHub address uses **Anonymous**. Scheduled refresh then works with no gateway, because the data is read from the public GitHub copy.
3. **Publish to web** makes a public embed link. It is turned off in many organisations, and anyone with the link can see the report. Only use it if you intend the report to be public.

## What's in the folder

- `SchoolsNSWSA.pbip`: open this.
- `SchoolsNSWSA.SemanticModel/definition/`: `tables/*.tmdl` (columns, measures, Power Query), `relationships.tmdl`, `expressions.tmdl` (the `DataFolder` parameter and a small `GetFile` function).
- `SchoolsNSWSA.Report/definition/`: one folder per page and visual (PBIR), `report.json` (the Plotlay theme registered), `StaticResources/RegisteredResources/Plotlay_drafting_film.json` (the theme).
- `SHOT-LIST.md`: the screenshots to take for the showcase page and the video.
