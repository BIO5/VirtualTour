# Inline documentation of the student experience VR scene html files
This readme includes inline information about how the scenes are specified. Please refer to the `art_exhibit` directory readme for additional information as well.



```

<html>

<head>
    <script src="https://aframe.io/releases/1.0.4/aframe.min.js"></script>
    <style>
    // iframe popout styling, 
        iframe {
                position: absolute;
                top: 0px;
                left: 0px;
                width: 50%;
                height: 100%;
                border: black solid 2px;
            }
    //specify placement of the audio icon in the bottom right           
        #audio {
            position: absolute;
            bottom: 0px;
            right: 0px;
        }
    // make the image 40px, and provide a curved border around it
        #audio img {
            height: 40px;
            border-radius:10px;
        }

        #audioOn {
            border: #1e94aa solid 2px;
        }
        #audioOff {
            border:#e37081 solid 2px
        }
    </style>
    <script>
    // create an audio object that supports the toggling behavior of "click on to hear, click on to stop hearing" audio
         let audioOb = () => {
            let ob = {}
            ob.on = false
            ob.create = () => {
                ob.audio = document.createElement("audio")
                ob.audio.src = "Logan_Beers.mp3"
                ob.toggle()
            }

            ob.toggle = () => {
                ob.img = document.querySelector("#audio img")
                ob.on = !ob.on
                if (ob.on) {
                    // play audio
                    ob.audio.play().catch(error=> {
                        




alert(`Please enable browser autoplay to hear audio on this site.

Directions: Click the icon in your address bar to the left of the URL.

On mobile go to settings > advanced > media > allow autoplay.

Or manually start the audio within each screen by clicking on the audio element.`)
                    })
                    // change style of icon
                    ob.img.id ="audioOn"
                } else {
                    ob.audio.pause()
                    ob.img.id="audioOff"

                }


            }
            return ob
        }
        let loganAudio
        window.onload = () => {
            loganAudio = audioOb()
            loganAudio.create()
        }
    </script>
</head>

<body>
    <script>
        // create function that loads an iframe of the popout page in this page
        let createTextPopout = (filehtml) => {
                // test with just loading the art_popout_bootstrap.html
                let iframeElement = document.createElement("iframe")
                iframeElement.src = filehtml
                document.body.append(iframeElement)
                // bind the close button
                // recursive timeout till selection passes
                let closeInt = setInterval(() => {
                        let btn = iframeElement.contentDocument.querySelector("button")
                if (btn) {

                    // add image to close it
                    let xPopoutIcon = document.createElement("img")
                    xPopoutIcon.src= "../ua-brand-icons/ua-brand-icons-image-files/PNG/x.png"
                    iframeElement.contentDocument.body.append(xPopoutIcon)
                    xPopoutIcon.style.position="absolute"
                    xPopoutIcon.style.top = "0px"
                    xPopoutIcon.style.right = "0px"
                    xPopoutIcon.addEventListener("click",()=> {
                        document.querySelector("iframe").remove()
                    })

                                btn.addEventListener("click", () => {
                                        document.querySelector("iframe").remove()
                                    })
                                clearInterval(closeInt)
                            }
                    }, 1000)
                iframeElement.contentDocument.querySelector("button").addEventListener("click", () => {
                        document.querySelector("iframe").remove()
                    })
            }
    </script>
    <script>
        AFRAME.registerComponent('mlisten', {
            schema: {
            popoutInnerHTML: { type: 'string' }},
            init: function () {
                    let el = this.el
                    let popout_html_file = this.data.popoutInnerHTML
                    this.el.addEventListener('mouseenter', function (e) {
                            // play with emissivity
                            el.setAttribute("material", "color", "#5ef7ff")
                            //el.setAttribute("material","emissiveIntensity","1")
                        })
                    this.el.addEventListener("mouseleave", function () {
                            //el.setAttribute("material", "emissiveIntensity",'.2')
                            el.setAttribute("material", "color", "white")
                        })
                    // this is if the html file was what was put in
                    //
                    this.el.addEventListener("click", function () {
                            console.log("cilcked on info");

                            // load whatever popout is needed for this part
                            if (popout_html_file !== "movement") {
                                    createTextPopout(popout_html_file)
                                } else {
                                    //move icon trannsfer to adjacent scene
                                    //remove current scene and call new basic scene create
                                    document.querySelector("a-scene").remove()
                                    let basicscene = basicScene("art-one")
                                    basicscene.create()
                                }

                        })
                }
    })
    </script>

    <div id="loaderHolder">
        <div class="loader">

        </div>
    </div>
    <style>
        #loaderHolder {
            height: 100%;
            width: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .loader {
            border: 16px solid #f3f3f3;
            /* Light grey */
            border-top: 16px solid #3498db;
            /* Blue */
            border-radius: 50%;
            width: 120px;
            height: 120px;
            animation: spin 2s linear infinite;
        }

        @keyframes spin {
            0% {
                transform: rotate(0deg);
            }

            100% {
                transform: rotate(360deg);
            }
    </style>
    <a-scene loading-screen="dotsColor: black; backgroundColor: white" cursor="rayOrigin: mouse">

    <a-assets>
    <img id="360-image" src="360_scene.jpg">
    </a-assets>
        <a-entity  id="Logan_Beers_cam" rotation="-4.470789675405814 90.07469497251672 0.06130648407899809" position="0 0 0">
        <a-camera look-controls></a-camera>
        </a-entity>

        <a-circle animation="property:material.emissive;to:#D2A5A5;dir:alternate;easing:linear;dur:1000;loop:true" material="emissive: #000000;emissiveIntensity: .3;side: double;src: info.png"
            mlisten="popoutInnerHTML:popout_0.html" id="Logan_Beers_circle_1" rotation="0 117.52510293723446 0"
            position="-19.32846 -3.27879 4.17347"></a-circle>
        <a-circle animation="property:material.emissive;to:#D2A5A5;dir:alternate;easing:linear;dur:1000;loop:true" material="emissive: #000000;emissiveIntensity: .3;side: double;src: info.png"
            mlisten="popoutInnerHTML:popout_1.html" id="Logan_Beers_circle_2" rotation="0 165.11726923198117 0"
            position="-0.68031 -9.03896 12.26563"></a-circle>
        <a-sky src="#360-image"></a-sky>
    </a-scene>

    <div id="exit" >

        <img  style="position:absolute;top:0px;right:0px;border:black solid;border-radius:10px;background:white"  onclick="window.close()" src="../ua-brand-icons/ua-brand-icons-image-files/PNG/x.png" alt="">
        </div>
        
        <div id="audio">
        <img id="audioOff" src="../audio_icon.svg" alt="" onclick="loganAudio.toggle()"></div>
</body>

</html>
```