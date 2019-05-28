# ProjecteFinalBaseDeDades

create table UserDjango (
id int not null,
username varchar(100) not null,
password varchar(100) not null,
CONSTRAINT userdjango_pk
    primary key(id)
);

create table Persona(
id int not null,
userdjango int not null,
nom VARCHAR(50) not null,
cognom VARCHAR(50) not null,
correu VARCHAR(50) not null,
constraint persona_pk
	primary key (id),
constraint user_fk 
    foreign key (userdjango) 
    references UserDjango (id)
);

create table Localitat (
id int not null,
nom varchar(100) not null,
latitud varchar(100) not null,
longitud varchar(100) not null,
CONSTRAINT localitat_pk
    primary key(id)
);

create table Categoria (
id int not null,
nom varchar(100) not null,
CONSTRAINT categoria_pk
    primary key(id)
);

create table activitat_persones_inscrites (
id int not null,
persona int not null,
activitat int not null,
asistira varchar(2) not null,
CONSTRAINT activitat_persones_inscrites_pk
    primary key(persona,activitat),
constraint persona_fk 
    foreign key (persona) 
    references Persona (id),
constraint activitat_fk 
    foreign key (activitat) 
    references Activitat (id)
);

create table Activitat(
id int not null,
nom VARCHAR(50) not null,
descripcio VARCHAR(500) not null,
dia date not null,
diafinal date not null,
localitat int not null,
categoria int not null,
creador VARCHAR(50) not null, 
constraint activitat_pk
	primary key (id),
constraint localitat_fk 
    foreign key (localitat) 
    references Localitat (id),
constraint categoria_fk 
    foreign key (categoria) 
    references Categoria (id)
);

create table Comentari (
id int not null,
text varchar(500) not null,
activitat int not null,
persona int not null,
data date not null,
CONSTRAINT comentari_pk
    primary key(id),
constraint comentari_fk 
    foreign key (activitat) 
    references Activitat (id),
CONSTRAINT cpersona_fk
    foreign key (persona) 
    references Persona(id)
    
);



INSERT INTO `userdjango` (`id`, `username`, `password`) VALUES ('1', 'david', 'david2202'), ('2', 'tomas', 'tomas2202')

INSERT INTO `persona` (`id`, `userdjango`, `nom`, `cognom`, `correu`) VALUES ('1', '1', 'david', 'ayachi', 'david@gmail.com'), ('2', '2', 'tomas', 'puig', 'tomas@gmail.com')

INSERT INTO `localitat` (`id`, `nom`, `latitud`, `longitud`) VALUES ('1', 'Figueres', '42.2666314', '2.9638434'), ('2', 'Girona', '41.9793006', '2.8199439')

INSERT INTO `categoria` (`id`, `nom`) VALUES ('1', 'Oci'), ('2', 'Cultura')

INSERT INTO `activitat` (`id`, `nom`, `descripcio`, `dia`, `diafinal`, `localitat`, `categoria`, `creador`) VALUES ('1', 'Visita al Dali', 'El proxim divendres es vol anar a visitar el museu dali ja que es realitzara un descompte', '2019-05-24', '2019-05-24', '1', '2', 'david'), ('2', 'Semana del Cine', 'A girona el aquesta setmana es la setmana del Cine, i es m\'agradaria anar el dimecres', '2019-05-22', '2019-05-22', '2', '1', 'tomas')

INSERT INTO `comentari` (`id`, `text`, `activitat`, `persona`, `data`) VALUES ('1', 'Perfecte, per la tarda hi ha menos gent', '2', '1', '2019-05-19'), ('2', 'Perfecte, jo asisitre, podem anar a veure la de Guerra Mundia Z', '2', '2', '2019-05-19')

INSERT INTO `activitat_persones_inscrites` (`id`, `persona`, `activitat`, `asistira`) VALUES ('1', '1', '1', '1'), ('2', '2', '2', '1')



delete persona where id = 1



select a.nom from persona p INNER JOIN activitat a on p.id = a.id where p.nom = 'david'

select * from persona where nom in (select creador from activitat)


Function
CREATE FUNCTION dbo.retornar_nom(	
	@identificador as int
)RETURNS varchar(100) 
AS
BEGIN
RETURN (select 
	nom
		from persona
		where @identificador = id	
)
END;

select dbo.retornar_nom (1)



create Procedure dbo.NouUsuari
@id as int,@nom as varchar(100),@password as varchar(100)
AS BEGIN 
begin transaction btx
set transaction isolation level serializable;
INSERT INTO `userdjango` (`id`, `username`, `password`) VALUES (@id, @nom, @password);
end;
commit;
END;



drop avisar 
CREATE TRIGGER avisar 
ON Persona
AFTER INSERT, UPDATE   
--AS RAISERROR ('Notify Customer Relations', 16, 10);  
as print ' Creat correctament
GO 
