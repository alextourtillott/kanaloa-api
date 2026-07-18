# Quick Start Guide: Welcome to Kanaloa!

Welcome! We're incredibly excited to partner with you. **Kanaloa** is a smart, adaptable oceanic restoration (API) application. Kanaloa is designed to help you analyze reef health, track water quality, clean debris, protect coral, and dynamically adjust restoration models as ecosystems evolve.

This guide will get you authenticated and making your very first API request to predict coral growth in under five minutes. Let’s dive in!

---

## Get Ready in 3 Easy Steps

### Step 1: Grab Your API Key

You need a secure token to interact with Kanaloa.

1. Log into your [Kanaloa Dashboard](https://www.duckduckgo.com/search?q=https://dashboard.kanaloa-ocean.io).
2. Navigate to **Developer Settings** > **API Keys**.
3. Click **Generate New Key**, name it (e.g., "Reef_Project_Alpha"), and copy. *Keep this safe, it's your private key to oceanic access!*

### Step 2: Set Up Your Environment

Kanaloa is built to adapt to your existing research workflow. We recommend using **Python** or **cURL** for quick testing. Ensure you have your favorite terminal or IDE open.

### Step 3: Run Your First Smart Prediction

Let's use Kanaloa's predictive modeling endpoint. In this example, we’ll send basic environmental data from a target site to get a localized coral growth recommendation.

Choose your preferred language tab below and run the script; remember to replace `YOUR_API_KEY_HERE` with your actual key:

#### Python

```python
import requests

url = "https://api.kanaloa-ocean.io/v1/restoration/predict"
headers = {
    "Authorization": "Bearer YOUR_API_KEY_HERE",
    "Content-Type": "application/json"
}
data = {
    "site_id": "reef-042",
    "water_temp_celsius": 27.8,
    "ph_level": 8.1,
    "target_species": "Acropora_cervicornis"
}

response = requests.post(url, json=data, headers=headers)
print(response.json())

```

#### cURL

```bash
curl -X POST https://api.kanaloa-ocean.io/v1/restoration/predict \
  -H "Authorization: Bearer YOUR_API_KEY_HERE" \
  -H "Content-Type: application/json" \
  -d '{
    "site_id": "reef-042",
    "water_temp_celsius": 27.8,
    "ph_level": 8.1,
    "target_species": "Acropora_cervicornis"
  }'

```

---

## Reading Your First Response

Kanaloa analyzes your inputs against evolving global oceanic datasets. Your successful request will return a JSON object that looks like this:

```json
{
  "site_id": "reef-042",
  "status": "success",
  "recommendation": {
    "growth_outlook": "Optimal",
    "optimal_depth_meters": 5.5,
    "confidence_score": 0.94,
    "adaptive_note": "Slight warming trend detected in adjacent grids. Prioritize shading-tolerant genotypes."
  }
}

```

**Success!** You just used smart data to inform your reef restoration strategy!

---

## Handy Troubleshooting Tips

**401 Unauthorized error?** Double-check that your API key is copied correctly and that `Bearer ` (with a space) precedes it in the header.
**422 Unprocessable Entity error?** Make sure your `water_temp_celsius` and `ph_level` values are numbers, not text strings.
**Need to see available species?** Send a quick `GET` request to `[https://api.kanaloa-ocean.io/v1/species](https://api.kanaloa-ocean.io/v1/species)` to see the full list of supported marine organisms.

## Deepen Your Knowledge

Because Kanaloa learns from ongoing ocean monitoring data, our models change to protect ecosystems. Check out the [Adaptation Log](https://www.google.com/search?q=https://docs.kanaloa-ocean.io/changelog) to see the latest parameters integrated into our smart models.

*Need a hand with your specific restoration site? Drop our support team a line at support@kanaloa-ocean.io and we'll respond faster than a message in a bottle!*