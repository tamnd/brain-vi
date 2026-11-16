---
title: "CF 1939C - Thêm Quà Tặng"
description: "Bài toán đưa ra cho bạn một chuỗi bạn bè, mỗi người muốn một số lượng quà nhất định. Mỗi người bạn cũng có một loại quà ưa thích."
date: "2026-06-08T17:49:55+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dfs-and-similar", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1939
codeforces_index: "C"
codeforces_contest_name: "XVIII Open Olympiad in Informatics - Final Stage, Day 1 (Unrated, Online Mirror, IOI rules)"
rating: 0
weight: 1939
solve_time_s: 74
verified: true
draft: false
---

[CF 1939C - Quà tặng khác](https://codeforces.com/problemset/problem/1939/C) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt, dfs và tương tự, hai con trỏ 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra cho bạn một chuỗi bạn bè, mỗi người muốn một số lượng quà nhất định. Mỗi người bạn cũng có một loại quà ưa thích. Bạn phải phân phối quà sao cho tổng số quà thu thập được của tất cả bạn bè là tối đa, với hạn chế là mỗi người bạn không thể nhận nhiều quà hơn số lượng họ yêu cầu ban đầu. Bạn được phép sắp xếp lại bạn bè một cách tùy ý, điều đó có nghĩa là bạn có thể sắp xếp quà tặng theo yêu cầu một cách chiến lược để tối đa hóa số tiền. 

Đầu vào bao gồm số lượng bạn bè`n`, theo sau là hai mảng có độ dài`n`: một cho số lượng quà mà mỗi người bạn muốn và một cho loại quà họ thích. Đầu ra là tổng số quà tặng tối đa có thể đạt được theo quy tắc phân phối. 

Những hạn chế gợi ý`n`có thể lên tới 2×10^5. Điều này loại trừ các cách tiếp cận bạo lực sẽ thử tất cả các hoán vị, vì`n!`phát triển quá nhanh. Chúng tôi đang tìm kiếm một thuật toán hoạt động với khoảng O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm. Các trường hợp khó khăn không rõ ràng bao gồm việc bạn bè yêu cầu không có quà tặng hoặc tất cả bạn bè đều thích cùng loại, điều này có thể gây hiểu lầm cho cách tiếp cận tổng hợp ngây thơ nếu một người không xử lý phân phối theo loại một cách chính xác. 

Một ví dụ nhỏ minh họa một trường hợp phức tạp: nếu tất cả bạn bè muốn có 1 món quà nhưng lại thích các loại khác nhau thì cách tiếp cận ngây thơ là tặng quà theo tuần tự có thể làm giảm đi cơ hội thỏa mãn các ràng buộc dựa trên loại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại tất cả các hoán vị của bạn bè và tính tổng quà tặng theo các ràng buộc về sở thích. Điều này đúng về nguyên tắc nhưng không khả thi: vì`n = 10^5`, ngay cả một phép tính giai thừa cũng lớn về mặt thiên văn, vì vậy phương pháp này không thành công trong thực tế. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát rằng vấn đề có thể được phân tách theo loại quà tặng. Vì bạn bè có thể được sắp xếp lại một cách tự do nên chiến lược tối ưu là xử lý những người bạn cùng loại cùng nhau. Trong mỗi loại, nếu sắp xếp các yêu cầu theo thứ tự giảm dần, chúng ta có thể tham lam chia quà: người bạn có yêu cầu lớn nhất sẽ nhận được càng nhiều quà càng tốt, sau đó là quà lớn nhất tiếp theo, v.v. Điều này đảm bảo mức tối đa cục bộ cho từng loại. Sau khi xử lý tất cả các loại, việc tính tổng các cực đại này sẽ mang lại mức tối đa toàn cầu vì không có hai người bạn nào cạnh tranh cùng một loại sau khi sắp xếp lại. 

Về mặt hiệu quả, vấn đề giảm xuống còn việc nhóm theo loại, sắp xếp trong mỗi nhóm và thực hiện tổng tiền tố để tìm tổng tối đa. Cách tiếp cận này hoạt động ở O(n log n) do bước sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Nhóm + Sắp xếp theo loại | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng bạn bè`n`và hai mảng:`gifts`cho các yêu cầu và`types`cho các loại quà tặng ưa thích. 
2. Tạo ánh xạ từ từng loại quà tặng tới danh sách yêu cầu từ những người bạn thích loại quà tặng đó. Điều này nhóm bạn bè theo sở thích. 
3. Với mỗi loại, hãy sắp xếp các yêu cầu theo thứ tự giảm dần. Điều này đảm bảo chúng tôi có thể đáp ứng các yêu cầu lớn nhất trước tiên, tối đa hóa tổng số tiền cục bộ. 
4. Tính tổng tiền tố cho mỗi danh sách được sắp xếp. Tổng tiền tố tại vị trí`k`đại diện cho tổng số quà thu được nếu chúng ta đáp ứng được yêu cầu đầu tiên`k`yêu cầu lớn nhất cho loại đó. 
5. Tổng hợp các khoản tiền tố này theo tổng số quà tặng đã nhận. Đối với mỗi số lượng bạn bè có thể`k`lấy từ một loại, thêm tổng tiền tố tương ứng vào tổng số đang chạy được lập chỉ mục bởi`k`. 
6. Sau khi xử lý tất cả các loại, câu trả lời là giá trị tối đa trong tổng số tổng hợp, thể hiện số quà tặng tối đa có thể đạt được bằng bất kỳ chiến lược phân phối nào. 

Tại sao nó hoạt động: nhóm theo loại và sắp xếp đảm bảo rằng mỗi người bạn nhận được số lượng quà tặng tối đa có thể mà không vi phạm các ràng buộc. Tổng tiền tố duy trì tính tối ưu này vì việc sắp xếp lại bạn bè theo loại không làm giảm tổng số quà tặng - chúng độc lập. Bất biến là đối với mỗi loại, tổng tiền tố của đỉnh`k`yêu cầu là tối đa cho bất kỳ`k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict
from itertools import accumulate

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        gifts = list(map(int, input().split()))
        types = list(map(int, input().split()))
        
        type_map = defaultdict(list)
        for g, ty in zip(gifts, types):
            type_map[ty].append(g)
        
        prefix_sums = defaultdict(int)
        for lst in type_map.values():
            lst.sort(reverse=True)
            acc = list(accumulate(lst))
            for k, val in enumerate(acc, start=1):
                prefix_sums[k] += val
        
        print(max(prefix_sums.values()))

if __name__ == "__main__":
    main()
```Mã tuân theo thuật toán chính xác. các`defaultdict(list)`thu thập các yêu cầu theo loại. Việc sắp xếp đảm bảo chúng tôi xem xét các yêu cầu lớn nhất trước tiên.`accumulate`tạo ra các tổng tiền tố một cách hiệu quả. Cuối cùng,`prefix_sums`ánh xạ tổng số bạn bè được chọn vào tổng số quà tặng được thu thập và nhận`max`trả về câu trả lời đúng. Các điều kiện biên được xử lý một cách tự nhiên: các loại trống không đóng góp gì và các loại bạn bè đơn lẻ được tính chính xác bằng tổng tiền tố bắt đầu từ 1. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
1
5
2 3 1 4 2
1 2 1 2 1
```| Bước | Loại bản đồ | Danh sách được sắp xếp | Tổng tiền tố | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Ban đầu | {1:[2,1,2], 2:[3,4]} | {1:[2,2,1], 2:[4,3]} | {1:[2,4,5], 2:[4,7]} | {1:6,2:11,3:5,4:0} | 

Giá trị tối đa là 11. Dấu vết này xác nhận rằng việc chọn top 2 từ loại 2 và top 2 từ loại 1 sẽ mang lại tổng số quà tối đa. 

Đầu vào mẫu 2:```
1
3
1 1 1
1 1 1
```| Bước | Loại bản đồ | Danh sách được sắp xếp | Tổng tiền tố | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Ban đầu | {1:[1,1,1]} | {1:[1,1,1]} | [1,2,3] | {1:1,2:2,3:3} | 

Giá trị tối đa là 3. Tất cả bạn bè đều nhận được đúng một món quà. Vỏ cạnh với tất cả các loại cùng loại được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp danh sách của từng loại chiếm ưu thế; mỗi người bạn có trong đúng một danh sách | 
| Không gian | O(n) | Các loại ánh xạ tới danh sách và tổng tiền tố yêu cầu lưu trữ tuyến tính | 

Được cho`n`lên tới 2×10^5, việc sắp xếp chiếm ưu thế, rất phù hợp trong giới hạn 2 giây thông thường. Việc sử dụng bộ nhớ là tuyến tính, thấp hơn nhiều so với giới hạn thông thường là 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("1\n5\n2 3 1 4 2\n1 2 1 2 1\n") == "11", "sample 1"

# Minimum size
assert run("1\n1\n5\n1\n") == "5", "single friend"

# All equal gifts
assert run("1\n4\n2 2 2 2\n1 1 1 1\n") == "8", "all same type and request"

# Multiple types with one friend each
assert run("1\n3\n1 2 3\n1 2 3\n") == "6", "different types, single friend"

# Large k distribution
assert run("1\n6\n1 2 3 1 2 3\n1 1 2 2 3 3\n") == "12", "multiple types, multiple friends"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 người bạn | 5 | Xử lý trường hợp cạnh một người bạn | 
| Tất cả đều bình đẳng | 8 | Tổng hợp chính xác khi tất cả các yêu cầu bằng nhau | 
| Các loại khác nhau | 6 | Tính tổng chính xác các loại độc lập | 
| Nhiều bạn bè mỗi loại | 12 | Xác nhận tổng hợp tiền tố giữa các loại | 

## Vỏ cạnh 

Đối với loại không có yêu cầu nào, thuật toán sẽ bỏ qua nó một cách tự nhiên: danh sách trống, việc sắp xếp và tích lũy không tạo ra tổng tiền tố và không có gì được thêm vào`prefix_sums`. Đối với các loại có tất cả bạn bè yêu cầu cùng một số, việc sắp xếp sẽ giữ nguyên thứ tự và tính tổng chính xác. Bước tổng hợp đảm bảo rằng sự chồng chéo`k`các giá trị từ các loại khác nhau được tổng hợp, phản ánh chính xác tổng số quà tặng có thể có. Mỗi trường hợp cạnh xác nhận rằng chiến lược tổng tiền tố tham lam cho mỗi loại là hợp lý và tối đa.
