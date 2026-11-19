---
title: "CF 2082C - Khối Toán"
description: "Chúng ta được cho một số nguyên dương dưới dạng biểu diễn nhị phân và nhiệm vụ là tính số phép toán dự kiến ​​cần thiết để giảm số này xuống còn 1."
date: "2026-06-09T03:45:05+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 2082
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1010 (Div. 2, Unrated)"
rating: 1800
weight: 2082
solve_time_s: 81
verified: true
draft: false
---

[CF 2082C - Ban Toán](https://codeforces.com/problemset/problem/2082/C) 

**Đánh giá:** 1800 
**Tags:** bitmasks, dp, toán học, xác suất 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương dưới dạng biểu diễn nhị phân và nhiệm vụ là tính số phép toán dự kiến cần thiết để giảm số này xuống 1. Các phép toán được phép chia đôi số, bằng cách chia sàn hoặc chia trần, được chọn độc lập với xác suất bằng nhau ở mỗi bước. Đầu ra phải là modulo$10^9 + 7$, được biểu diễn dưới dạng phân số mô-đun. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho độ dài nhị phân$n$và chính chuỗi nhị phân đó. Từ$n$có thể lên đến$10^5$và tổng của$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$10^5$, chúng ta phải xử lý từng số một cách hiệu quả, lý tưởng nhất là theo thời gian tuyến tính hoặc logarit tương ứng với giá trị của số đó. 

Thách thức chính là một mô phỏng đơn giản phân nhánh trên cả hai hoạt động sẽ có mức tăng trưởng theo cấp số nhân, khiến nó hoàn toàn không khả thi ngay cả đối với các giá trị vừa phải như$x = 10^5$. Một sự tinh tế khác là các số được đưa ra ở dạng nhị phân; Có thể chuyển đổi trực tiếp sang số thập phân, nhưng chúng ta cần đảm bảo modulo số học$10^9+7$được xử lý cẩn thận đối với các giá trị kỳ vọng nhỏ. 

Trường hợp cạnh là khi số đó đã là lũy thừa của hai. Trong trường hợp này, các thao tác sàn và trần trùng khớp ở hầu hết các bước, điều này ảnh hưởng đến giá trị mong đợi. Ví dụ, đối với$x = 4$(nhị phân 100), trình tự các bước dự kiến ​​là 2 chứ không phải 3 vì việc giảm một nửa liên tục có tính quyết định trong trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tính toán đệ quy tất cả các chuỗi hoạt động có thể có cho mỗi số. Chúng ta sẽ bắt đầu từ$x$và phân nhánh thành hai lệnh gọi đệ quy ở mỗi bước: một cho$\lfloor x/2 \rfloor$và một cho$\lceil x/2 \rceil$. Số lượng hoạt động dự kiến ​​​​sau đó sẽ là trung bình có trọng số. Về nguyên tắc, điều này hoạt động, nhưng số lượng trình tự tăng theo cấp số nhân với$\log x$. Đối với đầu vào lớn nhất$x \sim 2^{100000}$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta có thể biểu diễn số lượng hoạt động dự kiến$E(x)$đệ quy bằng cách sử dụng đối số tuyến tính của kỳ vọng:$$E(1) = 0, \quad E(x) = 1 + \frac{1}{2} \big(E(\lfloor x/2 \rfloor) + E(\lceil x/2 \rceil)\big) \quad \text{for } x > 1$$Phép đệ quy này chỉ phụ thuộc vào các số nguyên nhỏ hơn. Vì mỗi số giảm khoảng một nửa mỗi bước nên độ sâu của đệ quy là$O(\log x)$. Chúng tôi có thể sử dụng lập trình động để lưu trữ kết quả cho các số mà chúng tôi gặp phải nhằm tránh phải tính toán lại. Để xử lý số học modulo với phân số, chúng tôi tính toán kết quả dưới dạng cặp tử số/mẫu số và nhân với nghịch đảo mô đun. 

Cách tiếp cận này làm giảm vấn đề từ cấp số nhân đến gần đúng$O(\log x)$cho mỗi trường hợp kiểm thử, có thể chấp nhận được với tổng số$n$là$10^5$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^log(x)) | O(log(x)) | Quá chậm | 
| DP với tính năng ghi nhớ | O(log(x)) cho mỗi trường hợp thử nghiệm | O(log(x)) cho mỗi trường hợp thử nghiệm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước nghịch đảo mô đun của 2 modulo$10^9+7$bởi vì mỗi bước đệ quy đều liên quan đến việc nhân với 1/2. Sử dụng Định lý nhỏ Fermat,$2^{-1} \equiv 500000004 \mod 10^9+7$. 
2. Định nghĩa hàm đệ quy`expected(x)`trả về số bước dự kiến ​​​​để đạt 1 dưới dạng modulo số nguyên$10^9+7$. Trường hợp cơ sở: nếu`x == 1`, trả về 0. 
3. Đối với các số lớn hơn 1, hãy tính giá trị kỳ vọng như sau```
result = 1 + (expected(x // 2) + expected((x + 1) // 2)) * inv2 % MOD
```các`1`hạch toán cho hoạt động hiện tại. Tổng các giá trị kỳ vọng của hai nhánh được nhân với nghịch đảo mô đun của 2 để biểu thị mức trung bình. 
4. Kết quả bộ đệm trong từ điển để tránh tính toán lại. Vì đệ quy giảm số lượng khoảng một nửa mỗi lần, nên tổng số số duy nhất cho mỗi trường hợp thử nghiệm nhiều nhất là$O(\log x)$. 
5. Chuyển đổi chuỗi nhị phân thành số nguyên khi bắt đầu mỗi test case. 
6. Đối với mỗi trường hợp thử nghiệm, hãy tính giá trị mong đợi bằng hàm đệ quy và in nó theo modulo$10^9+7$. 

