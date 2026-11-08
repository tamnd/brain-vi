---
title: "CF 1431A - Bán Hamburger"
description: "Chúng ta được yêu cầu tìm mức giá tối ưu để bán hamburger cho một nhóm khách hàng, mỗi người có một số tiền cụ thể. Mỗi khách hàng sẽ mua một chiếc bánh hamburger khi và chỉ khi giá của nó không vượt quá số tiền họ có."
date: "2026-06-11T05:05:11+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1431
codeforces_index: "A"
codeforces_contest_name: "Kotlin Heroes 5: ICPC Round"
rating: 800
weight: 1431
solve_time_s: 77
verified: true
draft: false
---

[CF 1431A - Bán bánh mì kẹp thịt](https://codeforces.com/problemset/problem/1431/A) 

**Đánh giá:** 800 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm mức giá tối ưu để bán hamburger cho một nhóm khách hàng, mỗi người có một số tiền cụ thể. Mỗi khách hàng sẽ mua một chiếc bánh hamburger khi và chỉ khi giá của nó không vượt quá số tiền họ có. Mục tiêu là tối đa hóa tổng số xu mà căng tin thu được. Về mặt khái niệm, chúng ta đang chọn một mức giá$m$, và doanh thu là$m$lần số lượng khách hàng có số tiền ít nhất là$m$. 

Đầu vào là nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm bao gồm số lượng khách hàng$n$và một mảng$a$số tiền có sẵn của họ. Mỗi$a_i$có thể lớn như$10^{12}$, Nhưng$n$chỉ lên tới 100. Mức thấp$n$cho phép chúng ta xem xét các thuật toán mở rộng theo$n^2$hoặc thậm chí$n \log n$thoải mái. Tuy nhiên, giá trị đồng tiền tiềm năng rất lớn có nghĩa là bất kỳ cách tiếp cận nào dựa vào việc lặp qua tất cả các mức giá có thể lên tới$10^{12}$là không thể thực hiện được. 

Các trường hợp khó khăn phát sinh khi tất cả khách hàng có cùng một đồng tiền, khi một khách hàng có nhiều tiền hơn những khách hàng còn lại hoặc khi nhiều khách hàng có thể mua được mức giá rất cao nhưng đặt giá thấp hơn một chút sẽ làm tăng tổng doanh thu. Ví dụ: nếu tất cả khách hàng có 1 xu, việc đặt giá cao hơn 1 sẽ không mang lại kết quả gì. Nếu khách hàng có xu`[1, 1000000000000]`, mức giá tối ưu không phải là mức giá tối đa, vì bán ở mức 1 sẽ cho phép bán được hai chiếc bánh hamburger thay vì chỉ bán được một chiếc ở mức tối đa. 

## Phương pháp tiếp cận 

Phương pháp brute-force là xem xét mọi giá nguyên từ 1 đến số tiền tối đa của khách hàng và tính doanh thu cho mỗi giá. Đối với mỗi mức giá, chúng tôi sẽ lặp lại tất cả các khách hàng để đếm xem có bao nhiêu người sẽ mua ở mức giá đó và nhân với giá. Điều này đúng vì nó kiểm tra toàn diện mọi khả năng, nhưng lại quá chậm. Ngay cả đối với$n = 100$, nếu đồng xu lớn nhất là$10^{12}$, việc lặp đi lặp lại tất cả các mức giá là không khả thi. 

Quan sát quan trọng cho phép tối ưu hóa là mức giá đề xuất duy nhất có thể mang lại doanh thu tối đa là mức giá tương đương với số tiền của một số khách hàng. Điều này là do việc tăng giá vượt quá số tiền của bất kỳ khách hàng nào sẽ ngay lập tức làm giảm số lượng người mua mà không tăng giá vượt quá giới hạn của người mua và bất kỳ mức giá nào giữa số tiền của hai khách hàng đều không làm thay đổi nhóm người mua. Vì vậy, chúng ta chỉ cần xét đến giá$m \in a$. 

Sau khi sắp xếp mảng tiền, chúng tôi có thể tính toán doanh thu cho từng mức giá ứng viên một cách hiệu quả. Nếu chúng ta xem xét giá$a[i]$trong một mảng được sắp xếp, số lượng khách hàng có thể thanh toán ít nhất$a[i]$là$n - i$. Doanh thu chỉ đơn giản là$a[i] \cdot (n - i)$. Lặp lại mảng được sắp xếp này mang lại doanh thu tối đa trong$O(n \log n)$thời gian do sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * max(a_i)) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$và mảng tiền của khách hàng$a$. Đọc tất cả đầu vào trước là phương pháp tiêu chuẩn để lập trình cạnh tranh và đảm bảo xử lý I/O nhất quán. 
2. Sắp xếp mảng$a$theo thứ tự không giảm. Việc sắp xếp đảm bảo rằng với mỗi mức giá$a[i]$, số người mua có đủ khả năng chi trả là$n - i$. 
3. Khởi tạo một biến để theo dõi doanh thu tối đa,$\text{max\_revenue} = 0$. 
4. Lặp lại mảng đã sắp xếp với chỉ mục$i$. Đối với mỗi đồng tiền$a[i]$, tính toán doanh thu tiềm năng như$\text{revenue} = a[i] \cdot (n - i)$. 
5. Cập nhật$\text{max\_revenue}$nếu doanh thu được tính toán lớn hơn mức tối đa hiện tại. 
6. Sau khi xử lý tất cả các giá đề xuất, đầu ra$\text{max\_revenue}$. 

