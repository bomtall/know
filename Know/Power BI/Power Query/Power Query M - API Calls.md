#power-query #api #m #refresh #power-bi #data #query #load

### Related:
[[Get files]]

### API Call

To query APIs in Power Query, the base URL cannot be 'dynamic', if it is, it will refresh locally but not in the Power BI service. Therefore, do not construct the whole URL with runtime variables, but pass the base URL as the first parameter to the `web.contents()` function. Use variables to construct the relative path, and pass query parameters as comma separated key values.

**Example**

```
let
    FileBinary =
        Web.Contents(
            "https://dev.azure.com",
            [
                RelativePath =
                    Organization & "/" & Project &
                    "/_apis/git/repositories/" & Repository & "/items",
                Query = QueryParams,
                Headers = [
                    Authorization = AuthKey,
                    Accept = "application/octet-stream"
                ]
            ]
        )
in
    FileBinary
```

Full example inside a reusable Power Query function to get files from Azure DevOps via the API:

```
(
    Organization as text,
    Project as text,
    Repository as text,
    FilePath as text,
    PAT as text,
    optional Branch as nullable text
) as binary =>
let
    ApiVersion = "7.1-preview.1",

    AuthKey =
        "Basic " &
        Binary.ToText(
            Text.ToBinary(":" & PAT),
            BinaryEncoding.Base64
        ),

    QueryParams =
        if Branch <> null then
            [
                path = FilePath,
                download = "true",
                versionDescriptor.version = Branch,
                versionDescriptor.versionType = "branch",
                #"api-version" = ApiVersion
            ]
        else
            [
                path = FilePath,
                download = "true",
                #"api-version" = ApiVersion
            ],

    FileBinary =
        Web.Contents(
            "https://dev.azure.com",
            [
                RelativePath =
                    Organization & "/" & Project &
                    "/_apis/git/repositories/" & Repository & "/items",
                Query = QueryParams,
                Headers = [
                    Authorization = AuthKey,
                    Accept = "application/octet-stream"
                ]
            ]
        )
in
    FileBinary
```


### Iterate over a table

```
Table.AddColumn(
    #"Previous Step",
    "Column Name", 
    each fnFunctionNameForAPICall([Columns-for-arguments])
```