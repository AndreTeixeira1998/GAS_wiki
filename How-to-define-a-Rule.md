To define a rule we import a .json file named "GASconfig.json"

# How to add a rule

To add a rule we need to add in the config file in JSON format an array named "rules".

# How to define a rule

Inside the array we can add:

## Type
 The type is a number that can be:
0 - Rule That verifies if the value is lesser than the number used to compare
1 - Rule That verifies if the value is greater than the number used to compare
2 - Rule That verifies if the value is equal than the number used to compare
3 - Rule That verifies if the value is within +-5% margin of the number used to compare

##Value
Numeric value used to compare

##Sensors
Used to specify the sensors used in the rule. 
First we indicate the node ID and then, separed by a ".", the sensor ID.

##Actuators
Used to specify the actuators to update.
First we indicate the node ID and then, separed by a ".", the actuator ID.

##Childs
Its used to be able to logically compare, inside the same rule, diferent types of comparison.
If it has a child it is considered an AND, otherwise its an OR.  

#Example

"rules": [
                {
                    "type": 0,
                    "value": 10,
                    "sensors": ["0.0"],
                    "actuators": ["0.0"],
                    "childs": [
                        {
                            "type": 1,
                            "value": 40,
                            "sensors": ["0.1"],
                            "actuators": ["0.0"],
                            "childs": []
                        }
                    ]
                },
                {
                    "type": 0,
                    "value": 5,
                    "sensors": ["1.3"],
                    "actuators": ["1.10"],
                    "childs": [
                        {
                            "type": 1,
                            "value": 20,
                            "sensors": ["1.3"],
                            "actuators": ["0.0"],
                            "childs": []
                        }
                    ]
                }
            ]

