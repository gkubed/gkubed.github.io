---
layout: page
title: Random Linker
---

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
        #count { text-align: center; }
        #count strong { font-size: 1.3em; }
        #progress-outer {
            height: 6px; border-radius: 999px;
            background: rgba(255,255,255,.12); overflow: hidden;
        }
        #progress {
            height: 100%; width: 100%; background: #22b3eb; border-radius: 999px;
            animation: drain 3s linear forwards;
        }
        @keyframes drain { from { width: 100%; } to { width: 0%; } }
    </style>
</head>

Disclaimer: The internet is ephemeral and links die or change. Don't read too much into where I send you.


<p id="count"></p>
<div id="progress-outer"><div id="progress"></div></div>
<script>
    (function () {
    const LINKS_URL = "../../resources/su_links.txt";
    const DELAY_MS = 3000;

    let chosenUrl = null;

    function redirectTo(url) {
        window.location.replace(url);
    }

    fetch(LINKS_URL)
        .then(r => { if (!r.ok) throw new Error(r.status); return r.text(); })
        .then(text => {
            const links = text.split("\n")
                .map(l => l.trim())
                .filter(l => l && !l.startsWith("#"));

            document.getElementById("count").innerHTML =
                "Directing you to a random link out of the " + links.length + " in the list...";

            chosenUrl = links[Math.floor(Math.random() * links.length)];

            setTimeout(() => redirectTo(chosenUrl), DELAY_MS);
        })
        .catch(err => {
            console.error("Failed to load links:", err);
            document.getElementById("count").textContent = "Couldn’t load links.txt 😿";
        });
    })();
</script>