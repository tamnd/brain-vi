---
title: "CF 102222J - Tam giác lồng nhau"
description: "Chúng ta có hai trục quay cố định, (P) và (Q) và (n) các điểm khác (A1,ldots,An). Không có điểm nào khác nằm trên đường thẳng (PQ). Chúng ta muốn một chuỗi các chỉ số (v1,v2,ldots,vk) sao cho mọi điểm (A{v{i+1}}) đều nằm hoàn toàn bên trong tam giác tạo bởi (P,Q,A{vi})."
date: "2026-08-19T00:31:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102222
codeforces_index: "J"
codeforces_contest_name: "2018-2019 ACM-ICPC, China Multi-Provincial Collegiate Programming Contest"
rating: 0
weight: 102222
solve_time_s: 227
verified: true
draft: false
---

[CF 102222J - Tam giác lồng nhau](https://codeforces.com/problemset/problem/102222/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai trục xoay cố định, (P) và (Q) và (n) các điểm khác (A_1,\ldots,A_n). Không có điểm nào khác nằm trên đường thẳng (PQ). Chúng ta muốn một chuỗi các chỉ số (v_1,v_2,\ldots,v_k) sao cho mọi điểm (A_{v_{i+1}}) đều nằm hoàn toàn bên trong tam giác tạo bởi (P,Q,A_{v_i}). 

Mục tiêu đầu tiên là tối đa hóa (k). Trong số tất cả các chuỗi có độ dài tối đa đó, câu trả lời bắt buộc là chuỗi nhỏ nhất về mặt từ điển của các chỉ mục gốc. Mẫu chính thức bao gồm ba trường hợp, với các câu trả lời có độ dài (6), (3) và (1). 

Ràng buộc (n\le 10^5) đã loại trừ mọi thứ gần với thời gian bậc hai đối với một trường hợp lớn. Việc kiểm tra mỗi cặp điểm có chi phí khoảng (n(n-1)/2), tương đương với việc kiểm tra cặp (5\cdot10^9) khi (n=10^5). Tổng số điểm trong tất cả các trường hợp thử nghiệm là (10^6), do đó, ngay cả giải pháp (O(n\log^2 n)) cũng phải được triển khai với các hằng số khá nhỏ. Tọa độ có thể đạt tới (10^9) nên việc so sánh hình học phải chính xác chứ không dựa vào các góc có dấu phẩy động. 

Có hai cách đặc biệt dễ dàng dẫn đến câu trả lời sai. Đầu tiên, các điểm trên cùng một phía của (PQ) phải được xử lý độc lập. Ví dụ,```
1
0 0 10 0
2
5 1
5 -1
```có độ dài câu trả lời (1), không phải (2). Điểm thứ hai nằm ở phía đối diện của (PQ) nên không thể nằm trong tam giác có đỉnh thứ ba là điểm đầu tiên. 

Thứ hai, sự bằng nhau theo hướng góc đại diện cho một điểm trên cạnh tam giác và không được phép. Ví dụ,```
1
0 0 10 0
3
1 1
2 2
3 3
```có câu trả lời```
Case #1: 1
1
```Ba điểm cùng nằm trên một tia đi từ (P). Một LIS không nghiêm ngặt bất cẩn sẽ coi chúng như một dây chuyền, mặc dù mọi điểm sau đều nằm trên ranh giới của tam giác được xác định bởi điểm trước đó. 

Trường hợp góc thứ ba là đường (PQ) không nhất thiết phải nằm ngang. Ví dụ,```
1
0 0 0 10
4
1 5
2 5
-1 5
-2 5
```có chiều dài tối đa (2). Chuỗi bên phải là (2,1) và chuỗi bên trái là (4,3), do đó nghiệm tối đa nhỏ hơn về mặt từ điển là```
Case #1: 2
2
1
```Bất kỳ giải pháp nào dựa trên độ dốc thông thường như (y/x) sẽ cần xử lý đặc biệt đối với hướng thẳng đứng. Sử dụng các sản phẩm chéo sẽ tránh được toàn bộ vấn đề đó. 

## Phương pháp tiếp cận 

Phương pháp quy hoạch động trực tiếp xem xét mọi cặp điểm có thứ tự trên cùng một phía của (PQ). Đối với mỗi điểm bên ngoài có thể có (A_i), chúng tôi kiểm tra mọi điểm bên trong có thể có (A_j), kiểm tra xem (A_j) có nằm hoàn toàn bên trong tam giác (PQA_i) hay không và sử dụng mối quan hệ kết quả làm chuyển tiếp DP. Điều này đúng vì mối quan hệ lồng nhau tạo thành một trật tự tuần hoàn có hướng một khi tọa độ hình học được chuyển đổi thành các cấp phù hợp. Vấn đề là số lượng kiểm tra cặp. Với (n=10^5), các cặp có thể (\frac{n(n-1)}2) đã đưa ra các bài kiểm tra xấp xỉ (5\cdot10^9), trước khi xem xét chi phí của các vị từ hình học. 

Quan sát hữu ích là việc ngăn chặn hình tam giác có thể được mô tả chỉ bằng cách sử dụng các hướng của một điểm khi nhìn từ hai trục quay. Giả sử (A) hoàn toàn nằm trong tam giác (PQB). Khi đó (A) và (B) phải ở cùng một phía với (PQ). Từ (P) tia (PA) nằm giữa (PQ) và (PB). Từ (Q) tia (QA) nằm đúng giữa (QP) và (QB). 

Điều đó biến hình học thành hai quan hệ có trật tự chặt chẽ. Đối với mỗi điểm, chúng ta gán một cấp góc xung quanh (P), được đo từ tia (PQ), và một cấp góc khác xung quanh (Q), được đo từ (QP). Trong một cạnh của (PQ), một điểm có thể được lồng vào bên trong một cạnh khác một cách chính xác khi cả hai cấp của nó đều nhỏ hơn. 

Thứ hạng có thể được tính toán mà không cần góc hoặc dấu phẩy động. Cho hai vectơ (u) và (v), dấu của (u\times v) cho biết vectơ nào đứng đầu theo thứ tự góc bên trong nửa mặt phẳng. Các điểm trên cùng một tia có tích chéo bằng 0 và nhận cùng thứ hạng, thể hiện chính xác trường hợp biên không thể tham gia lồng ghép chặt chẽ. 

Sau khi có được hai hạng, mỗi bên trở thành một bài toán về dãy con tăng chặt hai chiều. Sắp xếp theo hạng đầu tiên và sử dụng cây Fenwick trên hạng thứ hai sẽ cho chuỗi dài nhất trong (O(n\log n)). 

Yêu cầu về từ điển phù hợp một cách tự nhiên với cùng một kết quả DP. Gọi (f[i]) là độ dài của chuỗi tăng dài nhất kết thúc tại điểm (i) trong không gian xếp hạng. Mọi điểm có (f[i]=L) có thể là điểm đầu tiên, ngoài cùng của câu trả lời tối ưu. Trong số tất cả các điểm như vậy tương thích với điểm bên ngoài đã được chọn, chúng ta chỉ cần chọn chỉ số gốc nhỏ nhất. Chúng tôi xử lý (f=L,L-1,\ldots,1), vì vậy mỗi điểm được kiểm tra chính xác một lần trong quá trình tái thiết. 

Hai cạnh của (PQ) được giải độc lập vì không có tam giác nào có thể chứa một điểm ở cạnh đối diện. Chúng tôi lấy kết quả dài hơn và nếu độ dài bằng nhau thì chuỗi có chỉ số đầu tiên nhỏ hơn sẽ nhỏ hơn về mặt từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) kiểm tra cặp | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính hướng của mọi (A_i) so với đường thẳng có hướng (PQ). Dấu của ((Q-P)\times(A_i-P)) xác định hai nửa mặt phẳng mở. Vì không có điểm nào nằm trên (PQ) nên không có trường hợp nào bằng 0. 
2. Sắp xếp các điểm xung quanh (P), sắp xếp riêng hai cạnh theo khoảng cách góc tới tia (PQ). Đối với một bên, việc so sánh sản phẩm chéo là theo chiều kim đồng hồ, và đối với bên kia thì ngược chiều kim đồng hồ. Các tia bằng nhau được nhóm lại với nhau và nhận cùng thứ hạng đầu tiên. 
3. Sắp xếp các điểm giống nhau xung quanh (Q), lần này đo khoảng cách góc từ (QP). Một lần nữa, các tia bằng nhau nhận được cùng hạng thứ hai. 
4. Làm việc từng mặt một. Sắp xếp điểm của nó bằng cách tăng hạng thứ nhất và khi hạng thứ nhất bằng nhau thì giảm hạng thứ hai. Thứ tự ràng buộc giảm dần ngăn cản hai điểm có cùng thứ hạng đầu tiên hình thành quá trình chuyển đổi chuỗi con. Chúng nằm trên cùng một tia từ (P), do đó sự chuyển tiếp như vậy sẽ biểu thị một điểm biên chứ không phải một điểm bên trong nghiêm ngặt. 
5. Quét các điểm theo thứ tự đó và duy trì cây Fenwick có vị trí là hạng thứ hai. Đối với điểm (i), truy vấn tất cả các cấp bậc thứ hai nhỏ hơn chính nó. Nếu giá trị lớn nhất là (x), hãy đặt (f[i]=x+1). Sau đó cập nhật cây Fenwick ở hạng thứ hai của điểm với (f[i]). 
6. Giá trị tối đa (f[i]) là độ sâu lồng ghép tối đa ở phía này. Lưu trữ mọi điểm trong nhóm theo giá trị DP của nó. Những nhóm này sẽ được sử dụng để xây dựng lại câu trả lời nhỏ nhất về mặt từ điển. 
7. Bắt đầu từ giá trị DP tối đa và di chuyển xuống dưới, chọn chỉ số ban đầu nhỏ nhất có hai cấp hoàn toàn nhỏ hơn cấp của điểm đã chọn trước đó. Đối với vị trí đầu tiên không có điểm trước đó, vì vậy hãy chọn chỉ số nhỏ nhất trong số tất cả các điểm có giá trị DP tối đa. 
8. Lặp lại phép tính cho phía bên kia của (PQ). So sánh hai chuỗi kết quả theo độ dài đầu tiên và theo chỉ số đầu tiên của chúng khi độ dài của chúng bằng nhau. 

