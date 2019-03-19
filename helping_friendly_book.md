# The Helping Friendly Book  
This is an ever-growing collection of commands, tricks and tips -- Read the Book.

## Python
---
#### Create virtual environment on Mac/Linux:  
```pip install virtualenv```  
```virtualenv --python=python2.7 venv```  
#### Activate virtualenv:  
```. venv/bin/activate```  

#### Load JSON file:  

```
def load_config():  
	with open(os.path.join(path_to_config, file_name) as conf:  
		cfg_file = json.load(conf)
```  

#### Find section of a string:  

```
result = re.search(r'Frequency:(.*)Hz', 'Frequency:44100Hz')  
print result.match(1)
# 44100
```  
#### Find digits:  

```match = re.search(r'\d+', 'I am 27 years old')```  
  
#### Find Serial Number:  
  
```pattern = re.compile("^([0-9]){8}$")```  

#### Get MAC Address from User:  
  
```
pattern = re.compile("^([0-9a-fA-F]{2}:){5}[0-9a-fA-F]{2}$")  
first_attempt = True
match = False  
while not match:
	if first_attempt:
		mac_addr = raw_input('Scan or type your MAC Address and press Enter: [>>] ')
	else:
		mac_addr = raw_input('MAC Address is not valid! Try again: [>>] ')
	
	match = pattern.match(mac_addr)
```  
#### Create SSH and SCP Instances  

```
import paramiko  


ssh_client = paramiko.SSHClient()  
ssh_client.set_missing_host_key_policy(paramiko.AutoAddPolicy())  

ssh_client.connect(
	hostname=<host>,
	username=<username>,  
	password=<password>
	)

scp_client = SCPClient(ssh_client.get_transport())
```  
#### Execute command over SSH and read stdout:  

```  
stdin, stdout, stderr, = self.ssh_client.exec_command(
	 '<your command> '
	 '-o <an option>'
	 '-a <another option>'
	 )  

exit_status = stdout.channel.recv_exit_status()  
lines = stdout.readlines()
if exit_status is not 0:
	self.log.error("[!!] Command Failed!!! Exit status {}".format(exit_status))
	for line in lines:
		self.log.error("[!!] Error message: %s", line.rstrip('\n'))  
		raise Exception('<your command> failed')
else:
	for line in lines:
    	self.log.info('\t{}'.format(line.rstrip()))
```  
#### Send and receive over SCP:  

```  
scp_client.put(<path_to_file>, <location_to_put_file>)  

scp_client.get(<path_to_file>, <location_to_get_file>)
```  
#### Get memory address of Python object:  

```
cat_sound = 'meow'   
print hex(id(cat_sound))
# 0x10a96fea0
```  
#### Retrying Decorator on Exception (on requests ConnectionError):  

```  
def retry_if_connection_error(exception):
    """Return True after requests connection error"""
    return isinstance(exception, requests.exceptions.ConnectionError)

@retrying.retry(wait_fixed=2000, stop_max_attempt_number=3,
                retry_on_exception=retry_if_connection_error)
def post(self):
	<try to post>
```  

#### Timeout  

```  
timeout = 30
start_time = time.time()
do_a_thing = True

while do_a_thing:
	current_time = round(time.time() - start_time, 3)
	if current_time >= timeout:
		do_a_thing = False
		break/continue
```  

## Git / Version Control  
---
#### Git Basic Commands:  
* **git status (-uno)**: display the state of the working directory and the staging area. The -uno option will hide untracked files.
* **git branch (-r)**: display current branch and other local branches. The -r option will display all remote branches on repository.
* **git add**: add a change in the working directory to the staging area.
* **git commit (-m 'Commit Message')**: save a snapshot of the staging directory to the repositories commit history.
* **git push (origin branch_name)**: send the committed changes to remote repositories.
* **git fetch**: update remote-tracking branches under refs/remotes/remote
* **git pull (--rebase)**: fetch and download content from a remote repository and immediately update the local repository. The rebase option can be used to ensure a linear history by preventing unnecessary merge commits.
* **git stash**: store your changes you've made to your working copy. This is needed before pulling if changes to files have been made.
* **git stash pop**: revert the previous stash command.
* **git clone (repository url)**: make a local copy of a remote repository.  

#### Feature Branch Workflow:  

