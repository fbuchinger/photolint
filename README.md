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
photolint comes with a default set of quality checks, but was designed to be very configurable. You can configure its behaviour by putting a ``.photolint`` file into the base directory of your photo collection, which is actually a json file that turns quality checks on/off or configures them:

```json
{
"folderpattern": "YYYY-MM-DD_%s", //required naming pattern for folders containing photos
"minjpegquality": 70, //minimum encoding quality for jpegs [0..100]
"nodupes": true, //check for binary identical duplicates in the same folder
"untampered": false, //allow only non-modified photos directly as they come from your camera
"anticorrupt": false, //check for corrupted files (e.g. due to memory card writing errors)
"filepattern": "fromcamera", //check if the photo file still carries the filename from camera
"noexif": false, //only allow images without exif metadata
"requirecopyright": false //IPTC copyright tag must be set
}
```
More on the configuration settings coming soon
