<div align="center">

# <img src="http://icons.iconarchive.com/icons/alecive/flatwoken/512/Apps-Terminal-Pc-104-icon.png"  height="50px" width="50px" >bsupdate<img src="http://icons.iconarchive.com/icons/alecive/flatwoken/512/Apps-Terminal-Pc-104-icon.png"  height="50px" width="50px" >

### (Bash Script Update)
#### A lightweight drop in bash script that can be added to any bash application/CLI to automate updating
##### Less then 4 KB

If you have a bash application/script that has an installer script and you host the project on github this script can automate updating.

![Usage](https://media.giphy.com/media/xUPGcszQZQWbPn4d8I/giphy.gif)

</div>

## Usage
* First clone the repository. ```git clone https://github.com/alexanderepstein/bsupdate```
* Git checkout to the latest stable version ```git checkout v2.1.0```
* Then edit the update utility.sh file located in the repository and change the variables at the top of the file to configure your updates. Read the comments on the variables to make sure that you do this correctly
* Run chmod on the updateutility.sh file to make it executable ```chmod a+x updateutility.sh```
* Copy the file over to the root directory of your project
* In the main script of your application add the ability to use either -u or --update and run my update utility.sh file (Look at the section on this on the README if don't already know how to implement this)
* Inside of your installer script make sure to copy this file next to wherever your main application script sits so the previous step works as expected
* Make sure to check the assumptions section of the README as if these assumptions are not met it will not work without some tinkering

## Assumptions
* All updates come through the form of github releases.
* Releases for unstable versions do not change the number on the version tag but add either a,b,rc to the end. The updater will not consider these as updates to end users.

## Adding the -u/--update functionality in your script

### The latter of the two options presented here allows for both -u and --update while the first only works with -u

#### If you are already using bash options this is what adding the -u functionality would look like
``` bash
while getopts ":uht:" opt; do
  case $opt in
    \?)
      echo "Invalid option: -$OPTARG" >&2
      exit 1
      ;;
    t)
      #do stuff
    ;;
    h)
      #do stuff
    ;;
    u)
      ./updateutility.sh #calling the update utility
    ;;
    :)
      echo "Option -$OPTARG requires an argument." >&2
      exit 1
      ;;
  esac
done
```

#### If you aren't using bash options then it would make more sense to do the following at the top of your script
``` bash
if [[ "$1" == "--update" || "$1" == "-u" ]]; then
	./updateutility.sh ## calling the update utility
	exit 0
fi
```
#### If you want to circumvent adding the file to your code you can just copy paste the code (after cloning b/c of copy paste issue in browsers w/ code) into the main script of your CLI
Then use either one of the above examples but instead of calling ```./updateutility.sh``` just call
```bash
getConfiguredClient || exit 1
checkInternet || exit 1
latestVersion=$(httpGet https://api.github.com/repos/$githubUserName/$repositoryName/tags | grep -Eo '"name":.*?[^\\]",'| head -1 | grep -Eo "[0-9.]+" ) #always grabs the tag without the v option
update || exit 1
```

## But you update this repository so how do I automate updating the updater?
There is no quick way to do this as the updater requires information from the user so it cannot just replace the code without having access to these variables.
I could workaround around this by having two files, but that would make it harder for the end user to implement whereas this is supposed to be simple to setup.
For now just it would make sense to watch the repository if you use it so that you will be notified for updates to bsupdate and can implement new versions of the updater as you see fit.

## Donate
If this project helped you in any way and you feel like buying a broke college kid a cup of coffee

[![Donate](https://img.shields.io/badge/Donate-Venmo-blue.svg)](https://venmo.com/AlexanderEpstein)
[![Donate](https://img.shields.io/badge/Donate-SquareCash-green.svg)](https://cash.me/$AlexEpstein)

## License

MIT License

Copyright (c) 2017 Alex Epstein

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
