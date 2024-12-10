# IOT
from sense_emu import SenseHat
import time

sense = SenseHat()

Màu sắc
text_colour = (255, 255, 0)  # Màu vàng
bg_color = (0, 0, 0)         # Nền đen
dot_color = (0, 255, 0)      # Màu xanh lá

x = y = 4

def clamp(value, min_value=0, max_value=7):
    return min(max_value, max(min_value, value))

def move_dot(event):
    global x, y
    if event.action in ('pressed', 'held'):
        x = clamp(x + {'left': -1, 'right': 1}.get(event.direction, 0))
        y = clamp(y + {'up': -1, 'down': 1}.get(event.direction, 0))

Hiển thị thông điệp 1 lần
sense.show_message("chinh", text_colour=text_colour, back_colour=bg_color, scroll_speed=0.1)

Vòng lặp chính
try:
    while True:
     Lấy dữ liệu từ cảm biến
        temp = sense.temperature
        humidity = sense.humidity
        
     Xử lý sự kiện joystick
        for event in sense.stick.get_events():
            move_dot(event)

     Hiển thị điểm joystick
        sense.clear()
        sense.set_pixel(x, y, dot_color)

     In ra console
        print(f"Nhiệt độ: {temp:.2f} °C")
        print(f"Độ ẩm: {humidity:.2f} %")
        print(f"Joystick tọa độ: (x={x}, y={y})")
        print("-" * 40)

        time.sleep(1)
except KeyboardInterrupt:
    sense.clear()
    print("Đã dừng chương trình.")
