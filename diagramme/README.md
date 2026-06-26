# Diagrammes Mermaid — rapport CYBEL

Générer les PNG sur [mermaid.live](https://mermaid.live) (Actions → PNG) ou via CLI :

```powershell
cd "c:\Users\clusa\Desktop\redaction_latex\diagramme"
npx -p @mermaid-js/mermaid-cli mmdc -i architecture_couches.mmd -o architecture_couches.png -b white
```

## Correspondance fichier → PNG → rapport LaTeX

| Fichier `.mmd` | PNG à exporter | Utilisé dans |
|----------------|----------------|--------------|
| `architecture_generale.mmd` | `architecture_generale.png` | chap. 4 |
| `architecture_couches.mmd` | `architecture_couches.png` | chap. 4, annexe E |
| `diagramme_composants.mmd` | `diagramme_composant.png` | chap. 4, annexe E |
| `cas_utilisation.mmd` | `diagramme_cas_usage.png` | chap. 4 |
| `diagramme_classes_sdk.mmd` | `diagramme_classes_sdk.png` | annexe E (optionnel) |
| `sequence_navigation.mmd` | `sequence_navigation.png` | chap. 4 |
| `sequence_tts.mmd` | `sequence_tts.png` | annexe E |
| `sequence_telemetry.mmd` | `sequence_telemetry.png` | annexe E (optionnel) |
| `sequence_lab_tour.mmd` | `sequence_lab_tour.png` | annexe E |
| `sequence_tour_halt.mmd` | `sequence_tour_halt.png` | annexe E (optionnel) |
| `topologie_reseau.mmd` | `topologie_reseau.png` | annexe E |
| `gantt_planning_stage.mmd` | `diagramme-gantt-transparent.png` | chap. 5 |

Après export, remplacer les `\fbox{...}` dans `tex/annexe.tex` par `\includegraphics`.
