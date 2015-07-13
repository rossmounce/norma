#Installation
Installing `norma` from Jenkins:
```
wget https://jenkins.ch.cam.ac.uk/view/AMI2/job/norma/org.xml-cml$norma/lastSuccessfulBuild/artifact/org.xml-cml/norma/0.1-SNAPSHOT/norma-0.1-SNAPSHOT.deb
sudo dpkg -i norma-0.1-SNAPSHOT.deb
# the password is password
```

at the time of writing I'm using norma 0.1.6 (#171 Jul 13, 2015 7:01 PM)
```
workshop@crunchbang:~$ norma --version
norma(0.1.6)
```

##Integration with other CM tools

###Integration with `getpapers`
```
#get 5 PeerJ papers as PDF and convert PDF to text with norma
getpapers --query 'JOURNAL:"PeerJ" AND FIRST_PDATE:[2015-04-01 TO 2015-04-05]' -p  --outdir peerjpdf
norma -q ./peerjpdf/ --transform pdf2txt -i fulltext.pdf -o fulltext.pdf.txt
```
in the above example, getpapers creates a project directory called peerjpdf, below which are created 5 subdirectories which are each valid ctrees. Norma then takes the project directory passed to -q , and transforms all the fulltext.pdf files within each of the ctrees into fulltext.pdf.txt (plain text)

### Integration with `quickscrape`

```
quickscrape \
  --url https://peerj.com/articles/384 \
  --scraper journal-scrapers/scrapers/peerj.json \
  --output peerj-384
  --outformat bibjson
norma -q peerj-384/https_peerj.com_articles_384/ -i fulltext.html -o tidied.html  --html jsoup
```
Tidying-up publisher supplied HTML obtained from quickscrape with jsoup.

## `norma` as an entry point to the CM toolchain, for PDF/HTML/XML
```
$ pwd
/home/workshop
$ git clone https://github.com/petermr/norma/
$ cd norma
$ pwd
/home/workshop/norma
$ norma -i src/test/resources/org/xmlcml/norma/pubstyle/ieee/ -o target/ieee/ -e html --cmdir
$ tree ieee/
ieee/
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_1196402edited_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_1196402_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_1196402site_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_1320344_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_1542681_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_4084542_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_4378148_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_4397121_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_4454831_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_4534103_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_5062278_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_5312847_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_5339169_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_5426037_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_5930031_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_6211494_html
│   └── fulltext.html
├── src_test_resources_org_xmlcml_norma_pubstyle_ieee_6533337_html
│   └── fulltext.html
└── src_test_resources_org_xmlcml_norma_pubstyle_ieee_6911089_html
    └── fulltext.html

18 directories, 18 files
```
(above) Takes a set of html files obtained from the publisher website and creates ctrees around them.

```
workshop@crunchbang:~/norma$ norma -i src/test/resources/org/xmlcml/norma/regressiondemos/singleFiles/test_xml_1471-2148-14-70.xml -o target/cmdirsxml
workshop@crunchbang:~/norma$ tree target/cmdirsxml/
target/cmdirsxml/
└── test_xml_1471-2148-14-70
    └── fulltext.xml

1 directory, 1 file
```
(above) Takes naked xml file and creates a ctree around it

```
$ norma -i random.pdf -o new-ctree
$ ls
new-ctree  peerj-384  peerjpdf  random.pdf
$ tree new-ctree/
new-ctree/
└── random
    └── fulltext.pdf

1 directory, 1 file
```
(above) Takes a random, user supplied PDF file and creates a ctree around it

## Convert eupmc XML to scholarly HTML
```
getpapers --query 'JOURNAL:"PeerJ" AND FIRST_PDATE:[2015-04-01 TO 2015-04-05]' -x  --outdir peerjxml
norma -q peerjxml/  -i fulltext.xml -o scholarly.html --transform nlm2html
```


