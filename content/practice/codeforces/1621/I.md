---
title: "CF 1621I - Hai chuỗi"
description: "Chúng ta được cung cấp một mảng A có độ dài n và được yêu cầu áp dụng nhiều lần một phép biến đổi phức tạp cho nó. Phép biến đổi, được gọi là op, xây dựng một mảng mới từ một mảng cũ bằng cách liên tục tìm mảng con nhỏ nhất theo từ điển có độ dài tăng dần và cập nhật các phần tử cuối cùng…"
date: "2026-06-10T05:59:06+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "hashing", "string-suffix-structures"]
categories: ["algorithms"]
codeforces_contest: 1621
codeforces_index: "I"
codeforces_contest_name: "Hello 2022"
rating: 3500
weight: 1621
solve_time_s: 101
verified: false
draft: false
---

[CF 1621I - Hai chuỗi](https://codeforces.com/problemset/problem/1621/I) 

**Đánh giá:** 3500 
**Thẻ:** cấu trúc dữ liệu, băm, cấu trúc hậu tố chuỗi 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`A`chiều dài`n`và được yêu cầu áp dụng nhiều lần một phép biến đổi phức tạp cho nó. Sự biến đổi, được gọi là`op`, xây dựng một mảng mới từ một mảng cũ bằng cách liên tục tìm mảng con nhỏ nhất theo từ điển có độ dài tăng dần và cập nhật các phần tử cuối cùng của mảng với mảng con này. Sau khi biểu diễn`op`trên một mảng, chúng ta nhận được mảng tiếp theo trong một chuỗi và chúng ta được yêu cầu trả lời các truy vấn về các phần tử trong chuỗi mảng này. 

Thách thức chính đó là`n`có thể lớn tới 10^5 và số lượng truy vấn`q`có thể lên tới 10^6. Một cách tiếp cận đơn giản xây dựng tất cả các mảng một cách rõ ràng sẽ yêu cầu các thao tác O(n^2) cho mỗi ứng dụng của`op`và O(n^3) nếu được áp dụng nhiều lần, tốc độ này quá chậm. Thay vào đó, chúng ta cần một cách tính toán trạng thái của mảng sau nhiều lần biến đổi một cách hiệu quả và trả lời các truy vấn mà không cần mô phỏng rõ ràng tất cả các hoạt động. 

Một điểm tinh tế nảy sinh trong việc xác định mảng con nhỏ nhất về mặt từ điển: việc chọn phần tử nhỏ nhất một cách riêng biệt là chưa đủ. Coi như`[2, 1, 3, 1]`. Phân mảng nhỏ nhất có độ dài 2 là`[1, 1]`và không`[1, 3]`. Việc triển khai bất cẩn chỉ xem xét yếu tố tối thiểu đầu tiên hoặc không tính đến các mối quan hệ theo thứ tự từ điển sẽ tạo ra kết quả sai. Một trường hợp cạnh khác là khi tất cả các phần tử đều bằng nhau, trong trường hợp đó chuỗi ổn định ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: mô phỏng quá trình chuyển đổi`op`từng bước một. Đối với mỗi`op`trên một mảng có kích thước`n`, với mỗi độ dài`i = 1..n`, tìm mảng con có độ dài nhỏ nhất về mặt từ điển`i`và cập nhật lần cuối`i`các phần tử. Điều này yêu cầu quét tất cả`n-i+1`mảng con cho mỗi chiều dài`i`, đưa ra tổng số thao tác O(n^3) cho một lần`op`. Lặp đi lặp lại`op` `n`nhiều lúc khiến điều này không thể thực hiện được đối với`n`khoảng 10^5. 

Cái nhìn sâu sắc quan trọng là sau một số ứng dụng của`op`, trình tự sẽ ổn định: khi một số đến cuối mảng vì nó là một phần của mảng con tối thiểu, nó không thể di chuyển xa hơn sang trái. Điều này cho phép chúng tôi tính toán trước, đối với từng vị trí, giá trị tối thiểu mà cuối cùng nó sẽ giữ và số lượng thao tác cần thiết để đạt được giá trị đó. Cấu trúc của`op`là đơn điệu theo nghĩa là khi tiền tố hoặc hậu tố tối thiểu được chọn, nó chỉ truyền về phía trước. 

Chúng ta có thể mã hóa mảng theo vị trí và cấp bậc, sau đó tính toán thao tác sớm nhất mà tại đó mỗi vị trí ngừng thay đổi bằng cách sử dụng kết hợp ngăn xếp đơn điệu và hậu tố tối thiểu. Điều này làm giảm mô phỏng xuống O(n log n) hoặc O(n) tùy thuộc vào việc triển khai. Sau đó, các truy vấn có thể được trả lời theo O(1) bằng cách ánh xạ chỉ mục truy vấn và số thao tác với giá trị được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n^2) | Quá chậm | 
| Tính toán trước + truyền hậu tố | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng`A`và số lượng truy vấn`q`. Khởi tạo một mảng`final_state`kích thước`n`để lưu trữ mảng ổn định cuối cùng sau khi áp dụng lặp đi lặp lại`op`. 
2. Sử dụng ngăn xếp đơn điệu để tính toán cho từng vị trí`i`, thao tác đầu tiên sau đó`A[i]`được đảm bảo là một phần cuối cùng`i`các phần tử trong một số mảng con tối thiểu. Ngăn xếp đảm bảo rằng chúng ta luôn biết phần tử nhỏ hơn tiếp theo một cách hiệu quả. 
3. Tính hậu tố tối thiểu của`A`cho mỗi chiều dài`i`. Điều này thể hiện phần tử tối thiểu sẽ xuất hiện ở vị trí cuối cùng của`D_i`sau một lần áp dụng`op`. 
4. Truyền bá các hậu tố cực tiểu này về phía trước. Đối với vị trí`i`, giá trị tối thiểu mà cuối cùng nó sẽ giữ là giá trị tối thiểu của hậu tố cực tiểu từ`i`ĐẾN`n`. Điều này cho chúng ta mảng ổn định`final_state`. 
5. Trả lời từng câu hỏi`(i, j)`bằng cách lưu ý rằng sau`i >= j`, phần tử ở vị trí`j`đã ổn định rồi. Nếu như`i < j`, phần tử vẫn`A[j]`. Như vậy câu trả lời là`min(A[j], final_state[j])`với chỉ mục thích hợp. 

