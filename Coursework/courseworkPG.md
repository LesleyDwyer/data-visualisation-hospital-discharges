---
follows: courseworkPG_Data
id: litvis

---

@import "../css/datavis.less"

# Postgraduate Coursework Template

{(questions|}

What trends or patterns can be found for different patient groups discharged from New York hospitals in 2016? More specifically:
  1) How are patients in different age groups admitted to hospitals?
  2) What trends can be seen for age groups and diagnoses?
  3) What does the length of stay look like for different age groups?
  4) How do charges and payment type vary by age group?

{|questions)}

{(visualization|}

```elm {v interactive}
ageAdm : Spec
ageAdm =
    let
        data =
            dataFromColumns []
                << dataColumn "Age Group" (strColumn "ageGroup" ageAdmTable |> strs)
                << dataColumn "Admission Type" (strColumn "admissionType" ageAdmTable |> strs)
                << dataColumn "Patient Discharges" (numColumn "count" ageAdmTable |> nums)

        cfg =
            configure
                << configuration
                    (coAxis
                        [axcoLabelAngle 0])

        transSelection =
            transform
                << filter (fiSelection "mySelection")

        sel =
            selection << select "mySelection"
                seSingle
                [ seFields ["Admission Type"]
                , seNearest True
                , seBind
                    [iSelect "Admission Type"
                        [inName "Select Admission Type "
                        , inOptions
                            ["Elective"
                            , "Emergency"
                            , "Newborn"
                            , "Not Available"
                            , "Trauma"
                            , "Urgent"
                            ]
                        ]
                    ]
                ]

        enc =
            encoding
                << position X [ pName "Age Group", pMType Nominal, pAxis [ axTitle "" ]
                , pScale [ scPaddingInner 0.4] ]
                << position Y [ pName "Patient Discharges", pMType Quantitative ]
                << color [ mName "Admission Type", mMType Nominal ]

    in
    toVegaLite
        [title "1) Hospital Admission Type by Age Group"
        , width 350
        , height 275
        , data []
        , enc []
        , sel []
        , transSelection []
        , bar []
        , cfg []
        ]

```

```elm {v interactive}
ageDiag : Spec
ageDiag =
    let
        data2 =
            dataFromColumns []
                << dataColumn "Diagnostic Category" (strColumn "MDC" ageDiagTable |> strs)
                << dataColumn "Age Group" (strColumn "ageGroup" ageDiagTable |> strs)
                << dataColumn "Count" (numColumn "count" ageDiagTable |> nums)

        cfg =
            configure
                << configuration
                    (coAxis
                        [axcoLabelAngle 0
                        , axcoLabelLimit 150
                        , axcoTitlePadding 10
                        ])

        enc =
            encoding
                << position X [ pName "Age Group", pMType Ordinal, pAxis [ axTitle "" ]  ]
                << position Y [ pName "Diagnostic Category", pMType Nominal
                    , pAxis [ axTitle "Major Diagnostic Category", axLabelAlign haLeft, axLabelPadding 150 ] ]
                << color [ mName "Age Group", mMType Ordinal]
                << size
                [ mName "Count"
                    , mMType Quantitative
                    , mLegend [ leTitle "Patient Discharges" ]
                    , mScale [ scRange (raNums [ 10, 1200 ]) ] --Sets the size range of circles
                    ]
    in
    toVegaLite
    [ title "2) Major Diagnostic Category by Age Group"
    , width 250
    , height 650
    , data2 []
    , circle []
    , cfg []
    , enc [] ]

```

```elm {v interactive}
ageLOS : Spec
ageLOS =
    let
        data3 =
            dataFromColumns []
                << dataColumn "Age Group" (strColumn "ageGroup" ageLOSTable |> strs)
                << dataColumn "Length of Stay" (numColumn "LOS" ageLOSTable |> nums)
                << dataColumn "Count" (numColumn "count" ageLOSTable |> nums)

        enc =
            encoding
                << position X [ pName "Length of Stay", pMType Quantitative, pTitle "Length of Stay (in Days, 30 is 30 or more days)"
                , pScale [ scDomain (doNums [ 1, 29 ]) ] ]
                << position Y [ pName "Count", pMType Quantitative, pAxis [ axGrid False],  pTitle "" ]
                << row [ fName "Age Group", fMType Ordinal, fHeader [ hdTitle "" ]  ]
                << color [ mName "Age Group", mMType Ordinal]

    in
    toVegaLite
        [title "3) Length of Stay by Age Group"
        , width 250
        , height 100
        , data3 []
        , enc []
        , bar []
        ]
```

```elm {v interactive}
ageCost : Spec
ageCost =
    let
        data4 =
            dataFromColumns []
                << dataColumn "Age Group" (strColumn "ageGroup" ageCostTable |> strs)
                << dataColumn "Primary Payment Type" (strColumn "payment1" ageCostTable |> strs)
                << dataColumn "Charges" (numColumn "charges" ageCostTable |> nums)

        transSelection =
            transform
                << filter (fiSelection "mySelection")

        cfg =
            configure
                << configuration (coAxis
                    [axcoLabelAngle 0])

        encHighlight =
            encoding
                << position X [ pName "Age Group", pMType Ordinal,  pAxis [ axTitle "" ]]
                << position Y [ pName "Charges", pMType Quantitative, pAxis [ axTitle "Charges (US $)" , axGrid False ]
                , pScale [ scDomain (doNums [ 5000, 41000 ]) ] ]
                << color
                    [ mSelectionCondition (selectionName "mySelection")
                        [ mName "Primary Payment Type", mMType Nominal ]
                        [ mStr "black" ] ]
                << opacity
                    [ mSelectionCondition (selectionName "mySelection")
                        [ mNum 1 ]
                        [ mNum 0.1 ] ]

        sel =
            selection << select "mySelection"
                seSingle
                [ seFields ["Primary Payment Type"]
                , seNearest True
                , seBind
                    [iSelect "Primary Payment Type"
                        [inName "Select Primary Payment Type "
                        , inOptions
                            ["Blue Cross/Blue Shield"
                            , "Department of Corrections"
                            , "Federal/State/Local/VA"
                            , "Managed Care, Unspecified"
                            , "Medicaid"
                            , "Medicare"
                            , "Miscellaneous/Other"
                            , "Private Health Insurance"
                            , "Self-Pay"
                            , "Unknown"]
                        ]
                    ]
                ]

    in
    toVegaLite
        [title "4) Median Charges for Patient Discharges by Age Group"
        , width 400
        , height 325
        , data4 []
        , encHighlight []
        , line []
        , sel []
        , cfg []
        ]
```

{|visualization)}

