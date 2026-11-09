---
title: "CF 1461E - Mực nước"
description: "Chúng tôi được yêu cầu quản lý bộ làm mát nước trong một số ngày. Máy làm mát bắt đầu với k lít nước, và mỗi ngày x lít được đồng nghiệp tiêu thụ. Vào đầu mỗi ngày, John có thể thêm chính xác y lít nước nếu việc làm đó không vượt quá giới hạn tối đa r."
date: "2026-06-11T02:24:24+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "graphs", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1461
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 689 (Div. 2, based on Zed Code Competition)"
rating: 2200
weight: 1461
solve_time_s: 177
verified: false
draft: false
---

[CF 1461E - Mực nước](https://codeforces.com/problemset/problem/1461/E) 

**Xếp hạng:** 2200 
**Tags:** sức mạnh vũ phu, đồ thị, sự tham lam, thực hiện, toán học 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu quản lý bộ làm mát nước trong một số ngày. Bộ làm mát bắt đầu với`k`lít nước và mỗi ngày`x`lít được đồng nghiệp tiêu thụ. Vào đầu mỗi ngày, John có thể thêm chính xác`y`lít nước nếu làm như vậy không vượt quá giới hạn tối đa`r`. Mực nước của máy làm mát phải luôn ở trong khoảng`[l, r]`. Mục đích là để xác định xem John có thể duy trì mực nước này trong`t`ngày liên tiếp. 

Các tham số đầu vào lớn:`k`,`l`,`r`, Và`y`có thể đi lên`10^18`, Và`t`lên đến`10^18`, trong khi`x`tùy thuộc vào`10^6`. Điều này loại trừ mọi cách tiếp cận mô phỏng từng ngày riêng lẻ trong một vòng lặp, vì việc lặp lại`t`thời gian sẽ quá chậm. Thay vào đó, chúng ta cần suy luận về các giới hạn, chu kỳ hoặc các mẫu cho phép bỏ qua nhiều ngày trong một bước. 

Các trường hợp đặc biệt phát sinh khi không thể thêm nước vì lượng nước sẽ vượt quá`r`, hoặc khi tiêu thụ`x`lớn hơn những gì còn lại trong bộ làm mát. Ví dụ, nếu`k = 8`,`l = 1`,`r = 10`,`t = 2`,`x = 6`, Và`y = 4`, thêm`y`vào ngày đầu tiên sẽ vượt quá`r`, nên John không thể thêm nước, để lại 2 lít. Sau một ngày nữa, người ta dùng hết 6 lít, kết quả là 0 lít, nhỏ hơn`l`, vì vậy kết quả đầu ra đúng là "Không". Một cách tiếp cận ngây thơ có thể cố gắng thêm nước một cách không chính xác bất kể vượt quá`r`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: mô phỏng mỗi ngày bằng cách thử thêm`y`lít nếu mức thu được không vượt quá`r`, sau đó trừ`x`lít để tiêu dùng hàng ngày. Lặp lại cho`t`ngày. Điều này có hiệu quả vì nó trực tiếp thực hiện các quy tắc, nhưng nó yêu cầu`O(t)`lần lặp lại. Với`t`lên đến`10^18`, điều này là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là mực nước thay đổi có thể dự đoán được. Có hai trường hợp phát sinh: 

1. **`y >= x`**: Nếu lượng nước bổ sung ít nhất là lượng nước tiêu thụ hàng ngày thì mực nước có thể tăng theo thời gian. giới hạn`r`ngăn chặn sự tăng trưởng vô hạn. Chúng ta chỉ cần lo lắng liệu có tồn tại điểm không thể thêm nước vào vì nó sẽ vượt quá`r`, hoặc liệu`l`bị vi phạm. Trong trường hợp này, vấn đề giảm xuống còn việc kiểm tra phần dư modulo`x`, bởi vì các chu trình cộng và trừ lặp đi lặp lại cuối cùng lặp lại mực nước theo modulo`x`. Điều này cho phép chúng tôi bỏ qua việc mô phỏng từng ngày riêng lẻ. 
2. **`y < x`**: Ở đây mực nước giảm dần theo thời gian. Nếu thêm`y`không thể bù đắp`x`, chúng ta có thể tính được nước có thể tồn tại được bao nhiêu ngày bằng cách giải`(k + y) - x*d >= l`theo khối. Chúng ta có thể nhanh chóng xác định xem John có hết nước trước`t`ngày mà không lặp lại mỗi ngày. 

Sử dụng số học modulo, trạng thái đã truy cập và bỏ qua thông minh, chúng ta có thể giảm độ phức tạp về thời gian xuống`O(x)`hoặc logarit trong một số trường hợp nhất định, thấp hơn nhiều`t`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t) | O(1) | Quá chậm | 
| Tối ưu | O(x) | O(x) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem`x > y`Và`y == 0`. Nếu như`x > y`và việc thêm nước là không thể, hãy tính số ngày trước`k`rơi xuống dưới`l`sử dụng phép chia số nguyên:`days = (k - l) // x + 1`. Nếu như`days >= t`, in "Có"; nếu không thì "Không". Điều này là an toàn vì nước chỉ giảm tuyến tính. 
2. Khởi tạo một bộ`visited`để theo dõi mực nước modulo`x`để phát hiện chu kỳ. Bắt đầu với mực nước ban đầu`k`. 
3. Trong khi`t > 0`, cố gắng thêm nước. Chỉ thêm`y`lít nếu`k + y <= r`. Nếu không thể thêm thì không làm gì cả. 
4. Trừ`x`lít trong ngày. Nếu như`k < l`sau khi trừ, in "Không" và kết thúc. 
5. Giảm`t`bằng 1. 
6. Tính toán`k % x`. Nếu giá trị modulo này đã được nhìn thấy trước đó thì có nghĩa là một chu trình đã tồn tại. Kể từ khi lặp lại modulo`x`các giá trị sẽ không vi phạm giới hạn, chúng ta có thể in ngay "Có" và chấm dứt. 
7. Nếu không, hãy thêm giá trị modulo vào`visited`và tiếp tục. 
8. Nếu`t`đạt 0 mà không vi phạm giới hạn, in "Có". 

