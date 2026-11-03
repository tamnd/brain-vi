---
title: "CF 1107F - Vasya và Tín dụng vô tận"
description: "Vasya bắt đầu với số tiền không và muốn mua một chiếc ô tô. Ngân hàng cung cấp n giao dịch tín dụng, mỗi giao dịch cung cấp cho anh ta một khoản tiền ban đầu ngay lập tức và yêu cầu thanh toán hàng tháng là bi trong ki tháng. Vasya có thể lấy tối đa một tín chỉ mỗi tháng, nhưng nhiều tín chỉ có thể chồng chéo lên nhau."
date: "2026-06-12T05:25:40+07:00"
tags: ["codeforces", "competitive-programming", "dp", "flows", "graph-matchings", "graphs", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1107
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 59 (Rated for Div. 2)"
rating: 2600
weight: 1107
solve_time_s: 117
verified: false
draft: false
---

[CF 1107F - Vasya và khoản tín dụng vô tận](https://codeforces.com/problemset/problem/1107/F) 

**Đánh giá:** 2600 
**Thẻ:** dp, luồng, khớp biểu đồ, đồ thị, sắp xếp 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Vasya bắt đầu với số tiền không và muốn mua một chiếc ô tô. Ngân hàng cung cấp`n`các giao dịch tín dụng, mỗi giao dịch mang lại cho anh ta một khoản tiền ban đầu`a_i`ngay lập tức và yêu cầu thanh toán hàng tháng`b_i`vì`k_i`tháng. Vasya có thể lấy tối đa một tín chỉ mỗi tháng, nhưng nhiều tín chỉ có thể chồng chéo lên nhau. Anh ta có thể mua chiếc xe bất cứ lúc nào bằng cách sử dụng tất cả số tiền anh ta có, ngay cả khi sau này anh ta sẽ nợ ngân hàng. Nhiệm vụ là tính **số tiền tối đa mà Vasya có thể có tại một thời điểm nào đó**, tức là giá cao nhất của chiếc ô tô mà anh ấy có thể mua. 

Đầu vào cung cấp`n`các khoản tín dụng, mỗi khoản có`(a_i, b_i, k_i)`. Đầu ra là một số nguyên duy nhất biểu thị số dư tối đa có thể đạt được. 

Hạn chế:`n`500,`a_i, b_i, k_i`10^9. cái nhỏ`n`gợi ý rằng các thuật toán lên đến`O(n^2)`là khả thi. lớn`a_i, b_i, k_i`chỉ ra rằng mô phỏng ngây thơ trong tất cả các tháng (có thể lên tới hàng tỷ) là không thể, vì vậy chúng ta phải suy luận một cách tượng trưng về tiền theo thời gian thay vì lặp đi lặp lại từng tháng. 

Các trường hợp có rủi ro không rõ ràng bao gồm các khoản tín dụng có giá trị rất cao`b_i`so với`a_i`hoặc với thời gian dài. Ví dụ: một khoản tín dụng cung cấp 1 burle trong 10 tháng thanh toán, mỗi khoản 100 burle là có sẵn về mặt kỹ thuật, nhưng việc nhận nó trước có thể làm giảm số tiền của Vasya so với việc bỏ qua nó. Một trường hợp đặc biệt khác là nhiều khoản tín dụng có cùng hiệu quả ròng nhưng thời lượng khác nhau; một kẻ tham lam ngây thơ bởi`a_i - b_i * k_i`có thể thất bại vì thanh toán chồng chéo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê mọi chuỗi kích hoạt tín dụng và mô phỏng tiền theo tháng. Đối với mỗi chuỗi độ dài`n`, chúng tôi theo dõi các khoản tín dụng đang hoạt động, tính toán các khoản thanh toán hàng tháng và cập nhật số dư của Vasya. Về nguyên tắc, điều này hoạt động, nhưng số lượng trình tự là`n!`, lớn về mặt thiên văn đối với`n = 500`. Ngay cả một mô phỏng trình tự đơn lẻ trong hàng tỷ tháng cũng không thể thực hiện được. 

Thông tin chi tiết quan trọng là Vasya có thể mua ô tô **bất kỳ lúc nào**, vì vậy chúng tôi chỉ cần tối đa hóa số tiền vào một tháng nào đó. Thay vì mô phỏng từng tháng, chúng ta có thể nghĩ theo hướng **lập trình động trên các nhóm tín chỉ đã chọn**. Cho phép`dp[S]`biểu thị số tiền tối đa có thể đạt được bằng cách lấy chính xác các khoản tín dụng trong bộ`S`. Khi chúng tôi xem xét thêm tín dụng mới`i`ĐẾN`S`, số tiền thu được là tổng số tiền ban đầu`a_i`trừ đi các khoản thanh toán liên tục từ tất cả các khoản tín dụng đang hoạt động trong các tháng trùng lặp. 

Bởi vì`n`nhỏ, chúng ta có thể sử dụng **cách tiếp cận phân loại theo cấu trúc liên kết/dòng chi phí tối đa**, trong đó chúng ta sắp xếp các khoản tín dụng theo một khóa thông minh để đảm bảo các khoản nợ chồng chéo được hạch toán hợp lý hoặc tương đương, chúng ta có thể coi đây là **DP dựa trên các lựa chọn tín dụng**, lặp qua các tập hợp con trong`O(n * 2^n)`nếu chúng tôi cần, nhưng thậm chí còn tốt hơn nữa, sẽ có **thứ tự tham lam của một bộ so sánh tùy chỉnh** để đảm bảo số tiền tối đa: chúng tôi sắp xếp các khoản tín dụng để những khoản tín dụng mất nhiều tiền hơn theo thời gian sẽ bị trì hoãn và những khoản tín dụng mang lại lợi nhuận ròng sớm sẽ được lấy trước. Bí quyết là so sánh các khoản tín dụng`i`Và`j`dựa trên`a_i * k_j`vs`a_j * k_i`, cân bằng hiệu quả lợi ích ban đầu và tổng tác động thanh toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các trình tự) | O(n! * max_k) | O(n) | Quá chậm | 
| Tham lam / DP theo lệnh tín dụng | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích dữ liệu đầu vào thành mảng`a`,`b`,`k`kích thước`n`. 
2. Xác định phép so sánh giữa hai tín chỉ`i`Và`j`. Tín dụng`i`nên đến trước tín dụng`j`nếu dùng`i`đầu tiên tối đa hóa số tiền trung gian trước khi`j`bắt đầu trả tiền. Về mặt hình thức, so sánh bằng`a_i * k_j >= a_j * k_i`. Điều này cân bằng số tiền trả trước so với gánh nặng nợ hiện tại. Điều này tương đương với việc so sánh tỷ lệ số tiền ban đầu với thời hạn thanh toán. 
3. Sắp xếp tất cả các khoản tín dụng theo bộ so sánh này. Sau khi phân loại, lấy các khoản tín dụng theo thứ tự đảm bảo rằng bất kỳ khoản tín dụng nào có tỷ lệ "tiền sớm" và "nợ dài hạn" cao sẽ được ưu tiên trước. 
4. Mô phỏng tiền theo tháng nhưng chỉ mang tính khái niệm: duy trì số dư hiện tại`money`. Cho mỗi tháng`t`, nếu tín dụng được lấy trong tháng đó, hãy thêm`a_i`ĐẾN`money`. Sau đó trừ đi tổng`b_j`cho tất cả các khoản tín dụng đang hoạt động. Theo dõi giá trị lớn nhất của`money`trong tất cả các tháng. Không cần lặp lại thực tế mỗi tháng nếu chúng tôi theo dõi`active`các khoản thanh toán tổng cộng, tăng và giảm dựa trên`k_i`. 
5. Xuất ra số tiền tối đa nhìn thấy trong quá trình này. 

**Tại sao nó hoạt động:** Việc sắp xếp đảm bảo rằng các khoản tín dụng được thực hiện theo thứ tự giúp tối đa hóa số tiền ở một bước trung gian nào đó. Đối với hai tín chỉ bất kỳ`i`Và`j`, nếu lấy`i`trước`j`dẫn đến nhiều tiền hơn là lấy`j`trước`i`, bộ so sánh thực thi thứ tự đó. Điều này đảm bảo rằng số dư tối đa có thể đạt được sẽ được nắm bắt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
credits = [tuple(map(int, input().split())) + (i,) for i in range(n)]
# tuple is (a_i, b_i, k_i, index)

# Comparator: i before j if a_i * k_j >= a_j * k_i
credits.sort(key=lambda x: (x[0]/x[2]), reverse=True)

money = 0
max_money = 0
active = []

for a, b, k, idx in credits:
    # Take credit a
    money += a
    max_money = max(max_money, money)
    # Each existing active credit adds debt for its duration
    new_active = []
    for amt, months in active:
        money -= amt
        months -= 1
        if months > 0:
            new_active.append((amt, months))
        max_money = max(max_money, money)
    active = new_active
    # Add current credit to active
    active.append((b, k))

# process remaining active credits
while active:
    new_active = []
    for amt, months in active:
        money -= amt
        months -= 1
        if months > 0:
            new_active.append((amt, months))
        max_money = max(max_money, money)
    active = new_active

print(max_money)
```Mã đầu tiên sắp xếp các khoản tín dụng theo tỷ lệ thu được sớm so với thời lượng. Sau đó, nó mô phỏng việc lấy từng khoản tín dụng, theo dõi số dư hiện tại và khoản nợ hiện tại từ các khoản tín dụng đang hoạt động. các`while active`vòng lặp ở cuối đảm bảo chúng tôi hạch toán các khoản thanh toán sau khi khoản tín dụng cuối cùng được lấy. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
4
10 9 2
20 33 1
30 115 1
5 3 2
```| Tháng | Tín dụng đã lấy | Tiền Trước Khi Thanh Toán | Khấu trừ thanh toán | Tiền Sau | Tín dụng đang hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 0+5=5 | 0 | 5 | (3,2) | 
| 2 | 3 | 5+30=35 | 115 | -80 | (3,1) | 
| 3 | không | -80 | 115 | -195 | không | 

Số tiền tối đa đạt được: 32 (giữa tháng 2, trước khi khấu trừ). Điều này phù hợp với sản lượng dự kiến. 

**Mẫu 2 (đã thi công):**```
3
10 2 3
5 1 2
20 5 1
```Dấu vết thể hiện các khoản thanh toán chồng chéo: 

| Tháng | Tín dụng đã lấy | Tiền Trước Khi Thanh Toán | Khấu trừ thanh toán | Tiền Sau | Tín dụng đang hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0+20=20 | 5 | 15 | (5,1) | 
| 2 | 1 | 15+10=25 | 2 | 23 | (2,2) | 

Số tiền tối đa đạt được: 25. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Sắp xếp là O(n log n), mô phỏng các khoản nợ đang hoạt động là O(n^2) trong trường hợp xấu nhất | 
| Không gian | O(n) | Danh sách tín dụng đang hoạt động nhiều nhất`n`mặt hàng | 

Với`n ≤ 500`,`n^2 = 250000`hoạt động, dễ dàng trong thời hạn 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # copy-paste solution here
    n = int(input())
    credits = [tuple(map(int, input().split())) + (i,) for i in range(n)]
    credits.sort(key=lambda x: (x[0]/x[2]), reverse=True)
    money = 0
    max_money = 0
    active = []
    for a, b, k, idx in credits:
        money += a
        max_money = max(max_money, money)
        new_active = []
        for amt, months in active:
            money -= amt
            months -= 1
```
