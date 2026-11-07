---
title: "CF 131B - Sự đối lập thu hút"
description: "Chúng ta được yêu cầu đếm tất cả các cặp khách hàng có số được chỉ định hoàn toàn đối lập nhau. Mỗi khách hàng có một số từ -10 đến 10 và một cặp hợp lệ nếu một khách hàng có số $x$ và khách hàng kia có số $-x$."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 131
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 95 (Div. 2)"
rating: 1200
weight: 131
solve_time_s: 94
verified: true
draft: false
---

[CF 131B - Sự đối lập thu hút](https://codeforces.com/problemset/problem/131/B) 

**Đánh giá:** 1200 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm tất cả các cặp khách hàng có số được chỉ định hoàn toàn đối lập nhau. Mỗi khách hàng có một số từ -10 đến 10 và một cặp hợp lệ nếu một khách hàng có số$x$và cái kia có số$-x$. Số 0 được coi là đối lập của chính nó, vì vậy hai khách hàng có số 0 tạo thành một cặp hợp lệ. Thứ tự của khách hàng không quan trọng, vì vậy mỗi cặp chỉ được tính một lần và khách hàng không thể ghép nối với chính họ. 

Đầu vào bao gồm số lượng khách hàng$n$và một danh sách các số được chỉ định của họ$t_1, t_2, \dots, t_n$. Đầu ra là tổng số cặp đối diện hợp lệ. 

Từ$n$có thể lớn như$10^5$, một cách tiếp cận mạnh mẽ để kiểm tra tất cả các cặp một cách rõ ràng sẽ yêu cầu khoảng$n^2/2$hoạt động, đó là$5 \cdot 10^9$trong trường hợp xấu nhất. Tốc độ này quá chậm đối với giới hạn 2 giây, vì vậy chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. Tất cả các số đều nằm trong một phạm vi nhỏ, cố định, gợi ý về cách tiếp cận dựa trên việc đếm. Các trường hợp cạnh bao gồm tất cả các số 0, số lặp lại và tính đối xứng âm-dương. Ví dụ: nếu đầu vào là`0 0 0`, có 3 cặp:`(1,2)`,`(1,3)`,`(2,3)`. Một cách tiếp cận đơn giản để kiểm tra các cặp có hai vòng lặp có thể là các cặp đếm đôi hoặc bỏ lỡ khả năng tự ghép nối bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại tất cả các cặp khách hàng, kiểm tra xem số lượng của chúng có đối lập nhau hay không và tăng một bộ đếm. Điều này hoạt động chính xác nhưng chạy trong$O(n^2)$, điều này là không thể thực hiện được đối với$n=10^5$vì nó sẽ hoạt động xung quanh$5 \cdot 10^9$so sánh. Bộ nhớ không đáng kể, nhưng thời gian chạy khiến cách tiếp cận này không được chấp nhận. 

Cách tiếp cận tối ưu sử dụng chiến lược đếm. Đầu tiên, tạo bản đồ tần số của tất cả các số trong danh sách. Đối với mỗi số$x$, số cặp hợp lệ được hình thành với số đối diện của nó$-x$là tích của số đếm$x$và số lượng$-x$. Cần xử lý đặc biệt đối với số 0: số cặp giữa các số 0 là$\binom{count_0}{2} = count_0 \cdot (count_0-1)/2$. Để tránh tính hai lần, chỉ xét các số dương (kể cả số 0) khi nhân số đếm với số đối của chúng. 

Quan sát rằng các số nhỏ và bị giới hạn khiến cho việc sử dụng mảng có kích thước cố định có độ dài 21 (cho -10 đến 10) hoặc từ điển để theo dõi số đếm là khả thi. Điều này làm giảm thời gian chạy xuống$O(n)$để đếm và$O(1)$để tính toán các cặp sử dụng số lượng giá trị cố định có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) (mảng cố định) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng truy cập`freq`có kích thước 21 để lưu trữ tần số của các số từ -10 đến 10. Chỉ số cho một số$x$là`x + 10`. 
2. Lặp lại danh sách đầu vào và tăng chỉ số tương ứng trong`freq`cho mỗi số. Điều này xây dựng một bản đồ tần số trong thời gian tuyến tính. 
3. Khởi tạo một biến`pairs`về 0 để lưu trữ tổng số cặp đối diện. 
4. Lặp qua các số từ 1 đến 10. Với mỗi số`x`, nhân lên`freq[x + 10]`qua`freq[-x + 10]`và thêm kết quả vào`pairs`. Điều này đếm tất cả các cặp có số đối diện một lần mà không tính hai lần. 
5. Xử lý số 0 riêng biệt. Nếu như`freq[10]`là số lượng số không, thêm`freq[10] * (freq[10] - 1) // 2`ĐẾN`pairs`để tính đến tất cả các cặp 0-0. 
6. In giá trị của`pairs`. 

