# Relationale Datenbanken
Es geht um relationale Datenbanken und SQL.
Meine Repository basiert vorwiegend auf die Tutorioals von [Sebastian Philippi](https://www.youtube.com/watch?v=JPC8FsYwi9s&list=PLCb8EhYsrW_tLk9U_V6oUDFaUojKu5nUR).

- **ERM**
  - [Domänenanalyse](#Domänenanalyse)
  - [Entity Relationship Modell](#Entity-Relationship-Modell)
- **Relationales Modell**
  - [Relationen](#Relationen) 
  - [1. Normalform)](#1-Normalform)
  - [2. Normalform](#2-Normalform)
  - [3. Normalform](#3-Normalform)
- **SQL**
  - [1. Tabellen erstellen](#1-Tabellen-erstellen)
  - [2. Referentielle Integrität](#2-referentielle-Integrität)
  - [3. Joins](#4-Joins)

# ERM

## Domänenanalyse

Schauen wir uns die folgene Anferdorung an:

<img src="https://github.com/Fazil-edu/Relationale-Datanbanken/blob/main/Bilder/Domaenenanalyse.png" >

In dem Text die rot gefärbten Nomen stehen für Entitäten, die blau gefärbten Nomen für Attribute und die grün gefärbten für Relationen zwischen diesen Entitäten. Die unterstrichene Wörter sagen in welcher Beziehung die Entitäten zueinander stehen. Z.B. ein Kunde darf aus dem europäischen Ausland sein.

## Entity Relationship Modell

<img src="https://github.com/Fazil-edu/Relationale-Datanbanken/blob/main/Bilder/ERM.png" >

# Relationales Modell

## Relationen
Unser ERM ist zwar fertig, dennoch ist es natürlich nicht aussreichend für eine Datenbank. Folgendes relationales Modell zeigt mehr Klarheit:
<img src="https://github.com/Fazil-edu/Relationale-Datanbanken/blob/main/Bilder/RM.png" >

## 1. Normalform
Für die 1. Normalform muss jedes Attribut in einer Relation einen automaren Wertebereich haben. Mit anderen Worten: in jeder Zeile einer Tabelle steht nur ein Wert.

Bei unserer Tabelle Kunde in der Spalte Adresse werden Strasse, Hausnummer etc. alles zusammen stehen und mit Komata getrennt. Das entspricht nicht der 1. Normalform. Damit sie der 1. Normalform enstpricht, muss sie so sein:

<img src="https://github.com/Fazil-edu/Relationale-Datanbanken/blob/main/Bilder/ErsteNormalform.png" >

## 2. Normalform
Für die 2. Normalform muss eine Tabelle zunächst ein mal in der 1. Normalform sein und alle Nicht-Schlüsselattribute müssen vom gesagmten Primärschlüssel abhängig sein. Mit anderen Worten: Jede Relation der Datenbank bildet nur einen Sachverhalt ab!

Da ein Artikel mehrfach in mehrere Aufträgen und in einem Auftrag mehrere Artikel vorkommen können, muss man da ein Primärschlüssel aus zwei Attributen nehmen. Dennoch hat z.B. das Datum mit der Artikelnummer nichts zu tun. Also, es ist nicht vom gesagmten Primärschlüssel abhängig, nur von einem Tel dessen. Ergo, die Tabelle entspricht nicht der 2. Normalform. So würde sie der 2. Normalform entsprechen:

<img src="https://github.com/Fazil-edu/Relationale-Datanbanken/blob/main/Bilder/ZweiteNormalform.png" >


## 3. Normalform
Für die 3. Normalform sind alle Datenfelder nur vom Primärschlüssel und nicht voneinander abhängig sein. Also, ein Nicht-Schlüsselattribut darf nicht aus dem anderen Nicht-Schlüsselattribut hervorgehen.

Bei unserer Tabelle Kunde kann man aus der PLZ die Stadt herleiten und aus der Stadt heraus kann man das Land bestimmen. Deswegen entspricht die Tabelle der 3. Normalform nicht.
So würde sie dann der 3. Normalform entsprechen:

<img src="https://github.com/Fazil-edu/Relationale-Datanbanken/blob/main/Bilder/DritteNormalform.png" >


# SQL

## 1. Tabellen erstellen

Wir erstellen nun unsere Tabellen. 
```
CREATE TABLE Kunde (
    Kunden_Nummer int primary key,
    Name varchar(30),
    Vorname varchar(30),
    Strasse varchar(30),
    Hausnummer int,
    PLZ_ID int,
    Land_ID int
);

CREATE TABLE Stadt (
    PLZ int primary key,
    Name varchar(30)
);

CREATE TABLE Land (
    ID int primary key,
    Name varchar(30)
);

CREATE TABLE Auftrag (
    Auftrags_Nummer int primary key,
    Datum date,
    Kundennummer int
);

CREATE TABLE WEIN (
Artikel_Nummer int primary key,
Bezeichnung varchar(30),
Farbe varchar(30),
Jahrgang int,
Preis float,
Hersteller varchar(30),
Lieferantennummer int
);

CREATE TABLE Lieferant(
Lieferanten_Nummer int primary key ,
Name varchar(30)
);

CREATE TABLE Auftragsposition (
    Auftragsnummer int NOT NULL,
    Artikelnummer int NOT NULL,
    Anzahl int,
    PRIMARY KEY (Auftragsnummer, Artikelnummer)
);

```

Wir fügen ein paar Datensätze hinzu.

```
INSERT INTO Kunde (Kunden_Nummer,Vorname,  Name, Strasse, Hausnummer, PLZ_ID, Land_ID) VALUES
(1,'Paul','Maier','Neustrasse', 11, 12345, 3),
(2, 'Julia', 'Schmidt', 'Am Anger', 12, 23456, 2),
(3, 'Max', 'Kurz', 'Webergasse', 13, 34567, 1),
(4, 'Philipp', 'Sauer', 'Kirchstrasse', 14, 34567, 1);

INSERT INTO Auftrag(Auftrags_Nummer, Datum, Kundennummer) VALUES
(1,'2020-04-14',1),
(2,'2020-05-17',4),
(3,'2020-05-21',2),
(4,'2020-05-21',3),
(5,'2020-05-22',1);

INSERT INTO Auftragsposition(Auftragsnummer, Artikelnummer, Anzahl) VALUES
(1,4,1),
(1,3,3),
(2,3,2),
(3,4,2),
(4,1,4),
(4,2,6),
(4,4,4),
(5,2,2),
(5,1,7);

INSERT INTO Wein (Artikel_Nummer, Bezeichnung, Farbe, Jahrgang, Preis, Hersteller,Lieferantennummer) VALUES
(1, 'Dornfelder', 'Rot', 2019, 7.99, 'Weingut Carl',1),
(2, 'Riesling', 'Weiss', 2019, 5.99, 'Weingut Mauer',2),
(3, 'Bachus', 'Weiss', NULL, 4.99, 'Weingut Künstler',1),
(4, 'Riesling', 'Weiss', 2020, 4.99, 'Weingut Breuer',2),
(5, 'Riesling', 'Weiss', 2020, 10.99, 'Weingut Breuer',3);

INSERT INTO Lieferant (Lieferanten_Nummer, Name) VALUES
(1, 'Müller GmbH'),
(2, 'Schmidt KG'),
(3, 'Maier KG');

INSERT INTO Stadt (PLZ, Name) VALUES
(12345, 'Brüssel'),
(23456, 'Wien'),
(34567, 'Hamburg');


INSERT INTO Land (ID, Name) VALUES
(1, 'Deutschland'),
(2, 'Belgien'),
(3, 'Oesterreich');

```
Nun bleibt die Frage übrig, wie man diese Tabellen in Relationen in SQL bringt, denn es gibt ja bei der Tabelle auf Auftrag die Spalte Kundennummer als den Fremdschlüssel? Wir haben oben gesegen, dass das mit Primär bzw. Fremdschlüsseln geht. Allerdings muss man dabei [referentielle Integrität](https://de.wikipedia.org/wiki/Referentielle_Integrit%C3%A4t) beachten.

## 2. Referentielle Integrität
Wenn wir einen Fremdschlüssel in eine Tabelle hinzufüge, dann heisst es ja, dass ich mit diesem Fremdschlüssen auf eine andere Tabelle referenziere. Das heisst, dass die Änderungen in einer Tabelle haben(müssen) Einfluß auf die andere Tabelle. Dafür sorgt die referentielle integtrität. Also, mit RI beugen wir die Anomalien vor.

Nehmen wir an, wir hätten zwei Tabellen: eine Mutter und eine Kind Tabelle. Da ein Kind nur eine biologische Mutter und eine Mutter mehrere Kinder haben können, ist die Beziehung natür lich 1:n. Das heistt, in der Tabelle Kind ist der Fremdschlüssel der Primärschlüssel der Tabelle Mutter. Wir müssen uns jetzt im Klaren sein, wie soll unsere Datenbank bzw. unser Kind Tabelle reagieren, wenn wir eine Mutter aus der Mutter Tabelle löschen? Soll dann das Kind in der Tabelle auch gelöscht  werden ?
Ähnlich taucht die Frage auf, ob wir in der Tabelle Kind ein Kind erzeugen dürfen, welches keine Mutter hat?

Dafür gibt folgende Einstellungen bzw. Regel oder sogenannte **constraint**:

**cascade** - ändert die Kind Tabelle dementsprechen, d.h. beim Löschen eine Mutter wird zu ihr gehörendes Kinde auch gelöscht.

**set null** - setzt die Mutter vom Kind auf Null, wenn z.B. gelöscht wird.

**no action** **restrict** - erlaubt die Änderung nicht, tut nichts. Es ist die Standart-Einstellung.

Wir wollen, dass beim Löschen eines Kunden bzw. Lieferanten mit ihnen referierende Datensätze auch gelöscht werden.

```
ALTER TABLE Auftrag ADD FOREIGN KEY (Kundennummer) REFERENCES Kunde(Kunden_Nummer) ON DELETE CASCADE;
ALTER TABLE Wein ADD FOREIGN KEY (Lieferantennummer) REFERENCES Lieferant(Lieferanten_Nummer) ON DELETE CASCADE;
```

Wir löschen den Lieferanten Maier KG aus der Tabelle, da er z.B. den Riesling vom selben Weingut zu teuer anbietet, so muss auch dann jener Riesling aus der Tablle Wein gelöscht werden.

```
DELETE FROM Lieferant WHERE Lieferanten_Nummer = 3;
```

## 4. Joins

**WHERE**

Wenn wir sehen wollen, welcher Wein von wem geliefert wird, muss man dann Abfragen über die Tabellen Wein und Lieferant machen. Schauen wir uns zunächst einmal diesen Befehl an:

```
SELECT  * FROM Wein, Lieferant;
```
Das erzeugt ein Kreuzprodukt zwischen Wein und Artikel und das ist eigentlich nicht das gewünschte Ziel, denn man muss dann noch selber die Lieferantennummer des Weines mit der Lieferantennummer vom Lieferant vergleichen, um zu sehen, von wem der Wein kommt. Mit dem folgenden Befehl heben wir das Problem auf:

```
SELECT  * FROM Wein, Lieferant WHERE Lieferantennummer = Lieferanten_Nummer;
```

**CROSS JOIN**

Das CROSS JOIN liefert dassellbe Egebniss wie bei SELECT  * FROM Wein, Lieferant;. Also, es entsteht ein Kreuzprodukt:

```
SELECT * FROM Wein CROSS JOIN Lieferant;
```
**INNER JOIN**

Bei Abfragen über mehrere Tabellen wie oben bei WHERE oder CROSS JOIN, wird im Hintergrund ein Kreuzprodukt erstellt und dann nach der Bedingung ausgegeben. Bei Tabellen mit großen Datensätzen ist das sicherlich nicht gut. Deswegen gibt es ein JOIN Befehl, der im Hintergrund anders arbeitet und deutlich schneller ist. Also, das gewünschte Ziel kann man auch so erreichen:

```
SELECT * FROM Wein INNER JOIN Lieferant ON Wein.Lieferantennummer = Lieferant.Lieferanten_Nummer;
```

Da die Namen der Lieferantennummer und Lieferanten_Nummer sich unterscheiden, verwenden wir hier ON. Wenn die Namen gleich wären, kann man dies mit USING handhaben, siehe NATURAL JOIN. 

**NATURAL JOIN**

NATURAL JOIN vergleicht Spalten automatisch mit gleichem Namen. Dabei wird dann die zweite Spalte gelöscht, also das Ergebnis wäre bei unseren Tabellen wie bei INNER JOIN bloß mit einer Spalte mit dem Namen Lieferantennummer. Das liegt an USING, mit ON sieht man beide Spalten, da sie unterschiedlich heißen. Dafür müssen wir die Spalte Lieferanten_Nummer zu Lieferantennummer ändern. Ansonsten würde  das Ergebnis wie bei CROSS JOIN aussehen. Das geht wie folgt:

```
ALTER TABLE Lieferant RENAME COLUMN Lieferanten_Nummer TO Lieferantennummer;
```
Und dann probieren wir das NATURAL JOIN
```
SELECT * FROM Lieferant NATURAL JOIN Wein;
```


**LEFT JOIN**

Angenommen wir haben einen Wein bekommen, bei dem wir, aus welchen Gründen auch immer, nicht wissen, wer der Lieferant ist. Darum steht bei diesem Wein als Lieferantennummer NULL:

```
INSERT INTO Wein (Artikel_Nummer, Bezeichnung, Farbe, Jahrgang, Preis, Hersteller,Lieferantennummer) VALUES
(5, 'Dornfelder', 'Rot', 2019, 7.99, 'Weingut Carl',NULL);
```

Wir wollen dennnoch alle Lieferanten und ihre Weine sehen, selbst wenn bei einem Wein der Lieferant irgendwie nicht bekannt ist. Dann funktioniert der INNER JOIN nicht. Dazu brauchen wir einen LEFT JOIN:

```
SELECT * FROM Wein LEFT JOIN Lieferant USING(Lieferantennummer);
```

**RIGHT JOIN**

Ebenso kann es ja sein, dass wir einen neuen Lieferanten haben und er steht auf unserer Liste:

```
INSERT INTO Lieferant (Lieferantennummer, Name) VALUES
(3, 'Schneider GmbH');
```

Aber wir haben bis jetzt noch keine Lieferung von ihm bekommen. Wie oben, wir wollen dennnoch alle Lieferanten und ihre Weine sehen, selbst wenn ein Lieferant noch keinen Wein geliefert hat.  Dazu brauchen wir einen RIGHT JOIN:

```
SELECT * FROM Wein RIGHT JOIN Lieferant USING(Lieferantennummer);
```
**FULL JOIN** 

Ein FULL JOIN fasst die LEFT und RIGHT JOINs zusammen:


```
SELECT * FROM Wein FULL JOIN Lieferant USING(Lieferantennummer);
```