Tại sao nó hoạt động: đối với hai điểm trên cùng một phía của (PQ), điểm (A) nằm hoàn toàn bên trong tam giác (PQB) chính xác khi tia (PA) nằm giữa (PQ) và (PB), và tia (QA) nằm giữa (QP) và (QB). Hai điều kiện góc chặt chẽ đó chính là hai bất đẳng thức xếp hạng. Do đó, mọi chuỗi lồng hợp lệ đều tương ứng với một chuỗi các cặp xếp hạng tăng dần khi đọc từ trong ra ngoài. Cây Fenwick tính toán chuỗi dài nhất như vậy. Trong quá trình xây dựng lại, một điểm có giá trị DP (d) luôn có một chuỗi (d-1) tiền thân, do đó, việc chọn chỉ mục gốc hợp lệ nhỏ nhất ở mọi cấp độ sẽ bảo toàn độ dài còn lại tối đa có thể trong khi giảm thiểu chỉ số khác biệt sớm nhất. Đó chính xác là sự giảm thiểu từ điển. 

## Giải pháp Python```python
import sys
from functools import cmp_to_key

input = sys.stdin.readline

class FenwickMax:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def update(self, i, value):
        n = self.n
        bit = self.bit
        while i <= n:
            if value > bit[i]:
                bit[i] = value
            i += i & -i

    def query(self, i):
        bit = self.bit
        ans = 0
        while i > 0:
            if bit[i] > ans:
                ans = bit[i]
            i -= i & -i
        return ans

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def solve_side(points, pivot_p, pivot_q, side):
    if not points:
        return []

    px, py = pivot_p
    qx, qy = pivot_q

    # The points are already assigned to one side.
    # Rank 1: angular order around P, starting from P->Q.
    def cmp_p(a, b):
        ax = a[0] - px
        ay = a[1] - py
        bx = b[0] - px
        by = b[1] - py
        c = ax * by - ay * bx

        if c == 0:
            return 0

        # side == 0 means cross(PQ, PA) < 0.
        # side == 1 means cross(PQ, PA) > 0.
        if side == 0:
            return -1 if c < 0 else 1
        return -1 if c > 0 else 1

    points.sort(key=cmp_to_key(cmp_p))

    rank = 0
    first = None

    for p in points:
        if first is None:
            rank = 1
            first = p
            p[4] = rank
            continue

        ax = first[0] - px
        ay = first[1] - py
        bx = p[0] - px
        by = p[1] - py

        if ax * by - ay * bx != 0:
            rank += 1
            first = p

        p[4] = rank

    # Rank 2: angular order around Q, starting from Q->P.
    def cmp_q(a, b):
        ax = a[0] - qx
        ay = a[1] - qy
        bx = b[0] - qx
        by = b[1] - qy
        c = ax * by - ay * bx

        if c == 0:
            return 0

        if side == 0:
            return -1 if c > 0 else 1
        return -1 if c < 0 else 1

    points.sort(key=cmp_to_key(cmp_q))

    rank = 0
    first = None

    for p in points:
        if first is None:
            rank = 1
            first = p
            p[5] = rank
            continue

        ax = first[0] - qx
        ay = first[1] - qy
        bx = p[0] - qx
        by = p[1] - qy

        if ax * by - ay * bx != 0:
            rank += 1
            first = p

        p[5] = rank

    # Strictly increasing rank pairs.
    # For equal rank1, decreasing rank2 prevents equal-rank1 transitions.
    points.sort(key=lambda p: (p[4], -p[5]))

    max_rank2 = rank
    bit = FenwickMax(max_rank2)

    groups = [[]]
    maximum = 0

    for p in points:
        f = bit.query(p[5] - 1) + 1
        p[6] = f

        if f > maximum:
            maximum = f
            groups.extend([[] for _ in range(f - len(groups) + 1)])

        groups[f].append(p)
        bit.update(p[5], f)

    # Reconstruct the lexicographically smallest chain.
    answer = []
    current = None

    for length in range(maximum, 0, -1):
        best = None

        if current is None:
            for p in groups[length]:
                if best is None or p[2] < best[2]:
                    best = p
        else:
            r1 = current[4]
            r2 = current[5]

            for p in groups[length]:
                if p[4] < r1 and p[5] < r2:
                    if best is None or p[2] < best[2]:
                        best = p

        current = best
        answer.append(current[2])

    return answer