```  
# checkout master branch
git checkout master

# create feature branch (copy of master)
git checkout -b feature_branch

# push feature branch to remote
git push origin feature_branch

# write some code! then add/commit
git add .
git commit -m 'This feature branch is awesome'

# push commits to remote
git push origin feature_branch

# pull and rebase from master branch
# this ensures a linear history when feature branch is merged back to master
# this step may involve resolving conflicts (better now than later)
git pull --rebase origin master

# push to remote after rebase
git push -f origin feature_branch

# push your changes to master
git push origin refs/heads/feature_branch:refs/heads/master

# checkout master and pull in your changes
git checkout master
git pull origin master

# remove your remote and local feature branch
git push origin -d feature_branch
git branch -D feature_branch
```  
#### SVN Basic Commands:  
* **Checkout**: 'svn checkout <URL> <checkout directory name>'
* **Status**: 'svn status'
* **Add**: 'svn add'
* **Update**: 'svn up'
* **Commit**: 'svn commit'
* **Get Info**: 'svn info'
* **Get Commit History**: 'svn log -l5 -v URL_of_your_repository'

## Command Line
---
#### Show USB devices on MacOS:  
```ioreg -p IOUSB -l -w 0```  

#### Monitor Linux eth ports:  
```while [ 1 ]; do  cat /proc/net/dev; sleep 1; clear; done```  

#### zip a folder:  
```zip -r -X zipped_name.zip folder_to_zip```  

#### Add public rsa ssh keys for remote machine access:  

```
cat ~/.ssh/id_rsa.pub  
# copy public key
ssh host -l user
vi ~/.ssh/authorized_keys
# paste public key
```  
#### Resolve ECDSA fingerprint change error:  

```ssh-keygen -R IPv4_of_remote_machine```  

#### Interactive process viewer and manager for Unix:  
```htop (M to sort by memory usage)```  

#### sed to replace text:  

``` 
# replace foo with bar, replace fee with fi 
sed -i '' -e 's|foo|bar|g' -e 's|fee|fi|g' *
```  
#### Find and Connect to Serial Device  
Find device in /dev/ and connect using screen  
```screen /dev/device_handle baud_rate -L```  
To quit and detach from screen:  
``` ctrl-alt-a ctrl-\ (y)```  

**minicom also works well**

#### DHCP Stuff:  

Check if processes are running:  

```
ps -elf | grep -i dhcp
lsof -i udp -nP
```  
Leases: **/var/lib/dhcp/dhcpd.leases**  
Config: **/etc/dhcp/dhcpd.conf**  
Server: **/etc/init.d/isc-dhcp-server** (to restart: **/etc/init.d/isc-dhcp-server restart**)  

#### TFTP Stuff:  

Config: **/etc/inet.d.conf** -- inet.d.conf is a super-daemon (runs other daemons)  

List TFTP Ports (port 69):  
```lsof -i udp -nP```

## Vim and Bash Scripting  
---
**Remove the first 'n' number of characters from each line:**  
```:%s/^.\{0,'n'\}//```  

**~/.vimrc config:**  

```
syntax on
# tab length
set tabstop=4
set expandtab

# nice theme
colorscheme desert

# use fn F6 to toggle line numbers
" set number
" Set toggle for line number
set nonumber
nnoremap <F6> :set number!<CR>  
```  
#### Bash Stuff:  

**Colored Icons:**  

```
COL_NC='\x1B[0m' # No Color
COL_LIGHT_GREEN='\x1B[1;32m'
COL_LIGHT_RED='\x1B[1;31m'
TICK="[${COL_LIGHT_GREEN}✓${COL_NC}]"
CROSS="[${COL_LIGHT_RED}✗${COL_NC}]"

if [ $? != 0 ]; then
    echo -e "${CROSS} Failed"
    exit 1
else
    echo -e "${TICK} Success."
```  

**Look for environment variable in list of variables:**  


```
# list available options
OPTIONSLIST="option1 option2 option3"

# check if ${1} is in list of options
if [[ $TESTLIST =~ (^|[[:space:]])${1}($|[[:space:]]) ]]; then
    echo -e "${TICK} Valid Test Name: ${1}"
else
    echo -e "${CROSS} Invalid Test Name -- select from the following tests: ${TESTLIST}"
    exit 1
fi
```
