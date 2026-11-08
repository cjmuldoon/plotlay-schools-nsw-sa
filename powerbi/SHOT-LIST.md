# Shot list for the showcase page and the video

Capture in Power BI Desktop at 1920 × 1080 (Windows display scaling 100%), with the Plotlay theme on, after the refresh (see `OPEN-ME.md`). In Parallels, use the Windows Snipping Tool (Win+Shift+S) for stills and Win+Alt+R (Xbox Game Bar) or Clipchamp for a clip. Save them to `powerbi/Screenshots/` with the file names below.

Close the Filters, Visualizations and Data panes before a shot (**View**, untick them) so the page fills the window. Keep the footer with the data credits in every shot, except the close crops in shots 3 and 6, which go next to a full-page shot on the site.

| # | File | Page | What is selected | Crop | Length |
|---|---|---|---|---|---|
| 1 | `01-nsw-page.png` | NSW | Year 2024, nothing clicked | The whole page, footer included | still |
| 2 | `02-sa-page.png` | SA | Year 2024, nothing clicked | The whole page, footer included (the same frame as shot 1, so the two can be swapped side by side) | still |
| 3 | `03-maps-pair.png` | NSW, then SA | Year 2024. Hover nothing | The map visual only (x 40 to 860, y 265 to 1010 on the page), one crop per state, set side by side later; the legend must show in both | 2 stills |
| 4 | `04-side-by-side.png` | NSW and SA | State slicer empty (both states), year 2024 | The whole page, footer included | still |
| 5 | `05-year-stepping.mp4` | NSW and SA | Pick 2015, 2018, 2020, 2022, 2024 in the year dropdown, about 1 s each; the summary table and the level chart change, the 2015 to 2024 line stays put | The top half of the page (y 0 to 420) | 6 to 8 s clip |
| 6 | `06-nsw-tight-school.png` | NSW | Year 2024. Click the top row of the Compared schools table (the least m² per student): the cards and the map cross-filter to it | The cards, the map and the table (x 40 to 1520, y 100 to 1010) | still, then a 3 s clip of the click |
| 7 | `07-model-view.png` | Model view | Nothing: the seven tables (States, Schools, Enrolments, Years, Spend, Outcomes, NAPLAN) and their relationships, laid out so `school_id` shows on the Schools to Enrolments line | The diagram only | still |

Checks before saving each shot:

- The cards on the NSW page read 79, 28.1, 29.4 and 10, and on the SA page 51, 42.4, 34.4 and 3 (year 2024). Any other figures mean the refresh or the year slicer is wrong.
- The map bins use the same colours on both pages: darkest red under 15 m², then 15 to 30, 30 to 60, palest 60 or more, grey for schools left out.
- Each line chart's axis is set to its data, so the lines fill the chart; the x axes stop at the last year with data.
- On the NSW and SA page, NSW is blue and SA ochre in every chart.
- No satellite or street basemap appears anywhere: the Shape Maps draw only the outlines.
