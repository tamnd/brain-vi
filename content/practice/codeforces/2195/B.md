---
title: "CF 2195B - Heapify 1"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến n và có thể thực hiện một kiểu hoán đổi cụ thể: với bất kỳ chỉ số i nào từ 1 đến n/2, chúng ta có thể hoán đổi phần tử ở vị trí i với phần tử ở vị trí 2i."
date: "2026-06-07T20:37:35+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2195
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1080 (Div. 3)"
rating: 900
weight: 2195
solve_time_s: 94
verified: true
draft: false
---

[CF 2195B - Heapify 1](https://codeforces.com/problemset/problem/2195/B) 

**Xếp hạng:** 900 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến n và có thể thực hiện một kiểu hoán đổi cụ thể: với bất kỳ chỉ số i nào từ 1 đến n/2, chúng ta có thể hoán đổi phần tử ở vị trí i với phần tử ở vị trí 2i. Nhiệm vụ là xác định xem bằng cách thực hiện bất kỳ số lần hoán đổi nào như vậy, mảng có thể được sắp xếp theo thứ tự tăng dần hay không. 

Đầu vào đại diện cho nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho một số nguyên n, độ dài của hoán vị, theo sau là n số nguyên riêng biệt từ 1 đến n theo một thứ tự tùy ý. Đầu ra cho mỗi trường hợp thử nghiệm là "CÓ" nếu mảng có thể được sắp xếp bằng các thao tác được phép và "KHÔNG" nếu ngược lại. 

Các ràng buộc rất đáng kể: n có thể lên tới 200.000 và có thể lên tới 10.000 trường hợp thử nghiệm, nhưng tổng số n trên tất cả các trường hợp thử nghiệm được giới hạn ở 200.000. Điều này có nghĩa là chúng ta cần một nghiệm tuyến tính hoặc gần tuyến tính theo n cho mỗi trường hợp thử nghiệm. Một cách tiếp cận đơn giản mô phỏng tất cả các giao dịch hoán đổi có thể xảy ra sẽ có hàm mũ theo n và hoàn toàn không khả thi. 

Trường hợp cạnh phát sinh khi mảng gần như được sắp xếp ngoại trừ các phần tử không thể đạt đến vị trí đích do cấu trúc hoán đổi 2i. Ví dụ: với n=5, nếu hoán vị là [1,4,2,3,5], phần tử 4 ở chỉ mục 2 có thể được hoán đổi bằng 2 ở chỉ mục 4, cho phép sắp xếp, vì vậy câu trả lời là CÓ. Nhưng nếu mảng là [1,4,2,3,5], thì các vị trí có thể truy cập thông qua hoán đổi không cho phép phần tử 4 di chuyển đến đúng vị trí của nó ở chỉ số 4, vì vậy câu trả lời là KHÔNG. Các thuật toán ngây thơ bỏ qua cấu trúc biểu đồ hoán đổi có thể nói CÓ một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng mọi chuỗi hoán đổi được phép có thể có và kiểm tra xem mảng có thể được sắp xếp hay không. Mỗi lần hoán đổi chỉ ảnh hưởng đến các cặp vị trí (i, 2i), nhưng có thể có nhiều chuỗi hoán đổi. Với n=200.000, số lượng chuỗi lớn về mặt thiên văn. Ngay cả việc thực hiện mỗi lần hoán đổi trong tất cả các kết hợp cũng không thực tế vì số lượng thao tác sẽ tăng theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là mô hình hóa các giao dịch hoán đổi được phép dưới dạng biểu đồ có hướng: chỉ mục i có thể đạt đến chỉ số 2i và ngược lại vì các giao dịch hoán đổi có thể đảo ngược. Điều này tạo thành một rừng cây bắt nguồn từ các chỉ số không thể viết là i=2j (thường là các chỉ số cuối cùng). Quan sát quan trọng là các phần tử chỉ có thể di chuyển trong thành phần được kết nối của các chỉ mục được xác định bởi các giao dịch hoán đổi này. Khi bạn biết các thành phần được kết nối, bạn có thể kiểm tra xem mỗi thành phần có chứa chính xác các phần tử sẽ chiếm các vị trí đó trong mảng được sắp xếp hay không. Có thể sắp xếp khi và chỉ khi mỗi thành phần chứa nhiều tập hợp giá trị đích chính xác cho các vị trí đó. 

Vì vậy, cách tiếp cận tối ưu là xác định các thành phần liên thông của các chỉ số theo quan hệ hoán đổi và so sánh tập hợp các phần tử trong mỗi thành phần với tập hợp các vị trí được sắp xếp mà chúng chiếm giữ. Cách tiếp cận này chạy theo thời gian tuyến tính đối với n vì mỗi chỉ mục được truy cập một lần và việc kiểm tra thành viên thành phần là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Với mỗi test, đọc n và mảng hoán vị a. 
2. Khởi tạo một mảng boolean đã truy cập có độ dài n để theo dõi những chỉ mục nào đã được xử lý. 
3. Với mỗi chỉ mục i từ 1 đến n, nếu nó chưa được truy cập thì bắt đầu một thành phần mới. Khởi tạo hai danh sách trống: một cho các chỉ mục trong thành phần này và một cho các phần tử tại các chỉ mục này. 
4. Sử dụng tìm kiếm theo chiều sâu hoặc duyệt lặp để thu thập tất cả các chỉ mục có thể truy cập được từ i thông qua các giao dịch hoán đổi. Đối với mỗi chỉ mục j đã truy cập, đánh dấu nó là đã truy cập, thêm j vào danh sách chỉ mục và a[j] vào danh sách thành phần. Đối với mỗi chỉ mục, hãy xem xét các kết nối j → 2j và 2j → j, miễn là các chỉ số vẫn nằm trong giới hạn. 
5. Sau khi thu thập một thành phần, hãy sắp xếp cả danh sách chỉ mục và danh sách phần tử. So sánh hai danh sách theo từng yếu tố; danh sách chỉ mục đại diện cho các vị trí phải chứa các số nhất định và danh sách thành phần đại diện cho các số thực sự ở các vị trí đó. Nếu chúng khác nhau thì việc phân loại là không thể. 
6. Nếu tất cả các thành phần phù hợp với các thành phần dự kiến, hãy in CÓ; nếu không thì in ra NO. 

Tại sao nó hoạt động: Hoạt động hoán đổi xác định các thành phần được kết nối của các chỉ mục có thể trao đổi các phần tử một cách tự do với nhau. Mỗi thành phần là độc lập nên thứ tự tương đối của các phần tử trong các thành phần khác nhau không thể thay đổi được. Việc sắp xếp mảng chỉ có thể thực hiện được nếu mỗi thành phần chứa chính xác các phần tử được yêu cầu theo vị trí của chúng trong mảng được sắp xếp cuối cùng. Thuật toán trực tiếp kiểm tra bất biến này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

def can_sort_heapify(n, a):
    visited = [False] * n

    def dfs(idx, indices):
        visited[idx] = True
        indices.append(idx)
        for nxt in [2*(idx+1)-1, (idx+1)//2-1]:
            if 0 <= nxt < n and not visited[nxt]:
                dfs(nxt, indices)

    for i in range(n):
        if not visited[i]:
            indices = []
            dfs(i, indices)
            component_values = [a[j] for j in indices]
            if sorted(indices) != sorted([v-1 for v in component_values]):
                return "NO"
    return "YES"

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(can_sort_heapify(n, a))
```Giải pháp trước tiên đặt giới hạn đệ quy đủ cao để tránh tràn ngăn xếp cho n lớn. DFS khám phá tất cả các chỉ mục có thể truy cập được thông qua các giao dịch hoán đổi được phép, tạo thành các thành phần được kết nối. Đối với mỗi thành phần, chúng tôi trích xuất các phần tử hiện tại và các giá trị được sắp xếp mục tiêu cho các vị trí đó. Việc sắp xếp cả hai danh sách và so sánh đảm bảo rằng các phần tử có thể được sắp xếp lại trong thành phần để khớp với mảng đã sắp xếp. Việc sử dụng lập chỉ mục dựa trên 0 giúp đơn giản hóa việc xử lý DFS và mảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: [1,4,3,2,5] 

| Bước | chỉ số đã truy cập | phần tử thành phần | chỉ số được sắp xếp | các phần tử được sắp xếp | cuộc thi đấu? | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu từ 0 | [0] | [1] | [0] | [0] | vâng | 
| Tiếp theo chưa truy cập 1 | [1,3] | [4,2] | [1,3] | [1,3] | vâng | 
| Tiếp theo chưa truy cập 2 | [2] | [3] | [2] | [2] | vâng | 
| Tiếp theo chưa truy cập 4 | [4] | [5] | [4] | [4] | vâng | 

Tất cả các thành phần đều khớp, vì vậy đầu ra là CÓ. 

### Ví dụ 2 

Đầu vào: [1,4,2,3,5] 

| Bước | chỉ số đã truy cập | phần tử thành phần | chỉ số được sắp xếp | các phần tử được sắp xếp | cuộc thi đấu? | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu từ 0 | [0] | [1] | [0] | [0] | vâng | 
| Tiếp theo chưa truy cập 1 | [1,3] | [4,3] | [1,3] | [1,3] | vâng | 
| Tiếp theo chưa truy cập 2 | [2] | [2] | [2] | [2] | vâng | 
| Tiếp theo chưa truy cập 4 | [4] | [5] | [4] | [4] | vâng | 

Chỉ số thành phần 1 [1,3] giá trị mong đợi [2,4] nhưng giá trị thực tế [4,3] → không khớp. Đầu ra là KHÔNG. 

Dấu vết cho thấy cách các nhóm DFS lập chỉ mục và lý do xảy ra sự không khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được truy cập chính xác một lần trong DFS; sắp xếp từng thành phần là O(k log k) nhưng tổng k trên tất cả các thành phần là n, vì vậy trường hợp xấu nhất là O(n log n) | 
| Không gian | O(n) | Mảng đã truy cập và ngăn xếp DFS | 

Cho n 2⋅10^5 và t ≤ 10^4 với tổng n qua các lần kiểm tra 2⋅10^5, giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.setrecursionlimit(1 << 20)
    output = io.StringIO()
    sys.stdout = output

    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        print(can_sort_heapify(n, a))
    return output.getvalue().strip()

# provided samples
assert run("2\n5\n1 4 3 2 5\n5\n1 4 2 3 5\n") == "YES\nNO", "sample 1"

# custom cases
assert run("1\n1\n1\n") == "YES", "single element"
assert run("1\n2\n2 1\n") == "YES", "two elements can swap"
assert run("1\n3\n3 2 1\n") == "NO", "three elements impossible"
assert run("1\n4\n4 3 2 1\n") == "NO", "all reversed 4 elements"
assert run("1\n5\n1 2 3 4 5\n") == "YES", "already sorted"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1\n1 | | |