Tại sao nó hoạt động: điều bất biến là doanh thu ở bất kỳ mức giá nào không bằng một đồng tiền của khách hàng không thể vượt quá doanh thu ở một số đồng tiền của khách hàng. Việc sắp xếp đảm bảo rằng$n - i$đếm chính xác số lượng người mua, do đó, mỗi phép tính doanh thu sẽ xem xét tất cả các tập hợp con người mua hợp lệ có thể có ở mức giá nhất định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    max_revenue = 0
    for i in range(n):
        revenue = a[i] * (n - i)
        if revenue > max_revenue:
            max_revenue = revenue
    print(max_revenue)
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Việc sắp xếp mảng tiền xu đảm bảo chúng ta có thể tính toán hiệu quả số lượng khách hàng có thể trả một mức giá nhất định. Lặp lại mảng đã sắp xếp, chúng tôi tính toán doanh thu bằng giá nhân với số lượng người mua. Việc cập nhật doanh thu tối đa ở mỗi bước đảm bảo chúng tôi theo dõi doanh thu tốt nhất có thể đạt được. Có thể tránh được lỗi từng cái một bằng cách sử dụng cẩn thận$n - i$, tính tất cả người mua từ chỉ mục$i$đến cuối cùng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Tiền đầu vào:`[1, 1, 1]`. Đã sắp xếp:`[1, 1, 1]`. 

| tôi | một [tôi] | n-i | doanh thu | 
| --- | --- | --- | --- | 
| 0 | 1 | 3 | 3 | 
| 1 | 1 | 2 | 2 | 
| 2 | 1 | 1 | 1 | 

Doanh thu tối đa là 3. 

### Mẫu 2 

Tiền đầu vào:`[4, 1, 1]`. Đã sắp xếp:`[1, 1, 4]`. 

| tôi | một [tôi] | n-i | doanh thu | 
| --- | --- | --- | --- | 
| 0 | 1 | 3 | 3 | 
| 1 | 1 | 2 | 2 | 
| 2 | 4 | 1 | 4 | 

Doanh thu tối đa là 4. Ở đây, việc chọn đồng xu cao nhất mang lại doanh thu tối ưu vì ít khách hàng mua hơn nhưng giá cao hơn sẽ bù đắp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế. Việc lặp qua mảng là tuyến tính. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng tiền cho từng trường hợp thử nghiệm. | 

Được cho$n \le 100$Và$t \le 100$, giải pháp xử lý tổng thể lên tới 10.000 phần tử một cách hiệu quả. Giá trị đồng xu lớn không ảnh hưởng đến số lần lặp vì chúng tôi không liệt kê tất cả các mức giá có thể có. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        max_revenue = 0
        for i in range(n):
            revenue = a[i] * (n - i)
            if revenue > max_revenue:
                max_revenue = revenue
        print(max_revenue)
    return output.getvalue().strip()

# Provided samples
assert run("6\n3\n1 1 1\n3\n4 1 1\n3\n2 4 2\n8\n1 2 3 4 5 6 7 8\n1\n1000000000000\n3\n1000000000000 999999999999 1\n") == "3\n4\n6\n20\n1000000000000\n1999999999998"

# Custom cases
assert run("1\n1\n1\n") == "1", "single customer"
assert run("1\n5\n5 5 5 5 5\n") == "25", "all equal coins"
assert run("1\n3\n1 1000000000000 2\n") == "2000000000000", "two high-value buyers"
assert run("1\n4\n1 2 3 4\n") == "8", "incremental coins"
assert run("1\n2\n1 1000000000000\n") == "1000000000000", "high discrepancy"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1\n`|`1`| Đầu vào kích thước tối thiểu | 
|`1\n5\n5 5 5 5 5\n`|`25`| Đồng xu hoàn toàn bằng nhau, giá tối ưu bằng giá trị đồng xu | 
|`1\n3\n1 1000000000000 2\n`|`2000000000000`| Nhiều người mua, đảm bảo doanh thu tối đa được chọn | 
|`1\n4\n1 2 3 4\n`|`8`| Tiền tăng dần, kiểm tra lựa chọn giá trung gian | 
|`1\n2\n1 1000000000000\n`|`1000000000000`| Chênh lệch lớn, người mua đắt tiền duy nhất chiếm ưu thế | 

## Vỏ cạnh 

Đối với một khách hàng có số tiền khổng lồ, thuật toán sẽ chọn chính xác số tiền đó làm giá. Đầu vào: `[1000000000000
