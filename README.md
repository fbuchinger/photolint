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
"folderpattern": "YYYY-MM-DD_%s", 
"minjpegquality": 70,
"nodupes": true,
"untampered": false,
"anticorrupt": false,
"filepattern": "fromcamera",
"noexif": false,
"requirecopyright": false
}
```

(See below for a description of the validators and their options)

Validators
-----------

### `folderpattern`
required naming pattern for folders containing photos, e.g. "YYYY-MM-DD_%s"

### `filepattern`
required naming pattern for folders containing photos, e.g. "YYYY-MM-DD_%s"
"fromcamera"

### `minjpegquality`
minimum encoding quality for jpegs [0..100]

### `untampered`
allow only non-modified photos directly as they come from your camera

### `anticorrupt` 
check for corrupted files (e.g. due to memory card writing errors)

### `noexif`
only allow images without exif metadata

### `requirecopyright`
IPTC copyright tag must be set



