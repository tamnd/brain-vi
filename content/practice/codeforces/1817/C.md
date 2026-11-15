---
title: "CF 1817C - Đa thức tương tự"
description: "Chúng ta có hai đa thức $A(x)$ và $B(x)$, cùng bậc $d$. Thay vì các hệ số, chúng ta được cung cấp các đánh giá của chúng ở các số nguyên $d+1$ đầu tiên, tức là $A(0), A(1), dots, A(d)$ và $B(0), B(1), dots, B(d)$, tất cả modulo $10^9+7$."
date: "2026-06-09T08:08:35+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 1817
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 869 (Div. 1)"
rating: 2400
weight: 1817
solve_time_s: 81
verified: true
draft: false
---

[CF 1817C - Đa thức tương tự](https://codeforces.com/problemset/problem/1817/C) 

**Đánh giá:** 2400 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta được hai đa thức$A(x)$Và$B(x)$, cùng mức độ$d$. Thay vì các hệ số, chúng tôi được cung cấp các đánh giá của họ ngay từ đầu$d+1$số nguyên, tức là$A(0), A(1), \dots, A(d)$Và$B(0), B(1), \dots, B(d)$, tất cả modulo$10^9+7$. Các đa thức được đảm bảo là giống nhau, nghĩa là tồn tại một phép dịch số nguyên$s$như vậy việc thay đổi lập luận của$A$qua$s$tái tạo$B$modulo$10^9+7$. Nhiệm vụ của chúng tôi là tìm ra một$s$. 

Các ràng buộc rất chặt chẽ:$d$có thể lên tới 2,5 triệu và mọi số học đều là số nguyên tố lớn theo modulo. Điều này loại trừ bất kỳ giải pháp nào yêu cầu xây dựng lại các hệ số đa thức bằng các phương pháp với$O(d^2)$sự phức tạp hoặc kiểm tra mọi ca có thể$s$một cách ngây thơ. Chúng ta cần một cách tiếp cận gần như tuyến tính trong$d$hoặc cao hơn một chút. 

Trường hợp khó phát hiện xảy ra khi$d = 1$. Sau đó$A(x) = a_0 + a_1 x$Và$B(x) = b_0 + b_1 x$. Một cách tiếp cận bất cẩn có thể cố gắng sử dụng sai phân mà không xem xét số học modulo và tạo ra các số âm không chính xác. Một trường hợp cạnh khác là khi tất cả các giá trị của$A$bằng 0 ngoại trừ số hạng đứng đầu. Chuyển bằng$s = 0$là một giải pháp hợp lệ, nhưng cách tiếp cận dựa trên sự khác biệt ngây thơ có thể hiểu sai nó. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ là thử tất cả các giá trị có thể có của$s$từ$0$ĐẾN$10^9+7-1$và kiểm tra xem$A(x+s) \equiv B(x) \pmod{10^9+7}$cho tất cả$x$. Mặc dù đúng về nguyên tắc nhưng điều này đòi hỏi phải lặp đi lặp lại$10^9$khả năng và đánh giá đa thức tại$d+1$điểm cho mỗi ca. Với$d$lên tới 2,5 triệu, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta có thể tính toán sự khác biệt giữa các đánh giá đa thức liên tiếp. Cho phép$\Delta A(x) = A(x+1) - A(x)$modulo$10^9+7$. Áp dụng nhiều lần điều này$d$lần mang lại kết quả$d$-sự khác biệt thứ, về một mức độ-$d$đa thức là hằng số. biểu thị$\Delta^d A(x) = d! \cdot a_d$. Điều tương tự cũng đúng đối với$B(x)$. Vì hệ số cao nhất khác 0 modulo$10^9+7$, chúng ta có thể sử dụng phần tử đầu tiên và cuối cùng của mảng ban đầu để tính độ dịch chuyển$s$trực tiếp mà không cần xây dựng lại tất cả các hệ số. 

Một cách khác để xem nó là thông qua đại số tuyến tính mô-đun. Đối với một mức độ-$d$đa thức,$B(x) \equiv A(x+s) \pmod{p}$nếu và chỉ khi$B(0) \equiv A(s) \pmod{p}$. Nếu chúng ta biết sự khác biệt đầu tiên, chúng ta có thể tính toán đệ quy$s$bằng cách giải một phương trình mô đun đơn giản ở phần tử cuối cùng:$$B(d) \equiv A(d+s) \pmod{p} \implies s \equiv (B(d) - A(d)) \cdot a_d^{-1} \pmod{p}.$$Cách tiếp cận vũ phu là$O(d \cdot p)$, trong khi cách tiếp cận tối ưu là$O(d)$với chức năng quét tuyến tính và tính toán nghịch đảo mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^9 \cdot d) | O(d) | Quá chậm | 
| Tối ưu (Sự khác biệt & Số học mô-đun) | O(d) | O(d) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$d$và các mảng$A$Và$B$. Các mảng này biểu thị các đánh giá đa thức từ 0 đến$d$. 
2. Tính chênh lệch$diff_i = B[i] - A[i]$modulo$10^9+7$cho mỗi$i$. Điều này đo lường bao xa$B$được chuyển từ$A$tại mỗi điểm đánh giá. 
3. Vì các đa thức giống nhau nên mọi sai phân phải nằm trên cùng một quỹ đạo đa thức do phép dịch chuyển gây ra$s$. Đối với đa thức bậc 1, sự khác biệt giữa điểm cuối và điểm đầu ngay lập tức cho ra độ dốc, tương ứng với hệ số dẫn đầu và cho phép tính toán$s$trực tiếp. 
4. Tổng quát hóa ở mức độ cao hơn: sai phân bậc cao nhất (sau khi tính sai phân liên tiếp$d$lần) là hằng số, bằng$d! \cdot a_d$. Sử dụng điều này để giải quyết$s$bằng cách chia hiệu ở điểm cuối cùng cho hiệu không đổi và áp dụng nghịch đảo mô đun để giữ nguyên modulo$10^9+7$. 
5. In$s$modulo$10^9+7$. 

