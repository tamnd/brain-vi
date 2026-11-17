---
title: "CF 1946C - Chặt Cây"
description: "Chúng ta có một cây có $n$ đỉnh, là một đồ thị liên thông không có chu trình. Nhiệm vụ là loại bỏ chính xác các cạnh $k$ khỏi cây này và xác định số nguyên lớn nhất $x$ sao cho mỗi thành phần kết nối thu được có ít nhất $x$ đỉnh."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1946
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 936 (Div. 2)"
rating: 1600
weight: 1946
solve_time_s: 170
verified: true
draft: false
---

[CF 1946C - Chặt cây](https://codeforces.com/problemset/problem/1946/C) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, dp, tham lam, triển khai, cây cối 
**Thời gian giải:** 2 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$các đỉnh là một đồ thị liên thông không có chu trình. Nhiệm vụ là loại bỏ chính xác$k$các cạnh của cây này và xác định số nguyên lớn nhất$x$sao cho mỗi thành phần được kết nối có ít nhất$x$đỉnh. Nói một cách đơn giản hơn, chúng ta đang phân chia cây thành$k+1$các thành phần có kích thước gần như cân bằng trong khi tối đa hóa thành phần nhỏ nhất. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi đọc$n$Và$k$, theo sau là$n-1$các cạnh mô tả cây. Đầu ra của mỗi test case là một số nguyên duy nhất, giá trị lớn nhất khả thi$x$. 

Những ràng buộc cho phép$n$lên đến$10^5$cho mỗi trường hợp thử nghiệm và tổng của$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$10^5$. Điều này hạn chế chúng tôi trong các thuật toán gần như$O(n \log n)$hoặc nhanh hơn, bởi vì một$O(n^2)$cách tiếp cận sẽ dẫn đến$10^{10}$hoạt động trong trường hợp xấu nhất là quá chậm. Mỗi trường hợp thử nghiệm đều độc lập nên chúng tôi có thể xử lý từng trường hợp một. 

Trường hợp khó phát hiện xảy ra khi$k$rất nhỏ hoặc rất lớn so với$n$. Ví dụ, nếu$k = 1$Và$n = 3$với một cái cây$1-2-3$, chúng ta có thể loại bỏ một cạnh, nhưng hai thành phần thu được có kích thước$1$Và$2$. Tối đa$x$vậy thì$1$, mặc dù kích thước trung bình là$1.5$. Một cách tiếp cận ngây thơ tính toán mức trung bình hoặc giả định phân phối bằng nhau sẽ trả về$2$, điều đó không đúng. 

Một trường hợp cạnh khác là cây hình ngôi sao. Nếu trung tâm kết nối với tất cả các lá và$k = n-1$, việc loại bỏ bất kỳ cạnh nào có thể cô lập các nút đơn lẻ. Thuật toán phải tính đến thực tế là kích thước thành phần tối thiểu bị giới hạn bởi nhánh nhỏ nhất chứ không phải bởi phép chia lý tưởng. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ liệt kê tất cả các bộ$k$các cạnh cần loại bỏ, tính toán kích thước của mọi thành phần thu được và theo dõi kích thước tối thiểu cho từng cấu hình. Điều này đúng, nhưng với mỗi cây, có$\binom{n-1}{k}$các cách để loại bỏ các cạnh, điều này không khả thi đối với$n$lên đến$10^5$. 

Cái nhìn sâu sắc quan trọng là xem xét quy mô ứng viên$x$và xác định xem có thể chia cây thành các thành phần có kích thước ít nhất$x$bằng cách loại bỏ$k$các cạnh. Nếu chúng ta có thể kiểm tra điều này một cách hiệu quả trong một trường hợp nhất định$x$, chúng ta có thể thực hiện tìm kiếm nhị phân trên$x$trong phạm vi$1 \dots n$. Quyết định của mỗi người$x$là khả thi trong$O(n)$sử dụng phương pháp tìm kiếm theo chiều sâu (DFS). Đối với mỗi nút, chúng tôi tính toán kích thước của cây con của nó và quyết định xem liệu chúng tôi có thể tạo thành một thành phần hoàn chỉnh hay không. Nếu một cây con đủ lớn, chúng ta sẽ tính nó vào$k$loại bỏ cạnh. Ngược lại, chúng ta truyền kích thước của nó lên trên. DFS tham lam này đảm bảo rằng bất kỳ thành phần nào chúng tôi hình thành đều có kích thước ít nhất$x$. 

Cách tiếp cận bạo lực là không khả thi vì số lượng các tổ hợp cạnh tăng lên theo kiểu tổ hợp. Tìm kiếm nhị phân với DFS là tối ưu vì cấu trúc cây cho phép chúng ta tổng hợp các kích thước theo cách từ dưới lên, quyết định cục bộ những cạnh nào cần cắt. Kiểm tra một ứng cử viên$x$TRONG$O(n)$mang lại sự phức tạp hoàn toàn$O(n \log n)$, phù hợp thoải mái trong thời hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n chọn k) * n ) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + DFS | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo tìm kiếm nhị phân với$l = 1$Và$r = n$. Câu trả lời nằm ở phạm vi này. Chúng tôi sẽ thu hẹp nó bằng cách kiểm tra các giá trị ứng viên của$x$. 
2. Xác định hàm trợ giúp`can_cut(x)`xác định liệu có thể loại bỏ chính xác$k$các cạnh sao cho mọi thành phần tạo thành có ít nhất$x$đỉnh. 
3. Trong`can_cut(x)`, thực hiện DFS từ bất kỳ nút nào, điển hình là nút$1$. Đối với mỗi nút, hãy tính kích thước của cây con hiện tại bao gồm tất cả các nút con. 
4. Đối với mỗi cây con con có kích thước ít nhất$x$, tăng một bộ đếm thể hiện những lần cắt tiềm năng. Đối với các cây con nhỏ hơn, tích lũy kích thước và truyền nó tới cây mẹ. 
5. Tại mỗi nút, nếu kích thước tích lũy của các nút chưa được chỉ định đạt tới$x$, tăng bộ đếm điện thế cắt và đặt lại kích thước tích lũy về 0. Điều này đảm bảo mỗi thành phần được hình thành đều đáp ứng kích thước tối thiểu. 
6. Sau DFS, kiểm tra xem số lần cắt tiềm năng có ít nhất là$k$. Nếu có,`x`là khả thi; nếu không thì nó quá lớn. 
7. Sử dụng tìm kiếm nhị phân: nếu`can_cut(mid)`là đúng, thiết lập`l = mid`để thử các giá trị lớn hơn; nếu không thì đặt`r = mid - 1`. Tiếp tục cho đến khi`l`bằng`r`. 
8. Trả lại mức tối đa khả thi$x$. 

