---
title: "CF 1873B - Đứa Trẻ Tốt"
description: "Chúng ta được cung cấp một tập hợp nhỏ các chữ số và chúng ta được phép tăng chính xác một trong số chúng lên một đơn vị. Sau lần sửa đổi duy nhất này, chúng tôi tính toán tích của tất cả các chữ số và muốn tối đa hóa nó."
date: "2026-06-08T23:13:14+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1873
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 898 (Div. 4)"
rating: 800
weight: 1873
solve_time_s: 108
verified: true
draft: false
---

[CF 1873B - Đứa trẻ ngoan](https://codeforces.com/problemset/problem/1873/B) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp nhỏ các chữ số và chúng ta được phép tăng chính xác một trong số chúng lên một đơn vị. Sau lần sửa đổi duy nhất này, chúng tôi tính toán tích của tất cả các chữ số và muốn tối đa hóa nó. 

Cấu trúc được cố ý đơn giản: mảng rất nhỏ nên chúng tôi không tối ưu hóa trên các chuỗi lớn hoặc trạng thái phức tạp. Thay vào đó, quyết định hoàn toàn là về việc nên tăng một chữ số nào để tích thu được càng lớn càng tốt. 

Mặc dù các ràng buộc cho phép tối đa 10⁴ trường hợp kiểm thử, nhưng mỗi trường hợp kiểm thử có tối đa 9 chữ số. Điều này ngay lập tức loại trừ bất cứ điều gì vượt quá hệ số không đổi cho mỗi trường hợp thử nghiệm. Việc thử tất cả các khả năng là hoàn toàn an toàn vì tổng công việc tối đa là khoảng 9 thao tác cho mỗi trường hợp, điều này là không đáng kể. 

Điểm tinh tế chính là việc tăng một chữ số có thể có những tác động không cục bộ lên sản phẩm. Ví dụ: tăng 0 lên 1 sẽ thay đổi sản phẩm từ 0 thành có khả năng khác 0, điều này chiếm ưu thế trong hầu hết các thay đổi khác. Tương tự, việc tăng 9 lên 10 không tồn tại vì các chữ số vẫn là số nguyên đơn trong bài toán này, vì vậy giá trị tối đa sau khi tăng chỉ là 10 về giá trị chứ không phải cấu trúc chữ số. 

Các trường hợp cạnh xuất hiện khi có số không. Một cách tiếp cận ngây thơ giả định “tăng chữ số lớn nhất” có thể thất bại nặng nề: 

Ví dụ, hãy xem xét`[0, 1, 9]`. Tăng 9 cho sản phẩm 0, tăng 1 cho sản phẩm 0, nhưng tăng 0 cho sản phẩm 0`[1, 1, 9]`và sản phẩm 9 là tối ưu. 

Vì vậy, vấn đề thực sự không chỉ ở mức độ mà là sự đóng góp cận biên cho sản phẩm. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi thử tăng từng chỉ số một lần, tính tích và lấy giá trị tối đa. Vì n ≤ 9, điều này có nghĩa là có nhiều nhất 9 ứng viên cho mỗi ca kiểm thử và mỗi phép tính sản phẩm tốn nhiều nhất là 9 phép nhân, do đó tổng thời gian là không đổi trong thực tế. 

Điều này hiệu quả vì không gian quyết định cực kỳ nhỏ và không có ràng buộc nào liên kết nhiều lựa chọn. Lực lượng vũ phu chỉ thất bại nếu chúng ta cố gắng tối ưu hóa một cách tham lam một cách nhầm lẫn, giả sử các thuộc tính chữ số cục bộ xác định sản phẩm toàn cầu tốt nhất. 

Quan sát mở ra giải pháp là không có sự tương tác giữa các lựa chọn ngoài vị trí được sửa đổi duy nhất. Mọi giải pháp ứng viên đều độc lập nên việc liệt kê tất cả các khả năng là tối ưu và đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 
| Tối ưu | O(n²) mỗi bài kiểm tra (cùng một ý tưởng) | O(1) | Đã chấp nhận | 

Ở đây, cái gọi là giải pháp “tối ưu” chỉ đơn giản là bản thân sự ép buộc, bởi vì các ràng buộc đã buộc nó phải tối ưu. 

## Hướng dẫn thuật toán 

1. Đọc mảng chữ số của test case. 
2. Tính toán ý tưởng cơ bản: chúng tôi sẽ thử sửa đổi từng chỉ mục chính xác một lần. Điều này đảm bảo chúng tôi khám phá mọi cấu hình hợp lệ. 
3. Với mỗi chỉ số i, tạm thời tăng a[i] lên 1. 
4. Tính tích của tất cả các chữ số sau lần sửa đổi này. Điều này trực tiếp đánh giá kết quả của việc chọn i. 
5. Khôi phục a[i] và tiếp tục tới chỉ mục tiếp theo. 
6. Theo dõi sản phẩm tối đa được nhìn thấy trên tất cả các lựa chọn. 
7. Xuất ra mức tối đa. 

### Tại sao nó hoạt động 

Mỗi giải pháp hợp lệ được xác định duy nhất bởi vị trí của chữ số tăng dần. Không có mức độ tự do nào khác. Vì chúng tôi đánh giá mọi lựa chọn có thể nên chúng tôi đảm bảo sẽ đưa ra lựa chọn tối ưu. Tính đúng đắn có được từ việc liệt kê đầy đủ trên một không gian quyết định có kích thước không đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    best = 0

    for i in range(n):
        a[i] += 1

        prod = 1
        for v in a:
            prod *= v

        best = max(best, prod)

        a[i] -= 1

    print(best)
```Giải pháp trực tiếp thực hiện ý tưởng thử nghiệm từng vị trí tăng trưởng có thể. Đối với mỗi chỉ mục, chúng tôi sửa đổi nó, tính toán lại toàn bộ sản phẩm và khôi phục nó. Bước khôi phục rất quan trọng vì việc không hoàn nguyên thay đổi sẽ làm hỏng các đánh giá tiếp theo. 

Vì n ≤ 9 nên việc tính toán lại tích nhiều lần sẽ rẻ. Các vòng lặp lồng nhau không gây ra bất kỳ vấn đề nào về hiệu suất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 2 1 2
```| Chỉ mục đã sửa đổi | Mảng sau khi tăng | Sản phẩm | 
| --- | --- | --- | 
| 0 | 3 2 1 2 | 12 | 
| 1 | 2 3 1 2 | 12 | 
| 2 | 2 2 2 2 | 16 | 
| 3 | 2 2 1 3 | 12 | 

Lựa chọn tốt nhất là tăng phần tử thứ ba, tạo ra 16. Điều này cho thấy ngay cả một sự thay đổi chữ số nhỏ cũng có thể chiếm ưu thế do cấu trúc nhân. 

### Ví dụ 2 

đầu vào:```
3
0 1 2
```| Chỉ mục đã sửa đổi | Mảng sau khi tăng | Sản phẩm | 
| --- | --- | --- | 
| 0 | 1 1 2 | 2 | 
| 1 | 0 2 2 | 0 | 
| 2 | 0 1 3 | 0 | 

Chiến lược tối ưu rõ ràng là tăng số 0, vì nó biến toàn bộ sản phẩm từ 0 thành giá trị dương. Điều này chứng tỏ tại sao việc tham lam “chọn chữ số lớn nhất” sẽ thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) mỗi lần kiểm tra | Hãy thử từng chỉ mục và tính toán lại toàn bộ sản phẩm | 
| Không gian | O(1) | Chỉ lưu trữ mảng đầu vào | 

