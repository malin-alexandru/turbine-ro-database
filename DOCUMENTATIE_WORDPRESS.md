# Documentatie WordPress (implementare)

Acest document este pentru echipa tehnica. Fisierele sunt deja pregatite pentru import.

---

## 1) Fisiere de import primite

Din `exports_csv`:

- `product_item.csv` -> produse (baza WooCommerce)
- `vehicle_make.csv` -> marci
- `vehicle_model.csv` -> modele
- `vehicle_engine.csv` -> motorizari
- `product_item_vehicle_map.csv` -> mapare produs <-> motorizare
- `product_sku.csv` -> SKU-uri unice
- `turbo_code.csv` -> coduri turbo unice
- `sku_turbo_code_map.csv` -> mapare SKU <-> cod turbo

Imagini:

- `product_images/{SKU}` -> imagini organizate pe SKU
- `ALL images` -> toate imaginile la un loc

---

## 2) Ce trebuie implementat

### A. Import produse in WooCommerce

Importati `product_item.csv` ca produse WooCommerce.

Mapare minima:

- `product_item_id` -> meta custom produs (ex: `_external_product_item_id`)
- `sku` -> SKU produs
- `title` -> nume produs
- `price`, `regular_price` -> preturi
- `stock_status` -> status stoc

### B. Creare tabele custom pentru compatibilitate

Creati tabele custom (prefix recomandat `wp_turbine_`):

- `wp_turbine_make`
- `wp_turbine_model`
- `wp_turbine_engine`
- `wp_turbine_product_engine_map`
- `wp_turbine_sku`
- `wp_turbine_code`
- `wp_turbine_sku_code_map`

Importati CSV-urile corespunzatoare in aceste tabele.

### C. Legatura WooCommerce <-> compatibilitate

Produsele WooCommerce trebuie legate prin `product_item_id` (meta `_external_product_item_id`) de:

- `wp_turbine_product_engine_map.product_item_id`

---

## 3) Filtru frontend (obligatoriu)

Filtru pe 3 nivele:

1. Marca (`vehicle_make`)
2. Model (`vehicle_model`)
3. Motorizare (`vehicle_engine`)

Rezultat:

- din `product_item_vehicle_map` obtineti `product_item_id`
- mapati `product_item_id` la produse WooCommerce prin meta extern

---

## 4) Search dupa cod turbo (separat de filtru)

Flux:

1. Cautare in `turbo_code.code`
2. `turbo_code_id` -> `sku_turbo_code_map`
3. `product_sku_id` -> `product_item`
4. Afisare produse WooCommerce

---

## 5) Imagini produse

Implementati asocierea imaginilor pe produs:

- sursa recomandata: `product_images/{SKU}`
- alternativ bulk: `ALL images`

Asocierea se face dupa `SKU` (sau dupa `product_item_id`, daca aveti mapping intern).

---

## 6) URL si SEO pentru filtre

URL dinamic recomandat:

- `/turbina/reconditionata/{marca}`
- `/turbina/reconditionata/{marca}/{model}`
- `?motorizare=...` pentru nivel motor

SEO:

- index: marca, marca+model
- noindex (de regula): nivel motorizare

---

## 7) Checklist de livrare

- [ ] Produse importate din `product_item.csv`
- [ ] Meta `_external_product_item_id` setat pe produse
- [ ] Tabele custom create si populate din CSV
- [ ] Filtru pe 3 nivele functional
- [ ] Search dupa cod turbo functional
- [ ] Imagini atasate produselor
- [ ] URL dinamic pe filtre implementat