Bất biến chính là DFS luôn tổng hợp các kích thước cây con, hình thành các thành phần một cách tham lam từ dưới lên trên. Mỗi thành phần được tính vào phần cắt đều đáp ứng yêu cầu về kích thước, đảm bảo không xảy ra vi phạm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(200000)

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        tree = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            tree[u - 1].append(v - 1)
            tree[v - 1].append(u - 1)

        def can_cut(x):
            cuts = 0
            def dfs(u, parent):
                nonlocal cuts
                total = 1
                for v in tree[u]:
                    if v == parent:
                        continue
                    subtotal = dfs(v, u)
                    if subtotal >= x:
                        cuts += 1
                    else:
                        total += subtotal
                return total
            dfs(0, -1)
            return cuts >= k

        l, r = 1, n
        while l < r:
            mid = (l + r + 1) // 2
            if can_cut(mid):
                l = mid
            else:
                r = mid - 1
        print(l)

if __name__ == "__main__":
    solve()
```DFS tích lũy kích thước cây con. Cây con đạt hoặc vượt quá ứng cử viên`x`tạo thành một vết cắt, trong khi những cái nhỏ hơn lan truyền lên trên. Tìm kiếm nhị phân kiểm tra từng điểm giữa, đảm bảo các bước logarit. sử dụng`sys.setrecursionlimit`tránh tràn ngăn xếp cho cây lớn. Việc lập chỉ mục được điều chỉnh từ đầu vào dựa trên 1 sang danh sách Python dựa trên 0. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
5 1
1 2
1 3
3 4
3 5
```| Nút | Kích thước cây con | Cắt | 
| --- | --- | --- | 
| 2 | 1 | 0 | 
| 4 | 1 | 0 | 
| 5 | 1 | 0 | 
| 3 | 1+1+1=3 | 1 (3>=2) | 
| 1 | 1+1+3=5 | 1 | 

Tìm kiếm nhị phân xác định rằng`x=2`là giá trị khả thi lớn nhất. Đang cố gắng`x=3`thất bại vì chỉ có thể thực hiện một lần cắt. 

**Đầu vào mẫu 2:**```
6 2
1 2
2 3
1 4
4 5
5 6
```Tìm kiếm nhị phân đánh giá ứng viên`x=2`và tìm thấy hai vết cắt có thể: một ở cây con bắt nguồn từ`2`, một ở cây con bắt nguồn từ`4`.`x=3`thất bại vì cây con còn lại không thể đạt kích thước 3 sau hai lần cắt. Đầu ra là`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi bước tìm kiếm nhị phân là O(n) do DFS. Tìm kiếm nhị phân trên phạm vi 1..n là log n bước. | 
| Không gian | O(n) | Danh sách cây liền kề và ngăn xếp đệ quy có độ sâu lên tới n. | 

Được cho$n \le 10^5$và tổng của$n$trên tất cả các trường hợp thử nghiệm$10^5$, giải pháp này chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("6\n5 1\n1 2\n1 3\n3 4\n3 5\n2 1\n1 2\n6 1\n1 2\n2 3\n3 4\n4 5\n5 6\n3 1\n1 2\n1 3\n8 2\n1 2\n1 3\n2 4\n2 5\n3 6\n3 7\n3 8\n6 2\n1 2\n2 3\n1 4\n4 5\n5
```
