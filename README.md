# ระบบจัดการลิงค์ (Link Management System) 🚀

ระบบรวมลิงก์โปรแกรมและเว็บไซต์สำหรับองค์กร เชื่อมต่อ Google Sheets ผ่าน Google Apps Script (GAS) และแสดงผลผ่านเว็บ Frontend บน GitHub Pages

---

## 🛠️ สิ่งที่ได้รับการปรับปรุงและอัปเกรด

### 1. ความปลอดภัย (Security)
- **ป้องกัน XSS (Cross-Site Scripting):** เพิ่มฟังก์ชัน `escapeHTML()` และ `sanitizeUrl()` ป้องกันการแทรกโค้ดอันตรายผ่านชื่อเว็บ, คำอธิบาย หรือ URL (เช่น `javascript:...`)
- **Session Token ชั่วคราว:** เมื่อเข้าสู่ระบบ Apps Script จะออก `token` (ผ่าน `CacheService`) อายุ 6 ชั่วโมง และฝั่ง Frontend จะเก็บใน `sessionStorage` แทนการส่งรหัสผ่านจริงไปพร้อมกับทุก Request
- **External Links Security:** เพิ่ม `rel="noopener noreferrer"` ในแท็กลิงก์ภายนอกทั้งหมด

### 2. ความถูกต้องของข้อมูล (Data Integrity)
- **เปลี่ยนมาใช้ Unique ID (`lnk_xxxx`):** ยกเลิกการอ้างอิงตำแหน่งแถว (`row`) ในการแก้ไขหรือลบ เพื่อป้องกันข้อผิดพลาดเวลาแถวใน Google Sheets มีการเลื่อนหรือสลับกัน
- **รองรับ Auto Migration:** หากรันฟังก์ชัน `setupDatabase()` ระบบจะตรวจจับและแทรกคอลัมน์ `ID` ให้ใน Google Sheets เดิมอัตโนมัติ โดยที่ข้อมูลเก่าไม่สูญหาย
- **ป้องกัน Concurrency ด้วย `LockService`:** ป้องกันการเขียนข้อมูลทับกันเวลาที่มีผู้ใช้งานกดคลิก หรือแอดมินแก้ไขข้อมูลพร้อมกัน

### 3. ฟีเจอร์และ UI/UX ใหม่ (New Features)
- **Auto Favicon:** หากผู้ใช้ไม่ได้ระบุ Emoji หรือใส่เป็น URL รูปภาพ ระบบจะดึง Favicon/โลโก้ของเว็บไซต์นั้นๆ มาแสดงผลให้อัตโนมัติ
- **ระบบจัดเรียง (Sorting):** สามารถเลือกเรียงตาม "ค่าเริ่มต้น", "🔥 ยอดนิยม (คลิกสูงสุด)" หรือ "🔤 ตามตัวอักษร" ได้
- **จัดการหมวดหมู่ผ่านหน้าเว็บ (Category Management):** แอดมินสามารถกดปุ่ม "📁 หมวดหมู่" เพื่อเพิ่มหรือลบหมวดหมู่ได้โดยตรง ไม่ต้องเปิดเข้าไปแก้ใน Google Sheets
- **ตัวนับจำนวนลิงก์:** แสดงผลว่ากำลังแสดงกี่รายการจากทั้งหมด
- **รองรับ PWA (Progressive Web App):** เพิ่มไฟล์ `manifest.json` ให้ติดตั้งลงหน้าจอมือถือหรือคอมพิวเตอร์ได้เนียนตายิ่งขึ้น

---

## 📋 ขั้นตอนการติดตั้งและ Deploy

### 1. ฝั่ง Google Apps Script
1. เปิดไฟล์ [Code.gs](file:///d:/appscript/ระบบจัดการลิงค์/Code.gs) แล้วคัดลอกโค้ดทั้งหมดไปวางในโปรเจกต์ Google Apps Script ของคุณ
2. ตรวจสอบตัวแปร `SHEET_ID` ที่บรรทัดแรกว่าตรงกับ Google Sheet ของคุณ
3. สั่งรันฟังก์ชัน **`setupDatabase`** 1 ครั้ง เพื่อให้ระบบสร้างหรืออัปเกรดโครงสร้างตาราง
4. กด **Deploy (การทำให้ใช้งานได้)** > **New deployment (การทำให้ใช้งานได้รายการใหม่)**
   - ประเภท: **Web app (เว็บแอป)**
   - Execute as: **Me (ฉัน)**
   - Who has access: **Anyone (ทุกคน)**
5. คัดลอก **Web app URL** ที่ได้ (ลงท้ายด้วย `/exec`)

### 2. ฝั่ง GitHub Pages (index.html)
1. เปิดไฟล์ [index.html](file:///d:/appscript/ระบบจัดการลิงค์/index.html)
2. ตรวจสอบค่าตัวแปร `API_URL` (ประมาณบรรทัด 286) ให้นำ Web app URL ที่ได้จากข้อ 1 มาใส่
3. อัปโหลดไฟล์ [index.html](file:///d:/appscript/ระบบจัดการลิงค์/index.html) และ [manifest.json](file:///d:/appscript/ระบบจัดการลิงค์/manifest.json) ขึ้น GitHub Repository ของคุณ
4. เข้าไปที่ **Settings > Pages** ใน GitHub แล้วเลือก Branch เพื่อเปิดใช้งาน GitHub Pages
