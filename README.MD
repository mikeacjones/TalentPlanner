# Talent Sequence 2

Talent Sequence 2 is a Classic-era WoW AddOn that shows a talent-point spending order next to the in-game talent frame.

This source tree is the cleaned top-level addon layout used for both supported clients:
- `TalentSequence2_Vanilla.toc` for Classic Era / Anniversary Vanilla
- `TalentSequence2_TBC.toc` for Burning Crusade Classic

The generic `TalentSequence2.toc` is intentionally unsupported and exists only to make the client choose the flavor-specific TOC.

## Supports

- Icy Veins talent calculator URLs
- Wowhead talent calculator URLs
- Blizzard talent UI
- Talented addon integration

## Current Wowhead Support

The Wowhead importer now supports a mapping-based decode path for flavors where the old fixed slot-index parser is unreliable.

Current mapped support:
- TBC Warrior
- TBC Paladin
- TBC Hunter
- TBC Rogue
- TBC Priest
- TBC Shaman
- TBC Mage
- TBC Warlock
- TBC Druid

The parser is structured as:
- `flavor -> class -> tree -> wowhead code -> talent name`

It then resolves the real in-game talent index dynamically with `GetTalentInfo`, which makes the importer much more robust across Classic/TBC tree differences.

Classic mappings can be added incrementally by supplying one calibration file per tree with:
- the full Wowhead URL
- the exact point-by-point order

## Source Layout

Top-level files:
- `TalentSequence.lua` main UI and sequence state
- `WowheadTalents.lua` Wowhead URL import and per-class mapping overrides
- `IcyVeinsTalents.lua` Icy Veins URL import
- `Localization.lua` strings
- `TalentSequence2_TBC.toc` TBC client entry point
- `TalentSequence2_Vanilla.toc` Vanilla client entry point

There is no nested addon subfolder in this repo anymore. Packaging is done directly from the root.

## Packaging

Run:

```sh
./package.sh
```

This creates a zip under `.release/` containing a single top-level `TalentSequence2/` addon folder with the Lua files, TOCs, and metadata needed for distribution.

## Installing Locally

Copy the packaged `TalentSequence2/` folder into:

```text
World of Warcraft/_anniversary_/Interface/AddOns/
```

Then restart the client if you changed TOC metadata, or use `/reload` for Lua-only changes.

## Notes

- If a saved active sequence is deleted, the addon now clears the active sequence instead of recreating it as `<unnamed>` on reload.
- The popup import dialog has been updated to work with the newer `StaticPopup` edit box field naming used by current Classic clients.
