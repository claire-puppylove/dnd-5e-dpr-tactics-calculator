# DPR Calculator formulas

<!-- MarkdownTOC -->

- [Notation](#notation)
    - [Minimum to Hit \(Ah\)](#minimum-to-hit-ah)
    - [Minimum to Crit \(Ac\)](#minimum-to-crit-ac)
    - [Accuracy](#accuracy)
    - [Dice rolls](#dice-rolls)
- [Modifiers](#modifiers)
    - [Halfling](#halfling)
    - [Current Attack is GWM Crit Bonus Attack](#current-attack-is-gwm-crit-bonus-attack)
    - [Great Weapon Fighting Style](#great-weapon-fighting-style)
    - [Feat: Elemental Adept Element](#feat-elemental-adept-element)
    - [Legendary resistances](#legendary-resistances)
    - [Assumed Relative Modifier to Hit](#assumed-relative-modifier-to-hit)
    - [Add Relative Modifier to Hit](#add-relative-modifier-to-hit)
    - [Min to Hit](#min-to-hit)
    - [Min to Crit](#min-to-crit)
- [Accuracies](#accuracies)
    - [Crit Percentage](#crit-percentage)
    - [Hit Percentage \(includes crit\)](#hit-percentage-includes-crit)
    - [Hit percentage \(not including crit\)](#hit-percentage-not-including-crit)
    - [Miss chance](#miss-chance)
- [Targets and Repeated Attacks](#targets-and-repeated-attacks)
    - [Repeated Attacks](#repeated-attacks)
    - [Area of Effect Expected Targets](#area-of-effect-expected-targets)
- [Dice notation reader](#dice-notation-reader)
    - [Splitting the dice notation \(e.g. 1d8\)](#splitting-the-dice-notation-eg-1d8)
    - [Average Damage for base damage dice \(single dice notation (e.g. 1d8\))](#average-damage-for-base-damage-dice-single-dice-notation-eg-1d8)
    - [Average Damage for other damage dice \(single dice notation (e.g. 1d8\))](#average-damage-for-other-damage-dice-single-dice-notation-eg-1d8)
    - [Average Damage for added dice notation \(e.g. 1d8+2d4+...\)](#average-damage-for-added-dice-notation-eg-1d82d4)
- [Bonus Damages](#bonus-damages)
    - [Bonus Flat Dmg](#bonus-flat-dmg)
    - [Great Weapon Master -5 to hit for 10 damage](#great-weapon-master--5-to-hit-for-10-damage)
- [Resulting Damages](#resulting-damages)
    - [Midpoint calculations](#midpoint-calculations)
        - [Dc: Damage added on crit \(all applicable\)](#dc-damage-added-on-crit-all-applicable)
        - [D: Damage on hit \(all applicable\)](#d-damage-on-hit-all-applicable)
        - [Dcr: Damage result on crit](#dcr-damage-result-on-crit)
        - [Dm: Damage on miss \(all applicable\) \(spells with 1/2 dmg only\)](#dm-damage-on-miss-all-applicable-spells-with-12-dmg-only)
    - [Damage Aggregates](#damage-aggregates)
        - [DPA: Damage per Attack \(uses DPR formula\)](#dpa-damage-per-attack-uses-dpr-formula)
        - [Average Resulting Damage per target \(for the row\)](#average-resulting-damage-per-target-for-the-row)
        - [Average Resulting Damage Total](#average-resulting-damage-total)
        - [Average Round Damage per Target](#average-round-damage-per-target)
        - [Average Round Damage Total](#average-round-damage-total)
        - [Average Tactic Damage per Target](#average-tactic-damage-per-target)
        - [Average Tactic Damage Total](#average-tactic-damage-total)
        - [Damage per Round per Target](#damage-per-round-per-target)
        - [Total Damage per Round](#total-damage-per-round)
- [DPR Rating](#dpr-rating)
    - [Max Expected Enemy HP at CR or LV](#max-expected-enemy-hp-at-cr-or-lv)
    - [DPR Rating Formula](#dpr-rating-formula)
    - [DPR rating flavor text](#dpr-rating-flavor-text)
- [Damage Type shortened](#damage-type-shortened)

<!-- /MarkdownTOC -->


<a id="notation"></a>
## Notation

<a id="minimum-to-hit-ah"></a>
### Minimum to Hit (Ah)
Assume 8, 
lower for modifiers higher than assumed
higher for modifiers lower than assumed
for saving throws, the math is the same

Assume modifier:
LV1-3: +3
LV4-7: +4
LV8-20:+5

*\* Ah is for attack roll to hit*

<a id="minimum-to-crit-ac"></a>
### Minimum to Crit (Ac)

Assume 20, w/exceptions
Such as
Hexblade's Curse, Champion Fighter Improved Critical, etc.

*\* Ac is for attack roll to crit*

<a id="accuracy"></a>
### Accuracy

STANDARD,
ADVANTAGE,
DISADVANTAGE, 
ELVEN ACCURACY (Adds third d20 to advantage rolls),
SPELL (SAVE DC),
SP DC MISS 1/2 DMG,
SP DC ENEMY DVG,
SP DC M 1/2 ENEMY DVG,
SP DC LEGEND RESIST,
SP DC M 1/2 LEG RESIST,
SP DC EN DVG LEG RES,
SP DC M 1/2 EN DVG L-RES

<a id="dice-rolls"></a>
### Dice rolls

Dice notation, but haven't figured out how to do multiple dice in one cell.

- Dr: damage roll on hit (crit multiplies)
- Db: bonus damage on hit (crit multiplies)
- Dbn: more bonus damage if necessary (Db+Dbn1+Dbn2...)
- Dxc: damage exclusive to crit (crit multiplies)
- Dxcn: bonus damage exclusive to crit that's not multiplied
- Dn: bonus damage that's not multiplied
- B: flat bonus damage (modifiers and such, not multiplied)

Possibly add columns between S and T (Db+Dbn1+Dbn2...)
If so, add columns between AL and AM.

<a id="modifiers"></a>
## Modifiers

<a id="halfling"></a>
### Halfling

Lucky attribute
rerolls one die when d20=1

Since Elven Accuracy is impossible, it just returns regular advantage
No effect on SPELL DC attaks.

<a id="current-attack-is-gwm-crit-bonus-attack"></a>
### Current Attack is GWM Crit Bonus Attack

I put a marker for if the current attack is the Great Weapon Master feat bonus attack on a crit. The hit and crit probabilities get multiplied by the previous crit chance.

<a id="great-weapon-fighting-style"></a>
### Great Weapon Fighting Style

This increases the base damage dice

Formula: ...+NumDice*(avg(1,DieSize)) + NumDice*(1-(2/DieSize))

<a id="feat-elemental-adept-element"></a>
### Feat: Elemental Adept Element

This increases each damage dice if the damage types match

Formula: ...+NumDice*(1/DieSize)

<a id="legendary-resistances"></a>
### Legendary resistances

I put a counter for legendary resistances.

- Only establish if using SPELL DC LEGENDARY RESISTANCE accuracy.
- When SP DC LEG RES is used, search for the MIN value in the column that matches the tactic ID, if none exist, start at 3.
- Otherwise "-"

```
[@[Legendary Resistances Left]]
=IF(
    OR(
        [@[Accuracy]]="SP DC LEGEND RESIST",
        [@[Accuracy]]="SP DC M 1/2 LEG RESIST",
        [@[Accuracy]]="SP DC EN DVG LEG RES",
        [@[Accuracy]]="SP DC M 1/2 EN DVG L-RES"
    ),
    (
        N("if leg res is used, check if previous others were cast for this tactic")+
        IF(
            SUMPRODUCT(
                (
                    [Accuracy]=
                    {
                        "SP DC LEGEND RESIST",
                        "SP DC M 1/2 LEG RESIST",
                        "SP DC EN DVG LEG RES",
                        "SP DC M 1/2 EN DVG L-RES"
                    }
                )
                *
                ([Tactic ID]=[@[Tactic ID]])
                *
                ([Row]<[@[Row]])
            )>0,
            (
                N("if so, search for the MIN for this tactic")+
                N("And subtract the last one used.")+
                IF(
                    MINIFS(
                        [Legendary Resistances Left],
                        [Tactic ID],[@[Tactic ID]],
                        [Row],"<"&[@[Row]]
                    )>0,
                    MINIFS(
                        [Legendary Resistances Left],
                        [Tactic ID],[@[Tactic ID]],
                        [Row],"<"&[@[Row]]
                    )-1,
                    N("Unless there's 0 already")+0
                )
            ),
            (N("else, start at 3 resistances")+3)
        )
    ),
    "-"
)
```


<a id="assumed-relative-modifier-to-hit"></a>
### Assumed Relative Modifier to Hit

You level up and get better stats, and the enemies also get better stats. The game by design assumes these stay on par with each other.

Say STR, DEX, INT, CHA, whichever is your main stat, this field assumes you start with at least a 16 in the relevant Ability Score.

Assume modifier:
LV1-3: +3
LV4-7: +4
LV8-20:+5

The enemy AC also has +3, +4 and +5 around the same time, so this relative modifier stays at 0.

For LV9 PCs, proficiency bonus increases but monster AC doesn't, so you get a relative +1 to hit.

PC AC doesn’t change much so I won’t add any rules for Enemy Characters at CR9

```
=IFS(
    AND([@[Player or Enemy]]="PC",[@[LV or CR]]=9),
    1,
    1,
    0
)
+
IF(
    [@[Great Weapon Master -5 to hit for +10 dmg]]="YES",
    (-5),
    0
)
```

<a id="add-relative-modifier-to-hit"></a>
### Add Relative Modifier to Hit

If you have a +1 to hit from a magic weapon or ability, or you leveled up faster than assumed, add those modifiers here.

Assumed modifiers:
LV1-3: +3
LV4-7: +4
LV8-20: +5

If say, for example, you start with 18 STR on LV1 because you rolled very well on your new fighter character, add +1 to hit.

If you are not using your main stat (say you make a ranged attack but your DEX is 14 at LV1, make this a -1 to hit.


<a id="min-to-hit"></a>
### Min to Hit

```
[@[Min to Hit]]
=N("Assuming a balanced value of 8, and subtract relative modifiers.")+
(
    8
    -
    IF(
        ISNUMBER([@[Assumed Relative Modifier to Hit]]),
        [@[Assumed Relative Modifier to Hit]],
        0
    )
    -
    IF(
        ISNUMBER([@[Add Relative Modifier to Hit]]),
        [@[Add Relative Modifier to Hit]],
        0
    )
)
```

<a id="min-to-crit"></a>
### Min to Crit

Usually 20, but I added an option for 19 or 18 for abilities like the Champion Fighter Improved Critical and Hexblade Warlock Hexblade's Curse.

<a id="accuracies"></a>
## Accuracies

<a id="crit-percentage"></a>
### Crit Percentage

Formulas

- Standard:              `C=1-(Ac-1)/20`
- Advantage:             `Cadv=1-(1-C)^2`
- Disadvantage:          `Cdvg=C^2`
- Elven Accuracy:        `Celv=1-(1-C)^3`
- Standard Halfling:     `Ch=C+(C/20)`
- Advantage Halfling:    `Chadv=Cadv+((2*(1-C)/20)-(1/400))*C`
- Disadvantage Halfling: `Chdvg=Cdvg+(2/20)*(C^2)`
- Spell DC:              `Csp=0`

```
[@[Crit % Chance]]
=IFS(
    AND([@[Accuracy]]="STANDARD",[@[Halfling]]="NO"),
        (N("Standard crit formula")+
        (1-([@[Min to Crit]]-1)/20)),
    AND([@[Accuracy]]="STANDARD",[@[Halfling]]="YES"),
        (N("Halfling standard crit formula")+
        ((1-([@[Min to Crit]]-1)/20)+((1-([@[Min to Crit]]-1)/20)/20))),
    AND([@[Accuracy]]="ADVANTAGE",[@[Halfling]]="NO"),
        (N("Advantage crit formula")+
        (1-(1-(1-([@[Min to Crit]]-1)/20))^2)),
    AND([@[Accuracy]]="ADVANTAGE",[@[Halfling]]="YES"),
        (
            N("Halfling advantage crit formula")+
            (
                (1-(1-(1-(([@[Min to Crit]]-1)/20)))^2)
                +
                ((2*(1-(1-(([@[Min to Crit]]-1)/20)))/20)-(1/400))
                *
                (1-(([@[Min to Crit]]-1)/20))
            )
        ),
    AND([@[Accuracy]]="DISADVANTAGE",[@[Halfling]]="NO"),
        (N("Disadvantage crit formula")+
        ((1-([@[Min to Crit]]-1)/20)^2)),
    AND([@[Accuracy]]="DISADVANTAGE",[@[Halfling]]="YES"),
        (
            N("Halfling disadvantage crit formula")+
            (
                ((1-([@[Min to Crit]]-1)/20)^2)+(2/20)
                *
                ((1-([@[Min to Crit]]-1)/20)^2)
            )
        ),
    AND([@[Accuracy]]="ELVEN ACCURACY",[@[Halfling]]="NO"),
        (N("Elven accuracy crit formula")+
        (1-(1-((21-[@[Min to Crit]])/20))^2)),
    AND([@[Accuracy]]="ELVEN ACCURACY",[@[Halfling]]="YES"),
        (
            N("Halfling advantage crit formula")+
            N("No halfling can have Elven Accuracy")+
            (
                (1-(1-(1-(([@[Min to Crit]]-1)/20)))^2)
                +
                ((2*(1-(1-(([@[Min to Crit]]-1)/20)))/20)-(1/400))
                *
                (1-(([@[Min to Crit]]-1)/20))
            )
        ),
    OR(
        [@[Accuracy]]="SPELL (SAVE DC)",
        [@[Accuracy]]="SP DC MISS 1/2 DMG",
        [@[Accuracy]]="SP DC ENEMY DVG",
        [@[Accuracy]]="SP DC M 1/2 ENEMY DVG",
        [@[Accuracy]]="SP DC LEGEND RESIST",
        [@[Accuracy]]="SP DC M 1/2 LEG RESIST",
        [@[Accuracy]]="SP DC EN DVG LEG RES",
        [@[Accuracy]]="SP DC M 1/2 EN DVG L-RES"
    ),
        (N("No crits for save throw spells")+0)
)
*
IF(
    AND(
        [@[Current Attack is GWM Crit Bonus Attack]]="YES",
        OR(
            [@[Accuracy]]="STANDARD",
            [@[Accuracy]]="ADVANTAGE",
            [@[Accuracy]]="DISADVANTAGE",
            [@[Accuracy]]="ELVEN ACCURACY",
        )
    ),
    (
        N("If the current attack is a Great Weapon Master crit bonus attack")+
        N("And it's not a spell attack")+
        N("The chances of this attack are multiplied by the crit chance of the previous attack of the same round and tactic")+
        SUMIFS(
            [Crit % Chance],
            [Tactic ID],[@[Tactic ID]],
            [Round],[@[Round]],
            [Row],[@[Row]]-1
        )
    ),
    (
        N("Else they just stay the same")+
        1
    )
)
```

<a id="hit-percentage-includes-crit"></a>
### Hit Percentage (includes crit)

Formulas:

- Standard:                      `H=1-((Ah-1)/20)`
- Advantage:                     `Hadv=1-(1-H)^2`
- Disadvantage:                  `Hdvg=H^2`
- Elven Accuracy:                `Helv=1-(1-H)^3`
- Standard Halfling:             `Hh=H+(H/20)`
- Advantage Halfling:            `Hhadv=Hadv+((2*(1-H)/20)-(1/400))*H`
- Disadvantage Halfling:         `Hhdvg=Hdvg+(2/20)*(H^2)`
- Spell DC:                      `Hsp=1-((Ah-1)/20)`
- Spell DC enemy disadvantage:   `Hspadv=1-(1-H)^2`
- Spell DC Legendary resistance: `Hsplr=0 (count 3)`


```
[@[Hit % Chance]]
=IFS(
    AND([@[Accuracy]]="STANDARD",[@[Halfling]]="NO"),
        (N("Standard hit formula")+
        (1-(([@[Min to Hit]]-1)/20))),
    AND([@[Accuracy]]="STANDARD",[@[Halfling]]="YES"),
        (N("Halfling standard hit formula")+
        ((1-(([@[Min to Hit]]-1)/20))+((1-(([@[Min to Hit]]-1)/20))/20))),
    AND([@[Accuracy]]="ADVANTAGE",[@[Halfling]]="NO"),
        (N("Advantage hit formula")+
        (1-(1-(1-(([@[Min to Hit]]-1)/20)))^2)),
    AND([@[Accuracy]]="ADVANTAGE",[@[Halfling]]="YES"),
        (
            N("Halfling advantage hit formula")+
            (
                (1-(1-(1-(([@[Min to Hit]]-1)/20)))^2)
                +
                ((2*(1-(1-(([@[Min to Hit]]-1)/20)))/20)-(1/400))
                *
                (1-(([@[Min to Hit]]-1)/20))
            )
        ),
    AND([@[Accuracy]]="DISADVANTAGE",[@[Halfling]]="NO"),
        (N("Disadvantage hit formula")+
        ((1-(([@[Min to Hit]]-1)/20))^2)),
    AND([@[Accuracy]]="DISADVANTAGE",[@[Halfling]]="YES"),
        (
            N("Halfling disadvantage hit formula")+
            (
                ((1-([@[Min to Hit]]-1)/20)^2)+(2/20)
                *
                ((1-([@[Min to Hit]]-1)/20)^2)
            )
        ),
    AND([@[Accuracy]]="ELVEN ACCURACY",[@[Halfling]]="NO"),
        (N("Elven accuracy hit formula")+
        (1-(1-(1-(([@[Min to Hit]]-1)/20)))^2)),
    AND([@[Accuracy]]="ELVEN ACCURACY",[@[Halfling]]="YES"),
        (
            N("Halfling advantage hit formula")+
            N("No halfling can have Elven Accuracy")+
            (
                (1-(1-(1-(([@[Min to Hit]]-1)/20)))^2)
                +
                ((2*(1-(1-(([@[Min to Hit]]-1)/20)))/20)-(1/400))
                *
                (1-(([@[Min to Hit]]-1)/20))
            )
        ),
    OR(
        [@[Accuracy]]="SPELL (SAVE DC)",
        [@[Accuracy]]="SP DC MISS 1/2 DMG"
    ),
        (N("standard hit formula for spells")+
        (1-(([@[Min to Hit]]-1)/20))),
    OR(
        [@[Accuracy]]="SP DC ENEMY DVG",
        [@[Accuracy]]="SP DC M 1/2 ENEMY DVG"
    ),
        (N("Advantage hit formula for spell save throws done by disadvantaged enemy")+
        (1-(1-(1-(([@[Min to Hit]]-1)/20)))^2)),
    AND(
        OR(
            [@[Accuracy]]="SP DC LEGEND RESIST",
            [@[Accuracy]]="SP DC M 1/2 LEG RESIST",
            [@[Accuracy]]="SP DC EN DVG LEG RES",
            [@[Accuracy]]="SP DC M 1/2 EN DVG L-RES"
        ),
        ([@[Legendary Resistances Left]]>0)
    ),
        (N("Legendary Resistance is a guaranteed miss")+0),
    AND(
        OR(
            [@[Accuracy]]="SP DC LEGEND RESIST",
            [@[Accuracy]]="SP DC M 1/2 LEG RESIST"
        ),
        ([@[Legendary Resistances Left]]=0)
    ),
        (N("After Legendary Resistances run out, standard hit formula")+
        (1-(([@[Min to Hit]]-1)/20))),
    AND(
        OR(
            [@[Accuracy]]="SP DC EN DVG LEG RES",
            [@[Accuracy]]="SP DC M 1/2 EN DVG L-RES"
        ),
        ([@[Legendary Resistances Left]]=0)
    ),
        (N("After Legendary Resistances run out, enemy disadvantage uses advantage hit formula")+
        (1-(1-(1-(([@[Min to Hit]]-1)/20)))^2))
)
*
IF(
    AND(
        [@[Current Attack is GWM Crit Bonus Attack]]="YES",
        OR(
            [@[Accuracy]]="STANDARD",
            [@[Accuracy]]="ADVANTAGE",
            [@[Accuracy]]="DISADVANTAGE",
            [@[Accuracy]]="ELVEN ACCURACY",
        )
    ),
    (
        N("If the current attack is a Great Weapon Master crit bonus attack")+
        N("And it's not a spell attack")+
        N("The chances of this attack are multiplied by the crit chance of the previous attack of the same round and tactic")+
        SUMIFS(
            [Crit % Chance],
            [Tactic ID],[@[Tactic ID]],
            [Round],[@[Round]],
            [Row],[@[Row]]-1
        )
    ),
    (
        N("Else they just stay the same")+
        1
    )
)
```


<a id="hit-percentage-not-including-crit"></a>
### Hit percentage (not including crit)

Formula: `Hx=H-C`

```
[@[Non-crit Hit % Chance]]
=N("Hit percentage (not including crit)")+
    ([@[Hit % Chance]]-[@[Crit % Chance]])
```

<a id="miss-chance"></a>
### Miss chance

Formula: M=1-H

`[@[Miss % Chance]]
=N("Miss percentage")+(1-[@[Hit % Chance]])`

<a id="targets-and-repeated-attacks"></a>
## Targets and Repeated Attacks

<a id="repeated-attacks"></a>
### Repeated Attacks

The column `[Repeat Attack Times]` will be considered in the total damage calculation.
This is mostly so you don't have to copy paste the same row if the stats are exactly the same.

<a id="area-of-effect-expected-targets"></a>
### Area of Effect Expected Targets

Made two columns:
```
[Area of Effect]

-
CONE
CUBE
CYLINDER
LINE
SPHERE
```

And 
```
[AOE size, length or radius]
```
Which is either empty, "-", or a number.

Now based on the Dungeon Master's Guide, page 249, "Adjudicating Areas of Effect" table:

|Area              |  Number of Targets      |
|--------------------------------------------|
|Cone              |  Size   / 10 (round up) |
|Cube or square    |  Size   / 5  (round up) |
|Cylinder          |  Radius / 5  (round up) |
|Line              |  Length / 30 (round up) |
|Sphere or circle  |  Radius / 5  (round up) |

I made the following formula.

```
[@[Area of Effect Expected Targets]]
=IFS(
    AND(
        [@[Area of Effect]]="CONE",
        ISNUMBER([@[AOE size, length or radius]])
    ),
        (
            N("Cone: Size   / 10 (round up)")+
            ROUNDUP(([@[AOE size, length or radius]]/10),0)
        ),
    AND(
        [@[Area of Effect]]="CUBE",
        ISNUMBER([@[AOE size, length or radius]])
    ),
        (
            N("Cube or square: Size   / 5  (round up)")+
            ROUNDUP(([@[AOE size, length or radius]]/5),0)
        ),
    AND(
        [@[Area of Effect]]="CYLINDER",
        ISNUMBER([@[AOE size, length or radius]])
    ),
        (
            N("Cylinder: Radius / 5  (round up)")+
            ROUNDUP(([@[AOE size, length or radius]]/5),0)
        ),
    AND(
        [@[Area of Effect]]="LINE",
        ISNUMBER([@[AOE size, length or radius]])
    ),
        (
            N("Line: Length / 30 (round up)")+
            ROUNDUP(([@[AOE size, length or radius]]/30),0)
        ),
    AND(
        [@[Area of Effect]]="SPHERE",
        ISNUMBER([@[AOE size, length or radius]])
    ),
        (
            N("Sphere or circle: Radius / 5  (round up)")+
            ROUNDUP(([@[AOE size, length or radius]]/5),0)
        ),
    (
        N("Based on the Dungeon Master's Guide, page 249, 'Adjudicating Areas of Effect' table")+
        1
    ),
        "-"
)
```


<a id="dice-notation-reader"></a>
## Dice notation reader

<a id="splitting-the-dice-notation-eg-1d8"></a>
### Splitting the dice notation (e.g. 1d8)

left of d
```
=N("Number of Dice")+
IF(
    (N("Condition: has to have dice notation")+
    COUNT(SEARCH("d",[@[Damage Dice]]))>0),
    (N("Left of cell from position x in the string")+
    LEFT(
        [@[Damage Dice]],
        (N("Position first d found -1 to delete d")+
        (SEARCH("d",[@[Damage Dice]])-1))
    )),
    (N("0 if no dice found.")+0)
)
```

right of d
```
=N("Die size")+
IF(
    (N("Condition: has to have dice notation")+
    COUNT(SEARCH("d",[@[Damage Dice]]))>0),
    (N("Right of cell from position x in the string.")+
    RIGHT(
        [@[Damage Dice]],
        (
            (N("Length of string to subtract position of d")+
            LEN([@[Damage Dice]]))
            -
            (N("Search first found d")+
            (SEARCH("d",[@[Damage Dice]])))
        )
    )),
    (N("0 if no dice found.")+0)
)
```


<a id="average-damage-for-base-damage-dice-single-dice-notation-eg-1d8"></a>
### Average Damage for base damage dice (single dice notation (e.g. 1d8))

Formula:
- NumDice*(avg(1,DieSize))
    + Elemental Adept turn 1s into 2s:
        * NumDice*(1/DieSize)
        * But only if the damage types match
    + If Great Weapon Fighting style:
        * NumDice*(avg(1,DieSize)) + NumDice*(1-(2/DieSize))
        * But only on the base damage dice

```
[@[Base Dice Dmg Avg]]
=N("Average damage")+
(
    (
        N("Number of Dice")+
        IF(
            (N("Condition: has to have dice notation")+
            COUNT(SEARCH("d",[@[Damage Dice]]))>0),
            (N("Left of cell from position x in the string")+
            LEFT(
                [@[Damage Dice]],
                (N("Position first d found -1 to delete d")+
                (SEARCH("d",[@[Damage Dice]])-1))
            )),
            (N("0 if no dice found.")+0)
        )
    )
    *
    (
        N("Average roll for the die size")+
        AVERAGE(
            N("Lowest roll possible")+
            1,
            (
                N("Die size")+
                IF(
                    (N("Condition: has to have dice notation")+
                    COUNT(SEARCH("d",[@[Damage Dice]]))>0),
                    (N("Right of cell from position x in the string.")+
                    RIGHT(
                        [@[Damage Dice]],
                        (
                            (N("Length of string to subtract position of d")+
                            LEN([@[Damage Dice]]))
                            -
                            (N("Search first found d")+
                            (SEARCH("d",[@[Damage Dice]])))
                        )
                    )),
                    (N("0 if no dice found.")+0)
                )
            )
        )
    )
)
+
N("Elemental Adept turn 1s into 2s.")+
N("Average increases by 1/DieSize per die")+
N("Condition: damage type matches EA type.")+
IF(
    ([@[Damage Type]]=[@[Feat: Elemental Adept Element]]),
    (
        (
            N("Number of Dice")+
            IF(
                (N("Condition: has to have dice notation")+
                COUNT(SEARCH("d",[@[Damage Dice]]))>0),
                (N("Left of cell from position x in the string")+
                LEFT(
                    [@[Damage Dice]],
                    (N("Position first d found -1 to delete d")+
                    (SEARCH("d",[@[Damage Dice]])-1))
                )),
                (N("0 if no dice found.")+0)
            )
        )
        *
        (
            1
            /
            (
                N("Die size")+
                IF(
                    (N("Condition: has to have dice notation")+
                    COUNT(SEARCH("d",[@[Damage Dice]]))>0),
                    (N("Right of cell from position x in the string.")+
                    RIGHT(
                        [@[Damage Dice]],
                        (
                            (N("Length of string to subtract position of d")+
                            LEN([@[Damage Dice]]))
                            -
                            (N("Search first found d")+
                            (SEARCH("d",[@[Damage Dice]])))
                        )
                    )),
                    (N("0 if no dice found.")+0)
                )
            )
        )
    ),
    (N("0 if no Elemental Adept applies.")+0)
)
+
N("Great Weapon Fighting style reroll 1s and 2s once.")+
IF(
    ([@[Great Weapon Fighting Style]]="YES"),
    (
        (
            N("Number of Dice")+
            IF(
                (N("Condition: has to have dice notation")+
                COUNT(SEARCH("d",[@[Damage Dice]]))>0),
                (N("Left of cell from position x in the string")+
                LEFT(
                    [@[Damage Dice]],
                    (N("Position first d found -1 to delete d")+
                    (SEARCH("d",[@[Damage Dice]])-1))
                )),
                (N("0 if no dice found.")+0)
            )
        )
        *
        (
            N("Reroll 1s or 2s on weapon dice only")+
            (
                1
                -
                (
                    2
                    /
                    (
                        N("Die size")+
                        IF(
                            (N("Condition: has to have dice notation")+
                            COUNT(SEARCH("d",[@[Damage Dice]]))>0),
                            (N("Right of cell from position x in the string.")+
                            RIGHT(
                                [@[Damage Dice]],
                                (
                                    (N("Length of string to subtract position of d")+
                                    LEN([@[Damage Dice]]))
                                    -
                                    (N("Search first found d")+
                                    (SEARCH("d",[@[Damage Dice]])))
                                )
                            )),
                            (N("0 if no dice found.")+0)
                        )
                    )
                )
            )
        )
    ),
    (N("0 if no Great Weapon Fighting style applies.")+0)
)
```


<a id="average-damage-for-other-damage-dice-single-dice-notation-eg-1d8"></a>
### Average Damage for other damage dice (single dice notation (e.g. 1d8))

Used referencing dice that are not weapon or base dice.
Drag formula to make references work.

```
=N("Average damage")+
(
    (
        N("Number of Dice")+
        IF(
            (N("Condition: has to have dice notation")+
            COUNT(SEARCH("d",S2))>0),
            (N("Left of cell from position x in the string")+
            LEFT(
                S2,
                (N("Position first d found -1 to delete d")+
                (SEARCH("d",S2)-1))
            )),
            (N("0 if no dice found.")+0)
        )
    )
    *
    (
        N("Average roll for the die size")+
        AVERAGE(
            N("Lowest roll possible")+
            1,
            (
                N("Die size")+
                IF(
                    (N("Condition: has to have dice notation")+
                    COUNT(SEARCH("d",S2))>0),
                    (N("Right of cell from position x in the string.")+
                    RIGHT(
                        S2,
                        (
                            (N("Length of string to subtract position of d")+
                            LEN(S2))
                            -
                            (N("Search first found d")+
                            (SEARCH("d",S2)))
                        )
                    )),
                    (N("0 if no dice found.")+0)
                )
            )
        )
    )
)
+
N("Elemental Adept turn 1s into 2s.")+
N("Average increases by 1/DieSize per die")+
N("Condition: damage type matches EA type.")+
IF(
    (Z2=[@[Feat: Elemental Adept Element]]:[@[Feat: Elemental Adept Element]]),
    (
        (
            N("Number of Dice")+
            IF(
                (N("Condition: has to have dice notation")+
                COUNT(SEARCH("d",S2))>0),
                (N("Left of cell from position x in the string")+
                LEFT(
                    S2,
                    (N("Position first d found -1 to delete d")+
                    (SEARCH("d",S2)-1))
                )),
                (N("0 if no dice found.")+0)
            )
        )
        *
        (
            1
            /
            (
                N("Die size")+
                IF(
                    (N("Condition: has to have dice notation")+
                    COUNT(SEARCH("d",S2))>0),
                    (N("Right of cell from position x in the string.")+
                    RIGHT(
                        S2,
                        (
                            (N("Length of string to subtract position of d")+
                            LEN(S2))
                            -
                            (N("Search first found d")+
                            (SEARCH("d",S2)))
                        )
                    )),
                    (N("0 if no dice found.")+0)
                )
            )
        )
    ),
    (N("0 if no Elemental Adept applies.")+0)
)
```


<a id="average-damage-for-added-dice-notation-eg-1d82d4"></a>
### Average Damage for added dice notation (e.g. 1d8+2d4+...)

I can't figure this one out because it becomes recursive with searches...

<a id="bonus-damages"></a>
## Bonus Damages

<a id="bonus-flat-dmg"></a>
### Bonus Flat Dmg

For the damage modifier based on your attack or casting ability.

<a id="great-weapon-master--5-to-hit-for-10-damage"></a>
### Great Weapon Master -5 to hit for 10 damage

```
[@[Great Weapon Master Bonus Dmg]]
=IF([@[Great Weapon Master -5 to hit for +10 dmg]]="YES",
    10,"-")
```

<a id="resulting-damages"></a>
## Resulting Damages

Variables:

- Dr: damage roll on hit (crit multiplies)
- Db: bonus damage on hit (crit multiplies)
- Dbn: more bonus damage if necessary (Db+Dbn1+Dbn2...)
- Dxc: damage exclusive to crit (crit multiplies)
- Dxcn: bonus damage exclusive to crit that's not multiplied
- Dn: bonus damage that's not multiplied
- Dc: damage added on crit (all applicable)
- D: damage on hit (all applicable)
- Dcr: Average resulting damage if it were a crit
- B: flat bonus damage (modifiers and such, not multiplied)
- R: Repeated attack times (must have the same bonuses applied.)

<a id="midpoint-calculations"></a>
### Midpoint calculations

<a id="dc-damage-added-on-crit-all-applicable"></a>
#### Dc: Damage added on crit (all applicable)

Formula: 

`Dc=Dr+Db+(Dbn1+Dbn2...)+(2*Dxc)+Dxcn`

```
[@[Average Damage Added on Crit]]
=N("Average damage added on crit")+
IF(
    NOT(OR(
        [@[Accuracy]]="SPELL (SAVE DC)",
        [@[Accuracy]]="SP DC MISS 1/2 DMG",
        [@[Accuracy]]="SP DC ENEMY DVG",
        [@[Accuracy]]="SP DC M 1/2 ENEMY DVG",
        [@[Accuracy]]="SP DC LEGEND RESIST",
        [@[Accuracy]]="SP DC M 1/2 LEG RESIST",
        [@[Accuracy]]="SP DC EN DVG LEG RES",
        [@[Accuracy]]="SP DC M 1/2 EN DVG L-RES"
    )),
    (
        (N("Dr: Damage roll on hit is doubled")+
        [@[Base Dice Dmg Avg]])
        +
        (N("Db+Dbn1+Dbn2...Any bonus dice in this range get doubled")+
        SUM([@[Bonus Dice Dmg Avg]]:[@[More Bonus Dice Dmg Avg]]))
        +
        (N("Dxc: Damage that gets doubled but only added on crit")+
        (2*[@[Crit-only Mult Dice Dmg Avg]]))
        +
        (N("Dxcn: Bonus damage that's added on crit but not doubled")+
        [@[Crit-only Flat Dice Dmg Avg]])
    ),
    (N("No crits for save throw spells")+0)
)
```

<a id="d-damage-on-hit-all-applicable"></a>
#### D: Damage on hit (all applicable)

Formula: 

`D=Dr+Db+(Dbn1+Dbn2...)+Dn`

```
[@[Average Dice Damage on Hit]]
=N("Average damage on hit")+
(
    (N("Dr: Damage roll on hit is doubled on crit")+
    [@[Base Dice Dmg Avg]])
    +
    (N("Db+Dbn1+Dbn2...Any bonus dice in this range get doubled on crit")+
    SUM([@[Bonus Dice Dmg Avg]]:[@[More Bonus Dice Dmg Avg]]))
    +
    (N("Dn: Damage bonus that is not doubled on crit")+
    [@[Non-crit Dice Dmg Avg]])
)
```

<a id="dcr-damage-result-on-crit"></a>
#### Dcr: Damage result on crit

Formula:

`Dcr=D+Dc`

Spell DCs don't crit

```
[@[Average Resulting Crit Dice Damage]]
=N("Damage result in case of a crit")+
IF(
    NOT(OR(
        [@[Accuracy]]="SPELL (SAVE DC)",
        [@[Accuracy]]="SP DC MISS 1/2 DMG",
        [@[Accuracy]]="SP DC ENEMY DVG",
        [@[Accuracy]]="SP DC M 1/2 ENEMY DVG",
        [@[Accuracy]]="SP DC LEGEND RESIST",
        [@[Accuracy]]="SP DC M 1/2 LEG RESIST",
        [@[Accuracy]]="SP DC EN DVG LEG RES",
        [@[Accuracy]]="SP DC M 1/2 EN DVG L-RES"
    )),
    (
        (N("D: Average damage on hit")+
        [@[Average Dice Damage on Hit]])
        +
        (N("Dc: Average damage added on crit")+
        [@[Average Damage Added on Crit]])
    ),
    (N("No crits for save throw spells")+0)
)
```

<a id="dm-damage-on-miss-all-applicable-spells-with-12-dmg-only"></a>
#### Dm: Damage on miss (all applicable) (spells with 1/2 dmg only)

Formula: 

`Dm=(Dr+Db+(Dbn1+Dbn2...)+Dn)/2`

```
[@[Average Damage on Miss]]
=N("Average damage on miss")+
IF(
    OR(
        [@[Accuracy]]="SP DC MISS 1/2 DMG",
        [@[Accuracy]]="SP DC M 1/2 ENEMY DVG",
        [@[Accuracy]]="SP DC M 1/2 LEG RESIST",
        [@[Accuracy]]="SP DC M 1/2 EN DVG L-RES"
    ),
    (
        (
            (N("Dr: Damage roll on hit is halved")+
            [@[Base Dice Dmg Avg]])
            +
            (
                N("B: Bonus modifiers halved too")+
                IF(ISNUMBER([@[Bonus Flat Dmg]]),
                [@[Bonus Flat Dmg]],0)
                +N("Spells don't get Great Weapon Master bonus damage")
            )
            +
            (N("Db+Dbn1+Dbn2...Any bonus dice is halved too")+
            SUM([@[Bonus Dice Dmg Avg]]:[@[More Bonus Dice Dmg Avg]]))
            +
            (N("Dn: Damage bonus that is not doubled on crit gets halved too")+
            [@[Non-crit Dice Dmg Avg]])
        )
        /
        2
    )
    ,
    (N("No miss damage for regular attacks")+0)
)
```


<a id="damage-aggregates"></a>
### Damage Aggregates

<a id="dpa-damage-per-attack-uses-dpr-formula"></a>
#### DPA: Damage per Attack (uses DPR formula)

Formulas:

`DPA=(C*Dc)+(Hx*(D+B))+(M*(Dm))`

```
[@[Damage per Attack]]
=(
    (
        (N("C: Crit chance")+
        [@[Crit % Chance]])
        *
        (N("Dc: Average damage added on crit")+
        [@[Average Damage Added on Crit]])
    )
    +
    (
        (N("Hx: Hit chance (excluding crit)")+
        [@[Non-crit Hit % Chance]])
        *
        (
            (N("D: Average damage on hit")+
            [@[Average Dice Damage on Hit]])
            +
            (N("B: Bonus modifiers")+
            IF(ISNUMBER([@[Bonus Flat Dmg]]),[@[Bonus Flat Dmg]],0))
            +
            (N("GWMb: Great Weapon Master -5 to hit for +10 dmg option")+
            IF(ISNUMBER([@[Great Weapon Master Bonus Dmg]]),[@[Great Weapon Master Bonus Dmg]],0))
        )
    )
    +
    (
        (N("M: Miss chance (for spell saves)")+
        [@[Miss % Chance]])
        *
        (N("Dm: Average damage on miss (only for spell saves)")+
        [@[Average Damage on Miss]])
    )
)
```

<a id="average-resulting-damage-per-target-for-the-row"></a>
#### Average Resulting Damage per target (for the row)

Multiply by the times the attack is repeated and the number of targets

```
[@[Average Resulting Damage per Target]]
=N("Average damage for the attack row")+
(
    N("R: Repeated attack times (must have the same bonuses applied)")+
    IF(        
        ISNUMBER([@[Repeat Attack Times]]),
        [@[Repeat Attack Times]],
        1
    )
)
*
(
    N("DPA: Damage per Attack")+
    [@[Damage per Attack]]
)
```

<a id="average-resulting-damage-total"></a>
#### Average Resulting Damage Total

```
[@[Average Resulting Damage Total]]
=(
    [@[Average Resulting Damage per Target]]
    *
    (
        N("AOE: Area of Effect targets (must have the same bonuses applied)")+
        IF(        
            ISNUMBER([@[Area of Effect Expected Targets]]),
            [@[Area of Effect Expected Targets]],
            1
        )
    )
)
```

<a id="average-round-damage-per-target"></a>
#### Average Round Damage per Target

Somehow identify and group tactic+Round number and add all attacks.

Show only if next row is a different round 
Sum only if the tactic ID and the round number are the same.

Column A: Tactic ID
Column D: Round number

```
[@[Average Round Damage per Target]]
=IF(
    OR(
        (D3<>[@[Round]]),
        (A3<>[@[Tactic ID]])
    ),
    (
        N("Leave blank if not the last on the list for this round")+
        N("Sum the attacks with the same tactic AND round number")+
        SUMIFS(
            [[Average Resulting Damage per Target]:[Average Resulting Damage per Target]],
            [[Tactic ID]:[Tactic ID]],[@[Tactic ID]],
            [[Round]:[Round]],[@[Round]]
        )
    ),
    "-"
)
```

<a id="average-round-damage-total"></a>
#### Average Round Damage Total

```
[@[Average Round Damage Total]]
=IF(
    ISNUMBER([@[Average Round Damage per Target]]),
    (
        [@[Average Round Damage per Target]]
        *
        (
            N("AOE: Area of Effect targets (must have the same bonuses applied)")+
            IF(        
                ISNUMBER([@[Area of Effect Expected Targets]]),
                [@[Area of Effect Expected Targets]],
                1
            )
        )
    ),
    "-"
)
```


<a id="average-tactic-damage-per-target"></a>
#### Average Tactic Damage per Target

Column A: Tactic ID

Only show if the next row is a different tactic ID or empty.

```
[@[Average Tactic Damage per Target]]
=IF(
    (B3<>[@[Tactic ID]]),
    (
        N("Leave blank if not the last on the list for this tactic")+
        N("Sum the attacks with the same tactic ID")+
        SUMIFS(
            [[Average Resulting Damage per Target]:[Average Resulting Damage per Target]],
            [[Tactic ID]:[Tactic ID]],[@[Tactic ID]]
        )
    ),
    "-"
)
```

<a id="average-tactic-damage-total"></a>
#### Average Tactic Damage Total

Column A: Tactic ID

Only show if the next row is a different tactic ID or empty.

```
[@[Average Tactic Damage Total]]
=IF(
    (B3<>[@[Tactic ID]]),
    (
        N("Leave blank if not the last on the list for this tactic")+
        N("Sum the attacks with the same tactic ID")+
        SUMIFS(
            [[Average Resulting Damage Total]:[Average Resulting Damage Total]],
            [[Tactic ID]:[Tactic ID]],[@[Tactic ID]]
        )
    ),
    "-"
)
```


<a id="damage-per-round-per-target"></a>
#### Damage per Round per Target

```
[@[Damage per Round per Target]]
=IF(
    ISNUMBER([@[Average Tactic Damage per Target]]),
    (
        N("Only show if the tactic damage is showing a number")+
        N("Damage per round: Damage per tactic / number of rounds")+
        (
            (N("Damage per tactic")+
            [@[Average Tactic Damage per Target]])
            /
            (N("Number of rounds")+
            [@[Round]])
        )
    ),
    "-"
)
```

<a id="total-damage-per-round"></a>
#### Total Damage per Round

```
[@[Total Damage per Round]]
=IF(
    ISNUMBER([@[Average Tactic Damage Total]]),
    (
        N("Only show if the tactic damage is showing a number")+
        N("Damage per round: Damage per tactic / number of rounds")+
        (
            (N("Damage per tactic")+
            [@[Average Tactic Damage Total]])
            /
            (N("Number of rounds")+
            [@[Round]])
        )
    ),
    "-"
)
```


<a id="dpr-rating"></a>
## DPR Rating

From [RPGBot: DnD 5e – The Fundamental Math of Character Optimization](https://rpgbot.net/dnd5/characters/fundamental_math/)

There's two important quotes:

>But how much DPR do you need? What constitutes “good” dpr?
>
>Finding the answer is surprisingly simple. The same rules that we use to find the fundamental math progression for attack bonuses also give us an hp progression for monsters. If we assume a party of 4 and 3-round encounters like the CR calculation rules do, we can divide the top end of the hp progression by 12 to get our target DPR. If your party’s average DPR is roughly that high, you’re doing okay.


>I’ve chosen to include four columns of DPR:
>
>- Low DPR: If you’re above this number but below Target DPR, you’re not doing quite as much damage as the game expects on average. If your character is primarily doing other things (support spells, area control, save or suck spells, etc.) this might be perfectly fine. 
>- Target DPR: The sweet spot. If you’re at least this high, you’re doing fine. If you absolutely can’t get above this number, ensure that whatever you’re doing in combat boosts your party’s total DPR to compensate.
>- High DPR: If you’re above this number, you’re doing two people worth of work. You might be your party’s Striker, and in combat you’re the scariest thing that isn’t a save or suck.
>- Dude Stop: If you’re above this number, you’re dealing enough damage to feasibly solo encounters. If you can maintain this damage output for three rounds, you’re expected to kill a level-appropriate monster entirely by yourself. Your DM may need to make significant adjustments to the difficulty of encounters to provide you with an adequate challenge. In effect, you’ve won character optimization. Stamp four stars on your character sheet in blue ink and send me a picture on twitter so I can revel in your success.

Now, the values that are output in the table are for eyeing a reference, but I want to make an automatic formula to get the rating, so I need to know exactly how RPGBOT decided the other ratings (Target DPR threshold is MaxEnemyHP/12).

I did that in the `DPR Rating Explanation` worksheet.

Turns out:

- Low DPR: MaxEnemyHP/24
- Target DPR: MaxEnemyHP/12
- Low DPR: MaxEnemyHP/6
- Low DPR: MaxEnemyHP/3

<a id="max-expected-enemy-hp-at-cr-or-lv"></a>
### Max Expected Enemy HP at CR or LV

Now, checking the Dungeon Master's Guide, page 274 under 'Creating Quick Monster Stats' we find a table with the CR rating and the HP ranges you can choose. The maximums are defined as such:

| CR or LV | Max Expected Enemy HP  |
|----------|------------------------|
| 1        | 85                     |
| 2        | 100                    |
| 3        | 115                    |
| 4        | 130                    |
| 5        | 145                    |
| 6        | 160                    |
| 7        | 175                    |
| 8        | 190                    |
| 9        | 205                    |
| 10       | 220                    |
| 11       | 235                    |
| 12       | 250                    |
| 13       | 265                    |
| 14       | 280                    |
| 15       | 295                    |
| 16       | 310                    |
| 17       | 325                    |
| 18       | 340                    |
| 19       | 355                    |
| 20       | 400                    |

For ease of use, I added a column with conditional output.

```
[@[Max Expected Enemy HP at CR or LV]]
=IFS(
    [@[LV or CR]]=1,  85,
    [@[LV or CR]]=2,  100,
    [@[LV or CR]]=3,  115,
    [@[LV or CR]]=4,  130,
    [@[LV or CR]]=5,  145,
    [@[LV or CR]]=6,  160,
    [@[LV or CR]]=7,  175,
    [@[LV or CR]]=8,  190,
    [@[LV or CR]]=9,  205,
    [@[LV or CR]]=10, 220,
    [@[LV or CR]]=11, 235,
    [@[LV or CR]]=12, 250,
    [@[LV or CR]]=13, 265,
    [@[LV or CR]]=14, 280,
    [@[LV or CR]]=15, 295,
    [@[LV or CR]]=16, 310,
    [@[LV or CR]]=17, 325,
    [@[LV or CR]]=18, 340,
    [@[LV or CR]]=19, 355,
    [@[LV or CR]]=20, 400
)
```

<a id="dpr-rating-formula"></a>
### DPR Rating Formula

```
[@[DPR Rating]]
=IF(
    ISNUMBER([@[Damage per Round per Target]]),
    IFS(
        ([@[Damage per Round per Target]]=0),
            0,
        ([@[Damage per Round per Target]]<=
            ([@[Max Expected Enemy HP at CR or LV]]/24)),
            1,
        ([@[Damage per Round per Target]]>
            ([@[Max Expected Enemy HP at CR or LV]]/24)),
            2,
        ([@[Damage per Round per Target]]>
            ([@[Max Expected Enemy HP at CR or LV]]/12)),
            3,
        ([@[Damage per Round per Target]]>
            ([@[Max Expected Enemy HP at CR or LV]]/6)),
            4,
        ([@[Damage per Round per Target]]>
            ([@[Max Expected Enemy HP at CR or LV]]/3)),
            5
    ),
    "-"
)
```

<a id="dpr-rating-flavor-text"></a>
### DPR rating flavor text

```
[@[DPR Rating Description]]
=IFS(
    ISNUMBER([@[DPR Rating]])=FALSE,"-",
    [@[DPR Rating]]=0,"☆☆☆  No damage",
    [@[DPR Rating]]=1,"🡇🡇🡇  Not helping (lowest)",
    [@[DPR Rating]]=2,"★☆☆  Low (support, control, debuff)",
    [@[DPR Rating]]=3,"★★☆  Target (expected)",
    [@[DPR Rating]]=4,"★★★  High (heavy hitter)",
    [@[DPR Rating]]=5,"🕱🕱🕱🕱  Over Powered (dude stop)"
)
```

Ratings

1: 🡇🡇🡇  Not helping (lowest)
2: ★☆☆  Low (support, control, debuff)
3: ★★☆  Target (expected)
4: ★★★  High (heavy hitter)
5: 🕱🕱🕱🕱  Over Powered (dude stop)

<a id="damage-type-shortened"></a>
## Damage Type shortened

Not using this at the moment.

```
=IFS(
    [@[Dmg Type]]="ACID",
        "ACD",
    [@[Dmg Type]]="BLUDGEONING",
        "BLG",
    [@[Dmg Type]]="COLD",
        "CLD",
    [@[Dmg Type]]="FIRE",
        "FIR",
    [@[Dmg Type]]="FORCE",
        "FRC",
    [@[Dmg Type]]="LIGHTNING",
        "LTG",
    [@[Dmg Type]]="NECROTIC",
        "NEC",
    [@[Dmg Type]]="PIERCING",
        "PRC",
    [@[Dmg Type]]="POISON",
        "PSN",
    [@[Dmg Type]]="PSYCHIC",
        "PSY",
    [@[Dmg Type]]="RADIANT",
        "RAD",
    [@[Dmg Type]]="SLASHING",
        "SLS",
    [@[Dmg Type]]="THUNDER",
        "THU",
    1,
    "-"
)
```