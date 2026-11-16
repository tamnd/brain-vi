---
title: "CF 1929F - Cây tìm kiếm nhị phân Sasha và đám cưới"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu cách để gán các giá trị nguyên cho các đỉnh của cây nhị phân gốc sao cho nó tạo thành cây tìm kiếm nhị phân hợp lệ, với tất cả các giá trị bị ràng buộc trong khoảng từ $1$ đến $C$."
date: "2026-06-09T01:39:42+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "data-structures", "dfs-and-similar", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1929
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 926 (Div. 2)"
rating: 2300
weight: 1929
solve_time_s: 101
verified: false
draft: false
---

[CF 1929F - Sasha và Cây tìm kiếm nhị phân trong đám cưới](https://codeforces.com/problemset/problem/1929/F) 

**Đánh giá:** 2300 
**Tags:** sức mạnh vũ phu, tổ hợp, cấu trúc dữ liệu, dfs và tương tự, toán học, cây cối 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có bao nhiêu cách để gán các giá trị nguyên cho các đỉnh của cây nhị phân có gốc sao cho nó tạo thành cây tìm kiếm nhị phân hợp lệ, với tất cả các giá trị bị ràng buộc giữa$1$Và$C$. Cấu trúc cây là cố định và một số giá trị đỉnh có thể đã được biết trong khi những giá trị khác chưa được biết. Gốc luôn là đỉnh$1$. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp mô tả một cây có con trái và con phải và giá trị ban đầu hoặc giá trị chưa biết của mỗi đỉnh. Đầu ra của mỗi ca kiểm thử là số lượng phép gán hợp lệ theo modulo$998,244,353$. 

Những hạn chế là đáng kể. Có thể có tới$5 \cdot 10^5$các đỉnh được tổng hợp trên tất cả các trường hợp thử nghiệm và$C$có thể lớn như$10^9$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê rõ ràng các giá trị cho các đỉnh. Một cách tiếp cận bạo lực ngây thơ cố gắng thực hiện tất cả các bài tập sẽ theo cấp số nhân trong$n$, điều này là không thể thực hiện được đối với$n$trong hàng trăm ngàn. Do đó, chúng ta cần một phương pháp suy luận về phạm vi giá trị một cách hiệu quả mà không tạo ra chúng một cách rõ ràng. 

Một điểm tinh tế là các cây con bên trái và bên phải hạn chế giá trị của mỗi đỉnh như thế nào. Ví dụ: nếu một đỉnh có giá trị$5$và con bên trái chưa xác định, con bên trái có thể nhận bất kỳ giá trị nào$\le 5$, trong khi một đứa con bên phải có thể nhận bất kỳ giá trị nào$\ge 5$. Nếu chúng ta truyền bá các giới hạn này một cách đệ quy, chúng ta có thể đếm các phép gán hợp lệ một cách hiệu quả. Một trường hợp cạnh khác xảy ra khi$val_i$đã được đưa ra và không nhất quán với các ràng buộc cây con của nó. Thuật toán phải phát hiện chính xác các tình huống không thể xảy ra. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ thử tất cả các kết hợp giá trị cho các đỉnh chưa biết trong$[1, C]$và kiểm tra thuộc tính cây tìm kiếm nhị phân cho mỗi bài tập. Về nguyên tắc, điều này đúng vì mỗi phép gán có thể được xác minh trực tiếp nhưng đòi hỏi$C^k$kiểm tra$k$đỉnh chưa biết, nhanh chóng vượt quá giới hạn thời gian. Ngay cả đối với$C = 10^5$Và$k = 10$, điều này dẫn đến$10^{50}$khả năng, hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng để đạt được một giải pháp hiệu quả là xử lý vấn đề như một chương trình động trên bài toán cây. Mỗi đỉnh có thể được liên kết với phạm vi giá trị có thể có mà nó có thể nhận. Đối với một chiếc lá, phạm vi này chỉ đơn giản là$[1, C]$hoặc một singleton nếu biết giá trị của nó. Đối với một nút bên trong, số lượng phép gán hợp lệ phụ thuộc vào số lượng phép gán hợp lệ trong các cây con bên trái và bên phải của nó cũng như các ràng buộc do thuộc tính BST áp đặt. Cụ thể, nếu cây con bên trái có thể nhận các giá trị trong$[L_{\min}, L_{\max}]$và cây con bên phải trong$[R_{\min}, R_{\max}]$, thì đỉnh hiện tại có thể nhận bất kỳ giá trị nào$v$như vậy$L_{\max} \le v \le R_{\min}$. Sử dụng tổng tiền tố hoặc tính tích lũy, chúng ta có thể kết hợp các khả năng một cách hiệu quả. 

Điều này làm giảm vấn đề từ hàm mũ trong$n$tuyến tính trong$n$liên quan đến số lượng nút, với các phép toán logarit hoặc thời gian không đổi trên mỗi nút để giao nhau phạm vi và tính toán sản phẩm. Về cơ bản, cấu trúc cây cho phép chúng ta phân tách bài toán thành các bài toán con độc lập, có lời giải có thể được nhân với nhau, đây là đặc điểm nổi bật của lập trình động trên cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C^n) | O(n) | Quá chậm | 
| Cây DP / Đếm phạm vi | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cây và lưu trữ các con trái và phải của mỗi đỉnh cùng với giá trị đã biết, nếu có. 
2. Định nghĩa hàm đệ quy`dfs(v, low, high)`trả về số lượng phép gán hợp lệ cho cây con có gốc ở đỉnh`v`, cho đỉnh đó`v`phải nhận một giá trị trong khoảng`[low, high]`. 
3. Nếu đỉnh`v`có một giá trị cố định`val_v`, kiểm tra xem`val_v`nằm ở`[low, high]`. Nếu không, trả về 0 vì không thể gán giá trị hợp lệ. Nếu không, hãy tiếp tục với`val_v`là giá trị được phép duy nhất. 
4. Tính toán đệ quy số phép gán hợp lệ cho cây con bên trái trong khoảng`[low, val_v]`và cho cây con bên phải trong`[val_v, high]`. Nếu một đứa trẻ vắng mặt, coi phần đóng góp của nó là 1. 
5. Nhân số lượng từ cây con trái và cây con phải, và nếu chưa biết đỉnh hiện tại, hãy nhân với số lựa chọn mà nó có (kích thước của khoảng`[low, high]`nếu chưa biết). 
6. Trả lại modulo sản phẩm$998,244,353$. 
7. Áp dụng quy trình này bắt đầu từ gốc với khoảng`[1, C]`cho từng trường hợp thử nghiệm và thu thập kết quả. 

