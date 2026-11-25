# DEALS GO — Application mobile (Flutter)

Application clients + conducteurs, connectée à Supabase. Cartes OpenStreetMap.

## Obtenir l'APK sans rien installer (recommandé)
1. Créez un dépôt GitHub vide, puis envoyez-y TOUT le contenu de ce dossier (y compris `.github`).
2. Ouvrez l'onglet **Actions** > « Build APK DEALS GO » > **Run workflow** (ou faites un push).
3. Au bout de quelques minutes, ouvrez l'exécution terminée et téléchargez l'artefact **deals-go-apk** : c'est `app-release.apk`.
4. Installez-le sur un téléphone Android (autoriser les sources inconnues).
   Si le build échoue, copiez l'erreur du journal : elle sera corrigée.

## Compiler soi-même
```
flutter create --platforms=android,ios --org com.dealsgo --project-name deals_go .
python3 tool/apply_platform_config.py      # permissions Android/iOS + nom de l'app
flutter pub get
flutter run                                  # ou : flutter build apk --release
```
Flutter recommandé : 3.24.x.

## Configuration
La clé publique Supabase est déjà dans `lib/core/config/env.dart` (clé « anon », conçue pour être dans l'app).
Surcharge possible : `--dart-define=SUPABASE_URL=... --dart-define=SUPABASE_ANON_KEY=...`.
Lien de partage de trajet : `--dart-define=SHARE_BASE_URL=https://votre-site.vercel.app/share/`.

## Fonctionnement
- Inscription : numéro +225 + mot de passe, puis écran « En attente de confirmation ». L'administrateur génère un code à 6 chiffres et l'envoie sur WhatsApp ; l'utilisateur le saisit.
- Client : carte, Taxi auto / moto, destination, liste des conducteurs, choix du conducteur, suivi en direct, appel, messages, partage de trajet, SOS, note, reçu, historique, favoris, contacts d'urgence, assistant, interurbain.
- Conducteur : en ligne / hors ligne, demandes (accepter / refuser), navigation, gains, véhicule, entretien, abonnement avec preuve Wave (vérifiée MANUELLEMENT par l'administration), certification, trajets interurbains.

## Limites à connaître
- Pas de notifications push du système (il faut un compte Firebase/Apple) : les notifications sont reçues dans l'application ouverte (temps réel).
- La position du conducteur est envoyée uniquement quand l'application est ouverte (pas de localisation en arrière-plan).
- Recherche d'adresses (Nominatim), itinéraires (OSRM) et tuiles (OpenStreetMap) : services publics gratuits à usage modéré. Pour une exploitation intensive, prévoir un fournisseur dédié.
- Le logo officiel n'a pas été fourni : un symbole provisoire est affiché. Le logo se remplace depuis l'administration (Contenu > Logo et bannières).
- Application non testée sur appareil réel : première compilation à valider.


## Publier la version Web sur Vercel (iPhone possible)

Ce dépôt est le code source Flutter, pas le site compilé. Vercel ne peut pas servir directement ce ZIP comme une app Flutter.

1. Envoyez le contenu du dossier à un dépôt GitHub (pas le ZIP lui-même comme fichier).
2. Dans GitHub, ouvrez **Actions** et lancez **Build DEALS GO Web** (ou faites un push sur main/master).
3. Une fois le workflow terminé, téléchargez l'artefact **deals-go-web**. Il contient les fichiers web compilés.
4. Sur Vercel, créez un projet statique à partir de ces fichiers compilés. Configurez le dossier de publication sur la racine contenant `index.html`; aucun build Flutter ne doit être exécuté par Vercel.

Le workflow compile le code pour le Web; il ne modifie pas vos fichiers Flutter d'origine. Vérifiez toutefois le build dans GitHub Actions avant de publier.
