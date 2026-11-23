---
title: "CF 417B - Tai nạn"
description: "Chúng tôi được cung cấp một danh sách các bài nộp được thực hiện bởi những người tham gia một cuộc thi lập trình. Mỗi lần gửi được mô tả bằng hai số: x, tính số lượng giải pháp duy nhất mà người tham gia này đã gửi trước giải pháp này và k, ID của người tham gia."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 417
codeforces_index: "B"
codeforces_contest_name: "RCC 2014 Warmup (Div. 2)"
rating: 1400
weight: 417
solve_time_s: 89
verified: true
draft: false
---

[CF 417B - Sự cố](https://codeforces.com/problemset/problem/417/B) 

**Đánh giá:** 1400 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các bài nộp được thực hiện bởi những người tham gia một cuộc thi lập trình. Mỗi lần gửi được mô tả bằng hai số: _x_, tính số lượng giải pháp duy nhất mà người tham gia này đã gửi trước giải pháp này và _k_, ID của người tham gia. Bài gửi của cùng một người tham gia phải xuất hiện theo thứ tự thời gian: nếu bài gửi hiển thị _x = 2_ thì phải có bài gửi trước đó của cùng một người tham gia với _x = 1_ ở đâu đó trước đó trong danh sách. Nhiệm vụ là xác minh rằng danh sách được khôi phục tôn trọng thứ tự thời gian này đối với mọi người tham gia. 

Các ràng buộc chỉ ra rằng có thể có tới 100.000 lượt gửi và mỗi ID người tham gia và số giải pháp cũng có thể lên tới 100.000. Điều này ngụ ý rằng thuật toán bậc hai quét tất cả các lần gửi trước đó cho mỗi lần gửi mới sẽ quá chậm. Chúng tôi cần một giải pháp có quy mô tuyến tính hoặc gần tuyến tính với số lượng bài gửi. Các trường hợp biên bao gồm nhiều người tham gia với các lần gửi xen kẽ, những người tham gia có khoảng trống trong giá trị _x_ của họ và trường hợp nhỏ nhất chỉ có một lần gửi. Ví dụ: một danh sách như:```
3
0 1
2 1
1 1
```phải ghi "KHÔNG" vì lần gửi thứ hai yêu cầu _x = 2_ không có _x = 1_ trước cho người tham gia 1, mặc dù _x = 1_ xuất hiện sau. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là duy trì danh sách tất cả các bài gửi trước đó cho mỗi người tham gia và đối với mỗi bài gửi mới, hãy quét ngược lại để đảm bảo tất cả các giá trị _x_ nhỏ hơn đã xuất hiện. Điều này có thể đúng, nhưng trong trường hợp xấu nhất, mỗi lần gửi có thể yêu cầu quét qua gần như tất cả các lần gửi trước đó, dẫn đến độ phức tạp về thời gian là O(n2). Với n lên tới 10^5, cách tiếp cận này không thực tế. 

Quan sát quan trọng là đối với mỗi người tham gia, chúng ta chỉ cần biết _x_ lớn nhất đã xuất hiện cho đến nay. Nếu nội dung gửi mới cho người tham gia _k_ có _x_ lớn hơn mức tối đa hiện tại cho _k_ cộng 1 thì danh sách không hợp lệ. Nếu _x_ bằng mức tối đa hiện tại hoặc mức tối đa hiện tại cộng 1 thì nó nhất quán. Do đó, chúng tôi có thể lưu trữ _x_ tối đa hiện tại cho mỗi người tham gia vào từ điển. Điều này làm giảm việc kiểm tra mỗi lần gửi tới O(1), mang lại giải pháp tổng thể O(n). Ý tưởng là thứ tự thời gian chỉ yêu cầu theo dõi chỉ mục được gửi mới nhất của mỗi người tham gia; chúng tôi không bao giờ cần danh sách đầy đủ các bài nộp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một từ điển trống để ánh xạ ID người tham gia tới _x_ lớn nhất được thấy cho đến nay. Điều này ghi lại nội dung gửi theo trình tự thời gian gần đây nhất của mỗi người tham gia. 
2. Lặp lại từng lần gửi trong danh sách. Đối với mỗi lần gửi, hãy đọc ID người tham gia _k_ và số giải pháp _x_. 
3. Kiểm tra từ điển để biết mức tối đa hiện tại của người tham gia. Nếu người tham gia không có bài nộp nào trước đó, hãy coi mức tối đa là -1. 
4. Nếu _x_ mới lớn hơn mức tối đa hiện tại cộng thêm 1, ngay lập tức xuất ra "NO" và dừng. Điều này có nghĩa là có một số bài nộp bị bỏ qua và thứ tự thời gian bị sai. 
5. Nếu không, hãy cập nhật _x_ tối đa của người tham gia thành giá trị tối đa của giá trị hiện tại và _x_. Điều này xử lý các lần gửi lặp lại với cùng một _x_ mà không phá vỡ trình tự. 
6. Nếu vòng lặp hoàn tất mà không phát hiện vi phạm, xuất ra "CÓ". 

Điều bất biến được duy trì là đối với mọi người tham gia, từ điển luôn lưu trữ _x_ lớn nhất được thấy cho đến thời điểm đó trong chuỗi. Nếu bất kỳ bài gửi nào vi phạm quy tắc _x ≤ current_max + 1_, thứ tự thời gian chắc chắn sẽ bị phá vỡ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
latest = {}

valid = True
for _ in range(n):
    x, k = map(int, input().split())
    current_max = latest.get(k, -1)
    if x > current_max + 1:
        valid = False
        break
    latest[k] = max(current_max, x)

print("YES" if valid else "NO")
```Từ điển`latest`theo dõi chỉ số gửi lớn nhất cho mỗi người tham gia. sử dụng`get(k, -1)`cho phép chúng tôi xử lý những người tham gia chưa gửi bất kỳ giải pháp nào. các`max`đảm bảo việc gửi đi lặp lại với cùng một _x_ không làm giảm mức tối đa được lưu trữ. Chấm dứt sớm khi vi phạm sẽ tránh được những công việc không cần thiết. 

## Ví dụ đã hoạt động 

Dấu vết cho đầu vào mẫu:```
2
0 1
1 1
```| Bước | x | k | hiện tại_max | Kiểm tra | mới nhất[k] | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | -1 | 0 ≤ -1+1 | 0 | Đúng | 
| 2 | 1 | 1 | 0 | 1 0+1 | 1 | Đúng | 

Thuật toán xác nhận thứ tự thời gian và in "CÓ". 

Dấu vết cho một trường hợp thất bại:```
3
0 1
2 1
1 1
```| Bước | x | k | hiện tại_max | Kiểm tra | mới nhất[k] | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | -1 | 0 0 0 | 0 | Đúng | 
| 2 | 2 | 1 | 0 | 2 ≤ 0+1? | - | Sai | 

Thuật toán dừng ở bước 2, in "KHÔNG", bắt chính xác bài gửi bị bỏ qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lần gửi được xử lý một lần, với quyền truy cập từ điển O(1). | 
| Không gian | O(n) | Từ điển lưu trữ một mục nhập cho mỗi người tham gia, tối đa n người tham gia. | 

Với n lên tới 100.000, thuật toán hoạt động tốt trong giới hạn thời gian 1 giây thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    latest = {}
    valid = True
    for _ in range(n):
        x, k = map(int, input().split())
        current_max = latest.get(k, -1)
        if x > current_max + 1:
            valid = False
            break
        latest[k] = max(current_max, x)
    return "YES" if valid else "NO"

# Provided sample
assert run("2\n0 1\n1 1\n") == "YES", "sample 1"

# Edge: single submission
assert run("1\n0 1\n") == "YES", "single submission"

# Skipped submission
assert run("3\n0 1\n2 1\n1 1\n") == "NO", "skipped submission"

# Multiple participants
assert run("4\n0 1\n0 2\n1 1\n1 2\n") == "YES", "multiple participants"

# Interleaved violation
assert run("4\n0 1\n1 2\n2 1\n1 2\n") == "NO", "interleaved violation"

# Large x repeated
assert run("3\n0 1\n0 1\n1 1\n") == "YES", "repeated same x allowed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n0 1`| CÓ | Trường hợp đầu vào tối thiểu | 
|`3\n0 1\n2 1\n1 1`| KHÔNG | Số bài nộp bị bỏ qua | 
|`4\n0 1\n0 2\n1 1\n1 2`| CÓ | Nhiều người tham gia duy trì trình tự độc lập | 
|`4\n0 1\n1 2\n2 1\n1 2`| KHÔNG | Đệ trình xen kẽ có thể phá vỡ trật tự | 
|`3\n0 1\n0 1\n1 1`| CÓ | Cho phép gửi bài trùng lặp có cùng x | 

## Vỏ cạnh 

Đối với trường hợp bỏ qua bài nộp`3\n0 1\n2 1\n1 1\n`, bộ thuật toán`latest[1]`về 0 sau lần gửi đầu tiên. Ở lần gửi thứ hai với x=2, việc kiểm tra`2 > 0+1`không thành công, ngay lập tức trả về "KHÔNG" mà không cần nhìn đến dòng cuối cùng. Điều này thể hiện việc xử lý chính xác các chuỗi không theo thứ tự. Đối với các giá trị x lặp lại, như`0 1`hai lần,`latest[1]`vẫn là 0, cho phép gửi lần tiếp theo với x=1, phù hợp với mức tối đa hiện tại, xác nhận rằng các giải pháp lặp lại có thể chấp nhận được.
