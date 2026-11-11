---
title: "CF 158A - Vòng tiếp theo"
description: "Chúng tôi được trao thứ hạng cuối cùng của một cuộc thi lập trình. Điểm đã được sắp xếp theo thứ tự không tăng dần, nghĩa là mỗi người tham gia có điểm lớn hơn hoặc bằng điểm của người tham gia tiếp theo. Người tham gia sẽ được vào vòng tiếp theo nếu có hai điều kiện đúng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 158
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2012 Qualification Round 1"
rating: 800
weight: 158
solve_time_s: 89
verified: true
draft: false
---

[CF 158A - Vòng tiếp theo](https://codeforces.com/problemset/problem/158/A) 

**Đánh giá:** 800 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao thứ hạng cuối cùng của một cuộc thi lập trình. Điểm đã được sắp xếp theo thứ tự không tăng dần, nghĩa là mỗi người tham gia có điểm lớn hơn hoặc bằng điểm của người tham gia tiếp theo. 

Người tham gia sẽ được vào vòng tiếp theo nếu có hai điều kiện đúng. Điểm của họ ít nhất phải lớn bằng điểm của người tham gia hiện ở vị trí thứ k và điểm của họ phải dương. 

Nhiệm vụ là đếm xem có bao nhiêu người tham gia thỏa mãn cả hai điều kiện. 

Những hạn chế là cực kỳ nhỏ. Có tối đa 50 người tham gia, vì vậy ngay cả một lần quét tuyến tính đơn giản cũng là quá đủ. Bất kỳ thuật toán nào có độ phức tạp về thời gian lên tới O(n²) sẽ nằm gọn trong giới hạn một cách thoải mái. Vấn đề này chủ yếu liên quan đến việc thực hiện cẩn thận hơn là tối ưu hóa. 

Phần khó khăn là xử lý chính xác điểm 0 và các mối quan hệ xung quanh vị trí thứ k. 

Hãy xem xét ví dụ này:```
5 3
10 5 0 0 0
```Người tham gia thứ 3 có điểm 0. Giải pháp bất cẩn có thể tính tất cả những người có điểm lớn hơn hoặc bằng 0 sẽ ra kết quả là 5. Điều đó sai vì người tham gia cũng phải có điểm dương. Câu trả lời đúng là 2. 

Một trường hợp tinh vi khác xuất hiện khi nhiều người tham gia có cùng số điểm với người tham gia thứ k.```
8 5
10 9 8 7 7 7 5 5
```Thí sinh đứng thứ 5 có điểm 7. Mọi người có điểm 7 trở lên sẽ tiến lên, kể cả thí sinh thứ 6. Câu trả lời đúng là 6. Việc triển khai sai có thể chỉ tạo ra k, điều này sẽ bỏ lỡ những người tham gia bị ràng buộc. 

Thêm một trường hợp cạnh là đầu vào nhỏ nhất có thể:```
1 1
0
```Người tham gia duy nhất đạt điểm 0, vì vậy không ai tiến bộ. Câu trả lời là 0 chứ không phải 1. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra từng người tham gia và kiểm tra xem họ có đáp ứng các quy định của cuộc thi hay không. Đầu tiên chúng ta tìm điểm của người tham gia thứ k, sau đó đếm xem có bao nhiêu điểm vừa dương và ít nhất là ngưỡng đó. 

Phương pháp vũ phu này đã hoàn toàn có thể chấp nhận được vì n nhiều nhất là 50. Trong trường hợp xấu nhất, chúng tôi thực hiện khoảng 50 so sánh, điều này thực sự là tức thời. 

Cách tiếp cận phức tạp hơn là không cần thiết vì đầu vào đã được sắp xếp. Quan sát duy nhất chúng ta cần là người tham gia thứ k xác định điểm đủ điều kiện tối thiểu. Khi chúng tôi biết giá trị đó, mọi người tham gia có thể được kiểm tra độc lập. 

Brute-force hoạt động vì quy tắc định tính chỉ phụ thuộc vào một ngưỡng cố định. Không cần tìm kiếm, sắp xếp hoặc cấu trúc dữ liệu bổ sung. Một lần quét tuyến tính là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`Và`k`. 
2. Đọc mảng điểm. 
3. Tìm số điểm của người tham gia ở vị trí thứ k. Vì Python sử dụng chỉ mục dựa trên 0 nên giá trị này là`scores[k - 1]`. 
4. Khởi tạo bộ đếm về 0. 
5. Lặp lại từng điểm trong mảng. 
6. Đối với mỗi điểm, hãy kiểm tra hai điều kiện: 

1. Điểm lớn hơn hoặc bằng điểm thứ k. 
2. Điểm hoàn toàn lớn hơn 0. 
7. Nếu cả hai điều kiện đều đúng, hãy tăng bộ đếm. 
8. In bộ đếm cuối cùng. 

Điều kiện thứ hai là cần thiết vì người tham gia có điểm 0 không bao giờ tiến lên, ngay cả khi người tham gia thứ k cũng đạt điểm 0. 

### Tại sao nó hoạt động 

Người tham gia thứ k thiết lập số điểm tối thiểu cần thiết để đủ điều kiện. Bất kỳ người tham gia nào có số điểm nhỏ hơn sẽ không thể tiến lên. Bất kỳ người tham gia nào có cùng số điểm hoặc số điểm lớn hơn sẽ đủ điều kiện, miễn là điểm của họ là dương. 

Thuật toán kiểm tra chính xác hai quy tắc này cho mọi người tham gia. Vì mỗi người tham gia đủ điều kiện được tính một lần và mọi người tham gia không đủ điều kiện đều bị bỏ qua nên số đếm cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
scores = list(map(int, input().split()))

threshold = scores[k - 1]

answer = 0

for score in scores:
    if score >= threshold and score > 0:
        answer += 1

print(answer)
```Chương trình bắt đầu bằng việc đọc số lượng người tham gia và vị trí đủ điều kiện. Điểm số được lưu trữ trong một danh sách. 

Biến`threshold`lưu trữ điểm của người tham gia thứ k. Đây là số điểm tối thiểu cần thiết để thăng tiến. 

Vòng lặp xử lý từng người tham gia một cách độc lập. Điều kiện:```
score >= threshold and score > 0
```phù hợp chính xác với thể lệ cuộc thi. 

Phần đầu tiên xử lý các mối quan hệ một cách chính xác. Nếu một số người tham gia có cùng số điểm với người tham gia thứ k thì tất cả họ đều được tính. 

Phần thứ hai ngăn cản những người tham gia có điểm 0 tiến lên. Đây là chi tiết dễ gây ra sai sót nhất. 

Việc thực hiện sử dụng`k - 1`vì danh sách Python được lập chỉ mục từ 0 trong khi vị trí cuộc thi bắt đầu từ 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8 5
10 9 8 7 7 7 5 5
```Người tham gia thứ k đứng ở vị trí thứ 5 với số điểm 7. 

| Chỉ mục | Điểm | Ngưỡng | Tích cực? | Đủ điều kiện? | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 7 | Có | Có | 1 | 
| 2 | 9 | 7 | Có | Có | 2 | 
| 3 | 8 | 7 | Có | Có | 3 | 
| 4 | 7 | 7 | Có | Có | 4 | 
| 5 | 7 | 7 | Có | Có | 5 | 
| 6 | 7 | 7 | Có | Có | 6 | 
| 7 | 5 | 7 | Có | Không | 6 | 
| 8 | 5 | 7 | Có | Không | 6 | 

Câu trả lời cuối cùng:```
6
```Ví dụ này chứng minh tại sao mối quan hệ lại quan trọng. Dù k là 5 nhưng người tham gia thứ 6 cũng tiến lên vì điểm của họ khớp với ngưỡng. 

### Ví dụ 2 

đầu vào:```
4 2
0 0 0 0
```Người tham gia thứ k có điểm 0. 

| Chỉ mục | Điểm | Ngưỡng | Tích cực? | Đủ điều kiện? | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | Không | Không | 0 | 
| 2 | 0 | 0 | Không | Không | 0 | 
| 3 | 0 | 0 | Không | Không | 0 | 
| 4 | 0 | 0 | Không | Không | 0 | 

Câu trả lời cuối cùng:```
0
```Dấu vết này nêu bật tầm quan trọng của yêu cầu về điểm tích cực. Chỉ phù hợp với ngưỡng là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét mảng điểm một lần | 
| Không gian | O(1) | Chỉ có một vài biến bổ sung được sử dụng | 

Với tối đa 50 người tham gia, thời gian chạy rất nhỏ. Việc sử dụng bộ nhớ cũng không đáng kể. Giải pháp dễ dàng phù hợp với giới hạn của cuộc thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    scores = list(map(int, input().split()))

    threshold = scores[k - 1]

    ans = 0

    for score in scores:
        if score >= threshold and score > 0:
            ans += 1

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided samples
assert run("8 5\n10 9 8 7 7 7 5 5\n") == "6\n", "sample 1"

# custom cases
assert run("1 1\n0\n") == "0\n", "minimum size with zero"
assert run("1 1\n100\n") == "1\n", "minimum size with positive score"
assert run("5 3\n10 5 0 0 0\n") == "2\n", "zero threshold handling"
assert run("6 4\n5 5 5 5 5 5\n") == "6\n", "all equal values"
assert run("5 5\n9 7 5 3 1\n") == "5\n", "k equals n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`|`0`| Người tham gia duy nhất không có điểm | 
|`1 1 / 100`|`1`| Người tham gia duy nhất đủ điều kiện | 
|`5 3 / 10 5 0 0 0`|`2`| Người tham gia có điểm 0 không được tiến lên | 
|`6 4 / 5 5 5 5 5 5`|`6`| Tất cả những người tham gia bị ràng buộc đều đủ điều kiện | 
|`5 5 / 9 7 5 3 1`|`5`| Xử lý khi k là người tham gia cuối cùng | 

## Vỏ cạnh 

Hãy xem xét trường hợp người tham gia thứ k có điểm 0. 

đầu vào:```
5 3
10 5 0 0 0
```Ngưỡng trở thành 0 vì người tham gia ở vị trí thứ 3 ghi được 0. 

Thuật toán kiểm tra mọi người tham gia:```
10 >= 0 and 10 > 0  -> counted
5 >= 0 and 5 > 0    -> counted
0 >= 0 and 0 > 0    -> not counted
```Đáp án cuối cùng là 2. Phần bổ sung`score > 0`điều kiện ngăn cản việc đếm sai. 

Bây giờ hãy xem xét các mối quan hệ xung quanh điểm cắt. 

đầu vào:```
8 5
10 9 8 7 7 7 5 5
```Ngưỡng là 7. Thuật toán tính mọi người tham gia có điểm ít nhất là 7. 

Điều đó bao gồm người tham gia ở vị trí thứ 6 vì:```
7 >= 7 and 7 > 0
```là đúng. 

Câu trả lời cuối cùng trở thành 6, xử lý chính xác các điểm hòa. 

Cuối cùng, kiểm tra đầu vào nhỏ nhất có thể. 

đầu vào:```
1 1
0
```Ngưỡng là 0. Người tham gia duy nhất không đạt được điều kiện điểm tích cực, do đó bộ đếm không bao giờ tăng. Đầu ra là 0, khớp chính xác với các quy tắc.
