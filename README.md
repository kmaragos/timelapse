Timelapse photography workflow
==============================

Steps to build a video file from still shots
--------------------------------------------

1. rename all picture files in sequence by datetime (if necessary)
2. resize pictures to video resolution frame (eg 3008x2000 --> 1920x1277[1920x1080])
3. crop pictures to video resolution (eg 1920x1277 --> 1920x1080)
4. create list of picture files
5. build video from files


Methods, tools & commands to make it happen
-------------------------------------------

1. My camera fills up folders with up to 500 shots, in sequential numbering, reset for each folder.  
   In order to get them all in a row, I use a 0-based prefix for each folder.   
   So pictures IMG001.JPG, IMG002.JPG, etc from the first folder become 0IMG001.JPG, 0IMG002.JPG, etc.  
   YMMV :-)
   eg. `for file in *.JPG; do mv "${file}" "0${file}"; done`  
2. `mogrify -verbose -resize 1920x1277 *.JPG`
3. `mogrify -verbose -crop 1920x1080+0+0 *.JPG`
4. `ls *.JPG > jpegs.txt`
5. mencoder is the goto tool for this  
   `mencoder -nosound -ovc lavc -lavcopts vcodec=mpeg4:aspect=16/9:vbitrate=8000000 -vf scale=1920:1080 -o timelapse.avi -mf type=jpeg:fps=24 mf://@jpegs.txt`


