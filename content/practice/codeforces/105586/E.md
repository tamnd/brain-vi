---
title: "CF 105586E - \u9ed1\u5854\u7684\u5947\u7269"
description: "Chúng ta được yêu cầu sắp xếp các mục $n^2$ trên một lưới $n lần n$. Mỗi mục có một loại từ $1$ đến $n$ và mỗi loại xuất hiện chính xác $n$ lần, vì vậy multiset hoàn toàn đồng nhất."
date: "2026-06-22T17:55:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105586
codeforces_index: "E"
codeforces_contest_name: "\u201c\u534e\u4e3a\u676f\u201d 2024 \u5e74\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66 ACM \u65b0\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u51b3\u8d5b\uff09"
rating: 0
weight: 105586
solve_time_s: 53
verified: true
draft: false
---

[CF 105586E - \u9ed1\u5854\u7684\u5947\u7269](https://codeforces.com/problemset/problem/105586/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu sắp xếp$n^2$các mục trên một$n \times n$lưới. Mỗi mục có một loại từ$1$ĐẾN$n$và mỗi loại xuất hiện chính xác$n$lần, vì vậy multiset hoàn toàn đồng nhất. 

Sau khi đặt chúng, mỗi hàng và mỗi cột có một giá trị liên quan được tính toán như XOR theo bit của$n$số bên trong nó. Giá trị này được gọi là “độ không ổn định” của hàng hoặc cột đó. Nếu bất kỳ hàng hoặc cột nào vượt quá ngưỡng đã chọn$m$, cấu hình được coi là không an toàn. 

Nhiệm vụ là thiết kế một vị trí của tất cả các số và chọn$m$sao cho XOR tối đa trên tất cả các hàng và cột được giảm thiểu. Chúng ta phải xuất ra cả mức tối thiểu có thể này$m$và một sự sắp xếp để đạt được nó. 

Ràng buộc$n < 500$Và$n$lẻ có nghĩa là lưới tối đa là khoảng$500 \times 500$, vì vậy chúng tôi có đủ khả năng$O(n^2)$sự thi công. Bất kỳ nỗ lực nào để khám phá các hoán vị của hàng hoặc cột đều không khả thi ngay lập tức vì số lượng sắp xếp là vô cùng lớn, vì vậy lời giải phải khai thác cấu trúc đại số của XOR thay vì tổ hợp. 

Một trường hợp thất bại tinh vi sẽ xuất hiện nếu người ta giả định rằng việc phân phối các số một cách tùy tiện hoặc ngẫu nhiên sẽ “kết quả trung bình” XOR. XOR không tính trung bình và sự mất cân bằng một bit sẽ lan truyền một cách xác định. Ví dụ, với$n = 3$, đặt các hàng làm hoán vị của$[1,2,3]$không có cấu trúc có thể tạo ra các XOR hàng như$1 \oplus 2 \oplus 3 = 0$, nhưng các cột có thể trở nên không nhất quán tùy theo cách căn chỉnh. Ràng buộc rằng mỗi giá trị xuất hiện chính xác$n$Times gợi ý rằng cần phải có một cấu trúc đối xứng, điển hình là kiểu hình vuông Latinh. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là liệt kê tất cả các hoán vị của mỗi hàng với ràng buộc là mỗi số xuất hiện chính xác$n$lần tổng thể, sau đó tính toán XOR hàng và cột và lấy mức tối đa tối thiểu có thể. Ngay cả khi hạn chế các hoán vị hình vuông Latinh hợp lệ, số lượng ứng cử viên vẫn xấp xỉ$(n!)^n$, vượt xa mọi giới hạn tính toán ngay cả đối với$n$. Nút thắt không phải là đánh giá mà là sự bùng nổ tổ hợp của các vị trí hợp lệ. 

Quan sát quan trọng là XOR hoạt động tuyến tính trên các hàng và cột và tính đối xứng có thể được thực thi bằng cách sử dụng cấu trúc tuần hoàn. Vì mỗi số xuất hiện chính xác$n$lần, chúng tôi muốn mỗi số xuất hiện chính xác một lần trong mỗi cột và mỗi hàng. Điều này tự nhiên gợi ý việc xây dựng một hình vuông Latin. 

Một hình vuông tuần hoàn Latin tiêu chuẩn hoạt động: xác định ô$(i, j)$BẰNG$(i + j) \bmod n + 1$. Điều này đảm bảo mỗi hàng và cột là một hoán vị của$1 \dots n$, do đó mỗi số xuất hiện chính xác một lần trên mỗi hàng và cột. Vì mỗi hàng chứa tất cả các số$1 \dots n$, hàng XOR giống hệt nhau cho mỗi hàng và giữ nguyên cho các cột. 

Do đó, vấn đề giảm xuống việc tính toán XOR của$1 \oplus 2 \oplus \dots \oplus n$, trở thành mức tối thiểu có thể$m$. Bất kỳ hình vuông Latinh hợp lệ nào đều thực thi tính đồng nhất và trong số các cấu trúc như vậy, cấu trúc tuần hoàn này là tối ưu và đơn giản nhất. 

Ý tưởng cốt lõi là tần số cân bằng là chưa đủ; Cấu trúc cân bằng trên cả hai chiều buộc các giá trị XOR giống hệt nhau ở mọi nơi, loại bỏ khả năng một hàng hoặc một cột chiếm ưu thế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | O(n^2) | Quá chậm | 
| Xây dựng Quảng trường Latin theo chu kỳ | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một$n \times n$lưới nơi mỗi ô$(i, j)$được gán giá trị$(i + j) \bmod n + 1$. Điều này đảm bảo mỗi hàng sẽ duyệt qua tất cả các giá trị theo thứ tự đã dịch chuyển, đảm bảo mỗi số xuất hiện chính xác một lần trên mỗi hàng. 
2. Vì cấu trúc đối xứng giữa các hàng nên mỗi cột cũng chứa mỗi giá trị đúng một lần. Điều này tránh sự phân bố không đồng đều có thể gây ra sự mất cân bằng XOR. 
3. Tính XOR của các số$1$bởi vì$n$. Giá trị này là XOR của bất kỳ hàng hoặc cột nào, vì mỗi hàng là một hoán vị của tất cả các số. 
4. Xuất XOR này dưới dạng$m$, theo sau là lưới được xây dựng. 

### Tại sao nó hoạt động 

Việc xây dựng bắt buộc rằng mỗi hàng và mỗi cột là một hoán vị của cùng một tập hợp$\{1, 2, \dots, n\}$. XOR là bất biến khi hoán vị nên mỗi hàng XOR có cùng giá trị toàn cục$S = 1 \oplus 2 \oplus \dots \oplus n$và điều tương tự cũng xảy ra với các cột. Vì tất cả các giá trị XOR hàng và cột đều giống hệt nhau nên độ không ổn định tối đa bằng$S$, và không có sự sắp xếp nào có thể làm tốt hơn vì mọi con số đều phải xuất hiện$n$lần, buộc mỗi hàng trung bình theo nghĩa XOR phải bao gồm tất cả các đóng góp chính xác một lần trên mỗi hàng trong bất kỳ cấu trúc cân bằng hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

grid = [[0] * n for _ in range(n)]

for i in range(n):
    for j in range(n):
        grid[i][j] = (i + j) % n + 1

m = 0
for i in range(1, n + 1):
    m ^= i

print(m)
for row in grid:
    print(*row)
```Việc xây dựng sử dụng công thức trực tiếp, do đó không có logic quay lui hoặc logic có điều kiện. Sự tinh tế duy nhất là ghi nhớ sự dịch chuyển modulo bằng$+1$, vì các giá trị được lập chỉ mục 1. Tính toán XOR là một phép tính tuyến tính đơn giản trên$1$ĐẾN$n$. 

Lựa chọn thiết kế chính là sử dụng$(i + j) \bmod n$, đảm bảo một cấu trúc dịch chuyển tuần hoàn hoàn hảo. Bất kỳ hoán vị thay thế nào trên mỗi hàng sẽ yêu cầu tính nhất quán cẩn thận giữa các cột, nhưng công thức này mã hóa đồng thời cả hai ràng buộc. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 3$. 

Chúng tôi xây dựng lưới: 

| tôi \ j | 0 | 1 | 2 | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | 3 | 
| 1 | 2 | 3 | 1 | 
| 2 | 3 | 1 | 2 | 

XOR hàng: 

| Hàng | Giá trị | XOR | 
| --- | --- | --- | 
| 0 | 1 2 3 | 0 | 
| 1 | 2 3 1 | 0 | 
| 2 | 3 1 2 | 0 | 

XOR cột: 

| Cột | Giá trị | XOR | 
| --- | --- | --- | 
| 0 | 1 2 3 | 0 | 
| 1 | 2 3 1 | 0 | 
| 2 | 3 1 2 | 0 | 

Ở đây tất cả các hàng và cột đều tạo ra XOR giống hệt nhau, xác nhận thuộc tính đồng nhất. 

Bây giờ hãy xem xét$n = 5$. Hàng đầu tiên là$1,2,3,4,5$, thứ hai được dịch chuyển$2,3,4,5,1$, vân vân. Mỗi hàng chứa tất cả các số đúng một lần, vì vậy mỗi hàng XOR là$1 \oplus 2 \oplus 3 \oplus 4 \oplus 5 = 1$. 

Điều này chứng tỏ rằng cấu trúc có quy mô không thay đổi về mặt logic; chỉ có chiều dài chu kỳ tăng lên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | mỗi ô lưới được tính một lần và XOR được tính trên n giá trị | 
| Không gian | O(n^2) | lưu trữ cho toàn bộ lưới đầu ra | 

Sự ràng buộc$n < 500$làm cho$n^2 = 250{,}000$, điều này không đáng kể cả về thời gian và bộ nhớ đối với Python. Giải pháp chỉ thực hiện số học đơn giản trên mỗi ô nên nó chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import builtins

    output = io.StringIO()
    sys.stdout = output

    n = int(sys.stdin.readline().strip())

    grid = [[0] * n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            grid[i][j] = (i + j) % n + 1

    m = 0
    for i in range(1, n + 1):
        m ^= i

    print(m)
    for row in grid:
        print(*row)

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# minimal case
assert run("1\n") == "1\n1"

# n = 3
assert run("3\n") == "0\n1 2 3\n2 3 1\n3 1 2"

# n = 5 structural check (only m and dimensions matter)
out = run("5\n").splitlines()
assert out[0] == "1"
assert len(out) == 6

# larger sanity check
out = run("7\n").splitlines()
assert out[0] == str(1 ^ 2 ^ 3 ^ 4 ^ 5 ^ 6 ^ 7)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | ô đơn | độ đúng cơ sở | 
| 3 | hình vuông tuần hoàn | tính đúng đắn của mẫu | 
| 5 | Tính nhất quán XOR | khả năng mở rộng | 
| 7 | Tính chính xác của công thức XOR | xác minh lớn hơn | 

## Vỏ cạnh 

cho$n = 1$, lưới chứa một phần tử duy nhất$1$. XOR hàng và cột duy nhất là$1$, Vì thế$m = 1$. Công thức xây dựng mang lại$(0+0)\bmod 1 + 1 = 1$, do đó nó xử lý trường hợp suy biến mà không cần sửa đổi. 

Đối với các giá trị lẻ nhỏ như$n = 3$, vị trí ngẫu nhiên ngây thơ có thể vô tình đáp ứng các ràng buộc về hàng nhưng lại phá vỡ tính nhất quán của cột. Sự thay đổi theo chu kỳ đảm bảo cả hai chiều vẫn được đồng bộ hóa và mọi XOR hàng và cột đều thu gọn về cùng một giá trị ổn định, ngăn chặn mọi sự mất cân bằng tiềm ẩn.
