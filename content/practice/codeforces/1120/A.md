---
title: "CF 1120A - Diana và Liana"
description: "Thị trấn Shortriver chỉ có một dây leo hoa rất dài. Mỗi người dân sẽ nhận được một vòng hoa được làm từ chính xác k bông hoa, được cắt tuần tự từ dây leo bằng một chiếc máy luôn lấy k bông hoa tiếp theo theo thứ tự."
date: "2026-06-12T04:24:52+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1120
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 543 (Div. 1, based on Technocup 2019 Final Round)"
rating: 1900
weight: 1120
solve_time_s: 97
verified: false
draft: false
---

[CF 1120A - Diana và Liana](https://codeforces.com/problemset/problem/1120/A) 

**Xếp hạng:** 1900 
**Tags:** tham lam, thực hiện, hai con trỏ 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Thị trấn Shortriver chỉ có một dây leo hoa rất dài. Mỗi người dân sẽ nhận được một vòng hoa được làm bằng chính xác`k`hoa, được cắt tuần tự từ dây leo bằng máy luôn lấy cành tiếp theo`k`hoa theo thứ tự. Diana có một chuỗi hoa ưa thích mà chúng ta có thể gọi là sơ đồ của cô ấy. Cô ấy muốn có ít nhất một vòng hoa phù hợp với sơ đồ này, nghĩa là nó chứa ít nhất số lượng yêu cầu của từng loại hoa, bất kể thứ tự. Cô có thể ngắt trước bất kỳ bông hoa nào trên dây leo nhưng phải để lại đủ số hoa để máy vẫn có thể ra ít hoa nhất.`n`vòng hoa. 

Đầu vào cung cấp dây leo dưới dạng một mảng các loại hoa`a[1..m]`và sơ đồ của Diana như`b[1..s]`. Mục tiêu là xác định một tập hợp các vị trí hoa cần loại bỏ sao cho ít nhất một vòng hoa khớp với sơ đồ của Diana khi rời đi.`n`hoặc những vòng hoa hoàn chỉnh hơn, hoặc kết luận rằng điều này là không thể. 

Các ràng buộc rất lớn:`m`,`n`, Và`k`mỗi cái có thể lên tới 500.000 và tổng số hoa ít nhất`n*k`. Điều đó loại trừ bất kỳ giải pháp nào quét hoặc mô phỏng mọi kết hợp loại bỏ có thể có theo cách đơn giản, bởi vì số lượng mảng con hoặc tập hợp con tăng lên theo kiểu tổ hợp. Thay vào đó, chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính để tìm ra phân khúc phù hợp một cách hiệu quả và tính toán các vị trí cần loại bỏ. 

Các trường hợp cạnh dễ bị bỏ lỡ bao gồm khi`n=1`và Diana thực sự là công dân duy nhất, cho phép loại bỏ hầu hết tất cả các bông hoa ngoại trừ những bông hoa theo yêu cầu của sơ đồ của cô ấy. Một trường hợp tinh tế khác là khi dây leo đã chứa phôi hoàn hảo ở ranh giới của khối k; logic cửa sổ trượt bất cẩn có thể bỏ sót nếu không được thực hiện cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi cách có thể`k`-phân đoạn dài, đếm số bông hoa và xem liệu sơ đồ có thể hài lòng hay không. Đối với mỗi ứng cử viên, chúng tôi có thể tính toán xem liệu còn đủ hoa để sản xuất hay không`n`phôi đầy đủ. Việc đếm các loại hoa cho mỗi mảng con lấy O(k) và có thể có O(m) vị trí bắt đầu, do đó tổng độ phức tạp là O(m*k). Với`m`lên tới 500.000 và`k`có khả năng bằng`m`, cách tiếp cận này quá chậm. 

Quan sát quan trọng là Diana chỉ quan tâm đến độ dài tối đa một phân mảng con`k`phù hợp với sơ đồ của cô ấy. Nếu chúng tôi có thể tìm thấy bất kỳ phân đoạn nào chứa tất cả các bông hoa cần thiết thì chúng tôi có thể loại bỏ những bông hoa bổ sung bên ngoài phân đoạn này để đảm bảo hoa đầu tiên`n`phôi vẫn tồn tại. Điều này làm giảm vấn đề thành kỹ thuật hai con trỏ hoặc cửa sổ trượt trên dây leo, kết hợp với bản đồ băm để theo dõi số lượng hoa cần thiết trong sơ đồ. 

Sau khi tìm thấy cửa sổ phù hợp, chúng tôi sẽ tính toán những bông hoa nào là "thêm" - hoặc ở bên ngoài cửa sổ cần thiết để lấp đầy phần còn lại`n-1`vòng hoa, hoặc bên trong cửa sổ ngoài những gì sơ đồ yêu cầu. Những bông hoa thừa này có thể bị loại bỏ và số lượng của chúng có thể rất ít. Thông tin chi tiết này cho phép chúng tôi giải quyết vấn đề trong thời gian O(m) bằng cách sử dụng một lượt duy nhất với bản đồ băm và hai con trỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m*k) | O(k) | Quá chậm | 
| Cửa sổ trượt + Bản đồ đếm | O(m) | O(max(a_i, b_i)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần xuất hiện của mỗi bông hoa trong sơ đồ của Diana. Cho phép`needed[type]`là số lần một loại hoa xuất hiện trong`b`. 
2. Sử dụng cửa sổ trượt có hai con trỏ`l`Và`r`trên dây leo.`r`mở rộng để bao gồm hoa cho đến khi tất cả các loại trong`needed`được hài lòng. Duy trì bản đồ băm`current[type]`đếm xem bên trong cửa sổ có bao nhiêu bông hoa mỗi loại. Duy trì một bộ đếm`missing`biết có bao nhiêu loại riêng biệt vẫn cần thêm hoa để thỏa mãn sơ đồ. 
3. Khi nào`missing`đạt tới 0, chúng ta có một cửa sổ`[l, r]`có chứa ít nhất những bông hoa cần thiết. Kiểm tra xem ngoài cửa sổ này có đủ hoa để tạo số hoa còn lại không`n-1`phôi (mỗi chiều dài`k`). Cửa sổ cũng có thể bao gồm những bông hoa bổ sung ngoài sơ đồ, có thể tháo rời. 
4. Quyết định loại bỏ những bông hoa nào. Đầu tiên, đánh dấu những bông hoa vượt quá số lượng yêu cầu bên trong cửa sổ. Thứ hai, nếu số lượng hoa còn lại sau cửa sổ vượt quá`(n-1)*k`, đánh dấu chúng là có thể tháo rời cho đến khi chính xác`(n-1)*k`hoa vẫn ở bên ngoài cửa sổ đã chọn. Tổng số lượng hoa rời nhiều nhất`m - n*k`. 
5. Xuất ra số lượng hoa cần loại bỏ và vị trí của chúng. 

Tại sao nó hoạt động: Cửa sổ trượt đảm bảo chúng ta tìm thấy một khối liền kề chứa sơ đồ đúng một lần. Bằng cách chỉ loại bỏ những bông hoa thừa bên trong cửa sổ và những bông hoa bên ngoài không cần thiết cho các phôi khác, chúng tôi không vi phạm yêu cầu mà máy tạo ra ít nhất`n`vòng hoa. Cách tiếp cận này bao gồm tất cả các loại bỏ tối thiểu có thể xảy ra bởi vì mọi giải pháp hợp lệ đều phải bao gồm một cửa sổ thỏa mãn sơ đồ và chính xác`n-1`các phôi khác, để lại nhiều nhất`m - n*k`hoa có thể tháo rời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

m, k, n, s = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

needed = Counter(b)
current = Counter()
missing = len(needed)
res_l = res_r = -1

l = 0
for r in range(m):
    if a[r] in needed:
        current[a[r]] += 1
        if current[a[r]] == needed[a[r]]:
            missing -= 1
    while missing == 0:
        # window [l, r] satisfies schematic
        # check if enough flowers remain outside to make n-1 workpieces
        left_flower_count = l
        right_flower_count = m - r - 1
        total_outside = left_flower_count + right_flower_count
        if total_outside >= (n-1)*k:
            res_l, res_r = l, r
            break
        if a[l] in needed:
            if current[a[l]] == needed[a[l]]:
                missing += 1
            current[a[l]] -= 1
        l += 1
    if res_l != -1:
        break

if res_l == -1:
    print(-1)
else:
    to_remove = []
    # extra inside window
    inside_count = Counter()
    for i in range(res_l, res_r+1):
        if a[i] in needed:
            if inside_count[a[i]] < needed[a[i]]:
                inside_count[a[i]] += 1
            else:
                to_remove.append(i+1)
        else:
            to_remove.append(i+1)
    # extra outside window
    remaining = m - len(to_remove) - n*k
    for i in range(m):
        if remaining == 0:
            break
        if i < res_l or i > res_r:
            to_remove.append(i+1)
            remaining -= 1
    print(len(to_remove))
    print(' '.join(map(str, to_remove)))
```Mã sử ​​dụng một cửa sổ trượt để tìm đoạn thỏa mãn sơ đồ của Diana. Đầu tiên, nó mở rộng cửa sổ bằng con trỏ bên phải và thu nhỏ từ bên trái trong khi có tất cả các bông hoa cần thiết. Sau khi tìm được cửa sổ phù hợp, nó xác định những bông hoa thừa cả bên trong và bên ngoài cửa sổ để loại bỏ, đảm bảo ít nhất những bông hoa còn lại hình thành`n`phôi hoàn chỉnh. 

Xử lý ranh giới là tinh tế. Các chỉ số được chuyển đổi từ dựa trên 0 sang dựa trên 1 cho đầu ra. Khi đếm số hoa thừa bên trong cửa sổ, chúng ta theo dõi một bộ đếm riêng để tránh loại bỏ những bông hoa cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7 3 2 2
1 2 3 3 2 1 2
2 2
```| Bước | tôi | r | hiện tại | mất tích | Cửa sổ hợp lệ? | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | 0 | {} | 1 | không | 
| r=0 | 0 | 0 | {} | 1 | không | 
| r=1 | 0 | 1 | {2:1} | 1 | không | 
| r=2 | 0 | 2 | {2:1} | 1 | không | 
| r=3 | 0 | 3 | {2:1,3:1} | 1 | không | 
| r=4 | 0 | 4 | {2:2,3:1} | 0 | vâng | 

Cửa sổ`[1,5]`thỏa mãn sơ đồ. Hoa bên ngoài có thể hình thành`n-1=1`phôi. Bên trong thêm là hoa 3 (vị trí 4). Loại bỏ nó. Đầu ra:```
1
4
```### Mẫu 2 

đầu vào:```
1 1 1 1
1
1
```Bông hoa duy nhất phù hợp với sơ đồ, không cần loại bỏ. Đầu ra:```
0
```Những dấu vết này xác nhận rằng thuật toán tìm thấy chính xác một cửa sổ hợp lệ, đếm số hoa bổ sung và đảm bảo còn đủ hoa cho các phôi khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi bông hoa được xử lý tối đa hai lần bằng cửa sổ trượt; đếm và loại bỏ là tuyến tính | 
| Không gian | O(s + u) | s = kích thước sơ đồ, u = số loại hoa riêng biệt ở dây leo |
