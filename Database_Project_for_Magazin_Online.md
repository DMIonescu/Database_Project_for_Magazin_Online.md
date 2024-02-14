# Database Project for "Magazin Online"
The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

Application under test: "Magazin Online".

Tools used: MySQL Workbench

### Database Schema

You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.

![reverse engineer1](https://github.com/DMIonescu/Database_Project_for_Magazin_Online.md/assets/154073184/ba3a74dd-eb70-4d54-92a1-6680c6927e89)
![reverse engineer12](https://github.com/DMIonescu/Database_Project_for_Magazin_Online.md/assets/154073184/de2206e1-005e-47c0-ad71-9753dce4f8ba)

### Database Queries

#### i. DDL (Data Definition Language)
##### The following instructions were written in the scope of CREATING the structure of the database (CREATE INSTRUCTIONS)

create database magazinOnline; <br>
use magazinOnline;<br>
-- drop database magazinOnline;<br>

-- Produse -> id, nume_produs, stoc, pret, descriere, recenzii, categoria, cluloare, furnizor, brand, id_reducere<br>
create table Produse (<br>
id INT NOT NULL auto_increment PRIMARY KEY,<br>
numeProdus VARCHAR (30) NOT NULL,<br>
stoc INT NOT NULL,<br>
pret DECIMAL(8,2) NOT NULL,<br>
descriere VARCHAR (150),<br>
recenzii VARCHAR (150),<br>
categorie VARCHAR (50),<br>
culoare VARCHAR (15),<br>
furnizor VARCHAR (30),<br>
brand VARCHAR (15),<br>
idReducere INT<br>
);<br>
DESC Produse;<br>
describe Produse;<br>
drop table Produse;<br>
-- Utilizatori -> id, nume, prenume, email, telefon, username, adresa, data_creare_cont, gen, cont_bancar<br>
create table Utilizatori (<br>
id INT PRIMARY KEY NOT NULL auto_increment,<br>
nume VARCHAR (15) NOT NULL,<br>
prenume VARCHAR (15) NOT NULL,<br>
email VARCHAR (30) NOT NULL UNIQUE,<br>
telefon VARCHAR (10) NOT NULL,<br>
username VARCHAR (15),<br>
adresa VARCHAR (50) NOT NULL,<br>
dataCreareCont DATE,<br>
gen ENUM ('M','F'),<br>
nrCard VARCHAR (16)<br>
);<br>
-- Comenzi -> id, nr_comanda, data_comenzii, id_produs, valoare_comanda, metoda_plate, id_utilizator, metoda_livrare, status_comanda<br>
create table Comenzi (<br>
id INT PRIMARY KEY auto_increment NOT NULL,<br>
nrComanda VARCHAR (10) NOT NULL,<br>
dataComenzii DATETIME NOT NULL,<br>
idProdus INT NOT NULL,<br>
valoareComanda DECIMAL (10,2) NOT NULL,<br>
metodaPlata VARCHAR (15),<br>
idUtilizator INT NOT NULL,<br>
metodaLivrare VARCHAR (15) NOT NULL,<br>
statusComanda VARCHAR (15) NOT NULL,<br>
FOREIGN KEY (idProdus) references Produse (id),<br>
FOREIGN KEY (idUtilizator) references Utilizatori (id)<br>
);<br>
desc Comenzi; <br>
-- Reducere - > id, cod_reducere, start_date, end_date, valoare_discount<br>
create table Reducere (<br>
id INT NOT NULL PRIMARY KEY auto_increment,<br>
codReducere VARCHAR (15) NOT NULL,<br>
startDate DATETIME,<br>
endDate DATETIME,<br>
valoareDiscount INT NOT NULL<br>
);<br>
-- Utilizatori_Comenzi -> id_utilizator, id_comanda<br>
create table UtilizatoriComenzi (<br>
idUtilizator int not null,<br>
idComanda int not null,<br>
primary key (idUtilizator, idComanda),<br>
foreign key (idUtilizator) references Utilizatori (id) on update cascade,<br>
foreign key (idComanda) references Comenzi (id) on update cascade<br>
);<br>
-- Facturi -> id, id_utilizator, id_comada, suma, data_emitere, data_scadenta<br>
create table Facturi (<br>
id int not null primary key auto_increment,<br>
idUtilizator int not null,<br>
idComanda int not null,<br>
suma decimal (10,2) not null,<br>
dataEmitere datetime,<br>
dataScadenta datetime,<br>
foreign key (idUtilizator) references Utilizatori (id) on update cascade,<br>
foreign key (idComanda) references Comenzi (id) on update cascade<br>
);<br>

#### ii. DML (Data Manipulation Language)
##### In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase.

##### Below you can find all the insert instructions that were created in the scope of this project:

alter table Facturi drop column suma;<br>
desc Facturi;<br>
alter table Facturi add column suma decimal (10,2) not null;<br>
alter table Facturi modify column suma decimal (10,2) not null ;<br>
alter table Produse add foreign key (idReducere) references Reducere (id);<br>
-- insert into Produse ()<br>
desc Produse;<br>
insert into Produse (numeProdus, stoc, pret)<br>
values ("telefon", 3, 1500);<br>
select * from Produse;<br>
alter table Produse modify stoc int not null default 0;<br>
insert into Produse (numeProdus, pret)<br>
values ("laptop", 4000);<br>
insert into Produse <br>
values (3, "tableta", 5, 1200, "tableta15Inch", "suntMultumit", "telefoane", "gri", "emag", "apple", 1);<br>

insert into Produse  values (3, "tableta", 5, 1200, "tableta15Inch", "suntMultumit", "telefoane", "gri", "emag", "apple", 1);<br>

insert into Reducere values (1, "winter20", "2023-12-15 00:00:00" , "2024-01-15 23:59:59", 20); <br>
desc Reducere;<br>
select * from Reducere;<br>
desc Utilizatori;<br>
insert into Utilizatori (nume, prenume, email, telefon, username, adresa, dataCreareCont, gen, nrCard)<br>
values <br>
("Popescu", "Ion", "popescu.ion@yahoo.com", "0756000123", "pIon", "str Unirii Nr 5, Bucuresti", "2023-12-01", "M", "1234000012340000" ),<br>
("Popescu", "Albert", "popescu.albert@yahoo.com", "0756000567", "pAlbert", "str Unirii Nr 5, Bucuresti", "2023-12-01", "M", "1234000012342345" ),<br>
("Georgescu", "Ion", "georgescu.ion@yahoo.com", "0756111123", "gIon", "str Republicii Nr 12, Craiova", "2023-12-23", "M", "2222000012340000" );<br>
select * from Utilizatori;<br>
desc Comenzi;<br>
select * from Produse;<br>
insert into Comenzi (nrComanda, dataComenzii, idProdus, valoareComanda, idUtilizator, metodaLivrare, statusComanda)<br>
values ("nr3", "2023-12-05 08:10:10", 1, 1525, 1, "curier", "in asteptare" );<br>
select * from Comenzi;<br>
select * from UtilizatoriComenzi;<br>
insert into UtilizatoriComenzi values (1, 1);<br>
desc Facturi;<br>
insert into Facturi values (1, 1, 1, "2023-12-05 10:53:49","2023-12-15 10:53:49", 1525 );<br>
select * from Facturi;<br>

##### After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:

SET SQL_SAFE_UPDATES=0;<br>
update Produse <br>
set pret = 3000.89 where numeProdus = "tableta";<br>
select * from Produse;<br>
update Produse<br>
set stoc = 2;<br>

#### iii. DQL (Data Query Language)
##### After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean:

delete from Produse where id = 2;<br>
update Produse set categorie = "telefoane", culoare= "rosu", brand = "Sony" where id = 1;<br>
desc Utilizatori;<br>
insert into Utilizatori values (4, "Ionescu", "Maria", "ionescu.maria@yahoo.com", "0732000123", "mionescu", "str. Florilor, nr 50, CLuj-Napoca", "2023-07-15", "F", <br>"4444111122223333");<br>
select * from Utilizatori;<br>
insert into Utilizatori values (5, "Ionescu", "Maria", "ionescu.maria2@yahoo.com", "0732000123", "mionescu", "str. Florilor, nr 50, CLuj-Napoca", "2023-07-15", "F",<br> "4444111122223333");<br>
insert into utilizatori (nume, prenume, email, telefon, username, adresa, dataCreareCont, gen, nrCard)<br>
values <br>
("Andreescu", "Roxana", "andreescu.r@yahoo.com", "0726723477", "Arox", "str Lalelelor Nr 2, Oradea", "2023-12-12", "F", "1234000037910000" ),<br>
("Popa", "Andreea", "popa.andreea@yahoo.com", "0756000268", "poprox", "str Zambilelor Nr 47, Calarasi", "2023-12-09", "F", "4528000012342345" ),<br>
("Marinescu", "Elena", "marinescu.elena@yahoo.com", "0756639523", "elena", "str Mihai Eminescu Nr 15, Craiova", "2023-12-07", "F", "5022000012340022" );<br>
insert into Produse values <br>
(4, "televizor", 12, 1850, "Smart TV Samsung", "claritate foarte buna a imaginii", "electronice", "negru", "emag", "Samsung", 1),<br>
(5, "laptop", 7, 6000, "Laptop Apple 25", "incantat de achizitie", "laptop", "gri", "Altex", "Apple",1),<br>
(6, "laptop", 3, 3600, "Laptop Lenovo I7", "recomand", "laptop", "negru", "Altex", "Lenovo",1),<br>
(7, "laptop", 9, 2800, "Laptop Lenovo I5", "recomand", "laptop", "negru", "Altex", "Lenovo",1),<br>
(8, "telefon", 16, 3350, "Huawei P30", "fotografii reusite", "telefoane", "albastru", "Altex", "Huawei",1);<br>

select nume, prenume, telefon from Utilizatori;<br>
select nume, prenume, telefon from Utilizatori where prenume = "Maria";<br>

select * from Produse;<br>
select * from produse where stoc>2;<br>
select * from Produse limit 3;<br>
select * from Produse limit 3 offset 2;<br>

insert into Comenzi (nrComanda, dataComenzii, idProdus, valoareComanda, idUtilizator, metodaLivrare, statusComanda)<br>
values ("nr4", "2023-12-05 12:10:10", 3, 2800, 3, "posta", "livrata" ),<br>
("nr6", "2023-12-09 18:10:10", 3, 810, 3, "curier", "expediata" ),<br>
("nr7", "2023-12-12 15:10:10", 4, 1050, 2, "curier", "expediata" );<br>

##### In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:

select * from Comenzi;<br>
select * from Comenzi where valoareComanda>850 AND idUtilizator=3;<br>
select * from Comenzi where valoareComanda>850 or idUtilizator=3;<br>
select * from Comenzi where valoareComanda!=810 ;<br>
select * from Comenzi where idUtilizator in (1,3);<br>
select * from Comenzi where idUtilizator not in (1,3);<br>

select * from produse where recenzii is null;<br>
select * from produse where recenzii is not null;<br>
select * from produse where numeProdus like "tel%";<br>
select * from produse where numeProdus like "%e__";<br>
select * from produse where descriere like "%TV%";<br>
select avg(pret) as "Medie pret" from Produse;<br>
select sum(pret) as "Suma" from produse;<br>
select min(pret) as "Valoare minima" from produse;<br>
select max(pret) as "Valoare maxima" from produse;<br>
select count(pret) from produse; -- ne spune cate randuri sunt pe coloana pret care au valori<br>
select * from Produse order by numeProdus; <br>
select * from Produse order by numeProdus desc; <br>
select * from Produse order by numeProdus asc;<br>
select count(*) from Produse group by recenzii; <br>
select count(*), recenzii from Produse group by recenzii having count(*)>1;<br>

select* from Comenzi where dataComenzii between "2023-12-05" and "2023-12-09 23:59:59" and idProdus=3 order by valoareComanda asc;<br>

select Utilizatori.nume, Utilizatori.prenume, Comenzi.nrComanda from Utilizatori inner join Comenzi on Utilizatori.id=Comenzi.idUtilizator;<br>
select Utilizatori.nume, Utilizatori.prenume, Comenzi.nrComanda from Utilizatori left join Comenzi on Utilizatori.id=Comenzi.idUtilizator;<br>
select Utilizatori.nume, Utilizatori.prenume, Comenzi.nrComanda from Utilizatori right join Comenzi on Utilizatori.id=Comenzi.idUtilizator;<br>
select Produse.id, numeProdus, pret, recenzii, valoareComanda, nrComanda from Produse inner join Comenzi on Produse.id=Comenzi.idProdus;<br>
select Produse.id, numeProdus, pret, recenzii, valoareComanda, nrComanda from Produse left join Comenzi on Produse.id=Comenzi.idProdus;<br>
select P.id as "Produse", C.id as "Comenzi", numeProdus, pret, recenzii, valoareComanda, nrComanda from Produse P left join Comenzi C on P.id=C.idProdus;<br>


