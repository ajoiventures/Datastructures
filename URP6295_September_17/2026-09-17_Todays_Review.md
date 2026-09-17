---
type: review
course: URP6295
date: 2026-09-17
status: current
source: 9_17_26/2026-09-17_Source_Transcription
---

# September 17 Today's Review

## The big picture

The screenshots show a complete data-analysis workflow:

1. Inspect what the data actually is.
2. Put the data into a useful structure.
3. Apply calculations to many values.
4. Compare observations or locations.
5. Make a graph that a person can interpret.
6. Record metadata so the result can be trusted and reproduced.

## Lesson 1 Debugging starts with inspection

When code gives a surprising result, ask the program questions before changing the code.

~~~python
import numpy as np

values = np.array([10, 20, 30])

print(type(values))
print(values.shape)
print(values.dtype)
print(f"First value: {values[0]}")
~~~

The type tells you what kind of object you have. The shape tells you its dimensions. The dtype tells you what kind of values are stored. The f-string creates a readable diagnostic message.

For a harder problem, pause execution:

~~~python
import pdb

total = 0
for value in values:
    pdb.set_trace()
    total += value
print(total)
~~~

At the breakpoint, inspect variables such as value and total. A debugger lets you see the program between steps instead of only seeing the final error.

## Lesson 2 NumPy arrays organize numeric data

NumPy arrays are structured containers for numerical values.

~~~python
import numpy as np

array_1d = np.array([1, 2, 3, 4])
array_1by4 = np.array([[1, 2, 3, 4]])

print(array_1d.shape)      # (4,)
print(array_1by4.shape)    # (1, 4)
~~~

The extra brackets create a second dimension. A larger sequence can become a grid:

~~~python
large_array = np.array([i for i in range(400)])
grid = large_array.reshape((20, 20))

print(grid.shape)          # (20, 20)
print(grid[0, 0])          # first row, first column
~~~

Reshape does not create new values. It reorganizes the existing 400 values. The requested dimensions must multiply to 400.

## Lesson 3 Array-wide calculations

NumPy applies many operations to every element at once.

~~~python
array_1 = np.array([1, 4, 9, 16])

print(array_1 + 5)
print(array_1 * 5)
print(np.sqrt(array_1))
print(np.power(array_1, 2))
print(np.exp(array_1))
print(np.log(array_1))
~~~

This is a vectorized operation. It replaces many small manual calculations with one clear array operation.

Check mixed values before doing numerical work:

~~~python
numbers = np.array([1, 2, 3.0])
mixed = np.array([1, 2, 3, 'a'])

print(numbers.dtype)
print(mixed.dtype)
~~~

A mixed numeric-and-text array may receive a dtype that is unsuitable for arithmetic.

## Lesson 4 Broadcasting compares many observations

The example creates 15 observations with 5 features each.

~~~python
samples = np.random.random((15, 5))
print(samples.shape)       # (15, 5)
~~~

Pairwise distance asks how far every observation is from every other observation.

~~~python
diff = samples[:, np.newaxis, :] - samples[np.newaxis, :, :]
print(diff.shape)          # (15, 15, 5)

distances = np.linalg.norm(diff, axis=-1)
print(distances.shape)     # (15, 15)
print(np.diag(distances))  # distances from each row to itself
~~~

The new axes let NumPy align every first row with every second row. The difference array stores five feature differences for each pair. The norm reduces those five differences to one distance.

SciPy provides another route:

~~~python
from scipy.spatial.distance import cdist

distances_with_scipy = cdist(samples, samples)
~~~

If rows are neighborhoods and columns are indicators, the result can help identify neighborhoods with similar profiles. Standardize features first when their units or scales differ.

## Lesson 5 Plotting turns calculations into evidence

The plotting workflow is import, create data, plot, format, and save or show.

~~~python
import numpy as np
import matplotlib.pyplot as plt

t = np.arange(0.0, 2.0, 0.01)
s = 1 + np.sin(2 * np.pi * t)

fig, ax = plt.subplots()
ax.plot(t, s)
ax.set(
    xlabel='time (s)',
    ylabel='voltage (mV)',
    title='About as simple as it gets, folks'
)
ax.grid()
fig.savefig('test.png')
plt.show()
~~~

Labels and units are part of the analysis. In urban work, identify what is measured, where it is measured, and over what period.

