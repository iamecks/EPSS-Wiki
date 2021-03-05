# EPSS Web Form Schema Designer
> An EPSS feature that integrates QMS forms design and version control. It generates form by rendering JSON file with valid syntax. Visit sandbox on [EPSS](https://salmon-sand-0d3da2a10.azurestaticapps.net).

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
A complete working file looks like below:
```json
{
  "title": "Triacta GATEWAY Installation Form",
  "version": "Met-QF-132n-Rev.03",
  "status": "production",
  "elements": [
    {
      "element": "header",
      "type": "section",
      "label": "PROJECT"
    },
    {
      "element": "input",
      "type": "text",
      "colspan": 6,
      "label": "MCMS Owner",
      "key": "contractor"
    },
    {
      "element": "input",
      "type": "text",
      "colspan": 6,
      "label": "Registration Number",
      "key": "mcrnum"
    }
   ]
}
```

## Global API
### colspan
Form is rendered virtually on 12 subdivided columns. Having a colspan of 6 means the element populates 1/2 of the form width. Example below will render 3 elements, 2 quarter width elements while the last element populate half of the width. To use the whole width, you can use either 12 or "full" as input.
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
![image](https://user-images.githubusercontent.com/30376638/110136502-c26a0e80-7d9d-11eb-8abb-8ed1d929ecfc.png)

### key
Design team will provide this list. Keys bind and map the fields back to the database when persisting form data. Any new fields must be requested. See appendix A for available keys at this moment.
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
      "colspan": 6,
      <b>"label": "Foo",</b>
      "key": "contractor"
    },
    {
      "element": "input",
      "type": "text",
      "colspan": 6,
      <b>"label": "Bar",</b>
      "key": "mcrnum"
    },
    ...someElements
</code></pre>
![image](https://user-images.githubusercontent.com/30376638/110136653-efb6bc80-7d9d-11eb-84d5-ea2e84db057e.png)

### type
Describes the data format and also determines how element will be rendered. Applicable only to "Input" type elements. At this moment, we only support text, date & checkbox. For future development, some type can be possibly added. See possible types [here](https://www.w3schools.com/tags/tag_input.asp).
<pre><code>
    ...someElements
    {
      "element": "input",
      <b>"type": "text",</b>
      "colspan": 4,
      "label": "Building Address",
      "key": "address"
    },
        {
      "element": "input",
      <b>"type": "date",</b>
      "colspan": 4,
      "label": "Activation Date",
      "key": "activationDate"
    },
    {
      "element": "input",
      <b>"type": "checkbox",</b>
      "colspan": 12,
      "label": "Known Load SOP-132",
      "key": "isSOP132Completed"
    },
    ...someElements
</code></pre>
![image](https://user-images.githubusercontent.com/30376638/110136991-55a34400-7d9e-11eb-9e82-5922f8f18a85.png)


## Element Specific API
### header
Aside from the static title header based on title and version, this is the only non-input element. It always use the full width
<pre><code>
    ...someElements
    {
      "element": "header",
      "type": "section",
      "label": "PROJECT"
    },
    ...someElements
</code></pre>
![image](https://user-images.githubusercontent.com/30376638/110137068-6d7ac800-7d9e-11eb-925d-8ba83ef1140b.png)

### dropdown
**values** - An array of strings that enumerates the dropdown options or selections.
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
![image](https://user-images.githubusercontent.com/30376638/110137106-78cdf380-7d9e-11eb-84c5-107698d6443b.png)

### table
**headers** - An array of objects based on [key:label] format. This determines the table header labels and the associated key to the database. See appendix B for available keys at this moment.\
**default** - Optional array of objects that initializes predetermine fields.
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
![image](https://user-images.githubusercontent.com/30376638/110137182-8e431d80-7d9e-11eb-8442-7282d0ea57f6.png)

## Recommendation
Although its fine to work directly in the online editor, we recommend to use an external tool like notepad++ or VS Code IDE to compose a schema. It gives more flexibility and better readility while writing a schema.

## Appendix A: Input Fields
|	Field	|	Label	|	Type	|
|	---	|	---	|	---	|
|	contractor	|	MCMS Owner	|	String	|
|	mcrnum	|	Registration Number	|	String	|
|	address	|	Building Address	|	String	|
|	productCode	|	Model / Configuration	|	String	|
|	activationDate	|	Activation Date	|	Date	|
|	voltage	|	Rated Voltage	|	String	|
|	approvalNum	|	Approval Number	|	String	|
|	meterBadge	|	Badge Number	|	String	|
|	mac	|	MAC Address	|	String	|
|	serial	|	Serial Number	|	String	|
|	sealYear	|	Seal Year	|	Number	|
|	meterLocation	|	Location	|	String	|
|	remarks	|	Remarks	|	String	|
|	isSOP132Completed	|	Known Load SOP-132	|	Boolean	|
|	isSOP135Completed	|	Visual SOP-135	|	Boolean	|
|	hasServiceMeterVerification	|	Service - Meter Verification	|	Boolean	|
|	hasServiceDesc	|	Service Description	|	Boolean	|
|	isQF130Completed	|	Refer to QF-130	|	Boolean	|
|	note	|	Notes:	|	String	|

## Appendix B: Table Fields
|	Field	|	Label	|	Type	|
|	---	|	---	|	---	|
|	register	|	Meter #	|	Number	|
|	unit	|	Service	|	String	|
|	ctID	|	CT ID	|	String	|
|	ctSerial	|	CT Serial #	|	String	|
|	phase	|	Phase	|	String	|
|	ctPolarity	|	CT Polarity	|	String	|
|	extCableNum	|	Extension Cable #	|	String	|
|	ctX1WhiteExt	|	CT X1 - White Extension	|	String	|
|	ctX1WhiteCable	|	CT X1 - White Cable	|	String	|
|	ctX1WhiteModule	|	CT X1 - White Module	|	String	|
|	ctX1BlackExt	|	CT X2 - Black Extension	|	String	|
|	ctX1BlackCable	|	CT X2 - Black Cable	|	String	|
|	ctX1BlackModule	|	CT X2 - Black Module	|	String	|
|	ctRatio	|	CT Ratio (/80mA)	|	String	|
|	extMultiplier	|	Billing Multiplier	|	String	|
|	seZeroFourStatus	|	Pass / Fail	|	String	|
