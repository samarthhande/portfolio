---

toc: False
layout: post
title: Word Game Writeup
description: A writeup of our changes to the word game
categories: ['Web Development', 'Hacks']
permalink: /wordgame/writeup
breadcrumb: true
---

### Our changes

We have made multiple quality-of-life improvements and mechanic redesigns for the Word Game. Below is a list of all the modifications we made.

- Changed the font of the text to Times New Roman
  - We updated a line in `drawText()` to `wordCtx.font = '24px "Times New Roman", Times, serif';`

- Added a gradient progress bar to display your progress
  - We first wrote CSS for the bar itself, the fill of the bar, and the text for it
  - Then we inserted the below line:
    <div class="progress-bar" aria-hidden="true"><div class="progress-fill"></div><div class="progress-text">0%</div></div>

- Game now handles accuracy and WPM differently:
  - The way it worked previously is that you had to have the word typed correctly at the end and fix any mistakes you made. The accuracy was just the number of mistakes you made.
  - Now, you can finish with typos. Your accuracy is the FINAL result, and WPM is computed at the end to avoid strange 1,000 WPM values at the beginning.
  - This is a more intuitive system that matches most typing tools online, like MonkeyType.

- Missed letters show up as red but as the same letter to prevent jumbled, unreadable, overlapping text
  - The letter remains the same: only the color changes
  - Color changing done by: `const color = typedChar === promptChar ? 'green' : 'red';`

- Cleaned up the "string length selection" menu
  - Added custom CSS code to the menu to stylize it

- Added a cursor
  - It uses the current time to determine if it should be visible or not, creating a blinking effect

### GitHub Issues
- #11: [Add a progress bar](https://github.com/CompSciTeam/student/issues/11)
- #24: [Stylize options menu](https://github.com/CompSciTeam/student/issues/24)
- #22: [Add more possible strings](https://github.com/CompSciTeam/student/issues/22)
- #23: [Typos make text unreadable](https://github.com/CompSciTeam/student/issues/23)
- #12: [Experiment with different fonts, colors, and styles](https://github.com/CompSciTeam/student/issues/12)

## Play our word game at [compsciteam.github.io/student/wordgame](https://compsciteam.github.io/student/wordgame)!

Made by Subteam 1: Anish Gupta, James Blazek, and Vihaan Budharaja
