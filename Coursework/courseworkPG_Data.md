---
id: litvis

narrative-schemas:
  - ../narrative-schemas/courseworkPG.yml

elm:
  dependencies:
    gicentre/elm-vegalite: latest
    gicentre/tidy: latest
---

@import "../css/datavis.less"

## Data Source
My data source for this project is the Hospital Inpatient Discharge data for the state of New York in 2016. These are de-identified records of patients discharged from New York hospitals in 2016. The full file and supporting documentation can be found here: https://health.data.ny.gov/Health/Hospital-Inpatient-Discharges-SPARCS-De-Identified/tsg2-5hds

## Data Processing
The source data for this is 2.3 million rows, and it includes each patient event for patients discharged from New York hospitals. As this is too large to process in Atom, I aggregated the data first in Python, exported it to .csv and copied it below. I've shown the first five rows of each table here. In my coursework submission files, I've also included an .html version of the Jupyter Notebook where I cleaned and agregated the data in Python.

```elm {l=hidden}
import Tidy exposing (..)
import VegaLite exposing (..)
```
## Data Samples
Below is a sample of the table used for Admission Type and Age Group.
```elm {m}
displayAgeAdmTable : List String
displayAgeAdmTable =
    tableSummary 5 ageAdmTable
```
Below is a sample of the table used for Major Diagnostic Category and Age Group.
```elm {m}
displayAgeDiagTable : List String
displayAgeDiagTable =
    tableSummary 5 ageDiagTable
```
Below is a sample of the table used for Length of Stay and Age Group.
```elm {m}
displayAgeLOSTable : List String
displayAgeLOSTable =
    tableSummary 5 ageLOSTable
```
Below is a sample of the table used for Charges, Payment Type and Age Group.
```elm {m}
displayAgeCostTable : List String
displayAgeCostTable =
    tableSummary 5 ageCostTable
```

```elm {l=hidden}
ageAdmTable : Table
ageAdmTable =
    """ageGroup,admissionType,count
0 to 17,Elective,17603
0 to 17,Emergency,94130
0 to 17,Newborn,224580
0 to 17,Not Available,31
0 to 17,Trauma,497
0 to 17,Urgent,10794
18 to 29,Elective,64812
18 to 29,Emergency,138933
18 to 29,Newborn,46
18 to 29,Not Available,80
18 to 29,Trauma,1152
18 to 29,Urgent,38622
30 to 49,Elective,124780
30 to 49,Emergency,277865
30 to 49,Newborn,79
30 to 49,Not Available,106
30 to 49,Trauma,1493
30 to 49,Urgent,50608
50 to 69,Elective,145326
50 to 69,Emergency,467717
50 to 69,Newborn,45
50 to 69,Not Available,111
50 to 69,Trauma,1741
50 to 69,Urgent,39590
70 or Older,Elective,88726
70 or Older,Emergency,520316
70 or Older,Newborn,29
70 or Older,Not Available,107
70 or Older,Trauma,1746
70 or Older,Urgent,31764

"""
    |> fromCSV
```

