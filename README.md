# x-translator — Traduction & Sous-titrage Vidéo

> **L'IA qui traduit vos vidéos en 20+ langues.**  
> Collez une URL YouTube, X, TikTok ou Instagram → obtenez une vidéo sous-titrée, traduite et prête à partager.  
> *Open source · Gratuit : 1h30 de transcription par jour*

<div align="center">

[![Version](https://img.shields.io/badge/version-0.1.237-blue?style=flat-square&logo=semver)](CHANGELOG.md)
[![i18n](https://img.shields.io/badge/i18n-100%25-brightgreen?style=flat-square&logo=checkmarx)](status.json)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square&logo=open-source-initiative)](LICENSE)
[![Next.js](https://img.shields.io/badge/Next.js-14.2-black?style=flat-square&logo=next.js)](https://nextjs.org)
[![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=flat-square&logo=python)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115-009688?style=flat-square&logo=fastapi)](https://fastapi.tiangolo.com)
[![Tailwind](https://img.shields.io/badge/Tailwind_v4-06B6D4?style=flat-square&logo=tailwindcss)](https://tailwindcss.com)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-4169E1?style=flat-square&logo=postgresql)](https://postgresql.org)
[![Redis](https://img.shields.io/badge/Redis-7-DC382D?style=flat-square&logo=redis)](https://redis.io)
[![Stripe](https://img.shields.io/badge/Stripe-pay-008CDD?style=flat-square&logo=stripe)](https://stripe.com)
[![Platforms](https://img.shields.io/badge/platforms-10-FA4B2B?style=flat-square&logo=github)](https://spottedyou.org)
[![Railway](https://img.shields.io/badge/Railway-deployed-0B0D0E?style=flat-square&logo=railway)](https://railway.app)

</div>

> _Dernière mise à jour : 18/05/2026 17:47 UTC_

---

## 📊 En un coup d'oeil

<div align="center">

| 🎬 Vidéos | 🌍 Langues | 📺 Plateformes | ⚡ Pipeline | 💳 Tarifs |
|:---------:|:----------:|:--------------:|:-----------:|:---------:|
| **~2 min** traitées | **20** supportées | **10** compatibles | **6** etapes | **Gratuit** |
| transcription | FR/EN/ES/DE/... | YouTube a Reddit | ~2 min/video | 1h30/j |

</div>

> **Toutes les fonctionnalites ne sont pas encore deployees.** Consultez [status.json](status.json) pour l'etat exact.

---

## ✨ Fonctionnalites

| Statut | Fonctionnalite | Description |
|--------|---------------|-------------|
| ✅ | Traduction video | Sous-titrez et traduisez vos videos en 20+ langues en un clic |
| ✅ | Telechargement video | Telechargez depuis YouTube, TikTok, Instagram et 50+ plateformes |
| 🚧 | Studio d'edition | Montez, editez et personnalisez avec overlays, watermarks, sous-titres |
| 🔧 | TV en direct | Regardez et enregistrez 50 chaines TV en direct dans le monde |
| 🚧 | Doublage vocal | Ajoutez une voix synthetique dans la langue de votre choix |
| 📋 | API publique | Integrez la traduction video dans vos propres outils |

---

## 🌍 Couverture linguistique

**100% traduit dans les 20 langues** — 2 171 textes synchronises.

| Drapeau | Langue | Code | Couverture |
|---------|--------|------|------------|
| 🇫🇷 | Francais | `fr` | 100% |
| 🇬🇧 | English | `en` | 100% |
| 🇪🇸 | Espanol | `es` | 100% |
| 🇩🇪 | Deutsch | `de` | 100% |
| 🇮🇹 | Italiano | `it` | 100% |
| 🇵🇹 | Portugues | `pt` | 100% |
| 🇳🇱 | Nederlands | `nl` | 100% |
| 🇵🇱 | Polski | `pl` | 100% |
| 🇷🇺 | Russkiy | `ru` | 100% |
| 🇺🇦 | Ukrainska | `uk` | 100% |
| 🇸🇦 | Alarabia | `ar` | 100% |
| 🇮🇷 | Farsi | `fa` | 100% |
| 🇮🇱 | Ivrit | `he` | 100% |
| 🇮🇳 | Hindi | `hi` | 100% |
| 🇨🇳 | Zhongwen | `zh` | 100% |
| 🇯🇵 | Nihongo | `ja` | 100% |
| 🇰🇷 | Hangugeo | `ko` | 100% |
| 🇻🇳 | Tieng Viet | `vi` | 100% |
| 🇮🇩 | Bahasa Indonesia | `id` | 100% |
| 🇹🇷 | Turkce | `tr` | 100% |

---

## 🏗️ Architecture

```
  UTILISATEUR  ──▶  FRONTEND (Next.js 14)  ──▶  BACKEND API (FastAPI)
                        │                           │
                        │                     ┌─────┴──────┐
                        │              ┌──────┴─────┐  ┌───┴────┐
                        │              │  WORKER     │  │  Redis  │
                        │              │  Celery     │  │  Queue  │
                        │              │  yt-dlp     │  │  Cache  │
                        │              │  Whisper    │  │         │
                        │              │  DeepSeek   │  └────────┘
                        │              │  FFmpeg     │
                        │              └────────────┘
                        │                     │
                        │              ┌──────┴──────┐
                        │              │  PostgreSQL  │
                        └──────────────│  Supabase    │
                                       │  Storage     │
                                       └─────────────┘
```

### Pipeline video (6 etapes, ~2 min)

```
1. URL ______  →  yt-dlp  →  Video + audio
2. Audio ____  →  Groq Whisper  →  Transcription SRT
3. SRT ______  →  Anti-hallucination  →  SRT nettoye
4. SRT ______  →  DeepSeek V3  →  20 traductions
5. Analyse ___  →  IA : scenes, intervenants
6. Export ____  →  WebVTT + video source  →  ✅ Disponible
```

> **Soft subs** : les sous-titres sont overlay cote lecteur. Pas de burn systematique.

---

## 🛠️ Stack

| Couche | Technologie |
|--------|-------------|
| **Backend** | FastAPI + asyncpg |
| **Worker** | Celery + Redis (5 queues) |
| **Transcription** | Groq Whisper large-v3-turbo |
| **Traduction** | DeepSeek V3 (OpenRouter) |
| **Stockage** | Supabase Storage |
| **Auth** | Supabase Auth (JWT) |
| **Paiement** | Stripe (Free / Pro / Master) |
| **Frontend** | Next.js 14 + Tailwind v4 + Framer Motion |
| **Etat** | Zustand |
| **DNS** | Cloudflare (spottedyou.org) |
| **Deploiement** | Railway (3 services) |
| **i18n** | next-intl + 20 fichiers JSON |

---

## 🔗 Liens

- [📄 Notes de version](CHANGELOG.md)
- [📊 Etat du projet](status.json)
- [🐛 Signaler un probleme](https://github.com/Nansoouu/x-translator-mvp/issues)
- [🌍 Site officiel](https://spottedyou.org)
- [💬 Discussion](https://github.com/spottedyou/translator/discussions)

---

<p align="center">
  <sub>Construit avec ☕ par <a href="https://github.com/Nansoouu">Nans</a> · 
  <a href="https://spottedyou.org">spottedyou.org</a></sub>
</p>