{(insights|}

For this project, I was looking for trends or patterns in different patient groups discharged from New York hospitals in 2016. I aligned each visualisation with each research question, and I gained the following insights.

In the first visualisation, I was interested in how patients in different age groups were admitted to hospitals. I found that apart from children (age 0-17), the most common admission type was emergency. Children were mostly admitted as newborns. The admissions due to emergency got larger as age increased, with the 70 or older group having the most emergency admissions. Trauma, while accounting for a small proportion of admissions, also increased with age. Elective admission was highest in the two groups spanning age 30-69. This could be due to more comprehensive health insurance coverage for these groups. Urgent admission rose and peaked at the 30-49 age group and then dropped again.

In the second visualisation, I was looking for trends with age groups and diagnoses. Here I found that events in some diagnostic categories, such as circulatory, kidney, and nervous system, increased with age with the highest values in the 70 or older group. However, some categories peaked in younger groups, such as mental disease and alcohol/drug use, which both peaked at the 30-49 age group. Perhaps this has to do with additional stress for that age group from work, raising children and caring for parents. Also, unsurprisingly, the newborns category is highest in the 0-17 age group and the pregnancy/childbirth category is high for both 18-29 and 30-49. These categories also represent the most patient events for both of those age groups, i.e. patients between 18-49 are most likely to be in a New York hospital in 2016 for pregnancy/childbirth. The respiratory system category is high in childhood, but then it drops in early adulthood followed by a steady increase with age.

For the third visualisation, I wanted to understand the length of stay for different age groups. The largest proportion of patients in each age group is two days, except for the 70 or older group which is three days. With the older groups, there is a wider spread on the histograms showing that more patients in those groups are staying longer. There is a sharp drop after three days for the 0-17 group, but for the older groups, there is a steady decrease in length of stay after 3 days. Given that the most common reason for the 0-17 group to be admitted is being born, if there are no complications, it seems logical that repetitive processes are followed and they would consistently leave after a standard timeframe. Also, with the older groups, there is more variation in diagnoses having large numbers of patients (as shown in the second visualisation), and there are more admissions as emergencies (as shown in the first), which could explain the longer stays.

For the last visualisation, I was interested in how charges and payment type varied for different age groups. There was a consistent upward trend in median charges from the younger to the older groups, except for the 70 or older group which was lower than the 50-69 group for some payment types. It would be interesting to know why charges using private healthcare, federal/state/local/VA, managed care and miscellaneous/other were lower for the oldest group when all other payment types were higher. It's possible this is related to the pattern seen with elective admissions, where they were highest for ages 30-69 and lower for 70 or older. Charges didn't vary too much among different payment types, except for the miscellaneous/other category which was much higher for most groups. There was also a bigger difference in charges for the two older groups.

{|insights)}

