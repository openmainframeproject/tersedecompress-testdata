# Test Data for TerseDecompress #

This test data is based on the [Canterbury Corpus](https://corpus.canterbury.ac.nz/) data for evaluating lossless compression methods. It contains a variety of data including data designed to be problematic for compression algorithms. It should be a good test for TerseDecompress.

Data was transferred to z/OS using FTP into FB and/or VB format. The resulting uncompressed datasets were downloaded using FTP. Output from TerseDecompress is expected to match the result of downloading the dataset using FTP.

Datasets were compressed using both PACK and SPACK formats for testing with TerseDecompress.
 
- Binary format unterse will be tested against all files (resulting in EBCDIC data output for text files).
- Text format unterse will be tested against text files which can be accurately represented by z/OS records i.e. the files contain logical record boundaries.

### Text mode decompression

Decompression in text mode will be tested against the result of transferring the file in text mode using the **SITE TRAILINGBLANKS** setting in z/OS FTP.

SITE TRAILINGBLANKS means that trailing blanks are not stripped from the records, which matches the processing of TerseDecompress. 

### Binary mode decompression

Decompression in binary mode will be tested against the result of transferring the file in binary mode using the **SITE RDW** setting in z/OS FTP.

SITE RDW means that variable length record RDWs which contain the record length information are transferred with the data. SITE RDW can be specified for fixed LRECL datasets but has no effect.

### Text Files

- Transferred to z/OS as text format which means they can be viewed on z/OS.
- Text format files without logical records i.e. files transferred with FB LRECL=1 are not correctly untersed in text mode because record separators will be inserted where they did not exist. Those files will be only be tested in binary mode.
- Artificial/a.txt is an exception to the LRECL=1 rule because it is a single character and a single z/OS record is an accurate representation.

### Binary Files

- Binary files or text files with no record boundaries can be treated as FB with LRECL of 1 byte but do not make sense as variable length records.
- Text/Binary format files are text but some non-text characters were found so
translation etc will be unreliable. They will only be tested as binary files, but they do have logical records so can also be tested as VB binary format.

## Files

The following data from the Canterbury Corpus are in the [CanterburyCorpus](./CanterburyCorpus) directory:


| File                      | Format      | FB LRECL | VB LRECL |
| ------------------------- | ----------- | -------- | -------- |
| enwik8.xml                | Text        |      N/A |     4200 |
| Artificial/a.txt          | Text        |        1 |      N/A |
| Artificial/aaa.txt        | Text        |        1 |      N/A |
| Artificial/alphabet.txt   | Text        |        1 |      N/A |
| Artificial/random.txt     | Text        |        1 |      N/A |
| Canterbury/alice29.txt    | Text/Binary |       80 |      N/A |
| Canterbury/asyoulik.txt   | Text/Binary |       80 |      255 |
| Canterbury/cp.html        | Text        |      180 |      255 |
| Canterbury/fields.c       | Text        |       80 |      255 |
| Canterbury/grammar.lsp    | Text        |       80 |      255 |
| Canterbury/kennedy.xls    | Binary      |        1 |      N/A |
| Canterbury/lcet10.txt     | Text        |      100 |      255 |
| Canterbury/plrabn12.txt   | Text/Binary |       80 |      N/A |
| Canterbury/ptt5           | Binary      |        1 |      N/A |
| Canterbury/sum            | Binary      |        1 |      N/A |
| Canterbury/xargs.1        | Text        |       80 |      255 |
| Large/bible.txt           | Text        |      529 |     1000 |
| Large/E.coli              | Text        |        1 |      N/A |
| Large/world192.txt        | Text        |       80 |      255 |
| Miscellaneous/pi.txt      | Text        |        1 |      N/A |


### Test Data

The test data itself is in the following directories:

| Directory | Contents                                                               |
| --------- | ---------------------------------------------------------------------- |
| TERSED    | Tersed z/OS datasets                                                   | 
| ZOSBINARY | Data transferred from z/OS using BINARY and SITE RDW options           |
| ZOSTEXT   | Data transferred from z/OS using ASCII and SITE TRAILINGBLANKS options |


### Notes

#### File TERSED/FB.A.TXT.SPACK (Artificial/a.txt)

Compression of a single character dataset using SPACK seems to be broken. This file fails unit tests, however it also uncompresses incorrectly using AMATERSE on z/OS.

#### File TERSED/VB.ENWIK8.XML.PACK (enwik8.xml)

Compression of this file results in repeated messages:

AMA513I  AN EMPTY RECORD WAS FOUND. THE DATA SET MIGHT NOT BE ABLE TO BE UNPACKED ON OTHER OPERATING SYSTEMS 