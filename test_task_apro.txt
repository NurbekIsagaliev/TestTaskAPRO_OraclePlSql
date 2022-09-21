Тестовое задание в  компанию  АПРО
/*таблица  passengers*/

create table passengers(passengers_id  number(3),fio varchar2(50) not  null,iin varchar2(12)  not null,age number(3) not null,
telefon_number number(11) not  null,constraint passengers_passengers_id_pk  primary key(passengers_id),
constraint passengers_age_ck check(age>=0));

create  sequence passengers_passengers_id
increment  by 1
start  with 1
maxvalue 1000
nocache
nocycle;

CREATE OR REPLACE TRIGGER "passengers" 
BEFORE INSERT
ON passengers
REFERENCING NEW AS New OLD AS Old
FOR EACH ROW
DECLARE
BEGIN
   SELECT passengers_passengers_id.NEXTVAL INTO :NEW.passengers_id FROM dual;
   null;
END "passengers";



ALTER TABLE passengers
ADD CONSTRAINT passengers_fio_varchar2
CHECK ( REGEXP_LIKE (fio,'\D'));

ALTER TABLE passengers
ADD CONSTRAINT passengers_iin_varchar2
CHECK (  REGEXP_LIKE (iin,'\d')  and length(iin)=12  );


ALTER TABLE passengers
ADD CONSTRAINT passengers_age_number
CHECK (  age>=18  and age<=100  );

ALTER TABLE passengers
ADD CONSTRAINT passengers_telefon_number
CHECK (  REGEXP_LIKE (iin,'\d')  and length(telefon_number)=11  );

ALTER TABLE passengers

ADD CONSTRAINT passengers_unique UNIQUE (iin,telefon_number);

insert into passengers values('isagaliev nurbek bibitovich','891002351206',32,87767790053);
insert into passengers values('myrzabai  adilbek kenjebejkuly','021456564568',26,87787587898);
insert into passengers values('sailauov azamat sailauovich','121545600001',35,87751455488);
insert into passengers values('doimanov  almas kuanyshovich','555689774011',34,87085214456);


describe  passengers;
select *  from  passengers;





/*Таблица stations*/

create  table  stations(stations_id number(2),stations_name varchar2(20)  not  null,constraint stations_stations_id primary key(stations_id) );

create  sequence stations_stations_id
increment  by 1
start  with 1
maxvalue 1000
nocache
nocycle;

insert into stations values(stations_stations_id.nextval,'Oral');
insert into stations values(stations_stations_id.nextval,'Aksai');
insert into stations values(stations_stations_id.nextval,'Aktobe');
insert into stations values(stations_stations_id.nextval,'Kandyagash');
insert into stations values(stations_stations_id.nextval,'Atyrau');
insert into stations values(stations_stations_id.nextval,'Kulsary');
insert into stations values(stations_stations_id.nextval,'Aktau');
insert into stations values(stations_stations_id.nextval,'Mangystau');
insert into stations values(stations_stations_id.nextval,'Nur-Sultan');
insert into stations values(stations_stations_id.nextval,'Kokshetau');
insert into stations values(stations_stations_id.nextval,'Pavlodar');
insert into stations values(stations_stations_id.nextval,'Ekibastuz');
insert into stations values(stations_stations_id.nextval,'Almaty');
insert into stations values(stations_stations_id.nextval,'Taldykorgan');


CREATE OR REPLACE TRIGGER "stations" 
BEFORE INSERT
ON stations
REFERENCING NEW AS New OLD AS Old
FOR EACH ROW
DECLARE
BEGIN
   SELECT stations_stations_id.NEXTVAL INTO :NEW.stations_id FROM dual;
   null;
END "stations";


ALTER TABLE stations

ADD CONSTRAINT stations_unique UNIQUE (stations_name);

ALTER TABLE stations
ADD CONSTRAINT stations_name_varchar2
CHECK ( REGEXP_LIKE (stations_name,'\D'));


describe stations;

select *  from  stations  order by  stations_id;














/* Таблица Routes */

create table routes(routes_id number(3),start_time_route varchar2(5) not null,end_time_route varchar2(5) not null,
count_hours number(3)  not  null,constraint routes_routes_id primary key(routes_id),constraint routes_count_hours_ck check(count_hours>0) );

create  sequence routes_routes_id
increment  by 1
start  with 1
maxvalue 1000
nocache
nocycle;


CREATE OR REPLACE TRIGGER "routes" 
BEFORE INSERT
ON routes 
REFERENCING NEW AS New OLD AS Old
FOR EACH ROW
DECLARE
BEGIN
   SELECT routes_routes_id.NEXTVAL INTO :NEW.routes_id FROM dual;
   null;
END "routes";
commit;
/



ALTER TABLE routes
  ADD station1 number(2);
ALTER TABLE routes
  ADD station2 number(2);
  
ALTER TABLE routes
ADD CONSTRAINT fk_routes_station1
  FOREIGN KEY (station1)
  REFERENCES stations(stations_id);
  
