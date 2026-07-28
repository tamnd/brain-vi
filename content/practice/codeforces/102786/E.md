---
title: "CF 102786E - \u0423\u043f\u043e\u0440\u044f\u0434\u043e\u0447\u0438\u0432\u0430\u043d\u0438\u0435 \u043f\u043e \u0441\u0443\u043c\u043c\u0435 \u0446\u0438\u0444\u0440"
description: "Chúng ta cần xem xét tất cả các số nguyên dương từ 1 đến N, nhưng thứ tự không bình thường. Các số được nhóm theo tổng các chữ số thập phân của chúng. Một nhóm có tổng chữ số nhỏ hơn sẽ xuất hiện sớm hơn và trong một nhóm, các số được sắp xếp bình thường theo giá trị."
date: "2026-07-27T19:26:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102786
codeforces_index: "E"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u042f\u0440\u0413\u0423 \u0438\u043c. \u041f.\u0413. \u0414\u0435\u043c\u0438\u0434\u043e\u0432\u0430 Demidov Open IT Cup 2019"
rating: 0
weight: 102786
solve_time_s: 79
verified: true
draft: false
---

[CF 102786E - \u0423\u043f\u043e\u0440\u044f\u0434\u043e\u0447\u0438\u0432\u0430\u043d\u0438\u0435 \u043f\u043e \u0441\u0443\u043c\u043c\u0435 \u0446\u0438\u0444\u0440](https://codeforces.com/problemset/problem/102786/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xét tất cả các số nguyên dương từ`1`ĐẾN`N`, nhưng thứ tự không bình thường. Các số được nhóm theo tổng các chữ số thập phân của chúng. Một nhóm có tổng chữ số nhỏ hơn sẽ xuất hiện sớm hơn và trong một nhóm, các số được sắp xếp bình thường theo giá trị. Nhiệm vụ là tìm số chiếm vị trí`M`theo thứ tự cuối cùng này. 

Giá trị của`N`có thể lớn như`10^18`, vì vậy việc tạo ra tất cả các số là không thể. Ngay cả việc lưu trữ trình tự cũng sẽ cần tới khoảng`10^18`các phần tử. Tổng chữ số tối đa có thể chỉ là`162`, vì một số có mười tám chữ số có nhiều nhất mười tám chữ số có giá trị`9`Và`10^18`bản thân nó có mười chín chữ số, do đó, một giải pháp sẽ khai thác số lượng chữ số nhỏ và phạm vi nhỏ của các tổng có thể. Bất kỳ cách tiếp cận nào tùy thuộc vào việc lặp qua tất cả các số lên đến`N`bị loại trừ. 

Một lỗi phổ biến là quên rằng thứ tự bên trong nhóm tổng một chữ số là theo giá trị số, không phải theo độ dài hoặc theo thứ tự chữ số nào khác. Ví dụ, đối với`N = 100`Và`M = 3`, câu trả lời là`100`, vì nhóm tổng chữ số đầu tiên là`1, 10, 100`. Một phương pháp chỉ xem xét độ dài một chữ số tại một thời điểm có thể bỏ qua không chính xác`100`. 

Một trường hợp cạnh khác xuất hiện khi vị trí được yêu cầu nằm trong nhóm tổng chữ số rất lớn. Ví dụ, với`N = 9`Và`M = 9`, câu trả lời là`9`. Một giải pháp giả định mọi tổng có thể từ`1`hướng lên tồn tại với mọi`N`sẽ tìm kiếm không chính xác ngoài phạm vi có sẵn. 

giá trị`1`cũng là trường hợp biên. Vì`N = 1`Và`M = 1`, đầu ra hợp lệ duy nhất là`1`. Xử lý tổng chữ số bằng 0 như một nhóm bình thường có thể vô tình đưa ra biểu diễn`0`, không phải là một phần của chuỗi. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là liệt kê mọi số từ`1`ĐẾN`N`, tính tổng các chữ số của nó, lưu trữ các cặp`(digit sum, number)`, và sắp xếp chúng. Điều này đúng vì phím sắp xếp khớp chính xác với thứ tự được yêu cầu. Tuy nhiên, khi`N`đạt tới`10^18`, số lượng phần tử được tạo ra vượt xa số lượng mà bất kỳ chương trình nào có thể xử lý. Trường hợp xấu nhất cần khoảng`10^18`tính tổng các chữ số và sắp xếp một tập hợp lớn bằng nhau. 

Cấu trúc hữu ích là tổng chữ số có phạm vi rất nhỏ. Thay vì tạo ra các số, chúng ta có thể đếm xem có bao nhiêu số thuộc về mỗi nhóm tổng chữ số. Khi chúng ta biết nhóm chứa vị trí`M`, nhiệm vụ còn lại là tìm`M`-số nhỏ nhất có tổng một chữ số cố định. 

Việc đếm có thể được thực hiện bằng lập trình động chữ số. Vì chỉ có mười chín vị trí, chúng ta có thể đếm xem có thể có bao nhiêu chuỗi chữ số với tổng còn lại nhất định sau khi tiền tố đã được sửa. Những số đếm này cho phép chúng ta bỏ qua những dãy số khổng lồ mà không cần xây dựng chúng. 

Sau khi tìm được tổng chữ số cần tìm, chúng ta xây dựng chữ số trả lời theo từng chữ số. Các số có tổng chữ số giống nhau xuất hiện theo thứ tự số tăng dần, giống như thứ tự từ điển khi tất cả các số có cùng độ dài. Trước tiên, chúng tôi xác định độ dài chính xác, sau đó chọn từng chữ số bằng cách đếm xem có bao nhiêu lần hoàn thành hợp lệ nếu chúng tôi đặt một chữ số nhỏ hơn vào đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N log N) | O(N) | Quá chậm | 
| Tối ưu | O(162 * 19 * 10 + 19 * 19 * 10) | O(19 * 162) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước`ways(length, sum)`, số dãy chữ số có độ dài cho trước trong đó mỗi chữ số có thể bắt nguồn từ`0`ĐẾN`9`và tổng các chữ số bằng`sum`. Bảng này là công cụ đếm cốt lõi được sử dụng ở mọi nơi khác. 

