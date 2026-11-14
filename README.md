# FreakyVille Official: data

Regler, kits, vagt vaults og links til LabyMod-addonet **FreakyVille Official**. Addonet henter filerne herfra (branch `main`), hver gang spillet starter. En rettelse her når spillerne næste gang de starter spillet, uden en ny version af addonet.

Addonet har altid en kopi af filerne indbygget. Den bruges, indtil filerne herfra er hentet, og hvis de ikke kan hentes eller læses. En fejl i en fil når derfor aldrig spillerne: så bruger de bare den indbyggede kopi.

## Filerne

| Fil | Indhold |
| --- | --- |
| `manifest.json` | Formatets nummer (`schema`) og hvilke filer der hører til |
| `shared.json` | Funktioner der kan slås fra, links, og regler der gælder i alle blokke |
| `blocks/a.json`, `b.json`, `c.json` | Blokkens regler, kits og vagt vaults |

### shared.json

- `features`: `prices`, `kits` og `vaults` kan sættes til `false`, hvis noget går galt. Så slår addonet funktionen fra hos alle, indtil den sættes til `true` igen.
- `links`: siderne på startsiden og i menuen (`website`, `report`, `shop`, `discord`).
- `vagtLore`: hvordan lore-linjen på vagt-gear begynder, fx `"Købt af:"`.
- `ignore`: kategorier eller entries, der aldrig giver en pris i spillet, fx `"Celler"` eller `"Rare Items > Fake VK"`.
- `notOwners`: ord i ejer-feltet på prislisten, der ikke er spillere, fx `"ukendt"`.
- `vagt`, `items`: se nedenfor.

### blocks/a.json (og b, c)

- `ignore`, `items`, `gear`, `names`, `references`: regler for blokken. De læses før reglerne i `shared.json`.
- `kitReceived`: chat-linjen når man tager et kit, med `{kit}` hvor kittets navn står, fx `"Received kit {kit}-a."`.
- `kits`: kittene, fx `"titan": {"name": "Titan"}`. Hvornår et taget kit kan tages igen, står som enten `"cooldown": "2d"` (en fast tid efter det blev taget) eller `"resetsAt": "00:00"` (et klokkeslæt, dansk tid, fx midnat for et dagligt kit). Uden nogen af dem står kittet som "Taget", indtil serveren spørges igen ved næste join.
- `vaults`: vagt vaults med navn, chat-linjer og cooldowns.

Placeholders (som `%luckperms_...%`) ligger ikke her. De ligger i addonet selv.

### Reglerne

| Liste | Nøgle | Værdi |
| --- | --- | --- |
| `vagt` | Item-id, fx `"*_leggings"` | Entry på prislisten for vagt-gear |
| `items` | Item-id uden enchants, fx `"red_stained_glass"` | Entry, fx `"Rød Glass"` |
| `gear` | Item-id med enchants, fx `"diamond_helmet protection:4 unbreaking:2-3 ..."` | Entry |
| `names` | Itemets eget navn, fx `"Legend Boots"` | Entry med et andet navn |
| `references` | Kort form i en pris, fx `"bo-heads"` | Kategorien den peger på |

Item-id'er er som i Minecraft 1.13 og nyere, uden `minecraft:`. `*` står for hvad som helst. En level kan være et interval (`unbreaking:1-3`), og `...` til sidst tillader andre enchants også. Den første regel, der rammer noget på blokkens prisliste, vinder.

## Versioner: `since`

Ældre versioner af addonet får rettelser til det, de allerede kender, men ikke nye ting, før de er markeret til det.

Alt, der er et objekt (et kit, en vault, et link), kan få `"since": "<addon-version>"`. Så ser kun den version og nyere det:

```json
"vaults": {
  "frederik": { "name": "Vagt Vault A" },
  "aplus":    { "name": "A+ Vagt Vault", "since": "1.1.0" }
}
```

En regel er normalt bare tekst, men kan skrives som et objekt for at få `since`:

```json
"items": {
  "red_stained_glass": "Rød Glass",
  "lime_stained_glass": { "entry": "Lime Glass", "since": "1.1.0" }
}
```

Uden `since` ser alle versioner tingen. Ændres en værdi (fx en cooldown), får alle versioner den nye værdi.

## Formatet: `schema`

`schema` i `manifest.json` hæves kun, når et felt skifter betydning eller forsvinder. En version af addonet, der ikke kender det nye nummer, bruger sin indbyggede kopi og beder spilleren opdatere addonet.