ALTER TABLE routes
ADD CONSTRAINT fk_routes_station2
  FOREIGN KEY (station2)
  REFERENCES stations(stations_id);  

ALTER TABLE routes MODIFY  start_time_route not NULL;
ALTER TABLE routes MODIFY  end_time_route not NULL;
ALTER TABLE routes MODIFY  count_hours not NULL;

ALTER TABLE routes
ADD start_time_route date ; 


ALTER TABLE routes
ADD end_time_route date ; 


ALTER TABLE routes
ADD end_time_route date ; 

ALTER TABLE routes
ADD count_hours number; 

  
insert into routes values(routes_routes_id.nextval,'09-00','22-00',13);
insert into routes values(routes_routes_id.nextval,'10-00','22-00',12);
insert into routes values(routes_routes_id.nextval,'22-00','07-00',21);
insert into routes values(routes_routes_id.nextval,'22-00','07-00',21);
insert into routes values(routes_routes_id.nextval,'21-00','07-00',10);
insert into routes values(routes_routes_id.nextval,'09-00','16-00',55);


CREATE OR REPLACE TRIGGER "diff_time_1" 
BEFORE INSERT  or update
ON routes
REFERENCING NEW AS new OLD AS old
FOR EACH ROW
DECLARE
BEGIN
  if :new.end_time_route is not null and :new.start_time_route is not null then
 :new.count_hours := (:new.end_time_route - :new.start_time_route)*24;

end if;

END "diff_time_1";



describe routes;

select *  from  routes;








/*  Таблица routes-stations*/

create table routes_stations(routes_stations_id number(3),stations_id number(2),routes_id number(3),start_time varchar2(5) not null,
end_time varchar2(5) not null,parking_time number(2)  not  null,constraint route_station_route_station_id primary key(routes_stations_id),
constraint routes_stations_stations_id_fk foreign key(stations_id) references stations(stations_id),
constraint routes_stations_routes_id_fk foreign key(routes_id) references routes(routes_id) );


create  sequence routes_stations_id
increment  by 1
start  with 1
maxvalue 1000
nocache
nocycle;


CREATE OR REPLACE TRIGGER "routes_stations" 
BEFORE INSERT
ON routes_stations
REFERENCING NEW AS New OLD AS Old
FOR EACH ROW
DECLARE
BEGIN
   SELECT routes_stations_id.NEXTVAL INTO :NEW.routes_stations_id FROM dual;
   null;
END "routes_stations";


ALTER TABLE routes_stations
  ADD start_time date;


ALTER TABLE routes_stations
  ADD end_time date;

ALTER TABLE routes_stations
  ADD parking_time number;


CREATE OR REPLACE TRIGGER "diff_time_2" 
BEFORE INSERT  or update
ON routes_stations
REFERENCING NEW AS new OLD AS old
FOR EACH ROW
DECLARE
BEGIN
  if :new.end_time is not null and :new.start_time is not null then
 :new.parking_time := (:new.end_time - :new.start_time)*24;

end if;

END "diff_time_2";


describe routes_stations;
select *  from  routes_stations;

/*4-ое  представление*/

create or  replace  view count_wagon_view
as  select type_wagon,station1,station2,count_wagons
from wagon_types,routes,count_wagons
where
wagon_types.type_id=routes.routes_id ;

describe  count_wagon_view;
select * from  count_wagon_view;








/* 3-е представление*/
create or  replace  view routes_fio
as  select station1,station2,fio
from routes,passengers
where
routes.routes_id=passengers.passengers_id;

describe  routes_fio;
select * from  routes_fio;












/* Таблица  wagon_types*/

create table wagon_types(type_id number(1),type_wagon varchar2(20)  not null,count_place number(3) not null,place_price number(5) not null,
top_places number(3),lower_places number(3),constraint wagon_types_type_id primary key(type_id));



ALTER TABLE wagon_types
ADD CONSTRAINT type_wagon_varchar2
CHECK ( REGEXP_LIKE (type_wagon,'\D'));

ALTER TABLE wagon_types
ADD CONSTRAINT count_place_number3
CHECK ( REGEXP_LIKE (count_place,'\d'));

ALTER TABLE wagon_types
ADD CONSTRAINT place_price_number5
CHECK ( REGEXP_LIKE (place_price,'\d'));

ALTER TABLE wagon_types
ADD CONSTRAINT top_places_number3
CHECK ( REGEXP_LIKE (top_places,'\d'));

ALTER TABLE wagon_types
ADD CONSTRAINT lower_places_number3
CHECK ( REGEXP_LIKE (lower_places,'\d'));

alter table wagon_types drop constraint type_wagon_varchar2;

describe wagon_types;
select *  from  wagon_types;





/* Таблица  count_wagons*/

create  table count_wagons(count_wagons_id number(3),type_id number(1),routes_id number(3),count_wagons number(4) not null,
CONSTRAINT count_wagons_id_pk  primary key(count_wagons_id),
constraint count_wagons_type_id_fk foreign key(type_id) references wagon_types(type_id) ,
constraint count_wagons_routes_id_fk foreign key(routes_id) references routes(routes_id) );



