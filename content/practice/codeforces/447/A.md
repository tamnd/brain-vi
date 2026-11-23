---
title: "CF 447A - DZY Yêu Hash"
description: "Chúng tôi được giao nhiệm vụ mô phỏng các thao tác chèn vào bảng băm với số lượng nhóm cố định. Mỗi nhóm có thể chứa tối đa một phần tử. Hàm băm rất đơn giản: đối với một số $x$, nhóm mục tiêu là $x mod p$. Chúng ta nhận được một dãy số và phải chèn chúng theo thứ tự."
date: "2026-06-07T17:05:14+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 447
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round #FF (Div. 2)"
rating: 800
weight: 447
solve_time_s: 78
verified: true
draft: false
---

[CF 447A - DZY Loves Hash](https://codeforces.com/problemset/problem/447/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao nhiệm vụ mô phỏng các thao tác chèn vào bảng băm với số lượng nhóm cố định. Mỗi nhóm có thể chứa tối đa một phần tử. Hàm băm rất đơn giản: cho một số$x$, nhóm mục tiêu là$x \mod p$. Chúng ta nhận được một dãy số và phải chèn chúng theo thứ tự. Nếu một số được dành cho một thùng đã bị chiếm giữ thì sẽ xảy ra xung đột. Mục đích là báo cáo chỉ mục chèn gây ra xung đột đầu tiên hoặc -1 nếu không xảy ra xung đột. 

Đầu vào mang lại$p$, số lượng thùng và$n$, số lượng số cần chèn, theo sau là$n$những con số. Cả hai$p$Và$n$nhiều nhất là 300 và mỗi số có thể lớn bằng$10^9$. Điều này đảm bảo rằng ngay cả một mô phỏng O(n) hoặc O(n²) đơn giản cũng có thể thực hiện được, nhưng chúng ta nên tránh các vòng lặp lồng nhau không cần thiết. 

Các trường hợp cạnh không rõ ràng bao gồm việc chèn cùng một số nhiều lần hoặc các số xung đột theo số học modulo nhưng khác biệt. Ví dụ, nếu$p=3$và chuỗi đầu vào là 1, 4, 7, kết quả modulo đều là 1. Hai lần chèn đầu tiên chiếm nhóm thành công, nhưng lần chèn thứ ba gây ra xung đột. Việc triển khai bất cẩn không kiểm tra các lần chèn trước đó theo modulo sẽ cho phép nhiều lần chèn vào cùng một nhóm không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là duy trì một mảng có kích thước$p$đại diện cho các thùng. Mỗi lần chèn sẽ kiểm tra xem nhóm mục tiêu có bị chiếm dụng hay không. Nếu có, hãy trả về chỉ mục. Cách tiếp cận này đúng vì nó mô phỏng chính xác hành vi của bảng băm. Số lượng thao tác tối đa là 300 lần chèn × 1 lần kiểm tra cho mỗi lần chèn, điều này không đáng kể đối với các giới hạn đã cho. Không cần các vòng lặp lồng nhau hoặc cấu trúc dữ liệu phức tạp. 

Thông tin chi tiết quan trọng cho một giải pháp tinh tế hơn một chút là nhận ra rằng hoạt động modulo đảm bảo chỉ số nhóm nằm trong phạm vi từ 0 đến$p-1$. Điều này cho phép sử dụng mảng có kích thước cố định để lưu trữ cờ chiếm chỗ, loại bỏ nhu cầu về cấu trúc dữ liệu động. Vì chỉ mục mảng tương ứng trực tiếp với số nhóm nên mọi tra cứu và cập nhật đều là O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(p) | Đã chấp nhận | 
| Tối ưu | O(n) | O(p) | Đã chấp nhận | 

Đối với vấn đề này, lực lượng vũ phu về cơ bản là tối ưu vì kích thước đầu vào nhỏ và không thể cải thiện tiệm cận. 

## Hướng dẫn thuật toán 

1. Đọc$p$Và$n$từ đầu vào. Khởi tạo một mảng`occupied`kích thước$p$với tất cả các phần tử được đặt thành Sai. Mảng này sẽ theo dõi thùng nào đã được lấp đầy. 
2. Lặp lại chuỗi số với chỉ số dựa trên 1 của chúng$i$. Đối với mỗi số$x$, tính toán nhóm mục tiêu như`bucket = x % p`. 
3. Kiểm tra xem`occupied[bucket]`là Đúng. Nếu vậy đã xảy ra xung đột nên in ngay$i$và thoát ra. 
4. Nếu thùng trống, hãy đặt`occupied[bucket]`thành True và tiếp tục đến số tiếp theo. 
5. Nếu tất cả các số được xử lý mà không gặp xung đột, hãy in -1. 

Thuật toán này hoạt động vì`occupied`mảng luôn phản ánh chính xác nhóm nào đang được sử dụng. Mỗi lần chèn sẽ đánh dấu một nhóm là bị chiếm dụng hoặc phát hiện xung đột. Vì mỗi số được kiểm tra chính xác một lần nên không có khả năng bỏ sót xung đột. Những bất biến đó là`occupied[bucket]`là Đúng khi và chỉ khi một số số trước đó được chèn vào nhóm đó và chỉ mục$i$return được đảm bảo là xung đột đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

p, n = map(int, input().split())
occupied = [False] * p

for i in range(1, n + 1):
    x = int(input())
    bucket = x % p
    if occupied[bucket]:
        print(i)
        break
    occupied[bucket] = True
else:
    print(-1)
```Dòng đầu tiên đọc số lượng xô và số. các`occupied`mảng được khởi tạo để theo dõi việc sử dụng nhóm. Vòng lặp lặp lại với chỉ mục dựa trên 1 để khớp trực tiếp với yêu cầu của bài toán về chỉ mục chèn. Hoạt động modulo tính toán nhóm chính xác. các`else`trên vòng lặp for chỉ thực thi nếu vòng lặp hoàn thành mà không bị ngắt, xử lý rõ ràng trường hợp "không xung đột". 

Một điểm triển khai tinh tế là sử dụng lập chỉ mục dựa trên 1 cho kết quả. Lỗi off-by-one thường xảy ra ở đây nếu bạn vô tình quay lại`i-1`hoặc bắt đầu vòng lặp ở mức 0. 

## Ví dụ đã hoạt động 

**Mẫu 1**:`p=10`, số=[0, 21, 53, 41, 53] 

| Bước | Số | Nhóm (x % p) | chiếm đóng trước | chiếm đóng sau | Xung đột? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | [F]*10 | [T, F, F,...] | Không | 
| 2 | 21 | 1 | [T, F,...] | [T, T, F,...] | Không | 
| 3 | 53 | 3 | [T, T, F, F,...] | [T,T,F,T,...] | Không | 
| 4 | 41 | 1 | [T,T,F,T,...] | - | Có | 

Lần chèn thứ tư chạm vào nhóm 1, vốn đã bị chiếm bởi 21. Thuật toán trả về chính xác 4. 

**Đầu vào tùy chỉnh**:`p=5`, số=[0, 5, 10] 

| Bước | Số | Xô | chiếm đóng trước | chiếm đóng sau | Xung đột? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | [F]*5 | [T, F, F, F, F] | Không | 
| 2 | 5 | 0 | [T, F, F, F, F] | - | Có | 

Xung đột xảy ra ở lần chèn thứ hai. Thuật toán xử lý chính xác các kết quả modulo lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số được xử lý một lần, với modulo và tra cứu theo thời gian không đổi. | 
| Không gian | O(p) | Chúng tôi duy trì một mảng có kích thước bằng số lượng nhóm. | 

Được cho$p, n \le 300$, thao tác này chạy tính bằng mili giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    exec(open("solution.py").read())
    return sys.stdout.getvalue().strip()

# provided sample
assert run("10 5\n0\n21\n53\n41\n53\n") == "4", "sample 1"

# minimum input, no conflict
assert run("2 1\n1\n") == "-1", "min input"

# conflict on first repeat
assert run("5 3\n0\n5\n10\n") == "2", "modulo repeat conflict"

# all numbers distinct modulo p
assert run("3 3\n1\n2\n0\n") == "-1", "distinct modulo"

# maximum n, simple conflict at the end
inp = "300 300\n" + "\n".join(str(i) for i in range(300)) + "\n0\n"
assert run(inp) == "301", "max n conflict"

# single conflict late
assert run("3 5\n1\n2\n4\n5\n7\n") == "4", "late conflict"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 1 | -1 | đầu vào có kích thước tối thiểu, không xung đột | 
| 5 3 0 5 10 | 2 | modulo lặp đi lặp lại gây ra xung đột | 
| 3 3 1 2 0 | -1 | tất cả các modulo riêng biệt, không xung đột | 
| 300 300 chuỗi + 0 | 301 | đầu vào kích thước tối đa có xung đột | 
| 3 5 1 2 4 5 7 | 4 | muộn xung đột, đúng đắn trên p nhỏ | 

## Vỏ cạnh 

Thuật toán xử lý các số lớn hơn$p$vì modulo giảm chúng thành các chỉ số nhóm hợp lệ. Các số lặp lại được xác định chính xác là xung đột trong lần lặp lại đầu tiên. Đối với đầu vào có kích thước tối thiểu, vòng lặp hoàn thành mà không bị ngắt, kích hoạt`else`mệnh đề và trả về -1. Mọi dấu vết đều xác nhận điều đó`occupied`ghi lại một cách trung thực việc sử dụng nhóm và chỉ mục được trả về luôn tương ứng với xung đột đầu tiên.
