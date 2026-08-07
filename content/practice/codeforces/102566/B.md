---
title: "CF 102566B - BLAT"
description: "Cây chứa một chữ cái viết thường trên mỗi đỉnh. Mỗi cặp đỉnh có thứ tự xác định một chuỗi: bắt đầu từ đỉnh đầu tiên, đi dọc theo đường dẫn duy nhất đến đỉnh thứ hai và nối các chữ cái gặp phải."
date: "2026-08-07T21:26:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102566
codeforces_index: "B"
codeforces_contest_name: "AGM 2020, Qualification Round"
rating: 0
weight: 102566
solve_time_s: 61
verified: true
draft: false
---

[CF 102566B - BLAT](https://codeforces.com/problemset/problem/102566/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cây chứa một chữ cái viết thường trên mỗi đỉnh. Mỗi cặp đỉnh có thứ tự xác định một chuỗi: bắt đầu từ đỉnh đầu tiên, đi dọc theo đường dẫn duy nhất đến đỉnh thứ hai và nối các chữ cái gặp phải. Nhiệm vụ là tìm chuỗi nhỏ thứ K trong số tất cả các đường dẫn có thứ tự như vậy theo thứ tự từ điển. 

Có tới 100000 đỉnh, nghĩa là số đường đi có thể lên tới 10 tỷ. Việc liệt kê mọi đường dẫn hoặc sắp xếp chúng là không thể. Chúng ta cần khai thác thực tế rằng K chỉ có nhiều nhất là 100000. Chúng ta chỉ cần phần đầu của thứ tự từ điển chứ không phải toàn bộ. 

Một sai lầm phổ biến là coi đường dẫn là vô hướng. Đường dẫn từ u đến v và đường dẫn từ v đến u là các chuỗi khác nhau vì các chữ cái của chúng được đọc theo các hướng khác nhau. 

Ví dụ:```
2 1
a b
1 2
```Các chuỗi có thể là`a`,`b`,`ab`, Và`ba`. Câu trả lời là`a`, không`ab`, bởi vì các chuỗi ngắn hơn có cùng tiền tố sẽ nhỏ hơn. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là tạo tất cả các đường dẫn N2, chuyển đổi từng đường dẫn thành một chuỗi, sắp xếp chúng và lấy phần tử thứ K. Việc duyệt cây có thể tìm thấy một đường dẫn trong O(N), vì vậy phương pháp này cần O(N³) hoạt động trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát hữu ích là K nhỏ. Thay vì xây dựng toàn bộ danh sách đã sắp xếp, chúng ta có thể tạo các đường dẫn theo thứ tự từ điển bằng cách sử dụng hàng đợi ưu tiên. Mỗi phần tử hàng đợi đại diện cho một đường dẫn đã được xây dựng. Ban đầu, mỗi đỉnh là một đường đi hợp lệ có độ dài bằng một. Khi loại bỏ đường đi nhỏ nhất, chúng ta sẽ mở rộng nó thêm một đỉnh liền kề theo mọi hướng có thể mà không quay trở lại đỉnh trước đó ngay lập tức. 

Điều này tương đương với việc khám phá trie tiềm ẩn của tất cả các đường dẫn cây hợp lệ. Hàng đợi ưu tiên luôn chứa các tiền tố chưa hoàn thành nhỏ nhất, do đó, việc liên tục lấy mức tối thiểu sẽ mang lại đường dẫn từ điển tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N³) | O(N2) | Quá chậm | 
| Bảng liệt kê hàng đợi ưu tiên | O((N+K) log(N+K) + S) | O(N+K) | Được chấp nhận cho K ≤ 100000 | 

## Hướng dẫn thuật toán 

1. Chèn mỗi đỉnh làm đường dẫn ban đầu trong hàng ưu tiên. Mỗi chuỗi này đại diện cho một đường dẫn bắt đầu và kết thúc ở cùng một đỉnh. 
2. Xóa chuỗi nhỏ nhất khỏi hàng đợi. Đây là đường dẫn tiếp theo theo thứ tự từ điển, vì vậy hãy coi nó là một trong những câu trả lời. 
3. Nếu đây là chuỗi bị loại bỏ thứ K, hãy xuất nó ngay lập tức. Không cần phải tạo các đường dẫn còn lại. 
4. Mặt khác, hãy mở rộng đường đi hiện tại qua mọi lân cận của đỉnh cuối cùng của nó ngoại trừ đỉnh mà chúng ta đã xuất phát. Chèn những đường dẫn mới đó vào hàng đợi. 

