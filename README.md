# APIcommunicateiondocumentation# Table of Content
1. [Conventions to define the API endpoints](#Conventions-to-define-the-API-endpoints)
2. [JSON Format for Response API](#Json-Format-For-Response-Api)
2. [JSON Format for Meta Data API](#json-format-for-meta-data-api)

# Conventions to define the API endpoints
Every endpoint will define 2 mandatory and one optional end point for each API.

    1. The first end point will have a unique PATH (e.g. http://www.example.com/JDA/v1/exceptions). This end point will responsible to return the data associated with this API
    
    2. The second end point will have the unique PATH suffixed with /metadata (e.g. http://www.example.com/JDA/v1/exceptions/metadata). This end point will be responsible to provide the meta data information associated with this API
    
    3. The third end point is optional and will have the unique PATH suffixed with /properties (e.g. http://www.example.com/JDA/v1/exceptions/properties).

# JSON Format for Response API

    POST : https://<domain>/enPoint

```
{
    "data": [
            {
                "<key>": "<value>"
            }
        ]
}
```

## Bibliography

### `data.cells` : Array<Object>

Contains the collection of data that is used in the UI



# JSON Format for Meta Data API

    POST : https://<domain>/enPoint/metaData

```
{
    
    "data": [
            {
                "fieldName": "<string>",
                "fieldLabel": "<string>",
                "datatype": "number | string | date | derived",
                "dataFormat":  {
                    "type" : "date",
                    format : "m/d/yyyy"
                },
                "mustForContext": "true | false",
                "isSystemColumn": "true | false",
                "rules": [
                    {
                        "type" : "text | conditional",
                        "conditions" : [
                            {
                                "condition" : "${fieldname} === 'value'",
                                "whenTrue" : "value",
                                "whenFalse" : "false"
                            }
                        ]
                    }
                ],
                "default": {
                    "hide": "true | false",
                    "sortOrder": "[0...n]",
                    "isGroupBy": "true | false"
                }
            }
}   ]
```

## Bibliography

## data : Object

Data that constitutes the response

### data.headers : Array<Object>

Collection of Metadata. This data defines what is available for the UI to process.

#### `data.headers.fieldName` : string

Unique name for the field name

    1. This field name can link to a key in the cells data.
    2. The value for this will be used as a reference to evaluate any calculations or derivations


#### `data.headers.fieldLabel` : string

Label that will be used in the UI.

#### `data.headers.datatype` : string

defines the type of the data that this field will carry

    Can have either of the following values
    ======================================================================
    number : Type is number and will be treated as a number. Will have format for decimal points
    string : Type of the data is string
    date : data type is string, date format will be applicable on this.
    derived : The value for this column is not available but will be derived by using the rules.


#### data.headers.dataFormat : Object

Defines the format expression that should be used to format the values for the field

##### `data.headers.dataFormat.type` : string

Defines the type of formatting to be done on the

     Can have either of the following values
    ======================================================================
    date : Value provided will be used a date format
    prefixText : Value provided will be prefixed to the data
    suffixText : Value provided will be suffixed to the data
    bgColor : Value provided will be used as a background color
    fontColor : Value provided will be used as a font color
    prefixIcon :  Icon provided will be prefixed to the data (SVG format)
    suffixIcon :  Icon provided will be suffixed to the data (SVG format)

##### `data.headers.dataFormat.format` : string

Defines the format that is to be used e.g. for date "mm/dd/yyyy"

#### `data.headers.mustForContext` : boolean

Defines if the data for this field is to be used in the context

#### `data.headers.isSystemColumn` : boolean

Defines that this is a system column and will never be shown on the UI

#### `data.headers.rules` : Array<Object>

Defines the rules that are applied to get the resultant value for the field

#### `data.headers.rules[n].type` : string

Defines type of the rule

    Can have either of the following values
    ======================================================================
    text : should comply to the JS literal template. example \` ${product} is simple\`;
    conditional : defines a series of conditions. based on which a particular value is evaluated

#### `data.headers.rules[n].conditions` : Array<Object>

Defines a set of conditions based on which the content will be decided

#### `data.headers.rules[n].conditions.evaluate` : string,

Defines an evaluation expressions
Example
`${filrate} == 100`
in the above example `filrate` is a fieldname and will have to be wrapped between "\${" and "}"

#### `data.headers.rules[n].conditions.whenTrue` : string | Array<Object>

Defines if the value that should be used when the condition expression evaluates to true

#### `data.headers.rules[n].conditions.whenFalse` : string | Array<Object>

Defines if the value that should be used when the condition expression evaluates to false

#### `data.headers.default` : Object

Defines the defaults that the UI will have to set for this field

#### `data.headers.default.hide` : boolean

Define if this field is to be hidden on the UI

#### `data.headers.default.sortOrder` : number

Define a sequence number that defines the relative order of this field

#### `data.headers.default.isGroupBy` : boolean

Define if the grouping should be performed based on this field
