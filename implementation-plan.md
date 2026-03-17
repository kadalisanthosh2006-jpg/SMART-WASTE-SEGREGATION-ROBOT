# Smart Waste Segregation Robot - Hackathon Implementation Plan

## 1. Implementation Plan
This section breaks down the end-to-end development of the MVP into clear, executable phases.

### Phase 1: Project Setup (Hour 1)
- **Repo Initialization:** Create GitHub repository.
- **Frontend Setup:** Initialize Next.js with TypeScript (`npx create-next-app@latest frontend --typescript --tailwind --eslint`). Install Framer Motion, Chart.js, and Lucide-React.
- **Backend Setup:** Keep Node.js/Express (`npm init -y`). Install dependencies: `express`, `cors`, `multer`, `dotenv`.
- **Database Setup:** Provision a free tier PostgreSQL database on Neon Cloud. Obtain connection strings.

### Phase 2: Backend & Database (Hours 2-4)
- **Database Schema:** Execute SQL commands to create `images`, `detections`, `categories`, and `analytics` tables.
- **Core API Routes:** Create `/detect` (POST for uploads) and `/analytics` (GET).
- **Storage:** Configure `multer` for local storage or memory storage for the hackathon (to save time).

### Phase 3: AI Development & Integration (Hours 5-8)
- **Model Setup:** Setup a quick Python/FastAPI microservice or use a local Python script running `YOLOv5` via `torch.hub.load`. (Alternatively, if strictly using Node.js, spawn a python child process for the hackathon).
- **Classification Engine:** Implement a simple rule-based mapping (e.g., "plastic bottle" -> "recyclable").
- **Integration:** Connect the Node backend `/detect` route to pass the image to the AI model and wait for bounding boxes and labels.

### Phase 4: Frontend Development (Hours 9-14)
- **UI Shell:** Create the Landing Page (`app/page.tsx`), Upload component, and static Dashboard layout using Tailwind CSS.
- **Upload Flow:** Implement drag-and-drop, state management (using React Context or simple state), and API integration to `/detect`.
- **Visualization:** Build the `BoundingBoxCanvas` component using TypeScript to render detections over the image.

### Phase 5: Simulation & Analytics (Hours 15-18)
- **Simulation Engine:** Build the `RobotArm` component using Framer Motion. Create a visual 2D workspace with SVG/CSS where the arm moves to the coordinates of the detected object, picks it, and moves to the color-coded bin.
- **Dashboard:** Feed API data (or mocked real-time state) into Chart.js to update the pie/bar charts immediately after sorting.

### Phase 6: Testing & Bug Bash (Hours 19-21)
- **Integration Testing:** Test the end-to-end flow with real test images (e.g., a photo of a bottle and an apple).
- **UI Polish:** Ensure animations are smooth and the UI looks premium. Fix broken states or unhandled API errors.

### Phase 7: Demo Preparation (Hours 22-24)
- **Seed Data:** Pre-fill the database with impressive mock analytics data so the dashboard looks active.
- **Demo Script:** Rehearse the upload -> detect -> simulate flow. Prepare 3-4 perfect images that the model easily detects to avoid live surprises.

---

## 2. Micro Task Breakdown
Organized for 10-20 minute sprints.

**Setup Tasks:**
- `[Task 1]` Run `npm create vite` for frontend and install Framer Motion & Chart.js.
- `[Task 2]` Initialize backend Node/Express project and install `multer` & `cors`.
- `[Task 3]` Create Neon Postgres DB and run the SQL creation script for 4 main tables.
- `[Task 4]` Scaffold frontend folders (`components`, `pages`, `utils`) and backend folders (`models`, `routes`, `middleware`, `controllers`).

**Backend / AI Tasks:**
- `[Task 5]` Write the `/detect` Express route with Multer to accept image uploads.
- `[Task 6]` Create Python script `infer.py` to accept image path, run YOLOv5s, and print JSON.
- `[Task 7]` Link the `/detect` route to execute `infer.py` via `child_process` (Hackathon shortcut) or HTTP request to a FastAPI instance.
- `[Task 8]` Write a helper function in backend to map YOLO classes to Waste Categories (Wet, Dry, Recycle, Hazardous).
- `[Task 9]` Write the `/analytics` GET route to aggregate detections from the DB.

**Frontend Core Tasks:**
- `[Task 10]` Build `UploadBox.jsx` with an `<input type="file">` and preview frame.
- `[Task 11]` Build API service function to POST the image to the backend.
- `[Task 12]` Build `BoundingBoxCanvas.jsx` to render the image and overlay absolute-positioned `div`s for bounding boxes based on API response.

**Simulation & Dash Tasks:**
- `[Task 13]` Build `RobotArm.jsx` component using `framer-motion` (Create a simple div/SVG that translates X/Y).
- `[Task 14]` Write coordinate-mapping logic (translate image coordinates to UI canvas coordinates for the robot).
- `[Task 15]` Implement sequenced sorting animation (Pick -> Move -> Drop) using `await` with Framer motion controls.
- `[Task 16]` Build `Dashboard.jsx` using `Chart.js` and render hardcoded data.
- `[Task 17]` Connect Dashboard to backend `/analytics` endpoint or derive data from local state to update the chart in real-time.

---

## 3. Folder Structure
A clean and scalable structure for the hackathon MVP.

