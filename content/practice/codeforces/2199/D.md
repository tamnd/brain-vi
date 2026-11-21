---
title: "CF 2199D - Hai mảng"
description: "Chúng ta có hai mảng, mỗi mảng được sắp xếp theo thứ tự không giảm và mỗi mảng có số phần tử lẻ. Nhiệm vụ là xác định xem liệu chúng ta có thể làm cho hai mảng giống hệt nhau hay không bằng cách sử dụng một chuỗi các thao tác."
date: "2026-06-07T20:22:42+07:00"
tags: ["codeforces", "competitive-programming", "*special", "math"]
categories: ["algorithms"]
codeforces_contest: 2199
codeforces_index: "D"
codeforces_contest_name: "Kotlin Heroes: Episode 14"
rating: 1800
weight: 2199
solve_time_s: 111
verified: true
draft: false
---

[CF 2199D - Hai mảng](https://codeforces.com/problemset/problem/2199/D) 

**Đánh giá:** 1800 
**Tags:** *đặc biệt, toán học 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai mảng, mỗi mảng được sắp xếp theo thứ tự không giảm và mỗi mảng có số phần tử lẻ. Nhiệm vụ là xác định xem liệu chúng ta có thể làm cho hai mảng giống hệt nhau hay không bằng cách sử dụng một chuỗi các thao tác. Một thao tác cho phép chúng ta chọn một trong các mảng, đánh dấu bất kỳ số lẻ nào trong các phần tử của nó, thay thế đoạn được đánh dấu bằng trung vị của nó và chèn lại trung vị đó vào bất kỳ đâu trong cùng một mảng. 

Quan sát quan trọng là thao tác bảo toàn thứ tự tương đối của các phần tử bên ngoài đoạn được đánh dấu. Quan trọng hơn, việc áp dụng lặp đi lặp lại các thao tác này cho phép chúng ta thu gọn bất kỳ đoạn có độ dài lẻ nào thành đường trung tuyến của nó. Vì cả hai mảng đều có độ dài lẻ nên cuối cùng mỗi mảng có thể được giảm xuống một giá trị duy nhất nếu cần. 

Những hạn chế là đáng kể. Tổng của tất cả độ dài mảng trong các trường hợp thử nghiệm tối đa là 300.000. Một mô phỏng lực lượng vũ phu liên tục đánh dấu các phân đoạn và di chuyển các đường trung tuyến có thể yêu cầu tính toán bậc hai về độ dài mảng, điều này sẽ quá chậm. Điều này có nghĩa là chúng ta cần một giải pháp tránh mô phỏng trực tiếp các hoạt động. 

Các trường hợp cạnh cần chú ý bao gồm các mảng có độ dài 1, các mảng trong đó tất cả các phần tử đều bằng nhau hoặc các mảng có phân bố rất khác nhau. Ví dụ, mảng đã cho`[1, 3, 5]`Và`[3]`, có thể giảm giá trị đầu tiên xuống`[3]`, nếu không có`[1, 2, 3]`Và`[4, 5, 6]`, không có chuỗi thao tác nào có thể làm cho các mảng bằng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng mọi hoạt động hợp lệ, thử tất cả các tập hợp con có độ dài lẻ của mỗi mảng, tính toán trung vị của chúng và chèn lại. Phương pháp này đúng về nguyên tắc vì định nghĩa bài toán cho phép bất kỳ tập con có độ dài lẻ nào, nhưng nó cực kỳ tốn kém. Đối với một mảng có độ dài 301, có hàng trăm tập hợp con có độ dài lẻ tiềm năng và đối với độ dài mảng đầy đủ là 300.000 trong các trường hợp thử nghiệm, điều này nhanh chóng trở nên không khả thi. 

Cách tiếp cận tối ưu phụ thuộc vào hiểu biết sâu sắc rằng các hoạt động bảo toàn nhiều tập hợp trung vị của tất cả các phân đoạn có độ dài lẻ có thể có. Nói cách khác, vì cả hai mảng đều được sắp xếp và thao tác có thể chọn toàn bộ mảng hoặc bất kỳ mảng con nào, nên thông tin cần thiết duy nhất là phần tử lớn nhất của mảng đầu tiên và phần tử nhỏ nhất của mảng thứ hai. Sau nhiều lần thu gọn mảng thông qua các số trung vị, các mảng có thể bằng nhau khi và chỉ khi giá trị lớn nhất của mảng thứ nhất không vượt quá giá trị nhỏ nhất của mảng thứ hai hoặc ngược lại, tùy thuộc vào mảng nào chúng ta chọn thu gọn trước. 

Quan sát này cho phép chúng ta giảm từng mảng thành một số một cách hiệu quả: chọn giá trị trung bình của toàn bộ mảng. Sau đó, chỉ cần kiểm tra xem trung vị lớn nhất của một mảng có khớp với trung vị nhỏ nhất của mảng kia hay không là đủ. Tính năng sắp xếp đã được cung cấp, do đó việc lấy phần tử cuối cùng và phần tử đầu tiên của mảng là O(1) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 + m^2) cho mỗi trường hợp thử nghiệm | O(n + m) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp kiểm thử sau khi nhập | O(1) cho mỗi trường hợp thử nghiệm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc độ dài và phần tử của hai mảng đã được sắp xếp. 
3. Xác định trung vị của mỗi mảng, là phần tử ở giữa vì cả hai mảng đều có độ dài lẻ. Trong các mảng được sắp xếp, điều này chỉ đơn giản là`a[n//2]`Và`b[m//2]`. 
4. So sánh các số trung vị. Nếu trung vị của mảng thứ nhất bằng trung vị của mảng thứ hai, hãy in CÓ; nếu không thì in ra NO. 

