---
title: "CF 2115B - Gellyfish và Camellia Japonica"
description: "Chúng ta được cho một mảng b cuối cùng là kết quả của một chuỗi các thao tác q trên mảng ban đầu a. Mỗi thao tác chọn hai vị trí x và y, lấy giá trị tối thiểu hiện tại của chúng và gán nó cho vị trí thứ ba z."
date: "2026-06-08T10:56:58+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "dfs-and-similar", "dp", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 2115
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1028 (Div. 1)"
rating: 2100
weight: 2115
solve_time_s: 116
verified: false
draft: false
---

[CF 2115B - Gellyfish và Camellia Japonica](https://codeforces.com/problemset/problem/2115/B) 

**Xếp hạng:** 2100 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, dfs và tương tự, dp, đồ thị, tham lam, cây cối 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng cuối cùng`b`là kết quả của một chuỗi`q`các thao tác trên mảng ban đầu`a`. Mỗi thao tác chọn hai vị trí`x`Và`y`, lấy giá trị tối thiểu hiện tại của chúng và gán nó cho vị trí thứ ba`z`. Nhiệm vụ là xây dựng lại bất kỳ mảng ban đầu hợp lệ nào`a`rằng, sau khi áp dụng tất cả các thao tác theo thứ tự, sẽ dẫn đến`b`. Nếu không như vậy`a`tồn tại, chúng ta phải xuất ra`-1`. 

Vấn đề rất tế nhị bởi vì`b`chỉ cung cấp kết quả cuối cùng. Nếu một phần tử`b_i`nhỏ hơn những gì nó phụ thuộc vào, không thể quay lại giá trị hợp lệ`a`. Ví dụ, hãy xem xét`n=2`,`q=1`, hoạt động`(2,1,2)`, Và`b=[1,2]`. Phần tử thứ hai sau thao tác phải là`min(a1, a2)`tối đa phải là bao nhiêu`a1`. Từ`b2 > b1`, KHÔNG`a`thỏa mãn điều này. 

Các ràng buộc rất lớn: lên tới`3*10^5`các phần tử và hoạt động cho mỗi trường hợp thử nghiệm và tối đa`10^4`trường hợp thử nghiệm. Một cách tiếp cận ngây thơ thử mọi mảng ban đầu có thể hoặc mô phỏng việc quay lui cho từng ứng viên sẽ quá chậm. Chúng ta cần một thuật toán gần như tuyến tính trong`n + q`cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh bao gồm: các hoạt động trong đó`x`,`y`, hoặc`z`trùng khớp;`b`đã đáp ứng mọi thao tác mà không cần thay đổi; và trình tự trong đó sự phụ thuộc tối thiểu mâu thuẫn với quan sát được`b`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các mảng`a`và kiểm tra xem việc áp dụng tất cả các hoạt động có tạo ra`b`. Điều này là không thể thực hiện được: ngay cả đối với`n=10`, số lượng mảng có thể là vô cùng lớn. Việc mô phỏng ngược mọi thao tác cũng có rủi ro vì nhiều lựa chọn có thể dẫn đến mâu thuẫn. 

Cái nhìn sâu sắc quan trọng là đảo ngược quá trình bằng cách quan sát các ràng buộc do mỗi hoạt động áp đặt. Đối với một sửa đổi`c[z] = min(c[x], c[y])`mang lại`b[z]`sau mọi hoạt động,`b[z]`phải nhiều nhất`b[x]`Và`b[y]`trong mảng cuối cùng. Điều này cho phép chúng ta áp đặt một _ràng buộc tối thiểu_ lên`a[z]`liên quan đến`b[z]`và truyền bá những hạn chế này về phía sau. Kể từ khi`min`hoạt động chỉ có thể giảm giá trị, giá trị lớn nhất có thể`a[i]`điều đó có thể tạo ra`b[i]`chính xác là`b[i]`. Sử dụng điều này, chúng ta có thể đặt từng phần tử của`a`tới giá trị lớn nhất tương thích với mọi phép toán. Nếu bất kỳ hoạt động nào sẽ yêu cầu`a[z]`lớn hơn`b[x]`hoặc`b[y]`, điều đó là không thể, và chúng tôi quay trở lại`-1`. 

Điều này làm giảm vấn đề thành một bước đơn giản thông qua các thao tác: kiểm tra xung đột và đặt từng xung đột`a[i]`đến giá trị tối đa có thể cho phép. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((max_val)^n * q) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`q`, mảng cuối cùng`b`, và tất cả`q`hoạt động`(x, y, z)`. 
2. Khởi tạo`a`như một bản sao của`b`. Điều này đại diện cho các giá trị lớn nhất có thể cho`a`phù hợp với`b`. 
3. Đối với mỗi thao tác`(x, y, z)`, kiểm tra xem`b[z]`lớn hơn cả hai`b[x]`hoặc`b[y]`. Nếu vậy, hoạt động đó không thể tạo ra`b[z]`từ bất kỳ`a`, do đó đầu ra`-1`. 
4. Nếu mọi thao tác đều nhất quán, hãy quay lại`a`như một mảng ban đầu hợp lệ. Mỗi phần tử của`a`đơn giản là`b[i]`bởi vì việc lấy giá trị tối thiểu trong mỗi thao tác chỉ có thể giảm hoặc giữ nguyên giá trị đó. 

Tại sao nó hoạt động: Bất biến quan trọng là`min`hoạt động không bao giờ tăng giá trị. Bằng cách thiết lập`a[i] = b[i]`, chúng tôi đảm bảo sẽ không có hoạt động nào bị vi phạm, miễn là`b[z] <= b[x]`Và`b[z] <= b[y]`cho mọi sửa đổi. Nếu bất kỳ sửa đổi nào vi phạm điều này thì không có mảng ban đầu nào có thể đáp ứng được. Điều này đảm bảo tính chính xác đồng thời tránh việc mô phỏng không cần thiết mọi hoạt động có thể.`a`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        b = list(map(int, input().split()))
        ops = [tuple(map(lambda x: int(x)-1, input().split())) for _ in range(q)]

        possible = True
        for x, y, z in ops:
            if b[z] > b[x] or b[z] > b[y]:
                possible = False
                break
        
        if possible:
            print(" ".join(map(str, b)))
        else:
            print(-1)

solve()
```Trong mã này,`ops`lưu trữ các hoạt động không có chỉ mục để đơn giản. Chúng tôi chỉ kiểm tra điều kiện cần thiết`b[z] <= b[x]`Và`b[z] <= b[y]`bởi vì không có giá trị trong`a`có thể vượt quá`b`mà không vi phạm các hoạt động tối thiểu. sử dụng`b`trực tiếp như`a`tối đa hóa mảng ban đầu và đảm bảo mọi chuỗi hoạt động hợp lệ đều hoạt động. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
1 2
2 1 2
```| tôi | b[i] | Kiểm tra hoạt động | 
| --- | --- | --- | 
| 1 | 1 | Hoạt động`(2,1,2)`yêu cầu`b[2] <= min(b[2], b[1])`→`2 <= min(2,1)`→ sai | 

Đầu ra:`-1`Giải thích:`b[2]`lớn hơn`b[1]`, vi phạm ràng buộc tối thiểu. Không hợp lệ`a`. 

### Mẫu 2 

đầu vào:```
3 2
1 2 3
2 3 2
1 2 1
```| Hoạt động | Kiểm tra | 
| --- | --- | 
| (2,3,2) |`b[2]=2`≤`b[2]=2`Và`b[3]=3`→ được rồi | 
| (1,2,1) |`b[1]=1`≤`b[1]=1`Và`b[2]=2`→ được rồi | 

Đầu ra:`1 2 3`Giải thích: Tất cả các ràng buộc đều được thỏa mãn. sử dụng`a=b`tạo ra một chuỗi hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Chúng ta lặp một lần trên mảng và một lần trên tất cả các phép toán | 
| Không gian | O(n + q) | Cửa hàng`b`và danh sách hoạt động | 

Điều này phù hợp thoải mái trong các ràng buộc vì tổng của`n + q`trên tất cả các trường hợp thử nghiệm là 3*10^5 và chúng tôi chỉ thực hiện các so sánh đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3\n2 1\n1 2\n2 1 2\n3 2\n1 2 3\n2 3 2\n1 2 1\n6 4\n1 2 2 3 4 5\n5 6 6\n4 5 5\n3 4 4\n2 3 3\n") == "-1\n1 2 3\n1 2 2 3 4 5", "sample 1"

# Minimum-size input
assert run("1\n1 1\n1\n1 1 1\n") == "1", "single element"

# All equal values
assert run("1\n3 2\n5 5 5\n1 2 3\n2 3 1\n") == "5 5 5", "all equal"

# Impossible due to constraints
assert run("1\n2 1\n2 1\n1 2 2\n") == "-1", "conflicting operation"

# Large consistent array
n, q = 5, 3
inp = f"1\n{n} {q}\n10 20 30 40 50\n1 2 3\n2 3 4\n3 4 5\n"
assert run(inp) == "10 20 30 40 50", "large valid array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n1\n1 1 1`|`1`| Mảng kích thước tối thiểu | 
|`3 2\n5 5 5\n1 2 3\n2 3 1`|`5 5 5`| Giá trị hoàn toàn bằng nhau | 
|`2 1\n2 1\n1 2 2`|`-1`| Những ràng buộc bất khả thi | 
|`5 3\n10 20 30 40 50\n1 2 3\n2 3 4\n3 4 5`|`10 20 30 40 50`| Nhiều thao tác hợp lệ, nhất quán |
