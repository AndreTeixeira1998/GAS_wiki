# **OUTDATED DOCUMENTATION. PLEASE REFER TO THE LIBRARY HEADER FILE**

# Data Structure

This library allows the storage of all data concerning a IoT node, it's sensor, actuators and the rooms they are inserted in.

## Structures

```
typedef struct _actuator Actuator;
typedef struct _sensor Sensor;
typedef struct _node Node;
typedef struct _room Room;
typedef struct _datastore Datastore;
```

### Datastore

* `list* rooms`: LinkedList of rooms owned by this datastore.

#### Functions

* `Datastore* createDatastore ()`
  * Creates a Datastore object
  * Returns:
    * `Datastore*`: The pointer to the new Datastore object. NULL if error occurs.

* `bool deleteDatastore (Datastore* datastore)`
  * Delete a Datastore object and all it's childs
  * Parameters:
  * `datastore`: The pointer to the Datastore object to be deleted.
  * Returns:
    * `true` if an Error occurs
    * `false` if All good


### Room

* `uint16_t id`: ID of the Room. Must be unique.
* `Datastore* parentDatastore`: Pointer to the Datastore object that owns this room.
* `list_element* listPtr`: Pointer to the LinkedList element where this room instance is stored.
* `char* name`: Name of this Room. Must be unique.
* `list* nodes`: LinkedList of nodes owned by this room.

#### Functions

* `Room* createRoom (Datastore* datastore)`
  * Creates a Room object
  * Parameters: 
    * `datastore`: Datastore in which this room is to be inserted.
  * Returns:
    * `Room*` The pointer to the new Room object. NULL if error occurs.

* `bool deleteRoom (Room* room)`
  * Delete a Room object and all it's childs
  * Parameters:
    * `room`: The pointer to the Room object to be deleted.
  * Returns:
    * `true` if an Error occurs
    * `false` if All good


* `bool setRoomID (Room* room, uint16_t id)`
  * Set the Room ID object
  * Parameters:
    * `room`: Pointer to the Room object
    * `id` 
  * Returns:
    * `true` if an Error occurs
    * `false` if All good

* `bool setRoomName(Room* room, const char* str)`
  * Set the Room Name object
  * Parameters:
    * `room`: Pointer to the Room object
    * `str`: Room name
  * Returns:
    * `true` if an Error occurs
    * `false` if All good

* `Room* findRoomByID (Datastore* datastore, uint16_t roomID)`
  * Searches the datastore for a Room with the specified roomID
  * Parameters:
    * `datastore`: Datastore object to search
    * `roomID`: roomID to find
  * Returns:
    * `Room*` Room object with 'roomID' id. NULL if not found.

* `Room* findRoomByName (Datastore* datastore, const char* roomName)`
  * Searches the datastore for a Room with the specified roomName
  * Parameters:
    * `datastore`: Datastore object to search
    * `roomName`: roomName to find
  * Returns:
    * `Room*` Room object with 'roomName' name. NULL if not found.


### Node

* `uint16_t id`: ID of the Node. Must be unique.
* `Room* parentRoom`: Pointer to the Room object where this Node is located.
* `list_element* listPtr`: Pointer to the LinkedList element where this node instance is stored.
* `list* sensors`: LinkedList of sensors owned by this node.
* `list* actuators`: LinkedList of actuators owned by this node.

#### Functions

* `Node* createNode (Room* room)`
  * Create a Node object
  * Parameters:
  * `room`: A pointer to the Room this Node will belong to.
  * Returns:
    * `Node*` The pointer to the new Node object. NULL if error occurs.

* `bool deleteNode (Node* node)`
  * Delete a Node object and all it's childs
  * Parameters:
    * `node`: The pointer to the Node object to be deleted.
  * Returns:
    * `true` if an Error occurs
    * `false` if All good

* `bool setNodeID (Node* node, uint16_t id)`
  * Set the Node ID object
  * Parameters:
    * `node`: Pointer to the Node object
    * `id`
  * Returns:
    * `true` if an Error occurs
    * `false` if All good

* `Node* findNodeByID (Datastore* datastore, uint16_t nodeID)`
  * Searches the datastore for a Node with the specified nodeID
  * Parameters:
    * `datastore`: Datastore object to search
    * `nodeID`: nodeID to find
  * Returns:
    * `Node*`: Node object with 'nodeID' id. NULL if not found.


### Actuator

Structure to hold all data concerning an actuator.

