# Project-code-notes
These is where i document things i have learnt, or found on the internet that are not easily findable
https://github.com/30-seconds/30-seconds-of-code


# CONVERT 24 HRS TO 12 HRS
```
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

```
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

  
  ```
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

```
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

```
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

```
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

————————————————————————————————————

# CHANGE NAV BACKGROUND COLOR WHEN SCROLL POSITION IS GREATER THAN X VALUE

```

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

```

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

```
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

 ```
headers: {
    Accept: "application/json, text/plain, */*",
    "Content-Type": "application/json ",
    "Access-Control-Allow-Headers": "Content-Type",
  },
})
```

————————————————————————————————————

# From 1200 to 12:00 code =>
```

mystring = mystring.replace(/(..)/g, '$1:').slice(0,-1)
```


————————————————————————————————————

# To get range of fromTime toTime

["9:30 AM","10:0 AM","10:30 AM","11:0 AM","11:30 AM","12:0 PM","12:30 PM","1:0 PM","1:30 PM","2:0 PM","2:30 PM","3:0 PM","3:30 PM","4:0 PM","4:30 PM","5:0 PM","5:30 PM","6:0 PM","6:30 PM","7:0 PM","7:30 PM","8:0 PM","8:30 PM","9:0 PM","9:30 PM","10:0 PM","10:30 PM"]

```
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

```
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

```
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

```

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
```

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
```

dayjs().endOf("month").date(),  of the month
```

————————————————————————————————————

# To get the current month dates, eg= [2021-01-01, 2021-01-02, 2021-01-03]
```

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

```

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
```

 _hover={{ bg: `${bg}80` }}
```
————————————————————————————————————

# Display something else when screen is idle for 15 seconds
```

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

```
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
```

<video autoPlay loop muted playsInline style={{ width: "100vw" }}>
  <source src={idleVideo} type="video/mp4" />
  Your browser does not support the video tag.
 </video>
```
————————————————————————————————————

# Javascript convert to boolean, doesn't work, eg: Boolean("false") === true

//right way \|/
```

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

 ```
const OVERRIDE_TIME_STYLE = `input[type="time"]::-webkit-calendar-picker-indicator {
//     display: none !important;
// }
// }`
```
————————————————————————————————————

# JavaScript program to run a function in a separate thread by using a Web Worker, allowing long running functions to not block the UI.

```
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

```
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

```
add type="image/x-icon"

<link rel="icon" href="%PUBLIC_URL%/favicon.ico" type="image/x-icon" />

```
————————————————————————————————————

# Change nav bar color when user scroll to x position


```
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
```
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
```
useEffect(() => {
  Aos.init({ duration: 1000 })
}, [])
  
<div data-aos="fade-right"></div>
```

 ————————————————————————————————————

# Hide Scrollbars But Keep Functionality
```

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

```

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
```
  
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

```
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


