# Lookbook's Breakdown

A short overview of how Lookbook is put together.

## Data

H&M's catalog, customers, and purchase history from Kaggle, loaded into PostgreSQL.
Every model is built from what happened before a cutoff date and tested on what customers bought after it.

## Search

Each article is turned into vectors from its photo and its description, so similar clothes can be found by a photo or by words.

## Recommendations

For each shopper, likely items are gathered from several sources and a LightGBM model ranks them to predict the next purchase.
Every model is compared against a simple baseline before it goes live.

## Discovery

A stylist agent turns a plain-language ask into a search and explains its picks.

## Serving

An API serves the recommendations, search, and the demo, with monitoring on latency.
