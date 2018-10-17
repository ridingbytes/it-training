# SENAITE Scripting

Zope allows to create Python based scripts called `Script (Python)` through the
Zope Management Interface (ZMI).

## Creating a Script (Python)

To create a new Script (Python), browse to the ZMI, and select in the upper
right selection box the option "Script (Python)".

Give it an ID, e.g. "script_test" and click "Add and Edit".

Clicking on the "Test" tab executes the script and should show an output similar
to this:

```
This is the Script (Python) "script_test" in http://localhost:8080/senaite
```

## Demo Scripts

The next sections will show some scripts which can be used as a starting point
to build own scripts.


### Hello World

Create a script called `hello` and add the following contents:

```python
print "Hello! My name is '%s', I live inside '%s' and my current context is '%s'" % (script, container,  repr(context))

return printed
```


### Export Analysis Requests to CSV

Create a script called `script_ar_to_csv` and add the following contents:

```python
from Products.CMFCore.utils import getToolByName

# get the catalog tool
catalog = getToolByName(context, "bika_catalog_analysisrequest_listing")

# query the catalog
results = catalog({
    "sort_on": "created",
    "sort_order": "descending",
})

header = [
    "ID",
    "Client",
    "Created",
]
print ",".join(header)

for result in results:
    obj = result.getObject()
    client = obj.getClient()

    created = obj.created()

    row = [
        '"{}"'.format(obj.getId()),
        '"{}"'.format(client.getName()),
        '"{}"'.format(obj.created().strftime("%Y-%m-%d")),
    ]
    print ",".join(row)


return printed
```


### Show all Instruments that are out of date

Create a script called `script_expired_instruments` and add the following contents:

```python
from Products.CMFCore.utils import getToolByName

# get the catalog tool
catalog = getToolByName(context, "bika_setup_catalog")

# query the catalog
results = catalog({
    "portal_type": "Instrument",
    "sort_on": "created",
    "sort_order": "descending",
})

header = [
    "ID",
    "URL",
]
print ",".join(header)

for result in results:
    obj = result.getObject()
    if not obj.isOutOfDate():
        continue

    row = [
        '"{}"'.format(obj.getId()),
        '"{}"'.format(obj.absolute_url()),
    ]
    print ",".join(row)


return printed
```


### Export all Clients to CSV

Create a script called `script_clients_to_csv` and add the following contents:

```python
from Products.CMFCore.utils import getToolByName

# get the catalog tool
catalog = getToolByName(context, "portal_catalog")

# query the catalog
results = catalog({
    "portal_type": "Client",
    "sort_on": "created",
    "sort_order": "descending",
})

header = [
    "ID",
    "Client Name",
    "URL",
]
print ",".join(header)

for result in results:
    obj = result.getObject()

    row = [
        '"{}"'.format(obj.getClientID()),
        '"{}"'.format(obj.getName()),
        '"{}"'.format(obj.absolute_url()),
    ]
    print ",".join(row)


return printed
```


## Export all Analysis Services to CSV

Create a script called `script_as_to_csv` and add the following contents:

```python
from Products.CMFCore.utils import getToolByName

# get the catalog tool
catalog = getToolByName(context, "bika_setup_catalog")

# query the catalog
results = catalog({
    "portal_type": "AnalysisService",
    "sort_on": "created",
    "sort_order": "descending",
})

header = [
    "Path",
    "URL",
    "Title",
    "Sort Key",
    "Unit",
    "Methods",
]

print ",".join(header)


for result in results:
    obj = result.getObject()

    path = "/".join(obj.getPhysicalPath())
    sort_key = str(obj.getSortKey())
    title = obj.Title()
    unit = obj.getUnit()
    url = obj.absolute_url()
    methods = ";".join(map(lambda m: m.Title(), obj.getMethods()))

    row = [
        '"{}"'.format(path),
        '"{}"'.format(url),
        '"{}"'.format(title),
        '"{}"'.format(sort_key),
        '"{}"'.format(unit),
        '"{}"'.format(methods),

    ]
    print ",".join(row)

return printed
```
