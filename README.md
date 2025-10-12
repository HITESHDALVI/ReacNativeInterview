# ReacNativeInterview
Interview questions and interview questions
1. Introduce yourself

  Answer -> “Hi, my name is Hitesh Dalvi. I have 3.5 years of experience as a Software Developer, specializing in cross-platform mobile development and front-end engineering using React Native, React.js, JavaScript, TypeScript, Redux, and Google Apps Script.
  Currently, I’m working at Homeville Group as an SDE2, where I’ve been responsible for building and maintaining two fintech applications — Homeville Fieldforce and Singularity Credit World. These applications streamline the home loan process from application to disbursal, used by both customers and internal teams.
  In these projects, I’ve developed critical features such as payment gateway integration, Firebase push notifications, secure data storage, crash analytics, and performance monitoring. I also handle end-to-end deployment to the App Store and Play Store, ensuring a smooth release process.
  Beyond delivering features, I focus on writing clean, maintainable code and optimizing app performance. My contributions have improved responsiveness and reduced API latency, resulting in a more reliable and crash-free user experience.
  I’m now looking forward to contributing my expertise to new challenges and building scalable, impactful mobile solutions with your team.”

2. **“You’ve worked on multiple apps in production. Suppose you are building a cross-platform feature that requires capturing and uploading user images.
  Can you walk me through:
  Which React Native components or libraries you’d use?
  What permissions are required on iOS and Android?
  And how would you verify that the upload was successful from within the app?”**

  Answer -> Sure. In one of my recent projects, I implemented an image capture and upload feature.
  For capturing images, I used react-native-vision-camera, which gives low-level access to the camera with good performance.
  For selecting images from the gallery or file manager, I used react-native-image-picker, which allows users to pick existing images or take new ones with a unified API.
  Regarding permissions:
  On Android, I request CAMERA, READ_EXTERNAL_STORAGE, and WRITE_EXTERNAL_STORAGE (for Android <10). On Android 11 and above, I also handle Scoped Storage by using content:// URIs.
  On iOS, I add NSCameraUsageDescription and NSPhotoLibraryUsageDescription in the Info.plist to comply with Apple’s privacy requirements.
  Once I have the image, I prepare a FormData object and upload it to the backend using axios with multipart/form-data headers.
  To verify the upload:
  I check the HTTP status code — typically 200 or 201 indicates success.
  I also parse the server response to ensure the backend confirms the file was saved (e.g., a file URL or file ID is returned).
On the UI side, I update the state to show a success message or show the uploaded image thumbnail.
Additionally, if needed, I track upload progress using axios’s onUploadProgress to provide the user with a progress bar or loader.

3. 👉 “How do you handle performance optimization in React Native applications, especially when dealing with complex or large lists?”
  Answer -> “To optimize performance in React Native, especially when working with large or complex lists, I usually take a multi-step approach:
  Efficient List Rendering –
  Instead of ScrollView, I prefer FlatList or SectionList since they render only the visible items using virtualization.
  I fine-tune props like initialNumToRender, maxToRenderPerBatch, and windowSize to balance performance and user experience.
  I always provide a stable keyExtractor to minimize unnecessary re-renders.
  Memoization & Pure Components –
  I wrap the renderItem component with React.memo (or useCallback when passing functions) so that items only re-render when their props actually change.
  For complex row components, I also use useMemo to cache expensive computations
  Image & Asset Optimization 
  Lazy-load and cache images using libraries like react-native-fast-image.
  Preload and compress images/videos when necessary to reduce load time.
  Avoiding Unnecessary State Updates –
  I keep the state localized and avoid putting large data arrays in global contexts (like Redux) unless necessary.
  For example, instead of updating the entire list on a minor change, I only update the affected item.
  JS and Native Bridge Optimization –
  Minimize frequent communication over the React Native bridge (e.g., avoid sending huge JSON payloads or calling native methods in tight loops).
  Batch updates or debounce them where possible.
  Profiling & Monitoring 
  I use tools like Flipper or React Native Performance Monitor to identify bottlenecks such as slow renders, high memory usage, or unnecessary re-renders.
  Based on profiling, I decide whether to refactor, paginate, or offload work to background threads.
  Additionally, for extremely large datasets, I sometimes implement infinite scroll with pagination or use specialized libraries like recyclerlistview for better virtualization than FlatList.
  Overall, my goal is to balance smooth scrolling, responsive UI, and minimal memory footprint.”

