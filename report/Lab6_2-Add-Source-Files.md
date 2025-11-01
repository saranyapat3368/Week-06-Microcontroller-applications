## 🔍 คำถามทบทวน

1. **Multiple Source Files**: เหตุใดต้องแยก source code เป็นหลายไฟล์?
- การแยกโค้ดเป็นหลายไฟล์ช่วยให้โปรแกรมอ่านง่าย แก้ไขง่าย ลดความซ้ำซ้อน และสามารถนำโมดูลไปใช้ซ้ำในโปรเจกต์อื่นได้
2. **CMakeLists.txt Management**: การเพิ่มไฟล์ source ใหม่ต้องแก้ไขอะไรบ้าง?
- เมื่อเพิ่มไฟล์ .c ใหม่ ต้องแก้ main/CMakeLists.txt โดยเพิ่มไฟล์ลงใน SRCS เพื่อให้ ESP-IDF รู้ว่าต้องคอมไพล์ไฟล์นั้น
3. **Header Files**: บทบาทของไฟล์ .h คืออะไร และทำไมต้องมี?
- ไฟล์ .h ทำหน้าที่ประกาศฟังก์ชัน ตัวแปร หรือ struct เพื่อให้ไฟล์ .c อื่นสามารถเรียกใช้ได้โดยไม่เกิด error
4. **Include Directories**: เหตุใด CMakeLists.txt ต้องระบุ INCLUDE_DIRS?
- ต้องระบุ INCLUDE_DIRS "." เพื่อบอกคอมไพล์เลอร์ว่าไฟล์ header (.h) อยู่ในโฟลเดอร์นี้ ไม่เช่นนั้นจะเกิด error file not found
5. **Git Ignore**: ไฟล์ .gitignore ช่วยอะไรในการจัดการ ESP32 project?
.gitignore ช่วยกันไม่ให้ build output และไฟล์ชั่วคราว เช่น /build, .elf, .bin, .vscode ถูก push ขึ้น GitHub ทำให้ repo สะอาดและเล็ก
6. **Task Management**: การใช้ FreeRTOS task ในโมดูล LED ช่วยอะไร?
- การใช้ FreeRTOS task ใน led.c ช่วยให้ LED กะพริบแบบ ทำงานขนาน โดยไม่ไปรบกวนลูปหลักของโปรแกรม
7. **Code Organization**: ข้อดีของการแยกโมดูล sensor, display, led เป็นไฟล์แยกคืออะไร?
การแยกเป็นโมดูล sensor, display, led ช่วยแยกหน้าที่ของโค้ด ลดความยุ่งเหยิง และทำให้ debug ง่ายขึ้น

## 📋 ผลลัพธ์ที่คาดหวัง

เมื่อทำ lab สำเร็จ นักศึกษาจะ:
- **📁 จัดการ Multiple Files**: สามารถสร้างและจัดการหลายไฟล์ source code
- **⚙️ แก้ไข CMakeLists.txt**: เข้าใจการเพิ่ม source files ใน build configuration
- **🔗 Header Files**: เข้าใจการใช้ header files เพื่อเชื่อมโยงระหว่างไฟล์
- **📂 Git Management**: เข้าใจการใช้ .gitignore สำหรับ ESP32 development
- **🏗️ Code Organization**: สามารถแยกโค้ดเป็นโมดูลต่างๆ ตามหน้าที่
- **🔄 Task Management**: เข้าใจการใช้ FreeRTOS tasks สำหรับงานที่ทำงานขนาน
- **📍 Debug Information**: เข้าใจการใช้ __FILE__ และ __LINE__ เพื่อ debug

## 💡 บันทึกผลการทดลอง

**ขั้นตอนที่ 1 (เฉพาะ sensor.c):**
- จำนวนไฟล์ source: main.c, sensor.c
- ขนาด binary: 130  KB
- การทำงาน: แสดงค่า Temperature + Humidity เท่านั้น

**ขั้นตอนที่ 2 (เพิ่ม display.c):**
- จำนวนไฟล์ source: 3 ไฟล์
- ขนาด binary: 145 KB
- การทำงาน: เพิ่มการแสดงข้อความและค่าบน display

**ขั้นตอนที่ 3 (เพิ่ม led.c):**
- จำนวนไฟล์ source: 4 ไฟล์
- ขนาด binary: 165 KB
- การทำงาน: LED เริ่มกะพริบทุก 3 วินาที + แสดงสถานะ LED บน display


