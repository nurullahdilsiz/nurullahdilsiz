<!DOCTYPE html>
<html>
<head>
  <style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "ROBOTO", sans-serif;
  font-size: 300%; /* Font size doubled */
}

.nav,
.slider {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100px;
  position: relative;
  background-color: #1e1f26;
  text-align: center;
  padding: 0 2em;
}

.nav h1,
.slider h1 {
  font-family: "Josefin Sans", sans-serif;
  font-size: 5vw;
  margin: 0;
  padding-bottom: 0.5rem;
  letter-spacing: 0.5rem;
  color: #0000FF; /* Color changed to bright blue */
  transition: all 0.3s ease;
  z-index: 3;
}

h1:hover {
  transform: translate3d(0, -10px, 22px);
  color: #0000FF; /* Color changed to bright blue */
}

.slider h2,
h3.span {
  font-size: 2vw;
  letter-spacing: 0.3rem;
  font-family: "ROBOTO", sans-serif;
  font-weight: 300;
  color: #faebd7;
  z-index: 4;
}

span:hover {
  color: #0000FF; /* Color changed to bright blue */
  font-weight: 500;
  font-size: 2.2vw;
}

a {
  text-decoration: none;
}

.nav-container {
  display: flex;
  flex-direction: row;
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 75px;
  box-shadow: 20px 20px 50px rgba(0, 0, 0, 0.5);
  background: #1e1f26;
  z-index: 10;
  transition: all 0.3s cubic-bezier(0.19, 1, 0.22, 1);
}

.nav-container--top-first {
  position: fixed;
  top: 75px;
  transition: all 0.3s cubic-bezier(0.19, 1, 0.22, 1);
}

.nav-container--top-second {
  position: fixed;
  top: 0;
}

.nav-tab {
  display: flex;
  justify-content: center;
  align-items: center;
  flex: 1;
  color: #03dac6;
  letter-spacing: 0.1rem;
  transition: all 0.5s ease;
  font-size: 2vw;
}

.nav-tab:hover {
  color: #1e1f26;
  background: #03dac6;
  transition: all 0.5s ease;
}

.nav-tab-slider {
  position: absolute;
  bottom: 0;
  width: 0;
  height: 2px;
  background: #03dac6;
  transition: left 0.3s ease;
}

.background {
  position: absolute;
  height: 100vh;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 0;
}

.loader span {
  color: #faebd7;
  text-shadow: 0 0 0 #faebd7;
  -webkit-animation: loading 1s ease-in-out infinite alternate;
}

@-webkit-keyframes loading {
  to {
    text-shadow: 20px 0 70px #0000FF; /* Color changed to bright blue */
    color: #0000FF; /* Color changed to bright blue */
  }
}

/* Other CSS code... */


  </style>

<script>
   /* Credit and Thanks:
Matrix - Particles.js;
SliderJS - Ettrics;
Design - Sara Mazal Web;
Fonts - Google Fonts
*/

window.onload = function () {
  Particles.init({
    selector: ".background"
  });
};
const particles = Particles.init({
  selector: ".background",
  color: ["#03dac6", "#ff0266", "#000000"],
  connectParticles: true,
  responsive: [
    {
      breakpoint: 768,
      options: {
        color: ["#faebd7", "#03dac6", "#ff0266"],
        maxParticles: 43,
        connectParticles: false
      }
    }
  ]
});

class NavigationPage {
  constructor() {
    this.currentId = null;
    this.currentTab = null;
    this.tabContainerHeight = 70;
    this.lastScroll = 0;
    let self = this;
    $(".nav-tab").click(function () {
      self.onTabClick(event, $(this));
    });
    $(window).scroll(() => {
      this.onScroll();
    });
    $(window).resize(() => {
      this.onResize();
    });
  }

  onTabClick(event, element) {
    event.preventDefault();
    let scrollTop =
      $(element.attr("href")).offset().top - this.tabContainerHeight + 1;
    $("html, body").animate({ scrollTop: scrollTop }, 600);
  }

  onScroll() {
    this.checkHeaderPosition();
    this.findCurrentTabSelector();
    this.lastScroll = $(window).scrollTop();
  }

