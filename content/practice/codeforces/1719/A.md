---
title: "CF 1719A - Trò Chơi Chip"
description: "Chúng ta có một bảng $n nhân m$ và một con chip bắt đầu ở góc dưới bên trái. Ở mỗi lượt, người chơi chọn chính xác một hướng, lên hoặc phải và di chuyển chip theo số lẻ ô bất kỳ theo hướng đó. Con chip không thể rời khỏi bảng."
date: "2026-06-09T19:33:38+07:00"
tags: ["codeforces", "competitive-programming", "games", "math"]
categories: ["algorithms"]
codeforces_contest: 1719
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 814 (Div. 2)"
rating: 800
weight: 1719
solve_time_s: 99
verified: true
draft: false
---

[CF 1719A - Trò chơi chip](https://codeforces.com/problemset/problem/1719/A) 

**Đánh giá:** 800 
**Tags:** trò chơi, toán học 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một$n \times m$bảng và một con chip bắt đầu ở góc dưới bên trái. Ở mỗi lượt, người chơi chọn chính xác một hướng, lên hoặc phải và di chuyển chip theo số lẻ ô bất kỳ theo hướng đó. Con chip không thể rời khỏi bảng. 

Trò chơi kết thúc khi chip đạt đến vị trí mà từ đó không có nước đi hợp pháp nào tồn tại. Vì người chơi luân phiên nhau và cả hai đều chơi tối ưu, chúng ta cần xác định xem vị trí xuất phát là thắng cho người chơi đầu tiên, Burenka, hay thua cho người chơi đầu tiên, trong trường hợp đó Tonya thắng. 

Đầu vào bao gồm một số bảng trò chơi độc lập. Đối với mỗi bảng, chúng ta phải in ra người chiến thắng nếu chơi hoàn hảo. 

Các ràng buộc là manh mối chính cho thấy một giải pháp toán học tồn tại. Cả hai chiều có thể lớn bằng$10^9$, điều này làm cho bất kỳ cách tiếp cận nào để khám phá các trạng thái bảng đều không thể thực hiện được. Ngay cả một bảng duy nhất cũng chứa tới$10^{18}$tế bào và có thể có$10^4$trường hợp thử nghiệm. Giải pháp phải được rút gọn thành một công thức đơn giản được tính toán theo thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Một lỗi phổ biến là tập trung vào tọa độ chính xác của chip thay vì cấu trúc chẵn lẻ của trò chơi. 

Xét bảng:```
1 1
```Không có ô nào phía trên hoặc bên phải nên người chơi đầu tiên không được di chuyển. Câu trả lời đúng là:```
Tonya
```Việc thực hiện bất cẩn giả định người chơi đầu tiên luôn có ít nhất một nước đi kỳ lạ sẽ thất bại ở đây. 

Một trường hợp phức tạp khác là:```
2 2
```Nước đi hợp pháp duy nhất là tiến lên một bước hoặc đúng một bước. Một trong hai nước đi ngay lập tức cho đối thủ nước đi cuối cùng. Câu trả lời đúng là:```
Tonya
```Nhiều người chơi ban đầu đoán rằng mọi bảng lớn hơn$1 \times 1$đang thắng, điều đó không đúng. 

Một ví dụ cuối cùng là:```
1 4
```Người chơi đầu tiên có thể di chuyển sang phải ba ô và kết thúc trò chơi ngay lập tức. Câu trả lời đúng là:```
Burenka
```Bất kỳ cách tiếp cận nào chỉ dựa trên quy mô hội đồng quản trị thay vì tính ngang giá sẽ khó giải thích được tại sao.$1 \times 4$Và$1 \times 3$cư xử khác đi. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô hình hóa mọi vị trí trên bàn cờ như một trạng thái trò chơi và tính toán xem nó thắng hay thua. Đặt một trạng thái đại diện cho tọa độ hiện tại của chip. Một bang sẽ thắng nếu tồn tại một nước đi đến một bang thua và sẽ thua nếu mọi nước đi hợp pháp đều dẫn đến bang chiến thắng. 

Phép đệ quy này đúng vì nó tuân theo định nghĩa tiêu chuẩn về trò chơi khách quan. Thật không may, nó trở nên không khả thi gần như ngay lập tức. Ngay cả đối với một người khiêm tốn$1000 \times 1000$hội đồng quản trị có một triệu tiểu bang và mỗi tiểu bang có thể có hàng trăm bước đi. Các ràng buộc thực tế cho phép kích thước lên tới$10^9$, khiến cho việc thăm dò trạng thái hoàn toàn không thể thực hiện được. 

Để tìm một mẫu, hãy tính toán một vài bảng nhỏ theo cách thủ công. Cho phép$f(n,m)$biểu thị trạng thái trò chơi khi chip bắt đầu ở góc dưới bên trái của một$n \times m$Cái bảng. 

Vị trí cuối cùng là$f(1,1)$, đang thua. 

Bây giờ hãy kiểm tra các giá trị nhỏ: 

| Ban | Kết quả | 
| --- | --- | 
| 1×1 | L | 
| 1×2 | W | 
| 1×3 | L | 
| 1×4 | W | 
| 2×1 | W | 
| 2×2 | L | 
| 2×3 | W | 
| 3×3 | L | 

Một mô hình rõ ràng xuất hiện. Vị trí ở đâu$n$Và$m$có cùng mức độ chẵn lẻ đang thua. Các vị trí mà sự tương đương của họ khác nhau sẽ giành chiến thắng. 

Tại sao sự ngang bằng lại quan trọng? 

Một nước đi thay đổi chính xác một tọa độ bằng một số lẻ. Việc thêm một số lẻ luôn làm đảo lộn tính chẵn lẻ. Vì vậy, mỗi bước di chuyển sẽ lật tính chẵn lẻ của chính xác một chiều. 

Chỉ biểu diễn một trạng thái bằng cặp chẵn lẻ:$$(n \bmod 2,\; m \bmod 2)$$Trạng thái thua cuộc$(1,1)$tương ứng với bảng thiết bị đầu cuối$1 \times 1$. 

Nếu cả hai chiều có cùng tính chẵn lẻ, mỗi bước di chuyển hợp pháp sẽ thay đổi chính xác một bit chẵn lẻ, tạo ra trạng thái có các tính chẵn lẻ khác nhau. 

Nếu các kích thước có số chẵn lẻ khác nhau thì luôn có một sự dịch chuyển làm cho chúng bằng nhau. Vì việc di chuyển một lượng lẻ sẽ làm lật một số chẵn lẻ, nên chúng ta có thể chọn một nước đi số lẻ hợp pháp để giảm một chiều một cách thích hợp và đạt đến trạng thái chẵn lẻ như nhau. 

Như vậy: 

- Tính chẵn lẻ giống nhau$\rightarrow$thua cuộc. 
- Tính chẵn lẻ khác nhau$\rightarrow$chiến thắng. 

Toàn bộ trò chơi sụp đổ để kiểm tra tính chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ / trạng thái-DP trên các vị trí trên bảng | O(nm) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$Và$m$. 
2. Tính tính chẵn lẻ của cả hai chiều bằng cách sử dụng$n \bmod 2$Và$m \bmod 2$. 
3. Nếu số chẵn lẻ bằng nhau, hãy in`"Tonya"`. 

Những vị trí này đang thua vì mỗi nước đi đều lật đúng một chẵn lẻ và đưa trò chơi đến một vị trí chẵn lẻ khác. 
4. Nếu không, hãy in`"Burenka"`. 

Từ thế cờ khác nhau, người chơi đầu tiên luôn có thể thực hiện một nước đi kỳ quặc để tạo ra các điểm chẵn lẻ bằng nhau, đặt đối thủ vào thế thua. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi động thái hợp pháp đều đảo ngược tính chẵn lẻ của chính xác một chiều. Điều này có nghĩa là trò chơi có thể được phân tích hoàn toàn thông qua cặp chẵn lẻ$(n \bmod 2, m \bmod 2)$. 

Các quốc gia có số chẵn lẻ bằng nhau đang thua vì mỗi nước đi đều đạt đến trạng thái chẵn lẻ khác nhau. Các quốc gia có mức chẵn lẻ khác nhau đang chiến thắng vì tồn tại sự chuyển dịch sang trạng thái ngang bằng nhau. Vì mọi vị trí đều thuộc về chính xác một trong hai loại này nên việc phân loại là đầy đủ và chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, m = map(int, input().split())

    if (n & 1) == (m & 1):
        print("Tonya")
    else:
        print("Burenka")
```Giải pháp chỉ cần sự tương đương của hai chiều. sử dụng`n & 1`Và`m & 1`là một cách nhỏ gọn để tính xem mỗi số là số lẻ hay số chẵn. 

Việc so sánh kiểm tra xem các điểm chẵn lẻ có khớp nhau hay không. Chẵn bằng đồng nghĩa với việc thua vị trí xuất phát nên Tonya thắng. Chẵn lẻ khác nhau có nghĩa là vị trí xuất phát chiến thắng, vì vậy Burenka thắng. 

Không cần mô phỏng và không có lo ngại tràn vì chỉ sử dụng tính chẵn lẻ. Kích thước có thể lớn bằng$10^9$, nhưng việc tính toán vẫn giữ nguyên thời gian không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 6
```| n | m | n % 2 | m% 2 | Cùng chẵn lẻ? | Người chiến thắng | 
| --- | --- | --- | --- | --- | --- | 
| 5 | 6 | 1 | 0 | Không | Burenka | 

Các kích thước có tính chẵn lẻ khác nhau. Burenka có thể di chuyển đến vị trí ngang bằng và buộc phải giành chiến thắng. 

### Ví dụ 2 

đầu vào:```
2 2
```| n | m | n % 2 | m% 2 | Cùng chẵn lẻ? | Người chiến thắng | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 0 | 0 | Có | Tonya | 

Cả hai chiều đều bằng nhau. Mỗi nước đi hợp lệ sẽ thay đổi chính xác một chẵn lẻ và mang lại cho đối thủ thế thắng. Trạng thái bắt đầu đang mất. 

Những ví dụ này minh họa sự bất biến hoàn toàn: chỉ vấn đề chẵn lẻ chứ không phải kích thước bảng thực tế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một so sánh chẵn lẻ cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Ngay cả với$10^4$trường hợp thử nghiệm, giải pháp chỉ thực hiện một số phép tính số học cho mỗi trường hợp. Nó dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())
        ans.append("Tonya" if ((n & 1) == (m & 1)) else "Burenka")

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""6
1 1
1 4
5 6
2 2
6 3
999999999 1000000000
"""
) == """Tonya
Burenka
Burenka
Tonya
Burenka
Burenka"""

# minimum board
assert run(
"""1
1 1
"""
) == "Tonya"

# odd-odd
assert run(
"""1
3 5
"""
) == "Tonya"

# even-even
assert run(
"""1
1000000000 1000000000
"""
) == "Tonya"

# parity mismatch
assert run(
"""1
1 2
"""
) == "Burenka"

# large boundary values
assert run(
"""1
999999999 1000000000
"""
) == "Burenka"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`Tonya`| Vị trí đầu cuối | 
|`3 5`|`Tonya`| Tính chẵn lẻ lẻ lẻ | 
|`1000000000 1000000000`|`Tonya`| Chẵn lẻ chẵn lẻ với giá trị tối đa | 
|`1 2`|`Burenka`| Sự không khớp chẵn lẻ nhỏ nhất | 
|`999999999 1000000000`|`Burenka`| Giá trị chẵn lẻ hỗn hợp lớn | 

## Vỏ cạnh 

### Bảng đơn bào 

đầu vào:```
1
1 1
```Thực thi: 

| n | m | n % 2 | m% 2 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | Tonya | 

Các số chẵn lẻ bằng nhau nên thuật toán đưa ra`Tonya`. Điều này phù hợp với trò chơi vì người chơi đầu tiên không có động thái hợp pháp. 

### Bảng Chẵn Nhỏ 

đầu vào:```
1
2 2
```Thực thi: 

| n | m | n % 2 | m% 2 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 2 | 2 | 0 | 0 | Tonya | 

Thuật toán xác định vị trí chẵn lẻ bằng nhau và trả về`Tonya`. Quả thực, mỗi nước đi đều mang lại cho đối thủ một đòn đáp trả thắng lợi. 

### Bảng một hàng 

đầu vào:```
1
1 4
```Thực thi: 

| n | m | n % 2 | m% 2 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 0 | Burenka | 

Các số chẵn lẻ khác nhau nên thuật toán đưa ra`Burenka`. Người chơi đầu tiên có thể di chuyển sang phải ba ô và ngay lập tức khiến đối thủ không được di chuyển. 

### Giá trị biên tối đa 

đầu vào:```
1
999999999 1000000000
```Thực thi: 

| n | m | n % 2 | m% 2 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 999999999 | 1000000000 | 1 | 0 | Burenka | 

Chỉ kiểm tra tính chẵn lẻ, do đó kích thước cực lớn được xử lý giống hệt như kích thước nhỏ. Thuật toán vẫn chạy trong thời gian không đổi và đưa ra kết quả chính xác`Burenka`.
