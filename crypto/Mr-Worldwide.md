### [Mr-Worldwide](https://play.picoctf.org/practice/challenge/40)

This challenge presents us with a text file with the following content.

    picoCTF{(35.028309, 135.753082)(46.469391, 30.740883)(39.758949, -84.191605)(41.015137, 28.979530)(24.466667, 54.366669)(3.140853, 101.693207)_(9.005401, 38.763611)(-3.989038, -79.203560)(52.377956, 4.897070)(41.085651, -73.858467)(57.790001, -152.407227)(31.205753, 29.924526)}

The numbers look like coordinates. If we use [google maps](maps.google.com), we can find their locations.

|Coordinates           |Location                  |
|----------------------|--------------------------|
|35.028309, 135.753082 |**K**yoto                 |
|46.469391, 30.740883  |**O**desa                 |
|39.758949, -84.191605 |**D**ayton                |
|41.015137, 28.979530  |**I**stanbul              |
|24.466667, 54.366669  |**A**bu Dhabi             |
|3.140853, 101.693207  |**K**uala Lumpur          |
|9.005401, 38.763611   |**A**ddis Ababa           |
|-3.989038, -79.203560 |**L**oja                  |
|52.377956, 4.897070   |**A**msterdam             |
|41.085651, -73.858467 |**S**leepy Hollow         |
|57.790001, -152.407227|**K**odiak                |
|31.205753, 29.924526  |**A**lexandria Governorate|

From this table, we can make out the flag using the first letters of the locations.

Flag: `picoCTF{kodiak_alaska}`