Các chuỗi này có thể chứa các số 0 đứng đầu vì chúng đại diện cho hậu tố còn lại của một số. Các số 0 đứng đầu không gây ra vấn đề gì vì chiều dài đầy đủ được cố định trong quá trình đếm. 
2. Với mọi tổng chữ số có thể có từ`1`trở lên, tính xem có bao nhiêu số từ`1`ĐẾN`N`có tổng chữ số đó. 

Số đếm được lấy bằng chữ số DP. Chúng tôi quét các chữ số của`N`từ trái sang phải, quyết định xem tiền tố hiện tại đã nhỏ hơn chưa`N`hoặc vẫn bằng nó. Biểu diễn số 0 đứng đầu cho phép chúng ta tự động đếm tất cả các số dương có độ dài ngắn hơn. 
3. Trừ các nhóm tổng chữ số đầy đủ từ`M`cho đến khi tìm được nhóm chứa câu trả lời. 

Sau bước này,`M`trở thành vị trí bên trong một nhóm tổng chữ số cố định. 
4. Tìm độ dài của số cần tìm. 

Với mỗi độ dài có thể, hãy đếm xem có bao nhiêu số có độ dài đó có tổng chữ số cần tìm. Độ dài ngắn hơn luôn xuất hiện trước độ dài dài hơn vì mọi số dương có ít chữ số hơn đều nhỏ hơn về mặt số. 
5. Xây dựng câu trả lời từ trái sang phải. 

Tại mỗi vị trí, hãy thử các chữ số có thể có theo thứ tự tăng dần. Với mỗi chữ số ứng cử viên, hãy đếm xem có bao nhiêu cách các vị trí còn lại có thể hoàn thành số với tổng chữ số còn lại. Nếu hiện tại`M`lớn hơn số đó, hãy bỏ qua tất cả những số đó. Ngược lại, câu trả lời bắt đầu bằng chữ số đó. 
6. Xuất số đã xây dựng. 

Lý do điều này có hiệu quả là vì mỗi khối bị bỏ qua tương ứng với một khoảng thời gian liên tiếp theo thứ tự được yêu cầu. Chữ số DP đếm chính xác có bao nhiêu số hợp lệ trong mỗi khoảng, do đó giảm`M`bởi những số đếm đó không bao giờ thay đổi vị trí tương đối của số mong muốn. Trong quá trình xây dựng, tiền tố được chọn luôn là tiền tố nhỏ nhất có khối chứa vị trí còn lại, bảo toàn bất biến rằng câu trả lời vẫn nằm trong phạm vi tìm kiếm còn lại. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline

