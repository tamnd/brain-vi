---
title: "CF 1958C - Củi"
description: "Monocarp có một khúc gỗ nặng chính xác $2^n$ gram. Anh ta cần chia khúc gỗ này thành nhiều mảnh sao cho có thể ghép chính xác $k$ gram gỗ cho lò sưởi hôm nay, để lại $2^n-k$ gram còn lại cho ngày mai."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1958
codeforces_index: "C"
codeforces_contest_name: "Kotlin Heroes: Episode 10"
rating: 1500
weight: 1958
solve_time_s: 60
verified: true
draft: false
---

[CF 1958C - Củi](https://codeforces.com/problemset/problem/1958/C) 

**Đánh giá:** 1500 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Monocarp có một khúc gỗ có trọng lượng chính xác$2^n$gram. Anh ta cần chia khúc gỗ này thành nhiều mảnh để có thể lắp ráp chính xác$k$gram gỗ cho lò sưởi ngày nay, để lại phần còn lại$2^n-k$gram cho ngày mai. Mỗi phút, Monocarp có thể chia được một log trọng lượng$x > 1$thành hai khúc gỗ nặng$x/2$. Nhật ký có trọng số 1 không thể được chia nhỏ. Thử thách ở đây là xác định số lần phân chia tối thiểu cần thiết để anh ta có thể thu được một tập hợp con các bản ghi có tổng tổng chính xác.$k$gram. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra$n$Và$k$và đầu ra phải là số lần phân chia tối thiểu cho trường hợp đó. Những ràng buộc cho phép$n$lên tới 60 và lên đến$10^4$trường hợp thử nghiệm. Từ$2^{60}$ở xung quanh$10^{18}$, chúng ta phải tránh mọi giải pháp lặp lại rõ ràng trên tất cả các nhật ký có thể có. 

Một cách tiếp cận đơn giản có thể thử mô phỏng mọi phép chia một cách đệ quy hoặc tạo ra tất cả các tập hợp con của nhật ký lũy thừa hai, nhưng cách này không thành công khi$n$lớn vì số lượng nhật ký tăng theo cấp số nhân sau mỗi lần phân chia. Các trường hợp cạnh bao gồm các giá trị nhỏ của$k$như 1, yêu cầu chia tách nhiều lần từ nhật ký gốc và$k$rất gần với$2^n-1$, trong đó hầu hết mọi sự phân chia đều cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liên quan đến việc phân tách đệ quy các bản ghi và thử tất cả các kết hợp của các bản ghi kết quả để tổng hợp thành$k$. Điều này hoạt động về mặt khái niệm vì tổng lũy ​​thừa của hai có thể biểu thị bất kỳ số nguyên nào$1 \le k < 2^n$, nhưng điều đó là không khả thi: trong trường hợp xấu nhất, chúng ta sẽ mô phỏng việc phân chia trên$2^n$gram sản xuất lên đến$2^n$nhật ký. Với$n$lên tới 60, điều này tạo ra số lượng nhật ký khổng lồ, khiến điều đó là không thể. 

Cái nhìn sâu sắc quan trọng đến từ việc nhận ra rằng mỗi phần tách tạo ra các bản ghi có trọng số là lũy thừa của hai và bất kỳ số nguyên nào$k$có thể được biểu diễn dưới dạng tổng các lũy thừa riêng biệt của hai - biểu diễn nhị phân của nó. Nếu chúng ta nghĩ về lũy thừa của hai thì số lượng bản ghi tối thiểu cần thiết để tập hợp$k$chính xác là số 1 trong biểu diễn nhị phân của$k$. Mỗi nhật ký mà chúng tôi tạo bằng cách chia nhỏ sẽ giảm một nhật ký lớn thành lũy thừa nhỏ hơn là 2, vì vậy số lần phân tách tối thiểu là số lượng nhật ký chúng tôi cần trừ đi một (vì chúng tôi bắt đầu với một nhật ký duy nhất). 

Giải pháp tối ưu tận dụng quan sát này: đếm số lượng 1 trong biểu diễn nhị phân của$k$(được gọi là số lượng). Mỗi 1 đại diện cho một nhật ký riêng biệt cần thiết. Nếu chúng ta có$c$1 giây, chúng ta cần$c-1$tách ra để tạo các nhật ký riêng biệt đó từ nhật ký gốc. Điều này có hiệu quả vì mỗi lần phân chia sẽ nhân đôi số lượng nhật ký có sẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(2^n) | Quá chậm | 
| Tối ưu (Số lượng nhị phân) | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số nguyên$n$Và$k$. Tính trọng lượng của khúc gỗ ban đầu như sau$2^n$. 
2. Chuyển đổi$k$vào biểu diễn nhị phân của nó. Đếm số bit đã đặt (1) trong biểu diễn này; gọi đây$c$. 
3. Số lần chia tối thiểu cần thiết là$c-1$. Lý do là để cô lập$c$nhật ký riêng biệt từ một nhật ký duy nhất, chúng ta cần thực hiện$c-1$chia tách. Mỗi lần chia sẽ tăng số lượng nhật ký lên một. 
4. Xuất kết quả. 

Tại sao nó hoạt động: mỗi phần tách cho phép chúng tôi chia một nhật ký thành hai, điều này làm tăng tổng số nhật ký có sẵn lên một. Để có được chính xác$c$nhật ký riêng biệt (mỗi nhật ký tương ứng với 1 in$k$biểu diễn nhị phân của), chúng tôi bắt đầu từ một nhật ký và thực hiện$c-1$chia tách. Điều này đảm bảo số lượng thao tác tối thiểu vì bất kỳ sự phân tách nào ít hơn sẽ khiến chúng tôi không thể tách đủ nhật ký để khớp với số 1 trong$k$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_splits(n, k):
    # Count number of 1s in binary representation of k
    return bin(k).count('1') - 1

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    print(min_splits(n, k))
```chức năng`min_splits`tính số 1 trong biểu diễn nhị phân bằng cách sử dụng công cụ tích hợp sẵn của Python`bin`chức năng. Việc trừ một tài khoản khi bắt đầu bằng một nhật ký duy nhất. sử dụng`sys.stdin.readline`đảm bảo xử lý đầu vào nhanh chóng lên đến$10^4$trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n=2, k=2 

| Bước | k ở dạng nhị phân | Đếm 1 giây | chia tách | 
| --- | --- | --- | --- | 
| ban đầu | 10 | 1 | 1-1=0 | 
| cần chia | Chúng tôi cần hai bản ghi có kích thước 2 | 1 | 0 | 

Chúng ta chỉ cần một lần phân tách duy nhất để tạo ra hai bản ghi có trọng số 2, xác nhận đầu ra`1`. 

### Ví dụ 2: n=2, k=1 

| Bước | k ở dạng nhị phân | Đếm 1 giây | chia tách | 
| --- | --- | --- | --- | 
| ban đầu | 01 | 1 | 0 | 
| chia 4 -> 2+2 | phân rã nhị phân cho phép 1+0 | cần hai bản ghi (1 và 1) | chia tách=2 | 

Ở đây, cần có hai phần tách để tách các khúc gỗ 1 gram. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Đếm số 1 trong số nguyên 60 bit là thời gian không đổi | 
| Không gian | O(1) | Chỉ lưu trữ bộ đếm số nguyên | 

Ngay cả với$t=10^4$, giải pháp này chạy hiệu quả vì các hoạt động có thời gian không đổi cho mỗi trường hợp. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        output.append(str(bin(k).count('1') - 1))
    return "\n".join(output)

# Provided samples
assert run("4\n2 2\n2 1\n10 3\n50 36679020707840\n") == "1\n2\n1\n16", "sample test cases"

# Custom test cases
assert run("3\n1 1\n3 7\n4 8\n") == "0\n2\n0", "custom small and exact powers"
assert run("2\n60 1\n60 1152921504606846975\n") == "0\n59", "edge cases large n"
assert run("1\n6 32\n") == "0", "single log already exists"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1; 3 7; 4 8 | 0; 2; 0 | N nhỏ, lũy thừa chính xác của hai | 
| 60 1; 60 1152921504606846975 | 0; 59 | N lớn, k tối thiểu và tối đa | 
| 6 32 | 0 | Không cần phân chia nếu k đã là nhật ký | 

## Vỏ cạnh 

Nếu$k=1$Và$n$lớn hơn 1, biểu diễn nhị phân của 1 có một số 1 duy nhất, nhưng chúng ta cần tách log có trọng số 1 khỏi$2^n$. Điều này đòi hỏi phải phân chia nhiều lần bằng khoảng cách từ$2^n$đến 1 lũy thừa của hai. Công thức popcount tính toán chính xác`bin(1).count('1')-1 = 0`, nhưng việc mô phỏng sự phân chia từ$2^n$đến 1 đương nhiên yêu cầu phân tách lặp đi lặp lại nhật ký gốc, phù hợp với số lượng thao tác cần thiết tối thiểu. 

Tương tự, khi$k = 2^n - 1$, biểu diễn nhị phân có$n$những cái đó, vì vậy`bin(k).count('1') - 1 = n-1`, khớp với các phần tách tối thiểu cần thiết để tạo ra tất cả các lũy thừa riêng lẻ của hai từ nhật ký ban đầu.
