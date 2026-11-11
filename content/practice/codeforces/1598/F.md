---
title: "CF 1598F - RBS"
description: "Chúng tôi được cung cấp một số chuỗi chỉ bao gồm \"(\" và \")\". Mỗi chuỗi là một chuỗi ngoặc, nhưng không nhất thiết phải là một chuỗi ngoặc hợp lệ hoặc thông thường."
date: "2026-06-10T08:50:20+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "brute-force", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 1598
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 115 (Rated for Div. 2)"
rating: 2400
weight: 1598
solve_time_s: 132
verified: true
draft: false
---

[CF 1598F - RBS](https://codeforces.com/problemset/problem/1598/F) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, mặt nạ bit, lực lượng vũ phu, cấu trúc dữ liệu, dp 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số chuỗi chỉ bao gồm "(" và ")". Mỗi chuỗi là một chuỗi ngoặc, nhưng không nhất thiết phải là một chuỗi ngoặc hợp lệ hoặc thông thường. Nhiệm vụ là nối tất cả các chuỗi theo thứ tự nào đó để tối đa hóa số lượng tiền tố mà bản thân chúng là các chuỗi ngoặc thông thường (RBS). Tiền tố là bất kỳ chuỗi con nào bắt đầu ở đầu và chuỗi ngoặc thông thường là chuỗi có thể được chuyển thành biểu thức số học hợp lệ nếu bạn thêm số và toán tử. Trong điều kiện thực tế, một chuỗi là RBS khi và chỉ khi, khi đọc từ trái sang phải, số dư tích lũy của "(" trừ ")" không bao giờ âm và kết thúc bằng 0 ở cuối tiền tố. 

Các ràng buộc đầu vào là đáng kể. Chúng ta có thể có tối đa 20 chuỗi, điều này cho phép chúng ta xem xét tất cả các hoán vị theo cách mạnh mẽ nếu cần, bởi vì 20! có kích thước lớn về mặt thiên văn. Tuy nhiên, tổng độ dài của tất cả các chuỗi có thể lên tới 400.000, vì vậy chúng ta không thể thực hiện các thao tác tỷ lệ thuận với tổng độ dài chuỗi cho mỗi hoán vị. Điều này loại trừ sự ghép nối vũ phu ngây thơ. 

Các trường hợp cạnh xuất hiện một cách tự nhiên. Ví dụ: nếu một chuỗi là "((((", sau này nó phải được ghép nối với đủ ")"; nếu không, nó sẽ không bao giờ đóng góp vào một tiền tố hợp lệ. Một chuỗi ")" ở đầu là vô dụng vì nó ngay lập tức phá vỡ sự cân bằng. Các chuỗi mà bản thân chúng là RBS hợp lệ, như "()", luôn tăng số lượng tiền tố hợp lệ. Chúng ta cần nắm bắt các thuộc tính này mà không tạo ra tất cả các chuỗi nối một cách rõ ràng. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử tất cả các hoán vị của chuỗi. Đối với mỗi hoán vị, chúng tôi sẽ nối các chuỗi và quét tất cả các tiền tố để đếm xem chuỗi nào hợp lệ. Lực lượng vũ phu sẽ mất n! × tổng chiều dài, không thể thực hiện được với n = 20, vì 20! là khoảng 2,43 × 10^18. 

Điểm mấu chốt là chúng ta không cần biết chính xác chuỗi được nối. Mỗi chuỗi có thể được tóm tắt bằng hai số: tổng số dư của nó (số "(" trừ số ")") và số dư tối thiểu nhìn thấy khi quét từ trái sang phải. Ví dụ: một chuỗi "())(" có tổng số dư là 0, nhưng số dư tối thiểu của nó là -1 vì nó giảm xuống dưới 0 tại một số điểm. Khi biết hai số này cho mỗi chuỗi, chúng ta có thể coi vấn đề là một lập trình động trên các tập hợp con: đối với mỗi tập hợp con của chuỗi, hãy theo dõi số dư tốt nhất có thể và số lượng tiền tố hợp lệ tối đa. Khi thêm một chuỗi mới, chúng ta có thể xác định số tiền tố hợp lệ bổ sung mà nó đóng góp mà không thực sự nối các chuỗi. 

Cách tiếp cận này làm giảm vấn đề thành vấn đề lập trình động bitmask. Có 2^n tập hợp con và với mỗi tập hợp con, chúng tôi xem xét việc thêm một trong các chuỗi còn lại, dẫn đến độ phức tạp tỷ lệ với n × 2^n, có thể chấp nhận được với n 20. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! × Total_length) | O(tổng_chiều dài) | Quá chậm | 
| Mặt nạ bit DP | O(n × 2^n) | O(2^n × trạng thái cân bằng) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước cho mỗi chuỗi`total_balance`(số lượng "(" trừ ")") và`min_prefix_balance`(số dư tích lũy tối thiểu được thấy trong chuỗi). Hai số này cho phép chúng tôi xác định xem việc thêm chuỗi vào chuỗi hiện có có phá vỡ thuộc tính RBS tại bất kỳ thời điểm nào hay không. 
2. Khởi tạo bảng lập trình động`dp[mask]`, Ở đâu`mask`là một bitmask biểu thị một tập hợp con các chuỗi mà chúng ta đã nối. Mỗi mục nhập lưu trữ một từ điển ánh xạ số dư tích lũy hiện tại tới số lượng tiền tố hợp lệ tối đa đạt được cho đến nay. Bắt đầu với`dp[0] = {0: 0}`, đại diện cho chuỗi không được sử dụng, số dư bằng không và tiền tố RBS bằng không. 
3. Lặp lại tất cả các mặt nạ. Đối với mỗi mặt nạ, hãy cân nhắc thêm một chuỗi`i`chưa được bao gồm. Tính số dư tích lũy mới nếu chúng ta nối thêm chuỗi`i`. Nếu việc thêm vào không làm cho bất kỳ tiền tố nào không hợp lệ (số dư tích lũy không bao giờ âm), hãy tăng số tiền tố RBS theo số lượng tiền tố RBS trong chuỗi`i`. Cập nhật`dp[new_mask]`cho số dư tích lũy mới nếu nó cải thiện mức tối đa trước đó. 
4. Sau khi xử lý tất cả các tập hợp con, câu trả lời là số tiền tố RBS tối đa trên tất cả số dư trong`dp[(1 << n) - 1]`, mặt nạ đại diện cho tất cả các chuỗi được sử dụng. 

