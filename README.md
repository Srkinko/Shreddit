# Shreddit

Original repo and Readme.md [here](https://github.com/x89/Shreddit).

# Donation

### If you wish to support development and bugfixing of this script or any others I work on please consider a donation

[![Donate via Paypal.me](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://paypal.me/djbp)

# FAQ - Common Error(s)

1. `<something> installed but version <something> is required.` - Upgrade 'setuptools' with `pip install --upgrade setuptools`
2. `Command "python setup.py egg_info" failed with error code 1` - You are missing a dependency. Try installing using manual instructions below.
3. ` Invalid requirement: '<<<<<<< HEAD' Traceback (most recent call last): et cetera, et cetera1` - You are trying to install the original repo's broken code. Clone and install mine with the fixes.

# Pre-install conditions:

Make sure you install pip

`$ apt install python3-pip`

Then update pip with

`$ python3 -m pip install --upgrade pip`

Note: Python2 is now depreciated. Use python3.

## Manual Installation

1. Clone the `shreddit` repository to a directory: `$ git clone https://github.com/pythonInRelay/Shreddit.git`
2. From the directory, run `$ pip3 install -r requirements.txt`
3. Run `$ python3 setup.py install` to install the package and the `shreddit` command line utility.  This is typically
   either run in a virtualenv or using administrative privileges for global installation (so you can run the `shreddit` command from anywhere).

## Post-install steps

After installing the `shreddit` command line utility, the first step is setting up the tool's configuration files.
Simply typing `shreddit -g` will generate configs. After configuring your user credentials and deletion scope (in the created praw.ini), running the tool with the `shreddit`
command will begin the tool's operation. Sit back and wait for it to parse each comment.

### Configuring Credentials

Running `shreddit -g` will generate a blank praw.ini file that looks like this:

```
# Credentials go here. Fill out default, or provide one or more names and call shreddit with the -u option to specify
# which set to use.
[default]
client_id=
client_secret=
username=
password=
ratelimit_seconds=31 ## HIGHLY RECOMMENDED as Reddit now limits API calls
```

**You must provide values for each of these.** As strange as it may seem to provide both a username/password pair *and*
a client id/secret pair, that is how the Reddit API does "OAuth" script applications.

Username and password are simply your Reddit login credentials for the account that will be used. However, to obtain the
client ID and secret, follow these steps (taken from 
[PRAW documentation](http://praw.readthedocs.io/en/latest/getting_started/authentication.html#script-application)):

1. Open your Reddit application preferences by clicking [here](https://www.reddit.com/prefs/apps/).
2. Add a new application. It doesn't matter what it's named, but calling it "shreddit" makes it easier to remember.
3. Select "script".
4. Redirect URL does not matter for script applications, so enter something like http://127.0.0.1:8080
5. Once created, you should see the name of your application followed by 14 character string. Enter this 14 character
   string as your `client_id`.
6. Copy the 27 character "secret" string into the `client_secret` field.

Finally, your praw.ini should look like this (with fake data provided here):

```
[default]
client_id=f3FaKeD4t40PsJ
client_secret=dfK3pfMoReFAkEDaTa123456789
username=testuser
password=123passwordgoeshere123
```

Keep your praw.ini either in the current directory when running `shreddit`, or in one of the config folders
[described here](http://praw.readthedocs.io/en/latest/getting_started/configuration/prawini.html) such as
`~/.config` in Linux or `%APPDATA%` in Windows.

To use more than one account, you can add multiple profiles instead of just `[default]` and use the `-u` option to 
`shreddit` to choose which one each time.

### Automating

The easiest way to automate this tool after the first run is by using the cron utility. Run `crontab -e` to edit your
user's crontab settings.

**Examples:**

The following examples require that the PRAW configuration file is located in the config directory. See [this PRAW
documentation](http://praw.readthedocs.io/en/latest/getting_started/configuration/prawini.html) for more information.

- Run every hour on the hour
        `0 * * * * shreddit -c <full path to shreddit.yml>`

- Run at 3am every morning
        `0 3 * * * shreddit -c <full path to shreddit.yml>`

- Run once a month on the 1st of the month
        `0 0 1 * * shreddit -c <full path to shreddit.yml>`

If virtualenv was used, be sure to add `source /full/path/to/venv/bin/activate &&` before the command. For example:

`0 * * * * source /full/path/to/venv/bin/activate && shreddit -c <full path to shreddit.yml>`

### Command Line Options

```
$ shreddit --help
usage: app.py [-h] [-c CONFIG] [-g] [-u USER]

Command-line frontend to the shreddit library.

optional arguments:
  -h, --help            show this help message and exit
  -c CONFIG, --config CONFIG
                        Config file to use instead of the default shreddit.yml
  -g, --generate-configs
                        Write shreddit and praw config files to current
                        directory.
  -u USER, --user USER  User section from praw.ini if not default
```

## For Windows users

0. Get Linux. *No really...*

1. Make sure you have Python installed.
   [Click here for the Python download page](https://www.python.org/downloads/).
        - **Note:** Install `python 3.x`, not `2.x`.
2. Follow the [pip installation](#pip-installation) instructions.
3. Open a new command prompt and verify that the `shreddit` command works before moving on to the [usage](#usage)
   section.
