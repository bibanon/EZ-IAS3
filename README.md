Assorted Archival Scripts
=========================

These are a collection of miscellaneous archival and upload scripts used by the Bibliotheca Anonoma.

## liveliness.sh

```
liveliness.sh : Checks the HTTP status code of all urls in a given url-list.

Usage:
liveliness.sh url-list.txt 


To continue where you left off before, first find the last URL scraped:
    cat results-2014-01-03.txt | tail
Then search for the URL given in url-list.txt:
    grep -n "example.com" url-list.txt
Then cut a slice of the url-list.txt starting only from that line number.
    sed '1,/example.com/d' url-list.txt > url-list-slice.txt
Finally, run the script again on the slice:
    liveliness.sh url-list-slice.txt
```

EZ-IAS3
=======

A set of cross-platform Internet Archive upload scripts for manipulating metadata. For typical upload, you should just use the `ia upload` function of the `internetarchive` python library.

## Usage (Python)

> Explanation of how to use the Python Scripts.

### Set up Internet Archive Python Library

Install the Internet Archive Python library:

```
pip3 install internetarchive
ia configure
```

Enter your Internet Archive account username and password. You will then be registered.

### Append subjects to a single item

Use this script to append subjects to a single item. This adds on top of the existing subjects, rather than deleting them.

```
python3 ia-subject-mod.py <identifier> 'subject1' 'subject2'
```

### Append tags to all items uploaded by a certain person

> **Note:** You can only use this script for your own account.

Use this script to append some tags (in this case, `subject1;subject2;subject3`) to all items in your account.

```
python3 ia-multisub-mod.py 'youremailaccount.registeredtoIA@gmail.com' 'subject1' 'subject2' 'subject3'
```

## Usage (Bash)

Currently, we are using basic Curl to send HTTP headers using a bash script. There are two of these scripts:

### Internet Archive S3-Like API Keys

Before starting, you need to create an Internet Archive account and obtain your S3 API keys. Get them (or revoke/renew them) at the link below:

<https://archive.org/account/s3.php>

![](http://i.imgur.com/E7CnZIT.png)

First, edit both `iaupload.sh` and `iareupload.sh`, find the lines below, and add the IA S3 Access/Secret Keys that you got from the link above.

```bash
# set required parameters
export IA_S3_ACCESS_KEY="ENTERACCESSKEYHERE"
export IA_S3_SECRET_KEY="ENTERSECRETKEYHERE"
```

### `iaupload.sh`

Use `iaupload.sh` if you are creating a new item while uploading. You will need to edit the script and specify the following item information as required by the Internet Archive:

```
# item information
export ITEM_TITLE="The Do/k/ument"
export ITEM_DESCRIPTION="The Do/k/ument is a collection of military training manuals, 4chan screencaps, and survival guides for the 4chan /k/ enthusiast. The Murdercube maintains a modern version. https://murdercube.com <\br>Magnet Link: magnet:?xt=urn:btih:f4294ed6278a78be6200131044a7e058017c2dbf&dn=The+Do%2Fk%2Fument&tr=udp%3A%2F%2Ftracker.openbittorrent.com%3A80&tr=udp%3A%2F%2Ftracker.publicbt.com%3A80&tr=udp%3A%2F%2Ftracker.istole.it%3A6969&tr=udp%3A%2F%2Fopen.demonii.com%3A1337"
export ITEM_KEYWORDS="Bibliotheca Anonoma;4chan;4chan /k/;/k/;Weapons;Survival;Defense"

# IA Directory
export FILE_IA_DIRECTORY="the-dokument"
export FILE_IA="http://s3.us.archive.org/$FILE_IA_DIRECTORY/$FILE_LOCAL_NAME"
```

After that's set up, you can use the script as below:

* Usage: `./iaupload.sh <path>/<filename>`
* Example: `./iareupload.sh ~/downloads/item.7z`

### `iareupload.sh`

Use `iareupload.sh` if you just need to upload files to an existing item. You only need to specify the filepath and the item name.

* **Usage:** `./iareupload.sh <ia-s3-bucket> <path>/<filename>`
* **Example:** `./iareupload.sh ibm-pccbbs ~/downloads/item.7z`

### `iafolderupload.sh`

Use `iafolderupload.sh` to upload the contents of an entire folder.

## Feature Wishlist

* Some sort of progress bar/ETA. For some reason we're not getting anything from Curl.
* Complete folder upload. We want to upload entire folder structures to the IA, and most tools don't make it easy.
* Python Requests version, for more extensibility. We could even create an IA S3 wrapper while we're at it, to attach it to things like the BASC-Archiver.
* PyQt/Kivy GUI. Make it easy for people to drag and drop files in, select multiple files, or choose entire folders.
  * Static HTML/JS Web GUI - People are lazy and love webapps, so might as well create one that would work on Github Pages.

## Sources

* IA S3 Headers in Curl based on info from here: https://gist.github.com/Asparagirl/6206247
