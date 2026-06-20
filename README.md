# Rapport-projet-CYBEL

Project structure and LaTeX build

Organisation proposée :

- `tex/` : fichiers LaTeX de niveau (intro, abstract, resume, remerciements, list_abreviation, conclusion)
- `tex/chapters/` : chapitres (chap1..chap4)
- `diagramme/`, `diagrammes/`, `images/` : figures (non déplacées automatiquement)
- `docs/` : documentation (ce fichier)

Compilation rapide (Windows, avec TeX Live/MiKTeX):

```powershell
cd "c:\Users\clusa\Desktop\redaction_latex"
latexmk -pdf -interaction=nonstopmode -file-line-error main.tex
```

Remarques :

- Les images et diagrammes restent dans `images/` et `diagramme/` pour l'instant.
- Après avoir vérifié la compilation, on pourra standardiser et déplacer les figures vers `figures/`.
