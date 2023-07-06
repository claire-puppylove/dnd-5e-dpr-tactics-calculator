# DnD 5e - DPR Tactics Calculator

*This tool set is for DnD 5e (Dungeons and Dragons 5th edition).*

DPR, or Damage per Round, is the basis for making sure you have a strong enough character to face the enemies that await you, or it could help you choose which new spells you get next level based on which are the most strategically sound.

There already are a few DnD 5e DPR calculators online already. In fact, mine was based off of the [DnD 5e – DPR Calculator by Tyler "RPGBOT" Kamstra](https://rpgbot.net/dnd5/tools/dpr-calculator/). RPGBOT's calculator is nice if you need to confirm something off the top of your head, or if you want to know just the DPR of a single round of attacks.

There is also a [Comprehensive DPR Calculator](https://forums.giantitp.com/showthread.php?582779-Comprehensive-DPR-Calculator-(v2-0)) with graphs showing efficiency and how strong a single attack is against different ACs, and many more functions.

However, I wanted to list all my different spells and attacks and even considered doing combinations of attacks across different rounds of combat. Say you spend a round casting a debuff spell, how much is that going to increase DPR in future rounds? Or perhaps you're up against an enemy with legendary resistances and you need to calculate how many attacks is it going to take you to do some good damage with saving throw spells, or perhaps you should stick to attack roll spells...

Oh! Not to mention, lots of DPR calculators assume that you're not casting spells that do damage on a miss.

So I made my own. I present to you the **DPR Tactics Calculator**.

## Requirements

### Excel (backward-compatible)

Currently the backward-compatible spreadsheet has a limitation of only one die reading per cell.

This should also work in Kingsoft's WPS Spreadsheet free version (tested 2019 version) or LibreOffice Calc (tested 7.5.4 version)

### Excel 365

The Microsoft 365 spreadsheet can read added dice notation (e.g. 1d8+1d4).

However if you don't have 365 the online free version should suffice.

## Documentation

I made note of all the formulas in an easier-to-read format.

- Excel (backward-compatible)
    + [DPR Calculator spreadsheet formulas](./excel_backwards_compatible/documentation/DPR%20Calculator%20spreadsheet%20formulas.md)
    + [Pivot Table spreadsheet formulas](./excel_backwards_compatible/documentation/Pivot%20Table%20spreadsheet%20formulas.md)
- Excel 365
    + [DPR Calculator spreadsheet formulas](./excel_365/documentation/DPR%20Calculator%20spreadsheet%20formulas.md)
    + [Pivot Table spreadsheet formulas](./excel_365/documentation/Pivot%20Table%20spreadsheet%20formulas.md)
    

## Terminology

### Tactics

In my calculator, a tactic is a series of rounds, and a round can have several rows detailing different attacks per row, or just multiply the same attack several times in the same row.

Therefore you got:

- Average Attack Damage: The expected damage for the row details
- Average Round Damage: The total expected damage for all the rows of the same round and tactic identifiers
- Average Tactic Damage: The total expected damage for all the rounds it took to perform a tactic
- Damage per Round: The average damage per round when dividing the tactic damage by the number of rounds

## Features

Since I was inspired by RPGBOT, most of the initial functions are the same, but I added some more.

### Manual input

- Row ID: Important for some other functions
- Tactic ID: It keeps the tactic intact and grouped together when doing calculations, and it's better if it's descriptive for the summary table later on.
- Character ID: It can be used as a filter in the summary table.
- Description
- Notes: just in case
- Player or Enemy: It influences the assumed relative modifiers.
- LV or CR: It influences the DPR rating at that level, and some relative modifiers.
- Round: Keep track of which rounds do more damage.
- Repeat Attacks: Use when you have several attacks per action with the exact same settings.
- Halfling Luck (Except Elven Accuracy and spells with saving throws)
    + *Halflings can't have Elven Accuracy, but if you select the option it just gives you the formula for halfling advantage*
    + *Halfling luck lets you reroll your d20s, but not your opponents.*
- Lucky feat: adds another die to the calculations! It can also be used for the Fortune's Favor spell.
- Current Attack is GWM Crit Bonus Attack: Great Weapon Master allows you to make an attack with a heavy weapon you're proficient with as a Bonus Action if you score a critical hit on your previous attack (or if you make an enemy reach 0 HP but we don't have a way to track that, just don't mark this as YES if you want to see how that is calculated).
    + The new probabilities get stacked with the previous crit percentage. (crit_n = crit_n-1\*crit; hit_n = crit_n-1\*hit...)
- Great Weapon Master -5 to hit for +10 dmg: You can toggle this and not worry about writing those specifics, they get added automatically to their respective columns.
- War Cleric Channel Divinity +10 to hit toggle
- Dice added to attack rolls: Useful for Bless or Bardic Inspiration.
- Optional relative modifiers to hit: For adding that +1 magic sword, or perhaps for characters with very good stats for their level, or perhaps -1 for stats under the expectations. This has to be something that's added from different sources than the automatic modifiers calculated above.
- Min to Crit: Some abilities like the Champion Fighter Improved Critical, or the Hexblade Warlock Hexblade's Curse let you crit on lower than 20.
- Accuracy types: We got formulas for days
    + Standard
    + Advantage
    + Disadvantage
    + Elven Accuracy
    + Spells with no roll (like Magic Missile, Mental Prison, healing spells)
    + Saving throw spells
    + Saving throw spells with half damage on miss
    + Saving throw spells when the enemy is disadvantaged
    + Saving throw spells with enemy disadvantage that do half damage on miss
    + Saving throw spells, but the enemy has legendary resistances
    + Saving throw spells with half damage on miss, considering legendary resistances
    + Saving throw spells when the enemy is disadvantaged, considering legendary resistances
    + Saving throw spells with enemy disadvantage that do half damage on miss, considering legendary resistances
- *Enemy Paralyzed close-range auto-crit on hit*: This toggle makes all hits critical hits. The calculator will treat the accuracy the same, so be sure that it's set on advantage unless you have a reason not to.
- Area of Effect: will affect the expected number of targets on calculation
- *Great Weapon Fighting Style* toggle: Since this ability lets you reroll some of the damage dice, I added the expected damage. (Note: it's (1-(2/DieSize)) per die)
- Dice that you only roll on crits and get to multiply (like smites)
- Dice that you only roll on crits but you don't multiply (like Savage Attacks)
- Dice that can't be multiplied even if you crit (like Booming Blade)
- Bonus flat damage: Put your damage modifiers here, except for the GWM+10 that gets auto-added on another column
- **Retaliatory damage dice and flat bonus** for when getting hit while you have Fire Shield cast. Flat damage only is also possible for Armor of Agathys and such.
    + Needs your AC
    + Declare an expected Enemy Accuracy.
- **Healing dice and flat bonus**: For spells that do healing (either only healing or also damage), they get a section for expected healing for the average player.
    + Note: to heal half the dice such as in Vampiric Touch, you write half the dice notation (e.g. 5d6 -> 2.5d6) and the calculations are done normally.
- *Elemental Adept Feat* options: If the damage type matches, all your ones turn into twos. You get an extra 1/DieSize average damage per die.
    + Damage types for all your dice (for the elemental adept feat, or your own satisfaction)

### Calculations

- **Legendary resistances counter**: Starting at 3 but manually modifiable, if your tactics depend on getting those resistances down, this calculator lets you check which way to spend those spell slots is better
- Assumed relative modifiers to hit: When you level up and the enemy levels up, how much are your chances actually increasing? This field updates automatically as you set your character's level. Additionally, it gets a -5 automatically if you use the GWM toggle described above, or +10 if the War Cleric toggle is active, and also it adds the dice added to the attack rolls by Bless or Bardic inspiration, but only if it's an attack roll (not added on save throw spells).
- Min to Hit: After all is said and done, this automatically reflects how much you have to roll on your 1d20 to hit the target at this level.
- Display of probabilities:
    + Critical Hit % chance
    + Hit % chance (including crits)
    + Hit % chance (NOT including crits)
    + Miss % chance
- Area of Effect Expected Targets: Based on the DMG 'Adjudicating Areas of Effect' 249
    + Area type
    + Size
    + Expected Targets
        * Cone            : Size   / 10 (round up)
        * Cube or square  : Size   / 5  (round up)
        * Cylinder        : Radius / 5  (round up)
        * Line            : Length / 30 (round up)
        * Sphere or circle: Radius / 5  (round up)
- Great Weapon Master Bonus Dmg: This becomes +10 automatically and added to the calculations if you activated the feature earlier.
- Dice average damage for all types of dice (so the mid-calculation steps are visible)
- Average Damage Added on Crit: so you can know exactly how much a crit helps you
- Average Dice Damage on Hit
- Average Resulting Crit Dice Damage: check it so you can have hopes and dreams
- Average Damage on Miss: useful for those half damage spells
    + **Save throw spells miss damage**: This function I didn't see in many other places. My calculator outputs the average damage on a miss and multiplies it by the probability of a miss, and adds it to the damage calculations.

Healing results:

- Basically all the same categories as Damage results.
- **HPR HP% Rating**: Instead of a 5 star rating system, I opted to calculate player HP % considering the Max HP of the average player at that level.
    + Here, the average player is average of all Hit Die (6+8+10+12)/4=?d9 and assuming a +2 CON stat, at a given player level.

Damage results:

- Damage per Attack: confirm before counting repetitions or multiple AOE targets.
- Average Resulting Damage per Target: after attack repetitions.
- Average Resulting Damage Total: after counting in multiple targets.
- Average Round Damage per Target: The expected damage per target for all the rows of the same round and tactic identifiers
- Average Round Damage Total: The total expected damage for the round
- Average Tactic Damage per Target: The expected damage per target for all the rounds it took to perform a tactic
- Average Tactic Damage Total: The total expected damage for the tactic
- Damage per Round per Target: The average damage per round per target when dividing the tactic damage per target by the number of rounds
- Total Damage per Round: The resulting DPR including all targets.
- DPR per Target Rating: Thanks to RPGBOT's [DnD 5e – The Fundamental Math of Character Optimization](https://rpgbot.net/dnd5/characters/fundamental_math/), I got some data as to how to make an automatic rating calculation. You get the DPR and check the level, and trusting the words of this article, you can know how well you stand instantly in my calculator. An additional "No damage" rating helps to not get a bad rating if you are actually casting debuffs and other useful non-damaging spells, or perhaps using the Help action, or even Healing yourself or others. You could technically input other player's actions in the tactic as well to up the rating if you would prefer to think about it that way.
    + Formula: Considering the Max Enemy HP at that CR, 3 rounds for OP, 6 rounds for high, 12 rounds for target, 24 rounds for low, and anything higher is not helping.
    + Ratings:
        * Rating 0: ☆☆☆☆  No damage
        * Rating 1: ★☆☆☆  Lowest (not helping)
        * Rating 2: ★★☆☆  Low (support, control, debuff)
        * Rating 3: ★★★☆  Target (expected)
        * Rating 4: ★★★★  High (heavy hitter)
        * Rating 5: 🕱🕱🕱🕱🕱  Deadly
- Total DPR Rating: This is an experimental field. With AOE attacks, the more enemies the more powerful the spell is, but any spell with more than 3 targets ends up rating as Deadly with the current math. I am not sure if this is a correct interpretation, or if I should modify the math to account for AOE...

## Future work

In the future, I plan to make a python version that takes CSV files as input to be free of proprietary software.

## References

- [DnD 5e - Damage per Round | RPGBOT](https://rpgbot.net/dnd5/characters/damage-per-round/): Most of the formulas I obtained from this guide. However, RPGBOT's calculator uses Min to Hit instead of modifiers against enemy AC. I did some calculations of my own to figure out the formula used there.
- [DnD 5e – DPR Calculator | RPGBOT](https://rpgbot.net/dnd5/tools/dpr-calculator/): I actually got my inspiration for this project by using this calculator and realizing I had bigger plans than I could type in my cellphone.
- [DnD 5e – The Fundamental Math of Character Optimization | RPGBOT](https://rpgbot.net/dnd5/characters/fundamental_math/): Helped me establish the DPR rating calculations.
- [Comprehensive DPR Calculator | LudicSavant](https://forums.giantitp.com/showthread.php?582779-Comprehensive-DPR-Calculator-(v2-0)) and its [documentation](https://docs.google.com/document/u/1/d/11eTMZPPxWXHY0rQEhK1msO-40BcCGrzArSl4GX4CiJE/edit?pli=1) helped me figure out the formulas for Halfling luck and for Elven Accuracy. It also made me realize I need to add the Great Weapon Master feat to the calculations as an option.
- Dungeon Master's Guide for AOE expected values and expected enemy HP
- [This guide with all ways to increase to hit bonus | u/wateryoshi on Reddit](https://www.reddit.com/r/dndnext/comments/km4i7x/heres_every_way_to_increase_your_to_hit_attack/)