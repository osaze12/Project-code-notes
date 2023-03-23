# Project-code-notes
These is where i document things i have learnt, or found on the internet that are not easily findable
https://github.com/30-seconds/30-seconds-of-code

# how to undo merge
```html
https://www.freecodecamp.org/news/git-undo-merge-how-to-revert-the-last-merge-commit-in-git/
```

# cover letter
```html
Hello,
I am applying for this job because I met some criteria, and our mission or goals align.

my goal is to build products that solve problems for organizations and help them grow their revenues quickly.

I've built a product, called In-cert, which helps people generate 100s of digital certificates in seconds, here's a link, https://in-cert.netlify.app/

I've also built a react library, which helps developers create a fully functional form with just 5 lines of code, here's a link, https://www.npmjs.com/package/@osaze12/bootstrapped-form
```

# if you're having issues with react showing error when it tries building the app on AWS/other platform
# add --openssl-legacy-provider to your package.json file
```javascript
  "start": "react-scripts start --openssl-legacy-provider",
```
# Clone from a specific branch on github
```javascipt
git clone --branch <branchname> <remote-repo-url>
or git clone -b <branchname> <remote-repo-url>
```

# Good stripe tutorial
```
https://linguinecode.com/post/integrate-stripe-payment-form-with-react
```
# Make ids from just alphabeth
```javascript
function makeid(length) {
    let result = '';
    const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
    const charactersLength = characters.length;
    let counter = 0;
    while (counter < length) {
      result += characters.charAt(Math.floor(Math.random() * charactersLength));
      counter += 1;
    }
    return result;
}
//makeid(5)
```

# Format bytes
```javascript
function formatBytes(bytes, decimals = 2) {
    if (!+bytes) return '0 Bytes'

    const k = 1024
    const dm = decimals < 0 ? 0 : decimals
    const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB']

    const i = Math.floor(Math.log(bytes) / Math.log(k))

    return `${parseFloat((bytes / Math.pow(k, i)).toFixed(dm))} ${sizes[i]}`
}

// formatBytes(bytes, decimals)

formatBytes(1024)       // 1 KB
formatBytes('1024')     // 1 KB
formatBytes(1234)       // 1.21 KB
formatBytes(1234, 3)    // 1.205 KB
formatBytes(0)          // 0 Bytes
formatBytes('0')        // 0 Bytes
```
# remove array of array
```javascript
var arrays = [
  ["$6"],
  ["$12"],
  ["$25"],
  ["$25"],
  ["$18"],
  ["$22"],
  ["$10"]
];
var merged = [].concat.apply([], arrays);

console.log(merged);

```

# Remove duplicate from an array
```javascript
// usage example:
var myArray = ['a', 1, 'a', 2, '1'];
var unique = myArray.filter((v, i, a) => a.indexOf(v) === i);

console.log(unique); // unique is ['a', 1, 2, '1']
```
# Plot graph with custom numbers
```javascript
export const MyGraph = ({ data }) => {
  const { labels, datasets } = data || {};

  useEffect(() => {
    const data = {
      labels,
      datasets,
    };

    //graph configuration
    const config = {
      type: "bar",
      data: data,
      options: {
        scales: {
          y: {
            min: 0,
            max: 10000,
            ticks: {
              callback: function (value, index, ticks) {
                function convert(value) {
                  if (value >= 1000000) {
                    let divided = value / 1000000;
                    value = divided.toFixed(1) + "M";
                  } else if (value >= 1000) {
                    let divided = value / 1000;
                    value = divided?.toFixed(1) + "K";
                  }
                  return value;
                }

                if (String(value).length < 4) {
                  return value;
                } else {
                  return convert(value);
                }
              },
              beginAtZero: true,
              steps: 0,
              stepSize: 100,
              //   max: 8000,
            },
          },
        },
      },
    };

    var graph = new Chart(
      document.getElementById("graph").getContext("2d"),
      config
    );

    return () => {
      graph.destroy();
    };
  }, []);

  return (
    <Box p={["1", "4"]} borderRadius={5} h={[600]} w={["100%"]}>
      <canvas height="450px" id="graph"></canvas>
    </Box>
  );
};
```
## BarChart.jsx
```javascript
const BarChart = ({ doctorEarningByService }) => {

 const labels = [
  "Jan",
  "Feb",
  "Mar",
  "Apr",
  "May",
  "Jun",
  "Jul",
  "Aug",
  "Sept",
  "Oct",
  "Nov",
  "Dec",
];

  const arrayData = doctorEarningByService?.wellnessCheckupArray?.map(
    (item) => {
      const keys = Object.keys(item);
      return item[keys[1]];
    }
  );

  const physicalConsultationArrayData =
    doctorEarningByService?.physicalConsultation?.map((item) => {
      const keys = Object.keys(item);
      return item[keys[1]];
    });

  const virtualConsultationArrayData =
    doctorEarningByService?.virtualConsultation?.map((item) => {
      const keys = Object.keys(item);

      return item[keys[1]];
    });

  const privateChatArrayData = doctorEarningByService?.privateChat?.map(
    (item) => {
      const keys = Object.keys(item);
      return item[keys[1]];
    }
  );
  console.log(privateChatArrayData, doctorEarningByService, "PPPP");
  const data = {
    labels,
    datasets: [
      {
        label: "Virtual Consultations",
        data: virtualConsultationArrayData,
        backgroundColor: "rgba(25, 165, 176)",
        borderWidth: 2,
        borderRadius: 9,
        borderSkipped: false,
        barThickness: 10,
      },
      {
        label: "Physical Consultations",
        data: physicalConsultationArrayData,
        backgroundColor: "rgba(234, 111, 6, 1)",
        borderWidth: 2,
        borderRadius: 9,
        borderSkipped: false,
        barThickness: 10,
      },
      {
        label: "Wellness CheckUp",
        data: arrayData,
        backgroundColor: "rgba(154,187,46)",
        borderWidth: 2,
        borderRadius: 9,
        borderSkipped: false,
        barThickness: 10,
      },
      {
        label: "Private Chats",
        data: privateChatArrayData,
        backgroundColor: "rgba(111, 25, 172, 1)",
        borderWidth: 2,
        borderRadius: 9,
        borderSkipped: false,
        barThickness: 10,
      },
    ],
  };

  return (
    <Box bg="#fff" p={["1", "4"]} borderRadius={5} h={[600]} w={["100%"]}>
      <MyGraph data={data} />
    </Box>
  );
};
```

# Respond when a user leaves the screen
```javascript
  useEffect(() => {
    document.addEventListener("visibilitychange", () => {
      setEnableChatTone(true);
    });
  }, []);
```
# Take a live photo from webcam and save photo file

