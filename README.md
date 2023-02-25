Complete the tasks below:
1. You should create a database named universe

1. Be sure to connect to your database with \c universe. Then, you should add tables named galaxy, star, planet, and moon

1. Each table should have a primary key

1. Each primary key should automatically increment

1. Each table should have a name column

1. You should use the INT data type for at least two columns that are not a primary or foreign key

1. You should use the NUMERIC data type at least once

1. You should use the TEXT data type at least once

1. You should use the BOOLEAN data type on at least two columns

1. Each "star" should have a foreign key that references one of the rows in galaxy

1. Each "planet" should have a foreign key that references one of the rows in star

1. Each "moon" should have a foreign key that references one of the rows in planet

1. Your database should have at least five tables

1. Each table should have at least three rows

1. The galaxy and star tables should each have at least six rows

1. The planet table should have at least 12 rows

1. The moon table should have at least 20 rows

1. Each table should have at least three columns

1. The galaxy, star, planet, and moon tables should each have at least five columns

1. At least two columns per table should not accept NULL values

1. At least one column from each table should be required to be UNIQUE

1. All columns named name should be of type VARCHAR

1. Each primary key column should follow the naming convention table_name_id. For example, the moon table should have a primary key column named moon_id

1. Each foreign key column should have the same name as the column it is referencing

# Creating Tables
**How to do solve?**