def solve():
    t = int(input())
    output = []

    for case_id in range(1, t + 1):
        xP, yP, xQ, yQ = map(int, input().split())
        n = int(input())

        P = (xP, yP)
        Q = (xQ, yQ)

        dx = xQ - xP
        dy = yQ - yP

        right = []
        left = []

        for idx in range(1, n + 1):
            x, y = map(int, input().split())
            c = dx * (y - yP) - dy * (x - xP)

            # p = [x, y, original_id, side, rank1, rank2, dp]
            point = [x, y, idx, 0, 0, 0, 0]

            if c < 0:
                point[3] = 0
                right.append(point)
            else:
                point[3] = 1
                left.append(point)

        ans_right = solve_side(right, P, Q, 0)
        ans_left = solve_side(left, P, Q, 1)

        if len(ans_right) > len(ans_left):
            answer = ans_right
        elif len(ans_left) > len(ans_right):
            answer = ans_left
        else:
            if ans_right[0] < ans_left[0]:
                answer = ans_right
            else:
                answer = ans_left

        output.append(f"Case #{case_id}: {len(answer)}")
        output.extend(map(str, answer))

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào trước tiên tính toán hướng của mọi điểm so với (PQ). Dấu hiệu là đủ vì câu lệnh đảm bảo rằng không có điểm nào nằm chính xác trên đường trục. 

