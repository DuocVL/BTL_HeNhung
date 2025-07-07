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

  
```cpp
/**
 * Hàm Set7SegDisplayValue(int val)
 * 
 * Mục đích: Thiết lập giá trị cần hiển thị trên LED 7 đoạn
 * 
 * Tham số:
 * - Input: int val - Giá trị số cần hiển thị (0-99)
 * - Output: Không có (void)
 * 
 * Hoạt động:
 * cppDisplayValue = val;  // Lưu giá trị cần hiển thị
 * pos = 0;                // Reset vị trí multiplexing
 */
void Set7SegDisplayValue(int val)
{
    DisplayValue = val;
    pos = 0;
}
    
  ```
  
### KẾT QUẢ

- Các ảnh chụp với caption giải thích.
- Hoặc video sản phẩm
