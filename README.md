# BÁO CÁO BÀI TẬP LỚN HỆ NHÚNG

Mô tả nội dung cần báo cáo của môn đồ án

## GIỚI THIỆU

__Đề bài__: Thiết kế cân đo trọng lượng người, hiển thị trên màn hình LED 7 thanh và gửi kết quả về PC qua UART
Mở rộng: quét thẻ RFID để định danh người cân, lưu trữ và theo dõi cân nặng theo thời gian. Ghép nối RC522 để quét thẻ, và built-in Real-time clock (có pin vào chân VBAT) để giữ thời gian khi mạch mất điện."
__Sản phẩm:__
1. Thiết kế chế tạo cân đo trọng lượng của người, hiển thị trên màn hình LED 7 thanh
2. Gửi kết quả cân nặng về máy tính (PC) thông qua UART.
3. Quét thẻ RFID để định danh người cân, lưu trữ và theo dõi cân nặng theo thời gian
- Ảnh chụp minh họa:\
  ![Ảnh minh họa](https://soict.hust.edu.vn/wp-content/uploads/logo-soict-hust-1-1024x416.png)

## TÁC GIẢ

- Tên nhóm:10h37
- Thành viên trong nhóm
  |STT|Họ tên|MSSV|Công việc|
  |--:|--|--|--|
  |1|Lê Văn Được|20225296|   |
  |2|Nguyễn Hoàng Giang|20225125|  |
  |3|Hoàng Trung Hải|20225307|   |

## MÔI TRƯỜNG HOẠT ĐỘNG

- Liệt kê module CPU/dev kit. Ví dụ ESP32 Dev Module, hoặc STM32F429-DISC.
- Liệt kê các kit, module được sử dụng: ví dụ: stm32 cảm biến...

## SO ĐỒ SCHEMATIC

_Cho biết cách nối dây, kết nối giữa các linh kiện_ 
Ví dụ có thể liệt kê dạng bảng
|STM32F429|Module ngoại vi|
|--|--|
|PA0|Nút bấm điều khiển trên board|
|PG13|Led báo trên board|
|PE2|MQ3 SCK|
|PE3|MQ3 SDA|

hoặc dạng ảnh:\
![image](https://github.com/user-attachments/assets/f15f7379-09bf-472a-b0d9-4de6d32a6ec0)

hoặc bất cứ dạng nào thể hiện được cách đấu nối giữa các linh kiện


### TÍCH HỢP HỆ THỐNG

- Mô tả các thành phần phần cứng và vai trò của chúng: máy chủ, máy trạm, thiết bị IoT, MQTT Server, module cảm biến IoT...
- Mô tả các thành phần phần mềm và vai trò của chúng, vị trí nằm trên phần cứng nào: Front-end, Back-end, Worker, Middleware...

### ĐẶC TẢ HÀM

- Giải thích một số hàm quan trọng: ý nghĩa của hàm, tham số vào, ra

  ```C
     /**
      *  Hàm tính ...
      *  @param  x  Tham số
      *  @param  y  Tham số
      */
     void abc(int x, int y = 2);
  ```
  
### KẾT QUẢ

- Các ảnh chụp với caption giải thích.
- Hoặc video sản phẩm
