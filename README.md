# 🦠 Ransomware Classification: Sequential (CNN-1D) vs Frequentist (ML) API Analysis

**Contexte :** Mémoire de Maîtrise (M.Sc. in Computer Science) - Université du Québec en Outaouais (UQO)

## 📝 Vue d'ensemble (Overview)
Ce projet de recherche propose un cadre de détection anticipée des ransomwares, capable d'identifier les activités malveillantes **avant le début du chiffrement des données (phase préchiffrement)**. 

L'étude évalue et compare deux approches distinctes basées sur l'analyse dynamique des appels API Windows :
1. **Approche Fréquentielle (ML Classique) :** Utilisation de LightGBM et Random Forest sur des représentations *Bag of Words (BoW)* et *Occurrence of Words (OoW)*.
2. **Approche Séquentielle (Deep Learning) :** Utilisation de Réseaux de Neurones Convolutifs (CNN-1D) sur des représentations *Sequence of Words (SoW)* pour capturer la syntaxe comportementale et l'ordre temporel des attaques.

## 🏗️ Pipeline d'Ingénierie des Données (Data Engineering)
Pour garantir la fiabilité de la "vérité terrain" (Ground Truth) et éliminer le bruit inhérent aux environnements Sandbox, un pipeline de traitement automatisé robuste a été développé :
* **Extraction Dynamique :** Exécution de 1 570 échantillons dans l'environnement isolé **CAPEv2**.
* **Nettoyage & Parsing :** Utilisation du framework **MALVADA** pour filtrer les rapports incomplets, gérer les timeouts et dédupliquer les traces (SHA-512).
* **Étiquetage Probabiliste :** Intégration de **ClarAVy** pour résoudre les conflits de nommage des antivirus via un modèle bayésien, garantissant un étiquetage familial d'une précision supérieure à 95%.
* **Sélection de Caractéristiques :** Réduction du vocabulaire à **46 appels API discriminants** via l'Information Mutuelle (Mutual Information).

## 🎯 Menaces Ciblées (Jeu de données)
Le dataset expérimental final comprend **1 055 échantillons** (707 ransomwares, 348 fichiers bénins issus du DikeDataset). Les ransomwares sont répartis en 14 familles contemporaines, incluant des acteurs majeurs du modèle RaaS et de la double extorsion :
* **Familles dominantes :** Conti, REvil, Maze, LockBit, DarkSide
* **Autres menaces :** BlackCat, RansomHub, Ryuk, WannaCry, Cerber, CryptoLocker, CryptoWall, Locky, NotPetya.

## 🧠 Architecture d'Apprentissage Profond (CNN-1D)
Le modèle séquentiel a été conçu pour traiter des séquences normalisées de 1500 appels API :
* **Couche d'Embedding :** Projection vectorielle (dimension 32).
* **Convolutions (Conv1D) :** Extraction des motifs locaux (128 filtres, puis 64 filtres, activation ReLU).
* **Global Max Pooling 1D :** Identification globale des comportements malveillants sur la séquence.
* **Classification (Dense) :** Couche totalement connectée avec Dropout (0.5) pour la prévention du surapprentissage, sortie Sigmoid.

## 📊 Résultats et Découvertes Clés
L'évaluation a été menée avec une validation croisée **Group K-Fold** et le protocole **LOFO (Leave-One-Family-Out)** pour simuler des attaques Zero-Day.

* **Performance Maximale Globale :** Le modèle **LightGBM avec la représentation BoW** a obtenu le meilleur score global avec un **F1-Score de 0.917** et une précision de 0.971, prouvant l'efficacité des gradients boostés sur les données fréquentielles préchiffrement.
* **Robustesse Séquentielle (Zero-Day) :** Le modèle **CNN-1D (F1-Score global de 0.886)** a surpassé les modèles classiques sur l'évaluation LOFO pour des familles atypiques et difficiles (comme *WannaCry* et *CryptoLocker*), démontrant que la préservation de l'ordre d'exécution (SoW) capture des intentions malveillantes indétectables par la simple fréquence.

## 🛠️ Stack Technique
* **Analyse de Malware (Sandbox) :** CAPEv2
* **Data Pipeline :** MALVADA, ClarAVy
* **Machine / Deep Learning :** Python 3, Scikit-Learn, LightGBM, TensorFlow/Keras
* **Optimisation :** Optuna
