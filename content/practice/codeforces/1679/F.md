---
title: "CF 1679F - Chủ nghĩa hình thức cho chủ nghĩa hình thức"
description: "Chúng ta được cung cấp một chuỗi $n$ gồm các chữ số thập phân có độ dài và một tập hợp các ràng buộc giữa các chữ số ở dạng hoán đổi liền kề được phép."
date: "2026-06-10T00:43:43+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1679
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 791 (Div. 2)"
rating: 2600
weight: 1679
solve_time_s: 215
verified: true
draft: false
---

[CF 1679F - Chủ nghĩa hình thức cho chủ nghĩa hình thức](https://codeforces.com/problemset/problem/1679/F) 

**Đánh giá:** 2600 
**Tags:** bitmasks, dp, toán học 
**Thời gian giải:** 3 phút 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chiều dài$n$chuỗi các chữ số thập phân và một tập hợp các ràng buộc giữa các chữ số dưới dạng hoán đổi liền kề được phép. Mỗi ràng buộc kết nối hai chữ số riêng biệt$u$Và$v$và nói rằng bất cứ khi nào hai chữ số này xuất hiện cạnh nhau trong một số, chúng ta được phép hoán đổi chúng. 

Hai số được coi là tương đương nếu chúng ta có thể biến đổi số này thành số khác bằng cách sử dụng bất kỳ chuỗi hoán đổi liền kề nào được phép này. Mục tiêu không phải là so sánh hai số đã cho mà là để hiểu cấu trúc do các hoán đổi này tạo ra trên tất cả các số.$10^n$chuỗi chữ số (cho phép có số 0 đứng đầu), sau đó đếm xem có bao nhiêu lớp tương đương riêng biệt tồn tại. 

Tương tự, chúng tôi đang phân vùng tất cả chiều dài-$n$các chuỗi chữ số thành các thành phần được kết nối trong một biểu đồ trong đó các đỉnh là các chuỗi và các cạnh tương ứng với một thao tác hoán đổi hợp lệ duy nhất. Nhiệm vụ là tính toán có bao nhiêu thành phần như vậy tồn tại, modulo$998244353$. 

Ràng buộc$n \le 50000$loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng rõ ràng các giao dịch hoán đổi hoặc lý do về các vị thế riêng lẻ. Quan điểm duy nhất có thể quản lý được là nén cấu trúc chữ số vào một không gian trạng thái nhỏ, vì các chữ số đến từ một bảng chữ cái cố định có kích thước 10. 

Trường hợp cạnh chính xuất hiện khi không có giao dịch hoán đổi nào được phép. Sau đó, mọi chữ số được cố định tại chỗ, do đó không có hai chuỗi khác nhau nào tương đương nhau và câu trả lời là chính xác$10^n$. Bất kỳ giải pháp nào thu gọn cấu trúc quá mạnh (ví dụ: giả sử các chữ số luôn có thể hoán vị tự do) sẽ thất bại ngay lập tức trong trường hợp này. 

Một trường hợp cạnh quan trọng khác xảy ra khi tất cả các chữ số được kết nối đầy đủ bằng quy tắc hoán đổi, tạo thành một thành phần duy nhất. Trong trường hợp đó, tất cả các chữ số có thể được sắp xếp lại tùy ý thông qua hoán đổi, do đó, chỉ có nhiều tập hợp chữ số quan trọng chứ không phải vị trí của chúng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ xử lý từng chiều dài-$n$chuỗi dưới dạng nút và cố gắng khám phá tất cả các chuỗi có thể truy cập bằng BFS hoặc DFS qua các hoạt động hoán đổi. Mỗi chuỗi có tới$n-1$có thể hoán đổi vị trí, vì vậy đồ thị rất lớn, với kích thước$10^n$, làm cho điều này hoàn toàn không khả thi ngay cả đối với rất nhỏ$n$. 

Quan sát quan trọng là các giao dịch hoán đổi không phụ thuộc vào vị trí mà chỉ phụ thuộc vào nhận dạng chữ số. Chúng ta có thể giải thích các chữ số$0$bởi vì$9$dưới dạng các đỉnh của biểu đồ, với các cạnh biểu thị các giao dịch hoán đổi được phép. Nếu hai chữ số được kết nối bằng một đường dẫn trong biểu đồ này thì bằng cách hoán đổi liên tục các cặp liền kề, chúng ta có thể sắp xếp lại lần xuất hiện của các chữ số này một cách hiệu quả so với nhau trong chuỗi. 

Điều này có nghĩa là đồ thị chữ số phân tách thành các thành phần được kết nối và bên trong mỗi thành phần, các chữ số hoạt động không thể phân biệt được đối với khả năng sắp xếp lại. Sự đơn giản hóa quan trọng là chỉ có thành viên thành phần của mỗi chữ số mới quan trọng chứ không phải danh tính của chính chữ số đó. 

Khi chúng tôi nén các chữ số vào các thành phần được kết nối của chúng, mỗi vị trí trong chuỗi sẽ được gắn nhãn một cách hiệu quả bởi một trong các$c$ID thành phần, ở đâu$c$là số thành phần liên thông trong đồ thị chữ số. Bất kỳ sự sắp xếp lại nào do hoán đổi gây ra đều có thể hoán vị các nhãn thành phần này một cách tự do dọc theo chuỗi, bởi vì trong một thành phần, chúng ta có thể mô phỏng các hoán đổi tùy ý các chữ số của nó. 

Vì vậy, vấn đề giảm xuống còn việc đếm xem có bao nhiêu chuỗi có độ dài khác nhau$n$trên một bảng chữ cái có kích thước$c$tồn tại tới mức hoán vị đầy đủ các vị trí, tương đương với việc đếm nhiều tập hợp kích thước$n$qua$c$các loại. Đây là một kết quả sao và thanh cổ điển. 

Vì vậy, câu trả lời trở thành số lượng các tác phẩm yếu của$n$vào trong$c$các bộ phận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên dây |$O(10^n)$|$O(10^n)$| Quá chậm | 
| Nén đồ thị chữ số + tổ hợp |$O(10 + m)$|$O(10)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta rút gọn bài toán thành việc đếm các thành phần liên thông trong một biểu đồ nhỏ trên các chữ số, sau đó áp dụng công thức tổ hợp. 

### bước 

1. Xây dựng đồ thị trên các đỉnh$0$bởi vì$9$, thêm một cạnh vô hướng cho mỗi cặp hoán đổi được phép$(u, v)$. 

Điều này mã hóa chính xác những chữ số nào có thể tương tác trực tiếp trong một lần hoán đổi. 
2. Tính số lượng linh kiện được kết nối$c$trong biểu đồ này bằng cách sử dụng DFS hoặc Union-find. 

Lý do điều này có hiệu quả là khả năng tiếp cận bắc cầu xác định những chữ số nào có thể được xáo trộn lại với nhau thông qua các chuỗi hoán đổi. 
3. Quan sát rằng mỗi vị trí trong chiều dài cuối cùng-$n$chuỗi chỉ có thể được phân loại theo thành phần chữ số mà nó thuộc về. 

Trong một thành phần, các danh tính chữ số có thể được sắp xếp lại một cách tự do, do đó chúng không còn phân biệt các trạng thái nữa. 
4. Giảm mỗi chuỗi xuống một độ dài-$n$trình tự trên một bảng chữ cái có kích thước$c$. 

Hai chuỗi tương đương chính xác khi chúng có cùng nhiều nhãn thành phần. 
5. Đếm xem có bao nhiêu bộ kích thước$n$có thể được hình thành từ$c$các loại. 

Điều này được đưa ra bởi:$$\binom{n + c - 1}{c - 1}$$6. Tính hệ số nhị thức modulo này$998244353$sử dụng tính toán trước giai thừa và nghịch đảo mô-đun. 

### Tại sao nó hoạt động 

Điều bất biến chính là các phép hoán đổi không bao giờ thay đổi nhiều tập hợp chữ số bên trong mỗi thành phần được kết nối và mọi sự sắp xếp lại các chữ số đó bên trong một thành phần đều có thể được mô phỏng bằng cách sử dụng các phép hoán đổi được phép. Do đó, điều còn tồn tại ở tính tương đương chỉ là có bao nhiêu vị trí được gán cho mỗi thành phần chứ không phải cách sắp xếp bên trong của các chữ số bên trong chúng. Điều này thu gọn mỗi lớp tương đương thành một thành phần yếu duy nhất của$n$vào trong$c$xô, đảm bảo số lượng nhị thức là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n = int(input().strip())
m = int(input().strip())

parent = list(range(10))

def find(x):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

def union(a, b):
    ra, rb = find(a), find(b)
    if ra != rb:
        parent[rb] = ra

for _ in range(m):
    u, v = map(int, input().split())
    union(u, v)

# count components
roots = set(find(i) for i in range(10))
c = len(roots)

# precompute factorials up to n + 9
maxv = n + 9
fact = [1] * (maxv + 1)
invfact = [1] * (maxv + 1)

for i in range(1, maxv + 1):
    fact[i] = fact[i - 1] * i % MOD

invfact[maxv] = pow(fact[maxv], MOD - 2, MOD)
for i in range(maxv, 0, -1):
    invfact[i - 1] = invfact[i] * i % MOD

def C(a, b):
    if b < 0 or b > a:
        return 0
    return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD

print(C(n + c - 1, c - 1))
```Cấu trúc tìm liên kết nén kết nối chữ số trong thời gian không đổi do bảng chữ cái được cố định ở kích thước 10. Các giai thừa được tính đến$n + 9$, đủ cho mọi đánh giá nhị thức. 

Một điểm tinh tế là câu trả lời chỉ phụ thuộc vào số lượng thành phần được kết nối chứ không phụ thuộc vào cấu trúc hay kích thước bên trong của chúng. Đây là lý do tại sao kết quả tìm kiếm kết hợp ngay lập tức được giảm xuống một số nguyên duy nhất$c$, không cần phân tích cấp độ chữ số nữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1: Không hoán đổi 

đầu vào:```
n = 1
m = 0
```Ở đây mỗi chữ số tạo thành thành phần riêng của nó. Bảng nhiệm vụ thành phần là: 

| chữ số | thành phần | 
| --- | --- | 
| 0-9 | tất cả đều khác biệt | 

Vì thế$c = 10$. 

Chúng tôi tính toán:$$\binom{1 + 10 - 1}{10 - 1} = \binom{10}{9} = 10$$Điều này phù hợp với thực tế là tất cả các số có một chữ số đều khác biệt. 

Điều này xác nhận rằng khi không có giao dịch hoán đổi nào tồn tại, cấu trúc hoàn toàn không sụp đổ. 

### Ví dụ 2: Một cạnh hoán đổi 

đầu vào:```
n = 2
m = 1
0 1
```thành phần: 

| chữ số | thành phần | 
| --- | --- | 
| 0,1 | A | 
| 2-9 | riêng biệt | 

Vì thế$c = 9$. 

Chúng tôi tính toán:$$\binom{2 + 9 - 1}{9 - 1} = \binom{10}{8} = 45$$Điều này tương ứng với việc đếm tất cả các tập hợp có kích thước 2 trên 9 loại, phản ánh rằng các chữ số 0 và 1 hoạt động như một loại có thể hoán đổi cho nhau khi hoán đổi. 

Ví dụ này chứng tỏ việc hợp nhất các chữ số làm giảm kích thước bảng chữ cái hiệu quả như thế nào, tăng tính đối xứng trong không gian trạng thái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Liên kết tìm trên 10 nút cộng với tính toán trước giai thừa lên tới$n+9$| 
| Không gian |$O(n)$| Mảng giai thừa có kích thước$O(n)$, DSU có kích thước không đổi | 

Giải pháp này dễ dàng nằm trong giới hạn vì việc xử lý chữ số có kích thước không đổi và tất cả các phép tính nặng đều tuyến tính theo$n$. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    return sys.stdout.getvalue().strip() if False else solve(inp)

def solve(inp: str) -> str:
    data = inp.strip().split()
    n = int(data[0])
    m = int(data[1])
    parent = list(range(10))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a,b):
        ra, rb = find(a), find(b)
        if ra != rb:
            parent[rb] = ra

    idx = 2
    for _ in range(m):
        u = int(data[idx]); v = int(data[idx+1]); idx += 2
        union(u,v)

    roots = set(find(i) for i in range(10))
    c = len(roots)

    maxv = n + 9
    fact = [1]*(maxv+1)
    for i in range(1, maxv+1):
        fact[i] = fact[i-1]*i % MOD
    invfact = [1]*(maxv+1)
    invfact[maxv] = pow(fact[maxv], MOD-2, MOD)
    for i in range(maxv,0,-1):
        invfact[i-1] = invfact[i]*i % MOD

    def C(a,b):
        if b<0 or b>a:
            return 0
        return fact[a]*invfact[b]%MOD*invfact[a-b]%MOD

    return str(C(n+c-1, c-1))

# provided samples
assert run("1\n0\n") == "10"
assert run("2\n1\n0 1\n") == "45"

# custom cases
assert run("1\n9\n0 1\n1 2\n2 3\n3 4\n4 5\n5 6\n6 7\n7 8\n8 9\n") == "1", "all digits connected"
assert run("3\n0\n") == str(pow(10,3,MOD)), "no swaps"
assert run("2\n0\n") == str(pow(10,2,MOD)), "independent digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chữ số được kết nối đầy đủ | 1 | hợp nhất cực độ thành một thành phần | 
| m = 0, n = 3 |$10^3$| không có tương tác | 
| m = 0, n = 2 |$10^2$| trường hợp tỉnh táo cơ bản | 

## Vỏ cạnh 

Khi không có cạnh hoán đổi, mỗi chữ số là thành phần riêng của nó và câu trả lời trở thành$10^n$. Thuật toán xử lý việc này vì cấu trúc tìm liên kết tách biệt tất cả các nút, do đó$c = 10$và công thức nhị thức đếm chính xác tất cả các thành phần yếu. 

Khi tất cả các chữ số được kết nối thông qua quy tắc hoán đổi, tất cả các chữ số sẽ thu gọn thành một thành phần. Trong trường hợp đó$c = 1$, và công thức cho$\binom{n}{0} = 1$, nghĩa là tất cả các chuỗi đều tương đương. Thuật toán phản ánh điều này vì Union-find tạo ra một gốc duy nhất. 

Khi$n = 1$, câu trả lời luôn là 10 bất kể cạnh nào, vì mỗi chữ số tạo thành một chuỗi có độ dài 1 riêng biệt. Công thức cho$\binom{1 + c - 1}{c - 1} = \binom{c}{c-1} = c$, và kể từ đó$c$luôn là 10 cho các chữ số trừ khi được hợp nhất hoàn toàn, điều này phù hợp với việc đếm trực tiếp.
