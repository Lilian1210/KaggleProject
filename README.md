# KaggleProject

ANALYSE DES DATA :

- Provenance des données :

Ces données abordent les résultats des élèves dans l'enseignement secondaire de deux écoles portugaises.
Le jeu de donnée comporte les notes des élèves, les caractéristiques démographiques, sociales et scolaires et ont été collectés à l'aide de rapports scolaires et de questionnaires. Deux ensembles de données sont fournis en ce qui concerne les performances en deux questions distinctes : les mathématiques (MAT) et la langue portugaise (POR).


- Conformité du dataset :

Nous allons commencer par vérifier la qualité de notre jeu de données.
En effet, il est possible que des valeurs aberrantes se soient introduites dans le dataset, ou alors qu’il y ait des valeurs manquantes.
La première étape est donc de vérifier si le jeu de données est bien complet, sans valeurs étranges (exemple : un élève ayant une note de 22/20 dans une matière).
On commence par la détection des outliers :

Une méthode classiquement employée pour détecter les outliers, consiste à réaliser un boxplot. On parle alors de méthode de détection univariée car elle ne concerne qu’une seule dimension, ou variable.
La méthode boxplot permet aussi d’encadrer dans un rectangle les données comprises entre le premier quartile et le troisième quartile.

ggplot(data=student, aes( x=age)) +
    geom_boxplot()+ 
    xlab(label = "âge") +
    ylab(label = "count") +
    theme(axis.text.x = element_text(angle=30, hjust=1, vjust=1))+
    theme(legend.position="none")+
    ggtitle("Exemple de boxplots sur les données mpg")

On voit un point éloigné du boxplot, on peut alors se demander d’où il provient, si c’est une anomalie ou si c’est une donnée à prendre en considération. Cependant nous allons garder ce point car il n’est pas impossible qu’il y ait un étudiant de 22 ans dans cette école (plusieurs redoublements, réorientation,…).

Regardons un autre exemple : l’analyse des notes des étudiants par trimestre : 

t1 <- ggplot(data=student, aes(x=G1)) +
    geom_boxplot()+ 
    xlab(label = "Notes") +
    ylab(label = "Proportion") +
    theme(axis.text.x = element_text(angle=30, hjust=1, vjust=1))+
    theme(legend.position="none")+
    geom_boxplot(outlier.colour="red", outlier.shape=16,outlier.size=2, notch=FALSE) +
    ggtitle("Trimestre 1") 

t2 <- ggplot(data=student, aes(x=G2)) +
    geom_boxplot()+ 
    xlab(label = "Notes") +
    ylab(label = "Proportion") +
    theme(axis.text.x = element_text(angle=30, hjust=1, vjust=1))+
    theme(legend.position="none")+
    geom_boxplot(outlier.colour="red", outlier.shape=16,outlier.size=2, notch=FALSE) +
    ggtitle("Trimestre 2") 

t3 <- ggplot(data=student, aes(x=G3)) +
    geom_boxplot()+ 
    xlab(label = "Notes") +
    ylab(label = "Proportion") +
    theme(axis.text.x = element_text(angle=30, hjust=1, vjust=1))+
    theme(legend.position="none")+
    geom_boxplot(outlier.colour="red", outlier.shape=16,outlier.size=2, notch=FALSE) +
    ggtitle("Trimestre 3") 

library("ggpubr")

figure <- ggarrange(t1, t2, t3,ncol = 3, nrow = 3)
figure

D’après le graphe obtenu, les notes des élèves sont bien comprises entre 0 et 20. 
Nous avons pris le soin de marquer en rouge les valeurs aberrantes détectées par le boxplot. Il y en a une dans le graphe du deuxième trimestre, mais elle correspond à un élève qui a une moyenne de 0, ce qui a été considéré comme une valeur aberrante étant donné de l’éloignement de sa moyenne par rapport à celle des autres élèves. Ce point n’est donc pas considéré comme un outlier car 0 est une note possible à obtenir.

Il n’y a donc pas de données aberrantes pour les notes.


Nous avons utilisé cette méthode pour les autres variables du dataset, et nous pouvons aisément conclure qu’il n’y a aucune valeur aberrante que nous ne prenons pas en compte.


Test sur les data nominales et binaire : 
sexuni <- student %>% distinct(sex)
sexuni == c('F', 'M') 

On remarque que le test d’égalité renvoie true ce qui signifie que la colonne sur le sexe ne comporte que les valeurs ‘F’ et ‘M’.

Nous avons procédé à la même méthode sur les autres colonnes et nous n’avons pas remarqué de valeurs aberrantes.

Maintenant, regardons s’il y a des données manquantes dans notre jeu de données.
La ligne de code suivant compte le nombre de ‘NA’ (c’est-à-dire le nombre de valeurs manquantes) dans notre dataset :

sum(is.na(student)==TRUE)==0

Cette ligne renvoie 0, donc il n’y a aucune valeur manquante.

On peut donc conclure que notre jeu de donnée est bien conforme, et qu’on peut l’utiliser tranquillement.




