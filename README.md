# Project-code-notes
This is where I document things I have learnt, or found on the internet that are not easily findable
https://github.com/30-seconds/30-seconds-of-code


#### Add animation to react app with a single line of code
```
https://auto-animate.formkit.com/#examples
```

#### React Native: remove spacing/space around text by the line height
```javascript
//change the line height:
<Text lineHeight={...} >...</Text> 
```

#### generate random alphanumeric values/ids 
```javascript
export const randomId = function (length = 6) {
  return Math.random()
    .toString(36)
    .substring(2, length + 2);
};

  const theCode = randomId(10);
  // result => G0F2MOFP4C
```

#### how to check an array against/in another array 
```javascript
let arr1 = [1, 2, 3];
let arr2 = [2, 3];

let isFound = arr1.some( ai => arr2.includes(ai) );

Alternatively, if must contain all values:

let allFound = arr2.every( ai => arr1.includes(ai) );
```

#### javascript library that provides easy functions to solve real word logic, without writing too many codes
```javacsript
https://immutable-js.com/docs/v4.3.7/hasIn()/
```

#### Raact Native: Best Date Picker
```
https://www.npmjs.com/package/react-native-date-picker
```

#### Best email template creation using react 
```
https://react.email
```

#### React Native: to hide bottom tab bar on a particular screen/component/UI
```javascript
  const { getParent } = useNavigation();
//or, get the navigation from the props, like props?.navigation.getParent();

  useLayoutEffect(() => {
       const parent = getParent();
	
 	parent?.setOptions({
      	tabBarStyle: { display: 'none' },
    	});


   

	return () => {
      	  parent?.setOptions({
          tabBarStyle: { display: 'flex' , ...with_the_style_back},
      	});
    };
  }, []);
```

#### Chaining(chain) Function
```javascript
//https://dev.to/sundarbadagala081/javascript-chaining-3h6g
//Example


// after calling this on your app, reload so you dont see one instace updated in multple places in your app

//the type here returns/bring suggestions when using/chaining the functions
type FeatureLimit = {
  noOfProduct(): Promise<FeatureLimit>;

  proceed: FeatureLimit | boolean;
  canProceed: FeatureLimit;
};
export const BRAND_BORDER_STYLE:FeatureLimit = {
  styles: {
    borderTopLeftRadius: 0 as any,
    borderTopRightRadius: 0 as any,
  },

  addBlackBackgroundColor: function () {
    this.styles = {
      ...this.styles,
      backgroundColor: '#000',
    };
    return this; // to expose every func in this object back with the specific object we just updated
  },
  spread: function () { // use function instead of ()=>, not to return errors
    return this.styles;
  },
};

console.log(BRAND_BORDER_STYLE.addBlackBackgroundColor().spread())



```
#### to chain async functions (let one finish running before running the rest (typescript)
```javascript
//https://stackoverflow.com/questions/55413162/how-to-use-function-chaining-with-async-function

// typescript
export class Walker {
  private task: Promise<void>;

  constructor() {
    // Initialize the task queue
    this.task = Promise.resolve();
  }

  // Schedules a callback into the task queue
  // The callback is expected to return a Promise
  doTask(cb: () => Promise<void>): this {
    // Schedule the callback to be run after the current task
    this.task = this.task.then(cb).catch((error) => {
      // TODO: Handle errors
      console.error('Error in task:', error);
    });
    return this;
  }

  // Allows using this like a promise, e.g. "await walker"
  then(onFulfilled: (value: Promise<void>) => void): void {
    this.task.then(() => onFulfilled(this.task));
  }

  // Example method to demonstrate chaining
  goToMainPage(): this {
    this.doTask(async () => {
      console.log('goToMainPage started');
      await new Promise<void>((resolve) => setTimeout(resolve, 1000));
      console.log('goToMainPage done');
    });
    return this;
  }
}




//That allows you to do:

 //await walker.goToMainPage();
 //await walker.goToMainPage().goToMainPage();
//If you return something from inside doTask, awaiting it will resolve to that:

 //returnStuff() {
   this.doTask(() => "stuff");
   return this;
 }

 //to get the response from "then", do this:
 new Walker()
        .noOfProductWithinLimit()
        .canProceed()
        .then(async (res) => res.then((r) => console.log(r, '*')));



// console.log(await walker.returnStuff()); // "stuff"
// console.log(await walker.returnStuff().goToMainPage()); // undefined, result gets lo
//example
//   await new Walker().goToMainPage().goToMainPage();
```
##### non typescript
```javascript

class Walker {
   constructor(t) {
     this.t = t;
     // set up a task queue for chaining
     this.task = Promise.resolve();
    }

    // shedules a callback into the task queue
    doTask(cb) {
       // TODO: error handling
       return this.task = this.task.then(cb);
    }

    // allows to use this like a promise, e.g. "await walker";
    then(cb) { cb(this.task); }

    goToMainPage () {
      this.doTask(async () => { // shedule to chain
        await t.goTo('url/main')
      });
      return this; // return walker for chaining
   }

 }
//example
  await new Walker().goToMainPage().goToMainPage();
```

#### React Native: how to change react native icon
```
1. Go to https://icon.kitchen
 2. Update AndroidManifest.xml (android)
<application
    android:icon="@mipmap/ic_launcher"
    android:roundIcon="@mipmap/ic_launcher_round"
    ... >
    ...
</application>

3.  Clean and Rebuild the Project(android)
cd android
./gradlew clean

4. start server

For IOS, it easier, go to xcode, and select image folder, to update it


```

#### Better Lazy Load(loading) Image, only show when in view port
```javascript
//https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API

const LazyImage=({id})=> {

const [inview, setInView]=useState(false)
	const ref = useRef()


let callback = (entries, observer) => {
  entries.forEach((entry) => {
	if (entry.isIntersecting) {
	 	setInView(true)
	}

    // Each entry describes an intersection change for one observed
    // target element:
    //   entry.boundingClientRect
    //   entry.intersectionRatio
    //   entry.intersectionRect
    //   entry.isIntersecting
    //   entry.rootBounds
    //   entry.target
    //   entry.time
  });
};

	

	useEffect(()=> {
	let observer = new IntersectionObserver(callback, options);
	if (ref.current){
	observer.observe(ref.current)
	}
	
	return ()=> {
		observer.disconnect()
	}
	}, [])

	return inview? <Img src={} /> : (

	<Img id={} ref={ref}  style={{width:"", height:"", backgroundColor:""}}/>
)
}
```

#### Boost/Increase React Native Performance / optimize app, solve rerender issue
```javascript
https://sugandsingh5566.medium.com/boosting-react-native-performance-with-lazy-loading-and-code-splitting-f7d0f7268e7e

https://legacy.reactjs.org/docs/code-splitting.html

// avoid creating more than one component in a single file, so that it doesnt cause the whole Ui from rerendering, causing you to start the process of getting to that particular UI again, which saves your battery 

//USE MEMO(COMPONENT) & USE_MEMO, SO THAT CHILD COMPONENT THAT HAVE NO BUSINESS RERENDERING, DOESNT RERENDER, AND CAUSE THE APP TO BE SLOW

//Lazyly load the screens
const HomeScreen = React.lazy(() => import('./HomeScreen'));
const ProfileScreen = React.lazy(() => import('./ProfileScreen'));


<React.Suspense fallback={<Text>Loading...</Text>}>
 </React.Suspense>

//A Few Functional Uses for Intersection Observer to Know When an Element is in View
//** https://css-tricks.com/a-few-functional-uses-for-intersection-observer-to-know-when-an-element-is-in-view/

// For Intersection Observer in React Native, it's flat list, below is a tutorial
// https://suelan.github.io/2020/01/21/onViewableItemsChanged/
```

#### How to change select input dropdown
```css
.filter-select{
	-webkit-appearance: none;
   	-moz-appearance: none;
   	appearance: none;       /* Remove default arrow */
	color: #0F1519 !important;
	background:url(${getAsset('images/chevron-down.png')}) no-repeat right #ffffff;

}

//or
.filter-select{
	-webkit-appearance: none;
   	-moz-appearance: none;
   	appearance: none;       /* Remove default arrow */
 	background: #5A71E1 url("data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' width='4' height='5' viewBox='0 0 4 5'><path fill='%23ffffff' d='M2 0L0 2h4zm0 5L0 3h4z'/></svg>") right .75rem center/8px 10px no-repeat!important;
 }

```

#### React Native: add scroll to a fixed height 
```
https://stackoverflow.com/questions/65815131/flatlist-set-height-and-scroll
```

#### React Native: why hide/show password input isnt working
```
secureTextEntry works if you set autoCapitalize={'none'}.

https://github.com/facebook/react-native/issues/30148#issuecomment-748288773

https://stackoverflow.com/questions/54684814/react-native-securetextentry-not-working-on-android
```

#### Android/IOS app change icons
```
https://icon.kitchen
```

#### Get nigeria state and all local government
```
https://www.npmjs.com/package/naija-state-local-government
```

#### Graphql pagination useQuery fetch more items as you scroll down functionality
```javascript
 onEndReachedThreshold={0.5}
        onEndReached={({ distanceFromEnd }) => {
          fetchMore({
            variables: {
              data: {
                page: {
                  limit: PAGINATION_LIMIT,
                  offset: data?.findCoupons?.data?.length, // Pass the current total number of items as offset
                },
                query: {
                  storeId,
                },
              },
            },

            updateQuery: (
              prev: any,
              { fetchMoreResult, variables: { offset = data?.findCoupons?.data?.length } }: any,
            ) => {
              if (!fetchMoreResult) return prev;
              const incomingData = fetchMoreResult?.findCoupons?.data;

              const updatedFeed = prev.findCoupons?.data.slice(0);

              for (let i = 0; i < incomingData.length; ++i) {
                updatedFeed[offset + i] = incomingData[i];
              }

              return { ...prev, findCoupons: { ...prev.findCoupons, data: updatedFeed } }; // this structure has to be the way it came in initially
            },
          });
        }}
```

#### React native: firebase push notification set up
```
https://medium.com/@ashoniaa/react-native-expo-push-notifications-with-fcm-a-step-by-step-guide-fa5cfc0372fd
```
#### screenshot and save html element
```javascript
  const ToCaptureRef = React.useRef();

const captureScreenshot = () => {
    var canvasPromise = html2canvas(ToCaptureRef.current, {
      useCORS: true,
    });
    canvasPromise.then((canvas) => {
      var dataURL = canvas.toDataURL("image/png");
      // Create an image element from the data URL
      var img = new Image();
      img.src = dataURL;
      img.download = dataURL;
      // Create a link element
      var a = document.createElement("a");
      a.innerHTML = "DOWNLOAD";
      a.target = "_blank";
      // Set the href of the link to the data URL of the image
      a.href = img.src;
      // Set the download attribute of the link
      a.download = img.download;
      // Append the link to the page
      document.body.appendChild(a);
      // Click the link to trigger the download
      a.click();
    });
  };

  <Stack
          flex={1}
          alignSelf={"stretch"}
          border="1px solid ####7c5dd6"
          borderRadius={"10px"}
          overflow={"hidden"}
          pos={"relative"}
          justifyContent={"center"}
          alignItems={"center"}
          ref={ToCaptureRef}
        >...

```

#### convert date to my local time zone in date-fns
```javascript
const transformDateToTimezone = (date: Date, timeZone: string) => {
  const targetTimezoneOffset = getTimezoneOffset(timeZone);
  return new Date(date.getTime() - date.getTimezoneOffset() + targetTimezoneOffset);
};


    // Get the time zone of the local environment
    const timeZone = Intl.DateTimeFormat().resolvedOptions().timeZone;

  const startOfToday = transformDateToTimezone( startOfDay(today), timeZone);
```

