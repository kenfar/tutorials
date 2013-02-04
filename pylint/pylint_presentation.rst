Presentation CouchDB - Toulibre
###############################

.. image:: images/couchdb-logo.png
   :align: center
   :width: 900

Qui sommes nous ?
-----------------

    * Etudiants à SUPINFO (3 et 4ème années)
    * Membre du laboratoire Linux
    * A la découverte de CouchDB

NOSQL
------

    * Pourquoi ?
    * "Tout ce qui n'est pas relationnel"
    * BigTable, Cassandra, MongoDB, CouchDB et autres. (une 40aine)
    * Documents (CouchDB)
    * Clefs / Valeurs (Cassandra)
    * Tables larges (BigTable)

CouchDB - Historique
---------------------

    * 2005, Damien Katz parles de CouchDB pour la première fois
    * 2 ans de developpements bénévole
    * 2007, Réécriture du projet C++/XML => Erlang/JSON
    * 2008, Un projet Apache (sous GPL avant)
    * 2010, Un projet en pleine évolution, des contributeurs, une certaine 
      stabilité

CouchDB - Architecture
-----------------------

    * HTTP (un protocole): Les 4 verbes (POST/PUT/GET/DELETE)
    * REST (Un style d'architecture utilisé pour le web)
    * JSON (Format de données)
    * Javascript* (ou PHP, Ruby, Python, Erlang ...)

CouchDB - Les avantages
------------------------

    * Proche de l'utilisateur (une plateforme web locale)
    * Offline par defaut
    * Réplication master/master (décentralisation)
    * Dénormalisation (schemaless)
    * Reliability
    * **Relax !**

CouchDB - Concepts
-------------------

    * Documents
    * Vues
    * Show functions

Documents
-----------

.. code-block:: javascript

    curl -X GET [...]/toulibre/presentation-couchdb
    {
        "_id":"presentation-couchdb",
        "lieu":"17 rue Bellegarde, Toulouse.",
        "date":[2010,6,23],
        "population":["geeks","barbus"],
        "doc_type": "presentation",
        "_attachments":{
            "mixli-200908-rollei-1280.jpg":{
                "stub":true,
                "content_type":"image/jpeg",
                "length":92456,
                "revpos":3
            }
        }
    }

Vues
----

    * Filtrer les documents que l'on souhaites afficher
    * Aggregation
    * Map/Reduce

Map
---
.. code-block:: javascript

    function(doc){
        if (doc.doc_type == "presentation"){
            for(var pop in doc.population){
                emit (doc.population[pop], doc._id);
            }
        }
    }

.. code-block:: javascript

    $ curl -X GET [...]/toulibre/_design/views/_view/population?reduce=false
    {"total_rows":5,"offset":0,"rows":[
    {"id":"presentation-couchdb","key":"barbus","value":"presentation-couchdb"},
    {"id":"presentation-django","key":"developpeurs","value":"presentation-django"},
    {"id":"presentation-couchdb","key":"geeks","value":"presentation-couchdb"},
    {"id":"presentation-django","key":"geeks","value":"presentation-django"},
    {"id":"presentation-django","key":"pythoneux","value":"presentation-django"}
    ]}

Reduce
------

.. code-block:: javascript

    function(keys, values, rereduce){
        return values.length;
    }

.. code-block:: javascript

    $ curl -X GET [...]/toulibre/_design/views/_view/events
    {"rows":[
    {"key":null,"value":5}
    ]}

Group
-----

.. code-block:: javascript

    $ curl -X GET [...]/toulibre/_design/views/_view/events?group=true
    {"rows":[
    {"key":"barbus","value":1},
    {"key":"developpeurs","value":1},
    {"key":"geeks","value":2},
    {"key":"pythoneux","value":1}
    ]}


Jointures
---------

    * Parce que c'est quand même bien pratique !
    * `?include_documents=true`

Révisions
---------
.. code-block:: javascript

   {
        "_id":"presentation-couchdb",
        "_rev":"1-b4393d7a1154b7d36bedaf98d554f191",
        "lieu":"17 rue Bellegarde, Toulouse.",
        "date":[2010,6,23],
        "population":["geeks","barbus"],
        "doc_type": "presentation"
    }

.. image:: images/1.png
   :align: center
   :width: 1500

.. image:: images/2.png
   :align: center
   :width: 1500

.. image:: images/3.png
   :align: center
   :width: 1500

.. image:: images/4.png
   :align: center
   :width: 1500

.. image:: images/5.png
   :align: center
   :width: 1500

.. image:: images/6.png
   :align: center
   :width: 1500

.. image:: images/7.png
   :align: center
   :width: 1500

.. image:: images/8.png
   :align: center
   :width: 1500

Conflits
---------
.. code-block:: javascript

    $ curl -X GET [...]/toulibre/presentation-couchdb?conflicts=true
    {
        "_id":"presentation-couchdb",
        "_rev":"4-8b26eda2beaa962201198034b40b6570",
        "lieu":"17 rue Bellegarde, Toulouse.",
        "date":[2010,6,23],
        "population":["geeks","barbus"],
        "doc_type":"presentation",
	"_conflicts":["3-57890f2aebfbff10103467bf5fbc7053"]
    }

Couchapps
---------

    * Avoir une application hebergée dans CouchDB
    * La base de donnée est un serveur Web
    * Un des objectifs de CouchDB

Futon
-----

    * Navigation
    * Vues
    * Révisions

Et une application concrète ?
-----------------------------

    * Gestion de notes
    * Aplication Web (en django)
    * Application android
    * Surement une future Couchapp

Le futur de CouchDB
--------------------

    * Geolocalisation
    * Gestion de OAuth pour l'authentification
    * Embarqué

Ressources
-----------

    * Wikipedia !
    * http://books.couchdb.org
    * http://couch.io (c'est une couchapp!)
    * http://dropit.notmyidea.org (notre documentation)
    * http://github.com/ametaireau/ & http://github.com/arnaudbos

Merci !
--------

    * Christophe Narbonne : christophe31@gmail.com
    * Arnaud Bos : arnaud.tlse@gmail.com
    * Alexis Metaireau : alexis@lolnet.org

Licence
--------

    * Présentation sous licence CC-Attribution-ShareAlike
    * http://creativecommons.org/licenses/by-sa/2.5/