Mỗi điểm lưu trữ tọa độ, chỉ số gốc, cạnh, hai cấp góc và giá trị DP của nó. Số nguyên Python có độ chính xác tùy ý, do đó tích chéo vẫn chính xác ngay cả khi tọa độ có thể đạt tới (10^9). 

Bộ so sánh tùy chỉnh đầu tiên sắp xếp các tia xung quanh (P), trong khi bộ so sánh thứ hai sắp xếp các tia xung quanh (Q). Các bộ so sánh cố tình sử dụng tích chéo thay vì`atan2`. Góc dấu phẩy động có thể phân biệt hầu hết các hướng, nhưng nó không thể đảm bảo thứ tự chính xác cho các hướng hợp lý có độ chênh lệch nhỏ hơn độ chính xác của máy. 

Việc gán thứ hạng so sánh mọi điểm với điểm đầu tiên của nhóm tia bằng hiện tại của nó. Tích chéo bằng 0 có nghĩa là hai vectơ có cùng hướng bên trong nửa mặt phẳng mở có liên quan. Chúng nhận được cùng thứ hạng vì chúng không thể hình thành quá trình chuyển đổi lồng nhau nghiêm ngặt. 

Cây Fenwick chỉ chứa giá trị DP tối đa.`query(r2 - 1)`thực thi sự bất bình đẳng nghiêm ngặt ở hạng thứ hai. Thứ tự hạng hai giảm dần bên trong các nhóm hạng nhất bằng nhau xử lý sự bất bình đẳng nghiêm ngặt khác mà không cần thẻ xử lý nhóm riêng. 

