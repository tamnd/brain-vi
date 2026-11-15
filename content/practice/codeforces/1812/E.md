---
title: "CF 1812E - Không phải là vấn đề hình học"
description: "Chúng ta được cho ba số nguyên nên được hiểu là tọa độ trong không gian ba chiều. Nhiệm vụ là tính toán một số thực duy nhất xuất phát từ ba giá trị này và kết quả mẫu cho biết đại lượng này đại diện cho điều gì: đó là độ dài Euclide của vectơ…"
date: "2026-06-09T08:32:08+07:00"
tags: ["codeforces", "competitive-programming", "*special", "constructive-algorithms", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1812
codeforces_index: "E"
codeforces_contest_name: "April Fools Day Contest 2023"
rating: 0
weight: 1812
solve_time_s: 64
verified: true
draft: false
---

[CF 1812E - Không phải là vấn đề về hình học](https://codeforces.com/problemset/problem/1812/E) 

**Đánh giá:** - 
**Tags:** *đặc biệt, thuật toán xây dựng, hình học, toán học 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba số nguyên nên được hiểu là tọa độ trong không gian ba chiều. Nhiệm vụ là tính toán một số thực duy nhất xuất phát từ ba giá trị này và kết quả đầu ra mẫu cho thấy đại lượng này đại diện cho điều gì: đó là độ dài Euclide của vectơ từ gốc đến điểm$(x, y, z)$. 

Về mặt hình học, đây là khoảng cách đường thẳng từ$(0, 0, 0)$ĐẾN$(x, y, z)$. Theo thuật ngữ đại số, khoảng cách này được xác định hoàn toàn bằng định lý Pythagore mở rộng thành ba chiều. 

Các ràng buộc cực kỳ nhỏ, với mỗi tọa độ được giới hạn trong khoảng từ -1000 đến 1000. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ số học nào liên quan đến hình vuông sẽ vẫn nằm trong giới hạn số nguyên 32 bit. Việc tính toán diễn ra liên tục nên không cần lo lắng về hiệu suất. 

Sự tinh tế chính trong loại vấn đề này không phải là độ phức tạp tính toán mà là độ chính xác về mặt số học. Việc triển khai đơn giản bằng cách sử dụng số học số nguyên là an toàn cho đến bước bình phương, nhưng căn bậc hai cuối cùng phải được tính bằng dấu phẩy động. Nếu thực hiện không đúng cách, đặc biệt bằng cách trộn số nguyên hoặc làm tròn quá sớm, kết quả có thể bị sai lệch. 

Không có trường hợp cạnh cấu trúc thực sự nào ngoài việc xử lý biển báo. Bình phương loại bỏ thông tin dấu hiệu, vì vậy các đầu vào như$(1000, -1000, 0)$cư xử giống hệt với$(1000, 1000, 0)$. Trường hợp ranh giới quan trọng duy nhất là khi tất cả đầu vào bằng 0, trong đó đầu ra phải chính xác bằng 0. 

Một lỗi phổ biến là cố gắng ước chừng giá trị theo cách thủ công hoặc sử dụng logic căn bậc hai số nguyên, điều này sẽ làm mất độ chính xác cần thiết. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là suy nghĩ về mặt hình học: đo khoảng cách giữa hai điểm trong không gian bằng cách phân tách nó thành các thành phần trực giao. Người ta có thể tưởng tượng việc mô phỏng chuyển động dọc theo các trục và tích lũy khoảng cách, nhưng vì các trục vuông góc nên quy tắc tổng hợp chính xác được cố định: mỗi tọa độ đóng góp độc lập thông qua bình phương. 

Một cách tiếp cận đơn giản nhưng vẫn đúng là tính toán rõ ràng khoảng cách bình phương bằng cách tính tổng bình phương của mỗi tọa độ và sau đó lấy căn bậc hai. Đây đã là công thức toán học tối ưu. Bất kỳ nỗ lực nào để phân tách sâu hơn, chẳng hạn như xây dựng hoặc mô phỏng hình học lặp đi lặp lại, đều không cần thiết và sẽ chỉ làm phức tạp thêm giải pháp. 

Quan sát quan trọng là định mức Euclide trong ba chiều được định nghĩa trực tiếp là:$$\sqrt{x^2 + y^2 + z^2}$$Không có cấu trúc ẩn nào ngoài danh tính này. Vấn đề về cơ bản là kiểm tra xem người ta có nhận ra đây là một ứng dụng trực tiếp của công thức khoảng cách trong không gian 3D hay không. 

Do đó, lời giải rút gọn thành việc tính tổng các bình phương theo sau là căn bậc hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tính toán hình học trực tiếp | O(1) | O(1) | Đã chấp nhận | 
| Bất kỳ công trình thay thế nào | O(1) | O(1) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc ba số nguyên$x, y, z$. Chúng đại diện cho một điểm trong không gian 3D so với điểm gốc. 
2. Tính toán đóng góp bình phương của từng tọa độ một cách độc lập. Điều này cô lập từng thành phần trục trước khi kết hợp chúng. 
3. Tính tổng các giá trị bình phương để thu được khoảng cách Euclide bình phương từ gốc tọa độ. 
4. Tính căn bậc hai của tổng này để tìm được khoảng cách hình học thực tế. 
5. Xuất kết quả dưới dạng số dấu phẩy động với độ chính xác mặc định, đủ với dung sai lỗi cho phép rất lớn. 

### Tại sao nó hoạt động 

Công thức khoảng cách Euclide bắt nguồn từ việc áp dụng nhiều lần định lý Pythagore. Đầu tiên, khoảng cách trong$xy$-máy bay là$\sqrt{x^2 + y^2}$. Việc mở rộng điều này sang chiều thứ ba coi khoảng cách phẳng đó là một cạnh của tam giác vuông có chiều cao$z$, cho$\sqrt{(x^2 + y^2) + z^2}$. Vì bình phương loại bỏ hướng và bảo toàn độ lớn nên thứ tự tích lũy không ảnh hưởng đến tính đúng đắn. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    x, y, z = map(int, input().split())
    ans = math.sqrt(x*x + y*y + z*z)
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào trong thời gian không đổi và áp dụng ngay công thức định mức Euclide. sử dụng`math.sqrt`đảm bảo hành vi dấu phẩy động chính xác. Phép nhân được sử dụng thay vì lũy thừa để đạt hiệu quả, mặc dù ở đây cả hai đều tương đương. 

Không có vòng lặp hoặc nhánh, do đó việc triển khai là tối thiểu và tránh mọi rủi ro về lỗi tích lũy hoặc lỗi tích lũy. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi việc tính toán cho hai đầu vào đại diện. 

### Ví dụ 1: (1, 1, 1) 

| Bước | x | y | z | x 2 + y 2 + z 2 | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | 1 | - | - | 
| Bình phương | 1 | 1 | 1 | 3 | - | 
| Căn bậc hai | - | - | - | 3 | 1.7320508075688772 | 

Điều này xác nhận đường chéo đơn vị tiêu chuẩn trong hình lập phương, khớp với giá trị mong đợi. 

### Ví dụ 2: (0, 0, 0) 

| Bước | x | y | z | x 2 + y 2 + z 2 | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 0 | - | - | 
| Bình phương | 0 | 0 | 0 | 0 | - | 
| Căn bậc hai | - | - | - | 0 | 0,0 | 

Điều này xác nhận rằng điểm gốc ánh xạ tới khoảng cách bằng 0. 

Trong cả hai trường hợp, quá trình tính toán hoạt động nhất quán với các kỳ vọng hình học, củng cố rằng không cần xử lý trường hợp đặc biệt nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện bất kể đầu vào | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Với kích thước đầu vào cực nhỏ và các hoạt động liên tục trong thời gian, giải pháp này nằm trong giới hạn tầm thường. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x, y, z = map(int, sys.stdin.readline().split())
    return str(math.sqrt(x*x + y*y + z*z))

# provided sample
assert abs(float(run("1 1 1\n")) - 1.7320508075688772) < 1e-9

# origin
assert abs(float(run("0 0 0\n")) - 0.0) < 1e-9

# axis-aligned
assert abs(float(run("3 0 0\n")) - 3.0) < 1e-9

# mixed signs
assert abs(float(run("-3 4 12\n")) - 13.0) < 1e-9

# symmetric point
assert abs(float(run("1000 1000 1000\n")) - math.sqrt(3000000)) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | √3 | tính đúng đắn cơ bản | 
| 0 0 0 | 0 | trường hợp cạnh gốc | 
| 3 0 0 | 3 | căn chỉnh trục | 
| -3 4 12 | 13 | bộ ba Pythagore cổ điển trong 3D | 
| 1000 1000 1000 | phao lớn | ổn định giới hạn trên | 

## Vỏ cạnh 

Đối với đầu vào`0 0 0`, thuật toán tính toán$0^2 + 0^2 + 0^2 = 0$và căn bậc hai chính xác bằng 0. Việc triển khai xử lý việc này một cách tự nhiên mà không cần bất kỳ sự phân nhánh đặc biệt nào, vì`math.sqrt(0)`trả về 0. 

Đối với tọa độ âm như`-3 4 12`, bình phương đảm bảo dấu được loại bỏ trước khi tổng hợp, do đó việc tính toán tiến hành như sau$9 + 16 + 144 = 169$và căn bậc hai mang lại 13. Điều này xác nhận rằng hướng không ảnh hưởng đến độ lớn và không cần xử lý dấu hiệu có điều kiện. 

Đối với đầu vào lớn bằng nhau như`1000 1000 1000`, tổng bình phương trở thành 3.000.000, vẫn an toàn trong độ chính xác dấu phẩy động đối với số học có độ chính xác kép của Python. Thuật toán không làm mất độ chính xác trong phạm vi dung sai yêu cầu và đầu ra vẫn ổn định.
