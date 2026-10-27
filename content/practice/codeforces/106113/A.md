---
title: "CF 106113A - El Camino del Robot Humanoide"
description: "Robot hình người bắt đầu tại tọa độ (0, 0) trên mặt phẳng 2D và thực hiện một chuỗi chuyển động. Mỗi ký tự trong chuỗi lệnh đại diện cho một nước đi: F di chuyển robot lên trên, tăng y thêm 1. B di chuyển robot xuống dưới, giảm y đi 1."
date: "2026-06-25T11:37:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106113
codeforces_index: "A"
codeforces_contest_name: "Coding Cup TecNM 2025"
rating: 0
weight: 106113
solve_time_s: 34
verified: true
draft: false
---

[CF 106113A - El Camino del Robot Humanoide](https://codeforces.com/problemset/problem/106113/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Robot hình người xuất phát tại tọa độ`(0, 0)`trên mặt phẳng 2D và thực hiện một chuỗi chuyển động. 

Mỗi ký tự trong chuỗi lệnh đại diện cho một nước đi:`F`di chuyển robot lên trên, tăng`y`bằng 1.`B`di chuyển robot xuống dưới, giảm`y`bằng 1.`L`di chuyển robot sang trái, giảm dần`x`bằng 1.`R`di chuyển robot sang phải, tăng`x`bằng 1. 

Sau khi xử lý tất cả các lệnh, chúng ta chỉ quan tâm đến vị trí nằm ngang cuối cùng. Nếu trận chung kết`x`tọa độ là âm, chúng tôi in`"Izquierda"`. Nếu nó dương, chúng tôi in`"Derecha"`. Nếu nó chính xác bằng 0, chúng tôi in`"Linea Recta"`. 

Kích thước đầu vào rất nhỏ. Số lần di chuyển nhiều nhất là 1000, do đó, ngay cả một mô phỏng đơn giản cũng đã đủ nhanh. Một lần truyền qua chuỗi chỉ thực hiện được 1000 thao tác trong trường hợp xấu nhất. 

Nguyên nhân chính của sai lầm là chú ý sai tọa độ. Di chuyển dọc chỉ thay đổi`y`, nhưng đầu ra phụ thuộc hoàn toàn vào`x`. 

Hãy xem xét đầu vào:```
5
FFFFF
```Robot kết thúc lúc`(0, 5)`. Đầu ra đúng là:```
Linea Recta
```Một giải pháp bất cẩn cố gắng phân loại vị trí cuối cùng bằng cả hai tọa độ có thể kết luận sai rằng robot đã di chuyển đi đâu đó và không nên xem xét trên đường thẳng. 

Một sai lầm dễ mắc phải khác là quên rằng chuyển động trái và phải có thể triệt tiêu lẫn nhau.```
4
LRLR
```Vị trí nằm ngang cuối cùng là`0`, vậy đáp án là:```
Linea Recta
```Đơn giản chỉ cần kiểm tra xem một`L`hoặc`R`xuất hiện trong chuỗi sẽ cho kết quả sai. 

Trường hợp cạnh thứ ba xảy ra khi không có chuyển động ngang nào cả.```
3
FBF
```Robot kết thúc lúc`x = 0`, bất chấp chuyển vị thẳng đứng. Câu trả lời vẫn là:```
Linea Recta
```## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng robot chính xác như mô tả. Bắt đầu với`x = 0`Và`y = 0`, xử lý mọi ký tự và cập nhật tọa độ theo quy tắc chuyển động. Sau khi tất cả các lệnh đã được xử lý, hãy kiểm tra giá trị cuối cùng của`x`và in từ tương ứng. 

Vì có tối đa 1000 lệnh nên mô phỏng này chỉ thực hiện 1000 lần cập nhật. Đó là hiệu quả ngay lập tức. 

Một quan sát thậm chí còn nhỏ gọn hơn là câu trả lời cuối cùng chỉ phụ thuộc vào sự khác biệt giữa số lượng`R`lệnh và số lượng`L`lệnh. Di chuyển theo chiều dọc không bao giờ ảnh hưởng đến quyết định. Chúng ta có thể đếm trực tiếp các bước di chuyển sang trái và phải thay vì duy trì cả hai tọa độ. 

Cả hai quan điểm đều dẫn đến cùng một giải pháp thời gian tuyến tính. Phiên bản mô phỏng phản ánh chính xác báo cáo vấn đề và dễ dàng xác minh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N) | O(1) | Đã chấp nhận | 
| Đếm số lần di chuyển trái/phải | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`N`, số lượng lệnh. 
2. Đọc chuỗi lệnh. 
3. Khởi tạo`x = 0`. 
4. Xử lý từng ký tự trong chuỗi. 

Nếu nhân vật là`L`, giảm`x`. 

Nếu nhân vật là`R`, tăng`x`. 

nhân vật`F`Và`B`không ảnh hưởng đến vị trí nằm ngang nên có thể bỏ qua. 
5. Sau khi xử lý toàn bộ chuỗi, kiểm tra`x`. 
6. Nếu`x < 0`, in`"Izquierda"`. 
7. Nếu`x > 0`, in`"Derecha"`. 
8. Nếu không thì in`"Linea Recta"`. 

### Tại sao nó hoạt động 

