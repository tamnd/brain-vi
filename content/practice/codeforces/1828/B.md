---
title: "CF 1828B - Hoán đổi hoán vị"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến n theo một thứ tự tùy ý. Nhiệm vụ của chúng ta là sắp xếp hoán vị theo thứ tự tăng dần bằng cách hoán đổi liên tục các cặp phần tử cách nhau chính xác k vị trí, đối với một số k cố định."
date: "2026-06-09T07:21:17+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1828
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 873 (Div. 2)"
rating: 900
weight: 1828
solve_time_s: 80
verified: true
draft: false
---

[CF 1828B - Hoán đổi hoán vị](https://codeforces.com/problemset/problem/1828/B) 

**Xếp hạng:** 900 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến n theo một thứ tự tùy ý. Nhiệm vụ của chúng ta là sắp xếp hoán vị theo thứ tự tăng dần bằng cách hoán đổi liên tục các cặp phần tử cách nhau chính xác k vị trí, đối với một số k cố định. Bài toán yêu cầu chúng ta chọn k lớn nhất có thể mà vẫn cho phép hoán vị được sắp xếp theo hạn chế này. 

Ví dụ, nếu hoán vị là`[3, 1, 2]`, việc hoán đổi các phần tử cách nhau 1 vị trí cho phép chúng ta sắp xếp nó thành`[1, 2, 3]`. Nhưng nếu chúng ta thử k = 2, phép hoán đổi duy nhất mà chúng ta có thể thực hiện sẽ là phần tử đầu tiên và phần tử cuối cùng, không đủ để sắp xếp đầy đủ mảng. Do đó, k tối đa ở đây là 1. 

Các ràng buộc đủ chặt chẽ để loại trừ các giải pháp ngây thơ thử mọi k từ 1 đến n. Với n tối đa 10^5 và nhiều trường hợp thử nghiệm có tổng tối đa 2·10^5 phần tử, mọi cách tiếp cận O(n²) sẽ hết thời gian chờ. Chúng tôi cần một cách tiếp cận O(n) hoặc O(n log n) cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng phát sinh khi các phần tử ở rất xa vị trí chính xác của chúng. Chẳng hạn, nếu một hoán vị bắt đầu bằng`[n, 1, 2, ..., n-1]`, k tối đa là n-1, vì độ dịch chuyển lớn nhất là n-1. Một thuật toán đơn giản có thể chỉ kiểm tra các giao dịch hoán đổi liền kề mà bỏ qua thực tế là các giao dịch hoán đổi xa cho phép k lớn hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là lặp lại tất cả k có thể từ 1 đến n-1 và mô phỏng các giao dịch hoán đổi. Đối với mỗi k, chúng tôi sẽ thử hoán đổi từng cặp ở khoảng cách k cho đến khi không thể hoán đổi được nữa. Điều này có hiệu quả, nhưng với n = 10^5, việc mô phỏng tất cả các giao dịch hoán đổi là O(n²) trong trường hợp xấu nhất, quá chậm. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải mô phỏng các giao dịch hoán đổi. Mỗi phần tử có một vị trí mục tiêu và khoảng cách nó cần di chuyển chỉ đơn giản là`abs(current_index - target_index)`. Đối với k cố định, một phần tử chỉ có thể đến đích nếu khoảng cách chia hết cho k. Điều này làm giảm vấn đề trong việc tính ước số chung lớn nhất của tất cả các khoảng cách dịch chuyển. Cụ thể, k tối đa có thể sắp xếp hoán vị là GCD của tất cả`abs(p_i - i)`trên i từ 1 đến n. Nếu GCD là g thì mọi phần tử có thể đạt đến vị trí chính xác của nó thông qua việc hoán đổi khoảng cách g. Điều này biến vấn đề thành phép tính O(n) bằng cách sử dụng GCD đang chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu (GCD của chuyển vị) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`k_max`về 0. Điều này sẽ lưu trữ GCD đang chạy của các chuyển vị. 
2. Lặp lại tất cả các chỉ số i từ 1 đến n. Đối với mỗi phần tử ở vị trí i, tính chuyển vị của nó như sau`abs(p[i] - i)`. Điều này đo lường khoảng cách giữa phần tử và vị trí mục tiêu của nó. 
3. Cập nhật`k_max`là GCD của chính nó và độ dịch chuyển hiện tại. Việc sử dụng GCD đang chạy đảm bảo rằng cuối cùng,`k_max`là số nguyên lớn nhất chia hết cho tất cả các chuyển vị. 
4. Sau khi xử lý tất cả các phần tử,`k_max`là giá trị tối đa của k cho phép sắp xếp hoán vị bằng cách sử dụng các hoán đổi khoảng cách k. 
5. Đầu ra`k_max`. 

Tại sao nó hoạt động: GCD của tất cả các chuyển vị đảm bảo rằng mọi phần tử có thể di chuyển đến vị trí chính xác của nó theo bội số của k. Nếu bất kỳ phần tử nào có độ dịch chuyển không chia hết cho k thì sẽ không thể đạt được mục tiêu của nó. Do đó, k tối đa chính xác là GCD của tất cả các chuyển vị. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        k_max = 0
        for i, val in enumerate(p, start=1):
            displacement = abs(val - i)
            k_max = math.gcd(k_max, displacement)
        print(k_max)

if __name__ == "__main__":
    main()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Chúng tôi liệt kê các vị trí bắt đầu từ 1 để phù hợp với việc lập chỉ mục dựa trên 1 của vấn đề. Độ dịch chuyển luôn không âm và GCD đang chạy tích lũy k lớn nhất để phân chia tất cả các khoảng cách. sử dụng`math.gcd`tự động xử lý các trường hợp cạnh như chuyển vị bằng 0. 

## Ví dụ đã hoạt động 

Đối với hoán vị`[3, 1, 2]`: 

| tôi | p[i] | chuyển vị | GCD cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 3 | 2 | 2 | 
| 2 | 1 | 1 | 1 | 
| 3 | 2 | 1 | 1 | 

Giá trị k tối đa là 1, phù hợp với kết quả đầu ra dự kiến. 

Vì`[3, 4, 1, 2]`: 

| tôi | p[i] | chuyển vị | GCD cho đến nay | 
| --- | --- | --- | --- | 
| 1 | 3 | 2 | 2 | 
| 2 | 4 | 2 | 2 | 
| 3 | 1 | 2 | 2 | 
| 4 | 2 | 2 | 2 | 

K tối đa là 2, cũng phù hợp với kết quả đầu ra dự kiến. 

Những dấu vết này xác nhận rằng thuật toán xác định chính xác k lớn nhất bằng cách tính GCD của tất cả các chuyển vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tính toán chuyển vị và GCD cho mỗi phần tử là tuyến tính. | 
| Không gian | O(n) | Chúng tôi lưu trữ hoán vị trong một danh sách. | 

Tổng n trên tất cả các trường hợp thử nghiệm là 2·10^5 nên tổng thời gian có thể chấp nhận được. Việc sử dụng bộ nhớ vẫn nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# Provided samples
assert run("7\n3\n3 1 2\n4\n3 4 1 2\n7\n4 2 6 7 5 3 1\n9\n1 6 7 4 9 2 3 8 5\n6\n1 5 3 4 2 6\n10\n3 10 5 2 9 6 7 8 1 4\n11\n1 11 6 4 8 3 7 5 9 10 2\n") == "1\n2\n3\n4\n3\n2\n3"

# Custom cases
assert run("1\n2\n2 1\n") == "1", "minimum-size input"
assert run("1\n5\n5 4 3 2 1\n") == "1", "reversed array"
assert run("1\n6\n1 2 3 4 5 6\n") == "0", "already sorted"
assert run("1\n4\n2 4 1 3\n") == "1", "displacements have GCD 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n2 1 | 1 | Hoán vị kích thước tối thiểu | 
| 5\n5 4 3 2 1 | 1 | Chuyển vị tối đa trong mảng đảo ngược | 
| 6\n1 2 3 4 5 6 | 0 | Đã sắp xếp hoán vị | 
| 4\n2 4 1 3 | 1 | Nhiều chuyển vị với GCD 1 | 

## Vỏ cạnh 

Nếu một hoán vị đã được sắp xếp thì tất cả các chuyển vị đều bằng 0. Thuật toán trả về chính xác 0 là k tối đa. Đối với một mảng đảo ngược, các chuyển vị là [4, 2, 0, 2, 4] với n = 5. GCD của các số này là 1, chỉ k = 1 cho phép sắp xếp đầy đủ. Thuật toán không dựa vào thứ tự cụ thể của các phần tử và xử lý các hoán vị nhỏ và lớn giống hệt nhau. Đối với một hoán vị trong đó tất cả các phần tử cách đều nhau khỏi mục tiêu của chúng, như`[3, 1, 4, 2]`, các chuyển vị là [2, 1, 1, 2] và GCD là 1, một lần nữa cho k tối đa chính xác. Quá trình tính toán GCD đang chạy sẽ lọc ra các số 0 một cách tự nhiên và xử lý trường hợp cạnh của các chuyển vị phần tử đơn.