## Lesson 6 Urban data comes in upddate git
many structures

Urban analysis may use Census and American Community Survey tables, GIS shapefiles, mobility flows, spatiotemporal ridership, travel surveys, imagery, text, points of interest, public-health data, air-pollution data, energy data, and web-scraped records.

A simple record structure might look like this:

~~~python
neighborhoods = [
    {'name': 'North', 'population': 12000, 'transit_riders': 3400},
    {'name': 'South', 'population': 9000, 'transit_riders': 2100},
]

for neighborhood in neighborhoods:
    rate = neighborhood['transit_riders'] / neighborhood['population']
    print(f"{neighborhood['name']}: {rate:.2%}")
~~~

The data structure should match the question. A shapefile supports geographic boundaries. A table supports attributes. A time series supports trends. An image requires different tools from a numeric array.

## Lesson 7 Metadata makes data usable

Metadata describes the creator, fields, units, geographic coverage, time period, update date, collection method, license, and limitations.

A simple metadata record can be represented as a dictionary:

~~~python
dataset_metadata = {
    'name': 'Transit Ridership',
    'publisher': 'City Data Portal',
    'geography': 'City limits',
    'time_period': '2020-2025',
    'units': 'boardings per month',
    'update_date': '2026-08-01',
}

for key, value in dataset_metadata.items():
    print(f"{key}: {value}")
~~~

Before analysis, ask who created the data, what one row or feature represents, what the fields mean, what units are used, what places and dates are included, how missing values are represented, and what limitations apply.

## Lesson 8 Census and ACS tables describe places

In a Census or American Community Survey table, each row may represent a census tract and each column may represent a variable. The row key tells you which place is being described. The column names and codebook tell you what each measurement means.

~~~python
import pandas as pd

acs = pd.read_csv('acs_data.csv')
print(acs.shape)
print(acs.columns.tolist())
print(acs[['tract_id', 'total_population']].head())
~~~

Do not treat a coded field as self-explanatory. Confirm whether it is a count, percentage, estimate, margin of error, or category code. Check the geography and year before comparing rows.

## Lesson 9 GIS sources provide geometry and attributes

GIS sources such as Census TIGER data, local government GIS portals, and OpenStreetMap can provide boundaries, roads, transit lines, water, land use, and points of interest. GIS analysis joins a location's geometry to descriptive attributes.

~~~python
import geopandas as gpd

tracts = gpd.read_file('census_tracts.shp')
print(tracts.shape)
print(tracts.geometry.geom_type.value_counts())
tracts.plot()
~~~

The geometry answers where. The attribute columns answer what. A map can look correct while still using the wrong coordinate system, geography, or date, so inspect the source metadata.

## Lesson 10 Travel surveys describe trips

Travel surveys are often collected from individuals or households and provide trip-level information such as origin, destination, travel time, cost, purpose, and mode. The screenshot names the National Household Travel Survey, state add-ons, Chicago Metropolitan Agency for Planning surveys, and the Google Distance Matrix API.

~~~python
trips = pd.read_csv('travel_survey.csv')
mode_counts = trips['mode'].value_counts()
average_time = trips.groupby('mode')['travel_time_minutes'].mean()
print(mode_counts)
print(average_time)
~~~

The unit of analysis matters. One row may be one trip, one person, or one household. Mixing those units can produce incorrect rates or totals.

## Lesson 11 Points of interest connect destinations to place

A point of interest is a mapped destination such as a school, clinic, station, park, or store. A POI dataset normally includes coordinates, a category, a name, and a source.

~~~python
pois = gpd.read_file('points_of_interest.geojson')
schools = pois[pois['category'] == 'school']
print(schools[['name', 'geometry']].head())
~~~

POI analysis can count destinations, measure access, find the nearest service, or compare neighborhoods. State the distance measure, search radius, category rules, and date so the result is reproducible.

## Lesson 12 Using new data with an existing algorithm

The final screenshot gives a practical open-source checklist. Read the README, follow the installation instructions, install only the required dependencies, look for obvious problems and recent changes, run a small test on your own computer, and document what worked.

~~~text
1. Read README.md.
2. Create or activate the required environment.
3. Install the minimum dependencies.
4. Run the smallest example first.
5. Add the new dataset only after the example works.
6. Record versions, settings, and output.
~~~

