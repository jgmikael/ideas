
# Korjausrakentamisen tilastoinnin tavoitetilan arkkitehtuuri

Tämä dokumentti kuvaa korjausrakentamisen tilastoinnin tavoitetilan arkkitehtuurin.
Alla oleva kaavio on toteutettu Mermaid-muodossa ja renderöityy Markdown-ympäristöissä,
joissa Mermaid on tuettu (esim. GitHub, GitLab, Obsidian).

## Arkkitehtuurikaavio

```mermaid
flowchart LR
  subgraph Sources[Lähdejärjestelmät]
    A1[Kuntien rakennusvalvonta
(luvat, hankestatus)]
    A2[Ryhti / valtakunnallinen koonti]
    B1[DVV rakennus- ja huoneistotiedot
(master data + tunnisteet)]
    C1[Veroraportointi / työmaatiedot
(urakat, työmaat)]
    D1[Julkiset hankinnat
(urakka-arvot)]
    E1[Taloushallinto / eLasku / isännöinti]
  end

  subgraph Integration[Integraatio- ja tapahtumakerros]
    I1[API-poiminnat & tapahtumaväylä]
    I2[Tunnisteiden linkitys ja resoluutio]
  end

  subgraph Lakehouse[Korjausrakentamisen tilastollinen data-alusta]
    L1[Raw]
    L2[Harmonized]
    L3[Curated]
  end

  subgraph Rules[Luokitus- ja sääntömoottori]
    R1[Korjaus vs uudis]
    R2[Korjauslajit]
    R3[Kohdeluokat]
  end

  subgraph Outputs[Julkaisu ja analytiikka]
    O1[Tilastojulkaisut]
    O2[Suhdanneindikaattorit]
    O3[Avoimet aggregaatit]
  end

  A1 --> I1
  A2 --> I1
  B1 --> I2
  C1 --> I1
  D1 --> I1
  E1 --> I1

  I1 --> L1
  I2 --> L2
  L1 --> L2 --> L3

  L2 --> R1
  L2 --> R2
  L2 --> R3
  R1 --> L3
  R2 --> L3
  R3 --> L3

  L3 --> O1
  L3 --> O2
  L3 --> O3
```