```javascript
 useEffect(() => {
    if (!props?.openModal?.open) return;

    setTimeout(async () => {
      await navigator.mediaDevices
        .getUserMedia({
          video: true,
          audio: false,
        })
        .then((stream) => {
          videoRef.current.srcObject = stream;
          setLoading(false);

          document
            .getElementById("take-photo")
            .addEventListener("click", async () => {
              let canvas = document.querySelector("#canvas");

              canvas
                .getContext("2d")
                .drawImage(videoRef.current, 0, 0, canvas.width, canvas.height);
              canvas.toBlob(async (blob) => {
                let file = new File([blob], "user_image.jpg", {
                  type: "image/jpeg",
                });

                // close the video and send image file

                await stream?.getTracks().forEach((track) => {
                  track.stop();
                });
                videoRef.current.srcObject = null;

                props?.onFinish(file);
              }, "image/jpeg");
            });

          document
            .getElementById("cancel")
            .addEventListener("click", async () => {
              await stream?.getTracks().forEach((track) => {
                track.stop();
              });
              videoRef.current.srcObject = null;
              props.close();
            });
        });
    }, 3000);
  }, []);

  return (
    <Stack
      position={"relative"}
      justifyContent="center"
      alignItems={"center"}
      py="20px"
      spacing={"20px"}
    >
      {loading && <VideoLoadingAnimation />}
      <video
        ref={videoRef}
        id="video"
        width="520"
        height="340"
        autoPlay
      ></video>
      <Stack direction={"row"} mt="20px">
        <Button
          disabled={loading}
          id="take-photo"
          p="20px"
          size={"xs"}
          bg="#07900C"
          color="#fff"
          _hover={{ background: "#07900C" }}
        >
          Take Photo
        </Button>
        <Button
          disabled={loading}
          id={"cancel"}
          p="20px"
          size={"xs"}
          border="1px solid #80808030"
          bg="transparent"
          _hover={{ background: "transparent" }}
        >
          Cancel
        </Button>
      </Stack>
      <canvas
        id="canvas"
        width="320"
        height="240"
        style={{ display: "none" }}
      ></canvas>
    </Stack>
```

# Record a live video with a stop timer & save video file 
```javascript
 useEffect(() => {
    if (!props?.openModal?.open) return;

    setTimeout(async () => {
      navigator.mediaDevices
        .getUserMedia({
          video: true,
          audio: true,
        })
        .then((stream) => {
          videoRef.current.srcObject = stream;
          setLoading(false);

          document
            .getElementById("start-video")
            .addEventListener("click", async () => {
              setDisableBtn(true);
              // set MIME type of recording as video/webm
              media_recorder = await new MediaRecorder(stream, {
                mimeType: "video/webm",
              });
              console.log(media_recorder, "here");
              media_recorder.start();
              setCountStarted(true);

              // event : new recorded video blob available
              media_recorder.addEventListener("dataavailable", function (e) {
                blobs_recorded.push(e.data);
              });

              // event : recording stopped & all blobs sent
              media_recorder.addEventListener("stop", async function () {
                // create local object URL from the recorded video blobs
                let video_local = URL.createObjectURL(
                  new Blob(blobs_recorded, { type: "video/webm" })
                );

                const blob = new Blob(blobs_recorded, { type: "video/webm" });
                const myFile = blobToFile(blob, "my-video.webm");
                console.log("THE END", myFile);
                setCountStarted(false);
                props?.onFinish(myFile);
              });
            });

          document
            .getElementById("cancel")
            .addEventListener("click", async () => {
              await stream.getTracks().forEach((track) => {
                track.stop();
              });

              videoRef.current.srcObject = null;
              props.close();
            });
        });
    }, 3000);
  }, [props?.openModal?.open]);
  console.log(videoCountDown);

  useEffect(() => {
    let interval;

    if (!countStarted) return;

    if (parseInt(videoCountDown) < 1) {
      clearInterval(interval);
      media_recorder.stop();
      cancelRef.current.click();

      return;
    }
    interval = setInterval(() => {
      setVideoCountDown(videoCountDown - 1);
    }, 2000);

    return () => clearInterval(interval);
  }, [countStarted, videoCountDown]);

  return (
    <Stack
      position={"relative"}
      justifyContent="center"
      alignItems={"center"}
      py="20px"
      spacing={"20px"}
    >
      {loading && <VideoLoadingAnimation />}

      <video
        ref={videoRef}
        id="video"
        width="520"
        height="340"
        autoPlay
      ></video>
      <Text fontSize={".76em"} textAlign="center">
        Lorem ipsum dolor sit, amet consectetur adipisicing elit. Odio
        reiciendis distinctio molestias nisi tempora perspiciatis, quam
        consequuntur ut ducimus
      </Text>
      <Stack
        direction={"row"}
        // mt="30px !important"
        alignItems={"center"}
        spacing="20px"
      >
        <Button
          id="start-video"
          disabled={loading || disableBtn}
          p="20px"
          size={"xs"}
          bg="#00157E"
          _hover={{ background: "#00157E" }}
          color="#fff"
          // onClick={() => handleVideo()}
        >
          Start Video
        </Button>

        <Button
          ref={cancelRef}
          disabled={loading}
          id="cancel"
          p="20px"
          size={"xs"}
          border="1px solid #80808030"
          bg="transparent"
          _hover={{ background: "transparent" }}

          // onClick={() => {
          //   // closeVideo().then(() => props.close());
          // }}
        >
          Cancel
        </Button>
        <CircularProgress value={videoCountDown * 10} color="green.400">
          <CircularProgressLabel>
            {videoCountDown} <span style={{ fontSize: ".8em" }}>sec</span>
          </CircularProgressLabel>
        </CircularProgress>
      </Stack>
    </Stack>
```

# How to add svg image directly to css
```javascript

link to icons https://fontawesomeicons.com/svg/icons/circle-chevron-left

  let w = ` <svg
      xmlns="http://www.w3.org/2000/svg"
      width="16"
      height="16"
      fill="currentColor"
      class="bi bi-chevron-left"
      viewBox="0 0 16 16"
    >
      <path
        fill-rule="evenodd"
        d="M11.354 1.646a.5.5 0 0 1 0 .708L5.707 8l5.647 5.646a.5.5 0 0 1-.708.708l-6-6a.5.5 0 0 1 0-.708l6-6a.5.5 0 0 1 .708 0z"
      />
    </svg>`;

  let p = window.btoa(w);
  
 let style = `
  background: url(data:image/svg+xml;base64,${p});
 `
```
# Dots animations
```
https://codepen.io/nzbin/pen/GGrXbp
```

# Center an absolute item
```
https://stackoverflow.com/questions/7720730/how-to-align-absolutely-positioned-element-to-center#:~:text=If%20you%20set%20both%20left,center%20an%20absolutely%20positioned%20element.
```

# Convert Blob to file
```javascript
function blobToFile(theBlob, fileName){
    //A Blob() is almost a File() - it's just missing the two properties below which we will add
    theBlob.lastModifiedDate = new Date();
    theBlob.name = fileName;
    return theBlob;
}

var myBlob = new Blob();

//do stuff here to give the blob some data...

var myFile = blobToFile(myBlob, "my-image.png");
```

# take live photo
Link to tutorial (video) == https://usefulangle.com/post/354/javascript-record-video-from-camera
Link to tutorial (photo) == https://usefulangle.com/post/352/javascript-capture-image-from-camera