This is reproducibility. A result is stronger when another person can rebuild the environment and repeat the steps. Do not install a package merely because it appears in a repository if the current task does not need it.

## Lesson 13 Public and private data have different workflows

Public data is often fragmented. Different groups may use different formats, collection equipment, cleaning habits, documentation, and upload practices. There may be no single loading tool that works for every source.

Start with a small preview:

~~~python
import pandas as pd

public_data = pd.read_csv('public_dataset.csv')
print(public_data.shape)
print(public_data.head())
print(public_data.dtypes)
~~~

Reading and printing a small preview can reveal wrong headers, missing values, unexpected types, or a file that is not the dataset you expected.

Private data is protected and may require permission, contracts, or internal credentials. Large organizations often standardize their internal tools and pipelines, but access to customer identifiers and purchases remains restricted. In urban analysis, treat privacy, access rights, and re-identification risk as part of the data workflow.

The practical lesson is to ask two questions before analysis: how can I access this data legitimately, and how consistent and documented is the pipeline that produced it?

## How the lessons connect

| Stage | Tool or concept | Beginner question |
|---|---|---|
| Inspect | type, shape, dtype, debugger | What do I actually have? |
| Organize | lists, arrays, reshape | How is the data arranged? |
| Calculate | vectorized NumPy operations | What should happen to every value? |
| Compare | broadcasting, norms, pairwise distance | How are observations related? |
| Communicate | Matplotlib, labels, units | Can another person understand the result? |
| Document | metadata | Why should anyone trust or reproduce it? |

## Common mistakes to avoid

- Assuming a printed value tells you the full type or shape.
- Reshaping without checking that the dimensions fit the number of values.
- Mixing text and numbers in a numeric array without checking dtype.
- Treating a distance matrix as meaningful before checking units and feature scaling.
- Making a graph without labels, units, or a clear time period.
- Using a dataset without reading its metadata.
- Confusing a source description with the actual observations.

## Practice for today's review

1. Explain what shape (15, 5) means in your own words.
2. Predict the shape of diff before the norm reduces it.
3. Describe the difference between a Python list and a NumPy array.
4. Run the plotting example and identify its five stages.
5. Choose a housing or transportation dataset and list five metadata questions.
6. Explain one urban question that needs geometry, one that needs time, and one that needs both.
7. Use type, shape, and dtype as a three-step inspection routine.

## Running glossary

- Array: A structured collection of values arranged in one or more dimensions.
- Shape: The size of an array along each dimension.
- Dtype: The type of values stored in a NumPy array.
- Reshape: Rearranging values into a new dimension layout.
- Vectorized operation: One array-wide instruction applied to many values.
- Broadcasting: Aligning compatible dimensions for an array operation.
- Pairwise distance: A distance calculated for every pair of observations.
- Norm: A calculation that converts a difference vector into a magnitude or distance.
- Plot: A visual representation of data.
- Metadata: Information describing a dataset, its source, fields, units, time, place, and limits.
- GIS shapefile: A common file set for geographic features and their attributes.
- Spatiotemporal data: Data that varies across space and time.
- Mobility flow: Movement between locations.
- Point of interest: A mapped location with a meaningful use or destination.
- Breakpoint: A location where a debugger pauses program execution.
- F-string: A formatted Python string that inserts values into text.
- Census tract: A small geographic area used for Census statistics.
- American Community Survey: A Census program providing demographic, housing, social, and economic estimates.
- TIGER data: Census geographic boundaries and features used in GIS.
- Travel survey: Data about trips, travel time, cost, purpose, and mode.
- Provenance: The record of where data came from and how it was prepared.
- Reproducibility: The ability to repeat documented steps and obtain the same result.
- Dependency: A software component required for a program to run.
- Public data: Data available outside a private organization, often with varied formats and documentation quality.
- Private data: Protected data whose access is controlled by an organization or agreement.
- Data pipeline: The sequence of steps used to collect, clean, transform, analyze, and store data.
- Data preview: A small inspection of rows, columns, types, and shape before full analysis.
- Access control: Rules that determine who may view or use data.

## Final review habit

Use this sequence every time:

inspect -> organize -> calculate -> visualize -> document

Before trusting a result, explain what the data is, how it is shaped, what operation was applied, what the output means, and what metadata supports the interpretation.
