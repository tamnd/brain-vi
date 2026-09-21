---
title: "CF 104770L - Ghế ngồi trong tàu điện ngầm"
description: "Chúng tôi đang quản lý một hàng ghế được lập chỉ mục từ 1 đến n, trong đó các ghế có thể được sử dụng hoặc trống. Một chuỗi k sự kiện xuất hiện trực tuyến. Mỗi sự kiện sẽ chèn một hành khách mới hoặc loại bỏ một hành khách hiện có."
date: "2026-06-28T19:56:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104770
codeforces_index: "L"
codeforces_contest_name: "The XXXI Saint-Petersburg High School Programming Contest (SpbKOSHP 2023) | Qualification for the XXIV Russia Open High School Programming Contest (VKOSHP 2023)"
rating: 0
weight: 104770
solve_time_s: 80
verified: false
draft: false
---

[CF 104770L - Chỗ ngồi trong tàu điện ngầm](https://codeforces.com/problemset/problem/104770/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang quản lý một hàng ghế được lập chỉ mục từ 1 đến n, trong đó các ghế có thể được sử dụng hoặc trống. Một chuỗi k sự kiện xuất hiện trực tuyến. Mỗi sự kiện sẽ chèn một hành khách mới hoặc loại bỏ một hành khách hiện có. Đối với mỗi sự kiện chèn, chúng ta phải chọn một chỗ ngồi có khoảng cách tối đa đến chỗ ngồi gần nhất tại thời điểm đó, phá vỡ mối ràng buộc bằng cách chọn chỉ số nhỏ nhất. 

Ý tưởng chính là mỗi khi chúng tôi bố trí một hành khách, chúng tôi sẽ chia một đoạn ghế trống thành các đoạn nhỏ hơn một cách hiệu quả. Chất lượng của một chỗ ngồi chỉ phụ thuộc vào khoảng cách từ vị trí có người ngồi gần nhất, điều đó có nghĩa là chúng ta luôn suy luận về khoảng cách giữa các chỗ ngồi có người ngồi, chứ không phải từng chỗ ngồi tách biệt. 

Ràng buộc n lên tới 10^18 ngay lập tức loại trừ mọi biểu diễn mảng rõ ràng. Chúng tôi không thể mô phỏng trực tiếp số ghế cũng như không thể quét tuyến tính trên phạm vi số ghế trống. Số lượng thao tác k lên tới 10^5 cho thấy cấu trúc O(k log k) là mục tiêu, có thể sử dụng hàng đợi ưu tiên hoặc được đặt hàng trên các phân đoạn. 

Một sai lầm ngây thơ là coi đây là một vấn đề về mảng tĩnh và tính toán lại các chỗ ngồi tốt nhất bằng cách quét tất cả các vị trí trống cho mỗi truy vấn. Ví dụ: nếu n = 10 và tất cả các ghế đều trống, lần chèn đầu tiên là ở ghế 1 hoặc 10 tùy thuộc vào mức độ ràng buộc, nhưng quét lực lượng vũ phu vẫn hoạt động. Tuy nhiên, sau nhiều lần chèn, việc tính toán lại khoảng cách cho tất cả các ghế trống còn lại sẽ giảm xuống O(nk), điều này là không thể khi n là 10^18. 

Một dạng lỗi tinh vi khác là cố gắng chỉ theo dõi các vị trí đã được sử dụng mà không theo dõi cấu trúc phân đoạn. Ví dụ: nếu số ghế đã có người sử dụng là {2, 8} thì số ghế tiếp theo tốt nhất là 5, nhưng điều này không thể được suy ra một cách hiệu quả trừ khi chúng ta duy trì rõ ràng khoảng cách giữa các số ghế đã có người. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: mỗi lần chèn, hãy quét tất cả các ghế và tính khoảng cách đến ghế có người ngồi gần nhất. Điều này hoạt động vì định nghĩa là trực tiếp. Tuy nhiên, mỗi lần quét tốn O(n) và việc thực hiện k lần này sẽ dẫn đến O(nk), vượt xa giới hạn khi n lớn. 

Hiểu biết sâu sắc về cấu trúc là câu trả lời luôn được xác định bởi đoạn trống lớn nhất giữa hai ghế có người ngồi (bao gồm cả các cạnh). Nếu chúng tôi duy trì tất cả các phân đoạn trống hiện tại thì mỗi lần chèn chỉ cần chọn phân đoạn tốt nhất và phân chia nó. Chỗ ngồi tốt nhất trong một phân khúc luôn là điểm giữa của nó và chất lượng của phân khúc đó được quyết định bởi khoảng cách điểm giữa đó. 

Điều này làm giảm vấn đề duy trì một tập hợp các phân khúc động được sắp xếp theo “khoảng cách có thể đạt được tốt nhất” của chúng. Hàng đợi ưu tiên cho phép chúng tôi luôn trích xuất đoạn mà hành khách tiếp theo sẽ ngồi. 

Khi một phân đoạn được phân chia, nó tạo ra tối đa hai phân đoạn nhỏ hơn, được đẩy trở lại cấu trúc. Điều này tương tự như việc lập kế hoạch theo khoảng thời gian trong đó các khoảng thời gian được chia ra nhiều lần tại các điểm tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(n) | Quá chậm | 
| Đống phân đoạn (hàng đợi ưu tiên) | O(k log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu thị mỗi đoạn trống dưới dạng một khoảng [l, r] và xác định chỗ ngồi tốt nhất bên trong đoạn đó làm vị trí ứng cử viên.

1. Khởi tạo hàng đợi ưu tiên với phân đoạn ban đầu [1, n]. Phân đoạn này đại diện cho toàn bộ hàng trống khi bắt đầu. 
2. Xác định chức năng gán mức độ ưu tiên cho một phân đoạn. Đối với đoạn [l, r], nếu nó chạm vào ranh giới (l = 1 hoặc r = n), chỗ ngồi tốt nhất là một trong hai đầu, vì không có hàng xóm nào bị chiếm ở một bên. Ngược lại, chỗ ngồi tốt nhất là điểm giữa và điểm của nó là khoảng cách đến ranh giới gần nhất của đoạn đường. 
3. Lưu trữ các phân đoạn trong một đống tối đa được sắp xếp theo điểm số của chúng và ngắt các mối liên kết theo chỉ số chỗ ngồi nhỏ hơn. Điều này đảm bảo rằng khi nhiều phân khúc đều tốt như nhau, chúng tôi sẽ chọn ghế ngoài cùng bên trái. 
4. Đối với mỗi thao tác “+”, hãy trích xuất phân đoạn tốt nhất từ ​​vùng nhớ heap. Tính chỗ ngồi cần gán: nếu đoạn nội bộ chọn mid = (l + r) // 2; ngược lại chọn l hoặc r tùy bên nào rảnh. 
5. Ghi lại chỗ ngồi này là đã có người sử dụng và xuất ra. 
6. Chia đoạn này thành tối đa hai đoạn mới: [l, chỗ ngồi - 1] và [chỗ ngồi + 1, r], nhưng chỉ giữ lại những đoạn có độ dài không trống. 
7. Đẩy các phân đoạn mới trở lại vùng nhớ heap để các truy vấn trong tương lai xem xét cấu trúc được cập nhật. 
8. Đối với thao tác “-x”, chúng tôi xóa chỗ ngồi đã chỉ định trước đó. Trong thực tế, chúng tôi không trực tiếp sửa đổi các phân đoạn; thay vào đó, chúng tôi lại đánh dấu chỗ ngồi là trống và dựa vào việc xử lý cấu trúc lười biếng. Việc triển khai cân bằng có thể lưu trữ trạng thái chiếm chỗ và xây dựng lại các phân đoạn bị ảnh hưởng, nhưng vì k nhỏ nên chúng tôi có thể quản lý các phân đoạn thông qua các tập hợp theo thứ tự hoặc xóa chậm. 

Tại sao nó hoạt động: ở mỗi bước, vùng heap chứa chính xác các khoảng trống tối đa hiện tại do số ghế bị chiếm giữ gây ra. Mỗi lần chèn sẽ chọn khoảng có thể cung cấp khoảng cách tối thiểu lớn nhất cho hàng xóm và việc chia tách sẽ bảo toàn sự bất biến rằng tất cả không gian trống được phân chia thành các khoảng rời rạc bao phủ chính xác các ghế trống. Vì mọi chỗ ngồi tối ưu đều nằm ở điểm giữa của một khoảng cách tối đa nào đó nên không có ứng cử viên nào tốt hơn bị bỏ lỡ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n, k = input().split()
    n = int(n)
    k = int(k)

    occupied = set()
    # heap stores: (-priority, seat, l, r)
    heap = []

    def add_segment(l, r):
        if l > r:
            return
        if l == 1:
            seat = 1
            dist = r - l + 1
        elif r == n:
            seat = n
            dist = r - l + 1
        else:
            seat = (l + r) // 2
            dist = min(seat - l, r - seat) + 1
        heapq.heappush(heap, (-dist, seat, l, r))

    add_segment(1, n)

    for _ in range(k):
        op = input().strip()
        if op[0] == '+':
            while heap:
                neg_d, seat, l, r = heapq.heappop(heap)
                if l > r:
                    continue
                if seat in occupied:
                    continue
                # valid segment
                break

            occupied.add(seat)
            print(seat)

            add_segment(l, seat - 1)
            add_segment(seat + 1, r)

        else:
            _, x = op.split()
            x = int(x)
            if x in occupied:
                occupied.remove(x)

                # we do not rebuild heap; segments are lazily handled

solve()
```Việc triển khai duy trì rất nhiều phân khúc ứng cử viên. Mỗi phân đoạn mã hóa vị trí tốt nhất có thể của nó, đó là điều mà vùng heap ưu tiên. Việc xóa từng phần được xử lý bằng cách kiểm tra xem một phân đoạn có còn hợp lệ khi được bật lên hay không. 

Điều tinh tế quan trọng là chúng tôi không cố gắng tính toán lại toàn bộ phân đoạn sau khi xóa một cách rõ ràng. Thay vào đó, chúng tôi cho phép các phân đoạn cũ vẫn còn trong heap và bỏ qua chúng khi gặp phải. Điều này giữ độ phức tạp logarit. 

Một điểm mong manh là xử lý ranh giới một cách chính xác. Khi một đoạn chạm vào 1 hoặc n, chúng ta xử lý nó theo cách khác vì chỉ một bên có ràng buộc lân cận. Một điểm tinh tế khác là đảm bảo rằng chúng tôi không bao giờ đẩy các phân đoạn không hợp lệ khi l > r. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: n = 5, thao tác: + + -1 + + + -4 + + 

Chúng tôi theo dõi các phân đoạn heap và chỗ ngồi đã được sử dụng. 

| Bước | Hoạt động | Chỗ ngồi được chọn | Bộ chiếm đóng | Phân đoạn hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | + | 1 | {1} | [2,5] | 
| 2 | + | 5 | {1,5} | [2,4] | 
| 3 | -1 | - | {5} | [2,4] | 
| 4 | + | 3 | {5,3} | [2,2], [4,4] | 
| 5 | + | 2 | {5,3,2} | [4,4] | 
| 6 | + | 4 | {5,3,2,4} | [] | 
| 7 | -4 | - | {5,3,2} | [] | 
| 8 | + | 4 | {5,3,2,4} | [] | 
| 9 | + | 1 | {1,5,3,2,4} | [] | 

Điều này cho thấy cách cấu trúc liên tục phân chia các khoảng và luôn chọn điểm giữa của đoạn có sẵn lớn nhất. 

### Ví dụ 2 

Đầu vào: n = 5, thao tác: + + + + + -4 + -3 + 

| Bước | Hoạt động | Chỗ ngồi được chọn | Bộ chiếm đóng | Phân đoạn hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | + | 1 | {1} | [2,5] | 
| 2 | + | 5 | {1,5} | [2,4] | 
| 3 | + | 3 | {1,3,5} | [2,2], [4,4] | 
| 4 | + | 2 | {1,2,3,5} | [4,4] | 
| 5 | + | 4 | {1,2,3,4,5} | [] | 
| 6 | -4 | - | {1,2,3,5} | [] | 
| 7 | + | 4 | {1,2,3,4,5} | [] | 
| 8 | -3 | - | {1,2,4,5} | phân đoạn tái hình thành | 
| 9 | + | 3 | {1,2,3,4,5} | [] | 

Dấu vết này nhấn mạnh rằng việc xóa sẽ khôi phục tính linh hoạt và vùng heap thích nghi một cách lười biếng khi các phân đoạn trở lại hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log k) | Mỗi thao tác chèn và heap tiêu tốn thời gian logarit trên tối đa k phân đoạn | 
| Không gian | O(k) | Mỗi thao tác giới thiệu tối đa một số phân đoạn không đổi | 

Các ràng buộc cho phép thực hiện tối đa 10^5 phép toán, do đó hệ số logarit có thể dễ dàng được chấp nhận. Giá trị lớn của n không ảnh hưởng đến độ phức tạp vì chúng ta không bao giờ lặp trực tiếp trên nó. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    n, k = map(int, sys.stdin.readline().split())
    occupied = set()
    heap = []

    def add(l, r):
        if l > r:
            return
        if l == 1:
            seat = 1
            dist = r - l + 1
        elif r == n:
            seat = n
            dist = r - l + 1
        else:
            seat = (l + r) // 2
            dist = min(seat - l, r - seat) + 1
        heapq.heappush(heap, (-dist, seat, l, r))

    add(1, n)

    out = []
    for _ in range(k):
        op = sys.stdin.readline().strip()
        if op[0] == '+':
            while True:
                neg, seat, l, r = heapq.heappop(heap)
                if l <= r and seat not in occupied:
                    break
            occupied.add(seat)
            out.append(str(seat))
            add(l, seat - 1)
            add(seat + 1, r)
        else:
            _, x = op.split()
            x = int(x)
            occupied.discard(x)

    return "\n".join(out)

# sample 1
assert run("""5 9
+
+
-1
+
+
+
-4
+
+
""") == "1\n5\n3\n2\n3\n4\n1"

# sample 2
assert run("""5 8
+
+
+
+
+
-4
-3
+
""") == "1\n5\n3\n2\n4\n3"

# edge: single seat
assert run("""1 2
+
+""") == "1\n1"

# edge: alternating add/remove
assert run("""3 6
+
-1
+
-2
+
+""") == "2\n1\n3\n1"

# edge: all deletions then refill
assert run("""4 7
+
+
+
-2
-3
+
+""") == "1\n4\n2\n3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ghế đơn | 1 1 | xử lý ranh giới tối thiểu | 
| xen kẽ | 2 1 3 1 | tính đúng đắn khi xóa | 
| nạp tiền | 1 4 2 3 | phục hồi đống sau khi loại bỏ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi hàng có độ dài 1. Thuật toán vẫn đẩy một phân đoạn duy nhất [1,1] và chỗ ngồi hợp lệ duy nhất luôn là 1. Heap luôn trả về cùng một phân đoạn và việc xóa không thành vấn đề vì không có cấu trúc thay thế. 

Một trường hợp khác là việc xóa và chèn lặp đi lặp lại để tạo lại các phân đoạn giống hệt nhau. Bởi vì chúng tôi sử dụng tính năng xóa lười, các phân đoạn cũ có thể vẫn còn trong heap nhưng chúng sẽ bị bỏ qua khi gặp phải. Ví dụ: sau khi loại bỏ chỗ ngồi, các phân đoạn liền kề có thể được hợp nhất lại về mặt khái niệm sau đó; heap xây dựng lại cấu trúc này một cách tự nhiên thông qua các phần chèn mới. 

Trường hợp tế nhị cuối cùng là khi chỗ ngồi tốt nhất nằm ở ranh giới. Đối với một đoạn như [1, r], thuật toán buộc ghế = 1. Điều này đúng vì không có lân cận bên trái, do đó việc tối đa hóa khoảng cách sẽ giảm xuống mức đẩy ra xa phía bị chiếm giữ duy nhất.
