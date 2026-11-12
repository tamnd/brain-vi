---
title: "CF 162F - Số 0 giai thừa"
description: "Nhiệm vụ là xác định có bao nhiêu số 0 ở cuối giai thừa của một số nguyên n cho trước. Giai thừa, ký hiệu là n!, là tích của tất cả các số nguyên từ 1 đến n."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 162
codeforces_index: "F"
codeforces_contest_name: "VK Cup 2012 Wild-card Round 1"
rating: 1800
weight: 162
solve_time_s: 63
verified: true
draft: false
---

[CF 162F - Số 0 giai thừa](https://codeforces.com/problemset/problem/162/F) 

**Đánh giá:** 1800 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xác định có bao nhiêu số 0 ở cuối giai thừa của một số nguyên _n_ đã cho. Giai thừa, ký hiệu là _n!_, là tích của tất cả các số nguyên từ 1 đến _n_. Các số 0 ở cuối là các số 0 ở cuối bên phải của số này, xuất hiện bất cứ khi nào số đó chia hết cho 10. Vì 10 là tích của 2 và 5 nên mỗi số 0 ở cuối tương ứng với một cặp thừa số 2 và 5 trong tích giai thừa. 

Đầu vào là một số nguyên _n_ có giới hạn trên là 1.000.000. Giới hạn này loại trừ các cách tiếp cận ngây thơ cố gắng tính toán giai thừa một cách rõ ràng, bởi vì _n!_ phát triển cực kỳ nhanh và không thể được lưu trữ trong các kiểu số tiêu chuẩn cho _n_ lớn. Ngay cả với số học có độ chính xác tùy ý, việc nhân trực tiếp các số lên tới 1.000.000 sẽ là quá chậm, ở mức hàng triệu phép nhân. 

Sự tinh tế đến từ việc nhận ra rằng thừa số 2 phong phú hơn nhiều so với thừa số 5 trong một giai thừa. Ví dụ: trong 10!, các thừa số của 2 xảy ra ở 2, 4, 6, 8, 10, trong khi các thừa số của 5 chỉ xảy ra ở 5 và 10. Điều này có nghĩa là số số 0 ở cuối được xác định hoàn toàn bằng số lượng thừa số 5. ​​Một cách tiếp cận ngây thơ cố gắng chia giai thừa cho 10 nhiều lần hoặc đếm 10 giây một cách rõ ràng sẽ thất bại đối với _n_ lớn do cả hiệu suất và tràn số. 

Các trường hợp cạnh bao gồm các số rất nhỏ như 1 hoặc 4, trong đó giai thừa không có số 0 ở cuối. Ví dụ: 1! = 1 và 4! = 24 đều tạo ra các số 0 ở cuối. Một trường hợp cạnh khác là lũy thừa chính xác của 5, như 5 hoặc 25, cộng nhiều hơn một số 0 khi bao gồm bội số của 25, 125, v.v. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ cố gắng tính toán _n!_ một cách rõ ràng, lưu trữ nó hoặc đếm các số 0 ở cuối khi chúng ta nhân lên. Mặc dù cách tiếp cận này đơn giản về mặt khái niệm nhưng nó yêu cầu phép nhân O(n) và dẫn đến các số có hàng trăm nghìn chữ số cho _n_ gần 1.000.000, khiến nó không khả thi trong thực tế. 

Thông tin chi tiết quan trọng là số 0 ở cuối được tạo ra theo hệ số 10, do đó yêu cầu hệ số 5 kết hợp với hệ số 2. Hệ số 2 rất phong phú, do đó số 5 giây xác định trực tiếp số lượng số 0 ở cuối. Để đếm số 5 trong _n!_, chúng ta cần tính tổng các số nguyên chia của _n_ theo lũy thừa 5: n//5 đếm các số chia hết cho 5, n//25 đếm các số chia hết cho 25 (góp phần thêm hệ số 5), n//125 đếm các số chia hết cho 125, v.v. Điều này tiếp tục cho đến khi 5^k vượt quá _n_. Cách tiếp cận này làm giảm vấn đề đối với các phép toán O(log_5 n), cực kỳ nhanh đối với các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) phép nhân, số trung gian rất lớn | O(1) hoặc O(n) tùy theo dung lượng lưu trữ | Quá chậm | 
| Tối ưu | O(log_5 n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`zeros`về 0. Điều này sẽ tích lũy tổng số số 0 ở cuối. 
2. Đặt biến`power_of_5`đến 5, đại diện cho lũy thừa hiện tại của 5 đang được xem xét. 
3. Trong khi`power_of_5`nhỏ hơn hoặc bằng`n`, tính toán`n // power_of_5`và thêm nó vào`zeros`. Điều này đếm các số chia hết cho lũy thừa 5 này, mỗi số đóng góp thêm thừa số 5 vào giai thừa. 
4. Nhân`power_of_5`bằng 5 để chuyển sang lũy ​​thừa cao hơn tiếp theo. 
5. Lặp lại bước 3 cho đến khi`power_of_5`vượt quá`n`. 
6. Xuất ra giá trị của`zeros`. 

Tại sao nó hoạt động: Mỗi số chia hết cho 5 đóng góp ít nhất một hệ số 5. ​​Các số chia hết cho 25 đóng góp một hệ số bổ sung, những số chia hết cho 125 đóng góp một hệ số khác, v.v. Tính tổng các số này đảm bảo mọi hệ số của 5 được tính chính xác một lần, kết hợp với hệ số dồi dào của 2 để xác định chính xác số lượng số 0 ở cuối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
zeros = 0
power_of_5 = 5
while power_of_5 <= n:
    zeros += n // power_of_5
    power_of_5 *= 5
print(zeros)
```Giải pháp đầu tiên đọc đầu vào số nguyên. Vòng lặp lặp lại tính tổng các phần đóng góp từ mỗi lũy thừa của 5, bắt đầu bằng chính 5 và tăng theo cấp số nhân. nhân`power_of_5`bằng 5 đảm bảo chúng tôi chỉ xem xét các lũy thừa cao hơn đóng góp các thừa số bổ sung là 5. Kết quả được in trực tiếp. Cần thận trọng để tránh từng lỗi một bằng cách sử dụng`<=`trong điều kiện vòng lặp, đảm bảo lũy thừa của 5 chính xác bằng`n`được tính. 

## Ví dụ đã hoạt động 

Mẫu 1: n = 6 

| sức mạnh_of_5 | n // sức mạnh_of_5 | số không | 
| --- | --- | --- | 
| 5 | 6 // 5 = 1 | 1 | 
| 25 | 6 // 25 = 0 | 1 | 

Thuật toán cộng 1 từ các số chia hết cho 5, sau đó dừng vì 25 > 6. Kết quả là 1 số 0 ở cuối. 

Mẫu 2: n = 24 

| sức mạnh_of_5 | n // sức mạnh_of_5 | số không | 
| --- | --- | --- | 
| 5 | 24 // 5 = 4 | 4 | 
| 25 | 24 // 25 = 0 | 4 | 

Các số chia hết cho 5 có 4 thừa số là 5. Không có lũy thừa nào cao hơn của 5 trong 24. Giai thừa 24! có 4 số 0 ở cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log_5 n) | Mỗi lần lặp nhân lên`power_of_5`cho 5, tạo ra số logarit bậc thang cơ số 5 | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ, độc lập với n | 

Cho n ≤ 10^6, log_5(n) ≈ 10, do đó thuật toán thực hiện tối đa 10 lần lặp. Điều này dễ dàng phù hợp với giới hạn thời gian 3 giây với bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    zeros = 0
    power_of_5 = 5
    while power_of_5 <= n:
        zeros += n // power_of_5
        power_of_5 *= 5
    return str(zeros)

# provided samples
assert run("6\n") == "1", "sample 1"
assert run("24\n") == "4", "sample 2"

# custom cases
assert run("1\n") == "0", "minimum input"
assert run("5\n") == "1", "exactly one factor of 5"
assert run("25\n") == "6", "includes power of 25"
assert run("1000000\n") == "249998", "maximum input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | Xử lý đầu vào tối thiểu, không có số 0 ở cuối | 
| 5 | 1 | Đếm chính xác một thừa số duy nhất là 5 | 
| 25 | 6 | Tính tổng chính xác các thừa số từ 5 và 25 | 
| 1000000 | 249998 | Đảm bảo thuật toán xử lý n lớn hiệu quả | 

## Vỏ cạnh 

Với n = 1, điều kiện vòng lặp`5 <= 1`thất bại ngay lập tức và`zeros`vẫn bằng 0. Đầu ra chính xác là 0, xác nhận rằng thuật toán không phát hiện sai các số 0 ở cuối đối với các số nhỏ. 

Với n = 25, lần lặp đầu tiên cộng 25 // 5 = 5, lần lặp thứ hai cộng 25 // 25 = 1, kết quả là 6. Có chính xác 6 thừa số 5 trên 25!, tạo ra đúng 6 số 0 ở cuối. Thuật toán xử lý chính xác các số có lũy thừa chính xác là 5.
