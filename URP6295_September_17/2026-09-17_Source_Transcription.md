---
type: source-transcription
course: URP6295
date: 2026-09-17
---

# September 17 Source Transcription

This file consolidates the six screenshot pages currently in the September 17 folder.

## Screenshot 1 Debugging tools

The slide lists practical Python checks:

- `array.shape`: find the dimensions of a NumPy array.
- `array.dtype`: check the array's data type, especially when precision or unexpected behavior matters.
- `type(stuff)`: find the type of a variable.
- `import pdb; pdb.set_trace()`: pause execution at a breakpoint and inspect the program.
- `print(f"My name is {name}")`: build a readable message with an f-string.

The main insight is that debugging starts with asking the data and program what they actually contain.

## Screenshot 2 Basic NumPy usage

NumPy arrays can be created from Python lists:

```python
array_1d = np.array([1, 2, 3, 4])
array_1by4 = np.array([[1, 2, 3, 4]])
large_array = np.array([i for i in range(400)])
large_array = large_array.reshape((20, 20))
```

NumPy can also hold mixed numeric types, but values are normally cast to a common precision. A list containing incompatible types, such as numbers and a string, should be treated as a warning sign.

Array-wide operations include addition, multiplication, square root, powers, exponential, and logarithm. Instead of writing a loop for every element, NumPy applies the operation across the array.

## Screenshot 3 Broadcasting for pairwise distance

The example creates 15 observations with 5 features:

```python
samples = np.random.random((15, 5))
```

Pairwise distance means the distance between every observation and every other observation. One approach manually expands and tiles the arrays, subtracts them, and computes the norm. Broadcasting expresses the same relationship more directly:

```python
diff = samples[:, np.newaxis, :] - samples[np.newaxis, :, :]
distances = np.linalg.norm(diff, axis=-1)
```

SciPy can provide the same type of result with a distance function. The insight is that broadcasting aligns dimensions so NumPy can compare all pairs without writing nested Python loops.

## Screenshot 4 Plotting

The plotting workflow is:

1. Import the plotting and numeric libraries.
2. Create the data.
3. Plot the data.
4. Format the plot with labels, a title, and a grid.
5. Save or show the figure.

Example:

```python
t = np.arange(0.0, 2.0, 0.01)
s = 1 + np.sin(2 * np.pi * t)
fig, ax = plt.subplots()
ax.plot(t, s)
ax.set(xlabel='time (s)', ylabel='voltage (mV)',
       title='About as simple as it gets, folks')
ax.grid()
fig.savefig("test.png")
plt.show()
```

The graph turns a sequence of values into a visual pattern. Labels and units make the result interpretable.

## Screenshot 5 Examples of urban data

The slide names data types common in urban analytics:

- Metadata sets
- Socio-demographic data from the Census or American Community Survey
- GIS shapefiles
- Mobility flows
- Spatiotemporal ridership
- Travel surveys
- Urban imagery, graphs, and text, including satellite imagery
- Points of interest
- Mobility-related public-health, air-pollution, and energy data
- Web scraping

The list is broad because urban questions often require several data structures and sources at once.

## Screenshot 6 Metadata sets

Metadata is information about a data source. It is commonly collected by universities, research centers, governments, websites, and individual researchers. The slide gives examples such as Northwestern University library guides, the City of Chicago Data Portal, the NREL Transport Data Center, and Kaggle.

Metadata helps answer: Who created the data? What does each field mean? What area and time period does it cover? How should it be accessed and cited? It is often ignored, but it is essential for using a dataset correctly.

## Newly added screenshots

### Screenshot 7 Kaggle dataset

Kaggle is shown as a place to find public datasets. The lesson is to inspect the dataset description, fields, files, license, and provenance before using it. A dataset page is a starting point for discovery, not automatic proof that the data is complete or appropriate.

### Screenshot 8 Census ACS data example

The example is a table in which rows represent spatial units such as census tracts and columns represent variables. The table illustrates how many coded fields can describe population, housing, or socioeconomic conditions. The row geography and the meaning of each column must be documented before analysis.

### Screenshot 9 GIS shapefiles

The slide names TIGER data from the Census Bureau, local GIS information from universities and governments, and OpenStreetMap. These sources can provide boundaries, roads, transit, amenities, land use, water, and other geographic features. GIS data combines geometry with attributes.

### Screenshot 10 Travel surveys

Travel surveys are commonly conducted at the individual level and provide trip-level information such as travel time, travel cost, and travel mode. Examples include the National Household Travel Survey, state add-ons, Chicago Metropolitan Agency for Planning surveys, and Google Distance Matrix API data for filling missing travel information.

### Screenshot 11 Points of interest

The point-of-interest example shows mapped places and nearby attributes. A point of interest can be a school, store, park, clinic, station, or other destination. The analysis depends on the point coordinates, the category, the source, and the spatial relationship being measured.

### Screenshot 12 Adding new data to an existing model or algorithm

The final slide describes a checklist for using an open-source GitHub repository: read the README installation instructions, install the minimum required dependencies, check obvious problems and recent commits, reproduce the work on a personal computer, and avoid installing a package that is not needed. The insight is that data analysis includes reproducibility and environment management, not only writing new code.

### Screenshot 13 Public versus private data

Public data usually has no single unified loading tool. It arrives in different forms for different tools, and different research groups may collect, clean, document, and upload it differently. A quick read-and-print preview is a practical first check before investing in a full analysis.

Private data is protected and often difficult to access, but large companies may have a unified internal workflow. The slide uses Amazon as an example: teams may use shared tools such as AWS or SageMaker, while access to customer identifiers and purchases is controlled. The key contrast is open-data fragmentation versus private-data access controls and standardized internal pipelines.

### Screenshot 14 Clean tables and maps

The Python lab question asks how to turn a large Florida table into a clean table and maps. The example contains population and demographic fields, ratio fields, missing values, and maps of median property value and median household income.

### Screenshot 15 NumPy arrays and pandas DataFrames

The slide compares the same ridership values in a NumPy array and a pandas DataFrame. The array uses numeric positions such as row 3 and column 8. The DataFrame uses labels such as Thursday and 08:00. A DataFrame is a two-dimensional array with row and column names.

### Screenshot 16 Four key elements of a table

Every table needs a clear observation unit, defined columns and dtypes, a key that identifies rows, and missing-value rules. The analyst should be able to finish the sentence one row is one ____. A join or group-by operation depends on the key; if the key is wrong, the code may still run while producing incorrect results.

### Screenshot 17 Parcel and permit tables

The example shows a parcel table with parcel_id, tract_geoid, year_built, heated_sqft, just_value, and use_code. A second permit table includes permit_id, parcel_id, issued_date, permit_type, and valuation. The shared parcel_id connects permits back to parcels. The year_built example includes NaN, demonstrating a missing value that requires an explicit rule.

### Screenshot 18 Parcel and permit example repeated

The example reinforces the relationship between parcel records and permit records. parcel_id links the tables, while tract_geoid connects a parcel to a census geography. The fields have different meanings and units, so the table must not be treated as one undifferentiated block of numbers.

### Screenshot 19 Missing values are not all the same

The slide distinguishes NaN, an empty string, 0, and -999. NaN may mean not recorded or lost in a join. An empty string may mean a blank form entry and may be treated as a value rather than missing. Zero is usually a genuine measured zero or default value. -999 may be a sentinel from an older system. Only NaN is automatically treated as missing by common pandas operations; the other codes need an explicit cleaning rule.