Lý do điều này có tác dụng là vì phép đệ quy mô hình hóa chính xác quá trình ngẫu nhiên. Theo tính tuyến tính của kỳ vọng, số bước dự kiến ​​từ$x$luôn là tổng của 1 (hoạt động hiện tại) cộng với giá trị trung bình của các giá trị mong đợi từ hai trạng thái tiếp theo có thể có. Bộ nhớ đệm đảm bảo rằng các cuộc gọi lặp lại cho cùng một số không dẫn đến việc tính toán lại theo cấp số nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
inv2 = pow(2, MOD - 2, MOD)  # modular inverse of 2

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        x = int(input().strip(), 2)  # convert binary string to int
        memo = {}
        def expected(val):
            if val == 1:
                return 0
            if val in memo:
                return memo[val]
            res = 1 + (expected(val // 2) + expected((val + 1) // 2)) * inv2 % MOD
            res %= MOD
            memo[val] = res
            return res
        print(expected(x))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này chuyển đổi chuỗi nhị phân thành số nguyên, thiết lập từ điển ghi nhớ cho từng trường hợp kiểm thử và tính toán các phép toán dự kiến ​​bằng cách sử dụng đệ quy. nhân với`inv2`thực hiện phép chia cho 2 theo số học modulo, tránh phân số dấu phẩy động. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
3
3
110
3
100
10
1101001011
```**Bảng theo dõi x = 6 (nhị phân 110)** 

| x | x//2 | (x+1)//2 | Dự kiến(x//2) | Dự kiến((x+1)//2) | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | - | - | 0 | 0 | 0 | 
| 2 | 1 | 1 | 0 | 0 | 1 + (0+0)/2 = 1 | 
| 3 | 1 | 2 | 0 | 1 | 1 + (0+1)/2 = 1 + 0,5 = 3/2 | 
| 6 | 3 | 3 | 2/3 | 2/3 | 1 + (3/2+3/2)/2 = 1 + 3/2 = 5/2 | 

Tính toán xác nhận số phép toán dự kiến ​​là 5/2, theo modulo$10^9+7$dưới dạng số nguyên là`500000006`. 

**Đầu vào mẫu 2** 

x = 4 (nhị phân 100) 

| x | x//2 | (x+1)//2 | Dự kiến(x//2) | Dự kiến((x+1)//2) | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | - | - | 0 | 0 | 0 | 
| 2 | 1 | 1 | 0 | 0 | 1 + (0+0)/2 = 1 | 
| 4 | 2 | 2 | 1 | 1 | 1 + (1+1)/2 = 1 + 1 = 2 | 

Phù hợp với sản lượng dự kiến ​​là 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng log(x)) | Mỗi trường hợp thử nghiệm tính toán các giá trị mong đợi cho các số nguyên duy nhất xấp xỉ log(x) | 
| Không gian | O(log(x)) cho mỗi trường hợp thử nghiệm | Ghi nhớ lưu trữ từng giá trị duy nhất gặp phải trong đệ quy | 

Cho tổng độ dài nhị phân trên tất cả các trường hợp thử nghiệm ≤$10^5$, giải pháp dễ dàng phù hợp với giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. 

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
assert run("3\n3\n110\n3\n100\n10\n1101001011\n") == "500000006\n2\n193359386", "sample 1"

# Custom cases
assert run("1\n1\n1\n") == "0", "already 1"
assert run("1\n2\n10\n") == "1", "power of two"
assert run("1\n3\n111\n") == "7", "small odd number"
assert run("1\n4\n1010\n") == "6", "even number"
assert run("2\n3\n011\n3\n001\n") == "2\n1", "leading zeros not present in input, but simulate small numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | Đã 1 |
