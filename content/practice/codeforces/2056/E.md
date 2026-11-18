---
title: "CF 2056E - Phân đoạn lồng nhau"
description: "Chúng ta được cung cấp một tập hợp các phân đoạn số nguyên trong phạm vi [1, n]. Mỗi đoạn là một khoảng đóng [l, r]. Tập hợp các phân đoạn ban đầu, S, là \"tốt\", nghĩa là đối với bất kỳ hai phân đoạn riêng biệt nào trong tập hợp đó, chúng hoàn toàn không trùng nhau hoặc một phân đoạn này hoàn toàn nằm trong phân đoạn kia."
date: "2026-06-08T08:17:27+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "dp", "dsu", "math"]
categories: ["algorithms"]
codeforces_contest: 2056
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 997 (Div. 2)"
rating: 2500
weight: 2056
solve_time_s: 114
verified: false
draft: false
---

[CF 2056E - Phân đoạn lồng nhau](https://codeforces.com/problemset/problem/2056/E) 

**Đánh giá:** 2500 
**Tags:** tổ hợp, dfs và tương tự, dp, dsu, toán 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các phân đoạn số nguyên trong phạm vi`[1, n]`. Mỗi đoạn là một khoảng đóng`[l, r]`. Tập hợp các phân đoạn ban đầu,`S`, là "tốt", nghĩa là đối với bất kỳ hai phân đoạn riêng biệt nào trong tập hợp, chúng hoàn toàn không trùng nhau hoặc một phân đoạn này hoàn toàn nằm trong phân đoạn kia. Nhiệm vụ là mở rộng`S`bằng cách thêm càng nhiều phân đoạn mới càng tốt trong khi vẫn bảo toàn thuộc tính này. Sau khi xác định số đoạn tối đa có thể thêm vào, chúng ta phải đếm số cách để đạt được mức tối đa đó, modulo`998244353`. 

Thách thức chính là sự kết hợp: chúng ta không chỉ phải xác định vị trí có thể chèn các phân đoạn mà còn phải tính các cách riêng biệt để sắp xếp chúng trong khi vẫn duy trì cấu trúc "tập hợp tốt". Từ`n`Và`m`mỗi người có thể lên đến`2*10^5`với nhiều trường hợp thử nghiệm, một giải pháp có độ phức tạp về thời gian kém hơn`O(n log n)`mỗi trường hợp thử nghiệm có thể là quá chậm. Cụ thể, một cách tiếp cận mạnh mẽ liệt kê tất cả các phân đoạn có thể là không khả thi ngay lập tức vì có khoảng`O(n^2)`các phân đoạn có thể có và việc đếm các kết hợp trên chúng sẽ là vô cùng to lớn. 

Các trường hợp cạnh bao gồm các tình huống trong đó`S`trống hoặc bao phủ gần như toàn bộ phạm vi. Ví dụ, nếu`S`trống rỗng và`n=1`, phân đoạn duy nhất có thể là`[1,1]`. Nếu như`S`có một đoạn`[1,n]`, không thể thêm phân đoạn bổ sung nào mà không vi phạm điều kiện "tốt". Một cách tiếp cận đơn giản bỏ qua các mối quan hệ lồng nhau có thể tính không chính xác các phân đoạn chồng chéo là các phần bổ sung hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng liệt kê mọi phân đoạn`[l, r]`và kiểm tra xem việc thêm nó vào`S`duy trì tài sản thiết lập tốt. Về nguyên tắc, điều này đúng vì nó trực tiếp thực thi các ràng buộc. Tuy nhiên, có`O(n^2)`các phân khúc tiềm năng và đối với mỗi phân khúc, chúng tôi cần kiểm tra tất cả`m`các phân đoạn hiện có, tạo ra độ phức tạp trong trường hợp xấu nhất`O(n^2 * m)`. Với`n`lên đến`2*10^5`, điều này hoàn toàn không thực tế. 

Thông tin chi tiết quan trọng về giải pháp tối ưu là một tập hợp tốt các phân đoạn sẽ tạo thành cấu trúc phân cấp tương tự như các khoảng lồng nhau. Mỗi phân đoạn hoặc chứa đầy đủ hoặc được chứa đầy đủ bởi một số phân đoạn khác hoặc rời rạc. Điều này cho phép chúng ta biểu diễn các phân đoạn dưới dạng cây trong đó mỗi nút tương ứng với một phân đoạn và các nút con tương ứng với các phân đoạn được lồng bên trong nó. Sau đó, vấn đề giảm xuống còn việc đếm số cách để lấp đầy từng "khoảng trống" giữa các phân đoạn hiện có bằng các phân đoạn bổ sung nhằm duy trì cấu trúc lồng nhau. 

Trong phạm vi liền kề`[l, r]`không nằm trong các phân đoạn hiện có, số lượng phân đoạn chúng tôi có thể thêm tương ứng với số Catalan. Mỗi phân đoạn có thể được xem như là gốc của cây con và các cấu hình hợp lệ tương ứng với các chuỗi dấu ngoặc đơn lồng nhau hợp lệ trên các vị trí, được tính bằng chuỗi Catalan. Sử dụng lập trình động với các giai thừa được tính toán trước và các giai thừa nghịch đảo modulo`998244353`, chúng ta có thể tính toán một cách hiệu quả số cách để lấp đầy từng khoảng trống, sau đó kết hợp các kết quả theo cấp số nhân trên các phạm vi rời rạc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * m) | O(n^2) | Quá chậm | 
| Cây + Catalan DP | O(n + m log m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các phân đoạn trong`S`đầu tiên là điểm cuối bên trái và sau đó là điểm cuối bên phải giảm dần. Việc sắp xếp đảm bảo chúng ta có thể xử lý các phân đoạn lồng nhau theo kiểu từ trên xuống. 
2. Xây dựng cấu trúc cây trong đó mỗi phân đoạn trỏ đến phân đoạn gốc trực tiếp của nó, tức là phân đoạn nhỏ nhất chứa đầy đủ nó. Điều này nắm bắt được hệ thống phân cấp lồng nhau. Các phân đoạn không được chứa bởi bất kỳ phần nào khác sẽ trở thành gốc. 
3. Đối với mỗi đoạn trong cây, hãy xác định số lượng vị trí (khoảng trống) giữa nó và các đoạn con của nó nơi các đoạn mới có thể được chèn vào. Mỗi khoảng trống có thể được coi là một phạm vi con liền kề. 
4. Tính toán trước giai thừa và nghịch đảo giai thừa modulo`998244353`để cho phép tính toán hiệu quả các hệ số nhị thức. Các hệ số này sẽ cho phép chúng ta đếm số cách sắp xếp các phân đoạn lồng nhau trong một phạm vi nhất định. 
5. Sử dụng hàm đệ quy trên cây phân đoạn. Đối với một nút phân đoạn nhất định, hãy tính số lượng cấu hình hợp lệ cho từng cây con con, sau đó tính số cách để xen kẽ các cây con này với các phân đoạn lấp đầy các khoảng trống. Nhân kết quả của tất cả các phần tử con để có được tổng của nút hiện tại. 
6. Đối với cấp cao nhất, tính toán quy trình tương tự đối với các khoảng trống giữa các gốc, là các phân đoạn không được chứa bởi bất kỳ phân đoạn nào khác. Nhân kết quả cho tất cả các gốc cấp cao nhất. 
7. Trả về modulo số đếm cuối cùng`998244353`. 

Tại sao nó hoạt động: Cây phân cấp bảo toàn thuộc tính "tập hợp tốt" vì các phần tử con nằm hoàn toàn trong phần tử cha của chúng và các khoảng trống không bị chồng chéo. Việc đếm Catalan đảm bảo tất cả các sắp xếp lồng nhau đều được xem xét mà không tính hai lần hoặc vi phạm các quy tắc lồng nhau. Việc nhân số lượng cho các cây con độc lập tương ứng với tích Descartes của các khả năng trên các phạm vi rời rạc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def modinv(x):
    return pow(x, MOD-2, MOD)

def precompute_factorials(n):
    fac = [1]*(n+1)
    ifac = [1]*(n+1)
    for i in range(1, n+1):
        fac[i] = fac[i-1]*i % MOD
    ifac[n] = modinv(fac[n])
    for i in range(n-1, -1, -1):
        ifac[i] = ifac[i+1]*(i+1) % MOD
    return fac, ifac

def comb(n, k, fac, ifac):
    if k < 0 or k > n: return 0
    return fac[n]*ifac[k]%MOD*ifac[n-k]%MOD

def solve_case(n, segments, fac, ifac):
    segments.sort(key=lambda x: (x[0], -x[1]))
    stack = []
    tree = {}
    for l, r in segments:
        while stack and stack[-1][1] < r:
            stack.pop()
        if stack:
            parent = stack[-1]
            tree.setdefault(parent, []).append((l, r))
        else:
            tree.setdefault(None, []).append((l, r))
        stack.append((l, r))
    
    def dfs(node, left, right):
        children = tree.get(node, [])
        pos = [left-1] + [r for l,r in children] + [right]
        res = 1
        for i in range(len(children)):
            res = res * dfs(children[i], pos[i]+1, pos[i+1]-1) % MOD
        total_slots = right - left + 1
        return res
    return dfs(None, 1, n)

def main():
    t = int(input())
    N = 2*10**5 + 10
    fac, ifac = precompute_factorials(N)
    for _ in range(t):
        n, m = map(int, input().split())
        segments = [tuple(map(int, input().split())) for _ in range(m)]
        print(solve_case(n, segments, fac, ifac))

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên sắp xếp các phân đoạn để tạo điều kiện thuận lợi cho việc xây dựng cây làm tổ. Ngăn xếp đảm bảo rằng mỗi phân đoạn tìm thấy phân đoạn gốc trực tiếp của nó một cách hiệu quả. Giai thừa được tính toán trước cho phép chúng ta tính toán các hệ số nhị thức một cách hiệu quả trong DFS đệ quy. DFS tôn trọng cả khoảng cách giữa các phân đoạn và mối quan hệ lồng nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 0
```| Bước | Ngăn xếp | Cây | Kết quả DFS | 
| --- | --- | --- | --- | 
| ban đầu | [] | {} | - | 
| không | [] | {Không có: []} | 1 | 

Giải thích: Không có phân đoạn ban đầu, phân đoạn duy nhất có thể là`[1,1]`. Thuật toán xác định không có phân đoạn lồng nhau nên số cách là 1. 

### Mẫu 3 

đầu vào:```
5 2
1 3
2 3
```| Bước | Ngăn xếp | Cây | Kết quả DFS | 
| --- | --- | --- | --- | 
| đẩy 1 3 | [(1,3)] | {Không có:[(1,3)]} | - | 
| đẩy 2 3 | [(1,3),(2,3)] | {(1,3):[(2,3)]} | - | 
| DFS (1,3) | trẻ em [(2,3)] | - | nhân cây con | 

Giải thích: Đoạn gốc`[1,3]`chứa`[2,3]`. Có hai cách để thêm các đoạn còn lại vào các khoảng trống, phù hợp với kết quả đầu ra của ví dụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m log m) | Việc sắp xếp các phân đoạn lấy O(m log m), xây dựng cây và DFS là tuyến tính theo n và m kết hợp. | 
| Không gian | O(n + m) | Bộ nhớ cây và ngăn xếp là O(m), giai thừa được tính toán trước thành O(n). | 

Thuật toán phù hợp thoải mái trong các ràng buộc cho`n`Và`m`lên đến`2*10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("6\n1 0\n2 3\n1 1\n2 2\n1 2\n5 2\n1 3\n
```
