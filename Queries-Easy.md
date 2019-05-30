Sensor Data
===============
SELECT timestamp AS timestamp, value AS measurement 
FROM sinf.sensor_state, sinf.sensor, sinf.room
WHERE "timestamp"
BETWEEN '2019-05-28 12:58:12' AND '2019-05-28 12:58:20'
AND room.room_id=1 AND sensor.type=3


Actuator Data
===============
select actuator_state.value state, room.name room, actuator.type sensor
from sinf.actuator_state
inner join sinf.node_actuator on actuator_state.actuator_id = node_actuator.actuator_id
inner join sinf.room_node on node_actuator.node_id = room_node.node_id
inner join sinf.room on room_node.room_id = room.room_id
inner join sinf.actuator on actuator_state.actuator_id=actuator.actuator_id
where actuator_state.actuator_state_id in 
(select max(actuator_state.actuator_state_id) from sinf.actuator_state 
group by  actuator_id)
group by actuator_state.value, room.name, actuator.type


Control Rules Data
===============
update sinf.rule set value = 50
where value in ( 
select  distinct(value)
from sinf.sensor_rule
inner join sinf.sensor on sensor_rule.sensor_id = sensor.sensor_id
inner join sinf.rule on sensor_rule.rule_id = rule.rule_id
inner join sinf.node_sensor on sensor_rule.sensor_id = node_sensor.sensor_id
inner join sinf.room_node on node_sensor.node_id = room_node.node_id
inner join sinf.room on room_node.room_id = room.room_id
where sensor.type = 2 and room.name='Shower Room Female');

select  rule.rule_id, room.name room, rule.value ref_value, sensor.type sensor
from sinf.sensor_rule
inner join sinf.sensor on sensor_rule.sensor_id = sensor.sensor_id
inner join sinf.rule on sensor_rule.rule_id = rule.rule_id
inner join sinf.node_sensor on sensor_rule.sensor_id = node_sensor.sensor_id
inner join sinf.room_node on node_sensor.node_id = room_node.node_id
inner join sinf.room on room_node.room_id = room.room_id
where sensor.type = '2' and room.name='Shower Room Female'


Energy Data
===============
SELECT timestamp AS timestamp, 0.22*value AS Energy FROM sinf.sensor_state, sinf.sensor, sinf.node_sensor, sinf.room_node WHERE "timestamp" BETWEEN '2019-05-28 12:58:12' AND '2019-05-28 12:58:20' AND sensor.sensor_id=sensor_state.sensor_id AND room_node.node_id=node_sensor.node_id AND node_sensor.sensor_id=sensor.sensor_id AND sensor.type='4' AND room_node.room_id='1'