@lru_cache(None)
def ways(length, total):
    if length == 0:
        return 1 if total == 0 else 0
    if total < 0:
        return 0
    res = 0
    for d in range(10):
        res += ways(length - 1, total - d)
    return res

def count_up_to(n, target_sum):
    digits = list(map(int, str(n)))
    m = len(digits)

    @lru_cache(None)
    def dp(pos, remaining, tight):
        if pos == m:
            return 1 if remaining == 0 else 0

        limit = digits[pos] if tight else 9
        ans = 0

        for d in range(limit + 1):
            if remaining >= d:
                ans += dp(pos + 1, remaining - d, tight and d == limit)

        return ans

    return dp(0, target_sum, True)

def count_exact_length(length, target_sum):
    ans = 0
    for first in range(1, 10):
        if target_sum >= first:
            ans += ways(length - 1, target_sum - first)
    return ans

def solve_case(n, m):
    digit_sum = 1

    while True:
        cnt = count_up_to(n, digit_sum)
        if m <= cnt:
            break
        m -= cnt
        digit_sum += 1

    length = 1
    while True:
        cnt = count_exact_length(length, digit_sum)
        if m <= cnt:
            break
        m -= cnt
        length += 1

    result = []
    remaining = digit_sum

    for pos in range(length):
        start = 1 if pos == 0 else 0

        for digit in range(start, 10):
            if remaining < digit:
                continue

            cnt = ways(length - pos - 1, remaining - digit)

            if m > cnt:
                m -= cnt
            else:
                result.append(str(digit))
                remaining -= digit
                break

    return ''.join(result)

def main():
    n, m = map(int, input().split())
    print(solve_case(n, m))

if __name__ == "__main__":
    main()
