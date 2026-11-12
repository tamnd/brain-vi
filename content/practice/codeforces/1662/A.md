---
title: "CF 1662A - Tổ chức SWERC"
description: "Gianni cần tập hợp một bộ bài toán cho SWERC từ một tập hợp các bài toán do ban giám khảo đưa ra. Mỗi vấn đề đều có điểm đẹp và mức độ khó từ 1 đến 10."
date: "2026-06-10T02:39:06+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1662
codeforces_index: "A"
codeforces_contest_name: "SWERC 2021-2022 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 0
weight: 1662
solve_time_s: 88
verified: true
draft: false
---

[CF 1662A - Tổ chức SWERC](https://codeforces.com/problemset/problem/1662/A) 

**Đánh giá:** - 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Gianni cần tập hợp một bộ bài toán cho SWERC từ một tập hợp các bài toán do ban giám khảo đưa ra. Mỗi bài toán đều có điểm đẹp và đánh giá độ khó từ 1 đến 10. Gianni muốn bộ bài toán cuối cùng có đúng một bài toán với mỗi độ khó từ 1 đến 10 và với mỗi độ khó anh sẽ chọn bài toán có độ đẹp cao nhất. Nếu thiếu bất kỳ khó khăn nào trong nhóm, Gianni không thể tạo thành một bộ đầy đủ. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định số lượng vấn đề và sau đó liệt kê cái hay và độ khó của từng vấn đề. Đầu ra phải là vẻ đẹp tổng thể của bộ vấn đề đã chọn hoặc chuỗi`MOREPROBLEMS`nếu không thể thực hiện được một bộ đầy đủ. 

Các ràng buộc rất nhỏ: mỗi trường hợp kiểm thử có tối đa 100 vấn đề và có nhiều nhất 100 trường hợp kiểm thử. Vì chỉ có 10 khó khăn nên giải pháp xử lý từng khó khăn riêng lẻ là khả thi. Kích thước đầu vào đủ nhỏ để các vòng lặp lồng nhau vượt qua 10 khó khăn và vấn đề có thể chấp nhận được. 

Các trường hợp khó khăn bao gồm khi thiếu hoàn toàn một khó khăn hoặc khi nhiều vấn đề có cùng độ khó nhưng có điểm đẹp khác nhau. Ví dụ: nếu đầu vào là:```
1
3
5 1
4 1
7 2
```Sản lượng dự kiến ​​​​là`MOREPROBLEMS`bởi vì khó khăn từ 3 đến 10 bị thiếu, mặc dù chúng ta có nhiều bài toán cho khó khăn 1 và 2. Một giải pháp bất cẩn có thể tính tổng tất cả các cái đẹp mà không kiểm tra những khó khăn còn thiếu, tạo ra tổng số không chính xác là 16 thay vì báo cáo điều không thể xảy ra. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là lặp lại tất cả 10 độ khó cho từng trường hợp thử nghiệm và với mỗi độ khó, hãy quét toàn bộ danh sách các vấn đề để tìm ra vẻ đẹp tối đa cho độ khó đó. Nếu thiếu một khó khăn, chúng tôi quay lại`MOREPROBLEMS`. Điều này hiệu quả vì cả số lượng khó khăn và vấn đề đều nhỏ. Số lượng hoạt động trong trường hợp xấu nhất là`t * 10 * n`Ở đâu`t`là số lượng ca kiểm thử và`n`số lượng vấn đề. Với`t ≤ 100`Và`n ≤ 100`, điều này mang lại tối đa 100.000 thao tác, có thể chấp nhận được trong giới hạn thời gian 2 giây. 

Cách tiếp cận tối ưu tận dụng một phạm vi khó khăn cố định nhỏ. Chúng ta có thể sử dụng một mảng có kích thước 11 (chỉ số từ 1 đến 10) để theo dõi vẻ đẹp tối đa cho từng độ khó. Chúng tôi lặp lại danh sách các vấn đề một lần, cập nhật vẻ đẹp tối đa cho độ khó của vấn đề. Sau khi xử lý xong mọi vấn đề, chúng tôi kiểm tra xem có khó khăn gì không (vẻ đẹp vẫn giữ nguyên giá trị ban đầu). Nếu vậy, xuất ra`MOREPROBLEMS`. Nếu không, hãy tổng hợp những vẻ đẹp tối đa được lưu trữ. Cách tiếp cận này làm giảm việc quét lặp lại và rõ ràng và trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t * n * 10) | O(1) | Đã chấp nhận | 
| Tối ưu | O(t * n) | O(10) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Đối với mỗi trường hợp thử nghiệm: 
2. Đọc số bài toán`n`. 
3. Khởi tạo một mảng`max_beauty`có kích thước 11 với tất cả các mục được đặt thành -1. chỉ mục`i`sẽ lưu giữ vẻ đẹp tối đa của những vấn đề khó khăn`i`. 
4. Đối với mỗi vấn đề, hãy đọc vẻ đẹp của nó`b`và khó khăn`d`. Cập nhật`max_beauty[d]`là giá trị tối đa của giá trị hiện tại của nó và`b`. Điều này đảm bảo rằng sau khi đọc tất cả các vấn đề,`max_beauty[d]`lưu trữ vấn đề khó khăn đẹp nhất`d`. 
5. Đánh dấu tất cả các độ khó từ 1 đến 10. Nếu có`max_beauty[d]`vẫn là -1, in`MOREPROBLEMS`cho trường hợp thử nghiệm này. 
6. Ngược lại, tính tổng các giá trị trong`max_beauty[1..10]`và in kết quả. 

