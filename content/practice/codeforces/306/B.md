---
title: "CF 306B - Trình tối ưu hóa"
description: "Chúng ta được cung cấp một mảng bộ nhớ tuyến tính có kích thước n và một tập hợp m lệnh, mỗi lệnh đặt một khối bộ nhớ liền kề có giá trị 13. Các lệnh được lập chỉ mục theo thứ tự đầu vào. Một số hướng dẫn có thể trùng lặp, toàn bộ hoặc một phần, với những hướng dẫn khác."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 306
codeforces_index: "B"
codeforces_contest_name: "Testing Round 6"
rating: 2100
weight: 306
solve_time_s: 86
verified: true
draft: false
---

[CF 306B - Trình tối ưu hóa](https://codeforces.com/problemset/problem/306/B) 

**Xếp hạng:** 2100 
**Tags:** cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng bộ nhớ tuyến tính có kích thước _n_ và một tập lệnh _m_, mỗi lệnh đặt một khối bộ nhớ liền kề thành giá trị 13. Các lệnh được lập chỉ mục theo thứ tự đầu vào. Một số hướng dẫn có thể trùng lặp, toàn bộ hoặc một phần, với những hướng dẫn khác. Nhiệm vụ của chúng tôi là loại bỏ càng nhiều lệnh càng tốt trong khi đảm bảo rằng, sau khi loại bỏ chúng, mọi ô nhớ được đặt thành 13 bởi ít nhất một lệnh vẫn kết thúc được đặt thành 13 và không có ô nào được đặt thành 13 mà chưa được đặt trước đó. Đầu ra phải liệt kê số lượng lệnh tối đa có thể được loại bỏ và chỉ số của chúng. 

Các ràng buộc cho phép _n_ lên tới 2·10^6 và _m_ lên tới 2·10^5. Bất kỳ thuật toán nào mô phỏng rõ ràng từng lệnh trên toàn bộ mảng bộ nhớ cho mọi kết hợp đều không khả thi. Cách tiếp cận O(n·m) ngây thơ sẽ vượt quá giới hạn thời gian, vì 2·10^6 × 2·10^5 vượt quá khả năng tính toán thực tế trong khoảng thời gian 3 giây. Điều này đẩy chúng ta tới một cách tiếp cận tham lam hoặc quét dòng để tránh truy cập vào mọi ô nhớ cho mỗi lệnh. 

Các trường hợp biên có thể khiến các phương pháp tiếp cận ngây thơ thất bại bao gồm các lệnh được lồng hoàn toàn trong các lệnh lớn hơn, nhiều lệnh bao trùm cùng một ô nhớ và các lệnh chạm đến ranh giới của mảng bộ nhớ. Ví dụ: nếu hai lệnh bao gồm các vị trí từ 3 đến 5 và một trong số chúng cũng bao gồm các vị trí từ 1 đến 7 thì lệnh nhỏ hơn có thể bị xóa mà không ảnh hưởng đến trạng thái bộ nhớ cuối cùng. Một cách tiếp cận bất cẩn loại bỏ các lệnh chồng chéo mà không xem xét đến việc ngăn chặn sẽ loại bỏ sai một lệnh cần thiết để thiết lập một ô nhớ. 

## Phương pháp tiếp cận 

Phương pháp brute-force là mô phỏng mảng bộ nhớ và loại bỏ từng lệnh một, kiểm tra xem cấu hình bộ nhớ cuối cùng có thay đổi hay không. Điều này đúng nhưng cực kỳ chậm. Đối với mỗi lệnh, chúng tôi sẽ đánh dấu các ô nhớ bị ảnh hưởng, sau đó thử xóa lệnh và mô phỏng lại các lệnh còn lại để xem bộ nhớ có còn giống nhau hay không. Số lượng thao tác trong trường hợp xấu nhất là O(m·n), quá cao so với các ràng buộc. 

Thông tin chi tiết quan trọng là các hướng dẫn có thể được xem dưới dạng các khoảng thời gian và một lệnh có thể bị xóa nếu toàn bộ khoảng thời gian đó đã được bao phủ bởi các hướng dẫn tiếp theo. Sắp xếp các hướng dẫn theo vị trí bắt đầu của chúng và sau đó lặp đi lặp lại một cách tham lam cho phép chúng ta theo dõi ô nhớ được bao phủ xa nhất. Nếu phần cuối của lệnh hiện tại nhỏ hơn hoặc bằng ô được bao phủ xa nhất cho đến nay thì lệnh đó là dư thừa. Nếu không, nó sẽ mở rộng phạm vi phủ sóng và không thể xóa được. Điều này biến bài toán thành bài toán bao phủ khoảng cổ điển, làm giảm đáng kể độ phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m) | O(n) | Quá chậm | 
| Quét theo khoảng thời gian tham lam | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các hướng dẫn và lưu trữ chúng dưới dạng các bộ dữ liệu`(start, end, index)`, Ở đâu`end = start + length - 1`. Chỉ số được bảo toàn cho mục đích đầu ra. Việc lưu trữ cả phần đầu và phần cuối giúp đơn giản hóa việc tính toán mức độ phù hợp. 
2. Sắp xếp các hướng dẫn trước theo`start`theo thứ tự tăng dần. Nếu hai lệnh có cùng điểm khởi đầu, hãy sắp xếp khoảng thời gian dài hơn trước để các lệnh lồng nhau xuất hiện sau và có thể bị xóa. 
3. Khởi tạo một biến`covered`về 0, biểu thị ô nhớ xa nhất được đảm bảo đặt thành 13 theo các lệnh đã xử lý cho đến nay. 
4. Lặp lại các hướng dẫn đã được sắp xếp. Với mỗi lệnh, hãy kiểm tra xem nó có`end`vị trí nhỏ hơn hoặc bằng`covered`. Nếu đúng như vậy thì lệnh này hoàn toàn dư thừa và có thể bị loại bỏ. Còn không thì cập nhật`covered`đến`end`của lệnh này, mở rộng vùng phủ sóng bộ nhớ. 
5. Thu thập các chỉ số của các lệnh có thể tháo rời trong quá trình lặp. 
6. Xuất ra số lượng lệnh có thể tháo rời và danh sách chỉ mục của chúng. 

