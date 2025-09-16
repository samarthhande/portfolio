---
layout: base
title: Onboarding Adventure
permalink: /onboarding/home
---

<style>
@keyframes fadeInUp {
  0% {
    transform: translateY(100%);
    opacity: 0;
  }
  100% {
    transform: translateY(0%);
    opacity: 1;
  }
}

.fadeInUp-animation {
  animation: 1.5s fadeInUp;
}

.glowing-text {
  color: #fff;
  text-shadow: 0 0 10px #8a2be2,
               0 0 20px #8a2be2,
               0 0 30px #4169e1,
               0 0 40px #4169e1;
}

.bigbutton {
    background-color: #4169e1;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
    border: none;
    font-weight: 600;
    border-radius: 8px;
    transition: background-color 0.3s ease, transform 0.2s ease;
}

.bigbutton:hover {
    background-color: #8a2be2;
    transform: scale(1.05);
}

/* Fullscreen viewport */
.onboard-viewport {
  position: relative;
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  overflow: hidden;
}

/* GIF as an img for perfect centering */
.background-gif {
  position: absolute;
  inset: 0; /* fill the viewport */
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center center; /* ensure centering */
  z-index: -1; /* sit behind the overlay and content */
}

.onboard-inner {
  position: relative;
  z-index: 1;
  max-width: 980px;
}
</style>

<center>
<div class="onboard-viewport">
  <!-- Background GIF -->
  <img src="https://media3.giphy.com/media/v1.Y2lkPTZjMDliOTUydTF0YjdlYnFla3F4eHZzZnlvc2NrYWFuaDZ2amloNThsYWRwajYyaiZlcD12MV9naWZzX3NlYXJjaCZjdD1n/26tn33aiTi1jkl6H6/source.gif"
       alt="Animated Background"
       class="background-gif">

  <div class="onboard-inner">
    <h4>The Tinkerers</h4>
    <hr>
    <h2 class="fadeInUp-animation glowing-text"> CSSE Trimester 1 Onboarding Adventure </h2>
    <br>
    <button 
        type="button" 
        class="bigbutton" 
        onclick="window.location.href='{{ site.baseurl }}/onboarding/navigation'">
        LET'S DIVE IN 🔥
    </button>  
  </div>
</div>
</center>