```text
/smart-waste-robot
│
├── /backend
│   ├── /config          # Database config, environment variables
│   ├── /controllers     # Route logic (e.g., uploadController.js, analyticsController.js)
│   ├── /middleware      # Express middlewares (multer, error handler, validation)
│   ├── /models          # DB Queries/Schemas or ORM models
│   ├── /routes          # Express route definitions (API endpoints)
│   ├── /services        # Core logic, AI script execution/fetching, data mapping
│   ├── /utils           # Helpers (logger, standard response wrappers)
│   └── server.js        # Express entry point
│
└── /frontend
    ├── /src
    │   ├── /assets      # Static images, icons, logos
    │   ├── /components  # Shared UI (UploadBox, RobotArm, BoundingBoxCanvas, Bins)
    │   ├── /pages       # Full views (Home.jsx, Dashboard.jsx)
    │   ├── /services    # Axios/Fetch API calls (api.js)
    │   ├── /hooks       # Custom React hooks (e.g., useFetchAnalytics)
    │   ├── /context     # React Context for global state (Image/Detections state)
    │   ├── /styles      # Vanilla CSS or Tailwind configuration
    │   ├── App.jsx      # Router & Layout wrapper
    │   └── main.jsx     # React entry point
    ├── index.html
    └── package.json
```

---

## 4. Middleware Design
For the backend, the `/middleware` folder will enforce security, validity, and stability.

1. **`upload.middleware.js` (Handling Uploads & Validation):**
   - *What it does:* Uses `multer` to intercept `multipart/form-data`, ensuring the file exists, fits size limits (<5MB), and is a valid image type (JPEG/PNG).
   - *Where to use:* Applied directly on the `POST /detect` route.
2. **`error.middleware.js` (Global Error Handling):**
   - *What it does:* Catches all unhandled exceptions, formats them into a standard JSON response (`{ success: false, error: '...' }`), preventing server crashes.
   - *Where to use:* Registered as the very last middleware in `server.js` (`app.use(errorHandler)`).
3. **`logger.middleware.js` (Request Logging):**
   - *What it does:* Logs the method, URL, status code, and response time (e.g., using `morgan`). Critical during a hackathon to debug failing API calls fast.
   - *Where to use:* Applied globally at the top of `server.js`.
4. **`rateLimit.middleware.js` (Rate Limiting):**
   - *What it does:* Prevents demo spam. Limits users/IPs to X requests per minute.
   - *Where to use:* Applied globally or specifically on the `/detect` API to prevent the AI model from being overloaded.
5. **`auth.middleware.js` (Authentication - Optional/Future):**
   - *What it does:* Verifies JWT tokens to restrict access to admin analytics or API consumption.
   - *Where to use:* (Skip for MVP unless time permits) Applied on restricted routes.

---

## 5. Development Order
To maximize efficiency and ensure a working demo at all times:

1. **Backend Skeleton & Mock API (High Priority)**
   - Start by building the Express server and setting up a hardcoded `/detect` endpoint that returns fake YOLO data. This completely unblocks the frontend team.
2. **Frontend Upload & Visualization (High Priority)**
   - Build the UI to upload an image and draw bounding boxes using the mock API data. Ensures the core visual of "AI Detection" is ready.
3. **AI Pipeline (High Priority)**
   - In parallel, wrap the YOLOv5 model. Test it independently. Once working, replace the mock API in step 1 with the real AI output.
4. **Simulation Engine (Medium Priority - The WOW Factor)**
   - With real bounding boxes rendering on the frontend, build the UI animation to move a fake robotic arm to those coordinates and drag the item to a colored bin.
5. **Database & Analytics Dashboard (Medium Priority)**
   - Connect the DB to store results and serve the `/analytics` endpoint. Build the frontend charts. (If time runs out, dashboard data can be managed in React local state).
6. **Polish & UX (Low Priority)**
   - Add loaders, error messages, and smooth transitions.

---

## 6. MVP Feature Selection
### Critical Features (Must Have for Demo)
- **Image Upload:** UI to drag/drop or select an image.
- **AI Detection Output:** Visual bounding boxes with confident scores drawn over the uploaded image.
- **Rule-based Classification:** Mapping detected objects (e.g., bottle) to their bin (e.g., blue/recycle).
- **2D UI Simulation:** A visual representation (even a simple CSS/Framer outline) moving an object from the image to a colored bin.
- **Dashboard Summary:** A simple chart summarizing the session's detected waste.

### Skippable Features (Drop if Time is Limited)
- **3D Three.js Simulation:** Stick to 2D animations (Framer Motion/CSS) to save massive amounts of time.
- **User Authentication & Accounts:** Hackathon judges don't care about a login screen unless it's a security hackathon. Drop it.
- **Real-Time Camera Feed:** Hard to guarantee high FPS processing in 24 hours. Sticking to static image uploads is safer and easier to script for a demo.
- **Physical Robot / IoT:** Simulating it fully on the frontend satisfies the "Robot" requirement for a software MVP.

---

## 7. Hackathon Optimization
Strategies to finish quickly and impressively:

- **AI Fast-Track:** Don't train a custom model from scratch. Use a pre-trained `YOLOv5s` or `YOLOv8n` (nano) model out of the box. They already detect "bottles", "cups", "apples" etc. Map these existing classes to waste categories manually via a JavaScript dictionary.
- **Fake the Database (If Needed):** If setting up Postgres/Neon takes too long, use an in-memory SQLite (`better-sqlite3`) or just an array in Node.js for the session. The judges won't inspect your connection strings, they want to see the UI.
- **Component Reuse:** Re-use the same "Card" component across the dashboard. Re-use generic layouts. Use a minimal CSS framework or pre-built UI library (e.g., `shadcn/ui` or `Mantine`) so you don't write CSS from scratch.
- **AI Code Generation:** Use AI tools specifically for writing boilerplate (e.g., "Generate an express setup with multer", "Give me Framer Motion code to animate a div from point A to point B").
- **The "Demo Path" Strategy:** Select 3 perfect sample images (e.g., a clear plastic bottle, a clear banana peel) before the demo. Hard-test the system on these 3 images to ensure 100% precision during judging. Provide these images on the UI as "Try Demo Image" buttons.
