---
title: "MEMES PA GUILLE"
date: 2024-11-08
showToc: false
searchHidden: true
robotsNoIndex: true
---

<script>
    function seededRandom(seed) {
        return function() {
            seed = (seed * 9301 + 49297) % 233280;
            return seed / 233280;
        };
    }

    function deterministicShuffle(array) {
        const random = seededRandom(1337);
        const result = array.slice(); // Create a copy of the array

        for (let i = result.length - 1; i > 0; i--) {
            const j = Math.floor(random() * (i + 1));
            [result[i], result[j]] = [result[j], result[i]]; // Swap elements
        }

        return result;
    }
    window.onload = function() {
        let videos = [
            "https://img-9gag-fun.9cache.com/photo/ajP98rQ_460sv.mp4",
        ]
        let all_links = [
            "https://i.redd.it/0pgf0zfomozd1.jpeg",
            "https://i.imgur.com/Xxbng.jpeg",
            "https://i.imgur.com/0QzDC3P.jpeg",
            "https://pbs.twimg.com/media/FWHNOntUIAMAaui.jpg",
            "https://pbs.twimg.com/media/E0MayFjUcAUATR-?format=jpg",
        ]
        all_links = all_links.concat(videos)
        all_links = deterministicShuffle(all_links)
        // random one, same one for each hour of the day
        let epochour = Math.floor(Date.now() / 1000 / 3600) +5;
        let random_link = all_links[epochour % all_links.length];
        
        if (videos.includes(random_link)) {
            let video = document.createElement("video");
            video.src = random_link;
            video.autoplay = true;
            video.loop = true;
            video.controls = true;
            video.style.width = "100%";
            document.getElementById("meme").appendChild(video);
        } else {
            let img = document.createElement("img");
            img.src = random_link;
            img.style.width = "100%";
            document.getElementById("meme").appendChild(img);
        }
    }
</script>

<div id="meme" />