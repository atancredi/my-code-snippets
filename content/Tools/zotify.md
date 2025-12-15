this is intended for personal offline use `:)`
# Download music from spotify with Zotify
```
zotify -m -l "zotify" https://open.spotify.com/playlist/2GZqlPOJbPJmGAVzoK0eow\?si\=9eac019997de4f5d --skip-previous --download-quality=very_high --print-progress --print-skips --print-warning  --download-real-time
```

### Reset credentials
```
rm Library/Application\ Support/Zotify/credentials.json
```


# Current installation on WSL (dec 25)
```
pipx install "git+https://github.com/DraftKinner/zotify"
```
```
zotify <PLAYLIST_URL> --download-real-time --playlist-library="" --skip-duplicates
```
