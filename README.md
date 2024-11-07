# Bat1K DwC-A

This repository contains a prototype of Bat1K's Index of Available Genomes for Living Bat Species expressed as a [Darwin Core](#1) archive [```[1]```](#1). This prototype enable the registration of Bat1K metadata with biodiversity data infrastructures such as Global Biodiversity Information Facility (GBIF) as to increase the visibility and resuse of the data.  

This was a data experiment was the outcome of a presentation given by Jorrit Poelen on 27 September 2024 at the Bat1K Symposium of the North American Society for Bat Research (NASBR) in Guadalajara, Mexico. See also https://github.com/jhpoelen/bat1k-talk-2024-10-27 and https://jhpoelen.nl/bat1k-talk-2024-10-27 .

## Example Usage

Now that Bat1K data is packaged in a DwC-A, we can use existing tools like [Preston](https://github.com/bio-guoda/preston) to track and process the bat1k index.

In other words, the Bat1K index has now increase their ability to be reused through a data standard (DwC) that is widely used in the Biodiversity Informatics and Natural History Collections communities.

### Track DwC-A

First, track the dwc-a to get a local versioned copy using

```
preston track https://github.com/jhpoelen/bat1k-dwca/archive/main.zip
```

Now, the most description of the recent version of the tracked archive can be accessed using:

```
preston head\
 | preston cat
```

We'll use this description in the examples below to avoid re-tracking (and downloading) the dwc-a from their location on github. 

### List first three records

The script below takes the lastest version and converts (or streams) them into darwin core record in [json lines](https://jsonlines.org/) format. Line json is JavaScript Object Notation with objects separated by newlines. For more information, see https://jsonlines.org/. Using this, the first three records are selected using ```head -n3```, which are then "pretty" printed by [jq](https://jqlang.github.io/jq/) via ```jq .```.

```bash
preston head\
 | preston cat\
 | preston dwc-stream\
 | head -n3\
 | jq .
```

```json
{
  "http://www.w3.org/ns/prov#wasDerivedFrom": "line:zip:hash://sha256/9558f7ba2dd45ff3574ea578337f9bb6a7c5db768d2dce757be9990e277b78d8!/bat1k-dwca-main/seq.tsv!/L2",
  "http://www.w3.org/1999/02/22-rdf-syntax-ns#type": "http://rs.tdwg.org/dwc/terms/Occurrence",
  "http://rs.tdwg.org/dwc/text/id": "hash://md5/d9ec2eec04ac1d0b65b9764a935889a4",
  "http://rs.tdwg.org/dwc/terms/scientificName": "Myzopoda aurita",
  "http://rs.tdwg.org/dwc/terms/occurrenceRemarks": null,
  "http://rs.tdwg.org/dwc/terms/occurrenceID": "hash://md5/d9ec2eec04ac1d0b65b9764a935889a4",
  "http://rs.tdwg.org/dwc/terms/sex": "female ",
  "http://rs.tdwg.org/dwc/terms/country": "Madagascar",
  "http://rs.tdwg.org/dwc/terms/family": "Myzopodidae",
  "http://rs.tdwg.org/dwc/terms/associatedSequences": "mMyzAur1.1.pri",
  "http://purl.org/dc/terms/basisOfRecord": "MaterialSample"
}
{
  "http://www.w3.org/ns/prov#wasDerivedFrom": "line:zip:hash://sha256/9558f7ba2dd45ff3574ea578337f9bb6a7c5db768d2dce757be9990e277b78d8!/bat1k-dwca-main/seq.tsv!/L3",
  "http://www.w3.org/1999/02/22-rdf-syntax-ns#type": "http://rs.tdwg.org/dwc/terms/Occurrence",
  "http://rs.tdwg.org/dwc/text/id": "hash://md5/6fb135f08ae132757a26d937c94011e1",
  "http://rs.tdwg.org/dwc/terms/scientificName": "Rhynchonycteris naso",
  "http://rs.tdwg.org/dwc/terms/occurrenceRemarks": null,
  "http://rs.tdwg.org/dwc/terms/occurrenceID": "hash://md5/6fb135f08ae132757a26d937c94011e1",
  "http://rs.tdwg.org/dwc/terms/sex": "male",
  "http://rs.tdwg.org/dwc/terms/country": "Panama",
  "http://rs.tdwg.org/dwc/terms/family": "Emballonuridae",
  "http://rs.tdwg.org/dwc/terms/associatedSequences": "mRhyNas1.2.hap1",
  "http://purl.org/dc/terms/basisOfRecord": "MaterialSample"
}
{
  "http://www.w3.org/ns/prov#wasDerivedFrom": "line:zip:hash://sha256/9558f7ba2dd45ff3574ea578337f9bb6a7c5db768d2dce757be9990e277b78d8!/bat1k-dwca-main/seq.tsv!/L4",
  "http://www.w3.org/1999/02/22-rdf-syntax-ns#type": "http://rs.tdwg.org/dwc/terms/Occurrence",
  "http://rs.tdwg.org/dwc/text/id": "hash://md5/efbdb4a8db3b3445bae4ddeccee0746b",
  "http://rs.tdwg.org/dwc/terms/scientificName": "Saccopteryx bilineata",
  "http://rs.tdwg.org/dwc/terms/occurrenceRemarks": null,
  "http://rs.tdwg.org/dwc/terms/occurrenceID": "hash://md5/efbdb4a8db3b3445bae4ddeccee0746b",
  "http://rs.tdwg.org/dwc/terms/sex": "male",
  "http://rs.tdwg.org/dwc/terms/country": "Suriname",
  "http://rs.tdwg.org/dwc/terms/family": "Emballonuridae",
  "http://rs.tdwg.org/dwc/terms/associatedSequences": "mSacBil1.1.pri",
  "http://purl.org/dc/terms/basisOfRecord": "MaterialSample"
}
```

### generate Citation

```bash
preston head\
 | preston cat\
 | preston cite
```

```
Bat1K: Best Available Genome Sequences for Living Bat (Chiroptera) Species. Accessed at <zip:hash://sha256/9558f7ba2dd45ff3574ea578337f9bb6a7c5db768d2dce757be9990e277b78d8!/bat1k-dwca-main/eml.xml> .
```

### Print first 10 lines of Myotis lucifugus sequence

In the example below, we select a Bat1K index record with scientific name _Myotis lucifugus_ (Little brown bat), select their occurrenceID, and use this occurrenceId to query a local private repository with Bat1K sequences to retrieve their associated sequence. Then, the first 10 lines are printed using ```head``` after decompressing the sequence data using ```gunzip```.

This example shows that you can use an index integrate with privately available data repositories using digital signatures [```[2]```](#2) of the associated genome data files. 


```bash
preston head\
 | preston cat\
 | preston dwc-stream\
 | jq 'select(.["http://rs.tdwg.org/dwc/terms/scientificName"] == "Myotis lucifugus")'\
 | jq --raw-output '.["http://rs.tdwg.org/dwc/terms/occurrenceID"]'\
 | xargs -L1 preston cat --no-cache --remote https://localhost:8080/\
 | gunzip\
 | head
```

```
>SUPER__1
ccctaaccctaaccctccaaccccctaaccctaaccctaccctaacccta
accctaaccctaaccctaaccctaacctaacctaaccctaaccctaaccc
cctaaccctaaccctaaccccaaccccacccctaaccctaaccctaactc
ctaaccctaaccctaaccctatccctaaccctacccctaaccctaaccct
aaccctacaccctaaccctaacctaaaccctaaccctaaccctaacccta
accctaaccctaacctaactcccctaaccctaaccctaaccctaacccta
accctaaccctaaccctaaccctaaccctaaccctaaccctaaccctaac
cctaaccctaaccctaacccctaaccctaacctctaccctaaccctaacc
caaccctaaccctaaccctaaccctaaccctaacccaacccaccctaacc
```

Note that the only the reference to the sequence is shared (i.e. ```hash://md5/14640c6ba5808d849e7a24da73893182```), the sequence itself is only available to those that are Bat1K members. In this case, a private collection of the Bat1K sequence data was indexed by Preston using:

```
preston track\
 --algo md5\
 -f <(ls -1 /var/cache/bat1k/*.fa.gz)
```

and made available using preston in server mode using:

```
preston s
```
 

## References

### 1 
Wieczorek J, Bloom D, Guralnick R, Blum S, Döring M, et al. (2012) Darwin Core: An Evolving Community-Developed Biodiversity Data Standard. PLoS ONE 7(1): e29715. https://doi.org/10.1371/journal.pone.0029715

### 2
Elliott M.J., Poelen, J.H. & Fortes, J.A.B. (2023) Signing data citations enables data verification and citation persistence. Sci Data. https://doi.org/10.1038/s41597-023-02230-y https://linker.bio/hash://sha256/f849c870565f608899f183ca261365dce9c9f1c5441b1c779e0db49df9c2a19d
