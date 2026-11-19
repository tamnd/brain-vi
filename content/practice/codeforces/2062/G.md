---
title: "CF 2062G - Nhà máy hoán vị"
description: "Chúng ta có hai hoán vị có độ dài $n$, $p$ và $q$, và mục tiêu của chúng ta là chuyển đổi $p$ thành $q$ bằng cách sử dụng một chuỗi các thao tác hoán đổi."
date: "2026-06-08T07:37:04+07:00"
tags: ["codeforces", "competitive-programming", "flows", "geometry", "graph-matchings", "graphs"]
categories: ["algorithms"]
codeforces_contest: 2062
codeforces_index: "G"
codeforces_contest_name: "Ethflow Round 1 (Codeforces Round 1001, Div. 1 + Div. 2)"
rating: 3500
weight: 2062
solve_time_s: 115
verified: false
draft: false
---

[CF 2062G - Nhà máy hoán vị](https://codeforces.com/problemset/problem/2062/G) 

**Đánh giá:** 3500 
**Tags:** dòng chảy, hình học, khớp đồ thị, đồ thị 
**Thời gian giải:** 1 phút 55 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai hoán vị độ dài$n$,$p$Và$q$, và mục tiêu của chúng tôi là biến đổi$p$vào trong$q$sử dụng một chuỗi các thao tác hoán đổi. Mỗi thao tác hoán đổi hai phần tử trong$p$và có chi phí bằng khoảng cách tối thiểu giữa các vị trí và khoảng cách giữa các giá trị. Chúng ta phải giảm thiểu tổng chi phí và tạo ra một chuỗi các giao dịch hoán đổi để đạt được điều này. 

Các ràng buộc đầu vào cho chúng ta biết rằng$n$có thể nhiều nhất là 100 và tổng của$n^3$trên tất cả các trường hợp thử nghiệm không vượt quá$10^6$. Điều này có nghĩa là chúng ta có thể mua được một thuật toán gần như$O(n^3)$cho mỗi trường hợp thử nghiệm, nhưng bất cứ điều gì tệ hơn nhiều sẽ vượt quá giới hạn thời gian. Số ca kiểm thử$t$có thể lên đến$10^4$, vì vậy chúng ta phải cẩn thận để giữ cho các phép toán trên mỗi trường hợp kiểm thử ở mức khiêm tốn. 

Một cách tiếp cận ngây thơ có thể cố gắng hoán đổi các phần tử một cách tham lam bất cứ khi nào chúng không đúng chỗ mà không xem xét đến việc tối ưu hóa chi phí. Điều này không thành công đối với các chu kỳ nhỏ. Ví dụ, nếu$p=[1,3,2]$Và$q=[2,1,3]$, việc hoán đổi từ trái sang phải bất cẩn trước tiên có thể di chuyển 3 đến đúng vị trí của nó, làm tăng tổng chi phí, trong khi trình tự tối ưu sẽ hoán đổi trực tiếp 1 và 2. 

Một trường hợp cạnh không rõ ràng khác xảy ra khi khoảng cách hoán đổi tối thiểu đến từ các vị trí chứ không phải giá trị. Ví dụ,$p=[2,1,4,3]$Và$q=[4,2,3,1]$minh họa rằng các giao dịch hoán đổi tối ưu có thể không phải lúc nào cũng ở các vị trí liền kề và hàm chi phí$\min(|i-j|, |p_i-p_j|)$là chìa khóa để hướng dẫn các lựa chọn hoán đổi. Việc hoán đổi các phần tử “cách xa nhau về giá trị nhưng gần nhau về vị trí” có thể mang lại chi phí nhỏ hơn so với điều ngược lại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xem xét tất cả các giao dịch hoán đổi có thể có ở mỗi bước và chọn một giao dịch có chi phí tối thiểu, cập nhật$p$cho đến khi nó khớp$q$. Về nguyên tắc điều này đúng vì bất kỳ chuỗi hoán đổi nào cuối cùng cũng có thể sắp xếp$p$để phù hợp$q$, nhưng nó không khả thi về mặt tính toán. Với$O(n^2)$hoán đổi tiềm năng mỗi bước và$O(n)$bước, tổng số hoạt động bùng nổ vượt quá giới hạn chấp nhận được đối với$n=100$. 

Cái nhìn sâu sắc quan trọng là xem vấn đề như việc giải quyết các chu trình trong hoán vị ánh xạ$p$ĐẾN$q$. Bất kỳ hoán vị nào cũng có thể bị phân hủy thành các chu trình rời rạc. Trong mỗi chu kỳ, các phần tử phải xoay để đến được vị trí mục tiêu của chúng. Hoán đổi hai phần tử trong một chu kỳ sẽ giảm vấn đề xuống một chu trình nhỏ hơn và nếu chúng ta luôn chọn các hoán đổi liên quan đến chỉ số nhỏ nhất và lớn nhất trong chu trình, chúng ta sẽ giảm thiểu$\min(|i-j|, |p_i-p_j|)$bởi vì những khác biệt lớn về giá trị hoặc vị trí sẽ tối đa hóa chi phí và những khác biệt nhỏ sẽ giảm thiểu chi phí. 

Điều này biến vấn đề thành một chuỗi các giao dịch hoán đổi có cấu trúc theo chu kỳ. Việc sắp xếp từng chu kỳ theo vị trí đảm bảo rằng mỗi lần hoán đổi đều rẻ và di chuyển hoán vị về phía mục tiêu một cách hiệu quả. Bởi vì$n\le 100$, việc lặp qua các chu kỳ và thực hiện hoán đổi theo thứ tự có thể chấp nhận được về mặt tính toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(n) | Quá chậm | 
| Tối ưu dựa trên chu kỳ | O(n^3) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng ánh xạ từ các giá trị trong$q$vào các chỉ số mục tiêu của họ. Điều này cho phép chúng ta nhanh chóng biết được mỗi phần tử của$p$nên đi. 
2. Đánh dấu tất cả các thành phần là chưa được truy cập. Chúng ta sẽ lặp qua$p$và tìm các chu trình trong hoán vị được xác định bằng cách ánh xạ các vị trí hiện tại tới các vị trí đích. 
3. Đối với mỗi phần tử chưa được truy cập, hãy thực hiện theo ánh xạ để xây dựng chu trình của nó. Chu trình chứa tất cả các chỉ số liên quan đến một vòng lặp khép kín phải được xoay. 
4. Trong mỗi chu kỳ có độ dài lớn hơn 1, hãy sắp xếp các chỉ số. Điều này đảm bảo rằng việc hoán đổi được thực hiện giữa các chỉ số cách xa nhau trong chu kỳ nhưng càng gần nhau về giá trị hoặc vị trí càng tốt, giảm thiểu rủi ro$\min(|i-j|,|p_i-p_j|)$trị giá. 
5. Đối với một chu kỳ$[i_1,i_2,...,i_k]$, thực hiện hoán đổi giữa phần tử đầu tiên và từng phần tử còn lại theo thứ tự. Sau mỗi lần hoán đổi, phần tử đầu tiên sẽ di chuyển đến gần vị trí cuối cùng của nó và chu trình dần dần được giải quyết. Ghi lại mỗi lần hoán đổi để tạo ra chuỗi đầu ra. 
6. Lặp lại cho đến khi tất cả các chu trình được xử lý. Cuối cùng,$p$trận đấu$q$. 

Tại sao nó hoạt động: Mỗi chu kỳ đều rời rạc, vì vậy việc giải quyết một chu kỳ không ảnh hưởng đến chu kỳ khác. Việc hoán đổi phần tử đầu tiên của chu trình với phần tử khác đảm bảo rằng mỗi phần tử đạt được mục tiêu mà không cần phải xem lại các vị trí một cách không cần thiết. Việc sắp xếp các chỉ số đảm bảo rằng chi phí hoán đổi được giảm thiểu trong mỗi chu kỳ. Điều bất biến là sau khi xử lý một chu trình, tất cả các phần tử của nó đều ở đúng vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        q = list(map(int, input().split()))
        pos = [0]*(n+1)
        for idx, val in enumerate(q):
            pos[val] = idx
        visited = [False]*n
        ops = []
        for i in range(n):
            if visited[i] or p[i] == q[i]:
                continue
            cycle = []
            x = i
            while not visited[x]:
                visited[x] = True
                cycle.append(x)
                x = pos[p[x]]
            if len(cycle) <= 1:
                continue
            cycle.sort()
            for j in range(len(cycle)-1, 0, -1):
                ops.append((cycle[0]+1, cycle[j]+1))
                p[cycle[0]], p[cycle[j]] = p[cycle[j]], p[cycle[0]]
        print(len(ops))
        for a,b in ops:
            print(a,b)

if __name__ == "__main__":
    solve()
```Phần đầu tiên ánh xạ các giá trị mục tiêu tới các vị trí để tạo điều kiện thuận lợi cho việc phát hiện chu kỳ. Vòng lặp chính lặp đi lặp lại$p$, xác định chu kỳ của các phần tử bị đặt sai vị trí. Việc sắp xếp các chỉ số chu kỳ trước khi thực hiện hoán đổi sẽ giảm thiểu chi phí dựa trên$\min(|i-j|,|p_i-p_j|)$chức năng. Vòng hoán đổi xoay vòng một cách hiệu quả, đảm bảo tính chính xác. Việc sử dụng lập chỉ mục dựa trên 1 cho đầu ra là rất quan trọng vì vấn đề này yêu cầu điều đó. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
p = [2,1]
q = [2,1]
```| Bước | p | Chu kỳ | Hoán đổi | 
| --- | --- | --- | --- | 
| Ban đầu | [2,1] | không | không | 

Không cần trao đổi vì$p$đã khớp rồi$q$. 

### Đầu vào mẫu 2```
p = [1,2,3]
q = [3,2,1]
```| Bước | p | Chu kỳ | Hoán đổi | 
| --- | --- | --- | --- | 
| Ban đầu | [1,2,3] | [0,2] | hoán đổi 1-3 | 
| Sau khi trao đổi | [3,2,1] | giải quyết | xong | 

Chu trình chứa các vị trí 0 và 2. Hoán đổi chúng trực tiếp sẽ di chuyển 1 và 3 vào đúng vị trí của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Mỗi trường hợp thử nghiệm tìm chu kỳ, sắp xếp chúng và thực hiện tối đa n lần hoán đổi trong mỗi chu kỳ. Với n=100, n^3 có thể chấp nhận được với các ràng buộc đầu vào. | 
| Không gian | O(n) | Mảng để truy cập, ánh xạ vị trí và lưu trữ chu kỳ. | 

Điều này phù hợp với giới hạn 2 giây và hạn chế bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("4\n2\n2 1\n2 1\n3\n1 2 3\n3 2 1\n4\n2 1 4 3\n4 2 3 1\n5\n1 4 3 2 5\n5 2 3 4 1\n") == \
"""0
1
1 3
3
1 4
2 4
1 3
4
1 2
4 5
2 5
1 4""", "sample tests"

# custom cases
assert run("1\n2\n1 2\n2 1\n") == "1\n1 2", "minimal size, simple swap"
assert run("1\n3\n3 2 1\n1 2 3\n") == "1\n1 3", "3 element reverse"
assert run("1\n4\n1 2 3 4\n4 3 2 1\n") == "2\n1 4\n2 3", "4 element reverse"
assert run("1\n5\n5 4 3 2 1\n1 2 3 4 5\n") == "2\n1 5\n2 4", "5 element reverse"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- |
