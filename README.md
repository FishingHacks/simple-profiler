# Simple Profiler

Create a simple overview over your process.

## How to use

1. copy timer.ts (or timer.js when you are using javascript) into your apps source directory
2. Customize the `writeFileSync` call in timer.js or timer.ts, instructions in the `print` method in the `Timer` class
3. Use it by calling timer.start(name) before your thing you want to time (for example a function call) and the returned end method after it. Note: When you want to get usable results in the Caller entry, you have to call the start method in the function you want to time. For any non-function call, this field is useless, as it shows the function name that called the function that called the start() method.
<blockquote>

**Example**
>
```ts
/* index.ts */
import { timer } from './timer';

function a() {
    const end = timer.start('a()');

    // really complex code that might take a while

    end();
}

function b(c: string) {
    const end = timer.start(`b("${c}")`);
    
    a();
    a();
    // other code

    end();
}

function c() {
    const end = timer.start('c()');

    b('a');

    end();
}

c();

// NOTE: You have to call timer.print in a function, and cant provide it as an argument.
process.on('beforeExit', function () { timer.print(); });
```

Output:

```console
$ tsc
$ node index.js
c(): Xms (Start: XX:XX:XX AM|PM | End: XX:XX:XX AM|PM | CalledBy: Object.<anonymous> in index.js:26:1) 
b(a): Xms (Start: XX:XX:XX AM|PM | End: XX:XX:XX AM|PM | CalledBy: c in index.js:21:5) 
a(): Xms (Start: XX:XX:XX AM|PM | End: XX:XX:XX AM|PM | CalledBy: Object.<anonymous> in index.js:11:5) 
a(): Xms (Start: XX:XX:XX AM|PM | End: XX:XX:XX AM|PM | CalledBy: Object.<anonymous> in index.js:12:5) 
```
</blockquote>

4. Add `process.on('beforeExit', function () { timer.print(); })` at the end of your mainfile. This will cause it to print and dump the profile.


## Viewing the profile
1. Open the [index.html](./index.html) in your browser ([Deployed Version](https://fishinghacks.github.io/simple-profiler))
2. Copy the content of your profiler file (`profiling-<your apps name>-DD-MM-YYYY-HH-MM-SS.json`)
3. Paste it into the textarea
4. Explore your profile.