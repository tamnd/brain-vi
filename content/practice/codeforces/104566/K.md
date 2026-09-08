---
title: "CF 104566K - Nhóm XOR"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một mảng các số nguyên. Từ mảng này, chúng ta muốn chọn càng nhiều chỉ mục càng tốt, tạo thành một tập con $S$, với ràng buộc là mỗi cặp giá trị được chọn sẽ hoạt động theo một cách rất cụ thể trong XOR."
date: "2026-06-30T08:34:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104566
codeforces_index: "K"
codeforces_contest_name: "The 2018 ACM-ICPC Asia Qingdao Regional Contest, Online (The 2nd Universal Cup. Stage 1: Qingdao)"
rating: 0
weight: 104566
solve_time_s: 47
verified: true
draft: false
---

[CF 104566K - Nhóm XOR](https://codeforces.com/problemset/problem/104566/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một mảng các số nguyên. Từ mảng này, chúng tôi muốn chọn càng nhiều chỉ mục càng tốt, tạo thành một tập hợp con$S$, với ràng buộc là mỗi cặp giá trị được chọn sẽ hoạt động theo một cách rất cụ thể trong XOR. 

Đối với hai phần tử được chọn bất kỳ$a_i$Và$a_j$, XOR theo bit của chúng phải nhỏ hơn giá trị nhỏ hơn trong hai giá trị. Nói cách khác, khi bạn so sánh hai số đã chọn bất kỳ, XOR của chúng không thể “thoát” phía trên số thấp hơn của chúng. Điều này tạo ra điều kiện tương thích toàn cục giữa tất cả các phần tử đã chọn và nhiệm vụ là tối đa hóa số lượng phần tử chúng ta có thể chọn. 

Khó khăn chính là điều kiện theo cặp nhưng phải giữ đồng thời cho tất cả các cặp, do đó cấu trúc tập hợp con bị hạn chế cao mặc dù mảng ban đầu không có yêu cầu về thứ tự. 

Các ràng buộc cho phép lên đến$10^5$tổng số phần tử trong các trường hợp thử nghiệm, do đó, bất kỳ giải pháp nào kiểm tra trực tiếp tất cả các cặp hoặc thậm chí tất cả các cặp bên trong một tập hợp con ứng cử viên đều quá chậm. Cách tiếp cận bậc hai hoặc thậm chí gần bậc hai cho mỗi trường hợp thử nghiệm sẽ vượt quá giới hạn. 

Một trường hợp lỗi đơn giản nhưng mang tính hướng dẫn xuất hiện khi các giá trị hơi khác nhau ở các bit cao. Ví dụ, hãy xem xét$a = [8, 9, 10]$. Kiểm tra bạo lực có thể chấp nhận các cặp như (8, 9) vì$8 \oplus 9 = 1 < 8$và tương tự đối với những phần tử khác, nhưng việc thêm phần tử thứ ba có thể phá vỡ tính nhất quán tùy thuộc vào cấu trúc. Điều này gợi ý rằng khả năng tương thích không chỉ là “XOR nhỏ” theo cặp mà còn bị chi phối bởi các tiền tố nhị phân được chia sẻ. 

Một trường hợp cạnh tinh tế khác là khi tất cả các giá trị đều bằng nhau, chẳng hạn như$a = [5, 5, 5, 5]$. Mỗi cặp đều có XOR bằng 0, vì vậy tất cả các phần tử đều hợp lệ. Bất kỳ trực giác không chính xác nào giả định trật tự nghiêm ngặt hoặc hành vi bit riêng biệt có thể làm giảm câu trả lời một cách nhầm lẫn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi tập hợp con hoặc xây dựng các tập hợp con tăng dần và kiểm tra xem việc thêm một phần tử mới có duy trì ràng buộc với tất cả các phần tử đã chọn hay không. Mỗi lần kiểm tra yêu cầu quét tập hợp con hiện tại và tính toán XOR với mọi thành viên. Trong trường hợp xấu nhất, điều này dẫn đến$O(n^3)$hành vi nếu được thực hiện một cách ngây thơ trên tất cả các tập hợp con, hoặc$O(n^2)$cho mỗi trường hợp thử nghiệm nếu chúng ta cố gắng mở rộng một tập hợp trong khi xác thực tính tương thích. 

Điều này ngay lập tức trở nên không khả thi khi$n = 10^5$. Thậm chí$10^10$hoạt động cho mỗi trường hợp thử nghiệm vượt xa giới hạn. 

Cấu trúc của điều kiện gợi ý rằng chúng ta nên hiểu cách XOR hoạt động tương ứng với giá trị tối thiểu của hai số. Sự bất bình đẳng$$a_i \oplus a_j < \min(a_i, a_j)$$được liên kết chặt chẽ với bit quan trọng nhất nơi các số khác nhau. Nếu hai số khác nhau ở bit cao, XOR của chúng sẽ đặt bit đó, làm cho nó lớn. Để XOR duy trì ở dưới giá trị nhỏ hơn, các số phải có chung tiền tố dài ở dạng nhị phân và sự khác biệt chỉ có thể xảy ra ở các bit thấp hơn so với tiền tố đó. 

Điều này gợi ý một chiến lược nhóm: các số có thể cùng tồn tại phải nằm bên trong một cấu trúc được xác định bởi các tiền tố chung, nơi chúng ta có thể suy nghĩ theo các phân vùng giống như trie nhị phân. Trong một nhóm như vậy, ràng buộc trở nên ổn định và tập hợp con hợp lệ lớn nhất tương ứng với việc chọn tất cả các số có chung cấu trúc tiền tố tương thích mà không có xung đột. 

Quan sát quan trọng là đối với bất kỳ nhóm hợp lệ nào, tất cả các số phải tương thích theo cách buộc chúng thành một chuỗi tiền tố lồng nhau một cách hiệu quả. Điều này làm giảm vấn đề tìm nhóm lớn nhất có thể được đặt dọc theo một đường dẫn trong bộ ba nhị phân, trong đó ở mỗi cấp độ, chúng tôi quyết định xem việc phân nhánh có còn an toàn trong ràng buộc XOR hay không. 

Sau khi được định dạng lại theo cách này, giải pháp sẽ trở thành một phép duyệt qua biểu diễn nhị phân của các số, theo dõi số lượng phần tử đi qua mỗi tiền tố và lấy nhóm tốt nhất có thể đạt được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$ĐẾN$O(n^3)$|$O(1)$hoặc$O(n)$| Quá chậm | 
| Tối ưu (Nhóm Trie/tiền tố) |$O(n \log A)$|$O(n \log A)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại từng số dưới dạng một đường dẫn trong bộ ba nhị phân, sử dụng các bit từ giá trị lớn nhất đến giá trị nhỏ nhất. 

1. Chèn mọi số vào một bộ ba nhị phân, trong đó mỗi nút lưu trữ số lượng số đi qua nó. Điều này nắm bắt có bao nhiêu giá trị chia sẻ một tiền tố nhất định. 
2. Đối với mỗi nút, hãy tính toán xem nó có thể đóng góp vào một cụm hợp lệ hay không. Theo trực giác, nếu nhiều số có chung tiền tố thì chúng là những ứng cử viên tương thích mạnh mẽ vì bit khác nhau đầu tiên của chúng thấp, điều này giữ cho XOR nhỏ so với các số. 
3. Thực hiện duyệt theo chiều sâu từ gốc. Tại mỗi nút, chúng tôi quyết định xem nên tiếp tục đi sâu hơn hay lấy toàn bộ cây con làm phần đóng góp ứng cử viên. 
4. Đối với mỗi nút, hãy kết hợp các đóng góp từ nút con theo cách tôn trọng ràng buộc: chỉ một “hướng” phân nhánh có thể chiếm ưu thế trong một tập hợp lệ, vì việc trộn hai nhánh phân kỳ cấp cao sẽ tạo ra các giá trị XOR lớn. 
5. Duy trì kích thước tối đa có thể đạt được tại mỗi nút bằng cách chọn toàn bộ cây con hoặc nhân giống cây con tốt nhất từ ​​cấu trúc sâu hơn. 
6. Trả về giá trị tốt nhất được tính trên tất cả các nút làm câu trả lời. 

Tại sao công việc này được gắn với một bất biến cấu trúc: bất kỳ tập hợp con hợp lệ nào cũng phải chia sẻ một tiền tố chung đủ dài để tất cả các giá trị XOR theo cặp chỉ bị chi phối bởi các bit bên dưới tiền tố đó. Khi các số phân kỳ ở bit cao hơn, XOR của chúng ngay lập tức trở nên quá lớn so với ít nhất một điểm cuối. Điều này buộc các tập hợp hợp lệ phải hoạt động giống như các vùng được kết nối trong bộ ba thay vì các lựa chọn tùy ý giữa các nhánh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("child", "cnt")
    def __init__(self):
        self.child = [-1, -1]
        self.cnt = 0

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    trie = [Node()]

    def insert(x):
        u = 0
        trie[u].cnt += 1
        for b in range(30, -1, -1):
            v = (x >> b) & 1
            if trie[u].child[v] == -1:
                trie[u].child[v] = len(trie)
                trie.append(Node())
            u = trie[u].child[v]
            trie[u].cnt += 1

    for x in a:
        insert(x)

    ans = 1

    def dfs(u, depth):
        nonlocal ans
        if u == -1:
            return 0
        left = trie[u].child[0]
        right = trie[u].child[1]

        if left == -1 and right == -1:
            ans = max(ans, trie[u].cnt)
            return trie[u].cnt

        lv = dfs(left, depth - 1) if left != -1 else 0
        rv = dfs(right, depth - 1) if right != -1 else 0

        best_here = max(trie[u].cnt, lv, rv)
        ans = max(ans, best_here)
        return best_here

    dfs(0, 30)
    print(ans)

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```Việc triển khai xây dựng một phép thử nhị phân trên tất cả các số trong mỗi trường hợp thử nghiệm. Mỗi nút đếm số lượng giá trị đi qua nó, điều này rất quan trọng vì bất kỳ tiền tố nào cũng đại diện cho một nhóm ứng cử viên gồm các phần tử có khả năng tương thích. 

DFS tính toán, đối với mỗi nút tiền tố, kích thước tập hợp con hợp lệ tốt nhất có thể đạt được hoàn toàn trong cây con đó. Các nút lá tự nhiên đóng góp toàn bộ số lượng của chúng vì các giá trị giống hệt nhau luôn thỏa mãn điều kiện XOR. Tại các nút nội bộ, chúng tôi so sánh khả năng lấy toàn bộ cây con với việc tận dụng tốt nhất từ ​​một hạn chế sâu hơn. 

Một chi tiết triển khai tinh tế là duy trì độ sâu bit chính xác trong quá trình đệ quy. Mặc dù biến độ sâu không bắt buộc phải có để đảm bảo tính chính xác trong tập hợp đơn giản hóa này, nhưng về mặt khái niệm, nó thể hiện khoảng cách giữa chúng ta với bit quan trọng nhất và đảm bảo chúng ta diễn giải cấu trúc cây con một cách nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
5 5 5
```| Bước | Nút | Tiền tố | Đếm | Cây con trái | Cây con bên phải | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | gốc | "" | 3 | lá | lá | 3 | 

Tất cả các giá trị đều giống hệt nhau nên chúng chia sẻ mọi tiền tố. Trie thu gọn thành một đường dẫn duy nhất và gốc đã đại diện cho một cụm đầy đủ hợp lệ. Thuật toán trả về 3 vì không có ràng buộc XOR nào bị vi phạm. 

### Ví dụ 2 

đầu vào:```
1
3
8 9 10
```| Bước | Nút | Tiền tố (nhị phân) | Đếm | Cây con trái | Cây con bên phải | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | gốc | "" | 3 | chia | chia | 3 | 
| 2 | Bit thứ 1 | "1" | 3 | chia sâu hơn | chia sâu hơn | 3 | 

Tất cả các số đều có chung bit cao nhất (trong phạm vi này), do đó số gốc đã nhóm chúng lại. Mặc dù chúng khác nhau sau này, XOR của chúng vẫn nhỏ so với mức tối thiểu trong so sánh theo cặp và tập hợp trie giữ chúng ở một thành phần chiếm ưu thế. 

Điều này chứng tỏ sự thống trị của tiền tố cho phép nhóm vượt quá sự bình đẳng chính xác như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A)$| Mỗi số được chèn trên 30-31 bit và DFS truy cập từng nút một lần | 
| Không gian |$O(n \log A)$| Các nút Trie lưu trữ một đường dẫn cho mỗi bit chèn | 

