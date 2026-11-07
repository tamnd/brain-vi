---
title: "CF 1353A - Mảng không ổn định nhất"
description: "Chúng ta được cung cấp một mảng có độ dài n và tổng số tiền yêu cầu là m. Nhiệm vụ là gán các số nguyên không âm cho một mảng có độ dài n sao cho tổng của tất cả các phần tử bằng m."
date: "2026-06-11T14:01:03+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1353
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 642 (Div. 3)"
rating: 800
weight: 1353
solve_time_s: 106
verified: true
draft: false
---

[CF 1353A - Mảng không ổn định nhất](https://codeforces.com/problemset/problem/1353/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chiều dài mảng`n`và tổng số tiền cần thiết`m`. Nhiệm vụ là gán các số nguyên không âm cho một mảng có độ dài`n`sao cho tổng của tất cả các phần tử bằng`m`. Trong số tất cả các mảng như vậy, chúng tôi muốn một mảng làm cho tổng “bước nhảy liền kề” càng lớn càng tốt, trong đó mỗi bước nhảy là sự khác biệt tuyệt đối giữa các phần tử lân cận. 

Biểu thức được tối đa hóa đo mức độ dao động của mảng khi đọc từ trái sang phải. Một mảng phẳng tạo ra số 0, trong khi việc xen kẽ giữa các giá trị lớn và nhỏ tạo ra sự đóng góp lớn từ mỗi lần chuyển đổi. 

Các ràng buộc là vô cùng lớn: cả hai`n`Và`m`có thể lên đến`10^9`, và có thể có tới`10^4`trường hợp thử nghiệm. Điều này ngay lập tức loại trừ việc xây dựng các mảng một cách rõ ràng hoặc mô phỏng bất cứ điều gì tỷ lệ thuận với`n`. Bất kỳ giải pháp nào cũng phải giảm từng trường hợp thử nghiệm thành số học có thời gian không đổi. 

Một nỗ lực ngây thơ sẽ cố gắng phân phối`m`trên mảng theo các mẫu khác nhau và tính tổng kết quả của sự khác biệt. Ngay cả khi chúng tôi chỉ cố gắng mô phỏng một mảng ứng viên cho mỗi trường hợp thử nghiệm, việc xây dựng hoặc lặp lại`n`các phần tử là không thể. 

Trường hợp cạnh tinh tế xuất hiện khi`n = 1`. Không có cặp liền kề nên câu trả lời luôn bằng 0 bất kể`m`. Một góc khác là khi`n = 2`, nơi câu trả lời trở nên đơn giản`|a1 - a2|`dưới sự ràng buộc`a1 + a2 = m`, được cực đại hóa bằng cách đẩy toàn bộ khối lượng sang một bên. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là thử tất cả các mảng có độ dài có thể`n`với tổng`m`, tính toán sự khác biệt liền kề của chúng và theo dõi mức tối đa. Ngay cả khi chúng ta giới hạn bản thân ở các thành phần số nguyên của`m`, số lượng khả năng tăng theo cấp số nhân với`m`và kết hợp với`n`. Điều này hoàn toàn không khả thi ngay cả đối với những đầu vào nhỏ. 

Quan sát cấu trúc quan trọng là mọi đơn vị giá trị được đặt trong mảng chỉ có thể góp phần tạo ra sự khác biệt khi nó được di chuyển giữa các vị trí không đối xứng. Để tối đa hóa sự khác biệt liền kề, chúng tôi muốn mảng xen kẽ giữa các giá trị cao và thấp càng nhiều càng tốt. Tuy nhiên, chúng ta bị hạn chế bởi tổng khối lượng`m`, vì vậy chúng ta không thể tự do gán các giá trị xen kẽ lớn ở mọi nơi. 

Việc xây dựng tối ưu hóa ra cực kỳ đơn giản: chúng ta đặt tất cả khối lượng khác 0 vào hai mẫu cực trị tùy thuộc vào`n`. Khi`n = 1`, không có cặp liền kề. Khi`n = 2`, chúng tôi đặt`m`hoàn toàn ở một bên. Khi`n ≥ 3`, chúng ta có thể đạt được mô hình “zig-zag” đầy đủ trong đó các giá trị xen kẽ giữa 0 và khác 0, và mỗi đơn vị khối lượng đóng góp hai lần vào tổng chênh lệch ngoại trừ các giới hạn biên. Điều này dẫn đến kết quả dạng đóng là giá trị lớn nhất là`2 * m`khi`n ≥ 3`, Và`m`khi`n = 2`. 

Lý do điều này có tác dụng là vì mỗi đơn vị được đặt ở vị trí bên trong có thể được tính hai lần trong quá trình chuyển đổi, một lần khi đi vào và một lần khi rời đi, đồng thời cấu trúc xen kẽ đảm bảo không lãng phí khối lượng trong các đoạn phẳng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm từng trường hợp thử nghiệm thành một công thức trực tiếp chỉ dựa trên`n`Và`m`. 

1. Nếu`n == 1`, trở lại`0`. Không có phần tử liền kề nên không có sự khác biệt. 
2. Nếu`n == 2`, trở lại`m`. Điều tốt nhất chúng ta có thể làm là chia tay`m`vào trong`(m, 0)`hoặc`(0, m)`, tạo ra sự khác biệt`m`. 
3. Nếu`n >= 3`, trở lại`2 * m`. Chúng ta có thể xây dựng một mô hình xen kẽ như`[0, m, 0, 0, 0, ...]`được sắp xếp lại thành một đường zig-zag trong đó mọi chuyển động của khối lượng đều đóng góp hai lần trong quá trình chuyển đổi. 

Lý do chính khiến bước 3 hoạt động là vì với ít nhất ba vị trí, chúng ta luôn có thể tách khối lượng sao cho nó được bao quanh bởi các số 0, cho phép nó được “nhập” và “thoát” trong các chuyển tiếp liền kề. 

### Tại sao nó hoạt động 

Giá trị của mục tiêu chỉ phụ thuộc vào sự thay đổi giữa các vị trí liên tiếp. Mỗi đơn vị dương được đặt trong mảng có thể đóng góp tối đa hai lần vào tổng chênh lệch tuyệt đối, một lần khi nó được tiếp cận từ điểm 0 (hoặc giá trị thấp hơn) và một lần khi nó quay về 0 (hoặc giá trị thấp hơn). Với ít nhất ba vị trí, chúng ta luôn có thể sắp xếp các số 0 xung quanh toàn bộ khối lượng theo cách tránh được sự can thiệp giữa các đóng góp, tạo ra`2m`có thể đạt được và tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        if n == 1:
            print(0)
        elif n == 2:
            print(m)
        else:
            print(2 * m)

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp thử nghiệm một cách độc lập và áp dụng phân biệt trường hợp trực tiếp. Việc triển khai dựa trên thực tế là câu trả lời chỉ phụ thuộc vào`n`Và`m`, do đó không cần xây dựng mảng. 

Điều tinh tế duy nhất là đảm bảo rằng phép nhân với 2 được thực hiện bằng số nguyên Python, xử lý an toàn các giá trị lên tới`2 * 10^9`trong những ràng buộc. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi logic cho các đầu vào đã chọn. 

### Ví dụ 1 

đầu vào:`n = 2, m = 2`| Bước | n | m | Quyết định | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | n == 2 | 2 | 

Ở đây mảng`[2, 0]`đạt được giá trị`|2 - 0| = 2`, phù hợp với đầu ra. 

Điều này xác nhận rằng với hai phần tử, toàn bộ khối lượng phải được đặt ở một điểm cuối. 

### Ví dụ 2 

đầu vào:`n = 5, m = 5`| Bước | n | m | Quyết định | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | n ≥ 3 | 10 | 

Một công trình như`[0, 5, 0, 0, 0]`được sắp xếp lại thành cách giải thích zig-zag mang lại sự luân phiên tối đa. Mỗi đơn vị đóng góp hai lần trong quá trình chuyển đổi, tạo ra`2 * 5 = 10`. 

Điều này cho thấy việc tăng các vị trí sẵn có cho phép khai thác tối đa dao động như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý bằng số học không đổi | 
| Không gian | O(1) | Không có bộ nhớ phụ tỷ lệ thuận với kích thước đầu vào | 

Các ràng buộc cho phép lên đến`10^4`các trường hợp thử nghiệm, vì vậy cần có giải pháp thời gian không đổi cho mỗi trường hợp thử nghiệm. Giải pháp này đáp ứng yêu cầu đó một cách dễ dàng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(sys.stdin.readline())
    for _ in range(t):
        n, m = map(int, sys.stdin.readline().split())
        if n == 1:
            output.append("0")
        elif n == 2:
            output.append(str(m))
        else:
            output.append(str(2 * m))
    return "\n".join(output)

# provided samples
assert run("""5
1 100
2 2
5 5
2 1000000000
1000000000 1000000000
""") == """0
2
10
1000000000
2000000000"""

# minimum size
assert run("1\n1 0\n") == "0"

# small alternating case
assert run("1\n3 1\n") == "2"

# large single element
assert run("1\n1 1000000000\n") == "0"

# large two element
assert run("1\n2 1000000000\n") == "1000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 0`|`0`| trường hợp cạnh phần tử đơn | 
|`3 1`|`2`| trường hợp dao động tối thiểu | 
|`1 1000000000`|`0`| lớn n=1 độ ổn định | 
|`2 1000000000`|`1000000000`| ranh giới cho n=2 | 

## Vỏ cạnh 

cho`n = 1`, thuật toán trả về`0`ngay lập tức. Với đầu vào`n = 1, m = 100`, không có cặp liền kề nào, do đó giá trị được tính toán có cấu trúc bằng 0 bất kể phân phối. điều kiện`n == 1`trực tiếp thực thi điều này. 

Vì`n = 2`, thuật toán trả về`m`. Nếu chúng ta lấy`n = 2, m = 7`, cách duy nhất để chia số tiền đó là`(7, 0)`hoặc`(3, 4)`hoặc bất kỳ phân vùng nào. Trường hợp tốt nhất là luôn đặt mọi khối lượng về một phía, tạo ra sự khác biệt`7`. chi nhánh`n == 2`nắm bắt chính xác hành vi này. 

Vì`n ≥ 3`, coi như`n = 3, m = 4`. Thuật toán trả về`8`. Một cách xây dựng hợp lệ là`[0, 4, 0]`, sản xuất`|0-4| + |4-0| = 8`. Điều này cho thấy hiệu ứng nhân đôi có thể đạt được ngay cả trong trường hợp nhỏ nhất khi áp dụng.
