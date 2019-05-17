```
CREATE TABLE actuator (
actuator_id SERIAL PRIMARY KEY,
type integer,
pixel_id integer NOT NULL UNIQUE REFERENCES pixel(pixel_id)
);

CREATE TABLE sensor (
sensor_id SERIAL PRIMARY KEY,
type integer,
pixel_id integer NOT NULL UNIQUE REFERENCES pixel(pixel_id)
);

CREATE TABLE node (
node_id SERIAL PRIMARY KEY
);

CREATE TABLE rules (
rules_id SERIAL PRIMARY KEY,
operation integer,
value integer
rules_id integer REFERENCES rules(rules_id)
);

CREATE TABLE pixel (
pixel_id SERIAL PRIMARY KEY,
x_position integer,
y_position integer
);

CREATE TABLE room (
room_id SERIAL PRIMARY KEY,
name varchar(30)
room_id integer REFERENCES room(room_id)
);

CREATE TABLE node_actuator (
node_actuator_id SERIAL PRIMARY KEY,
start_date date,
end_date date,
node_id integer REFERENCES node(node_id),
actuator_id integer REFERENCES actuator(actuator_id)
);

CREATE TABLE node_sensor (
node_sensor_id SERIAL PRIMARY KEY,
start_date date,
end_date date,
node_id integer REFERENCES node(node_id),
sensor_id integer REFERENCES sensor(sensor_id)
);

CREATE TABLE node_room (
node_id integer REFERENCES node(node_id),
room_id integer REFERENCES room(room_id),
start_date date,
end_date date
);

CREATE TABLE sensor_rules (
sensor_id integer REFERENCES sensor(sensor_id),
rules_id integer REFERENCES rules(rules_id)
);

CREATE TABLE actuator_rules (
actuator_id integer REFERENCES actuator(actuator_id),
rules_id integer REFERENCES rules(rules_id)
);

CREATE TABLE profiles (
profiles_id SERIAL PRIMARY KEY,
start_date date,
end_date date
);

CREATE TABLE actuator_state (
actuator_state_id SERIAL PRIMARY KEY,
actuator_id integer REFERENCES actuator(actuator_id),
actuator_value integer,
log_timestamp date
);

CREATE TABLE sensor_state (
sensor_state_id SERIAL PRIMARY KEY,
sensor_id integer REFERENCES sensor(sensor_id),
sensor_value integer,
log_timestamp date
);

CREATE TABLE profiles_rules (
profiles_id integer REFERENCES profiles(profiles_id),
rules_id integer REFERENCES rules(rules_id)
);

CREATE TABLE actuator_state_actuator (
actuator_state_id integer REFERENCES actuator_state(actuator_state_id),
actuator_id integer REFERENCES actuator(actuator_id)
);

CREATE TABLE sensor_state_sensor (
sensor_state_id integer REFERENCES sensor_state(sensor_state_id),
sensor_id integer REFERENCES sensor(sensor_id)
);
```