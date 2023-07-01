# Pivot Table

New Sheet: DPR Summary.

## DPR rating

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

## Info and notes

Added a column to add per attack to the original DPR Tactics Calculator sheet:

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

Although I didn't like it that much. Deleted, but I'll keep the formula.

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