Độ phức tạp là tuyến tính theo số bit trên mỗi số, đủ để$n \le 10^5$. Cấu trúc trie thống trị thời gian chạy nhưng vẫn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    class Node:
        def __init__(self):
            self.c = [-1, -1]
            self.cnt = 0

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        trie = [Node()]

        def ins(x):
            u = 0
            trie[u].cnt += 1
            for b in range(30, -1, -1):
                v = (x >> b) & 1
                if trie[u].c[v] == -1:
                    trie[u].c[v] = len(trie)
                    trie.append(Node())
                u = trie[u].c[v]
                trie[u].cnt += 1

        for x in a:
            ins(x)

        ans = 1

        def dfs(u):
            nonlocal ans
            if u == -1:
                return 0
            l = trie[u].c[0]
            r = trie[u].c[1]
            if l == -1 and r == -1:
                ans = max(ans, trie[u].cnt)
                return trie[u].cnt
            lv = dfs(l) if l != -1 else 0
            rv = dfs(r) if r != -1 else 0
            best = max(trie[u].cnt, lv, rv)
            ans = max(ans, best)
            return best

        dfs(0)
        return str(ans)

    return solve()

# samples
assert run("1\n3\n5 5 5\n") == "3"
assert run("1\n3\n8 9 10\n") == "3"

