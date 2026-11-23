---
title: "CF 56A - Thanh"
description: "Vasya bước vào quán bar và nhìn thấy một số khách hàng. Đối với mỗi khách hàng, anh ta chỉ biết một thông tin: tuổi của người đó hoặc đồ uống họ gọi. Ông muốn xác định có bao nhiêu người vẫn phải được kiểm tra để đảm bảo rằng không ai dưới 18 tuổi uống rượu."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 56
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 52 (Div. 2)"
rating: 1000
weight: 56
solve_time_s: 88
verified: true
draft: false
---

[CF 56A - Thanh](https://codeforces.com/problemset/problem/56/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vasya bước vào quán bar và nhìn thấy một số khách hàng. Đối với mỗi khách hàng, anh ta chỉ biết một thông tin: tuổi của người đó hoặc đồ uống họ gọi. Ông muốn xác định có bao nhiêu người vẫn phải được kiểm tra để đảm bảo rằng không ai dưới 18 tuổi uống rượu. 

Một khách hàng chắc chắn cần kiểm tra trong hai tình huống. Đầu tiên là khi chúng tôi đã biết khách hàng dưới 18 tuổi vì chúng tôi phải xác minh đồ uống. Thứ hai là khi chúng tôi đã biết khách hàng đặt rượu vì chúng tôi phải xác minh độ tuổi. 

Đầu vào là một chuỗi các chuỗi. Một số chuỗi là số đại diện cho độ tuổi và một số là tên đồ uống được viết bằng chữ in hoa. Nhiệm vụ chỉ đơn giản là đếm xem có bao nhiêu mục đáng ngờ và cần xác minh. 

Những hạn chế là rất nhỏ. Có tối đa 100 khách hàng và mỗi chuỗi có độ dài tối đa là 100. Ngay cả các giải pháp kém hiệu quả cũng sẽ chạy ngay lập tức. Quét tuyến tính qua đầu vào là quá đủ. 

Khó khăn chính không phải là hiệu suất mà là phân biệt chính xác độ tuổi và tên đồ uống. 

Một sai lầm dễ mắc phải là quên rằng độ tuổi chính xác là 18 là hợp pháp. Chỉ có độ tuổi dưới 18 mới cần kiểm tra. 

Ví dụ:```
3
17
18
19
```Đầu ra đúng là:```
1
```Chỉ có thanh niên 17 tuổi phải được kiểm tra. 

Một sai lầm phổ biến khác là coi mọi lời nói đều như rượu. Chỉ tính đồ uống trong danh sách nhất định. 

Ví dụ:```
4
WATER
JUICE
BEER
COKE
```Đầu ra đúng là:```
1
```Chỉ một`BEER`là nghiện rượu. 

Một lỗi tinh vi hơn xuất hiện khi phân tích số. đầu vào`"0"`hoặc`"5"`vẫn còn lâu đời và phải được xử lý bằng số chứ không phải dưới dạng tên đồ uống theo chuỗi. 

Ví dụ:```
2
5
VODKA
```Đầu ra đúng là:```
2
```Khách hàng đầu tiên chưa đủ tuổi vị thành niên và khách hàng thứ hai gọi rượu. 

## Phương pháp tiếp cận 

Việc giải thích vũ phu là rất trực tiếp. Đối với mỗi khách hàng, chúng tôi cố gắng xác định xem thông tin đã biết có chứng minh được rằng người đó có thể vi phạm pháp luật hay không. 

Nếu mã thông báo đầu vào là một số, chúng tôi so sánh nó với 18. Nếu độ tuổi nhỏ hơn, người này phải được kiểm tra. 

Nếu mã thông báo là một từ, chúng tôi sẽ so sánh nó với danh sách đồ uống có cồn. Nếu từ đó thuộc tập hợp đó thì người này cũng phải được kiểm tra. 

Mặc dù điều này nghe có vẻ giống như giải pháp cuối cùng nhưng trước tiên chúng ta vẫn có thể nghĩ đến phiên bản đơn giản. Giả sử chúng ta lưu trữ tên rượu trong một danh sách và đối với mỗi đồ uống, quét tuyến tính toàn bộ danh sách. Vì chỉ có 11 tên rượu và nhiều nhất là 100 khách hàng nên tổng công việc vẫn rất nhỏ, khoảng 1100 so sánh. 

Tối ưu hóa sạch hơn là lưu trữ tất cả đồ uống có cồn trong một bộ. Sau đó, việc kiểm tra thành viên trở thành thời gian trung bình không đổi. Điều này làm giảm logic xuống còn một lần chuyển qua đầu vào với việc tra cứu ngay lập tức. 

Quan sát chính là mỗi khách hàng có thể được đánh giá độc lập. Không có sự tương tác giữa các khách hàng khác nhau, do đó không cần sắp xếp, lập trình động hoặc xử lý đồ thị. Chúng tôi chỉ đơn giản là đếm các mục đáng ngờ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × 11) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả tên đồ uống có cồn trong một bộ. 

