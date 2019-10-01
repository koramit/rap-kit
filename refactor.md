# original

```js
    window.addEventListener("keydown", function(e) {
        const key = document.querySelector(`.key[data-keyCode="${e.keyCode}"`);
        const audio = document.querySelector(`audio[data-keyCode="${e.keyCode}"`);
        if (!key) return;
        if (key.classList.contains("loop")) {
            if (!audio.paused) {
                audio.pause();
                key.classList.remove("looping");
                return;
            }
            key.classList.add("looping");
            audio.volume = .25;
        } else {
            key.classList.add("playing");    
        }
        audio.currentTime = 0;
        audio.play();
    });

    const keys = document.querySelectorAll(".key");
    keys.forEach(function (key) {
        key.addEventListener("transitionend", function (e) {
            if (e.propertyName != "transform" || this.classList.contains("loop")) return;
            this.classList.remove("playing");
        });
    });
```


# refactor

```js
function playSound(e) {
    const key = document.querySelector(`.key[data-keyCode="${e.keyCode}"`);
    const audio = document.querySelector(`audio[data-keyCode="${e.keyCode}"`);
    if (!key) return;
    const applyClass = key.classList.contains("loop") ? "looping" : "playing";
    if (applyClass == "looping") {
        if (!audio.paused) {
            audio.pause();
            key.classList.remove("looping");
            return;
        }
        audio.volume = .25;
    }        
    key.classList.add(applyClass);
    audio.currentTime = 0;
    audio.play();
}

function removeStyle(e) {
    if (e.propertyName != "transform" || this.classList.contains("loop")) return;
    this.classList.remove("playing");
}

window.addEventListener("keydown", playSound);
const keys = document.querySelectorAll(".key");
keys.forEach( key => key.addEventListener("transitionend", removeStyle));
```