Tọa độ ngang bắt đầu từ 0. Mọi`L`giảm nó đi một, và mọi`R`tăng nó lên một. Không có thay đổi lệnh nào khác`x`. Sau khi tất cả các lệnh được xử lý, giá trị cuối cùng của`x`chính xác là chuyển vị ngang của robot so với gốc tọa độ. Câu lệnh xác định đầu ra chỉ từ dấu của giá trị này, vì vậy hãy kiểm tra xem`x`là âm, dương hoặc bằng 0 sẽ tạo ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = input().strip()

x = 0

for c in s:
    if c == 'L':
        x -= 1
    elif c == 'R':
        x += 1

if x < 0:
    print("Izquierda")
elif x > 0:
    print("Derecha")
else:
    print("Linea Recta")
```Việc triển khai chỉ giữ tọa độ ngang vì tọa độ dọc không bao giờ ảnh hưởng đến đầu ra. 

Bên trong vòng lặp, chỉ`L`Và`R`sửa đổi trạng thái. Bỏ qua`F`Và`B`hoàn toàn an toàn vì chúng chỉ thay đổi`y`. 

Điều kiện cuối cùng tuân theo thứ tự chính xác mà câu lệnh yêu cầu. Việc kiểm tra các trường hợp âm tính và dương tính trước tiên sẽ để lại khả năng còn lại là`x = 0`, tương ứng với`"Linea Recta"`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
FFLRF
```| Bước | Lệnh | x | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| 1 | F | 0 | 
| 2 | F | 0 | 
| 3 | L | -1 | 
| 4 | R | 0 | 
| 5 | F | 0 | 

Cuối cùng`x = 0`. 

Đầu ra:```
Linea Recta
```Ví dụ này cho thấy chuyển động thẳng đứng không quan trọng. Chỉ có các bước di chuyển trái và phải mới góp phần đưa ra câu trả lời. 

### Ví dụ 2 

đầu vào:```
8
FRFRLLBL
```| Bước | Lệnh | x | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| 1 | F | 0 | 
| 2 | R | 1 | 
| 3 | F | 1 | 
| 4 | R | 2 | 
| 5 | L | 1 | 
| 6 | L | 0 | 
| 7 | B | 0 | 
| 8 | L | -1 | 

Cuối cùng`x = -1`. 

Đầu ra:```
Izquierda
```Dấu vết này chứng tỏ rằng câu trả lời phụ thuộc vào chuyển vị ngang thực chứ không phải tổng số lần di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Một lần đi qua chuỗi lệnh | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Với`N ≤ 1000`, việc quét tuyến tính không khó để thực hiện trong giới hạn. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    s = input().strip()

    x = 0
    for c in s:
        if c == 'L':
            x -= 1
        elif c == 'R':
            x += 1

    if x < 0:
        return "Izquierda\n"
    elif x > 0:
        return "Derecha\n"
    else:
        return "Linea Recta\n"

# provided samples
assert run("5\nFFLRF\n") == "Linea Recta\n", "sample 1"
assert run("4\nFFRR\n") == "Derecha\n", "sample 2"
assert run("8\nFRFRLLBL\n") == "Izquierda\n", "sample 3"

# custom cases
assert run("1\nL\n") == "Izquierda\n", "single left move"
assert run("1\nR\n") == "Derecha\n", "single right move"
assert run("4\nLRLR\n") == "Linea Recta\n", "cancelling moves"
assert run("3\nFBF\n") == "Linea Recta\n", "only vertical moves"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / L`|`Izquierda`| Kích thước tối thiểu, độ dịch chuyển âm | 
|`1 / R`|`Derecha`| Kích thước tối thiểu, chuyển vị dương | 
|`LRLR`|`Linea Recta`| Di chuyển trái và phải hủy bỏ chính xác | 
|`FBF`|`Linea Recta`| Di chuyển theo chiều dọc không được ảnh hưởng đến đáp án | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các bước di chuyển đều theo chiều dọc:```
3
FBF
```Thuật toán bắt đầu với`x = 0`. Mỗi nhân vật đều là`F`hoặc`B`, Vì thế`x`không bao giờ thay đổi. Sau vòng lặp,`x`vẫn bằng 0 và thuật toán in ra:```
Linea Recta
```Điều này đúng vì robot không bao giờ di chuyển sang trái hoặc sang phải. 

Bây giờ hãy xem xét việc hủy các chuyển động ngang:```
4
LRLR
```Các bản cập nhật là:`0 → -1 → 0 → -1 → 0`Giá trị cuối cùng bằng 0, vì vậy đầu ra là:```
Linea Recta
```Mặc dù chuyển động ngang đã xảy ra, nhưng độ dịch chuyển thực bằng 0, đó chính xác là điều mà bài toán yêu cầu chúng ta đánh giá. 

Cuối cùng, hãy xem xét một sự dịch chuyển tiêu cực mạnh mẽ:```
5
LLLLF
```Tọa độ ngang phát triển như sau:`0 → -1 → -2 → -3 → -4`trận chung kết`F`không thay đổi`x`. Từ`x = -4`, thuật toán in:```
Izquierda
```Điều này xác nhận rằng việc di chuyển theo chiều dọc sau khi di chuyển theo chiều ngang không làm thay đổi cách phân loại.
