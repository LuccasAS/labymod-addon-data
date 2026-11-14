# FreakyVille Official: data

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
