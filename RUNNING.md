#Installation
Installing `norma` from Jenkins:
```
wget https://jenkins.ch.cam.ac.uk/view/AMI2/job/norma/org.xml-cml$norma/lastSuccessfulBuild/artifact/org.xml-cml/norma/0.1-SNAPSHOT/norma-0.1-SNAPSHOT.deb
sudo dpkg -i norma-0.1-SNAPSHOT.deb
# the password is password
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
