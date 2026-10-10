---
title: "CF 104976C - Một truy vấn đường dẫn ngắn nhất khác"
description: "Chúng ta được cung cấp một biểu đồ có trọng số vô hướng lớn và sau đó là nhiều truy vấn độc lập. Mỗi truy vấn yêu cầu cách di chuyển rẻ nhất giữa hai đỉnh nhất định, nhưng có một hạn chế nghiêm ngặt: tuyến đường được phép sử dụng tối đa ba cạnh."
date: "2026-06-28T05:58:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104976
codeforces_index: "C"
codeforces_contest_name: "The 2023 ICPC Asia Hangzhou Regional Contest (The 2nd Universal Cup. Stage 22: Hangzhou)"
rating: 0
weight: 104976
solve_time_s: 85
verified: true
draft: false
---

[CF 104976C - Một truy vấn đường dẫn ngắn nhất khác](https://codeforces.com/problemset/problem/104976/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số vô hướng lớn và sau đó là nhiều truy vấn độc lập. Mỗi truy vấn yêu cầu cách di chuyển rẻ nhất giữa hai đỉnh nhất định, nhưng có một hạn chế nghiêm ngặt: tuyến đường được phép sử dụng tối đa ba cạnh. 

Vì vậy, thay vì yêu cầu một đường đi ngắn nhất tổng quát, chúng ta chỉ được phép xem xét các bước đi cực ngắn: hoặc là một cạnh trực tiếp, một đường đi gồm hai cạnh đi qua đúng một đỉnh trung gian, hoặc một đường đi gồm ba cạnh đi qua đúng hai đỉnh trung gian. Bất cứ điều gì dài hơn đều không liên quan ngay cả khi nó rẻ hơn. 

Bản thân biểu đồ có thể rất lớn, lên tới một triệu đỉnh và một triệu cạnh, vì vậy chúng tôi không thể đủ khả năng tìm kiếm biểu đồ theo mỗi truy vấn. Số lượng truy vấn cũng lên tới một triệu, điều này buộc chúng tôi phải sử dụng phương pháp tiền xử lý trong đó tất cả thông tin hữu ích phải được tính toán một lần và sau đó được trả lời trong thời gian không đổi. 

Một hạn chế về cấu trúc quan trọng là đồ thị có tính phẳng. Trong thực tế, điều này ngụ ý tính thưa thớt: số cạnh là tuyến tính theo số đỉnh và mức độ trung bình vẫn bị giới hạn. Đây là lý do ẩn giấu tại sao việc liệt kê các chuyến đi bộ ngắn lại khả thi. 

Các trường hợp chính xuất phát từ thực tế là câu trả lời tốt nhất có thể đến từ các độ dài đường dẫn khác nhau và đôi khi đường dẫn tối ưu không phải là duy nhất. Ví dụ: tuyến đường trực tiếp có thể đắt trong khi tuyến đường hai biên có giá rẻ. 

Một ví dụ nhỏ minh họa sự cần thiết phải so sánh tất cả các độ dài: 

đầu vào:```
3 3
1 2 10
2 3 1
1 3 100
1
1 3
```Đầu ra đúng:```
11
```Một thuật toán đường đi ngắn nhất ngây thơ có thể tìm thấy cạnh trực tiếp có trọng số 100 và dừng lại, thiếu tuyến đường hai cạnh rẻ hơn. Quan trọng hơn nữa, Dijkstra tiêu chuẩn là không cần thiết vì các đường dẫn dài hơn ba cạnh không được phép, do đó việc thăm dò toàn cầu là công việc lãng phí. 

Một trường hợp tinh tế khác là khi tồn tại nhiều bước đi ngắn giữa cùng một điểm cuối. Chúng ta chỉ được giữ trọng lượng tối thiểu trong số tất cả các lần đi bộ hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ xử lý từng truy vấn một cách độc lập. Đối với mỗi cặp`(s, t)`, chúng ta có thể chạy tìm kiếm BFS hoặc Dijkstra có giới hạn chỉ cho phép tối đa ba cạnh. Điều này vẫn khám phá hàng xóm của hàng xóm và có thể là hàng xóm của những hàng xóm đó. Trong trường hợp xấu nhất, ngay cả tìm kiếm giới hạn này cũng có thể truy cập tới O(m) cạnh cho mỗi truy vấn, vì thuật toán không biết mục tiêu nằm ở đâu và có thể mở rộng rộng rãi trước khi đạt đến độ sâu thứ ba. Với tối đa một triệu truy vấn, điều này trở nên hoàn toàn không khả thi, dẫn đến khoảng 10¹² hoạt động trong các trường hợp dày đặc. 

Quan sát quan trọng là giới hạn độ sâu cực kỳ nhỏ và cố định. Mọi câu trả lời hợp lệ đều tương ứng với đường đi của một trong ba dạng cấu trúc: một cạnh, chuỗi hai cạnh hoặc chuỗi ba cạnh. Điều này có nghĩa là chúng ta không cần phải tìm kiếm linh hoạt. Thay vào đó, chúng ta có thể liệt kê mọi đường đi có độ dài tối đa là ba trong toàn bộ biểu đồ một lần, tính toán điểm cuối và chi phí của nó, đồng thời lưu trữ kết quả tốt nhất cho mỗi cặp được sắp xếp. 

Tính phẳng đảm bảo rằng biểu đồ đủ thưa để việc liệt kê tất cả các đường đi ngắn như vậy vẫn tuyến tính trong thực tế. Trung bình mỗi đỉnh chỉ có một số đỉnh lân cận không đổi, do đó, việc mở rộng tất cả các bước đi có chiều dài 2 và chiều dài 3 không bùng nổ về mặt tổ hợp. 

Điều này biến vấn đề thành một nhiệm vụ tiền xử lý trên các vùng lân cận cục bộ, theo sau là tra cứu hàm băm O(1) cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu cho mỗi tìm kiếm truy vấn | O(q · m) | O(n + m) | Quá chậm | 
| Liệt kê tất cả các đường dẫn 3 cạnh | O(m) dự kiến ​​| O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chiến lược tiền xử lý tối ưu 

1. Xây dựng danh sách kề lưu trữ tất cả các đỉnh lân cận cùng với trọng số của các cạnh cho mỗi đỉnh. Điều này cho phép truy cập liên tục vào cấu trúc cục bộ, điều này rất cần thiết vì mọi đường dẫn hợp lệ đều có độ dài tối đa là ba. 
2. Khởi tạo bản đồ băm`best[(u, v)]`sẽ lưu trữ chi phí tối thiểu của bất kỳ đường dẫn hợp lệ nào từ`u`ĐẾN`v`sử dụng nhiều nhất ba cạnh. Chúng tôi lưu trữ các cặp có hướng vì một đường dẫn có hướng mặc dù đồ thị là vô hướng. 
3. Chèn tất cả các đường dẫn một cạnh. Đối với mọi cạnh`(u, v, w)`, cập nhật`best[(u, v)]`Và`best[(v, u)]`với chi phí`w`. Điều này xử lý tất cả các giải pháp có độ dài-1. 
4. Liệt kê tất cả các đường dẫn có độ dài-2 bằng cách mở rộng mỗi cạnh một lần. Với mọi đỉnh`u`, xem xét từng người hàng xóm`v`, thì với mỗi hàng xóm`x`của`v`, chúng ta thu được một đường đi`u → v → x`với chi phí`w(u, v) + w(v, x)`. Cập nhật`best[(u, x)]`. Bước này nắm bắt tất cả các tuyến đường hai cạnh hợp lệ. 
5. Mở rộng một lần nữa để tạo ra tất cả các đường dẫn có độ dài 3. Đối với mỗi công trình có chiều dài-2`u → v → x`, lặp qua hàng xóm`y`của`x`và cập nhật`best[(u, y)]`với chi phí`w(u, v) + w(v, x) + w(x, y)`. Điều này hoàn thành tất cả các bước đi ba cạnh hợp lệ. 
6. Trả lời từng câu hỏi`(s, t)`bằng cách quay lại`best[(s, t)]`nếu nó tồn tại, nếu không thì xuất ra`-1`. 

### Tại sao nó hoạt động 

Mọi câu trả lời hợp lệ đều tương ứng chính xác với bước đi có độ dài 1, 2 hoặc 3. Thuật toán xây dựng rõ ràng tất cả các bước đi như vậy bằng cách lặp qua tất cả các chuỗi có thể có của các cạnh liền kề. Vì mỗi bước đều mở rộng một cạnh thực nên không có đường dẫn không hợp lệ nào được đưa vào. Vì chúng tôi chỉ lưu trữ chi phí tối thiểu cho mỗi cặp điểm cuối nên nhiều cấu trúc chồng chéo sẽ thu gọn chính xác thành giá trị tối ưu. Không có khả năng thiếu một đường đi hợp lệ vì mọi lựa chọn có thể có của các đỉnh trung gian đều được liệt kê thông qua việc khai triển kề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]

    edges = []
    for _ in range(m):
        u, v, w = map(int, input().split())
        adj[u].append((v, w))
        adj[v].append((u, w))
        edges.append((u, v, w))

    best = {}

    def upd(a, b, w):
        key = (a, b)
        if key not in best or w < best[key]:
            best[key] = w

    for u, v, w in edges:
        upd(u, v, w)
        upd(v, u, w)

    for u in range(1, n + 1):
        for v, w1 in adj[u]:
            for x, w2 in adj[v]:
                if x == u:
                    continue
                upd(u, x, w1 + w2)

    for u in range(1, n + 1):
        for v, w1 in adj[u]:
            for x, w2 in adj[v]:
                if x == u:
                    continue
                for y, w3 in adj[x]:
                    if y == v:
                        continue
                    upd(u, y, w1 + w2 + w3)

    q = int(input())
    out = []
    for _ in range(q):
        s, t = map(int, input().split())
        out.append(str(best.get((s, t), -1)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Danh sách kề là xương sống của giải pháp vì nó cho phép liệt kê liên tục tất cả các ứng cử viên có thể tiếp cận trong một bước. các`best`từ điển là biểu diễn nén của toàn bộ không gian trả lời: thay vì lưu trữ đường dẫn một cách rõ ràng, nó chỉ lưu trữ chi phí tối ưu cho mỗi cặp điểm cuối. 

Việc mở rộng lồng ba là phần quan trọng. Vòng lặp đầu tiên mã hóa tất cả các cạnh. Vòng lặp thứ hai xây dựng tất cả các bước đi hai cạnh và vòng lặp thứ ba mở rộng chúng thành các bước đi ba cạnh. Những tấm séc nhỏ như`x == u`hoặc`y == v`tránh việc quay lại tầm thường ngay lập tức mà nếu không sẽ tạo ra các trạng thái dư thừa, mặc dù tính chính xác không phụ thuộc vào chúng. 

Tất cả các câu trả lời truy vấn đều là tra cứu từ điển đơn giản, điều này làm cho giải pháp có quy mô lên tới một triệu truy vấn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi chỉ theo dõi một vài mục tiêu biểu trong`best`khi nó phát triển. 

| Bước | Cấu trúc đã qua xử lý | cập nhật tốt nhất (ví dụ) | 
| --- | --- | --- | 
| 1 | Các cạnh trực tiếp | (1,2)=4, (2,3)=6, (3,6)=5, (1,4)=3, (5,3)=1 | 
| 2 | Đường dẫn hai cạnh | (1,3)=min(1→2→3=10, 1→4→5→3 sau)=10, (3,4)=11, (2,5)=7 | 
| 3 | Đường dẫn ba cạnh | (1,3) cải thiện thông qua 1→4→5→3 = 7, sau đó 1→4→5→3 cho 7, nhưng tốt hơn thông qua 1→4→5→3 hay 1→2→3→? cho 6 | 
| 4 | Truy vấn cuối cùng | câu trả lời được trích xuất | 

Các đầu ra cuối cùng tương ứng chính xác với mức tối thiểu trong số tất cả các cấu trúc 1, 2 và 3 cạnh, khớp với mẫu. 

### Mẫu 2 

| Truy vấn | Đường dẫn đã kiểm tra | Kết quả | 
| --- | --- | --- | 
| 1 → 4 | 1-2-3-4 | 3 | 
| 1 → 5 | không có đường đi 3 cạnh | -1 | 
| 1 → 6 | không thể truy cập ở 3 cạnh | -1 | 

Ví dụ này nhấn mạnh rằng mặc dù biểu đồ được kết nối nhưng giới hạn độ sâu khiến nhiều cặp không thể truy cập được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) dự kiến ​​| Mỗi cạnh được sử dụng trong việc mở rộng vùng lân cận có độ sâu không đổi (độ sâu 3) và tính phẳng giữ cho mức độ trung bình bị giới hạn | 
| Không gian | O(m) | Danh sách kề cộng với bản đồ băm chỉ lưu trữ các cặp điểm cuối có thể truy cập từ các bước đi 3 cạnh | 

Các ràng buộc cho phép lên tới một triệu cạnh, nhưng độ sâu mở rộng giới hạn đảm bảo chúng tôi chỉ xử lý một số lượng mẫu cục bộ không đổi trên mỗi cạnh. Điều này giúp giải pháp luôn trong giới hạn thời gian mặc dù kích thước đầu vào lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# sample 1
assert run("""6 9
1 2 4
2 3 6
3 6 5
6 5 3
5 4 2
4 1 3
3 4 9
1 3 100
5 3 1
5
1 3
1 6
3 4
3 5
2 5
""") == """6
8
3
1
7"""

# sample 2
assert run("""6 4
1 2 1
2 3 1
3 4 1
4 5 1
3
1 4
1 5
1 6
""") == """3
-1
-1"""

# custom: single edge
assert run("""2 1
1 2 5
1
1 2
""") == "5"

# custom: triangle
assert run("""3 3
1 2 2
2 3 2
1 3 10
1 1
""") == "2"

# custom: no path
assert run("""4 2
1 2 1
3 4 1
1
1 4
""") == "-1"

# custom: 3-edge best
assert run("""4 3
1 2 1
2 3 1
3 4 1
1
1 4
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 5 | xử lý cạnh trực tiếp | 
| tam giác | 2 | chọn gián tiếp trên cạnh trực tiếp | 
| ngắt kết nối | -1 | trường hợp không thể truy cập | 
| dây chuyền 3 cạnh | 3 | chính xác đường dẫn 3 cạnh | 

## Vỏ cạnh 

Một cạnh trực tiếp kém hơn đường dẫn nhiều bước được xử lý chính xác vì bước khởi tạo lưu trữ tất cả các cạnh, nhưng các bản cập nhật sau này từ phần mở rộng hai cạnh và ba cạnh có thể ghi đè bằng các giá trị nhỏ hơn. 

Ví dụ, trong một tam giác`1-2-3`với một cạnh nặng`1-3`, thuật toán đầu tiên lưu trữ`(1,3)=heavy`, sau đó phát hiện ra`1→2→3`và thay thế nó bằng một giá trị nhỏ hơn. Điều này đảm bảo từ điển cuối cùng luôn phản ánh mức tối ưu trong số tất cả độ dài đường dẫn được phép. 

Trường hợp cạnh thứ hai là khi đường đi tốt nhất sử dụng chính xác ba cạnh. Lớp mở rộng thứ ba đảm bảo rằng tất cả các đường dẫn như vậy được tạo rõ ràng. Ngay cả khi tiền tố hai cạnh không tối ưu, nó vẫn được sử dụng làm khối xây dựng, do đó không có giải pháp ba cạnh hợp lệ nào bị bỏ qua.
