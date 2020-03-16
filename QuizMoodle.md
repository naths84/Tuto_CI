# Quiz Moodle

[![](http://img.youtube.com/vi/-Qq-EcUEQ9A/0.jpg)](http://www.youtube.com/watch?v=-Qq-EcUEQ9A "")

Chaque question peut être créée individuellement, par exemple pour du vocabulaire en langue ou bien des questions de compréhension. Dans ce cas, je recommande vivement de vous orienter vers la documentation déjà existante: 

[Description générale](https://docs.moodle.org/3x/fr/Active_Quiz)

[Quiz module](https://docs.moodle.org/25/en/Quiz_module)

[Tutoriel vidéo](https://www.youtube.com/watch?v=GqIECZJD4fE)

Les quiz Moodle sont un super moyen de mener des évaluations formatives, car vous obtenez ensuite un rapport détaillé de la performance de chaque élève.

## Partagez!
Ce qu'il y a d'intéressant avec les ressources Moodle, c'est qu'elles peuvent être facilement partagées. Donc au lieu de créer du contenu chacun de son côté, mettez-vous par groupe, informez et partagez vos ressources avec vos collègues.

## Création de quiz générés aléatoirement

Le type de questions que nous avons vu dans la vidéo s'appelle des questions Cloze ([Ici pour plus d'info](https://docs.moodle.org/19/fr/Question_Cloze_%C3%A0_r%C3%A9ponses_int%C3%A9gr%C3%A9s)). 

Pour comprendre la syntaxe d'une question Cloze, il faut aller dans Moodle dans la banque de question, créer une question et ensuite l'exporter au format XML Moodle. Cela permet de comprendre la syntaxe que Moodle souhaite recevoir pour ce type de questions. Nous générons un nombre donné de questions basé sur cette syntaxe, puis nous les importons dans Moodle qui les reconnait.

### Prérequis
Pour cela vous devrez pouvoir coder dans un langage de programmation (on a choisi Python, ne vous inquiétez pas le code est très basique) et pour écrire des maths, connaître la syntaxe basique de LaTex. 

### Exemple des fractions

#### Décortiquer la question

Je ne m'y connais pas bien en HTML ni en Python, donc excusez s'il vous plaît la qualité du code, il fait l'affaire. 

Voici un exemple d'une question que nous avons vu dans la vidéo. 

      <question type="cloze">
    <name>
      <text>fraction addition 0, niveau add 3, niveau compl 1, </text>
    </name>
    <questiontext format="html">
      <text><![CDATA[<p>Quel est le résultat de l'opération suivante? ATTENTION: Donne la réponse sous forme de fraction irréductible. \[\dfrac{18}{3}+\dfrac{12}{2}=\]</p>
    <table border="0">
    <tbody>
    <tr>
    <td>{1:NUMERICAL:=12:0.1}</td>
    </tr>
    <tr>
    <td><span style="text-decoration: line-through;">                  <br /></span></td>
    </tr>
    <tr>
    <td><span style="text-decoration: overline;">{1:NUMERICAL:=1:0.1}</span></td>
    </tr>
    </tbody>
    </table>]]></text>
        </questiontext>
        <generalfeedback format="html">
              <text><![CDATA[<p>On applique la deuxième règle qui est que si deux 
              fractions n'ont pas le même dénominateur, il faut déterminer des fractions 
              équivalentes aux termes de l'addition de sorte qu'elles aient toutes 
              le même dénominateur. 
              Dans ce cas, on commence par déterminer le \(ppmc(3,2)\). 
              <br> <br>  On obtient que \(ppmc(3,2)=6\). On va chercher des fractions 
              équivalentes à \(\dfrac{18}{3}\) et \(\dfrac{12}{2}\) dont le dénominateur 
              égal à \(6\).<br>  <br>  1) Puisque \(3\cdot 2=6\), on amplifie \(\dfrac{18}{3}\) par 2 pour obtenir \(\dfrac{36}{6}\). <br> <br>  2) Puisque \(2\cdot 3=6\), on amplifie 
              \(\dfrac{12}{2}\) par 3 pour obtenir \(\dfrac{36}{6}\). <br> <br> 
              On effectue à présent le calcul avec les fraction équivalentes en 
              appliquant le première règle, c'est-à-dire on calcule 
              \[\dfrac{18}{3}+\dfrac{12}{2}=\dfrac{36}{6}+\dfrac{36}{6}=\dfrac{36+36}{6}=\dfrac{72}{6}\] 
              et \(pgcd(72,6)=6\). <br> Donc la réponse sous forme de fraction 
              irréductible est \[\dfrac{12}{1}\]</p>]]></text>
            </generalfeedback>
            <penalty>0</penalty>
            <hidden>0</hidden>
          </question>

La ligne 

      <question type="cloze">

indique justement le type de la question.

        <name>
      <text>fraction addition 0, niveau add 3, niveau compl 1, </text>
    </name>

indique le nom de la question tel qu'il sera importé dans Moodle.

Ensuite vient la quesiton à proprement dit entre les balises:

    <questiontext format="html">

 avec les champs de réponse:

    {1:NUMERICAL:=12:0.1}

et le feedback:

        <generalfeedback format="html">

#### Le code pour générer ces questions

Vous trouverez également le code ici.

    # -*- coding: utf-8 -*-
    from sympy import prod
    from sympy.ntheory import factorint
    import random 
    import itertools
    import sys

    #Pour modifier le niveau de complexité des questions
    niveau_complexite=1
    niveau_add_sous=int(sys.argv[1])


    #Les champs de la question qui ne changent pas. 
    #Ils ont été extraits de la question exportée à partir de Moodle
    p_0="""
      <question type="cloze">
        <name>
          <text>"""
    p_1="""</text>
        </name>
        <questiontext format="html">
          <text><![CDATA[<p>"""
    p_2="""</p>
    <center>
    <table border="0">
    <tbody>
    <tr>
    <td>{1:NUMERICAL:="""
    p_3=""":0.1}</td>
    </tr>
    <tr>
    <td><span style="text-decoration: line-through;">                  <br /></span></td>
    </tr>
    <tr>
    <td><span style="text-decoration: overline;">{1:NUMERICAL:="""
    p_4=""":0.1}</span></td>
    </tr>
    </tbody>
    </table>]]>
    <\center>
    </text>
        </questiontext>
        <generalfeedback format="html">
              <text><![CDATA[<p>"""
    p_5="""</p>]]></text>
            </generalfeedback>
            <penalty>0</penalty>
            <hidden>0</hidden>
          </question>"""

    #Une fonction pour imprimer
    def printf(format, *args):
        sys.stdout.write(format % args)

    #Pour écrire en LaTex la décomposition d'un nombre en facteurs premiers
    def dec_nom(a):
        adec_p=list(factorint(a))
        adec_f=list(factorint(a).values())
        ecriture_a=""
        #ecriture_a=str(a)+"="
        for j in range(0,len(adec_p)-1):
            ecriture_a=ecriture_a+str(adec_p[j])+"^"+str(adec_f[j])+"\cdot "
            ecriture_a=ecriture_a+str(adec_p[-1])+"^"+str(adec_f[-1])
        return ecriture_a

    #Pour calculer le pgdc
    def gcd_f(a,b): 
        if(b==0): 
            return a 
        else: 
            return gcd_f(b,a%b) 
    def lcm(a,b): 
        return (a*b) / gcd_f(a,b) 

    #Pour donner un nom à la question
    nom_exo="add_sous_"+"met_"+str(niveau_add_sous)+"niv_"+str(niveau_complexite)

    #Générer les possibilités pour la valeurs du numérateur et du dénominateur.
    list_choix_numerateur=range(1,niveau_complexite*10-3)
    list_choix_denominateur=range(2,niveau_complexite*10-3)

    #Pour générer autant de questions que l'argument entré par l'utilisateur
    #Écrire la question
    iteration=range(0,int(sys.argv[2]))
    file=open(nom_exo+".xml","w")
    file.write("""<?xml version="1.0" encoding="UTF-8"?>
    <quiz>""")
    for i in iteration:
        amplificateur=random.choice([1,1,2,3,4])
        a=random.choice(list_choix_numerateur)*amplificateur
        b=random.choice(list_choix_numerateur)*amplificateur
        c=random.choice(list_choix_denominateur)
        if(niveau_add_sous==1):
            d=c;
        elif(niveau_add_sous==2 and i%2==0):
            d=c*random.choice([2,3,4,5])
        elif(niveau_add_sous==2 and i%2==1):
            c=d*random.choice([2,3,4,5])
        else:
            d=random.choice(list_choix_denominateur)
            while(d%c==0 or c%d==0):
                d=random.choice(list_choix_denominateur)
        #La fraction est donnée par a/c et b/d
        repNumPasSimplifie=a*d/gcd_f(c,d)+b*c/gcd_f(c,d)
        repDenPasSimplifie=lcm(c,d)
        gcdNumDen=gcd_f(repNumPasSimplifie,repDenPasSimplifie)
        repNumSimplifie=repNumPasSimplifie/gcdNumDen
        repDenSimplifie=repDenPasSimplifie/gcdNumDen
        file.write(p_0)
        file.write("fraction addition %d, niveau add %d, niveau compl %d, " % (i,niveau_add_sous,niveau_complexite))
        file.write(p_1)
        file.write("Quel est le résultat de l'opération suivante? ATTENTION: Donne la réponse sous forme de fraction irréductible. \[\dfrac{%d}{%d}+\dfrac{%d}{%d}=\]" % (a,c,b,d))
        file.write(p_2)
        file.write("%d" % (repNumSimplifie))
        file.write(p_3)
        file.write("%d" % (repDenSimplifie))
        file.write(p_4)

        if(niveau_add_sous==1 and gcdNumDen==1):
                file.write("On applique la première règle qui est que si deux fractions ont le même dénominateur, il suffit d'effectuer l'addition ou la soustraction de leur numérateur. Ainsi \[\dfrac{%d}{%d}+\dfrac{%d}{%d}=\dfrac{%d+%d}{%d}=\dfrac{%d}{%d}.\] Remarquons que \(pgcd(%d,%d)=%d\), donc la fraction est déjà irréductible, il n'y a rien de plus à faire. La réponse est \[\dfrac{%d}{%d}.\]" % (a,c,b,d,a,b,c,repNumPasSimplifie,repDenPasSimplifie,repNumPasSimplifie,repDenPasSimplifie,gcdNumDen,repNumSimplifie,repDenSimplifie))

        if(niveau_add_sous==1 and gcdNumDen!=1):
                file.write("On applique la première règle qui est que si deux fractions ont le même dénominateur, il suffit d'effectuer l'addition ou la soustraction de leur numérateur. Ainsi \[\dfrac{%d}{%d}+\dfrac{%d}{%d}=\dfrac{%d+%d}{%d}=\dfrac{%d}{%d}\] et on simplifie par \(pgcd(%d,%d)=%d\) pour obtenir une fraction irréductible.  La réponse sous forme de fraction irréductible est \[\dfrac{%d}{%d}.\]" % (a,c,b,d,a,b,c,repNumPasSimplifie,repDenPasSimplifie,repNumPasSimplifie,repDenPasSimplifie,gcdNumDen,repNumSimplifie,repDenSimplifie))

        if(niveau_add_sous==2):
            if(c<d):
                a_n=a
                b_n=b
                c_n=c
                d_n=d
            else:
                a_n=b
                b_n=a
                c_n=d
                d_n=c
                
            file.write("On applique la deuxième règle qui est que si deux fractions n'ont pas le même dénominateur, il faut déterminer des fractions équivalentes aux termes de l'addition de sorte qu'elles aient toutes le même dénominateur. Dans ce cas, %d est un multiple de %d, en effet \(%d\cdot %d=%d\). On amplifie \(\dfrac{%d}{%d}\) par %d (ce qui donne \(\dfrac{%d}{%d}\)) et on applique la première règle, c'est-à-dire on calcule \[\dfrac{%d}{%d}+\dfrac{%d}{%d}=\dfrac{%d+%d}{%d}=\dfrac{%d}{%d}.\] De plus \(pgcd(%d,%d)=%d\), donc la réponse sous forme de fraction irréductible est \[\dfrac{%d}{%d}.\]" % (c_n,d_n,c_n,d_n/c_n,d_n,a_n,c_n,d_n/c_n,a_n*d_n/c_n,c_n*d_n/c_n,a_n*d_n/c_n,c_n*d_n/c_n,b_n,d_n,a_n*d_n/c_n,b_n,c_n*d_n/c_n,repNumPasSimplifie,repDenPasSimplifie,repNumPasSimplifie,repDenPasSimplifie,gcdNumDen,repNumSimplifie,repDenSimplifie))

        if(niveau_add_sous==3):
            file.write("On applique la deuxième règle qui est que si deux fractions n'ont pas le même dénominateur, il faut déterminer des fractions équivalentes aux termes de l'addition de sorte qu'elles aient toutes le même dénominateur. Dans ce cas, on commence par déterminer le \(ppmc(%d,%d)\). <br> <br>  On obtient que \(ppmc(%d,%d)=%d\). On va chercher des fractions équivalentes à \(\dfrac{%d}{%d}\) et \(\dfrac{%d}{%d}\) dont le dénominateur égal à \(%d\).<br>  <br>  1) Puisque \(%d\cdot %d=%d\), on amplifie \(\dfrac{%d}{%d}\) par %d pour obtenir \(\dfrac{%d}{%d}\). <br> <br>  2) Puisque \(%d\cdot %d=%d\), on amplifie \(\dfrac{%d}{%d}\) par %d pour obtenir \(\dfrac{%d}{%d}\). <br> <br> On effectue à présent le calcul avec les fraction équivalentes en appliquant le première règle, c'est-à-dire on calcule \[\dfrac{%d}{%d}+\dfrac{%d}{%d}=\dfrac{%d}{%d}+\dfrac{%d}{%d}=\dfrac{%d+%d}{%d}=\dfrac{%d}{%d}\] et \(pgcd(%d,%d)=%d\). <br> Donc la réponse sous forme de fraction irréductible est \[\dfrac{%d}{%d}\]" % (c,d,c,d, lcm(c,d),a,c,b,d,lcm(c,d),c,lcm(c,d)/c,lcm(c,d),a,c,lcm(c,d)/c,a*lcm(c,d)/c,lcm(c,d),d,lcm(c,d)/d,lcm(c,d),b,d,lcm(c,d)/d,b*lcm(c,d)/d,lcm(c,d),a,c,b,d,a*lcm(c,d)/c,lcm(c,d),b*lcm(c,d)/d,lcm(c,d),a*lcm(c,d)/c,b*lcm(c,d)/d,lcm(c,d),repNumPasSimplifie,repDenPasSimplifie,repNumPasSimplifie,repDenPasSimplifie,gcdNumDen,repNumSimplifie,repDenSimplifie))
        file.write(p_5)
    file.write("""</quiz>""")
    file.close()