Một bộ cho phép kiểm tra tư cách thành viên nhanh chóng khi chúng ta gặp tên đồ uống. 
2. Đọc số lượng khách hàng`n`. 
3. Khởi tạo bộ đếm`ans = 0`. 
4. Đối với mỗi`n`chuỗi đầu vào: 

Kiểm tra xem chuỗi đại diện cho một số hay một từ. 
5. Nếu chuỗi là số: 

Chuyển đổi nó thành một số nguyên tuổi. 

Nếu dưới 18 tuổi thì tăng dần`ans`. 

Những khách hàng này có khả năng uống rượu trái phép nên phải kiểm tra. 
6. Nếu không, chuỗi này là tên đồ uống. 

Nếu đồ uống có trong bộ đồ uống có cồn, hãy tăng`ans`. 

Những khách hàng này có thể chưa đủ tuổi nên phải được kiểm tra. 
7. In`ans`. 

### Tại sao nó hoạt động 

Mỗi khách hàng rơi vào đúng một trong hai loại: đã biết độ tuổi hoặc đã biết đồ uống. 

Nếu chúng tôi biết độ tuổi và dưới 18 tuổi, chúng tôi phải kiểm tra đồ uống để xác minh tính hợp pháp. Nếu biết đồ uống đó có cồn thì chúng ta phải kiểm tra niên đại. 

Những khách hàng ít nhất 18 tuổi đã được an toàn bất kể đồ uống. Khách hàng uống đồ uống không cồn cũng đã an toàn bất kể tuổi tác. 

Thuật toán đếm chính xác những khách hàng thiếu thông tin có thể phát hiện hành vi vi phạm. Vì mỗi khách hàng được xử lý độc lập và mọi trường hợp đáng ngờ đều được tính một lần nên câu trả lời cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

alcohol = {
    "ABSINTH",
    "BEER",
    "BRANDY",
    "CHAMPAGNE",
    "GIN",
    "RUM",
    "SAKE",
    "TEQUILA",
    "VODKA",
    "WHISKEY",
    "WINE"
}

n = int(input())
ans = 0

for _ in range(n):
    s = input().strip()

    if s.isdigit():
        if int(s) < 18:
            ans += 1
    else:
        if s in alcohol:
            ans += 1

print(ans)
```Giải pháp bắt đầu bằng việc cất giữ tất cả đồ uống có cồn trong một bộ. Tập hợp là cấu trúc tự nhiên ở đây vì việc kiểm tra tư cách thành viên diễn ra nhanh chóng và ngắn gọn. 

Vòng lặp xử lý một khách hàng tại một thời điểm. phương pháp`isdigit()`tách biệt rõ ràng độ tuổi khỏi tên đồ uống. Vì tên đồ uống chỉ chứa chữ in hoa nên không có sự mơ hồ. 

Khi mã thông báo là số, mã sẽ chuyển đổi nó thành số nguyên và kiểm tra xem nó có nhỏ hơn 18 hay không. Sử dụng`< 18`thay vì`<= 18`là quan trọng vì 18 tuổi là hợp pháp. 

Khi mã thông báo không phải là số, mã sẽ kiểm tra xem đồ uống có thuộc bộ rượu hay không. Chỉ những đồ uống được liệt kê mới được tính là có cồn. 

Thuật toán không bao giờ lưu trữ đồng thời tất cả khách hàng, do đó mức sử dụng bộ nhớ không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
18
VODKA
COKE
19
17
```| Bước | Đầu vào | Loại | Khả nghi? | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 18 | tuổi | Không | 0 | 
| 2 | VODKA | rượu | Có | 1 | 
| 3 | COKE | không cồn | Không | 1 | 
| 4 | 19 | tuổi | Không | 1 | 
| 5 | 17 | tuổi | Có | 2 | 