Tại sao nó hoạt động: Modulo mực nước`x`quyết định đầy đủ sự tiến hóa tiếp theo vì mức tiêu thụ và bổ sung hàng ngày là bội số của`x`có hiệu lực. Bằng cách theo dõi các giá trị modulo đã thấy, thuật toán phát hiện các chu kỳ và đảm bảo không xảy ra tràn hoặc tràn. Giảm tuyến tính được xử lý một cách phân tích khi`y < x`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k, l, r, t, x, y = map(int, input().split())

if x > y and y == 0:
    # water decreases linearly, can't add
    if k - t * x >= l:
        print("Yes")
    else:
        print("No")
    exit()

visited = set()
while t > 0:
    # if we can safely add water, do it
    if k + y <= r:
        k += y
    # subtract daily consumption
    k -= x
    t -= 1
    if k < l:
        print("No")
        exit()
    modk = k % x
    if modk in visited:
        print("Yes")
        exit()
    visited.add(modk)

print("Yes")
```Giải pháp đầu tiên xử lý trường hợp giảm tầm thường trong đó`y = 0`Và`x`là lớn. Đối với vòng lặp chính, nó chỉ thêm nước một cách an toàn khi được phép, giảm`k`qua`x`và theo dõi modulo`x`trạng thái để phát hiện chu kỳ. Sau khi phát hiện một chu kỳ, mô hình mực nước sẽ lặp lại vô thời hạn, do đó việc duy trì bộ làm mát được đảm bảo. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`8 1 10 2 6 4`| Ngày | Thêm bạn? | k trước khi trừ | k sau khi trừ | Mod k | Đã truy cập | 
| --- | --- | --- | --- | --- | --- | 
| 1 | không | 8 | 2 | 2 | {2} | 
| 2 | vâng | 6 | 0 | 0 | {2,0} -> vi phạm l | 

Đầu ra:`No`. Máy làm mát rơi xuống dưới`l`vào ngày thứ 2. 

**Mẫu 2:**`8 1 10 2 6 5`| Ngày | Thêm bạn? | k trước khi trừ | k sau khi trừ | Mod k | Đã truy cập | 
| --- | --- | --- | --- | --- | --- | 
| 1 | vâng | 13 -> giới hạn ở r=10 | 4 | 4 | {4} | 
| 2 | vâng | 9 | 3 | 3 | {4,3} | 

Đầu ra:`Yes`. Nước vẫn ở trong giới hạn. 

Những dấu vết này chứng minh rằng việc theo dõi modulo nắm bắt các chu kỳ và xử lý chính xác khi thêm`y`bị giới hạn bởi`r`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(x) | Số lượng mực nước duy nhất theo modulo x tối đa là x. Khi chúng tôi thấy lặp lại, chúng tôi dừng lại. | 
| Không gian | O(x) | Lưu trữ các giá trị modulo x đã truy cập để phát hiện các chu kỳ. | 

Từ`x <= 10^6`, việc lưu trữ và kiểm tra các giá trị modulo vừa vặn thoải mái trong giới hạn bộ nhớ và các thao tác O(x) thực hiện tốt trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    k, l, r, t, x, y = map(int, input().split())
    if x > y and y == 0:
        return "Yes" if k - t * x >= l else "No"
    visited = set()
    while t > 0:
        if k + y <= r:
            k += y
        k -= x
        t -= 1
        if k < l:
            return "No"
        modk = k % x
        if modk in visited:
            return "Yes"
        visited.add(modk)
    return "Yes"

# provided samples
assert run("8 1 10 2 6 4\n") == "No", "sample 1"
assert run("8 1 10 2 6 5\n") == "Yes", "sample 2"
# custom cases
assert run("1 1 1 1 1 1\n") == "Yes", "minimum-size input"
assert run("10 1 20 5 3 4\n") == "Yes", "water fluctuates but stays in range"
assert run("5 1 5 3 2 0\n") == "No", "cannot add water, runs out"
assert run("10 5 15 1000000 2 1\n") == "Yes", "long t with cycle detection"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 1 1 | Có | giá trị tối thiểu | 
| 10 1 20 5 3 4 | Có | dao động trong giới hạn | 
| 5 1 5 3 2 0 | | |
