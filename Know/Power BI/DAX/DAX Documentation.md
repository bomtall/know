Use DAX to query tables, columns, measures, and relationships in a Power BI report and create a table of those objects to serve as documentation of the report and contents. 


```
Documentation = VAR _columns =

                    SELECTCOLUMNS(

                        FILTER(

                            INFO.VIEW.COLUMNS(),

                            [Table] <> "Model Documentation" && NOT([IsHidden])

                        ),

                        "Type", "Column",

                        "Name", [Name],

                        "Description", [Description],

                        "Location", [Table],

                        "Expression", [Expression],

                        "Data Type", [DataType],

                        "Key", [IsKey]

                    )

                VAR _measures =

                    SELECTCOLUMNS(

                        FILTER(

                            INFO.VIEW.MEASURES(),

                            [Table] <> "Model Documentation" && NOT([IsHidden])

                        ),

                        "Type", "Measure",

                        "Name", [Name],

                        "Description", [Description],

                        "Location", [Table],

                        "Expression", [Expression],

                        "Data Type", BLANK(),

                        "Key", BLANK()

                    )

                VAR _tables =

                    SELECTCOLUMNS(

                        FILTER(

                            INFO.VIEW.TABLES(),

                            [Name] <> "Model Documentation" && [Name] <> "Calculations" && NOT([IsHidden])

                        ),

                        "Type", "Table",

                        "Name", [Name],

                        "Description", [Description],

                        "Location", BLANK(),

                        "Expression", [Expression],

                        "Data Type", BLANK(),

                        "Key", BLANK()

                    )

                VAR _relationships =

                    SELECTCOLUMNS(

                        INFO.VIEW.RELATIONSHIPS(),

                        "Type", "Relationship",

                        "Name", [Relationship],

                        "Description", BLANK(),

                        "Location", BLANK(),

                        "Expression", [Relationship],

                        "Data Type", BLANK(),

                        "Key", BLANK()

                    )

                RETURN

                    UNION(_columns, _measures, _tables, _relationships)
```


