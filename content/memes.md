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
        let images = document.querySelectorAll("img");
        images = Array.from(images)
        let all_links = [
            "https://i.imgur.com/Xxbng.jpeg",
            "https://i.imgur.com/0QzDC3P.jpeg",
            "https://pbs.twimg.com/media/FWHNOntUIAMAaui.jpg",
            "https://pbs.twimg.com/media/E0MayFjUcAUATR-?format=jpg",
        ]
        all_links = deterministicShuffle(all_links)
        // random one, same one for each hour of the day
        let current_utc_hour = new Date().getUTCHours();
        let random_link = all_links[current_utc_hour % all_links.length];

        images.forEach(a => a.src = random_link);
    }
</script>

![meme]()