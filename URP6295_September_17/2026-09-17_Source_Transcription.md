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

