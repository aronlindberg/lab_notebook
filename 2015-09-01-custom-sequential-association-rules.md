# Custom Sequential Association Rules
With the crowd_sequencing data, essentially what we are doing is to test custom-made sequential association rules, rather than letting an engine generate a large number of rules by itself. The more manual approach we are taking has a number of benefits: it is theoretically driven, more parsimonious (since the number of rules is limited relative to computational approaches), and is practically easier to execute.

The way it is done is through cutting the dataset in multiple ways (each based on 3 cuts):

* Stopping at every single or streaks of continuous I1, I2, and I3. `(.*?)(?:I[3]-)*I3(?:-I[3])*`
* Stopping at every streak and count all the way up to the first substreak of I3s, I2s, or I1s.
* Stopping at every streak (defined as containing at least 1 I3, 1 I2, or 1 I1) `(.*?)(?:I[0-9]-)*I3(?:-I[0-9])*`

Then when we apply rules, we can see which rules are most prevalent in each mode predicting high/low innovation.

Formulating these rules is difficult. Here is a half-finished SO question:

`data <- c("A-B-C-I1-I2-D-E-F-I1-I3-D-D-D-D-I1-I1-I3-I2-I1-I1-I3-I3-I7A-B-C-I1-I2-D-E-F-I1-I3-D-D-D-D-I1-I1-I2-I1-I1-I3-I3-I7A-B-C-I1-I2-D-E-F-I1-I3-D-D-D-D-I1-I1-I2-I1-I1-A-B-C-I1-I2-D-E-F-I1-I3-D-D-D-D-I1-I1-I2-I1-I1-I3-I3-I7I3-I3-I7A-B-C-I1-I2-D-E-F-I1-I3-D-D-D-D-I1-I1-I2-I1-I1-I3-I3-I7A-B-C-I1-I2-D-E-F-I1-I3-D-D-D-D-I1-I1-I2A-B-C-I1-I2-D-E-F-I1-I3-D-D-D-D-I1-I1-I2-I1-I1-I3-I3-I7-I1-I1-I3-I3-I7)`

I have the following regexes:

Rule 1: `(.*?)(?:I[3]-)*I3(?:-I[3])*` which gets all instances of I3 or adjacent sequences of I3

Rule 2: `(.*?)(?:I[0-9]-)*I3(?:-I[0-9])*` which gets all instances where `IX` markers are adjacent to each other and each such subsequence contains an `I3`

Now, using the same form of multiple non-capturing groups I want to construct a regex that gets every sequence of `IX` markers (similar to rule 2, but which stops at the last I3 marker.

The expected output would be to match initially:

group 0: `A-B-C-I1-I2-D-E-F`, `D-D-D-D`

group 1: `I1-I3`, `I1-I1-I3`
`I1-I3, I1-I1-I3`
