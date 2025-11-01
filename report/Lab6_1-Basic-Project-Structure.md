```bash
# ดูขนาด binary
idf.py size
```
# บันทึกภาพตาราง ใส่ในไฟล์ส่งงาน
![](https://github.com/user-attachments/assets/a1ad121b-25c7-40c5-a8e6-4e6fdeb9801e)


# ดูรายละเอียดขนาดตาม component
idf.py size-components

![](https://github.com/user-attachments/assets/6deae4f1-cd96-4641-bc92-a6c62399beb7)


```bash
## การทดลองเพิ่มเติม
idf.py size
```
บันทึกผลการ simulate ในโฟลเดอร์ส่งงาน
![](https://github.com/user-attachments/assets/8e7d9c05-ea17-4b1d-8265-43139b8899b0)



## 🔍 คำถามทบทวน

1. **Docker vs Native Setup**: อธิบายข้อดีของการใช้ Docker เปรียบเทียบกับการติดตั้ง ESP-IDF บน host system
- Docker ไม่ต้องติดตั้ง ESP-IDF และ toolchain บนเครื่องจริง , Environment เหมือนกันทุกเครื่อง (ป้องกัน version conflict) , ลบหรือ reset ได้ง่าย ไม่ทำให้ระบบเสีย
- Native ไม่ต้องเปิด container ใช้งานเร็วกว่าเล็กน้อย , เข้าถึง hardware ได้โดยตรง (เช่น USB flashing)
2. **Build Process**: อธิบายขั้นตอนการ build ของ ESP-IDF ใน Docker container ตั้งแต่ source code จนได้ binary
- เขียนโค้ดใน main/*.c - รัน idf.py build - CMake สร้าง build system - Compiler แปลง source → .o - Linker รวมไฟล์เป็น firmware.elf - Convert ELF → BIN - ได้ output ในโฟลเดอร์ build/
3. **CMake Files**: บทบาทของไฟล์ CMakeLists.txt แต่ละไฟล์คืออะไร และทำงานอย่างไรใน Docker environment?
- CMakeLists.txt (root)	ตั้งชื่อโปรเจกต์ + โหลดระบบ build ของ ESP-IDF
- main/CMakeLists.txt	ระบุไฟล์ .c ที่จะ compile
- project.cmake	ระบบ build หลักของ ESP-IDF
4. **Git Ignore**: ไฟล์ .gitignore มีความสำคัญอย่างไรสำหรับ ESP32 project development?
- ป้องกันไฟล์ขยะ เช่น build/, binary, log ไม่ให้ถูก push ขึ้น GitHub
- ลดขนาด repo และทำให้ clone เร็ว
5. **Container Persistence**: ข้อมูลใดบ้างที่จะหายไปเมื่อ restart container และข้อมูลใดที่จะอยู่ต่อ?
- ข้อมูลที่อยู่ใน Docker volume หรือ bind mount Image ของ container Dockerfile settings / ENV / IDF_PATH
6. **Development Workflow**: เปรียบเทียบ workflow การพัฒนาระหว่างการใช้ Docker กับการทำงานบน native system
- Docker เหมาะกับทีมพัฒนาที่ต้องการ environment เหมือนกันทุกเครื่องและจัดการ dependency ง่าย เน้นความร่วมมือในทีมและต้องการความเสถียรของ environment
- Native system เหมาะกับงานที่ต้องการประสิทธิภาพสูงหรือเข้าถึงฮาร์ดแวร์โดยตรง เน้นประสิทธิภาพสูง