```javascript
 let stream;
  const videoRef = useRef();

  useEffect(() => {
    if (!props.open) return;

    setTimeout(async () => {
      stream = await navigator.mediaDevices.getUserMedia({
        video: true,
        audio: false,
      });
      videoRef.current.srcObject = stream;
    }, 3000);
  }, [props?.open]);

  const handleImage = () => {
    let canvas = document.querySelector("#canvas")

    canvas
      .getContext("2d")
      .drawImage(videoRef.current, 0, 0, canvas.width, canvas.height);
    canvas.toBlob((blob) => {
      let file = new File([blob], "user_image.jpg", { type: "image/jpeg" });
      console.log(file);

      closeVideo();

      props?.onFinish(file);
    }, "image/jpeg");

  };

  const closeVideo = async () => {
    if (!stream) return;

    await stream.getTracks().forEach(function (track) {
      if (track.readyState == "live") {
        track.stop();
      }
    });
    return true;
  };
  
 
```
### the html code
```html
 <video
            style={{ border: "1px solid red", height: "300px" }}
            ref={videoRef}
            id="video"
            width="320"
            height="340"
            autoPlay
          ></video>
          <Stack direction={"row"} mt="20px">
            <Button
              size={"xs"}
              bg="#07900C"
              color="#fff"
              onClick={() => handleImage()}
            >
              Take Photo
            </Button>
            <Button
              size={"xs"}
              border="1px solid #80808030"
              bg="transparent"
              onClick={() => {
                closeVideo().then(() => props.close());
              }}
            >
              Cancel
            </Button>
          </Stack>
          <canvas
            id="canvas"
            width="320"
            height="240"
            style={{ display: "none" }}
          ></canvas>
```


# Formatting number to have 1k, 2.5M
```javascript
  function nFormatter(num) {
    if (num >= 1000000000) {
      return (num / 1000000000).toFixed(1).replace(/\.0$/, "") + "G";
    }
    if (num >= 1000000) {
      return (num / 1000000).toFixed(1).replace(/\.0$/, "") + "M";
    }
    if (num >= 1000) {
      return (num / 1000).toFixed(1).replace(/\.0$/, "") + "K";
    }
    return num;
  }
```

# Do something when video has loaded
```javascript
 useEffect(() => {
    if (isVideo === false) return;
    props?.pause();

    const videoElement = document?.getElementById("_video");

    videoElement?.addEventListener("loadeddata", (e) => {
      //Video should now be loaded but we can add a second check

      if (videoElement.readyState >= 3) {
        props?.resume();
        console.log("code has resumed");
        //your code goes here
      }
    });
  }, [isVideo]);
```

# My details
```
My name is Osaze Agbi, I’m from Nigeria
I’m a frontend engineer, focused in react library, I have been building frontend app/ library for 4 years now, all through the years I have gain skill and tricks that many people are not aware of, and may not be easily find on the internet, which I will be teaching if I’m chosen
I have written a documentation of the react package library I coded, which helps developers create forms with just few lines of code https://www.npmjs.com/package/@osaze12/bootstrapped-form, this shows my goal aligns with yours, which which is to teach people.
I also have a very short post on LinkedIn making the package library known to people 
https://www.linkedin.com/posts/osaze-agbi-b68328176_react-library-developers-ugcPost-6999720518215970816-qrdX?utm_source=share&utm_medium=member_desktop

How to observe changes of an html element width or height

How to display a video when the screen is idle after 15 seconds, just like how your bank’s Automated Teller Machine does it

This is a link to my portfolio where you would be able to see what I have created https://tinyurl.com/osas-portfolio
```


# How to preserve line breaks from text areas to p tag, just add that to p tag
```css
white-space: pre-wrap;
```

# Add dark background on image
```css
  background:`linear-gradient(90deg, rgb(0 0 0 / 63%) 0%, rgb(0 0 0 / 63%) 51%), url(${previewImg})`
```

# if .gitignore isnt ignoring your file/folders
```
Just remove the initial git configuration from your folder by rm -rf .git. 
Use the created .gitignore file and add all the file you want to add. 
Then reinitialize the git configuration in your folder with git init. This will solve your problem.
```

# error with socket IO chat failing 
```
WebSocket connection failed: Error during WebSocket handshake: Unexpected response code: 400
______
SOLUTION
_________
https://stackoverflow.com/questions/41381444/websocket-connection-failed-error-during-websocket-handshake-unexpected-respon

```

# play a sound when notification arrives
```javascript
   socketClient.on("in-app-notification", (data) => {
      //play sound when a new notification comes in
      const audio = new Audio(bellSound);
      audio?.play();
    });
```

# how to get the width of an html element
```javascript
  useEffect(() => {
    let p = document.getElementsByClassName("zz")[0];
    setSize(window.getComputedStyle(p).width);
  }, []);

```

# convert px to number
```javascript
 parseFloat("12px")
 parseInt("12px")
```


# Customize website's scrollbar like Mac OS (doesnt hide scroll on leave)
```css
body::-webkit-scrollbar-track, .scroll-container::-webkit-scrollbar-track {
  border-radius: 10px;
  background-color: ${scrollBgColor};
}

body::-webkit-scrollbar, .scroll-container::-webkit-scrollbar {
  width: 4px;
  background-color: #f5f5f5;
}

body::-webkit-scrollbar-thumb, .scroll-container::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background-color: ${scrollThumbColor};
}
```

# scroll to bottom smoothly
```javascript
  //auto scroll to bottom
  document
    .getElementById("messages")
    .scrollIntoView({ behavior: "smooth", block: "end", inline: "nearest" });
```

#  Customize website's scrollbar like Mac OS (hide scroll on leave)
```css

 body{
    overflow-y: ${
      url === "/messaging" || /\/trading-group/gm.test(url) //if url is /trading-group/[0-9] return true
        ? "hidden"
        : "initial"
    };
    overflow-x: hidden;
  }
     .scroll-container{
   overflow-x:hidden !important;
     }

  body::-webkit-scrollbar-track, 
  .scroll-container::-webkit-scrollbar-track,
 {
    border-radius: 10px;
    background-color: ${scrollBgColor};
   
  }

  body::-webkit-scrollbar, .scroll-container::-webkit-scrollbar {
    width: 4px;
    background-color: #f5f5f5;
  }

  body::-webkit-scrollbar-thumb, .scroll-container::-webkit-scrollbar-thumb {
    border-radius: 10px;
    background-color: ${scrollThumbColor};
  }

  body::-webkit-scrollbar-track-piece:end, .scroll-container::-webkit-scrollbar-track-piece:end {
    background: ${scrollTrackColor};
    margin-bottom: 0px; 
}

body::-webkit-scrollbar-track-piece:start, .scroll-container::-webkit-scrollbar-track-piece:start {
    background: ${scrollTrackColor};
    margin-top: 0px;
}

```


# project you have built
```
I've built software solutions that cut across Education, E-commerce, Health care, e.t.c for companies.

I've worked in a team, to build a software solution that manages communication between doctors and patients, so patients can easily communicate the issue he/she is having without leaving the his/her home, and payments for the service are easily deducted.

Technologies used:
1. Javascript/ReactJs library
2. HTML & CSS3
3. Chakra UI library
4. Github version control

Role : member of the team


i've also built an  e-commerce (women care products) website for a client, where people can buy easily buy products and pay.

Technologies used:
1. Javascript/ReactJs library
2. HTML & CSS3
3. Chakra UI library
4. Paystack payment method
5.  Github version control

Role: 
Lead


I have successfully created a javascript (ReactJs) library that has 318 monthly downloads (November 2022), on node package manager(npm) website.
https://www.npmjs.com/package/@osaze12/bootstrapped-form

Technologies used:
1. Javascript/ReactJs library
2. Typescript
3. HTML & CSS3
4. Github version control

Role: 
Lead


And many other project...
```