  onResize() {
    if (this.currentId) {
      this.setSliderCss();
    }
  }

  checkHeaderPosition() {
    const headerHeight = 75;
    if ($(window).scrollTop() > headerHeight) {
      $(".nav-container").addClass("nav-container--scrolled");
    } else {
      $(".nav-container").removeClass("nav-container--scrolled");
    }
    let offset =
      $(".nav").offset().top +
      $(".nav").height() -
      this.tabContainerHeight -
      headerHeight;
    if (
      $(window).scrollTop() > this.lastScroll &&
      $(window).scrollTop() > offset
    ) {
      $(".nav-container").addClass("nav-container--move-up");
      $(".nav-container").removeClass("nav-container--top-first");
      $(".nav-container").addClass("nav-container--top-second");
    } else if (
      $(window).scrollTop() < this.lastScroll &&
      $(window).scrollTop() > offset
    ) {
      $(".nav-container").removeClass("nav-container--move-up");
      $(".nav-container").removeClass("nav-container--top-second");
      $(".nav-container-container").addClass("nav-container--top-first");
    } else {
      $(".nav-container").removeClass("nav-container--move-up");
      $(".nav-container").removeClass("nav-container--top-first");
      $(".nav-container").removeClass("nav-container--top-second");
    }
  }

  findCurrentTabSelector(element) {
    let newCurrentId;
    let newCurrentTab;
    let self = this;
    $(".nav-tab").each(function () {
      let id = $(this).attr("href");
      let offsetTop = $(id).offset().top - self.tabContainerHeight;
      let offsetBottom =
        $(id).offset().top + $(id).height() - self.tabContainerHeight;
      if (
        $(window).scrollTop() > offsetTop &&
        $(window).scrollTop() < offsetBottom
      ) {
        newCurrentId = id;
        newCurrentTab = $(this);
      }
    });
    if (this.currentId != newCurrentId || this.currentId === null) {
      this.currentId = newCurrentId;
      this.currentTab = newCurrentTab;
      this.setSliderCss();
    }
  }

  setSliderCss() {
    let width = 0;
    let left = 0;
    if (this.currentTab) {
      width = this.currentTab.css("width");
      left = this.currentTab.offset().left;
    }
    $(".nav-tab-slider").css("width", width);
    $(".nav-tab-slider").css("left", left);
  }
}

new NavigationPage();
/* Credit and Thanks:
Matrix - Particles.js;
SliderJS - Ettrics;
Design - Sara Mazal Web;
Fonts - Google Fonts
*/

  </script>
</head>
<body>
<section class="nav">
  <h3>
    <span class="loader">
      <span class="m">N</span>
      <span class="m">U</span>
      <span class="m">R</span>
      <span class="m">U</span>
      <span class="m">L</span>
      <span class="m">L</span>
      <span class="m">A</span>
      <span class="m">H</span>
      <span class="m">&nbsp;</span>
      <span class="m">D</span>
      <span class="m">I</span>
      <span class="m">L</span>
      <span class="m">S</span>
      <span class="m">I</span>
      <span class="m">Z</span>
    </span>
  </h3>
</section>

<canvas class="background"></canvas>

<h3 align="center">Languages</h3>
<p align="center">
  <a href="https://www.cprogramming.com/" target="_blank"> 
    <img src="https://img.shields.io/badge/C%20programming-A8B9CC.svg?style=for-the-badge&logo=c&logoColor=white"
      alt="c"/>
  </a>
  <a href="https://www.java.com" target="_blank"> 
    <img src="https://img.shields.io/badge/Java-007396.svg?style=for-the-badge&logo=java&logoColor=white" 
      alt="java"/> 
  </a>
  <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript" target="_blank"> 
    <img src="https://img.shields.io/badge/Javascript-F7DF1E.svg?style=for-the-badge&logo=javascript&logoColor=black"
      alt="javascript"/> 
  </a>
  <a href="https://www.w3.org/html/" target="_blank"> 
    <img src="https://img.shields.io/badge/html-E34F26.svg?style=for-the-badge&logo=html5&logoColor=white"
      alt="html5"/> 
  </a>
  <a href="https://www.w3schools.com/css/" target="_blank">
    <img src="https://img.shields.io/badge/css-1572B6.svg?style=for-the-badge&logo=css3&logoColor=white"
      alt="css3"/>
  </a>
  <a href="https://www.typescriptlang.org/" target="_blank"> 
    <img src="https://img.shields.io/badge/typescript-3178C6.svg?style=for-the-badge&logo=typescript&logoColor=white"
      alt="typescript"/>
  </a>