Việc tái thiết có chủ đích hoạt động từ giá trị DP lớn nhất xuống còn một. Điểm có giá trị DP (L) là điểm ngoài cùng, trong khi điểm có giá trị DP (L-1) được đặt bên trong nó. Việc chọn chỉ số ban đầu nhỏ nhất trong số tất cả các ứng cử viên tương thích về mặt hình học sẽ cho chỉ số tiếp theo nhỏ nhất có thể trong khi vẫn giữ nguyên độ dài chuỗi còn lại. 

Không có phép trừ hoặc nhân nào được thực hiện bằng cách sử dụng dấu phẩy động và các số nguyên có độ chính xác tùy ý của Python sẽ loại bỏ vấn đề tràn vốn cần được chăm sóc bằng ngôn ngữ cấp thấp hơn. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên đặc biệt hữu ích vì tất cả sáu điểm đều nằm trên cùng một phía của (PQ) và chúng tạo thành một chuỗi hoàn chỉnh. Đầu ra chính thức là (6,5,4,3,2,1). 

| Điểm | Xếp hạng đầu tiên | Hạng nhì | DP | Tái thiết | 
| --- | --- | --- | --- | --- | 
| (A_1=(5,1)) | 1 | 1 | 1 | được chọn cuối cùng | 
| (A_2=(5,2)) | 2 | 2 | 2 | chọn thứ năm | 
| (A_3=(5,3)) | 3 | 3 | 3 | được chọn thứ tư | 
| (A_4=(6,4)) | 4 | 4 | 4 | được chọn thứ ba | 
| (A_5=(6,5)) | 5 | 5 | 5 | được chọn thứ hai | 
| (A_6=(6,6)) | 6 | 6 | 6 | được chọn đầu tiên | 

