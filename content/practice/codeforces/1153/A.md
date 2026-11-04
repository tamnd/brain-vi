---
title: "CF 1153A - Dịch vụ và xe buýt"
description: "Serval sẽ đến bến xe vào thời điểm xác định t. Có n tuyến xe buýt, mỗi tuyến có xe buýt đầu tiên đến cách đó 4 phút và các chuyến tiếp theo cách nhau 1 phút. Serval sẽ bắt chuyến xe buýt đầu tiên đến sau hoặc đúng vào thời điểm t."
date: "2026-06-12T02:51:19+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 1153
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 551 (Div. 2)"
rating: 1000
weight: 1153
solve_time_s: 103
verified: true
draft: false
---

[CF 1153A - Dịch vụ và xe buýt](https://codeforces.com/problemset/problem/1153/A) 

**Đánh giá:** 1000 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Serval sẽ đến bến xe vào một thời điểm cụ thể`t`. có`n`các tuyến xe buýt, mỗi tuyến có xe buýt đầu tiên đến`s_i`phút và các chuyến xe buýt tiếp theo mỗi`d_i`phút. Serval sẽ bắt chuyến xe buýt đầu tiên đến sau hoặc đúng giờ`t`. Nếu có nhiều xe buýt đến cùng một thời điểm sớm nhất thì bất kỳ tuyến đường nào trong số đó đều được chấp nhận. Nhiệm vụ là xác định tuyến xe buýt nào Serval sẽ lên. 

Đầu vào bao gồm`n`Và`t`, theo sau là`n`cặp`(s_i, d_i)`thể hiện thời gian và khoảng thời gian đến đầu tiên của mỗi tuyến xe buýt. Đầu ra là một số nguyên duy nhất biểu thị chỉ số dựa trên 1 của tuyến xe buýt đã chọn. 

Các hạn chế ở mức vừa phải:`n`nhiều nhất là 100 và thời gian có thể lên tới`10^5`. Việc kiểm tra mạnh mẽ trên từng tuyến xe buýt là khả thi vì ngay cả khi chúng tôi lặp lại từng phút, số lượng hoạt động sẽ nằm trong giới hạn. Các trường hợp biên bao gồm xe buýt đầu tiên của tuyến đường đến đúng lúc`t`, các tuyến đường mà tất cả các xe buýt đều đến sau`t`hoặc nhiều tuyến chia sẻ xe buýt sớm nhất sau`t`. Ví dụ, nếu`t=2`và hai tuyến đường có xe buýt vào các thời điểm`[2,4,6]`Và`[2,5,8]`, cả hai đều là câu trả lời hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tạo ra tất cả thời gian đến cho mỗi tuyến xe buýt cho đến khi chúng ta đạt hoặc vượt qua`t`và sau đó chọn sớm nhất trong số đó. Điều này hoạt động vì`n`là nhỏ và các khoảng`d_i`được giới hạn. Tuy nhiên, việc xây dựng tất cả thời gian bus là không cần thiết và có thể tránh được bằng cách sử dụng số học đơn giản. 

Thông tin chi tiết quan trọng là đối với tuyến đường bắt đầu tại`s_i`với khoảng thời gian`d_i`, xe buýt đầu tiên Serval có thể bắt xảy ra lúc`s_i`nếu như`s_i >= t`. Ngược lại, chúng ta có thể tính toán bus đầu tiên sau`t`trực tiếp là bội số nhỏ nhất của`d_i`thêm vào`s_i`đó là ít nhất`t`. Về mặt toán học, đây là`s_i + ceil((t - s_i)/d_i) * d_i`. Sử dụng số học số nguyên, chúng ta có thể tính toán điều này với`(t - s_i + d_i - 1) // d_i`để tránh các phép toán dấu phẩy động. Điều này làm giảm vấn đề phải quét từng tuyến xe buýt một lần và tính toán xe buýt có thể bắt được sớm nhất của nó, đưa ra giải pháp O(n) kịp thời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tạo mọi lúc) | O(n * max((t - s_i)/d_i)) | O(1) | Hoạt động với t nhỏ, không hiệu quả trong khoảng thời gian lớn | 
| Tối ưu (tính toán trực tiếp) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`t`. 
2. Khởi tạo hai biến:`best_time`đến một giá trị rất lớn (để theo dõi xe buýt sớm nhất) và`best_route`đến -1. 
3. Đối với từng chỉ số tuyến xe buýt`i`từ 1 đến`n`, đọc`s_i`Và`d_i`. 
4. Nếu`s_i >= t`, đặt xe buýt tiếp theo cho tuyến đường này thành`s_i`. 
5. Ngược lại, hãy tính xem chúng ta cần đợi bao nhiêu khoảng thời gian`s_i`để đạt được ít nhất`t`. Đây là`(t - s_i + d_i - 1) // d_i`. Nhân số này với`d_i`và thêm`s_i`để có được giờ xe buýt sớm nhất cho tuyến đường này. 
6. Nếu thời gian xe buýt này ít hơn`best_time`, cập nhật`best_time`và thiết lập`best_route`ĐẾN`i`. 
7. Sau khi kiểm tra tất cả các tuyến đường, hãy in`best_route`. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà`best_time`luôn giữ chuyến xe buýt sớm nhất mà Serval có thể bắt được trong số tất cả các tuyến đường đã xử lý. Bằng cách tính toán chuyến xe buýt có thể bắt đầu tiên cho mỗi tuyến đường bằng số học số nguyên, chúng tôi đảm bảo rằng chúng tôi không bao giờ bỏ lỡ chuyến xe buýt thực tế sớm nhất và chuyến cuối cùng.`best_route`được đảm bảo là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, t = map(int, input().split())
best_time = float('inf')
best_route = -1

