---
title: "CF 105020N - Có bao nhiêu hình chữ nhật?"
description: "Mỗi hình chữ nhật trong đầu vào được xác định đầy đủ bởi góc dưới bên trái và góc trên bên phải của nó. Vì tất cả các hình chữ nhật đều bắt đầu ở góc phần tư không âm nên gốc (0, 0) đóng vai trò là điểm tham chiếu tự nhiên. Một truy vấn đưa ra một điểm (x, y)."
date: "2026-06-28T02:02:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105020
codeforces_index: "N"
codeforces_contest_name: "TCPC Tunisian Collegiate Programming Contest 2022"
rating: 0
weight: 105020
solve_time_s: 77
verified: false
draft: false
---

[CF 105020N - Có bao nhiêu hình chữ nhật?](https://codeforces.com/problemset/problem/105020/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi hình chữ nhật trong đầu vào được xác định đầy đủ bởi góc dưới bên trái và góc trên bên phải của nó. Vì tất cả các hình chữ nhật đều bắt đầu ở góc phần tư không âm nên gốc (0, 0) đóng vai trò là điểm tham chiếu tự nhiên. 

Một truy vấn đưa ra một điểm (x, y). Điều này xác định một hình chữ nhật được căn chỉnh theo trục từ điểm gốc đến điểm đó. Nhiệm vụ là đếm xem có bao nhiêu hình chữ nhật đã cho nằm hoàn toàn bên trong hình chữ nhật truy vấn này. 

Một hình chữ nhật được chứa đầy đủ nếu mọi điểm của nó đều nằm trong ranh giới truy vấn. Vì tất cả các hình chữ nhật đã bắt đầu từ tọa độ không âm, nên việc ngăn chặn giảm xuống một điều kiện đơn giản ở góc trên bên phải của chúng: một hình chữ nhật hợp lệ cho một truy vấn nếu góc trên bên phải của nó (x2, y2) thỏa mãn x2 ≤ x và y2 ≤ y. 

Vì vậy, bài toán không còn mang tính hình học theo nghĩa phức tạp nữa. Nó trở thành một bài toán đếm ưu thế 2D trên tập hợp các điểm được hình thành bởi các góc trên bên phải của hình chữ nhật. 

Các ràng buộc n, Q ≤ 100000 ngụ ý rằng mọi giải pháp gần với O(nQ) đều không thể thực hiện được. Việc kiểm tra trực tiếp cho mỗi truy vấn sẽ yêu cầu tối đa 10^10 so sánh, vượt xa giới hạn thời gian. Ngay cả O(n log n + Q log n) cũng cần thiết và chúng tôi thường hướng tới O((n + Q) log n). 

Một vấn đề tế nhị phát sinh từ việc xử lý sự bình đẳng. Phải bao gồm các hình chữ nhật có x2 chính xác bằng truy vấn x và điều tương tự cũng áp dụng cho y. Bất kỳ lỗi bất đẳng thức nghiêm ngặt nào cũng sẽ được tính dưới mức âm thầm. 

Một trường hợp cạnh khác là khi nhiều hình chữ nhật có cùng giá trị x2 hoặc y2. Cách tiếp cận sắp xếp đơn giản không nhóm các giá trị bằng nhau một cách cẩn thận có thể dẫn đến cập nhật gia tăng không chính xác nếu các truy vấn được xử lý sai thứ tự. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, lặp lại tất cả các hình chữ nhật và kiểm tra xem x2 ≤ x và y2 ≤ y. Điều này đúng vì nó mã hóa trực tiếp định nghĩa về ngăn chặn. Tuy nhiên, mỗi truy vấn có chi phí O(n) và với truy vấn Q, giá trị này trở thành O(nQ), trong trường hợp xấu nhất là khoảng 10^10 thao tác. Như vậy là quá chậm. 

Quan sát quan trọng là hình chữ nhật có thể được rút gọn thành các điểm (x2, y2) và mỗi truy vấn sẽ hỏi xem có bao nhiêu điểm nằm trong góc phần tư phía dưới bên trái được giới hạn bởi (x, y). Đây là một bài toán đếm tiền tố cổ điển trong hai chiều. 

Nếu sắp xếp hình chữ nhật theo x2, chúng ta có thể xử lý các truy vấn theo thứ tự tăng dần của x. Khi di chuyển qua các truy vấn, chúng tôi duy trì cấu trúc dữ liệu lưu trữ tất cả các giá trị y2 của hình chữ nhật có x2 đủ nhỏ. Sau đó, mỗi truy vấn giảm xuống số tiền tố 1D trên y. 

Cây Fenwick (Cây chỉ mục nhị phân) trên tọa độ y được nén cho phép chúng ta duy trì số đếm và trả lời tổng tiền tố theo thời gian logarit. Mỗi hình chữ nhật được chèn một lần và mỗi truy vấn thực hiện một tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nQ) | O(1) | Quá chậm | 
| Phân loại + Cây Fenwick | O((n + Q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Thay thế mỗi hình chữ nhật bằng một điểm (x2, y2). Điều này hiệu quả vì chỉ góc trên bên phải xác định việc ngăn chặn và góc dưới bên trái không liên quan đến vấn đề này vì tất cả các hình chữ nhật đều bắt đầu từ tọa độ không âm. 
2. Lưu trữ tất cả các truy vấn dưới dạng bộ ba (x, y, chỉ mục), vì chúng ta cần trả về câu trả lời theo thứ tự ban đầu. 
3. Sắp xếp các hình chữ nhật theo x2 theo thứ tự tăng dần. Đồng thời sắp xếp các truy vấn theo x theo thứ tự tăng dần. Điều này đảm bảo rằng khi chúng tôi xử lý một truy vấn, tất cả các hình chữ nhật có x2 ≤ x đều đã được xem xét. 
4. Nén tất cả các giá trị y2 và tất cả các giá trị y truy vấn vào một phạm vi tọa độ nhỏ hơn. Điều này cho phép Cây Fenwick hoạt động hiệu quả ngay cả khi tọa độ ban đầu lên tới 10^9. 
5. Quét qua các truy vấn theo thứ tự tăng dần của x. Duy trì một con trỏ trên hình chữ nhật. Đối với mỗi truy vấn, hãy chèn tất cả các hình chữ nhật có x2 ≤ truy vấn hiện tại x vào Cây Fenwick, cập nhật tần số y2 của chúng. 
6. Đối với mỗi truy vấn, hãy tính xem có bao nhiêu hình chữ nhật được chèn có y2 ≤ truy vấn y bằng cách sử dụng truy vấn tổng tiền tố trên Cây Fenwick. Lưu trữ kết quả. 
7. Kết quả đầu ra theo thứ tự truy vấn ban đầu. 

### Tại sao nó hoạt động 

Tại mỗi điểm truy vấn x, cấu trúc dữ liệu chứa chính xác tập hợp các hình chữ nhật có x2 đủ nhỏ để vừa với ranh giới truy vấn. Cây Fenwick duy trì số lượng trên các giá trị y2, do đó việc truy vấn tiền tố trên y sẽ đếm trực tiếp chính xác những hình chữ nhật cũng thỏa mãn y2 ≤ y. Vì cả hai điều kiện đều được thực thi tăng dần và độc lập nên không có hình chữ nhật nào bị tính hai lần hoặc bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

def solve():
    n = int(input())
    rects = []
    ys = []

    for _ in range(n):
        x1, y1, x2, y2 = map(int, input().split())
        rects.append((x2, y2))
        ys.append(y2)

    q = int(input())
    queries = []
    for i in range(q):
        x, y = map(int, input().split())
        queries.append((x, y, i))
        ys.append(y)

    ys = sorted(set(ys))
    def get(v):
        l, r = 0, len(ys) - 1
        while l <= r:
            m = (l + r) // 2
            if ys[m] <= v:
                l = m + 1
            else:
                r = m - 1
        return l

    rects.sort()
    queries.sort()

    fw = Fenwick(len(ys))
    ans = [0] * q

    i = 0
    for x, y, idx in queries:
        while i < n and rects[i][0] <= x:
            fw.add(get(rects[i][1]), 1)
            i += 1
        ans[idx] = fw.sum(get(y))

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ giảm hình học thành một bài toán quét được sắp xếp. Cây Fenwick chỉ chịu trách nhiệm đếm tiền tố y, trong khi bước sắp xếp đảm bảo các ràng buộc x được thực thi khi xây dựng. Việc nén tọa độ là cần thiết vì việc lập chỉ mục trực tiếp với các giá trị lên tới 10^9 sẽ không khả thi. 

Tìm kiếm nhị phân bên trong`get`chuyển đổi tọa độ y thành chỉ số nén của nó. sử dụng`<= v`đảm bảo rằng sự bình đẳng được xử lý chính xác để bao gồm các hình chữ nhật chính xác trên đường biên. 

## Ví dụ đã hoạt động 

Xét các hình chữ nhật (1, 2)-(7, 6), (7, 0)-(10, 3) và truy vấn (8, 10). 

Chúng ta biến hình chữ nhật thành các điểm (7, 6) và (10, 3). Sau khi sắp xếp, chúng tôi xử lý theo thứ tự x. 

| Bước | Hình chữ nhật đã qua xử lý | Nội dung Fenwick (số lượng y2) | Truy vấn | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | (7,6) | {6:1} | (8,10) | 1 | 
| 2 | (10,3) chưa được đưa vào | {6:1} | - | - | 

Truy vấn chỉ bao gồm (7,6) vì 7 ≤ 8 và 6 ≤ 10, trong khi (10,3) bị loại do x2 > 8. 

Bây giờ hãy xem xét ví dụ thứ hai với các hình chữ nhật (2,2)-(5,5), (1,1)-(3,3), (4,4)-(6,6) và các truy vấn (3,3) và (6,6). 

| Truy vấn | Hình chữ nhật hoạt động | Đếm kết quả điều kiện | 
| --- | --- | --- | 
| (3,3) | (3,3), (5,5 chưa theo thứ tự x), (6,6 chưa) | 1 | 
| (6,6) | tất cả các hình chữ nhật | 3 | 

Điều này xác nhận rằng quá trình quét tích lũy chính xác các hình chữ nhật hợp lệ khi x tăng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Sắp xếp cộng với các cập nhật và truy vấn của Fenwick | 
| Không gian | O(n + q) | Lưu trữ hình chữ nhật, truy vấn và tọa độ nén | 

Các phép toán có quy mô thoải mái trong giới hạn vì mỗi hình chữ nhật và truy vấn được xử lý một số lượng phép toán Fenwick logarit không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return capture_output(solve)

def capture_output(func):
    import sys, io
    old = sys.stdout
    sys.stdout = io.StringIO()
    func()
    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# minimal
assert run("""1
1 0 2 3
1
2 3
""") == "1"

# boundary equality
assert run("""2
1 1 3 3
3 3 5 5
1
3 3
""") == "1"

# all rectangles inside
assert run("""3
1 1 2 2
2 2 3 3
3 3 4 4
1
5 5
""") == "3"

# none inside
assert run("""2
5 5 10 10
6 6 9 9
1
4 4
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | 1 | độ đúng cơ sở | 
| trường hợp bình đẳng | 1 | bao gồm ranh giới | 
| tất cả bên trong | 3 | tích lũy đúng đắn | 
| không có bên trong | 0 | logic loại trừ | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi hình chữ nhật có chung giá trị x2 hoặc y2. Thuật toán xử lý việc này một cách an toàn vì việc sắp xếp và cập nhật Fenwick không phụ thuộc vào tính duy nhất; mỗi hình chữ nhật được chèn độc lập và được tính theo mức tăng tần số. 

Một trường hợp khác là khi truy vấn nằm chính xác trên một ranh giới hình chữ nhật. Vì cả x2 ≤ x và y2 ≤ y đều được sử dụng với các phép so sánh không nghiêm ngặt thông qua nén tọa độ và tổng tiền tố, nên các hình chữ nhật biên được đưa vào một cách chính xác. 

Cuối cùng, khi tất cả các hình chữ nhật nằm ngoài phạm vi truy vấn tối đa, cây Fenwick vẫn trống trong tất cả các truy vấn, tạo ra số lượng bằng 0 một cách nhất quán mà không cần xử lý đặc biệt.
