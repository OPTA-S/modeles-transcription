# Modèles de transcription (skill Claude « transcription-reunions »)

Ce dépôt héberge les modèles utilisés par le skill Claude **transcription-reunions** d'OPTA-S. Il permet à Claude de transcrire des enregistrements (dictaphone, réunions Teams, entretiens) avec un simple abonnement, sans connecteur : l'environnement de calcul de Claude a le droit de télécharger depuis github.com, mais pas depuis les plateformes de modèles habituelles.

## Contenu

| Dossier | Modèle | Usage | Licence |
|---|---|---|---|
| `whisper-large-v3-turbo-int8/` | Whisper large-v3-turbo (OpenAI), format CTranslate2, quantifié int8 | Reconnaissance vocale (faster-whisper) | MIT |
| `locuteurs/segmentation.onnx` | pyannote segmentation 3.0, export ONNX sherpa-onnx | Détection des tours de parole | MIT |
| `locuteurs/embedding.onnx` | 3D-Speaker ERes2Net base, export ONNX sherpa-onnx | Distinction des voix | Apache-2.0 |

Les fichiers de plus de 45 Mo sont découpés en parties (`*.part-00`, `*.part-01`…) pour respecter les limites de GitHub sans Git LFS. Le fichier `manifeste.json` donne pour chaque fichier reconstitué sa taille, son empreinte SHA-256 et la liste ordonnée de ses parties.

## Reconstitution

```bash
git clone --depth 1 https://github.com/OPTA-S/modeles-transcription
cd modeles-transcription
cat whisper-large-v3-turbo-int8/model.bin.part-* > whisper-large-v3-turbo-int8/model.bin
cat locuteurs/embedding.onnx.part-* > locuteurs/embedding.onnx
sha256sum whisper-large-v3-turbo-int8/model.bin locuteurs/embedding.onnx   # comparer avec manifeste.json
```

Le skill fait tout cela automatiquement (script `preparer.py`).

## Origine

- Whisper large-v3-turbo : OpenAI, conversion CTranslate2 int8 publiée sur Hugging Face (Zoont/faster-whisper-large-v3-turbo-int8-ct2), vérifiée par OPTA-S sur un enregistrement de test en français (qualité équivalente à la version float16).
- Modèles de locuteurs : projet k2-fsa/sherpa-onnx (versions publiées sur GitHub).

Aucune donnée audio ni transcription n'est stockée dans ce dépôt.
