### Setting proxy in /etc/yum.conf

    proxy=http://proxy.iiit.ac.in:8080


### To arrange a file in the order that higher number of words in each line at top

    awk '{print NF,$0}' file | sort -nr | cut -d' ' -f 2-


**Explanation**:  *awk prints number of fields in each line which is piped to sort commands and then using cut command
we extract the second column that is the actual text*

### To generate all possible word forms from en_US.dic and en_Us.aff

    unmunch en_US.dic en_US.aff

### To input newline in perl command line argument

    ctrl+v ctrl+j , then press return

### To extract text from doc

    catdoc filename.doc

### To convert text to doc
	
	pandoc filename.txt
### To extract text from rtf 
	
	unrtf filename.rf

## Vim commands

    :.! date ##execute a command from vi editor 
    :.! ls

    :earlier 15m ##rolls back to the way the file was 15mins earlier

    :w !sudo tee %  ## write to a file when not logged into sudo 

    vim http://stackoverflow.com/ ## open source code in vim

    :~ ##repeats last substitute command, but uses the most recent search for pattern.

    :%TOhtml  ##current file into html format

    Typing == will correct the indentation of the current line based on the line above.

### To replace sysnet dictionary of 5 lines in single line

    :%s/ID\(.*\)\nCAT\(.*\)\nCONCEPT\(.*\)\nEXAMPLE\(.*\)\nSYNSET-HINDI\(.*\)/ID\1,CAT\2,CONCEPT\3,EXAMPLE\4,SYNSET-HINDI\5/g

## Latex
    yum install texlive
    yum install texlive-latex

## JPM/jpm

### Instansiate JPM
    ./jpm init

### To run:
    ./jpm run -b /usr/bin/firefox

### To deploy or build addon to xpi:
    ./jpm xpi

### To sign JPM
    ./jpm sign --api-key user:12236970:394 --api-secret 372d2d2017bf07279ec1540ce0c85f8a51b62464c347a33b70ae3f9fc85e0002 --xpi \@addto-0.0.2.xpi


### Symlink point to another directory

    ln -sfn /targetdir/ symlink

### To recover text files content that are removed (rm -rf)

    grep -i -a -B100 -A100 '<some unique text form lost file>' /dev/sda2 > file.txt

### To extract a range of lines from a text file 

    sed -n 162,200p filename > newfile

    p - Print out the pattern space (to the standard output). This command is usually only used in conjunction with the -n command-line option.

    n - If auto-print is not disabled, print the pattern space, then, regardless, replace the pattern space with the next line of input. If there is no more input then sed exits without processing any more commands.

### Run sampark using dash command.

    dash -c /home/nagaraju/sampark/bin/sys/hin_pan/hin_pan_setu.spec -i $HOME/in -o /home/nagaraju/myoutput/


### Indent .php/htm files with best possible formatting

    :set filetype=html


## Curl rest api usage

### curl post request:

    curl --data "param1=nagaraju&param2=nagaraju" http://localhost:8020/postedittool-1.6.4/getTaskInfo

### curl get request:

    curl curl http://server:5050/a/c/getName/?param1=pradeep

### File upload using curl (multipart form date)

**You need to use the -F option:
-F/--form <name=content> Specify HTTP multipart POST data (H)**

>	Try this:

    curl \
		-F "userid=1" \
		-F "filecomment=This is an image file" \
		-F "image=@/home/user1/Desktop/test.jpg" \
		localhost/uploader.php  

			(OR)

    curl -i -X POST -H "Content-Type: multipart/form-data" 
    -F "data=@test.mp3" http://mysuperserver/media/1234/upload/  

### Catching the user id as part of the form:

    curl -i -X POST -H "Content-Type: multipart/form-data" 
    -F "data=@test.mp3;userid=1234" http://mysuperserver/media/upload/


### Remove all files except one file:

    find . ! -name 'file.txt' -type f -exec rm -f {} +	//current directory

    find ! -name 'simpleparser-1.1.1.tmp' -type f -exec rm -rf {} +	//all to remove hardlinks

    find ! -name 'simpleparser-1.1.1.tmp' -type l -exec rm -rf {} +	//to remove symlinks

	
### Convert xls to csv:

    ssconvert <input.xls> <output.csv>

    ssconvert 2015-Jun-Dec.xls  2015-Aug-Dec.csv


### Fontawesome version4 to version5 changes

    Change in head section of html page.
	Find and replace /<i class="fa /<i class="fas /g
	
>   **Icon changes**

	fa-pencil to fa-pencil-alt
	fa-refresh to fa-sync
	fa-arrow-left to fa-arrow-long-alt-left
	fa-arrow-long-left to fa-arrow-long-alt-left
	fa-external-link to fa-external-link-alt
	fa-square-o to fa-square
	fa-file-o to fa-file
	fa-check-circle-o to fa-check-circle
	fa-square-o to fa-square
	fa-circle-o-notch to fa-circle-notch


### Vlc command to convert video file into flac file

    vlc -I dummy 56336_Commencing_the_1st_week.mp4 ":sout=#transcode{vcodec=none,acodec=flac,ab=128,channels=1,samplerate=16000,scodec=none}:file{dst='/home/nagaraju/56336_Commencing_the_1st_week.flac',no-overwrite}" vlc://quit

### combine multiple zip files that are split by Google Drive
    mkdir combined
    unzip '*.zip' -d combined

### rsync between two folders excluding some files
    rsync -avz --exclude='morph.log' --exclude='morph_output' --exclude='in.txt'  source_dir/ user@remote:/var/www/html/source_dir



## Process to set up monitor of deleting files

### Install Audit
	sudo apt install auditd audispd-plugins

### Start and enable the service:
	sudo systemctl start auditd
    sudo systemctl enable auditd

### Create an audit rule for the directory you want to monitor: For example, to monitor file deletions in /var/www/html:
    sudo auditctl -w /var/www/html/ -p wxa -k file_deletion_monitor

### or add in below file permanently
    vim /etc/audit/rules.d/audit.rules
	sudo systemctl restart auditd

	-w /var/www/html/: Watches the directory.
    -p wxa: Monitors write, execute, and attribute changes.
    -k file_deletion_monitor: Sets a key for easy searching in audit logs.

### to view deleted files
    sudo ausearch -k file_deletion_monitor -i
	sudo ausearch -k file_deletion_monitor -i | grep "rm"

### check which command was used to delete file
	type=SYSCALL msg=audit(1633980915.351:290): arch=c000003e syscall=87 success=yes exit=0 a0=ffffff9c a1=1 a2=1 a3=7fff6d3960f0 items=2 ppid=1234 pid=5678 auid=1000 uid=1000 gid=1000 euid=1000 suid=1000 fsuid=1000 egid=1000 sgid=1000 tty=pts1 ses=3 comm="rm" exe="/usr/bin/rm" key="file_deletion_monitor"


## Python Virtual Environment VENV

### Creating VENV
    python -m venv /path/to/new/virtual/environment

### activating VENV
    source ada-env/bin/activate

### recursively find word count and sort in directory
	find . -type f -exec wc -w {} \; | sort -n
