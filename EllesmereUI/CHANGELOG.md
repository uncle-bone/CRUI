# EllesmereUI

## Conquest Reborn backport hotfix (2026-11-08, r21)

- Raised custom player-frame class-resource pips above the player border and health text overlay. The previous frame level could put the pips underneath those overlays even when their values, alpha, and anchors were correct.
- Keeps r20's generic `CoAResourceSegmentBar.value` / `maxValue` reader for all active stock class/spec segment resources, independent of Resource Bars, with aura/native fallbacks.
- In-game confirmation is still needed for the pips' visual appearance across classes/specs.

## Conquest Reborn backport hotfix (2026-11-08, r20)

- Player-frame class-resource pips now read the active `CoAResourceSegmentBar` value and cap directly. This is the game's shared class/spec-aware segmented-resource state, so every class/spec routed through that stock bar is supported without Resource Bars being enabled.
- Existing per-class aura readers remain as fallbacks when the stock segment widget is unavailable; ordinary WoW class resources continue to use their native power APIs.
- Includes r19's icon preview and resource-provider changes, plus earlier recovery fixes.
- Static source review only; in-game verification is still needed for all class/spec segment trackers and the visible player-frame pips.

## Conquest Reborn backport hotfix (2026-11-08, r19)

- Fixed player-frame CoA class-resource pips reading 0 while Resource Bars showed the current aura stacks. The player frame now uses Resource Bars' same live stack provider and cached CoA class token, keeping Demonfire and other supported aura-stack resources in sync.
- Keeps the local aura reader as a fallback if Resource Bars is not loaded.
- Fixed striped combat-indicator dropdown/preview icons by loading the CoA-ready uncompressed icon textures in the options panel.
- Static source check only; verify the player-frame count against the known-good Resource Bars tracker in game.

## Conquest Reborn backport hotfix (2026-11-08, r18)

- Reworked the native micro menu to size each original stock icon texture to the EUI button, including the standalone Character portrait. Explicitly clears leftover stock button layers so their default square artwork no longer shows behind the EUI skin.
- Removed the unnecessary DragonUI-specific micro-menu handling; this build does not assume DragonUI is used with EUI.
- Includes r17 ownership fallback and all prior recovery fixes.
- Static source check only; requires in-game confirmation.

## Conquest Reborn backport hotfix (2026-11-08, r17)

- Fixed a r16 micro-menu regression where DataBars could claim menu ownership while its menu block had no visible buttons, hiding the native menu as well. DataBars now reports ownership only while its bar and at least one configured menu button are visible; otherwise the EUI native menu remains available.
- Includes all r16 changes and prior recovery fixes.
- Static source/package check only; requires in-game confirmation.

## Conquest Reborn backport development update (2026-11-08, r16)

- ActionBars now resets DragonUI's no-op micro-button anchor method before applying EUI positions and hides DragonUI's competing strip while EUI owns the menu.
- When a DataBars Micro Menu block is active, its own buttons take ownership and the stock/DragonUI micro bar is hidden; ownership changes are detected while playing.
- The DataBars Game Menu now calls CoA's `ToggleGameMenu` first and falls back to the stock button handler and legacy openers.
- Minimap option dependencies refresh immediately after parent toggles change; coordinates re-sync/retry the Wrath map API and show a dash when the client has no position; zone text now uses the minimap subzone and can show zone plus subzone.
- The late-created Blizzard `TimeManagerClockButton` fix from r15 and the CoA class-resource defaults/Demonfire handling from r14 are included.
- Added `/crui` as another settings command; `/eui` and the existing aliases remain available. Branding and addon folder identifiers stay unchanged.
- Audit: all 21 root addon TOCs checked with no missing files; all 531 Lua files parse as Lua 5.1. No live-game test was performed.
- Known limits: resource providers across every class/spec still need in-game confirmation; unverified CoA spell IDs and caps are not invented. Mythic+ remains deferred.

## Conquest Reborn backport development update (2026-11-08, r15)

- Fixed the native `TimeManagerClockButton` appearing over the EUI minimap when CoA creates it after the minimap module's first initialization. EUI now captures its original visibility/mouse state when it appears, hides it while the EUI minimap is enabled, and restores it if the module is disabled.
- The EUI minimap clock and calendar remain the user-facing controls.
- Static source/package review only; not verified in a live game.

