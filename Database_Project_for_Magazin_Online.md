# Database Project for "Magazin Online"
The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

Application under test: "Magazin Online".

Tools used: MySQL Workbench
### Database Queries
#### i. DDL (Data Definition Language)
The following instructions were written in the scope of CREATING the structure of the database (CREATE INSTRUCTIONS)

create database magazinOnline;
use magazinOnline;
-- drop database magazinOnline;

-- Produse -> id, nume_produs, stoc, pret, descriere, recenzii, categoria, cluloare, furnizor, brand, id_reducere
create table Produse (
id INT NOT NULL auto_increment PRIMARY KEY,
numeProdus VARCHAR (30) NOT NULL,
stoc INT NOT NULL,
pret DECIMAL(8,2) NOT NULL,
descriere VARCHAR (150),
recenzii VARCHAR (150),
categorie VARCHAR (50),
culoare VARCHAR (15),
furnizor VARCHAR (30),
brand VARCHAR (15),
idReducere INT
);
DESC Produse;
describe Produse;
drop table Produse;
-- Utilizatori -> id, nume, prenume, email, telefon, username, adresa, data_creare_cont, gen, cont_bancar
create table Utilizatori (
id INT PRIMARY KEY NOT NULL auto_increment,
nume VARCHAR (15) NOT NULL,
prenume VARCHAR (15) NOT NULL,
email VARCHAR (30) NOT NULL UNIQUE,
telefon VARCHAR (10) NOT NULL,
username VARCHAR (15),
adresa VARCHAR (50) NOT NULL,
dataCreareCont DATE,
gen ENUM ('M','F'),
nrCard VARCHAR (16)
);
-- Comenzi -> id, nr_comanda, data_comenzii, id_produs, valoare_comanda, metoda_plate, id_utilizator, metoda_livrare, status_comanda
create table Comenzi (
id INT PRIMARY KEY auto_increment NOT NULL,
nrComanda VARCHAR (10) NOT NULL,
dataComenzii DATETIME NOT NULL,
idProdus INT NOT NULL,
valoareComanda DECIMAL (10,2) NOT NULL,
metodaPlata VARCHAR (15),
idUtilizator INT NOT NULL,
metodaLivrare VARCHAR (15) NOT NULL,
statusComanda VARCHAR (15) NOT NULL,
FOREIGN KEY (idProdus) references Produse (id),
FOREIGN KEY (idUtilizator) references Utilizatori (id)
);
desc Comenzi; 
-- Reducere - > id, cod_reducere, start_date, end_date, valoare_discount
create table Reducere (
id INT NOT NULL PRIMARY KEY auto_increment,
codReducere VARCHAR (15) NOT NULL,
startDate DATETIME,
endDate DATETIME,
valoareDiscount INT NOT NULL
);
-- Utilizatori_Comenzi -> id_utilizator, id_comanda
create table UtilizatoriComenzi (
idUtilizator int not null,
idComanda int not null,
primary key (idUtilizator, idComanda),
foreign key (idUtilizator) references Utilizatori (id) on update cascade,
foreign key (idComanda) references Comenzi (id) on update cascade
);
-- Facturi -> id, id_utilizator, id_comada, suma, data_emitere, data_scadenta
create table Facturi (
id int not null primary key auto_increment,
idUtilizator int not null,
idComanda int not null,
suma decimal (10,2) not null,
dataEmitere datetime,
dataScadenta datetime,
foreign key (idUtilizator) references Utilizatori (id) on update cascade,
foreign key (idComanda) references Comenzi (id) on update cascade
);

#### ii. DML (Data Manipulation Language)
In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase.

Below you can find all the insert instructions that were created in the scope of this project:

