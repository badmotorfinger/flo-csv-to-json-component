# flo-csv-to-json-component

This repository is an example component which demonstrates how to build a flow component so it will be available as a workflow component, able to accept various inputs and produce an output for use in other components.

## Synopsis

You have developed a workflow and you require a component which takes a CSV file and transforms it into JSON so you have a JavaScript object to work with in other components and so you can avoid performing string maniplulation. For this you decide to create a component.

# Implementation

The first step in creating the component is to first design it using a manifest file. The manifest describes the component inputs and configuration and is used when uploading the component for use in your workflow.

In the following example, our component has 3 inputs which you can see as being `file`, `firstRowContainsHeadings` and `delimiter`. Each of our fields has a description which will be available to the end-user, a data type and also if it's required. More information about `parameterDescriptors` can be found [here](https://link/to/somewhere/else).

The other two important properties are named `id`. The first `id` parameter is a GUID of the actual component which has not yet been created in our example and the second `id` parameter is also a GUID which refers to the method which will be called in the component (the components entry point). These values need to be unique in the flow system and can easily be created with a call to the C# method `Guid.NewGuid()` or in PowerShell by executing the command `New-Guid`.

Lastly, there's a link pointing to the function endpoint, in this case it's `https://fl0-appactions-dev-aue.azurewebsites.net/api/apps-csvtojson-actions`. This endpoint should reflect the name of the component in the flow system, i.e. `apps-csvtojson-actions`.

```json
{
  "id": "5babd1da-aadb-4bc2-9204-0fee1237a531",
  "name": "CSV to JSON",
  "description": "Converts CSV data into a JSON formatted document.",
  "category": "Data Transformation",
  "version": "1.0",
  "iconFile": "icon.png",
  "bannerFiles": [],
  "features": {
    "flowActions": {
      "url": "https://fl0-appactions-dev-aue.azurewebsites.net/api/apps-csvtojson-actions",
      "actions": [
        {
          "id": "9d567bbb-9181-4718-9c66-cc5938e49160",
          "name": "Convert CSV data to a JSON document.",
          "description": "Takes Base64 encoded CSV data as input and transforms it into a JSON document.",
          "parameterDescriptors": [
            {
              "displayName": "CSV file as Base64 string",
              "key": "file",
              "isRequired": true,
              "type": "string"
            },
			      {
              "displayName": "First row contains column names",
              "key": "firstRowContainsHeadings",
              "isRequired": true,
              "type": "string",
			        "stringFormat" : "Boolean",
			        "metadata" : {
				         "default" : true
			        }
            },
            {
              "displayName": "Delimiter",
              "key": "delimiter",
              "isRequired": true,
              "type": "string",
              "stringFormat" : "Enum",
              "metadata" : {
                "options": [
                    { "key" : "Comma", "displayName" : "Comma" },
                    { "key" : "Tab", "displayName" : "Tab" }
                ],
                "defaultOptionKey" : "Comma"
              }
            }
          ],
          "outputPathNames": [ "Next" ]
        }
      ]
    }
  }
}
```
