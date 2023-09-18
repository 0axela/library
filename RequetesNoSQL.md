<h1>Exemples de requêtes SQL en mobilisant les attributs JSON</h1>


<h3>SELECT pour avoir des informations sur les exemplaires d'une ressource en fonction du code</h3>

```sql
SELECT R.Titre, R.date_apparition, CAST(ex->>'numero' AS INTEGER) AS numero_exemplaire, ex->>'etat' AS etat_exemplaire, CAST(ex->>'disponible' AS BOOLEAN) AS disponibilité_exemplaire
FROM Ressource R, JSON_ARRAY_ELEMENTS(R.exemplaire) ex,
WHERE R.code = 1;
```

<h3>Création et obtention des détails d'un livre</h3>

```sql
INSERT INTO livre (code,isbn,resume,langue,nb_pages,auteur, details) VALUES (4,  '978-2-01-169169-9',  'Candide est un jeune homme naïf qui vit dans le château du baron de Thunder-Ten-Tronckh. Il est chassé de ce petit bout de paradis parce qu’il est surpris avec Cunégonde, la fille du baron, dont il est amoureux. Il quitte alors sa bien-aimée et Pangloss, le précepteur de la maison qui a enseigné à Candide que tout est au mieux dans le meilleur des mondes.',  'francais',  '192',  4, '{"type" : "roman", "codebarre" : "ABC-abc-1234"}');

SELECT L.code, L.resume, L.nb_pages, L.auteur, L.details->>'type' AS type_livre, L.details->>'codebarre' AS code_barre_livre
FROM Livre L
WHERE L.code = 4;
```

<h3>Création et obtention des détails d'un film</h3>

```sql
INSERT INTO film (code,synopsis,langue,duree,realisateur, details) VALUES ( 2,
'Malgré la destruction de l’Étoile noire dans Un nouvel espoir, l’Empire galactique est toujours aussi puissant et continue à persécuter les rebelles. Ceux-ci ont élu domicile sur la planète des glaces, Hoth. Le jeune Luke Skywalker s’en va quant à lui trouver un nouveau maître Jedi afin de maîtriser la Force. De leur côté, Han Solo et Leia s’en vont trouver de l’aider dans une étrange cité perchée dans les nuages et retrouvent une vieille connaissance de Han.',
 'anglais',  129,  9, '{"note_allocine" : 3}');

SELECT F.code, F.synopsis, CAST(F.details->>'note_allocine' AS INTEGER) AS note_allocine
FROM Film F
WHERE F.code = 2;
```