</p>

<h3 align="center">Frontend</h3>
<p align="center">
      <a href="https://getbootstrap.com" target="_blank">
    <img src="https://img.shields.io/badge/bootstrap-7952B3.svg?style=for-the-badge&logo=bootstrap&logoColor=white"
      alt="bootstrap"/>
  </a>
  <a href="https://babeljs.io/" target="_blank">
    <img src="https://img.shields.io/badge/babel-F9DC3E.svg?style=for-the-badge&logo=babel&logoColor=black" alt="babel"/> 
  </a>
  <a href="https://bulma.io/" target="_blank">
    <img src="https://img.shields.io/badge/bulma-00D1B2.svg?style=for-the-badge&logo=bulma&logoColor=white"
      alt="bulma"/>
  </a>
  <a href="https://www.gatsbyjs.com/" target="_blank">
    <img src="https://img.shields.io/badge/gatsbyjs-663399.svg?style=for-the-badge&logo=gatsby&logoColor=white" alt="gatsby" />
  </a>
  <a href="https://reactjs.org/" target="_blank"> 
    <img src="https://img.shields.io/badge/reactjs-61DAFB.svg?style=for-the-badge&logo=react&logoColor=black"
      alt="react"/> 
  </a>
  <a href="https://redux.js.org" target="_blank"> 
    <img src="https://img.shields.io/badge/redux-764ABC.svg?style=for-the-badge&logo=redux&logoColor=white" alt="redux"/> 
  </a> 
  <a href="https://jquery.com/" target="_blank">
    <img src="https://img.shields.io/badge/jquery-0769AD.svg?style=for-the-badge&logo=jquery&logoColor=white" alt="jquery"/> 
  </a>
  <a href="https://webpack.js.org" target="_blank">
    <img src="https://img.shields.io/badge/webpack-8DD6F9.svg?style=for-the-badge&logo=webpack&logoColor=black"
      alt="webpack"/>
  </a>
</p>

<h3 align="center">Backend</h3>
<p align="center">
  <a href="https://nodejs.org" target="_blank"> 
    <img src="https://img.shields.io/badge/node.js-339933.svg?style=for-the-badge&logo=nodedotjs&logoColor=white"
      alt="nodejs"/> 
  </a>
  <a href="https://expressjs.com" target="_blank">
    <img src="https://img.shields.io/badge/express-000000.svg?style=for-the-badge&logo=express&logoColor=white"
      alt="express" />
  <a href="https://hibernate.org/" target="_blank"> 
    <img src="https://img.shields.io/badge/hibernate-59666C.svg?style=for-the-badge&logo=hibernate&logoColor=white" alt="hibernate " /> 
  </a>
    <a href="https://spring.io/" target="_blank"> 
      <img src="https://img.shields.io/badge/spring%20IOC-6DB33F.svg?style=for-the-badge&logo=spring&logoColor=white" alt="spring" /> 
  </a>
  <a href="https://spring.io/" target="_blank"> 
    <img src="https://img.shields.io/badge/spring%20boot-6DB33F.svg?style=for-the-badge&logo=springboot&logoColor=white" alt="spring Boot" /> 
  </a>
  <a href="https://graphql.org" target="_blank">
    <img src="https://img.shields.io/badge/graphql-E10098.svg?style=for-the-badge&logo=graphql&logoColor=white" alt="graphql" />
  </a>
  <a href="https://kubernetes.io" target="_blank"> 
    <img src="https://img.shields.io/badge/kubernetes-326CE5.svg?style=for-the-badge&logo=kubernetes&logoColor=white" alt="kubernetes"/>
  </a>
  <a href="https://www.nginx.com" target="_blank"> 
    <img src="https://img.shields.io/badge/nginx-009639.svg?style=for-the-badge&logo=nginx&logoColor=white" 
      alt="nginx"/> 
  </a> 
