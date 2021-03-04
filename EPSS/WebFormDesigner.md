# EPSS Web Form Schema Designer
> An EPSS feature that integrates QMS forms design and control. It generates form by rendering JSON file with valid syntax. Visit sandbox on [EPSS](https://salmon-sand-0d3da2a10.azurestaticapps.net).

## Requirements
JSON (JavaScript Object Notation) is a standard file format that uses text to communicate data objects to array data types. We assume that user have a basic understanding on how to create and modify this file format.

## File Structure
The file is divided into two section

Metadata:
- title
- version
- status (auto-injected based on form lifecycle)

Form Elements:
- input/text
- input/date
- input/dropdown
- input/checkbox
- table
- textarea
- header/section 

```
{
  ...metadata
  "elements": [
    ...formElements
  ]
}
```

## Global API
### colspan
Form rendered is virtually subdivided into 12 columns. Having a colspan of 6 means the element populates 1/2 of the form width. Thus the example below will render 3 elements with 2 with 1/4 of the witdh while the last element have the rest of the width. To use the whole width, you can use 12 or "full" as input.
```
    ...someElements
    {
      "element": "input",
      "type": "text",
      <b>"colspan": 3,</b>
      "label": "MCMS Owner",
      "key": "contractor"
    },
    {
      "element": "input",
      "type": "text",
      <b>"colspan": 3,</b>
      "label": "Registration Number",
      "key": "mcrnum"
    },
    {
      "element": "input",
      "type": "text",
      <b>"colspan": 6,</b>
      "label": "Building Address",
      "key": "address"
    },
    ...someElements
```

### key
Design team will provide this list. This important beacause these keys bind and map the fields back to the database. Any new fields must be requested.
```
    ...someElements
    {
      "element": "input",
      "type": "text",
      "colspan": 3,
      "label": "Foo",
      <b>"key": "key1"</b>
    },
    {
      "element": "input",
      "type": "text",
      "colspan": 3,
      "label": "Bar",
      <b>"key": "key2"</b>
    },
    ...someElements
```

### label
Any arbitrary label that best described the data field
```
    ...someElements
    {
      "element": "input",
      "type": "text",
      "colspan": 3,
      <b>"label": "Foo",<b>
      "key": "contractor"
    },
    {
      "element": "input",
      "type": "text",
      "colspan": 3,
      <b>"label": "Bar",<b>
      "key": "mcrnum"
    },
    ...someElements
```

### type
Applicable only to Input type elements
```
    ...someElements
    {
      "element": "input",
      <b>"type": "text",</b>
      "colspan": 6,
      "label": "Building Address",
      "key": "address"
    },
        {
      "element": "input",
      <b>"type": "date",</b>
      "colspan": 6,
      "label": "Activation Date",
      "key": "activationDate"
    },
    {
      "element": "input",
      <b>"type": "checkbox",</b>
      "colspan": 3,
      "label": "Known Load SOP-132",
      "key": "isSOP132Completed"
    },
    ...someElements
```

## Element Specific API
### dropdown
values - An array of strings that enumerates the dropdown options/selections.
```
    ...someElements
    {
      "element": "dropdown",
      "colspan": 6,
      "label": "Rated Voltage",
      "key": "voltage",
      <b>"values": [
        "120V - 277V",
        "347V",
        "480V",
        "600V"
      ]</b>
    },
    ...someElements
```

### table
headers - An array of objects based on [key:label] format. This determines the table header labels and the associated key to the database.
default - Optional array of objects that initializes predetermine fields.
```
    ...someElements
 {
      "element": "table",
      "colspan": "full",
      "key": "table1",
      "headers": [
        {
          "register": "Meter #"
        },
        {
          "unit": "Service"
        },
        {
          "ctID": "CT ID"
        },
        {
          "ctSerial": "CT Serial #"
        },
        {
          "phase": "Phase"
        },
        {
          "ctPolarity": "CT Polarity"
        },
        {
          "extCableNum": "Extension Cable #"
        },
        {
          "ctX1WhiteExt": "CT X1 - White Extension"
        },
        {
          "ctX1WhiteCable": "CT X1 - White Cable"
        },
        {
          "ctX1WhiteModule": "CT X1 - White Module"
        },
        {
          "ctX1BlackExt": "CT X2 - Black Extension"
        },
        {
          "ctX1BlackCable": "CT X2 - Black Cable"
        },
        {
          "ctX1BlackModule": "CT X2 - Black Module"
        },
        {
          "ctRatio": "CT Ratio (/80mA)"
        },
        {
          "extMultiplier": "Billing Multiplier"
        },
        {
          "seZeroFourStatus": "Pass / Fail"
        }
      ],
      "default": [
        {
          "register": 1,
          "ctX1WhiteCable": "Black",
          "ctX1BlackCable": "Red"
        },
        {
          "register": 2,
          "ctX1WhiteCable": "Black",
          "ctX1BlackCable": "White"
        },
        {
          "register": 3,
          "ctX1WhiteCable": "Black",
          "ctX1BlackCable": "Green"
        },
        {
          "register": 4,
          "ctX1WhiteCable": "Black",
          "ctX1BlackCable": "Blue"
        },
        {
          "register": 5,
          "ctX1WhiteCable": "Black",
          "ctX1BlackCable": "Yellow"
        },
        {
          "register": 6,
          "ctX1WhiteCable": "Black",
          "ctX1BlackCable": "Brown"
        },
        {
          "register": 7,
          "ctX1WhiteCable": "Black",
          "ctX1BlackCable": "Orange"
        },
        {
          "register": 8,
          "ctX1WhiteCable": "Red",
          "ctX1BlackCable": "White"
        },
        {
          "register": 9,
          "ctX1WhiteCable": "Red",
          "ctX1BlackCable": "Green"
        },
        {
          "register": 10,
          "ctX1WhiteCable": "Red",
          "ctX1BlackCable": "Blue"
        },
        {
          "register": 11,
          "ctX1WhiteCable": "Red",
          "ctX1BlackCable": "Yellow"
        },
        {
          "register": 12,
          "ctX1WhiteCable": "Red",
          "ctX1BlackCable": "Brown"
        }
      ]
    },
    ...someElements
```

## Recommendation
Although its fine to work directly on the online editor, we recommend to use external tool like notepad++ or VS Code IDE to compose a schema. It gives more flexibility and better readility while writing a schema.