# To observe changes of an html element, e.g the div width
```javascript

  useEffect(() => {
  
    setParentWidth(ref.current.offsetWidth);
    
    const resize = () => {
      console.log("Is resize");
      setParentWidth(ref?.current?.offsetWidth);
    };
    

    const resizeObserver = new ResizeObserver((entries) => {
      console.log("yesss");
      resize();
    });
    

    resizeObserver.observe(document.getElementsByClassName("chat-area")[0]);
    
  }, []);
```

# convert image file e.target.files[0] to a base url
```javascript

 onChange={async (e) => {
   setPreviewImg(await readURL(e.target.files?.[0]));
 }}
              

// convert file to a base64 url

const readURL = file => {
  return new Promise((res, rej) => {
      const reader = new FileReader();
      reader.onload = e => res(e.target.result);
      reader.onerror = e => rej(e);
      reader.readAsDataURL(file);
  });
};
```

# scroll to a particular position/box using the id
```javascript
import { ScrollMenu} from "react-horizontal-scrolling-menu";


 const apiRef = React.useRef({});
 
  const scrollToStart = () => {
    apiRef?.current?.scrollToItem(
      apiRef?.current.getItemByIndex(activeIndex - 1),
      "smooth",
      "start"
    );
  };
  
    useEffect(() => {
    scrollToStart();
  }, [activeIndex, apiRef?.current]);
  

   <ScrollMenu
   apiRef={apiRef}
   LeftArrow={
     <IoIosArrowBack
       cursor={"pointer"}
       onClick={() => apiRef?.current?.scrollPrev()}
     />
   }
   RightArrow={
     <IoIosArrowForward
       cursor={"pointer"}
       onClick={() => apiRef?.current?.scrollNext()}
     />
   }
 >
   {getAllDaysInMonth(
     getCurrentDate?.getFullYear(),
     getCurrentDate?.getMonth()
   ).map((data, i) => {
     let addOneToLoopIndex = i + 1;

     return (
       <DateCard
         itemId={addOneToLoopIndex}
         id={addOneToLoopIndex}
         onClick={(date) => {
           console.log(date, "Date clicked");
           getSchedule(date);
           setClickedDate(date);
           setActiveIndex(addOneToLoopIndex);
         }}
         isActive={activeIndex === addOneToLoopIndex ? true : false}
       />
     );
   })}
 </ScrollMenu>

```


# Cover letter
```
I've built software solutions that cut across Ecommerce, Healthcare, etc. 
I've gained the required experience to add value to the company, the following below are some of the project i've worked on:
https://www.howbodi.io/
A health care web application where doctors can schedule doctors & have conversations via video/voice call or text chat, right on the app.
More can be found on my portfolio at https://tinyurl.com/osas-portfolio
And i've also attached my CV
Hope to hear from you soon
```

# If you have problem converting string to object using JSON.parse()
```javascript

  let jsonStr = String(r).replace(/(\w+:)|(\w+ :)/g, function (matchedStr) {
      return '"' + matchedStr.substring(0, matchedStr.length - 1) + '":';
    });
    
    JSON.parse(jsonStr)
```

# Convering string to array/object

```javascript
let object = {"name": "osas"}

use eval(object) if JSON.parse(object) doesnt work
```


# Create a persistent countdown timer

### Install react-countdown library
```javascript
import Countdown from "react-countdown";
```

```javascript
import Countdown from "react-countdown";

import { BOOKING_TIMER_IN_SECONDS } from "../app/constants";

function PersistentCountdownTimer({
  countDownStartTime,
  seconds = BOOKING_TIMER_IN_SECONDS,
  startTimer = false,
  trackingId,
  refreshOnCountDownComplete,
  setCountDownHasCompleted,
}) {
  const [data, setData] = useState(
    { date: Number(new Date(countDownStartTime)), delay: seconds } //miliseconds
  );
  
  const wantedDelay = seconds;

  const getSessionStorageValue = (s) => sessionStorage.getItem(s);

  const Completionist = () => (
    <p style={{ fontWeight: "bold " }}>Chat ended...</p>
  );

  // Renderer callback with condition
  const renderer = ({ hours, minutes, seconds, completed }) => {
    if (completed) {
      if (sessionStorage.getItem(`end_date-${trackingId}`) != null) {
        sessionStorage.removeItem(`end_date-${trackingId}`);

        sessionStorage.removeItem("timerDetailsObject");
      }
      // Render a complete state

      return <Completionist />;
    } else {
      // Render a countdown
      return (
        <Stack
          background="black"
          color="#fff"
          padding="5px 7px"
          borderRadius="5px"
          direction={"row"}
          fontWeight={"bold"}
          fontSize="1.3em"
        >
          <Text>{hours} </Text>
          <Text>:</Text>
          <Text>{minutes}</Text>
          <Text>:</Text>
          <Text>{seconds}</Text>
        </Stack>
      );
    }
  };

  //Code runs only one time after each reloading
  useEffect(() => {
    const savedDate = getSessionStorageValue(`end_date-${trackingId}`);
    if (savedDate != null && !isNaN(savedDate)) {
      const currentTime = Date.now();
      const delta = parseInt(savedDate, 10) - currentTime;

      //Do you reach the end?
      if (delta > wantedDelay) {
        //Yes we clear uour saved end date
        if (sessionStorage.getItem(`end_date-${trackingId}`).length > 0)
          sessionStorage.removeItem(`end_date-${trackingId}`);
      } else {
        //No update the end date
        setData({ date: currentTime, delay: delta });
      }
    }
    //eslint-disable-next-line
  }, []);

  if (!trackingId) return null;

  return startTimer || getSessionStorageValue(`end_date-${trackingId}`) ? (
    <div>
      <Countdown
        intervalDelay={0}
        precision={3}
        date={data.date + data.delay}
        renderer={renderer}
        // onTick={(res) => console.log(res)}
        onStart={(delta) => {
          //Save the end date
          if (sessionStorage.getItem(`end_date-${trackingId}`) === null)
            sessionStorage.setItem(
              `end_date-${trackingId}`,
              JSON.stringify(data.date + data.delay)
            );
        }}
        onComplete={() => {
          if (sessionStorage.getItem(`end_date-${trackingId}`) !== null) {
            sessionStorage.removeItem(`end_date-${trackingId}`);
          }

          setCountDownHasCompleted(true);
          refreshOnCountDownComplete();
        }}
      />
    </div>
  ) : null;
}

export default PersistentCountdownTimer;

```


# WHEN IDLE FOR X SECONDS, SHOW A VIDEO, & WHEN THERE'S A MOUSE EVENT, RESTART THE COUNTER
```javascript
  useEffect(() => {
    var IDLE_TIMEOUT = 15; //seconds
    var _idleSecondsCounter = 0;

    document.addEventListener("mousemove", () => {
      _idleSecondsCounter = 0;
      setIsIdle(false);
    });

    const interval = setInterval(CheckIdleTime, 1000);

    function CheckIdleTime() {
      _idleSecondsCounter++;

      if (_idleSecondsCounter >= IDLE_TIMEOUT) {
        setIsIdle(true);
      }
    }

    return () => clearInterval(interval);
  }, [isIdle]);
```

# ASK USER FOR PERMISSION & SUBSCRIBE FOR PUSH NOTIFICATION SERVICE

### In public folder, a created sw.js file
```javascript
self.addEventListener("push", e => {
  const { title, description } = e?.data?.json() || {}

  const options = {
    body: description,
    vibrate: [200, 100, 200],
    requireInteraction: true,
  }

  self.registration.showNotification(title, options)
})

```