* `Node* parentNode`: Node that holds/owns this actuator.
* `list_element* listPtr`: Pointer to the LinkedList element where this actuator instance is stored.
* `uint8_t r`: Red component to send to the RGB Matrix
* `uint8_t g`: Green component to send to the RGB Matrix
* `uint8_t b`: Blue component to send to the RGB Matrix

#### Functions

* `Actuator* createActuator (Node* node)`
  * Create a Actuator object
  * Parameters:
    * `node`: A pointer to the Node this Actuator will belong to.
  * Returns:
    * `Actuator*`: The pointer to the new Actuator object. NULL if error occurs.

* `bool deleteActuator (Actuator* actuator)`
  * Delete a Actuator object
  * Parameters:
    * `actuator`: The pointer to the Actuator object to be deleted.
  * Returns:
    * `true` if an Error occurs
    * `false` if All good


### Sensor

Structure to hold all data concerning a Node's Sensors.

```
#define N_TYPE_SENSOR           5
#define TYPE_SENSOR_VOLTAGE     0
#define TYPE_SENSOR_TEMPERATURE 1
#define TYPE_SENSOR_HUMIDITY    2
#define TYPE_SENSOR_LIGHT       3
#define TYPE_SENSOR_CURRENT     4

#define TYPE_SENSOR_VOLTAGE_VALUE_CALCULATOR        (&voltageSensorValue)
#define TYPE_SENSOR_TEMPERATURE_VALUE_CALCULATOR    (&temperatureSensorValue)
#define TYPE_SENSOR_HUMIDITY_VALUE_CALCULATOR       (&humiditySensorValue)
#define TYPE_SENSOR_LIGHT_VALUE_CALCULATOR          (&lightSensorValue)
#define TYPE_SENSOR_CURRENT_VALUE_CALCULATOR        (&currentSensorValue)

#define VALUE_CALCULATOR(x) (x ## _VALUE_CALCULATOR)
```

`typedef float sensorValueCalculator(uint16_t value)`: Only functions with this configuration are allowed to be used as calculators for the actual sensor values, given the 16bit values.

* `Node* parentNode`: Node that holds/owns this sensor.
* `list_element* listPtr`: Pointer to the LinkedList element where this sensor instance is stored.
* `uint8_t type`: Type of sensor
* `sensorValueCalculator* calculator`: Pointer to the calculator function
* `uint16_t value`: Current sensor value

#### Functions

* `Sensor* createSensor (Node* node, uint8_t type)`
  * Create a Sensor object
  * Parameters:
    * `node`: A pointer to the Node this Sensor will belong to.
  * Return:
    * `Sensor*`: The pointer to the new Sensor object. NULL if error occurs.

* `bool deleteSensor (Sensor* sensor)`
  * Delete a Sensor object
  * Parameters:
    * `sensor`: The pointer to the Sensor object to be deleted.
  * Returns:
    * `true` if an Error occurs
    * `false` if All good

* `float getSensorValue (Sensor* sensor)`
  * Calculate the value of the Sensor object
  * Parameters:
    * `sensor`: Pointer to the sendor object
  * Returns:
    * `float`: Value of the sensor. 0 in case of error

* `bool isValidType (uint8_t type)`
  * Determines if a type is valid
  * Parameters:
    * `type`: Sensor type
  * Returns:
    * `true` if the type is valid, `false` if it wasn't.

* `float voltageSensorValue (uint16_t value)`
  * Calculates the voltage a sensor is measuring from it raw data.
  * Parameters:
    * `value`: Raw 16bit value obtained from the sensor
  * Returns:
    * `float`: result in Volts

* `float temperatureSensorValue (uint16_t value)`
  * Calculates the temperature a sensor is measuring from it raw data.
  * Parameters:
    * `value`: Raw 16bit value obtained from the sensor
  * Returns:
    * `float`: result in degrees Celsius

* `float humiditySensorValue (uint16_t value)`
  * Calculates the relative humidity a sensor is measuring from it raw data.
  * Parameters:
    * `value`: Raw 16bit value obtained from the sensor
  * Returns:
    * `float`: result in percentage

* `float lightSensorValue (uint16_t value)`
  * Calculates the light intensity a sensor is measuring from it raw data.
  * Parameters:
    * `value`: Raw 16bit value obtained from the sensor
  * Returns:
    * `float`: result in Lux

* `float currentSensorValue (uint16_t value)`
  * Calculates the current a sensor is measuring from it raw data.
  * Parameters:
    * `value`: Raw 16bit value obtained from the sensor
  * Returns:
    * `float`: result in Ampère