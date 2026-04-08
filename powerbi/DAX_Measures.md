# DAX Measures — Power BI Dashboard

All financial KPIs are implemented as DAX measures using SUMX and SUBSTITUTE to handle the European numeric format (comma decimal separator) inherited from SAP exports.

## Pattern Used
```dax
SUMX(Query1, VALUE(SUBSTITUTE(Query1[ColumnName], ",", ".")))
```

## Financial KPIs

### IGM% Actual
```dax
IGM% Actual = 
DIVIDE(
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[VIPP IGM], ",", "."))),
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Net Net Value], ",", "."))),
    0
)
```

### Cost Overrun %
```dax
Cost Overrun % = 
DIVIDE(
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Total costs incl. Comm.], ",", "."))) - 
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Budget], ",", "."))),
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Budget], ",", "."))),
    0
)
```

### Billed %
```dax
Billed % = 
DIVIDE(
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Billed Value], ",", "."))),
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Net Net Value], ",", "."))),
    0
)
```

### Unbilled Exposure
```dax
Unbilled Exposure = 
SUMX(Query1, VALUE(SUBSTITUTE(Query1[Unbilled Value], ",", ".")))
```

### VIPP Consistency Check
```dax
VIPP Consistency Check = 
SUMX(Query1, VALUE(SUBSTITUTE(Query1[VIPP Cost of Sales], ",", "."))) - 
SUMX(Query1, VALUE(SUBSTITUTE(Query1[Total costs incl. Comm.], ",", ".")))
```

### Revenue Plan Accuracy
```dax
Revenue Plan Accuracy = 
DIVIDE(
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Net Net Value], ",", "."))),
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Revenue Plan pv2], ",", "."))),
    0
)
```

## Cost Structure KPIs

### Labour %
```dax
Labour % = 
DIVIDE(
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Labour], ",", "."))),
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Total costs incl. Comm.], ",", "."))),
    0
)
```

### Material %
```dax
Material % = 
DIVIDE(
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Material_1], ",", "."))),
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Total costs incl. Comm.], ",", "."))),
    0
)
```

### Service %
```dax
Service % = 
DIVIDE(
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Service], ",", "."))),
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Total costs incl. Comm.], ",", "."))),
    0
)
```

### Accrual %
```dax
Accrual % = 
DIVIDE(
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Accrual], ",", "."))),
    SUMX(Query1, VALUE(SUBSTITUTE(Query1[Total costs incl. Comm.], ",", "."))),
    0
)
```

## Operational KPIs

### Delay Flag Count
```dax
Delay Flag Count = 
COUNTX(
    VALUES(Query1[WBS Element]),
    VAR CurrentDate = MAX(Query1[Snapshot_Period])
    VAR PreviousDate = CALCULATE(MAX(Query1[Snapshot_Period]), Query1[Snapshot_Period] < CurrentDate)
    VAR CurrentPlanned = CALCULATE(MAX(Query1[Planned Recogn. Date]), Query1[Snapshot_Period] = CurrentDate, Query1[Planned Recogn. Date] <> "#")
    VAR PreviousPlanned = CALCULATE(MAX(Query1[Planned Recogn. Date]), Query1[Snapshot_Period] = PreviousDate, Query1[Planned Recogn. Date] <> "#")
    VAR CurrentPlannedDate = IF(NOT ISBLANK(CurrentPlanned), DATEVALUE(SUBSTITUTE(CurrentPlanned, ".", "/")))
    VAR PreviousPlannedDate = IF(NOT ISBLANK(PreviousPlanned), DATEVALUE(SUBSTITUTE(PreviousPlanned, ".", "/")))
    RETURN IF(AND(NOT ISBLANK(CurrentPlannedDate), NOT ISBLANK(PreviousPlannedDate)), IF(CurrentPlannedDate > PreviousPlannedDate, 1, BLANK()), BLANK())
)
```

### Forecast Shift Days
```dax
Forecast Shift Days = 
VAR CurrentDate = MAX(Query1[Snapshot_Period])
VAR PreviousDate = CALCULATE(MAX(Query1[Snapshot_Period]), Query1[Snapshot_Period] < CurrentDate)
RETURN
AVERAGEX(
    VALUES(Query1[WBS Element]),
    VAR CurrentPlanned = CALCULATE(MAX(Query1[Planned Recogn. Date]), Query1[Snapshot_Period] = CurrentDate, Query1[Planned Recogn. Date] <> "#")
    VAR PreviousPlanned = CALCULATE(MAX(Query1[Planned Recogn. Date]), Query1[Snapshot_Period] = PreviousDate, Query1[Planned Recogn. Date] <> "#")
    VAR CurrentPlannedDate = IF(NOT ISBLANK(CurrentPlanned), DATEVALUE(SUBSTITUTE(CurrentPlanned, ".", "/")))
    VAR PreviousPlannedDate = IF(NOT ISBLANK(PreviousPlanned), DATEVALUE(SUBSTITUTE(PreviousPlanned, ".", "/")))
    RETURN IF(AND(NOT ISBLANK(CurrentPlannedDate), NOT ISBLANK(PreviousPlannedDate)), DATEDIFF(PreviousPlannedDate, CurrentPlannedDate, DAY), BLANK())
)
```
