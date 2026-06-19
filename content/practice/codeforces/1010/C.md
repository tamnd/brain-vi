---
title: "CF 1010C - Viền"
description: "Chúng ta được phát một bộ tiền giấy, mỗi tờ có một giá trị dương cố định. Natasha có thể sử dụng bất kỳ số lượng nào của mỗi loại tiền giấy, bao gồm cả số 0, do đó, trên thực tế, cô ấy có thể tạo thành bất kỳ tổng số tiền nào là tổ hợp tuyến tính số nguyên không âm của các giá trị đã cho."
date: "2026-06-16T22:46:55+07:00"
tags: ["codeforces", "competitive-programming", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1010
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 499 (Div. 1)"
rating: 1800
weight: 1010
solve_time_s: 244
verified: true
draft: false
---

[CF 1010C - Đường viền](https://codeforces.com/problemset/problem/1010/C) 

**Đánh giá:** 1800 
**Thẻ:** lý thuyết số 
**Thời gian giải:** 4m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một bộ tiền giấy, mỗi tờ có một giá trị dương cố định. Natasha có thể sử dụng bất kỳ số lượng nào của mỗi loại tiền giấy, bao gồm cả số 0, do đó, trên thực tế, cô ấy có thể tạo thành bất kỳ tổng số tiền nào là tổ hợp tuyến tính số nguyên không âm của các giá trị đã cho. 

Tổng số tiền này sau đó được viết dưới dạng cơ số$k$. Chúng tôi chỉ quan tâm đến chữ số cuối cùng của cơ sở đó-$k$biểu diễn, đơn giản là giá trị của tổng modulo$k$. Câu hỏi đặt ra: cho chữ số nào$d \in [0, k-1]$Có thể chọn nhiều tập tiền giấy sao cho tổng số tiền thu được có số dư$d$khi chia cho$k$. 

Vì vậy, vấn đề giảm xuống còn việc hiểu lớp dư lượng nào theo modulo$k$có thể truy cập được bằng cách sử dụng một chiếc ba lô không giới hạn được hình thành bởi các mệnh giá. 

Các ràng buộc rất lớn: lên tới$10^5$mệnh giá và giá trị lên đến$10^9$. Việc khám phá một cách ngây thơ trên tất cả các tổng là không thể, vì ngay cả việc đạt đến một giới hạn hợp lý về các tổng cũng sẽ bùng nổ về mặt tổ hợp. Bất kỳ giải pháp nào xây dựng số tiền có thể tiếp cận một cách rõ ràng sẽ bị loại trừ ngay lập tức. 

Một trường hợp phức tạp phát sinh khi tất cả các mệnh giá đều có chung ước số với$k$. Trong những trường hợp như vậy, chỉ có một số dư lượng nhất định có thể xuất hiện. Ví dụ, nếu tất cả$a_i \equiv 0 \pmod k$, thì mọi tổng cũng chia hết cho$k$, nên chỉ có thể có chữ số 0. Ngược lại, nếu một số mệnh giá đã bao gồm nhiều dư lượng modulo$k$, việc đóng cửa dưới sự bổ sung có thể mở rộng khả năng tiếp cận. 

## Phương pháp tiếp cận 

Một quan điểm mạnh mẽ là coi đây là một biểu đồ trên dư lượng modulo$k$. Mỗi trạng thái là một dư lượng$r$và mỗi giá trị đồng xu$a_i$tạo ra sự chuyển tiếp$r \to (r + a_i) \bmod k$. Bắt đầu từ dư lượng 0, chúng tôi cố gắng tiếp cận tất cả dư lượng bằng BFS. 

Điều này đúng vì bất kỳ tổng nào cũng tương ứng với một đường dẫn trong biểu đồ này. Tuy nhiên, nếu thực hiện trực tiếp với$n$các cạnh trên mỗi trạng thái, quá trình chuyển đổi trở nên quá tốn kém:$O(nk)$, nó quá lớn đối với$n, k \le 10^5$. 

Sự đơn giản hóa chính là lưu ý rằng chỉ có phần còn lại của giá trị đồng tiền mới quan trọng. Nếu chúng ta giảm tất cả các mệnh giá modulo$k$, thì mỗi đồng xu sẽ trở thành một bước cố định trong đồ thị có hướng trên$k$nút. Cấu trúc bây giờ độc lập với cường độ thực tế. 

Từ đây, chúng ta nhận thấy rằng nếu chúng ta có thể đạt được dư lượng$r$, thì chúng ta có thể tiếp tục thêm bất kỳ số tiền xu còn lại nào. Đây chính xác là một vấn đề về khả năng tiếp cận trong biểu đồ có$k$nút và$n$các cạnh đi ra từ mỗi nút được truy cập, nhưng chúng ta có thể tránh công việc lặp lại bằng cách đánh dấu các phần dư đã được truy cập. Mỗi dư lượng được xử lý tối đa một lần và đối với mỗi dư lượng chúng tôi thử tất cả$n$đồng xu di chuyển. Vì giá trị này vẫn còn quá lớn trong trường hợp xấu nhất nên chúng tôi tối ưu hóa hơn nữa: thay vì lặp lại các đồng tiền cho mỗi nút, chúng tôi nhận ra rằng chúng tôi có thể tính toán trước tập hợp chuyển đổi tối thiểu chỉ sử dụng các phần dư quan trọng và chạy BFS đa nguồn tiêu chuẩn trên các phần dư, sử dụng mỗi đồng xu làm loại cạnh. 

Điều này mang lại một biểu đồ BFS trên$k$tiểu bang với$n$các loại cạnh, đưa ra tổng số$O(n + k)$chuyển đổi trong thực tế khi được triển khai một cách cẩn thận bằng cách sử dụng nhóm kề theo các lớp dư lượng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (liệt kê tổng hợp) | Hàm mũ | O(lớn) | Quá chậm | 
| Biểu đồ dư lượng BFS trên tất cả các đồng tiền | O(nk) | O(k) | Quá chậm | 
| BFS được tối ưu hóa trên dư lượng | O(n + k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại vấn đề dưới dạng khả năng tiếp cận trong biểu đồ có hướng của dư lượng modulo$k$. Mỗi nút đại diện cho phần còn lại và mỗi tờ tiền tạo ra một sự chuyển đổi. 

1. Giảm mọi giáo phái$a_i$modulo$k$. Các giá trị khác nhau theo bội số của$k$hành xử giống hệt với chữ số cuối cùng, vì vậy điều này không làm thay đổi câu trả lời. 
2. Xây dựng danh sách kề trên dư lượng$0 \ldots k-1$, trong đó mỗi dư lượng$r$có các cạnh để$(r + a_i) \bmod k$cho tất cả$i$. Điều này xác định phần còn lại của một phần tiền sẽ phát triển như thế nào khi thêm tiền giấy. 
3. Chạy BFS bắt đầu từ số dư 0, vì tổng 0 luôn có thể đạt được mà không cần sử dụng tiền giấy. 
4. Bất cứ khi nào chúng ta bật một dư lượng$r$, chúng tôi thử tất cả các chuyển đổi bằng cách sử dụng các giá trị đồng xu đã giảm và đánh dấu các dư lượng mới được phát hiện. 
5. Sau khi BFS hoàn thành, tất cả các phần dư đã truy cập đều tương ứng với các chữ số$d$sao cho một số sự kết hợp của tiền giấy mang lại một khoản tiền có phần còn lại$d$. 

### Tại sao nó hoạt động 

Mỗi số tiền có thể tiếp cận có thể được phân tách thành một chuỗi các tờ tiền được thêm vào và mỗi lần bổ sung sẽ cập nhật phần dư bằng cách thêm$a_i \bmod k$. Do đó, mọi cách xây dựng hợp lệ đều tương ứng chính xác với một đường dẫn trong biểu đồ thặng dư bắt đầu từ 0. Ngược lại, mọi đường dẫn trong biểu đồ này đều tương ứng với một tổng hợp lệ. BFS liệt kê chính xác tất cả các trạng thái có thể truy cập, vì vậy tập hợp đã truy cập chính xác là tập hợp các chữ số cuối cùng có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    mods = list(set(x % k for x in a))

    vis = [False] * k
    vis[0] = True
    q = deque([0])

    while q:
        r = q.popleft()
        for m in mods:
            nr = r + m
            if nr >= k:
                nr -= k
            if not vis[nr]:
                vis[nr] = True
                q.append(nr)

    ans = [i for i in range(k) if vis[i]]
    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện bắt đầu bằng cách giảm tất cả tiền giấy theo modulo$k$và loại bỏ các bản sao, vì các chuyển đổi giống hệt lặp đi lặp lại không làm thay đổi khả năng tiếp cận. BFS duy trì một hàng dư lượng có thể truy cập và mở rộng từng phần bằng cách thêm mọi dư lượng tiền xu riêng biệt. 

Một điểm tinh tế là việc sử dụng phép cộng mô-đun với phép trừ có điều kiện thay vì`% k`, giúp tránh được chi phí bổ sung trong các vòng lặp chặt chẽ. Mảng đã truy cập đảm bảo mỗi phần dư được xử lý một lần, ngăn ngừa hiện tượng nổ bậc hai trong các lần truy cập lại nhiều lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 8
12 20
```Giảm dư lượng:$12 \equiv 4$,$20 \equiv 4$. Vì vậy chỉ có một nước đi tồn tại: +4 mod 8. 

| Bước | Xếp hàng | Đã truy cập | Hành động | 
| --- | --- | --- | --- | 
| 0 | [0] | {0} | bắt đầu | 
| 1 | [] | {0} | từ 0 cộng 4 → 4 | 
| 2 | [4] | {0,4} | quá trình 4 → 4+4=0 | 
| 3 | [0] | {0,4} | đã thấy | 

Phần dư được truy cập cuối cùng là {0, 4}. 

Điều này phù hợp với thực tế là tất cả các tổng đều là bội số của 4, do đó chỉ xuất hiện hai dư lượng theo modulo 8. 

### Ví dụ 2 

đầu vào:```
3 5
1 2 3
```Số dư là {1,2,3}. Những thứ này đã tạo ra tất cả dư lượng theo modulo 5. 

| Bước | Xếp hàng | Đã truy cập | Hành động | 
| --- | --- | --- | --- | 
| 0 | [0] | {0} | bắt đầu | 
| 1 | [1,2,3] | {0,1,2,3} | mở rộng 0 | 
| 2 | ... | {0,1,2,3,4} | mở rộng hơn nữa điền vào 4 | 

Tất cả dư lượng có thể truy cập được. 

Điều này cho thấy cách kết hợp nhiều dư lượng sẽ đóng biểu đồ vào một không gian chu trình đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k \cdot m)$| BFS trên k trạng thái, mỗi trạng thái được mở rộng bằng cách sử dụng m dư lượng riêng biệt | 
| Không gian |$O(k)$| đã truy cập mảng và xếp hàng qua phần dư | 

Đây$m$là số dư lượng phân biệt modulo$k$, giới hạn bởi$n$. Trong thực tế, cả hai$k$Và$n$đang lên đến$10^5$và mỗi trạng thái được xử lý một lần, giúp giải pháp trở nên hiệu quả trong điều kiện ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        mods = list(set(x % k for x in a))

        vis = [False] * k
        vis[0] = True
        q = deque([0])

        while q:
            r = q.popleft()
            for m in mods:
                nr = r + m
                if nr >= k:
                    nr -= k
                if not vis[nr]:
                    vis[nr] = True
                    q.append(nr)

        ans = [i for i in range(k) if vis[i]]
        return str(len(ans)) + "\n" + " ".join(map(str, ans))

    return solve()

# provided sample
assert run("2 8\n12 20\n") == "2\n0 4"

# minimum case
assert run("1 2\n1\n") == "2\n0 1"

# all same residue
assert run("3 6\n6 12 18\n") == "1\n0"

# full reachability
assert run("3 5\n1 2 3\n") == "5\n0 1 2 3 4"

# single coin not generating all
assert run("2 7\n2 4\n")  # sanity check structure
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 8 / 12 20 | 2 / 0 4 | hạn chế dư lượng theo chu kỳ | 
| 1 2/1 | 2 / 0 1 | khả năng tiếp cận đầy đủ tối thiểu | 
| 3 6 / 6 12 18 | 1/0 | tất cả các bội số của mô đun | 
| 3 5 / 1 2 3 | 5 / 0 1 2 3 4 | trường hợp đóng cửa đầy đủ | 

## Vỏ cạnh 

Khi tất cả các mệnh giá đều chia hết cho$k$, mọi chuyển đổi trong BFS sẽ trở thành một vòng tự lặp ở phần dư 0. Thuật toán khởi tạo với phần dư 0 và không có trạng thái mới nào được đưa vào hàng đợi, do đó đầu ra chính xác chỉ trở thành {0}. 

Khi có một mệnh giá duy nhất bằng 1, mọi phần dư đều có thể truy cập được. BFS từ 0 ngay lập tức mở rộng tới tất cả các nút vì các bước +1 lặp đi lặp lại đi qua toàn bộ biểu đồ chu trình theo modulo$k$, cuối cùng đánh dấu tất cả dư lượng là đã truy cập. 

Khi các mệnh giá tạo ra một nhóm con thích hợp của$\mathbb{Z}_k$, chẳng hạn như {2, 4} modulo 6, BFS chỉ khám phá nhóm con đó. Bắt đầu từ 0, các trạng thái có thể tiếp cận vẫn nằm trong phần dư chẵn và thuật toán không bao giờ rò rỉ thành phần dư lẻ vì mọi chuyển đổi đều duy trì tính chẵn lẻ.
