detection_catcher_readme.md

Introduction
------------

Detection_catcher is an application which collects TED detections that come in from Kafka through Tomcat and writes them to a postgres database.

Installation and Dependencies
-----------------------------

The ted conda environment must be activated for whichever user is running PDL. To activate this environment, type:
    source activate ted
If the environment has not been created yet or does not exist, type:
    ./install.sh 

Detection_catcher requires that the detection_ext Postgres table exists. If this table has not been created, first activate the desired PostgreSQL database from the terminal by typing:

    psql -d <DB_name> -U <DB_user> -p <DB_port>

And entering the same password that is used for the TED Dev database.

Create the detection_ext table by typing:

    create sequence detection_ext_id_seq;
    create table detection_ext(
        id bigint not null primary key default nextval('detection_ext_id_seq'),
        detection_id bigint not null,
        detection_lat numeric(8,5),
        detection_lon numeric(8,5),
        detection_time timestamp without time zone,
        first_trigger_time timestamp without time zone
    );


Detection_catcher has been designed to run in Python 3.

Running detection_catcher
-------------------------

To run detection_catcher, first copy over detection_catcher from ./bin into ~/tedapp. Make sure that trigger_funcs.py has been copied from ./ted into ~/tedapp.

The config file for detection_catcher must exist in ~/tedapp and be named catcher_config.ini. An example catcher_config.ini can be found in this Git repository under ./exampleConfigFiles. The following pieces of information must be updated in the example catcher_config.ini to use it with detection_catcher:

    consumer_name       Kafka consumer name
    group_id            group_id for Kafka consumer
    bootstrap_server    bootstrap_server for Kafka consumer
    db_ip               IP address of the Postgres database
    db_port             port number of the Postgres database
    db_name             name of the Postgres database
    db_user             username for the Postgres database
    db_password         password for the Postgres database

Run detection_catcher as a background process by typing:

    python detection_catcher &
