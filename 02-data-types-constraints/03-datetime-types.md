# Date/Time Types

## Types Overview

| Type | Description | Example |
|------|-------------|---------|
| `DATE` | Date only | 2024-01-15 |
| `TIME` | Time only | 14:30:00 |
| `TIMESTAMP` | Date and time | 2024-01-15 14:30:00 |
| `TIMESTAMPTZ` | Timestamp with timezone | 2024-01-15 14:30:00+05:30 |
| `INTERVAL` | Time duration | 1 day 2 hours |

## DATE

```sql
CREATE TABLE events (
    event_date DATE
);

INSERT INTO events VALUES ('2024-01-15');
INSERT INTO events VALUES (CURRENT_DATE);

SELECT CURRENT_DATE;  -- Today's date
```

## TIME

```sql
CREATE TABLE schedule (
    start_time TIME,
    end_time TIME
);

INSERT INTO schedule VALUES ('09:00:00', '17:30:00');
SELECT CURRENT_TIME;
```

## TIMESTAMP vs TIMESTAMPTZ

```sql
-- Without timezone (stores as-is)
CREATE TABLE logs (
    created_at TIMESTAMP
);

-- With timezone (converts to UTC, displays in local)
CREATE TABLE events (
    created_at TIMESTAMPTZ
);

-- Always prefer TIMESTAMPTZ for real applications
INSERT INTO events VALUES (NOW());
INSERT INTO events VALUES ('2024-01-15 14:30:00+05:30');
```

## INTERVAL

```sql
-- Duration between times
SELECT INTERVAL '1 year 2 months 3 days';
SELECT INTERVAL '4 hours 30 minutes';

-- Arithmetic with intervals
SELECT NOW() + INTERVAL '7 days';           -- 1 week from now
SELECT NOW() - INTERVAL '1 month';          -- 1 month ago
SELECT '2024-12-31'::DATE - '2024-01-01'::DATE;  -- Days between
```

## Useful Functions

```sql
-- Current date/time
SELECT CURRENT_DATE;
SELECT CURRENT_TIME;
SELECT CURRENT_TIMESTAMP;
SELECT NOW();

-- Extract parts
SELECT EXTRACT(YEAR FROM NOW());
SELECT EXTRACT(MONTH FROM NOW());
SELECT EXTRACT(DAY FROM NOW());
SELECT EXTRACT(DOW FROM NOW());  -- Day of week (0=Sunday)

-- Date truncation
SELECT DATE_TRUNC('month', NOW());  -- First day of month
SELECT DATE_TRUNC('year', NOW());   -- First day of year

-- Formatting
SELECT TO_CHAR(NOW(), 'YYYY-MM-DD');
SELECT TO_CHAR(NOW(), 'HH24:MI:SS');
SELECT TO_CHAR(NOW(), 'Day, DD Month YYYY');

-- Parsing
SELECT TO_DATE('15-01-2024', 'DD-MM-YYYY');
SELECT TO_TIMESTAMP('2024-01-15 14:30', 'YYYY-MM-DD HH24:MI');
```

## Age Calculation

```sql
SELECT AGE(NOW(), '1990-05-15');
-- Result: 33 years 8 mons 5 days

SELECT AGE('2024-01-01', '2023-06-15');
-- Result: 6 mons 17 days
```

## Timezone Handling

```sql
-- Show current timezone
SHOW timezone;

-- Set timezone for session
SET timezone = 'Asia/Kolkata';
SET timezone = 'UTC';

-- Convert between timezones
SELECT NOW() AT TIME ZONE 'UTC';
SELECT NOW() AT TIME ZONE 'America/New_York';
```
