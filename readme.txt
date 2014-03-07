RVK-VISUAL

==========


Visualisierung von RVK-Notationen mittels Javascript



<h6>Projektbeschreibung "Contentvisualisierung f�r Bibliothekskataloge" <p>

Bereits seit Ende der 2000er Jahre wurde im Bibliotheksumfeld unter dem Stichwort Katalog 2.0 verschiedene Verfahren zur verbesserten Pr�sentation 
von hochwertigen Kataloginhalten thematisiert. Contentvisualisierung ist hierbei unterst�tzendes Werkzeug zur Wissensexploration und zur Eingrenzung 
von Suchergebnissen.
 Mit der Regensburger Verbundklassifikation (RVK) kann eine Bibliothek ihren Bestand nach Sachgebieten gegliedert aufstellen. 
Die RVK ist in 33 Fachgebieten hierarchisch geordnet, die vergebenen Notationen sind 4- bis 5-stellig, die Zahlenkombination allein aber f�r den Nutzer oft 
wenig verst�ndlich. Das Projekt entwickelte eine M�glichkeit, die Notationen f�r den Nutzer unabh�ngig vom Bibliothekssystem klarer zu visualisieren: 
Mit Hilfe eines Javascript-Snippets und der von der UB- Regensburg angebotenen RVK-API kann dem Nutzer der Klarname der Notation und erweitert der 
gesamte RVK-Notationsbaum mit allen Abh�ngigkeiten angezeigt werden.




<h6>Projekttr�ger <p>

Bayerische Bibliotheksverbund (BVB), Kooperative Bibliotheksverbund Berlin-Brandenburg (KOBV)
 <p>

Die Realisierung des Projekts erfolgt im Rahmen der gemeinsamen Entwicklungsprojekte der strategischen Allianz (Dez. 2007) zwischen KOBV und BVB.



<h6>Arbeitsverteilung im Projekt
 <p>

- Erstellung von Links zur direkten Nachrecherche (Lateralsuche) von RVK-Notationen --> BVB, 2011
 <p>

- textliche Visualisierung der RVK-Notation durch ein Javascript-Snippet --> BVB, 2011
 <p>

- Generalisierung des Javascript-Codes --> KOBV, 2012
 <p>

- Anpassungen des Codes an die RVK-API (Live-Schaltung Juni 2013) --> KOBV, 2013
 <p>

- Test des Codes mit verschiedenen Bibliotheken (UB Regensburg, TU Berlin) --> KOBV, 2013
 <p>

- Ver�ffentlichung, Dokumentation --> KOBV, 2014




<h6>Erl�uterung <p>

Bei Aufruf eines Katalogisats (unabh�ngig vom Bibliotheksinformationssystem), sorgt die in die Webseite eingebundene Javascript-Datei daf�r, die detaillierte Benennung f�r die vorhandenen RVK-Notationen via RVK-API (bereitgestellt durch die UB Regensburg) abzugleichen und dem Nutzer anzuzeigen.
 Dabei wird nun die direkte Benennung/der Klarname der Notation angezeigt, bei einem Klick auf diesen Bereich wird der gesamte RVK-Notationsbaum mit allen Abh�ngigkeiten gezeigt.



Beispiel
 <p>

RVK-Notation PZ 3300 <p>


mit Javascript_Datei

PZ 3300: Schuldrecht, insbesondere Vertragsrecht <p>


ausgeklappt

PZ 3300: Schuldrecht, insbesondere Vertragsrecht <p>

PZ 3000 - PZ 3800: Computerrecht, EDV-Recht, Recht des Internet <p>

PZ: Datenverarbeitung und Recht <p>

P: Rechtswissenschaft



<h6>
Funktion <p>

Die Javascript-Datei enth�lt mehrere Arbeitsschritte. <p>

- Zun�chst wird die Webseite bei Aufruf nach RVK-Notationen abgesucht (function rvk_stringifyRVKLinks)

- Werden RVK-Notationen gefunden, wird f�r jede einzelnen Notation die RVK-API (UB Regensburg) angefragt (function rvk_getRVKData(rvk))

- Die RVK-API antwortet (mit der Benennung/dem Klarnamen und dem abh�ngigen Baum) im Format XML, JSON oder JSONP (function rvk_parseRequest(response)).

- Das Javascript ersetzt die RVK-Notation mit der Benennung/dem Klarnamen und dem Wurzelbaum (function rvk_visualize (rvk_array,notation)).

- Bei Klick auf die Notation/Benennung �ffnet sich der Baum (function rvk_path(node) ; function rvk_pathAsArray(node,key) ; function rvk_notation(node)).
 <p>

F�r die Aufl�sung der Notationen muss die RVK-API ansprechbar sein. Sollte dies nicht der Fall sein, wird die Notation nicht aufgel�st.



<h6>
Installation <p>

Die Javascript-Datei sowie die Stylesheet-Datei m�ssen am gleichen Ort wie die anderen f�r die Darstellung der Webseite ben�tigten Dateien abgelegt werden.

Damit die Javascript-Datei korrekt arbeitet, m�ssen folgende zwei Zeilen Code in die Header-Section der auszuliefernden Webseite eingebunden werden:


< link href="rvk_style.css" type="text/css" rel="stylesheet" >
< script src="rvk_links.js" type="application/javascript"></script >.



<h6>
Ben�tigte Konfiguration <p>

Um RVK-Notationen auffinden zu k�nnen, m�ssen folgende Anpassungen vorgenommen werden.
 <p>

Im aktuellen Code der Javascript-Datei werden die RVK Notationen �ber div-Klassen mit der Bezeichnung <p>

�rvklink�  

> < div class='rvklink'>PZ 3300 < / div >
<p>
 als RVK-Notation erkannt und dann automatisch die in der Javascript-Datei vordefinierten Arbeitsschritte angeworfen.
 <p>

Werden die RVK-Notationen im Code der auszuliefernden Webseite mit einem anderen Element als �div� gekennzeichnet, muss dies <p>

a. in der Javascript-Datei angepasst werden: <p>

    Ersetzen Sie im Code in der Zeile 36
 <p>

> function rvk_stringifyRVKLinks() { <p>

> var arr = new Array(); <p>

> arr = document.getElementsByTagName( "div" ); <p>


> for(var i=0; i < arr.length; i++) <p>

{ <p>
var tagObj = arr[i]; <p>

if (tagObj.className==="rvklink") { <p>

var rvkTag=tagObj.innerHTML; <p>

rvkTag=rvkTag.replace(/\ /,"+"); <p>

tagObj.setAttribute("name",tagObj.innerHTML); <p>

rvk_getRVKData(rvkTag); <p>

//alert("\n\ninnerText:\n" + tagObj.innerHTML);  <p>

}}} <p>


b. muss das beschreibende Element im Code der auszuliefernden Webseite mit der Benennung �rvklink� versehen werden: <p>


Damit die RVK-Notationen im System gefunden werden k�nnen, m�ssen Sie also auch dementsprechend im Website Code gekennzeichnet werden. <p>



> < span class="rvk"> NR 6910 < / span> wird zu < span class="rvklink"> NR 6910 < / span>.
 <p>




<h6>
Release/ Freigabe <p>
Der Code gemeinsam mit einer Kurzdokumentation wird freigegeben unter der Lizenz GNU GPLv3.


 <p>

<h6>
Anhang <p>

Kurzdokumentation der RVK-API unter: http://rvk.uni-regensburg.de/api/
