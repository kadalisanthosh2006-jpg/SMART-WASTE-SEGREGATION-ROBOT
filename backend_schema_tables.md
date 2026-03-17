📄 ADVANCED DATABASE DESIGN DOCUMENTATION
🧠 Project: Smart Waste Segregation Robot
🏗️ 1. Database Overview
🎯 Purpose:

Store and manage:

Uploaded images

Detection results

Waste classifications

Analytics data

System logs

🧩 Database Type:

PostgreSQL (Recommended – Neon Cloud)

👉 Why:

Structured data

Strong relationships

Scalable

Perfect for analytics

📊 2. High-Level Entity Relationships
Users (optional)
   ↓
Images → Detections → Categories
   ↓
Processing Logs
   ↓
Analytics
🧱 3. Core Tables (Detailed)
🧑‍💻 3.1 Users Table (Optional for Hackathon)
📌 Table: users
Field	Type	Description
id	UUID (PK)	Unique user ID
name	VARCHAR	User name
email	VARCHAR	User email
created_at	TIMESTAMP	Account creation
🖼️ 3.2 Images Table
📌 Table: images
Field	Type	Description
id	UUID (PK)	Unique image ID
user_id	UUID (FK)	Linked user
image_url	TEXT	Stored image path
uploaded_at	TIMESTAMP	Upload time
status	VARCHAR	(processing/completed/failed)
processing_time	FLOAT	Time taken (seconds)
🔍 3.3 Detections Table
📌 Table: detections

👉 Stores each detected object

Field	Type	Description
id	UUID (PK)	Detection ID
image_id	UUID (FK)	Related image
label	VARCHAR	Object label (e.g., bottle)
confidence	FLOAT	Model confidence
bbox_x	FLOAT	X coordinate
bbox_y	FLOAT	Y coordinate
bbox_width	FLOAT	Width
bbox_height	FLOAT	Height
category_id	UUID (FK)	Waste category
created_at	TIMESTAMP	Detection time
🗂️ 3.4 Categories Table
📌 Table: categories

👉 Master table for waste types

Field	Type	Description
id	UUID (PK)	Category ID
name	VARCHAR	Wet / Dry / Recyclable
color_code	VARCHAR	UI color
description	TEXT	Category details
🤖 3.5 Simulation Logs Table
📌 Table: simulation_logs

👉 Tracks robotic actions

Field	Type	Description
id	UUID (PK)	Log ID
detection_id	UUID (FK)	Related object
action	VARCHAR	pick/move/drop
start_time	TIMESTAMP	Action start
end_time	TIMESTAMP	Action end
status	VARCHAR	success/failure
📊 3.6 Analytics Table
📌 Table: analytics

👉 Aggregated data for dashboard

Field	Type	Description
id	UUID (PK)	Record ID
date	DATE	Day
total_processed	INT	Total objects
wet_count	INT	Wet waste
dry_count	INT	Dry waste
recyclable_count	INT	Recyclable
hazardous_count	INT	Hazardous
📜 3.7 Processing Logs Table
📌 Table: processing_logs

👉 System performance tracking

Field	Type	Description
id	UUID (PK)	Log ID
image_id	UUID (FK)	Related image
processing_time	FLOAT	Time taken
status	VARCHAR	success/failure
error_message	TEXT	Error details
created_at	TIMESTAMP	Timestamp
🔗 4. Relationships
🧠 Key Relations:

images.id → detections.image_id

detections.category_id → categories.id

detections.id → simulation_logs.detection_id

images.id → processing_logs.image_id

📊 ER Flow:
Image
  ↓
Detections
  ↓
Category
  ↓
Simulation Logs

Image → Processing Logs
Image → Analytics (aggregated)
⚡ 5. Indexing Strategy

👉 To improve performance:

Index on:

image_id

category_id

created_at

🚀 6. Optimization Strategy

Use batch inserts for detections

Use Redis caching for:

Frequent analytics queries

Store only metadata (not heavy images)

🔐 7. Security Considerations

Use UUID instead of incremental IDs

Validate input data

Prevent SQL injection (ORM recommended)

🧪 8. Sample Data Flow
📥 Insert Flow:

Image uploaded → stored in images

AI detects objects → stored in detections

Categories assigned → linked via category_id

Simulation runs → stored in simulation_logs

Metrics stored → analytics

📤 9. Example Query
🔹 Get detections for an image:
SELECT d.label, d.confidence, c.name AS category
FROM detections d
JOIN categories c ON d.category_id = c.id
WHERE d.image_id = 'IMAGE_ID';
📈 10. Future Scalability

Add video_frames table for live feed

Add iot_devices table

Add bin_status table (smart bins)