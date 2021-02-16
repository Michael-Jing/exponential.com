+++
title = "Get Data From AWS IoT Device Shadow and Store In Dynamodb"
date = 2021-02-02
+++

We need to implement an IoT platform on AWS, and we will be using device shadows to communicate between device
and other parts such as Apps. One requirement for the plotform is that we need to access historic data.
My plan is that I'm going to create a Rule that reads from device shadows and store data in dynamodb

# Where to get data

I plan to collect data from each device shadow's /update/documents topic, so the full
topic would be ```"$aws/things/thingName/shadow/name/shadowName/update/documents"```

an example message from the topic is like the following
```
{
  "previous" : {
    "state": {
        "desired": {
            "attribute1": integer2,
            "attribute2": "string2",
            ...
            "attributeN": boolean2
        },
        "reported": {
            "attribute1": integer1,
            "attribute2": "string1",
            ...
            "attributeN": boolean1
        }
    },
    "metadata": {
        "desired": {
            "attribute1": {
                "timestamp": timestamp
            },
            "attribute2": {
                "timestamp": timestamp
            },
            ...
            "attributeN": {
                "timestamp": timestamp
            }
        },
        "reported": {
            "attribute1": {
                "timestamp": timestamp
            },
            "attribute2": {
                "timestamp": timestamp
            },
            ...
            "attributeN": {
                "timestamp": timestamp
            }
        }
    },
    "version": version-1
  },
  "current": {
    "state": {
        "desired": {
            "attribute1": integer2,
            "attribute2": "string2",
            ...
            "attributeN": boolean2
        },
        "reported": {
            "attribute1": integer2,
            "attribute2": "string2",
            ...
            "attributeN": boolean2
        }
    },
    "metadata": {
        "desired": {
            "attribute1": {
                "timestamp": timestamp
            },
            "attribute2": {
                "timestamp": timestamp
            },
            ...
            "attributeN": {
                "timestamp": timestamp
            }
        },
        "reported": {
            "attribute1": {
                "timestamp": timestamp
            },
            "attribute2": {
                "timestamp": timestamp
            },
            ...
            "attributeN": {
                "timestamp": timestamp
            }
        }
    },
    "version": version
  },
  "timestamp": timestamp,
  "clientToken": "token"
}
```
and data will be retrieved from the ```current/state/reported``` paths

# Create Rule to collect data from topics

Here is an example Rule query statement
```
   SELECT
  'The fuel level for ' + device + ' is currently at ' + round(fuel_level) + '%. The car is at '
  + longitude + ' of longitude and ' + latitude + ' of latitude.' AS message
FROM 'topicName'
WHERE
  fuel_level < 25
```

Since I don't want to crate a rule for each thing, so I'm going to use wildcard to specify multiple topics for the topicName field, it's called a topic filter.
two wildcard is available, '#' means multi-level and '+' means single-level. Suppose I want to collect data from all things which has a certain prefix in their
name, and all theses thins each has a shadow of the same name, the topic filter I'm going to use is
```
"$aws/things/thingNamePrefix+/shadow/name/shadowName/update/documents"
```

Unfortunately, the above topic filter does not work, since both '#' and '*' need to be the single character in its path level (between two slashes), so a working
topic filter would be like the following

```
"$aws/things/+/shadow/name/shadowName/update/documents"
```
So now the problem is how can we compose a rule that only works for some type of things, but I don't have an answer yet.