ALTER TABLE count_wagons
ADD CONSTRAINT count_wagons_number
CHECK (  REGEXP_LIKE ( count_wagons,'\d'));


create  sequence count_wagons_id
increment  by 1
start  with 1
maxvalue 1000
nocache
nocycle;

CREATE OR REPLACE TRIGGER "count_wagons" 
BEFORE INSERT or  update
ON count_wagons
REFERENCING NEW AS New OLD AS Old
FOR EACH ROW
DECLARE
BEGIN
   SELECT count_wagons_id.NEXTVAL INTO :NEW.count_wagons_id FROM dual;
   null;
END "count_wagons";



describe  count_wagons;

select *  from  count_wagons;


/*1-ое представление*/
create or  replace  view type_wagon_routes_count_wagons
as  select station1,station2,type_wagon,count_wagons
from wagon_types,routes,count_wagons
where
wagon_types.type_id=COUNT_WAGONS.COUNT_WAGONS_id  and
wagon_types.type_id=ROUTES.ROUTES_ID;

describe type_wagon_routes_count_wagons;
select * from  type_wagon_routes_count_wagons;


/*2-ое представление*/
create or  replace  view type_wagon_routes_passengers
as  select station1,station2,type_wagon,fio
from wagon_types,routes,passengers
where
wagon_types.type_id=passengers.passengers_id  and
wagon_types.type_id=ROUTES.ROUTES_ID;

describe type_wagon_routes_passengers;
select * from  type_wagon_routes_passengers;








/*Таблица time_table*/


create  table time_table(time_table_id number(3),routes_id number(3),date_routes date not null,
constraint time_table_id_pk primary  key(time_table_id),
constraint time_table_routes_id_fk foreign key(routes_id) references routes(routes_id) );


create  sequence time_table_id
increment  by 1
start  with 1
maxvalue 1000
nocache
nocycle;

CREATE OR REPLACE TRIGGER "time_table" 
BEFORE INSERT or  update
ON time_table
REFERENCING NEW AS New OLD AS Old
FOR EACH ROW
DECLARE
BEGIN
   SELECT time_table_id.NEXTVAL INTO :NEW.time_table_id FROM dual;
   null;
END "time_table";



describe time_table;
select *  from  time_table;








/*Таблица ticket_prices*/



create  table  ticket_prices(ticket_prices_id number(2),routes_id number(3),type_id number(1),is_top number(2),is_lower number(2),
number_of_seat number(2)  not  null,price number(6) not  null,
constraint places_seats_id_pk primary key(ticket_prices_id),
constraint places_routes_id_fk foreign key(routes_id)  references routes(routes_id),
constraint places_type_id_fk foreign key(type_id)  references wagon_types(type_id));


create  sequence places_id
increment  by 1
start  with 1
maxvalue 1000
nocache
nocycle;


CREATE OR REPLACE TRIGGER "ticket_prices" 
BEFORE INSERT or  update
ON ticket_prices
REFERENCING NEW AS New OLD AS Old
FOR EACH ROW
DECLARE
BEGIN
 
   SELECT places_id.NEXTVAL into :NEW.ticket_prices_id FROM dual;
   null;
END "ticket_prices";



ALTER TABLE ticket_prices
ADD CONSTRAINT is_top_number2
CHECK (  REGEXP_LIKE ( is_top,'\d'));

ALTER TABLE ticket_prices
ADD CONSTRAINT is_lower_number2
CHECK (  REGEXP_LIKE ( is_lower,'\d'));


ALTER TABLE ticket_prices
ADD CONSTRAINT number_of_seat_number2
CHECK (  REGEXP_LIKE ( number_of_seat,'\d'));

ALTER TABLE ticket_prices
ADD CONSTRAINT price_number6
CHECK (  REGEXP_LIKE ( price,'\d'));




describe ticket_prices;
select *  from  ticket_prices;








/* Таблица tickets*/

create  table tickets(tickets_id number(3), passengers_id number(3),time_table_id number(3),
constraint tickets_tickets_id_pk primary key(tickets_id),
constraint tickets_passengers_id_fk foreign key(passengers_id) references passengers(passengers_id),
constraint tickets_time_table_id_fk foreign key(time_table_id) references time_table(time_table_id) );



ALTER TABLE tickets
ADD ticket_prices_id number(2);

ALTER TABLE tickets
ADD CONSTRAINT tickets_id_ticket_prices_id_fk
FOREIGN KEY (ticket_prices_id)
REFERENCES ticket_prices(ticket_prices_id);


create  sequence tickets_id
increment  by 1
start  with 1
maxvalue 1000
nocache
nocycle;

CREATE OR REPLACE TRIGGER "tickets" 
BEFORE INSERT or  update
ON tickets
REFERENCING NEW AS New OLD AS Old
FOR EACH ROW
DECLARE
BEGIN
   SELECT tickets_id.NEXTVAL INTO :NEW.tickets_id FROM dual;
   null;
END "tickets";




describe tickets;
select *  from  tickets; 





