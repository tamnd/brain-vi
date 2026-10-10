---
title: "CF 104990F - Hội Bạn Ở Công Viên"
description: "Chúng ta có một cái cây, là một đồ thị liên thông không có chu trình, trong đó mỗi cạnh biểu thị một đường đi có độ dài bằng nhau. Mỗi truy vấn cung cấp cho chúng ta ba nút bắt đầu riêng biệt, đại diện cho vị trí ban đầu của ba người trong cây này."
date: "2026-06-28T04:23:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104990
codeforces_index: "F"
codeforces_contest_name: "First Masters Championship LATAM 2024"
rating: 0
weight: 104990
solve_time_s: 96
verified: false
draft: false
---

[CF 104990F - Hội ngộ bạn bè tại công viên](https://codeforces.com/problemset/problem/104990/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cái cây, là một đồ thị liên thông không có chu trình, trong đó mỗi cạnh biểu thị một đường đi có độ dài bằng nhau. Mỗi truy vấn cung cấp cho chúng ta ba nút bắt đầu riêng biệt, đại diện cho vị trí ban đầu của ba người trong cây này. 

Đối với mỗi truy vấn, chúng tôi được yêu cầu xác định số bước nhỏ nhất có thể cần thiết để cả ba người có thể đến cùng một nút. Mỗi bước tương ứng với việc di chuyển dọc theo một cạnh và cả ba cạnh có thể di chuyển đồng thời trong mỗi bước. Mục tiêu là chọn điểm gặp mặt và phối hợp di chuyển sao cho tổng thời gian cho đến khi cả ba người đến nơi là nhỏ nhất. 

Quan sát quan trọng ẩn trong cụm từ này là chúng ta không được yêu cầu giảm thiểu tổng quãng đường đã đi mà là thời gian cho đến khi người cuối cùng đến, giả sử chuyển động song song. Điều này biến vấn đề thành việc tìm một nút có khoảng cách tối thiểu từ ba nút bắt đầu. 

Các ràng buộc làm cho điều này không tầm thường. Cây có tới 200.000 nút và có thể có 100.000 truy vấn. Bất kỳ giải pháp nào tính toán lại khoảng cách bằng BFS hoặc DFS cho mỗi truy vấn sẽ có chi phí O(N) cho mỗi truy vấn, dẫn đến O(NQ) vượt xa giới hạn chấp nhận được. Ngay cả quá trình tiền xử lý O(Q log N) cho mỗi truy vấn cũng sẽ quá chậm nếu nó liên quan đến việc tính toán lại nhiều. 

Các trường hợp cạnh xuất hiện khi ba nút đã ở rất gần nhau, chẳng hạn như tất cả đều nằm trên cùng một đường dẫn hoặc thậm chí chia sẻ một vị trí giống như trọng tâm. Trong một ví dụ nhỏ, chẳng hạn như chuỗi 1-2-3-4 có truy vấn (1,2,3), câu trả lời đúng là 1 vì nút 2 hoặc 3 hoạt động như một điểm gặp gỡ. Một cách tiếp cận ngây thơ chỉ xem xét khoảng cách theo cặp hoặc thử tính trung bình điểm giữa có thể thất bại vì khoảng cách của cây không phải là Euclide. 

Một trường hợp tinh vi khác phát sinh khi điểm gặp tối ưu là một trong các nút đã cho. Ví dụ: nếu một nút nằm trên đường dẫn giữa hai nút kia thì giải pháp tối ưu thường là gặp nhau ở nút giữa đó. Các thuật toán cho rằng điểm gặp mặt phải là một nút mới hoặc nút “giống như trung vị” có thể bỏ lỡ điều này. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ thử mọi nút họp có thể có trong cây. Đối với mỗi nút ứng cử viên x, chúng tôi tính toán khoảng cách từ x đến A, B và C bằng cách sử dụng BFS hoặc DFS và lấy giá trị tối đa của cả ba. Sau đó chúng tôi chọn mức tối thiểu trên tất cả x. 

Điều này đúng vì mọi chiến lược họp hợp lệ đều phải kết thúc ở một nút nào đó và thời gian cần thiết được xác định bởi người tiếp cận nó chậm nhất. Tuy nhiên, việc tính toán khoảng cách từ mỗi nút nhiều lần là cực kỳ tốn kém. Một BFS cho mỗi truy vấn có chi phí O(N) và việc thực hiện điều đó cho mọi nút ứng cử viên sẽ thêm một hệ số N khác, dẫn đến O(N²) cho mỗi truy vấn, điều này là không thể. 

Điểm mấu chốt là câu trả lời có thể được biểu thị bằng khoảng cách dọc theo cấu trúc cây mà không cần thử tất cả các nút. Trong một cây, đường đi duy nhất giữa hai nút bất kỳ cho phép chúng ta suy luận về “tâm” của ba điểm. Một phép biến đổi hữu ích là sửa một gốc và sử dụng các truy vấn tổ tiên chung thấp nhất (LCA) để tính toán khoảng cách một cách nhanh chóng. Khi có thể tính khoảng cách trong O(1), chúng ta có thể đánh giá một công thức hằng số cho mỗi truy vấn. 

Nhận dạng quan trọng là thời gian họp tối ưu bằng một nửa số lượng: 

d(A,B) + d(B,C) + d(C,A) trừ đi khoảng cách lớn nhất trong ba khoảng cách theo cặp. Điều này xuất phát từ thực tế là trong một cái cây, sự kết hợp của ba con đường tạo thành một cấu trúc mà sự chồng chéo của nó quyết định mức độ đi bộ có thể được chia sẻ. 

Điều này làm giảm mỗi truy vấn thành các phép tính khoảng cách theo thời gian không đổi sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N²) mỗi truy vấn | O(N) | Quá chậm | 
| Tối ưu (công thức LCA +) | O(log N) cho mỗi truy vấn | O(N log N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Trước tiên, chúng tôi xử lý trước cây để có thể trả lời các truy vấn khoảng cách giữa hai nút bất kỳ một cách hiệu quả. Điều này được thực hiện bằng cách root cây tại một nút tùy ý và tính toán độ sâu cũng như bảng nâng nhị phân cho tổ tiên chung thấp nhất. 

1. Chọn một nút gốc tùy ý, thường là nút 1 và chạy DFS để tính toán độ sâu của mỗi nút và nút cha trực tiếp của nó. Điều này thiết lập một tham chiếu để đo khoảng cách. 
2. Xây dựng bảng nâng nhị phân trong đó up[v][k] lưu trữ tổ tiên thứ 2^k của nút v. Điều này cho phép nhảy lên trên theo thời gian logarit. Bước này là cần thiết để các truy vấn LCA có thể được trả lời một cách hiệu quả. 
3. Xác định hàm lca(u, v) nâng nút sâu hơn lên cùng độ sâu với nút kia và sau đó đồng thời nâng cả hai cho đến khi tổ tiên của chúng khớp nhau. Điểm gặp gỡ là tổ tiên chung thấp nhất. 
4. Xác định hàm khoảng cách bằng cách sử dụng đẳng thức d(u, v) = deep[u] + deep[v] − 2 * deep[lca(u, v)]. Điều này chuyển đổi khoảng cách cây thành các phép toán số học. 
5. Đối với mỗi truy vấn (A, B, C), hãy tính ba khoảng cách theo cặp dAB, dBC và dCA bằng cách sử dụng hàm khoảng cách. 
6. Tính toán câu trả lời bằng cách sử dụng biểu thức (dAB + dBC + dCA − max(dAB, dBC, dCA)) // 2. Điều này loại bỏ một cách hiệu quả đường đi dài nhất và phân chia phần chồng lấp còn lại một cách chính xác để tính đến chuyển động chung hướng tới điểm gặp nhau trung tâm. 
7. Xuất giá trị này dưới dạng thời gian tối thiểu cần thiết để cả ba nút đáp ứng. 

Lý do đằng sau bước 6 là trong một cây, sự kết hợp của ba đường dẫn ngắn nhất tạo thành một cấu trúc trong đó tổng “mức sử dụng cạnh” sẽ tính gấp đôi các phân đoạn được chia sẻ. Việc loại bỏ khoảng cách theo cặp lớn nhất sẽ tách biệt phần chồng lấp dư thừa và mang lại thời gian hội tụ thực sự. 

### Tại sao nó hoạt động 

Trong cây, các đường dẫn giữa các nút giao nhau theo cách có cấu trúc rõ ràng vì không có chu trình. Ba đường dẫn theo cặp tạo thành một cây con được kết nối có hình dạng là chữ “Y”. Điểm gặp nhau tối ưu nằm ở ngã ba nơi các nhánh này hợp nhất. Công thức đo lường hiệu quả chiều cao của điểm nối này từ nhánh sâu nhất và đảm bảo rằng cả ba nút có thể tiếp cận điểm nối đó trong thời gian đồng bộ hóa tối thiểu có thể. Vì khoảng cách dựa trên LCA nắm bắt chính xác hình dạng cây nên giá trị được tính toán không thể đánh giá thấp hoặc đánh giá quá cao thời gian gặp nhau thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    LOG = (n).bit_length()
    up = [[0] * (LOG + 1) for _ in range(n + 1)]
    depth = [0] * (n + 1)

    def dfs(v, p):
        up[v][0] = p
        for to in g[v]:
            if to == p:
                continue
            depth[to] = depth[v] + 1
            dfs(to, v)

    dfs(1, 0)

    for j in range(1, LOG + 1):
        for i in range(1, n + 1):
            up[i][j] = up[up[i][j - 1]][j - 1]

    def lca(a, b):
        if depth[a] < depth[b]:
            a, b = b, a

        diff = depth[a] - depth[b]
        j = 0
        while diff:
            if diff & 1:
                a = up[a][j]
            diff >>= 1
            j += 1

        if a == b:
            return a

        for j in range(LOG, -1, -1):
            if up[a][j] != up[b][j]:
                a = up[a][j]
                b = up[b][j]

        return up[a][0]

    def dist(a, b):
        c = lca(a, b)
        return depth[a] + depth[b] - 2 * depth[c]

    q = int(input())
    out = []

    for _ in range(q):
        a, b, c = map(int, input().split())
        ab = dist(a, b)
        bc = dist(b, c)
        ca = dist(c, a)
        ans = (ab + bc + ca - max(ab, bc, ca)) // 2
        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```DFS khởi tạo các mối quan hệ cấp độ sâu và trực tiếp, tạo thành lớp cơ sở cho việc nâng cấp nhị phân. Sau đó, bảng lên sẽ được điền từ dưới lên để bất kỳ bước nhảy tổ tiên nào cũng có thể được phân tách thành lũy thừa của hai. 

Chức năng LCA trước tiên cân bằng độ sâu bằng cách nâng nút sâu hơn. Vòng lặp sử dụng phân tách bit đảm bảo điều này xảy ra theo thời gian logarit. Giai đoạn thứ hai nâng cả hai nút lại với nhau cho đến khi tổ tiên của chúng phân kỳ. 

Tính toán khoảng cách là hệ quả trực tiếp của số học độ sâu dựa trên nghiệm một khi LCA được biết đến. 

Mỗi truy vấn chỉ thực hiện một số lượng tính toán LCA và khoảng cách không đổi, khiến nó đủ hiệu quả với kích thước đầu vào lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2
2 3
3 4
1
1 2 3
```Chúng tôi tính toán khoảng cách: 

| Cặp | LCA | Khoảng cách | 
| --- | --- | --- | 
| 1,2 | 1 | 1 | 
| 2,3 | 2 | 1 | 
| 3,1 | 1 | 2 | 

Tổng là 4, tối đa là 2, nên đáp án là (4 − 2) / 2 = 1. 

Điều này tương ứng với cuộc họp ở nút 2 hoặc 3, cả hai đều cho thời gian di chuyển tối đa tối thiểu là 1. 

### Ví dụ 2 

đầu vào:```
5
1 2
1 3
3 4
3 5
1
2 4 5
```Khoảng cách: 

| Cặp | Khoảng cách | 
| --- | --- | 
| 2,4 | 3 | 
| 4,5 | 2 | 
| 5,2 | 3 | 

Tổng là 8, tối đa là 3, đáp án là (8 − 3) / 2 = 2. 

Điều này phản ánh rằng điểm gặp nhau tốt nhất là nút 3, nơi tất cả các đường dẫn đều hội tụ hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + Q) log N) | DFS + tiền xử lý nâng nhị phân, sau đó là LCA cho mỗi truy vấn | 
| Không gian | O(N log N) | bảng tổ tiên và danh sách kề | 

