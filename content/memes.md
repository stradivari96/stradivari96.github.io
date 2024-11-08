---
title: "MEMES PA GUILLE"
date: 2024-11-08
showToc: false
searchHidden: true
robotsNoIndex: true
---

<script>
    window.onload = function() {
        let images = document.querySelectorAll("img");
        images = Array.from(images)
        let all_links = [
            "https://i.imgur.com/0QzDC3P.jpeg",
            "https://pbs.twimg.com/media/FWHNOntUIAMAaui.jpg",
        ]
        // random one, same one for each hour of the day
        let current_utc_hour = new Date().getUTCHours();
        let random_link = all_links[current_utc_hour % all_links.length];

        images.forEach(a => a.src = random_link);
    }
</script>

![meme]()