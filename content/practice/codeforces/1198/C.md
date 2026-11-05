---
title: "CF 1198C - Bộ So khớp và Bộ Độc lập"
description: "Chúng ta có một số đồ thị vô hướng, mỗi đồ thị có đúng $3n$ đỉnh. Đối với mỗi biểu đồ, chúng ta phải xây dựng một trong hai cấu trúc cụ thể có kích thước $n$: hoặc một kết hợp bao gồm các cạnh $n$ hoặc một tập hợp độc lập bao gồm các đỉnh $n$."
date: "2026-06-13T14:43:22+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1198
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 576 (Div. 1)"
rating: 2000
weight: 1198
solve_time_s: 404
verified: false
draft: false
---

[CF 1198C - Bộ so khớp và bộ độc lập](https://codeforces.com/problemset/problem/1198/C) 

**Xếp hạng:** 2000 
**Tags:** thuật toán xây dựng, đồ thị, tham lam, sắp xếp 
**Thời gian giải:** 6 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số đồ thị vô hướng, mỗi đồ thị có chính xác$3n$đỉnh. Đối với mỗi biểu đồ, chúng ta phải xây dựng một trong hai cấu trúc kích thước cụ thể$n$: hoặc một kết quả phù hợp bao gồm$n$các cạnh hoặc một tập hợp độc lập bao gồm$n$đỉnh. Nếu không có cấu trúc nào tồn tại, chúng tôi phải báo cáo là không thể thực hiện được. 

Sự so khớp là một tập hợp các cạnh trong đó không có đỉnh nào xuất hiện ở nhiều hơn một cạnh được chọn. Một tập hợp độc lập là một tập hợp các đỉnh sao cho không có cạnh nào trong đồ thị nối bất kỳ cặp đỉnh nào đã chọn. 

Khó khăn chính là đồ thị có kích thước lớn và các ràng buộc thưa thớt không được đảm bảo:$n$có thể lên đến$10^5$cho mỗi trường hợp thử nghiệm và tổng số cạnh lên tới$5 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê các tập hợp con của các đỉnh hoặc các cạnh, hoặc thậm chí bất cứ thứ gì bậc hai trong$n$. Giải pháp về cơ bản phải là tuyến tính hoặc logarit tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi cả hai cấu trúc tồn tại. Ví dụ: trong một biểu đồ trong đó các đỉnh tạo thành nhiều cạnh rời nhau cộng với các nút bị cô lập, cả tập hợp kích thước khớp và độc lập$n$là có thể. Bài toán cho phép một trong hai câu trả lời, do đó mọi chiến lược lựa chọn tất định đều có thể chấp nhận được. 

Một trường hợp cạnh quan trọng khác là khi đồ thị rất dày đặc. Nếu có nhiều cạnh thì có thể dễ dàng tìm thấy các kết quả phù hợp nhưng các tập độc lập sẽ trở nên nhỏ. Ngược lại, nếu các cạnh rất thưa, các tập độc lập sẽ trở nên lớn nhưng cấu trúc khớp có thể không thành công. Giải pháp không được thừa nhận một trong hai chế độ. 

Trường hợp thứ ba không rõ ràng là khi biểu đồ được cấu trúc sao cho không có kích thước phù hợp đầy đủ$n$cũng không phải một tập hợp kích thước độc lập$n$tồn tại. Việc xây dựng trong giải pháp chính thức đảm bảo trường hợp này không bao giờ yêu cầu logic dự phòng phức tạp ngoài việc kiểm tra mâu thuẫn đơn giản. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là thử xây dựng một tập hợp độc lập tối đa và một tập hợp độc lập tối đa một cách độc lập. Người ta có thể chạy khớp tham lam và cũng tính toán một tập độc lập tham lam từ phần bù hoặc cố gắng tính toán một tập hợp độc lập tối đa thông qua DP hoặc rút gọn dựa trên luồng. Những cách tiếp cận này theo cấp số nhân hoặc yêu cầu giải các bài toán con NP-hard, không khả thi ở$n = 10^5$. 

Cái nhìn sâu sắc về cấu trúc quan trọng là biểu đồ có$3n$các đỉnh, nhưng chúng ta chỉ cần một cấu trúc có kích thước$n$, chính xác bằng một phần ba tập đỉnh. Sự mất cân bằng này là rất quan trọng. Nó gợi ý rằng nếu chúng ta không thể tìm thấy đủ các cạnh rời nhau thì nhiều đỉnh phải vẫn “không khớp” theo nghĩa mạnh và những đỉnh này có thể được tổ chức thành một tập độc lập. 

Ý tưởng mang tính xây dựng tiêu chuẩn là cố gắng tìm kiếm sự phù hợp một cách tham lam. Nếu chúng ta có thể chọn$n$các cạnh rời rạc, chúng ta đã hoàn thành. Nếu chúng ta thất bại, sự thất bại ngụ ý rằng có quá nhiều đỉnh bị “chặn” bởi các lựa chọn trước đó, điều này buộc phải tồn tại một tập hợp độc lập lớn. Thuật toán theo dõi cẩn thận các đỉnh được sử dụng và trích xuất cấu trúc trong một lần. 

Quan điểm sâu hơn là mỗi cạnh chúng ta chọn sẽ làm giảm các đỉnh có sẵn và mỗi khi chúng ta không chọn được một cạnh phù hợp, chúng ta sẽ thu được cấu trúc trong biểu đồ phần bù góp phần tạo nên một tập hợp độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(n + m) | Quá chậm | 
| Xây dựng tham lam | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp được xây dựng xoay quanh việc duy trì một quy trình tham lam đơn giản, đồng thời cố gắng xây dựng một kết quả khớp trong khi ngầm chuẩn bị một tập hợp dự phòng độc lập. 

Chúng tôi duy trì một danh sách các đỉnh không được sử dụng và một mảng đã truy cập. Chúng tôi cũng duy trì danh sách lân cận. 

### 1. Tham lam cố gắng xây dựng sự phù hợp 

Chúng tôi lặp qua các cạnh theo thứ tự đầu vào. Đối với mỗi cạnh$(u, v)$, nếu cả hai điểm cuối hiện không được sử dụng, chúng tôi sẽ đưa cạnh này vào kết quả khớp của mình và đánh dấu cả hai đỉnh là đã sử dụng. Chúng tôi tiếp tục cho đến khi chúng tôi thu thập được$n$các cạnh hoặc hoàn thiện tất cả các cạnh. 

Điều này có hiệu quả vì mọi kết quả khớp hợp lệ chỉ yêu cầu các điểm cuối rời rạc và việc tham lam chọn các cạnh sớm sẽ tránh được những xung đột không cần thiết sau này. 

### 2. Nếu khớp thành công 

Nếu chúng ta có thể chọn$n$các cạnh, chúng tôi xuất chúng ngay lập tức. Điều này đáp ứng yêu cầu. 

### 3. Nếu khớp không thành công 

Nếu chúng ta không thể đạt được$n$cạnh thì nhỏ hơn$n$các cạnh rời nhau tồn tại trong đồ thị. Điều này ngụ ý rằng ít nhất$3n - 2(n-1)$các đỉnh vẫn chưa được ghép nối hoàn toàn bởi quy trình tham lam của chúng tôi, quy trình này đảm bảo đủ cấu trúc để tạo thành một tập hợp độc lập. 

Chúng ta xây dựng một tập hợp độc lập bằng cách lấy tất cả các đỉnh chưa bao giờ được sử dụng trong quá trình so khớp. Vì mỗi cạnh được chọn đều có hai đỉnh nên tập còn lại chứa ít nhất$n$đỉnh. Sau đó chúng tôi lấy bất kỳ$n$của họ. 

Chúng ta phải đảm bảo tập hợp này là độc lập. Bất kỳ cạnh nào bên trong tập hợp còn lại này sẽ có sẵn để so khớp trước đó (vì cả hai điểm cuối đều không được sử dụng), mâu thuẫn với tính cực đại của quá trình so khớp tham lam. 

### Tại sao nó hoạt động 

Bất biến là sau khi kết thúc tham lam, không có cạnh nào tồn tại giữa hai đỉnh không sử dụng. Nếu một cạnh như vậy tồn tại thì thuật toán sẽ chọn nó khi xử lý cạnh đó. Do đó, tập hợp các đỉnh không được sử dụng tạo thành một tập hợp độc lập trong đồ thị còn lại. Vì kết quả khớp không đạt được kích thước$n$, bộ chưa sử dụng phải chứa ít nhất$n$đỉnh, đảm bảo tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, m = map(int, input().split())
        adj = [[] for _ in range(3*n + 1)]
        edges = []

        for i in range(1, m + 1):
            u, v = map(int, input().split())
            adj[u].append((v, i))
            adj[v].append((u, i))
            edges.append((u, v))

        used = [False] * (3*n + 1)
        matching = []

        for i, (u, v) in enumerate(edges, 1):
            if not used[u] and not used[v]:
                used[u] = used[v] = True
                matching.append(i)
                if len(matching) == n:
                    break

        if len(matching) == n:
            print("Matching")
            print(*matching)
            continue

        indep = []
        for v in range(1, 3*n + 1):
            if len(indep) < n and not used[v]:
                indep.append(v)

        print("IndSet")
        print(*indep)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã xây dựng thông tin kề và lưu trữ các cạnh theo thứ tự đầu vào. Sau đó, nó thực hiện quét tham lam trên các cạnh, chọn các điểm cuối rời rạc để xây dựng kết quả khớp. khoảnh khắc$n$các cạnh được thu thập, nó sẽ xuất chúng. 

Nếu quá trình tham lam thất bại, nó sẽ thu thập các đỉnh không sử dụng. Chi tiết quan trọng là chúng ta chỉ cần lấy cái đầu tiên$n$các đỉnh không được sử dụng, vì tính chính xác đảm bảo có ít nhất$n$của họ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1 2
1 2
1 3
```| Bước | Cạnh | Đã sử dụng bạn | Đã qua sử dụng v | Kích thước phù hợp | Đỉnh không sử dụng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,2) | 1 | 2 | 1 | {3} | 

Chúng tôi ngay lập tức đạt được kích thước phù hợp 1, vì vậy chúng tôi xuất nó. Điều này cho thấy rằng ngay cả trong một biểu đồ nhỏ, lựa chọn tham lam có thể kết thúc sớm và thỏa mãn yêu cầu. 

### Ví dụ 2 

đầu vào:```
1
1 0
```| Bước | Phù hợp | Đỉnh được sử dụng | Đỉnh không sử dụng | 
| --- | --- | --- | --- | 
| kết thúc | 0 | ∅ | {1,2,3} | 

Không có cạnh nào tồn tại nên việc so khớp không thành công. Chúng ta lấy bất kỳ đỉnh nào chưa được sử dụng làm một tập hợp độc lập. Vì không tồn tại cạnh nào nên mọi tập đỉnh đều độc lập. 

Trường hợp này xác nhận rằng cấu trúc dự phòng hoạt động ngay cả trong các biểu đồ hoàn toàn trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi cạnh được xử lý một lần, mỗi đỉnh được quét một lần | 
| Không gian |$O(n + m)$| danh sách kề và mảng kế toán | 

Tổng số ràng buộc trên tất cả các trường hợp thử nghiệm là$10^5$đỉnh và$5 \cdot 10^5$các cạnh, do đó, một đường tuyến tính duy nhất trên mỗi biểu đồ vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline
    T = int(input())
    out = []

    for _ in range(T):
        n, m = map(int, input().split())
        edges = []
        for i in range(1, m+1):
            u, v = map(int, input().split())
            edges.append((u, v, i))

        used = [False]*(3*n+1)
        match = []

        for u, v, i in edges:
            if not used[u] and not used[v]:
                used[u] = used[v] = True
                match.append(i)
                if len(match) == n:
                    break

        if len(match) == n:
            out.append("Matching")
            out.append(" ".join(map(str, match)))
        else:
            ind = []
            for v in range(1, 3*n+1):
                if not used[v] and len(ind) < n:
                    ind.append(v)
            out.append("IndSet")
            out.append(" ".join(map(str, ind)))

    return "\n".join(out)

# provided samples
assert run("""4
1 2
1 3
1 2
1 2
1 3
1 2
2 5
1 2
3 1
1 4
5 1
1 6
2 15
1 2
1 3
1 4
1 5
1 6
2 3
2 4
2 5
2 6
3 4
3 5
3 6
4 5
4 6
5 6
""") == """Matching
2
IndSet
1
IndSet
2 4
Matching
1 15"""

# custom cases
assert run("""1
1 0
""") == """IndSet
1"""

assert run("""1
1 1
1 2
""") in ["Matching\n1", "IndSet\n1"]  # either valid depending on greedy order

assert run("""1
2 3
1 2
3 4
5 6
""") == """Matching
1 2"""

assert run("""1
2 0
""") == """IndSet
1 2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị trống | Bộ Ấn Độ | tính chính xác dự phòng | 
| cạnh đơn | So khớp/IndSet | xử lý cà vạt | 
| các cạnh rời rạc | Phù hợp | sự ổn định tham lam | 
| không có cạnh | Bộ Ấn Độ | hành vi dự phòng đầy đủ | 

## Vỏ cạnh 

Trong đồ thị không có cạnh, thuật toán không bao giờ chọn cạnh phù hợp và kết thúc với tất cả các đỉnh không được sử dụng. Việc xây dựng sau đó chỉ cần thực hiện đầu tiên$n$các đỉnh, tạo thành một tập hợp độc lập hợp lệ vì tính độc lập được đảm bảo bằng việc không có cạnh. 

Trong một biểu đồ có các cạnh tồn tại nhưng tập trung nhiều xung quanh một vài đỉnh, việc so khớp tham lam có thể bị đình trệ sớm. Ví dụ: nếu một đỉnh kết nối với hầu hết các đỉnh khác, việc chọn bất kỳ cạnh sự cố nào sẽ nhanh chóng chặn các kết quả phù hợp trong tương lai liên quan đến đỉnh đó, nhưng vẫn để lại một lượng lớn các đỉnh chưa được chạm tới. Các đỉnh không được chạm tới đó tạo thành một tập hợp độc lập hợp lệ vì bất kỳ cạnh bên trong nào cũng sẽ được chọn trước đó làm ứng cử viên phù hợp. 

Trong một biểu đồ có thể khớp hoàn hảo, lựa chọn tham lam có thể tìm thấy hoặc không tìm thấy kết quả khớp hoàn toàn tùy thuộc vào thứ tự cạnh, nhưng đảm bảo rằng nếu không, vẫn còn đủ các đỉnh không được sử dụng để tạo thành một tập hợp kích thước độc lập$n$. Điều này xuất phát từ thực tế là mọi thất bại trong việc mở rộng sự so khớp đều tương ứng với sự dư thừa về cấu trúc làm dịch chuyển khối lượng sang tập phần bù.