4. Can you explain the React Native architecture, especially the JS thread, shadow thread, and UI thread?
   Ansswer -> React Native’s architecture is based on a multi-threaded model where different responsibilities are split across threads to ensure performance and responsiveness. The three main threads involved are:
JavaScript Thread (JS Thread):
This is where all the business logic of the React Native app runs.
When we write React or JavaScript code (like components, state updates, API calls, event handling), it gets executed on this thread.
It’s a single-threaded, event loop–based model, similar to how JavaScript works in the browser.
The JS thread never directly manipulates UI—it only calculates what the UI should look like and then sends that information to the native side.
Shadow Thread:
This is a background thread where React Native uses a layout engine called Yoga to calculate the layout of components.
When the JS thread decides the UI tree (what elements exist and their properties), this layout calculation (flexbox, positions, dimensions) happens in the shadow thread.
Offloading layout calculation to the shadow thread ensures that the UI thread remains free for smooth rendering and doesn’t get blocked by heavy computations.
UI Thread (a.k.a. Main Thread):
This is the thread where actual native rendering happens.
The UI thread is responsible for drawing UI elements, handling animations, and processing touch events.
It receives the final layout instructions (calculated by the shadow thread) and renders the UI natively using platform-specific APIs (like UIView in iOS and View in Android).
Since this is the most critical thread for user experience, React Native tries to keep it as light as possible to maintain 60fps performance.
How they communicate:
The threads communicate through a bridge (in the older architecture). The JS thread sends JSON messages with UI changes, which go through the bridge to the shadow thread and then the UI thread.
In the new architecture (Fabric + JSI), this communication is more direct, with synchronous and asynchronous calls, making it faster and reducing bottlenecks.
Example:
Let’s say I update a component’s style in React Native:
The JS thread registers the change.
The shadow thread calculates the new layout.
The UI thread applies that layout to native views and renders it on the screen.

5.React Native is a cross-platform mobile framework that lets you write apps using JavaScript and React concepts. Unlike native development where you write separate codebases in Swift/Objective-C for iOS and Kotlin/Java for Android, React Native uses one codebase that renders to actual native components, not web views. This gives you near-native performance while sharing most of your code between platforms. 

6.n JavaScript, "this" refers to the execution context. In regular functions, "this" depends on how the function is called - it could be the global object, undefined in strict mode, or the object that called it. In arrow functions, "this" is lexically bound to the surrounding scope. In React class components, you often need to bind "this" or use arrow functions to maintain the correct context. 

7. When a React Native app lags while rendering a long list, it’s usually because rendering all items at once blocks the JS thread, causing frame drops. To optimize this, I typically use FlatList, which renders only the items visible on the screen plus some buffer, instead of rendering the entire dataset.
Some specific optimizations include:
initialNumToRender — controls how many items are rendered initially to reduce startup work.
maxToRenderPerBatch and windowSize — control batching of offscreen items, balancing memory and responsiveness.
getItemLayout — if list items have a fixed height, this allows FlatList to skip measurement calculations and jump to any item efficiently.
keyExtractor — ensures stable keys to prevent unnecessary re-renders.
Additionally, I make sure each item is pure / memoized using React.memo or PureComponent to avoid re-rendering when props haven’t changed. For extremely long lists, I might also consider pagination / lazy loading or moving expensive computations off the JS thread, for example using InteractionManager.runAfterInteractions or react-native-reanimated for animations.
By combining these strategies, the list remains smooth, memory-efficient, and responsive, even with thousands of items.”

8.The event loop is the mechanism that lets JavaScript perform non-blocking I/O while staying single-threaded. Conceptually there are three players: the call stack (where synchronous code runs), the host environment (browser/Node/React-Native—provides timers, networking, etc.), and one or more queues for deferred callbacks.
When synchronous code runs it uses the call stack. If code initiates an asynchronous operation (network request, timer), the host environment handles that work and, when done, enqueues a callback into a task queue. The event loop repeatedly checks: if the call stack is empty it first drains the microtask queue (Promise callbacks, queueMicrotask), running each microtask to completion; only after the microtask queue is empty does it take the next macrotask (e.g. setTimeout, I/O callbacks) and run it. This ordering (microtasks before tasks) is why Promise.then() callbacks run before setTimeout(..., 0) callbacks.
In React Native specifically, the JS thread is single-threaded — long-running synchronous operations block the UI because the UI thread and JavaScript thread must coordinate (unless work is on the native side). So understanding the event loop is important: avoid blocking the JS thread, prefer microtask-friendly patterns for short async work, and offload heavy work to native modules or background threads. Modern RN tooling (Hermes + JSI/TurboModules) reduces some overheads, but the event loop semantics remain the same.

