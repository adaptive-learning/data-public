# slepemapy.cz

The dataset contains answers from online application http://slepemapy.cz. The application allows the users to practice location of places (countries, cities etc.).

## How Was the Dataset Created?

Firstly, the raw data was obtained from:
* www.slepemapy.cz/csv/answer
* www.slepemapy.cz/csv/answer_options
* www.slepemapy.cz/csv/place

and then:

```python
import proso.geography.answers
import proso.geography.dfutil

data_all = proso.geography.answers.from_csv(
    "geography.answer.csv",
    answer_options_csv="geography.answer_options.csv")
data_all = proso.geography.answers.apply_filter(
    data_all,
    lambda x: x['number_of_options'] == len(x['options']))
data_all.drop('test_id', axis=1, inplace=True)
data_all = proso.geography.answers.anonymize(data_all)
data_train, data_test = proso.geography.answers.sample(data_all, 0.5)

data_place = proso.geography.dfutil.load_csv("./geography.place.csv").drop('name', axis=1)

data_all.to_csv("answers_all.csv", index=False)
data_test.to_csv("answers_test.csv", index=False)
data_train.to_csv("answers_train.csv", index=False)
data_place.to_csv("places.csv", index=False)
```

## Description

The dataset contains 3 files with the answers of users practicing location of countries, cities, regions etc. and another file describing the given places.

### Answers

|        name       | description                                                                                          |
|:-----------------:|------------------------------------------------------------------------------------------------------|
|         id        | identifier                                                                                           |
|        user       | user's identifier                                                                                    |
|    place_asked    | identifier of the asked place                                                                        |
|   place_answered  | identifier of the answered place, empty if the user answered "I don't know"                          |
|        type       | type of the answer: (1) find the given place on the map; (2) pick the name for the highlighted place |
|      inserted     | datetime (yyyy-mm-dd HH:mm:ss) when the answer was inserted to the system                            |
|   response_time   | how much time the answer took (measured in milliseconds)                                             |
| number_of_options | number of options (0 - no option given)                                                              |
|    place_map      | identifier of the place representing a map for which the question was asked                          |
| options           | list of identifiers of options (the asked place included)                                            |

### Places

| name | description                                              |
|:----:|----------------------------------------------------------|
|  id  | identifier of the place                                  |
| code | code of the place (ISO 3166-1 alpha-2 if it is possible) |
| type | type of the place (described below)                      |

Places have the following types:

1. country
2. city
3. world
4. continent
5. river
6. lake
7. region (cz)
8. bundesland
9. province
10. region (it)
11. region
12. autonomus community
13. mountains
14. island

and also unknown type (0).

## Terms of Use

The following terms apply for this dataset:

* You agree to use these data for academic research purposes only. You agree that these data and any analyses of these data will not be used in development or marketing of commercial products.
* You will acknowledge Adaptive Learning Group (http://www.fi.muni.cz/adaptivelearning/) in any publication resulting from this research and notify Radek Pelanek (xpelanek@fi.muni.cz) of any publications resulting from this research.
* You will not attempt to determine the identity of any individuals represented in the dataset.
* Any published results must not include the identity of any individuals, whether such identities are determined from the data itself or from some other source.

### Citation

Please cite the following paper when you use our dataset:

```
@inproceedings{papouvsek2014adaptive,
   author = {Papou{\v{s}}ek, Jan and Pel{\'a}nek, Radek and Stanislav, V{\'\i}t}
   address = {London},
   booktitle = {Proceedings of the 7th International Conference on Educational Data Mining},
   language = {eng},
   location = {London},
   isbn = {978-0-9839525-4-1},
   pages = {6-13},
   publisher = {International Conference on Educational Data Mining 2014},
   title = {Adaptive Practice of Facts in Domains with Varied Prior Knowledge},
   year = {2014}
}
```
