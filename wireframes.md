🎨 WIREFRAMES DOCUMENTATION
🧠 Smart Waste Segregation Robot
🟢 1. LANDING PAGE (Home Screen)
 ------------------------------------------------------
| LOGO              Smart Waste Segregation Robot      |
|------------------------------------------------------|
|                                                      |
|        🧠 AI Waste Detection Platform                |
|  Upload waste images & see smart robotic sorting     |
|                                                      |
|   [ Upload Image ]   [ Try Demo Image ]              |
|                                                      |
|------------------------------------------------------|
|  Features:                                           |
|  ✔ AI Detection   ✔ Simulation   ✔ Analytics         |
|------------------------------------------------------|
| Footer                                               |
 ------------------------------------------------------
🎯 Purpose:

Entry point

CTA for upload

🟡 2. IMAGE UPLOAD SCREEN
 ------------------------------------------------------
| ← Back                 Upload Waste Image            |
|------------------------------------------------------|
|                                                      |
|   ┌──────────────────────────────┐                  |
|   |   Drag & Drop Image Here     |                  |
|   |   or Click to Upload         |                  |
|   └──────────────────────────────┘                  |
|                                                      |
|   Supported: JPG, PNG                               |
|                                                      |
|   [ Upload ]                                         |
|                                                      |
 ------------------------------------------------------
🔵 3. IMAGE PREVIEW SCREEN
 ------------------------------------------------------
| ← Back                Preview Image                  |
|------------------------------------------------------|
|                                                      |
|        [ Uploaded Image Display ]                    |
|                                                      |
|   File: waste.jpg                                    |
|                                                      |
|   [ Analyze Waste 🚀 ]                               |
|                                                      |
 ------------------------------------------------------
⚡ 4. LOADING / PROCESSING SCREEN
 ------------------------------------------------------
| Processing Image...                                  |
|------------------------------------------------------|
|                                                      |
|           ⏳ AI is analyzing waste...                 |
|                                                      |
|           [ Loading Spinner Animation ]              |
|                                                      |
|        Please wait (~1 sec)                          |
|                                                      |
 ------------------------------------------------------
🧠 5. DETECTION RESULT SCREEN
 ------------------------------------------------------
| Detection Results                                    |
|------------------------------------------------------|
|                                                      |
|   [ Image with Bounding Boxes ]                     |
|                                                      |
|   ┌───────────────┐                                 |
|   | Bottle 92%    | → Recyclable                    |
|   | Food 85%      | → Wet                           |
|   └───────────────┘                                 |
|                                                      |
|   [ Start Simulation ▶ ]                            |
|                                                      |
 ------------------------------------------------------
🤖 6. SIMULATION SCREEN (CORE FEATURE)
 ------------------------------------------------------
| Robotic Simulation                                   |
|------------------------------------------------------|
|                                                      |
|      🤖 Robot Arm Visualization                      |
|                                                      |
|     Object → Moving → Bin                           |
|                                                      |
|   🟩 Wet Bin     🟦 Recyclable     🟥 Hazardous       |
|                                                      |
|   Animation Flow:                                    |
|   Pick → Move → Drop                                 |
|                                                      |
|   [ Replay ]     [ Next → Dashboard ]               |
|                                                      |
 ------------------------------------------------------
📊 7. DASHBOARD SCREEN
 ------------------------------------------------------
| Dashboard                                            |
|------------------------------------------------------|
|                                                      |
|   Total Processed: 120                               |
|                                                      |
|   ┌───────────────┐   ┌───────────────┐             |
|   | Pie Chart     |   | Bar Graph     |             |
|   | Waste Dist.   |   | Category Cnt  |             |
|   └───────────────┘   └───────────────┘             |
|                                                      |
|   Stats:                                             |
|   Wet: 30   Dry: 40   Recyclable: 50                 |
|                                                      |
|   [ View History ]                                   |
|                                                      |
 ------------------------------------------------------
🗄️ 8. HISTORY / ANALYTICS SCREEN
 ------------------------------------------------------
| Analytics History                                    |
|------------------------------------------------------|
|                                                      |
|   Date        | Total | Wet | Dry | Recyclable       |
|------------------------------------------------------|
|   17 Mar      | 120   | 30  | 40  | 50               |
|   16 Mar      | 95    | 20  | 30  | 45               |
|                                                      |
|   [ View Trends Graph ]                              |
|                                                      |
 ------------------------------------------------------
⚠️ 9. ERROR STATE SCREENS
❌ Invalid File
[ Error ]
Only JPG/PNG files are allowed
❌ No Detection
No waste detected in the image
Try another image
❌ API Error
Processing failed. Please try again.
📱 10. MOBILE VIEW (Responsive)
 -------------------------
|   Upload Image         |
|------------------------|
|   [ Image Preview ]    |
|                        |
|   [ Analyze ]          |
|                        |
|   Results              |
|   - Bottle → Recycle   |
|                        |
|   [ Simulate ]         |
|                        |
|   Dashboard            |
|   Pie Chart            |
 -------------------------
🔄 11. COMPLETE USER FLOW (WIREFLOW)
Landing Page
     ↓
Upload Image
     ↓
Preview
     ↓
Analyze
     ↓
Detection Results
     ↓
Simulation
     ↓
Dashboard
     ↓
History / Analytics
🎯 12. COMPONENT-LEVEL WIREFRAME
Upload Component
[ UploadBox ]
[ ImagePreview ]
[ Button ]
Detection Component
[ Canvas Overlay ]
[ Labels ]
Simulation Component
[ RobotArm ]
[ WasteItem ]
[ Bin ]
Dashboard Component
[ PieChart ]
[ BarChart ]
[ StatsCard ]