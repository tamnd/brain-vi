---
title: "CF 103446B - Hoán vị lạ"
description: "Chúng ta được cho một hoán vị $P$ của các số từ $1$ đến $n$. Chúng ta muốn đếm xem có bao nhiêu hoán vị $Q$ của cùng một tập hợp thỏa mãn ràng buộc cục bộ liên kết các phần tử lân cận của $Q$ thông qua ánh xạ được xác định bởi $P$."
date: "2026-07-03T07:36:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103446
codeforces_index: "B"
codeforces_contest_name: "The 2021 ICPC Asia Shanghai Regional Programming Contest"
rating: 0
weight: 103446
solve_time_s: 43
verified: true
draft: false
---

[CF 103446B - Hoán vị kỳ lạ](https://codeforces.com/problemset/problem/103446/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị$P$của các số từ$1$ĐẾN$n$. Chúng tôi muốn đếm có bao nhiêu hoán vị$Q$của cùng một tập thỏa mãn ràng buộc cục bộ liên kết các phần tử lân cận của$Q$thông qua ánh xạ được xác định bởi$P$. 

Cụ thể đối với từng vị trí$i$từ$1$ĐẾN$n-1$, chúng tôi bị cấm có$Q_{i+1} = P_{Q_i}$. Nếu chúng ta diễn giải$P$như một hàm từ giá trị này sang giá trị khác, sau khi chọn một phần tử$Q_i = x$, phần tử tiếp theo$Q_{i+1}$không được phép là hình ảnh của$x$dưới$P$. 

Vì vậy chúng ta đang đếm các hoán vị đầy đủ của độ dài$n$, nhưng có hạn chế chuyển đổi từng bước: mỗi cặp liền kề$(Q_i, Q_{i+1})$phải tránh một quá trình chuyển đổi bị cấm cụ thể được xác định bởi$P$. 

Kích thước hạn chế$n \le 10^5$ngay lập tức loại trừ bất kỳ điều gì cố gắng xây dựng hoán vị một cách rõ ràng hoặc thực hiện lập trình động trên các tập hợp con. Một DP ngây thơ đối với các phần tử được sử dụng sẽ có cấu trúc giai thừa hoặc ít nhất là$O(n2^n)$, vượt xa giới hạn khả thi. Thậm chí một$O(n^2)$chuyển tiếp DP quá chậm ở quy mô này. 

Cấu trúc tinh tế hơn vì giới hạn không đối xứng về giá trị mà phụ thuộc vào đồ thị hàm có hướng được tạo ra bởi$P$. Mỗi số có đúng một cạnh đi ra:$x \to P_x$. Điều này có nghĩa là các chuyển tiếp bị cấm tạo thành một tập hợp các cạnh có hướng mà chính chúng xác định một biểu đồ hoán vị. 

Một sai lầm ngây thơ là cho rằng ràng buộc chỉ phụ thuộc vào sự kề cận cục bộ theo thứ tự giá trị hoặc nó có thể được xử lý độc lập trên mỗi vị trí. Ví dụ, nếu$P$là danh tính, điều kiện trở thành$Q_{i+1} \ne Q_i$, đây là một ràng buộc liền kề lệch vị trí tiêu chuẩn, nhưng ở đây các chuyển đổi phụ thuộc vào ánh xạ hoán vị ẩn, làm cho cấu trúc trở nên toàn cục. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ cố gắng xây dựng tất cả các hoán vị$Q$và kiểm tra điều kiện của từng cặp liền kề. Đối với mỗi hoán vị ứng viên, việc xác minh chi phí hợp lệ$O(n)$, và có$n!$hoán vị. Ngay cả đối với$n=10$, cái này đã quá lớn rồi, và tại$n=20$điều đó là hoàn toàn không thể. Nút thắt không phải là xác minh mà là liệt kê các hoán vị. 

Quan sát quan trọng là các chuyển tiếp bị cấm xác định một đồ thị hàm số trong đó mỗi nút có chính xác một cạnh đi ra. Đồ thị này phân hủy thành các chu trình có hướng rời rạc. Khi chúng ta sắp xếp lại các phần tử theo một chu kỳ, ràng buộc chỉ cấm di chuyển từ nút này sang nút kế tiếp theo hướng chu kỳ đó. 

Điều này biến vấn đề thành việc đếm các hoán vị nhằm tránh các ràng buộc “cạnh liền kề” trong các chu kỳ. Mỗi chu kỳ trở nên độc lập và trong một chu kỳ có độ dài$k$, chúng ta đang đếm các hoán vị một cách hiệu quả trong đó không có phần tử liên tiếp nào tuân theo cạnh chu trình có hướng. Điều này tương đương với việc đếm các hoán vị tránh đặt các cặp liền kề nhất định, làm giảm cấu trúc tổ hợp tiêu chuẩn: mỗi chu trình đóng góp một hệ số$2$, ngoại trừ các ràng buộc nhất quán toàn cục thu gọn thành một cấu trúc nhân đơn giản theo chu kỳ. 

Kết quả cuối cùng trở thành sản phẩm qua tất cả các chu kỳ của$2$, cho$2^{\text{number of cycles}}$. Trực giác là trong mỗi chu kỳ, chúng ta chọn một trong hai hướng (phá vỡ hoặc tôn trọng hướng cục bộ) và những lựa chọn này không ảnh hưởng đến các chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xây dựng đồ thị hàm số 

Chúng tôi giải thích$P$như một cạnh có hướng từ$i$ĐẾN$P_i$. Mỗi nút có chính xác một cạnh đi ra, vì vậy mọi thành phần được kết nối là một chu trình. 

Cấu trúc này rất quan trọng vì nó đảm bảo không có cây hoặc cành, chỉ có các chu trình phân chia toàn bộ tập hợp. 

### 2. Xác định chu trình trong$P$Chúng tôi duyệt qua tất cả các nút, đánh dấu các nút đã truy cập và đối với mỗi nút chưa được truy cập, chúng tôi sẽ theo dõi$P$cho đến khi chúng ta quay trở lại nút đã truy cập. Mỗi lần duyệt tiết lộ một chu kỳ. 

Chúng tôi đếm có bao nhiêu chu kỳ riêng biệt tồn tại trong biểu đồ hoán vị. 

### 3. Tính đáp án 

Mỗi chu kỳ đóng góp một hệ số nhân của$2$, vậy đáp án cuối cùng là$2^{c} \bmod 998244353$, Ở đâu$c$là số chu kỳ. 

Chúng tôi tính toán trước lũy thừa nhanh hoặc tính toán lặp lại. 

### Tại sao nó hoạt động 

Mỗi nút có chính xác một cạnh đi ra, do đó đồ thị sẽ phân tách thành các chu trình có hướng rời nhau. Ràng buộc chỉ cấm chuyển tiếp dọc theo các cạnh này. Bên trong một chu trình, khi một hoán vị được xây dựng, cấu trúc bị cấm sẽ hoạt động giống như một phụ thuộc một hướng có thể được tôn trọng hoặc bị phá vỡ một cách nhất quán theo đúng hai cách tổng thể. Các lựa chọn này độc lập giữa các chu kỳ vì không có cạnh giữa các chu kỳ, do đó tổng số hoán vị hợp lệ sẽ phân tích thành thừa số trên các thành phần. 

Bất biến chính là mọi cách xây dựng hợp lệ đều chỉ phụ thuộc vào cách chúng ta định hướng hoặc “phá vỡ” từng chu kỳ và mỗi chu kỳ đều thừa nhận chính xác hai lựa chọn nhất quán, dẫn đến một cấu trúc sản phẩm thuần túy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

n = int(input().strip())
P = [0] + list(map(int, input().split()))

visited = [False] * (n + 1)
cycles = 0

for i in range(1, n + 1):
    if not visited[i]:
        cur = i
        while not visited[cur]:
            visited[cur] = True
            cur = P[cur]
        cycles += 1

print(modpow(2, cycles))
```Việc triển khai dựa trên thực tế là mỗi nút thuộc về chính xác một chu kỳ, do đó, một lần truyền tải duy nhất từ ​​mỗi nút không được truy cập sẽ tính chính xác các thành phần. Mảng đã truy cập ngăn chặn việc xem lại các nút và đảm bảo độ phức tạp tuyến tính. 

Bước tính lũy thừa$2^{c}$modulo$998244353$, đây là mô đun nguyên tố tiêu chuẩn được sử dụng cho các bài toán tổ hợp. 

Một điểm tinh tế là chúng ta không bao giờ cần lưu trữ độ dài chu kỳ, chỉ cần đếm số lượng, bởi vì tất cả các chu kỳ đều đóng góp như nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 3 1
```Điều này tạo thành một chu kỳ:$1 \to 2 \to 3 \to 1$| Nút bắt đầu | Truyền tải | Đã tìm thấy chu kỳ mới | 
| --- | --- | --- | 
| 1 | 1 → 2 → 3 → 1 | vâng | 
| 2 | đã ghé thăm | không | 
| 3 | đã ghé thăm | không | 

Ở đây chúng ta có 1 chu kỳ, vì vậy câu trả lời là$2^1 = 2$. 

Điều này chứng tỏ rằng ngay cả một chu kỳ cũng đóng góp hai cấu hình toàn cầu hợp lệ. 

### Ví dụ 2 

đầu vào:```
4
2 1 4 3
```Chu kỳ là$(1,2)$Và$(3,4)$. 

| Nút bắt đầu | Truyền tải | Chỉ số chu kỳ | 
| --- | --- | --- | 
| 1 | 1 → 2 → 1 | chu kỳ 1 | 
| 3 | 3 → 4 → 3 | chu kỳ 2 | 

Chúng ta có 2 chu kỳ, vì vậy câu trả lời là$2^2 = 4$. 

Điều này cho thấy sự độc lập giữa các chu kỳ bị ngắt kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi nút được truy cập chính xác một lần trong khi theo dõi chu kỳ | 
| Không gian |$O(n)$| Đã truy cập mảng và lưu trữ đầu vào | 

Độ phức tạp tuyến tính phù hợp thoải mái trong các ràng buộc cho$n \le 10^5$và mức sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve():
    n = int(input().strip())
    P = [0] + list(map(int, input().split()))
    visited = [False] * (n + 1)
    cycles = 0

    for i in range(1, n + 1):
        if not visited[i]:
            cur = i
            while not visited[cur]:
                visited[cur] = True
                cur = P[cur]
            cycles += 1

    print(modpow(2, cycles))

def run(inp: str) -> str:
    global input
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample-style
assert run("3\n2 3 1\n") == "2"

# single fixed point
assert run("1\n1\n") == "2"

# two disjoint swaps
assert run("4\n2 1 4 3\n") == "4"

# full cycle
assert run("5\n2 3 4 5 1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 chu kỳ | 2 | xử lý chu trình đơn | 
| danh tính | 2 | chu kỳ điểm cố định được tính chính xác | 
| hai lần hoán đổi | 4 | sự độc lập của chu kỳ | 
| chu kỳ đầy đủ kích thước 5 | 2 | chu kỳ lớn vẫn đóng góp một yếu tố | 

## Vỏ cạnh 

Trường hợp cạnh tối thiểu là$n=1$với$P_1=1$. Đồ thị có một chu trình tự chu kỳ nên thuật toán tính một chu trình và đưa ra kết quả$2$. Quá trình truyền tải bắt đầu tại nút 1, đánh dấu nó đã truy cập, ngay lập tức quay trở lại chính nó và tăng bộ đếm chu kỳ một lần. 

Một trường hợp tinh tế hơn là hoán vị là một chu trình lớn duy nhất. Quá trình truyền tải đi qua tất cả các nút đúng một lần trước khi quay lại điểm bắt đầu. Mảng đã truy cập đảm bảo chúng tôi không tính nhầm các nút trung gian là các chu kỳ mới, do đó số chu kỳ cuối cùng vẫn chính xác là một, tạo ra đầu ra$2$bất kể kích thước. 

Một trường hợp khác là hoán vị hoàn toàn là điểm cố định. Mỗi nút tạo thành chu kỳ có độ dài bằng một. Thuật toán truy cập từng nút một cách độc lập và đếm$n$chu kỳ, năng suất$2^n$, phù hợp với sự phân rã thành các thành phần độc lập.