```elm {l=hidden}
ageDiagTable : Table
ageDiagTable =
    """ageGroup,MDC,count
0 to 17,Alcohol/Drug Use and Alcohol/Drug Induced Organic Mental Disorders,214
0 to 17,Burns,559
0 to 17,"Blood, Blood Forming Organs and Immunological Disorders",4356
0 to 17,Circulatory System,2970
0 to 17,Digestive System,14094
0 to 17,Eye,573
0 to 17,Female Reproductive System,575
0 to 17,Hepatobiliary System and Pancreas,1105
0 to 17,Kidney and Urinary Tract,3686
0 to 17,Male Reproductive System,387
0 to 17,Musculoskeletal System and Conn Tissue,6694
0 to 17,Nervous System,12298
0 to 17,Respiratory System,27928
0 to 17,"Skin, Subcutaneous Tissue and Breast",4594
0 to 17,"Ear, Nose, Mouth, Throat and Craniofacial Diseases and Disorders",7814
0 to 17,"Endocrine, Nutritional and Metabolic Diseases and Disorders",5195
0 to 17,Human Immunodeficiency Virus Infections,13
0 to 17,"Infectious and Parasitic Diseases, Systemic or Unspecified Sites",5907
0 to 17,"Lymphatic, Hematopoietic, Other Malignancies, Chemotherapy and Radiotherapy",1984
0 to 17,Mental Diseases and Disorders,10682
0 to 17,Multiple Significant Trauma,264
0 to 17,Newborns and Other Neonates with Conditions Originating in the Perinatal Period,229534
0 to 17,"Poisonings, Toxic Effects, Other Injuries and Other Complications of Treatment",2265
0 to 17,Pre-MDC or Ungroupable,8
0 to 17,"Pregnancy, Childbirth and the Puerperium",2495
0 to 17,"Rehabilitation, Aftercare, Other Factors Influencing Health Status and Other Health Service Contacts",1441
18 to 29,Alcohol/Drug Use and Alcohol/Drug Induced Organic Mental Disorders,13790
18 to 29,Burns,253
18 to 29,"Blood, Blood Forming Organs and Immunological Disorders",5198
18 to 29,Circulatory System,4199
18 to 29,Digestive System,13147
18 to 29,Eye,323
18 to 29,Female Reproductive System,1837
18 to 29,Hepatobiliary System and Pancreas,4821
18 to 29,Kidney and Urinary Tract,4154
18 to 29,Male Reproductive System,342
18 to 29,Musculoskeletal System and Conn Tissue,6950
18 to 29,Nervous System,8933
18 to 29,Respiratory System,6269
18 to 29,"Skin, Subcutaneous Tissue and Breast",4719
18 to 29,"Ear, Nose, Mouth, Throat and Craniofacial Diseases and Disorders",2977
18 to 29,"Endocrine, Nutritional and Metabolic Diseases and Disorders",7381
18 to 29,Human Immunodeficiency Virus Infections,465
18 to 29,"Infectious and Parasitic Diseases, Systemic or Unspecified Sites",5731
18 to 29,"Lymphatic, Hematopoietic, Other Malignancies, Chemotherapy and Radiotherapy",1475
18 to 29,Mental Diseases and Disorders,29736
18 to 29,Multiple Significant Trauma,893
18 to 29,"Poisonings, Toxic Effects, Other Injuries and Other Complications of Treatment",3706
18 to 29,Pre-MDC or Ungroupable,4
18 to 29,"Pregnancy, Childbirth and the Puerperium",114970
18 to 29,"Rehabilitation, Aftercare, Other Factors Influencing Health Status and Other Health Service Contacts",1372
30 to 49,Alcohol/Drug Use and Alcohol/Drug Induced Organic Mental Disorders,34660
30 to 49,Burns,430
30 to 49,"Blood, Blood Forming Organs and Immunological Disorders",7201
30 to 49,Circulatory System,27498
30 to 49,Digestive System,34930
30 to 49,Eye,734
30 to 49,Female Reproductive System,11888
30 to 49,Hepatobiliary System and Pancreas,16276
30 to 49,Kidney and Urinary Tract,13192
30 to 49,Male Reproductive System,813
30 to 49,Musculoskeletal System and Conn Tissue,24852
30 to 49,Nervous System,22386
30 to 49,Respiratory System,18419
30 to 49,"Skin, Subcutaneous Tissue and Breast",13295
30 to 49,"Ear, Nose, Mouth, Throat and Craniofacial Diseases and Disorders",4618
30 to 49,"Endocrine, Nutritional and Metabolic Diseases and Disorders",19454
30 to 49,Human Immunodeficiency Virus Infections,2396
30 to 49,"Infectious and Parasitic Diseases, Systemic or Unspecified Sites",16303
30 to 49,"Lymphatic, Hematopoietic, Other Malignancies, Chemotherapy and Radiotherapy",3164
30 to 49,Mental Diseases and Disorders,38421
30 to 49,Multiple Significant Trauma,931
30 to 49,"Poisonings, Toxic Effects, Other Injuries and Other Complications of Treatment",6573
30 to 49,Pre-MDC or Ungroupable,2
30 to 49,"Pregnancy, Childbirth and the Puerperium",132667
30 to 49,"Rehabilitation, Aftercare, Other Factors Influencing Health Status and Other Health Service Contacts",3828
50 to 69,Alcohol/Drug Use and Alcohol/Drug Induced Organic Mental Disorders,26073
50 to 69,Burns,449
50 to 69,"Blood, Blood Forming Organs and Immunological Disorders",7680
50 to 69,Circulatory System,113717
50 to 69,Digestive System,67601
50 to 69,Eye,1084
50 to 69,Female Reproductive System,8084
50 to 69,Hepatobiliary System and Pancreas,27138
50 to 69,Kidney and Urinary Tract,33284
50 to 69,Male Reproductive System,5443
50 to 69,Musculoskeletal System and Conn Tissue,87878
50 to 69,Nervous System,50649
50 to 69,Respiratory System,64717
50 to 69,"Skin, Subcutaneous Tissue and Breast",21343
50 to 69,"Ear, Nose, Mouth, Throat and Craniofacial Diseases and Disorders",8407
50 to 69,"Endocrine, Nutritional and Metabolic Diseases and Disorders",24200
50 to 69,Human Immunodeficiency Virus Infections,3280
50 to 69,"Infectious and Parasitic Diseases, Systemic or Unspecified Sites",44019
50 to 69,"Lymphatic, Hematopoietic, Other Malignancies, Chemotherapy and Radiotherapy",9258
50 to 69,Mental Diseases and Disorders,28411
50 to 69,Multiple Significant Trauma,1095
50 to 69,"Poisonings, Toxic Effects, Other Injuries and Other Complications of Treatment",9411
50 to 69,"Pregnancy, Childbirth and the Puerperium",78
50 to 69,"Rehabilitation, Aftercare, Other Factors Influencing Health Status and Other Health Service Contacts",11231
70 or Older,Alcohol/Drug Use and Alcohol/Drug Induced Organic Mental Disorders,1282
70 or Older,Burns,201
70 or Older,"Blood, Blood Forming Organs and Immunological Disorders",8714
70 or Older,Circulatory System,144772
70 or Older,Digestive System,66147
70 or Older,Eye,881
70 or Older,Female Reproductive System,2899
70 or Older,Hepatobiliary System and Pancreas,15453
70 or Older,Kidney and Urinary Tract,48414
70 or Older,Male Reproductive System,3451
70 or Older,Musculoskeletal System and Conn Tissue,78135
70 or Older,Nervous System,56940
70 or Older,Respiratory System,76600
70 or Older,"Skin, Subcutaneous Tissue and Breast",16576
70 or Older,"Ear, Nose, Mouth, Throat and Craniofacial Diseases and Disorders",7453
70 or Older,"Endocrine, Nutritional and Metabolic Diseases and Disorders",19624
70 or Older,Human Immunodeficiency Virus Infections,282
70 or Older,"Infectious and Parasitic Diseases, Systemic or Unspecified Sites",62160
70 or Older,"Lymphatic, Hematopoietic, Other Malignancies, Chemotherapy and Radiotherapy",5967
70 or Older,Mental Diseases and Disorders,7800
70 or Older,Multiple Significant Trauma,1379
70 or Older,"Poisonings, Toxic Effects, Other Injuries and Other Complications of Treatment",4721
70 or Older,"Rehabilitation, Aftercare, Other Factors Influencing Health Status and Other Health Service Contacts",12837
"""
        |> fromCSV
```