#### Timeline UI and logic (dots arrow, horizontal line)
```
     <Box pt="20px">
              {[0, 1, 2].map((status, index) => (
                <HStack key={index} alignItems="flex-start" spacing="20px">
                  <VStack alignItems="center" spacing={0}>
                    <Box position="relative" width="20px" height="20px">
                      <Box
                        as={motion.div}
                        width="10px"
                        height="10px"
                        rounded="full"
                        bg={orderStatus >= status ? Colors.purple : Colors.gray}
                        zIndex="1"
                        position="absolute"
                        top="50%"
                        left="50%"
                        transform="translate(-50%, -50%)"
                      ></Box>
                      <Box
                        position="absolute"
                        top="50%"
                        left="50%"
                        width="20px"
                        height="20px"
                        border="2px solid"
                        borderColor={
                          orderStatus >= status ? Colors.purple : Colors.gray
                        }
                        borderRadius="50%"
                        transform="translate(-50%, -50%)"
                      ></Box>
                    </Box>
                    {index < 2 && (
                      <Box
                        width="2px"
                        height="90px"
                        bg={orderStatus > status ? Colors.purple : Colors.gray}
                        backgroundImage={
                          index === 1
                            ? `linear-gradient(${Colors.white} 50%, transparent 50%)`
                            : "none"
                        }
                        backgroundSize="2px 8px"
                      ></Box>
                    )}
                  </VStack>
                  <Stack spacing={1}>
                    <Text
                      fontSize="18px"
                      fontWeight="semibold"
                      color={orderStatus >= status ? Colors.black : Colors.gray}
                    >
                      {status === 0
                        ? "Order Placed"
                        : status === 1
                        ? "Order Pending"
                        : "Order Confirmed"}
                    </Text>
                    <Text
                      fontSize="14px"
                      color={orderStatus >= status ? Colors.black : Colors.gray}
                      fontWeight="light"
                    >
                      {status === 0
                        ? "You’ve successfully placed your order on the website."
                        : status === 1
                        ? "Your order has been received but is waiting for further action, such as store confirmation."
                        : "The physical store has confirmed your order and is ready to proceed with fulfilment."}
                    </Text>
                  </Stack>
                </HStack>
              ))}
            </Box>
```
#### How to convert array to object
```javascript
const list = [
  { name: "Abi" },
  { age: 25 },
  { location: "New York" },
  { name: "John" } // this will overwrite "Abi" if merged directly
];

const ob = list.reduce((acc, curr) => {
  return { ...acc, ...curr };
}, {});

console.log(ob); // { name: "John", age: 25, location: "New York" }

```

#### React native: scroll item not reaching the bottom
```javascript
use flexGrow:1 instead of flex in :<ScrollView contentContainerStyle={{ flexGrow: 1 }}/>
```

#### remove object with duplicate keys from an array
```javascript
const arrayWithDuplicates = [
  { id: 1, name: 'John' },
  { id: 2, name: 'Jane' },
  { id: 1, name: 'John Doe' },
  { id: 3, name: 'Smith' },
  { id: 2, name: 'Jane Doe' }
];

const removeDuplicates = (arr, key) => {
  const seen = new Set();
  return arr.reduce((acc, item) => {
    const keyValue = item[key];
    if (!seen.has(keyValue)) {
      seen.add(keyValue);
      acc.push(item);
    }
    return acc;
  }, []);
};

const uniqueArray = removeDuplicates(arrayWithDuplicates, 'id');

console.log(uniqueArray);

```

#### add params to url
```
https://medium.com/@bobjunior542/how-to-use-usesearchparams-in-react-router-6-for-url-search-parameters-c35b5d1ac01c
```

#### analytics library that works with any analytics service, with single code (same)
```
https://github.com/DavidWells/analytics
```

#### react native/ react web: to force to update/rerender on navigating to a page/component
```javascript
//If you are using React Navigation 5.X, just do the following:

import { useIsFocused } from '@react-navigation/native'

export default function App(){

    const isFocused = useIsFocused()

    useEffect(() => {
        if(isFocused){
            //Update the state you want to be updated
        }
    }, [isFocused])

}

```
#### react native: how to resolve simulator error "cannot boot"
```
https://stackoverflow.com/questions/77177016/xcode-15-unable-to-boot-the-simulator
```
#### react native: scroll to the bottom on every new data
```javascript
//https://stackoverflow.com/questions/29310553/is-it-possible-to-keep-a-scrollview-scrolled-to-the-bottom

import { useRef } from 'react';
import { ScrollView } from 'react-native';

const ScreenComponent = (props) => {
  const scrollViewRef = useRef();
  return (
    <ScrollView
      ref={scrollViewRef}
      onContentSizeChange={() => scrollViewRef.current.scrollToEnd({ animated: true })}
    />
  );
};
```

#### react native: create responsive width, height, font size, etc
```javascript
//https://medium.com/simform-engineering/create-responsive-design-in-react-native-f84522a44365


import { Dimensions } from 'react-native';

const { width, height } = Dimensions.get('window');

const guidelineBaseWidth = 375;
const guidelineBaseHeight = 812;

const horizontalScale = (size) => (width / guidelineBaseWidth) * size;
const verticalScale = (size) => (height / guidelineBaseHeight) * size;
const moderateScale = (size, factor = 0.5) => size + (horizontalScale(size) - size) * factor;

export { horizontalScale, verticalScale, moderateScale };




const styles = StyleSheet.create({
    container: {
        ...,
        height: verticalScale(70),
        width: horizontalScale(150),
        marginTop: verticalScale(100),
    },
    containerText: {
        ...,
        fontSize: moderateScale(18)
    }
});
```

#### react native: generate apk file
```
https://aboutreact.com/generate-debug-android-apk/


// if the bundle apk is old
delete old build first, and then run app normally, before following this
It is because, you have old index.android.bundle file. Follow these steps to update this file and create a new apk:

1. react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res
2. curl "http://localhost:8081/index.bundle?platform=android" -o "android/app/src/main/assets/index.android.bundle"
3. cd android && ./gradlew clean assembleDebug






How to Generate APK?
To generate a debug APK in React Native we need to first bundle our app and then need to build the debug app. But before these two steps, we need to create an index.android for once. Once you create the file then you don’t need to create it again.

1. Create an assets directory using (One Time Step)

mkdir ./android/app/src/main/assets/
2. create a blank file with the name index.android (One Time Step)

touch ./android/app/src/main/assets/index.android
3. bundle the app using

npx react-native bundle --dev false --platform android --entry-file index.js --bundle-output ./android/app/src/main/assets/index.android --assets-dest ./android/app/src/main/res/
react_native_generate_debug_apk1

4. Build the debug APK using

cd android/ && ./gradlew assembleDebug
react_native_generate_debug_apk2react_native_generate_debug_apk3

Once you see Build Success in the log you can find the app in YourProject/android/app/build/outputs/apk/debug/ with the name app-debug.apk

react_native_generate_debug_apk4

This is how to generate debug APK of the Android project in React Native. #1 and#2 is for the one time only, If you have generated the APK for one time then you just need to run #3 and #4.
```
#### add country code to phone number
```javascript
import { parsePhoneNumberFromString, AsYouType } from 'libphonenumber-js';

// Parse phone number string
const phoneNumber = parsePhoneNumberFromString('123456789', 'US');

// Create formatter
const formatter = new AsYouType('US');

// Format phone number without spaces and dashes
const formattedNumber = formatter.input(phoneNumber.number);
console.log(formattedNumber); // Output: +1123456789

```
#### react native: good/best action sheet
```
https://rnas.vercel.app/
```

#### react native: custom shadow for ios, for android, use: elevation:2
```javascript
   shadowOffset={{ width: -2, height: 1 }}
                    shadowColor="#000"
                    shadowOpacity={0.16}
                    shadowRadius={5}
```

#### add two decimal point to int or float number
```
toLocaleString('en', { minimumFractionDigits: 2 })
```

#### react native: set text input from ref
```javascript
   // update note input
  useEffect(() => {
    if (noteInputRef.current) {
      noteInputRef.current.setNativeProps({ text: note || '' });
    }
  }, []);
```

#### remove duplicate object
```javascript
const key = 'place';
const unique = [...new Map(arr.map(item => [item[key], item])).values()]

//It can be put into a function:
function getUniqueListBy(arr, key) {
    return [...new Map(arr.map(item => [item[key], item])).values()]
}
```
#### react native: best side bar/ drawer to use within a component or page

```javascript
import Sidebar from 'react-native-sidebar';

 <Pressable
          onPress={() => {
            setOpen((prev) => !prev);
            open ? sidebarRef.current.close() : sidebarRef.current.open('right/left');
          }}>
        </Pressable>

  <Sidebar
        rightSidebarWidth={150}
        ref={sidebarRef}
        rightSidebar={<RightSidebarContent />}
        overlayColor={'#C1C4CD'}
        style={{ flex: 1, height: '100%' }}>
        {!Array.isArray(children)
          ? React.Children.map(children, (child, index) =>
              React.cloneElement(child, { ...(filteredProducts?.length > 0 ? { products: filteredProducts } : {}) }),
            )
          : children}
      </Sidebar>
```

#### prefetch or cache audio in React Native?
```javascript
//https://www.npmjs.com/package/rn-fetch-blob This will allow you to save a stream to phone storage, give you a path to the file on the 

//An example from their docs.

RNFetchBlob.config({
// add this option that makes response data to be stored as a file,
// this is much more performant.
fileCache : true,
})
.fetch('GET', 'http://www.example.com/file/example.zip', {
  //some headers ..
})
.then((res) => {
  // the temp file path
  console.log('The file saved to ', res.path())
})
```

#### to parse human readable text to date 
```
https://www.npmjs.com/package/any-date-parser#exhaustive-list-of-date-formats
npm i any-date-parser
```

#### react native: scroll to view
```javascript
const lastChatRef = useRef<any>(null);

  const [textPos, setTextPos] = useState(0);

  useEffect(() => {
    setTimeout(() => {
      if (lastChatRef.current) {
        lastChatRef.current?.scrollTo?.({ y: textPos + 1000, animated: true });
      }
    }, 100);
  }, [chatList]);

     <ScrollView ref={lastChatRef} style={{ marginBottom: 20 }}>
          {chatList?.map((chat, index) => (
            <Stack>
              <Text
                {...(index + 1 === chatList?.length
                  ? {
                      onLayout: (event) => {
                        event.target.measure((x, y, width, height, pageX, pageY) => {
                          setTextPos(y + pageY);
                        });
                      },
                    }
                  : {})}
              >
                {chat?.text}
              </Text>
            </Stack>
          ))}
        </ScrollView>
```

#### count down function from by passing an end date
```javascript
function countdown(endDate, callback) {
    let timeoutId; // Variable to store the timeout ID

    // Define the countdown function
    function tick() {
        const now = new Date();
        const difference = endDate - now;

        // Check if the countdown has ended
        if (difference <= 0) {
            callback(); // Call the callback function when the countdown ends
            clearTimeout(timeoutId); // Clear the timeout
            return; // Exit the function
        }

        // Calculate the remaining time in milliseconds
        const seconds = Math.floor((difference / 1000) % 60);
        const minutes = Math.floor((difference / 1000 / 60) % 60);
        const hours = Math.floor((difference / (1000 * 60 * 60)) % 24);
        const days = Math.floor(difference / (1000 * 60 * 60 * 24));

        // Output the remaining time (optional)
        console.log(`${days} days, ${hours} hours, ${minutes} minutes, ${seconds} seconds`);

        // Wait for one second and call the tick function recursively
        timeoutId = setTimeout(tick, 1000);
    }

    // Start the countdown
    tick();
}

// Example usage:
const endDate = new Date(Date.now() + 5000); // Set the end date 5 seconds from now
countdown(endDate, () => {
    console.log("Countdown has ended!"); // This callback will be called when the countdown ends
});
```

