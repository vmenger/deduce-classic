# Deduce: de-identification method for Dutch medical text

> Hello! This project contains the *classic* version of DEDUCE: the version as released with [Menger et al (2017)](http://www.sciencedirect.com/science/article/pii/S0736585316307365). It is an archived project, to ensure the specific validated version remains available for those who need it. The maintained and updated version is available at [vmenger/deduce](https://github.com/vmenger/deduce). Note that this classic version is not pip installable, but only by cloning and installing locally (see installation guidelines). Running `pip install deduce` on the other hand will install the latest version. 
> 
> The now following readme is a copy of the original. 

This project contains the code for DEDUCE: de-identification method for Dutch medical text as described in [Menger et al (2017)](http://www.sciencedirect.com/science/article/pii/S0736585316307365). De-identification of medical text is needed for using text data for analysis, to comply with legal requirements and to protect the privacy of patients. Our pattern matching based method removes Protected Health Information (PHI) in the following categories:

1. Person names, including initials
2. Geographical locations smaller than a country
3. Names of institutions that are related to patient treatment
4. Dates
5. Ages
6. Patient numbers
7. Telephone numbers
8. E-mail addresses and URLs

The details of the development and workings of the method, and its validation can be found in: 

[Menger, V.J., Scheepers, F., van Wijk, L.M., Spruit, M. (2017). DEDUCE: A pattern matching method for automatic de-identification of Dutch medical text, Telematics and Informatics, 2017, ISSN 0736-5853](http://www.sciencedirect.com/science/article/pii/S0736585316307365)

### Prerequisites

* `nltk`

### Installing

Only from source, simply download and use python to install. Note that running `pip install deduce` will not install this classic version, but the up to date and maintained version.

``` python
>>> python setup.py install
```

## Getting started

The package has a method for annotating (`annotate_text`) and for removing the annotations (`deidentify_annotations`).

``` python

import deduce 

deduce.annotate_text(
        text,                       # The text to be annotated
        patient_first_names="",     # First names (separated by whitespace)
        patient_initials="",        # Initial
        patient_surname="",         # Surname(s)
        patient_given_name="",      # Given name
        names=True,                 # Person names, including initials
        locations=True,             # Geographical locations
        institutions=True,          # Institutions
        dates=True,                 # Dates
        ages=True,                  # Ages
        patient_numbers=True,       # Patient numbers
        phone_numbers=True,         # Phone numbers
        urls=True,                  # Urls and e-mail addresses
        flatten=True                # Debug option
    )    
    
deduce.deidentify_annotations(
        text                        # The annotated text that should be de-identified
    )
    
```

## Examples
``` python
>>> import deduce

>>> text = u"Dit is stukje tekst met daarin de naam Jan Jansen. De patient J. Jansen (e: j.jnsen@email.com, t: 06-12345678) is 64 jaar oud 
    en woonachtig in Utrecht. Hij werd op 10 oktober door arts Peter de Visser ontslagen van de kliniek van het UMCU."
>>> annotated = deduce.annotate_text(text, patient_first_names="Jan", patient_surname="Jansen")
>>> deidentified = deduce.deidentify_annotations(annotated)

>>> print (annotated)
"Dit is stukje tekst met daarin de naam <PATIENT Jan Jansen>. De <PATIENT patient J. Jansen> (e: <URL j.jnsen@email.com>, t: <TELEFOONNUMMER 06-12345678>) 
is <LEEFTIJD 64> jaar oud en woonachtig in <LOCATIE Utrecht>. Hij werd op <DATUM 10 oktober> door arts <PERSOON Peter de Visser> ontslagen van de kliniek van het <INSTELLING umcu>."
>>> print (deidentified)
"Dit is stukje tekst met daarin de naam <PATIENT>. De <PATIENT> (e: <URL-1>, t: <TELEFOONNUMMER-1>) is <LEEFTIJD-1> jaar oud en woonachtig in <LOCATIE-1>.
Hij werd op <DATUM-1> door arts <PERSOON-1> ontslagen van de kliniek van het <INSTELLING-1>."
```

### Configuring

The lookup lists in the `data/` folder can be tailored to the users specific needs. This is especially recommended for the list of names of institutions, since they are by default tailored to location of development and testing of the method. Regular expressions can be modified in `annotate.py`, this is for the same reason recommended for detecting patient numbers. 

## Contributing

Contributions to the deduce project are very much welcomed - feel free to get in touch with the author via issue or e-mail. 

## Versioning

1.0.2 - Release to PyPI

1.0.1 - Small bugfix for None as input

1.0.0 - Initial version

## Authors

* **Vincent Menger** - *Initial work* 
* **Jonathan de Bruin** - *Code review*

## License

This project is licensed under the GNU LGPLv3 license - see the [LICENSE.md](LICENSE.md) file for details
