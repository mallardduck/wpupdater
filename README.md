# WpUpdater

A script for quickly updating or replaceing WordPress core files with those of the `latest.tar.gz`

This is a nifty little bash script that can be used to quickly update, or refresh, the Core WordPress files of a website. This can be particularly useful for hacked WordPress sites that need the core files updated. While this can be done manually by anyone and they can probably complete it within a quick time [5-10min], the benefit of this is that the script can do it much faster and it does it in a way that verifies the MD5 of the souce packages.

An ideal use case would be situations where a customer has asked us to update a bunch of WordPress sites, or if a WordPress has been compromised then this will quickly backup the current core files and then put in place verifiable clean versions. In either case this script is useful since the time this script takes to remove and replace the core files is much faster than what a human could do; as a result this decreases any downtime between the folders deletion and recreation.

## Setup

The script is hard coded to run from `/usr/local/bin/wpupdater` so you can really 'install' this how ever you want; either using Git to pull/clone the repo into that folder, or just grab the zip/tar and extract it there. GitHub likes to put the repo into a folder named after the branch, so you might have to just extract the zip somewhere and then move just the `wpupdater` folder into `/usr/local/bin/`.

The end result is that the script must be run in: `/usr/local/bin/wpupdater`
The directory tree should look something like this:

```
/usr/local/bin/wpupdater
    ./wpupdater.sh
    ./colors
    ./wpupdater.sh.md5
```

Once the files are in place it might be nice to symlink this to a useful spot. Some examples might be trying:

    ln -s /usr/local/bin/wpupdater/wpupdater.sh /usr/local/bin/

or

    ln -s /usr/local/bin/wpupdater/wpupdater.sh /scripts/
    
The first one is useful since `/usr/local/bin` is usually in the system `PATH` variable, doing this would allow you to call wpupdater.sh from any locaiton on the server.

The second one is cool for use on cPanel servers since people will generally look for scripts in this location by habit.

## Usage

To use this script you will simply provdie the script a flag and a location of the WordPress install. The path given should be the valid locaiton of the `wp-admin` and `wp-includes` folders. Additonally, something to note is that it does a sanity check for the `wp-config.php` file in this folder as well.

To run a test on the folder:

     ./wpupdater.sh -t /home/cpuser/public_html

or

     ./wpupdater.sh -t /home/cpuser/public_html/wp

The output of the command in test mode will essentiall just tell you the version that is there and the version it can pull from the `latest.tar.gz`.

To actaully run the script on the folder:

     ./wpupdater.sh -s /home/cpuser/public_html

or

     ./wpupdater.sh -s /home/cpuser/public_html/wp

Once this runs it will report back and will prompt the user for input before actually taking out any steps in the process that will affect any live or active files.

## TO-DO:

* Add usage logging
* Fully move the script to defaulting in `/usr/local/bin/wpupdater`
* Add code that only clears the Temp folder if the script wasn't run the same day.
* Clean up the code for first run and prep.
* Add a `-v` flag for verbose mode
* Adjust some of the DEBUG outputs to be verbose outputs
* Fix the scripts own hash check so that it will first attempt to pull the newest `.sh.md5` file before hard failing.

### Feature Request:

* Add the ability to provide multiple locations to do multiple updates at once
* Add a very basic fallback that attempts to check the most common WordPress subfolder for the install.
* Add an `-l` flag that will report back the recent logged events or usages.
