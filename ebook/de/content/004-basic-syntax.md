# Grundlegende Syntax

In diesem Kapitel wird die grundlegende Syntax von SQL behandeln.

SQL-Statements kann man als 'commands' sehen, die gegen eine bestimmte Datenbank gefeuert werden. Durch SQL-Statements teilst du MySQL mit, was du erreichen möchtest. Um beispielsweise alle Benutzernamen `username` aus der `users`-Tabelle zu erhalten, würdest du folgendes schreiben:

```sql
SELECT username FROM users;
```

Schauen wir uns das Statement mal genauer an:

* `SELECT`: Als erstes begegnen wir dem `SELECT`-Schlüsselwort, welches aussagt, dass wir Daten aus der Datenbank auslesen bzw. auswählen wollen. Andere typische Schlüsselwörter sind: `INSERT`, `UPDATE` oder auch `DELETE`.
* `username`: Nun geben wir an, welche Spalte wir auswählen wollen.
* `FROM users`: Schließlich geben wir mit dem `FROM`-Schlüsselwort die Tabelle an, aus der wir Daten lesen wollen.
* Das Semikolon `;` am Ende wird als Best Practice angesehen. 
Standard-SQL-Syntax erfordert es, einige "Database Management Systeme' (DBMS)" sind dabei tolerant, aber man sollte es nicht darauf anlegen.

Wenn dieses Statement ausführen, werden wir kein Ergebniss aus der `users`-Tabelle erhalten, da diese gerade erst angelegt wurde und noch leer ist.

> Ebenso "Good Practice" ist es, SQL-Schlüsselworte groß zu schreiben, allerdings würde es auch so funktionieren.

Schauen wir uns als nächstes die grundlegenden Operatoren an:
## INSERT

Zum Hinzufügen von Daten in deine Datenbank wird das `INSERT`-Statement benutzt.

Benutzen wir die Tabelle, die wir im letzten Kapitel erstellt haben und fügen einen Benutzer in die `users`-Tabelle hinzu:

```sql
INSERT INTO users (username, email, active)
VALUES ('bobby', 'bobby@bobbyiliev.com', true);
```

Schauen wir uns das Insert-Statement mal im Detail an:

* `INSERT INTO`: Zunächst schreiben wir: `INSERT INTO users`, wodurch wir MySQL sagen, dass wir Daten in die Tabelle einfügen wollen.
* `users (username, email, active)`: nun geben wir den Tabellennamen `users`   und schließlich die Tabellenspalten, welche beschrieben werden sollen, an.
* `VALUES`: Jetzt geben wir die eigentlichen Daten an, welche eingespeichert werden sollen. Die Reihenfolge der Eigenschaften deckt sich dabei mit der Reihenfolge in `users (...)`.

## SELECT

Um die Daten nach dem Einspeichern wieder auszulesen, können wir wieder auf das `SELECT`-Statement zurückgreifen:

```sql
SELECT * FROM users;
```

Output:

```
+----+----------+-------+----------+--------+---------------+
| id | username | about | birthday | active | email         |
+----+----------+-------+----------+--------+---------------+
|  1 | bobby    | NULL  | NULL     |      1 | bobby@b...com |
+----+----------+-------+----------+--------+---------------+
```

Durch die Verwendung der Wildcard `*` direkt nach dem `SELECT`-Schlüsselwort sagen wir, dass wir alle Spalten aus der `users`-Tabelle haben wollen.

Wenn wir beispielsweise nur den Benutznamen `username`- und die `email`-Spalten haben wollen, passen wir das Statement wie folgt an:

```sql
SELECT username, email FROM users;
```

Somit erhalten wir alle Benutzer, aktuell haben wir jedoch nur einen erstellt:

```
+----------+----------------------+
| username | email                |
+----------+----------------------+
| bobby    | bobby@bobbyiliev.com |
+----------+----------------------+
```

## UPDATE

Um Daten in der Datenbank zu verändern, benutzt man das `UPDATE`-Statement.

Die Syntax sieht wie folgt aus:

```sql
UPDATE users SET username='bobbyiliev' WHERE id=1;
```

Das Statement im Detail:

* `UPDATE users`: Zunächst beginnen wir mit dem `UPDATE`-Schlüsselwort, gefolgt von der Tabelle, die wir bearbeiten wollen.
* `SET username='bobbyiliev'`: Dann geben wir die Spalten an, die wir bearbeiten wollen, sowie den neuen Wert.
* `WHERE id=1`: Zum Abschluss wird die `WHERE`-Bedingung verwendet um einen spezifischen Nutzer auszuwählen, hier der Nutzer mit der ID 1.

> Achtung: Ohne eine `WHERE`-Bedingung würden die Änderungen auf alle Einträge ausgeführt werden, und alle Nutzernamen `username` würden auf `bobbyiliev` geändert werden. Es gilt also Vorsicht bei der Benutzung des `UPDATE`-Statements ohne `WHERE`-Bedingung, denn so betrifft die Änderung alle Einträge der Tabelle.
In den kommenden Kapiteln werden wir auf `WHERE` noch detaillierter eingehen.

## DELETE

Wie aus dem Namen bereits hervorgeht, kann `DELETE` verwendet werden, um Daten aus der Datenbank zu löschen.

Die Syntax sieht wie folgt aus:

```sql
DELETE FROM users WHERE id=1;
```

Genau wie das `UPDATE`-Statement betrifft `DELETE` alle Einträge, wenn keine `WHERE`-Bedingung festgelegt wird. Ergo würden alle Datensätze aus der entsprechenden Tabelle gelöscht werden.

## Kommentare

Für größere SQL-Skripte können Kommentare Sinn ergeben, damit du dir nicht merken musst, was jede Zeile macht, wenn du das Skript nach längerer Zeit wieder anschaust.

Wie in allen Programmiersprachen, können Kommentare in SQL verwendet werden.

Es gibt 2 Arten von Kommentaren:

* Einzeilige Kommentare:

Dafür muss man lediglich `--` vor den Text setzen, den man auskommentieren will:

```sql
SELECT * FROM users; -- Erhalte alle Benutzer
```

* Mehrzeilige Kommentare:

So wie in diversen anderen Programmiersprachen, sind mehrzeilige Kommentare durch das Umschließen des Kommentars durch `/*` und `*/` möglich:

```sql
/*
 Erhalte alle Nutzer
 aus der Datenbank
*/
SELECT * FROM users;
```

Du könntest das in einer `.sql`-Datei nutzen und später aussführen, oder die Zeilen zum Test direkt ausführen

## Fazit

Das waren einige der häufigsten und grundlegendsten SQL-Statements.

In den folgenden Kapiteln werden wir jedes dieser Statments nochmal im Detail durchgehen.
