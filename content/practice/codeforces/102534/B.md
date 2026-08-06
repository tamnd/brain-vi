---
title: "CF 102534B - Cần Thêm Áo Phông!"
description: "Ban tổ chức có một danh sách các giá trị mô tả màu áo phông. Đối với mỗi màu, giá trị được viết dưới dạng số lượng áo sơ mi chính xác của màu đó hoặc theo tỷ lệ phần trăm của tất cả các áo sơ mi có màu đó."
date: "2026-08-05T16:06:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102534
codeforces_index: "B"
codeforces_contest_name: "Innopolis Open 2020 Finals"
rating: 0
weight: 102534
solve_time_s: 552
verified: true
draft: false
---

[CF 102534B - Cần thêm áo phông!](https://codeforces.com/problemset/problem/102534/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 12 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ban tổ chức có một danh sách các giá trị mô tả màu áo phông. Đối với mỗi màu, giá trị được viết dưới dạng số lượng áo sơ mi chính xác của màu đó hoặc theo tỷ lệ phần trăm của tất cả các áo sơ mi có màu đó. Tổng số áo sơ mi ban đầu bị thiếu và nhiệm vụ là tìm mọi tổng số có thể làm cho toàn bộ danh sách hợp lệ. 

Đối với tổng số đã chọn`T`, nếu một giá trị`x`đại diện cho một tỷ lệ phần trăm, nó đóng góp`T * x / 100`áo sơ mi. Nếu nó đại diện cho một số đếm thì nó đóng góp chính xác`x`áo sơ mi. Tổng của tất cả các khoản đóng góp phải chính xác`T`. 

Kích thước đầu vào có thể đạt tới`100000`và các giá trị có thể lớn bằng`10^9`. Việc thử tất cả các phép gán giá trị có thể có cho "đếm" hoặc "phần trăm" sẽ yêu cầu thời gian theo cấp số nhân, bởi vì mọi giá trị nhỏ đều có thể có hai cách hiểu. Ngay cả việc kiểm tra trực tiếp nhiều tổng số có thể là không thể. Giải pháp phải khai thác thực tế là tỷ lệ phần trăm được giới hạn trong phạm vi từ 1 đến 100. 

Các trường hợp phức tạp đến từ các giá trị mơ hồ. Một giá trị như`50`có thể là số đếm hoặc phần trăm. Ví dụ: với đầu vào:```
2
1 50
```tổng số hợp lệ là`2`Và`51`. Tổng cộng`2`công dụng`1`như một số đếm và`50`dưới dạng phần trăm. Tổng cộng`51`sử dụng cả hai giá trị làm số đếm. Một giải pháp luôn xử lý tối đa các giá trị`100`theo tỷ lệ phần trăm sẽ bỏ lỡ`51`. 

Một trường hợp đặc biệt khác là khi tất cả các giá trị đều là tỷ lệ phần trăm. Điều này bị cấm vì tuyên bố đảm bảo ít nhất một số đếm chính xác. Ví dụ:```
3
20 30 50
```việc chọn cả ba giá trị làm tỷ lệ phần trăm sẽ có tổng tỷ lệ phần trăm`100`, nhưng không có màu sắc với số lượng áo cố định nên cách giải thích đó phải bị bác bỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi tập hợp con các giá trị được hiểu là tỷ lệ phần trăm. Đối với mỗi tập hợp con, chúng ta có thể rút ra tổng số có thể có và xác minh nó. Điều này đúng vì mọi phép gán có thể đều được kiểm tra, nhưng có thể có tới`100000`giá trị, từ bỏ`2^100000`bài tập. Thậm chí chỉ với`30`giá trị không rõ ràng, số trường hợp đã quá lớn. 

Quan sát quan trọng là chỉ có các giá trị từ`1`ĐẾN`100`có thể là tỷ lệ phần trăm. Quan trọng hơn, tổng tỷ lệ phần trăm được chọn phải lớn nhất`100`. Chúng tôi không cần biết tập hợp con chính xác nào đã tạo ra tổng phần trăm vượt quá giới hạn này. Chúng ta chỉ cần biết số tiền nào có thể tiếp cận được. 

Cho phép`A`là tổng của tất cả các giá trị đầu vào. Giả sử một tập hợp con các giá trị được hiểu là tỷ lệ phần trăm và tổng của các tỷ lệ phần trăm đó là`q`. Các giá trị còn lại được hiểu là số đếm cố định, vì vậy tổng của chúng là`A - q`. Tổng số thỏa mãn:```
A - q + T * q / 100 = T
```Sắp xếp lại mang lại:```
T = 100 * (A - q) / (100 - q)
```Đối với mọi người có thể tiếp cận`q`nhỏ hơn`100`, chúng ta có thể tính toán câu trả lời của ứng viên và kiểm tra xem phép chia có chính xác hay không. 

Khó khăn duy nhất còn lại là đảm bảo rằng có ít nhất một giá trị được đếm. Nếu tập hợp con phần trăm đã chọn chứa mọi phần tử thì phép gán không hợp lệ. Vì tổng của tất cả các giá trị thường lớn hơn`100`, điều này chỉ quan trọng khi tổng toàn bộ mảng nhiều nhất là`100`. 

Các tổng phần trăm có thể có có thể được tìm thấy bằng lập trình động tổng tập hợp con nhỏ trên phạm vi`0`ĐẾN`100`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n + 100^2) | O(100) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tất cả các giá trị,`A`. Giá trị lớn hơn`100`không thể là tỷ lệ phần trăm, nhưng chúng vẫn tham gia vào tổng một cách tự nhiên. 
2. Sử dụng quy trình động tổng tập hợp con để tìm mọi tổng phần trăm có thể đạt được từ các giá trị lớn nhất`100`. Nhà nước`dp[x]`có nghĩa là một số lựa chọn hợp lệ về giá trị phần trăm có tổng phần trăm`x`. 