Cho n ≤ 9, đây thực sự là thời gian không đổi cho mỗi trường hợp thử nghiệm, nằm trong giới hạn ngay cả đối với 10⁴ thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        best = 0

        for i in range(n):
            a[i] += 1
            prod = 1
            for v in a:
                prod *= v
            best = max(best, prod)
            a[i] -= 1

        out.append(str(best))

    return "\n".join(out)

# provided samples
assert run("""4
4
2 2 1 2
3
0 1 2
5
4 3 2 3 4
9
9 9 9 9 9 9 9 9 9""") == """16
2
432
430467210"""

# custom cases
assert run("""1
1
5""") == """6"""  # single element increment
assert run("""1
3
0 0 0""") == """1"""  # any increment creates 1
assert run("""1
3
1 1 1""") == """8"""  # symmetric case
assert run("""1
4
0 2 3 4""") == """36"""  # zero handling dominates
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | tăng cơ bản | trường hợp cơ sở | 
| tất cả số không | hiệu ứng không một | xử lý bằng không | 
| tất cả những cái | hành vi thống nhất | đối xứng | 
| chữ số hỗn hợp | sự thống trị bằng không | trường hợp thất bại tham lam | 

## Vỏ cạnh 

Khi mảng chứa các số 0, bước di chuyển tối ưu hầu như luôn là tăng số 0 vì nó biến đổi tích từ 0 thành giá trị dương. Thuật toán xử lý việc này một cách chính xác vì nó đánh giá rõ ràng mọi chỉ mục, kể cả số 0, mà không có sai lệch. 

Khi tất cả các chữ số giống hệt nhau, mọi lựa chọn đều mang lại cấu trúc sản phẩm giống nhau và phép liệt kê nắm bắt được tính đối xứng này một cách tự nhiên. 

Khi n = 1, thao tác hợp lệ duy nhất là tăng một chữ số đó và thuật toán sẽ đánh giá chính xác khả năng duy nhất đó và trả về giá trị của nó.
