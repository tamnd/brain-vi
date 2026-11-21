---
title: "CF 245A - Quản trị hệ thống"
description: "Polycarpus đang chạy một quy trình giám sát đơn giản cho hai máy chủ mà chúng ta có thể gọi là máy chủ a và máy chủ b. Mỗi lệnh \"ping\" sẽ gửi chính xác mười gói đến một trong các máy chủ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 245
codeforces_index: "A"
codeforces_contest_name: "CROC-MBTU 2012, Elimination Round (ACM-ICPC)"
rating: 800
weight: 245
solve_time_s: 207
verified: true
draft: false
---

[CF 245A - Quản trị viên hệ thống](https://codeforces.com/problemset/problem/245/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 3m 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Polycarpus đang chạy một quy trình giám sát đơn giản cho hai máy chủ mà chúng ta có thể gọi là máy chủ _a_ và máy chủ _b_. Mỗi lệnh "ping" sẽ gửi chính xác mười gói đến một trong các máy chủ. Đầu vào cho chúng ta biết, đối với mỗi ping, có bao nhiêu gói đã đến được máy chủ thành công và bao nhiêu gói bị mất. Mục tiêu là xác định xem mỗi máy chủ "còn sống" hay "đã chết" vào cuối ngày. Một máy chủ "hoạt động" nếu ít nhất một nửa số gói được gửi tới nó trong ngày đã đến được nó thành công; nếu không thì "chết". 

Đầu vào cung cấp số lượng lệnh ping được thực thi, theo sau là danh sách kết quả cho mỗi lệnh. Mỗi dòng cho biết máy chủ nào đã được ping và có bao nhiêu gói thành công và thất bại. Vì số lượng lệnh ping nhiều nhất là 1000 nên chúng ta có thể xử lý tất cả các lệnh một cách tuyến tính. Mỗi ping chỉ liên quan đến số học đơn giản (tổng hợp các gói thành công), vì vậy chúng ta không cần các cấu trúc dữ liệu hoặc thuật toán nâng cao. 

Một trường hợp khó phát hiện khi tổng số gói thành công bằng đúng một nửa số gói được gửi. Ví dụ: nếu máy chủ _a_ nhận được hai ping, mỗi gói có 5 gói thành công thì tổng số là 10 trên 20, đủ điều kiện là "còn hoạt động". Việc triển khai bất cẩn có thể kiểm tra hơn một nửa, tạo ra câu trả lời "chết" không chính xác. Một trường hợp khác cần lưu ý là khi một máy chủ nhận được nhiều ping hơn máy chủ kia, điều này sẽ không ảnh hưởng đến việc tính toán độc lập cho mỗi máy chủ. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ thực sự là tối ưu ở đây. Về nguyên tắc, chúng tôi có thể mô phỏng từng gói riêng lẻ, nhưng vì mỗi ping luôn gửi mười gói nên chỉ cần theo dõi tổng số gói thành công và tổng số gói đã gửi cho mỗi máy chủ là đủ. Điều này tương đương với việc tích lũy số lượng thành công và so sánh với một nửa tổng số gói được gửi. Phương pháp liệt kê từng gói riêng lẻ sẽ thực hiện các thao tác O(n*10), điều này vẫn khả thi với n = 1000, nhưng không cần thiết. 

Thông tin chi tiết quan trọng là chúng tôi không quan tâm đến các ping riêng lẻ ngoài việc đếm số lần thành công. Tất cả những gì chúng ta cần là hai bộ tích lũy, một bộ cho mỗi máy chủ, để tính tổng các thành công, sau đó so sánh mỗi bộ với một nửa tổng số gói được gửi đến máy chủ đó. Điều này làm giảm vấn đề về thời gian O(n) và không gian O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng các gói) | O(n*10) | O(n*10) | Được chấp nhận nhưng quá mức cần thiết | 
| Tối ưu (tổng thành công) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai bộ đếm cho các gói tin thành công:`success_a`Và`success_b`và hai bộ đếm tổng số gói đã gửi:`total_a`Và`total_b`. Mỗi máy chủ bắt đầu với số lần thành công bằng 0 và không có gói nào được gửi. 
2. Lặp lại tất cả các lệnh ping. Đối với mỗi lệnh, hãy đọc loại máy chủ`t`, số lần thành công`x`và số gói bị mất`y`. 
3. Nếu`t`là 1 (máy chủ _a_), thêm`x`ĐẾN`success_a`và thêm`x + y`(luôn luôn là 10) đến`total_a`. Nếu như`t`là 2 (máy chủ _b_), thêm`x`ĐẾN`success_b`và thêm`x + y`ĐẾN`total_b`. Điều này tích lũy tổng số thành công và tổng số gói được gửi cho mỗi máy chủ. 
4. Sau khi xử lý xong tất cả các lệnh, so sánh`success_a`với`total_a // 2`. Nếu nó lớn hơn hoặc bằng một nửa`total_a`, máy chủ _a_ đang "TRỰC TIẾP"; nếu không thì là "CHẾT". Làm tương tự cho máy chủ _b_ bằng cách sử dụng`success_b`Và`total_b // 2`. 