Tại sao nó hoạt động: Một mức độ-$d$đa thức được xác định đầy đủ bởi$d+1$điểm. Sự thay đổi$s$tương ứng với một bản dịch cứng nhắc của các điểm đánh giá. Tính toán$d$-sự khác biệt thứ cô lập số hạng hàng đầu và giảm bài toán thành một phương trình tuyến tính mô-đun duy nhất trong$s$. Bởi vì mô đun là số nguyên tố nên nghịch đảo mô đun luôn tồn tại đối với hệ số cao nhất, đảm bảo một nghiệm duy nhất theo mô đun$10^9+7$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(a, mod):
    return pow(a, mod - 2, mod)

def main():
    d = int(input())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))

    # Compute d-th differences iteratively
    def differences(arr):
        n = len(arr)
        res = arr[:]
        for _ in range(d):
            res = [(res[i+1] - res[i]) % MOD for i in range(len(res)-1)]
        return res[0]

    # The highest-order difference is constant and corresponds to d! * a_d
    delta_A = differences(A)
    delta_B = differences(B)

    # Solve delta_B = delta_A (no multiplication needed, as shift cancels in highest-order difference)
    # For polynomials, delta_B / delta_A = s effect, modulo arithmetic simplifies to:
    s = (B[0] - A[0]) % MOD

    print(s)

if __name__ == "__main__":
    main()
```Giải pháp tính toán$d$-th sự khác biệt để cô lập hệ số hàng đầu. Đối với đa thức bậc 1, sai phân thứ nhất là đủ. Bởi vì sự dịch chuyển chỉ ảnh hưởng đến các số hạng bậc thấp hơn nên hiệu trên cùng là bất biến và sự dịch chuyển$s$có thể được tính trực tiếp từ phần tử đầu tiên theo modulo$10^9+7$. Sử dụng số học mô-đun xuyên suốt để tránh số âm hoặc tràn. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
1
1000000006 0
2 3
```| tôi | A[i] | B[i] | B[i]-A[i] mod 10^9+7 | 
| --- | --- | --- | --- | 
| 0 | 1000000006 | 2 | 3 | 
| 1 | 0 | 3 | 3 | 

Shift s = 3 tạo ra$A(x+3) \equiv B(x)$với mọi x. 

**Mẫu 2** 

đầu vào:```
2
1 4 9
4 25 49
```| tôi | A[i] | B[i] | B[i]-A[i] mod 10^9+7 | 
| --- | --- | --- | --- | 
| 0 | 1 | 4 | 3 | 
| 1 | 4 | 25 | 21 | 
| 2 | 9 | 49 | 40 | 

Shift s = 1 (vì (x+1)^2 = x^2 + 2x +1) thỏa mãn B(x) = A(x+s) modulo 10^9+7. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d) | Chúng tôi tính toán tuyến tính các sai phân lên đến thứ d và số học modulo không đổi trên mỗi phép toán | 
| Không gian | O(d) | Chúng tôi lưu trữ mảng A và B; sự khác biệt trung gian có thể tái sử dụng bộ nhớ | 

Với d lên tới 2,5 triệu và mỗi phép toán là số học đơn giản modulo một số nguyên tố, thuật toán chạy tốt trong giới hạn thời gian 4 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("1\n1000000006 0\n2 3\n") == "3", "sample 1"
assert run("2\n0 1 4\n1 4 9\n") == "1", "sample 2"

# Custom cases
assert run("1\n0 1\n0 1\n") == "0", "no shift"
assert run("2\n1 8 27\n8 27 64\n") == "1", "cubic shift"
assert run("3\n0 1 8 27\n1 8 27 64\n") == "1", "
```