9. Closures in JavaScript happen when a function remembers and accesses variables from its outer scope, even after that outer function has finished executing.
    In simple terms — a closure allows an inner function to “close over” variables from its parent function.
   function counter() {
  let count = 0;

  return function increment() {
    count++;
    return count;
  };
}

const increase = counter();
console.log(increase()); // 1
console.log(increase()); // 2

Here, even though counter() has finished executing, the inner function increment() still has access to the count variable — that’s a closure.

🔹 Real-world React Native Example:

Closures are often used when managing state or event handlers.
For example, if you have a timer that updates a variable even after a component is rendered:
function useTimer() {
  let seconds = 0;

  const start = () => {
    setInterval(() => {
      seconds++;
      console.log(seconds); // closure keeps access to "seconds"
    }, 1000);
  };

  return { start };
}
Here, the callback inside setInterval still remembers the seconds variable because of closure.

11. Debouncing and throttling are techniques to control how often a function is executed, which is especially useful in React Native to avoid unnecessary re-renders or expensive operations.
Debouncing ensures a function runs only after a certain period of inactivity. If the event keeps firing, the timer resets, so the function executes only once the events stop. A common use case is search input — making API calls only after the user stops typing.
Throttling ensures a function runs at most once in a specified interval, no matter how many times the event fires. This is useful for high-frequency events like scrolling, dragging, or window resizing, where you want to limit updates to, say, once every 100ms.
In React Native, for example, you might debounce a TextInput change before sending an API request, or throttle a scroll handler in a FlatList to avoid overloading the JS thread.

12.“In the classic React Native architecture, JavaScript communicates with native modules via the Bridge. The Bridge is asynchronous and serializes messages (usually JSON) between the JS thread and the native thread. For example, when JS calls a native module like Camera, the call is queued, sent through the Bridge, executed on the native side, and the result is sent back asynchronously. While this works, it introduces latency, especially for high-frequency operations like animations or gestures, because every call must be serialized, sent, and deserialized.
The new Fabric architecture improves this by introducing JSI (JavaScript Interface) and TurboModules. Instead of going through a serialized Bridge, JS can now communicate directly and synchronously with native modules via C++ objects. This reduces the overhead of passing data back and forth, allows synchronous updates for UI, and makes rendering more predictable and performant.
In practice, Fabric improves performance in complex UI interactions, animations, and high-frequency updates because it eliminates the Bridge bottleneck. Combined with hooks and React’s concurrent renderer, this allows React Native apps to feel more “native” with smoother frame rates and reduced latency.”

13. “Redux is a predictable state container for JavaScript apps. It centralizes application state in a single store and updates state via actions and reducers. It is especially useful in large-scale applications where many components need access to the same state and you need strict control over state changes.
The Context API is a React built-in feature for sharing data across the component tree without passing props manually. It’s ideal for small to medium apps, or for sharing theme, language, or authentication data, but it doesn’t include middleware, dev tools, or structured state updates like Redux.
A key performance consideration:
With Context, updating a value can cause all consuming components to re-render, which can be expensive if the state changes frequently. You can mitigate this with useMemo or by splitting contexts.
Redux allows fine-grained control; connected components only re-render when the specific part of the state they subscribe to changes.
In React Native, I usually use Context for lightweight global state (like theme or auth), and Redux for complex apps with large shared state or when I need middleware for async operations like API calls.”

14. “Class components in React have lifecycle methods such as:
componentDidMount → runs after the component is mounted.
componentDidUpdate → runs after state or props change.
componentWillUnmount → runs before the component is removed from the DOM (or JS tree in React Native).
Functional components use hooks to achieve the same behavior, primarily useEffect. For example:
useEffect(() => { ... }, []) → runs only once after mount, similar to componentDidMount.
useEffect(() => { ... }, [dependencies]) → runs when dependencies change, similar to componentDidUpdate.
Returning a cleanup function from useEffect → runs before unmount, similar to componentWillUnmount.
The advantages of functional components with hooks include simpler syntax, no need for this binding, better logic reuse via custom hooks, and easier testing. In React Native, this is especially useful for managing subscriptions, timers, or asynchronous API calls without cluttering lifecycle methods across multiple classes.
For new projects, I generally prefer functional components with hooks because they make the code more readable, maintainable, and consistent with modern React best practices.”

