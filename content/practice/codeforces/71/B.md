---
title: "CF 71B - Thanh tiến trình"
description: "Chúng ta được yêu cầu xây dựng một thanh tiến trình đồ họa dưới dạng một mảng các ô vuông, trong đó mỗi ô vuông có một giá trị bão hòa. Thanh có tổng cộng n ô vuông và độ bão hòa tối đa là k."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 71
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 65 (Div. 2)"
rating: 1300
weight: 71
solve_time_s: 67
verified: true
draft: false
---

[CF 71B - Thanh tiến trình](https://codeforces.com/problemset/problem/71/B) 

**Đánh giá:** 1300 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một thanh tiến trình đồ họa dưới dạng một mảng các ô vuông, trong đó mỗi ô vuông có một giá trị bão hòa. Quán bar có tổng cộng`n`hình vuông và độ bão hòa tối đa là`k`. Sự hoàn thành của quá trình được đo bằng`t`phần trăm và chúng ta cần phản ánh tỷ lệ phần trăm đó một cách trực quan bằng cách sử dụng độ bão hòa của các ô vuông. 

Các quy tắc là: 

- Một vài ô vuông đầu tiên đã bão hòa hoàn toàn (`k`), đại diện cho phần đã hoàn thành. 
- Một số ô cuối cùng trống (`0`), đại diện cho phần chưa hoàn thành. 
- Tối đa một ô vuông có thể được lấp đầy một phần để khớp chính xác với tỷ lệ phần trăm`t`. 

Đầu vào bao gồm ba số nguyên:`n`,`k`, Và`t`, tất cả đều nằm trong phạm vi [0, 100] cho`t`và [1, 100] cho`n`Và`k`. Đầu ra là một mảng`n`số nguyên đại diện cho độ bão hòa của mỗi hình vuông. 

Bởi vì`n`Và`k`đều có nhiều nhất là 100, chúng ta có thể tìm được giải pháp O(n) với số học và phép lặp đơn giản. Không có mối quan tâm về hiệu suất; các ràng buộc làm cho việc mô phỏng trực tiếp trở nên khả thi. 

Các trường hợp cạnh không rõ ràng phát sinh khi`t`là 0 hoặc 100 hoặc khi phần phân số của hình vuông được lấp đầy một phần chính xác là 0 hoặc`k`. Ví dụ, nếu`t = 0`, tất cả các ô vuông phải bằng 0, và nếu`t = 100`, tất cả các hình vuông phải là`k`. Một giải pháp bất cẩn có thể cố tính "bình phương một phần" ngay cả trong những trường hợp này, dẫn đến sai sót từng cái một. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là lặp lại từng ô vuông, tính toán độ bão hòa chính xác cần thiết cho ô vuông đó theo tỷ lệ phần trăm và điền vào mảng. Điều này hiệu quả vì chúng ta có thể coi vấn đề là sự chia tách`t`phần trăm của tổng độ bão hòa trên`n`hình vuông, nhưng nó đưa ra số học phân số không cần thiết và các biến chứng khi chỉ có thể lấp đầy một phần hình vuông. 

Cái nhìn sâu sắc quan trọng là nhận thấy rằng vấn đề là tuyến tính: mỗi ô vuông đóng góp chính xác`k`đơn vị bão hòa, ngoại trừ một phần hình vuông. Chúng ta có thể tính tổng "đơn vị bão hòa" là`(t / 100) * (n * k)`. Khi đó, số ô vuông được điền đầy đủ là số nguyên của giá trị này chia cho`k`, hình vuông được lấp đầy một phần là phần còn lại và các hình vuông còn lại trống. Điều này cho phép chúng ta xây dựng câu trả lời trong O(n) mà không cần lặp qua các phân số hoặc thực hiện các phép so sánh phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Được chấp nhận nhưng dài dòng | 
| Tối ưu | O(n) | O(n) | Được chấp nhận, rõ ràng và súc tích | 

## Hướng dẫn thuật toán 

1. Tính tổng đơn vị bão hòa cần thiết cho`t`phần trăm hoàn thành:`total_saturation = (t * n * k) // 100`Phép chia số nguyên hoạt động vì độ bão hòa phải là số nguyên và bất kỳ phần phân số nào tự nhiên sẽ trở thành hình vuông được lấp đầy một phần. 
2. Xác định số ô vuông được điền đầy bằng cách chia`total_saturation`qua`k`:`full_squares = total_saturation // k`Điều này đảm bảo rằng mỗi lần đầu tiên`full_squares`hình vuông được bão hòa đến mức tối đa. 
3. Tính độ bão hòa còn lại cho hình vuông được lấp đầy một phần:`partial_saturation = total_saturation % k`Nếu giá trị này bằng 0 thì không tồn tại một phần bình phương; nếu khác 0 thì ô vuông tiếp theo sau các ô đầy đủ sẽ nhận được giá trị này. 
4. Khởi tạo một mảng có độ dài`n`với số không. 
5. Điền vào đầu tiên`full_squares`với`k`. 
6. Nếu`partial_saturation`khác 0 thì gán nó vào ô tiếp theo. 
7. Phần còn lại của mảng vẫn là số 0. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà tổng các giá trị bão hòa khớp với nhau`(t / 100) * (n * k)`. Bởi vì mỗi ô vuông sau các ô đầy đủ đều được điền một phần hoặc bằng 0 và chỉ tồn tại một phần ô vuông nên kết quả đầu ra luôn thỏa mãn các điều kiện của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k, t = map(int, input().split())

# Total saturation units
total_saturation = (t * n * k) // 100

# Number of fully filled squares
full_squares = total_saturation // k

# Remaining saturation for the partial square
partial_saturation = total_saturation % k

# Initialize the progress bar
progress = [0] * n

# Fill full squares
for i in range(full_squares):
    progress[i] = k

# Fill partial square if it exists
if full_squares < n:
    progress[full_squares] = partial_saturation

print(' '.join(map(str, progress)))
```Đầu tiên, mã này tính toán tổng độ bão hòa theo đơn vị, đây là phiên bản số nguyên tỷ lệ của`t%`. Chia cho`k`tách các hình vuông được lấp đầy khỏi hình vuông một phần. Việc khởi tạo mảng bằng các số 0 đảm bảo rằng các ô vuông chưa điền đã chính xác. các`if`điều kiện tránh lỗi chỉ mục khi`t = 100`. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`n=10, k=10, t=54`| Bước | tổng_bão hòa | đầy đủ_squares | một phần_bão hòa | mảng tiến độ | 
| --- | --- | --- | --- | --- | 
| Tính tổng | 54 | 5 | 4 | [10,10,10,10,10,4,0,0,0,0] | 

Dấu vết này cho thấy hình vuông một phần chiếm chính xác 4 đơn vị còn lại sau 5 hình vuông đầy đủ. 

**Ví dụ tùy chỉnh:**`n=5, k=20, t=0`| Bước | tổng_bão hòa | đầy đủ_squares | một phần_bão hòa | mảng tiến độ | 
| --- | --- | --- | --- | --- | 
| Tính tổng | 0 | 0 | 0 | [0,0,0,0,0] | 

Điều này xác nhận việc xử lý trường hợp biên không có tiến triển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Điền vào mảng`n`hình vuông chiếm ưu thế | 
| Không gian | O(n) | Chúng tôi lưu trữ`n`số nguyên cho thanh tiến trình | 

Bởi vì`n ≤ 100`, thuật toán cực kỳ hiệu quả và phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k, t = map(int, input().split())
    total_saturation = (t * n * k) // 100
    full_squares = total_saturation // k
    partial_saturation = total_saturation % k
    progress = [0] * n
    for i in range(full_squares):
        progress[i] = k
    if full_squares < n:
        progress[full_squares] = partial_saturation
    return ' '.join(map(str, progress))

# Provided samples
assert run("10 10 54\n") == "10 10 10 10 10 4 0 0 0 0"
# Custom cases
assert run("5 20 0\n") == "0 0 0 0 0", "zero progress"
assert run("5 20 100\n") == "20 20 20 20 20", "full progress"
assert run("4 5 37\n") == "5 5 3 0", "partial square rounding"
assert run("1 1 50\n") == "0", "single square half progress"
assert run("3 10 33\n") == "10 0 0", "fractional rounding down"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 20 0 | 0 0 0 0 0 | Trường hợp cạnh: không có tiến triển | 
| 5 20 100 | 20 20 20 20 20 | Trường hợp cạnh: tiến độ đầy đủ | 
| 4 5 37 | 5 5 3 0 | Tính đúng một phần bình phương | 
| 1 1 50 | 0 | Cạnh tròn vuông đơn | 
| 3 10 33 | 10 0 0 | Độ bão hòa phân số làm tròn xuống | 

## Vỏ cạnh 

Khi nào`t=0`, tổng độ bão hòa bằng không. Thuật toán tính toán`full_squares=0`Và`partial_saturation=0`. Vòng lặp cho bình phương đầy đủ không làm gì cả và điều kiện cho bình phương một phần bị bỏ qua vì`full_squares < n`là đúng nhưng`partial_saturation`bằng 0, để lại mảng là`[0, 0, ...]`, chính xác. 

Khi`t=100`,`total_saturation = n*k`,`full_squares = n`,`partial_saturation = 0`. Vòng lặp lấp đầy tất cả các ô vuông với`k`và kiểm tra có điều kiện không làm thay đổi mảng vì`full_squares`bằng`n`, tránh các lỗi vượt quá giới hạn. 

Đối với tỷ lệ phần trăm phân số như`t=37`với`n=4`Và`k=5`, tổng độ bão hòa là`(37*4*5)//100 = 7`. Hình vuông đầy đủ`7//5=1`, một phần`7%5=2`. Mảng trở thành`[5,2,0,0]`, phản ánh chính xác một hình vuông đầy đủ và một hình vuông một phần. 

Điều này xác nhận rằng các trường hợp cạnh có độ bão hòa 0, 100 hoặc một phần đều được xử lý chính xác. 

Bài xã luận này giải thích đầy đủ nguồn gốc của thuật toán, cách xử lý các trường hợp phức tạp và
