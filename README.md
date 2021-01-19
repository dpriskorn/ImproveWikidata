# ImproveWikidata
ImproveWikidata is a python REPL TUI that reads from a rule-directory with
json-files and lets the user semi-automatically improve Wikidata based on the
choosen rule.

In essence this is a TUI frontend to
[[Wikidata:WikibaseIntegrator|WikibaseIntegrator]], that enables users to share
json rules using git or other measures and collaborate on editing Wikidata in a
semi-automatic way in the terminal.

It's licensed under GPL-v3 or later unless they are snippets from someone else
(source is then noted above the code)

## Requirements
* Python
* requests 
* wikibaseintegrator

Install requirements using your OS package manager or by using pip:
`$ sudo pip install wikibaseintegrator requests`

If pip fails with errors related to python 2.7 you need to upgrade your OS. E.g. if you are using an old version of Ubuntu like 18.04.

## Getting started
To get started install the following libraries with your package manager or
python PIP:
* requests
* wikibaseintegrator

Please create a bot password for running the script for
safety reasons here: https://www.wikidata.org/wiki/Special:BotPasswords

Add the following variables to your ~/.bashrc (recommended): 
export IMPROVEWIKIDATA_USERNAME="username"
export IMPROVEWIKIDATA_PASSWORD="password"

Alternatively edit the file named config.py yourself and adjust the following
content:

username = "username"
password= "password"

### Pseudo code describing the internal operation of the script
load the config
check if username and password is set and fail if not
read the rules directory
start a repl and wait for a command

command run [rulenumber]
run the rule with the number or fail

command list
list rules and exit

command show [rulenumber]
show the rules details in human readable form

command new [optional name of new rule] 
create a new rule by asking the user a series of questions

run:
read the rules json
fail if invalid 
fetch the items limiting to the first 50
loop through the items
  skip if improvement is already made
    show the improvement to be made
    ask the user whether to make it or not
      upload to WD

list:
loop through the rules and list 
e.g.: 
number name                                 valid 
1      add-operating-area-to-swedish-unions yes

if no rules exist encourage the user to create a new one

new:
ask for name if not given
ask for type (add/remove)

note: during all these questions it should be possible to search for either PID
or QID, if the search matches a valid item, accept it immediately

ask for predicate
ask for object
while loop: ask if narrow is wanted
  ask for predicate
  ask for object

if add:
  ask for add predicate
  ask for add object
while loop: ask if qualifier is wanted
  ask for predicate
  ask for object
while loop: ask if reference is wanted
  ask for predicate
  ask for object
  ask if retrieved date is today
  while loop: ask if qualifier is wanted
    ask for predicate
    ask for object

note: this function for now only supports removing either the whole
predicate/object or a single qualifier or a single reference or qualifier on a
reference. 

if remove:
  ask for remove predicate
  ask for remove object
  ask if the whole thing should be removed or only a qualifier or reference
    while loop: ask if qualifier is wanted
      ask for predicate
      ask for object
    while loop: ask if reference is wanted
      ask for predicate
      ask for object
      while loop: ask if qualifier is wanted
        ask for predicate
        ask for object
      
