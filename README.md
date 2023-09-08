While working as a consultant for an integration consultancy, we had a client who wanted to be able to ingest the client list data set from WorkflowMax into SQL.
The client data set consists of basic client information (name, email etc) and a list of the client contacts, of which there might be one or more.

WorkflowMax exposes its data sets in XML, the process of getting the data set from WorkflowMax is quite trivial, 
1. Get an access token,
2. Get the client list.
3. Store the result in blob storage.
4. Trigger an Azure Data Factory pipeline 
 a. To convert the XML to JSON.
 b. Store th JSON file in blob storage.
 c. Within the pipeline, insert or upsert top level client details into the database.

In order to handle the list of contacts, the outputed json file has to be processed. However for some reason, the azure data factory copy activity, produces an array of contacts when there are multiple contacts, but an object of contacts when there is just one contact.

```json
{
    "_id": "64fafdab0148d179b4bad3d6",
    "index": 0,
    "guid": "a5a30726-560b-4548-9219-d3e579ff4df0",
    "name": "Joanna Stephenson",
    "company": "SYNKGEN",
    "contacts": [
    {
        "id": 0,
        "name": "Church Cleveland",
        "email": "churchcleveland@synkgen.com",
        "age": 37
    },
    {
        "id": 1,
        "name": "Tabitha Gentry",
        "email": "tabithagentry@synkgen.com",
        "age": 26
    }     
    ]    
}
```

 or

 ```json
{
    "_id": "64fafdabea7f6eecf0e80b44",
    "index": 1,
    "guid": "3f273e26-2dec-4a56-a139-760977630590",
    "name": "Lynnette Guerra",
    "company": "COMTOURS",    
    "contacts": {
    "id": 1,
    "name": "Marcie Spence",
    "email": "marciespence@comtours.com",
    "age": 40
    }
}  
```

The json file can be processed by a logic app, however there is no simple way to handle processing elements where the schema contains an object or an array for the same element.

A possible solution can be found at [Dynamically Parse JSON as object or Array](https://powerusers.microsoft.com/t5/General-Power-Automate/Dynamically-Parse-JSON-as-object-or-Array/td-p/1212427) where the question was answered and a solution proposed.

Creating a logic app to process these records is not complicated, the following page is a walkthrough on how this can be done.

[Logic App Setup](logicapp01.md)
