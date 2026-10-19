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
| Postal codes       | `/postal-codes`           | ~1.8 million postal codes with place names, subdivisions and estimated coordinates (GeoNames). |
| City aliases       | `/city-aliases`           | Alternate, translated, abbreviated and historic city names (`München` → `Munich`), linked to `/cities` where possible. |
| Municipalities     | `/municipalities`         | Third- to fifth-order administrative divisions (ADM3–ADM5) with population and coordinates. |
| Airports           | `/airports`               | ~86,000 airports, heliports and seaplane bases with IATA/ICAO codes (OurAirports). |
| Time zones         | `/timezones`, `/country-timezones` | IANA time zone identifiers (tz database release 2026e) and the zones each country uses. |
| Currency precision | `/currency-precision`     | Decimal digits and rounding per currency (Unicode CLDR). |
| Address rules      | `/address-field-rules`    | Address layout, required fields and naming conventions per country (libaddressinput). |
| Postal code formats | `/postal-code-patterns`  | Postal code regex and examples per country (libaddressinput). |
| NACE               | `/nace-codes`             | EU activity classification NACE Rev. 2 and 2.1 with hierarchy, English and German titles. |
| CPA                | `/cpa-codes`              | EU product classification CPA 2008, 2.1 and 2.2 with hierarchy. |
| CPV                | `/cpv-codes`              | EU Common Procurement Vocabulary (CPV 2008) with the official hierarchy, English and German. |
| Administrative districts | `/administrative-districts` | Second-order divisions (districts, counties, provinces) from GeoNames. |
| Named places       | `/named-places`           | Populated places plus stations, airports, ports, landmarks and peaks (curated GeoNames features), `/geographic-feature-types` explains the codes. |
| Postal code localities | `/postal-code-localities` | Localities of a country with all the postal codes that serve them. |
| UN/LOCODE          | `/un-locodes`             | Places with a UN/LOCODE (from GeoNames). |
| Boundaries         | `/country-boundaries`, `/state-boundaries`, `/urban-areas` | Generalized GeoJSON polygons (Natural Earth, public domain). |
| EU municipalities  | `/lau-units`              | Official EU local administrative units with codes (e.g. German AGS), NUTS 3 region, population, area, degree of urbanisation. |
| ISO 3166-2         | `/iso3166-subdivisions`   | Subdivision codes with hierarchy and English names. |
| Country facts      | `/country-capitals`, `/country-tlds`, `/country-driving-side` | Capitals, top-level domains and traffic side per country (Wikidata, CC0). |
| Runways, frequencies, navaids | `/airport-runways`, `/airport-frequencies`, `/navigation-aids` | OurAirports data - not for flight operations. |
| Time zone transitions | `/timezone-transitions`, `/timezone-rules/{tz_id}` | DST and offset changes 1970-2037 plus the POSIX rule afterwards. |
| Localization       | `/locales`, `/languages`, `/language-translations`, `/region-translations`, `/subdivision-translations`, `/currency-translations`, `/currency-symbols`, `/currency-territories`, `/number-formats`, `/date-time-formats`, `/timezone-translations`, `/measurement-unit-names`, `/week-rules`, `/measurement-systems`, `/un-m49-regions`, `/writing-scripts`, `/locale-aliases`, `/territory-languages` | Unicode CLDR data with locale inheritance (`de-AT` → `de` → `root`) resolved by the API. |
| Exchange rates     | `/exchange-rates`         | ECB euro reference rates since 1999, history and conversion. |
| Phone numbers      | `/phone-validation-patterns`, `/phone-number-formats`, `/phone-prefix-regions` | Dialling metadata, number patterns, formatting rules and area descriptions (libphonenumber). |
| PRODCOM, NUTS, CN, WZ | `/prodcom-codes`, `/nuts-codes`, `/cn-codes`, `/wz-codes`, `/wz-correspondences`, `/nace-correspondences` | More EU / German classifications with hierarchy. |
| Transport places   | `/railway-stations`, `/metro-tram-stations`, `/ferry-terminals`, `/seaports` | Stations, ferry terminals and ports from Wikidata (CC0) with `near=lat,lon` search; loaded country scope in `/data-sources/wikidata`. |
| ISO currency codes | `/iso4217-currencies`     | ISO 4217 alphabetic and numeric codes, minor units and the countries that use each currency (SIX). Rights not verified. |
| ISO language codes | `/iso639-languages`, `/iso639-macrolanguages` | ISO 639-3 languages with ISO 639-1 / 639-2 equivalents and macrolanguage mappings (SIL). Rights not verified. |
| Legal forms        | `/legal-forms`            | ISO 20275 entity legal forms (GmbH, AG, Ltd ...) per country and language (GLEIF). Rights not verified. |
| VAT rates          | `/vat-rates`, `/vat-rate-goods` | Standard and reduced VAT rates of the EU member states with categories and CN / CPA codes (European Commission TEDB). Not a reference for legal purposes. |
| E-invoicing lists  | `/einvoicing-code-lists`, `/peppol-participants` | OpenPeppol code lists (document types, participant schemes, processes ...) and a lookup of registered Peppol participant identifiers. Rights not verified. |
| Units and packages | `/units-of-measure`, `/package-types`, `/incoterms` | UN/ECE Recommendation 20 / 21 codes (**UN terms restrict commercial use**) and the Incoterms 2020 codes. |
| Banking            | `/iban-formats`, `/sepa-countries`, `/bank-branches` | IBAN structures and SEPA flag (SWIFT registry via php-iban - **copying restricted**) and the German bank code directory (Bundesbank). |
| Vehicles           | `/vehicle-makes`, `/vehicle-manufacturers`, `/vehicle-models`, `/vehicle-wmi-codes`, `/vehicle-recalls`, `/vehicle-model-years` | U.S. NHTSA vPIC catalog and recalls. U.S. market only. Rights not verified. |
| European car models | `/vehicle-co2-models`, `/vehicle-fuel-types` | New cars registered in the EU 2010-2022 per make, model, year and fuel with registrations and average mass, power and type-approval CO2 (EEA monitoring, aggregated). |
| Country indicators | `/worldbank-countries`, `/country-indicator-definitions`, `/country-indicators` | World Bank classification and 20 headline indicators per country and year (CC BY 4.0). |
| Postal code to NUTS | `/postal-code-nuts`      | Which NUTS 3 region(s) a postal code lies in (Eurostat GISCO TERCET). |
| Data sources       | `/data-sources`           | Source, licence, attribution and loaded snapshot of every dataset above. |

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
| GET    | `/postal-codes`                    | `country` (ISO2), `postal_code` (prefix), `q` (place prefix), `admin1`, `limit` (1–1000), `offset` |
| GET    | `/postal-codes/{country}/{code}`   | path: ISO2 country + postal code                       |
| GET    | `/city-aliases`                    | `q` (prefix, ≥ 2 chars), `country`, `language`, `historic`, `limit` (1–500), `offset` |
| GET    | `/city-aliases/{geoname_id}`       | path: GeoNames id — all aliases of one place           |
| GET    | `/municipalities`                  | `q` (prefix), `country`, `admin1`, `admin2`, `admin_level` (3–5), `limit` (1–1000), `offset` |
| GET    | `/municipalities/{geoname_id}`     | path: GeoNames id                                      |
| GET    | `/airports`                        | `q` (name / city / exact IATA or ICAO), `country`, `type`, `scheduled_service`, `include_closed`, `limit` (1–500), `offset` |
| GET    | `/airports/{code}`                 | path: ICAO code, IATA code or OurAirports ident        |
| GET    | `/timezones`                       | `q`, `kind` (`zone`/`link`), `canonical`, `country`, `limit`, `offset` |
| GET    | `/timezones/{tz_id}`               | path: IANA identifier, e.g. `Europe/Berlin`            |
| GET    | `/country-timezones`               | `country`, `timezone`, `limit`, `offset`               |
| GET    | `/country-timezones/{country}`     | path: ISO2 country                                     |
| GET    | `/currency-precision`              | `q` (code prefix), `current`, `limit`, `offset`        |
| GET    | `/currency-precision/{code}`       | path: ISO 4217 code                                    |
| GET    | `/address-field-rules`             | `country`, `limit`, `offset`                           |
| GET    | `/address-field-rules/{country}`   | path: ISO2 country                                     |
| GET    | `/postal-code-patterns`            | `country`, `has_postal_code`, `limit`, `offset`        |
| GET    | `/postal-code-patterns/{country}`  | path: ISO2 country                                     |
| GET    | `/nace-codes`                      | `revision` (`2`, `2.1`, `all`; default latest), `q`, `level` (1–4), `parent`, `section`, `limit`, `offset` |
| GET    | `/nace-codes/{revision}/{code}`    | path: revision + code, e.g. `2.1/62.10`                |
| GET    | `/cpa-codes`                       | `version` (`2008`, `2.1`, `2.2`, `all`; default latest), `q`, `level` (1–6), `parent`, `section`, `limit`, `offset` |
| GET    | `/cpa-codes/{version}/{code}`      | path: version + code, e.g. `2.2/62.10`                 |
| GET    | `/cpv-codes`                       | `q`, `level`, `parent`, `limit`, `offset`              |
| GET    | `/cpv-codes/{code}`                | path: CPV code with or without check digit             |
| GET    | `/administrative-districts`, `/named-places` | `q`, `country`, `admin1`, `feature_class`/`feature_code`, `limit`, `offset`; detail by GeoNames id |
| GET    | `/postal-code-localities`          | `country` (**required**), `q`, `limit`, `offset`       |
| GET    | `/un-locodes`                      | `code`, `q`, `country`, `limit`, `offset`              |
| GET    | `/country-boundaries`, `/state-boundaries` | `q`, `country`/`continent`, `bbox=minLon,minLat,maxLon,maxLat`, `include_geometry` (max 25 results); detail by ISO code |
| GET    | `/urban-areas`                     | `bbox` (**required**), `min_area_sqkm`, `include_geometry` |
| GET    | `/lau-units`                       | `country`, `q`, `nuts3`, `lau_code`, `degurba` (1–3), `min_population` |
| GET    | `/iso3166-subdivisions`            | `country`, `q`, `parent`, `top_level`                  |
| GET    | `/country-capitals`, `/country-tlds`, `/country-driving-side` | `country` (+ `q`, `side`)             |
| GET    | `/airport-runways`, `/airport-frequencies`, `/navigation-aids` | `airport` (ICAO / IATA / id), `type`, `country`, `surface`, `min_length_ft`; `/airports/{code}/runways` and `/frequencies` |
| GET    | `/timezone-transitions`            | `tz_id` (**required**), `from`, `to` (window ≤ 3,660 days); `/timezone-transitions/offset?tz_id=&at=` |
| GET    | `/exchange-rates`                  | `date`, `currency` (comma list); `/exchange-rates/history?currency=&start=&end=` (≤ 366 days); `/exchange-rates/convert?from=&to=&amount=&date=` |
| GET    | `/phone-validation-patterns`, `/phone-number-formats` | `region`, `country_calling_code`, `number_type`; detail by region |
| GET    | `/phone-prefix-regions`            | `prefix`, `country_calling_code`, `q`, `language` (`en`/`de`); `/phone-prefix-regions/lookup?number=` |
| GET    | `/prodcom-codes`, `/nuts-codes`, `/cn-codes`, `/wz-codes` | `q`, `level`, `parent`, and the revision / year / version / edition (default: latest, `0`/`all` = every one) |
| GET    | `/nace-correspondences`, `/wz-correspondences` | `from_revision`, `code`, `to_revision` / `wz2008`, `wz2025` |
| GET    | `/locales`, `/languages`, `/writing-scripts`, `/locale-aliases`, `/territory-languages` | `q`, `locale`, `language`, `variants`, `rtl`, `kind`, `territory`, `official` |
| GET    | `/language-translations`, `/region-translations`, `/currency-translations`, `/subdivision-translations` | `locale` (or `code`), `code`, `q`, `alt`, `inherit` (default `true`: resolve `de-AT` → `de` → `root`) |
| GET    | `/currency-symbols`, `/number-formats`, `/date-time-formats`, `/timezone-translations`, `/measurement-unit-names` | `locale` (**required** except `/number-formats`), `inherit`, and `currency` / `kind`,`name` / `tz_id`,`q` / `unit`,`width` |
| GET    | `/currency-territories`, `/week-rules`, `/measurement-systems`, `/un-m49-regions` | `territory`, `currency`, `current`, `system`, `kind`, `locale` |
| GET    | `/railway-stations`, `/metro-tram-stations`, `/ferry-terminals`, `/seaports` | `q`, `country`, `code` (station code / UIC / IBNR / UN/LOCODE / QID), `near=lat,lon`, `radius_km`; detail by Wikidata QID |
| GET    | `/iso4217-currencies`, `/iso4217-currencies/{code}` | `q`, `entity`, `numeric`, `limit`, `offset`; detail by alphabetic or numeric code |
| GET    | `/iso639-languages`, `/iso639-languages/{code}`, `/iso639-macrolanguages` | `q`, `scope`, `language_type`; `macrolanguage`, `individual`, `status`; detail by ISO 639-1 / 639-2 / 639-3 code |
| GET    | `/legal-forms`, `/legal-forms/{elf_code}` | `q`, `country`, `status` (`ACTV`/`INAC`), `elf_code`, `limit`, `offset` |
| GET    | `/vat-rates`, `/vat-rates/{country}`, `/vat-rate-goods` | `country`, `type`, `kind`, `category`, `q`; goods: `code_type` (`CN`/`CPA`), `code` prefix |
| GET    | `/incoterms`, `/incoterms/{code}`  | `q`, `transport_mode`                                  |
| GET    | `/einvoicing-code-lists`, `/einvoicing-code-lists/{list_name}` | list names; entries: `q`, `state`, `country`, `limit`, `offset` |
| GET    | `/peppol-participants`, `/peppol-participants/{scheme}/{identifier}` | `scheme` (**required**), `identifier` (exact) **or** `prefix` (≥ 4 characters); `limit` ≤ 100 |
| GET    | `/units-of-measure`, `/package-types` (+ `/{code}`) | `q`, `include_withdrawn`, `limit`, `offset`        |
| GET    | `/iban-formats`, `/iban-formats/{country}`, `/sepa-countries` | `q`, `sepa`; detail by ISO2 country                |
| GET    | `/bank-branches`, `/bank-branches/{branch_id}` | `q`, `bank_code`, `bic`, `city`, `postal_code`, `main_only`, `include_deleted` |
| GET    | `/vehicle-makes`, `/vehicle-manufacturers`, `/vehicle-models`, `/vehicle-wmi-codes` | `q`, `country`, `vehicle_type`; models: `make` (id or name); WMI: `wmi` prefix, `manufacturer_id`; detail by id / WMI |
| GET    | `/vehicle-recalls`, `/vehicle-recalls/{campaign_no}`, `/vehicle-model-years` | `make`, `model`, `year`, `q`, `component`; model years: `make` (**required**), `model` |
| GET    | `/vehicle-co2-models`, `/vehicle-co2-models/makes`, `/vehicle-fuel-types` | `make`, `model` (prefix), `year`, `category`, `fuel_type`, `status`; makes: `q`, `year` |
| GET    | `/worldbank-countries`, `/country-indicator-definitions`, `/country-indicators` | `q`, `region`, `income_level`; indicators: `country`, `indicator`, `year`, `year_from`, `year_to`, `latest`, `include_aggregates` |
| GET    | `/postal-code-nuts`                | `country`, `postal_code`, `nuts3` (prefix)             |
| GET    | `/calendar-names`                  | `locale` (**required**), `kind` (`month`, `day`, `quarter`, `day_period`, `era`), `context`, `width`, `inherit` |
| GET    | `/plural-rules`                    | `language`, `type` (`cardinal`, `ordinal`)             |
| GET    | `/unit-conversions`, `/unit-conversions/convert`, `/unit-preferences` | `unit`, `quantity`, `system`, `q`; convert: `value`, `from`, `to`; preferences: `category`, `usage`, `region` |
| GET    | `/country-neighbours`              | `country`                                              |
| GET    | `/country-groups`, `/country-groups/{group}`, `/country-group-memberships` | `on` (date, default today); group: `EU`, `EUROZONE`, `SCHENGEN`, `EEA`, `EFTA`, `history`; memberships: `country` (**required**) |
| GET    | `/country-codes`                   | `q`, `iso2`, `destatis`, `vehicle_sign`                |
| GET    | `/national-municipalities`, `/national-municipalities/{country}/{code}` | `country` (`DE`, `AT`, `CH`), `q` (prefix), `postal_code`, `state`, `district`, `tax_office` |
| GET    | `/tax-offices`, `/tax-offices/{office_number}` | `q`, `postal_code`, `city`                 |
| GET    | `/registration-authorities`, `/registration-authorities/{ra_code}` | `q`, `country`, `ra_code`       |
| GET    | `/courts`, `/courts/{xjustiz_id}`  | `q`, `state`, `type`, `dissolved`                      |
| GET    | `/oenace-codes`, `/oenace-codes/{edition}/{code}` | `edition` (`2025`, `2008`), `level`, `parent`, `q`, `nace` |
| GET    | `/vehicle-code-lists`, `/vehicle-code-lists/{list_name}` | lists: `vehicle_class_body`, `fuel`, `emission_class`; `code`, `sub_code`, `q`, `current` |
| GET    | `/kba-vehicle-types`, `/kba-vehicle-types/{hsn}/{tsn}`, `/kba-manufacturers` | one of `hsn`, `make`, `q`, `vehicle_class`, `fuel` (**required**), `booklet`; detail by HSN + TSN |
| GET    | `/food-allergens`, `/food-allergens/{number}` | `language` (24 EU languages, default `en`)   |
| GET    | `/food-additives`, `/food-additives/{e_number}` | `language`, `group` (`colour`, `sweetener`, `other`), `q`; detail by E number (`E330`) |
| GET    | `/vat-number-formats`              | `country` (VAT prefix or ISO2: `DE`, `EL` / `GR`, `XI`)  |
| GET    | `/special-tax-territories`         | `language`, `country`, `customs` (inside / outside the EU customs territory) |
| GET    | `/asset-useful-lives`, `/asset-useful-lives/{position}` | `q`, `kind` (`position`, `index`), `parent`, `years` |
| GET    | `/license-plate-regions`, `/license-plate-regions/{code}` | `q`, `status` (`current`, `expiring`); detail by prefix (`FL`, `MÜ`) |
| GET    | `/driving-licence-codes`, `/driving-licence-codes/{code}` | `language`, `parent`, `q`; detail by code (`78`, `01.01`) |
| GET    | `/nature-of-transaction-codes`, `/nature-of-transaction-codes/{code}` | `language`, `group`; detail by code (`11`, `21`) |
| GET    | `/media-types`, `/media-types/{top_level}/{subtype}`, `/file-extensions` | `q`, `top_level`, `status`; extensions: `extension`, `media_type`, `q`; detail by type (`application/json`) |
| GET    | `/language-subtags`, `/uri-schemes`, `/uri-schemes/{scheme}`, `/character-sets`, `/character-sets/{name}` | `type`, `subtag`, `q`, `deprecated`; schemes: `status`; sets: `q`; detail by scheme / charset name or alias |
| GET    | `/http-status-codes`, `/http-status-codes/{code}`, `/smtp-status-codes`, `/smtp-status-codes/{code}`, `/service-ports` | `assigned`; SMTP: `level`, detail `5.1.1`; ports: `port`, `name`, `protocol`, `q`, `include_unassigned` |
| GET    | `/unicode-blocks`, `/unicode-blocks/{code_point}`, `/short-number-patterns`, `/paper-sizes` | blocks: `q`, detail by `U+00E4` or a character; short numbers: `region`, `category`; paper: `series`, `code`, `width_mm` + `height_mm` (+ `tolerance_mm`) finds a page size |
| GET    | `/list-patterns`, `/relative-time-fields`, `/date-interval-formats`, `/compact-number-formats`, `/person-name-patterns`, `/person-name-settings`, `/exemplar-characters`, `/quotation-marks`, `/emoji-annotations` | `locale` (**required**), `inherit` (resolve through the locale's parents), per-resource filters and `limit` / `offset` |
| GET    | `/windows-timezone-map`, `/likely-subtags`, `/calendar-eras`, `/numbering-systems`, `/hour-cycle-preferences`, `/emoji-groups` | `windows_zone`, `iana_zone`, `territory`; `subtag`, `language`; `calendar`; `code`, `type`; `region`, `preferred`; `group`, `subgroup`, `q` |
| GET    | `/clp-statements/{kind}`, `/clp-pictograms` | kind `H` or `P`; `language` (24 EU languages), `code`, `series`, `combined`, `q`; pictograms: `code` (GHS01 ...) |
| GET    | `/eu-waste-codes`, `/eu-waste-codes/{code}`, `/waste-treatment-operations`, `/waste-hazard-properties` | `language`, `level`, `parent`, `hazardous`, `code`, `q`; operations: `type`; properties: `number` |
| GET    | `/weee-categories`, `/textile-fibre-names`, `/reference-intakes`, `/nutrition-claims`, `/euro-conversion-rates` | `language`, `annex`; `number`, `q`; `part`; `currency`, `country` |
| GET    | `/financial-statement-lines`, `/operating-cost-types`, `/invoice-required-particulars`, `/vat-special-provisions` | `country` (DE, AT), `law`, `statement`, `variant`, `parent`, `position`; `number`; `invoice_type`; `kind`, `q` |
| GET    | `/record-retention-periods`, `/statutory-notice-periods`, `/labour-law-minimums`, `/de-payroll-tax-keys`, `/energy-performance-classes` | `country`, `law`, `date`; `relationship`, `party`; `topic`; `key_group`, `key_code`; `class_code` |
| GET    | `/einvoice-profiles`, `/meal-plan-codes` | `format`, `id_kind`, `identifier` (exact), `q`; meal plans: `code` (or an alias like `MAP`), `q` |
| GET    | `/tyre-label-classes`, `/ev-connector-types`, `/euro-emission-limits` | `parameter`, `tyre_class`, `class_code`, `value` (class of a measured value); `vehicle_category`, `current_type`, `connector`, `power_class`; `standard`, `vehicle_group`, `engine_type`, `pollutant`, `test_cycle` |
| GET    | `/vehicle-weights-dimensions`, `/driving-time-rules`, `/vin-character-tables` | `kind`, `point`, `q`; `topic`, `limit_kind`, `rule_key`; `kind`, `key` |
| GET    | `/gs1-application-identifiers`, `/barcode-symbologies` | `ai`, `q`, `fixed_length`; `kind`, `code`, `q` |
| GET    | `/classifications`, `/classifications/{scheme}` | schemes `isced-f`, `cofog`, `ecoicop2`, `ecoicop`, `cpv-suppl`, `naics2022`, `uksic2007`, `onet-soc`; `q`, `level`, `parent`, `code` |
| GET    | `/proficiency-levels/{scale}`, `/eurovoc`, `/eurovoc/{kind}/{concept_id}`, `/nace-explanatory-notes`, `/nace-isic-correspondences` | scale `eqf` / `cefr`; `language`, `kind`, `q`, `domain`, `microthesaurus`, `broader`; notes: `revision`, `code`, `note_type`; `nace_code`, `isic_code` |
| GET    | `/data-sources`, `/data-sources/{dataset}` | source, licence, attribution, snapshot       |

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

## Data sources and attribution

The newer datasets are built from open sources and keep their licences and credit lines. Every response set is described by `GET /api/data-sources`
(source, licence, attribution text, loaded snapshot, row count).

| Dataset | Source | Licence |
| ------- | ------ | ------- |
| Postal codes, municipalities, city aliases | [GeoNames](https://www.geonames.org) | CC BY 4.0 |
| Airports | [OurAirports](https://ourairports.com) | Public domain |
| Time zones | [IANA Time Zone Database](https://www.iana.org/time-zones) | Public domain |
| Address rules, postal code formats | [Google libaddressinput](https://github.com/google/libaddressinput) | CC BY 4.0 |
| NACE, CPA, PRODCOM, NUTS, LAU, Combined Nomenclature | [Eurostat](https://ec.europa.eu/eurostat) / European Commission, mostly via the EU Vocabularies service | Eurostat free re-use, source acknowledged — adapted; Eurostat is not responsible for the adaptation |
| WZ 2025 | [Statistisches Bundesamt (Destatis)](https://www.destatis.de) | Reproduction and distribution with source citation |
| Localization data, currency precision | [Unicode CLDR](https://cldr.unicode.org) | Unicode License v3 (© Unicode, Inc.) |
| Exchange rates | [European Central Bank](https://www.ecb.europa.eu/stats/eurofxref/) | Free use with the ECB cited; for information purposes only; available free of charge from the ECB |
| Phone number metadata | [Google libphonenumber](https://github.com/google/libphonenumber) | Apache License 2.0 |
| Boundaries | [Natural Earth](https://www.naturalearthdata.com) | Public domain |
| Country facts, stations, ports | [Wikidata](https://www.wikidata.org) | CC0 1.0 |
| CPV | [Publications Office of the EU (TED)](https://ted.europa.eu/en/simap/cpv) | EU Commission re-use policy |
| ISO 4217 currency codes | [SIX Group](https://www.six-group.com) (ISO 4217 Maintenance Agency) | No licence stated; ISO standard - rights not verified |
| ISO 639-3 language codes | [SIL International](https://iso639-3.sil.org) | SIL terms - rights not verified |
| Legal forms | [GLEIF](https://www.gleif.org) ISO 20275 code list | No licence stated on the file - rights not verified |
| IBAN formats, SEPA flag | SWIFT IBAN registry, compiled by [php-iban](https://github.com/globalcitizen/php-iban) | SWIFT: copying "not as a source of profit"; php-iban LGPL-3.0 - **rights not verified** |
| E-invoicing code lists, Peppol participants | [OpenPeppol](https://docs.peppol.eu/edelivery/codelists/) / [Peppol Directory](https://directory.peppol.eu) | No licence stated - rights not verified |
| VAT rates | [European Commission, TEDB](https://ec.europa.eu/taxation_customs/tedb/) | Commission reuse policy presumed; not a reference for legal purposes - rights not verified |
| German bank code directory | [Deutsche Bundesbank](https://www.bundesbank.de) | No licence stated - rights not verified |
| Vehicle makes, models, WMI codes, recalls | [U.S. DOT / NHTSA](https://vpic.nhtsa.dot.gov) | No licence statement (U.S. government work) - rights not verified |
| European car models | [European Environment Agency](https://www.eea.europa.eu/en/datasets/dataset/co2-cars-emission-22) (Regulation (EU) 2019/631), aggregated by Doards | EEA data policy: open licence (ODC-By or similar); source acknowledged |
| Country indicators | [The World Bank](https://data.worldbank.org) | CC BY 4.0 (per the World Bank; terms page not retrievable) |
| Postal code to NUTS | [Eurostat GISCO TERCET](https://ec.europa.eu/eurostat/web/nuts/correspondence-tables) | Eurostat reuse; postal data CC BY-SA 4.0 per GISCO - rights not verified |
| Units of measure, package types | UN/ECE Recommendations 20 / 21 via [UN/CEFACT vocab-codes](https://github.com/uncefact/vocab-codes) | **UN terms: non-commercial, no redistribution** (repository says GPL-3.0) - rights not verified |
| Incoterms | International Chamber of Commerce | Codes and names only; Incoterms® is an ICC trademark |
| Calendar names, plural rules, unit conversions and preferences | [Unicode CLDR](https://cldr.unicode.org) | Unicode License v3 (© Unicode, Inc.) |
| Neighbouring countries | [GeoNames](https://www.geonames.org) | CC BY 4.0 |
| EU, euro area, Schengen, EEA, EFTA membership | [European Union country profiles](https://european-union.europa.eu/principles-countries-history/eu-countries_en), European Commission, EFTA Secretariat | © European Union, CC BY 4.0; compiled |
| Country keys and code systems, German municipality register | [Statistisches Bundesamt (Destatis)](https://www.destatis.de) | Reproduction and distribution with source citation |
| ÖNACE, Austrian municipality list | [STATISTIK AUSTRIA](https://www.statistik.at) | Reproduction with the source named; edited |
| Swiss register of communes | [Bundesamt für Statistik (BFS)](https://www.bfs.admin.ch) | opendata.swiss "Open use" |
| Vehicle classes, body types, fuels, emission classes | [Kraftfahrt-Bundesamt (KBA)](https://www.kba.de), SV 1 | Reproduction and distribution with source citation |
| German type keys (HSN / TSN) | [Kraftfahrt-Bundesamt (KBA)](https://www.kba.de), SV 4.1 - 4.5 | Contradictory statements in the booklets - rights not verified |
| German tax offices | [Bundeszentralamt für Steuern (BZSt)](https://www.bzst.de), GemFA | No licence stated - rights not verified |
| Registration authorities | [GLEIF](https://www.gleif.org) Registration Authorities List | No licence stated - rights not verified |
| German courts | XJustiz code list GDS.Gerichte ([XRepository](https://www.xrepository.de)) | No licence found - rights not verified |
| Food allergens | [EUR-Lex](https://eur-lex.europa.eu/eli/reg/2011/1169), Regulation (EU) No 1169/2011, Annex II | © European Union; consolidated text CC BY 4.0 |
| Food additives (E numbers) | [EUR-Lex](https://eur-lex.europa.eu/eli/reg/2008/1333), Regulation (EC) No 1333/2008, Annex II Part B | © European Union; consolidated text CC BY 4.0 |
| Territories outside the EU VAT area | [EUR-Lex](https://eur-lex.europa.eu/eli/dir/2006/112), Directive 2006/112/EC, Article 6 | © European Union; consolidated text CC BY 4.0; country code added |
| VAT number formats | [European Commission, VIES](https://ec.europa.eu/taxation_customs/vies/#/faq) | © European Union, CC BY 4.0 |
| German plate prefixes | [Kraftfahrt-Bundesamt (KBA)](https://www.kba.de), Kfz-Kennzeichen und auslaufende Kennzeichen | Reproduction and distribution with source citation |
| Driving licence codes | [EUR-Lex](https://eur-lex.europa.eu/eli/dir/2006/126), Directive 2006/126/EC, Annex I | © European Union; consolidated text CC BY 4.0 |
| Nature-of-transaction codes | [EUR-Lex](https://eur-lex.europa.eu/eli/reg_impl/2020/1197), Implementing Regulation (EU) 2020/1197, Annex V | © European Union; consolidated text CC BY 4.0 |
| German depreciation table AV | [Bundesministerium der Finanzen](https://www.bundesfinanzministerium.de), BMF letter of 15.12.2000 | Official instruction; no terms stated - rights not verified |

Coverage notes you should know before relying on the data: postal-code coordinates are estimates and the postal data is **not** address verification;
airports, runways, frequencies and navigation aids are community maintained and **not** for flight operations; time zone offsets are informational (time zones change by law); currency digits and CLDR formats are display
metadata, not tax-law rounding; phone patterns are syntax checks only; boundaries are generalized cartography; Wikidata is community edited and incomplete; the ECB rates are for information only. **The datasets marked "rights not verified" were added in 0.4.0 on the owner's instruction; whether they may be served or redistributed is still undecided** - `data_sources.review_status` says `pending_human_review`. Vehicle data covers the U.S. market only; `/vehicle-model-years` lists only recalled vehicles; `/peppol-participants` is a snapshot of identifiers (check the live Peppol SMP before sending); the VAT rates are not legal advice. Details per dataset: `DATA_SOURCES.md` and `DATA_LICENSES.md` in the repository.

---

## About Doards

Doards is a customizable ERP platform. We maintain a large set of reference data
to power the product, and we publish the read-only parts here so other
developers can benefit from it too.

Found an error in the data, or have a codelist / dataset you'd like to see
added? Open an issue or pull request — contributions are very welcome.

For API keys and account-related questions, visit **<https://doards.com/api>**.