## Conquest Reborn backport development update (2026-11-08, r14)

- DataBars micro menu Game Menu now clicks Blizzard's own `MainMenuMicroButton` first, so CoA's client-specific Escape menu action is used instead of relying on a legacy frame toggle. Existing fallback openers remain for clients without the stock button.
- Native micro menu now preserves each Blizzard/CoA button's own icon textures, follows the client's published `MICRO_BUTTONS` order, and reapplies its layout after login/world entry to catch buttons created later during startup.
- Unit Frames now recognizes CoA custom-class identity more reliably and seeds the player class-resource display once for existing profiles. KoX Demonfire uses the existing confirmed 500906 stack tracker.
- Class/spec resource support remains incomplete where CoA spell/aura IDs and stack limits have not been confirmed. Mythic+ remains deferred.
- Code/package check only; not verified in a live game.

## Conquest Reborn backport development update (2026-11-05, r13)

- DataBars: run at most one registered one-second heartbeat callback per rendered frame, rotating through the active callbacks. This prevents multiple bars from performing their periodic refreshes together on a single frame. Callback cadence remains approximately once per second; if a batch takes longer than one second to drain, it is allowed to finish before the next batch begins.

## Conquest Reborn backport development update (2026-11-05, r12 diagnostic)

- DataBars: log a one-time chat warning when an individual heartbeat callback or queued layout pass takes at least 8 ms. This identifies the slow block during an instrumented reproduction without adding recurring output for normal callbacks.

## Conquest Reborn backport development update (2026-11-05, r11)

- Nameplates: retain immediate aura-event refreshes and lengthen the missed-event recovery scan from 0.15 to 0.5 seconds. This cuts repeated aura-list scans across visible plates while preserving an event-driven fast path.

## Conquest Reborn backport development update (2026-11-05, r10)

- Nameplates: only read class-resource auras or combo points when the target plate is displaying class-resource pips. The renderer runs at 20 Hz for every visible plate; this avoids repeating the same target aura lookup for unrelated plates.

## Conquest Reborn backport development update (2026-11-05, r9)

- Resource Bars and Unit Frames: add Starcaller Scattered Stars (804378) as a target-debuff stack tracker, while preserving Warden's separate spec-gated Energy bar. Nameplate class-resource tracking reads the target debuff too.
- Unit Frames: retry the bounded CoA secondary-power resolver when a player's maximum-power event arrives and no secondary was cached, or when the cached pool becomes unavailable. This lets power bars recover across resource/spec transitions without scanning candidates on every ordinary power update.

## Conquest Reborn backport development update (2026-11-05)