Truy vấn Fenwick cho mỗi điểm sẽ xem mọi thứ hạng thứ hai trước đó, do đó giá trị DP trở thành (1,2,3,4,5,6). Quá trình tái thiết bắt đầu tại DP (6), chọn điểm (6), sau đó đến điểm (5) và tiếp tục đi xuống điểm (1). Kết quả chính xác là thứ tự từ ngoài vào trong được yêu cầu. 

Đối với mẫu thứ hai, các trục là (P=(6,6)) và (Q=(0,0)) và chuỗi tối đa là (1,3,2). Ba điểm được chọn nằm trên cùng một phía của đường trục. DP tìm thấy một chuỗi có độ dài (3), trong khi các điểm khác thuộc về phía đối diện hoặc không đạt một trong hai bất đẳng thức xếp hạng nghiêm ngặt. 

| Giai đoạn tái thiết | DP bắt buộc | Chỉ số được chọn | Lý do | 
| --- | --- | --- | --- | 
| Điểm đầu tiên | 3 | 1 | Điểm nhỏ nhất có khả năng bắt đầu chuỗi dài 3 | 
| Điểm thứ hai | 2 | 3 | Điểm tương thích nhỏ nhất với chiều dài còn lại | 
| Điểm thứ ba | 1 | 2 | Điểm tương thích hoàn thành chuỗi | 

Ví dụ thứ hai chứng minh tại sao việc giảm thiểu từ điển không thể đơn giản chọn chỉ mục nhỏ nhất trên toàn cầu. Điểm (1) là lựa chọn đầu tiên tốt nhất, nhưng sau khi sửa xong, lựa chọn tiếp theo phải thỏa mãn mối quan hệ lồng hình học cũng như yêu cầu DP còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Hai loại góc chính xác, xử lý xếp hạng, Fenwick DP và tái cấu trúc tuyến tính | 
| Không gian | (O(n)) | Kho lưu trữ điểm, cây Fenwick và thùng DP | 

Trường hợp lớn nhất chứa (10^5) điểm và tổng của tất cả các trường hợp là (10^6). Thuật toán thực hiện số phép toán logarit trên mỗi điểm cho các giai đoạn sắp xếp và Fenwick, trong khi tất cả các bước tái cấu trúc và gán thứ hạng đều là tuyến tính. Việc sử dụng bộ nhớ là tuyến tính theo số điểm trong trường hợp thử nghiệm hiện tại. 

## Trường hợp thử nghiệm```python
# This test block assumes the solve() function from the solution above
# has already been defined.

import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Official samples.
sample = """\
3
0 0 10 0
6
5 1
5 2
5 3
6 4
6 5
6 6
6 6
0 0
9
1 6
2 3
4 7
6 8
8 2
9 3
7 6
2 4
2 7
0 10 10 0
9
0 0
0 2
2 0
0 4
4 0
0 6
6 0
0 8
8 0
"""

expected_sample = """\
Case #1: 6
6
5
4
3
2
1
Case #2: 3
1
3
2
Case #3: 1
1
"""

assert run(sample) == expected_sample, "official samples"

# Minimum-size input.
assert run("""\
1
0 0 10 0
1
5 1
""") == """\
Case #1: 1
1
""", "minimum n"

# Equal ray from P: every point is on a boundary ray, so no pair can nest.
assert run("""\
1
0 0 10 0
3
1 1
2 2
3 3
""") == """\
Case #1: 1
1
""", "equal ray must remain strict"

# Both sides have a chain of length 2.
# The two maximum solutions are [2, 1] and [4, 3],
# so lexicographic order chooses [2, 1].
assert run("""\
1
0 0 10 0
4
5 1
5 2
5 -1
5 -2
""") == """\
Case #1: 2
2
1
""", "tie between the two sides"

# Vertical PQ. This catches implementations that rely on ordinary slopes.
assert run("""\
1
0 0 0 10
4
1 5
2 5
-1 5
-2 5
""") == """\
Case #1: 2
2
1
""", "vertical pivot line"

