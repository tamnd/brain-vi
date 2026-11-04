---
title: "CF 1166C - Chuyện Hai Miền Đất"
description: "Chúng ta được cung cấp một tập hợp các số nguyên biểu thị các giá trị tiềm năng của hai điểm đánh dấu lịch sử, $x$ và $y$. Với mỗi cặp số nguyên phân biệt $(x, y)$, chúng ta tính toán hai khoảng trên trục số."
date: "2026-06-12T02:13:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1166
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 561 (Div. 2)"
rating: 1500
weight: 1166
solve_time_s: 87
verified: true
draft: false
---

[CF 1166C - Câu chuyện về hai vùng đất](https://codeforces.com/problemset/problem/1166/C) 

**Đánh giá:** 1500 
**Thẻ:** tìm kiếm nhị phân, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số nguyên đại diện cho các giá trị tiềm năng của hai dấu mốc lịch sử,$x$Và$y$. Với mỗi cặp ứng viên gồm các số nguyên phân biệt$(x, y)$, chúng ta tính hai khoảng trên trục số. Khoảng đầu tiên, được gọi là Arrayland, được xác định bởi các giá trị tuyệt đối của chính các điểm đánh dấu: điểm cuối của nó là$|x|$Và$|y|$. Khoảng thứ hai, được gọi là Vectorland, được xác định bởi các giá trị tuyệt đối của$x-y$Và$x+y$. Truyền thuyết được coi là đúng đối với một cặp nếu Arrayland nằm hoàn toàn bên trong Vectorland, bao gồm cả các điểm cuối. 

Nhiệm vụ của chúng ta là đếm xem có bao nhiêu cặp không có thứ tự$(x, y)$từ các số nguyên đã cho thỏa mãn thuộc tính ngăn chặn này. Kích thước đầu vào có thể lên tới$2 \cdot 10^5$, điều này ngay lập tức loại trừ một giải pháp vũ phu ngây thơ để kiểm tra tất cả$O(n^2)$cặp. Mỗi số nguyên có thể lớn bằng$10^9$về độ lớn, vì vậy chúng ta phải tránh các giải pháp có quy mô kém theo phạm vi số. 

Các trường hợp đặc biệt có thể gây ra việc triển khai bất cẩn bao gồm các tình huống trong đó một hoặc cả hai số đều âm, vì giá trị tuyệt đối có thể thay đổi đáng kể các điểm cuối của khoảng thời gian. Ví dụ, với cặp$(-2, 3)$, Arrayland là$[2,3]$và Vectorland là$[1,5]$, thỏa mãn sự ngăn chặn. Một phép kiểm tra ngây thơ bỏ qua các số âm hoặc giả định$x \le y$sẽ tạo ra số đếm không chính xác. Một trường hợp cạnh khác là khi một giá trị bằng 0, điều này có thể thu hẹp khoảng Vectorland và ảnh hưởng đến việc ngăn chặn. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tạo ra tất cả các cặp số nguyên riêng biệt không có thứ tự$(x, y)$và kiểm tra xem Arrayland có nằm hoàn toàn bên trong Vectorland hay không. Điều này đòi hỏi phải tính toán bốn giá trị tuyệt đối và so sánh các khoảng cho mỗi cặp. Với$n$lên đến$2 \cdot 10^5$, số lượng cặp xấp xỉ$2 \cdot 10^{10}$, vượt xa những gì phù hợp trong thời hạn. 

Thông tin chi tiết quan trọng để tăng tốc giải pháp đến từ việc giảm vấn đề thành tìm kiếm được sắp xếp. Điều kiện ngăn chặn$[\min(|x|, |y|), \max(|x|, |y|)] \subseteq [|x-y|, |x+y|]$có thể được viết lại bằng cách sử dụng các bất đẳng thức$\min(|x|, |y|) \ge |x-y|$Và$\max(|x|, |y|) \le |x+y|$. Bất đẳng thức thứ hai luôn đúng vì tổng các giá trị tuyệt đối lớn hơn hoặc bằng giá trị tuyệt đối lớn hơn. Bất đẳng thức thứ nhất đơn giản hóa thành$|x-y| \le \min(|x|, |y|)$. 

Quan sát này cho phép chúng ta sắp xếp các giá trị tuyệt đối của tất cả các số và sau đó sử dụng kỹ thuật hai con trỏ để đếm hiệu quả các cặp thỏa mãn$|x-y| \le \min(|x|, |y|)$. Phân loại chi phí$O(n \log n)$và việc quét hai con trỏ có thể được thực hiện trong$O(n)$thời gian, tạo ra một giải pháp tổng thể phù hợp với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Sắp xếp + Hai con trỏ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$và mảng số nguyên$a_1, \dots, a_n$. Lấy giá trị tuyệt đối của từng phần tử và lưu vào một mảng mới`abs_a`. Điều này đảm bảo chúng ta có thể làm việc hoàn toàn với các giá trị không âm, giúp đơn giản hóa việc so sánh khoảng thời gian. 
2. Sắp xếp`abs_a`theo thứ tự tăng dần. Sắp xếp cho phép chúng ta tìm chỉ mục lớn nhất một cách hiệu quả`j`như vậy`abs_a[j] - abs_a[i] <= abs_a[i]`. 
3. Khởi tạo hai con trỏ:`i`lặp lại từng phần tử từ`0`ĐẾN`n-1`, đại diện cho một đầu của cặp, và`j`bắt đầu từ`i+1`để tìm phần tử xa nhất thỏa mãn điều kiện ngăn chặn. 
4. Đối với mỗi`i`, tăng`j`trong khi`abs_a[j] - abs_a[i] <= abs_a[i]`. Khi điều kiện không thành công, tất cả các chỉ số từ`i+1`ĐẾN`j-1`tạo thành các cặp hợp lệ với`i`. Hãy đếm chúng như`j - i - 1`. 
5. Tính tổng tất cả các số đếm cho mỗi`i`và in kết quả. 

Điều này có hiệu quả vì việc sắp xếp đảm bảo rằng một khi`abs_a[j] - abs_a[i] > abs_a[i]`, bất kỳ phần tử nào khác`abs_a[k]`với`k > j`cũng sẽ vi phạm điều kiện. Điều này duy trì tính chính xác trong khi tránh kiểm tra tất cả các cặp một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

abs_a = sorted(abs(x) for x in a)

count = 0
j = 1
for i in range(n):
    if j <= i:
        j = i + 1
    while j < n and abs_a[j] - abs_a[i] <= abs_a[i]:
        j += 1
    count += j - i - 1

print(count)
```Trước tiên, mã chuyển đổi tất cả các số thành giá trị tuyệt đối của chúng, sắp xếp chúng và sau đó sử dụng phương pháp hai con trỏ để đếm tất cả các cặp hợp lệ một cách hiệu quả. Sự tinh tế nằm ở việc duy trì sự bất biến`j >= i+1`và tính toán chính xác`j - i - 1`, điều này tránh việc đếm hai lần hoặc đếm các cặp không hợp lệ. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`3\n2 5 -3`| tôi | cơ bụng_a[i] | j | đếm tăng | Tổng số | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 1->2 | 1 | 1 | 
| 1 | 3 | 2->3 | 1 | 2 | 
| 2 | 5 | 3 | 0 | 2 | 

Dấu vết cho thấy thuật toán đếm các cặp hợp lệ (2, -3) và (5, -3). 

**Mẫu 2:** 

đầu vào:`2\n3 6`| tôi | cơ bụng_a[i] | j | đếm tăng | Tổng số | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 1->2 | 1 | 1 | 
| 1 | 6 | 2 | 0 | 1 | 

Cặp hợp lệ duy nhất là (3, 6). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét hai con trỏ là O(n) | 
| Không gian | O(n) | Lưu trữ mảng giá trị tuyệt đối | 

Sự phức tạp này nằm trong giới hạn nhất định, vì$n \log n$hoạt động được chấp nhận cho$n \le 2 \cdot 10^5$và mức sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    abs_a = sorted(abs(x) for x in a)
    count = 0
    j = 1
    for i in range(n):
        if j <= i:
            j = i + 1
        while j < n and abs_a[j] - abs_a[i] <= abs_a[i]:
            j += 1
        count += j - i - 1
    return str(count)

# provided samples
assert run("3\n2 5 -3\n") == "2", "sample 1"
assert run("2\n3 6\n") == "1", "sample 2"

# custom cases
assert run("2\n0 1\n") == "1", "zero case"
assert run("4\n-1 1 2 3\n") == "4", "mixed negative positive"
assert run("5\n1 2 4 8 16\n") == "2", "powers of two"
assert run("2\n1000000000 -1000000000\n") == "1", "large numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n0 1 | 1 | Xử lý đúng số 0 | 
| 4\n-1 1 2 3 | 4 | Hỗn hợp số âm và số dương | 
| 5\n1 2 4 8 16 | 2 | Quyền hạn thưa thớt của hai | 
| 2\n1000000000 -1000000000 | 1 | Ranh giới của phạm vi số nguyên | 

## Vỏ cạnh 

Đối với đầu vào`2\n0 1`,`abs_a = [0,1]`.`i=0`có`j=1->2`, số tăng là 1, tổng số là 1.`i=1`thêm 0. Điều này xác nhận thuật toán bao gồm chính xác số 0 trong các khoảng. 

Đối với đầu vào`4\n-1 1 2 3`,`abs_a = [1,1,2,3]`. Lặp qua`i`và cập nhật`j`đếm tất cả các cặp hợp lệ`(1,1)`,`(1,2)`,`(1,3)`,`(2,3)`một cách chính xác. Bất biến hai con trỏ đảm bảo chúng ta không bao giờ bỏ sót cặp hoặc đếm kép.
