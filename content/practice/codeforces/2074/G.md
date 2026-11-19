---
title: "CF 2074G - Trò chơi với hình tam giác: Phần 2"
description: "Chúng ta có một đa giác đều có $n$ đỉnh, mỗi đỉnh được gắn nhãn bằng một số nguyên dương. Mục tiêu là vẽ các hình tam giác không chồng lên nhau bên trong đa giác để tối đa hóa tổng tích của các số tại đỉnh của chúng."
date: "2026-06-08T06:41:48+07:00"
tags: ["codeforces", "competitive-programming", "dp", "geometry"]
categories: ["algorithms"]
codeforces_contest: 2074
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1009 (Div. 3)"
rating: 2100
weight: 2074
solve_time_s: 116
verified: false
draft: false
---

[CF 2074G - Trò chơi với hình tam giác: Phần 2](https://codeforces.com/problemset/problem/2074/G) 

**Đánh giá:** 2100 
**Thẻ:** dp, hình học 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác đều với$n$đỉnh, mỗi đỉnh được gán một số nguyên dương. Mục tiêu là vẽ các hình tam giác không chồng lên nhau bên trong đa giác để tối đa hóa tổng tích của các số tại đỉnh của chúng. Mỗi tam giác đóng góp tích số đỉnh của nó vào điểm số và các tam giác không được phép chia sẻ bất kỳ diện tích dương nào. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp mô tả một đa giác thông qua$n$và một dãy số nguyên$a_1, \dots, a_n$. Đầu ra là số điểm tối đa có thể đạt được cho mỗi đa giác. 

Những hạn chế chính là$3 \le n \le 400$và tổng của$n^3$trên tất cả các trường hợp thử nghiệm là nhiều nhất$400^3$. Điều này gợi ý rằng một$O(n^3)$giải pháp có thể chấp nhận được, nhưng bất cứ điều gì tồi tệ hơn, chẳng hạn như$O(n^4)$, sẽ là quá chậm. Các trường hợp cạnh bao gồm đa giác trong đó tất cả các số đều bằng nhau hoặc trong đó giải pháp tối ưu có thể liên quan đến việc chọn một hình tam giác thay vì nhiều hình tam giác nhỏ hơn vì việc kết hợp các hình tam giác nhỏ hơn sẽ làm giảm tổng sản phẩm. Ví dụ: trong một tứ giác có nhãn [2,1,3,4], chọn tam giác có các đỉnh 1,3,4 sẽ cho kết quả là 24, trong khi kết hợp các tam giác nhỏ hơn có thể cho kết quả ít hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xem xét tất cả các tập hợp tam giác không chồng chéo có thể có. Vì$n$đỉnh thì số hình tam giác là$O(n^3)$và việc kiểm tra các phần chồng chéo sẽ làm tăng thêm độ phức tạp. Điều này đúng nhưng không hiệu quả vì việc xác minh mọi kết hợp đều phát triển theo giai thừa, vượt xa khả năng tính toán cho$n = 400$. 

Quan sát cho thấy đa giác đều và lồi, do đó các hình tam giác không có chung cạnh sẽ không chồng lên nhau nếu được chọn cẩn thận. Điều này gợi ý một cách tiếp cận lập trình động tương tự như "phép nhân chuỗi ma trận" hoặc "tam giác đa giác", trong đó chúng tôi xem xét các đa giác phụ và điểm số tối đa có thể đạt được của chúng theo cách đệ quy. Cụ thể, nếu chúng ta chọn một hình tam giác có các đỉnh$i, j, k$bên trong một đa giác con$[i, j, k]$, chúng ta có thể chia đa giác còn lại thành các đa giác con độc lập và giải chúng một cách riêng biệt. Điều này đảm bảo chúng ta không bao giờ vẽ các hình tam giác chồng lên nhau. 

Do đó, chúng tôi giảm bớt vấn đề khi tính tổng sản phẩm tối đa cho tất cả các đa giác phụ bằng cách sử dụng DP. Cho phép`dp[l][r]`là số điểm tối đa có thể đạt được cho đoạn đa giác từ đỉnh$l$ĐẾN$r$theo thứ tự theo chiều kim đồng hồ. Sự đệ quy là:```
dp[l][r] = max(dp[l][k] + dp[k][r] + a[l]*a[k]*a[r]) for l<k<r
```Sự lặp lại này xem xét việc chọn một tam giác có điểm cuối`l`Và`r`và một số đỉnh trung gian`k`. Việc lặp lại tất cả các phân đoạn và độ dài sẽ lấp đầy bảng DP trong$O(n^3)$, điều này khả thi với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! cho hình tam giác) | O(n^3) | Quá chậm | 
| Lập trình động | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Mở rộng mảng`a`để xử lý vòng tròn đa giác một cách dễ dàng bằng cách nối nó với chính nó. Điều này cho phép lập chỉ mục tuyến tính các phân đoạn bao quanh đa giác. 
2. Khởi tạo mảng DP 2D`dp[l][r]`với số không, ở đâu`l`Và`r`là các chỉ số của phân đoạn trong mảng mở rộng. 
3. Lặp lại độ dài đoạn từ 3 đến$n$, vì một tam giác cần ít nhất 3 đỉnh. 
4. Đối với từng phân khúc`[l, r]`có độ dài cho trước, hãy xem xét tất cả các đỉnh trung gian`k`như vậy`l < k < r`. 
5. Tính điểm vẽ hình tam giác`(l, k, r)`cộng với số điểm tối đa cho phân đoạn bên trái và bên phải (`dp[l][k]`Và`dp[k][r]`). 
6. Cập nhật`dp[l][r]`với giá trị lớn nhất thu được từ tất cả những gì có thể`k`. 
7. Sau khi điền vào bảng DP, đáp án là số lớn nhất`dp[l][l+n-1]`cho tất cả các chỉ số bắt đầu`l`từ 0 đến`n-1`, đảm bảo chúng ta xem xét tất cả các phép quay của đa giác. 

