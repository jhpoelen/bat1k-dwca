# Bat1K DwC-A

This repository contains a prototype of Bat1K's Index of Available Genomes for Living Bat Species expressed as a [Darwin Core](#1) Archive. This prototype enable the registration of Bat1K metadata with biodiversity data infrastructures such as Global Biodiversity Information Facility (GBIF) as to increase the visibility and resuse of the data.  

This was a data experiment was the outcome of a presentation given by Jorrit Poelen on 27 September 2024 at the Bat1K Symposium of the North American Society for Bat Research (NASBR) in Guadalajara, Mexico. See also https://github.com/jhpoelen/bat1k-talk-2024-10-27 and https://jhpoelen.nl/bat1k-talk-2024-10-27 .

## Example Usage

Now that Bat1K data is packaged in a DwC-A, we can use existing tools like [Preston](https://github.com/bio-guoda/preston) to track and process the bat1k index.

In other words, the Bat1K index has now increase their ability to be reused through a data standard (DwC) that is widely used in the Biodiversity Informatics and Natural History Collections communities.

### List first three records

```bash
preston track https://github.com/jhpoelen/bat1k-dwca/archive/main.zip\
 | preston dwc-stream\
 | head -n3\
 | jq .
```

### generate Citation

```bash
preston track https://github.com/jhpoelen/bat1k-dwca/archive/main.zip\
 | preston cite
```

### Print first 10 lines of Myotis lucifugus sequence

```bash
preston track https://github.com/jhpoelen/bat1k-dwca/archive/main.zip\
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


