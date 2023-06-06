## Some basic steps to quickly look at steg files (general files/pdf/images/music)

### strings
Potential quick win with using strings on the file, if it's too huge, search for flag with ```strings FILE | grep -i flag```

### exiftool
```exiftool FILE``` will check the metadata of the file, some potential quick wins there too

### binwalk
```binwalk -e FILE``` will extract everything hidden in the file, sometimes a quick win when a ```.zip``` file is discovered<br>
If it's password protected, just bruteforce it with our friend john :)<

### stegsolve
Run the tool and load the image, use the GUI

### SonicVisualiser
This is for sound files. Load the file in Sonic Visualiser, and then go to ```Pane -> Add Spectrogram -> Channel 1```<br>
You can then play with different waveforms and add other panes to look into the patterns