#### regex expression to extract date, eg: 1-5 weeks, 1-12 months, 1-5 years,1-30 days, today, and tomorrow, from string
```javascript
  const inputString = "I plan to complete the task in 2 week";
  const regexPattern =
    /\b((?:(?:1|2|3|4|5)-?[1-4]?\s(?:week|weeks))|(?:1-[1-9]|1[0-2])\s(?:month|months?)|(?:(?:1|2)?[0-9]|30)\sday(?:s)?|(?:(?:1|2)?[0-9]|3[0-1])\shour(?:s)?|(?:(?:1-5)?[0-9]|60)\sminute(?:s)?|1-[1-5]?\s(?:year|years?)|today|tomorrow)\b/g;

  const matches = inputString.match(regexPattern);

  if (matches) {
    console.log("Matches found:");
    matches.forEach((match) => console.log(match));
  } else {
    console.log("No matches found.");
  }
```

#### fix: contentible div(input) cursor caret moves to the begining of the text box, when text is updated (state)
```javascript
    contentEditableDiv.addEventListener("input", function (event) {
   const updatedValue = contentEditableDiv.textContent;

     const position = updatedValue?.length;

          const el = contentEditableDiv;
          const selection = document.getSelection();

          if (!selection || !el) return;

          // Set the caret to the beggining
          selection.collapse(el, 0);

          // Move the caret to the position
          for (let index = 0; index < position; index++) {
            selection.modify("move", "forward", "character");
          }
      });
```


#### change focus of input to another input in an input list array
```javascript
  const handleNew = () => {
    setState((prev) => [...prev, { time: Date.now() }]);

    setTimeout(() => {
      const inputElement = document.querySelector(`[data-id="1"]`);

      if (inputElement) {
        inputElement.focus();
      }
    }, 200);
  };


<Input
    placeContent={"Enter Text"}
    onChange={(e) =>
      setState((prev) => [
        ...prev?.map((d, inx) => {
          if (inx === index) {
            return { ...d, content: e.target.value };
          }
          return d;
        }),
      ])
    }
// add data id with value only to the last input, so we can focus on it
    {...(index + 1 === state.length ? { "data-id": "1" } : {})}
    ref={inputRef}
    value={list?.content}
    onKeyDown={(e) => e.key === "Enter" && handleNew()}
  />
```

#### get updated value of html element with contentible="true"
```javascript
  useEffect(() => {
    setTimeout(() => {
      const contentEditableDiv = document.getElementById(
        `fake_textarea${index}`
      );

      // Add event listener for input event
      contentEditableDiv.addEventListener("input", function () {
        // Get the updated value using textContent
        const updatedValue = contentEditableDiv.textContent;
        console.log(updatedValue);
      });
    }, 1000);
  }, []);


 <Box
	id={`fake_textarea${index}`}
	contenteditable="true"
    >
      {list?.content}
    </Box>
```

#### import react component into a different web app
```
https://javascript.plainenglish.io/how-to-share-and-reuse-react-components-to-build-apps-faster-4b3b0b542798#:~:text=We%20can%20use%20the%20bit,That's%20it.
```

#### filter/ compare data from another array
```javascript
//filter product, where you dont want each pro in array2
  products={data?.findProducts?.data?.filter(
		(product) => !selectedProducts?.some((sProduct) => product?._id === sProduct?._id),
  )}
```

#### debounce input onChange from rerendering the whole component, for every character added
```javascript
import {  useDebouncedCallback } from 'use-debounce';

 const debounced = useDebouncedCallback(
          (name, value) => {
            setFieldValue(name, value, true);
          },
          // delay in ms
          2000,
        );

//do not add value to input prop
  onChangeText={(value: any) => {
    debounced('salesChannel', value);
  }}
```

#### get all state and cities/city in Nigeria
```
https://github.com/chukaofili/states-cities-db
yarn add states-cities-db

```
#### get country, state and city in the world
```
npm i react-country-state-city
```

#### React google search location, to get logitude and latitude and google location

##### if you're putting it in a modal, these zIndex must be present, else, it would come out, at the back of the modal
```javascript
 <Modal
	zIndex="1051 !important"
	>
        <ModalOverlay zIndex="1051 !important" />
        <style>
          {`
            .chakra-modal__content-container{
              z-index: 1051 !important;
            }
            `}
        </style>
        <ModalContent bg={bg || "#fff"} zIndex="1051 !important">
          <ModalBody zIndex="1051 !important" mb="10px">
            {childrenWithProps}
          </ModalBody>
        </ModalContent>
      </Modal>
```

```javascript
import { usePlacesWidget } from "react-google-autocomplete";

  const placesApiKey = import.meta.env.VITE_GOOGLE_PLACES_API_KEY;

  const { ref } = usePlacesWidget({
    apiKey: placesApiKey,
    onPlaceSelected: async (places) => {
      const formatted_address = places?.formatted_address;
      const { lat, lng } = places?.geometry?.location;
      setGeo({ lat: lat(), lng: lng() });
      setSelectedLocation(formatted_address);
    },
    options: {
      //types: ["(regions)"],
	 types: ["geocode", "establishment"],
      componentRestrictions: { country: "ng" },
    },
  });

 return (
    <div style={{ width: "100%" }} className={`search-location-input${label}`}>
// this z-index is very important
      <style>
        {`
          .pac-container {
            z-index: 1051 !important;
        }
          `}
      </style>
        <Input
          ref={ref}
          placeholder="Search Places ..."
        />
    </div>
  );
```


#### how to get props of a child component
```javascript
  let r = Children?.map(props?.children, (child) => child?.props);
  console.log(r?.[0]?.products, '))111');
```

#### add commas to sting price value
```javascript
let text = 
 onChangeText={(text) => setPrice(text?.replace(',', '').replace(/\B(?=(\d{3})+\b)/g, ','))}
//text?.replace(',', '') replace the old comma string with nothing, so as not to get comma in the wrong place
//.replace(/\B(?=(\d{3})+\b)/g, ',') // add comma into every last 3 digits
```

#### you want to convert item value in an object to integer base on the key list
```javascript
const itemsToConvert = { "price": true };

const data = {
  "price": "10",
  "quantity": "5",
  "value": "Red"
};

const convertValuesToInteger = (object, keys) => {
  const convertedObject = { ...object };

  for (const key in convertedObject) {
    if (keys[key]) {
      convertedObject[key] = parseInt(convertedObject[key], 10);
    }
  }

  return convertedObject;
};

const newData = convertValuesToInteger(data, itemsToConvert);
console.log(newData);
```


#### reduce/reformat data
```javascript
const data = [
  { "Length": { "price": "2", "quantity": "5", "value": "1cm" } },
  { "Length": { "price": "2", "quantity": "5", "value": "2cm" } },
  { "Length": { "price": "4", "quantity": "4", "value": "3 cm" } }
];

const reducedData = data.reduce((acc, item) => {
  const key = Object.keys(item)[0]; // Get the key of the item (e.g., "Length")
  if (!acc[key]) {
    acc[key] = []; // Initialize array if it doesn't exist
  }
  acc[key].push(item[key]); // Push the value of the key to the corresponding array
  return acc;
}, {});

console.log(reducedData);

// returns
{
  Length: [
    { price: "2", quantity: "5", value: "1cm" },
    { price: "2", quantity: "5", value: "2cm" },
    { price: "4", quantity: "4", value: "3 cm" }
  ]
}
```

#### git clone fail:RPC failed; curl 18 transfer closed with outstanding read data remaining
```
git clone http://github.com/large-repository --depth 1

https://stackoverflow.com/questions/38618885/error-rpc-failed-curl-transfer-closed-with-outstanding-read-data-remaining

```

#### best react native keyboard aware scroll view
```javascript
react-native-keyboard-aware-scroll-view npm


//to stop moving to top when typing, add this prop
//enableResetScrollToCoords={false}




```

#### fix github, different commit histories
```
git merge develop --allow-unrelated-histories
```

####  how to remove large file from local .git/cache git
#####  add the folder location => remove every jar extension it the folder
```javascript
//get list of huge files, default list length is 10, can be increased
git rev-list --objects --all | grep -f <(git verify-pack -v  .git/objects/pack/*.idx| sort -k 3 -n | cut -f 1 -d " " | tail -10)

git filter-branch --index-filter 'git rm --cached --ignore-unmatch android/gradle/caches/jars-9/*jar' --tag-name-filter cat -- --all
```
##### remove the folder and every file/folder inside it
```javascript
git filter-branch --index-filter 'git rm -r --cached --ignore-unmatch android/gradle/caches/jars-9' --tag-name-filter cat -- --all
```
##### Once this is done run the following commands to clean up the local repository:
```javascript
rm -rf .git/refs/original/
git reflog expire --expire=now --all
git gc --prune=now
git gc --aggressive --prune=now
```
##### Now push all the changes to the remote repository:
```javascript
git push --all --force
```

#### how to remove/clear all git commit history
```
https://xebia.com/blog/deleting-your-commit-history/
```


#### fix react native path issue: ReactNative can't locate Android SDK
```
https://stackoverflow.com/questions/62797240/reactnative-cant-locate-android-sdk
https://kashanhaider.com/set-up-android-environment-variables-on-macos/
```

#### git error: RPC failed; curl transfer closed with outstanding read data remaining
```
git clone https://github.com/Zillight-Innovation-Labs/fixmeet.git --depth 1
https://stackoverflow.com/questions/38618885/error-rpc-failed-curl-transfer-closed-with-outstanding-read-data-remaining
```


#### create invoice with html and jspdf (screen shot and save as pdf)
```javascript
import { jsPDF } from "jspdf";

const download = () => {
    var doc = new jsPDF();

    // Source HTMLElement or a string containing HTML.
    var elementHTML = document.querySelector("#pdf-container");

    doc.html(elementHTML, {
      callback: function (doc) {
        // Save the PDF
        doc.save("sample-document.pdf");
      },
      x: 15,
      y: 15,
      width: 170, //target width in the PDF document
      windowWidth: 650, //window width in CSS pixels
    });
  };
```

#### fix SSH git issue
```css
https://dev.classmethod.jp/articles/fix-gitgithub-com-permission-denied-publickey-fatal-could-not-read-from-remote-repository/
```

#### React native: how to render svg icons
```javascript
import { createIcon } from 'native-base';
import { Path, G } from 'react-native-svg';


export const Email = (props: any) => {
  let F = createIcon({
    viewBox: '0 0 24 24',

    path: (
      <G>
        <Path
          d="M21 16.2222V5H3V16.2222C3 17.7563 4.24365 19 5.77778 19H18.2222C19.7563 19 21 17.7563 21 16.2222Z"
          stroke="#9EA5AD"
          stroke-width="1.85185"
          stroke-linecap="round"
          {...props}
        />
        <Path
          d="M21 7L13.7783 13.0181C12.7482 13.8765 11.2518 13.8765 10.2217 13.0181L3 7"
          stroke="#9EA5AD"
          stroke-width="1.85185"
          stroke-linejoin="round"
          {...props}
        />
      </G>
    ),
  });
  return <F />;
};

);
```

#### how to get element width and height without padding, margin , border box
```javascript
const innerDimensions = (node) => {
    var computedStyle = getComputedStyle(node);

    let width = node.clientWidth; // width with padding
    let height = node.clientHeight; // height with padding

    height -=
      parseFloat(computedStyle.paddingTop) +
      parseFloat(computedStyle.paddingBottom);
    width -=
      parseFloat(computedStyle.paddingLeft) +
      parseFloat(computedStyle.paddingRight);
    return { height, width };
  };

  useEffect(() => {
    setTimeout(() => {
      const dimension = innerDimensions(document.getElementById("x"));
      setW(dimension.width);
    }, 500);
  }, []);
```

