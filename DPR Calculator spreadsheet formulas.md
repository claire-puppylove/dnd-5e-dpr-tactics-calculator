# DPR Calculator formulas

## Notation

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

### Minimum to Crit (Ac)

Assume 20, w/exceptions
Such as
Hexblade's Curse, Champion Fighter Improved Critical, etc.

*\* Ac is for attack roll to crit*

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

### Halfling

Lucky attribute
rerolls one die when d20=1

Since Elven Accuracy is impossible, it just returns regular advantage
No effect on SPELL DC attaks.

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

## Modifiers

### Assumed Relative Modifier to Hit

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

## Accuracies

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


#### Legendary resistances

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



### Hit percentage (not including crit)

- `Hx=H-C`
```
[@[Non-crit Hit % Chance]]
=N("Hit percentage (not including crit)")+
([@[Hit % Chance]]-[@[Crit % Chance]])
```

### Miss chance

M=1-H
`[@[Miss % Chance]]
=N("Miss percentage")+(1-[@[Hit % Chance]])`

## Dice notation reader

### Splitting the dice notation (e.g. 1d8)

* Note: needs to default to 0 to avoid errors

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

right of d: Right now, only single digits... hmm...
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


### Average Damage for a single dice notation (e.g. 1d8)

Formula:
- NumDice*(avg(1,DieSize))
    + If Great Weapon Fighting style:
        NumDice*(avg(1,DieSize)) + NumDice*(1-(2/DieSize))
        But only on the weapon dice (column O)

Used in column O to produce column AG

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


### Average Damage for added dice notation (e.g. 1d8+2d4)

I can't figure this one out because it becomes recursive with searches...

## Bonus Damages

### Great Weapon Master -5 to hit for 10 damage

```
[@[Great Weapon Master Bonus Dmg]]
=IF([@[Great Weapon Master -5 to hit for +10 dmg]]="YES",
    10,"-")
```

## Average Resulting Damages

### Average Attack Damage

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
                IF(ISNUMBER([@[Bonus Flat Damage]]),
                [@[Bonus Flat Damage]],0)
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


#### DPA: Stacked Damage per attack (uses DPR formula)

Formulas:  

`DPA=R*((C*Dc)+(Hx*(D+B))+(M*(Dm)))`

