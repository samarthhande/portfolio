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
  color: #fff; /* Set the text color to white or a light color for better contrast */
  text-shadow: 0 0 10px #8a2be2, /* Purple glow */
               0 0 20px #8a2be2, /* Deeper purple glow */
               0 0 30px #4169e1, /* Blue glow */
               0 0 40px #4169e1; /* Deeper blue glow */
}

.bigbutton {
    background-color: #4169e1;
    color: white;
    padding: 15px 32px;
    text-align: center; /* Center the text */
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
    border: none;
    font-weight: 600;
    border-radius: 8px;
    transition: background-color 0.3s ease, transform 0.2s ease; /* Add transform transition */
}

.bigbutton:hover {
    background-color: #8a2be2;
    transform: scale(1.05); /* Slightly enlarge on hover */
}

</style>

<style>
.onboard-viewport {
  position: relative; /* needed for overlay */
  min-height: calc(100vh - 120px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 32px 16px;
  box-sizing: border-box;
  text-align: center;
  background-image: url('https://media3.giphy.com/media/v1.Y2lkPTZjMDliOTUydTF0YjdlYnFla3F4eHZzZnlvc2NrYWFuaDZ2amloNThsYWRwajYyaiZlcD12MV9naWZzX3NlYXJjaCZjdD1n/26tn33aiTi1jkl6H6/source.gif');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

/* Dark overlay */
.onboard-viewport::before {
  content: "";
  position: absolute;
  top: 0; left: 0; right: 0; bottom: 0;
  background-color: rgba(0, 0, 0, 0.5); /* 0.5 = 50% darker */
  z-index: 0; /* behind text */
}

.onboard-inner {
  position: relative;
  z-index: 1; /* text stays above overlay */
  max-width: 980px;
}
</style>


<div class="onboard-viewport">
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