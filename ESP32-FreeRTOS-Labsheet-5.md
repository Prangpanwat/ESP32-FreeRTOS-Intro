# การทดลอง ESP32 FreeRTOS 
##  ฟังก์ชัน vTaskSuspend() และ vTaskResume()

จากการทดลองที่แล้ว `vTaskDelete(MySecondTaskHandle);` จะลบ `MySecondTaskHandle` ออกจาก task  list และไม่สามารถเรียกกลับมาได้ใหม่ เรามีวิธีการที่จะหยุดและรันต่อ โดยใช้คำสั่ง `vTaskSuspend()` และ `vTaskResume()`

1. แก่ไข Code จากโปรแกรมที่แล้ว โดยการเพิ่ม task เข้าไปอีก 1 task (ดู comment ใน code)

```c  ![image](https://github.com/Prangpanwat/ESP32-FreeRTOS-Intro/assets/116150897/fbeda464-0b72-453d-8abf-55ad07af34b1)

...

void My_First_Task(void * arg)
{
	uint32_t i = 0;
	while(1)
	{
		printf("Hello My First Task %d\n",i);
		vTaskDelay(1000/portTICK_RATE_MS);
		i++;

		if(i == 5)
		{
			vTaskSuspend(MySecondTaskHandle);
			printf("Second Task suspended\n");
		}

		if(i == 10)
		{
			vTaskResume(MySecondTaskHandle);
			printf("Second Task Resumed\n");
		}

		if(i == 15)
		{
			vTaskDelete(MySecondTaskHandle);
			printf("Second Task deleted\n");
		}

		if(i == 20)
		{
			printf("MyFirstTaskHandle will suspend itself\n");
			vTaskSuspend(NULL);
		}
	}
}
...
```
โดยปกติเราจะใส่ชื่อ task handle เป็นอาร์กิวเมนต์สำหรับฟังก์ชัน `vTaskDelete()`, `vTaskSuspend()` และ `vTaskResume()`  แต่การใส่อาร์กิวเมนต์เป็น `NULL` เช่น  `vTaskSuspend(NULL);` จะหมายถึงการกระทำกับ task ผู้ออกคำสั่งเอง 

4. รันและบันทึกผลจากโปรแกรมข้างบน วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร

## [>> ต่อไป >>](./ESP32-FreeRTOS-Labsheet-6.md) 
