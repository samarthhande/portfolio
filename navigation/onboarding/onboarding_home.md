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
    border-radius: 8px; /* Rounded corners */
    transition: background-color 0.3s ease; /* Smooth transition for background color on hover */
}

.bigbutton:hover {
    background-color: #8a2be2; /* Darker green on hover */

}

</style>

<style>
  /* Full height flex wrapper to center content vertically and horizontally */
  .onboard-viewport{
    min-height: calc(100vh - 120px); /* leave some space for header/footer if present */
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 32px 16px;
    box-sizing: border-box;
    text-align: center;
  }
  .onboard-inner{ max-width: 980px; }
</style>

<div class="onboard-viewport">
  <div class="onboard-inner">
    <h4>The Tinkerers</h4>
    <hr>
    <h1 class="glowing-text">CSSE Trimester 1 Onboarding Adventure</h1>
    <br>
    <strong><button type="button" class="bigbutton">LET'S DIVE IN</button></strong>
  </div>
</div>


<center>  

![Computer image](https://www.crio.do/blog/content/images/size/w600/2020/09/Sep_01.png)
</center>