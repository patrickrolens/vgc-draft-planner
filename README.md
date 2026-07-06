# VGC Draft Planner

This is the repository for a webapp hosted at https://planner.springfieldbattleleague.com . This webapp is intended to replace the [old VGC Draft Planner that was hosted in Google Sheets]( https://docs.google.com/spreadsheets/d/1P0O6COVcBJ_CYNqKhby2YqCXH2W4k_IsHoJ575RohQ4/edit?usp=sharing). This document will outline the features of the webapp.

The README is currently under construction but should match the current version of the webapp.

---

## Overview

The Planner is a single-page webapp built using HTML, CSS, and JavaScript. It uses the **pokemon.json** file as the main reference for various key pieces of data. The information in this file has been constructed from the Index tab of the Google Sheets version, which was itself built using resources from Serebii and Pokemon Showdown. Updates to information such as new movesets, new Pokemon stats, or any other changes will come from these resources as well.

This section will outline the different views broadly and then break them down in more detail in their own subsections.

The Planner features several different views which present the information from **pokemon.json** in different ways and provide interactive features for users to plan out their own drafts.
- **Draft Builder** - 
- **Matchup** - This view allows the user to take two saved drafts and directly compare them. The information in each card is simplified compared to the Draft Builder view in order to fit everything on a standard desktop screen. *note
- **Pokédex** - This is a simple table that displays all of the Pokemon available along with most of the attributes for each Pokemon stored in **pokemon.json**. The table provides a variety of filters and a search feature to make looking up information as easy as possible.
- **Draft Pool** - This view displays all available Pokemon based on the current point values loaded into **pokemon.json**. This will typically reflect the most recent season of the Springfield Battle League. Selecting a Pokemon here will add it to the user's currently selected draft in the Draft Builder view unless the team is full.


### Draft Builder View
This is the main view and the landing page of the app. This view can be populated using the search bar feature, and the side panels will automatically update to reflect the user's selected draft.

#### The Draft Panel
The center of the three-panel structure is the main interactive feature of the entire webapp.
- It features a **search bar** that can be used to look up and add Pokemon to the user's draft.
- It also features a **random button** that can be clicked to add a randomly selected Pokemon to the user's draft.
- The **Legal only** toggle will allow/restrict illegal Pokemon in search results or random selections. This allows users to de-clutter search results if they are only interested in Pokemon that are legally available for the current SBL season.
- **Import/Export** can be used to import Pokemon from Pokepaste or export the current draft in the Pokepaste format.
  - Pokemon imported from Pokepaste will have their unused attributes (EVs, nicknames, held items, etc.) preserved in the import/export feature, making jumping back and forth between Pokemon Showdown and the Planner as seamless as possible
  - *A helpful note for anyone trying to import from a Pokepaste link:* Add "/raw" to the end of a Pokepaste url to get an easily copy/paste-able format
- Users can also **Save/Load** teams. Note that this uses LocalStorage, so teams are saved to the user's device in their browser's files. Incognito/Private mode browsing or deleting temporary internet files will make saved teams disappear. There is no cross-device saving, so users will need to manage this themselves
- **Clear All** will remove all currently selected Pokemon from the user's draft.

The Draft Panel contains 10 slots where informational cards for the user's drafted Pokemon will populate. These cards have a great deal of information and interactive features packed into them:
- An empty card features a clickable interface to **Add new Pokemon** which pops up a searchbar in-line that the user can use just as they would use the main search bar at the top of the Draft Panel.
- Empty cards also have the slot number and a grabbable **handle** which can be used to rearrange slots.
- **Point Value** - displayed in the top-left corner of the Pokemon's sprite. This reflects the point value in the current instance of **pokemon.json**, which usually corresponds to the most recent season of The Springfield Battle League.
- **Sprite** - Sourced originally from Serebii but hosted in-repository, the sprites are usually the Pokemon Home sprite for the selected Pokemon. If a sprite is unavailable, an SVG Pokeball will serve as a placeholder.
- **Name**
- **Alt/Base Form Toggle** - If the Pokemon has an alternate form that is not separately draftable or if the Pokemon is a Mega form, there is a toggle that allows the user to view the stats for the alternate form. A base-form Pokemon that has a Mega form does not receive this toggle since base forms and mega forms are separately draftable Pokemon. A user who has a Mega form drafted might need to see the base form's stats, but the Mega form's stats are not relevant to a user who drafted a base form.
- **Type(s)**
- **Ability(ies)** - Features a clickable tooltip with Ability descriptions from Pokemon Showdown's database
- **Base Stats** - Features special formatting based on the Pokemon's speed tier.
- **Type Matchups** - Unfavorable and favorable defensive type matchups are displayed with triangle symbols to denote weakness or resistance. They are also color-coded. *Note that abilities are taken into account for the displayed type matchups.*
- **Utility Move** - A list of useful moves, color-coded to various broad categories. This list contains an internal priority system, so some of the more common/broadly useful moves are typically prioritized toward the top of the list.
- **Base Stat Chart** - A Radar Chart that can be toggled to a simple bar chart which visually represents the Pokemon's base stats.
- There is a delete button in the top-right corner of each card to remove each Pokemon as well as an undo button in case the user mistakenly deletes a Pokemon.

#### Points Panel
The first panel in the left sidebar, the Points Panel provides a quick sum of the selected draft's point value. The default limit will typically match whatever the most recent season of SBL used, but the user can adjust the limit themselves as desired. The point limit does not prevent the user from drafting over the limit. It only provides a visual for the user to judge their draft against the point limit.

