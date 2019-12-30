# Cricket Analytics


https://cricstat.in

## Features
       Able to see all the players
       
### Feature 1:List the name of the players

---sql

create table player_list (Cap_No varchar2(5),Player_Name varchar2(40) not null,Nation varchar2(20) not null,Debut int not null,
constraint Cap_No_pk primary key (Cap_No));
---

---sql

insert into player_list values ('I175','VIRAT_KOHLI','India',2008);
insert into player_list values ('SA78','AB_DE_VILLIIERS','South_Africa',2005);
insert into player_list values ('A197','A0RON_FINCH','Australia',2013);
insert into player_list values ('I220','WASHINGTON_SUNDAR','India',2017);
insert into player_list values ('I211','CHAHAL','India',2016);
insert into player_list values ('E232','MOEEN_ALI','England',2014);
insert into player_list values ('SA82','DALE_STEYN','South_Africa',2005);
select * from player_list;

---