alter table Facturi drop column suma;
desc Facturi;
alter table Facturi add column suma decimal (10,2) not null;
alter table Facturi modify column suma decimal (10,2) not null ;
alter table Produse add foreign key (idReducere) references Reducere (id);
-- insert into Produse ()
desc Produse;
insert into Produse (numeProdus, stoc, pret)
values ("telefon", 3, 1500);
select * from Produse;
alter table Produse modify stoc int not null default 0;
insert into Produse (numeProdus, pret)
values ("laptop", 4000);
insert into Produse 
values (3, "tableta", 5, 1200, "tableta15Inch", "suntMultumit", "telefoane", "gri", "emag", "apple", 1);

insert into Produse  values (3, "tableta", 5, 1200, "tableta15Inch", "suntMultumit", "telefoane", "gri", "emag", "apple", 1);

insert into Reducere values (1, "winter20", "2023-12-15 00:00:00" , "2024-01-15 23:59:59", 20); 
desc Reducere;
select * from Reducere;
desc Utilizatori;
insert into Utilizatori (nume, prenume, email, telefon, username, adresa, dataCreareCont, gen, nrCard)
values 
("Popescu", "Ion", "popescu.ion@yahoo.com", "0756000123", "pIon", "str Unirii Nr 5, Bucuresti", "2023-12-01", "M", "1234000012340000" ),
("Popescu", "Albert", "popescu.albert@yahoo.com", "0756000567", "pAlbert", "str Unirii Nr 5, Bucuresti", "2023-12-01", "M", "1234000012342345" ),
("Georgescu", "Ion", "georgescu.ion@yahoo.com", "0756111123", "gIon", "str Republicii Nr 12, Craiova", "2023-12-23", "M", "2222000012340000" );
select * from Utilizatori;
desc Comenzi;
select * from Produse;
insert into Comenzi (nrComanda, dataComenzii, idProdus, valoareComanda, idUtilizator, metodaLivrare, statusComanda)
values ("nr3", "2023-12-05 08:10:10", 1, 1525, 1, "curier", "in asteptare" );
select * from Comenzi;
select * from UtilizatoriComenzi;
insert into UtilizatoriComenzi values (1, 1);
desc Facturi;
insert into Facturi values (1, 1, 1, "2023-12-05 10:53:49","2023-12-15 10:53:49", 1525 );
select * from Facturi;

After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:

SET SQL_SAFE_UPDATES=0;
update Produse 
set pret = 3000.89 where numeProdus = "tableta";
select * from Produse;
update Produse
set stoc = 2;

#### iii. DQL (Data Query Language)
After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean:

delete from Produse where id = 2;
update Produse set categorie = "telefoane", culoare= "rosu", brand = "Sony" where id = 1;
desc Utilizatori;
insert into Utilizatori values (4, "Ionescu", "Maria", "ionescu.maria@yahoo.com", "0732000123", "mionescu", "str. Florilor, nr 50, CLuj-Napoca", "2023-07-15", "F", "4444111122223333");
select * from Utilizatori;
insert into Utilizatori values (5, "Ionescu", "Maria", "ionescu.maria2@yahoo.com", "0732000123", "mionescu", "str. Florilor, nr 50, CLuj-Napoca", "2023-07-15", "F", "4444111122223333");
insert into utilizatori (nume, prenume, email, telefon, username, adresa, dataCreareCont, gen, nrCard)
values 
("Andreescu", "Roxana", "andreescu.r@yahoo.com", "0726723477", "Arox", "str Lalelelor Nr 2, Oradea", "2023-12-12", "F", "1234000037910000" ),
("Popa", "Andreea", "popa.andreea@yahoo.com", "0756000268", "poprox", "str Zambilelor Nr 47, Calarasi", "2023-12-09", "F", "4528000012342345" ),
("Marinescu", "Elena", "marinescu.elena@yahoo.com", "0756639523", "elena", "str Mihai Eminescu Nr 15, Craiova", "2023-12-07", "F", "5022000012340022" );
insert into Produse values 
(4, "televizor", 12, 1850, "Smart TV Samsung", "claritate foarte buna a imaginii", "electronice", "negru", "emag", "Samsung", 1),
(5, "laptop", 7, 6000, "Laptop Apple 25", "incantat de achizitie", "laptop", "gri", "Altex", "Apple",1),
(6, "laptop", 3, 3600, "Laptop Lenovo I7", "recomand", "laptop", "negru", "Altex", "Lenovo",1),
(7, "laptop", 9, 2800, "Laptop Lenovo I5", "recomand", "laptop", "negru", "Altex", "Lenovo",1),
(8, "telefon", 16, 3350, "Huawei P30", "fotografii reusite", "telefoane", "albastru", "Altex", "Huawei",1);

