#power-query #power-bi #airtable #api 

Power Query to get data from Airtable via API

```
let Pagination = List.Skip(List.Generate( () => [Page_Key = "init", Counter=0],

  each [Page_Key] <> null, // Condition under which the next execution will happen
  each [
    Page_Key = try if [Counter]<1 then ""
    else
	    // determine the LastKey for the next execution
	    WebCall][Value][offset] otherwise null, 
	    WebCall = try if [Counter]<1
    then
      Json.Document(
	      Web.Contents(
		      "https://api.airtable.com",
		      [
			      RelativePath="v0/"&BASE_ID&"/"&TABLE_ID,
			      Headers=[Authorization="Bearer "&API_KEY]
			]
		)
	)
    else
      Json.Document(
	      Web.Contents(
		      "https://api.airtable.com",
		      [
			      RelativePath="v0/"&BASE_ID&"/"&TABLE_ID&
			      "?offset="&[WebCall][Value][offset], 
			      Headers=[Authorization="Bearer "&PERSONAL_ACCESS_TOKEN]])),
				    Counter = [Counter]+1
			],
  each [WebCall]
),1),
   #"Converted to Table" = Table.FromList(
     Pagination, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"HasError", "Value"}, {"HasError", "Value"}),
    #"Expanded Value" = Table.ExpandRecordColumn(#"Expanded Column1", "Value", {"records", "offset"}, {"records", "offset"}),
    #"Expanded records" = Table.ExpandListColumn(#"Expanded Value", "records"),
    #"Expanded records1" = Table.ExpandRecordColumn(#"Expanded records", "records", {"id", "createdTime", "fields"}, {"id", "createdTime", "fields"})
    
in
	#"Expanded records1"

```