### In App.js 
```javascript
  //register service worker
  useEffect(() => {
    if ("serviceWorker" in navigator) {
      window.addEventListener("load", async () => {
        await navigator.serviceWorker.register("./sw.js");
      });
    }
  }, []);
```
```javascript

  function askPermission() {
    return new Promise(function (resolve, reject) {
      const permissionResult = Notification.requestPermission(function (result) {
        resolve(result);
      });

      if (permissionResult) {
        permissionResult.then(resolve, reject);
      }
    }).then(function (permissionResult) {
      if (permissionResult !== "granted") {
        throw new Error("We weren't granted permission.");
      }
    });
  }
  
 askPermission().then((d) => {
      navigator.serviceWorker.ready.then((swreg) => {
        const push = swreg.pushManager.subscribe({
          userVisibleOnly: true,
          applicationServerKey: urlBase64ToUint8Array(WEB_PUSH_KEY),
        });

        push.then((res) => {
          const payload = {
            ...formValues,
            pushSubscription: res,
          };

          setLoading(true);
          companyLogin(payload, setLoading);
        });
      });
    });
```

# REACT CLONING REACT COMPONENT AND INJECTING DATA INTO IT
```javascript
  <Box>
    {!Array.isArray(children)
      ? React.Children.map(children, (child, index) =>
          React.cloneElement(child, {
            id: index,
            update,
            dataResponse,
            loading,
          })
        )
      : children}
  </Box>
```

# SENDING CV FORMAT TO HR
```
Dear Hiring Manager,

Please find attached a copy of my CV for the advertised role. 

I have two years experience in branding and marketing. 

My core skills include ...

I look forward to hearing from you. 

Thank you for your time and consideration.

Kind regards,

(full name)
```

# ["7:00 AM - 7:30 AM","7:30 AM - 8:00 AM","8:00 AM - 8:30 AM","8:30 AM - 9:00 AM","9:00 AM - 9:30 AM","9:30 AM - 10:00 AM"]
```javascript
 const getTimes = (start, end, step = 30) => {
    const dt = new Date(`December 17, 1995 ${start}`);
    const dc = new Date(`December 17, 1995 ${end}`);
    const rc = [];
    while (dt < dc) {
      //let newTime = dt.setMinutes(dt.getMinutes() + Number(step));
      let firstTime = dt.toLocaleTimeString("en-US", { timeStyle: "short" });
      let newTime = dt.setMinutes(dt.getMinutes() + Number(step));
      rc.push(
        `${firstTime} - ${new Date(newTime).toLocaleTimeString("en-US", { timeStyle: "short" })}`);}
   
    return rc;
  };

console.log(getTimes('7:00 am', '10:00 am', "30"))
```

# CONVERT 24 HRS TO 12 HRS

```javascript
function tConvert (time) {
  // Check correct time format and split into components
  time = time.toString ().match (/^([01]\d|2[0-3])(:)([0-5]\d)(:[0-5]\d)?$/) || [time];

  if (time.length > 1) { // If time format correct
    time = time.slice (1);  // Remove full string match value
    time[5] = +time[0] < 12 ? 'AM' : 'PM'; // Set AM/PM
    time[0] = +time[0] % 12 || 12; // Adjust hours
  }
  return time.join (''); // return adjusted time or original string
}

tConvert ('18:00:00');
```


# —CHANGING REACT-CALENDAR DAY VIEW & CUSTOM CONTROL LINK

```javascript
<Calendar
  onChange={setDate}
  value={date}
  maxDate={new Date()}
  formatDay ={
    (ff, date) => new Intl.DateTimeFormat(
      locale, 
      {
        year: "numeric", 
        month: "2-digit", 
        day: "2-digit"
      }).format(date)
    }
   />
   
   https://codesandbox.io/s/react-calendar-with-custom-controls-qe99x?file=/src/ClosingDateField.js
   
   https://codesandbox.io/s/adoring-franklin-zvmti?file=/src/App.js:262-703
   
```

# —TO GET ALL DAYS IN A MONTH

  
  ```javascript
  function getAllDaysInMonth(year, month) 
  
  {
  
    const date = new Date(year, month, 1);

    const dates = [];

    while (date.getMonth() === month) {
      dates.push(new Date(date));
      date.setDate(date.getDate() + 1);
    }

    return dates;
  }
  const date = new Date("2022-03-24");

  // 👇️ All days in March of 2022
  console.log(
    getAllDaysInMonth(date.getFullYear(), date.getMonth()),
    "<<<<<<<>>>>>>>>>>"
  );
  ```
—————————————————————————————



# —TO GET TIMES BASED ON THE START TIME AND END TIME, BUT NOT LESS THAN THE CURRENT TIME

//the last parameter, make sure users dont select time that is less than the specified datetime

```javascript
  const getTimes = (start, end, step = "30", date) => {
    if (!date) return [];
    const selectedDay = dayjs(date).format("YYYY-MM-DD");
    const today = dayjs().format("YYYY-MM-DD");
    let dt = new Date(`December 17, 1995 ${start}`);
    let dc = new Date(`December 17, 1995 ${end}`);

    if (selectedDay > today) {
      let rc = [];
      while (dt <= dc) {
        let availability =
          dt.toLocaleTimeString("en-US", { timeStyle: "short" }) +
          " - " +
          new Date(
            dt.setMinutes(dt.getMinutes() + Number(step))
          ).toLocaleTimeString("en-US", { timeStyle: "short" });
        let splitAvailability = availability.split("-");
        let startAvailability = splitAvailability[0];
        rc.push(startAvailability);
      }
      return rc;
    } else {
      const nowTime = dayjs().format("HH:mm");
      let ft = [];

      if (dayjs(dc).format("HH:mm") > nowTime) {
        while (dt <= dc) {
          if (dayjs(dt).format("HH:mm") > nowTime) {
            let availability =
              dt.toLocaleTimeString("en-US", { timeStyle: "short" }) +
              " - " +
              new Date(
                dt.setMinutes(dt.getMinutes() + Number(step))
              ).toLocaleTimeString("en-US", { timeStyle: "short" });
            let splitAvailability = availability.split("-");
            let startAvailability = splitAvailability[0];
            ft.push(startAvailability);
          } else {
            dt.setMinutes(dt.getMinutes() + Number(step));
          }
        }
        return ft;
      } else {
        return [];
      }
    }
  };
  ```
—————————————————————————————————

# —TO GET TIMES BASED ON THE START TIME AND END TIME

```javascript
  // const getTimes = (start, end, step = "30") => {
  //   const dt = new Date(`December 17, 1995 ${start}`);
  //   const dc = new Date(`December 17, 1995 ${end}`);
  //   const rc = [];
  //   while (dt <= dc) {
  //     rc.push(dt.toLocaleTimeString("en-US", { timeStyle: "short" }));
  //     dt.setMinutes(dt.getMinutes() + Number(step));
  //   }
  //   return rc;
  // };
  ```


————————————————————————————————————
# — TO GROUP SIMILAR VALUES IN AN ARRAY

```javascript
var cars = [{date: "2022-06-27"},{date: "2022-06-27"}, {date: "2022-06-28"}],

response=>
    [
[{date: “2022-06-27”}, {date: “2022-06-27”}],
[{date: “2022-06-28”},{date: “2022-06-28”}]
]
    
    result = cars.reduce(function (r, a) {
        r[a.date] = r[a.date] || [];
        r[a.date].push(a);
        return r
    }, Object.create(null));
    
```