```elm {l=hidden}
ageLOSTable : Table
ageLOSTable =
    """ageGroup,LOS,count
0 to 17,1,53061
0 to 17,2,151456
0 to 17,3,72250
0 to 17,4,22790
0 to 17,5,9406
0 to 17,6,6285
0 to 17,7,5417
0 to 17,8,3541
0 to 17,9,2700
0 to 17,10,2281
0 to 17,11,1797
0 to 17,12,1486
0 to 17,13,1335
0 to 17,14,1221
0 to 17,15,923
0 to 17,16,868
0 to 17,17,694
0 to 17,18,580
0 to 17,19,521
0 to 17,20,536
0 to 17,21,529
0 to 17,22,464
0 to 17,23,374
0 to 17,24,326
0 to 17,25,300
0 to 17,26,264
0 to 17,27,284
0 to 17,28,235
0 to 17,29,247
0 to 17,30,5464
18 to 29,1,36997
18 to 29,2,71966
18 to 29,3,55523
18 to 29,4,24795
18 to 29,5,12623
18 to 29,6,7569
18 to 29,7,5725
18 to 29,8,4181
18 to 29,9,3079
18 to 29,10,2363
18 to 29,11,2035
18 to 29,12,1700
18 to 29,13,1650
18 to 29,14,1779
18 to 29,15,1176
18 to 29,16,879
18 to 29,17,745
18 to 29,18,652
18 to 29,19,636
18 to 29,20,587
18 to 29,21,707
18 to 29,22,470
18 to 29,23,420
18 to 29,24,364
18 to 29,25,311
18 to 29,26,294
18 to 29,27,345
18 to 29,28,774
18 to 29,29,237
18 to 29,30,3063
30 to 49,1,76052
30 to 49,2,115571
30 to 49,3,93508
30 to 49,4,49383
30 to 49,5,27670
30 to 49,6,17180
30 to 49,7,12907
30 to 49,8,9327
30 to 49,9,6892
30 to 49,10,5418
30 to 49,11,4524
30 to 49,12,3729
30 to 49,13,3495
30 to 49,14,3894
30 to 49,15,2581
30 to 49,16,2091
30 to 49,17,1724
30 to 49,18,1438
30 to 49,19,1394
30 to 49,20,1377
30 to 49,21,1599
30 to 49,22,1017
30 to 49,23,838
30 to 49,24,760
30 to 49,25,731
30 to 49,26,613
30 to 49,27,712
30 to 49,28,1554
30 to 49,29,510
30 to 49,30,6442
50 to 69,1,109091
50 to 69,2,118098
50 to 69,3,101294
50 to 69,4,70974
50 to 69,5,50882
50 to 69,6,36641
50 to 69,7,29724
50 to 69,8,22030
50 to 69,9,16370
50 to 69,10,12758
50 to 69,11,10343
50 to 69,12,8560
50 to 69,13,7758
50 to 69,14,7637
50 to 69,15,5757
50 to 69,16,4562
50 to 69,17,3911
50 to 69,18,3387
50 to 69,19,3051
50 to 69,20,2924
50 to 69,21,3082
50 to 69,22,2247
50 to 69,23,1844
50 to 69,24,1588
50 to 69,25,1476
50 to 69,26,1321
50 to 69,27,1297
50 to 69,28,1669
50 to 69,29,1015
50 to 69,30,13239
70 or Older,1,72659
70 or Older,2,90046
70 or Older,3,101550
70 or Older,4,77309
70 or Older,5,59455
70 or Older,6,47174
70 or Older,7,38418
70 or Older,8,28287
70 or Older,9,20827
70 or Older,10,16471
70 or Older,11,13257
70 or Older,12,10689
70 or Older,13,9217
70 or Older,14,8348
70 or Older,15,6650
70 or Older,16,5279
70 or Older,17,4387
70 or Older,18,3557
70 or Older,19,3112
70 or Older,20,2811
70 or Older,21,2770
70 or Older,22,2149
70 or Older,23,1771
70 or Older,24,1501
70 or Older,25,1325
70 or Older,26,1081
70 or Older,27,1130
70 or Older,28,1125
70 or Older,29,814
70 or Older,30,9519

"""
    |> fromCSV
```

