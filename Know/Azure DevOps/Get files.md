#api

[[Power Query M - API Calls]]

## Power Query

**Function:**

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


**Query:**

```
let
    // Call the function
    FileBinary =
        fnGetFileFromDevOps(
            DevOpsOrganisation,
            DevOpsProject,
            "DevOpsTeam",
            "/Path/To/File.json",
            AzureDevOpsPAT,
            "main"
        ),

    // Parse JSON
    FileJson = Json.Document(FileBinary),

    // Convert to table
    #"Converted to Table" =
        Table.FromList(
            FileJson,
            Splitter.SplitByNothing(),
            null,
            null,
            ExtraValues.Error
        )
in
    #"Converted to Table"
```


