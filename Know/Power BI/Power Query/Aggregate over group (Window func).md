
Example below is to find the latest records and add a flag, where there are multiple records with each `GroupKey` for example, with version control / merged history table, and no flag for current records, but an `updated on` date or similar.

```
let
    Source = #"Previous Step",

    // 1) Create a 2-col lookup: GroupKey -> MaxDate
    MaxDates =
        Table.Group(
            Source,
            {"GroupKey"},
            {{"MaxDate", each List.Max([Date]), type date}}
        ),

    // 2) Merge back onto original table
    Merged =
        Table.NestedJoin(
            Source,
            {"GroupKey"},
            MaxDates,
            {"GroupKey"},
            "MaxLkp",
            JoinKind.LeftOuter
        ),

    // 3) Expand MaxDate
    Expanded =
        Table.ExpandTableColumn(
            Merged,
            "MaxLkp",
            {"MaxDate"},
            {"MaxDate"}
        ),

    // 4) Flag latest rows
    AddFlag =
        Table.AddColumn(
            Expanded,
            "IsLatest",
            each if [Date] = [MaxDate] then 1 else 0,
            Int64.Type
        )
in
    AddFlag
    
    
```