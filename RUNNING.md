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
