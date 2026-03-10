## Pandas Basics
![[Screenshot 2025-01-30 at 8.16.44 PM.png]]

## Tidy Data

Why do we normalize? To avoid anomalies

- **Variable**- A quantity, quality, or property that you can measure
- **Value**- The state of a variable when you measure it (data)
- **Observation** - The values of several variables measured under similar conditions.

**Two uses of Data**: (1) Visualization; (2) Prediction

**Definition of Tidy Data:**
1. Each variable forms a column
2. Each observation forms a row
3. Each type of observational unit forms a table

**Five Reasons for Untidyness**
1. Column headers are values, not variable names
2. Multiple variables are stored in one column
3. Variables are stored in both rows and columns
4. Multiple types of observational units are stored in the same table
5. A single observational unit is stored in multiple tables

## Reshaping Dataframes
### `melt`

![[Screenshot 2025-01-30 at 8.46.47 PM.png]]
### `pivot / pivot_table`

```
pd.pivot(df, 
		 index=None, 
		 columns=None, 
		 values=None)
```

- **`df`** → Input DataFrame.
- **`index`** → Column(s) to set as the new index.
- **`columns`** → Column(s) to spread across the columns.
- **`values`** → Column(s) to fill the table with.

```
df.pivot_table(index='Name', 
			   columns='Exam', 
			   values='Score', 
			   aggfunc=lambda s: s.mean())
```

![[Screenshot 2025-01-30 at 9.28.42 PM.png]]
### `unstack`

### `stack`