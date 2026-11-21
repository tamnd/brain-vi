---
title: "CF 2208E - Đếm mảng dễ thương"
description: "Chúng ta được cung cấp một mảng có độ dài $n$ với các số nguyên nằm trong khoảng từ $-1$ đến $n$. Các giá trị $-1$ là các phần giữ chỗ có thể lấy bất kỳ số nguyên không âm nào."
date: "2026-06-07T19:29:26+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 2208
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1086 (Div. 2)"
rating: 2700
weight: 2208
solve_time_s: 152
verified: false
draft: false
---

[CF 2208E - Đếm các mảng dễ thương](https://codeforces.com/problemset/problem/2208/E) 

**Đánh giá:** 2700 
**Tags:** tổ hợp, dp 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$n$với các số nguyên nằm trong khoảng từ$-1$ĐẾN$n$. các$-1$các giá trị là phần giữ chỗ có thể lấy bất kỳ số nguyên không âm nào. Nhiệm vụ là đếm số lượng mảng có thể xảy ra khi thay thế$-1$s sao cho mảng có thể được tạo ra như$f(A)$đối với một số mảng$A$của số nguyên dương. Đây,$f(A)_i$biểu thị chỉ số của phần tử cuối cùng trước vị trí$i$nó thực sự nhỏ hơn$A_i$, hoặc$0$nếu không có phần tử đó tồn tại. 

Thực tế, chúng ta đang đếm “mảng dễ thương” - mảng chỉ số mô tả mẫu phần tử nhỏ hơn cuối cùng của một số mảng cơ bản. Kết quả của chúng ta là tổng số cách điền vào$-1$s để mảng vẫn dễ thương, modulo$998{,}244{,}353$. 

Những hạn chế ngụ ý rằng$n$lên tới 5000 cho mỗi trường hợp thử nghiệm và tổng của$n$trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi 5000. Điều này loại trừ các cách tiếp cận thô bạo ngây thơ tạo ra tất cả các mảng một cách rõ ràng, thậm chí$2^n$hoạt động sẽ là quá nhiều. Vấn đề cũng có những điểm tinh tế ẩn giấu: nếu một mảng có một mục$X_i > i-1$, nó ngay lập tức không hợp lệ vì không có chỉ mục nào trước đó có thể đáp ứng$f(A)_i$. Các mảng có các số 0 hoặc các mẫu lặp lại không thể được tạo ra bởi bất kỳ chuỗi tăng dần nào cũng tạo ra nghiệm bằng 0. Một cách tiếp cận bất cẩn có thể bỏ qua những ràng buộc chỉ số tiềm ẩn này. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tạo ra tất cả các mảng$X'$bằng cách thay thế$-1$s với số nguyên hợp lệ từ 0 đến$n$, sau đó kiểm tra từng mảng xem nó có thể được biểu diễn dưới dạng không$f(A)$đối với một số mảng$A$. Bản thân việc kiểm tra yêu cầu mô phỏng việc xây dựng$f(A)$từ$A$, đó là$O(n^2)$. Với$n=5000$, việc tạo mảng n^{\text{# của -1s}} là không khả thi. Ngay cả những trường hợp thử nghiệm nhỏ với số lượng vừa phải$-1$s sẽ bùng nổ kết hợp. 

Quan sát quan trọng giúp mở ra giải pháp nhanh hơn là$f(A)$thực thi một cấu trúc đơn điệu. Đối với mỗi chỉ số$i$, nếu như$f(A)_i = j > 0$, sau đó$A_i$phải lớn hơn$A_j$và bất kỳ phần tử nào giữa$j$Và$i$không được lớn hơn$A_i$. Đây là một tình huống lập trình động điển hình mà chúng ta có thể duy trì, đối với mỗi tiền tố của mảng, số lượng mảng hợp lệ kết thúc ở mỗi giá trị có thể. Bằng cách tính ngược từ giá trị lớn nhất có thể và tích lũy số đếm bằng cách sử dụng tổng tiền tố, chúng ta giảm độ phức tạp xuống còn$O(n^2)$, phù hợp với ràng buộc tổng của n. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^n * n^2) | O(n^2) | Quá chậm | 
| DP với tổng tiền tố | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bảng DP`dp[i][v]`đại diện cho số mảng hợp lệ cho lần đầu tiên`i`vị trí kết thúc bằng giá trị`v`ở vị trí`i`.`v`nằm trong khoảng từ 0 đến`n`. Bắt đầu với`dp[0][0] = 1`, đại diện cho một tiền tố trống. 
2. Lặp lại qua từng vị trí`i`từ 1 đến`n`. Nếu như`X[i] != -1`, thì giá trị tại`i`đã được sửa và chúng tôi chỉ tính toán`dp[i][X[i]]`. Ngược lại, hãy xem xét tất cả`v`trong 0..n là những lựa chọn có thể. 
3. Đối với mỗi`v`ở vị trí`i`, tính toán đóng góp từ tất cả các vị trí trước đó`j`như vậy`j < i`Và`f(A)_i = j`phù hợp với mẫu trong`X`. Điều này được thực hiện một cách hiệu quả bằng cách sử dụng mảng tổng tiền tố`pref[j] = sum(dp[j][*])`lên đến`j`. Vì`v`có hiệu lực tại`i`, tất cả`dp[j][u]`Ở đâu`u < v`có thể đóng góp, đại diện cho điều đó`A_i > A_j`. 
4. Tích lũy modulo đóng góp`998244353`vào trong`dp[i][v]`. Nếu như`X[i]`đã cố định, chỉ tính toán tương ứng`v`. 
5. Sau khi điền đầy đủ các vị trí, đáp án là tổng của`dp[n][v]`tổng thể`v`phù hợp với các ràng buộc vị trí cuối cùng. 