- Pixel Perfect: guard the optional Retail OnBackdropSizeChanged helper so CoA's backdrop resize callback no longer errors when that mixin method is absent.
- Resource Bars: improve MH/OH recovery when CoA hand flags are ambiguous. A positive offhand attack speed can keep the OH row available despite a conflicting legacy helper; infer a new OH phase from plausible cadence, clear expired phases on restart, and clear OH state when offhand capability is gone.
- Bags: keep the five-second inventory recovery scan limited to an open live bag or bank view. Settled bag events still capture changes while the windows are closed, avoiding a full inventory walk during routine play.
- Chat: guard whisper-mute cleanup when UnmuteSoundFile is unavailable and retain the EUI mute-ownership marker for a later supported client.
- Nameplates: confirm the CoA level fallback uses UnitLevel only for a still-matching bound unit and displays ?? when no reliable level is available.
- Performance investigation: the closed-window bag scan is now gated, but the reported dungeon hitch remains unprofiled; nameplate aura refresh and other periodic loops remain candidates.
- Resource Bars: add a live settings preview for Health, Power, and the active class resource or secondary power on the Class, Power and Health Bars page. The sample reflects bar dimensions, fills, colors, textures, text, and the appropriate pip count without toggling live preview mode.
- Resource Bars and Unit Frames: only show a CoA secondary continuous power when the archived class/spec crosswalk lists it; remove generic scans that could invent a second bar from unrelated positive power slots. The active Starcaller spec is resolved through CoA's native active-spec API.
- Resource Bars: update Knight of Xoroth/Fleshwarden Demonfire (spell 500906) to six pips and stop rendering an inferred secondary continuous power alongside those class-resource pips.
- Nameplates: use the confirmed six-stack maximum for Knight of Xoroth/Fleshwarden Demonfire and ignore stale generic spell-stack metadata for that aura.
- Unit Frames: stop inferring a secondary continuous power for Fleshwarden/KoX, whose confirmed layout is Rage plus Demonfire class-resource pips.
- Resource Bars: prioritize the CoA class token before the underlying Wrath class so custom classes do not accidentally inherit Rogue combo points, Druid form handling, or Death Knight runes; cap custom stack geometry to the ten pips the module builds.
- Resource Bars: clear a cached CoA class token when the client confirms the character is no longer on a custom class, while retaining it during a temporary API lookup failure.
- Resource Bars: require positive spell-knowledge confirmation for gated class-stack providers; unknown client results no longer enable them by default.
- CoA class-resource trackers: keep Knight of Xoroth Demonfire fixed at the observed six stacks, read debuff-backed stacks from the target, and refresh player/target aura providers with a native UnitAura fallback when AuraUtil is unavailable.
- Settings: label Mythic+ Tools as Coming Soon on the 3.3.5 CoA build; its runtime addon is not included in this client build.
- Resource Bars: use a positive CoA off-hand flag to identify OH swings; use swing-timer cadence when the flag is false or absent, after tester feedback showed the strict false-to-MH mapping stopped OH updates.
- Unit Frames: set the secondary power label font before either text branch, preventing the 3.3.5 `SetText(): Font not set` startup error after Reset All on fresh defaults.
- Settings: use the modern class sprite for the Spec Overrides toolbar button and enlarge the hit target to improve its clarity beside module search.
- DataBars: open CoA's actual Game Menu with `GameMenuFrame_Show`, falling back to the client's other menu toggles only when needed.
- Micro Menu: use the installed CoA button order in the Native HUD, then reconcile the client's button list and optional controls without duplicates.
- Micro Menu skin: guard the optional `IsForbidden` API and include separate Wrath/CoA Spellbook, Talents, Social, and PvP buttons in the skin pass.
- DataBars: resolve specialization labels with CoA's shipped `GetActiveChrSpec` API, including spec IDs 1–3; retain Wrath talent trees as fallback and show “Specialization Unknown” rather than “No Talents” when the client cannot resolve an active spec safely.
- Damage Meters: include CoA/Details-supported building damage and building-miss combat-log events in damage, damage-taken, and miss activity totals. The meter continues to read native 3.3.5 event arguments directly; it does not depend on Details or its Retail-layout translator.
- Damage Meters: keep observed summon links to their immediate owner, resolve nested pets with cycle protection, and preserve an established owner when duplicate summon events arrive.
- Bags: debounce inventory snapshots and redraws after BAG_UPDATE, use BAG_UPDATE_DELAYED when supported, and retain a 0.2-second fallback so split/move bursts settle before displayed stack counts are read.
- Chat: add a Tabs on Bottom option; combine it with the existing Input on Top setting for an input field above messages and chat tabs below the window. The tab backdrop and sidebar follow the selected edge.
- Resource Bars: address the tester-reported MH/OH regression by treating false/zero hand flags as ambiguous and returning those events to timing attribution; a positive true/1 flag still explicitly starts OH.
## Conquest Reborn backport development update (2026-11-04)

