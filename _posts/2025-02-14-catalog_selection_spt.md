---
layout: post
title: "Hunting for the most distant dusty galaxies: Catalog Matching and SQL Queries"
date: 2025-02-14
categories: data-science
author: Jack Birkin
---

In my journey to identify high-redshift (z \~7 and beyond) submillimeter galaxies (SMGs), I’ve been working with multi-wavelength catalogs from **SPT3G, MeerKAT, and SPIRE**. Here’s a summary of what I’ve learned and how I’ve used SQL to filter and explore these datasets.

---

## 📊 1. SQL Queries to Cross-Match Catalogs

### Finding SPIRE Dropouts for High-z Candidates

SPIRE dropouts (sources detected in SPT but not in SPIRE) can indicate z > 5 galaxies.

```sql
SELECT s.spt3g_id, s.ra, s.dec, s.flux_1p4mm, sp.flux_500um, mk.flux_radio
FROM spt3g s
LEFT JOIN spire sp ON s.spt3g_id = sp.source_id
LEFT JOIN meerkat mk ON s.spt3g_id = mk.source_id
WHERE s.flux_1p4mm > 10  -- Bright SPT source
  AND (sp.flux_500um IS NULL OR sp.flux_500um < 5)  -- SPIRE dropout
  AND (mk.flux_radio IS NOT NULL AND mk.flux_radio < 0.1);  -- Faint MeerKAT detection
```

This query combines data from multiple catalogs to flag sources likely to be high-z SMGs.

---

## 🧪 2. SPIRE Confusion Limit: Can We Trust Low-Flux Sources?

- **SPIRE confusion limits**: \~5.8 mJy (250µm), 6.3 mJy (350µm), 6.8 mJy (500µm). Sources below these limits are usually unresolved blends.
- **Can we have sources below the confusion limit?** Yes, if they were detected via:
  - **Deblending techniques** using SPT or MeerKAT priors.
  - **Stacking analysis** or **matched-filtering**.

### Filtering Potential Artifacts with SQL

```sql
SELECT *
FROM spt3g
WHERE flux_500um < 6.8  -- Below confusion limit
  AND snr > 5           -- High confidence detection
  AND EXISTS (
    SELECT 1 FROM meerkat
    WHERE meerkat.source_id = spt3g.source_id
  );
```

This query ensures only significant detections with MeerKAT priors are kept.

---

## 🚀 3. Matching SPT3G Sources to SPIRE Footprints

### Finding Sources Inside the SPIRE Footprint

```sql
SELECT *
FROM spt3g
WHERE ra BETWEEN min_spire_ra AND max_spire_ra
  AND dec BETWEEN min_spire_dec AND max_spire_dec;
```

When using a polygon footprint from a SPIRE image, I learned to exclude internal NaN regions to avoid false matches.

---

## 💡 Key Takeaways

- ✅ **SPIRE and MeerKAT dropouts** are valuable high-z SMG candidates.
- ✅ Be cautious with **sources below the SPIRE confusion limit**—use priors or cross-matches.
- ✅ Use **SQL queries with spatial filters** to refine your catalog matches.
