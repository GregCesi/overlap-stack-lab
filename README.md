# Overlap Stack Lab

Outil de composition photo fait pour un projet client réel — [Jardin de Marguerite](https://jardینdemarguerite.fr). Permet d'assigner et d'agencer des photos par section dans différents layouts superposés, avant intégration dans le site.

Fichier HTML autonome, zéro dépendance, zéro installation.

---

## Ce que ça fait

- **Banque de photos** — import par drag & drop, filtrage par section
- **5 variants de layout** — compositions superposées préconfigurées
- **Édition par slot** — zoom, recadrage, repositionnement par photo
- **Auto-save** — persistance localStorage, rien à installer côté serveur

## Utilisation

### 1. Avec un serveur local (recommandé)

```bash
# Python (installé par défaut sur macOS/Linux)
python3 -m http.server 8000
# puis http://localhost:8000/overlap-stack-lab.html
```

> **Pourquoi un serveur ?** L'auto-save repose sur `localStorage`, qui est instable sur `file://` (Safari le bloque, Chrome crée des conflits entre fichiers). Avec `localhost`, l'origine est propre et la persistance est garantie entre sessions.

### 2. Test rapide sans installation

Télécharge le fichier et double-clique dessus. L'outil fonctionne — la sauvegarde des réglages (zoom, recadrage) ne sera juste pas fiable entre sessions selon le navigateur.
