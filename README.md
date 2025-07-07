# BÁO CÁO BÀI TẬP LỚN HỆ NHÚNG

Dự án thiết kế một thiết bị cân kỹ thuật số sử dụng vi điều khiển STM32F429-DISC. Nó đọc dữ liệu từ một cảm biến lực thông qua ADC, xử lý các phép đo và hiển thị trọng lượng đã tính toán trên màn hình LED 7 thanh. Trọng lượng cũng được truyền qua UART về máy tính kết hợp với tính năng quét thẻ RFID để định danh người phục vụ lưu trữ và theo dõi cân nặng theo thời gian.

## GIỚI THIỆU

__Đề bài__: Thiết kế cân đo trọng lượng người, hiển thị trên màn hình LED 7 thanh và gửi kết quả về PC qua UART. Tính năng mở rộng: ghét nổi RC522 để quét thẻ RFID, định danh người cân, lưu trữ và theo dõi cân nặng theo thời gian

__Sản phẩm:__
1. Thiết kế chế tạo cân đo trọng lượng của người, hiển thị trên màn hình LED 7 thanh
2. Gửi kết quả cân nặng về máy tính (PC) thông qua UART.
3. Quét thẻ RFID để định danh người cân, lưu trữ và theo dõi cân nặng theo thời gian
- Ảnh chụp minh họa:\
  ![Ảnh minh họa](https://soict.hust.edu.vn/wp-content/uploads/logo-soict-hust-1-1024x416.png)

## TÁC GIẢ

- Tên nhóm: 10h37
- Thành viên trong nhóm
  |STT|Họ tên|MSSV|Công việc|
  |--:|--|--|--|
  |1|Lê Văn Được|20225296|   |
  |2|Nguyễn Hoàng Giang|20225125|  |
  |3|Hoàng Trung Hải|20225307|   |

## MÔI TRƯỜNG HOẠT ĐỘNG

- **Module CPU/dev kit:** STM32F429-DISC
- **Kit, module phần cứng được sử dụng**
  + **STM32F429I Discovery kit**
  + **Module HX711:** bộ chuyển đổi tín hiệu ADC để đọc tín hiệu từ cảm biến lực, chuyển đổi lực tác động thành tín hiệu điện
  + **Module RC522:** sử dụng để đọc và ghi dữ liệu cho thẻ RFID / NFC
- **Phần mềm và công cụ phát triển**
  + **STM32CubeIDE:** môi trường phát triển tích hợp (IDE) dùng để lập trình và debug vi điều khiển STM32.
  + **Hercules:** giao tiếp, kiểm tra và mô phỏng các kết nối nối tiếp (UART)

## SƠ ĐỒ SCHEMATIC

![Untitled Sketch 2_bb](https://github.com/user-attachments/assets/2107ced8-3b4a-4645-872c-b71cd2a6ddce)



### TÍCH HỢP HỆ THỐNG

- Mô tả các thành phần phần cứng và vai trò của chúng: máy chủ, máy trạm, thiết bị IoT, MQTT Server, module cảm biến IoT...
- Mô tả các thành phần phần mềm và vai trò của chúng, vị trí nằm trên phần cứng nào: Front-end, Back-end, Worker, Middleware...

### ĐẶC TẢ HÀM

- Giải thích một số hàm quan trọng: ý nghĩa của hàm, tham số vào, ra


  
- File 7seg.c

  
```cpp
void Set7SegDisplayValue(int val)
{
    DisplayValue = val;
    pos = 0;
}
/*
  HÀM Set7SegDisplayValue(int val) (7seg.c)

  . MỤC ĐÍCH: Thiết lập giá trị cần hiển thị trên LED 7 đoạn

  . THAM SỐ:
      - Input: int val - Giá trị số cần hiển thị (0-99)
      - Output: không có (void)

  . HOẠT ĐỘNG/CHỨC NĂNG:
     - DisplayValue = val;  // Lưu giá trị cần hiển thị
     - pos = 0;               // Reset vị trí multiplexing
 */
```
```cpp
void Run7SegDisplay()
{
	unsigned char val;
	pos++;
	HAL_GPIO_WritePin(PORT_7SEG_CONTROL0, PIN_7SEG_CONTROL0, GPIO_PIN_RESET);
	HAL_GPIO_WritePin(PORT_7SEG_CONTROL1, PIN_7SEG_CONTROL1, GPIO_PIN_RESET);

	if (pos & 0x1)
		val = Mask[DisplayValue % 10];
	else
		val = Mask[(DisplayValue / 10) % 10];

	if (val & 0x80)
		HAL_GPIO_WritePin(PORT_7SEG_P, PIN_7SEG_P, GPIO_PIN_SET);
	else
		HAL_GPIO_WritePin(PORT_7SEG_P, PIN_7SEG_P, GPIO_PIN_RESET);

	if (val & 0x40)
		HAL_GPIO_WritePin(PORT_7SEG_G, PIN_7SEG_G, GPIO_PIN_SET);
	else
		HAL_GPIO_WritePin(PORT_7SEG_G, PIN_7SEG_G, GPIO_PIN_RESET);

	if (val & 0x20)
		HAL_GPIO_WritePin(PORT_7SEG_F, PIN_7SEG_F, GPIO_PIN_SET);
	else
		HAL_GPIO_WritePin(PORT_7SEG_F, PIN_7SEG_F, GPIO_PIN_RESET);

	if (val & 0x10)
		HAL_GPIO_WritePin(PORT_7SEG_E, PIN_7SEG_E, GPIO_PIN_SET);
	else
		HAL_GPIO_WritePin(PORT_7SEG_E, PIN_7SEG_E, GPIO_PIN_RESET);

	if (val & 0x8)
		HAL_GPIO_WritePin(PORT_7SEG_D, PIN_7SEG_D, GPIO_PIN_SET);
	else
		HAL_GPIO_WritePin(PORT_7SEG_D, PIN_7SEG_D, GPIO_PIN_RESET);

	if (val & 0x4)
		HAL_GPIO_WritePin(PORT_7SEG_C, PIN_7SEG_C, GPIO_PIN_SET);
	else
		HAL_GPIO_WritePin(PORT_7SEG_C, PIN_7SEG_C, GPIO_PIN_RESET);

	if (val & 0x2)
		HAL_GPIO_WritePin(PORT_7SEG_B, PIN_7SEG_B, GPIO_PIN_SET);
	else
		HAL_GPIO_WritePin(PORT_7SEG_B, PIN_7SEG_B, GPIO_PIN_RESET);

	if (val & 0x1)
		HAL_GPIO_WritePin(PORT_7SEG_A, PIN_7SEG_A, GPIO_PIN_SET);
	else
		HAL_GPIO_WritePin(PORT_7SEG_A, PIN_7SEG_A, GPIO_PIN_RESET);


	if (pos & 0x1)
		HAL_GPIO_WritePin(PORT_7SEG_CONTROL0, PIN_7SEG_CONTROL0, GPIO_PIN_SET);
	else
		HAL_GPIO_WritePin(PORT_7SEG_CONTROL1, PIN_7SEG_CONTROL1, GPIO_PIN_SET);
}

/*
  HÀM Run7SegDisplay() (7seg.c)

  . MỤC ĐÍCH: Thực hiện multiplexing để hiển thị 2 digit trên LED 7 đoạn

  . THAM SỐ:
      - Input: Không có
      - Output: không có (void)

  . HOẠT ĐỘNG/CHỨC NĂNG:
      pos++;  // Tăng bộ đếm vị trí để chuyển đổi giữa các chữ số

      // Tắt tất cả LED 7 đoạn
      HAL_GPIO_WritePin(PORT_7SEG_CONTROL0, PIN_7SEG_CONTROL0, GPIO_PIN_RESET);
      HAL_GPIO_WritePin(PORT_7SEG_CONTROL1, PIN_7SEG_CONTROL1, GPIO_PIN_RESET);
      
      // Chọn chữ số hiển thị dựa trên vị trí quét
      if (pos & 0x1)
          val = Mask[DisplayValue % 10];      // Vị trí lẻ: hàng đơn vị
      else
          val = Mask[(DisplayValue / 10) % 10]; // Vị trí chẵn: hàng chục
      
      // Xuất mã LED cho từng đoạn (kiểm tra từng bit và điều khiển GPIO tương ứng)
      if (val & 0x80) HAL_GPIO_WritePin(PORT_7SEG_P, PIN_7SEG_P, GPIO_PIN_SET);
      if (val & 0x40) HAL_GPIO_WritePin(PORT_7SEG_G, PIN_7SEG_G, GPIO_PIN_SET);
      // ... tương tự cho các đoạn F, E, D, C, B, A
      
      // Bật LED tương ứng với vị trí đang quét
      if (pos & 0x1)
          HAL_GPIO_WritePin(PORT_7SEG_CONTROL0, PIN_7SEG_CONTROL0, GPIO_PIN_SET);
      else
          HAL_GPIO_WritePin(PORT_7SEG_CONTROL1, PIN_7SEG_CONTROL1, GPIO_PIN_SET);

   . NGUYÊN LÝ HOẠT ĐỘNG: Sử dụng kỹ thuật multiplexing - chỉ hiển thị 1 chữ số tại 1 thời điểm nhưng chuyển đổi nhanh để mắt thấy cả 2 chữ số sáng đồng thời.
 */

```

- File main.c
```cpp
void microDelay(uint16_t delay)
{
  __HAL_TIM_SET_COUNTER(&htim2, 0);
  while (__HAL_TIM_GET_COUNTER(&htim2) < delay);
}
/*
  HÀM microDelay(uint16_t delay) (main .c)

  . MỤC ĐÍCH: Tạo độ trễ chính xác ở mức microsecond cho giao tiếp HX711
  . THAM SỐ:
      - Input: uint16_t delay - Thời gian delay (0-65535 μs)
      - Output: không có (void)

  . HOẠT ĐỘNG/CHỨC NĂNG:
     
	  __HAL_TIM_SET_COUNTER(&htim2, 0);          // Reset bộ đếm Timer 2 về 0
	  while (__HAL_TIM_GET_COUNTER(&htim2) < delay); // Chờ đợi cho đến khi bộ đếm đạt giá trị delay

	- Sử dụng Timer 2 làm timebase cho độ trễ chính xác
	- Độ trễ được tính bằng microsecond dựa trên tần số timer
 */
```
  
### KẾT QUẢ

- Các ảnh chụp với caption giải thích.
- Hoặc video sản phẩm