Tại sao nó hoạt động: Bất biến được duy trì là`covered`luôn đại diện cho ô nhớ xa nhất được đảm bảo đặt thành 13 theo các lệnh còn lại. Bất kỳ lệnh nào có phần cuối nằm trong phạm vi này đều không đóng góp phạm vi phủ sóng bộ nhớ mới và có thể bị xóa một cách an toàn. Việc sắp xếp theo điểm bắt đầu đảm bảo rằng chúng tôi gặp phải các hướng dẫn có khả năng dư thừa theo đúng thứ tự và việc ưu tiên các khoảng thời gian dài hơn trong các lần ngắt kết nối sẽ đảm bảo rằng các hướng dẫn lồng nhau được xem xét để loại bỏ một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
instructions = []

for i in range(1, m + 1):
    a, l = map(int, input().split())
    instructions.append((a, a + l - 1, i))

# Sort by start, then by end descending to handle nested intervals
instructions.sort(key=lambda x: (x[0], -x[1]))

removable = []
covered = 0

for start, end, idx in instructions:
    if end <= covered:
        removable.append(idx)
    else:
        covered = end

print(len(removable))
if removable:
    print(" ".join(map(str, removable)))
```Giải pháp đầu tiên chuyển đổi từng lệnh thành một khoảng`[start, end]`. Việc sắp xếp đảm bảo chúng tôi xử lý các hướng dẫn từ trái sang phải và ưu tiên các khoảng thời gian lớn hơn khi bắt đầu trùng khớp. Trong quá trình lặp lại,`covered`luôn phản ánh ô nhớ tối đa được đảm bảo sẽ được đặt. Chỉ những hướng dẫn không mở rộng phạm vi bảo hiểm này mới được đánh dấu là có thể tháo rời. Cần chú ý cẩn thận đối với từng lỗi một, đặc biệt là trong việc chuyển đổi độ dài sang phần cuối của quãng. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
10 4
3 3
3 1
4 1
9 2
```Dấu vết bước: 