————————————————————————————————————
# CUSTOM MONTH NAVIGATION RENDERER FOR REACT-DAY-PICKER

https://github.com/hypeserver/react-date-range/issues/222


# selecting multiple dates on react calendar
```javascript
https://stackoverflow.com/questions/60446117/how-to-mark-particular-dates-in-react-calender
```
————————————————————————————————————

# CHANGE NAV BACKGROUND COLOR WHEN SCROLL POSITION IS GREATER THAN X VALUE

```javascript

  useEffect(() => {
    let myNav = document.getElementsByClassName("nav_bar")[0];
    window.onscroll = function () {
      if (window.pageYOffset >= 150) {
        myNav.style.backgroundColor = "#E4F3F6";
      } else {
        myNav.style.backgroundColor = "transparent";
      }
    };
  }, []);
  
```

————————————————————————————————————

# Base64 string to image file

```javascript

fetch(image)
    .then(res => res.blob())
    .then(blob => {
      const file = new File([blob], "Filename.png", { type: "image/png" })
      console.log(file, "Hello")
      setImage(file)
    })
```

————————————————————————————————————

# Tutorial on web push
10:58
https://itnext.io/react-push-notifications-with-hooks-d293d36f4836

————————————————————————————————————

# Good tutorial on push notification

 https://www.youtube.com/watch?v=HlYFW2zaYQM

————————————————————————————————————

# Loading an image and getting width and height

```javascript
let image = new Image()
      image.onload = e => {
        let width = image.width
        let height = image.height
        console.log(width, height)

        dispatch("PAYLOAD", { img, imgWidth: width, imgHeight: height })
      }
      image.src = img
```


————————————————————————————————————

# Good React Calendar library

https://mmehdinasiri.github.io/react-calendar-datetime-picker/docs/examples/


————————————————————————————————————

# Having cors issues, add  "Access-Control-Allow-Headers": "Content-Type" to =>

 ```javascript
headers: {
    Accept: "application/json, text/plain, */*",
    "Content-Type": "application/json ",
    "Access-Control-Allow-Headers": "Content-Type",
  },
})
```

————————————————————————————————————

# From 1200 to 12:00 code =>
```javascript

mystring = mystring.replace(/(..)/g, '$1:').slice(0,-1)
```


————————————————————————————————————

# To get range of fromTime toTime

["9:30 AM","10:0 AM","10:30 AM","11:0 AM","11:30 AM","12:0 PM","12:30 PM","1:0 PM","1:30 PM","2:0 PM","2:30 PM","3:0 PM","3:30 PM","4:0 PM","4:30 PM","5:0 PM","5:30 PM","6:0 PM","6:30 PM","7:0 PM","7:30 PM","8:0 PM","8:30 PM","9:0 PM","9:30 PM","10:0 PM","10:30 PM"]

```javascript
export const getTimes = (start, end) => {
  if (!start || !end) return
  start = parseInt(start) * 2 + (+start.slice(-2) > 0)
  end = parseInt(end) * 2 + (+end.slice(-2) > 0) + 1
  return Array.from({ length: end - start }, (_, i) => [
    (i + start) >> 1,
    ((i + start) % 2) * 30,
  ]).map(([h, m]) =>
    `${h % 12 || 12}:${m} ${"AP"[+(h > 11)]}M`.replace(/bdb/g, "0$&")
  )
}
```
https://www.tutorialguruji.com/javascript/javascript-generate-an-array-of-times-based-on-a-start-and-an-end/

————————————————————————————————————

# Check if user is authenticated

```javascript
useEffect(() => {
    // Handle user state changes
    const getAuthData = async obj => {
      dispatch(authSetUser({ ...obj }))
      setIsLoggedIn(true)
      if (checkingStatus) setCheckingStatus(false)
    }

    const deleteTokenAndKickUserOut = async () => {
      localStorage.removeItem("11#221#")
      if (checkingStatus) setCheckingStatus(false)
    }

    const token = localStorage.getItem("11#221#")

    if (token) {
      const decoded = jwtDecode(token)
      const expiryDate = new Date(decoded.exp * 1000)
      return new Date() > expiryDate
        ? deleteTokenAndKickUserOut()
        : getAuthData(decoded)
    }
    return deleteTokenAndKickUserOut()
  }, [checkingStatus, dispatch])

```
————————————————————————————————————

# How to implement web workers

https://blog.logrocket.com/real-time-processing-web-workers/



————————————————————————————————————

# HOW TO SET UP WEB WORKERS

```javascript
// in App.js

  useEffect(() => {
    const notification = setInterval(function () {
      textAnalyzer.postMessage("start")
    }, 5000)
    return () => clearInterval(notification)
  }, [])

  textAnalyzer.addEventListener("message", event => {
    const data = event.data
    console.log(data, "received in app")
  })

//in notification function, in the public folder

async function getNotificationsData() {
  const response = fetch("https://api.github", {
    method: "GET",
    headers: { "Content-type": "application/json;charset=UTF-8" },
  })
    .then(response => response.json())
    .then(json => {
      return "returned true"
    })
    .catch(err => {
      return "returned false"
    })
  const gottenResponse = await response
  return gottenResponse
}

self.addEventListener("message", event => {
  console.log(event.data)
  if (typeof event.data === "string") {
    getNotificationsData().then(data => {
      postMessage(data)
    })
  } else {
    throw new Error("Unable to analyze non-string data")
  }
})

```
————————————————————————————————————

# Updated web worker code 1

```javascript

async function getNotificationsData(obj) {
  const { adminId, companyCode, token, baseURL } = obj || {}
  const response = fetch(
    `${baseURL}/employee/appointment-notification?adminId=${adminId}&companyCode=${companyCode}`,
    {
      method: "GET",
      headers: {
        "Content-type": "application/json;charset=UTF-8",
        Authorization: `Bearer ${token}`,
      },
    }
  )
    .then(response => response.json())
    .then(json => {
      return json
    })
    .catch(() => {
      return false
    })
  const gottenResponse = await response
  if (gottenResponse) {
    return gottenResponse
  }
  throw new Error()
}

self.addEventListener("message", event => {
  getNotificationsData(event.data)
    .then(data => {
      postMessage(data)
    })
    .catch(() => console.log("error Fetching notification"))
})
```

————————————————————————————————————

# Updated web worker code 2
```javascript

async function getNotificationsData(obj) {
  const { adminId, companyCode, token, baseURL } = obj || {}
  const response = fetch(
    `${baseURL}/employee/appointment-notification?adminId=${adminId}&companyCode=${companyCode}`,
    {
      method: "GET",
      headers: {
        "Content-type": "application/json;charset=UTF-8",
        Authorization: `Bearer ${token}`,
      },
    }
  )
    .then(response => response.json())
    .then(json => {
      return json
    })
    .catch(() => {
      return false
    })
  const gottenResponse = await response
  if (gottenResponse) {
    return gottenResponse
  }
  throw new Error()
}

self.addEventListener("message", event => {
  getNotificationsData(event.data)
    .then(data => {
      postMessage(data)
    })
    .catch(() => console.log("error Fetching notification"))
})
```

————————————————————————————————————

# To remove error from eslint on hook dependency


// eslint-disable-next-line react-hooks/exhaustive-deps