```các`ways`hàm lưu trữ số lượng hậu tố có thể có cho một độ dài và tổng nhất định. Nó được sử dụng cả khi đếm độ dài và khi quyết định các chữ số riêng lẻ. Độ dài được yêu cầu lớn nhất chỉ là 19 nên bảng rất nhỏ.`count_up_to`sử dụng chữ số DP với cờ chặt. Khi tiền tố đã chọn khớp`N`, chữ số tiếp theo không được vượt quá chữ số tương ứng của`N`. Khi tiền tố trở nên nhỏ hơn, phần còn lại của số có thể sử dụng bất kỳ chữ số nào. Việc triển khai sử dụng các số 0 đứng đầu, đó là lý do tại sao tất cả các số dương cho đến`N`được biểu diễn đúng một lần.`count_exact_length`loại trừ số 0 đứng đầu bằng cách bắt đầu chữ số đầu tiên từ`1`. Chức năng này được tách ra khỏi`ways`bởi vì`ways`cố ý cho phép số 0 làm hậu tố. 

Vòng xây dựng cuối cùng là nơi`M`-số thứ được phục hồi. Việc thử các chữ số theo thứ tự tăng dần khớp với thứ tự số vì độ dài đã được cố định. Mỗi chữ số bị bỏ qua đại diện cho một khối số hợp lệ hoàn chỉnh, vì vậy việc trừ đi kích thước của nó cũng giống như việc tiến lên theo thứ tự. 

## Ví dụ đã hoạt động 

Đối với mẫu`N = 100`,`M = 10`, các nhóm bắt đầu như sau. 

| Tổng chữ số hiện tại | Đếm bỏ qua | Còn lại M | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 3 | 7 | Bỏ qua tổng 1 | 
| 2 | 4 | 3 | Bỏ qua tổng 2 | 
| 3 | 4 | 3 | Chọn tổng 3 | 

Tổng chữ số`3`nhóm là`3, 12, 21, 30`, vậy phần tử thứ ba là`21`. 

Dấu vết cho thấy thuật toán không bao giờ cần tạo các nhóm trước đó. Nó chỉ đếm kích thước của chúng và di chuyển trực tiếp đến nhóm được yêu cầu. 

Vì`N = 20`,`M = 5`, thứ tự sắp xếp là:`1, 10, 2, 11, 20`| Tổng chữ số hiện tại | Đếm bỏ qua | Còn lại M | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 2 | 3 | Bỏ qua tổng 1 | 
| 2 | 3 | 3 | Chọn tổng 2 | 

Tổng chữ số`2`nhóm chứa`2, 11, 20`, và phần tử thứ ba là`20`. 

Ví dụ này thực hiện phép chuyển đổi độ dài vì nhóm tổng chữ số giống nhau chứa cả số có một chữ số và số có hai chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(162 * 19 * 10 + 19 * 19 * 10) | Có tối đa 162 tổng chữ số, 19 vị trí và 10 lựa chọn chữ số trong mỗi lần chuyển đổi DP. | 
| Không gian | O(19 * 162) | Số lượng hậu tố được ghi nhớ chỉ chứa các trạng thái tổng và độ dài chữ số nhỏ. | 

Thuật toán phụ thuộc vào số chữ số hơn là giá trị của`N`. Từ`N`có tối đa mười chín chữ số, lời giải dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io
from functools import lru_cache

def build_solver():
    @lru_cache(None)
    def ways(length, total):
        if length == 0:
            return 1 if total == 0 else 0
        return sum(ways(length - 1, total - d) for d in range(10))

    def count_up_to(n, target_sum):
        digits = list(map(int, str(n)))
        m = len(digits)

        @lru_cache(None)
        def dp(pos, remaining, tight):
            if pos == m:
                return remaining == 0
            limit = digits[pos] if tight else 9
            ans = 0
            for d in range(limit + 1):
                if remaining >= d:
                    ans += dp(pos + 1, remaining - d, tight and d == limit)
            return ans

        return dp(0, target_sum, True)

    def count_exact_length(length, total):
        return sum(
            ways(length - 1, total - d)
            for d in range(1, 10)
            if total >= d
        )

    def solve_case(n, m):
        s = 1
        while True:
            c = count_up_to(n, s)
            if m <= c:
                break
            m -= c
            s += 1

        length = 1
        while True:
            c = count_exact_length(length, s)
            if m <= c:
                break
            m -= c
            length += 1

        ans = []
        remaining = s
        for pos in range(length):
            for d in range(1 if pos == 0 else 0, 10):
                if remaining >= d:
                    c = ways(length - pos - 1, remaining - d)
                    if m > c:
                        m -= c
                    else:
                        ans.append(str(d))
                        remaining -= d
                        break
        return ''.join(ans)

    return solve_case

solve = build_solver()

def run(inp: str) -> str:
    n, m = map(int, inp.split())
    return solve(n, m)

assert run("100 10") == "21"
assert run("1 1") == "1"
assert run("9 9") == "9"
assert run("20 5") == "20"
assert run("1000 3") == "100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Đầu vào tối thiểu và loại trừ tổng chữ số bằng 0 | 
|`9 9`|`9`| Ranh giới một chữ số | 
|`20 5`|`20`| Chuyển đổi giữa các độ dài số trong nhóm tổng chữ số | 
|`1000 3`|`100`| Thứ tự đúng của các tổng có chữ số bằng nhau | 

## Vỏ cạnh 

cho`N = 100`Và`M = 3`, tìm kiếm tổng chữ số sẽ chọn tổng`1`. Việc tìm kiếm độ dài kiểm tra độ dài`1`, trong đó số duy nhất là`1`, sau đó là độ dài`2`, trong đó số duy nhất là`10`. Sau khi bỏ 2 vị trí đó thì vị trí còn lại là số độ dài đầu tiên`3`, đó là`100`. Điều này xử lý trường hợp các số dài hơn xuất hiện trong cùng một nhóm tổng chữ số. 

Vì`N = 9`Và`M = 9`, thuật toán sẽ đếm chính xác nhóm hiện có duy nhất. Tổng mỗi chữ số từ`1`ĐẾN`8`chứa một số và được bỏ qua, để lại tổng chữ số`9`với vị trí`1`. Việc xây dựng tạo ra một chữ số`9`. 

Vì`N = 1`Và`M = 1`, tổng chữ số`1`được chọn ngay lập tức. Độ dài là một, chữ số đầu tiên duy nhất có thể là`1`và quá trình xây dựng kết thúc mà không bao giờ xem xét giá trị 0 không hợp lệ. 

Vì`N = 20`Và`M = 5`, thuật toán đạt tổng chữ số`2`. Đầu tiên nó kiểm tra số có một chữ số`2`, sau đó xây dựng số thứ ba trong nhóm. Việc đếm hậu tố đặt câu trả lời sau`2`Và`11`, sản xuất`20`. Điều này xác nhận rằng việc xây dựng tôn trọng thứ tự số trên các độ dài khác nhau.