- First, type `\c universe` to connect `universe` command.
- Creating `galaxy` main table:
```sql
CREATE TABLE galaxy (galaxy_id SERIAL PRIMARY KEY NOT NULL, speed INT, description TEXT);
```
Output:
```txt
List of relations
+--------+----------------------+----------+--------------+
| Schema |         Name         |   Type   |    Owner     |
+--------+----------------------+----------+--------------+
| public | galaxy               | table    | freecodecamp |
| public | galaxy_galaxy_id_seq | sequence | freecodecamp |
+--------+----------------------+----------+--------------+
(2 rows)
```
Type `\d galaxy`:
```txt
Table "public.galaxy"
+-------------+---------+-----------+----------+-------------------------------------------+
|   Column    |  Type   | Collation | Nullable |                  Default                  |
+-------------+---------+-----------+----------+-------------------------------------------+
| galaxy_id   | integer |           | not null | nextval('galaxy_galaxy_id_seq'::regclass) |
| speed       | integer |           |          |                                           |
| description | text    |           |          |                                           |
+-------------+---------+-----------+----------+-------------------------------------------+
Indexes:
    "galaxy_pkey" PRIMARY KEY, btree (galaxy_id)
```
- Create the `star` table:
```sql
CREATE TABLE star(star_id SERIAL PRIMARY KEY NOT NULL, radius INT NOT NULL, color VARCHAR(255) NOT NULL, name VARCHAR(255) NOT NULL, galaxy_id INT, CONSTRAINT fk_galaxy FOREIGN KEY(galaxy_id) REFERENCES galaxy(galaxy_id));
```
Output: type `\d star`:
```txt
Table "public.star"
+-----------+------------------------+-----------+----------+---------------------------------------+
|  Column   |          Type          | Collation | Nullable |                Default                |
+-----------+------------------------+-----------+----------+---------------------------------------+
| star_id   | integer                |           | not null | nextval('star_star_id_seq'::regclass) |
| radius    | integer                |           | not null |                                       |
| color     | character varying(255) |           | not null |                                       |
| name      | character varying(255) |           | not null |                                       |
| galaxy_id | integer                |           |          |                                       |
+-----------+------------------------+-----------+----------+---------------------------------------+
Indexes:
    "star_pkey" PRIMARY KEY, btree (star_id)
Foreign-key constraints:
    "fk_galaxy" FOREIGN KEY (galaxy_id) REFERENCES galaxy(galaxy_id)
```
- Add `name` column not null:
```sql
ALTER TABLE galaxy ADD COLUMN name VARCHAR(255) NOT NULL;
```
Output:
```txt
Table "public.galaxy"
+-------------+------------------------+-----------+----------+-------------------------------------------+
|   Column    |          Type          | Collation | Nullable |                  Default                  |
+-------------+------------------------+-----------+----------+-------------------------------------------+
| galaxy_id   | integer                |           | not null | nextval('galaxy_galaxy_id_seq'::regclass) |
| speed       | integer                |           |          |                                           |
| description | text                   |           |          |                                           |
| name        | character varying(255) |           | not null |                                           |
+-------------+------------------------+-----------+----------+-------------------------------------------+
Indexes:
    "galaxy_pkey" PRIMARY KEY, btree (galaxy_id)
Referenced by:
    TABLE "star" CONSTRAINT "fk_galaxy" FOREIGN KEY (galaxy_id) REFERENCES galaxy(galaxy_id)
```
- Create the `planet` table:
```sql
CREATE TABLE planet(planet_id SERIAL PRIMARY KEY NOT NULL, name VARCHAR(255) NOT NULL, amount_of_people NUMERIC, time_travel boolean DEFAULT(false) NOT NULL, star_id INT NOT NULL, CONSTRAINT fk_star FOREIGN KEY(star_id) REFERENCES star(star_id));
```
Output: type `\d planet`
```txt
Table "public.planet"
+------------------+------------------------+-----------+----------+-------------------------------------------+
|      Column      |          Type          | Collation | Nullable |                  Default                  |
+------------------+------------------------+-----------+----------+-------------------------------------------+
| planet_id        | integer                |           | not null | nextval('planet_planet_id_seq'::regclass) |
| name             | character varying(255) |           | not null |                                           |
| amount_of_people | numeric                |           |          |                                           |
| time_travel      | boolean                |           | not null | false                                     |
| star_id          | integer                |           | not null |                                           |
+------------------+------------------------+-----------+----------+-------------------------------------------+
Indexes:
    "planet_pkey" PRIMARY KEY, btree (planet_id)
Foreign-key constraints:
    "fk_star" FOREIGN KEY (star_id) REFERENCES star(star_id)
```
- Create the `moon` table:
```sql
CREATE TABLE moon(moon_id SERIAL PRIMARY KEY NOT NULL, name VARCHAR(255) NOT NULL, name_code VARCHAR(255) UNIQUE, has_water boolean NOT NULL, planet_id INT NOT NULL, CONSTRAINT fk_planet FOREIGN KEY (planet_id) REFERENCES planet(planet_id));
```
Output: Type `\d moon`:
```txt
Table "public.moon"
+-----------+------------------------+-----------+----------+---------------------------------------+
|  Column   |          Type          | Collation | Nullable |                Default                |
+-----------+------------------------+-----------+----------+---------------------------------------+
| moon_id   | integer                |           | not null | nextval('moon_moon_id_seq'::regclass) |
| name      | character varying(255) |           | not null |                                       |
| name_code | character varying(255) |           |          |                                       |
| has_water | boolean                |           | not null |                                       |
| planet_id | integer                |           | not null |                                       |
+-----------+------------------------+-----------+----------+---------------------------------------+
Indexes:
    "moon_pkey" PRIMARY KEY, btree (moon_id)
    "moon_name_code_key" UNIQUE CONSTRAINT, btree (name_code)
Foreign-key constraints:
    "fk_planet" FOREIGN KEY (planet_id) REFERENCES planet(planet_id)
```
- Drop column `name_code` then add again column
```sql
universe=> ALTER TABLE moon DROP COLUMN name_code;
ALTER TABLE
universe=> ALTER TABLE moon ADD COLUMN name_code VARCHAR(255) UNIQUE NOT NULL;
ALTER TABLE
```
Output:
```txt
Table "public.moon"
+-----------+------------------------+-----------+----------+---------------------------------------+
|  Column   |          Type          | Collation | Nullable |                Default                |
+-----------+------------------------+-----------+----------+---------------------------------------+
| moon_id   | integer                |           | not null | nextval('moon_moon_id_seq'::regclass) |
| name      | character varying(255) |           | not null |                                       |
| has_water | boolean                |           | not null |                                       |
| planet_id | integer                |           | not null |                                       |
| name_code | character varying(255) |           | not null |                                       |
+-----------+------------------------+-----------+----------+---------------------------------------+
Indexes:
    "moon_pkey" PRIMARY KEY, btree (moon_id)
    "moon_name_code_key" UNIQUE CONSTRAINT, btree (name_code)
Foreign-key constraints:
    "fk_planet" FOREIGN KEY (planet_id) REFERENCES planet(planet_id)
```
- Create the `blackhole` table:
```sql
CREATE TABLE blackhole(blackhole_id SERIAL PRIMARY KEY NOT NULL, gravity INT, galaxy_id INT, wormhole BOOLEAN DEFAULT(false) NOT NULL);
```
Output: Type `\d blackhole`:
```txt
universe=> \d blackhole
                                     Table "public.blackhole"
+--------------+---------+-----------+----------+-------------------------------------------------+
|    Column    |  Type   | Collation | Nullable |                     Default                     |
+--------------+---------+-----------+----------+-------------------------------------------------+
| blackhole_id | integer |           | not null | nextval('blackhole_blackhole_id_seq'::regclass) |
| gravity      | integer |           |          |                                                 |
| galaxy_id    | integer |           |          |                                                 |
| wormhole     | boolean |           | not null | false                                           |
+--------------+---------+-----------+----------+-------------------------------------------------+
Indexes:
    "blackhole_pkey" PRIMARY KEY, btree (blackhole_id)
```
Let's check all tables
```txt
universe=> \d
                        List of relations
+--------+----------------------------+----------+--------------+
| Schema |            Name            |   Type   |    Owner     |
+--------+----------------------------+----------+--------------+
| public | blackhole                  | table    | freecodecamp |
| public | blackhole_blackhole_id_seq | sequence | freecodecamp |
| public | galaxy                     | table    | freecodecamp |
| public | galaxy_galaxy_id_seq       | sequence | freecodecamp |
| public | moon                       | table    | freecodecamp |
| public | moon_moon_id_seq           | sequence | freecodecamp |
| public | planet                     | table    | freecodecamp |
| public | planet_planet_id_seq       | sequence | freecodecamp |
| public | star                       | table    | freecodecamp |
| public | star_star_id_seq           | sequence | freecodecamp |
+--------+----------------------------+----------+--------------+
(10 rows)
```
# Inserting Data
## Inserting Galaxys Data
```sql
INSERT INTO galaxy(name) VALUES('andromeda');
```
Output:
```txt
universe=> INSERT INTO galaxy(name) VALUES('andromeda');
INSERT 0 1
universe=> SELECT * FROM galaxy;
+-----------+-------+-------------+-----------+
| galaxy_id | speed | description |   name    |
+-----------+-------+-------------+-----------+
|         1 |       |             | andromeda |
+-----------+-------+-------------+-----------+
(1 row)
```
Let's create name galaxys any:
```sql
INSERT INTO galaxy(name) VALUES('galaxy2');
INSERT INTO galaxy(name) VALUES('galaxy3');
INSERT INTO galaxy(name) VALUES('galaxy4');
INSERT INTO galaxy(name) VALUES('galaxy5');
INSERT INTO galaxy(name) VALUES('galaxy6');
```
Output:
```txt
universe=> SELECT * FROM galaxy;
+-----------+-------+-------------+-----------+
| galaxy_id | speed | description |   name    |
+-----------+-------+-------------+-----------+
|         1 |       |             | andromeda |
|         2 |       |             | galaxy2   |
|         3 |       |             | galaxy3   |
|         4 |       |             | galaxy4   |
|         5 |       |             | galaxy5   |
|         6 |       |             | galaxy6   |
+-----------+-------+-------------+-----------+
(6 rows)
```
## Inserting Stars Data
```sql
INSERT INTO star(radius, color, name, galaxy_id) VALUES(123461234, 'red', 'beatlejuice', 1);
```
Testing: Type `SELECT * FROM star;` command.
```txt
universe=> SELECT * FROM star;
+---------+-----------+-------+-------------+-----------+
| star_id |  radius   | color |    name     | galaxy_id |
+---------+-----------+-------+-------------+-----------+
|       1 | 123461234 | red   | beatlejuice |         1 |
+---------+-----------+-------+-------------+-----------+
(1 row)
```
Let's create these stars
```sql
INSERT INTO star(radius, color, name, galaxy_id) VALUES(123461234, 'yellow', 'joe', 1);
INSERT INTO star(radius, color, name, galaxy_id) VALUES(123461234, 'yellow', 'gary', 1);
INSERT INTO star(radius, color, name, galaxy_id) VALUES(123461234, 'yellow', 'big yellow', 1);
INSERT INTO star(radius, color, name, galaxy_id) VALUES(123461234, 'orange', 'small orange', 1);
INSERT INTO star(radius, color, name, galaxy_id) VALUES(123461234, 'orange', 'star in galaxy 2', 2);
```
Output:
```txt
universe=> SELECT * FROM star;
+---------+-----------+--------+------------------+-----------+
| star_id |  radius   | color  |       name       | galaxy_id |
+---------+-----------+--------+------------------+-----------+
|       1 | 123461234 | red    | beatlejuice      |         1 |
|       2 | 123461234 | yellow | joe              |         1 |
|       3 | 123461234 | yellow | gary             |         1 |
|       4 | 123461234 | yellow | big yellow       |         1 |
|       5 | 123461234 | orange | small orange     |         1 |
|       6 | 123461234 | orange | star in galaxy 2 |         2 |
+---------+-----------+--------+------------------+-----------+
(6 rows)
```
## Inserting Planets Data
```sql
INSERT INTO planet(name, star_id) VALUES('earth', 1);
INSERT INTO planet(name, star_id) VALUES('mars', 1);
INSERT INTO planet(name, star_id) VALUES('neptune', 1);
INSERT INTO planet(name, star_id) VALUES('jupiter', 1);
INSERT INTO planet(name, star_id) VALUES('unarnus', 1);
INSERT INTO planet(name, star_id) VALUES('venus', 1);
INSERT INTO planet(name, star_id) VALUES('mercury', 1);
INSERT INTO planet(name, star_id) VALUES('saturn', 1);
INSERT INTO planet(name, star_id) VALUES('pluto', 1);
INSERT INTO planet(name, star_id) VALUES('yolo', 1);
INSERT INTO planet(name, star_id) VALUES('jalala', 2);
INSERT INTO planet(name, star_id) VALUES('malala', 2);
INSERT INTO planet(name, star_id) VALUES('kento', 3);
```
Output: Type `SELECT * FROM planet;` command.
```txt
universe=> SELECT * FROM planet;
+-----------+---------+------------------+-------------+---------+
| planet_id |  name   | amount_of_people | time_travel | star_id |
+-----------+---------+------------------+-------------+---------+
|         1 | earth   |                  | f           |       1 |
|         2 | mars    |                  | f           |       1 |
|         3 | neptune |                  | f           |       1 |
|         4 | jupiter |                  | f           |       1 |
|         5 | unarnus |                  | f           |       1 |
|         6 | venus   |                  | f           |       1 |
|         7 | mercury |                  | f           |       1 |
|         8 | saturn  |                  | f           |       1 |
|         9 | pluto   |                  | f           |       1 |
|        10 | yolo    |                  | f           |       1 |
|        11 | jalala  |                  | f           |       2 |
|        12 | malala  |                  | f           |       2 |
|        13 | kento   |                  | f           |       3 |
+-----------+---------+------------------+-------------+---------+
(13 rows)
```
## Inserting Moons Data
```sql
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon1', true, 2, 'moon1');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon2', true, 3, 'moon2');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon3');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon4');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon5');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon6');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon7');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon8');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon9');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon10');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon11');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon13');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon14');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon12');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon15');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon16');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon17');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon18');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon19');
INSERT INTO moon(name, has_water, planet_id, name_code) VALUES ('moon3', true, 4, 'moon20');
```
Output: Type `SELECT * FROM moon;` command.
```txt
universe=> SELECT * FROM moon;
+---------+-------+-----------+-----------+-----------+
| moon_id | name  | has_water | planet_id | name_code |
+---------+-------+-----------+-----------+-----------+
|       1 | moon1 | t         |         2 | moon1     |
|       2 | moon2 | t         |         3 | moon2     |
|       3 | moon3 | t         |         4 | moon3     |
|       4 | moon3 | t         |         4 | moon4     |
|       5 | moon3 | t         |         4 | moon5     |
|       6 | moon3 | t         |         4 | moon6     |
|       7 | moon3 | t         |         4 | moon7     |
|       8 | moon3 | t         |         4 | moon8     |
|       9 | moon3 | t         |         4 | moon9     |
|      10 | moon3 | t         |         4 | moon10    |
|      11 | moon3 | t         |         4 | moon11    |
|      12 | moon3 | t         |         4 | moon13    |
|      13 | moon3 | t         |         4 | moon14    |
|      14 | moon3 | t         |         4 | moon12    |
|      15 | moon3 | t         |         4 | moon15    |
|      16 | moon3 | t         |         4 | moon16    |
|      17 | moon3 | t         |         4 | moon17    |
|      18 | moon3 | t         |         4 | moon18    |
|      19 | moon3 | t         |         4 | moon19    |
|      20 | moon3 | t         |         4 | moon20    |
+---------+-------+-----------+-----------+-----------+
(20 rows)
```
# Fixing a few tables
- Add column `rotation_speed` and set default is 100000.
```txt
universe=> ALTER TABLE galaxy ADD COLUMN rotation_speed INT NOT NULL DEFAULT(100000);
ALTER TABLE
universe=> SELECT * FROM galaxy;
+-----------+-------+-------------+-----------+----------------+
| galaxy_id | speed | description |   name    | rotation_speed |
+-----------+-------+-------------+-----------+----------------+
|         1 |       |             | andromeda |         100000 |
|         2 |       |             | galaxy2   |         100000 |
|         3 |       |             | galaxy3   |         100000 |
|         4 |       |             | galaxy4   |         100000 |
|         5 |       |             | galaxy5   |         100000 |
|         6 |       |             | galaxy6   |         100000 |
+-----------+-------+-------------+-----------+----------------+
(6 rows)
```
# Exporting the database
You must recommended use Github interface to upload source code.
# Running and fixing tests
As you can see, there was error: SUBTASKS 1.1 :5 All tables should have a "name" column
```txt
universe=> \d blackhole
                                     Table "public.blackhole"
+--------------+---------+-----------+----------+-------------------------------------------------+
|    Column    |  Type   | Collation | Nullable |                     Default                     |
+--------------+---------+-----------+----------+-------------------------------------------------+
| blackhole_id | integer |           | not null | nextval('blackhole_blackhole_id_seq'::regclass) |
| gravity      | integer |           |          |                                                 |
| galaxy_id    | integer |           |          |                                                 |
| wormhole     | boolean |           | not null | false                                           |
+--------------+---------+-----------+----------+-------------------------------------------------+
Indexes:
    "blackhole_pkey" PRIMARY KEY, btree (blackhole_id)
```
*How do I fix?*
```sql
ALTER TABLE blackhole ADD COLUMN name VARCHAR(255) NOT NULL UNIQUE;
```
Output:
```txt
universe=> ALTER TABLE blackhole ADD COLUMN name VARCHAR(255) NOT NULL UNIQUE;
ALTER TABLE
universe=> \d blackhole
                                             Table "public.blackhole"
+--------------+------------------------+-----------+----------+-------------------------------------------------+
|    Column    |          Type          | Collation | Nullable |                     Default                     |
+--------------+------------------------+-----------+----------+-------------------------------------------------+
| blackhole_id | integer                |           | not null | nextval('blackhole_blackhole_id_seq'::regclass) |
| gravity      | integer                |           |          |                                                 |
| galaxy_id    | integer                |           |          |                                                 |
| wormhole     | boolean                |           | not null | false                                           |
| name         | character varying(255) |           | not null |                                                 |
+--------------+------------------------+-----------+----------+-------------------------------------------------+
Indexes:
    "blackhole_pkey" PRIMARY KEY, btree (blackhole_id)
    "blackhole_name_key" UNIQUE CONSTRAINT, btree (name)
```
Let's create 3 blackholes
```sql
INSERT INTO blackhole(name) VALUES ('bh1');
INSERT INTO blackhole(name) VALUES ('bh2');
INSERT INTO blackhole(name) VALUES ('bh3');
```
As you can see, there is only a requirement: At least one column from each table should be required to be `UNIQUE`.