- Unit Frames: CoA custom classes fall back to the custom class-resource display when an imported profile requests Blizzard's class-power style, since these classes have no native Blizzard class-power widget.
- Damage Meters: ensure an existing profile gets the matching current-target Threat window if it is missing, even if an earlier initialization set the migration marker without creating the window.
## [v9.4](https://github.com/EllesmereGaming/EllesmereUI/tree/v9.4) (2026-10-07)
[Full Changelog](https://github.com/EllesmereGaming/EllesmereUI/compare/v9.3.8...v9.4) [Previous Releases](https://github.com/EllesmereGaming/EllesmereUI/releases)

- Release v9.4  
- Merge pull request #2525 from IslayLaphroaig/add-form-and-stance-only-cooldown-state-visibility  
    Cooldown Manager: add form and stance visibility effects to Cooldown State Effects  
- 9.3.9  
- Merge pull request #2529 from Barbiero/locale/ptbr-since-938  
    ptBR: translate strings added since v9.3.8  
- Merge pull request #2487 from Hasenburg/feat/rounded-corners  
    feat: optional rounded corners (Corner Radius)  
- ptBR: translate the bags Junk Marker and bank display, nameplate debuff coloring class cards, WoW Forever name format settings and combo point placement, the /cd Cooldown Manager toggle, ignoring older expansion quests, and Raid Frames / Forever Essentials outline overrides  
- Merge pull request #2524 from Shiyan66666/main  
- feat: optional rounded corners (Corner Radius)  
    Adds an opt-in Corner Radius (default 0, off) for unit frames (incl. a  
    detached power bar), raid and party frames, nameplates (health and cast  
    bar), the Resource Bars (health, power, class resource and pips) and the  
    swing timer, set from an inline cog on each Border Size control.  
    Shared core in EllesmereUI\_RoundedCorners.lua: one nine-sliced rounded  
    mask per body texture, hosted on the owner frame; a Solid border becomes a  
    rounded fill with the body's inverse cut out; Glow and Shadow are redrawn  
    round. Other border styles keep square corners. Previews follow.  
    Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>  
- Fix typo in date format in zhCN.lua  
- Merge pull request #2518 from DlargeX/main  
- Update zhCN  
- 9.3.9  
- Merge pull request #2522 from dfrisone/fix/forever-inspect-talents-button  
    fix(blizzard skin): restyle the inspect Talents button on Forever  
- fix(blizzard skin): fit Forever's ranged slot in the inspect weapon row  
    The row was laid out for two weapon slots; Forever's ranged slot ran into  
    the Talents button once it moved to the bottom corner. Recentre the three  
    slots and narrow the two buttons when InspectRangedSlot exists.  
- fix(blizzard skin): restyle the inspect Talents button on Forever  
    Forever parents InspectTalents to InspectPaperDollFrame rather than  
    InspectPaperDollItemsFrame, so the lookup came back nil and the button  
    stayed at its stock top-centre spot under the average item level.  
- Merge pull request #2520 from JuJuFX-dev/refactor/raidframes-main-split  
    Split EllesmereUIRaidFrames.lua into eighteen files  
- Merge pull request #2521 from JuJuFX-dev/feat/np-debuff-coloring-layout  
    UI(Nameplates): group Debuff Coloring lists into class cards  
- Merge pull request #2519 from JuJuFX-dev/fix/np-debuff-border-combo-priority  
- Regenerate the locale key list  
- Nameplates: group Debuff Coloring lists into class cards  
    The debuff and combo rows of every class ran through one two-column grid, told apart only by the class name in each label. Each class that holds an entry now gets its own expandable card (class icon, name in class color, entry counts) under a CLASSES header, with debuffs down the left column and combos down the right so both read top to bottom in priority order.  
    - Rows read "1. Debuff" / "1. Combo"; Spec Overrides and the Custom Spell popup keep the class-qualified name, so existing override entries still match and two classes cannot share one.  
    - Add Debuff / Add Combo close their column and add to their own class; a single wide Add Class button below the cards starts a class.  
    - A class without entries has no card, the player's included; the player's card starts expanded, the others collapsed.  
- Merge pull request #2462 from defnotjec/feature/bags-junk-marker  
- Merge pull request #2516 from LoChinAn/cdm-picker-status-translatable  
- Merge remote-tracking branch 'upstream/main'  
- CDM: harden form-only visibility event handling and retirement  
    - Cache uncertain tooltip results as a visible sentinel; retry after  
      PLAYER\_REGEN\_ENABLED so combat-secret text re-reads once readable.  
    - Publish shapeshift form names only when the full list is readable.  
    - Invalidate a single cached spell on SPELL\_DATA\_LOAD\_RESULT and  
      SPELL\_TEXT\_UPDATE; skip uncached spells.  
    - Pass allActiveFrames to watch reconciliation so inactive pool frames  
      leave the watch but keep their prior bar claim.  
    - Register SPELL\_UPDATE\_USABLE only while Hidden Until Usable  
      subscribers exist.  
    - Preset rearm toggles form events only when the need changes and  
      keeps PLAYER\_REGEN\_ENABLED for remaining cooldown rules.  
- Drop unread imports and an unread copy, guard the preview file, name the part files in comments  
    Review follow-up to the Raid Frames split. EUI\_RaidFrames\_Extras.lua imported the ten names of the ns.\_internals literal that moved to the main file, and EUI\_RaidFrames\_Lifecycle.lua kept an inCombat copy it only wrote. EUI\_RaidFrames\_Preview.lua now returns like the chain files when an earlier file failed. playerFriendlySpell and playerRezSpell are false in the table for a class without one, so their import goes through the guard like every other. Comments that pointed at a place in the old single file name the file that holds the code now.  
- fix(nameplates): rank debuff border colors so a combo draws over single debuffs  
- Merge pull request #2514 from JuJuFX-dev/refactor/quickdraw-main-split  
- Wire up the Raid Frames files: headers, shared locals, setters, load order  
- Move the Raid Frames code into seventeen new files (verbatim)  
- added more Localstrings  
- fixes and removed unused locals  
- Merge remote-tracking branch 'upstream/main'  
- added new Strings for German Locals  
- Add form-only cooldown state visibility  
- Merge pull request #2513 from JuJuFX-dev/fix/qt-auto-accept-trivial  
- Merge pull request #2515 from LoChinAn/locale-zhtw-debuff-colors-gossip-font-outline  
- fix(cdm): make custom spell, item and slot popup status messages translatable  
    The red status line in the Cooldown Manager's add-by-ID popups stayed  
    English on non-English clients: the five local SetStatus() helpers  
    passed their text straight to SetText().  
    Pass it through L() instead. All 15 callers hand in English literals,  
    so nothing is localized twice and English output is unchanged. Add  
    zhTW for the six new keys.  
- locale(zhTW): translate 47 new keys, prune 26 dead ones and refile Threat keys  
    Covers v9.3.7..v9.3.8: nameplate debuff coloring, auto gossip, tooltip  
    buffs, auto uprank messages, Debuff Manager hints and the Window Mover  
    tab, plus three Click Casting row labels that reach L() through  
    RowLabel().  
    The Fonts "Module Outline" tooltip joins the module name into the  
    sentence before L(), so the existing %1$s key never matched; it is  
    replaced by one full sentence per module.  
    The Unit Frames combat and leader indicator strings used three words  
    for "indicator"; they now match the panel's other indicators. 42  
    Threat Meter keys filed under Core / General before Forever Essentials  
    had a section move there, and one Unit Frames key leaves Nameplates.  
    The 26 removed keys are strings the source no longer produces.  
- feat(quest tracker): auto accept ignore older expansion quests option  
- Build the four fan-out setters in one loop, name the main file in the OnEnable comment  
- Point the nest sweep's extract at the two files that hold the geometry  
- Wire up the Quickdraw files: headers, shared locals, setters, load order  
- Move the Quickdraw code into twelve new files (verbatim)  
- Bags: Junk Marker -- opt-in vendor-value sort + Show Junk in Recent  
    Ordering items by vendor value deviates from the EUI norm (categories keep  
    their saved drag/visual order), so make it opt-in; and give control over  
    whether junk clutters the Recent Items section. Both live in the Junk cog.  
    - Sort Junk by Vendor Value (bagJunkSortByValue, default OFF): gates every  
      junk vendor-sort site -- the All Items pre-pass, the grouped-member and  
      selected-category paths, and the OneBag/MultiBag pull-out. Off -> Junk  
      orders like any other category (saved order / VisualSortCompare).  
    - Show Junk in Recent (bagJunkShowInRecent, default ON): shared  
      ns.JunkHiddenFromRecent gate filters junk (grey + player-marked) out of the  
      Recent Items section in the grid (OneBag + All Items) and list views when  
      turned off. Default-on path returns early with no IsJunk lookup, and it is a  
      no-op whenever the feature itself is off.  
    Also drops the stale "(sorted by vendor value)" note from the One Bag /  
    MultiBag pull-out tooltips, since that ordering is now opt-in.  
- Bags: Junk Marker -- actually order the Junk category by vendor value  
    Found (during in-game validation):  
    - The Junk category never ordered by vendor value. Two latent bugs: (1)  
      SortJunkByVendor read sell price from a bare itemID via C\_Item.GetItemInfo,  
      which returns nil on this client, so every value was 0; (2) the All Items  
      view ordered every category (Junk included) with ApplySavedOrder in a  
      pre-pass and only ran SortJunkByVendor in the grouped/selected paths, so in  
      All Items the Junk section used the saved/bag order and never vendor-sorted.  
    Verified:  
    - List view's Sell Price column reads the same price fine via the item LINK  
      (C\_Item.GetItemInfo(link) index 11); the bare-id form is what failed.  
    - In All Items, itemsByCat[junk] went through ApplySavedOrder (line pre-pass)  
      with no vendor sort anywhere downstream. Confirmed in-game: values now sort  
      high-to-low in both All Items and the selected Junk category.  
    Did:  
    - SortJunkByVendor reads sell price from d.itemLink (PreCacheSortFields warms  
      it just above); the SellJunk earned total reads it from info.hyperlink.  
    - All Items pre-pass vendor-sorts the Junk category instead of ApplySavedOrder,  
      matching the grouped-member and selected-category paths.  
- Bags: Junk Marker -- one Junk section in list view, not two  
    Elle:  
    - List view already puts grey items in its own "Junk" section, and marked  
      items now land in the new Junk category, so with the feature on there are  
      two sections called Junk. Send cat.isJunk items into the list's existing  
      junk bucket.  
    Verified:  
    - In category (non-slot) list view, grey items bucket to "junk" by quality,  
      while player-marked items carry the Junk category index and bucketed to  
      that category's own section -- also labelled "Junk" -- giving two.  
    Did:  
    - Route cat.isJunk items into the existing "junk" bucket alongside grey items  
      (key = (d.\_lvQuality == 0 or cat.isJunk) and "junk" or ci). cat.isJunk is  
      nil when the feature is off, so behaviour is unchanged there.  
- Bags: Junk Marker -- never auto-sell a still-refundable item  
    Elle:  
    - SellJunk calls UseContainerItem on items that are still refundable, which  
      Blizzard's own right-click never does (it shows the refund confirmation  
      instead). Marks are per item ID and account-wide, so e.g. if you buy a  
      fresh copy of a marked item with currency, auto-sell sells it at the next  
      merchant without asking. Skip slots where  
      C\_Container.GetContainerItemPurchaseInfo(bag, slot, false) has refundSeconds.  
    Verified:  
    - NextJunkSlot returned any junk slot with vendor value, so a freshly bought  
      copy of a marked item (marks are account-wide per itemID) would sell on the  
      next sweep with no refund prompt -- unlike Blizzard's right-click.  
    Did:  
    - Added IsRefundable(bag, slot): GetContainerItemPurchaseInfo(bag, slot,  
      false).refundSeconds > 0. NextJunkSlot now skips refundable slots.  
      Capability- and type-guarded so a missing or odd-shaped return degrades to  
      "sellable" rather than erroring mid-sweep.  
