photolint
=========

your private photo collection has become an unmaintained directory tree full of meaningless folder names, duplicate photos and abandoned working copies like `try1.psd`? 

<online photo competition use case> 

Keeping your photo collection smooth and tidy
---------------------------------------------
Meet photolint. This command-line tool is inspired by the famous [jslint code quality tool](http://www.jslint.com/) and allows you to establish a common quality standard in a private or contributed photo collection.

More specifically ``photolint`` allows you to enforce a quality standard regarding...
* **the contained photos**:  only tolerate photos that haven't been modified in image editors, that still contain their original filename, that support a minimum JPEG quality,...
* **the equipment used to produce the photos**: only allow photos that were taken with dslr or smartphone cameras, with certain lenses, without or without builtin flash,...
* **the photo folder structure**:  enforce naming patterns like `YYYY-MM-DD description` across all folders of your photo collection tree and avoid nonsense folder names like `pics of may`

Usage
-----
First, install photolint by downloading it or installing it via [pip](https://pip.pypa.io/):
```
pip install photolint
```
Then run photolint from the base directory of your photo collection
```
> photolint -r D:\photos
D:\photos\pics from Santorin: folder doesn't follow YYYY-MM-DD naming convention
D:\photos\pics from Santorin\IMG98273.preview.JPG: does not meet required JPEG quality of 80
D:\photos\pics from Santorin\IMG98273-01.JPG: is a duplicate to IMG98273.JPG
...
```
Note: the `-r` option tells photolint to scan recursively through your photo collection directory, documentation on photolint runtime options coming soon.

Configuration
-------------
photolint comes with a default set of quality checks (validators), but was designed to be very configurable. You can configure its behaviour by putting a ``.photolint`` file into the base directory of your photo collection, which is actually a json file that turns quality checks on/off or configures them:

```
{
"folderpattern": "%Y-%m-%d_%s", 
"minjpegquality": 70,
"nodupes": true,
"untampered": false,
"anticorrupt": false,
"filepattern": "dcf",
"noexif": false,
"requirecopyright": false
}
```

(See below for a description of the validators and their options)

Validators
-----------

### `folderpattern`
Use this pattern to ensure a consistent naming of all your photo folders, e.g. `2014-11-27_Paris`. The pattern can either be a regular expression or a [strptime-parseable date pattern](https://docs.python.org/2/library/datetime.html#strftime-strptime-behavior), such as `%Y-%m-%d_%s` (Note: the `%s` placeholder denotes an arbitrary string).

### `filepattern`
Use this pattern to ensure a consistent naming of all photo files, e.g. `P8175804.ORF`. To accomplish this, you can either specify the pattern in [fnmatch](https://docs.python.org/2/library/fnmatch.html#module-fnmatch) or [regex](https://docs.python.org/2/library/re.html) format. If you want to enforce [DCF](http://en.wikipedia.org/wiki/Design_rule_for_Camera_File_system) you can use the `dcf` pattern  All files that will not match the pattern will be reported. 

### `minjpegquality`
this validator tries to identify the jpeg compression quality by using [JPEG DQT Digests](http://search.cpan.org/dist/Image-ExifTool/lib/Image/ExifTool/JPEGDigest.pm) and reports images that are below the threshold (0..100). Use this validator to ensure a minimum compression quality in your photo collection.

### `untampered`
reports photos that have been modified in an image processor by performing metadata analysis. Useful if you want to archive the original images from your camera and make sure they haven't been been modified.

### `anticorrupt` 
this validator reports corrupted files (e.g. due to memory card writing errors).

### `noexif`
use this validator to ensure that your photos are free from exif metadata (e.g. before handing them over to the client)

### `requirecopyright`
use this validator to ensure that all your photos contain the IPTC copyright tag.

... MORE VALIDATORS TO COME SOON...


