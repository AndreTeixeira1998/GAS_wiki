# Configuration File Documentation

The JSON configuration file is structured in the following manner:

* `rooms` : Array
    * `id` : Room ID (unique)
    * `name` : Room Name (unique)
    * `nodes` : Array
        * `id` : Node ID (unique)
        * `sensors` : Array
            * `type` : [Sensor type](#Sensor-types) (Limited to one per type per node)
            * `posX` : X axis position in the RGBMatrix representation of the sensor data
            * `posY` : Y axis position in the RGBMatrix representation of the sensor data
            * `rangeMin` : Minimum value of the sensor data (for blue scale representation in the RGBMatrix)
            * `rangeMax` : Maximum value of the sensor data (for blue scale representation in the RGBMatrix)
        * `actuators` : Array
            * `id` : ID of the Actuator (unique per node)
            * `type` : Actuator type (for user reference) **For future use**
            * `posX` : X axis position in the RGBMatrix representation of the actuator status
            * `posY` : Y axis position in the RGBMatrix representation of the actuator status
    * `rules` : Array
        * `type` : [Rule type](#Rule-types)
        * `value` : Value to compare.
        * `sensors` : Array
            * `"<node-id>.<sensor-type>"` : sensor identification representation
        * `actuators`
            * `"<node-id>.<actuator-id>"` : actuator identification representation
        * `childs` : Array
            * Rules on the same hierarchical level are evaluated with a OR and nested/child rules are evaluated with a AND.
* `pixels` : Array of embelishment pixels on the RGBMatrix
    * `posX` : X axis position in the RGBMatrix representation
    * `posY` : Y axis position in the RGBMatrix representation
    * `r` : 0-255 value for color Red
    * `g` : 0-255 value for color Green
    * `b` : 0-255 value for color Blue

## Sensor types
* TYPE_SENSOR_VOLTAGE     0
* TYPE_SENSOR_TEMPERATURE 1
* TYPE_SENSOR_HUMIDITY    2
* TYPE_SENSOR_LIGHT       3
* TYPE_SENSOR_CURRENT     4


## Rule types
* TYPE_RULE_LESS_THEN     0
* TYPE_RULE_GREATER_THEN  1
* TYPE_RULE_EQUAL_TO      2
* TYPE_RULE_WITHIN_MARGIN 3

## Configuration Example

```
{
    "rooms": [
        {
            "id": 0,
            "name": "Room 0",
            "nodes": [
                {
                    "id": 1,
                    "sensors": [
                        {
                            "type": 1,
                            "posX": 0,
                            "posY": 0,
                            "rangeMin": 10,
                            "rangeMax": 40
                        },
                        {
                            "type": 2,
                            "posX": 0,
                            "posY": 1,
                            "rangeMin": 15,
                            "rangeMax": 100
                        }
                    ],
                    "actuators": [
                        {
                            "id": 0,
                            "type": 0,
                            "posX": 1,
                            "posY": 0
                        },
                        {
                            "id": 1,
                            "type": 0,
                            "posX": 1,
                            "posY": 1
                        }
                    ]
                }
            ],
            "rules": [
                {
                    "type": 0,
                    "value": 20,
                    "sensors": ["1.1"],
                    "actuators": ["1.0"],
                    "childs": []
                },
                {
                    "type": 1,
                    "value": 50,
                    "sensors": ["1.2"],
                    "actuators": ["1.1"],
                    "childs": []
                }
            ]
        },
        {
            "id": 1,
            "name": "Room 1",
            "nodes": [
                {
                    "id": 2,
                    "sensors": [
                        {
                            "type": 1,
                            "posX": 0,
                            "posY": 2,
                            "rangeMin": 10,
                            "rangeMax": 40
                        },
                        {
                            "type": 2,
                            "posX": 0,
                            "posY": 3,
                            "rangeMin": 15,
                            "rangeMax": 100
                        }
                    ],
                    "actuators": [
                        {
                            "id": 0,
                            "type": 0,
                            "posX": 1,
                            "posY": 2
                        },
                        {
                            "id": 1,
                            "type": 0,
                            "posX": 1,
                            "posY": 3
                        }
                    ]
                }
            ],
            "rules": [
                {                           |
                    "type": 0,              |
                    "value": 20,            |
                    "sensors": ["2.1"],     | If (sensor2.1 < 20)
                    "actuators": ["2.0"],   |
                    "childs": []            |
                },                          | OR
                {                           |
                    "type": 1,              |
                    "value": 30,            | If (sensor2.1 > 30)
                    "sensors": ["2.1"],     |
                    "actuators": ["2.0"],   |
                    "childs": []            |
                },                          |
                {
                    "type": 1,
                    "value": 50,
                    "sensors": ["2.2"],
                    "actuators": ["2.1"],
                    "childs": []
                }
            ]
        },
        {
            "id": 2,
            "name": "Room 2",
            "nodes": [
                {
                    "id": 3,
                    "sensors": [
                        {
                            "type": 1,
                            "posX": 0,
                            "posY": 4,
                            "rangeMin": 10,
                            "rangeMax": 40
                        },
                        {
                            "type": 2,
                            "posX": 0,
                            "posY": 5,
                            "rangeMin": 15,
                            "rangeMax": 100
                        }
                    ],
                    "actuators": [
                        {
                            "id": 0,
                            "type": 0,
                            "posX": 1,
                            "posY": 4
                        },
                        {
                            "id": 1,
                            "type": 0,
                            "posX": 1,
                            "posY": 5
                        }
                    ]
                }
            ],
            "rules": [
                {                               |
                    "type": 0,                  |
                    "value": 30,                | IF (sensor3.1 < 30)
                    "sensors": ["3.1"],         |
                    "actuators": ["3.0"],       |
                    "childs": [                 | AND
                        {                       |
                            "type": 1,          |
                            "value": 20,        | IF (sensor3.1 > 20)
                            "sensors": ["3.1"], |
                            "actuators": [],    |
                            "childs": []        |
                        }                       |
                    ]                           |
                },                              |
                {
                    "type": 1,
                    "value": 50,
                    "sensors": ["3.2"],
                    "actuators": ["3.1"],
                    "childs": []
                }
            ]
        }
    ],
    "pixels": [
        {
            "posX": 2,
            "posY": 2,
            "r": 0,
            "g": 255,
            "b": 255
        }
    ]
}
```