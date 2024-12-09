

## convert all .mp3 in a folder to .wav
```bash
mkdir WAV
for f in *.mp3; do ffmpeg -i "${f}" -vn -c:a pcm_s16le  -ar 44100  "WAV/${f%.*}.wav" ; done 
```

&nbsp;
## Convert mp4 to WAV
```bash
ffmpeg -i <infile> -ac 2 -f wav -ar 16000 <outfile>

# wav 16kHz
ffmpeg -i <infile> -acodec pcm_s16le -ac 2 -ar 16000 <outfile>
```