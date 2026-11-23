---
title: "CF 475D - CGCDSSQ"
description: "Chúng ta được cung cấp một danh sách các số nguyên và một tập hợp các truy vấn. Mỗi truy vấn hỏi có bao nhiêu mảng con liền kề của danh sách có ước số chung lớn nhất bằng giá trị truy vấn."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "math"]
categories: ["algorithms"]
codeforces_contest: 475
codeforces_index: "D"
codeforces_contest_name: "Bayan 2015 Contest Warm Up"
rating: 2000
weight: 475
solve_time_s: 71
verified: true
draft: false
---

[CF 475D - CGCDSSQ](https://codeforces.com/problemset/problem/475/D) 

**Xếp hạng:** 2000 
**Tags:** bạo lực, cấu trúc dữ liệu, toán học 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và một tập hợp các truy vấn. Mỗi truy vấn hỏi có bao nhiêu mảng con liền kề của danh sách có ước số chung lớn nhất bằng giá trị truy vấn. Nói một cách đơn giản hơn, đối với mỗi số trong truy vấn, chúng tôi muốn biết có bao nhiêu phần tử liên tiếp trong mảng chia sẻ số đó làm ước số chung lớn nhất của chúng. Ví dụ, nếu mảng là`[2, 6, 3]`và truy vấn là`2`, các mảng con`[2]`Và`[2, 6]`có GCD bằng 2, cho kết quả là 2. 

Mảng có thể có tới`10^5`các phần tử và số lượng truy vấn có thể đạt tới`3 × 10^5`. Mỗi phần tử và mỗi truy vấn có thể lớn bằng`10^9`. Sự kết hợp cao này`n`và cao`q`ngay lập tức loại trừ bất kỳ giải pháp nào xem xét từng mảng con riêng lẻ. Một cách tiếp cận đơn giản sẽ yêu cầu kiểm tra các mảng con O(n^2), với n = 10^5 dẫn đến khoảng 5 × 10^9 phép toán, vượt xa giới hạn thời gian 2 giây. Mỗi truy vấn cũng không thể được xử lý độc lập bằng cách quét lại mảng, vì điều đó sẽ nhân công việc với q. 

Các trường hợp cạnh không rõ ràng xuất hiện khi các phần tử được lặp lại hoặc khi GCD của các đoạn dài giảm nhanh chóng. Ví dụ, trong mảng`[4, 8, 16]`và truy vấn`2`, quá trình quét ngây thơ có thể bỏ lỡ các mảng con như`[8, 16]`có GCD là 8 chứ không phải là 2, hoặc`[16]`có GCD là 16. Một trường hợp tinh vi khác xảy ra khi một truy vấn yêu cầu một số không phải là ước số của bất kỳ phần tử nào; câu trả lời phải là 0, không phải là đếm sai các trận đấu gần nhất. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ lặp lại tất cả các chỉ số ban đầu có thể có, sau đó mở rộng từng mảng con sang bên phải, tính toán GCD tăng dần. Đối với mỗi lần mở rộng, chúng tôi sẽ kiểm tra xem GCD hiện tại có khớp với bất kỳ truy vấn nào không. Điều này đúng vì nó đếm mọi mảng con theo đúng nghĩa đen, nhưng độ phức tạp trong trường hợp xấu nhất là O(n^2), không thành công với n lên tới 10^5. 

Thông tin chi tiết quan trọng để cải thiện hiệu quả là hàm GCD không tăng khi mở rộng mảng con sang bên phải. Khi chúng ta biết GCD của một mảng con kết thúc ở vị trí i, việc mở rộng nó bằng cách thêm phần tử tiếp theo sẽ chỉ làm giảm GCD hoặc giữ nguyên GCD. Điều này có nghĩa là chúng ta có thể duy trì ánh xạ từ các GCD có thể có tới số lượng mảng con kết thúc ở vị trí hiện tại. Đối với mỗi phần tử, chúng tôi lấy tất cả các GCD trước đó, kết hợp chúng với phần tử hiện tại bằng GCD và tổng số lượng. Phương pháp lập trình động này tạo ra tất cả các GCD mảng con có thể có trong thời gian tuyến tính tương ứng với số lượng giá trị GCD duy nhất được nhìn thấy ở mỗi bước. Quan sát cho thấy số lượng GCD riêng biệt ở mỗi bước được giới hạn bởi O(log max(a_i)) đảm bảo hiệu quả. 

Cách tiếp cận này chuyển đổi một vấn đề O(n^2) thành một vấn đề gần hơn với O(n log A) trong đó A là phần tử tối đa. Nó cũng cho phép chúng ta tính toán trước số lượng cho tất cả các GCD có thể, khiến truy vấn trả lời O(1) cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tổng hợp GCD | O(n log A + q) | O(n log A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo từ điển`count_gcd`để lưu trữ số lượng tích lũy của từng GCD gặp phải trên tất cả các mảng con. Điều này sẽ cho phép trả lời truy vấn trực tiếp sau khi xử lý mảng. 
2. Khởi tạo một từ điển khác`current_gcds`ánh xạ giá trị GCD tới số mảng con kết thúc ở phần tử hiện tại có GCD đó. Bắt đầu trống rỗng. 
3. Lặp lại từng phần tử`a[i]`trong mảng. Đối với mỗi bước, khởi tạo`next_gcds`như một từ điển trống để lưu trữ số lượng GCD mới cho các mảng con kết thúc tại`i`. 
4. Bao gồm mảng con chỉ chứa`a[i]`chính nó. Bộ`next_gcds[a[i]] = 1`. Điều này xử lý các mảng con một phần tử. 
5. Đối với mỗi GCD`g`TRONG`current_gcds`, tính toán`gcd(g, a[i])`và tăng dần`next_gcds[gcd(g, a[i])]`theo số lượng`current_gcds[g]`. Bản cập nhật này được tính cho tất cả các mảng con mở rộng. 
6. Hợp nhất`next_gcds`vào trong`count_gcd`bằng cách cộng số lượng của chúng. Điều này giữ tổng số lượng cho mỗi GCD gặp phải cho đến nay. 
7. Thay thế`current_gcds`với`next_gcds`và tiếp tục đến phần tử tiếp theo. 
8. Sau khi xử lý toàn bộ mảng, mỗi truy vấn`x`có thể được trả lời bằng cách nhìn lên`count_gcd.get(x, 0)`. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến`current_gcds`đếm chính xác số mảng con kết thúc ở chỉ mục hiện tại cho mỗi GCD có thể. Bằng cách lặp lại điều này trên mảng, tất cả các mảng con có thể được tính chính xác một lần. Thuộc tính không tăng của GCD đảm bảo không có mảng con GCD nào bị bỏ sót hoặc bị tính hai lần. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
q = int(input())
queries = [int(input()) for _ in range(q)]

from collections import defaultdict

count_gcd = defaultdict(int)
current_gcds = {}

for num in a:
    next_gcds = defaultdict(int)
    next_gcds[num] += 1
    for g, cnt in current_gcds.items():
        new_g = math.gcd(g, num)
        next_gcds[new_g] += cnt
    for g, cnt in next_gcds.items():
        count_gcd[g] += cnt
    current_gcds = next_gcds

for x in queries:
    print(count_gcd.get(x, 0))
```Khối đầu tiên đọc đầu vào và chuẩn bị lưu trữ truy vấn. các`count_gcd`từ điển tích lũy số lượng toàn cầu trong khi`current_gcds`theo dõi các mảng con kết thúc ở vị trí hiện tại. sử dụng`math.gcd`đảm bảo tính chính xác cho mọi sự kết hợp, và`defaultdict(int)`ngăn chặn KeyErrors. Việc tăng số lượng thay vì ghi đè là rất quan trọng để duy trì số lượng chính xác cho các mảng con có chung GCD. Vòng lặp cuối cùng in kết quả trực tiếp từ từ điển được tính toán trước. 

## Ví dụ đã hoạt động 

Xem xét đầu vào mẫu`[2, 6, 3]`với các truy vấn`[1, 2, 3, 4, 6]`. 

| Bước | current_gcds | next_gcds | đếm_gcd | 
| --- | --- | --- | --- | 
| 2 | {} | {2:1} | {2:1} | 
| 6 | {2:1} | {6:1, 2:1} | {2:2, 6:1} | 
| 3 | {6:1, 2:1} | {3:2,1:1} | {2:2,6:1,3:2,1:1} | 

Dấu vết này cho thấy mỗi mảng con kết thúc ở mỗi vị trí đóng góp như thế nào vào tổng số GCD. Số lượng khớp với các câu trả lời dự kiến cho các truy vấn: 1 bản đồ thành 1, 2 bản đồ thành 2, 3 bản đồ thành 2, 4 bản đồ thành 0, 6 bản đồ thành 1. 

Một ví dụ khác: mảng`[4, 8, 16]`và truy vấn`[4,8,16,2]`. 

| Bước | current_gcds | next_gcds | đếm_gcd | 
| --- | --- | --- | --- | 
| 4 | {} | {4:1} | {4:1} | 
| 8 | {4:1} | {8:1,4:1} | {4:2,8:1} | 
| 16 | {8:1,4:1} | {16:1,8:1,4:1} | {4:3,8:2,16:1} | 

Tất cả các mảng con đều được tính chính xác, bao gồm cả các GCD chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A + q) | Tại mỗi phần tử, chúng tôi mở rộng tối đa O(log A) GCD riêng biệt, với q truy vấn được truy cập trong O(1) mỗi GCD. | 
| Không gian | O(n log A) | Từ điển lưu trữ số lượng GCD riêng biệt được thấy cho đến nay; mỗi phần tử có thể là O(log A) cho mỗi phần tử. | 

Với`n`lên đến 10^5 và`A`lên tới 10^9, log A ≈ 30, tương đương khoảng 3 × 10^6 thao tác, thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())  # assume solution in solution.py
    return output.getvalue().strip()

# Provided sample
assert run("3\n2 6 3\n5\n1\n2\n3\n4\n6\n") == "1\n2\n2\n0\n1"

# Minimum-size input
assert run("1\n7\n3\n1\n7\n10\n") == "0\n1\n0"

# All-equal elements
assert run("3\n5 5 5
```
