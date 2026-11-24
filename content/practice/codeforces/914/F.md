---
title: "CF 914F - Chuỗi con trong chuỗi"
description: "Chúng tôi được cung cấp một chuỗi có thể thay đổi và một chuỗi truy vấn. Mỗi truy vấn sẽ thay đổi một ký tự ở một vị trí cụ thể hoặc hỏi xem một chuỗi nhỏ hơn xuất hiện dưới dạng chuỗi con bao nhiêu lần trong một chuỗi con cụ thể của chuỗi chính."
date: "2026-06-12T10:06:58+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "data-structures", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 914
codeforces_index: "F"
codeforces_contest_name: "Codecraft-18 and Codeforces Round 458 (Div. 1 + Div. 2, combined)"
rating: 3000
weight: 914
solve_time_s: 174
verified: true
draft: false
---

[CF 914F - Chuỗi con trong chuỗi](https://codeforces.com/problemset/problem/914/F) 

**Đánh giá:** 3000 
**Tags:** bitmasks, brute Force, cấu trúc dữ liệu, cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 2m 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi có thể thay đổi và một chuỗi truy vấn. Mỗi truy vấn sẽ thay đổi một ký tự ở một vị trí cụ thể hoặc hỏi xem một chuỗi nhỏ hơn xuất hiện dưới dạng chuỗi con bao nhiêu lần trong một chuỗi con cụ thể của chuỗi chính. Chuỗi và tất cả các truy vấn được lập chỉ mục 1 và các lần xuất hiện của chuỗi con có thể trùng nhau. 

Kích thước đầu vào lớn: chuỗi chính có thể lên tới 100.000 ký tự và có thể có tới 100.000 truy vấn. Mỗi truy vấn chuỗi con có tổng độ dài chuỗi truy vấn được tính tổng trên tất cả các truy vấn tối đa là 100.000. Một giải pháp đơn giản quét chuỗi con cho mọi truy vấn sẽ kiểm tra đại khái |substring| × |y| ký tự cho mỗi truy vấn. Trong trường hợp xấu nhất, đó là 10^5 × 10^5 = 10^10 thao tác, quá lớn so với giới hạn 6 giây. 

Các trường hợp cạnh rất tinh tế. Ví dụ, các lần xuất hiện chồng chéo phải được tính. Nếu như`s = "aaaaa"`và chúng tôi truy vấn`y = "aa"`trên toàn bộ chuỗi, số đếm đúng là 4 vì "aa" xuất hiện bắt đầu ở vị trí 1, 2, 3 và 4. Việc triển khai bất cẩn khi sử dụng`str.count`hoặc`find`nếu không xử lý cẩn thận sự chồng chéo sẽ trả về 2, điều này là sai. Một trường hợp đặc biệt khác là truy vấn chuỗi con bao gồm các vị trí mà các ký tự được cập nhật gần đây. Chúng tôi phải đảm bảo các bản cập nhật được truyền tải chính xác để số lượng chuỗi con tiếp theo phản ánh trạng thái chuỗi hiện tại. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mọi truy vấn loại 2, hãy trích xuất chuỗi con, sau đó quét từ trái sang phải, kiểm tra tại từng vị trí xem chuỗi truy vấn có khớp hay không. Đối với một chuỗi con có độ dài`m`và chuỗi truy vấn có độ dài`k`, chi phí này là O(m × k). Với 10^5 truy vấn, việc này trở nên quá chậm. 

Thông tin chi tiết quan trọng xuất phát từ quan sát rằng tổng độ dài của tất cả các chuỗi truy vấn được giới hạn bởi 10^5. Điều đó có nghĩa là nếu chúng tôi tính toán trước thông tin cho từng chuỗi truy vấn riêng biệt, chúng tôi có thể phân bổ công việc trên nhiều truy vấn chuỗi con. Cụ thể, chúng ta có thể theo dõi các vị trí mà mỗi chuỗi truy vấn xuất hiện trong chuỗi chính. Khi cập nhật loại 1 xảy ra, chỉ những vị trí mà ký tự đã thay đổi có thể ảnh hưởng đến kết quả khớp mới cần được điều chỉnh. Điều này đương nhiên dẫn đến một giải pháp sử dụng ánh xạ từ chuỗi truy vấn đến danh sách được sắp xếp của các vị trí bắt đầu, cho phép tìm kiếm nhị phân đếm một cách hiệu quả số lượng kết quả phù hợp nằm trong một phạm vi nhất định. 

Chúng tôi sử dụng các quan sát sau: so khớp chuỗi`y`bắt đầu ở vị trí`i`chỉ bị vô hiệu nếu bản cập nhật sửa đổi một trong các`len(y)`ký tự bắt đầu tại`i`. Ngược lại, một kết quả trùng khớp mới có thể xuất hiện bắt đầu từ các vị trí có ký tự được cập nhật. Việc sử dụng tìm kiếm nhị phân trên danh sách lần xuất hiện được sắp xếp sẽ giúp bạn nhanh chóng đếm được có bao nhiêu kết quả phù hợp nằm trong phạm vi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q × n × k) | O(n) | Quá chậm | 
| Tối ưu | O(q × log(n) × | y | _max) khấu hao | 

## Hướng dẫn thuật toán 

1. Phân tích chuỗi và truy vấn. Duy trì chuỗi dưới dạng danh sách các ký tự để cho phép cập nhật O(1). 
2. Xác định tất cả các chuỗi truy vấn duy nhất`y`trên các truy vấn loại 2. Đối với mỗi chuỗi`y`, tính toán trước tất cả các vị trí bắt đầu trong chuỗi ban đầu trong đó`y`xảy ra và lưu trữ chúng trong một danh sách được sắp xếp. 
3. Đối với mỗi truy vấn: 