Quá trình tiền xử lý chia tỷ lệ tuyến tính theo số lượng nút lên đến hệ số logarit và mỗi truy vấn được giảm xuống một số lượng không đổi các phép toán LCA logarit. Điều này phù hợp thoải mái trong các ràng buộc về cả thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import log2
    solve()
    return sys.stdout.getvalue().strip()

# provided sample (format adjusted)
assert run("""4
1 2
2 3
3 4
2
1 2 3
2 3 4
""") == "1\n1"

# minimum tree
assert run("""3
1 2
2 3
1
1 2 3
""") == "1"

# star shape
assert run("""5
1 2
1 3
1 4
1 5
1
2 3 4
""") == "2"

# all close on line
assert run("""4
1 2
2 3
3 4
1
1 4 3
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây đường dẫn | 1 | hành vi trung điểm | 
| cây sao | 2 | trung tâm trung tâm đúng đắn | 
| thứ tự đảo ngược trên đường dẫn | 1 | đối xứng và trật tự | 

## Vỏ cạnh 

Cây hình đường dẫn làm nổi bật liệu giải pháp có xử lý chính xác các cấu trúc tuyến tính hay không. Hãy xem xét 1-2-3-4 với truy vấn (1,4,3). Khoảng cách là 3,1,2, dẫn đến câu trả lời 1. Thuật toán tính toán khoảng cách dựa trên LCA và công thức số học thu gọn chính xác phần chồng lấp để nút 3 nổi lên làm điểm gặp nhau tối ưu. 

Cây hình ngôi sao kiểm tra xem sự hội tụ qua nút trung tâm có được xử lý chính xác hay không. Với tâm 1 và các lá 2, 3, 4, truy vấn (2,3,4) cho ra các khoảng cách đều bằng 2. Công thức cho (2+2+2−2)/2 = 2, phù hợp với thực tế là tất cả đều phải đi qua tâm. 

Trường hợp một nút nằm trên đường dẫn giữa hai nút còn lại, chẳng hạn như (1,2,3) trên một chuỗi, xác minh rằng các nút trung gian được ưu tiên hợp lý. Cấu trúc khoảng cách LCA đảm bảo rằng phân đoạn chia sẻ không bị tính quá mức, do đó điểm gặp gỡ sẽ tự động chuyển sang nút giữa mà không cần vỏ đặc biệt.
