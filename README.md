# Politische Stimmung im Social Web

Mit Hilfe der Twitter - API sammeln wir Tweets, die mindestens eine der folgenden Parteien nennen: CDU/CSU, SPD, FDP, Bündnis90/Die Grünen\*, DIE LINKE, AfD.

agalea91 gibt eine umfangreiche Anleitung für einen scrape-code. [Hier](https://github.com/agalea91/twitter_search) gehts zu seinem repository.

Im Focus unserer Analyse stehen sowohl die Quantität der Tweets zu den einzelnen Parteien als auch deren jeweilige Tonalität. Wird also eine Partei häufig bei Twitter kommentiert? Und steht der Tweet eher in einem positiven oder negativen Kontext? Und welche Wörter werden am häufigsten in den Tweets genannt. Ebenso erstellen wir einen Index über die Tonalität der Tweets und vergleichen somit die Stimmung zu den Parteien im Social Web.

Die Untersuchung führen wir mit den Methoden des Text Minings - z.B. Term-Häufigkeit oder Sentiment Analyse - mithilfe der Statistik Software R durch.

Alle Rmd Files (R-Code) sind [hier](https://github.com/franziloew/politsentiment/tree/master/Rmd_files) zu finden

# A) Analysen

#### SPD Mitgliedervotum zur Groko [05.März 2008](https://franziloew.github.io/politsentiment/SPD_mitgliedervotum.html)

#### Sentimentanalys Jamaika-Aus [24.November](https://franziloew.github.io/politsentiment/sentimentR.html)

#### 19.Bundestag [24.Oktober](bundestag.md)

#### Oktober I [07.Oktober - 14.Oktober](07_14_okt.md)

#### Digitalisierung [26.September - 14.Oktober](01_07_10.md)

### Bundestagswahl 2017

#### Nach der Wahl II [01.Oktober - 07.Oktober](01_07_okt.md)

#### Nach der Wahl I [26.September - 29.September](25_30.md)

#### Wahltag [24.September](election.md)

#### Vor der Wahl II [18.September - 23.September](18_23_09.md)

#### Vor der Wahl I [08.September - 15.September](17_09.md)

#### TV Duell [09. September](TVDuell.md)

# B) Analyse-Schritte

### 1. Von welchen Plattformen werden die meisten Tweets gesendet?
Einige Parteien, Newsportale, etc. benutzen autmatisierte Twitter-Bots um bestimmte Themen viraler zu machen. Wir schauen uns an, von welchen Plattformen die Tweets abgesetzt wurde.

### 2. Wer retweeted wen? Und was?
Hierzu erstellen wir eine Netzwerkdarstellung, in der die Größe der Knotenpunkte den "Retweet-Grad" darstellt (retweeten und retweeted werden). Aus Gründen der Übersichtlichkeit, werden nur nicht alle User-Namen aufgeführt.

Aus der Twitter’s API wird nicht erkennenbar, woher ein Retweet kommt. Aber, man kann erkennen, wer retweeted wird und wer, bzw. wieviele an der Konversation teilnehmen und welche Accounts bei eines solche Konverstation im "Mittelpunkt" stehen.

Die Netzwerkdarstellung gibt also einen guten Überblick darüber, wo sich Knotenpunkte bilden.

### 3. Über welche Parteien wird am meisten getweeted?
Wir betrachten, in wievielen Tweets eine Partei im Verhältnis zu der gesamten Anzahl der Tweets genannt wird.

Wir unterscheiden zwischen:

  a) Großen News-Portalen.
  Viele davon verwenden mehrere Twitter-Accounts (und wir haben versucht alle zu berücksichtigen)
  FOCUS Online, FAZ, SPIEGEL ONLINE, stern, BILD, N24, ntv, WELT, ZEIT ONLINE, Handelsblatt, BuzzFeedNewsDE, BW Breaking News, taz, HuffPost Deutschland, MEEDIA, Der Tagesspiegel, Süddeutsche Zeitung, Stuttgarter Zeitung, Hamburger Abendblatt, Westdeutsche Zeitung, FrankfurterRundschau, ZDF, tagesschau, tagesthemen, Die Nachrichten, Deutschlandfunk, DW (Deutsch), PHOENIX, WDR Aktuelle Stunde, NDR, MDR

  b) Anderer User - Accounts.
  Um die Tweets zu betrachten, die nicht von den großen privaten und öffentlichen Nachrichtendiensten gesendet wurde, werden alle Tweets gefiltert, die nicht von diesen Nachrichtenportalen und die nicht von automatisierten Bots gesendet wurden.

### 4. Term - Häufigkeit
#### 4.1. Wordclouds
Welche Wörter werden am häufigsten in Verbindung mit den Parteien getweetet? Zur Visualisierung der am häufigsten verwendeten Wörter in Bezug auf eine Partei, erstellen wir eine Wordcloud.


#### 4.2 term frequency - inverse document frequency (tf-idf)
Die Idee des tf-idf Wertes (aus dem englischen "term frequency - inverse document frequency") ist es, die Relevanz eines Wortes für den Inhalt eines Dokumentes (in diesem Fall einer Partei) zu finden - und zwar im Vergleich zu allen im Korpus enthaltenen Dokumente (bzw. Parteien).

TF(t) = (Anzahl von Term t pro Patei) / (Anzahl aller Terme pro Partei)

IDF(t) = log\_e(Anzahl aller Parteien / Anzahl von Parteien, die den Term t enthalten)

In den folgenden Abbildungen, sind die Wörter (sog. unique terms) mit den höchsten tf-idf Werten pro Partei aufgelistet. Diese Wörter werden sind also im Zusammenhang einer Partei am "relevantesten".

### 5. Sentiment Analyse

Mittels der Sentimentanalyse versucht man, Stimmungen zu analysieren um zu erkennen, ob ein Tweet eine positive oder negative Stimmung oder Meinung ausdrückt.

Interessant ist zunächst der Blick auf die am meisten verwendeten positiven und negativen Wörter um die Stimmung oder Emotionen (Sentiment) im Zusammenhang mit den Parteien auszulesen (ungewichtete Sentiment-Analyse)   

Gewichtete Sentiment-Analyse: Bei der gewichten Analyse wird den einzelnen "Sentiment" Worten ein Wert zugewiesen. Der Score gibt den Wert an, der sich aus der Summe der zugewiesenen Werte der positiven und negativen Wörter ergibt.

Das Prinzip dieser Analyse im einfachsten Fall so:

Für jedes Wort im Text:
  Überprüfe, ob das Wort im Lexikon\* existiert.
  Wenn JA, dann:
    Weise dem Wort den Sentiment-Wert aus dem Lexikon zu UND
    Addiere diesen Wert zu dem Sentimentwert des Dokuments.
  Wenn NEIN, dann:
    Gehe weiter zum nächsten Wort.

  Gebe die Summenwerte pro Sentiment (z.b. negativ, positiv)

Hierbei sei darauf hingewiesen, dass der Wert für eine Partei nicht unbedingt die Stimmung bzgl. dieser Partei widergibt. Vielmehr ist der Wert ein Indikator dafür, wie die Wortwahl in Zusammenhang mit dieser Partei ist. Da viele Tweets aber mehr als eine Partei beinhalten, kann sich ein negatives oder ein positives Wort auch auf eine andere Partei beziehen. Interessant ist in diesem Hinblick dann ein Blick in die Tweets mit den entsprechend negativsten oder positivsten Werten.

\* Wir verwenden das Lexikon der [Leipzig Corpora Collection](http://wortschatz.uni-leipzig.de/de/download).
