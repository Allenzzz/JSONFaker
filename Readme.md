#JSON Faker

A provider extension for the fantastic [Faker-Factory](https://pypi.python.org/pypi/fake-factory) module. Instead of creating discrete values of a particular type or utility, it uses [JSON Schemas](http://json-schema.org/) to generate  structures with multiple key-value pairs and hierarchy in [JSON](http://www.json.org/) markup. 

In addition to Faker-Factory, the JSON Faker also uses the [rstr](https://pypi.python.org/pypi/rstr/2.1.2) module.

I have used this for integration testing and [fuzzing](https://www.owasp.org/index.php/Fuzzing) [RESTful](http://www.restdoc.org/spec.html) APIs.


###Example script:
This will generate some random JSON based on the ```resource.schema``` and POSTs it to ```https://www.some.api/resource```

```
import JSONFaker
import requests
import faker
import sys
import json

fake = faker.Faker()
fake.add_provider(JSONFaker.JSONProvider)

response = requests.post(sys.argv[1], 
						 data=fake.json(json.load(open(sys.argv[2]))))
						 
assert(response.status_code == 200), "Error: Response code was {}".format(response.status_code)

sys.exit(0)

# python example.py https://www.some.api/resource pat/to/some/resource.schema
```


###Requirements & Installation  

+	Python 2.7
+	[pip](https://pypi.python.org/pypi/pip)
+	[virtualenv](http://docs.python-guide.org/en/latest/dev/virtualenvs/) _strongly recommended_

```pip install -r requirements.txt``` will install all of the required support libraries.

###Another Example


```
[Cat the schema]

(jsonprovider)pickard@Pocket-2 ~/projects> cat JSONFaker/example_schemas/globalreach_registration.requestschema.json | python -mjson.tool
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "additionalproperties": false,
    "properties": {
        "emailaddress": {
            "description": "The email address of the registrant",
            "type": "string"
        },
        "macaddress": {
            "description": "The MAC address of the registrant device, encrypted",
            "pattern": "^ENC[0-9a-f]{48}",
            "type": "string"
        },
        "uuid": {
            "description": "The generated client identifier generated by GlobalReach",
            "type": "string"
        }
    },
    "required": [
        "emailaddress",
        "macaddress",
        "uuid"
    ],
    "title": "Global Reach LinkNYC registration request Schema",
    "type": "object"
}

[Generate an example]

(jsonprovider)pickard@Pocket-2 ~/projects> python gen_json_from_schema.py JSONFaker/example_schemas/globalreach_registration.requestschema.json | python -mjson.tool
{
    "emailaddress": "8StL*%AyYhk^OI8OhzAg3DzEs2p*y#",
    "macaddress": "ENCa0b0d3822f25c7efca27ce6af2fac5e40098c31629c6a4bf",
    "uuid": "255(uBYXsoMsgVirOMPX71X3m@AchY"
}

[Generate an another example]

(jsonprovider)pickard@Pocket-2 ~/projects> python gen_json_from_schema.py JSONFaker/example_schemas/globalreach_registration.requestschema.json | python -mjson.tool
{
    "emailaddress": "xc8MgpLSYSXH$!uM3LV0xEO!9y%UUq",
    "macaddress": "ENCde56abbf657243d6516c30ccf4c9f1fcad936d11504bed00",
    "uuid": "KFn86yflI&$hO9fCEC5Z76zKHSZVYE"
}
(jsonprovider)pickard@Pocket-2 ~/projects> 

```