Đầu ra cuối cùng:```
2
```Dấu vết này cho thấy cả hai loại khách hàng đáng ngờ.`VODKA`yêu cầu kiểm tra tuổi, trong khi`17`yêu cầu kiểm tra đồ uống. 

### Ví dụ 2 

đầu vào:```
6
WINE
16
JUICE
18
BEER
7
```| Bước | Đầu vào | Loại | Khả nghi? | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | RƯỢU | rượu | Có | 1 | 
| 2 | 16 | tuổi | Có | 2 | 
| 3 | NƯỚC ÉP | không cồn | Không | 2 | 
| 4 | 18 | tuổi | Không | 2 | 
| 5 | BIA | rượu | Có | 3 | 
| 6 | 7 | tuổi | Có | 4 | 

Đầu ra cuối cùng:```
4
```Ví dụ này thực hiện tất cả các điều kiện biên. Tuổi`18`được coi là hợp pháp một cách chính xác, trong khi đồ uống không cồn như`JUICE`bị bỏ qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi khách hàng được xử lý một lần | 
| Không gian | O(1) | Bộ cồn có kích thước không đổi | 

Với tối đa 100 khách hàng, chương trình sẽ diễn ra ngay lập tức. Việc sử dụng bộ nhớ cũng không đáng kể vì danh sách rượu luôn chứa chính xác 11 chuỗi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    alcohol = {
        "ABSINTH",
        "BEER",
        "BRANDY",
        "CHAMPAGNE",
        "GIN",
        "RUM",
        "SAKE",
        "TEQUILA",
        "VODKA",
        "WHISKEY",
        "WINE"
    }

    n = int(input())
    ans = 0

    for _ in range(n):
        s = input().strip()

        if s.isdigit():
            if int(s) < 18:
                ans += 1
        else:
            if s in alcohol:
                ans += 1

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run(
"""5
18
VODKA
COKE
19
17
""") == "2", "sample 1"

# minimum-size input
assert run(
"""1
18
""") == "0", "minimum case"

# all alcoholic drinks
assert run(
"""3
BEER
WINE
VODKA
""") == "3", "all alcoholic"

# boundary ages
assert run(
"""4
17
18
19
0
""") == "2", "boundary ages"

# mixed safe and unsafe
assert run(
"""6
WATER
JUICE
5
TEQUILA
100
MILK
""") == "2", "mixed case"

# maximum-style repeated inputs
assert run(
"""5
1
2
3
4
5
""") == "5", "all underage"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Khách hàng độc thân 18 tuổi | 0 | Tuổi ranh giới pháp lý | 
| Chỉ đồ uống có cồn | 3 | Tra cứu đồ uống đúng cách | 
| Tuổi 17, 18, 19, 0 | 2 | So sánh chặt chẽ với 18 | 
| Đồ uống hỗn hợp và lứa tuổi | 2 | Logic kết hợp | 
| Tất cả các lứa tuổi vị thành niên | 5 | Mỗi khách hàng chưa đủ tuổi được tính | 

## Vỏ cạnh 

Hãy xem xét độ tuổi ranh giới pháp lý. 

đầu vào:```
3
17
18
19
```Thuật toán xử lý từng giá trị bằng số.`17 < 18`, do đó câu trả lời trở thành 1.`18 < 18`là sai, vì vậy câu trả lời vẫn là 1.`19 < 18`cũng là sai. 

Đầu ra cuối cùng là:```
1
```Điều này xác nhận thuật toán xử lý chính xác 18 là hợp pháp. 

Bây giờ hãy xem xét đồ uống chưa biết không có cồn. 

đầu vào:```
4
WATER
COKE
BEER
JUICE
```Thuật toán kiểm tra từng từ dựa trên bộ rượu.`WATER`không có mặt.`COKE`không có mặt.`BEER`có mặt, vì vậy câu trả lời tăng lên 1.`JUICE`không có mặt. 

Đầu ra cuối cùng là:```
1
```Điều này ngăn ngừa sai lầm phổ biến khi coi mọi tên đồ uống đều là rượu. 

Cuối cùng, hãy xem xét độ tuổi rất nhỏ. 

đầu vào:```
2
0
VODKA
```Mã thông báo`0`là số nên được chuyển thành số nguyên 0. Vì`0 < 18`, câu trả lời trở thành 1.`VODKA`thuộc tập rượu nên đáp án là 2. 

Đầu ra cuối cùng là:```
2
```Điều này xác nhận thuật toán xử lý chính xác các chuỗi số có một chữ số theo độ tuổi thay vì tên đồ uống.