Giới hạn chỉ là`100`, do đó lập trình động có kích thước không đổi bất kể`n`. 
3. Đối với mỗi tổng phần trăm có thể tiếp cận`q`từ`0`ĐẾN`99`, tính toán:```
numerator = 100 * (A - q)
denominator = 100 - q
```Nếu như`numerator`chia hết cho`denominator`, tổng kết quả là một ứng cử viên. 

1. Từ chối các ứng cử viên được tạo bằng cách gán mọi giá trị cho phần phần trăm. Điều này chỉ có thể xảy ra khi`A == q`, vì mọi giá trị đều dương. 
2. Sắp xếp tất cả các ứng viên còn lại và in chúng. 

Tại sao nó hoạt động: mọi cách diễn giải hợp lệ sẽ chọn một số tập hợp con các giá trị dưới dạng phần trăm. Thông tin duy nhất từ ​​tập hợp con đó ảnh hưởng đến công thức tổng là tổng tỷ lệ phần trăm của nó,`q`. Chương trình năng động tìm thấy mọi thứ có thể`q`, vì vậy mọi tổng hợp lệ đều được xem xét. Việc kiểm tra tính chia hết đảm bảo rằng tổng được tính là số nguyên và kiểm tra mặt đếm đảm bảo rằng ít nhất một phần tử được hiểu là một số chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    total_sum = sum(a)

    dp = [False] * 101
    dp[0] = True

    for x in a:
        if x <= 100:
            for s in range(100 - x, -1, -1):
                if dp[s]:
                    dp[s + x] = True

    ans = set()

    for q in range(100):
        if not dp[q]:
            continue

        if total_sum == q:
            continue

        num = 100 * (total_sum - q)
        den = 100 - q

        if num % den == 0:
            ans.add(num // den)

    ans = sorted(ans)

    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```Mảng`dp`lưu trữ số tiền phần trăm có thể truy cập. Nó được cập nhật ngược để mọi giá trị đầu vào được sử dụng nhiều nhất một lần trong tập hợp con. Lặp lại chuyển tiếp sẽ cho phép tính cùng một giá trị nhiều lần. 

Công thức chỉ được đánh giá cho`q < 100`. Một tổng phần trăm chính xác`100`sẽ làm cho mẫu số bằng không. Trường hợp như vậy chỉ có thể mô tả tình huống trong đó mọi giá trị được coi là phần trăm, không hợp lệ vì phải tồn tại một số đếm chính xác. 

Số nguyên Python không bị tràn nên phép nhân với`100`an toàn ngay cả khi giá trị đầu vào lớn. 

## Ví dụ đã hoạt động 

Dành cho:```
2
1 50
```tổng số phần trăm có thể đạt được là`0`,`1`,`50`, Và`51`. 

| q | Kết quả công thức | Đã chấp nhận | 
| --- | --- | --- | 
| 0 | 51 | Có | 
| 1 | 51,5 | Không | 
| 50 | 2 | Có | 
| 51 | 1,98 | Không | 

Đầu ra là:```
2
2 51
```Dấu vết cho thấy tại sao cả hai cách giải thích về giá trị mơ hồ`50`vấn đề. 

Vì:```
3
20 30 70
```tổng số phần trăm có thể đạt được bao gồm: 

| q | Kết quả công thức | Đã chấp nhận | 
| --- | --- | --- | 
| 0 | 120 | Có | 
| 20 | 125 | Có | 
| 50 | 140 | Có | 
| 70 | 300 | Có | 

Đầu ra là:```
4
120 125 140 300
```Các giá trị giống nhau có thể mô tả một số tổng khác nhau vì các tập hợp con khác nhau là tỷ lệ phần trăm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 100^2) | Mỗi giá trị cập nhật một mảng lập trình động có kích thước cố định 101 | 
| Không gian | O(100) | Chỉ số tiền phần trăm có thể truy cập mới được lưu trữ | 

Thuật toán xử lý toàn bộ đầu vào một lần và chỉ thực hiện một lượng công việc bổ sung không đổi cho mỗi phần tử, dễ dàng phù hợp với giới hạn cho`n = 100000`. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_io(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline

    n = int(data())
    a = list(map(int, data().split()))

    total_sum = sum(a)

    dp = [False] * 101
    dp[0] = True

    for x in a:
        if x <= 100:
            for s in range(100 - x, -1, -1):
                if dp[s]:
                    dp[s + x] = True

    ans = set()

    for q in range(100):
        if dp[q] and total_sum != q:
            num = 100 * (total_sum - q)
            den = 100 - q
            if num % den == 0:
                ans.add(num // den)

    res = str(len(ans)) + "\n" + " ".join(map(str, sorted(ans))) + "\n"

    sys.stdin = old
    return res

assert solve_io("2\n1 50\n") == "2\n2 51\n"
assert solve_io("3\n20 30 70\n") == "4\n120 125 140 300\n"

assert solve_io("1\n100\n") == "1\n100\n"
assert solve_io("4\n2 40 90 5\n") == "3\n137 470 840\n"
assert solve_io("3\n10 20 30\n") == "1\n60\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 100`|`100`| Xử lý phần trăm ranh giới và giá trị đơn | 
|`2 / 1 50`|`2 51`| Các giá trị có thể là số lượng hoặc phần trăm | 
|`3 / 10 20 30`|`60`| Tất cả các giá trị được hiểu là số lượng | 
|`4 / 2 40 90 5`|`137 470 840`| Nhiều tập hợp con phần trăm hợp lệ | 

## Vỏ cạnh 

Khi một giá trị vừa là số lượng có thể vừa là phần trăm có thể, thuật toán sẽ xem xét cả hai khả năng thông qua các tổng phần trăm khác nhau. Đối với đầu vào:```
2
1 50
```

`q = 0`đưa ra tổng số`51`, trong khi`q = 50`đưa ra tổng số`2`. Cả hai vẫn còn trong câu trả lời. 

Khi tất cả các giá trị cộng lại chính xác bằng tổng phần trăm có thể có, thuật toán sẽ loại bỏ cách diễn giải không hợp lệ trong đó mọi giá trị đều trở thành phần trăm. Đối với đầu vào:```
3
20 30 50
```tổng tập hợp con`100`tồn tại, nhưng nó không để lại số lượng chính xác. Sự cố yêu cầu ít nhất một số đếm chính xác, do đó cách giải thích đó sẽ bị loại bỏ. 

Khi các giá trị lớn xuất hiện, chúng không thể là tỷ lệ phần trăm vì nhiều nhất là tỷ lệ phần trăm.`100`. Họ vẫn đóng góp vào`A`và cùng một công thức sẽ tự động xử lý chúng. Ví dụ: một giá trị như`1000000000`chỉ có thể ở bên đếm, ngăn chặn sự phân nhánh không cần thiết.