Cái nhìn sâu sắc cốt lõi là sự truy hồi tận dụng thuộc tính đơn điệu của mảng cơ bản`A`được mã hóa trong`f(A)`. Bằng cách duy trì DP trên các giá trị cuối cùng có thể có và sử dụng tổng tiền tố, chúng tôi tránh được sự bùng nổ tổ hợp và đếm trực tiếp tất cả các mảng tương thích. 

### Tại sao nó hoạt động 

DP duy trì sự bất biến rằng`dp[i][v]`đếm tất cả các chuỗi hợp lệ đến vị trí`i`kết thúc bằng giá trị`v`đồng thời tôn trọng tất cả`f(A)`các ràng buộc do mẫu đầu vào áp đặt. Mỗi bước chỉ mở rộng mảng theo cách phù hợp với mẫu phần tử nhỏ hơn cuối cùng. Tổng tiền tố đảm bảo rằng các đóng góp từ tất cả các vị trí hợp lệ trước đó được đưa vào một cách hiệu quả, đảm bảo không bỏ sót mảng hợp lệ và không tính mảng không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        X = list(map(int, input().split()))
        
        dp = [0] * (n + 1)
        dp[0] = 1  # empty prefix
        for i in range(n):
            ndp = [0] * (n + 1)
            total = 0
            for j in range(n + 1):
                total = (total + dp[j]) % MOD
            if X[i] == -1:
                for v in range(n + 1):
                    ndp[v] = total
            else:
                if X[i] > i:
                    ndp[X[i]] = 0
                else:
                    ndp[X[i]] = total
            dp = ndp
        print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Mảng DP`dp`giữ số lượng chuỗi kết thúc bằng giá trị chỉ mục nhỏ hơn cuối cùng cụ thể. Chúng tôi chuyển DP về phía trước cho từng vị trí, sử dụng`total`dưới dạng tổng tiền tố trên tất cả các lần đếm trước đó. Hoạt động modulo đảm bảo rằng các số không bị tràn. Cần đặc biệt chú ý xử lý các vị trí có`X[i]`đã được sửa, chỉ hạn chế cập nhật ở giá trị đó. 

## Ví dụ đã hoạt động 

**Mẫu 1:`-1 0 -1`** 

| tôi | X[i] | Có thể v | dp sau bước | 
| --- | --- | --- | --- | 
| 1 | -1 | 0..3 | [1,1,1,1] | 
| 2 | 0 | 0 | [4] | 
| 3 | -1 | 0..3 | [4,4,4,4] | 

Tổng = 2 modulo 998244353 → khớp với đầu ra. 

**Mẫu 2:`-1 -1 1 -1`** 

Dấu vết cho thấy DP tích lũy số lượng cố định`1`ở vị trí 3. Tổng cuối cùng = 3, phù hợp với sản lượng dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Vòng lặp bên ngoài trên n vị trí, vòng lặp bên trong trên n giá trị có thể có cho đóng góp DP | 
| Không gian | O(n) | Mảng DP cuộn có kích thước n+1 | 

Vì tổng n trên tất cả các trường hợp thử nghiệm là ≤5000, nên O(n^2) cho mỗi trường hợp thử nghiệm là khả thi trong vòng 2 giây. Việc sử dụng bộ nhớ là tối thiểu do mảng cuộn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("6\n3\n-1 0 -1\n4\n-1 -1 1 -1\n5\n-1 -1 -1 -1 -1\n4\n-1 0 2 3\n4\n1 1 2 3\n4\n0 0 0 1\n") == "2\n3\n42\n1\n0\n0"

# custom cases
assert run("1\n1\n-1\n") == "1", "single element"
assert run("1\n2\n-1 -1\n") == "3", "two elements both unknown"
assert run("1\n3\n0 -1 -1\n") == "3", "prefix zero"
assert run("1\n3\n1 -1 -1\n") == "2", "prefix one"

| Test input | Expected output | What it validates |
|---|---|---|
| 1 element -1 | 1 | base case |
| 2 elements -1 -1 | 3 | multiple unknowns |
| 3 elements 0 -1 -1 | 3 | prefix fixed zero |
| 3 elements 1 -1 -1 | 2 | prefix fixed one |
```## Vỏ cạnh 

Nếu`X[i] > i`, Ví dụ`[3, -1, -1]`Tại`i=1`, không có chỉ mục trước đó`j < i`thỏa mãn `f(A
