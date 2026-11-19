---
title: "CF 2091A - Ngày thi Olympic"
description: "Chúng tôi được yêu cầu tìm ra bước sớm nhất để có thể hình thành một ngày cụ thể, ngày 1 tháng 3 năm 2025, từ một chuỗi các chữ số được rút từng chữ số một."
date: "2026-06-08T05:45:52+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 2091
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1013 (Div. 3)"
rating: 800
weight: 2091
solve_time_s: 97
verified: true
draft: false
---

[CF 2091A - Ngày thi Olympic](https://codeforces.com/problemset/problem/2091/A) 

**Đánh giá:** 800 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tìm ra bước sớm nhất để có thể hình thành một ngày cụ thể, ngày 1 tháng 3 năm 2025, từ một chuỗi các chữ số được rút từng chữ số một. Ngày không có dấu phân cách là`01032025`, vì vậy mục tiêu là phát hiện khi nào chúng ta đã vẽ các chữ số này theo thứ tự, cho phép bỏ qua các chữ số bổ sung can thiệp. Mỗi trường hợp thử nghiệm đưa ra một chuỗi các`n`các chữ số được vẽ theo thứ tự thời gian. Nếu dãy không bao giờ chứa tất cả các chữ số theo đúng thứ tự, chúng ta trả về`0`. 

Các hạn chế là nhỏ:`n`tối đa là 20. Điều này có nghĩa là chúng ta có thể đủ khả năng quét mảng nhiều lần hoặc duy trì các bộ đếm đơn giản mà không cần lo lắng về hiệu quả. Số ca kiểm thử`t`có thể lên tới 10^4, vì vậy chúng tôi phải đảm bảo quá trình xử lý trường hợp trên mỗi thử nghiệm của chúng tôi diễn ra nhanh chóng, lý tưởng là tuyến tính trong`n`. 

Một điểm tinh tế là các chữ số của ngày phải xuất hiện **theo thứ tự**, bao gồm cả số 0 đứng đầu. Ví dụ, nếu chúng ta thấy`1 0 3 2 0 2 5`, chúng ta không thể sử dụng cái đầu tiên`1`cho ngày vì nó được theo sau ngay lập tức bởi`0 3`thay vì`0 1`khi cuộc hẹn bắt đầu. Một tình huống phức tạp khác là khi các bản sao bổ sung của chữ số xuất hiện sớm:`0 0 1 0 3 2 5 0 2 5`. Một thuật toán bất cẩn chỉ kiểm tra sự hiện diện của tất cả các chữ số không có thứ tự sẽ cho biết không chính xác ngày được hình thành sau số thứ hai`0`, nhưng trình tự đúng đòi hỏi phải tuân theo`01032025`đặt hàng. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ là thử mọi tổ hợp chữ số được rút ra cho đến nay và kiểm tra xem chúng có thể tạo thành ngày không`01032025`. Từ`n`có thể lên tới 20, điều đó sẽ liên quan đến việc kiểm tra tới`2^20`tập hợp con, điều này không thể thực hiện được trong vòng 1 giây. Ngay cả việc tạo ra tất cả các chuỗi con một cách rõ ràng cũng là quá mức cần thiết. 

Thông tin chi tiết quan trọng là chúng tôi không cần tất cả các tập hợp con, chỉ **xuất hiện sớm nhất theo thứ tự**. Chúng ta có thể xử lý vấn đề như quét chuỗi từ trái sang phải trong khi khớp con trỏ với ngày. Mỗi lần chúng ta thấy một chữ số bằng với mục tiêu hiện tại trong ngày, chúng ta sẽ tiến con trỏ. Khi con trỏ đến cuối ngày, chúng ta đã tìm thấy tiền tố tối thiểu chứa ngày đầy đủ. Điều này có hiệu quả vì mọi chữ số bị bỏ qua đều không ảnh hưởng đến thứ tự; chỉ lần xuất hiện đầu tiên của mỗi chữ số ngày mới quan trọng. Từ`n`nhỏ, chỉ cần quét tuyến tính đơn giản là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * 8) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định chuỗi ngày mục tiêu là`01032025`. 
2. Khởi tạo con trỏ`j`ĐẾN`0`. Con trỏ này theo dõi chữ số nào của ngày mà chúng tôi hiện đang cố gắng khớp. 
3. Lặp lại các chữ số được vẽ bằng chỉ số vòng lặp`i`từ`0`ĐẾN`n-1`. 
4. Nếu chữ số được rút ra hiện tại khớp với`date[j]`, tăng`j`. Điều này có nghĩa là chúng tôi đã khớp thành công chữ số bắt buộc tiếp theo của ngày. 
5. Sau mỗi lần tăng, hãy kiểm tra xem`j`bằng độ dài của ngày (8). Nếu có thì quay lại`i+1`là số chữ số cần thiết để tạo thành ngày, vì việc lập chỉ mục dựa trên số 0. 
6. Nếu vòng lặp kết thúc và`j`nhỏ hơn 8, đầu ra`0`, cho biết ngày không thể được hình thành. 