*How do I fix?*

Type following command here:
```sql
ALTER TABLE galaxy ADD CONSTRAINT name_unique UNIQUE (name);
```
```txt
universe=> ALTER TABLE galaxy ADD CONSTRAINT name_unique UNIQUE (name);
ALTER TABLE
universe=> \d galaxy
                                            Table "public.galaxy"
+----------------+------------------------+-----------+----------+-------------------------------------------+
|     Column     |          Type          | Collation | Nullable |                  Default                  |
+----------------+------------------------+-----------+----------+-------------------------------------------+
| galaxy_id      | integer                |           | not null | nextval('galaxy_galaxy_id_seq'::regclass) |
| speed          | integer                |           |          |                                           |
| description    | text                   |           |          |                                           |
| name           | character varying(255) |           | not null |                                           |
| rotation_speed | integer                |           | not null | 100000                                    |
+----------------+------------------------+-----------+----------+-------------------------------------------+
Indexes:
    "galaxy_pkey" PRIMARY KEY, btree (galaxy_id)
    "name_unique" UNIQUE CONSTRAINT, btree (name)
Referenced by:
    TABLE "star" CONSTRAINT "fk_galaxy" FOREIGN KEY (galaxy_id) REFERENCES galaxy(galaxy_id)
```
Let's add 2 constraints for `star` and `planet` tables:
```sql
ALTER TABLE star ADD CONSTRAINT name_unique_star UNIQUE (name);
ALTER TABLE planet ADD CONSTRAINT name_unique_planet UNIQUE (name);
```
Tutorial Complete! Congratulations!!!