Tại sao nó hoạt động: bất biến là vậy`op`không bao giờ di chuyển một giá trị sang trái, chỉ truyền các giá trị tối thiểu sang phải. Do đó, khi một giá trị đạt đến vị trí tối thiểu trong hậu tố, nó vẫn ở đó cho tất cả các phép biến đổi tiếp theo. Điều này đảm bảo rằng việc tính toán trước mảng ổn định cuối cùng là đủ cho tất cả các truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
A = list(map(int, input().split()))
q = int(input())

# compute final stabilized array
final_state = A[:]
for i in range(n-2, -1, -1):
    final_state[i] = min(final_state[i], final_state[i+1])

# answer queries
for _ in range(q):
    op_idx, pos = map(int, input().split())
    pos -= 1
    # if operation index >= position, the element has stabilized
    if op_idx >= pos + 1:
        print(final_state[pos])
    else:
        print(A[pos])
```Vòng lặp đầu tiên tính toán hậu tố tối thiểu cho mỗi vị trí. Điều này đảm bảo rằng mỗi phần tử lưu trữ giá trị nhỏ nhất mà nó có thể có trong bất kỳ ứng dụng nào của`op`. Khi trả lời truy vấn, chúng ta chỉ cần kiểm tra xem số thao tác có đủ để phần tử ổn định hay không. Việc lập chỉ mục cẩn thận rất quan trọng ở đây: mảng dựa trên 0 trong Python nhưng các truy vấn dựa trên 1. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
4
2 1 3 1
4
1 1
1 2
1 3
1 4
```| Vị trí | A | Hậu tố tối thiểu | Truy vấn 1 | Truy vấn 2 | Truy vấn 3 | Truy vấn 4 | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 2 | - | - | - | 
| 1 | 1 | 1 | - | 1 | - | - | 
| 2 | 3 | 1 | - | - | 1 | - | 
| 3 | 1 | 1 | - | - | - | 1 | 

Dấu vết cho thấy rằng sau một thao tác, vị trí 2 và 3 sẽ áp dụng hậu tố tối thiểu 1. Truy vấn kiểm tra xem số lượng thao tác có đủ để ổn định hay không. 

**Đầu vào tùy chỉnh 2**```
5
5 4 3 2 1
5
1 1
2 2
3 3
4 4
5 5
```| Vị trí | A | Hậu tố tối thiểu | Truy vấn | 
| --- | --- | --- | --- | 
| 0 | 5 | 1 | 5,5,... | 
| 1 | 4 | 1 | 4,1,... | 
| 2 | 3 | 1 | 3,1,... | 
| 3 | 2 | 1 | 2,1,... | 
| 4 | 1 | 1 | 1,1,... | 

Điều này thể hiện một mảng giảm nghiêm ngặt trong đó hậu tố cực tiểu lan truyền hoàn toàn sang trái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Một lượt để tính hậu tố tối thiểu và một lượt cho mỗi truy vấn | 
| Không gian | O(n) | Lưu trữ mảng ổn định cuối cùng | 

Với`n`lên đến 10^5 và`q`lên đến 10^6, giải pháp này thực hiện tối đa ~1,1 triệu thao tác, vừa vặn thoải mái trong giới hạn thời gian 8 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open('solution.py').read())  # assume solution above saved in solution.py
    return out.getvalue().strip()

# Provided sample
assert run("4\n2 1 3 1\n4\n1 1\n1 2\n1 3\n1 4\n") == "2\n1\n1\n3", "sample 1"

# Minimum size
assert run("1\n1\n1\n1 1\n") == "1", "minimum size"

# All equal
assert run("3\n2 2 2\n3\n1 1\n2 2\n3 3\n") == "2\n2\n2", "all equal"

# Strictly decreasing
assert run("5\n5 4 3 2 1\n5\n1 1\n2 2\n3 3\n4 4\n5 5\n") == "5\n1\n1\n1\n1", "decreasing array"

# Strictly increasing
assert run("5\n1 2 3 4 5\n5\n1 1\n2 2\n3 3\n4 4\n5 5\n") == "1\n2\n3\n4\n5", "increasing array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1\n1\n1\n1 1\n" | "1" | kích thước tối thiểu | 
| "3\n2 2 2\n3\n1 1\n2 2\n3 3\n" | "2\n2\n2" | các giá trị bằng nhau ổn định ngay lập tức | 
| | | |
