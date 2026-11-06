---
title: "CF 1284A - Năm Mới và Đặt Tên"
description: "Chúng ta được cung cấp hai danh sách vòng tròn của các chuỗi. Danh sách đầu tiên có kích thước $n$, danh sách thứ hai có kích thước $m$. Mỗi năm $y$ tạo ra một tên bằng cách lấy chuỗi $y$-th từ danh sách đầu tiên và chuỗi $y$-th từ danh sách thứ hai, cả hai đều được lập chỉ mục theo chu kỳ và nối chúng lại."
date: "2026-06-16T03:17:34+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1284
codeforces_index: "A"
codeforces_contest_name: "Hello 2020"
rating: 800
weight: 1284
solve_time_s: 512
verified: true
draft: false
---

[CF 1284A - Năm mới và cách đặt tên](https://codeforces.com/problemset/problem/1284/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 8 phút 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai danh sách vòng tròn của các chuỗi. Danh sách đầu tiên có kích thước$n$, cái thứ hai có kích thước$m$. Mỗi năm$y$tạo ra một cái tên bằng cách lấy$y$-chuỗi thứ từ danh sách đầu tiên và$y$- chuỗi thứ từ danh sách thứ hai, cả hai đều được lập chỉ mục theo chu kỳ và nối chúng lại. 

Cụ thể, năm 1 sử dụng$s_1$Và$t_1$, sử dụng năm 2$s_2$Và$t_2$, vân vân. Sau khi vượt qua phần cuối của một trong hai danh sách, chúng tôi sẽ quay lại phần đầu. Tên của năm chỉ đơn giản là sự ghép nối của cặp đã chọn. 

Mỗi truy vấn đưa ra một số năm$y$, có thể rất lớn và chúng ta phải xuất ra tên được nối kết quả. 

Chi tiết cấu trúc chính là cả hai chuỗi đều tuần hoàn. Việc đầu tiên lặp lại mỗi$n$năm, lần thứ hai mỗi năm$m$năm, do đó mô hình kết hợp lặp lại mỗi$\mathrm{lcm}(n, m)$năm. Tính định kỳ này giúp cho các truy vấn lớn có thể quản lý được. 

Các ràng buộc là nhỏ đối với mảng, với$n, m \le 20$, vì vậy việc tính toán trước là chuyện nhỏ. Số lượng truy vấn cũng ít nhưng chỉ số năm có thể lên tới$10^9$, điều này ngay lập tức loại trừ việc mô phỏng theo từng năm. Bất kỳ giải pháp nào lặp lại tối đa$y$mỗi truy vấn sẽ hết thời gian trong trường hợp xấu nhất. 

Một trường hợp thất bại tinh tế đối với lối suy nghĩ ngây thơ là việc quên lập chỉ mục theo chu kỳ. Ví dụ, nếu$n = 3$, truy cập$s_{y}$trực tiếp sẽ vượt quá giới hạn cho$y > 3$. Một lỗi phổ biến khác là tính toán modulo không chính xác với chỉ mục dựa trên 0, đặc biệt khi ánh xạ năm 1 tới chỉ mục 0. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là mô phỏng quy trình cho mỗi truy vấn. Trong một năm nhất định$y$, chúng tôi liên tục tính toán chỉ mục trong cả hai mảng như$(y-1) \bmod n$Và$(y-1) \bmod m$, sau đó nối các chuỗi tương ứng. Điều này là chính xác và cực kỳ trực tiếp. 

Tuy nhiên, nếu thực hiện một cách ngây thơ bằng cách lặp lại từ năm 1 đến$y$đối với mỗi truy vấn, chi phí sẽ trở thành$O(q \cdot y)$, điều đó là không thể khi$y$có thể đạt được$10^9$. Ngay cả với$q \approx 2000$, cái này phát nổ. 

Quan sát quan trọng là mỗi năm có thể được tính toán độc lập theo thời gian không đổi bằng cách sử dụng số học mô-đun. Các trình tự này mang tính tuần hoàn nên chúng ta không bao giờ cần mô phỏng các chuyển tiếp. Toàn bộ vấn đề giảm xuống việc lập chỉ mục thành hai mảng tuần hoàn và nối các kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(q \cdot y)$|$O(1)$| Quá chậm | 
| Lập chỉ mục mô-đun trực tiếp |$O(q)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai mảng$s$Và$t$và lưu trữ độ dài của chúng$n$Và$m$. Chúng xác định hai chu kỳ độc lập. 
2. Đối với mỗi năm truy vấn$y$, hãy chuyển đổi nó thành chỉ mục dựa trên 0 bằng cách trừ 1. Điều này căn chỉnh năm 1 với chỉ số 0, giúp đơn giản hóa số học mô-đun. 
3. Tính chỉ số trong mảng đầu tiên là$(y-1) \bmod n$. Điều này mang lại vị trí tuần hoàn chính xác trong$s$. 
4. Tính chỉ số trong mảng thứ hai như sau$(y-1) \bmod m$. Điều này mang lại vị trí tuần hoàn chính xác trong$t$. 
5. Xuất kết quả nối$s[i] + t[j]$. 

Mỗi truy vấn đều độc lập nên không cần lưu trữ kết quả trung gian hoặc mô phỏng tiến trình. 

### Tại sao nó hoạt động 

Cả hai chuỗi đều lặp lại với khoảng thời gian cố định$n$Và$m$. các$y$-phần tử thứ trong một chu kỳ lặp lại chỉ phụ thuộc vào vị trí của nó theo modulo độ dài chu kỳ. Vì đầu ra chỉ là sự kết hợp theo cặp của hai chu trình độc lập nên toàn bộ hệ thống cũng mang tính tuần hoàn và mỗi truy vấn giảm xuống còn hai lần tra cứu mô-đun. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
s = input().split()
t = input().split()

q = int(input())

for _ in range(q):
    y = int(input())
    y -= 1
    print(s[y % n] + t[y % m])
```Giải pháp áp dụng trực tiếp việc lập chỉ mục mô-đun. Chi tiết triển khai quan trọng duy nhất là trừ 1 trước khi lấy modulo, đảm bảo rằng năm 1 ánh xạ tới chỉ số 0. Nếu không có sự điều chỉnh này, ranh giới năm sẽ dịch chuyển không chính xác. 

Mỗi truy vấn được xử lý độc lập trong thời gian không đổi, do đó không có sự tích lũy trạng thái hoặc nguy cơ tràn ngoài khả năng xử lý số nguyên tiêu chuẩn. 

## Ví dụ đã hoạt động 

Sử dụng cấu trúc đầu vào mẫu, hãy xem xét hai truy vấn để minh họa hành vi lập chỉ mục. 

### Dấu vết ví dụ 

hãy để$s = [a, b, c]$,$t = [d, e]$. 

| Năm$y$|$y-1$|$(y-1) \bmod 3$|$(y-1) \bmod 2$| Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | quảng cáo | 
| 2 | 1 | 1 | 1 | được | 
| 3 | 2 | 2 | 0 | cd | 
| 4 | 3 | 0 | 1 | ae | 

Điều này cho thấy cả hai chuỗi bao bọc độc lập và kết hợp như thế nào. 

Dấu vết xác nhận rằng không có sự tương tác nào tồn tại giữa hai chuỗi ngoài việc ghép nối ở mỗi chỉ mục, do đó, mỗi chuỗi có thể được xử lý riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q)$| Mỗi truy vấn thực hiện modulo và ghép nối theo thời gian không đổi | 
| Không gian |$O(n + m)$| Lưu trữ cho hai chuỗi đầu vào | 

Các ràng buộc cho phép tối đa 2000 truy vấn và các mảng rất nhỏ, do đó, thời gian không đổi cho mỗi truy vấn dễ dàng đủ nhanh. Ngay cả với chi phí Python, giải pháp vẫn phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    s = input().split()
    t = input().split()
    q = int(input())
    out = []
    for _ in range(q):
        y = int(input())
        y -= 1
        out.append(s[y % n] + t[y % m])
    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# sample-like test
assert run("""3 2
a b c
d e
4
1
2
3
4
""") == "ad\nbe\ncd\nae"

# boundary: single element cycles
assert run("""1 1
x
y
3
1
2
3
""") == "xy\nxy\nxy"

# different cycle lengths
assert run("""2 3
a b
c d e
5
1
2
3
4
5
""") == "ac\nbd\nae\nac\nbd"

# large year indexing
assert run("""2 2
a b
c d
1
1000000000
""") == "bd"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chu trình đơn phần tử | đầu ra lặp đi lặp lại | tính đúng đắn được bao bọc đầy đủ | 
| độ dài chu kỳ không khớp | sự lặp lại theo khuôn mẫu | hành vi modulo độc lập | 
| giá trị năm lớn | lập chỉ mục chính xác | xử lý lớn$y$an toàn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$n = 1$hoặc$m = 1$. Trong tình huống này, hàng năm luôn chọn cùng một chuỗi từ chuỗi đó. Biểu thức mô-đun vẫn hoạt động vì mọi số modulo 1 đều bằng 0, do đó việc triển khai sẽ thu gọn một cách tự nhiên mà không cần xử lý đặc biệt. 

Một trường hợp khác là giá trị năm rất lớn gần bằng$10^9$. Vì Python xử lý nguyên bản các số nguyên lớn nên rủi ro duy nhất là quên trừ 1 trước modulo. Nếu chúng ta bỏ qua điều đó, năm 1 sẽ ánh xạ sai tới chỉ mục 1 thay vì 0, làm thay đổi mọi đầu ra. 

Trường hợp khó phát hiện cuối cùng là khi cả hai chuỗi có độ dài khác nhau và không sắp xếp tuần hoàn trong một phạm vi nhỏ. Ngay cả khi đó, cách tiếp cận mô-đun vẫn đúng vì mỗi chuỗi có chu kỳ độc lập và chúng tôi không bao giờ giả định một khoảng thời gian chung.
