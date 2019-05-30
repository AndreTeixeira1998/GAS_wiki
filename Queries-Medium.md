Sensor Data
===============
SELECT room_node.room_id AS Room, AVG(value) AS Average FROM sinf.sensor_state, sinf.room_node, sinf.node_sensor, sinf.sensor
WHERE "timestamp"
BETWEEN '2019-05-28 12:58:12' AND '2019-05-28 12:58:20' AND sensor_state.sensor_id=node_sensor.sensor_id AND node_sensor.node_id=room_node.node_id 
AND sensor.sensor_id=sensor_state.sensor_id AND sensor.type=2
GROUP BY (room_node.room_id)


Actuator Data
===============
select count(distinct(actuator.actuator_id)) change, room.name room from sinf.node_actuator inner join sinf.room_node on node_actuator.node_id = room_node.node_id inner join sinf.room on room_node.node_id = room.room_id inner join sinf.actuator on node_actuator.actuator_id = actuator.actuator_id where actuator.type = 2 group by room.name


Configuration Data
===============
SELECT room_node.room_id AS Room, COUNT(DISTINCT node_sensor.node_id) AS Mote_Count, COUNT(DISTINCT node_sensor.sensor_id) AS Sensor_Count FROM sinf.room_node, sinf.node_sensor
WHERE node_sensor.node_id=room_node.node_id
GROUP BY (room_node.room_id)


Control Rules Data
===============
select count(rule.rule_id) rules, room.name room
from sinf.sensor_rule
inner join sinf.sensor on sensor_rule.sensor_id = sensor.sensor_id
inner join sinf.rule on sensor_rule.rule_id = rule.rule_id
inner join sinf.node_sensor on sensor_rule.sensor_id = node_sensor.sensor_id
inner join sinf.room_node on node_sensor.node_id = room_node.node_id
inner join sinf.room on room_node.room_id = room.room_id
group by room.name


Energy Data
===============
select i.energ*0.1544 as cost,  i.room as room
from(
select sum(sensor_state.value) as current, sum(sensor_state.value*0.220) as power, sum(sensor_state.value*0.220*1/3600) as energ, room.name as room
from sinf.sensor_state 
inner join sinf.node_sensor on sensor_state.sensor_id = node_sensor.sensor_id
inner join sinf.room_node on node_sensor.node_id = room_node.node_id
inner join sinf.room on room_node.room_id = room.room_id
inner join sinf.sensor on sensor_state.sensor_id = sensor.sensor_id
where sensor.type = 4
group by room.name)i