for i in range(1, n + 1):
    s, d = map(int, input().split())
    if s >= t:
        next_bus = s
    else:
        wait = (t - s + d - 1) // d
        next_bus = s + wait * d
    if next_bus < best_time:
        best_time = next_bus
        best_route = i

print(best_route)
```Mã đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Vòng lặp tính toán xe buýt có thể bắt được sớm nhất cho mỗi tuyến đường. các`(t - s + d - 1) // d`công thức đảm bảo chúng ta làm tròn số khi khoảng chia không đều. Đang cập nhật`best_time`Và`best_route`chỉ khi tìm thấy một xe buýt hoàn toàn sớm hơn mới tránh được sự mơ hồ và việc sử dụng chỉ mục dựa trên 1 mới phù hợp với kết quả đầu ra dự kiến ​​của vấn đề. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
6 4
9 5
```| Tuyến đường | s_i | d_i | next_bus | thời gian tốt nhất | best_route | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 6 | 4 | 6 | 6 | 1 | 
| 2 | 9 | 5 | 9 | 6 | 1 | 

Chuyến xe buýt sớm nhất là tuyến 1 vào thời điểm 6. Bảng phục vụ tuyến 1. 

### Mẫu 2 

đầu vào:```
3 2
2 2
2 3
2 4
```| Tuyến đường | s_i | d_i | next_bus | thời gian tốt nhất | best_route | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 2 | 1 | 
| 2 | 2 | 3 | 2 | 2 | 1 | 
| 3 | 2 | 4 | 2 | 2 | 1 | 

Tất cả các tuyến đều có xe buýt đầu tiên lúc 2. Bất kỳ tuyến nào từ 1 đến 3 đều hợp lệ; thuật toán chọn mức tối thiểu gặp phải đầu tiên, tuyến 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi tuyến đường được quét một lần và số học cho xe buýt tiếp theo là O(1) | 
| Không gian | O(1) | Chỉ có một số biến được duy trì; không cần cấu trúc dữ liệu lớn | 

Cho n ≤ 100, giải pháp này chạy trong một phần nghìn giây. Mức sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, t = map(int, input().split())
    best_time = float('inf')
    best_route = -1
    for i in range(1, n + 1):
        s, d = map(int, input().split())
        if s >= t:
            next_bus = s
        else:
            wait = (t - s + d - 1) // d
            next_bus = s + wait * d
        if next_bus < best_time:
            best_time = next_bus
            best_route = i
    return str(best_route)

# Provided samples
assert run("2 2\n6 4\n9 5\n") == "1", "sample 1"
assert run("3 2\n2 2\n2 3\n2 4\n") == "sample 2"

# Custom cases
assert run("1 1\n1 1\n") == "1", "single route catches immediately"
assert run("2 10\n1 3\n2 4\n") == "1", "earliest bus requires multiple intervals"
assert run("3 5\n2 2\n3 3\n5 1\n") == "3", "exact match at t"
assert run("3 7\n2 2\n3 3\n4 4\n") == "1", "multiple options, first min picked"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1 1 | 1 | Tuyến đơn đánh bắt ngay | 
| 2 10\n1 3\n2 4 | 1 | Chờ đợi nhiều khoảng thời gian, kiểm tra tính toán | 
| 3 5\n2 2\n3 3\n5 1 | 3 | Tuyến đường chính xác tại t được chọn | 
| 3 7\n2 2\n3 3\n4 4 | 1 | Nhiều tuyến đường đến cùng một xe buýt tiếp theo; chọn tối thiểu đầu tiên | 

## Vỏ cạnh 

Nếu`t`ở trước chiếc xe buýt đầu tiên của một tuyến đường,`s_i >= t`, chuyến xe buýt tiếp theo là`s_i`. Ví dụ,`t=1`,`s=5`,`d=3`cho`next_bus=5`. Thuật toán xác định chính xác đây là xe buýt sớm nhất. Khi nhiều tuyến đường có cùng một xe buýt tiếp theo sớm nhất, thuật toán sẽ chọn mức tối thiểu gặp phải đầu tiên. Vì`t=2`và xe buýt`[2,4]`,`[2,5]`,`[2,6]`,`best_time`trở thành 2 ở tuyến 1 và đầu ra là tuyến 1. Điều này thỏa mãn bài toán vì bất kỳ tuyến nào có xe buýt sớm nhất đều có thể chấp nhận được.
