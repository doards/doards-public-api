# Doards Public API

The **Doards Public API** is a free, read-only HTTP API that exposes the shared
reference data we maintain for our ERP platform — so you don't have to collect,
clean and host it yourself.

It started as a set of PEPPOL codelists (still available as plain CSV / SQL in
this repository, see [Downloadable datasets](#downloadable-datasets)) and has
grown into a single endpoint for the everyday lookup data that business software
needs: countries, states, cities, industries, public holidays, vehicle brands,
tire sizes, chart-of-accounts frameworks and the PEPPOL e-invoicing codelists.

- **Base URL:** `https://public.doards.com/api`
- **Format:** JSON over HTTPS, `GET` only
- **Interactive docs:** `https://public.doards.com/docs` (OpenAPI / Swagger UI)
- **Get an API key:** **<https://doards.com/api>**

> This repository is informational. It documents what the API offers and ships
> the static PEPPOL codelists. The API service itself is hosted by Doards — you
> only need an API key to start using it.

---

## What data can I retrieve?

| Resource           | Endpoint                  | What it gives you |
| ------------------ | ------------------------- | ----------------- |
| Countries          | `/countries`              | ISO2/ISO3 codes, native name, phone code, capital, currency + symbol, region/subregion, emoji flag, lat/long. Optional translated names. |
| States / regions   | `/states`                 | States, provinces and regions linked to a country (ISO codes, type, lat/long). |
| Cities             | `/cities`                 | Cities linked to a state and country, with coordinates. |
| Industries         | `/industries`             | Industry / sector list with group names. |
| Public holidays    | `/public-holidays`        | Public holidays per country (and optionally per state). |
| Car brands         | `/car-brands`             | Vehicle manufacturer list with descriptions and logos. |
| Tire sizes         | `/tire-sizes`             | Standard tire-size designations. |
| Chart of accounts  | `/accounts`               | German SKR03 / SKR04 accounting frameworks (account number, name, class, group, category). |
| PEPPOL EAS         | `/peppol-eas`             | Electronic Address Scheme codes (EAS / ISO 6523) for e-invoicing. |
| PEPPOL UNCL4461    | `/peppol-uncl4461`        | UNCL 4461 payment-means codes for e-invoicing. |

Most list endpoints accept a `q` free-text search and a `limit` parameter. See
[Endpoint reference](#endpoint-reference) for the full list of parameters.

---

## Getting started

### 1. Get an API key

Request a key at **<https://doards.com/api>**. It's free.

### 2. Make a request

Send your key in the `Authorization` header as a Bearer token:

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://public.doards.com/api/countries?q=germ&limit=5"
```

Example response:

```json
[
  {
    "id": 82,
    "name": "Germany",
    "native": "Deutschland",
    "iso2": "DE",
    "iso3": "DEU",
    "phonecode": "49",
    "capital": "Berlin",
    "currency": "EUR",
    "currency_symbol": "€",
    "region": "Europe",
    "subregion": "Western Europe",
    "emoji": "🇩🇪",
    "latitude": 51.0,
    "longitude": 9.0
  }
]
```

### 3. Check service health (no key required)

```bash
curl https://public.doards.com/health
# {"status":"ok"}
```

---

## Authentication

Every endpoint except `/health` requires an API key:

```
Authorization: Bearer YOUR_API_KEY
```

- `401 Unauthorized` — the header is missing or the key is invalid/inactive.
- Keys are issued and managed at <https://doards.com/api>.

---

## Endpoint reference

All paths are relative to the base URL `https://public.doards.com/api`.

| Method | Path                               | Query / path params                                    |
| ------ | ---------------------------------- | ------------------------------------------------------ |
| GET    | `/health`                          | *(no auth)*                                            |
| GET    | `/countries`                       | `q`, `lang` (translation language), `limit` (1–500)    |
| GET    | `/countries/{iso}`                 | path: ISO2 or ISO3 code                                |
| GET    | `/states`                          | `q`, `country` (id / ISO2 / ISO3 / name), `limit`      |
| GET    | `/cities`                          | `q`, `state` (id / name), `limit`                      |
| GET    | `/industries`                      | `q`, `limit`                                           |
| GET    | `/public-holidays`                 | `iso` (ISO2, **required**), `state` (optional)         |
| GET    | `/car-brands`                      | `q`, `limit`                                           |
| GET    | `/tire-sizes`                      | `q`, `limit`                                           |
| GET    | `/accounts`                        | `q`, `skr` (`03` or `04`), `limit` (1–2000)            |
| GET    | `/accounts/{skr}/{account_number}` | path: SKR framework + account number                   |
| GET    | `/peppol-eas`                      | `q`, `limit`                                           |
| GET    | `/peppol-uncl4461`                 | `q`, `limit`                                           |
| GET    | `/peppol-uncl4461/by-name/{name}`  | path: code name                                        |

`q` is a case-insensitive free-text search. `limit` defaults to 50–100
depending on the resource and is capped per endpoint as noted above.

The full, always-current schema (request params and response shapes) is
published as OpenAPI at **<https://public.doards.com/docs>**.

---

## Downloadable datasets

If you only need the PEPPOL codelists and don't want to call the API, this
repository ships them as static files you can import directly:

```
csv/
├── eas.csv          European Address Scheme (EAS) codes
└── uncl4461.csv     UNCL 4461 payment-means codes
sql/
├── eas.sql          Same data as ready-to-import MySQL tables
└── uncl4461.sql
```

- **EAS** — Electronic Address Scheme (ISO 6523) identifiers used to address
  participants on the PEPPOL network.
- **UNCL4461** — UN/CEFACT code list 4461 for payment means in e-invoices.

These mirror the `/peppol-eas` and `/peppol-uncl4461` API endpoints. The API is
the recommended source if you want it kept up to date automatically.

---

## About Doards

Doards is a customizable ERP platform. We maintain a large set of reference data
to power the product, and we publish the read-only parts here so other
developers can benefit from it too.

Found an error in the data, or have a codelist / dataset you'd like to see
added? Open an issue or pull request — contributions are very welcome.

For API keys and account-related questions, visit **<https://doards.com/api>**.
