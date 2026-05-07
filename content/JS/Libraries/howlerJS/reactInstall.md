# howlerJS react-howler library

https://github.com/thangngoc89/react-howler

### Install library
```bash
npm install --save react-howler 
npm i --save-dev @types/react-howler
```

### Example
```ts
import { useState } from 'react'
import ReactHowler from 'react-howler'


export default function LooperPlayer() {

    const [isPlaying, setIsPlaying] = useState(false);
    const togglePlaying = setIsPlaying(!isPlaying);

    return (
        <>
            <ReactHowler
                src='loops/loop.mp3'
                playing={isPlaying}
            />
            <button onClick={() => setIsPlaying(!isPlaying)}>{isPlaying ? "pause" : "play"}</button>
        </>
    )

}
```
