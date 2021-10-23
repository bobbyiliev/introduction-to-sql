# Grundlegende Syntax

In diesem Kapitel werden die grundlegende Syntax von SQL behandeln.

SQL-Statements kann man als 'commands' sehen, die gegen eine bestimmte Datenbank gefeuert werden. Durch SQL-Statements teilst du MySQL mit, was man erreichen möchte. Beispielsweise, wenn man alle Benutzernamen `username` aus der `users`-Tabelle den erfahren möchte, würde man folgendes ausführen:

```sql
SELECT username FROM users;
```

Schauen wir uns das Statement mal genauer an:

* `SELECT`: Als erstes begegnen wir dem `SELECT`-Schlüsselwort, welches aussagt, dass wir Daten aus der Datenbank auslesen bzw. auswählen wollen. Andere typische Schlüsselwörter sind: `INSERT`, `UPDATE` oder auch `DELETE`.
* `username`: Nun geben wir an, was wir selektieren wollen. In diesem Fall die Tabellenspalte `username`.
* `FROM users`: Schließlich geben wir mit dem `FROM`-Schlüsselwort die Tabelle an, aus der wir Daten lesen wollen.
* Das Semikolon `;` am Ende wird als Best Practice angesehen. 
Standard-SQL-Syntax erfordert es, aber einige "Database Management Systeme' (DBMS)" sind dabei tolerant, aber man sollte es nicht darauf anlegen.

Wenn dieses Statement ausgeführt werden wir keinerlei Ergebnisse aus der `users`-Tabelle bekommen, da diese gerade erst angelegt wurde und noch leer ist.

> Ebenso "Good Practice" ist es SQL-Schlüsselworte großgeschrieben zu verwenden, aber auch das ist nicht Pflicht.

Fahren wird fort und behandeln die nächste, grundlegende Operation.

## INSERT

Zum Hinzufügen von Daten in deine Datenbank wird das `INSERT`-Statement benutzt.

Benutzen wir die Tabelle, die wir im letzten Kapitel erstellt haben und fügen einen Benutzer in die `users`-Tabelle hinzu:

```sql
INSERT INTO users (username, email, active)
VALUES ('bobby', 'bobby@bobbyiliev.com', true);
```

Schauen wir in das Insert-Statement mal im Detail an:

* `INSERT INTO`: Zunächst benutzen wir `INSERT INTO users`, wodurch MySQL weiß, dass Daten in eine Tabelle geschrieben werden sollen.
* `users (username, email, active)`: Nun spezifizieren wir den Tabellennamen `users` und schließlich die Tabellenspalten, welche beschrieben werden sollen.
* `VALUES`: Jetzt geben wir die Daten selbst an, welche eingespeichert werden sollen. Die Reihenfolge der Eigenschaften deckt sich dabei mit der Reihenfolge in `users (...)`.

## SELECT

Nachdem einspeichern, würden wir die Daten gerne aus der Datenbank auslesen.

Dafür können wir wieder wieder das `SELECT`-Statement verwenden:

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

Durch die Verwendung der Wildcard `*` direkt nach dem `SELECT`-Schlüsselwort, sagen wir dass wir alle Spalten aus der `users`-Tabelle haben wollen.

Wenn wir beispielsweise nur den Benutznamen `username`- und die `email`-Spalten haben wollen, passen wir das Statement wie folgt an:

```sql
SELECT username, email FROM users;
```

Somit erhalten wir alle Benutzer, wobei momentan nun mal nur einen haben:

```
+----------+----------------------+
| username | email                |
+----------+----------------------+
| bobby    | bobby@bobbyiliev.com |
+----------+----------------------+
```

## UPDATE

Um Daten in der Datenbank zu verändern benutzt man das `UPDATE`-Statement.

Die Syntax lautet wie folgt:

```sql
UPDATE users SET username='bobbyiliev' WHERE id=1;
```

Das Statement im Detail:

* `UPDATE users`: Zunächst beginnen wir mit dem `UPDATE`-Schlüsselwort um einzuleiten, dass wir schließlich die `users`-Tabellendaten verändern wollen.
* `SET username='bobbyiliev'`: Dann geben wir die Spalten an, die wir updaten wollen mit der Zuweisung des Wertes der geschrieben werden soll.
* `WHERE id=1`: Zum Abschluss wird die `WHERE`-Klausel verwendet um die Benutzer zu spezifizieren, auf die das Update ausgeführt werden soll. In diesem Fall wollen wir den Benutzer mit der ID 1.

> Achtung: Ohne eine `WHERE`-Klausel würden die Änderungen auf alle Einträge ausgeführt werden und alle hätten den `username` auf `bobbyiliev` geändert. Es gilt also Vorsicht bei der Benutzung des `UPDATE`-Statements ohne `WHERE`-Klausel, ansonsten werden immer alle Datensätze betroffen.

In den kommenden Kapiteln werden wir auf `WHERE` noch detaillierter eingehen.

## DELETE

Wie sicherlich schon erahnbar wird `DELETE` benutzt um Daten aus der Datenbank zu tilgen.

Die Syntax lautet wie folgt:

```sql
DELETE FROM users WHERE id=1;
```

Genau wie beim `UPDATE`-Statement werden alle Datensätze betroffen, wenn keine `WHERE`-Klausel verwendet wird. Ergo würden alle Datensätze aus der entsprechenden Tabelle gelöscht werden.

## Kommentare

Falls größere SQL-Skripte geschrieben werden können Kommentare nützlich sein, wenn man nach längerer Zeit wieder auf den Code schaut und die einstigen Gedankengänge nachvollziehen möchte.

Das geht natürlich wie in anderen Programmiersprachen ebenso in SQL.

Es gibt 2 Arten von Kommentaren:

* Einzeilige Kommentare:

Dafür muss man lediglich `--` vor den Text setzen und die Zeile ist auskommentiert:

```sql
SELECT * FROM users; -- Gib mir alle Benutzer
```

* Mehrzeilige Kommentare:

So wie in diversen anderen Programmiersprachen sind mehrzeilige Kommentare durch das Umschließen von `/*` und `*/` möglich:

```sql
/*
 Gib mir alle Benutzer
 aus der Datenbank.
*/
SELECT * FROM users;
```

Du könntest das in einer `.sql`-Datei nutzen und es später komplett oder auch nur einzelne Zeilen direkt ausführen.

## Fazit

Das waren ein paar der häufigsten und grundlegendsten SQL-Statements.

In den folgenden Kapiteln werden wir jedes dieser Statments nochmal im Detail durch.