#### filter by start date and end date
```javascript
    const filterByDate = DBdata?.filter((data) => {
      const dataDate = data?.startDate || data?.date;
      const check1 =
        // month
        parseInt(dayjs(dataDate).format("M")) >=
          new Date(fromDate).getMonth() + 1 &&
        // day
        parseInt(dayjs(dataDate).format("D")) >= new Date(fromDate).getDate() &&
        // year
        parseInt(dayjs(dataDate).format("YYYY")) ===
          new Date(fromDate).getFullYear();

      const check2 =
        // month
        parseInt(dayjs(dataDate).format("M")) <=
          new Date(toDate).getMonth() + 1 &&
        // day
        parseInt(dayjs(dataDate).format("D")) <= new Date(toDate).getDate() &&
        // year
        parseInt(dayjs(dataDate).format("YYYY")) ===
          new Date(toDate).getFullYear();
      return check1 && check2;
    });
```
#### add custom drop down arrow on select element
```javascript
  let dropDownSvg = `<svg width="18" height="10" viewBox="0 0 18 10" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path d="M7.79943 9.59891C8.51077 10.1328 9.48923 10.1328 10.2006 9.59891L17.1945 4.34957C18.7314 3.19603 17.9156 0.75 15.9939 0.75H2.00606C0.0844002 0.75 -0.731424 3.19603 0.805498 4.34957L7.79943 9.59891Z" fill="#F98614"/>
    </svg>`;

  let converToBase64 = window.btoa(dropDownSvg);

 <style>
          {`
                select {
                    background: url(data:image/svg+xml;base64,${converToBase64}) no-repeat right #ddd !important;
                    background-position: calc(100% - 0.75rem) center !important;
                    -moz-appearance:none; /* Firefox */
                    -webkit-appearance:none; /* Safari and Chrome */
                    appearance:none;
                    
                  }
                `}
        </style>

```


#### React native: install cocoapods with brew and do pod install
```
I love u all!
I had the same issue on Mac, installing a react-native 0.60 and node 10.24.1 project builded and published on 2019, and nobody made maintenance...
be sure to install cocoa pods
$ gem uninstall cocoapods
$ gem install cocoapods
And then you can install in ios project folder with pod install



In my case problem was about cocoapods. I removed old versions,installed from ground and linked, problem solved :)

brew uninstall --cask cocoapods

brew install cocoapods

brew link --overwrite cocoapods
```

#### loop call function(s), x number of times quietly, not greater than the max tries
```javascript
 let count = 0;
let maxTries = 3;

while (true) {
  try {
    leave();
    // if the function above works, then break out of loop, else try again
    break;
  } catch {
    if (++count === maxTries) break;
  }
}
```


#### React native: border radius
```javascript
 <Flex
  mt={5}
  borderTopLeftRadius={20}
  borderTopRightRadius={20}
  borderBottomLeftRadius={20}
  borderBottomRightRadius={20}
  backgroundColor={"#F2F9F9"}
  borderWidth={1}
  borderColor={"#D6EBEB"}
  p={10}
>
```

#### React native: access navigation, to go back or to a different route
```javascript
// it is automatically in the props
function HomeScreen({ navigation }) {...

 <Button
  onPress={() => navigation.navigate("MainStack", {})}
>...

```

#### React native: safe screen area, so text don't go off the top of the screen
```javascript
import { useSafeAreaInsets } from "react-native-safe-area-context";

  const insets = useSafeAreaInsets();
 <View
      style={{
        paddingTop: insets.top,
        paddingBottom: insets.bottom,
        paddingLeft: insets.left,
        paddingRight: insets.right,
        display: "flex",
        justifyContent: "space-evenly",
        alignItems: "center",
        flex: 1,
      }}
    ></View>
```

#### React native: load fonts
```javascript
import { useFonts } from "expo-font";

const [fontsLoaded] = useFonts({
    "brevia-bold": require("./assets/font/BreviaBold.otf"),
    "brevia-semi-bold": require("./assets/font/BreviaSemiBold.otf"),
    "brevia-extra-bold": require("./assets/font/BreviaExtraBold.otf"),
    "brevia-normal-bold": require("./assets/font/BreviaNormalBold.otf"),
    "brevia-regular": require("./assets/font/BreviaRegular.otf"),
  });

  if (!fontsLoaded) {
    console.log("unloaded");

    return null;
  }

  const theme = extendTheme({
    fontConfig: {
      "brevia-bold": {
        100: {
          normal: "brevia-bold",
          bold: "brevia-bold",
        },
        200: {
          normal: "brevia-bold",
          bold: "brevia-bold",
        },
        300: {
          normal: "brevia-bold",
          bold: "brevia-bold",
        },
        400: {
          normal: "brevia-bold",
          bold: "brevia-bold",
        },
	 500: {
          normal: "brevia-bold",
          bold: "brevia-bold",
        },
        600: {
          normal: "brevia-bold",
          bold: "brevia-bold",
        },
      },
    },

    // Make sure values below matches any of the keys in `fontConfig`
    fonts: {
      heading: "brevia-bold",
      body: "brevia-bold",
      mono: "brevia-bold",
    },
  });
  <NativeBaseProvider theme={theme}></NativeBaseProvider>
```

#### React native: tab navigation
```javascript
A simple tab bar on the bottom of the screen that lets you switch between different routes. Routes are lazily initialized -- their screen components are not mounted until they are first focused.
https://reactnavigation.org/docs/bottom-tab-navigator


  const Tab = createBottomTabNavigator();
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";

<Tab.Navigator
      initialRouteName="Main"
      screenOptions={({ route }) => ({
        tabBarIcon: ({ focused, color, size }) => {
          if (route.name === "Main") {
            return (
              <Ionicons
                name={focused ? "md-home-sharp" : "home-outline"}
                size={size}
                color={"#fff"}
              />
            );
          } else if (route.name === "Settings") {
            return (
              <Ionicons
                name={focused ? "settings-sharp" : "settings-outline"}
                size={size}
                color={"#fff"}
              />
            );
          } else if (route.name === "Profile") {
            return (
              <FontAwesome5
                name={focused ? "user-alt" : "user"}
                size={22}
                color={"#fff"}
              />
            );
          }

          return <Ionicons name={"null"} size={size} color={"#fff"} />;
        },

	tabBarStyle: {
          position: "absolute",
          backgroundColor: "#000",

          elevation: 0,
          marginBottom: 20,
          padding: 10,
          borderRadius: 40,
          height: 80,
          width: "70%",
          left: 50,
          right: 50,
        },
      })}
    >
      <Tab.Screen
        name="Main"
        component={MainScreen}
        options={{
          tabBarLabel: "",
          headerShown: false,
        }}
      />
	<Tab.Screen
        name="Settings"
        component={SettingsScreen}
        // options={{
        //   tabBarLabel: "",
        //   headerShown: false,
        // }}

        options={({ navigation }) => ({
          // headerTitle: (props) => null,
          tabBarLabel: "",

          headerShadowVisible: false,
          headerLeft: () => (
            <Pressable onPress={() => navigation.goBack()}>
              <Flex
                w={39}
                h={39}
                bg={"#000"}
                alignItems="center"
                justifyContent={"center"}
                borderRadius={50}
                ml={1}
              >
                <SvgXml xml={_CHEVRON_LEFT_ICON} color="#e4e4e4" />
              </Flex>
            </Pressable>
          ),
        })}
      />
	<Tab.Screen
        name="Profile"
        component={ProfileScreen}
        options={({ navigation }) => ({
          tabBarLabel: "",
          headerTitle: () => null,
          headerShadowVisible: false,
          headerStyle: {
            backgroundColor: "#ecd853",
          },

          headerLeft: () => (
            <Pressable onPress={() => navigation.goBack()}>
              <BlackCircle noBorder>
                <SvgXml xml={_CHEVRON_LEFT_ICON} color="#e5e5e5" />
              </BlackCircle>
            </Pressable>
          ),
        })}
      />
    </Tab.Navigator>
```

#### React native: stack navigation
```javascript
Stack Navigator provides a way for your app to transition between screens where each new screen is placed on top of a stack.
https://reactnavigation.org/docs/stack-navigator

import { NavigationContainer } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";
const Stack = createNativeStackNavigator();

 <NativeBaseProvider theme={theme}>
 <NavigationContainer>
        <Stack.Navigator>
          <Stack.Screen
            name="Home"
            component={HomeScreen}
            options={{ headerShown: false }}
          />
          <Stack.Screen
            name="MainStack"
            options={{ headerShown: false }}
            component={MainStack}
          />

          <Stack.Screen
            name="Playing"
            component={PlayingScreen}
            options={({ navigation }) => ({
              headerTitle: (props) => null,
              headerShadowVisible: false,
              headerLeft: () => (
                <Pressable onPress={() => navigation.goBack()}>
                  <Flex
                    w={39}
                    h={39}
                    bg={"#000"}
                    alignItems="center"
                    justifyContent={"center"}
                    borderRadius={50}
                  >
                    <SvgXml xml={_CHEVRON_LEFT_ICON} color="#e4e4e4" />
                  </Flex>
                </Pressable>
              ),
            })}
          />
 	</Stack.Navigator>
      </NavigationContainer>
    </NativeBaseProvider>
```


#### React native: Load svg image
```javascript
import { SvgXml } from "react-native-svg";

  const _CHEVRON_LEFT_ICON = `
  <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M16.2426 6.34317L14.8284 4.92896L7.75739 12L14.8285 19.0711L16.2427 17.6569L10.5858 12L16.2426 6.34317Z" fill="currentColor" /></svg>


  <SvgXml xml={_CHEVRON_LEFT_ICON} color="#e4e4e4" />
  `;


```
#### React native: tab navigation
```javascript
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";

const Tab = createBottomTabNavigator();

 <Tab.Navigator
      initialRouteName="Main"
      screenOptions={({ route }) => ({
        tabBarIcon: ({ focused, color, size }) => {
          if (route.name === "Main") {
            return (
              <Ionicons
                name={focused ? "md-home-sharp" : "home-outline"}
                size={size}
                color={"#fff"}
              />
            );
          } else if (route.name === "Settings") {
            return (
              <Ionicons
                name={focused ? "settings-sharp" : "settings-outline"}
                size={size}
                color={"#fff"}
              />
            );
          } else if (route.name === "Profile") {
            return (
              <FontAwesome5
                name={focused ? "user-alt" : "user"}
                size={22}
                color={"#fff"}
              />
            );
          }

          return <Ionicons name={"null"} size={size} color={"#fff"} />;
        },

        tabBarStyle: {
          position: "absolute",
          backgroundColor: "#000",

          elevation: 0,
          marginBottom: 20,
          padding: 10,
          borderRadius: 40,
          height: 80,
          width: "70%",
          left: 50,
          right: 50,
        },
      })}
    >
      <Tab.Screen
        name="Main"
        component={MainScreen}
        options={{
          tabBarLabel: "",
          headerShown: false,
        }}
      />

      <Tab.Screen
        name="Settings"
        component={SettingsScreen}
        // options={{
        //   tabBarLabel: "",
        //   headerShown: false,
        // }}

        options={({ navigation }) => ({
          // headerTitle: (props) => null,
// dont add tab label
          tabBarLabel: "",

          headerShadowVisible: false,
          headerLeft: () => (
            <Pressable onPress={() => navigation.goBack()}>
              <Flex
                w={39}
                h={39}
                bg={"#000"}
                alignItems="center"
                justifyContent={"center"}
                borderRadius={50}
                ml={1}
              >
                <SvgXml xml={_CHEVRON_LEFT_ICON} color="#e4e4e4" />
              </Flex>
            </Pressable>
          ),
        })}
      />
      <Tab.Screen
        name="Profile"
        component={ProfileScreen}
        options={({ navigation }) => ({
          tabBarLabel: "",
          headerTitle: () => null,
          headerShadowVisible: false,
          headerStyle: {
            backgroundColor: "#ecd853",
          },

          headerLeft: () => (
            <Pressable onPress={() => navigation.goBack()}>
              <BlackCircle noBorder>
                <SvgXml xml={_CHEVRON_LEFT_ICON} color="#e5e5e5" />
              </BlackCircle>
            </Pressable>
          ),
        })}
      />
    </Tab.Navigator>
```

