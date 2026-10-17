# Gemini Omni 1.1 Flash Ext API (`gemini-omni-1.1-flash-ext`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-ad-ceramic-mug-thumb.jpg" width="820" alt="Sample video frame generated through APIMart">
</p>

> **from $0.35 per second** on the APIMart route — un seul endpoint compatible OpenAI sur `https://api.apimart.ai/v1`, sans abonnement mensuel. *(relevé le 2026-10-12)*

**[Obtenir une clé API](https://go.apimart.ai/k-ca105f)** · **[Tarifs en direct](https://go.apimart.ai/k-c61f4d)** · **[Page du modèle](https://go.apimart.ai/k-2853bc)** · [⚡ Démarrage en 60 secondes](#quickstart)

**Pourquoi les équipes passent par APIMart pour Gemini Omni 1.1 Flash Ext (`gemini-omni-1.1-flash-ext`)**

- **Une clé, tout le catalogue.** La même base URL `https://api.apimart.ai/v1` et le même en-tête `Authorization` donnent accès à Gemini Omni 1.1 Flash Ext (`gemini-omni-1.1-flash-ext`) et à plus de 300 autres modèles d'image, de vidéo et de langage — changez le champ `model`, pas votre client.
- **$1 minimum, paiement à l'usage.** Ni abonnement ni forfait prépayé à dimensionner à l'avance : rechargez à partir de $1 et consommez-le en appels.
- **Le montant revient dans la réponse.** Chaque tâche terminée indique le montant facturé (`cost` / `credits_cost`) ; une tâche marquée `failed` est remboursée.
- **Asynchrone par conception.** Soumettez, récupérez le `task_id`, interrogez `GET /v1/tasks/{id}` — le traitement par lots et les retries relèvent d'une file d'attente ordinaire, pas d'une intégration sur mesure.

<!-- /conv-kit:v1 -->



Gemini Omni 1.1 Flash Ext API on APIMart provides streamlined text-to-video generation with the gemini-omni-1.1-flash-ext model. Send model, prompt, duration, resolution, size, and optional image_urls through APIMart's unified video generation endpoint, making gemini-omni-1.1-flash-ext easy to test, integrate, and scale.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Text-to-video / image-to-video | `gemini-omni-1.1-flash-ext` | per generated second, by resolution | see the [model page](https://go.apimart.ai/k-2853bc) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/videos/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Resolution | List price | Effective price |
| --- | --- | --- |
| default | 0.4375 | 0.35 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-c61f4d) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | the shot description; camera and subject motion belong here |
| `duration` | integer | route default | clip length in seconds — billing is per generated second |
| `resolution` | string | route default | 480P / 720P / 1080P or the route’s own tiers |
| `size` / `aspect_ratio` | string | `16:9` | output framing |
| `image_urls` | string[] | — | first frame, first + last frame, or style references, where supported |
| `seed` | integer | — | fix the seed to reproduce a take |

The table lists the fields accepted by the APIMart `videos/generations` endpoint. Check the [model page](https://go.apimart.ai/k-2853bc) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/videos/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"gemini-omni-1.1-flash-ext","prompt":"A bamboo forest at sunrise, slow push-in","duration":5,"resolution":"720P","size":"16:9"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/videos/generations", headers=HEADERS, timeout=60, json={
    "model": "gemini-omni-1.1-flash-ext", "prompt": "A bamboo forest at sunrise, slow push-in", "duration": 5,
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["videos"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**Quel est l'id du modèle Gemini Omni 1.1 Flash Ext ?**

`gemini-omni-1.1-flash-ext`. Voir le tableau des routes ci-dessus.

**Comment la facturation fonctionne-t-elle ?**

**from $0.35 per second** on the APIMart route. La tâche terminée indique le montant exact dans son champ `cost` ; une tâche échouée est remboursée.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-ca105f) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [gemini-omni-1-1-flash-ext-prompts-fr](https://github.com/h3ehdwc/gemini-omni-1-1-flash-ext-prompts-fr)
- **Same model, other languages** — 10 more language builds: English [gemini-omni-1-1-flash-ext-api](https://github.com/luu6sbymoi0o/gemini-omni-1-1-flash-ext-api) · 中文 [gemini-omni-1-1-flash-ext-api-cn](https://github.com/coeye-4kanfy/gemini-omni-1-1-flash-ext-api-cn) · Deutsch [gemini-omni-1-1-flash-ext-api-de](https://github.com/egbh9o4r/gemini-omni-1-1-flash-ext-api-de) · Español [gemini-omni-1-1-flash-ext-api-es](https://github.com/y80kwsn0mp80/gemini-omni-1-1-flash-ext-api-es) · Relay/gateway `gemini-omni-1-1-flash-ext-api-gateway` · Bahasa `gemini-omni-1-1-flash-ext-api-id` · 日本語 [gemini-omni-1-1-flash-ext-api-ja](https://github.com/bx2b1/gemini-omni-1-1-flash-ext-api-ja) · 한국어 `gemini-omni-1-1-flash-ext-api-ko` · Português [gemini-omni-1-1-flash-ext-api-pt](https://github.com/tqwzrjgbduy4/gemini-omni-1-1-flash-ext-api-pt) · Русский [gemini-omni-1-1-flash-ext-api-ru](https://github.com/saklt/gemini-omni-1-1-flash-ext-api-ru)
- **More models in this language** — [midjourney-api-fr](https://github.com/apimartt1q/midjourney-api-fr) · [gemini-2-5-flash-image-preview-api-fr](https://github.com/haftslut7ha/gemini-2-5-flash-image-preview-api-fr) · [nano-banana-api-fr](https://github.com/f84q06w110/nano-banana-api-fr) · [gemini-3-1-flash-image-preview-api-fr](https://github.com/ok71ckyc/gemini-3-1-flash-image-preview-api-fr) · [nano-banana-2-api-fr](https://github.com/ms0p12t/nano-banana-2-api-fr) · [gemini-3-pro-image-preview-api-fr](https://github.com/w-44vba/gemini-3-pro-image-preview-api-fr)
- **Topic hubs** — every build of this model, all languages: [gemini-omni-1-1-flash-ext](https://github.com/topics/gemini-omni-1-1-flash-ext) · [gemini-omni-1-1-flash-ext-api](https://github.com/topics/gemini-omni-1-1-flash-ext-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-2853bc> | `apimart.ai/fr/model/omni-flash-ext` |
| Live pricing | <https://go.apimart.ai/k-c61f4d> | `apimart.ai/fr/pricing` |
| Get an API key | <https://go.apimart.ai/k-ca105f> | `apimart.ai/fr/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Gemini Omni 1.1 Flash Ext is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