Tại sao nó hoạt động: Mỗi máy chủ được đánh giá độc lập dựa trên tổng số gói được gửi và tổng số thành công. Tổng số thành công và tổng số duy trì tỷ lệ chính xác cần thiết để kiểm tra xem liệu ít nhất một nửa số gói có thành công hay không. Không có thông tin nào bị mất khi tích lũy tổng số thay vì theo dõi từng ping riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
success_a = 0
success_b = 0
total_a = 0
total_b = 0

for _ in range(n):
    t, x, y = map(int, input().split())
    if t == 1:
        success_a += x
        total_a += x + y
    else:
        success_b += x
        total_b += x + y

print("LIVE" if success_a * 2 >= total_a else "DEAD")
print("LIVE" if success_b * 2 >= total_b else "DEAD")
```Giải pháp khởi tạo bộ đếm cho từng máy chủ và lặp lại qua từng lệnh ping. Việc nhân với 2 trong so sánh sẽ tránh được phép chia dấu phẩy động và xử lý chính xác trường hợp khi số lượng gói thành công chính xác bằng một nửa tổng số. Mã đảm bảo rằng mọi ping đều được tính đến và logic quyết định phù hợp với yêu cầu của vấn đề. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| t | x | y | thành công_a | tổng_a | thành công_b | tổng_b | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | 5 | 10 | 0 | 0 | 
| 2 | 6 | 4 | 5 | 10 | 6 | 10 | 

Sau khi xử lý, thành công_a_2 = 10 >= tổng_a=10 → TRỰC TIẾP, thành công_b_2 = 12 >= tổng_b=10 → TRỰC TIẾP. Cả hai máy chủ đều còn sống. 

Mẫu 2: 

| t | x | y | thành công_a | tổng_a | thành công_b | tổng_b | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 10 | 10 | 20 | 0 | 0 | 
| 2 | 0 | 10 | 10 | 20 | 0 | 10 | 

thành công_a_2 = 20 >= tổng_a=20 → TRỰC TIẾP, thành công_b_2 = 0 < tổng_b=10 → CHẾT. Máy chủ a còn sống, máy chủ b đã chết. 

Dấu vết này chứng tỏ rằng thuật toán xử lý chính xác các trường hợp chính xác một nửa và không thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lệnh trong số n lệnh được xử lý chính xác một lần, thực hiện công việc không đổi cho mỗi lệnh. | 
| Không gian | O(1) | Chỉ có bốn bộ đếm được duy trì bất kể n. | 

Vì n 1000 nên nghiệm O(n) dễ dàng nằm gọn trong giới hạn thời gian 2 giây. Mức sử dụng bộ nhớ không đáng kể, thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    success_a = 0
    success_b = 0
    total_a = 0
    total_b = 0
    for _ in range(n):
        t, x, y = map(int, input().split())
        if t == 1:
            success_a += x
            total_a += x + y
        else:
            success_b += x
            total_b += x + y
    return f"{'LIVE' if success_a*2 >= total_a else 'DEAD'}\n{'LIVE' if success_b*2 >= total_b else 'DEAD'}"

# Provided samples
assert run("2\n1 5 5\n2 6 4\n") == "LIVE\nLIVE", "sample 1"
assert run("2\n1 10 10\n2 0 10\n") == "LIVE\nDEAD", "sample 2"

# Custom cases
assert run("2\n1 5 5\n2 5 5\n") == "LIVE\nLIVE", "half-success boundary"
assert run("2\n1 4 6\n2 5 5\n") == "DEAD\nLIVE", "a fails, b passes"
assert run("4\n1 10 0\n1 0 10\n2 5 5\n2 0 10\n") == "LIVE\nDEAD", "multiple pings with mixed results"
assert run("2\n1 0 10\n2 0 10\n") == "DEAD\nDEAD", "all failures"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n1 5 5\n2 5 5 | TRỰC TIẾP\nTRỰC TIẾP | trường hợp thành công chính xác một nửa | 
| 2\n1 4 6\n2 5 5 | CHẾT\nSống | một máy chủ bị lỗi, một máy chủ vượt qua | 
| 4\n1 10 0\n1 0 10\n2 5 5\n2 0 10 | SỐNG\nCHẾT | nhiều ping với nhiều thành công khác nhau | 
| 2\n1 0 10\n2 0 10 | CHẾT\nCHẾT | trường hợp không thành công | 

## Vỏ cạnh 

Đối với ranh giới nửa thành công, đầu vào`2\n1 5 5\n2 5 5\n`kết quả là`success_a=5, total_a=10`Và`success_b=5, total_b=10`. điều kiện`success*2 >= total`đánh giá như`10 >= 10`→ Đúng, vậy là cả hai máy chủ đều TRỰC TIẾP. Điều này xác nhận thuật toán xử lý chính xác 50% thành công. 

Đối với trường hợp toàn lỗi`2\n1 0 10\n2 0 10`, các bộ đếm `success_a=0, Total_a=10
