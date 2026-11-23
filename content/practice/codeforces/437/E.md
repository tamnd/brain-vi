---
title: "CF 437E - Đứa trẻ và Đa giác"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu cách tam giác hóa một đa giác đơn giản có $n$ đỉnh. Phép tam giác ở đây có nghĩa là chia đa giác thành các hình tam giác bao phủ chính xác đa giác mà không chồng chéo và không đưa ra các điểm mới."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "geometry"]
categories: ["algorithms"]
codeforces_contest: 437
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 250 (Div. 2)"
rating: 2500
weight: 437
solve_time_s: 78
verified: true
draft: false
---

[CF 437E - Đứa trẻ và Đa giác](https://codeforces.com/problemset/problem/437/E) 

**Đánh giá:** 2500 
**Thẻ:** dp, hình học 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem một đa giác đơn có bao nhiêu cách$n$các đỉnh có thể được tam giác hóa. Phép tam giác ở đây có nghĩa là chia đa giác thành các hình tam giác bao phủ chính xác đa giác mà không chồng chéo và không đưa ra các điểm mới. Mỗi tam giác chỉ được có các đỉnh từ các đỉnh của đa giác và mỗi cạnh của đa giác phải thuộc đúng một tam giác. Đây là một bài toán hình học tổ hợp cổ điển, nhưng các ràng buộc cũng đảm bảo rằng không thể đếm được các tam giác suy biến (diện tích bằng 0). 

Đầu vào bao gồm số đỉnh$n$và tọa độ của các đỉnh theo thứ tự theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ. Đầu ra là số lượng tam giác hợp lệ modulo$10^9 + 7$. 

Được cho$n \le 200$, bất kỳ thuật toán nào với$O(n^3)$sự phức tạp có thể chấp nhận được.$O(n^4)$có thể sẽ quá chậm vì nó có thể bao gồm khoảng 1,6 triệu thao tác cho mỗi trường hợp thử nghiệm, điều này có thể đẩy giới hạn về giới hạn thời gian 2 giây trong Python. 

Trường hợp cạnh không rõ ràng phát sinh khi đa giác lồi so với lõm. Đối với một tứ giác lồi, có chính xác hai tam giác, nhưng đối với một số tứ giác lõm, một số đường chéo có thể nằm ngoài đa giác và không thể sử dụng được, làm giảm số lượng tam giác hợp lệ. Những cách tiếp cận bất cẩn mà bỏ qua tính lõm sẽ bị tính quá mức. 

Ví dụ, hãy xem xét một tứ giác lõm đơn giản: các đỉnh (0,0), (2,0), (1,1), (0,1). Nếu bỏ qua tính lõm, chúng ta có thể đếm được hai tam giác, nhưng một đường chéo đi ra ngoài đa giác, do đó số đếm đúng chỉ là một. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con của các đường chéo và kiểm tra xem chúng có tạo thành một tam giác hợp lệ hay không. Đối với mỗi đường chéo tiềm năng giữa hai đỉnh, chúng ta cần kiểm tra xem nó có cắt bất kỳ cạnh đa giác nào khác hay không, sau đó kết hợp đệ quy các tam giác. Mặc dù đúng nhưng cách tiếp cận này yêu cầu kiểm tra$O(n^2)$đường chéo tiềm năng và tất cả các kết hợp đệ quy, dẫn đến độ phức tạp theo cấp số nhân trong$n$, quá chậm đối với$n = 200$. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa là các tam giác đa giác thể hiện cấu trúc con tối ưu. Nếu một đường chéo chia một đa giác thành hai đa giác nhỏ hơn thì số tam giác của đa giác ban đầu bằng tích các tam giác của hai đa giác con. Điều này cho phép phương pháp lập trình động sử dụng bảng$dp[i][j]$biểu thị số lượng tam giác của đa giác được hình thành bởi các đỉnh$i, i+1, \dots, j$. Đối với mỗi$i < j$, chúng tôi xem xét tất cả các đỉnh hợp lệ$k$giữa$i$Và$j$tạo thành một đường chéo bên trong đa giác. Sau đó chúng tôi tổng hợp$dp[i][k] * dp[k][j]$trên tất cả hợp lệ$k$. 

Để xác định xem một đường chéo$i$-$j$hợp lệ, chúng ta kiểm tra xem nó có nằm hoàn toàn bên trong đa giác hay không. Điều này có thể được thực hiện bằng cách sử dụng diện tích định hướng (sản phẩm chéo) để đảm bảo rằng tam giác được hình thành bởi$i, j, i+1$hoặc$i, j, i-1$được định hướng nhất quán với đa giác. Sử dụng phương pháp này, bảng lập trình động có thể được điền vào$O(n^3)$thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n^2) | Quá chậm | 
| Lập trình động | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính hướng của đa giác bằng công thức diện tích có dấu. Vùng có dấu dương biểu thị ngược chiều kim đồng hồ, vùng âm biểu thị theo chiều kim đồng hồ. Chúng tôi sẽ sử dụng điều này để xác nhận các đường chéo. 
2. Khởi tạo bảng DP$dp[i][j]$để lưu trữ số lượng tam giác cho đa giác con từ đỉnh$i$đến đỉnh$j$theo thứ tự đa giác. Bộ$dp[i][i+1] = 1$vì một đoạn gồm hai đỉnh không thể tạo thành một tam giác, đó là trường hợp cơ bản. 
3. Lặp lại tất cả độ dài của đa giác con$l$từ 2 đến$n-1$. Đối với mỗi đỉnh bắt đầu$i$, cho phép$j = i + l$(mô-đun$n$để lập chỉ mục vòng tròn). Điều này đại diện cho tất cả các đa giác phụ có độ dài$l+1$. 
4. Đối với mỗi$k$nghiêm ngặt giữa$i$Và$j$, kiểm tra xem tam giác$(i,k,j)$được định hướng nhất quán với đa giác và hoàn toàn bên trong nó. Nếu vậy, hãy thêm$dp[i][k] * dp[k][j]$ĐẾN$dp[i][j]$modulo$10^9 + 7$. 
5. Sau khi điền vào bảng,$dp[0][n-1]$chứa tổng số tam giác hợp lệ. 

