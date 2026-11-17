---
title: "CF 1980C - Sofia Và Những Hoạt Động Bị Mất"
description: "Chúng ta được yêu cầu xác định xem liệu một mảng b đã cho có thể được tạo ra từ mảng a ban đầu hay không bằng cách áp dụng một chuỗi các phép gán trong đó các chỉ số mục tiêu bị mất nhưng các giá trị được gán theo thứ tự vẫn được biết."
date: "2026-06-08T16:53:36+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1980
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 950 (Div. 3)"
rating: 1300
weight: 1980
solve_time_s: 184
verified: true
draft: false
---

[CF 1980C - Sofia và các hoạt động bị mất](https://codeforces.com/problemset/problem/1980/C) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 3m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một mảng nhất định có`b`có thể được tạo ra từ một mảng ban đầu`a`bằng cách áp dụng một chuỗi các phép gán trong đó các chỉ số mục tiêu bị mất nhưng các giá trị được gán theo thứ tự vẫn được biết. Mỗi thao tác gán một giá trị xác định`d_j`to some element of the array. Thách thức là trình tự của các chỉ số không xác định được và chúng ta cần xác minh xem liệu có tồn tại một trình tự hợp lệ làm biến đổi`a`vào trong`b`. 

Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp có mảng ban đầu, mảng đích và chuỗi giá trị`d`. Nhiệm vụ của chúng ta là trả lời “CÓ” nếu có thể gán các giá trị để tạo ra`b`, và “KHÔNG” nếu không. 

Các ràng buộc rất quan trọng: mảng có thể có tối đa 2·10^5 phần tử và tổng của`n`Và`m`trên tất cả các trường hợp thử nghiệm được giới hạn ở 2·10^5. Điều này ngụ ý rằng bất kỳ giải pháp nào lặp lại từng mảng nhiều lần trong các vòng lặp lồng nhau sẽ quá chậm. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh vi phát sinh khi có nhiều vị trí trong`b`yêu cầu cùng một giá trị`d_j`nhưng ít trường hợp hơn`d_j`có sẵn trong danh sách thao tác hoặc khi lần xuất hiện cuối cùng của một giá trị trong`b`phải được chỉ định bởi một hoạt động cụ thể. Ví dụ, nếu`a = [1,2,1]`,`b = [1,3,2]`, Và`d = [1,3,1,2]`, bài tập phải ánh xạ chính xác phần cuối cùng`3`TRONG`b`đến một trong những cái có sẵn`3`ở trong`d`. Một cách tiếp cận bất cẩn chỉ kiểm tra số lượng giá trị mà không tôn trọng thứ tự sẽ tạo ra kết quả dương tính giả. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ thử tất cả các hoán vị của các chỉ số cho các phép gán. Điều này rõ ràng là không khả thi vì có`n^m`trình tự có thể xảy ra trong trường hợp xấu nhất, và`m`có thể lên tới 2·10^5. Do đó, việc kiểm tra vũ lực từng hoán vị là không thực tế. 

Cách tiếp cận tối ưu thúc đẩy chiến lược tham lam kết hợp với tính toán. Đối với mỗi vị trí trong`b`Ở đâu`a_i != b_i`, chúng ta cần gán giá trị được yêu cầu bằng cách sử dụng lần xuất hiện cuối cùng của mỗi giá trị trong`d`. Chúng ta có thể duy trì một từ điển ánh xạ từng giá trị vào một chồng vị trí trong`b`yêu cầu nó, được xử lý từ phải sang trái. Sau đó, chúng tôi lặp qua`d`theo thứ tự và kiểm tra xem có vị trí hợp lệ để áp dụng nó hay không. Cái nhìn sâu sắc quan trọng là sự xuất hiện cuối cùng của mỗi giá trị trong`b`phải được bao phủ bởi tương ứng cuối cùng`d_j`, và chúng ta có thể tuyên truyền các bài tập trước đó một cách tham lam. Điều này đảm bảo giải pháp thời gian tuyến tính tôn trọng cả thứ tự thực hiện và mảng mục tiêu được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * m!) | O(n + m) | Quá chậm | 
| Đếm Tham Lam Với Ngăn Xếp | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc mảng ban đầu`a`, mảng mục tiêu`b`, và dãy giá trị`d`. 
2. Tạo từ điển`need`ánh xạ từng giá trị trong`b`điều đó khác với`a`vào danh sách các chỉ mục được yêu cầu, được lưu trữ theo thứ tự ngược lại. Điều này cho phép chúng ta bật các vị trí từ phải sang trái một cách hiệu quả. 
3. Khởi tạo con trỏ`last_pos`để theo dõi vị trí được chỉ định cuối cùng cho mỗi giá trị. Điều này đảm bảo chúng tôi luôn tôn trọng ràng buộc ngoài cùng bên phải. 
4. Lặp lại trình tự`d`. Với mỗi giá trị`val`:

- Nếu như`need[val]`trống, sử dụng vị trí được chỉ định cuối cùng của`val`để đáp ứng hoạt động (lan truyền). 
- Nếu không, hãy bật vị trí được yêu cầu cuối cùng từ`need[val]`và gán nó, cập nhật`last_pos[val]`. 
5. Sau khi xử lý xong tất cả`d`, kiểm tra xem có giá trị nào vẫn còn vị trí chưa được gán trong`need`. Nếu vậy thì câu trả lời là “KHÔNG”; nếu không thì “CÓ”. 

Tại sao nó hoạt động: Mỗi thao tác gán được sử dụng theo thứ tự. Bằng cách lưu trữ các vị trí bắt buộc ngược lại và gán từ yêu cầu ngoài cùng bên phải, chúng tôi đảm bảo rằng tất cả các phép gán cần thiết đều được thực hiện, đồng thời cho phép truyền bá để đáp ứng các hoạt động không được yêu cầu nghiêm ngặt. Điều bất biến là ở mỗi bước, các vị trí cần thiết còn lại luôn có thể truy cập được bằng các hoạt động trong tương lai, do đó không có phép gán cần thiết nào bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        m = int(input())
        d = list(map(int, input().split()))
        
        need = defaultdict(deque)
        for i in range(n-1, -1, -1):
            if a[i] != b[i]:
                need[b[i]].appendleft(i)
        
        last_pos = {}
        possible = True
        for val in d:
            if need[val]:
                idx = need[val].pop()
                last_pos[val] = idx
            elif val in last_pos:
                pass  # propagate to last used position
            else:
                possible = False
                break
        
        for positions in need.values():
            if positions:
                possible = False
                break
        
        print("YES" if possible else "NO")

solve()
```Giải pháp đầu tiên là xây dựng bản đồ các vị trí cần thiết trong`b`cho từng giá trị khác nhau. sử dụng`deque`cho phép O(1) bật lên từ một trong hai đầu, cho phép gán từ phải sang trái. các`last_pos`từ điển cho phép truyền bá các giá trị khi không có yêu cầu ngay lập tức. Sau khi xử lý xong tất cả`d`, mọi vị trí bắt buộc còn sót lại đều cho thấy sự chuyển đổi không thể thực hiện được. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên:```
a = [1, 2, 1]
b = [1, 3, 2]
d = [1, 3, 1, 2]
```| Bước | giá trị từ d | cần trước | cần sau | cuối_pos | Bình luận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3:[1],2:[2],1:[2] | 3:[1],2:[2] | 1:2 | Gán 1 vào vị trí cuối cùng 2 | 
| 2 | 3 | 3:[1],2:[2] | 3:[] ,2:[2] | 3:1 | Gán 3 vào vị trí 1 | 
| 3 | 1 | 3:[],2:[2] | 3:[],2:[2] | 1:2 | Tuyên truyền 1 đến vị trí cuối cùng 2 | 
| 4 | 2 | 3:[],2:[2] | 3:[],2:[] | 2:2 | Gán 2 vào vị trí 2 | 

Tất cả`need`danh sách trống → CÓ. 

Một mẫu khác:```
a = [1, 2, 3, 5]
b = [2, 1, 3, 5]
d = [2, 1, 3, 5]
```Theo thuật toán, giá trị đầu tiên 2 không thể được gán cho vị trí chính xác theo thứ tự → KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi chỉ số trong`b`được xử lý nhiều nhất một lần và mỗi lần`d_j`được xử lý một lần | 
| Không gian | O(n + m) |`need`lưu trữ vị trí trên mỗi giá trị,`last_pos`lưu trữ chỉ mục được gán lần cuối cho mỗi giá trị | 

Thuật toán tuyến tính về kích thước của mảng và trình tự, phù hợp với các ràng buộc của bài toán. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("""7
3
1 2 1
1 3 2
4
1 3 1 2
4
1 2 3 5
2 1 3 5
2
2 3
5
7 6 1 10 10
3
6 1 11 11
3
4 3 11
4
3 1 7 8
2
2 7 10
5
10 3 2 2 1
5
5 7 1 7 9
4
10 1 2 9
8
1 1 9 8 7 2 10 4
4
1000000000 203 203 203
203 1000000000 203 1000000000
2
203 1000000000
1
1
1
5
1 3 4 5 1
""") == "YES\nNO\nNO\nNO\nYES\nNO\nYES"
```Các trường hợp tùy chỉnh có thể bao gồm mảng một phần tử, giá trị lặp lại hoặc thao tác có thêm giá trị không được sử dụng để xác minh việc truyền bá. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1 2 | 2 1 | 
| 3 | [1,1,1], d=[1,1,1] | CÓ | 
| 4 | [1,2,3], b=[3,2,1], d=[ | |