```elm {l=hidden}
ageCostTable : Table
ageCostTable =
    """ageGroup,payment1,charges
0 to 17,Blue Cross/Blue Shield,9394
0 to 17,Department of Corrections,14829
0 to 17,Federal/State/Local/VA,6935
0 to 17,"Managed Care, Unspecified",7151
0 to 17,Medicaid,9850
0 to 17,Medicare,11751
0 to 17,Miscellaneous/Other,25847
0 to 17,Private Health Insurance,10399
0 to 17,Self-Pay,6044
0 to 17,Unknown,10391
18 to 29,Blue Cross/Blue Shield,17523
18 to 29,Department of Corrections,20784
18 to 29,Federal/State/Local/VA,13073
18 to 29,"Managed Care, Unspecified",17417
18 to 29,Medicaid,17705
18 to 29,Medicare,22691
18 to 29,Miscellaneous/Other,31575
18 to 29,Private Health Insurance,20599
18 to 29,Self-Pay,15266
18 to 29,Unknown,15060
30 to 49,Blue Cross/Blue Shield,22344
30 to 49,Department of Corrections,22977
30 to 49,Federal/State/Local/VA,19387
30 to 49,"Managed Care, Unspecified",22560
30 to 49,Medicaid,20278
30 to 49,Medicare,25864
30 to 49,Miscellaneous/Other,37086
30 to 49,Private Health Insurance,24608
30 to 49,Self-Pay,17912
30 to 49,Unknown,21729
50 to 69,Blue Cross/Blue Shield,34410
50 to 69,Department of Corrections,25165
50 to 69,Federal/State/Local/VA,28755
50 to 69,"Managed Care, Unspecified",33126
50 to 69,Medicaid,28371
50 to 69,Medicare,34076
50 to 69,Miscellaneous/Other,40341
50 to 69,Private Health Insurance,37613
50 to 69,Self-Pay,23942
50 to 69,Unknown,27173
70 or Older,Blue Cross/Blue Shield,37631
70 or Older,Department of Corrections,30117
70 or Older,Federal/State/Local/VA,24040
70 or Older,"Managed Care, Unspecified",22594
70 or Older,Medicaid,37178
70 or Older,Medicare,35321
70 or Older,Miscellaneous/Other,25583
70 or Older,Private Health Insurance,32586
70 or Older,Self-Pay,25043
70 or Older,Unknown,30590

"""
    |> fromCSV
```
{(questions|}
Please see main litvis file - added this to prevent Linter errors from appearing in Atom
{|questions)}

{(visualization|}
Please see main litvis file - added this to prevent Linter errors from appearing in Atom
{|visualization)}

{(insights|}
Please see main litvis file - added this to prevent Linter errors from appearing in Atom
{|insights)}

{(designJustification|}
Please see main litvis file - added this to prevent Linter errors from appearing in Atom
{|designJustification)}

{(validation|}
Please see main litvis file - added this to prevent Linter errors from appearing in Atom
{|validation)}

{(references|}
Please see main litvis file - added this to prevent Linter errors from appearing in Atom
{|references)}
