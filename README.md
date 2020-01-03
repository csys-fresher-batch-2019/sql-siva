# CRICKET ANALYTICS

## Features
       Able to see all the players
       
### Feature 1:List the name of the players

```sql
--drop table player_list;

create table player_list(
cap_no varchar2(5),
player_name varchar2(40) not null,
nation varchar2(20) not null,
batting_style varchar2(30) not null,
constraint cap_no_pk primary key (cap_no),
constraint nation_ck check (nation in('India','Australia','South Africa','England','Pakistan')));

update player_list set cap_no = 'e226' where player_name = 'Jos Buttler'
--drop table player_list

select * from player_list


insert into player_list values ('i175','Virat Kohli','India','Right Hand Batsman');
insert into player_list values ('sa78','De Villiers','South Africa','Right Hand Batsman');
insert into player_list values ('sa17','Jonty Rhodes','South Africa','Right Hand Batsman');
insert into player_list values ('a197','Aoron Finch','Australia','Right Hand Batsman');
insert into player_list values ('i158','Mahendra Singh Dhoni','India','Right Hand Batsman');
insert into player_list values ('a123','Ricky Ponting','Australia','Right Hand Batsman');
insert into player_list values ('e226','Jos Buttler','England','Right Hand Batsman');
insert into player_list values ('p203','Babar Azam','Pakistan','Right Hand Batsman');
select * from player_list;
--delete player_list;
```
### Feature 2;Batting career  

```SQL

select * from player_list
drop table  player_career;
create table player_career
( cap_no varchar2(5),
match_id varchar2(5) not null,
matches number not null,
runs number not null,
fifty number not null,
hundred number not null,
high_score number not null,
constraint cap_no_fk foreign key(cap_no) references player_list(cap_no),
constraint match_id_ck check (match_id in ('test','odi','t20')),
constraint matches_ck check (matches>0),
constraint runs_ck check (runs>=0),
constraint fifty_ck check (fifty>=0),
constraint hundred_ck check (hundred>=0),
constraint high_score_ck check (high_score>=0)
);
```
```SQL
insert into player_career values ('i175','odi',242,11609,55,43,183);
insert into player_career values ('i175','test',84,7202,22,27,254);
insert into player_career values ('i175','t20',75,2633,24,0,94);
insert into player_career values ('i158','test',90,4876,33,8,224);
insert into player_career values ('i158','odi',350,10733,73,10,183);
insert into player_career values ('i158','t20',98,1617,2,0,56);
insert into player_career values ('sa78','odi',228,9577,53,25,176);
insert into player_career values ('sa78','test',114,8765,46,22,278);
insert into player_career values ('sa78','t20',78,1678,10,0,79);
insert into player_career values ('a123','odi',375,13704,82,30,164);
insert into player_career values ('a123','test',168,13378,62,41,257);
insert into player_career values ('a123','t20',17,401,2,0,98);
insert into player_career values ('e226','odi',141,3843,20,9,150);
insert into player_career values ('e226','test',37,2012,15,1,106);
insert into player_career values ('sa78','t20',66,1260,7,0,73);
insert into player_career values ('sa78','odi',228,9577,53,25,176);
insert into player_career values ('sa17','odi',245,5935,33,2,122);
insert into player_career values ('sa17','test',52,2352,17,3,117);
insert into player_career values ('sa78','t20',228,9577,53,25,176);
insert into player_career values ('a197','odi',119,4559,24,15,153);
insert into player_career values ('a197','test',5,278,2,0,62);
insert into player_career values ('a197','t20',58,1878,11,2,172);
insert into player_career values ('p203','odi',74,3359,15,11,125);
insert into player_career values ('p203','test',25,1707,13,7,127);
insert into player_career values ('p203','t20',36,1405,12,0,97);
select * from player_career

```
```SQL
### Feature 2 : Display the career of individual player in all formats
select * from player_career where cap_no='i175' 



### Feature 3: display the overall hundred, fifty, and runs by individual player
                                                                                 
select sum(hundred), sum(fifty),sum(runs) from player_career where cap_no='i175' 

### Feature 4: Display the player name with highscore in a descending order 

select l.player_name,c.high_score from player_list l,player_career c where c.match_id='test' and l.cap_no = c.cap_no
order by high_score desc ;  

### Feature 5: display the overall hundred fifty and runs by all players
                                                                                                                                                
select cap_no, sum(hundred), sum(fifty),sum(runs) from player_career group by cap_no; 


### Feature 6: Display the career of all players in all formats

select l.player_name,t.* from player_career t ,player_list l where l.cap_no=t.cap_no  
```                                                            

```SQL
create table match_data (cap_no varchar2(5),
match_type varchar2(5) not null,
runs int not null,
constraint runs_match_ck check (runs>=0),
constraint cap_match_no_fk foreign key (cap_no) references player_list(cap_no),
constraint match_type_ck check (match_type in ('test','odi','t20'))
);
insert into match_data values ('i175','odi',130);
insert into match_data values ('i158','odi',150);
insert into match_data values ('p203','t20',80);
insert into match_data values ('a123','test',160);
insert into match_data values ('e226','t20',104);
insert into match_data values ('sa78','t20',133);
insert into match_data values ('sa17','test',190);
insert into match_data values ('a197','test',78);
insert into match_data values ('i175','test',28);
select * from match_data

```
Feature 7: Update the career of the player
```SQL
create or replace PROCEDURE update_career(i_cap_no in varchar2,i_type in varchar2,i_runs in number)
AS
v_fifty number;
v_hundred number;
v_scored_fifty number := 0;
v_scored_hundred number:=0;
v_highscore number;
BEGIN
    select fifty, hundred, high_score into v_fifty, v_hundred, v_highscore from player_career where cap_no = i_cap_no  
     and match_id = i_type;
      
     if i_runs > v_highscore then
         v_highscore:=i_runs;
     --update player_career set high_score = i_runs where cap_no = i_cap_no  and i_type=i_match_id;
     end if;
     
     if i_runs >100 then
     v_scored_hundred:=1;
    -- update player_career set hundred = i_hundred+1 where i_cap_no = cap_no and i_type=i_match_id;
     end if;
     
     if i_runs>50 and i_runs<100 then 
     v_scored_fifty := 1;
     end if;
     
     
     update player_career set matches = matches+1, runs = runs+i_runs , fifty = v_fifty+ v_scored_fifty,
     hundred = v_hundred + v_scored_hundred, high_score = v_highscore where cap_no = i_cap_no and match_id = i_type;
      
     
          
     commit;
END UPDATE_CAREER;


declare
 v_cap_no  varchar2(5) := 'sa17';
 v_type  varchar2(5) := 'test';
 v_runs  number := 190;
 
 begin 
 update_career (v_cap_no,v_type,v_runs);
 end;
 
 
 
 select * from player_career p inner join player_list l on l.cap_no=p.cap_no;
```
