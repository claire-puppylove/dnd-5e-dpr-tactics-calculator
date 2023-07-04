# Pivot Table

New Sheet: DPR Summary.

## DPR rating

The Rating Column is automatic, but no text can be output, so I'll add a manual column that has to be updated.

```
=IF(
    AND(
        ISBLANK(L4)=FALSE,
        ISNUMBER($B4)=FALSE
    ),
    IFS(
        L4=0,
            "☆☆☆☆  No damage",
        AND(L4>0,L4<2),
            "★☆☆☆  Lowest (not helping)",
        AND(L4>=2,L4<3),
            "★★☆☆  Low (support, control, debuff)",
        AND(L4>=3,L4<4),
            "★★★☆  Target (expected)",
        AND(L4>=4,L4<5),
            "★★★★  High (heavy hitter)",
        L4=5,
            "🕱🕱🕱🕱🕱  Deadly",
        1,"-"
    ),
    ""
)
```

I also decided to do one by one in conditional formatting:

```
=0: "☆☆☆☆  No damage"
<2: "★☆☆☆  Lowest (not helping)"
<3: "★★☆☆  Low (support, control, debuff)"
<4: "★★★☆  Target (expected)"
<5: "★★★★  High (heavy hitter)"
=5: "🕱🕱🕱🕱🕱  Deadly"
```

But it goes away if I change the table...never mind.