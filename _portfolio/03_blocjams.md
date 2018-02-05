---
layout: post
title: Bloc Jams
thumbnail-path: "img/blocjams.png"
short-description: Bloc Jams is a music app that gives users an intuitive and easy to navigate interface.
---

{:.center}
![]({{ site.baseurl }}/img/blocjams.png)

## Explanation
Bloc Jams is a music player app that gives users a variety of options to navigate and manipulate a music library. I built two separate versions of Bloc Jams; one with a single page interface using jQuery, and another with multiple page interfaces displaying a music library and albums on different pages.

## Problem
Users want a variety of features in their music player, including the ability to see individual song titles and track their progress as they are playing, manipulate volume and play status from a main menu, and easily navigate between the music library and album of their choice.

## Solution
The jQuery version of Bloc Jams has a simple, single page interface which features song progress tracking, and play/pause/skip functionality through a main menu as well as by selecting individual songs. The React version is more sophisticated, giving users a library feature and tabs for site navigation in addition to playback features in the jQuery version.

## Results & Conclusions
jQuery is quick to learn and intuitive, and more importantly, provides a great option where browser compatibility issues are a concern. React router offers a speedy and intuitive setup to achieve the music player navigation needs of the client, however, the unidirectional data flow is a bit trickier to program. On the upside, it's great JavaScript practice and, while many argue React's a framework, when you're programming a React app, you don't lose the freedom of a library, you still feel in charge of your code, and less restricted compared to MVCs. I think React is a great solution for meeting many frontend needs.

Checkout my code for Bloc Jams on jQuery version [Github](https://github.com/cheneyshreve/bloc-jams-jquery) and React version [Github](https://github.com/cheneyshreve/bloc-jams-react)
