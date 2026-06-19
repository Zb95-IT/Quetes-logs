# Quête Les logs

TP réalisé sur VM Ubuntu 24 LTS avec Apache 2.4.

## Ton serveur web est correctement configuré et génère des logs

Installation : `sudo apt install apache2 -y`

<img width="805" height="387" alt="image" src="https://github.com/user-attachments/assets/76735eed-be6d-401c-a885-03ab24b390be" />



Après génération de trafic avec `curl` (requêtes vers la home + URLs inexistantes), l'analyse des logs avec `grep`, `awk` et `sort` donne **298 requêtes 200**, **4 erreurs 404** et **127.0.0.1** comme IP principale (trafic en localhost).

<img width="806" height="379" alt="image" src="https://github.com/user-attachments/assets/3001594e-4275-4382-96fc-dea6a437e036" />


## Tu peux expliquer la structure des logs de ton serveur web

Les logs Apache sont dans `/var/log/apache2/`. Deux fichiers :
- `access.log` : toutes les requêtes HTTP reçues
- `error.log` : les erreurs serveur

### Structure d'`access.log` (format combined)

Exemple de ligne :
```
127.0.0.1 - - [19/Jun/2026:20:36:24 +0200] "GET /admin HTTP/1.1" 404 487 "-" "curl/8.5.0"
```

- `127.0.0.1` → IP du client
- `- -` → identité + utilisateur authentifié (vides ici)
- `[19/Jun/2026:20:36:24 +0200]` → horodatage
- `"GET /admin HTTP/1.1"` → méthode + URL + version HTTP
- `404` → code de statut HTTP (200 = OK, 404 = non trouvé, 500 = erreur serveur)
- `487` → taille de la réponse en octets
- `"-"` → page d'origine (Referer)
- `"curl/8.5.0"` → logiciel client (User-Agent)

### Structure d'`error.log`

Exemple de ligne :
```
[Fri Jun 19 20:08:55 2026] [core:notice] [pid 4282] AH00094: Command line: '/usr/sbin/apache2'
```

- `[Fri Jun 19 20:08:55 2026]` → horodatage
- `[core:notice]` → module Apache + niveau de gravité (`notice`, `warn`, `error`, `crit`)
- `[pid 4282]` → identifiant du processus
- `AH00094` → code de l'événement Apache
- Le reste : message descriptif
