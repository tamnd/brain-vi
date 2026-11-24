---
title: "CF 915A - Sân vườn"
description: "Chúng ta được yêu cầu tưới một khu vườn tuyến tính có chiều dài k bằng cách sử dụng một trong n thùng. Mỗi xô tưới một đoạn cố định của khu vườn mỗi giờ, cụ thể là một đoạn dài liên tục ai. Luba phải tưới toàn bộ khu vườn không để lại khoảng trống và không được tưới bất kỳ phần nào hai lần."
date: "2026-06-13T01:46:54+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 915
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 36 (Rated for Div. 2)"
rating: 900
weight: 915
solve_time_s: 704
verified: true
draft: false
---

[CF 915A - Vườn](https://codeforces.com/problemset/problem/915/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 11 phút 44 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tưới nước cho một khu vườn tuyến tính có chiều dài _k_ bằng cách sử dụng một trong _n_ xô. Mỗi xô tưới một đoạn cố định của khu vườn mỗi giờ, cụ thể là một đoạn dài liên tục _a_i_. Luba phải tưới toàn bộ khu vườn không để lại khoảng trống và không được tưới bất kỳ phần nào hai lần. Nhiệm vụ của chúng ta là xác định nhóm nào giảm thiểu tổng số giờ cần thiết. 

Dữ liệu đầu vào cho biết số lượng xô và chiều dài khu vườn trên dòng đầu tiên, tiếp theo là chiều dài mà mỗi xô có thể tưới trong một giờ. Kết quả là một số nguyên duy nhất, số giờ tối thiểu cần thiết để tưới vườn bằng một thùng duy nhất. 

Các ràng buộc rất nhỏ: _n_ và _k_ đều có nhiều nhất là 100. Điều đó có nghĩa là ngay cả một cách tiếp cận đơn giản, lặp lại trên tất cả các nhóm cũng hiệu quả. Chúng ta có thể yên tâm kiểm tra lần lượt từng thùng và thực hiện các phép tính đơn giản để xem nó có chia đều khu vườn hay không. Trường hợp đặc biệt có thể khiến việc triển khai ngây thơ không thành công là khi chiều dài tưới nước của xô không chia đều chiều dài khu vườn. Ví dụ: nếu _k_ = 6 và xô là [5, 2] thì xô có chiều dài 5 không thể tưới đầy đủ cho khu vườn, trong khi 2 chia chính xác cho 6, cho 3 giờ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi thùng, hãy kiểm tra xem chiều dài tưới nước của nó có chia chính xác chiều dài khu vườn hay không. Nếu có, hãy tính số giờ cần thiết như`k // a_i`. Theo dõi số giờ tối thiểu trên tất cả các nhóm. Điều này đúng vì việc tưới nước chỉ có thể được thực hiện theo từng đoạn liên tục có chiều dài chính xác bằng xô và không được phép tưới một phần của một đoạn. 

Một biến thể ngây thơ có thể cố gắng mô phỏng việc đặt các đoạn có độ dài`a_i`dọc theo vườn, nhưng điều đó là không cần thiết vì bộ phận trực tiếp xử lý việc đó. Vì giá trị tối đa cho _n_ và _k_ là 100, nên việc lặp qua tất cả các nhóm và thực hiện phép chia số nguyên cực kỳ nhanh, cho số lượng thao tác tối đa là 100, không đáng kể. 

Cái nhìn sâu sắc quan trọng giúp đơn giản hóa vấn đề này là nhận ra rằng vấn đề quy về phép chia số nguyên. Với mỗi thùng chia chính xác chiều dài khu vườn thì số giờ là`k // a_i`. Chúng ta chỉ cần chọn nhóm lớn nhất vì các phân đoạn lớn hơn sẽ giảm số giờ. Sự quan sát đó`k % a_i == 0`cho phép chúng ta loại bỏ những chiếc xô không thể tưới đầy đủ nước cho khu vườn trong nhiều giờ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số thùng`n`và chiều dài vườn`k`. 
2. Đọc danh sách độ dài xô`a`. 
3. Khởi tạo một biến`min_hours`đến một số lượng lớn để theo dõi số giờ tối thiểu. 
4. Lặp lại qua từng chiều dài nhóm`length`TRONG`a`. 
5. Kiểm tra xem`k % length == 0`. Nếu không, hãy bỏ qua thùng này vì nó không thể tưới vườn đầy đủ trong số giờ nguyên. 
6. Tính số giờ là`hours = k // length`. 
7. Cập nhật`min_hours`nếu như`hours`nhỏ hơn hiện tại`min_hours`. 
8. Sau khi kiểm tra tất cả các nhóm, hãy in`min_hours`. 

Bất biến ở đây là chúng ta chỉ xét chiều dài xô để phân chia khu vườn một cách chính xác. Vì vấn đề đảm bảo ít nhất một nhóm hợp lệ,`min_hours`sẽ luôn được cập nhật với giá trị chính xác. Vì các phân đoạn lớn hơn làm giảm số giờ nên phương pháp này đảm bảo chúng tôi tìm được số giờ tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

min_hours = float('inf')
for length in a:
    if k % length == 0:
        hours = k // length
        if hours < min_hours:
            min_hours = hours

print(min_hours)
```Giải pháp đọc đầu vào hiệu quả với`sys.stdin.readline`. Chúng tôi khởi tạo`min_hours`đến vô cùng để đảm bảo mọi số giờ hợp lệ đều nhỏ hơn. Chúng tôi lặp lại tất cả các nhóm, kiểm tra khả năng chia hết theo chiều dài khu vườn và cập nhật số giờ tối thiểu. Séc`k % length == 0`là rất quan trọng vì nó đảm bảo bao phủ toàn bộ khu vườn. Bản in cuối cùng cho ra số giờ tối thiểu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 6
2 3 5
```| Chiều dài xô | k % chiều dài | Giờ (k//chiều dài) | phút_giờ | 
| --- | --- | --- | --- | 
| 2 | 0 | 3 | 3 | 
| 3 | 0 | 2 | 2 | 
| 5 | 1 | - | 2 | 

Điều này chứng tỏ rằng chỉ những chiếc xô chia chính xác chiều dài khu vườn mới được xem xét. Số giờ tối thiểu là 2. 

### Mẫu 2 

đầu vào:```
2 7
1 3
```| Chiều dài xô | k % chiều dài | Giờ (k//chiều dài) | phút_giờ | 
| --- | --- | --- | --- | 
| 1 | 0 | 7 | 7 | 
| 3 | 1 | - | 7 | 

Ở đây chỉ có chiều dài xô 1 hoạt động, cho 7 giờ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Lặp lại từng nhóm một lần | 
| Không gian | O(n) | Lưu trữ danh sách độ dài nhóm | 

Với các ràng buộc nhỏ n ≤ 100 và k ≤ 100, thuật toán này chạy ngay lập tức trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    min_hours = float('inf')
    for length in a:
        if k % length == 0:
            hours = k // length
            if hours < min_hours:
                min_hours = hours
    return str(min_hours)

# provided samples
assert run("3 6\n2 3 5\n") == "2", "sample 1"
assert run("1 1\n1\n") == "1", "sample 2"

# custom cases
assert run("2 7\n1 3\n") == "7", "only bucket 1 works"
assert run("5 100\n1 2 4 5 10\n") == "10", "bucket 10 is optimal"
assert run("3 15\n4 5 6\n") == "3", "bucket 5 is best"
assert run("1 100\n100\n") == "1", "single bucket matches exactly"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 7\n1 3 | 7 | Chỉ có nhóm 1 hoạt động | 
| 5 100\n1 2 4 5 10 | 10 | Nhóm có ước số lớn nhất được chọn | 
| 3 15\n4 5 6 | 3 | Lựa chọn đúng giữa nhiều ước số | 
| 1 100\n100 | 1 | Xô đơn bằng chiều dài vườn | 

## Vỏ cạnh 

Nếu chiều dài khu vườn bằng thùng lớn nhất, chẳng hạn như k = 100 và thùng = 100, thì thuật toán sẽ xác định chính xác 1 giờ. Nếu nhóm nhỏ nhất là ước số duy nhất, như k = 7 và nhóm = 1, thì thuật toán sẽ tạo ra 7 giờ. Thuật toán tự động bỏ qua các thùng không phân chia, ngăn chặn việc tưới nước một phần không chính xác. Tất cả các trường hợp biên đều được kiểm tra khả năng phân chia và đảm bảo rằng ít nhất một nhóm hoạt động.
