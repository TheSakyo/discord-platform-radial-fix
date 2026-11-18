# Platform Radial Fix

> 🇬🇧 English version: [README.md](README.md)

Un petit thème de compatibilité pour Discord qui fait fonctionner ensemble le plugin **PlatformIndicators** (Equicord) et le thème **RadialStatus**, et qui restaure l'**indicateur d'écriture par défaut de Discord** que RadialStatus casse.

Ce thème **ne modifie pas** PlatformIndicators ni RadialStatus. Il surcharge uniquement le nécessaire par-dessus, donc les deux peuvent continuer à être mis à jour normalement.

---

## Qu'est-ce que ça corrige ?

| Problème | Sans le fix | Avec le fix |
| --- | --- | --- |
| Indicateur d'écriture (l'animation « ... ») | Un gros cercle sombre décalé de l'avatar, et les points dessinés par-dessus l'avatar | La pilule Discord par défaut avec 3 points animés, en bas à droite de l'avatar |
| Disposition des icônes de plateforme | Mal alignées à côté du pseudo à certains endroits | Icônes alignées horizontalement et espacées de façon cohérente (liste des membres, profils, messages) |
| Icône de téléphone mobile | En double (dessinée par PlatformIndicators et par RadialStatus) | Seule celle de PlatformIndicators est conservée |

---

## Prérequis

Il faut **tout** ce qui suit :

1. **Un mod de client Discord qui fournit le plugin ci-dessous**
   - [Equicord](https://github.com/Equicord/Equicord)
2. **Le plugin PlatformIndicators, activé**
   - [Equicord/src/plugins/platformIndicators](https://github.com/Equicord/Equicord/tree/main/src/plugins/platformIndicators)
   - Ses dépendances (`MemberListDecoratorsAPI`, `MessageDecorationsAPI`, `NicknameIconsAPI`) sont activées automatiquement par le mod.
3. **Le thème RadialStatus, activé**
   - [DiscordStyles/RadialStatus](https://github.com/DiscordStyles/RadialStatus)
4. **Un client Discord basé sur Chromium** (l'application de bureau officielle convient), car le thème utilise `:has()` et `@property`.

> Sans RadialStatus, ce thème n'a rien à corriger. Sans PlatformIndicators, la partie « Platform Indicators » du thème n'est simplement pas utilisée.

---

## Installation

> ⚠️ **L'ordre de chargement est important.** Ce thème doit être chargé **après** RadialStatus, sinon les variables de RadialStatus l'emportent sur celles définies ici.

### Option A : thème en ligne (recommandé, reste à jour)

1. Ouvre **Paramètres → Thèmes → Thèmes en ligne**.
2. Vérifie que la ligne de RadialStatus est **au-dessus** de celle-ci, puis ajoute :

```
https://raw.githubusercontent.com/TheSakyo/discord-platform-radial-fix/main/platform-radial-fix.theme.css
```

3. Active-le, c'est terminé.

### Option B : thème local

1. Télécharge [`platform-radial-fix.theme.css`](platform-radial-fix.theme.css).
2. Ouvre **Paramètres → Thèmes → Ouvrir le dossier des thèmes** et dépose le fichier dedans.
3. Active-le **après** RadialStatus.

---

## Personnalisation

Tout se règle avec trois variables CSS définies dans le bloc `:root` (section 1 du fichier). Tu peux modifier le fichier, ou les surcharger depuis ton propre CSS :

```css
:root {
    --vc-typing-bg: #2b2d31;   /* Couleur de fond de la pilule */
    --vc-typing-width: 69%;    /* Largeur de la pilule, relative à l'avatar */
    --vc-typing-height: 40%;   /* Hauteur de la pilule, relative à l'avatar */
}
```

| Variable | Défaut | Description |
| --- | --- | --- |
| `--vc-typing-bg` | `#2b2d31` | Couleur de fond de la pilule d'écriture |
| `--vc-typing-width` | `69%` | Largeur de la pilule, relative à la taille de l'avatar |
| `--vc-typing-height` | `40%` | Hauteur de la pilule, relative à la taille de l'avatar |

Astuce : garde une hauteur d'environ **58 % de la largeur** pour conserver les proportions de la pilule.

Les tailles étant en pourcentage, la pilule s'adapte à toutes les tailles d'avatar (32, 40, 80, 120px...). Les avatars de 16px sont ignorés volontairement.

---

## Comment ça marche

RadialStatus remplace le point de statut par un anneau coloré autour de l'avatar, selon ton statut (en ligne, inactif, ne pas déranger...). Mais pour l'indicateur d'écriture, ses règles agrandissent le fond de la pilule en un gros cercle sombre semi-transparent de 30px et forcent les coordonnées (`cx` / `cy`) des trois points, ce qui les décale par rapport à l'avatar. CSS ne peut pas annuler ces valeurs, donc au lieu de les combattre, ce thème :

1. **Masque** uniquement le rendu d'écriture de RadialStatus (`display: none !important`), sans toucher à l'anneau de statut.
2. **Détecte** un avatar en état « en train d'écrire » avec `:has(...)`.
3. **Redessine** la pilule Discord avec un pseudo-élément `::after` sur l'avatar, en utilisant trois dégradés radiaux comme points.
4. **Anime** l'opacité de chaque point via des propriétés personnalisées enregistrées (`@property`), avec 0,2s de décalage entre les points.

Le fichier est découpé en sections numérotées avec un sommaire en haut, pour le lire et l'ajuster facilement.

---

## Dépannage

**La pilule d'écriture n'apparaît pas**
- Vérifie que quelqu'un est **vraiment en train d'écrire** dans le salon pendant que tu regardes la liste des membres.
- Vérifie que le thème est chargé **après** RadialStatus.
- RadialStatus (et ce thème) s'appuient sur la classe hachée de Discord `wrapper_XpIUHy`. Discord change ces hash de temps en temps. Si la classe du conteneur d'avatar a changé, RadialStatus lui-même ne fonctionne plus non plus. Inspecte un avatar avec `Ctrl+Shift+I`, trouve la nouvelle classe `wrapper_xxxxxx` et remplace `XpIUHy` dans ce fichier.

**La pilule est trop grosse / trop petite / de la mauvaise couleur**
- Ajuste les trois variables décrites dans [Personnalisation](#personnalisation).

**L'icône de téléphone est toujours en double**
- Vérifie l'ordre de chargement : ce thème doit venir après RadialStatus pour que `--rs-phone-visible: none` l'emporte.

---

## Crédits

- **PlatformIndicators** : par kemo, TheSun, Nuckyz, Ven et neoarz, dans [Equicord](https://github.com/Equicord/Equicord) / Vencord.
- **RadialStatus** : par [Gibbu](https://github.com/DiscordStyles/RadialStatus).
- **Platform Radial Fix** : par TheSakyo.

Ce projet n'est affilié ni à Discord, ni à Equicord, ni à Vencord, ni à DiscordStyles. Les modifications de client ne sont pas approuvées par les conditions d'utilisation de Discord : à utiliser à tes risques.

## Licence

[MIT](LICENSE)