- Nếu là cập nhật loại 1 tại vị trí`i`thay đổi ký tự thành`c`, cập nhật chuỗi. Sau đó với mỗi chuỗi truy vấn`y`, xét các vị trí từ`i - len(y) + 1`ĐẾN`i`khi khả năng bị ảnh hưởng bắt đầu. Đối với mỗi vị trí trong giới hạn này, hãy kiểm tra xem`y`bây giờ phù hợp. Nếu kết quả trùng khớp tồn tại trước khi thay đổi và không còn khớp nữa, hãy xóa nó khỏi danh sách. Nếu kết quả khớp mới xuất hiện, hãy chèn nó vào danh sách, duy trì thứ tự được sắp xếp. 
- Nếu là truy vấn loại 2, sử dụng tìm kiếm nhị phân trên danh sách đã sắp xếp các vị trí bắt đầu cho chuỗi truy vấn`y`để đếm nhanh có bao nhiêu vị trí bắt đầu nằm trong chuỗi con đã cho`[l, r - len(y) + 1]`. Điều này mang lại số lần xuất hiện một cách hiệu quả. 
4. Xuất số lượng cho tất cả truy vấn loại 2. 

Tại sao nó hoạt động: điều bất biến là danh sách được sắp xếp luôn phản ánh chính xác tất cả các vị trí bắt đầu trong chuỗi hiện tại cho mỗi chuỗi truy vấn. Tìm kiếm nhị phân đảm bảo tính chính xác trong một phạm vi nhất định và các bản cập nhật cục bộ đảm bảo rằng chỉ các vị trí bị ảnh hưởng mới được kiểm tra, giúp cập nhật luôn hiệu quả. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

s = list(input().strip())
n = len(s)
q = int(input())
queries = []
query_strings = set()

for _ in range(q):
    parts = input().split()
    if parts[0] == '1':
        queries.append((1, int(parts[1])-1, parts[2]))
    else:
        l, r, y = int(parts[1])-1, int(parts[2])-1, parts[3]
        queries.append((2, l, r, y))
        query_strings.add(y)

occurrences = {}
for y in query_strings:
    occ = []
    m = len(y)
    for i in range(n - m + 1):
        if ''.join(s[i:i+m]) == y:
            occ.append(i)
    occurrences[y] = occ

for query in queries:
    if query[0] == 1:
        idx, c = query[1], query[2]
        s[idx] = c
        for y in query_strings:
            m = len(y)
            start = max(0, idx - m + 1)
            end = min(n - m, idx)
            occ_list = occurrences[y]
            for i in range(start, end + 1):
                slice_now = ''.join(s[i:i+m])
                pos = bisect.bisect_left(occ_list, i)
                exists = pos < len(occ_list) and occ_list[pos] == i
                if slice_now == y and not exists:
                    occ_list.insert(pos, i)
                elif slice_now != y and exists:
                    occ_list.pop(pos)
    else:
        l, r, y = query[1], query[2], query[3]
        occ_list = occurrences[y]
        m = len(y)
        left = bisect.bisect_left(occ_list, l)
        right = bisect.bisect_right(occ_list, r - m + 1)
        print(right - left)
```Giải pháp đọc chuỗi dưới dạng danh sách để cho phép cập nhật O(1). Việc tính toán trước các vị trí xuất hiện sẽ tránh việc quét lại các chuỗi truy vấn lặp lại. Khi xử lý các bản cập nhật, chỉ những vị trí có khả năng bị ảnh hưởng bởi thay đổi mới được kiểm tra, giúp giảm bớt công việc không cần thiết. Tìm kiếm nhị phân trên danh sách được sắp xếp đảm bảo việc đếm số lần xuất hiện trong một phạm vi được nhanh chóng. Cần chú ý cẩn thận khi lập chỉ mục và xử lý phạm vi bao gồm/loại trừ. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
ababababa
3
2 1 7 aba
1 5 c
2 1 7 aba
```| Bước | s | Sự xuất hiện của "aba" | Hành động truy vấn | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | ababababa | [0,2,4,6] | - | - | 
| 1 | ababababa | [0,2,4,6] | Đếm [1,7] | Vị trí 0,2,4 → đếm 3 | 
| 2 | ababc b aba | [0,2,4,6] cập nhật | Cập nhật idx=4 | Tính toán lại các vị trí 1→4 bị ảnh hưởng | 
| 3 | ababc b aba | [0,2,6] | Đếm [1,7] | Vị trí 0,2 → đếm 2? Trên thực tế các vị trí trong [0,6] → đếm 1 | 

Điều này thể hiện việc xử lý cập nhật và tính toán chính xác các lần xuất hiện chồng chéo. 

**Đầu vào tùy chỉnh 2**```
aaaa
3
2 1 4 aa
1 2 b
2 1 4 aa
```| Bước | s | Số lần xuất hiện của "aa" | Hành động truy vấn | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | aaa | [0,1,2] | Đếm [1,4] | 3 | 
| 1 | a ba aa | [0,1,2] cập nhật idx=1 | Vị trí 0→1 bị ảnh hưởng | Xuất hiện mới [2] | 
| 2 | a ba aa | [2] | Đếm [1,4] | 1 | 

Các trường hợp trùng lặp và cập nhật trùng lặp được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q × | y | 
| Không gian | O(tổng của | y | 

Cho trước các ràng buộc, tổng |y| trên tất cả các truy vấn ≤ 10^5 và n ≤ 10^5, do đó cả thời gian và bộ nhớ đều nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution
    s = list(input().strip())
    n = len(s)
    q = int(input())
    queries = []
    query_strings = set()
    for _ in range(q):
        parts = input().split
```
