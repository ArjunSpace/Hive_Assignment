table creation 

    create table parking_voilations(
    Summons_number int,
    plate_id string,
    Registration_State string,
    plate_type string,
    issue_date Date,
    voilation_code int,
    vehicle_body_type string,
    vehicle_make string,
    issuing_agency string,
    street_code1 int,
    street_code2 int,
    street_code3 int,
    vehicle_expire_date string,
    voilation_location int,
    voilation_precinct int,
    issuer_precinct int,
    issuer_code int,
    issuer_command string,
    issuer_squad string,
    voilation_time string,
    time_first_observed string,
    voilation_county string,
    voilation_infront_opposite string,
    house_number string,
    street_name string,
    intersecting_street string,
    date_first_observed string,
    law_section int,
    sub_division string,
    voilation_legal_code string,
    days_parking_effect string,
    from_hours_ineffect string,
    to_hours_ineffect string,
    vehicle_colour string,
    unregistered_vehicle string,
    vehicle_year int,
    meter_number string,
    feet_from_curb int,
    voilation_post_code string,
    voilation_description string)
    row format delimited 
    fields terminated by ','
    tblproperties("skip.header.line.count"="1");
    
    
 Data loading
 
      load data inpath '/data/parking.csv' into table parking_voilations
      
 Partitioning 
    create table parking_voilations_part(
    Summons_number int,
    plate_id string,
    Registration_State string,
    plate_type string,
    issue_date Date,
    vehicle_body_type string,
    vehicle_make string,
    issuing_agency string,
    street_code1 int,
    street_code2 int,
    street_code3 int,
    vehicle_expire_date string,
    voilation_location int,
    voilation_precinct int,
    issuer_precinct int,
    issuer_code int,
    issuer_command string,
    issuer_squad string,
    voilation_time string,
    time_first_observed string,
    voilation_county string,
    voilation_infront_opposite string,
    house_number string,
    street_name string,
    intersecting_street string,
    date_first_observed string,
    law_section int,
    sub_division string,
    voilation_legal_code string,
    days_parking_effect string,
    from_hours_ineffect string,
    to_hours_ineffect string,
    vehicle_colour string,
    unregistered_vehicle string,
    vehicle_year int,
    meter_number string,
    feet_from_curb int,
    voilation_post_code string,
    voilation_description string)
    partitioned by (voilation_code int);
    
 
 
