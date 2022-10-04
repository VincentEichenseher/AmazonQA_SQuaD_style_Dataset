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

- model_finetuned_amazonqa.zip: 
Das finetuned Modell, komprimiert als .zip Datei für schnelleres Herunterladen

-Aufbereitete_Daten

-BaseModelSelectionData

-Code

-FinetuningVisualized