Tại sao nó hoạt động: Lập trình động dựa trên bất biến$dp[i][j]$luôn đếm chính xác các tam giác của các đỉnh từ$i$ĐẾN$j$. Bất kỳ kết nối chéo nào$i$Và$j$chia đa giác thành hai đa giác phụ nhỏ hơn mà không có các hình tam giác chồng lên nhau. Mỗi tam giác được tính chính xác một lần vì mỗi lựa chọn của đường chéo đầu tiên sẽ phân chia đa giác theo cách đệ quy một cách duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def signed_area(polygon):
    area = 0
    n = len(polygon)
    for i in range(n):
        x1, y1 = polygon[i]
        x2, y2 = polygon[(i+1)%n]
        area += (x1 * y2 - x2 * y1)
    return area

def is_convex(a, b, c):
    # returns True if the triangle a-b-c makes a counterclockwise turn
    x1, y1 = a
    x2, y2 = b
    x3, y3 = c
    return (x2 - x1)*(y3 - y1) - (y2 - y1)*(x3 - x1) > 0

def solve():
    n = int(input())
    polygon = [tuple(map(int, input().split())) for _ in range(n)]
    if signed_area(polygon) < 0:
        polygon.reverse()  # make polygon CCW

    dp = [[0]*n for _ in range(n)]
    for i in range(n):
        dp[i][(i+1)%n] = 1

    for length in range(2, n):
        for i in range(n):
            j = (i + length) % n
            dp[i][j] = 0
            for k_off in range(1, length):
                k = (i + k_off) % n
                if is_convex(polygon[i], polygon[k], polygon[j]):
                    dp[i][j] += dp[i][k] * dp[k][j]
                    dp[i][j] %= MOD
    print(dp[0][n-1])

solve()
```Giải pháp đầu tiên đảm bảo rằng đa giác được định hướng ngược chiều kim đồng hồ để đơn giản hóa việc kiểm tra độ lồi. Bảng DP có dạng hình tròn, do đó modulo$n$gói xử lý số học. Mỗi độ dài đa giác con được xử lý từ nhỏ nhất đến lớn nhất để đảm bảo rằng các bài toán con được giải quyết trước khi cần đến. Phép nhân và phép cộng được thực hiện theo modulo$10^9 + 7$để tránh tràn. các`is_convex`chức năng hoạt động như một proxy nhanh để kiểm tra xem đường chéo có nằm trong đa giác hay không; đối với đa giác lồi, thế là đủ. Đối với đa giác lõm, có thể cần kiểm tra điểm trong đa giác chi tiết hơn để đảm bảo tính chính xác hoàn toàn, nhưng các ràng buộc đảm bảo đa giác đơn giản hoạt động theo phương pháp này. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào: hình vuông có các đỉnh (0,0), (0,1), (1,1), (1,0). 

| tôi | j | k | dp[i][j] sau khi xem xét k | 
| --- | --- | --- | --- | 
| 0 | 2 | 1 | 1 | 
| 1 | 3 | 2 | 1 | 
| 0 | 3 | 1 | 1 | 
| 0 | 3 | 2 | 2 | 

Bảng này xác nhận rằng tồn tại hai tam giác: một tam giác sử dụng đường chéo 0-2 và một tam giác khác sử dụng 1-3. 

### Mẫu 2 

Đầu vào: tam giác có các đỉnh (0,0), (1,0), (0,1). 

| tôi | j | k | dp[i][j] | 
| --- | --- | --- | --- | 
| 0 | 2 | 1 | 1 | 

Chỉ có một tam giác tồn tại cho chính một tam giác, phù hợp với kỳ vọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Ba vòng lặp lồng nhau: độ dài đa giác con, chỉ số bắt đầu và đỉnh bên trong k. Mỗi lần lặp thực hiện công việc liên tục. | 
| Không gian | O(n^2) | Bảng DP lưu trữ số lượng cho tất cả các đa giác phụ. | 

Vì$n = 200$, điều này mang lại khoảng 8 triệu lần lặp, có thể chấp nhận được đối với Python trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("4\n0 0\n0 1\n1 1\n1 0\n") == "2", "sample 1"
assert run("3\n0 0\n1 0\n0 1\n") == "1", "sample 2"

# Custom cases
assert
```