————————————————————————————————————

# To get last day of month
```javascript

dayjs().endOf("month").date(),  of the month
```

————————————————————————————————————

# To get the current month dates, eg= [2021-01-01, 2021-01-02, 2021-01-03]
```javascript

const getAllDatesForCurrentMonth = () => {
    var date = new Date()
    var month = date.getMonth()
    date.setDate(1)
    var all_days = []
    while (date.getMonth() === month) {
      var d =
        date.getFullYear() +
        "-" +
        (date.getMonth() + 1).toString().padStart(2, "0") +
        "-" +
        date.getDate().toString().padStart(2, "0")
      all_days.push(d)
      date.setDate(date.getDate() + 1)
    }

    const formattedDates = all_days.map(date => {
      const dateArray = date.split("-")
      const year = dateArray[0]
      const month = dateArray[1]
      const day = dateArray[2]

      return {
        year,
        month,
        day,
      }
    })
    return formattedDates
  }
  ```

————————————————————————————————————

# Better and optimised
["9:00 AM","12:00 PM","3:00 PM","6:00 PM","9:00 PM"]

```javascript

function generateTime(start, end, step) {
        const dt = new Date(`December 17, 1995 ${start}`);
        const dc = new Date(`December 17, 1995 ${end}`);
        const rc = [];
        while (dt <= dc) {
            rc.push(dt.toLocaleTimeString('en-US',{timeStyle: 'short'}));
            dt.setMinutes(dt.getMinutes() + step);
        }
        return rc;
    }
console.log(generateTime("9:00", "23:00", 180))
```

————————————————————————————————————

# To have a lighter color
```javascript

 _hover={{ bg: `${bg}80` }}
```
————————————————————————————————————

# Display something else when screen is idle for 15 seconds
```javascript

const [isIdle, setIsIdle] = useState(false)
useEffect(() => {
    var IDLE_TIMEOUT = 15 //seconds
    var _idleSecondsCounter = 0

    document.addEventListener("mousemove", () => {
      _idleSecondsCounter = 0
      setIsIdle(false)
    })

    window.setInterval(CheckIdleTime, 1000)

    function CheckIdleTime() {
      _idleSecondsCounter++

      if (_idleSecondsCounter >= IDLE_TIMEOUT) {
        setIsIdle(true)
      }
    }
  }, [])
  ```

————————————————————————————————————

# To have 3 box in a row

```css
.Row {
    display: table;
    width: 100%; /*Optional*/
    table-layout: fixed; /*Optional*/
    border-spacing: 10px; /*Optional*/
}

.Column {
    display: table-cell;
    background-color: red; /*Optional*/
}

<div class="Row">
    <div class="Column">C1</div>
    <div class="Column">C2</div>
    <div class="Column">C3</div>
</div>
```

————————————————————————————————————

# To  auto play video, in a loop, in react
```javascript

<video autoPlay loop muted playsInline style={{ width: "100vw" }}>
  <source src={idleVideo} type="video/mp4" />
  Your browser does not support the video tag.
 </video>
```
————————————————————————————————————

# Javascript convert to boolean, doesn't work, eg: Boolean("false") === true

//right way \|/
```javascript

const bool =
      e.toLowerCase() === "true"
        ? true
        : e.toLowerCase() === "false"
        ? false
        : e.toLowerCase()
 ```


————————————————————————————————————

# Create .zip files using JavaScript.


https://stuk.github.io/jszip/

stuk.github.io
JSZip
Create .zip files using JavaScript. Provides a simple API to place any content generated by JavaScript into a .zip file for your users.

————————————————————————————————————

# An HTML5 saveAs() FileSaver 

https://github.com/eligrey/FileSaver.js

eligrey/FileSaver.js
An HTML5 saveAs() FileSaver implementation
Website
https://eligrey.com/blog/saving-generated-files-on-the-client-side/




————————————————————————————————————

