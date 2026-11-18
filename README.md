# Tartu Tourist Guide

Application mobile et desktop (.NET MAUI) qui sert de guide touristique de **Tartu**, en Estonie.
Elle liste 51 lieux répartis en quatre catégories, permet de cocher ceux déjà visités et débloque
des récompenses au fil de la progression.

## Fonctionnalités

- **51 lieux** : History (20), Relaxation (8), Entertainment (12), Restaurants (11)
- Une fiche par lieu : photo, description, adresse, lien vers le site et bouton « visité »
- **Progression globale** sur l'écran d'accueil (`x / 51`) et détail par catégorie
- **5 récompenses** débloquées automatiquement selon les lieux visités
- Progression **enregistrée sur l'appareil** : elle est conservée entre deux lancements

## Prérequis

| Outil | Version | Nécessaire pour |
| --- | --- | --- |
| [.NET SDK](https://dotnet.microsoft.com/download) | 11 | tout |
| Workload MAUI | `dotnet workload install maui` | tout |
| Visual Studio 2026 (optionnel) | 18.x | éditer et déboguer |
| JDK 21 + Android SDK | — | uniquement pour la cible Android |

Visual Studio 2022 ne sait pas cibler `net11.0` : utilisez Visual Studio 2026, ou la ligne de commande.

## Installation

```bash
git clone https://github.com/yalac/TartuTouristGuide.git
cd TartuTouristGuide

# une seule fois, si le workload n'est pas déjà installé
dotnet workload install maui

# restauration des dépendances
dotnet restore
```

## Lancer l'application

### Windows (recommandé)

```bash
dotnet build -f net11.0-windows10.0.19041.0
dotnet build -t:Run -f net11.0-windows10.0.19041.0
```

Dans Visual Studio : ouvrez `TartuTouristGuide.sln`, choisissez la cible **Windows Machine** et
appuyez sur F5. Si ce bouton refuse de lancer l'app alors que la compilation réussit, utilisez la
commande `dotnet build -t:Run` ci-dessus : elle fait la même chose de façon fiable.

### Android (optionnel)

```bash
dotnet build -t:Run -f net11.0-android
```

Un émulateur ou un téléphone connecté est nécessaire, ainsi que le JDK 21 et l'Android SDK.

### iOS / Mac Catalyst

Compilation possible depuis un Mac avec Xcode. Les cibles existent dans le projet mais ne sont pas
nécessaires pour faire tourner l'application sur Windows.

## Structure du projet

```
TartuTouristGuide/
├── MauiProgram.cs        Point d'entrée : injection des services, ViewModels et pages
├── App.xaml(.cs)         Application et fenêtre principale
├── AppShell.xaml(.cs)    Navigation et routes (une route par écran)
├── Data/
│   ├── PlacesData.cs     Les 51 lieux (contenu statique)
│   └── RewardsData.cs    Les 5 récompenses et leurs conditions
├── Models/               Place, Reward
├── ViewModels/           Logique des écrans (MVVM), un par page
├── Views/                Interfaces XAML : Home, CategoryList, PlaceDetail, Rewards, Tartu101
├── Services/             VisitedPlacesService : lecture/écriture de la progression
└── Resources/            Images, icônes, polices, styles
```

La progression n'utilise pas de base de données : les identifiants des lieux visités sont stockés
dans les préférences de l'appareil, sous la clé `visited_Places`.

## Réinitialiser sa progression

Désinstallez l'application, ou effacez ses données depuis les réglages du système. Sur Windows,
supprimez le dossier de données local de l'application.