#### React native: load lottie animation
```javascript
import LottieAnimate from "./LottieAnimate";

 <LottieAnimate
  play={startedPlaying}
  width={400}
  height={400}
  source={require("../assets/rotate-circle.json")}
/>



import LottieView from "lottie-react-native";
import { memo, useEffect, useRef } from "react";
import { Animated, Easing } from "react-native";

function LottieAnimate({ play = null, source, ...props }) {
  const animationProgress = useRef(new Animated.Value(0));
  const AnimatedLottieView = Animated.createAnimatedComponent(LottieView);

  useEffect(() => {
    if (play === false) {
      Animated.loop(
        Animated.timing(animationProgress.current, {
          toValue: 1,
          duration: 5000,
          easing: Easing.linear,
          useNativeDriver: false,
        })
      ).stop();
    } else if (play === true) {
      Animated.loop(
        Animated.timing(animationProgress.current, {
          toValue: 1,
          duration: 5000,
          easing: Easing.linear,
          useNativeDriver: false,
        })
      ).start();
    } else {
      Animated.loop(
        Animated.timing(animationProgress.current, {
          toValue: 1,
          duration: 5000,
          easing: Easing.linear,
          useNativeDriver: false,
        })
      ).start();
    }
  }, [play]);
  return (
    <AnimatedLottieView
      loop
      style={{ width: 500, height: 500, ...props }}
      source={source}
      progress={animationProgress.current}
    />
  );
}

export default memo(LottieAnimate);
```

#### expo project quick start
```javascript
 npx create-expo-app my-app

npm run andriod/ios/web
```

#### custom ionic react tab
```javascript
<IonTabButton tab="tab1" href="/tab1">
            <IonIcon icon={home} className="selected"></IonIcon>
            <IonIcon icon={homeOutline} className="unselected"></IonIcon>
            <IonLabel>Home</IonLabel>
</IonTabButton>


.tab-selected .unselected {
  display: none;
}

.tab-selected .selected {
  display: initial !important;
}

.selected {
  display: none;
}
```

#### business logic to check if it's the scheduled date and also check if the scheduled date/time has elapsed/passed
```javascript
//checking date and time, if it is the scheduled date
  const isScheduledDate = () => {
    if (
      new Date().setHours(0, 0, 0, 0) ===
        new Date(scheduledDate).setHours(0, 0, 0, 0) &&
      dayjs().format("HH:mm") >= dayjs(scheduledDate).format("HH:mm")
    ) {
      return true;
    }
    return false;
  };

// check if the scheduled date/time has elapsed/passed, by checking the current time against the end date
const scheduleDateHasPassed = () => {
    if (
      new Date(scheduleEndDate).setHours(0, 0, 0, 0) >
      new Date().setHours(0, 0, 0, 0)
    ) {
      return false;
    } else if (
      new Date().setHours(0, 0, 0, 0) <
        new Date(scheduleEndDate).setHours(0, 0, 0, 0) ||
      new Date().setHours(0, 0, 0, 0) !==
        new Date(scheduleEndDate).setHours(0, 0, 0, 0) ||
      dayjs().format("HH:mm") > dayjs(scheduleEndDate).format("HH:mm")
    ) {
      return true;
    } else {
      return false;
    }
  };
```

#### show changing/updating time, in real time
```javascript
const timeElement = document.getElementById("clock");

function updateTime() {
    const now = new Date();
    const hours = now.getHours();
    const minutes = now.getMinutes();
    const seconds = now.getSeconds();

    // Format the string with leading zeroes
    const clockStr = `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;

    timeElement.innerText = clockStr;
}

updateTime();
// 1000 is 1sec, 60,000 is 60 sec = 1 minute
setInterval(updateTime, 1000);
```

#### auto play video, with/without controls
```javascript
  <video
    className="video-showcase"
    controls
    autoPlay
    loop
    muted
    style={{
      width: "1000px",
      height: "700px",
      borderRadius: "10px",
    }}
  >
            <source
              src="https://www.dropbox.com/scl/fi/kzqw537enawc15lbsozz8/Sample-Videos-_-Dummy-Videos-For-Demo-Use.mp4?raw=1&rlkey=otro2x2e8s6sgd37keod9hme2&dl=0&loop=1&autoplay=1&mute=1"
              type="video/mp4"
            />
          </video>
```


#### Embed a dropbox video, using embed code
```css
https://support.whatfix.com/docs/embedding-a-dropbox-video-using-embed-code

Helpful, but what worked for me was to leave text after the ? and add ?raw=1
```

#### login, register/sign up, forgot/reset password code, for Firebase

##### frontend code
```javascript
  const [formValues, setFormValues] = useState({});
  const { fullName, email, password, confirmPassword } = formValues || {};

  const handleSubmit = () => {
    switch (show) {
      case "SIGN_IN":
        if (!email || !password) {
          errorNotifier("Some fields are empty");
          return;
        }

        logInWithEmailAndPassword(email, password)
          .then(() => {
            goToNext && goToNext();
            successNotifier("Successfully logged in");
            setFormValues({});
          })
          .catch((er) => errorNotifier(er.message));

        break;

      case "REGISTER":
        if (!email || !password || !fullName) {
          errorNotifier("Some fields are empty");
          return;
        }
        if (password !== confirmPassword) {
          errorNotifier("Password doen't match");
          return;
        }

        registerWithEmailAndPassword(fullName, email, password)
          .then(() => {
            goToNext && goToNext();
            successNotifier("Successfully Registered");
            // if user didnt add gotonext prop, then redirect to signin screen
            !goToNext && setShow("SIGN_IN");
          })
          .catch((er) => errorNotifier(er.message));
        break;

      case "RESET_PASSWORD":
        if (!email) {
          errorNotifier("Some fields are empty");
          return;
        }

        sendPasswordReset(email);

        break;
      default:
        break;
    }
  };
```
##### backend code
```javascript
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";

import {
  GoogleAuthProvider,
  getAuth,
  signInWithPopup,
  signInWithEmailAndPassword,
  createUserWithEmailAndPassword,
  sendPasswordResetEmail,
  signOut,
  updateProfile,
} from "firebase/auth";
import { getFirestore } from "firebase/firestore";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: process.env.REACT_APP_API_KEY,
  authDomain: process.env.REACT_APP_AUTH_DOMAIN,
  projectId: process.env.REACT_APP_PROJECT_ID,
  storageBucket: process.env.REACT_APP_STORAGE_BUCKET,
  messagingSenderId: process.env.REACT_APP_MESSAGING_SENDER_ID,
  appId: process.env.REACT_APP_APP_ID,
  measurementId: process.env.REACT_APP_MEASUREMENT_ID,
};
// Initialize Firebase
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);

const googleProvider = new GoogleAuthProvider();
const signInWithGoogle = async () => {
  try {
    await signInWithPopup(auth, googleProvider);
  } catch (err) {
    throw new Error(err.message);
  }
};

const logInWithEmailAndPassword = async (email, password) => {
  try {
    await signInWithEmailAndPassword(auth, email, password);
  } catch (err) {
    const readableCode = err.code?.split("/")?.[1]?.split("-")?.join(" ");

    throw new Error(readableCode);
  }
};
const registerWithEmailAndPassword = async (name, email, password) => {
  try {
    createUserWithEmailAndPassword(auth, email, password).then((userCred) => {
      updateProfile(userCred.user, { displayName: name });
    });
  } catch (err) {
    const readableCode = err.code?.split("/")?.[1]?.split("-")?.join(" ");

    throw new Error(readableCode);
  }
};

const sendPasswordReset = async (email) => {
  try {
    await sendPasswordResetEmail(auth, email);
    alert("Password reset link sent!");
  } catch (err) {
    const readableCode = err.code?.split("/")?.[1]?.split("-")?.join(" ");

    throw new Error(readableCode);
  }
};
const logout = () => {
  signOut(auth);
};
```

#### linear gradient on text
```css
   background="linear-gradient(90deg, rgba(2,0,36,1) 0%, rgba(115,230,202,1) 58%)"
    backgroundClip="text"
   textFillColor="transparent"
```

#### scroll down to bottom of div when text/chat/<p/> is added
```javascript
  const [chatBoxRef, setChatBoxRef] = useState(null);

  useEffect(() => {
    if (chatBoxRef) {
      chatBoxRef.scrollTop = chatBoxRef.scrollHeight;
    }
  }, [messages, chatBoxRef]);


// for the message boxes
  let messagesEnd = useRef(null);

  const scrollToBottom = () => {
    messagesEnd?.current?.scrollIntoView({ behavior: "smooth", block: "end" });
  };
  useEffect(() => {
    scrollToBottom();
    //eslint-disable-next-line
  }, [chatBoxRef, updateScroll]);
