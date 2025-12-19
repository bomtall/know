#pill #DAX #SVG #data-visualisation

Create a new column in Power BI using DAX that takes a categorical column in the data table and creates SVG *pills* with colours dependent on the category. Adjust the sizing values to suit.

```
State Type Pill Label =

VAR TextValue = [State]

VAR Color =

    SWITCH(

        TRUE(),

        TextValue = "New", "#CFEBD7",

        TextValue = "Active", "#AF314926",

        TextValue = "Resolved", "#FAE6D6",

        TextValue = "Closed", "#FAE6D6",

        TextValue = "Removed", "#FAE6D6",

        "#95A5A6"

    )

-- dynamic pill width based on text length

VAR TextLength = LEN(TextValue)

VAR Width = 400

VAR Height = 100

VAR Radius = 60

VAR TextY = (Height / 2) + 12    -- centers the text vertically

VAR SVG =

    "data:image/svg+xml;utf8," &

    "<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 " & Width & " " & Height & "' preserveAspectRatio='xMidYMid meet'>

        <rect x='0' y='0' rx='" & Radius & "' ry='" & Radius & "' width='" & Width & "' height='" & Height & "' fill='" & Color & "' />

        <text x='" & Width / 2 & "' y='" & TextY & "' font-family='arial' font-size='50' fill='#333333' text-anchor='middle'>" &

            TextValue &

        "</text>

    </svg>"

RETURN

    SVG
```