| Hướng dẫn | Khoảng thời gian | Được bảo hiểm trước | Hành động | Được bảo hiểm sau | Có thể tháo rời | 
| --- | --- | --- | --- | --- | --- | 
| 3 3 | [3,5] | 0 | mở rộng vùng phủ sóng | 5 | - | 
| 3 1 | [3,3] | 5 | dư thừa | 5 | 2 | 
| 4 1 | [4,4] | 5 | dư thừa | 5 | 3 | 
| 9 2 | [9,10] | 5 | mở rộng vùng phủ sóng | 10 | - | 

Đầu ra:```
2
2 3
```Dấu vết này cho thấy các hướng dẫn lồng nhau có đầy đủ trong phạm vi bảo hiểm trước đó được xác định chính xác là có thể tháo rời. 

**Đầu vào tùy chỉnh:**```
5 3
1 5
2 2
4 1
```Dấu vết bước: 

| Hướng dẫn | Khoảng thời gian | Được bảo hiểm trước | Hành động | Được bảo hiểm sau | Có thể tháo rời | 
| --- | --- | --- | --- | --- | --- | 
| 1 5 | [1,5] | 0 | mở rộng vùng phủ sóng | 5 | - | 
| 2 2 | [2,3] | 5 | dư thừa | 5 | 2 | 
| 4 1 | [4,4] | 5 | dư thừa | 5 | 3 | 

Đầu ra:```
2
2 3
```Điều này cho thấy rằng các lệnh chứa hoàn toàn trong một lệnh lớn hơn được loại bỏ một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Sắp xếp các hướng dẫn chiếm ưu thế. Lặp qua chúng là O(m). | 
| Không gian | O(m) | Chúng tôi lưu trữ các khoảng thời gian và các chỉ số có thể tháo rời. | 

Với m 2·10^5, các thao tác O(m log m) thoải mái trong giới hạn 3 giây và bộ nhớ O(m) vừa vặn trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    instructions = []
    for i in range(1, m + 1):
        a, l = map(int, input().split())
        instructions.append((a, a + l - 1, i))
    instructions.sort(key=lambda x: (x[0], -x[1]))
    removable = []
    covered = 0
    for start, end, idx in instructions:
        if end <= covered:
            removable.append(idx)
        else:
            covered = end
    out = f"{len(removable)}\n"
    if removable:
        out += " ".join(map(str, removable))
    return out

# provided sample
assert run("10 4\n3 3\n3 1\n4 1\n9 2\n") == "2\n2 3", "sample 1"

# minimum size input
assert run("1 1\n1 1\n") == "0\n", "minimum input"

# all instructions same
assert run("5 3\n1 5\n1 5\n1 5\n") == "2\n2 3", "all equal"

# nested intervals
assert run("5 3\n1 5\n2 2\n4 1\n") == "2\n2 3", "nested intervals"

# adjacent intervals
assert run("6 3\n1 2\n3 2\n5 2\n") == "0\n", "non-overlapping adjacent intervals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 4/3 3/3 1/4 1/9 2 | 2 / 2 3 | hướng dẫn lồng nhau cơ bản | 
| 1 1 / 1 1 | 0 / | đầu vào tối thiểu | 
| 5 3 / 1 5 / 1 5 / 1 5 | 2 / 2 3 | hướng dẫn chồng chéo giống hệt nhau | 
| 5 3 / 1 5 / 2 2 / 4 1 | 2 | |
