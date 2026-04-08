# Power Query M — ETL Pipeline

This script connects directly to the SharePoint folder, detects all available Deepdive .xlsm files, extracts the Snapshot_Period variable from each filename and consolidates all snapshots into a single longitudinal dataset.

## Query1 — Main ETL Pipeline

```powerquery
let
    Source = SharePoint.Files("https://share.lighting.com/teams/SSEurope-ProposalProjectManagement", [ApiVersion = 15]),
    #"Filtered Rows" = Table.SelectRows(Source, each Text.Contains([Folder Path], "Shared Documents/General/03. Project Management/08. Subteams/1. North East/098 Finance")),
    #"Filtered Rows1" = Table.SelectRows(#"Filtered Rows", each Text.Contains([Name], "deep dive")),
    #"Filtered Hidden Files1" = Table.SelectRows(#"Filtered Rows1", each [Attributes]?[Hidden]? <> true),
    #"Added Snapshot_Period" = Table.AddColumn(#"Filtered Hidden Files1", "Snapshot_Period", each 
        let
            dateStr = Text.Start([Name], 8),
            year  = Number.FromText(Text.Start(dateStr, 4)),
            month = Number.FromText(Text.Middle(dateStr, 4, 2)),
            day   = Number.FromText(Text.Middle(dateStr, 6, 2))
        in
            #date(year, month, day), type date),
    #"Invoke Custom Function1" = Table.AddColumn(#"Added Snapshot_Period", "Transform File", each #"Transform File"([Content])),
    #"Renamed Columns1" = Table.RenameColumns(#"Invoke Custom Function1", {"Name", "Source.Name"}),
    #"Removed Other Columns1" = Table.SelectColumns(#"Renamed Columns1", {"Source.Name", "Snapshot_Period", "Transform File"}),
    #"Expanded Table Column1" = Table.ExpandTableColumn(#"Removed Other Columns1", "Transform File", Table.ColumnNames(#"Transform File"(#"Sample File")))
in
    #"Expanded Table Column1"
```

## Key Logic

- **Source:** SharePoint folder containing all weekly Deepdive .xlsm files
- **Filter:** Only files containing "deep dive" in the filename
- **Snapshot_Period:** Extracted from the first 8 characters of each filename (YYYYMMDD format) and converted to date type
- **Consolidation:** All files are expanded and merged into a single table
- **Output:** Longitudinal panel dataset with Snapshot_Period appended to every row
