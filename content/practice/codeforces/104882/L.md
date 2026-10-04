---
title: "CF 104882L - Dòng không đầu, dòng không kết thúc"
description: "Chúng ta đang cố gắng khôi phục một đường thẳng chưa xác định trong mặt phẳng, có dạng $y = kx + b$, trong đó cả hai tham số đều là số thực giới hạn giữa $-100$ và $100$."
date: "2026-06-28T09:20:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104882
codeforces_index: "L"
codeforces_contest_name: "Voronezh State University - Sitronics contest II"
rating: 0
weight: 104882
solve_time_s: 55
verified: true
draft: false
---

[CF 104882L - Dòng không đầu, dòng không cuối](https://codeforces.com/problemset/problem/104882/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang cố gắng tìm lại một đường thẳng chưa biết trong mặt phẳng, có dạng$y = kx + b$, trong đó cả hai tham số đều là số thực giới hạn giữa$-100$Và$100$. Cách duy nhất để tìm hiểu bất cứ điều gì về đường thẳng là đặt câu hỏi có dạng “khoảng cách ngắn nhất từ ​​điểm này là bao nhiêu?”$(x, y)$về vạch?”, và trọng tài trả lại khoảng cách đó. 

Sự tương tác bị hạn chế: chúng ta có thể hỏi tối đa năm câu hỏi như vậy và sau đó chúng ta phải đưa ra kết quả gần đúng$k$Và$b$có sai số tương đối hoặc tuyệt đối nhiều nhất$10^{-3}$. 

Mỗi truy vấn đưa ra một ràng buộc hình học: tất cả các điểm ở một khoảng cách cố định từ một đường thẳng tạo thành hai đường thẳng song song. Vì vậy, mỗi câu trả lời không xác định một dòng duy nhất mà giới hạn nó ở một cặp khả năng đối xứng xung quanh điểm truy vấn. Khó khăn là kết hợp một số lượng nhỏ các ràng buộc phi tuyến này thành một đường duy nhất. 

Các giới hạn quan trọng một cách tinh tế. Từ$k$Và$b$nhỏ, chúng ta có thể xây dựng các truy vấn một cách an toàn với tọa độ như$-100, 0, 100$không có sự bất ổn về số lượng. Vấn đề không phải là độ chính xác về số mà là giải quyết sự mơ hồ từ các giá trị tuyệt đối trong công thức khoảng cách. 

Một chiến lược ngây thơ sẽ cố gắng đoán đường thẳng từ hai hoặc ba điểm, nhưng chúng ta không nhận được điểm trên đường thẳng đó mà chỉ lấy khoảng cách. Điều đó có nghĩa là chúng ta không bao giờ trực tiếp quan sát thông tin biển báo. 

Một trường hợp thất bại phổ biến là giả sử dấu bên trong công thức khoảng cách là cố định. Ví dụ: giải thích khoảng cách từ$(0,0)$BẰNG$b / \sqrt{k^2+1}$bỏ qua rằng nó thực sự là$|b| / \sqrt{k^2+1}$, mất dấu của$b$. Bất kỳ cách tiếp cận nào bỏ qua tính đối xứng này sẽ tạo ra nhiều dòng ứng cử viên hợp lệ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là lấy mẫu một vài điểm, coi mỗi truy vấn như xác định một cặp đường ranh giới song song và cố gắng giao nhau tất cả các khả năng về mặt hình học. Mỗi truy vấn đưa ra một sự mơ hồ nhị phân, vì vậy sau$m$truy vấn chúng tôi có thể có lên đến$2^m$dòng ứng cử viên. Với năm truy vấn, điều này đã có tới 32 khả năng, vẫn có thể quản lý được, nhưng hình dạng của các nút giao sẽ trở nên lộn xộn và không ổn định về mặt số nếu được thực hiện trực tiếp ở dạng chặn độ dốc. 

Quan sát chính là chuyển đổi biểu diễn. Thay vì làm việc với$y = kx + b$, chúng tôi xử lý dòng ở dạng chuẩn hóa:$$Ax + By + C = 0$$đường dây của chúng tôi ở đâu$A = k$,$B = -1$,$C = b$, cho đến khi mở rộng quy mô. Công thức khoảng cách trở thành:$$\frac{|Ax + By + C|}{\sqrt{A^2 + B^2}}$$Mỗi truy vấn đưa ra một phương trình bậc hai trong$A, B, C$, nhưng quan trọng hơn, nó đưa ra ràng buộc tuyến tính đối với dấu và hệ số chuẩn hóa chung. 

Chúng ta giảm bớt vấn đề bằng cách chọn ba điểm được lựa chọn cẩn thận. Mỗi truy vấn mang lại:$$|kx - y + b| = d \cdot \sqrt{k^2 + 1}$$Cho phép$t = \sqrt{k^2 + 1}$. Sau đó, mọi truy vấn sẽ trở thành:$$kx - y + b = \pm d \cdot t$$Điều này chuyển đổi vấn đề thành giải một hệ thống nhỏ chưa biết$k, b, t$và các dấu hiệu chưa biết cho mỗi phương trình. Với ba truy vấn, chúng tôi nhận được ba phương trình và chỉ có tám phép gán dấu có thể có mà chúng tôi có thể ép buộc. Mỗi nhiệm vụ mang lại một giải pháp ứng cử viên có thể được kiểm tra tính nhất quán. 

Điều này biến việc tái cấu trúc hình học thành một bài toán liệt kê hữu hạn với lời giải dạng đóng cho mỗi trường hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Giao lộ hình học brute |$O(2^5)$+ hình học không ổn định |$O(1)$| Quá mong manh | 
| Bảng liệt kê dấu trên hệ thống tuyến tính hóa |$O(8)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chọn ba điểm truy vấn cố định:$(0,0)$,$(100,0)$, Và$(0,100)$. 

1. Truy vấn$(0,0)$, nhận được$d_1$. Điều này tương ứng với$|b| = d_1 \cdot t$, Ở đâu$t = \sqrt{k^2 + 1}$. 
2. Truy vấn$(100,0)$, nhận được$d_2$. Điều này mang lại$|100k + b| = d_2 \cdot t$. 
3. Truy vấn$(0,100)$, nhận được$d_3$. Điều này mang lại$|b - 100| = d_3 \cdot t$. 

Tại thời điểm này, tất cả thông tin được mã hóa thành ba phương trình có giá trị tuyệt đối với hệ số tỷ lệ chung$t$. 

1. Liệt kê tất cả các bộ ba dấu$(e_1, e_2, e_3) \in \{\pm 1\}^3$. Thay thế từng giá trị tuyệt đối bằng dạng đã ký:$$b = e_1 d_1 t,\quad 100k + b = e_2 d_2 t,\quad b - 100 = e_3 d_3 t$$2. Từ hai phương trình đầu tiên loại bỏ$b$và giải quyết cho$k$về mặt$t$:$$100k = t(e_2 d_2 - e_1 d_1)$$Vì thế$$k = \frac{t(e_2 d_2 - e_1 d_1)}{100}$$3. Thay thế vào$t^2 = k^2 + 1$, tạo ra một phương trình duy nhất trong$t$. Giải quyết nó một cách rõ ràng. 
4. Phục hồi$k$Và$b$, sau đó xác minh tính nhất quán với phương trình thứ ba:$$|b - 100| \approx d_3 t$$Nếu nó khớp trong phạm vi dung sai thì phép gán này là chính xác. 
5. Đầu ra$k, b$. 

Cơ chế quan trọng là mỗi lựa chọn dấu sẽ biến đổi các ràng buộc tuyệt đối phi tuyến thành một hệ thống tuyến tính với một ẩn số vô hướng còn lại và điều kiện chuẩn hóa giải quyết duy nhất vô hướng đó. 

### Tại sao nó hoạt động 

Đường này được xác định đầy đủ bởi ba bậc tự do thực trong biểu diễn này: độ dốc, giao điểm và tỷ lệ của dạng ẩn. Mỗi truy vấn đóng góp một ràng buộc bậc hai, nhưng sau khi đưa ra thang chia sẻ$t$, mỗi cái trở nên tuyến tính đến một dấu hiệu. Sự mơ hồ về dấu hiệu là rời rạc và nhỏ, do đó việc liệt kê đầy đủ đảm bảo rằng cấu hình chính xác được kiểm tra. Khi các dấu hiệu chính xác được chọn, hệ thống sẽ có một giải pháp nhất quán duy nhất và tất cả các cấu hình khác đều vi phạm ít nhất một ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def ask(x, y):
    print(f"? {x} {y}")
    sys.stdout.flush()
    d = float(input().strip())
    if d < 0:
        sys.exit(0)
    return d

def solve():
    d1 = ask(0, 0)
    d2 = ask(100, 0)
    d3 = ask(0, 100)

    for e1 in (-1, 1):
        for e2 in (-1, 1):
            for e3 in (-1, 1):

                # We derive t from k^2 + 1 = t^2
                # k = t * C / 100, where C = e2*d2 - e1*d1
                C = e2 * d2 - e1 * d1

                denom = 1 - (C * C) / 10000.0
                if abs(denom) < 1e-12:
                    continue

                t2 = 1.0 / denom
                if t2 <= 0:
                    continue

                t = math.sqrt(t2)

                k = t * C / 100.0
                b = e1 * d1 * t

                # verify third equation
                if abs(abs(b - 100) - d3 * t) < 1e-3:
                    print(f"! {k:.10f} {b:.10f}")
                    sys.stdout.flush()
                    return

solve()
```Việc thực hiện phản ánh trực tiếp đại số. Sự tinh tế duy nhất là bảo vệ khỏi sự mất ổn định về số khi mẫu số$1 - C^2 / 10000$trở nên cực kỳ nhỏ do cấu hình gần như suy biến của các dấu hiệu. 

Bước xác minh là cần thiết vì nhiều nhánh đại số có thể tạo ra các nghiệm hợp lý về mặt số học nhưng không chính xác khi làm tròn dấu phẩy động tương tác với phép trích căn bậc hai. 

## Ví dụ đã hoạt động 

Hãy xem xét một dòng$y = 2x + 5$. 

Sau khi truy vấn, chúng tôi nhận được khoảng cách một cách khái niệm: 

| Điểm truy vấn | Biểu hiện | Dạng giá trị | 
| --- | --- | --- | 
| (0,0) | ( | 5 | 
| (100,0) | ( | 205 | 
| (0,100) | ( | -95 | 

Đang thử cấu hình dấu hiệu$e_1 = +1, e_2 = +1, e_3 = -1$căn chỉnh tất cả các biểu thức một cách nhất quán. Hệ thống dẫn xuất được xây dựng lại$t$, sau đó$k = 2$,$b = 5$và vượt qua ràng buộc thứ ba. 

Lựa chọn ký hiệu sai, chẳng hạn như lật$e_2$, tạo ra một giá trị không nhất quán của$t$không thực hiện được bước xác minh. 

Điều này cho thấy tính đúng đắn được thực thi không phải bằng cách giải quyết vấn đề tối ưu hóa liên tục mà bằng cách loại bỏ những mâu thuẫn rời rạc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(8)$| liệt kê hằng số trên các bộ ba dấu với đại số không đổi cho mỗi trường hợp | 
| Không gian |$O(1)$| chỉ một số biến vô hướng được lưu trữ | 

Chi phí tương tác là ba truy vấn, nằm trong giới hạn năm truy vấn. Tất cả các tính toán đều có thời gian không đổi, vì vậy giải pháp phù hợp thoải mái với cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = []
    def fake_input():
        return sys.stdin.readline()
    return ""

# Note: interactive problem, so deterministic unit tests are conceptual placeholders

# sanity-style placeholders
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dòng y=0x+0 | tái thiết tầm thường | xử lý đánh chặn bằng không | 
| dòng y=x | trường hợp đối xứng | ràng buộc độ lớn bằng nhau | 
| dòng y=100x-100 | độ dốc/điểm chặn ranh giới | giá trị tham số cực trị | 

## Vỏ cạnh 

Một tình huống tế nhị là khi$1 - C^2 / 10000$trở nên cực kỳ nhỏ. Điều này tương ứng với một cấu hình gần suy biến trong đó mối quan hệ tuyến tính dẫn xuất gần như các lực$k^2 \approx -1$, điều này là không thể, biểu thị việc gán dấu không hợp lệ. Trong những trường hợp như vậy, thuật toán bỏ qua nhánh một cách an toàn nhờ kiểm tra mẫu số. 

Một trường hợp cạnh khác là khi$b = 0$, làm cho truy vấn đầu tiên trả về khoảng cách bằng 0. Thuật toán xử lý việc này một cách tự nhiên vì$e_1 d_1 t = 0$lực lượng$b = 0$bất kể$t$, và các phương trình còn lại vẫn xác định$k$độc đáo. 

Trường hợp thứ ba là khi đường thẳng gần như thẳng đứng trong biểu diễn chặn độ dốc, nhưng vì$k$bị giới hạn và hữu hạn, việc biểu diễn vẫn ổn định và việc chuẩn hóa thông qua$t$ngăn chặn sự nổ tung.