Tại sao nó hoạt động: Điều bất biến là sau khi xử lý mọi vấn đề,`max_beauty[d]`chứa đựng vẻ đẹp tối đa của mọi vấn đề khó khăn`d`, hoặc -1 nếu không có vấn đề như vậy tồn tại. Việc kiểm tra -1 đảm bảo chúng ta không bỏ sót bất kỳ khó khăn nào. Tổng các mức tối đa này tạo nên vẻ đẹp tổng thể của bộ bài toán vì Gianni luôn chọn bài toán đẹp nhất cho mỗi độ khó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    max_beauty = [-1] * 11  # indices 1..10
    for _ in range(n):
        b, d = map(int, input().split())
        if b > max_beauty[d]:
            max_beauty[d] = b
    if -1 in max_beauty[1:]:
        print("MOREPROBLEMS")
    else:
        print(sum(max_beauty[1:]))
```Mã khởi tạo`max_beauty`với -1 để phát hiện rõ ràng những khó khăn còn thiếu. Mỗi vấn đề cập nhật vẻ đẹp tối đa cho độ khó của nó. Việc kiểm tra cho`-1`đảm bảo rằng chúng ta chỉ tính tổng nếu giải quyết được mọi khó khăn. Việc sử dụng các chỉ số từ 1 đến 10 sẽ phù hợp với việc đánh số độ khó của bài toán và tránh được các lỗi sai sót một. Nhập nhanh qua`sys.stdin.readline`được sử dụng để xử lý nhiều trường hợp thử nghiệm một cách hiệu quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
8 4
9 3
6 7
```| Bước | Vấn đề | max_beauty[1..10] sau khi cập nhật | 
| --- | --- | --- | 
| 1 | 8 4 | [-1, -1, -1, 8, -1, -1, -1, -1, -1, -1, -1] | 
| 2 | 9 3 | [-1, -1, 9, 8, -1, -1, -1, -1, -1, -1, -1] | 
| 3 | 6 7 | [-1, -1, 9, 8, -1, -1, 6, -1, -1, -1, -1] | 

Kiểm tra`max_beauty[1..10]`: thiếu mục tại các chỉ số 1,2,5,6,8,9,10 → đầu ra`MOREPROBLEMS`. 

### Mẫu 2 

đầu vào:```
12
3 10
10 1
10 2
10 3
10 4
3 10
10 5
10 6
10 7
10 8
10 9
1 10
```| Bước | Vấn đề | max_beauty[1..10] sau khi cập nhật | 
| --- | --- | --- | 
| 1 | 3 10 | [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, 3] | 
| 2 | 10 1 | [-1, 10, -1, -1, -1, -1, -1, -1, -1, -1, 3] | 
| ... | ... | ... | 
| 12 | 1 10 | vấn đề cuối cùng có vẻ đẹp 1 → max_beauty[10] còn lại 3 | 

Tổng`max_beauty[1..10] = 10 + 10 + 10 + 10 + 10 + 10 + 10 + 10 + 10 + 3 = 93`. 

Dấu vết này xác nhận thuật toán cập nhật chính xác vẻ đẹp tối đa cho mỗi độ khó và tính toán vẻ đẹp tổng thể trong khi xử lý nhiều vấn đề cho mỗi độ khó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi vấn đề được xử lý một lần, cập nhật một mảng có kích thước 10. Việc kiểm tra những khó khăn còn thiếu mất O(10) cho mỗi trường hợp kiểm thử. | 
| Không gian | O(10) |`max_beauty`mảng lưu trữ 10 giá trị cho mỗi trường hợp thử nghiệm. | 

Với t 100 và n 100, giải pháp thực hiện tối đa 10.000 thao tác, nằm trong giới hạn 2 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        t = int(input())
        for _ in range(t):
            n = int(input())
            max_beauty = [-1] * 11
            for _ in range(n):
                b, d = map(int, input().split())
                max_beauty[d] = max(max_beauty[d], b)
            if -1 in max_beauty[1:]:
                print("MOREPROBLEMS")
            else:
                print(sum(max_beauty[1:]))
    return out.getvalue().strip()

# Provided samples
assert run("2\n3\n8 4\n9 3\n6 7\n12\n3 10\n10 1\n10 2\n10 3\n10 4\n3 10\n10 5\n10 6\n10 7\n10 8\n10 9\n1 10") == "MOREPROBLEMS\n93"

# Minimum-size input
assert run("1\n1\n1 1") == "MOREPROBLEMS", "only one problem, missing difficulties 2..10"

# Maximum-size input with all difficulties
input_data = "1\n100\n" + "\n".join(f"{i%10+1} {i%10+1}" for i in range(100))
expected_sum =
```