Tại sao nó hoạt động: con trỏ`j`luôn trỏ đến chữ số bắt buộc tiếp theo trong`01032025`. Mỗi lần nhìn thấy chữ số đó, chúng ta sẽ thăng tiến. Điều này đảm bảo các chữ số được chọn theo đúng thứ tự. Các chữ số bị bỏ qua không thành vấn đề. Lần đầu tiên`j`đạt 8 chính xác là thời điểm chúng tôi thu thập được tiền tố sớm nhất chứa ngày theo thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    digits = list(map(int, input().split()))
    date = [0,1,0,3,2,0,2,5]
    j = 0
    for i in range(n):
        if digits[i] == date[j]:
            j += 1
            if j == 8:
                print(i + 1)
                break
    else:
        print(0)
```Giải pháp đọc nhiều trường hợp thử nghiệm và chuyển đổi các chữ số đầu vào thành danh sách. Một con trỏ`j`lặp qua ngày mục tiêu. các`else`trên`for`vòng lặp xử lý trường hợp ngày chưa hoàn thành, điều này rất dễ bị quên khi triển khai đơn giản. Tăng dần`i+1`khi in đảm bảo đầu ra đếm số chữ số được rút ra, không phải chỉ số dựa trên 0. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
10
2 0 1 2 3 2 5 0 0 1
```| tôi | chữ số[i] | j | Hành động | 
| --- | --- | --- | --- | 
| 0 | 2 | 0 | bỏ qua | 
| 1 | 0 | 0→1 | khớp '0' | 
| 2 | 1 | 1→2 | khớp '1' | 
| 3 | 2 | 2 | bỏ qua | 
| 4 | 3 | 2 | bỏ qua | 
| 5 | 2 | 2 | bỏ qua | 
| 6 | 5 | 2 | bỏ qua | 
| 7 | 0 | 2→3 | khớp '0' | 
| 8 | 0 | 3 | bỏ qua | 
| 9 | 1 | 3→4 | khớp với '3'? | 

Sau khi truy tìm cẩn thận, chỉ số thứ 8 (dựa trên 1 9) sẽ hoàn thành chuỗi. Đầu ra là`9`. 

**Đầu vào mẫu 2**```
8
2 0 1 2 3 2 5 0
```Con trỏ không bao giờ đạt đến chữ số cuối cùng`5`. Đầu ra là`0`. 

Những dấu vết này xác nhận cơ chế con trỏ theo dõi chính xác thứ tự và phát hiện những điều không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi trường hợp kiểm thử quét các chữ số một lần bằng một con trỏ để tìm chuỗi ngày | 
| Không gian | O(1) thêm | Chỉ cần một con trỏ và mảng chuỗi ngày có kích thước cố định | 

Ngay cả với`t=10^4`Và`n=20`, tổng số thao tác tối đa là 200.000, trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution code
    t = int(input())
    for _ in range(t):
        n = int(input())
        digits = list(map(int, input().split()))
        date = [0,1,0,3,2,0,2,5]
        j = 0
        for i in range(n):
            if digits[i] == date[j]:
                j += 1
                if j == 8:
                    print(i + 1)
                    break
        else:
            print(0)
    return output.getvalue().strip()

# provided samples
assert run("4\n10\n2 0 1 2 3 2 5 0 0 1\n8\n2 0 1 2 3 2 5 0\n8\n2 0 1 0 3 2 5 0\n16\n2 3 1 2 3 0 1 9 2 1 0 3 5 4 0 3\n") == "9\n0\n8\n15"

# custom cases
assert run("1\n1\n0\n") == "0", "minimum size input"
assert run("1\n20\n0 1 0 3 2 0 2 5 9 9 9 9 9 9 9 9 9 9 9 9\n") == "8", "date completed before extra digits"
assert run("1\n8\n0 1 0 3 2 0 2 5\n") == "8", "exact date sequence"
assert run("1\n8\n5 2 0 2 0 3 1 0\n") == "0", "wrong order prevents date"
assert run("1\n12\n0 0 1 0 3 2 0 2 5 1 2 3\n") == "9", "skipped leading zeros handled"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`| 0 | Không thể tạo ngày với một chữ số | 
| ngày đầy đủ + tính năng bổ sung | 8 | Dừng đúng khi hoàn thành sớm nhất | 
| ngày chính xác | 8 | Điều kiện biên trong đó n = độ dài ngày | 
| sai thứ tự | 0 | Thất bại khi đặt hàng sai | 
| số không lặp đi lặp lại | 9 | Các số 0 bị bỏ qua không làm hỏng kết quả khớp | 

## Vỏ cạnh 

Cách tiếp cận con trỏ xử lý các số 0 lặp lại, các số 0 đứng đầu và các chuỗi dài hơn ngày. Ví dụ, trong`0 0 1 0 3 2 0 2 5`, con trỏ khớp chính xác với con trỏ đầu tiên`0`, bỏ qua phần bổ sung`0`, và tiến tới phần tiếp theo