{(designJustification|}

I chose a dataset for this project that I found interesting, but it was very large with over 2.3 million rows. After some initial testing, I knew I would need to limit or aggregate the data before building the visualisations. I decided to focus on patient groups, i.e. the five age groups, which could provide a common theme across different visualisations while allowing me to compare the groups for different variables. I also liked the idea of investigating the flow of the patient, i.e. admission, diagnosis, stay and payment. Tufte (2001, pp.178) mentions that attractive graphics often tell a story about the data. With this in mind, I aggregated the data using counts of the patient discharge records and totals of the charges to produce more manageable sizes for the visualisations. Wilkinson (2010) refers to this kind of summarisation as 'statistics' in his grammar of graphics system, which can be used to transform initial data into a graphic. Below, I describe the design justification for each visualisation.

For the first visualisation showing admission type and age group, I chose a stacked bar chart with a drop-down for interaction. I used colour hue as the visual variable to represent admission types, a nominal attribute. According to Andrienko and Andrienko (2006, pp.171-172), referring to Bertin's theory of graphical representation of data, colour is associative and selective, and these types of variables work well with the nominal scale. The associative property allows the viewer to group similar marks together, such as all orange emergency admissions. This helps with seeing trends and similarities and with comparing age categories. The selective property allows the viewer to isolate marks, such as the lone red mark for newborn admissions, making comparisons within an age group easier to see. I chose to add a drop-down menu for interaction, as data in some of the categories is small, e.g., for trauma, and difficult to see when viewing the whole visualisation. This follows the design concept from Shneiderman (1996, pp.337) of 'overview first, zoom and filter, then details-on-demand.' The user can gain an overview of all data before filtering to look at a specific category. He/she can then view the details by hovering over a bar to see the count of patient events represented. I removed the x-axis title, as I felt it wasn't needed. I also configured the axis labels to read horizontally following Tufte's (2001, pp.183) advice that text should be presented from left to right. Finally, I added some spacing between the bars to improve readability.

For the second visualisation, I looked at how major diagnostic category varied with age group. I chose a scatterplot with circles to plot this data. I double-encoded the age group with location and colour saturation. Munzner (2014, pp.97) mentions that although double-encoding uses up more visual variables, the attributes that are double-encoded will be easily perceived. I felt this was important given the high number of markings shown on this visualisation. Andrienko and Andrienko (2006, pp.171-177), also list location and colour saturation both as ordered visual variables, so they are appropriate for encoding the ordinal age group data. I chose size of the circles to encode the count of patient discharges for each diagnosis/age group combination. This allows the user to compare many quantitative values in a compact space using Munzner's (2014, pp.15-16) idea of high information density. To improve readability, I configured the axis labels to horizontal, aligned the vertical axis labels to the left and trimmed the labels. Some diagnostic categories are quite long, but the full name can be seen when using the interactive hover over any of the circles. I also added some padding under the vertical heading and removed the x-axis title. I set the size of the circles so that the smallest could still be viewed and the largest did not overlap with other data points. This could be seen to lack graphical integrity, as described by Tufte (2001, pp.53-56), because the circles representing the smaller numbers are not proportional to those representing 50,000. However, he also suggested to clearly label the graphics to reduce misperception, so I included a legend and hover interaction showing the actual values. In addition, I was using this visualisation to look at trends, and not to determine if one value was a certain percentage bigger or smaller than another value.

For the third graph, I wanted to show more details than the median or mean of the length of stay, which would have been better suited to a table. Also, as Munzner (2014, pp.7-9) points out, descriptive statistics can hide the true patterns of the data. I decided a good layout for this would be a facet of histograms, so the viewer can compare each age group in one view. In my initial visualisation, the bars were small and the shape of the distributions were difficult to see, because the length of stay data is very skewed with a long tail extending to 120 days. The majority of the patients had stays under 30 days. To improve visibility of this, I modified the data by changing anything 30 days or more to 30, which had the same effect as creating one larger bin at the end of the visualisation. This is why a small bump in the graphs appear at 30 days. I included information about this in the x-axis label for clarification, and the actual value appears when hovering over each bar. I attempted to annotate this on each graph next to the 30 day mark, however I was unable to get an annotation to appear with a faceted graph.

For the final visualisation, I chose a line graph with a drop-down menu of payment type. I again used colour hue to represent the nominal attribute payment type. This visualisation also followed Shneiderman's (1996, pp.337) 'overview first, zoom and filter, then details-on-demand' approach. For the interaction, this time I chose to change the opacity when applying the filter, so the viewer can see the selected payment type highlighted and still see the other lines for comparison. I used Tufte's (2001, pp.107-116) idea of minimising 'chartjunk' by removing grid lines, which I felt added clutter, and the x-axis title. I also presented the labels horizontally. Initially, I tried using a stacked area graph. I found this misleading, because when a cost went up or down from one category to the next, it sometimes didn't appear that way because of the pattern caused by the stacking. It took more effort on the part of the viewer to compare values between age groups for the different payment types.

{|designJustification)}

