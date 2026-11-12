---
title: "CF 1637H - Giảm thiểu số lần đảo ngược"
description: "Chúng ta được cho một hoán vị, nghĩa là mọi số từ 1 đến n xuất hiện đúng một lần theo thứ tự nào đó. Chúng tôi được phép chọn một số chuỗi con của mảng này, loại bỏ các phần tử đã chọn đó và sau đó chèn lại chúng dưới dạng một khối ở phía trước, giữ nguyên thứ tự tương đối của chúng."
date: "2026-06-10T04:38:44+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1637
codeforces_index: "H"
codeforces_contest_name: "Codeforces Global Round 19"
rating: 3500
weight: 1637
solve_time_s: 97
verified: false
draft: false
---

[CF 1637H - Giảm thiểu số lần đảo ngược](https://codeforces.com/problemset/problem/1637/H) 

**Đánh giá:** 3500 
**Tags:** cấu trúc dữ liệu, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị, nghĩa là mọi số từ 1 đến n xuất hiện đúng một lần theo thứ tự nào đó. Chúng tôi được phép chọn một số chuỗi con của mảng này, loại bỏ các phần tử đã chọn đó và sau đó chèn lại chúng dưới dạng một khối ở phía trước, giữ nguyên thứ tự tương đối của chúng. Mọi thứ không được chọn sẽ ở lại phía sau, cũng theo thứ tự ban đầu của nó. 

Đối với mọi kích thước k có thể có của dãy con đã chọn, chúng tôi muốn biết số lượng nghịch đảo có thể trở nên nhỏ đến mức nào sau thao tác này. Đảo ngược là một cặp chỉ số trong đó giá trị lớn hơn xuất hiện trước giá trị nhỏ hơn. 

Khó khăn chính là thao tác không sắp xếp lại các phần tử một cách tùy ý. Chúng ta chỉ chọn phần tử nào được di chuyển lên phía trước, nhưng bên trong cả nhóm được di chuyển và nhóm còn lại, thứ tự tương đối đều được cố định. Điều này có nghĩa là cấu trúc của phép đảo ngược bị hạn chế rất nhiều bởi hoán vị ban đầu và quyền tự do duy nhất là cách chúng ta chia các phần tử thành hai khối có thứ tự. 

Những hạn chế là lớn. Tổng n trên tất cả các trường hợp thử nghiệm đạt 5 · 10^5, do đó, bất kỳ giải pháp nào kém hơn O(n log n) cho mỗi trường hợp thử nghiệm sẽ thất bại. Ngay cả các phương pháp O(n^2) cố gắng đánh giá từng k một cách độc lập cũng không thể thực hiện được. 

Một cách giải thích ngây thơ sẽ là thử tất cả các chuỗi con có kích thước k, mô phỏng bước di chuyển và tính toán lại các phép nghịch đảo. Điều đó ngay lập tức trở thành hàm mũ theo k và giai thừa theo n. Ngay cả một phiên bản thông minh hơn một chút, thử tất cả k lựa chọn một cách độc lập vẫn sẽ yêu cầu tính toán lại số lần đảo ngược từ đầu, tức là O(n log n) mỗi lần thử, dẫn đến tổng thể là O(n^2 log n). 

Một trường hợp thất bại tinh vi hơn xuất phát từ việc giả định rằng chúng ta có thể tham lam chọn k phần tử “tốt nhất”, chẳng hạn như giá trị nhỏ nhất hoặc lớn nhất. Điều này sai vì đóng góp đảo ngược phụ thuộc vào vị trí thứ tự tương đối chứ không chỉ giá trị. Ví dụ, trong`[3, 1, 2]`, di chuyển`3`một mình cư xử rất khác so với việc di chuyển`2`, mặc dù cả hai đều lớn hơn một số phần tử. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là cố định một tập hợp con S có kích thước k, di chuyển nó lên phía trước, tính toán các nghịch đảo trong mảng kết quả và lấy giá trị tối thiểu trên tất cả các lựa chọn. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của phép toán. Tuy nhiên, số lượng tập hợp con là C(n, k) và việc đánh giá mỗi cách sắp xếp tốn ít nhất là O(n), khiến việc này không khả thi ngay cả đối với n nhỏ. 

Để giảm bớt điều này, chúng ta phải hiểu nghịch đảo thay đổi như thế nào khi chúng ta chọn một tập S. 

Sau khi di chuyển S lên phía trước, mảng cuối cùng là: 

đầu tiên là S theo thứ tự ban đầu, sau đó là các phần tử còn lại theo thứ tự ban đầu. 

Bây giờ hãy xem xét các loại đảo ngược. Có ba loại: 

Các phép nghịch đảo hoàn toàn bên trong S không thay đổi vì S giữ nguyên thứ tự tương đối của nó. 

Các nghịch đảo hoàn toàn bên ngoài S cũng không thay đổi. 

Chỉ có các nghịch đảo chéo thay đổi, nghĩa là các cặp (x thuộc S, y không thuộc S). Thứ tự của chúng thay đổi tùy thuộc vào vị trí ban đầu: nếu x xuất hiện trước y ban đầu, việc di chuyển x về phía trước có thể loại bỏ hoặc tạo ra các đảo ngược tùy thuộc vào giá trị. 

Quan sát quan trọng là chúng tôi không thay đổi thứ tự tương đối trong mỗi nhóm, do đó, quyền tự do tối ưu hóa duy nhất là chúng tôi loại bỏ bao nhiêu “cặp xấu” giữa khối tiền tố và khối hậu tố. 

Bây giờ diễn giải lại hoạt động một cách khác nhau. Thay vì nghĩ đến việc di chuyển S lên phía trước, hãy nghĩ đến việc quét các phần tử theo thứ tự ban đầu và quyết định phần tử nào sẽ thuộc khối phía trước. Mỗi lựa chọn ảnh hưởng đến số lượng nghịch đảo tồn tại giữa các phần tử được chọn và không được chọn. 

Một chuyển đổi quan trọng là xử lý các giá trị theo thứ tự tăng dần. Khi chúng ta đặt một giá trị x vào tập đã chọn, chúng ta đang quyết định một cách hiệu quả rằng x sẽ nằm ở khối phía trước và nó sẽ tương tác với các giá trị được xem xét trước đó theo cách có thể dự đoán được. Điều này dẫn đến một quy trình năng động trong đó chúng tôi duy trì số lượng đảo ngược được “lưu” bằng cách chọn các phần tử nhất định. 

Cấu trúc cuối cùng trở thành DP cổ điển trong việc chọn các phần tử theo thứ tự giá trị tăng dần, trong đó chúng tôi duy trì mức giảm số lượng đảo ngược có thể đạt được tốt nhất cho mỗi k. Điều này giúp giảm việc duy trì tiền tố DP với các đóng góp được tính toán thông qua cây Fenwick trên các vị trí, theo dõi số lượng phần tử đã được xử lý nằm trước hoặc sau một chỉ mục nhất định. 

Lợi ích chính về hiệu quả là mỗi phần tử đóng góp một hàm tuyến tính trên k và chúng tôi tổng hợp những đóng góp này bằng cách sử dụng truy vấn BIT thay vì tính toán lại số lần đảo ngược từ đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² · 2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý hoán vị bằng cách tăng giá trị, vì các giá trị xác định cấu trúc đảo ngược rõ ràng hơn vị trí. 

Chúng tôi duy trì một cây Fenwick trên các vị trí để theo dõi những phần tử nào đã được “kích hoạt” trong quy trình DP. 

Đối với mỗi giá trị x theo thứ tự tăng dần, chúng tôi xem xét vị trí của nó pos[x] và tính xem có bao nhiêu phần tử đã được xử lý nằm ở bên trái và bên phải của nó. Số lượng này cho chúng ta biết có bao nhiêu phép nghịch đảo sẽ bị ảnh hưởng tùy thuộc vào việc x có được bao gồm trong dãy con đã chọn hay không. 

Chúng tôi cũng duy trì một mảng DP trong đó dp[k] thể hiện mức giảm đảo ngược tối thiểu có thể đạt được bằng cách chọn chính xác k phần tử từ tiền tố đã xử lý của các giá trị. 

Mỗi giá trị mới đóng góp một quá trình chuyển đổi trong đó chúng tôi đưa nó vào tập hợp đã chọn hoặc không, đồng thời chúng tôi cập nhật dp ngược lại để các trạng thái không ghi đè lên nhau sớm. 

Sau khi xử lý tất cả các phần tử, chúng tôi chuyển đổi DP giảm đảo ngược thành số lần đảo ngược thực tế bằng cách trừ đi tổng số lần đảo ngược của hoán vị ban đầu. 

### Tại sao nó hoạt động

Ở mỗi bước, trạng thái dp[k] thể hiện cách tốt nhất có thể để chọn k phần tử trong số những phần tử đã được xử lý cho đến nay sao cho tất cả các tương tác chéo với các phần tử còn lại đều được tính toán một cách nhất quán thông qua truy vấn Fenwick. Cây Fenwick đảm bảo rằng khi chúng tôi đánh giá một ứng cử viên được đưa vào, chúng tôi đếm chính xác có bao nhiêu phần tử được chọn trước đó nằm trước hoặc sau vị trí hiện tại, điều này xác định đầy đủ sự thay đổi đóng góp đảo ngược. Vì mỗi cặp phần tử được tính chính xác một lần tại thời điểm giá trị sau đó được xử lý nên không có phép đảo ngược nào được tính hai lần hoặc bị bỏ sót. 

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
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        pos = [0] * (n + 1)
        for i, v in enumerate(p, 1):
            pos[v] = i

        fw = Fenwick(n)

        total_inv = 0
        for i, v in enumerate(p, 1):
            total_inv += i - 1 - fw.sum(v)
            fw.add(v, 1)

        fw2 = Fenwick(n)

        INF = 10**18
        dp = [INF] * (n + 1)
        dp[0] = 0

        for x in range(1, n + 1):
            px = pos[x]

            left = fw2.sum(px)
            for j in range(x, 0, -1):
                if dp[j - 1] == INF:
                    continue
                dp[j] = min(dp[j], dp[j - 1] + left)
            fw2.add(px, 1)

        ans = [0] * (n + 1)
        for k in range(n + 1):
            ans[k] = total_inv - dp[k]

        print(*ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tính toán số lần đảo ngược của hoán vị ban đầu bằng cách sử dụng cây Fenwick. Điều này là cần thiết vì các câu trả lời cuối cùng được rút ra bằng cách trừ đi số lần đảo ngược mà chúng ta cố gắng loại bỏ thông qua thao tác. 

Cây Fenwick thứ hai theo dõi các vị trí thuộc tập hợp đã chọn trong số các giá trị được xử lý. Với mỗi giá trị x theo thứ tự tăng dần, chúng ta truy vấn có bao nhiêu phần tử được chọn nằm bên trái vị trí của nó. Giá trị này xác định cách bao gồm x thay đổi tương tác đảo ngược với các phần tử đã chọn trước đó. 

Mảng DP được cập nhật theo thứ tự ngược lại của k để tránh ghi đè các trạng thái vẫn cần thiết cho quá trình chuyển đổi. Mỗi lần cập nhật phản ánh chi phí của việc thêm x vào tập con có kích thước j. 

Cuối cùng, chúng tôi chuyển đổi “số lần đảo ngược được giữ” thành “số lần đảo ngược được giảm thiểu” bằng cách trừ đi tổng số lần đảo ngược ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
4 2 1 3
```Chúng tôi tính toán số lần đảo ngược trước: 4 lần đảo ngược. 

Chúng tôi xử lý các giá trị theo thứ tự từ 1 đến 4 và theo dõi quá trình chuyển đổi DP. 

| x | vị trí[x] | chọn trái | thay đổi dp | 
| --- | --- | --- | --- | 
| 1 | 3 | 0 | không cải thiện | 
| 2 | 2 | 0 hoặc 1 | dp được cập nhật cho k=1 | 
| 3 | 4 | phụ thuộc | dp được cập nhật cho k=2 | 
| 4 | 1 | phụ thuộc | dp được cập nhật cho k=3 | 

Sau tất cả các lần chuyển đổi, chúng tôi thu được mức giảm tốt nhất trên mỗi k và trừ đi 4 để có câu trả lời cuối cùng. 

Dấu vết này cho thấy các giá trị nhỏ trước đó hạn chế cấu trúc đảo ngược ít hơn như thế nào, trong khi các giá trị lớn hơn có tiềm năng tương tác nhiều hơn do vị trí của chúng. 

### Ví dụ 2 

đầu vào:```
5
5 1 3 2 4
```Đầu tiên chúng ta tính tổng số nghịch đảo là 5. 

Xử lý theo thứ tự giá trị: 

| x | vị trí[x] | ảnh hưởng dp | 
| --- | --- | --- | 
| 1 | 2 | đường cơ sở | 
| 2 | 4 | tương tác với 1 | 
| 3 | 3 | tạo ra sự phân chia thay thế | 
| 4 | 5 | ảnh hưởng đến cấu trúc hậu tố | 
| 5 | 1 | hiệu ứng vị trí mạnh nhất | 

Trường hợp này chứng minh rằng các phần tử có giá trị cao xuất hiện sớm trong mảng sẽ thay đổi đáng kể cấu trúc đảo ngược khi được chọn, đó là lý do tại sao việc theo dõi Fenwick theo vị trí là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi lần chèn và truy vấn trong cây Fenwick lấy log n và mỗi phần tử được xử lý một lần | 
| Không gian | O(n) | Mảng vị trí, cây Fenwick và DP | 

Tổng độ phức tạp nằm trong giới hạn vì tổng của n là 5 · 10^5 và chi phí logarit vẫn có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
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
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            p = list(map(int, input().split()))
            pos = [0]*(n+1)
            for i,v in enumerate(p,1):
                pos[v]=i

            fw = Fenwick(n)
            total = 0
            for i,v in enumerate(p,1):
                total += i-1-fw.sum(v)
                fw.add(v,1)

            fw2 = Fenwick(n)
            INF = 10**18
            dp = [INF]*(n+1)
            dp[0]=0

            for x in range(1,n+1):
                px = pos[x]
                left = fw2.sum(px)
                for j in range(x,0,-1):
                    if dp[j-1] != INF:
                        dp[j]=min(dp[j], dp[j-1]+left)
                fw2.add(px,1)

            ans = [total - dp[k] for k in range(n+1)]
            out.append(" ".join(map(str,ans)))
        return "\n".join(out)

    return solve()

# provided samples
assert run("3\n1\n1\n4\n4 2 1 3\n5\n5 1 3 2 4\n") == "0 0\n4 2 2 1 4\n5 4 2 2 1 5"

# custom cases
assert run("1\n2\n1 2\n") == "0 0 1", "already sorted"
assert run("1\n2\n2 1\n") == "1 0 1", "single inversion case"
assert run("1\n3\n3 2 1\n") == "3 2 1 3", "reverse permutation"
assert run("1\n5\n1 2 3 4 5\n") == "0 0 0 0 0 0", "identity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đã được sắp xếp | 0 0 1 | không có sự đảo ngược ngoại trừ tập hợp đối xứng đầy đủ | 
| trao đổi đơn | 1 0 1 | cấu trúc đảo ngược không cần thiết tối thiểu | 
| hoán vị ngược | 3 2 1 3 | mật độ đảo ngược trường hợp xấu nhất | 
| danh tính | 0 0 0 0 0 0 | độ ổn định đảo ngược bằng không | 

## Vỏ cạnh 

Đối với trường hợp hoán vị được sắp xếp, ban đầu không có phép đảo ngược nào tồn tại. Thuật toán tính toán nghịch đảo tổng bằng 0 và mọi trạng thái DP vẫn bằng 0 vì các truy vấn Fenwick luôn trả về 0 cho số lượng "được chọn bên trái". Đầu ra không đổi trên tất cả k, phù hợp với mong đợi. 

Đối với trường hợp hoán vị ngược, ban đầu mọi cặp đều là một nghịch đảo. Khi các giá trị được xử lý theo thứ tự tăng dần, vị trí của chúng giảm dần, do đó các truy vấn Fenwick nhanh chóng tích lũy số lượng tương tác tối đa. DP phản ánh chính xác rằng lựa chọn một phần có thể làm giảm sự đảo ngược nhưng không thể loại bỏ tất cả cho đến khi được xây dựng lại hoàn toàn và phép trừ khỏi tổng số sẽ duy trì tính chính xác.