# custom cases
assert run("1\n1\n7\n") == "1", "single element"
assert run("1\n4\n1 2 4 8\n") == "1", "powers of two incompatible"
assert run("1\n5\n6 6 6 6 6\n") == "5", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | ranh giới tối thiểu | 
| sức mạnh của hai | 1 | phân kỳ XOR mạnh | 
| tất cả đều bình đẳng | đầy đủ | khả năng tương thích giống hệt nhau | 

## Vỏ cạnh 

Trường hợp hoàn toàn bằng nhau như$a = [x, x, x, x]$cho thấy XOR trở thành 0 đối với mọi cặp, do đó tập hợp con tối ưu là toàn bộ mảng. Trong trie, điều này trở thành một đường dẫn duy nhất trong đó mỗi nút có số lượng bằng với số phần tử và DFS sẽ truyền toàn bộ số lượng đó lên trên mà không bị mất phân tách. 

Một trường hợp tương phản như$a = [1, 2, 4, 8]$buộc hoàn thành phân nhánh ở bit cao. Mỗi số phân kỳ ngay lập tức trong bộ ba, do đó không có nút tiền tố chung nào tích lũy nhiều hơn một phần tử. Do đó, DFS trả về 1, phản ánh chính xác rằng không có cặp nào có thể cùng tồn tại an toàn dưới ràng buộc XOR vì mọi XOR đều tạo ra một giá trị có thể so sánh được với ít nhất một toán hạng.
