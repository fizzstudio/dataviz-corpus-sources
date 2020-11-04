# Quick Guide to the Scatter Plot Indicator Format For Intentions

## Format
    {
        "name": "SP000_0.PNG",
        "primary_intention": -1,
        "secondary_intention": -1,
        "signals": {
            "trend": -1,
            "variance": -1,
            "outliers": [],
            "lineOfBestFit": {
                "hasSalientColor": false,
                "isAnnotated": false,
                "isCurved": false
            },
            "salientDatapoints": {
                "colorOrShape": -1,
                "annotation": -1,
                "annotationColorOrShape": -1
            },
            "nonstandardComparisonLines": [],
            "salientCaptionVerbs": []
        }
    }

The data file utilizes JSON to structure chart data. Each chart is represented by a JSON object in the data file's array.

## Name
It's the name. The name of each object in the data file maps almost directly to the same filename in the library subdirectory. The files within the current library are prefaced with *SP* (**S**catter **P**lot), followed by the ID of the article in which they were found (IDs can be found in librarySourceData.json). If the article contained multiple scatter plots, an underscore followed by an ID assigned to each SP in the article is appended to the end. Sometimes, an image in an article contains more than one scatter plot. In that case, an underscore and a letter (beginning with a) is appended to the end. For existing images in the library, letters are assigned from left to right, top to bottom as they appear.

So,
> "name": "SP010_3_b"

is the second scatter plot (b) in the fourth image (3, zero-based) assigned an ID in article 10. The image associated with this plot would be found in the library with the filename *SP010_3*.

## Intentions (primary and secondary)
Intentions are assigned IDs according to the following list:

| ID | Intention                                 |
|----|-------------------------------------------|
| 1  | Correlation                               |
| 2  | Negative Correlation                      |
| 3  | Non-Linear Correlation                    |
| 4  | No Correlation                            |
| 5  | Outlier                                   |
| 6  | Comparison to line of best fit            |
| 7  | Comparison between named data points      |
| 8  | Giving context to a specific data point   |
| 9  | Nonstandard comparison                    |
| 10 | Comparison between different groups/types |

## Signals
Signals are represented by yet another object with a set of properties as follows.

## Trend
Trend is the overall slope pattern found in the scatter plot. For the purposes of this analysis, trend is limited to the range [-1, 1]. -1 represents an extreme negative slope (\\), 0 represents no slope (--), 1 represents an extreme positive slope (/).

So, a scatter plot with a moderate negative slope could be assigned a trend value of -0.4.

## Variance
Variance is the overall distribution pattern found in the scatter plot in terms of how close data points are to one another and how coherently they form a pattern. Variance is assigned an ID according to the following list:

| ID | Name     |
|----|----------|
| 0  | Low      |
| 1  | Moderate |
| 2  | Extreme  |

## Outliers
Outliers are represented by an array of numbers, each representing an entry in the following list:

| ID | Name     |
|----|----------|
| 0  | Slight   |
| 1  | Moderate |
| 2  | Extreme  |

So,
> "outliers": [0, 1, 1, 1, 2, 2]

represents a chart with one slight outlier, three moderate outliers, and two extreme outliers.

And
> "outliers": []

represents a chart with no clear outliers.

## Line of Best Fit
The line of best fit is another nested object with three boolean properties. If there is no line of best fit present, the object is assigned `null`.

| Property        | Criteria                                                                         |
|-----------------|----------------------------------------------------------------------------------|
| hasSalientColor | Is the line of best fit an obviously different color from the rest of the graph? |
| isAnnotated     | Is the line of best fit labelled?                                                |
| isCurved        | Is the line of best fit curved?                                                  |

So, 

    "lineOfBestFit": {
        "hasSalientColor": true,
        "isAnnotated": false,
        "isCurved": true
    }

represents a chart with a curved line of best fit with salient color.

And
> "lineOfBestFit": null

represents a chart with no line of best fit.

## Salient Data Points
Yet another nested object. This one has three properties and is `null` if there are no salient data points.

| Property               | Criteria                                                                                          |
|------------------------|---------------------------------------------------------------------------------------------------|
| colorOrShape           | How many data points have an obviously different color or shape than the rest of the data points? |
| annotation             | How many data points are labelled?                                                                |
| annotationColorOrShape | How many data points are labelled and have an obviously differently colored or shaped label?      |

Each property is assigned an ID according to the number of data points which fulfill the criteria of the property.

| ID | Name |
|----|------|
| 0  | None |
| 1  | One  |
| 2  | Few  |
| 3  | Many |

So,

    "salientDatapoints": {
        "colorOrShape": 2,
        "annotation": 1,
        "annotationColorOrShape": 0
    }

represents a chart with one annotated data point and (relatively) few data points which stand out due to their color or shape.

And
> "salientDatapoints": null

represents a chart with no salient data points.

## Nonstandard Comparison Lines
These are lines of comparison in a chart that are not lines of best fit, the chart's axes, or a gridline. They are represented with an array of strings, each containing the label or stated meaning of each nonstandard comparison made in the chart.

So,
> "nonstandardComparisonLines": ["average"]

represents a chart with a nonstandard comparison line which represents *average*.

And
> "nonstandardComparisonLines": []

represents a chart with no nonstandard comparison lines.

## Salient Caption Verbs
These are words or phrases in a caption for a chart that tell you something about the chart's intentions. They are represented with an array of strings, each containing the meaningful word or phrase found in the caption.

So,
> "salientCaptionVerbs": ["stands alone"]

represents a chart with a caption that mentions a data point *standing alone*.

And
> "salientCaptionVerbs": []

represents a chart with no caption or a caption with no relevant words or phrases.
