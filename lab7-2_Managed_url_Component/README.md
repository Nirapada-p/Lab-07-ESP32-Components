# Lab 7-2: Managed Component from GitHub URL Demo

## คำอธิบาย
การทดลองนี้แสดงการใช้งาน managed component จาก GitHub Repository
ใช้ `Sensors` component จาก https://github.com/APPLICATIONS-OF-MICROCONTROLLERS/Lab7_Components

## ผลลัพธ์ที่คาดหวัง
- แสดงข้อความการเริ่มต้น sensor จาก GitHub component
- แสดงข้อมูล temperature และ humidity ทุก 4 วินาที
- แสดงสถานะการทำงานของ sensor
- แสดงแหล่งที่มาของ component (GitHub Repository)

## ความแตกต่างจาก Lab 7-1
- Lab 7-1: ใช้ local component (ในเครื่อง)
- Lab 7-2: ใช้ managed component จาก GitHub URL

## การใช้งาน
1. เข้าไปในโฟลเดอร์ lab7-2_Managed_url_Component
2. รันคำสั่ง `idf.py build` (จะดาวน์โหลด component จาก GitHub อัตโนมัติ)
3. ทดสอบด้วย QEMU

![alt text](image.png)

```c
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "esp_system.h"
#include "esp_log.h"
#include "sensor.h"
#include "display.h"

static const char *TAG = "MAIN";

void app_main(void)
{
    ESP_LOGI(TAG, "🚀 Lab 6.2: Multiple Source Files Demo");
    ESP_LOGI(TAG, "📍 Main function from file: %s, line: %d", __FILE__, __LINE__);
    ESP_LOGI(TAG, "ESP-IDF Version: %s", esp_get_idf_version());
    ESP_LOGI("TEST", "✅ app_main started!");

    // Initialize modules
    sensor_init();
    display_init();

    display_show_message("System Starting...");

    int counter = 0;

    while (1) {
        ESP_LOGI(TAG, "=== Loop %d ===", counter++);

        // Clear display every loop
        display_clear_screen();

        // Read sensor data
        sensor_read_data();

        // Show data on display
        float temp = 25.5 + (float)(counter % 10);
        float humid = 60.0 + (float)(counter % 20);
        display_show_data(temp, humid);

        // Check sensor status every 3 loops
        if (counter % 3 == 0) {
            sensor_check_status();
            display_show_message("Status Check Complete");
        }

        vTaskDelay(pdMS_TO_TICKS(2000)); // 2 seconds delay
    }
}
```