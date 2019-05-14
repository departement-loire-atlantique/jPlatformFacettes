## Portlet FacetedSearch

### Objectif

Pouvoir réaliser simplement des moteurs à facette gràce à une Portlet dédiée
TODO : Explication de laportlet, voir si tout sert encore
TODO : Analyse de tous les cas - Rappel des fonctionnalités

### Fichiers impliqués 

types/PortletFacetedSearch/doPortletFacetedSearch.jsp
TODO : avoir un gabarit qui prend en compte toutes les fonctionnalités 

WEB-INF/data/types/PortletFacetedSearch/PortletFacetedSearch.xml
TODO : a reevoir

## QueryFilter et indexation personnalisés des types de contenu Canton et Ville

TODO : communes limitrophes à ajouter dans les facettes

### Objectif

Quand un contenu est associé à un canton, alors il est automatiquement associé à toutes les villes du canton.
Quand un contenu est associé à une ville, alors le code INSEE de la ville est ajouté aux meta donnée d'indexation du contenu.

Le datacontroleur "IndexationDataController" détecte si il est nécessaire de mettre à jour l'indexation si un contenu ville ou canton a été modifié (code INSEE ou le lien vers canton de la ville). Ce controleur ne réindexe que les publications impactées.

Le qurey filter canton et ville permet d'utiliser cette indexation personnalisée dans les requêtes. Dès que la recherche implique un contenu canton ou commune, la recherche se base automatiquement sur les champs lucene personnalisés et indexés spécifiquement.

### Fichiers impliqués

WEB-INF/classes/fr/cg44/plugin/socle/indexation/policyfilter/PublicationFacetedSearchCantonEnginePolicyFilter.java
WEB-INF/classes/fr/cg44/plugin/socle/indexation/policyfilter/PublicationFacetedSearchCityEnginePolicyFilter.java
WEB-INF/classes/fr/cg44/plugin/socle/indexation/datacontroller/IndexationDataController.java
WEB-INF/classes/fr/cg44/plugin/facettes/queryfilter/CantonQueryFilter.java
WEB-INF/classes/fr/cg44/plugin/facettes/queryfilter/CityQueryFilter.java
TODO - A finir et à décrire WEB-INF/classes/fr/cg44/plugin/facettes/queryfilter/TitleQueryFilter.java
TOD - faire une classe mère pour les filter - Nom LuceneQueryFilter (Abstraite avec nouvelle méthode)

Tests unitaires pour l'indexation :
unitests/fr/cg44/plugin/facettes/ModifSearchCityTest.java
unitests/fr/cg44/plugin/facettes/SearchCityTest.java
unitests/fr/cg44/plugin/facettes/SocleDataInit.java
TODO : A relire


## Surcharge des query handler des portlets "Portlet requête itération" et "Portlet itération détaillée"

### Objectif

Pouvoir gérer la recherche par catégorie / sous catégories avec par défaut les règles suivantes :
 - union entre les éléments d'une même branche (cids)
 - intersection entre les éléments de différentes branches(cidBranches)
 - si on sélectionne un enfant et son parent alors le parent est automatiquement ignoré
  
Pour rappel, nativement, JCMS aurait simplement réalisé une requête en mode union sur toutes les catégories.

### Fichiers impliqués

- types/PortletQueryForeach/doQuery.jspf
- types/PortletQueryForeach/doQueryHandlers.jspf
 
