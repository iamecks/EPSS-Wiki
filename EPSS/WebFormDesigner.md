# EPSS Web Form Schema Designer
> An EPSS feature that integrates QMS forms design and control. It generates form by rendering JSON file with valid syntax. Visit sandbox on [EPSS](https://salmon-sand-0d3da2a10.azurestaticapps.net).

## Requirements
JSON (JavaScript Object Notation) is a standard file format that uses text to communicate data objects to array data types. We assume that user have a basic understanding on how to create and modify this file format.

## File Structure
The file is divided into two sections

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

```json
{
  ...metadata
  "elements": [
    ...formElements
  ]
}
```

## Global API
### colspan
Form is rendered virtually on 12 subdivided columns. Having a colspan of 6 means the element populates 1/2 of the form width. Example below will render 3 elements, 2 with 1/4 witdh while the last element have the rest of the width. To use the whole width, you can use either 12 or "full" as input.
<pre><code>
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
</code></pre>

### key
Design team will provide this list. Keys bind and map the fields back to the database when persisting form data. Any new fields must be requested.
<pre><code>
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
</code></pre>

### label
Any arbitrary label that best describe the data field
<pre><code>
    ...someElements
    {
      "element": "input",
      "type": "text",
      "colspan": 3,
      <b>"label": "Foo",</b>
      "key": "contractor"
    },
    {
      "element": "input",
      "type": "text",
      "colspan": 3,
      <b>"label": "Bar",</b>
      "key": "mcrnum"
    },
    ...someElements
</code></pre>

### type
Applicable only to "Input" type elements
<pre><code>
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
</code></pre>

## Element Specific API
### dropdown
values - An array of strings that enumerates the dropdown options or selections.
<pre><code>
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
</code></pre>

### table
headers - An array of objects based on [key:label] format. This determines the table header labels and the associated key to the database.
default - Optional array of objects that initializes predetermine fields.
<pre><code>
    ...someElements
 {
      "element": "table",
      "colspan": "full",
      "key": "table1",
      <b>"headers": [
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
      ]</b>
    },
    ...someElements
</code></pre>

## Recommendation
Although its fine to work directly in the online editor, we recommend to use an external tool like notepad++ or VS Code IDE to compose a schema. It gives more flexibility and better readility while writing a schema.
