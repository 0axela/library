<h1>Modifications apportées avec ajout JSON</h1>


<h3>Premier changement</h3>


On avait d'abord une composition entre Exemplaire et Ressource, ce qui se prête à la création d'un attribut JSON dans Ressource. La table Exemplaire est donc supprimée, la relation Ressource-Exemplaire également.


- **Ressource** (#code : int, #exemplaire: JSON, titre : varchar(), date_apparition : date, editeur : varchar(), code_classification : varchar(), prix : float)


~~**Exemplaire** (#numero : int, #code_ressource => Ressource.code, etat : {“neuf”|“bon”|“abime”|“perdu”} , disponible : boolean), avec disponible NOT NULL~~


La table Prêt faisant référence à Exemplaire dans notre modèle relationnel, nous devons modifier l'accès aux attributs:


- **Pret** (#id_pret : int, date_pret : date, duree_pret : integer, etat_pret: boolean, numero_ressource => Ressource.Exemplaire->>numero, code_ressource => Ressource.code), avec numero_ressource KEY et code_ressource KEY


~~**Pret** (#id_pret : int, date_pret : date, duree_pret : integer, etat_pret: boolean, numero_ressource => Exemplaire.numero, code_ressource => Exemplaire.code_ressource), avec numero_ressource KEY et code_ressource KEY~~


<h3>Deuxième changement</h3>

Phrase tirée du sujet: *"On souhaite également conserver des informations spécifiques suivant le type du document, par exemple : l'ISBN d'un livre et son résumé, la langue des documents écrits et des films, la longueur d'un film ou d'une œuvre musicale, le synopsis d'un film, etc."*

Le "etc" à la fin nous laisse supposer qu'on peut ajouter des attributs supplémentaires pour chaque Livre, Film et Musique qui ne sont pas nécessairement présents dans tous.

On décide donc d'ajouter un attribut détails JSON dans Livre, Musique et Film.


- **Livre** (#code => Ressource.code, ISBN: varchar(), resume: varchar(), langue : varchar(), nb_pages int, auteur => Contributeur.id_contributeur, details JSON), avec ISBN key, auteur NOT NULL

~~**Livre** (#code => Ressource.code, ISBN: varchar(), resume: varchar(), langue : varchar(), nb_pages int, auteur => Contributeur.id_contributeur), avec ISBN key, auteur NOT NULL~~

- **Film** (#code => Ressource.code, synopsis: varchar(), langue : varchar(), duree: integer, realisateur => Contributeur.id_contributeur, details JSON), realisateur NOT NULL

~~**Film** (#code => Ressource.code, synopsis: varchar(), langue : varchar(), duree: integer, realisateur => Contributeur.id_contributeur), realisateur NOT NULL~~

- **Musique** (#code => Ressource.code, durée int, style varchar(), compositeur => Contributeur.id_contributeur, details JSON), compositeur NOT NULL

~~**Musique** (#code => Ressource.code, durée int, style varchar(), compositeur => Contributeur.id_contributeur), compositeur NOT NULL~~