</p>

<h3 align="center">Database</h3>
<p align="center">
  <a href="https://www.postgresql.org" target="_blank"> 
    <img src="https://img.shields.io/badge/postgreSQL-4169E1.svg?style=for-the-badge&logo=postgresql&logoColor=white"
      alt="postgresql"/> 
  </a>
  <a href="https://redis.io" target="_blank"> 
    <img src="https://img.shields.io/badge/redis-DC382D.svg?style=for-the-badge&logo=redis&logoColor=white"
      alt="redis"/>
  </a>
  <a href="https://www.sqlite.org/" target="_blank"> 
    <img src="https://img.shields.io/badge/sqlite-003B57.svg?style=for-the-badge&logo=sqlite&logoColor=white"
      alt="sqlite"/> 
  </a>
  <a href="https://www.mongodb.com/" target="_blank"> 
    <img src="https://img.shields.io/badge/mongodb-47A248.svg?style=for-the-badge&logo=mongodb&logoColor=white"
      alt="mongodb"/> 
  </a> 
</p>

<h3 align="center">Cloud & Hosting:</h3>
<p align="center">
  <a href="https://azure.microsoft.com/en-in/" target="_blank">
    <img  src="https://img.shields.io/badge/Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white" alt="azure"/> 
  </a>
  <a href="https://firebase.google.com/" target="_blank">
    <img src="https://img.shields.io/badge/firebase-FFCA28.svg?style=for-the-badge&logo=firebase&logoColor=black" alt="firebase"/>
  </a>
  <a href="https://netlify.com/" target="_blank">
    <img src="https://img.shields.io/badge/netlify-00C7B7.svg?style=for-the-badge&logo=netlify&logoColor=black" alt="firebase"/>
  </a>
  <a href="https://heroku.com" target="_blank"> 
    <img src="https://img.shields.io/badge/heroku-430098.svg?style=for-the-badge&logo=heroku&logoColor=white"
      alt="heroku"/> 
  </a> 
</p>

<h3 align="center">Testing</h3>
<p align="center"> 
  <a href="https://www.selenium.dev" target="_blank"> 
    <img src="https://img.shields.io/badge/selenium-43B02A.svg?style=for-the-badge&logo=selenium&logoColor=white"
      alt="selenium" /> 
  </a> 
  <a href="https://junit.org/junit5/" target="_blank"> 
    <img src="https://img.shields.io/badge/junit-25A162.svg?style=for-the-badge&logo=junit5&logoColor=white" alt="junit5" /> 
  </a> 
</p>

<h3 align="center">Version Control & CI/CD</h3>
<p align="center">
  <a href="https://git-scm.com/" target="_blank">
    <img src="https://img.shields.io/badge/git-F05032.svg?style=for-the-badge&logo=git&logoColor=white"
      alt="git"/>
  </a>
  <a href="https://github.com/ELanza-48" target="_blank">
    <img src="https://img.shields.io/badge/github-181717.svg?style=for-the-badge&logo=github&logoColor=white" alt="github" />
  </a>
  <a href="https://gitlab.com/Elanza-48" target="_blank">
    <img src="https://img.shields.io/badge/gitlab-181717.svg?style=for-the-badge&logo=gitlab&logoColor=white"
      alt="git"/>
  </a>
    <a href="https://www.docker.com/" target="_blank">
    <img src="https://img.shields.io/badge/docker-2496ED.svg?style=for-the-badge&logo=docker&logoColor=white"
      alt="docker"/>
  </a>
  <a href="https://www.jenkins.io" target="_blank"> 
    <img src="https://img.shields.io/badge/jenkins-D24939.svg?style=for-the-badge&logo=jenkins&logoColor=white" alt="jenkins"/> 
  </a>
</p>