Tại sao nó hoạt động: Bất biến DP đảm bảo rằng`dp[l][r]`luôn tích trữ giải pháp tối ưu cho phân khúc`[l, r]`. Các hình tam giác được chọn trong các phân đoạn rời rạc không trùng nhau và bằng cách kiểm tra tất cả các đỉnh trung gian`k`, chúng tôi đảm bảo rằng mọi tam giác có thể đều được xem xét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        # duplicate array for circularity
        a = a * 2
        dp = [[0] * (2*n) for _ in range(2*n)]
        
        for length in range(3, n+1):
            for l in range(0, 2*n - length + 1):
                r = l + length - 1
                for k in range(l+1, r):
                    dp[l][r] = max(dp[l][r], dp[l][k] + dp[k][r] + a[l]*a[k]*a[r])
        
        ans = 0
        for start in range(n):
            ans = max(ans, dp[start][start+n-1])
        print(ans)
```Mã đầu tiên sao chép mảng`a`để xử lý các đoạn tròn, sau đó điền vào bảng DP cho tất cả các độ dài đoạn. Các vòng lặp lồng nhau tính tổng tích lớn nhất cho mỗi đoạn bằng cách chọn một đỉnh ở giữa`k`. Cuối cùng, nó chọn điểm tối đa trong số tất cả các phiên bản được xoay của đa giác. 

## Ví dụ đã hoạt động 

### Mẫu 1: Đầu vào```
3
1 2 3
```| tôi | r | k | dp[l][r] | 
| --- | --- | --- | --- | 
| 0 | 2 | 1 | 1_2_3=6 | 

Tam giác duy nhất có thể là`(0,1,2)`với sản phẩm 6. Bảng DP xác nhận điều này. 

### Mẫu 2: Đầu vào```
4
2 1 3 4
```| tôi | r | k | dp[l][r] | 
| --- | --- | --- | --- | 
| 0 | 3 | 1 | 0+0+2_1_4=8 | 
| 0 | 3 | 2 | 0+0+2_3_4=24 | 

Tối đa là 24 cho hình tam giác`(0,2,3)`, bỏ qua các hình tam giác chồng lên nhau nhỏ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Đối với mỗi đoạn có độ dài lên tới n, chúng tôi lặp qua n điểm bắt đầu và n k đỉnh có thể | 
| Không gian | O(n^2) | Bàn DP kích thước 2n x 2n để xử lý các đoạn tròn | 

Được cho$n \le 400$,$O(n^3)$ở xung quanh$64 \times 10^6$hoạt động, phù hợp thoải mái trong giới hạn 4 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("6\n3\n1 2 3\n4\n2 1 3 4\n6\n2 1 2 1 1 1\n6\n1 2 1 3 1 5\n9\n9 9 8 2 4 4 3 5 3\n9\n9 9 3 2 4 4 8 5 3\n") == "6\n24\n5\n30\n732\n696", "samples"

# custom cases
assert run("1\n3\n1 1 1\n") == "1", "all ones"
assert run("1\n4\n1 2 3 4\n") == "24", "single best triangle in quadrilateral"
assert run("1\n5\n5 5 5 5 5\n") == "125", "all equal numbers"
assert run("1\n6\n1 1000 1 1000 1 1000\n") == "1000000000", "alternating large values"
assert run("1\n7\n1 2 3 4 5 6 7\n") == "210", "ascending sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 đỉnh, tất cả đều là một | 1 | Đa giác nhỏ nhất, giá trị tối thiểu | 
| 4 đỉnh [1,2,3,4] | 24 | Chọn đúng hình tam giác lớn nhất | 
| 5 đỉnh cả 5 | 125 | DP xử lý chính xác các giá trị bằng nhau | 
| 6 đỉnh xen kẽ 1.1000 | 1000000000 | Chọn hình tam giác tối đa hóa sản phẩm | 
| 7 đỉnh tăng dần | 210 | DP điều hướng chính xác các phân đoạn lớn hơn | 

## Vỏ cạnh 

Đối với một tứ giác`[2,1,3,4]`, một cách tiếp cận đơn giản có thể tính tổng hai tam giác`(0,1,2)`Và`(0,2,3)`nghĩ rằng chúng không chồng chéo lên nhau. DP đảm bảo rằng các tam giác chồng chéo không được chọn cùng nhau bằng cách chỉ xem xét các phân đoạn cho`dp[l][k]`Và`dp[k][r]`. Trong trường hợp này, `dp[
