# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Nature du dépôt

Ce dépôt contient les **fichiers de configuration Cura 5.12** pour deux imprimantes 3D :
- **Alfawise U20** (imprimante principale)
- **Dagoma DiscoEasy200**

Il ne s'agit pas d'un projet logiciel — il n'y a pas de build, tests, ni lint. Les fichiers sont des configs INI gérées par Cura.

## Structure des fichiers

| Dossier | Contenu |
|---|---|
| `machine_instances/` | Profils machine globaux (empilement de conteneurs Cura) |
| `definition_changes/` | Surcharges de paramètres machine/extrudeur |
| `extruders/` | Configurations d'extrudeurs |
| `quality_changes/` | Profils d'impression personnalisés |
| `quality/` | Profils qualité de base |
| `user/` | Réglages utilisateur par imprimante |
| `cura.cfg` | Configuration générale de l'application |

## Format des fichiers `.inst.cfg` / `.extruder.cfg` / `.global.cfg`

Tous les fichiers de configuration suivent ce format INI :

```ini
[general]
version = 4
name = Nom du profil
definition = machine_definition_id  # ou fdmprinter pour les profils génériques

[metadata]
type = quality_changes | definition_changes | machine | extruder_train
quality_type = draft | fine | normal
setting_version = 26

[values]
parametre = valeur
```

## Profils quality_changes existants (Alfawise U20)

- **Draft Classique PETG** — 0.2mm, bed 70°C, raft, supports partout
- **Bague Filament Classique PLA** — 0.06mm (très fin), bed 60°C, raft
- **Bague Filament Bois PLA** — 0.2mm, bed 60°C, raft
- **Bague Filament Classique PETG** — profil PETG dédié

Chaque profil quality_changes a un fichier machine (`alfawise_u20_*.inst.cfg`) et un fichier extrudeur associé (`alfawise_u20_extruder_0_%232_*.inst.cfg`).

## Empilement des conteneurs Cura (machine_instances)

Dans `machine_instances/Alfawise+U20.global.cfg`, les conteneurs sont listés par priorité décroissante (0 = priorité la plus haute) :
```
0 = user settings
1 = quality_changes (profil actif)
2 = intent
3 = quality (base)
4 = material
5 = variant
6 = definition_changes
7 = machine definition
```

## Paramètres clés Alfawise U20

Le G-code de fin est défini dans `definition_changes/Alfawise+U20_settings.inst.cfg` : home X, amène le plateau Y280, rétracte 50mm de filament.
