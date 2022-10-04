# Finetuning eines Reader-Modells für Review-Based Question-Answering

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

- **FinetuningVisualized** Die Veränderungen des Finetuned Modells während dem Training. Der Code, mit dem die Daten visualisiert wurden, befindet sich in dem Verzeichnis [Code](https://github.com/VincentEichenseher/Finetuning-eines-Reader-Modells-fuer-Review-Based-Question-Answering/tree/main/Code/Finetuning_visualisiert.ipynb)