- Bags: Junk Marker -- refresh the Junk category index each classify pass  
    Elle:  
    - \_junkCatIdx is only set in InitCategories, but ReorderCategory,  
      AddCustomCategory and RemoveCustomCategory change the list in place without  
      a rebuild. E.g. drag a category from above Junk to below it and every grey  
      item goes to the wrong category until a reload. Refresh it in ClassifyAll  
      each pass, the same way setCatIdx already is.  
    Verified:  
    - ClassifyItem routes grey items to self.\_junkCatIdx, but that index was only  
      computed in InitCategories; the in-place reorder/add/remove paths shift cats  
      without rebuilding it, so grey items landed in a stale index until /reload.  
      setCatIdx is rebuilt each ClassifyAll pass for exactly this reason.  
    Did:  
    - Rebuild \_junkCatIdx at the top of ClassifyAll, folded into the existing  
      unconditional counts-reset loop over cats (one pass, no extra traversal).  
      Stays nil when the feature is off.  
- Bags: Junk Marker -- make Junk the last default so saved order is stable  
    Elle:  
    - Junk is inserted before Miscellaneous, but bagVisualOrder (each category's  
      saved drag order) is keyed by category index. So turning the feature on or  
      off shifts Miscellaneous and everything after it by one, and they pick up  
      each other's saved orders. Make Junk the very last default with  
      appendLast = true, added after the Forever block so it lands after Special  
      Bags there too.  
    Verified:  
    - The Junk def sat between Housing and the Miscellaneous catch-all, so every  
      index at or below Junk shifted by one when the feature toggled, and  
      bagVisualOrder (index-keyed) then applied the wrong saved order.  
    Did:  
    - Removed the inline Junk def; append it as the last DEFAULT\_CATEGORIES entry  
      after the Forever block (so it lands after Special Bags, which is also  
      appendLast). InitCategories already routes appendLast defaults to the end  
      without disturbing any saved index, and the catch-all is found by its  
      isCatchAll flag, not position, so Junk after Miscellaneous is fine.  