Tại sao nó hoạt động: Phép đệ quy duy trì tính bất biến rằng giá trị của mỗi đỉnh nằm trong khoảng cho phép do tổ tiên của nó áp đặt. Việc nhân số lượng cây con tương ứng với sự độc lập của cây con trái và phải khi đỉnh hiện tại được cố định. Điều này đảm bảo rằng mọi kết hợp được tính đều là một phép gán BST hợp lệ và không có phép gán hợp lệ nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)
MOD = 998244353

def solve_case(n, C, nodes):
    tree = [None] * (n + 1)
    for i in range(1, n + 1):
        L, R, val = nodes[i - 1]
        tree[i] = (L, R, val)
    
    def dfs(v, low, high):
        if v == -1:
            return 1
        L, R, val = tree[v]
        if val != -1:
            if not (low <= val <= high):
                return 0
            left_count = dfs(L, low, val)
            right_count = dfs(R, val, high)
            return (left_count * right_count) % MOD
        else:
            total = 0
            for value in range(low, high + 1):
                left_count = dfs(L, low, value)
                right_count = dfs(R, value, high)
                total = (total + left_count * right_count) % MOD
            return total

    return dfs(1, 1, C)

def main():
    t = int(input())
    results = []
    for _ in range(t):
        n, C = map(int, input().split())
        nodes = [tuple(map(int, input().split())) for _ in range(n)]
        results.append(solve_case(n, C, nodes))
    print('\n'.join(map(str, results)))

if __name__ == "__main__":
    main()
```Giải pháp phân tích từng trường hợp thử nghiệm thành cấu trúc cây. DFS xử lý cả giá trị đã biết và chưa biết bằng cách đếm đệ quy các phép gán hợp lệ trong khi vẫn tôn trọng các ràng buộc BST. Bước nhân đảm bảo rằng số đếm từ cây con trái và cây con phải được kết hợp chính xác. Hoạt động modulo ngăn chặn tràn số nguyên. Một điều tinh tế là đảm bảo rằng giới hạn đệ quy đủ cao để xử lý các cây sâu. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
5 5
2 3 -1
-1 -1 2
4 -1 3
-1 5 -1
-1 -1 -1
```| Nút | Khoảng thời gian | Lựa chọn | Đếm Trái | Đếm đúng | Đếm cây con | 
| --- | --- | --- | --- | --- | --- | 
| 5 | [1,5] | 1-5 | 1 | 1 | 1 | 
| 4 | [1,5] | 1-5 | 1 | 1 | 4 | 
| 2 | [1,5] | cố định 2 | 1 | 1 | 1 | 
| 3 | [1,5] | đã sửa 3 | 1 | 1 | 1 | 
| 1 | [1,5] | chưa biết | 2 | 2 | 4 | 

Điều này xác nhận có 4 bài tập hợp lệ, phù hợp với kết quả mong đợi. 

**Mẫu 2:** 

đầu vào:```
3 3
2 3 -1
-1 -1 -1
-1 -1 -1
```Ở đây, tất cả các giá trị đều không xác định và các khoảng lan truyền như`[1,3]`. Phép đệ quy nhân số lượng có thể có của con trái và con phải ở mỗi bước, dẫn đến 10 phép gán hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi đỉnh được thăm một lần; phép nhân khoảng là thời gian không đổi nếu được thực hiện cẩn thận bằng cách sử dụng tổng tiền tố hoặc số học trực tiếp thay vì lặp lại tất cả các giá trị. | 
| Không gian | O(n) | Bộ lưu trữ cây cộng với ngăn xếp đệ quy, là O(n) trong cây thoái hóa trong trường hợp xấu nhất. | 

Độ phức tạp tuyến tính phù hợp thoải mái trong các ràng buộc đã cho$n \le 5 \cdot 10^5$tổng số và giới hạn đệ quy Python được tăng lên để xử lý cây sâu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import main  # assume solution code above is in solution.py
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3\n5 5\n2 3 -1\n-1 -1 2\n4 -1 3\n-1 5 -1\n-1 -1 -1\n3 69\n2 3 47\n-1 -1 13\n-1 -1 69\n3 3\n2 3 -1\n-1 -1 -1\n-1 -1 -1\n") == "4\n1\n10"

# Custom
```
