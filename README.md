# neomod.js

Experimental library to interact with the neomod server, and other neomod-related tooling.

### Installation

```
npm install neomod
```

### pp

Runs a WASM version of neomod's pp calculation code.

See [the neomod repository](https://github.com/kiwec/neomod/tree/master/tools/neomod.js) if you want to build it from source.

```js
import neomod from 'neomod';

console.log("pp version:", neomod.PP_ALGORITHM_VERSION);

// https://osu.ppy.sh/scores/1641562531 (636pp)
const res = await fetch('https://osu.ppy.sh/osu/3337690');
const map_bytes = await res.bytes();

const beatmap = await neomod.Beatmap(map_bytes);
try {
    // You must pass ALL these parameters or else you will get 0pp.
    const pp = beatmap.calculate({
        num300s: 3027,
        num100s: 115,
        num50s: 4,
        numMisses: 11,
        score: 300461780,
        comboMax: 3483,
    });
    console.log("pp:", pp);
} catch(err) {
    console.error("failed to calc pp:", err);
} finally {
    // You must delete the Beatmap object after usage to prevent memory leaks!
    beatmap.delete();
}
```