Tại sao nó hoạt động: thuật toán đếm chính xác tất cả các cặp bằng cách sử dụng bất biến cho mỗi số$x > 0$, chúng tôi bao gồm tất cả các kết hợp với số âm của nó đúng một lần. Số 0 được xử lý bằng công thức kết hợp vì nó ghép đôi với chính nó. Điều này đảm bảo không có cặp nào được tính hai lần và không xảy ra hiện tượng tự ghép nối không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
nums = list(map(int, input().split()))

# frequency array for numbers -10 to 10
freq = [0] * 21
for num in nums:
    freq[num + 10] += 1

pairs = 0
# count positive-negative pairs
for i in range(1, 11):
    pairs += freq[i + 10] * freq[-i + 10]

# count zero pairs
pairs += freq[10] * (freq[10] - 1) // 2

print(pairs)
```Phần đầu tiên đọc đầu vào và xây dựng mảng tần số. Vòng lặp trên các số dương tính toán các cặp có số âm mà không cần đếm hai lần. Trường hợp 0 ​​sử dụng công thức kết hợp để chọn 2 phần tử từ`freq[10]`số không. Chúng tôi sử dụng phép chia số nguyên để có được số nguyên chính xác. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`-3 3 0 0 3`| số | chỉ số tần số | tần số sau khi đếm | 
| --- | --- | --- | 
| -3 | 7 | 1 | 
| 3 | 13 | 1 | 
| 0 | 10 | 1 | 
| 0 | 10 | 2 | 
| 3 | 13 | 2 | 

Tính toán cặp: 

| tôi | tần số[i+10] | tần số[-i+10] | sản phẩm | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 
| 2 | 0 | 0 | 0 | 
| 3 | 2 | 1 | 2 | 

Không có cặp:`freq[10] * (freq[10]-1)//2 = 2*1//2 = 1`Tổng số cặp = 2 + 1 = 3 

**Mẫu 2:** 

đầu vào:`1 -1 1 -1`| tôi | tần số[i+10] | tần số[-i+10] | sản phẩm | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 4 | 

Không có cặp: không có 

Tổng số cặp = 4 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Xây dựng mảng tần số là tuyến tính theo số lượng khách hàng. Việc tính toán các cặp bao gồm tối đa 10 lần lặp. | 
| Không gian | O(1) | Mảng tần số có 21 phần tử cố định, không phụ thuộc vào n. | 

Vì n 10^5 nên việc truyền tuyến tính qua danh sách là đủ nhanh. Việc sử dụng bộ nhớ là tối thiểu nên giải pháp phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    nums = list(map(int, input().split()))
    freq = [0]*21
    for num in nums:
        freq[num+10] += 1
    pairs = 0
    for i in range(1, 11):
        pairs += freq[i+10]*freq[-i+10]
    pairs += freq[10]*(freq[10]-1)//2
    return str(pairs)

# provided samples
assert run("5\n-3 3 0 0 3\n") == "3", "sample 1"
assert run("4\n1 -1 1 -1\n") == "4", "sample 2"

# custom cases
assert run("1\n0\n") == "0", "single zero"
assert run("3\n0 0 0\n") == "3", "three zeros"
assert run("6\n-10 10 -10 10 -10 10\n") == "9", "max absolute value"
assert run("5\n1 1 1 1 1\n") == "0", "all equal positive"
assert run("5\n-1 -1 -1 -1 -1\n") == "0", "all equal negative"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n0 | 0 | đầu vào tối thiểu, không có cặp | 
| 3\n0 0 0 | 3 | nhiều số không, logic ghép nối bằng 0 | 
| 6\n-10 10 -10 10 -10 10 | 9 | giá trị tuyệt đối lớn, ghép nối lặp đi lặp lại | 
| 5\n1 1 1 1 1 | 0 | tất cả các số dương giống nhau | 
| 5\n-1 -1 -1 -1 -1 | 0 | tất cả các số âm giống nhau | 

## Vỏ cạnh 

Đối với trường hợp 0, hãy nhập`0 0 0`kết quả về tần số`freq[10] = 3`. Công thức tính số cặp`3*2/2 = 3`cặp, liệt kê đúng`(1,2)`,`(1,3)`,`(2,3)`. Đối với tất cả các số dương giống hệt nhau, ví dụ:`1 1 1`, mảng đếm có`freq[11] = 3`, Nhưng`freq[-1+10] = freq[9] = 0`, vì vậy không có cặp nào được thêm vào.