# To remove time icon from input

 ```javascript
const OVERRIDE_TIME_STYLE = `input[type="time"]::-webkit-calendar-picker-indicator {
//     display: none !important;
// }
// }`
```
————————————————————————————————————

# JavaScript program to run a function in a separate thread by using a Web Worker, allowing long running functions to not block the UI.

```javascript
//https://www.w3resource.com/javascript-exercises/fundamental/javascript-fundamental-exercise-151.php

const runAsync = fn => {
  const worker = new Worker(
    URL.createObjectURL(new Blob([`postMessage((${fn})());`]), {
      type: 'application/javascript; charset=utf-8'
    })
  );
  return new Promise((res, rej) => {
    worker.onmessage = ({ data }) => {
      res(data), worker.terminate();
    };
    worker.onerror = err => {
      rej(err), worker.terminate();
    };
  });
};
const longRunningFunction = () => {
  let result = 0;
  for (let i = 0; i < 1000; i++) {
    for (let j = 0; j < 700; j++) {
      for (let k = 0; k < 300; k++) {
        result = result + i + j + k;
      }
    }
  }
  return result;
};
/*
*/
runAsync(longRunningFunction).then(console.log); // 209685000000
runAsync(() => 10 ** 3).then(console.log); // 1000
let outsideVariable = 50;
runAsync(() => typeof outsideVariable).then(console.log); // 'undefined'
```
————————————————————————————————————

# Best web worker to use in react

```javascript
//https://www.npmjs.com/package/web-worker-hooks#installation

import { useWorker } from "web-worker-hooks"

const [tableValues, setTableValues] = useState([])

  const worker = useWorker((postMessage, setOnMessage) => {
    setOnMessage(msg => {
      function resultss(getData) {
        if (!Array.isArray(getData)) return []
        return getData.reduce(function (results, org) {
          ;(results[org?.id] = results[org?.id] || [])?.push(org)
          return results
        }, {})
      }
      postMessage(resultss(msg.data))
    })
  })

  useEffect(() => {
    worker.onmessage = msg => {
      setTableValues(Object.values(msg.data))
    }
    worker.postMessage(getData)
    //eslint-disable-next-line
  }, [getData])

```
————————————————————————————————————

# To remove white background from favicon

```html
add type="image/x-icon"

<link rel="icon" href="%PUBLIC_URL%/favicon.ico" type="image/x-icon" />

```
————————————————————————————————————

# Change nav bar color when user scroll to x position


```javascript
useEffect(() => {
    let myNav = document.getElementsByClassName("nav_bar")[0];
    window.onscroll = function () {
      if (window.pageYOffset >= 150) {
        myNav.style.backgroundColor = "#E4F3F6";
      } else {
        myNav.style.backgroundColor = "transparent";
      }
    };
  }, []);
  ```




————————————————————————————————————

# Overlay text on an image using css grid
```html
   <div
      display="grid"
      gridTemplateColumns="repeat (5, 20%)"
      gridTemplateRows="repeat (5, 20%)"
      columnGap="10px"
    >
      <img
        w="100%"
        src=""
        alt="imgage"
        zIndex="444"
      />
      <div position={"absolute"} zIndex="555" gridColumn="1">
        <p>WE FOCUS ON GATHERING DATA THAT ENHANCE USER EXPERIENCE</p>
        <p>
          Our mission is to analyze and solve the strategic or routine IT
        </p>
      </div>
    </div>
```
    
    
————————————————————————————————————

# Dot loading animation svg

https://codepen.io/nikhil8krishnan/pen/rVoXJa
https://css-tricks.com/single-element-loaders-the-dots/

 ————————————————————————————————————

# Animation library for react
"aos": "^2.3.4",
```javascript
useEffect(() => {
  Aos.init({ duration: 1000 })
}, [])
  
<div data-aos="fade-right"></div>
```

 ————————————————————————————————————
 # scroll on Drag for users with older laptops
 ### index.css
 ```css
 .items {
  display: flex;
  overflow: scroll;
  /* scroll-behavior: smooth; */
}
 ```
 ### index.html
 ```
 <div class="items">
    <div class="item item1"></div>
    <div class="item item2"></div>
    <div class="item item3"></div>
    <div class="item item4"></div>
    <div class="item item5"></div>
  </div>
 ```
 
 ### index.js
 
 ```javascript
 const slider = document.querySelector(".items");
let isDown = false;
let startX;
let scrollLeft;

slider.addEventListener("mousedown", (e) => {
  isDown = true;
  slider.classList.add("active");
  startX = e.pageX - slider.offsetLeft;
  scrollLeft = slider.scrollLeft;
});
slider.addEventListener("mouseleave", () => {
  isDown = false;
  slider.classList.remove("active");
});
slider.addEventListener("mouseup", () => {
  isDown = false;
  slider.classList.remove("active");
});
slider.addEventListener("mousemove", (e) => {
  if (!isDown) return;
  e.preventDefault();
  const x = e.pageX - slider.offsetLeft;
  const walk = (x - startX) * 3; //scroll-fast
  slider.scrollLeft = scrollLeft - walk;
  console.log(walk);
});

 ```
 

# Hide Scrollbars But Keep Functionality
```css

/* Hide scrollbar for Chrome, Safari and Opera */
.example::-webkit-scrollbar {
  display: none;
}

/* Hide scrollbar for IE, Edge and Firefox */
.example {
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}
```


 ————————————————————————————————————

# Scroll forward/backward with a button/arrow

```javascript

const scrollRight = () => {
    let obj = document.getElementById("review-scroll");
    obj.scrollBy(400, 0);
    console.log(obj);
  };
  const scrollLeft = () => {
    let obj = document.getElementById("review-scroll");
    obj.scrollBy(-400, 0);
  };
  
  <div
  overflowX="scroll"
  id="review-scroll"
  scrollBehavior={"smooth"}>
  </div>
```
  
  
 ————————————————————————————————————

# allow html formatting in a string, inner html
```javascript
  
  const App = () => {
  const data = `lorem ipsum <img src="" onerror="alert('message');" />`;

  return (
    <div
      dangerouslySetInnerHTML={{__html: data}}
    />
  );
}
```
 ————————————————————————————————————

# TYPES OF COMMENTS

// ! critical comment
// * hightlighted comment
// TODO: todo comment
// ? question comment
// normal comment



 ————————————————————————————————————

# ADD DAYS TO DATE, 1 Mon 2022 => 2 Tue 2022 => 3 Wed 2022

```javascript
function addDays(date, days) {
    var result = new Date(date);
    result.setDate(result.getDate() + days);
    return result;
}
```



 ————————————————————————————————————

# Ux for mobile keyboard to the next input

```
enterKeyhint="next"
enterKeyhint="done"
```


# Paginated table code
```javascript
const limit = DATA_ROWS.LIMIT;

export const PaginatedTable = ({
  columns = [],
  data,
  updateTable,
  length = 0,
  total = 0,
  noPagination,
  bg,

  tableSize,
}) => {
  const [loading, setLoading] = useState(false);
  const [page, setPage] = useState(0);
  const [isEmpty, setEmpty] = useState(false);
  const [maxPageLimit] = useState(5);

  useEffect(() => {
    if (length < 1 && !noPagination) {
      setEmpty(({}, () => true));
    } else {
      setEmpty(({}, () => false));
    }
  }, [length, noPagination]);

  const NUMBER_OF_PAGES = Math.ceil(Number(total) / DATA_ROWS.LIMIT);

  const getPageNumbers = () => {
    let currentPage = page;
    let p = NUMBER_OF_PAGES >= maxPageLimit ? maxPageLimit : NUMBER_OF_PAGES;
    let start = Math.floor(currentPage / p) * p;
    return new Array(p).fill().map((_, idx) => start + idx + 1); //get range 1-5, 6-10
  };

  const goBack = () => {
    if (page === 0) return;

    setPage((initial) => initial - 1);
    setLoading(true);
    updateTable(page - 1).then(() => setLoading(false));
  };

  const isLastPage = total && limit * (page + 1) >= total;

  const goNext = () => {
    if (!length) {
      return;
    }

    if (length < limit || isLastPage) {
      return;
    }

    setPage((initial) => initial + 1);
    setLoading(true);
    updateTable(page + 1).then(() => setLoading(false));
  };

  const ARROW_STYLE = {
    borderRadius: "10px",
    bg: "transparent",
    _hover: { background: "transparent" },
    _focus: { boxShadow: "none" },
    p: "5px",
    cursor: "pointer",
  };

  const goToPageX = (page) => {
    setLoading(true);
    setPage(page);
    updateTable(page)
      .then(() => {
        setLoading(false);
      })
      .catch(() => {
        console.log("Encountered error");
      });
  };

  return (
    <Box
      overflow="auto"
      h="600px"
      bg={bg || "#fff"}
      p="10px"
      my="20px"
      borderRadius="10px"
      position="relative"
    >
      {loading ? (
        <FullPageLoader h="100%" bg="transparent" />
      ) : (
        <Table size={tableSize || "md"} variant="simple">
          <Thead>
            <Tr>
              {columns?.map((item) => {
                return (
                  <Th py="10px" key={nanoid()}>
                    {item}
                  </Th>
                );
              })}
            </Tr>
          </Thead>
          <Tbody position="relative" fontSize=".8em">
            {isEmpty ? (
              <Th>
                <Box
                  position="absolute"
                  transform="translate(-50%, 30%)"
                  left="50%"
                >
                  <GrDocumentMissing fontSize="150px" opacity=".05" />
                </Box>
              </Th>
            ) : (
              data
            )}
          </Tbody>
        </Table>
      )}

      {!noPagination && (
        <HStack spacing="2px" justifyContent="flex-end" pr="10px">
          <Text fontSize={".9em"}>
            Showing 1 - {limit} of {NUMBER_OF_PAGES}
          </Text>
          <Button {...ARROW_STYLE} onClick={goBack} disabled={page === 0}>
            <BiChevronLeft />
          </Button>
          <Stack direction="row">
            {getPageNumbers()?.map((pageNumber) => {
              let t = pageNumber - 1;
              return pageNumber <= NUMBER_OF_PAGES ? (
                <Text
                  border="1px solid #8080806b"
                  borderRadius={"5px"}
                  fontSize=".8em"
                  paddingX="7px"
                  cursor={"pointer"}
                  onClick={() => (page === t ? null : goToPageX(t))}
                  bg={page === t ? "#299d8f" : "transparent"}
                  color={page === t ? "#fff" : "#000"}
                >
                  {pageNumber}
                </Text>
              ) : null;
            })}
          </Stack>
          <Button
            {...ARROW_STYLE}
            onClick={goNext}
            disabled={length < limit || isLastPage}
          >
            <BiChevronRight />
          </Button>
        </HStack>
      )}
    </Box>
  );
};
```