<h3 align="center">Preferred IDEs  & Tools :</h3>
<p align="center"> 
  <a href="https://eclipse.org" target="_blank">
    <img src="https://img.shields.io/badge/eclipse-2C2255.svg?style=for-the-badge&logo=eclipse&logoColor=white" alt="eclipse IDE"/> 
  </a>
  <a href="https://code.visualstudio.com/" target="_blank">
    <img src="https://img.shields.io/badge/vscode-007ACC.svg?style=for-the-badge&logo=visualstudiocode&logoColor=white" alt="vsCode"/> 
  </a>
  <a href="https://www.jetbrains.com/" target="_blank">
    <img src="https://img.shields.io/badge/jetbrains%20IDE-000000.svg?style=for-the-badge&logo=jetbrains&logoColor=white" alt="jetbrains" />
  </a>
  <a href="https://postman.com" target="_blank"> 
    <img src="https://img.shields.io/badge/postman-FF6C37.svg?style=for-the-badge&logo=postman&logoColor=white" alt="postman"/>
  </a>
  <a href="https://www.virtualbox.org/" target="_blank">
    <img src="https://img.shields.io/badge/virtualbox-183A61.svg?style=for-the-badge&logo=virtualbox&logoColor=white"
      alt="virtualbox"/>
  </a>
  <a href="https://ubuntu.com/" target="_blank"> 
    <img src="https://img.shields.io/badge/ubuntu-E95420.svg?style=for-the-badge&logo=ubuntu&logoColor=white" alt="ubuntu"/>
  </a>
</p>

----

<h3 style="text-align: center; font-size: 2em; color: #000000; font-weight: normal; font-family: 'Bangers', cursive;">Let's Connect & Collaborate</h3>



<div style="margin-top:10px" align="center">
  <div>
    <a  href="https://dev.to/example" target="_blank">
      <img src="https://img.shields.io/badge/DEV.to-0A0A0A.svg?style=for-the-badge&logo=devdotto&logoColor=white" alt="example"/>
    </a>
    <a href="https://medium.com/@example" target="_blank">
      <img src="https://img.shields.io/badge/medium-000000.svg?style=for-the-badge&logo=medium&logoColor=white" alt="example"/>
    </a>
    <a href="https://codepen.io/@example" target="_blank">
      <img src="https://img.shields.io/badge/Codepen-000000.svg?style=for-the-badge&logo=codepen&logoColor=white" alt="example"/>
    </a>
  </div>
  <div>
    <a  href="https://linkedin.com/in/example" target="_blank">
      <img src="https://img.shields.io/badge/Linked%20In-0A66C2.svg?style=for-the-badge&logo=linkedin&logoColor=white" alt="example"/>
    </a>
    <a href="https://twitter.com/example" target="_blank">
      <img src="https://img.shields.io/badge/Twitter-1DA1F2.svg?style=for-the-badge&logo=twitter&logoColor=white" alt="example"/>
    </a>
    <a href="https://dribbble.com/example" target="_blank">
      <img src="https://img.shields.io/badge/Dribbble-EA4C89.svg?style=for-the-badge&logo=dribbble&logoColor=black" alt="example"/>
    </a>
  </div>
  <div>
    <a  href="https://www.codechef.com/users/example" target="_blank">
      <img src="https://img.shields.io/badge/Codechef-5B4638.svg?style=for-the-badge&logo=codechef&logoColor=white" alt="example"/>
    </a>
    <a href="https://www.hackerrank.com/example" target="_blank">
      <img src="https://img.shields.io/badge/Hackerrank-00EA64.svg?style=for-the-badge&logo=hackerrank&logoColor=black" alt="example"/>
    </a>
    <a href="https://www.leetcode.com/example" target="_blank">
      <img src="https://img.shields.io/badge/LeetCode-FFA116.svg?style=for-the-badge&logo=leetcode&logoColor=black" alt="example"/>
    </a>
  </div>
</div>

<h3 align="center">Reach me</h3>

<p align="center">
  <a  href="https://t.me/example" target="_blank">
    <img src="https://img.shields.io/badge/Telegram-26A5E4.svg?style=for-the-badge&logo=telegram&logoColor=white" alt="example"/>
  </a>
  <a href="mailto:example@outlook.com?subject=Feedback%20From%20Github&body=Hello," target="_blank">
    <img src="https://img.shields.io/badge/Outlook-0078D4.svg?style=for-the-badge&logo=microsoftoutlook&logoColor=white" alt="example"/>
  </a>
</p>




</body>
</html>