{(validation|}

The visualisations provided insight not immediately available by looking at the source data, and they allowed me to answer my research questions very well. However, there are also some limitations as described below.

The strength of the first graph is that it provides an overall view for each age group as well as the ability to filter for each admission type, which is useful for the types with smaller numbers.

The second design allowed me to explore different diagnostic categories with different age groups in detail, but also gave me an overall view of the combination of the two. I was focused on trends, and the circle sizes made this easy to spot larger and smaller circles near each other, either for the same diagnostic category or the same age group. A limitation is that it would not be appropriate for estimating if one circle is a certain percentage bigger or smaller than another circle, as the smaller sizes (0 - 50,000) are not proportional to the 50,000 circle. Another limitation is that it's difficult to tell the difference between the very small circles, i.e. anything under 1,000. For the eye diagnostic category, the circles look virtually the same, but they actually represent values between 323 and 1,084.

The third visualisation provides the patterns for length of stay for each age group up to 30 days. For 30 days and higher, the days are binned into one bucket labelled '30'. The limitation is that no detail beyond 30 days can be seen.

On the last design, the different coloured lines provide an overview and trends of charge amounts and payment types. There is some overlap of the lines, but the filtering allows me to isolate one payment type while still seeing the shape of the others.

Approaches from the literature can also be used to validate this design. Shneiderman's (1996, pp.337) 'overview first, zoom and filter, then details-on-demand' approach is apparent in the first and last graph. The encoding of visual variables is consistent with guidance from Munzner (2014, pp.97) and Andrienko and Andrienko (2006, pp.171-177). Elements of Wilkinson's (2010) grammar of graphics system, such as variables, statistics, geometry and aesthetics, can be found when considering how the data has been transformed into visual designs. Finally, some of Tufte's (2001, pp.107-116, 183) minimalist approaches are used throughout.

One limitation of this entire design is that it can only be used with static (offline) data. The data shaping to create this involved other tools, i.e. Python and Excel, and manual steps which would not be practical for displaying dynamic (online) data.

If I were to do this exercise again, I would try linking the data used in all four visualisations so that the user could filter or highlight areas on one and see the selection applied on the others. This may help explain some of the insights in more detail, such as why elective admissions were higher in the 30-49 and 50-69 age groups or if longer stays are related to emergency admission and variation in diagnoses in older age groups. This kind of design may not be feasible with this large dataset and this toolset, as more detail would be needed to link all four visualisations together. Atom was unable to process around 100,000 records when adding the data to a .md file, but perhaps another method like loading data from a file could be used if needed. If feasible, I would also try building a zoom interactive option with the faceted histograms to allow viewing the details of the small and larger lengths of stay.

{|validation)}

{(references|}

**Andrienko, N., Andrienko, G.** (2006) *Exploratory Analysis of Spatial and Temporal Data*. Berlin: Springer.

**Munzner, T., Maguire, E.** (2014) *Visualization Analysis and Design*. Boca Raton, Florida: CRC Press.

**Shneiderman, B.** (1996) The eyes have it: A task by data type taxonomy for information visualization, *Proceedings of the IEEE Symposium on Visual Languages*, Boulder, Colorado, doi: 10.1109/VL.1996.545307.

**Tufte, E.** (2001) *The Visual Display of Quantitative Information*, Cheshire, Connecticut: Graphics Press.

**Wilkinson, L.** (2010) 'The grammar of graphics', *Wiley interdisciplinary reviews: Computational Statistics*, 2010 (2), pp.673–677. doi: 10.1002/wics.118

{|references)}
