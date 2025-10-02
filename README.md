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

5.  
