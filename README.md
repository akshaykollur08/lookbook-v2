# Lookbook

Lookbook is a fashion recommender and visual search, discovery system.

Given a catalog of clothing items with images and a user's purchase history, it returns the items the user is likely to want, fast, and lets them explore the catalog with unspecific, natural-language asks.

In corporate strategy this is called next-purchase recommendation, or sequential recommendation, and it is a very important problem since it decides what each shopper sees first.
AI-driven and automated next-purchase recommendations are heavily expanding in 2026, shifting from simple "people who bought this also bought..." lists to proactive, real-time AI shopping agents and hyper-personalized post-purchase flows.

## The Data

I used the [H&M Personalized Fashion Recommendations](https://www.kaggle.com/competitions/h-and-m-personalized-fashion-recommendations/data) dataset from Kaggle: 105,542 articles with photos, 1.37 million customers, and 31.8 million real purchases from 2018 to 2020.
Everything is stored in PostgreSQL.

## How It Works

**Articles into vectors.**
Every article is embedded with FashionCLIP, once from its photo and once from its description.
Similar clothes end up close together, so a photo or a sentence can find matching items.
The vectors are indexed in pgvector for fast similarity search.

**Predicting the next purchase.**
For each shopper I gather candidates from five places: what's popular, what they bought before, what matches their taste, new colors of things they own, and things bought together with theirs.
A LightGBM model then ranks those candidates to predict what they'll buy next.

**Exploring in plain language.**
A stylist agent takes a vague ask like "an outfit for a beach wedding" and turns it into a search.

## The Result

Tested on 5,712 random customers against what they actually bought in the next two weeks:

| | recall@20 |
|---|---|
| Baseline (repurchase, then popularity) | 4.51% |
| LightGBM ranker | 7.37% |

That's a 63% lift over the baseline, and a bootstrap test confirms it isn't noise.

## Stack

**Core:** Python, FastAPI, React, PostgreSQL, pgvector, FashionCLIP, LightGBM (ML Model/Gradient Boosted Decision Tree Framework)

**Supporting tools:** MLflow for experiment tracking and model management; LangGraph for conversational discovery; Redis (optional) for caching; Prometheus for metrics collection and time-series storage; Grafana for dashboards and visualizations.

## More

See [Lookbook's Breakdown](docs/overview.md) for a short overview of each stage.
This repository is just a presentation of the work. 