# Maximum-size case with a deliberately simple answer.
# All 100000 points lie on the same ray from P, so the answer is still 1.
points = "\n".join(f"{i} {i}" for i in range(1, 100001))
max_case = "1\n0 0 1 0\n100000\n" + points + "\n"

assert run(max_case) == """\
Case #1: 1
1
""", "n = 100000"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (P=(0,0),Q=(10,0),A_1=(5,1)) |`Case #1: 1 / 1`| Kích thước đầu vào tối thiểu | 
| Ba điểm ((1,1),(2,2),(3,3)) |`Case #1: 1 / 1`| Các cấp bậc góc cạnh bằng nhau và sự nghiêm ngặt | 
| Hai chuỗi ở hai phía đối diện |`Case #1: 2 / 2 / 1`| Tách bên và ràng buộc từ vựng | 
| Dọc (PQ) |`Case #1: 2 / 2 / 1`| Xử lý định hướng không phân chia | 
| (100000) điểm trên một tia |`Case #1: 1 / 1`| Hành vi tối đa (n) và bộ nhớ tuyến tính | 

## Vỏ cạnh 

Đối với các điểm nằm ở phía đối diện của (PQ), thuật toán sẽ đặt chúng vào các mảng khác nhau trước khi thực hiện bất kỳ DP nào. Vì```
1
0 0 10 0
2
5 1
5 -1
```điểm đầu tiên chỉ nhận được thứ hạng trong tính toán phía trên và điểm thứ hai chỉ nhận được thứ hạng trong tính toán phía dưới. Mỗi bên tạo ra một chuỗi có độ dài (1), vì vậy câu trả lời cuối cùng là```
Case #1: 1
1
```Việc so sánh không bao giờ tạo ra sự chuyển tiếp giữa hai cạnh, phù hợp với hình học vì một điểm trong của tam giác (PQA) phải nằm cùng một phía của (PQ) với (A). 

Đối với các tia bằng nhau, hãy xem xét```
1
0 0 10 0
3
1 1
2 2
3 3
```Cả ba điểm đều có cùng hạng góc thứ nhất quanh (P). Do đó, chúng được xử lý bên trong một nhóm xếp hạng bằng nhau và thứ tự xếp hạng thứ hai giảm dần sẽ ngăn cản một nhóm mở rộng nhóm khác. Mỗi giá trị DP là (1), do đó việc xây dựng lại chọn chỉ số gốc nhỏ nhất, tạo ra```
Case #1: 1
1
```Đây là trường hợp ranh giới nghiêm ngặt bắt được LIS không nghiêm ngặt thông thường. 

Đối với đường trục đứng,```
1
0 0 0 10
4
1 5
2 5
-1 5
-2 5
```thuật toán không bao giờ tính toán độ dốc như (y/x). Nó so sánh các vectơ bằng cách sử dụng tích chéo, do đó hướng dọc của (PQ) không yêu cầu nhánh đặc biệt. Ở mỗi bên, điểm xa hơn (PQ) là điểm bên ngoài, tạo thành hai chuỗi có thể có (2,1) và (4,3). Vì cả hai đều có độ dài (2), chỉ số đầu tiên quyết định câu trả lời, mang lại```
Case #1: 2
2
1
```Cuối cùng, bài kiểm tra (n=100000) đặt mọi điểm trên một tia. Các cấp bậc góc được chia thành một nhóm xếp hạng đầu tiên, vì vậy Fenwick DP không bao giờ tạo ra một chuỗi dài hơn một. Thuật toán vẫn chỉ thực hiện việc sắp xếp và chuyển tuyến tính cũng như đầu ra của nó```
Case #1: 1
1
```Ví dụ này cũng xác nhận tại sao việc lưu trữ các nhóm hướng chính xác lại quan trọng ngay cả khi đầu vào chứa nhiều điểm có hướng hình học rất giống nhau.
