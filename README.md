# Finetuning eines Reader-Modells für Review-Based Question-Answering

## Abstract

Viele Nutzer von E-Commerce Seiten berufen
sich auf Reviews, um eine Kaufentscheidung
zu treffen. Da diese Aufgabe mit zunehmender
Anzahl und Umfang von Reviews schwieriger
wird, scheint es sinnvoll, ein Question Answering System zu trainieren, das die Fragen der
Nutzer mit Informationen aus den Reviews
beantworten kann. Mit diesem Ziel wurden
mehrere Reader, die auf SQuaD vortrainierte
Transformer Modelle umsetzten, auf dem AmazonQA Datensatz von 
[Gupta et al. (2019)](https://arxiv.org/pdf/1908.04364.pdf) Datensatz getestet. Anschließend wurde das beste
dieser Modelle, DeBERTaV3 base, 5 Epochen
lang auf einem Teil von AmazonQA trainiert.
Der F1 Score des resultierenden Modells ist
mit 37.94 um 28.5% höher als DeBERTav3
base und um 11.6% höher als die Baseline von
[Gupta et al. (2019)](https://arxiv.org/pdf/1908.04364.pdf) . Der Exact Match Score des
Finetuned Model ist mit 0.94 zwar dreimal so
hoch wie der von DeBERTaV3 base, jedoch
84.2% unter der Baseline. Bessere Ergebnisse
könnten möglicherweise mit einer höheren
Maximalen Sequence Tokenlänge erreicht wer-
den.

## Benutzung des Finetuned Reader Modells

### Schritt 1: Laden und entpacken des Modells
```
!wget https://github.com/VincentEichenseher/Finetuning-eines-Reader-Modells-fuer-Review-Based-Question-Answering/blob/main/model_finetuned_amazonqa.zip 
!unzip "model_finetuned_amazon.zip"
```

### Schritt 2: Erstellen eines Readers, der dieses Modell umsetzt
```
from haystack.nodes import FARMReader

reader = FARMReader(model_name_or_path="./model_finetuned_amazonqa", use_gpu=True)
```

### Schritt 3: Erstellen eines Retrievers und einer Pipeline, die die Reader und Retriever Komponenten kombiniert
```
from haystack.nodes import BM25Retriever
from haystack.pipelines import ExtractiveQAPipeline

retriever = BM25Retriever(document_store=document_store)
pipe = ExtractiveQAPipeline(reader, retriever)
```
wobei document_store ein DocumentStore ist, in dem Amazon Reviews geindext sind. Für wietere Informationen siehe die [Haystack Dokumentation](https://haystack.deepset.ai/components/document-store)

## Übersicht über die Dateien und Verzeichnisse in diesem Repository

- **model_finetuned_amazonqa.zip**: 
Das finetuned Modell, komprimiert als .zip Datei für schnelleres Herunterladen. Der Code, mit dem das Model trainiert wurde befindet sich in dem Verzeichnis [Code](https://github.com/VincentEichenseher/Finetuning-eines-Reader-Modells-fuer-Review-Based-Question-Answering/tree/main/Code/FinetuningOnSample.ipynb)

- **Aufbereitete_Daten**:
In diesem Verzeichnis sind die aufbereiteten (ins [SquadData](https://github.com/deepset-ai/haystack/blob/main/haystack/utils/squad_data.py) Format gebrachten) Splits des AmazonQA Datensatzes. Dabei sind sowohl die ganzen Splits als auch die reduzierten Splits, die nur 10% ihrer originalen Größe sind, vorhanden. Für weitere Details siehe die README in diesem Verzeichnis. Der Code, mit dem die Datensätze aufbereitet wurde befindet sich in dem Verzeichnis [Code](https://github.com/VincentEichenseher/Finetuning-eines-Reader-Modells-fuer-Review-Based-Question-Answering/tree/main/Code/Preprocessing.ipynb)

- **BaseModelSelectionData**:
In diesem Verzeichnis sind die Ergebnisse der Evaluation der Modelle auf dem gesamten Validationssplit (in Abschnitt 3.2 der Arbeit) als .json Datei abgespeichert. Der Code, mit dem diese Ergebnisse Erhalten wurden befindet sich in dem Verzeichnis [Code](https://github.com/VincentEichenseher/Finetuning-eines-Reader-Modells-fuer-Review-Based-Question-Answering/tree/main/Code/BaseModelSelection.ipynb)

- **Code**: Der Code mit dem das Aufbereiten der Datensätze, die Auswahl des Basismodells, das Finetuning des Modells auf AmazonQA, die Evaluation der Modelle und das Visualisieren der Veränderungen des Modells während dem Training umgesetzt wurden. Für mehr Details siehe die README in diesem Verzeichnis.

- **EvaluationResults**:
In diesem Verzeichnis sind die Ergebnisse der Evaluation der Modelle auf dem gesamten Test-split (in Abschnitt 4 der Arbeit) als .json Datei abgespeichert. Der Code, mit dem diese Ergebnisse Erhalten wurden befindet sich in dem Verzeichnis [Code](https://github.com/VincentEichenseher/Finetuning-eines-Reader-Modells-fuer-Review-Based-Question-Answering/tree/main/Code/Evaluation.ipynb)

- **FinetuningVisualized**: Die Veränderungen des Finetuned Modells während dem Training. Der Code, mit dem die Daten visualisiert wurden, befindet sich in dem Verzeichnis [Code](https://github.com/VincentEichenseher/Finetuning-eines-Reader-Modells-fuer-Review-Based-Question-Answering/tree/main/Code/Finetuning_visualisiert.ipynb)

## Ergebnisse des Finetuned Reader Modells auf dem Test-Split von AmazonQA im Vergleich zu anderen Modellen