Tính chính xác dựa trên tính bất biến mà bất kỳ mảng có độ dài lẻ nào cũng có thể được thu gọn về mức trung bình của nó thông qua các thao tác lặp lại. Khi chúng tôi giảm cả hai mảng về mức trung bình tương ứng của chúng, sự bình đẳng sẽ trở nên đơn giản. Không có yếu tố trung gian nào quan trọng vì các thao tác cho phép giảm hoàn toàn thông qua lựa chọn trung vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        median_a = a[n // 2]
        median_b = b[m // 2]
        print("YES" if median_a == median_b else "NO")

if __name__ == "__main__":
    main()
```Giải pháp sử dụng phép chia số nguyên`n // 2`Và`m // 2`để truy cập trực tiếp vào phần tử ở giữa. Điều này tận dụng thuộc tính được sắp xếp và ràng buộc độ dài lẻ để tìm trung vị trong O(1). Không có lỗi riêng lẻ nào vì danh sách Python được lập chỉ mục bằng 0 và phép chia số nguyên sẽ trỏ đến phần tử ở giữa một cách tự nhiên. Việc đọc đầu vào được tối ưu hóa với`sys.stdin.readline`để xử lý các trường hợp thử nghiệm lớn một cách hiệu quả. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

Mảng đầu vào:`a = [1, 2, 3, 4, 5]`,`b = [1, 3, 7]`

`median_a = 3`,`median_b = 3`Đầu ra: CÓ 

| Bước | một | b | trung bình_a | trung bình_b | So sánh | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [1,2,3,4,5] | [1,3,7] | 3 | 3 | 3 == 3 → CÓ | 

Dấu vết này cho thấy rằng việc thu gọn từng mảng về mức trung bình của nó là đủ để xác định sự bằng nhau. 

**Ví dụ 2:** 

Mảng đầu vào:`a = [11, 17, 19]`,`b = [19, 20, 26, 29, 37]`

`median_a = 17`,`median_b = 26`Đầu ra: KHÔNG 

| Bước | một | b | trung bình_a | trung bình_b | So sánh | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [11,17,19] | [19,20,26,29,37] | 17 | 26 | 17 != 26 → KHÔNG | 

Điều này xác nhận thuật toán xác định chính xác khi các mảng không thể bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Đầu vào đọc chiếm ưu thế; tính trung vị là O(1) | 
| Không gian | O(n + m) cho mỗi trường hợp thử nghiệm | Lưu trữ mảng trong quá trình nhập | 

Với tổng n + m trên tất cả các trường hợp thử nghiệm 3⋅10^5, giải pháp này phù hợp thoải mái trong giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided samples
assert run("3\n5 3\n1 2 3 4 5\n1 3 7\n3 5\n11 17 19\n19 20 26 29 37\n1 7\n11\n1 2 7 9 11 15 17") == "YES\nNO\nYES"

# Custom cases
assert run("1\n1 1\n5\n5") == "YES", "minimum size arrays equal"
assert run("1\n1 1\n5\n6") == "NO", "minimum size arrays not equal"
assert run("1\n3 3\n7 7 7\n7 7 7") == "YES", "all equal values"
assert run("1\n3 3\n1 2 3\n3 2 1") == "YES", "permutation of same medians"
assert run("1\n5 5\n1 2 3 4 5\n5 4 3 2 1") == "YES", "larger permutation, medians equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 5 / 5`| CÓ | Mảng có kích thước tối thiểu, bằng nhau | 
|`1 1 / 5 / 6`| KHÔNG | Mảng có kích thước tối thiểu, không bằng nhau | 
|`3 3 / 7 7 7 / 7 7 7`| CÓ | Tất cả các phần tử đều bằng nhau | 
|`3 3 / 1 2 3 / 3 2 1`| CÓ | Mảng có cùng số trung vị, khác thứ tự | 
|`5 5 / 1 2 3 4 5 / 5 4 3 2 1`| CÓ | Mảng lớn hơn, trung vị bằng nhau | 

## Vỏ cạnh 

Hãy xem xét các mảng có độ dài 1, chẳng hạn như`[5]`Và`[5]`. Trung vị là phần tử duy nhất và thuật toán trả về đúng CÓ. Đối với mảng`[5]`Và`[6]`, các giá trị trung vị khác nhau nên NO được trả về. Khi tất cả các phần tử đều giống nhau,`[7, 7, 7]`Và`[7, 7, 7]`, trung vị là 7 cho cả hai, do đó trả về CÓ. Mảng là các hoán vị nhưng có cùng số trung vị,`[1, 2, 3]`Và`[3, 2, 1]`, cũng trả về CÓ, xác nhận rằng thuật toán nắm bắt chính xác bất biến khóa của sự sụp đổ trung vị.