```
[@[Average Attack Damage]]
=N("Average damage per attack")+
(
    N("R: Repeated attack times (must have the same bonuses applied)")+
    [@[Repeat Attack Times]]
)
*
(
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
            IF(ISNUMBER([@[Bonus Flat Damage]]),[@[Bonus Flat Damage]],0))
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


### Average Round Damage

Somehow identify and group tactic+Round number and add all attacks.

Show only if next row is a different round 
Sum only if the tactic ID and the round number are the same.

Column A: Tactic ID
Column D: Round number

```
[@[Average Round Damage]]
=IF(
    OR(
        (D3<>[@[Round]]),
        (A3<>[@[Tactic ID]])
    ),
    (
        N("Leave blank if not the last on the list for this round")+
        N("Sum the attacks with the same tactic AND round number")+
        SUMIFS(
            [[Average Attack Damage]:[Average Attack Damage]],
            [[Tactic ID]:[Tactic ID]],[@[Tactic ID]],
            [[Round]:[Round]],[@[Round]]
        )
    ),
    "-"
)
```

### Average Tactic Damage

Column A: Tactic ID

Only show if the next row is a different tactic ID or empty.

```
[@[Average Tactic Damage]]
=IF(
    (A3<>[@[Tactic ID]]),
    (
        N("Leave blank if not the last on the list for this tactic")+
        N("Sum the attacks with the same tactic ID")+
        SUMIFS(
            [[Average Attack Damage]:[Average Attack Damage]],
            [[Tactic ID]:[Tactic ID]],[@[Tactic ID]]
        )
    ),
    "-"
)
```

### Damage per Round

Column A: Tactic ID
Column D: Round number

```
[@[Damage per Round]]
=IF(
    ISNUMBER([@[Average Tactic Damage]]),
    (
        N("Only show if the tactic damage is showing a number")+
        N("Damage per round: Damage per tactic / number of rounds")+
        (
            (N("Damage per tactic")+
            [@[Average Tactic Damage]])
            /
            (N("Number of rounds")+
            [@[Round]])
        )
    ),
    "-"
)
```

### DPR rating

From [RPGBot: DnD 5e – The Fundamental Math of Character Optimization](https://rpgbot.net/dnd5/characters/fundamental_math/)

>I’ve chosen to include four columns of DPR:
>
>- Low DPR: If you’re above this number but below Target DPR, you’re not doing quite as much damage as the game expects on average. If your character is primarily doing other things (support spells, area control, save or suck spells, etc.) this might be perfectly fine. 
>- Target DPR: The sweet spot. If you’re at least this high, you’re doing fine. If you absolutely can’t get above this number, ensure that whatever you’re doing in combat boosts your party’s total DPR to compensate.
>- High DPR: If you’re above this number, you’re doing two people worth of work. You might be your party’s Striker, and in combat you’re the scariest thing that isn’t a save or suck.
>- Dude Stop: If you’re above this number, you’re dealing enough damage to feasibly solo encounters. If you can maintain this damage output for three rounds, you’re expected to kill a level-appropriate monster entirely by yourself. Your DM may need to make significant adjustments to the difficulty of encounters to provide you with an adequate challenge. In effect, you’ve won character optimization. Stamp four stars on your character sheet in blue ink and send me a picture on twitter so I can revel in your success.

```
[@[DPR Rating]]
=IF(
    ISNUMBER([@[Damage per Round]]),
    IFS(
        [@[LV or CR]]=1,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=3.5),1,
            AND(3.5<[@[Damage per Round]],[@[Damage per Round]]<=7.1),2,
            AND(7.1<[@[Damage per Round]],[@[Damage per Round]]<=14.2),3,
            AND(14.2<[@[Damage per Round]],[@[Damage per Round]]<=28.3),4,
            ([@[Damage per Round]]>28.3),5
        ),
        [@[LV or CR]]=2,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=4.2),1,
            AND(4.2<[@[Damage per Round]],[@[Damage per Round]]<=8.3),2,
            AND(8.3<[@[Damage per Round]],[@[Damage per Round]]<=16.7),3,
            AND(16.7<[@[Damage per Round]],[@[Damage per Round]]<=33.3),4,
            ([@[Damage per Round]]>33.3),5
        ),
        [@[LV or CR]]=3,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=4.8),1,
            AND(4.8<[@[Damage per Round]],[@[Damage per Round]]<=9.6),2,
            AND(9.6<[@[Damage per Round]],[@[Damage per Round]]<=19.2),3,
            AND(19.2<[@[Damage per Round]],[@[Damage per Round]]<=38.3),4,
            ([@[Damage per Round]]>38.3),5
        ),
        [@[LV or CR]]=4,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=5.4),1,
            AND(5.4<[@[Damage per Round]],[@[Damage per Round]]<=10.8),2,
            AND(10.8<[@[Damage per Round]],[@[Damage per Round]]<=21.7),3,
            AND(21.7<[@[Damage per Round]],[@[Damage per Round]]<=43.3),4,
            ([@[Damage per Round]]>43.3),5
        ),
        [@[LV or CR]]=5,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=6.0),1,
            AND(6.0<[@[Damage per Round]],[@[Damage per Round]]<=12.1),2,
            AND(12.1<[@[Damage per Round]],[@[Damage per Round]]<=24.2),3,
            AND(24.2<[@[Damage per Round]],[@[Damage per Round]]<=48.3),4,
            ([@[Damage per Round]]>48.3),5
        ),
        [@[LV or CR]]=6,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=6.7),1,
            AND(6.7<[@[Damage per Round]],[@[Damage per Round]]<=13.3),2,
            AND(13.3<[@[Damage per Round]],[@[Damage per Round]]<=26.7),3,
            AND(26.7<[@[Damage per Round]],[@[Damage per Round]]<=53.3),4,
            ([@[Damage per Round]]>53.3),5
        ),
        [@[LV or CR]]=7,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=7.3),1,
            AND(7.3<[@[Damage per Round]],[@[Damage per Round]]<=14.6),2,
            AND(14.6<[@[Damage per Round]],[@[Damage per Round]]<=29.2),3,
            AND(29.2<[@[Damage per Round]],[@[Damage per Round]]<=58.3),4,
            ([@[Damage per Round]]>58.3),5
        ),
        [@[LV or CR]]=8,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=7.9),1,
            AND(7.9<[@[Damage per Round]],[@[Damage per Round]]<=15.8),2,
            AND(15.8<[@[Damage per Round]],[@[Damage per Round]]<=31.7),3,
            AND(31.7<[@[Damage per Round]],[@[Damage per Round]]<=63.3),4,
            ([@[Damage per Round]]>63.3),5
        ),
        [@[LV or CR]]=9,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=8.5),1,
            AND(8.5<[@[Damage per Round]],[@[Damage per Round]]<=17.1),2,
            AND(17.1<[@[Damage per Round]],[@[Damage per Round]]<=34.2),3,
            AND(34.2<[@[Damage per Round]],[@[Damage per Round]]<=68.3),4,
            ([@[Damage per Round]]>68.3),5
        ),
        [@[LV or CR]]=10,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=9.2),1,
            AND(9.2<[@[Damage per Round]],[@[Damage per Round]]<=18.3),2,
            AND(18.3<[@[Damage per Round]],[@[Damage per Round]]<=36.7),3,
            AND(36.7<[@[Damage per Round]],[@[Damage per Round]]<=73.3),4,
            ([@[Damage per Round]]>73.3),5
        ),
        [@[LV or CR]]=11,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=9.8),1,
            AND(9.8<[@[Damage per Round]],[@[Damage per Round]]<=19.6),2,
            AND(19.6<[@[Damage per Round]],[@[Damage per Round]]<=39.2),3,
            AND(39.2<[@[Damage per Round]],[@[Damage per Round]]<=78.3),4,
            ([@[Damage per Round]]>78.3),5
        ),
        [@[LV or CR]]=12,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=10.4),1,
            AND(10.4<[@[Damage per Round]],[@[Damage per Round]]<=20.8),2,
            AND(20.8<[@[Damage per Round]],[@[Damage per Round]]<=41.7),3,
            AND(41.7<[@[Damage per Round]],[@[Damage per Round]]<=83.3),4,
            ([@[Damage per Round]]>83.3),5
        ),
        [@[LV or CR]]=13,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=11.0),1,
            AND(11.0<[@[Damage per Round]],[@[Damage per Round]]<=22.1),2,
            AND(22.1<[@[Damage per Round]],[@[Damage per Round]]<=44.2),3,
            AND(44.2<[@[Damage per Round]],[@[Damage per Round]]<=88.3),4,
            ([@[Damage per Round]]>88.3),5
        ),
        [@[LV or CR]]=14,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=11.7),1,
            AND(11.7<[@[Damage per Round]],[@[Damage per Round]]<=23.3),2,
            AND(23.3<[@[Damage per Round]],[@[Damage per Round]]<=46.7),3,
            AND(46.7<[@[Damage per Round]],[@[Damage per Round]]<=93.3),4,
            ([@[Damage per Round]]>93.3),5
        ),
        [@[LV or CR]]=15,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=12.3),1,
            AND(12.3<[@[Damage per Round]],[@[Damage per Round]]<=24.6),2,
            AND(24.6<[@[Damage per Round]],[@[Damage per Round]]<=49.2),3,
            AND(49.2<[@[Damage per Round]],[@[Damage per Round]]<=98.3),4,
            ([@[Damage per Round]]>98.3),5
        ),
        [@[LV or CR]]=16,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=12.9),1,
            AND(12.9<[@[Damage per Round]],[@[Damage per Round]]<=25.8),2,
            AND(25.8<[@[Damage per Round]],[@[Damage per Round]]<=51.7),3,
            AND(51.7<[@[Damage per Round]],[@[Damage per Round]]<=103.3),4,
            ([@[Damage per Round]]>103.3),5
        ),
        [@[LV or CR]]=17,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=13.5),1,
            AND(13.5<[@[Damage per Round]],[@[Damage per Round]]<=27.1),2,
            AND(27.1<[@[Damage per Round]],[@[Damage per Round]]<=54.2),3,
            AND(54.2<[@[Damage per Round]],[@[Damage per Round]]<=108.3),4,
            ([@[Damage per Round]]>108.3),5
        ),
        [@[LV or CR]]=18,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=14.2),1,
            AND(14.2<[@[Damage per Round]],[@[Damage per Round]]<=28.3),2,
            AND(28.3<[@[Damage per Round]],[@[Damage per Round]]<=56.7),3,
            AND(56.7<[@[Damage per Round]],[@[Damage per Round]]<=113.3),4,
            ([@[Damage per Round]]>113.3),5
        ),
        [@[LV or CR]]=19,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=14.8),1,
            AND(14.8<[@[Damage per Round]],[@[Damage per Round]]<=29.6),2,
            AND(29.6<[@[Damage per Round]],[@[Damage per Round]]<=59.2),3,
            AND(59.2<[@[Damage per Round]],[@[Damage per Round]]<=118.3),4,
            ([@[Damage per Round]]>118.3),5
        ),
        [@[LV or CR]]=20,
        IFS(
            ([@[Damage per Round]]=0),0,
            AND(0<[@[Damage per Round]],[@[Damage per Round]]<=16.7),1,
            AND(16.7<[@[Damage per Round]],[@[Damage per Round]]<=33.3),2,
            AND(33.3<[@[Damage per Round]],[@[Damage per Round]]<=66.7),3,
            AND(66.7<[@[Damage per Round]],[@[Damage per Round]]<=133.3),4,
            ([@[Damage per Round]]>133.3),5
        )
    ),
    "-"
)
```

#### DPR rating flavor text

With break lines

```
[@[DPR Rating Description]]
=IFS(
    ISNUMBER([@[DPR Rating]])=FALSE,"-",
    [@[DPR Rating]]=0,"☆☆☆"&CHAR(10)&"No damage",
    [@[DPR Rating]]=1,"🡇🡇🡇"&CHAR(10)&"Not helping"&CHAR(10)&"(lowest)",
    [@[DPR Rating]]=2,"★☆☆"&CHAR(10)&"Low"&CHAR(10)&"(support, control, debuff)",
    [@[DPR Rating]]=3,"★★☆"&CHAR(10)&"Target"&CHAR(10)&"(expected)",
    [@[DPR Rating]]=4,"★★★"&CHAR(10)&"High"&CHAR(10)&"(heavy hitter)",
    [@[DPR Rating]]=5,"🕱🕱🕱🕱"&CHAR(10)&"Over Powered"&CHAR(10)&"(dude stop)"
)
```


Without breaklines

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

1:
🡇🡇🡇
Not helping
(lowest)

2:
★☆☆
Low
(support, control, debuff)

3:
★★☆
Target
(expected)

4:
★★★
High
(heavy hitter)

5:
🕱🕱🕱🕱
Over Powered
(dude stop)

## Damage Type shortened

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

## Pivot Table

New Sheet: DPR Summary.

### DPR rating

The Rating Column is automatic, but no text can be output, so I'll add a manual column that has to be updated.


```
=IF(
    ISBLANK(H2)=FALSE,
    IFS(
        H2=0,
            "☆☆☆  No damage",
        AND(H2>0,H2<2),
            "🡇🡇🡇  Not helping (lowest)",
        AND(H2>=2,H2<3),
            "★☆☆  Low (support, control, debuff)",
        AND(H2>=3,H2<4),
            "★★☆  Target (expected)",
        AND(H2>=4,H2<5),
            "★★★  High (heavy hitter)",
        H2=5,
            "🕱🕱🕱🕱  Over Powered (dude stop)",
        1,"-"
    ),
    ""
)
```

I also decided to do one by one in conditional formatting:

```
=0: "☆☆☆  No damage"
<2: "🡇🡇🡇  Not helping (lowest)"
<3: "★☆☆  Low (support, control, debuff)"
<4: "★★☆  Target (expected)"
<5: "★★★  High (heavy hitter)"
=5: "🕱🕱🕱🕱  Over Powered (dude stop)"
```

But it goes away if I change the table...never mind.

### Info and notes

Added new column to add per attack to the original DPR Tactics Calculator sheet:

RoundInfo

```
=CONCATENATE(
    "Round: ",
    [@[Round]],
    CHAR(10),
    [@[Description]],
    CHAR(10),
    "Attack Times: ",
    [@[Repeat Attack Times]],
    CHAR(10),
    "Accuracy: ",
    [@[Accuracy]],
    CHAR(10),
    "Damage Roll: ",
    IF(COUNT(SEARCH("d",[@[Damage Dice]]))>0,
        [@[Damage Dice]],""),
    IF(COUNT(SEARCH("d",[@[Bonus Dmg Dice]]))>0,
        CONCATENATE("+",[@[Bonus Dmg Dice]]),""),
    IF(COUNT(SEARCH("d",[@[More Bonus Dmg Dice]]))>0,
        CONCATENATE("+",[@[More Bonus Dmg Dice]]),""),
    IF(COUNT(SEARCH("d",[@[Crit-only Mult Dmg Dice]]))>0,
        CONCATENATE("+",[@[Crit-only Mult Dmg Dice]]),""),
    IF(COUNT(SEARCH("d",[@[Crit-only Flat Dmg Dice]]))>0,
        CONCATENATE("+",[@[Crit-only Flat Dmg Dice]]),""),
    IF(COUNT(SEARCH("d",[@[Non-crit Dmg Dice]]))>0,
        CONCATENATE("+",[@[Non-crit Dmg Dice]]),""),
    IF(
        OR(
            ISNUMBER([@[Bonus Flat Damage]]),
            ISNUMBER([@[Great Weapon Master Bonus Dmg]]),
        ),
        CONCATENATE(
            "+",
            (
                IF(ISNUMBER([@[Bonus Flat Damage]]),[@[Bonus Flat Damage]],0)
                +
                IF(ISNUMBER([@[Great Weapon Master Bonus Dmg]]),[@[Great Weapon Master Bonus Dmg]],0)
            )
        ),
        ""
    )
)
```

Although I don't like it that much. Deleted, but I'll keep the formula.

BUT!

I thought of adding it to the Pivot table sheet, and reference the first sheet.

```
=IF(
    ISNUMBER(C2),
    (
        # check the latest tactic ID
        # concatenate all the descriptions for all the attacks. (replace line breaks with spaces)
    ),
    ""
)
```