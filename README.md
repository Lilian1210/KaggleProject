# KaggleProject

ANALYSE DES DATA�:

- Provenance des donn�es�:

Ces donn�es abordent les r�sultats des �l�ves dans l'enseignement secondaire de deux �coles portugaises.
Le jeu de donn�e comporte les notes des �l�ves, les caract�ristiques d�mographiques, sociales et scolaires et ont �t� collect�s � l'aide de rapports scolaires et de questionnaires. Deux ensembles de donn�es sont fournis en ce qui concerne les performances en deux questions distinctes : les math�matiques (MAT) et la langue portugaise (POR).


- Conformit� du dataset�:

Nous allons commencer par v�rifier la qualit� de notre jeu de donn�es.
En effet, il est possible que des valeurs aberrantes se soient introduites dans le dataset, ou alors qu�il y ait des valeurs manquantes.
La premi�re �tape est donc de v�rifier si le jeu de donn�es est bien complet, sans valeurs �tranges (exemple�: un �l�ve ayant une note de 22/20 dans une mati�re).
On commence par la d�tection des outliers�:

Une�m�thode classiquement employ�e�pour d�tecter les outliers, consiste � r�aliser un�boxplot. On parle alors de m�thode de d�tection univari�e car elle ne concerne qu�une seule dimension, ou variable.
La m�thode boxplot permet aussi d�encadrer dans un rectangle les donn�es comprises entre le premier quartile et le troisi�me quartile.

ggplot(data=student, aes( x=age)) +
    geom_boxplot()+ 
    xlab(label = "�ge") +
    ylab(label = "count") +
    theme(axis.text.x = element_text(angle=30, hjust=1, vjust=1))+
    theme(legend.position="none")+
    ggtitle("Exemple de boxplots sur les donn�es mpg")

On voit un point �loign� du boxplot, on peut alors se demander d�o� il provient, si c�est une anomalie ou si c�est une donn�e � prendre en consid�ration. Cependant nous allons garder ce point car il n�est pas impossible qu�il y ait un �tudiant de 22 ans dans cette �cole (plusieurs redoublements, r�orientation,�).

Regardons un autre exemple�: l�analyse des notes des �tudiants par trimestre�: 

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

D�apr�s le graphe obtenu, les notes des �l�ves sont bien comprises entre 0 et 20. 
Nous avons pris le soin de marquer en rouge les valeurs aberrantes d�tect�es par le boxplot. Il y en a une dans le graphe du deuxi�me trimestre, mais elle correspond � un �l�ve qui a une moyenne de 0, ce qui a �t� consid�r� comme une valeur aberrante �tant donn� de l��loignement de sa moyenne par rapport � celle des autres �l�ves. Ce point n�est donc pas consid�r� comme un outlier car 0 est une note possible � obtenir.

Il n�y a donc pas de donn�es aberrantes pour les notes.


Nous avons utilis� cette m�thode pour les autres variables du dataset, et nous pouvons ais�ment conclure qu�il n�y a aucune valeur aberrante que nous ne prenons pas en compte.


Test sur les data nominales et binaire�: 
sexuni <- student %>% distinct(sex)
sexuni == c('F', 'M') 

On remarque que le test d��galit� renvoie true ce qui signifie que la colonne sur le sexe ne comporte que les valeurs �F� et �M�.

Nous avons proc�d� � la m�me m�thode sur les autres colonnes et nous n�avons pas remarqu� de valeurs aberrantes.

Maintenant, regardons s�il y a des donn�es manquantes dans notre jeu de donn�es.
La ligne de code suivant compte le nombre de �NA� (c�est-�-dire le nombre de valeurs manquantes) dans notre dataset�:

sum(is.na(student)==TRUE)==0

Cette ligne renvoie 0, donc il n�y a aucune valeur manquante.

On peut donc conclure que notre jeu de donn�e est bien conforme, et qu�on peut l�utiliser tranquillement.