select nume, prenume, telefon from Utilizatori;
select nume, prenume, telefon from Utilizatori where prenume = "Maria";

select * from Produse;
select * from produse where stoc>2;
-- cu limit imi aduc doar o anumita info - mai jos aduc primele 3 produse
select * from Produse limit 3;
-- cu offset scot info dintr-un anumit interval de randuri, aduc cele 3 randuri dupa primele 2
select * from Produse limit 3 offset 2;

insert into Comenzi (nrComanda, dataComenzii, idProdus, valoareComanda, idUtilizator, metodaLivrare, statusComanda)
values ("nr4", "2023-12-05 12:10:10", 3, 2800, 3, "posta", "livrata" ),
("nr6", "2023-12-09 18:10:10", 3, 810, 3, "curier", "expediata" ),
("nr7", "2023-12-12 15:10:10", 4, 1050, 2, "curier", "expediata" );

In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:

select * from Comenzi;
select * from Comenzi where valoareComanda>850 AND idUtilizator=3;
select * from Comenzi where valoareComanda>850 or idUtilizator=3;
select * from Comenzi where valoareComanda!=810 ;
select * from Comenzi where idUtilizator in (1,3);
-- imi aduc valori care nu sunt 1 si 3
select * from Comenzi where idUtilizator not in (1,3);

select * from produse where recenzii is null;
select * from produse where recenzii is not null;
-- % inlocuieste orice sintaxa. _ penultimul, ultimul caracter, cate un underscore pt cate un caracter
select * from produse where numeProdus like "tel%";
select * from produse where numeProdus like "%e__";
select * from produse where descriere like "%TV%";
-- alias cu "as"
select avg(pret) as "Medie pret" from Produse;
select sum(pret) as "Suma" from produse;
select min(pret) as "Valoare minima" from produse;
select max(pret) as "Valoare maxima" from produse;
select count(pret) from produse; -- ne spune cate randuri sunt pe coloana pret care au valori
select * from Produse order by numeProdus; -- le ordoneaza alfabetic. pot pune si asc
select * from Produse order by numeProdus desc; -- le ordoneaza descrescator
select * from Produse order by numeProdus asc;
select count(*) from Produse group by recenzii; -- group by merge doar cu functiile agregate
select count(*), recenzii from Produse group by recenzii having count(*)>1; -- steluta inseamna orice

select* from Comenzi where dataComenzii between "2023-12-05" and "2023-12-09 23:59:59" and idProdus=3 order by valoareComanda asc;

select Utilizatori.nume, Utilizatori.prenume, Comenzi.nrComanda from Utilizatori inner join Comenzi on Utilizatori.id=Comenzi.idUtilizator;
select Utilizatori.nume, Utilizatori.prenume, Comenzi.nrComanda from Utilizatori left join Comenzi on Utilizatori.id=Comenzi.idUtilizator;
select Utilizatori.nume, Utilizatori.prenume, Comenzi.nrComanda from Utilizatori right join Comenzi on Utilizatori.id=Comenzi.idUtilizator;
select Produse.id, numeProdus, pret, recenzii, valoareComanda, nrComanda from Produse inner join Comenzi on Produse.id=Comenzi.idProdus;
select Produse.id, numeProdus, pret, recenzii, valoareComanda, nrComanda from Produse left join Comenzi on Produse.id=Comenzi.idProdus;
select P.id as "Produse", C.id as "Comenzi", numeProdus, pret, recenzii, valoareComanda, nrComanda from Produse P left join Comenzi C on P.id=C.idProdus;


