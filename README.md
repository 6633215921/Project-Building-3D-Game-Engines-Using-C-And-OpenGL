# Project-Building-3D-Game-Engines-Using-C-And-OpenGL

---

# ✈️ 3D Flight Simulator & Boids Flocking System

โปรเจกต์นี้เป็นการสร้างระบบเกมจำลองการบิน (Flight Simulator) ในพื้นที่กว้างใหญ่ พร้อมระบบจำลองพฤติกรรมฝูงสัตว์ (Boids Flocking Algorithm) ที่ตอบสนองต่อผู้เล่นแบบเรียลไทม์ พัฒนาด้วย C++ และ OpenGL โดยมีการปรับใช้เทคนิคกราฟิกขั้นสูงอย่าง GPU Instancing และการสร้างภูมิประเทศจาก Heightmap

## 🌟 ฟีเจอร์หลัก (Key Features)

โปรเจกต์นี้แสดงผลการทำงานของกราฟิกและระบบเกมเพลย์ดังนี้:

### 1. Boids Flocking System & GPU Instancing (ระบบฝูงนกอัจฉริยะ)
- **Flocking Algorithm:** คำนวณพฤติกรรมของฝูง Boids จำนวน 500 ตัวด้วยกฎ 3 ข้อ: Separation (การแยกตัว), Alignment (การจัดทิศทาง), และ Cohesion (การรวมกลุ่ม) พร้อมเพิ่มพฤติกรรม `FleePredator` เพื่อบินหนีเมื่อเครื่องบินของผู้เล่นเข้าใกล้
- **GPU Instancing:** เรนเดอร์โมเดล Boids จำนวนมากพร้อมกันใน Draw Call เดียว (`glDrawElementsInstanced`) ผ่าน Instance VBO เพื่อรักษาระดับ Framerate (Performance Optimization)

### 2. Procedural Heightmap Terrain (ระบบภูมิประเทศ)
- **Terrain Generation:** สร้างโมเดลพื้นผิว 3 มิติแบบไดนามิกโดยอ่านค่าความสูงจากไฟล์ภาพขาวดำ (Heightmap Image)
- **Dynamic Normals & Multi-Texturing:** คำนวณ Normal เวกเตอร์แบบเรียลไทม์ตามความชัน และผสมผสาน Texture 5 ชนิด (Water, Sand, Grass, Rock, Snow) เข้าด้วยกัน
- **Fog System:** เพิ่มหมอกเชิงปริมาตร (Distance Fog) เพื่อสร้างมิติความลึกให้กับสภาพแวดล้อม

### 3. Flight Mechanics & Gameplay (ระบบการบินและเกมเพลย์)
- **6-DOF Movement:** ควบคุมเครื่องบินได้ทั้ง Pitch, Yaw, Roll, และปรับความเร็ว (Acceleration) พร้อมจำกัดเพดานบิน
- **Stamina System:** ระบบพลังงานที่ลดลงตามความเร็วในการบิน หากพลังงานหมดความเร็วจะตกลง
- **Collision & Collection:** ระบบตรวจสอบการชนแบบวงกว้าง (Radius) ผู้เล่นสามารถบินชน Boids เพื่อ "เก็บสะสม" เติม Stamina และเล่นเสียง Effect (ใช้ `PlaySound` แบบ Asynchronous)

### 4. Dynamic Follow Camera & UI (กล้องติดตามและหน้าต่างสถานะ)
- กล้องจะคำนวณตำแหน่งและวิ่งตามหลังเครื่องบินเสมอ พร้อมเอียงมุมกล้องตามองศาการ Roll ของเครื่องบิน
- **ImGui Integration:** ผสานไลบรารี ImGui เพื่อแสดงหน้าต่างแบบโปร่งใส (HUD) สำหรับบอกจำนวน Boids ที่เหลืออยู่, ความเร็วปัจจุบัน, และหลอดพลังงาน Stamina

## 🛠 เทคโนโลยีที่ใช้ (Tech Stack)

- **Language:** C++ (Standard 11+)
- **Graphics API:** OpenGL 3.3 (Core Profile)
- **Libraries:**
  - `GLFW` (Window & Input handling)
  - `GLAD` (OpenGL Function Loader)
  - `GLM` (Mathematics: Vector & Matrix operations)
  - `Assimp` (3D Model Loading)
  - `stb_image` (Texture & Heightmap Loading)
  - `Dear ImGui` (In-game UI/HUD)
  - `Windows API (mmsystem)` (Audio playback)
- **Shader Language:** GLSL

## 🎮 การควบคุม (Controls)

- **Up / Down Arrow:** เชิดหัวขึ้น / กดหัวลง (Pitch)
- **Left / Right Arrow:** เลี้ยวซ้าย / เลี้ยวขวา (Yaw)
- **Q / E:** เอียงเครื่องซ้าย / เอียงเครื่องขวา (Roll)
- **W / S:** เร่งความเร็ว / ลดความเร็ว
- **R:** รีเซ็ตตำแหน่งเครื่องบินและสถานะทั้งหมด
- **ESC:** ออกจากโปรแกรม

## 📂 โครงสร้างระบบ (System Structure)

- **3D Models:** โหลดโมเดลด้วยคลาส `Model` เช่น เครื่องบินของผู้เล่น (`Player.obj`) และโมเดลฝูงนก (`BoidModel.obj`)
- **Shaders:**
  - `terrain.vs` / `terrain.fs` สำหรับเรนเดอร์ Heightmap, คำนวณแสงแบบ Multi-Texture และระบบหมอก
  - `model.vs` / `model.fs` สำหรับเรนเดอร์โมเดลผู้เล่นและรับค่าแสง
  - `boid.vs` / `boid.fs` สำหรับเรนเดอร์ Boids ผ่านเทคนิค GPU Instancing
  - `skybox.vs` / `skybox.fs` สำหรับเรนเดอร์ท้องฟ้า (Cubemap)
- **Resources:** ใช้ไฟล์ภาพสำหรับสร้าง Terrain (`iceland_heightmap.png`) และไฟล์เสียงเอฟเฟกต์ (`pickup.wav`)

## 📸 ตัวอย่างการทำงาน (Previews)
<img width="1282" height="758" alt="image" src="https://github.com/user-attachments/assets/f32722b6-d394-4af6-b6a5-9b7b4fa5d0cf" />
<video src= "https://github.com/user-attachments/assets/68291c04-5609-41f0-b269-d0613aceb026"></video>




---

**ผู้จัดทำ:** วรพล พันทอง (Worapol Punthong)

**Attribution** 
- Foundaitional Codes : https://learnopengl.com/
- Texture Sky Box : https://opengameart.org/content/sky-box-sunny-day
- Height Map : https://www.flickr.com/photos/onformative/5451441370
- Texture Terrain : https://opengameart.org/
  - grass : https://opengameart.org/content/grass-1
  - rock : https://opengameart.org/content/lunar-rock
  - sand : https://opengameart.org/content/generic-tileable-sand-texture
  - snow : https://opengameart.org/content/seamless-snow-texture-0
  - water : https://opengameart.org/content/texture-water
- Model Boid : https://sketchfab.com/3d-models/flying-bird-eb843194e06d429ebef7dd4aa7e265c1 
- Model Player : https://sketchfab.com/3d-models/psx-harpy-cc96098b51df4c8eb0da738b34211cbe
- Audio : mixkit-arcade-retro-game-over-213 : https://mixkit.co/free-sound-effects/video-game/



