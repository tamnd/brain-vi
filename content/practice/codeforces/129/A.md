---
title: "CF 129A - Bánh quy"
description: "Chúng tôi được phát một số túi bánh quy, trong đó mỗi túi chứa một số bánh quy. Olga muốn ăn trộm chính xác một chiếc túi. Sau khi lấy chiếc túi đó ra, tổng số bánh còn lại phải là số chẵn để hai chị em có thể chia đều."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 129
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 94 (Div. 2 Only)"
rating: 900
weight: 129
solve_time_s: 104
verified: true
draft: false
---

[CF 129A - Cookie](https://codeforces.com/problemset/problem/129/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát một số túi bánh quy, trong đó mỗi túi chứa một số bánh quy. Olga muốn ăn trộm chính xác một chiếc túi. Sau khi lấy chiếc túi đó ra, tổng số bánh còn lại phải là số chẵn để hai chị em có thể chia đều. 

Nhiệm vụ là đếm xem Olga có thể chọn bao nhiêu túi sao cho tổng số còn lại là số chẵn. 

Dữ liệu đầu vào bao gồm số túi và số lượng bánh quy trong mỗi túi. Đầu ra là một số nguyên duy nhất, số lượng túi hợp lệ có thể bị loại bỏ. 

Những hạn chế là rất nhỏ. Có nhiều nhất 100 túi, mỗi túi chứa nhiều nhất 100 bánh quy. Ngay cả một giải pháp bậc hai cũng sẽ chạy ngay lập tức, vì tệ nhất là chúng ta sẽ chỉ thực hiện khoảng 10.000 phép tính. Điều đó có nghĩa là chúng ta có thể tập trung hoàn toàn vào sự rõ ràng và chính xác thay vì các thủ thuật tối ưu hóa. 

Phần khó khăn là hiểu cách tính chẵn lẻ hoạt động sau khi loại bỏ một túi. Việc thực hiện bất cẩn có thể tính toán lại số tiền không chính xác hoặc hiểu sai khi tổng số chẵn vẫn là số chẵn. 

Hãy xem xét ví dụ này:```
1
1
```Tổng số bánh quy là 1. Nếu Olga loại bỏ túi duy nhất thì tổng số còn lại sẽ là 0, tức là số chẵn. Câu trả lời đúng là:```
1
```Một giải pháp có lỗi có thể quên rằng số 0 là số chẵn và in không chính xác số 0. 

Một nơi dễ mắc lỗi khác là logic chẵn lẻ. Coi như:```
3
1 2 3
```Tổng số là 6, là số chẵn. Loại bỏ túi có số chẵn sẽ giữ số tiền còn lại là số chẵn, trong khi loại bỏ túi lẻ sẽ làm cho số tiền còn lại là số lẻ. Ở đây chỉ có túi chứa 2 tác phẩm nên đáp án là:```
1
```Nếu ai đó chỉ đếm các túi lẻ hoặc chỉ kiểm tra tổng số chẵn lẻ ban đầu mà không suy luận cẩn thận, họ có thể nhận được kết quả sai. 

Trường hợp cạnh cuối cùng là khi mỗi túi có cùng số chẵn lẻ. Ví dụ:```
4
2 2 2 2
```Tổng số tiền là 8. Bỏ túi bất kỳ ra còn lại 6, là số chẵn. Câu trả lời là 4. Việc triển khai vô tình kiểm tra tính chẵn lẻ của túi bị loại bỏ thay vì số tiền còn lại có thể âm thầm thất bại ở đây. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Đầu tiên chúng tôi tính toán tổng số cookie. Sau đó, với mỗi túi, chúng ta tưởng tượng lấy nó ra và kiểm tra xem tổng số còn lại có chẵn không. 

Đối với túi`i`, tổng số còn lại là:```
total_sum - a[i]
```Nếu giá trị đó chia hết cho 2 thì túi này là lựa chọn hợp lệ. 

Cách tiếp cận này đã đủ nhanh. Với tối đa 100 túi, chúng tôi thực hiện tối đa 100 lần kiểm tra sau khi tính tổng một lần. 

Ngoài ra còn có một quan sát tính chẵn lẻ hữu ích đằng sau vấn đề này. Tính chẵn lẻ của tổng số còn lại chỉ phụ thuộc vào tính chẵn lẻ của tổng số tiền và túi bị loại bỏ. 

Nếu tổng số tiền là số chẵn thì việc loại bỏ túi chẵn sẽ giữ kết quả là số chẵn, trong khi loại bỏ túi lẻ sẽ làm cho kết quả là số lẻ. 

Nếu tổng số tiền là số lẻ thì việc loại bỏ túi lẻ sẽ làm cho tổng số còn lại là số chẵn. 

Vì vậy, bài toán cũng có thể được xem như là đếm các túi có số chẵn lẻ trùng với số chẵn lẻ của tổng số tiền. 

Phiên bản brute-force hoạt động vì mọi túi ứng viên đều có thể được kiểm tra độc lập. Quan sát tính chẵn lẻ giải thích tại sao việc kiểm tra đó lại đơn giản đến vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng túi và dãy số lượng bánh quy. 
2. Tính tổng số bánh quy trên tất cả các túi. 
3. Khởi tạo bộ đếm câu trả lời về 0. 
4. Lặp lại từng túi. 
5. Đối với túi hiện tại, tính tổng còn lại sau khi tháo túi:```
remaining = total_sum - current_bag
```6. Kiểm tra xem`remaining`là chẵn. 

Nếu như`remaining % 2 == 0`, tăng câu trả lời. 
7. Sau khi xử lý tất cả các túi, hãy in câu trả lời. 

### Tại sao nó hoạt động 

Đối với mỗi túi, thuật toán sẽ tính toán trực tiếp số lượng bánh quy còn lại sau khi loại bỏ túi đó. Một túi được tính chính xác khi tổng số còn lại là số chẵn, phù hợp chính xác với yêu cầu của bài toán. 

Vì mỗi túi có thể được kiểm tra một lần và kiểm tra tính chẵn lẻ là chính xác về mặt toán học nên thuật toán không thể bỏ sót câu trả lời hợp lệ hoặc đếm câu trả lời không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

total = sum(a)

answer = 0

for x in a:
    if (total - x) % 2 == 0:
        answer += 1

print(answer)
```Giải pháp bắt đầu bằng cách đọc số lượng túi và số lượng bánh quy. Tổng số tiền được tính một lần ngay từ đầu nên chúng tôi không tính toán lại nhiều lần cho mỗi túi. 

Vòng lặp kiểm tra từng túi một cách độc lập. Đối với một túi chứa`x`cookie, loại bỏ nó lá`total - x`bánh quy phía sau. Nếu số tiền còn lại này chia hết cho 2 thì túi sẽ đóng góp một lựa chọn hợp lệ. 

Việc triển khai vẫn đơn giản vì các ràng buộc rất nhỏ. Không có mối lo ngại về tràn trong Python và không cần xử lý đặc biệt cho số 0 vì`0 % 2 == 0`một cách tự nhiên hoạt động. 

Một điểm tinh tế là thứ tự thực hiện các thao tác. Chúng ta phải trừ túi hiện tại trước khi kiểm tra tính chẵn lẻ. Việc chỉ kiểm tra tính chẵn lẻ của túi không phải lúc nào cũng đúng trừ khi chúng ta lý luận riêng về tính chẵn lẻ của tổng số tiền. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1
```Tổng số tiền là 1. 

| Túi hiện tại | Tổng số còn lại | Thậm chí? | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 0 | Có | 1 | 

Câu trả lời cuối cùng là:```
1
```Ví dụ này xác nhận rằng việc loại bỏ túi duy nhất là hợp lệ vì số 0 là số chẵn. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
```Tổng số tiền là 10. 

| Túi hiện tại | Tổng số còn lại | Thậm chí? | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 9 | Không | 0 | 
| 2 | 8 | Có | 1 | 
| 3 | 7 | Không | 1 | 
| 4 | 6 | Có | 2 | 

Câu trả lời cuối cùng là:```
2
```Dấu vết này cho thấy hành vi chẵn lẻ một cách rõ ràng. Vì tổng số tiền là số chẵn nên việc loại bỏ các túi chẵn sẽ giữ cho tổng số còn lại là số chẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi tính tổng một lần và quét mảng một lần | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với tối đa 100 túi, thuật toán dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. Ngay cả những cách tiếp cận chậm hơn nhiều cũng có thể vượt qua, nhưng quét tuyến tính vừa đơn giản vừa tối ưu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    total = sum(a)

    ans = 0

    for x in a:
        if (total - x) % 2 == 0:
            ans += 1

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run("1\n1\n") == "1", "sample 1"

# minimum size, even remaining becomes zero
assert run("1\n7\n") == "1", "single bag"

# all even values
assert run("4\n2 2 2 2\n") == "4", "all bags valid"

# mixed parity
assert run("4\n1 2 3 4\n") == "2", "only even bags work"

# no valid choices
assert run("2\n2 2\n") == "0", "removing any bag leaves odd total"

# larger case
assert run("5\n2 2 2 2 99\n") == "1", "only odd bag works"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`1`| Không còn cookie nào được coi là chẵn | 
|`4 / 2 2 2 2`|`4`| Mỗi túi có thể được gỡ bỏ | 
|`4 / 1 2 3 4`|`2`| Xử lý chẵn lẻ hỗn hợp | 
|`2 / 2 2`|`0`| Không có lượt xóa hợp lệ | 
|`5 / 2 2 2 2 99`|`1`| Chỉ có một túi thỏa mãn điều kiện | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
1
```Tổng số tiền là 1. Loại bỏ các lá túi duy nhất:```
1 - 1 = 0
```Vì 0 là số chẵn nên thuật toán sẽ tăng câu trả lời và in ra 1. Điều này xác nhận rằng các cookie trống còn lại được xử lý chính xác. 

Bây giờ hãy xét trường hợp mọi túi đều chẵn:```
4
2 2 2 2
```Tổng số tiền là 8. Bỏ túi bất kỳ ra còn lại 6, là số chẵn. 

Thuật toán kiểm tra tất cả bốn túi:```
8 - 2 = 6
```mọi lúc, vì vậy câu trả lời trở thành 4. 

Cuối cùng, hãy xem xét trường hợp chỉ có một túi hoạt động:```
5
2 2 2 2 99
```Tổng số tiền là 107, là số lẻ. 

Bỏ túi chẵn nào ra 105, vẫn lẻ. 

Tháo túi 99 lá:```
107 - 99 = 8
```đó là số chẵn. 

Thuật toán đếm chính xác một lựa chọn hợp lệ.