#### Type Matchups Panel
The second panel the in left sidebar, the Type Matchups Panel is a quick summary of the selected draft's defensive type matchups. Types are displayed in a 3X6 grid with weaknesses as the left side of the ratio and resistances and immunities combined into the right side of the ratio. Hovering over a type highlights the cards in the user's Draft Panel to quickly visualize how the user's draft matches up with various types. Clicking on a type will pin it, allowing the user to scroll through the draft freely while keeping the highlighting for that type matchup in place.

The Type Matchups Panel can also be expanded with the arrow in the top right corner to shift the visual into a ranked list of types the user's draft is strong against, neutral against, or weak against.

#### Speed Tiers Panel
The third panel in the left sidebar, the Speed Tiers Panel presents a quick visualization of the team's distribution of speed stats. This panel uses the same color-coding seen in the Draft Panel's cards to make identifying speed tiers visually easy.

#### Utility Coverage Panel
The only panel in the right sidebar, the Utility Moves Panel contains a long list of moves identified as having supportive value on a team. They range from near-universally useful moves like Tailwind, Trick Room, Follow Me, and Fake Out to niche tech moves like Speed Swap and Roar. Users can toggle individual moves or whole categories on/off to change whether or not they display in the Draft Panel's cards. This panel is useful for easily seeing what supportive capabilities a given draft has available to it.


### Matchup View
This view provides a more compact view of a selected draft and compares it to an opposing draft. This view can be used to quickly assess how each team's offensive and defensive type matchups compare as well as a ranking of speed stats.

#### Team Displays
Each team is displayed compactly such that an entire draft of 10 Pokemon can be displayed in the average 1920x1080 display.

Each team has the same set of options available at the top of each list:
- **Team Name** - This automatically populates as "Team A" and "Team B" but is customizable by the user. If the user loaded a saved team, then that team name will be displayed here by default instead.
- **Add** - Allows the user to manually add Pokemon individually. This can also be accomplished with the in-line Add Pokemon option in a blank card.
- **Import** - Takes Pokepaste formatted Pokemon for easy data intake
- **Load** - Loads teams from the users saved drafts created in the Draft Builder.

Each Pokemon's compact info card is displayed below. The cards feature the following information and features:
- **Sprite**
- **Name**
- **Type(s)**
- **Speed** - Unlike elsewhere in the app, this is the actual calculated speed stat of the Pokemon at Level 50, not their base stat. This is important for the key speed comparison feature of this view. Speed stats can be modified by clicking them. The app calculates four main speed benchmarks:
  * **Max+** - The maximum EV investment plus a boosting nature
  * **Max** - The maxumum EV investment with a neutral nature
  * **0** - No EV investment with a neutral nature
  * **Min** - No EV investment with a negative nature
- **Ability(ies)** - Features a clickable tooltip with Ability descriptions from Pokemon Showdown's database
- **Coverage** - This feature brings up a selector where the user can assign the Pokemon offensive type coverage. By default, the Pokemon's type(s) are selected, but the user can freely select and deselect as they please. This information is used in the Type Matchup Matrix Panel
- There is a delete button in the top-right corner of each card to remove each Pokemon as well as an undo button in case the user mistakenly deletes a Pokemon.
- Additionally, clicking on a Pokemon's card will highlight it in yellow and add it to the team's side in the Lead Matchup Panel. This selection can be canceled by clicking on a highlighted card. Selection follows First-In, First-Out rules, so selecting a third Pokemon will deselect the first Pokemon selected.

#### Lead Matchup Panel
This panel uses the selected lead Pokemon (selected by the user by way of the card-clicking interaction mentioned above) to provide a simple visualization of the matchup between two leads. The display ranks the four Pokemon in the order in which they will act based on their computed speed stats (priority rules or other abilities are not accounted for). It also provides special badges for Pokemon that possess the following moves:
* Fake Out
* Tailwind
* Trick Room
* Redirection moves (Follow Me, Rage Powder, Ally Switch)
Hovering over a Pokemon will add highlighting to the opposing team to reflect if the selected Pokemon has a super effective type based on its currently selected coverage types.

#### Speed Ladder Panel
This panel shows a vertical descending display of each team's speed stats based on their selected speed benchmarks and modifiers. The Pokemon can be selected in the ladder and modified just as they can be in their info cards. There are Team Speeds selectors on each side of the panel allowing the user to quickly modify all selected Pokemon's speed stats uniformly. This can be useful for finding theoretical maximum and minimum speeds of a team or find comparisons versus a team with a team-wide speed modifier like Tailwind active.

#### Type Matchup Matrix Panel
This panel displays the attacking team's type coverage and the defending team's defensive matchup against those types.

If a type panel is lit up, that indicates that at least one of the attacker's Pokemon possesses coverage in that type. If the panel is dimmed, then no Pokemon on the attacker's team have coverage in that type.

The numerical values in each panel show the ratio of weaknesses to the number of resistances and immunities on a team.

Hovering over a type in the Matrix also highlights the defending team's Pokemon based on their defensive interaction with that type, similar to the Type Matchup highlighting in the Draft Builder view.

The user can swap which team is the attacker and which is the defender by clicking the "Attacker" and "Defender" buttons in the top corners of the panel. This immediately flips the roles of each team and updates the display accordingly.

--- UNDER CONSTRUCTION ---