//end of for messages box
<Stack
    ref={(ref) => ref && setChatBoxRef(ref)}
    width="100%"
    pb={5}
    maxH={"55vh"}
    overflow="auto"
  >
 {messages?.map((message) => (<Flex ref={messagesEnd}>)}

</Flex>
...
</Stack>
```

#### cache website resources
```javascript
implementing a service worker for caching website resources, enabling offline functionality and improved performance for returning visitors

https://web.facebook.com/photo/?fbid=323366160542162&set=gm.7144751595576025&idorvanity=411741268877125
```

#### how to remove empty object values from object list
```javascript
//{"color": {}, "qwert": {"ed": "e4"}}

 function removeEmpty(obj) {
    return Object.entries(obj).reduce((acc, [key, val]) => {
      console.log(val, 'P');
      if (Object.keys(val).length > 0) acc[key] = val;
      return acc;
    }, {});
  }
  console.log(removeEmpty(variants), '***');
// result: {"qwert": {"ed": "e4"}}
```

#### you can map/loop through object
```javascript

for (let x in {one: 1, two: 2) {
      console.log(x); // one, two
    }
```

#### Add one month to date to get the future end date (subscription)
```javascript
import { addMonths } from "https://cdn.jsdelivr.net/npm/date-fns/+esm";

  const FutureSubscriptionEndDate = addMonths(new Date(created_at),1);
```

#### to convert data url to image
```javascript
 const imgUrl = `data:image/png;base64, ${preview}`;

  <Image src={previewImage} loading="lazy" alt="preview" />

```

#### base64 string to image file
```javascript

  var reader = new FileReader();
  reader.onload = function (f) {
    var data = f.target.result;
    setPreviewImage(data);
  };
  reader.readAsDataURL(base64Url);


```


#### How to get browser width, when user resizes his/her browser
```javascript
  const [windowSize, setWindowSize] = useState<number | null>(null);

  // to get browser width, when user resizes his/her browser
  useEffect(() => {
    addEventListener("resize", (event: UIEvent) => {
      const size = event?.target as Window;
      setWindowSize(size.outerWidth);
    });
  }, []);
```

#### How to create custom element width length
```css
//also apply to custom height

    width: 21px;
    background-clip: content-box;

    border-left: 5px solid black;// the height, remove to only have width
    border-image: linear-gradient(to right, #000 50%, rgba(0,0,0,0) 50%); // to (right/top), then increase/reduce 50%
    border-image-slice: 1;
```
#### How to create a custom progress pie round border with custom fill background color
```javascript

import {
  RadialBar,
  PolarAngleAxis,
  RadialBarChart,
  ResponsiveContainer,
} from "recharts";
import { BiBowlRice } from "react-icons/bi";

 const data = [{ name: "L1", value: 80 }];

  const circleSize = 300;

  const scale = ".3";

  return (
    <>
      <style>
        {`
    .container {
      position: relative;
      padding: 0px;
      height: ${circleSize};
      width: ${circleSize};
    }
    .circle {
      display: flex;
      justify-content: center;
      align-items: center;
      font-family: sans-serif;
      color: #fff;
      height: 100%;
      width: 100%;
      border-radius: 50%;
      background: #F4E9CD;
    }

    `}
      </style>
      <ResponsiveContainer width="100%" height="100%" style={{ scale }}>
        <div className="container">
          <BiBowlRice
            fontSize="4em"
            fill="#C89104"
            style={{
              position: "absolute",
              top: 0,
              left: 0,
              right: 0,
              bottom: 0,
              margin: "auto",
              zIndex: 1,
            }}
          />

          <div
            className="circle"
            style={{
              position: "absolute",
              top: 0,
              left: 0,
              right: 0,
              bottom: 0,
              width: "189px",
              height: "170px",
              margin: "auto",
            }}
          ></div>

          <RadialBarChart
            width={circleSize}
            height={circleSize}
            cx={circleSize / 2}
            cy={circleSize / 2}
            innerRadius={130}
            outerRadius={88}
            barSize={19}
            data={data}
            startAngle={90}
            endAngle={-270}
          >
            <PolarAngleAxis
              type="number"
              domain={[0, 100]}
              angleAxisId={0}
              tick={false}
            />
            <RadialBar
              // background
              // clockWise={true}
              dataKey="value"
              cornerRadius={circleSize / 2}
              fill="#C89104"
            />
          </RadialBarChart>
        </div>
      </ResponsiveContainer>
    </>
  );

```

#### How to create a progress pie round border with Rechart
```javascript

  const data = [{ name: "L1", value: 49 }];
  const circleSize = 200;

  return (
    <>
      <style>
        {`
    .container {
      position: relative;
      padding: 30px;
      height: 200px;
      width: 200px;
    }
    .circle {
      display: flex;
      justify-content: center;
      align-items: center;
      font-family: sans-serif;
      color: #fff;
      height: 100%;
      width: 100%;
      border-radius: 50%;
      background: radial-gradient(ellipse at     center,
        rgba(255,113,12,0) 60%,
        #0466C866 51.5%);
    }

    `}
      </style>
      <ResponsiveContainer width="100%" height="100%">
        <div className="container">
          <div className="circle">
            {/* Ello World */}

            <RadialBarChart
              width={circleSize}
              height={circleSize}
              cx={circleSize / 2}
              cy={circleSize / 2}
              innerRadius={100}
              outerRadius={80}
              barSize={18}
              data={data}
              startAngle={90}
              endAngle={-270}
            >
              <PolarAngleAxis
                type="number"
                domain={[0, 100]}
                angleAxisId={0}
                tick={false}
              />
              <RadialBar
                dataKey="value"
                cornerRadius={circleSize / 2}
                fill="#0466C8"
              />
              <text
                x={circleSize / 2}
                y={circleSize / 2}
                textAnchor="middle"
                dominantBaseline="middle"
                className="progress-label"
                fontSize="3em"
                fontWeight="bold"
                fill="#0466C8"
              >
                49%
              </text>
            </RadialBarChart>
          </div>
        </div>
      </ResponsiveContainer>
    </>
  );
```
#### how to create a circle/round hole border, pie border, with a div & css
```javascript

 <style>
        {`
      .container {
        position: relative;
        padding: 30px; 
        height: 200px;
        width: 200px;
        // @media (max-width: 1199px) {
        //   height: 300px;
        //   width: 300px;
        // }
        // @media (max-width: 991px) {
        //   height: 100px;
        //   width: 100px;
        // }
      }
      .circle {
        display: flex;
        justify-content: center;
        align-items: center;
        font-family: sans-serif;
        color: #fff;
        height: 100%;
        width: 100%;
        border-radius: 50%;
        background: radial-gradient(ellipse at     center, 
          rgba(255,113,12,0) 60%,
          #000 51.5%);
      }
      
      `}
      </style>

  <div className="container">
          <div className="circle">
	  </div>
   </div>

```
#### logic to group message in chat, by hiding the avatar
```javascript
  {/* if the author of the previous message is same with the current one, then hide the avatar,
             we have the same user then, and only show the chat */}
        {name !== chats[id - 1]?.name ? (
          <Avatar
            loading="lazy"
            src="https://fastly.picsum.photos/id/237/200/300.jpg?hmac=TmmQSbShHz9CdQm0NkEjx1Dyh_Y984R9LpNrpvH2D_U"
            alt="user pics"
          />
        ) : (
          <Stack ml="35px" />
        )}
```
#### copy to clip board
```javascript
const copyToClipboard = (text) => {
  navigator.clipboard.writeText(text).then(
    () => {
      console.log("Content copied to clipboard");
      successNotifier("Copied successfully");
    },
    () => {
      errorNotifier("Faild to copy")
    }
  );
};
```

#### To center an element both vertically and horizontally:
```javascript
 position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: auto;
```

#### to animate the recently added item/object in an array
```javascript
/you must give the object an id, so react would know which to update, and not rerender everything
  <motion.div
initial={{ scale: 0 }}
animate={{ scale: 1 }}

// to start from far down and animate to default position
 initial={{ y: "200px" }}
 animate={{ y: "0px" }}
>
 <p key={i}>{l}</p>
</motion.div>
```
#### to render just enough to fill the viewport, and not all the list, (virtualized list)
```javascript
//React window works by only rendering part of a large data set (just enough to fill the viewport).
https://react-window.vercel.app/#/examples/list/fixed-size

import { VariableSizeList as List } from 'react-window';
//  VariableSizeList means to set the box size base on the content, while fixedSizeList is constant

// to get column/box size
const columnSizes = new Array(1000)
  .fill(true)
  .map(() => x); // change x to change size of box/column


// get index of a particular box by id
const getItemSize = index => columnSizes[index];

const Column = ({ index, style }) => (
  <div className={index % 2 ? 'ListItemOdd' : 'ListItemEven'} style={style}>
    Column {index}
  </div>
);

const Example = () => (
  <List
    className="List"
    height={300}
    itemCount={1000}
    itemSize={getItemSize}
    layout="vertical"
    width={300}
  >
    {Column}
  </List>
);
```

####  dynamically import font 
```javascript
  {/* dynamically import font.
      map font list and return url string, without commas (by using the join(" ")).
      and then make it available to this app using font-face */}
      <style
        dangerouslySetInnerHTML={{
          __html: `
          ${state.fontList
            .map(
              (font) =>
                ` @import url('https://fonts.googleapis.com/css2?family=${font}:wght@400;700;900&display=swap');`
            )
            .join(" ")}
         
          
          @font-face {
            font-family: ${[...new Set(state.fontList)]?.map(
              (family) => `"${family}"`
            )}
    
          }
       
       
        `,
        }}
      />
```

#### to get html element by position/x/y axis
```javascript

  // use effect is so that after component has mounted else you will get null or undefined from currentPosition
  useEffect(() => {
	//get element by the id
	  const currentElPosition = document.querySelector('#vv').getBoundingClientRect()
	  console.log(currentElPosition);
  }, []);

// console returns
{
  "x": 713.625,
  "y": 0,
  "width": 12.7421875,
  "height": 24,
  "top": 0,
  "right": 726.3671875,
  "bottom": 24,
  "left": 713.625
}

```

#### best react native chart library
```javascript
https://gifted-charts.web.app/barchart
```
#### to revert to pervious commit
```javascript
git log --oneline  // shows all your commit with their unique IDs

git reset 5914db0  // to revert back to code with a commit of ID 5914db0

```

#### filtering table data
```javascript
 <Filter
  filters={["name", "tel", "add"]} // data you want to filter
  filterBy={setFilterBy} //callback with data user wants to filter by
  info={s} // the data array []

/>

function Filter({ filters = [], filterBy, info, mb, ...props }) {
  const [defaultValueState, setDefaultValueState] = useState("");
  return (
    <>
      { (
        <Flex
          flexDir={["column", "row"]}
          alignItems={"center"}
          gap="20px"
          mb={mb ? mb : "40px"}
        >
          {filters.map((filter, idx) => (
            <Selector
              idx={idx}
              filter={filter}
              filterBy={filterBy}
              defaultValueState={defaultValueState}
              info={info}
              keys={filters}
            />
          ))}

          <Text
            cursor={"pointer"}
            w="100px"
            fontSize={".8em"}
            onClick={() => {
              filterBy(null);
              setDefaultValueState({});
            }}
          >
            Clear filter
          </Text>
          {/* <SlReload
            style={{ fontSize: "1.2em", width: "80px" }}
            cursor={"pointer"}
            onClick={() => {
              filterBy(null);
              setDefaultValueState({});
            }}
          /> */}
        </Flex>
      )}
    </>
  );
}

// the data table
 <CustomTable head={r}>
{s
  ?.filter((data) => CustomTable.filterFunc(data, filterBy))
  ?.map((data) => (
    <Tr>
    </Tr>
))}

// check if key value exist from data and check if key value (from data) and filterBy value is the same
export const filterFunc = (data, filterBy) =>
  data?.[filterBy?.key] ? data?.[filterBy?.key] === filterBy.value : data;

```


#### to use the url for state management to avoid prop drilling
##### https://localhost:3000/movies?page=3&rowsCount=10

```javascript
const [searchParams, setSearchParams]= useSearchParams()
const rowCount = searchParams.get("rowsCount")
const page = searchParams.get("page")
```

#### to get next page (pagination) from backend,
```javascript
  const page = skip * limit=20;
```


#### to fix flex box items (children) with different heights (fitting the content)
```css
align-items: "initial"
```
#### how to filter or get unique items or data, and access object in an object
<img width="847" alt="Screenshot 2024-01-23 at 1 25 42 PM" src="https://github.com/osaze12/Project-code-notes/assets/68048008/fb7243ba-1fbc-4257-a6e0-c2ff916f9a2e">

```javascript
$filters={["requestData.truckType", "status", "customerData.name"]}

const unique = (list) => {
	const unique = [];
	
	list.map((x) => {
	const xx = $filter?.split(".")?.reduce((a, b) => a?.[b], x);
	
	return unique.filter((data) => data === xx).length > 0
	? null
	: unique.push(xx);
	});
	
	return unique;
};

console.log( unique([{request:{truckType:""}]).map(...))
```

#### Access object in another object using the dot notation
```javascript
var r = { a:1, b: {b1:11, b2: 99}};
var s = "b.b2";

var value = s.split('.').reduce((a, b) => a[b], r);

console.log(value);
```

#### to customize React video player
```javascript
//Use the playIcon prop for the play button. This can be a JSX element.
//Pass the poster image URL to light prop.
//Example

import ReactPlayer from "react-player";

<ReactPlayer
  url="https://vimeo.com/243556536"
  width="100%"
  height="500px"
  playing
  playIcon={<button>Play</button>}
  light="https://i.stack.imgur.com/zw9Iz.png"
/>
```

#### reduce array of objects into one object
```javascript
 const specificationList =  [{"key": "Weight", "value": "ww"}, {"key": "Length", "value": "12w"}]
 const object = specificationList.reduce(
      (obj, item) => ({
        ...obj,
        [item.key]: item.value,
      }),
      {},
    );
```

#### filter out duplicate from an array with object
```javascript
info
.filter(
  (data, index, array) =>
    array.findIndex((d) => d.id == data.id) === index
)
.map((data, idx) => (
  <option key={idx}>{data[filter]}</option>
))

```
#### center element with transform
```css
    transform: translate(-50%, -50%);
    top: 50%;
    left: 50%;
```

#### stop height value from being more than the view height number(100vh++) on any screen size
```css
   maxHeight: `calc(screen.height.toString() - ${(screen.height - 100).toString()})`,
```

#### Generate random values from list without duplicate
```javascript
let _COLORS =['#e6194b', '#3cb44b', '#ffe119', '#4363d8', '#f58231', '#911eb4', '#46f0f0', '#f032e6', '#bcf60c', '#fabebe', '#008080', '#e6beff', '#9a6324', '#fffac8', '#800000', '#aaffc3', '#808000', '#ffd8b1', '#000075', '#808080', '#ffffff', '#000000']

let haveIt = [];

function generateUniqueRandom(maxNr) {
    //Generate random number
    let random = (Math.random() * maxNr).toFixed();

    //Coerce to number by boxing
    random = Number(random);

    if(!haveIt.includes(random)) {
        haveIt.push(random);
        return random;
    } else {
        if(haveIt.length < maxNr) {
          //Recursively generate number
         return  generateUniqueRandom(maxNr);
        } else {
          console.log('No more numbers available.')
          return false;
        }
    }
}


console.log(generateUniqueRandom(10));
console.log(generateUniqueRandom(10));


_COLORS?.[generateUniqueRandom()];


console.log('Unique random numbers:' ,haveIt);
```

#### How to make un-equal image's width and height equal
```css
.photos img {
	width: 15%;
	aspect-ratio: 3/2;
	object-fit: contain;
	mix-blend-mode: color-burn;
}
```
#### best easy web worker library
```javascript
import workerize from "workerize";

let worker = workerize(`
	export function add(a, b) {

 return fetch(
      "https://api.flutterwave.com/v3/subscriptions?email=osazeagbi@gmail.com",
      {
        method: "GET",
        headers: {
          // content-type: "ja"
          "content-type": "application/json",
          Authorization: "Bearer FLWSECK_TEST-5d9a72afc39854cbb8015beccf30d18b-X",
        }
      }
    )

  }
   
`);

  useEffect(() => {
    (async () => {
      console.log("3 + 9 = ", await worker.add(3, 9));
      console.log("1 + 2 = ", await worker.add(1, 2));
    })();
  }, []);
```
#### mobile hambuger navigation
```javascript
import { Box, Flex, Text } from "@chakra-ui/react";
import { useState } from "react";
import { FaTimes } from "react-icons/fa";
import { GiHamburgerMenu } from "react-icons/gi";
import { Link } from "react-router-dom";

function MobileHamBugerNav() {
  const [show, setShow] = useState(false);

  return (
    <>
      <GiHamburgerMenu
        onClick={() => setShow((prev) => !prev)}
        fontSize={"2em"}
      />
      {show && (
        <Box
          data-aos="fade-left"
          position="fixed"
          bottom={"0"}
          left="0"
          right="0"
          top="0"
          w="100vw"
          bg="#000"
          zIndex={1022}
        >
          <Flex
            flexDir={"column"}
            justifyContent="center"
            alignItems={"center"}
            w="100%"
            p="20px"
          >
            <FaTimes
              style={{ alignSelf: "flex-end" }}
              fontSize="1.5em"
              cursor={"pointer"}
              onClick={() => setShow(false)}
            />
            <Flex
              flexDir={"column"}
              alignItems="center"
              justifyContent="center"
            >
              <Link to={"/pricing"} onClick={() => setShow(false)}>
                <Text>Pricing</Text>
              </Link>

              <Link to={"/tests"} onClick={() => setShow(false)}>
                <Text>Tests</Text>
              </Link>
            </Flex>
          </Flex>
        </Box>
      )}{" "}
    </>
  );
}

export default MobileHamBugerNav;

```

#### scroll to top
```javascript
  useEffect(() => {
    document.body.scrollTop = 0; // For Safari
    document.documentElement.scrollTop = 0; // For Chrome, Firefox, IE and Opera
  }, []);
```

#### truncate text
```javascript
export const trunc = (text, length = 10, showDot = true) => {
  if (!text) return "";
  if (text?.length <= length) return text;
  return `${text?.substring(0, length)}${showDot ? "..." : ""}`;
};

```

#### to add font family to react app from asset folder/google font

#### from asset folder
```javascript
@font-face {
  font-family: Trenda-regular;
  src: url("./assets/font/LatinotypeTrendaRegular.woff");
}

@font-face {
  font-family: Trenda-bold;
  src: url("./assets/font/LatinotypeTrendaBold.woff");
}
@font-face {
  font-family: Trenda-heavy;
  src: url("./assets/font/LatinotypeTrendaHeavy.otf");
}

body {
  margin: 0;
  font-family: Trenda-regular, Trenda-bold, Trenda-heavy, "Segoe UI", "Roboto",
    "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
```

#### from google font
```html
    <link
      href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800&display=swap"
      rel="stylesheet"
    />
```

#### Add font family to chakra provider
```javascript
const theme = extendTheme({
  initialColorMode: "light",
  useSystemColorMode: false,

  styles: {
    global: (props) => ({
      body: {
        fontFamily: "Trenda-regular",
      },
    }),
  },
});
```

#### to get the title or text or option name of the select option
```javascript
  onChange={(e) => {
  console.log(
    e.target.options[e.target.selectedIndex].text,
    "J"
  );
```

#### set up google analytics
```javascript
import ReactGA from "react-ga";




export const useAnalytics = () => {
  const [initialized, setInitialized] = useState(true);
  const location = useLocation();
  const GOOGLE_ANALYTICS_ID = process.env.REACT_APP_GA_ID;

  useEffect(() => {
    setInitialized(true);
  }, []);
 

  useEffect(() => {
    if (initialized) {
      ReactGA.initialize(GOOGLE_ANALYTICS_ID);
      ReactGA.pageview(location.pathname + location.search);
    }
  }, [initialized, location, GOOGLE_ANALYTICS_ID]);

  return null;
};

  useAnalytics();
  
  
  
   // to track a specific event(action, like button action)
  export const Event = (category, action, label) => {
 ReactGA.event({
  category: category,
  action: action,
  label: label
 });
};

const handleClickPhone = () => {
  Event("KALENDER", "MODAL BTN TRANSAKSI", "TELEPON");
};

/* inside the render you must create onClick func to detect if the user clicked it or not */
<Button onClick={handleClickPhone}>
  Phone
</Button>
```

#### Add csv button and functionality
```javascript
import { creatCsvFile, downloadFile } from "download-csv";

  const csvColumns = {
    date: "Date",
    rating: "Rating",
    review: "Review",
    department: "Department",
  };
  
  
   const csvData =
    review?.feedback?.map((data) => ({
      date: dayjs(data?.createdAt).format("DD-MMM-YYYY"),
      rating: parseInt(data?.rating),
      review: data?.review,
      host: data?.host,
      department: data?.department,
    }));

  const handleCsvDownload = () => {
    const csvFile = creatCsvFile(csvData, csvColumns);
    downloadFile(csvFile);
  };
```

#### Area chart customization (REChart)
```javascript
const data = [
  {
    name: "Page A",
    uv: 4000,
    pv: 2400,
    amt: 2400
  },
  {
    name: "Page B",
    uv: 3000,
    pv: 1398,
    amt: 2210
  },
  {
    name: "Page C",
    uv: 2000,
    pv: 9800,
    amt: 2290
  },
  {
    name: "Page D",
    uv: 2780,
    pv: 3908,
    amt: 2000
  },
  {
    name: "Page E",
    uv: 1890,
    pv: 4800,
    amt: 2181
  },
  {
    name: "Page F",
    uv: 2390,
    pv: 3800,
    amt: 2500
  },
  {
    name: "Page G",
    uv: 3490,
    pv: 4300,
    amt: 2100
  }
];

export default function App() {
  return (
    <AreaChart
      width={500}
      height={400}
      data={data}
      margin={{
        top: 10,
        right: 30,
        left: 0,
        bottom: 0
      }}
    >
      <defs>
        <linearGradient id="colorUv" x1="0" y1="0" x2="0" y2="1">
          <stop offset="5%" stopColor="#FF5403" stopOpacity={0.2} />
          <stop offset="95%" stopColor="#FF5403" stopOpacity={0} />
        </linearGradient>
      </defs>
      <CartesianGrid strokeDasharray="3 3" vertical={false} />
      <XAxis dataKey="name" axisLine={false} tickLine={false} />
      <YAxis axisLine={false} tickLine={false} />
      <Tooltip />
      <Area
        type="linear"
        dataKey="uv"
        stroke="#FF5403"
        strokeWidth="3"
        fillOpacity={1}
        fill="url(#colorUv)"
      />
    </AreaChart>
  );
```
#### more chart customization with shadow, custom tooltip (Rechart)
```javascript

 <LineChart
          width={250}
          height={150}
          style={{
            display: "flex",
            alignItems: "flex-start",
          }}
          data={_WEEKLY_CHART_DATA}
          margin={{
            top: 5,
            right: 30,
            left: -55,
            bottom: 5,
          }}
        >
          <CartesianGrid
            strokeDasharray="3 3"
            // verticalFill={""}
            opacity={0.2}
          />
          <XAxis
            tick={{ fill: _LIGHTER_BLACK }}
            dataKey="name"
            axisLine={false}
            tickLine={false}
          />
          <YAxis
            tick={{ fill: _LIGHTER_BLACK, display: "none" }}
            axisLine={false}
            tickLine={false}
          />
          <Tooltip
            wrapperStyle={{ outline: "none" }}
            content={(props) => <CustomToolbarContent {...props} />}
            cursor={{
              fill: "rgba(206, 206, 206, 0.2)",
            }}
          />
          <Line
            type="linear"
            style={{
              filter: `drop-shadow(20px 10px 8px #6e6d7a)`,
            }}
            dataKey="pv"
            stroke={_YELLOW}
            activeDot={{ r: 8 }}
            strokeWidth="3"
          />
        </LineChart>


 <BarChart
          width={250}
          height={150}
          style={{
            display: "flex",
            alignItems: "flex-start",
          }}
          data={_WEEKLY_CHART_DATA}
          margin={{
            top: 5,
            right: 30,
            left: -55,
            bottom: 5,
          }}
        >
          <CartesianGrid
            strokeDasharray="3 3"
            // verticalFill={""}
            opacity={0.2}
          />
          <XAxis
            tick={{ fill: _LIGHTER_BLACK }}
            dataKey="name"
            axisLine={false}
            tickLine={false}
          />
          <YAxis axisLine={false} tickLine={false} />
          <Tooltip
            wrapperStyle={{ outline: "none" }}
            content={(props) => <CustomToolbarContent {...props} />}
            cursor={{
              fill: "rgba(206, 206, 206, 0.2)",
              radius: [10, 10, 10, 10],
            }}
          />
          <Bar fill={"red"} dataKey="pv" style={{ borderRadius: "10px" }}>
            {_WEEKLY_CHART_DATA.map((entry, index) => (
              <Cell
                radius={[10, 10, 10, 10]}
                key={`cell-${index}`}
                fill={barColors[Math.floor(Math.random() * barColors.length)]}
              />
            ))}
          </Bar>
        </BarChart>



export const CustomToolbarContent = ({ payload }) => {
  console.log(payload);
  return (
    <Box
      bg="#fff"
      borderRadius={"5px"}
      p="6px 20px"
      boxShadow={"5px 4px 10px 1px #8080801c"}
      textAlign="center"
    >
      <Text fontSize={"1.2em"} color={_BLACKER} fontWeight={"700"}>
        {payload?.[0]?.value?.toLocaleString()}
      </Text>
      <Text fontSize={".8em"} color={_LIGHT_BLACK}>
        {(parseInt(payload?.[0]?.value) / 10000) * 100}%
      </Text>
    </Box>
  );
};
```

#### how to undo merge
```html
https://www.freecodecamp.org/news/git-undo-merge-how-to-revert-the-last-merge-commit-in-git/
```

#### download file from link
```javascript
  const downloadURI = async (url, filename) => {
    const data = await fetch(url);
    const blob = await data.blob();
    const objectUrl = URL.createObjectURL(blob);

    const link = document.createElement("a");

    link.setAttribute("href", objectUrl);
    link.setAttribute("download", filename);
    link.style.display = "none";

    document.body.appendChild(link);

    link.click();

    document.body.removeChild(link);
  };
```

#### cover letter
```html
Hello,
I am applying for this job because I met some criteria, and our mission or goals align.

my goal is to build products that solve problems for organizations and help them grow their revenues quickly.

I've built a product, called In-cert, which helps people generate 100s of digital certificates in seconds, here's a link, https://in-cert.netlify.app/

I've also built a react library, which helps developers create a fully functional form with just 5 lines of code, here's a link, https://www.npmjs.com/package/@osaze12/bootstrapped-form
```

#### if you're having issues with react showing error when it tries building the app on AWS/other platform
#### add --openssl-legacy-provider to your package.json file
```javascript
  "start": "react-scripts start --openssl-legacy-provider",
```
#### Clone from a specific branch on github
```javascipt
git clone --branch <branchname> <remote-repo-url>
or git clone -b <branchname> <remote-repo-url>
```

#### Good stripe tutorial
```
https://linguinecode.com/post/integrate-stripe-payment-form-with-react
```
#### Make ids from just alphabeth
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

#### Format bytes
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
#### remove array of array
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

#### Remove duplicate from an array
```javascript
// usage example:
var myArray = ['a', 1, 'a', 2, '1'];
var unique = myArray.filter((v, i, a) => a.indexOf(v) === i);

console.log(unique); // unique is ['a', 1, 2, '1']
```
#### Plot graph with custom numbers
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
##### BarChart.jsx
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

#### Respond when a user leaves the screen
```javascript
  useEffect(() => {
    document.addEventListener("visibilitychange", () => {
      setEnableChatTone(true);
    });
  }, []);
```
#### Take a live photo from webcam and save photo file

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

#### Record a live video with a stop timer & save video file 
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

#### How to add svg image directly to css
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
#### Dots animations
```
https://codepen.io/nzbin/pen/GGrXbp
```

#### Center an absolute item
```
https://stackoverflow.com/questions/7720730/how-to-align-absolutely-positioned-element-to-center#:~:text=If%20you%20set%20both%20left,center%20an%20absolutely%20positioned%20element.
```

#### Convert Blob to file
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

#### take live photo
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
#### the html code
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


#### Formatting number to have 1k, 2.5M, (convert million/thousand to M, K)
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

#### Do something when video has loaded
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

#### My details
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


#### How to preserve line breaks from text areas to p tag, just add that to p tag
```css
white-space: pre-wrap;
```

#### add dark background on Image in react native
```
https://stackoverflow.com/questions/68754443/how-add-dark-gradient-on-bottom-of-an-image-in-react-native
```

#### Add dark background on image
```css
  background:`linear-gradient(90deg, rgb(0 0 0 / 63%) 0%, rgb(0 0 0 / 63%) 51%), url(${previewImg})`
```

#### if .gitignore isnt ignoring your file/folders
```
Just remove the initial git configuration from your folder by rm -rf .git. 
Use the created .gitignore file and add all the file you want to add. 
Then reinitialize the git configuration in your folder with git init. This will solve your problem.
```

#### error with socket IO chat failing 
```
WebSocket connection failed: Error during WebSocket handshake: Unexpected response code: 400
______
SOLUTION
_________
https://stackoverflow.com/questions/41381444/websocket-connection-failed-error-during-websocket-handshake-unexpected-respon

```

#### play a sound when notification arrives
```javascript
   socketClient.on("in-app-notification", (data) => {
      //play sound when a new notification comes in
      const audio = new Audio(bellSound);
      audio?.play();
    });
```

#### how to get the width of an html element
```javascript
  useEffect(() => {
    let p = document.getElementsByClassName("zz")[0];
    setSize(window.getComputedStyle(p).width);
  }, []);

```

#### convert px to number
```javascript
 parseFloat("12px")
 parseInt("12px")
```


#### Customize website's scrollbar like Mac OS (doesnt hide scroll on leave)
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

#### scroll to bottom smoothly
```javascript
  //auto scroll to bottom
  document
    .getElementById("messages")
    .scrollIntoView({ behavior: "smooth", block: "end", inline: "nearest" });

//OR ( this is the best)
// Get a reference to the div you want to auto-scroll.
var someElement = document.querySelector('.className');

    function scrollToBottom() {
      someElement.scrollTop = someElement.scrollHeight;
    }


// Create an observer and pass it a callback.
var observer = new MutationObserver(scrollToBottom);
// Tell it to look for new children that will change the height.
var config = {childList: true};
observer.observe(someElement, config);

// OR

    function updateScroll() {
      var element = document.getElementById("scroll-container");
      element.scrollTop = element.scrollHeight;
    }
    setInterval(updateScroll, 100);
```

####  Customize website's scrollbar like Mac OS (hide scroll on leave)
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


#### project you have built
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

#### To observe changes of an html element, e.g the div width
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

#### convert image file e.target.files[0] to a base url
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

#### scroll to a particular position/box using the id
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


#### Cover letter
```
I've built software solutions that cut across Ecommerce, Healthcare, etc. 
I've gained the required experience to add value to the company, the following below are some of the project i've worked on:
https://www.howbodi.io/
A health care web application where doctors can schedule doctors & have conversations via video/voice call or text chat, right on the app.
More can be found on my portfolio at https://tinyurl.com/osas-portfolio
And i've also attached my CV
Hope to hear from you soon
```

#### If you have problem converting string to object using JSON.parse()
```javascript

  let jsonStr = String(r).replace(/(\w+:)|(\w+ :)/g, function (matchedStr) {
      return '"' + matchedStr.substring(0, matchedStr.length - 1) + '":';
    });
    
    JSON.parse(jsonStr)
```

#### Convering string to array/object

```javascript
let object = {"name": "osas"}

use eval(object) if JSON.parse(object) doesnt work
```


#### Create a persistent count down timer (store/save count down time to session/localstorage)

#### Install react-countdown library
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


#### WHEN IDLE FOR X SECONDS, SHOW A VIDEO, & WHEN THERE'S A MOUSE EVENT, RESTART THE COUNTER
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

#### ASK USER FOR PERMISSION & SUBSCRIBE FOR PUSH NOTIFICATION SERVICE

##### In public folder, a created sw.js file
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

##### In App.js 
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

#### REACT CLONING REACT COMPONENT AND INJECTING DATA INTO IT
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

#### SENDING CV FORMAT TO HR
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

#### ["7:00 AM - 7:30 AM","7:30 AM - 8:00 AM","8:00 AM - 8:30 AM","8:30 AM - 9:00 AM","9:00 AM - 9:30 AM","9:30 AM - 10:00 AM"]
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
#### convert 12 hrs to 24 hrs
```javascript
const convertTime12to24 = (time12h) => {
  const [time, modifier] = time12h.split(' ');

  let [hours, minutes] = time.split(':');

  if (hours === '12') {
    hours = '00';
  }

  if (modifier === 'PM') {
    hours = parseInt(hours, 10) + 12;
  }

  return `${hours}:${minutes}`;
}

console.log(convertTime12to24('01:02 PM'));
console.log(convertTime12to24('05:06 PM'));
console.log(convertTime12to24('12:00 PM'));
console.log(convertTime12to24('12:00 AM'));
```

#### CONVERT 24 HRS TO 12 HRS

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


#### —CHANGING REACT-CALENDAR DAY VIEW & CUSTOM CONTROL LINK

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

#### —TO GET ALL DAYS IN A MONTH

  
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



#### —TO GET TIMES BASED ON THE START TIME AND END TIME, BUT NOT LESS THAN THE CURRENT TIME

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

#### —TO GET TIMES BASED ON THE START TIME AND END TIME

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
#### — TO GROUP SIMILAR VALUES IN AN ARRAY

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
#### CUSTOM MONTH NAVIGATION RENDERER FOR REACT-DAY-PICKER

https://github.com/hypeserver/react-date-range/issues/222


#### selecting multiple dates on react calendar
```javascript
https://stackoverflow.com/questions/60446117/how-to-mark-particular-dates-in-react-calender
```
————————————————————————————————————

#### CHANGE NAV BACKGROUND COLOR WHEN SCROLL POSITION IS GREATER THAN X VALUE

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

#### Base64 string to image file

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

#### Tutorial on web push
10:58
https://itnext.io/react-push-notifications-with-hooks-d293d36f4836

————————————————————————————————————

#### Good tutorial on push notification

 https://www.youtube.com/watch?v=HlYFW2zaYQM

————————————————————————————————————

#### Loading an image and getting width and height

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

#### Good React Calendar library

https://mmehdinasiri.github.io/react-calendar-datetime-picker/docs/examples/


————————————————————————————————————

#### Having cors issues, add  "Access-Control-Allow-Headers": "Content-Type" to =>

 ```javascript
headers: {
    Accept: "application/json, text/plain, */*",
    "Content-Type": "application/json ",
    "Access-Control-Allow-Headers": "Content-Type",
  },
})
```

————————————————————————————————————

#### From 1200 to 12:00 code =>
```javascript

mystring = mystring.replace(/(..)/g, '$1:').slice(0,-1)
```


————————————————————————————————————

#### To get range of fromTime toTime

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

#### Check if user is authenticated

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

#### How to implement web workers

https://blog.logrocket.com/real-time-processing-web-workers/



————————————————————————————————————

#### HOW TO SET UP WEB WORKERS

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

#### Updated web worker code 1

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

#### Updated web worker code 2
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

#### To remove error from eslint on hook dependency


// eslint-disable-next-line react-hooks/exhaustive-deps

————————————————————————————————————

#### To get last day of month
```javascript

dayjs().endOf("month").date(),  of the month
```

————————————————————————————————————

#### To get the current month dates, eg= [2021-01-01, 2021-01-02, 2021-01-03]
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

#### Better and optimised
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

#### To have a lighter color
```javascript

 _hover={{ bg: `${bg}80` }}
```
————————————————————————————————————

#### Display something else when screen is idle for 15 seconds
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

#### To have 3 box in a row

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

#### To  auto play video, in a loop, in react
```javascript

<video autoPlay loop muted playsInline style={{ width: "100vw" }}>
  <source src={idleVideo} type="video/mp4" />
  Your browser does not support the video tag.
 </video>
```
————————————————————————————————————

#### Javascript convert to boolean, doesn't work, eg: Boolean("false") === true

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

#### Create .zip files using JavaScript.


https://stuk.github.io/jszip/

stuk.github.io
JSZip
Create .zip files using JavaScript. Provides a simple API to place any content generated by JavaScript into a .zip file for your users.

————————————————————————————————————

#### An HTML5 saveAs() FileSaver 

https://github.com/eligrey/FileSaver.js

eligrey/FileSaver.js
An HTML5 saveAs() FileSaver implementation
Website
https://eligrey.com/blog/saving-generated-files-on-the-client-side/




————————————————————————————————————

#### To remove time icon from input

 ```javascript
const OVERRIDE_TIME_STYLE = `input[type="time"]::-webkit-calendar-picker-indicator {
//     display: none !important;
// }
// }`
```
————————————————————————————————————

#### JavaScript program to run a function in a separate thread by using a Web Worker, allowing long running functions to not block the UI.

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

#### Best web worker to use in react

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

#### To remove white background from favicon

```html
add type="image/x-icon"

<link rel="icon" href="%PUBLIC_URL%/favicon.ico" type="image/x-icon" />

```
————————————————————————————————————

#### Change nav bar color when user scroll to x position


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

#### Overlay text on an image using css grid
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

#### Dot loading animation svg

https://codepen.io/nikhil8krishnan/pen/rVoXJa
https://css-tricks.com/single-element-loaders-the-dots/

 ————————————————————————————————————

#### Animation library for react
"aos": "^2.3.4",
```javascript

<link href="https://unpkg.com/aos@2.3.1/dist/aos.css" rel="stylesheet">

useEffect(() => {
  Aos.init({ duration: 1000,  once: false,
      startEvent: "load" })
}, [])
  
<div data-aos="fade-right"></div>
```

 ————————————————————————————————————
 #### scroll on Drag for users with older laptops
 ##### index.css
 ```css
 .items {
  display: flex;
  overflow: scroll;
  /* scroll-behavior: smooth; */
}
 ```
 ##### index.html
 ```
 <div class="items">
    <div class="item item1"></div>
    <div class="item item2"></div>
    <div class="item item3"></div>
    <div class="item item4"></div>
    <div class="item item5"></div>
  </div>
 ```
 
 ##### index.js
 
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
 

#### Hide Scrollbars But Keep Functionality
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

#### Scroll forward/backward with a button/arrow

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

#### allow html formatting in a string, inner html
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

#### TYPES OF COMMENTS

// ! critical comment
// * hightlighted comment
// TODO: todo comment
// ? question comment
// normal comment



 ————————————————————————————————————

#### ADD DAYS TO DATE, 1 Mon 2022 => 2 Tue 2022 => 3 Wed 2022

```javascript
function addDays(date, days) {
    var result = new Date(date);
    result.setDate(result.getDate() + days);
    return result;
}
```



 ————————————————————————————————————

#### Ux for mobile keyboard to the next input

```
enterKeyhint="next"
enterKeyhint="done"
```


#### Paginated table code
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