- Bags: Junk Marker -- keep the full-screen dim for normal assign mode  
    Elle:  
    - The overlay change in EnterAssignSelectMode also hits the normal "+" assign  
      mode, so everyone gets the bag-only dim now (pin mode still dims the whole  
      screen). Keep the old overlay for normal assign mode and use the bag-only  
      one just for junk mode.  
    Verified:  
    - EnterAssignSelectMode built a single cached \_assignOverlay parented to the  
      bag window; normal "+" assign (DoAssign) and junk-select both reused it, so  
      the normal mode lost its long-standing full-viewport dim.  
    Did:  
    - Two overlays cached separately and picked per mode: full-screen (UIParent /  
      FULLSCREEN\_DIALOG, the pre-feature look) for normal assign, bag-only for  
      junk select. The active one is assigned to \_assignOverlay so Exit, the  
      fades and the catcher are untouched. EnterJunkSelectMode passes bagOnly.  
- Bags: Junk Marker -- only pay for it while it is enabled  
    Elle:  
    - The MERCHANT\_SHOW/MERCHANT\_CLOSED watcher registers at file load, so it  
      runs for everyone even with the feature off. Register it only while  
      bagJunkMarker is on. Same for the junkItems/diverted tables and the  
      RenderJunkSection/RenderJunkCatTop closures -- RenderGridView builds those  
      on every render even when it is off.  
    Verified:  
    - The watcher called RegisterEvent at parse time, so its OnEvent ran for all  
      players. In RenderGridView the OneBag/MultiBag block allocated junkItems +  
      diverted and defined RenderJunkSection every render, and the All Items  
      block scanned cats + defined RenderJunkCatTop every render, regardless of  
      the toggle.  
    Did:  
    - Watcher no longer registers at load; new EUI\_Bags:SyncJunkMerchantWatcher  
      registers/unregisters MERCHANT\_SHOW/CLOSED to match the toggle (and hides  
      the Sell Junk button when off). Called at header build (DB ready) and from  
      the options toggle.  
    - OneBag/MultiBag junk machinery (tables + RenderJunkSection + junkAtTop) is  
      built only inside "if pullJunk"; readers use "not (diverted and ...)" and  
      "if RenderJunkSection and not junkAtTop", matching the file's own  
      "not (special and special[...])" idiom.  
    - All Items junk-top scan + RenderJunkCatTop closure built only inside  
      "if IsJunkMarkerEnabled() and bagJunkAtTop". Zero tables/closures when off.  
- Bags: Junk Marker -- mark items as junk, group them, sell at a vendor  
    Off by default; master toggle 'Enable Junk Marker' in Bags > Extras.  
    - Junk category collects grey (Poor) items plus anything the player marks;  
      marks persist per itemID (account-wide), so future copies auto-classify.  
    - A coin button in the bag header enters a click-to-toggle select mode -- the  
      dim is scoped to the bag window so grid items stay bright; middle-click also  
      unmarks. Marked items desaturate and get a rounded coin badge whose corner  
      is pickable (TL/TR/BL/BR), lifted just outside the slot.  
    - The Junk category orders items by vendor value. 'Move Junk Category to Top'  
      renders it just below Pinned in All Items / OneBag / MultiBag; optional  
      per-view pull-out toggles for OneBag and MultiBag.  
    - A 'Sell Junk' button appears at merchants; optional auto-sell on open behind  
      a confirm popup. Selling is throttled (one item per tick), bails if the  
      merchant closes or combat starts, leaves no-value items in place, and can  
      silence its chat summary ('No Sale Summary Text').  
    Retail/other clients unaffected -- everything is gated on the feature toggle.  
