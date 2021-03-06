# Open Source Voting Results Reporter (ORR)

## Election results report generator (HTML/PDF/XLS)

See [`doc/orr.md`](doc/orr.md) for a description of the orr application
and how it can be used to create collections of html/pdf/xml/xls
files from election data.

Following an election orr can be used to create results summary
pages on election night, and detailed Statement of Vote reports
with precinct detail on each contest.

This is an official project of San Francisco's [Open Source Voting System
Technical Advisory Committee][osvtac] (OSVTAC).


### To install

* Requires python version 3.6

```
$ pip install -e ./src
```


### To run

For command-line help:

```
$ orr -h
```

For an HTML example:

```
$ orr -v --input sampledata/sv-testing/election.json \
      --template templates/sv-testing --output-dir html
```

Then open `_build/html/index.html` in a browser.

Or for an example that creates TSV, XLSX, and PDF files:

```
$ orr -v --input sampledata/contest-totals.json \
      --template templates/grid-testing --output-dir grid
```

Then open `_build/grid/index.html`.

Both of the above write the output files to a subdirectory of the
directory `_build`.  The `templates` directory contains more sample
Jinja2 templates.

See the `doc` directory for more info.

#### Using data model (experimental / in progress)

This is still being worked on:

```
$ orr --debug --use-data-model --input sampledata/test-minimal \
      --extra templates/test-minimal/extra \
      --template templates/test-minimal --output-dir minimal
```


### To run tests

```
$ python -m unittest discover orr
```

To regenerate the end-to-end test expectation:

```
$ orr --debug --input sampledata/test-minimal --use-data-model \
      --build-time "2018-06-01 20:48:12" --template templates/test-minimal \
      --extra templates/test-minimal/extra \
      --output-parent src/orr/tests/end2end --output-dir expected_minimal
```


## Docker (experimental)

```
$ docker build -t orr .
$ docker run orr -h
```

To render a template directory to the build directory using Docker:

```
$ docker build -t orr . \
    && docker rm orr_builder; echo "removed container: orr_builder" \
    && docker run --name orr_builder orr --input sampledata/results-sv.json \
    && docker cp orr_builder:/app/_build/. _build
```

To run tests:

```
$ docker build -t orr . \
    && docker run --entrypoint python orr -m unittest discover orr
```


TODO: change the above to a bash script?

## History

Work on this project started informally around the beginning of May 2018.
The [Open Source Voting System Technical Advisory Committee][osvtac]
(OSVTAC) adopted it as an official project by unanimous vote at its
June 14, 2018 meeting.  The source code was first made public on June 18,
2018 under OSVTAC's GitHub account.

Note that because OSVTAC is an official meeting body of the City and
County of San Francisco, members must follow San Francisco's [Sunshine
Ordinance][sunshine-ordinance] and California's [Brown Act][brown-act].
Thus, members are limited in the extent to which they can collaborate
with one another outside of meetings.


## Copyright

Copyright (C) 2018  Carl Hage

Copyright (C) 2018  Chris Jerdonek


## License

This file is part of Open Source Voting Results Reporter (ORR).

ORR is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.


## Contact

The authors can be reached at--

* Carl Hage <ch@carlhage.com>
* Chris Jerdonek <chris.jerdonek@gmail.com>


[brown-act]: https://en.wikipedia.org/wiki/Brown_Act
[osvtac]: https://osvtac.github.io/
[sunshine-ordinance]: https://www.sfcityattorney.org/good-government/sunshine/sunshine-ordinance/