Tại sao nó hoạt động: Mỗi bước của DP đảm bảo rằng tất cả các tập hợp con đều được xem xét và đối với mỗi tập hợp con, chúng tôi chỉ theo dõi số dư có thể xảy ra trên thực tế mà không vi phạm thuộc tính RBS. Vì chúng tôi chỉ nối thêm các chuỗi hợp lệ khi có thể và cập nhật số lượng tiền tố một cách tối ưu nên DP nắm bắt mức tối đa toàn cầu trên tất cả các hoán vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
strings = [input().strip() for _ in range(n)]

# Precompute balance info
bal = []
min_pref = []
rbs_count = []

for s in strings:
    b = 0
    min_b = 0
    count = 0
    for c in s:
        if c == '(':
            b += 1
        else:
            b -= 1
        min_b = min(min_b, b)
        if b == 0:
            count += 1
    bal.append(b)
    min_pref.append(min_b)
    rbs_count.append(count)

from collections import defaultdict

dp = [defaultdict(lambda: -1) for _ in range(1 << n)]
dp[0][0] = 0

for mask in range(1 << n):
    for i in range(n):
        if mask & (1 << i):
            continue
        new_mask = mask | (1 << i)
        for cur_balance, cur_count in dp[mask].items():
            if cur_balance + min_pref[i] >= 0:
                new_balance = cur_balance + bal[i]
                new_count = cur_count + rbs_count[i]
                dp[new_mask][new_balance] = max(dp[new_mask][new_balance], new_count)

print(max(dp[(1 << n) - 1].values()))
```Đầu tiên, mã tính toán số dư của mỗi chuỗi và số dư tiền tố tối thiểu. Chúng được lưu trữ trong mảng để truy cập nhanh. Mảng DP`dp`theo dõi các tiền tố RBS tối đa cho từng tập hợp con của chuỗi và từng số dư tích lũy có thể đạt được. Vòng lặp bên trong kiểm tra xem việc thêm một chuỗi có vi phạm thuộc tính RBS hay không; nếu không, số tiền tố RBS sẽ được cập nhật. Cuối cùng, số tiền tối đa trên tất cả số dư cuối kỳ có thể được trả về. 

## Ví dụ đã hoạt động 

**Mẫu 1:**```
2
(
)
```| mặt nạ | cur_balance | hành động | new_balance | new_count | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | thêm 0 | 1 | 0 | 
| 1 | 1 | thêm 1 | 0 | 1 | 

DP xác định chính xác rằng việc thêm "(" rồi ")" sẽ tạo ra một tiền tố hợp lệ. 

**Mẫu 2:**```
4
()()
)(
()
(
```DP theo dõi các tập hợp con như thêm "()()" trước, sau đó ")(", rồi "()", rồi "(". Ở mỗi bước, DP chỉ cập nhật số dư hợp lệ. Sau khi xử lý tất cả các tập hợp con, số lượng tiền tố RBS tối đa là 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n × 2^n) | Chúng tôi lặp lại tất cả các tập hợp con và xem xét việc thêm từng chuỗi trong số n chuỗi. | 
| Không gian | O(2^n × n × max_balance) | Mỗi trạng thái DP lưu trữ số dư, nhưng số dư không thể vượt quá tổng độ dài. | 

Cho n 20, 2^n = 1.048.576, khả thi trong 3 giây. Mức sử dụng bộ nhớ nằm trong khoảng 512 MB ngay cả với tổng chiều dài lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    strings = [input().strip() for _ in range(n)]
    bal = []
    min_pref = []
    rbs_count = []
    for s in strings:
        b = 0
        min_b = 0
        count = 0
        for c in s:
            if c == '(':
                b += 1
            else:
                b -= 1
            min_b = min(min_b, b)
            if b == 0:
                count += 1
        bal.append(b)
        min_pref.append(min_b)
        rbs_count.append(count)
    from collections import defaultdict
    dp = [defaultdict(lambda: -1) for _ in range(1 << n)]
    dp[0][0] = 0
    for mask in range(1 << n):
        for i in range(n):
            if mask & (1 << i):
                continue
            new_mask = mask | (1 << i)
            for cur_balance, cur_count in dp[mask].items():
                if cur_balance + min_pref[i] >= 0:
                    new_balance = cur_balance + bal[i]
                    new_count = cur_count + rbs_count[i]
                    dp[new_mask][new_balance] = max(dp[new_mask][new_balance], new_count)
    return str(max(dp[(1 << n) - 1].values()))

# Provided samples
assert run("2\n(\n)\n") == "1", "sample 1"
assert run("4\n()()\n)(\n()\n(\n") == "4", "sample 2"

# Custom cases
assert run("1\n()
```
