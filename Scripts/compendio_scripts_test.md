# Compendio de Scripts útiles e interesantes

## 'pdftk' related:
### Printing Booklets With 'pdfjam'
This creates a file name file-book.pdf. This file is ready for duplex printing and folding into a booklet afterwards.

```{bash}
pdfbook --short-edge file.pdf
```

### Remove pages from a PDF using 'pdftk'
From PDF.pdf remove pages 22 and 90, then save it to PDF_NEW.pdf

```{bash}
pdftk A=PDF.pdf cat A1-21 A23-89 A91-end output PDF_NEW.pdf
```

### Merge several PDFs using 'pdftk'
Retrieves PDF1.pdf PDF2.pdf PDF3.pdf and merges them into a new file called PDF_JOINED.pdf

```{bash}
pdftk PDF1.pdf PDF2.pdf PDF3.pdf cat output PDF_JOINED.pdf
```

### Make a blank single-paged pdf file using 'pdftk' and 'ImageMagick'
A4 Vertical blank page:

```{bash}
convert xc:none -page A4 a.pdf
```

A4 Horizontal blank page:

```{bash}
convert xc:none -page 842x595 a.pdf
```

### Shuffle evenly blank pages in a pdf

```{bash}
pdftk A=original.pdf B=blank.pdf cat A1 B1 A2 B1 A3 B1 A4 B1... output shuffled.pdf
```

## System related

### Rename a bash command
Using `alias` command allows one to rename commands as the examples suggests:

```{bash}
cd:
alias code-pb='cd ~/Dropbox/code/code-pb; ll' # Change folder to code-pb 
alias lpthw='cd ~/Dropbox/code/code-pb/python/LPTHW; ll' # Change directory to LPTHW3 folder
alias dt='cd ~/Desktop; ll'
alias dl='cd ~/Downloads; ll'
alias db='cd ~/Dropbox; ll'
alias dm='cd ~/Documents; ll'
alias home='cd ~'
alias ..='cd ../'
alias ...='cd ../../'
alias usb='cd /media/hector/; ll' # Change directory to usb folder

ls:
alias ll='ls -lF'
alias ññ='ls -lF'
alias la='ls -alF'
alias l='ls -CF'

System:
alias update='sudo apt-get update' # Update system
alias upgrade='sudo apt-get upgrade' # Upgrade system
alias rm='rm -i' # Always asks confirmation on whether a file or dir wants to be deleted
alias rmg='rm *~' # Remove every file that gedit uses as a backup

4Python:
alias venv-activate='source venv/bin/activate' # Activate virtual environment

Miscellaneous:
alias open='xdg-open' # To open with the default app.
```

**NOTE1**: Place all aliases in `~/.bash_aliases`, as Ubuntu's guide suggests.

**NOTE2**: After editing file, run `. ~/.bash_aliases` so that the code can be runnable every time a terminal is opened.

### Update all outdated python libraries
Solution 1:

```{bash}
pip freeze --local | grep -v '^\-e' | cut -d = -f 1  | xargs -n1 pip install -U
```

Solution 2:

```{python3}
import pip
from subprocess import call

packages = [dist.project_name for dist in pip.get_installed_distributions()]
call("pip install --upgrade " + ' '.join(packages), shell=True)
```

### Sync jupyter with venv

```{bash}
python3 -m venv projectname
source projectname/bin/activate
pip3 install ipykernel
ipython kernel install --user --name=projectname
```

## Miscellanous - ATM RN ;)) -
### Convert epub to mobi
Single book

```{bash}
ebook-convert "book.epub" "book.mobi"
```

batch conversion

```{bash}
for book in *.epub; do echo "Converting $book"; ebook-convert "$book" "$(basename "$book" .epub).mobi"; done
```

### Unzip a file from terminal
Unzip is a default package

```{bash}
unzip file.zip -d destination_folder
```

### Merge multiple videos

```{bash}
mkvmerge -o outfile.mkv infile_01.mp4 \+ infile_02.mp4 \+ infile_03.mp4
```

### How to use youtube-dl
Download the audio from some playlist (for example JordanBPeterson Biblical Lectures):

```{bash}
youtube-dl --extract-audio --audio-format mp3 --audio-quality 0 -o "%(title)s.%(ext)s" https://www.youtube.com/playlist?list=PL22J3VaeABQD_IZs7y60I3lUrrFTzkpat
```

### How to sync BT-Keyboard through CLI
Connect a bluetooth keyboard to the raspberry pi through bluetoothctl

```
bluetoothctl 						# open bluetoothctl
[bluetoothctl]# power on 				# turn on BT
[bluetoothctl]# agent KeyboardOnly		# look only for keyboards
[bluetoothctl]# default-agent 		# make searching keyboards default
[bluetoothctl]# pairable on			# make the device pairable
[bluetoothctl]# scan on				# start sacanning
[bluetoothctl]# pair 01:02:03:4:05:06		# pair to that ip of the keyboard
[bluetoothctl]# trust 01:02:03:4:05:06		# trust the keyboard
[bluetoothctl]# connect 01:02:03:4:05:06	# connect to the kayboard
[bluetoothctl]# quit					# quit
```