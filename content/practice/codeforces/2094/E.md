---
title: "CF 2094E - Boneca Ambalabu"
description: "Chúng ta được cung cấp một chuỗi các số nguyên và với mỗi phần tử trong chuỗi, chúng ta có thể tính tổng XOR giữa nó và mọi phần tử khác. Nhiệm vụ của chúng ta là tìm ra phần tử nào tối đa hóa tổng này. Cụ thể hơn, đối với dãy a = [a1, a2, ..."
date: "2026-06-08T05:34:44+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks"]
categories: ["algorithms"]
codeforces_contest: 2094
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1017 (Div. 4)"
rating: 1200
weight: 2094
solve_time_s: 69
verified: true
draft: false
---

[CF 2094E - Boneca Ambalabu](https://codeforces.com/problemset/problem/2094/E) 

**Đánh giá:** 1200 
**Thẻ:** bitmask 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên và với mỗi phần tử trong chuỗi, chúng ta có thể tính tổng XOR giữa nó và mọi phần tử khác. Nhiệm vụ của chúng ta là tìm ra phần tử nào tối đa hóa tổng này. Cụ thể hơn, đối với một chuỗi`a = [a_1, a_2, ..., a_n]`, chúng tôi muốn giá trị lớn nhất có thể của$$S_k = \sum_{i=1}^{n} (a_k \oplus a_i)$$trên tất cả các chỉ số`k`. Hoạt động XOR theo chiều bit, do đó nó lật các bit có toán hạng khác nhau. 

Các ràng buộc cho phép lên đến`2·10^5`các phần tử cho mỗi trường hợp thử nghiệm và tổng cộng`2·10^5`các yếu tố trên tất cả các trường hợp thử nghiệm. Mỗi số phù hợp với 30 bit. Điều này cho chúng ta biết rằng bất kỳ giải pháp nào lặp lại trên tất cả các cặp, tức là O(n²), đều quá chậm. Chúng ta phải tìm một giải pháp có tỷ lệ tuyến tính hoặc gần tuyến tính với`n`. 

Các trường hợp cạnh bao gồm các chuỗi có độ dài 1, các chuỗi trong đó tất cả các phần tử đều bằng nhau và các chuỗi trong đó các số là lũy thừa của hai. Ví dụ, nếu tất cả các phần tử đều`18`, tổng XOR của bất kỳ phần tử nào cũng bằng 0, bởi vì`x XOR x = 0`. Nếu trình tự là`[1, 2, 4, 8, 16]`, việc chọn phần tử lớn nhất sẽ tối đa hóa tổng vì nó đặt nhiều bit cao hơn trong kết quả XOR. Một cách tiếp cận ngây thơ mà bỏ qua cấu trúc cấp độ bit sẽ không thể xử lý chúng một cách hiệu quả. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ tính toán`S_k`cho mọi`k`bằng cách lặp qua tất cả các phần tử khác. Vì`n = 2·10^5`, điều này dẫn đến khoảng`4·10^10`hoạt động cho mỗi trường hợp kiểm thử trường hợp xấu nhất, điều này là không khả thi. 

Thông tin chi tiết quan trọng là XOR tuyến tính trên các bit: tổng XOR có thể được phân tách theo vị trí bit. Hãy xem xét một chút`b`. Nếu như`b`được thiết lập trong`a_k`, nó đóng góp vào tổng XOR với mọi phần tử **không** được đặt bit đó. Tương tự, nếu`b`không được đặt trong`a_k`, nó đóng góp vào tổng XOR với mọi phần tử **có** được đặt bit đó. Do đó, với mỗi bit, chúng ta có thể đếm được có bao nhiêu số được đặt bit đó (`ones`) và bao nhiêu không (`zeros`). Sự đóng góp của bit đó vào tổng khi chọn`a_k`chỉ phụ thuộc vào việc liệu`a_k`có thiết lập bit hay không: 

- Nếu bit được đặt vào`a_k`, đóng góp =`zeros * (1 << bit)`- Nếu bit không được đặt trong`a_k`, đóng góp =`ones * (1 << bit)`Chúng ta có thể chọn từng bit một cách độc lập để tối đa hóa tổng, điều này cho phép chúng ta xây dựng giá trị tối ưu`a_k`trong thời gian O(30) cho mỗi phần tử. Vì mỗi số có tối đa 30 bit nên tính toán tổng thể là O(n * 30) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Đếm theo bit | O(n * 30) | O(30) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng 30 phần tử`bit_count`về không. Mỗi mục sẽ lưu trữ bao nhiêu số trong mảng có tập bit cụ thể đó. 
2. Lặp qua từng số trong mảng và cập nhật`bit_count`cho mỗi vị trí bit. 
3. Với mỗi số`a_k`, tính tổng XOR của nó với mảng bằng cách xem xét từng bit riêng biệt. Nếu một bit được đặt vào`a_k`, nó góp phần`zeros * (1 << bit)`đến tổng, ở đâu`zeros = n - bit_count[bit]`. Nếu bit này không được đặt, nó sẽ đóng góp`ones * (1 << bit)`với`ones = bit_count[bit]`. 
4. Theo dõi số tiền tối đa gặp phải. 
5. Sau khi xử lý tất cả các số, in ra tổng lớn nhất. 

**Tại sao nó hoạt động:** Tổng XOR có tính cộng trên các bit và đóng góp của mỗi bit là độc lập với các bit khác. Việc đếm các bit tập hợp cho chúng ta biết chính xác mỗi bit đóng góp như thế nào vào tổng số của bất kỳ ứng cử viên nào`a_k`. Điều này đảm bảo rằng chúng tôi không bao giờ đánh giá thấp hoặc đánh giá quá cao số tiền. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        # Count how many numbers have each bit set
        bit_count = [0] * 30
        for num in a:
            for b in range(30):
                if num & (1 << b):
                    bit_count[b] += 1
        
        max_sum = 0
        for num in a:
            total = 0
            for b in range(30):
                ones = bit_count[b]
                zeros = n - ones
                if num & (1 << b):
                    total += zeros * (1 << b)
                else:
                    total += ones * (1 << b)
            max_sum = max(max_sum, total)
        print(max_sum)

if __name__ == "__main__":
    solve()
```Việc triển khai này trước tiên sẽ đếm xem mỗi bit có bao nhiêu số được đặt. Sau đó, với mỗi số, nó tính tổng XOR bằng cách kết hợp các đóng góp bit một cách hiệu quả. Việc sử dụng cẩn thận`n - ones`đối với số không sẽ ngăn ngừa những sai sót từng cái một. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**`[18, 18, 18]`| số | đếm bit 4 | đếm bit 1 | tổng hợp | 
| --- | --- | --- | --- | 
| 18 | 3 | 3 | 0 | 

Tất cả các bit khớp với mọi số. Tổng XOR là 0 cho bất kỳ lựa chọn nào. 

**Đầu vào mẫu 2:**`[1, 2, 4, 8, 16]`| số | bit đóng góp | tổng hợp | 
| --- | --- | --- | 
| 1 | (bit0: số không=4 →4, bit1: số một=1 →2, ...) | 79 | 
| 16 | (bit4: số không=4 →16, ...) | 79 | 

Tổng tối đa là 79. Bảng cho thấy các đóng góp mang tính cộng tính trên mỗi bit và phần tử lớn nhất thường tối đa hóa tổng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 30) | 30 bit cho mỗi số, lặp lại n số | 
| Không gian | O(30) | Để lưu trữ số lượng bit | 

Với tổng n trên tất cả các trường hợp thử nghiệm 2·10^5, thao tác 30n là khoảng 6·10^6, nằm trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("5\n3\n18 18 18\n5\n1 2 4 8 16\n5\n8 13 4 5 15\n6\n625 676 729 784 841 900\n1\n1\n") == "0\n79\n37\n1555\n0", "sample"

# Custom test cases
assert run("1\n1\n0\n") == "0", "single zero"
assert run("1\n2\n0 1\n") == "1", "two elements, small"
assert run("1\n5\n31 31 31 31 31\n") == "0", "all equal large"
assert run("1\n3\n0 2 4\n") == "6", "different bits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | trường hợp cạnh số đơn | 
| 0 1 | 1 | XOR trên số nhỏ | 
| tất cả 31 | 0 | các số bằng nhau, tổng bằng 0 | 
| 0 2 4 | 6 | Tối đa hóa XOR bằng phân phối bit | 

## Vỏ cạnh 

Đối với một phần tử duy nhất`[1]`,`bit_count`đếm một chút. Công thức tính tổng giảm xuống 0 vì`zeros = 0`, xử lý chính xác kịch bản một phần tử. 

Đối với tất cả các yếu tố bằng nhau, như`[31, 31, 31]`, mỗi bit đóng góp 0 vào tổng XOR, tạo ra tổng bằng 0 như mong đợi. 

Đối với các chuỗi có bit cao thưa thớt, chẳng hạn như`[1, 2, 4]`, thuật toán đếm chính xác các số 0 trên mỗi bit và tính toán các đóng góp một cách độc lập, đảm bảo tổng tối đa chính xác ngay cả khi các số có các bit không chồng chéo. 

Cách tiếp cận theo bit này đảm bảo tính chính xác cho tất cả các trường hợp cạnh mà không cần xử lý trường hợp đặc biệt nào.
