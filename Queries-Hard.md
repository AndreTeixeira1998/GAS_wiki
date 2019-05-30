Sensor Data
===============
SELECT room_node.room_id AS Room, sensor_state.sensor_id AS Sensor_type, AVG(value) AS Average FROM sinf.room_node, sinf.node_sensor, sinf.sensor_state
WHERE "timestamp"
BETWEEN '2019-05-28 12:58:12' AND '2019-05-28 12:58:20'
AND node_sensor.node_id=room_node.node_id AND node_sensor.sensor_id=sensor_state.sensor_id
GROUP BY (room_node.room_id, sensor_state.sensor_id)


Actuator Data
===============
select actuator.type as actuator, case when node_actuator.end_date is null then age(now(), node_actuator.start_date) else age(end_date, start_date) end duration, actuator_state.value 
from sinf.node_actuator
inner join sinf.actuator on node_actuator.actuator_id = actuator.actuator_id
inner join sinf.actuator_state on node_actuator.actuator_id = actuator_state.actuator_id
order by duration desc
limit 1


Configuration Data
===============
select count(distinct(i.actuator_id)) actuator, count(distinct(i.sensor_id)) sensor, i.name room from ( select node_actuator.node_id, node_actuator.actuator_id, node_sensor.sensor_id, room.name from sinf.node_actuator inner join sinf.node_sensor on node_actuator.node_id = node_sensor.node_id inner join sinf.room_node on node_sensor.node_id = room_node.node_id inner join sinf.room on room_node.room_id = room.room_id ) i group by name


Control Rules Data
===============
select sensor_state.value measurement, sensor_state.timestamp, rule.value reference from sinf.sensor_state inner join sinf.sensor_rule on sensor_state.sensor_id = sensor_state.sensor_id inner join sinf.sensor on sensor_rule.sensor_id = sensor.sensor_id inner join sinf.rule on sensor_rule.rule_id = rule.rule_id inner join sinf.node_sensor on sensor_rule.sensor_id = node_sensor.sensor_id inner join sinf.room_node on node_sensor.node_id = room_node.node_id inner join sinf.room on room_node.room_id = room.room_id where room.name = 'WC Female' and sensor.type = 3


Energy Data
===============
select i.energ*0.11 as offpeak, i.energ*0.187 as peak,  i.room as room
from(
select sum(sensor_state.value) as current, sum(sensor_state.value*0.220) as power, sum(sensor_state.value*0.220*1/3600) as energ, room.name as room
from sinf.sensor_state 
inner join sinf.node_sensor on sensor_state.sensor_id = node_sensor.sensor_id
inner join sinf.room_node on node_sensor.node_id = room_node.node_id
inner join sinf.room on room_node.room_id = room.room_id
inner join sinf.sensor on sensor_state.sensor_id = sensor.sensor_id
where sensor.type = 4
group by room.name)i