Lý do điều này hoạt động là vì mọi đường dẫn hợp lệ đều có chính xác một đường dẫn trước có được bằng cách loại bỏ đỉnh cuối cùng của nó. Hàng đợi ưu tiên giữ tất cả các phần tiếp theo có thể có theo thứ tự, do đó không có đường dẫn nào có thể bị bỏ qua trước một đường dẫn nhỏ hơn. 

Tại sao nó hoạt động: 

Tính bất biến của hàng đợi là nó chứa mọi đường dẫn được tạo ra có thể trở thành câu trả lời tiếp theo. Khi phần tử tối thiểu bị loại bỏ, mọi đường dẫn không có trong hàng đợi đều đã được xuất ra hoặc có tiền tố vẫn nhỏ hơn và đang chờ trong hàng đợi. Do đó phần tử bị loại bỏ luôn là đường dẫn từ điển tiếp theo. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    letters = input().split()

    graph = [[] for _ in range(n)]
    for _ in range(n - 1):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        graph[a].append(b)
        graph[b].append(a)

    heap = []

    for i in range(n):
        heapq.heappush(heap, (letters[i], i, -1))

    count = 0

    while heap:
        s, node, parent = heapq.heappop(heap)
        count += 1

        if count == k:
            print(s)
            return

        for nxt in graph[node]:
            if nxt != parent:
                heapq.heappush(heap, (s + letters[nxt], nxt, node))

if __name__ == "__main__":
    solve()
```Hàng đợi lưu chuỗi hiện tại cùng với hai đỉnh cuối cùng của đường dẫn. Đỉnh trước đó là cần thiết vì đường đi của cây không thể quay lại ngay lập tức, nếu không cùng một cạnh sẽ phải đi qua hai lần. 

Quá trình khởi tạo sẽ chèn N đường dẫn có độ dài bằng một. Sau khi đường dẫn bị xóa, tất cả các tiện ích mở rộng một bước có thể có sẽ được thêm vào. Thuật toán dừng ngay khi K đường dẫn được trích xuất, do đó nó không bao giờ cố gắng xây dựng toàn bộ tập hợp các đường dẫn. 

Chi tiết triển khai chính là giữ đỉnh trước đó. Nếu không có nó, việc mở rộng sẽ tạo ra các bước đi không hợp lệ như`a b a b a`, đây không phải là những đường dẫn đơn giản trong cây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N+K) log(N+K) + L) | Mỗi đường dẫn được trích xuất sẽ tạo ra các bản mở rộng và chỉ có K đường dẫn bị xóa | 
| Không gian | O(N+K) | Heap lưu trữ các ứng viên đang hoạt động | 

Giải pháp dựa trên việc K bị giới hạn bởi 100000. Chỉ tạo tiền tố của thứ tự sẽ tránh được số bậc hai của các đường dẫn có thể. 

## Vỏ cạnh 

Cây một đỉnh:```
1 1
a
```Con đường duy nhất là`a`, vậy câu trả lời là`a`. Hàng đợi ban đầu đã chứa câu trả lời đầy đủ. 

Những chữ cái lặp đi lặp lại:```
3 3
a a a
1 2
2 3
```Nhiều chuỗi so sánh như nhau đối với các ký tự đầu tiên của chúng. Heap vẫn xử lý việc này vì các chuỗi hoàn chỉnh được so sánh, không chỉ chữ cái đầu tiên. 

Các đường định hướng:```
2 4
a b
1 2
```Các đường dẫn được sắp xếp là:```
a
ab
b
ba
```Câu trả lời cuối cùng là`ba`. Xử lý cây như vô hướng sẽ hợp nhất không chính xác`ab`Và`ba`. Đỉnh trước đó được lưu giữ giữ hướng di chuyển.
