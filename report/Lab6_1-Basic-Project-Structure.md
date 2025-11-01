
```bash
# ดูขนาด binary
idf.py size

# บันทึกภาพตาราง ใส่ในไฟล์ส่งงาน

# ดูรายละเอียดขนาดตาม component
idf.py size-components

# ถ้ามีปัญหาในการดูรายละเอียดขนาดตาม component บนหน้าจอ ให้ใช้คำสั่ง
idf.py size-components > size-components.txt

# แล้วแนบไฟล์ size-components.txt ในโฟลเดอร์ส่งงาน

# ดูรายละเอียดขนาดตาม source file
idf.py size-files

# ถ้ามีปัญหาในการดูรายละเอียดขนาดตาม source file บนหน้าจอ ให้ใช้คำสั่ง
idf.py size-files > size-files.txt

# แล้วแนบไฟล์ size-files.txt ในโฟลเดอร์ส่งงาน

```

```bash
## การทดลองเพิ่มเติม

### 1. เพิ่ม Build Information (ใน Docker Container)

แก้ไข main/lab6_1_basic_build.c:

```bash
# เข้า container (ถ้ายังไม่ได้เข้า)
docker-compose exec esp32-dev bash
source $IDF_PATH/export.sh
cd lab6_1_basic_build

# แก้ไขไฟล์
```

```c
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "esp_system.h"
#include "esp_log.h"

static const char *TAG = "LAB1";

void print_build_info(void)
{
    ESP_LOGI(TAG, "=== Build Information ===");
    ESP_LOGI(TAG, "Project Name: lab6_1_basic_build");
    ESP_LOGI(TAG, "ESP-IDF Version: %s", esp_get_idf_version());
    ESP_LOGI(TAG, "Compile Date: %s", __DATE__);
    ESP_LOGI(TAG, "Compile Time: %s", __TIME__);
    ESP_LOGI(TAG, "Chip Model: %s", CONFIG_IDF_TARGET);
    ESP_LOGI(TAG, "Free Heap: %d bytes", esp_get_free_heap_size());
}

void app_main(void)
{
    print_build_info();
    
    int counter = 0;
    
    while (1) {
        ESP_LOGI(TAG, "Running... Counter: %d", counter++);
        
        // แสดงสถานะ memory ทุกๆ 10 ครั้ง
        if (counter % 10 == 0) {
            ESP_LOGI(TAG, "Current free heap: %d bytes", esp_get_free_heap_size());
        }
        
        vTaskDelay(pdMS_TO_TICKS(1000));
    }
}
```


บันทึกผลการ simulate ในโฟลเดอร์ส่งงาน


## 🔍 คำถามทบทวน

1. **Docker vs Native Setup**: อธิบายข้อดีของการใช้ Docker เปรียบเทียบกับการติดตั้ง ESP-IDF บน host system
2. **Build Process**: อธิบายขั้นตอนการ build ของ ESP-IDF ใน Docker container ตั้งแต่ source code จนได้ binary
3. **CMake Files**: บทบาทของไฟล์ CMakeLists.txt แต่ละไฟล์คืออะไร และทำงานอย่างไรใน Docker environment?
4. **Git Ignore**: ไฟล์ .gitignore มีความสำคัญอย่างไรสำหรับ ESP32 project development?
5. **Container Persistence**: ข้อมูลใดบ้างที่จะหายไปเมื่อ restart container และข้อมูลใดที่จะอยู่ต่อ?
6. **Development Workflow**: เปรียบเทียบ workflow การพัฒนาระหว่างการใช้ Docker กับการทำงานบน native system

## 📋 ผลลัพธ์ที่คาดหวัง

เมื่อทำ lab สำเร็จ นักเรียนจะ:
- **🐳 เข้าใจการใช้ Docker**: สามารถใช้ Docker สำหรับ ESP32 development ได้
- **🏗️ เข้าใจโครงสร้าง project**: สามารถสร้างและจัดการ ESP32 project structure
- **📂 Git Management**: เข้าใจการใช้ .gitignore สำหรับ ESP32 development
- **⚙️ ใช้ idf.py ได้**: สามารถใช้คำสั่ง idf.py เบื้องต้นใน Docker environment
- **📊 วิเคราะห์ build output**: เข้าใจ build process และสามารถวิเคราะห์ผลลัพธ์ได้
- **🔧 ปรับแต่ง build**: สามารถแก้ไข CMakeLists.txt เพื่อปรับแต่ง build configuration

## 🛠️ การแก้ไขปัญหาที่พบบ่อย

### ปัญหา: Docker Container ไม่ start
```bash
docker-compose up -d
# Error: Cannot connect to Docker daemon
```
**วิธีแก้**: 
- ตรวจสอบว่า Docker Desktop เปิดอยู่
- รัน `docker --version` เพื่อตรวจสอบการติดตั้ง

### ปัญหา: Cannot access /project directory
```bash
bash: cd: /project: No such file or directory
```
**วิธีแก้**: 
- ตรวจสอบ volume mapping ใน docker-compose.yml
- ตรวจสอบว่าอยู่ใน directory ที่ถูกต้องบน host

### ปัญหา: ESP-IDF not found
```bash
idf.py: command not found
```
**วิธีแก้**: 
- รัน `source $IDF_PATH/export.sh` ใน container
- ตรวจสอบ environment variables

### ปัญหา: Permission denied when building
```bash
Permission denied: cannot create directory 'build'
```
**วิธีแก้**: 
- ตรวจสอบ file permissions บน host system
- ใช้ `chmod 755` ให้กับ project directory

### ปัญหา: Container stops immediately
```bash
docker-compose ps
# Shows container as "Exited"
```
**วิธีแก้**: 
- ตรวจสอบ docker-compose.yml configuration
- ใช้ `docker-compose logs esp32-dev` เพื่อดู error logs

Docker environment ที่ setup ในสัปดาห์นี้จะใช้ต่อเนื่องตลอดเทอม