15. “In React Native, if an API call resolves after the user navigates away, directly updating a component’s local state can cause ‘setState on unmounted component’ warnings or even memory leaks. To handle this safely:
Global state management: Move the API call logic and data storage to a global store, using Context or Redux. Components subscribe to the state and render updates if they are mounted. This ensures that even if the user navigates away, the API call continues and updates are safely reflected in any subscribed component.
Cleanup with hooks: In functional components, use a flag or cleanup function in useEffect to check if the component is still mounted before updating local state.
Optional cancellation: If using fetch or axios, you can cancel the request when the component unmounts to avoid unnecessary network work.

16.In React Native, animations sometimes drop frames or feel janky, especially on lower-end devices.
How would you optimize animations to ensure they run smoothly without blocking the JS thread?
“In React Native, animations can drop frames or feel janky when they run entirely on the JS thread, especially if the thread is busy with API calls, state updates, or heavy computations. To optimize animations:
Use the native driver: Libraries like Animated or react-native-reanimated allow animations to run on the native UI thread. This offloads the work from the JS thread, ensuring smoother 60 FPS animations even if the JS thread is busy.
Memoize animated values and components: Avoid unnecessary re-renders that can block the JS thread.
Reduce layout calculations: For example, use getItemLayout in FlatLists and avoid nested Views with heavy layouts during animations.
Use Reanimated v2+ or JSI-based libraries: These leverage direct synchronous communication with native (Fabric + JSI), further improving frame rates.

17. Suppose a React Native app is lagging or freezing intermittently.
How would you identify whether the issue is on the JS thread or native thread, and what tools or techniques would you use to profile and fix performance bottlenecks?
When a React Native app lags or freezes, the bottleneck could be on the JS thread or the native thread. To identify the source, I follow these steps:
Check JS thread usage:
Use React Native Performance Monitor (⌘D → Show Perf Monitor on iOS, or Dev Menu on Android) to see FPS and JS thread activity.
If JS thread usage spikes during interactions, it indicates heavy computations, API calls, or frequent re-renders.
Check native thread usage:
Use Xcode Instruments (iOS) or Android Profiler (Android) to monitor CPU/GPU usage.
Frame drops here often indicate expensive layout passes, heavy animations, or slow native modules.
Profile rendering and re-renders:
Use why-did-you-render or React DevTools profiler to detect unnecessary re-renders.
Optimize components with React.memo, useMemo, or useCallback.
Network / Async checks:
Ensure long-running API calls or JSON parsing aren’t blocking the JS thread. Offload heavy computation to background threads or native modules.
Fixes:
Offload animations to native driver.
Memoize expensive components.
Split complex FlatLists with windowSize and getItemLayout.
Move heavy logic to TurboModules or background threads.
Using this combination of performance monitors, profiling tools, and code optimization, I can pinpoint whether the JS thread, native thread, or layout computation is causing the lag and apply targeted fixes.”

18.“Securing API requests and sensitive data in a React Native app is critical because the JS code runs on the client and can be inspected. My approach covers secure communication, token management, and data storage:
HTTPS / TLS: Always use HTTPS for all API calls to ensure encrypted communication. Avoid HTTP entirely.
Authentication tokens:
Store tokens securely using react-native-keychain or SecureStore instead of AsyncStorage, which is not encrypted.
Implement short-lived tokens with refresh tokens to minimize risk if a token is compromised.
Avoid exposing secrets in code: Never hardcode API keys or credentials. Use environment variables and tools like react-native-config.
Certificate pinning: For highly sensitive apps, use certificate pinning to prevent man-in-the-middle attacks.
Input validation & API error handling: Sanitize user input and handle server errors to prevent injection attacks or crashes.
Obfuscation / Hermes: Use JS code obfuscation or Hermes bytecode to make it harder for attackers to read sensitive logic.
Network request practices:
Use libraries like Axios or fetch with proper timeout, retry, and cancellation handling.
Prefer background-safe network calls using InteractionManager or native modules for long-running requests.
By combining secure storage, encrypted communication, token best practices, and runtime safety, we minimize the risk of leaks or attacks while maintaining smooth app functionality.”

19. 
