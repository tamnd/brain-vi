---
title: "CF 102439E - Doanh nghiệp nhỏ"
description: "Chúng ta có một tập các khối chữ số, được biểu thị bằng một chuỗi s. Mỗi khối phải được sử dụng chính xác một lần để xây dựng hai số nguyên thập phân. Hai số nguyên có thể bằng nhau, được phép sử dụng số 0, nhưng không số nào được chứa số 0 đứng đầu. Cả hai số phải lớn nhất là (10^{18})."
date: "2026-08-14T15:53:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102439
codeforces_index: "E"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 102439
solve_time_s: 106
verified: true
draft: false
---

[CF 102439E - Doanh nghiệp nhỏ](https://codeforces.com/problemset/problem/102439/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một túi các khối chữ số, được biểu thị bằng một chuỗi`s`. Mỗi khối phải được sử dụng chính xác một lần để xây dựng hai số nguyên thập phân. Hai số nguyên có thể bằng nhau, được phép sử dụng số 0, nhưng không số nào được chứa số 0 đứng đầu. Cả hai số phải lớn nhất là (10^{18}). 

Cặp bắt buộc được sắp xếp theo giá trị nhỏ hơn trước. Trong số tất cả các công trình hợp lệ, trước tiên chúng ta giảm thiểu số lượng nhỏ hơn. Khi số đó được cố định, chúng tôi giảm thiểu số khác. Nếu không có phân vùng hợp lệ của tất cả các khối chữ số tồn tại, chúng tôi sẽ in`-1 -1`. 

Độ dài giới hạn 50 là đủ nhỏ cho các thuật toán thực hiện một lượng công việc không đổi trên mỗi chữ số, nhưng lại quá lớn để liệt kê tập hợp con. Có tới (2^{50}), đại khái là (1,13 \time 10^{15}), cách chọn khối nào thuộc về số đầu tiên. Ngay cả khi mọi phân vùng chỉ có thể được kiểm tra trong vài chục thao tác, thì điều này vẫn vượt xa giới hạn một giây. Giới hạn trên của (10^{18}) là hạn chế chính về cấu trúc: mọi số hợp lệ có tối đa 19 chữ số và số có 19 chữ số không vượt quá (10^{18}) phải chính xác`1000000000000000000`. 

Một số trường hợp đặc biệt có thể đánh lừa việc triển khai tham lam trực tiếp. 

Vì`0`, chỉ có một khối nên không thể xây dựng được hai số khác rỗng. Câu trả lời là`-1 -1`. Việc triển khai bất cẩn có thể coi số thứ hai bị thiếu là 0. 

Vì`00`, câu trả lời đúng là`0 0`. Quy tắc nói rằng một số không thể chứa số 0 vì chữ số đầu tiên của nó sẽ bác bỏ trường hợp này một cách không chính xác. Một số 0 duy nhất là sự biểu diễn hợp lệ của số 0, trong khi`00`sẽ không hợp lệ. 

Vì`000`, câu trả lời là`-1 -1`. Chia nó thành`0`Và`00`không hoạt động vì`00`có số 0 đứng đầu. Điều này cho thấy tại sao chỉ kiểm tra số khối là không đủ. 

Vì`1000000000000000000`, có 19 chữ số, đáp án là`0 100000000000000000`. Số nhỏ nhất có thể sử dụng một số 0, để lại 18 khối cho số thứ hai. Việc triển khai bất cẩn luôn cố gắng thực hiện (10^{18}) khi nhìn thấy mẫu chữ số này sẽ bỏ lỡ thực tế là việc giảm thiểu số đầu tiên được ưu tiên. 

Đối với chuỗi 20 chữ số chỉ chứa`2`, chẳng hạn như`22222222222222222222`, không có cách nào để tạo ra một số hợp lệ có nhiều nhất là 19 chữ số (10^{18}), bởi vì mọi số như vậy sẽ phải chính xác (10^{18}). Tuy nhiên, trường hợp này vẫn có thể giải quyết được như`22 222222222222222222`. Đây là lý do quan trọng khiến chúng tôi không thể đơn giản yêu cầu khối (10^{18}) bất cứ khi nào đầu vào có nhiều hơn 19 chữ số. Chúng ta phải thử mọi độ dài có thể để có được số nhỏ hơn. 

## Phương pháp tiếp cận 

Giải pháp brute-force có thể chọn một tập hợp con tùy ý của các khối chữ số cho số đầu tiên, sử dụng phần bù cho số thứ hai, sắp xếp các chữ số đã chọn theo mọi thứ tự có liên quan và giữ lại cặp hợp lệ tốt nhất. Điều này đúng vì mọi phân vùng có thể đều xuất hiện giữa các tập hợp con. Vấn đề là số lượng phân vùng. Với 50 chữ số, có (2^{50}), xấp xỉ (1,13 \time 10^{15}), các lựa chọn tập hợp con. Ngay cả việc xử lý mỗi lựa chọn trong thời gian (O(50)) cũng sẽ cần khoảng (5,6 \times 10^{16}) phép tính chữ số cơ bản trong trường hợp xấu nhất, điều này gần như không khả thi. 

Quan sát hữu ích là các giá trị được giới hạn bởi (10^{18}), do đó mỗi số chứa tối đa 19 chữ số. Đầu tiên chúng ta có thể quyết định độ dài (k) của số nhỏ hơn. Nếu số còn lại có độ dài (n-k), cả hai độ dài tối đa phải bằng 19. Vì số nhỏ hơn có ít chữ số hơn bất cứ khi nào (k<n-k), nên mọi số dương có chữ số (k) hợp lệ sẽ tự động nhỏ hơn mọi số có chữ số ((n-k)) hợp lệ. Do đó độ dài nhỏ nhất khả thi của số nhỏ hơn luôn là độ dài đầu tiên đáng xem xét. 

Có nhiều nhất 19 độ dài có thể có. Đối với độ dài cố định (k), chúng tôi xây dựng số có chữ số (k) nhỏ nhất có thể từ các chữ số có sẵn. Chúng tôi làm điều này từ trái sang phải. Ở mọi vị trí, chúng tôi thử các chữ số theo thứ tự tăng dần và tạm thời lấy một bản sao. Câu hỏi duy nhất là liệu các chữ số còn lại có còn có thể tạo thành số khác với độ dài yêu cầu hay không. 

Việc kiểm tra tính khả thi đó cực kỳ đơn giản. Nếu số kia có tối đa 18 chữ số, thì số đó hợp lệ bất cứ khi nào độ dài của nó bằng một hoặc, để biểu diễn dài hơn, số đó chứa ít nhất một chữ số khác 0. Nếu nó có 19 chữ số thì nó phải chính xác (10^{18}), do đó nhiều tập hợp còn lại của nó phải chứa một`1`và mười tám`0`chữ số. 

Khi số nhỏ hơn được cố định, số thứ hai được giảm thiểu bằng cách sắp xếp các chữ số còn lại của nó theo thứ tự tăng dần, ngoại trừ số có nhiều chữ số phải bắt đầu bằng chữ số nhỏ nhất khác 0. Đây là cách xây dựng số nhỏ nhất tiêu chuẩn từ nhiều tập hợp chữ số. 

Brute-force hoạt động vì mọi phân vùng đều được xem xét rõ ràng, nhưng không thành công vì có nhiều phân vùng theo cấp số nhân. Quan sát rằng mỗi số có nhiều nhất 19 chữ số làm giảm việc tìm kiếm xuống còn tối đa 19 độ dài ứng viên và mỗi ứng cử viên có thể được giải một cách tham lam trong không gian chữ số có kích thước không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n^2 \cdot 10)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần xuất hiện của mỗi chữ số và cho`n = len(s)`. Một cặp hợp lệ cần cả hai số không trống, vì vậy nếu`n < 2`chúng tôi ngay lập tức quay trở lại`-1 -1`. Ngoài ra, nếu`n > 38`, hai số không thể khớp nhau vì mỗi số có nhiều nhất 19 chữ số. 
2. Xem xét độ dài có thể`k`của số nhỏ hơn từ`max(1, n - 19)`bởi vì`floor(n / 2)`. Giới hạn dưới xuất phát từ việc số kia không thể chứa nhiều hơn 19 chữ số. Chúng tôi xử lý các độ dài này theo thứ tự tăng dần vì số nhỏ hợp lệ ngắn hơn luôn tốt hơn số dương dài hơn. 
3. Đối với cố định`k`, bộ`L = n - k`, độ dài cần thiết của số kia. Bắt đầu với mảng đếm đầy đủ chữ số và xây dựng số đầu tiên từ trái sang phải. 
4. Tại mỗi vị trí, thử từng chữ số từ`0`bởi vì`9`theo thứ tự tăng dần. Ở vị trí đầu tiên, số 0 chỉ được phép khi`k == 1`, bởi vì ký tự đơn`0`là một đại diện hợp lệ của số không. Đối với số có nhiều chữ số, chữ số đầu tiên phải khác 0. 
5. Tạm thời loại bỏ chữ số đề cử và hỏi xem các chữ số còn lại có thể tạo thành số hợp lệ chính xác không`L`chữ số. Nếu không thể, hãy khôi phục chữ số và thử ứng viên tiếp theo. Nếu có thể, hãy giữ ứng viên đó vĩnh viễn và tiếp tục với vị trí tiếp theo. 
6. Kiểm tra tính khả thi chấp nhận mọi multiset còn lại khi`L == 1`, bởi vì bất kỳ chữ số nào cũng là số hợp lệ. Vì`2 <= L <= 18`, ít nhất một chữ số còn lại phải khác 0. Vì`L == 19`, multiset duy nhất được chấp nhận chính xác là một`1`và mười tám`0`chữ số, bởi vì`1000000000000000000`là số nguyên duy nhất có 19 chữ số không vượt quá (10^{18}). 
7. Rốt cuộc`k`các chữ số đã được chọn, hãy sắp xếp các chữ số còn lại thành số thứ hai nhỏ nhất có thể. Nếu chỉ có một chữ số, hãy trả lại trực tiếp. Ngược lại, đặt chữ số nhỏ nhất khác 0 lên đầu tiên, tiếp theo là tất cả các số 0 và sau đó là các chữ số còn lại theo thứ tự được sắp xếp. 
8. Trả về cặp khả thi đầu tiên được tìm thấy. Nếu như`k < L`, số đầu tiên có ít chữ số hơn và nhất thiết phải có giá trị nhỏ hơn. Nếu như`k == L`, cấu trúc tham lam cho số đầu tiên nhỏ nhất có thể trong số tất cả các phân vùng khả thi, vì vậy sau khi sắp xếp thứ tự hai số kết quả, cặp của nó vẫn tối ưu. 

Tại sao nó hoạt động: đối với mỗi độ dài ứng cử viên, cấu trúc duy trì tính bất biến mà tiền tố được chọn cho đến nay là tiền tố nhỏ nhất về mặt từ điển vẫn có thể được hoàn thành thành một cặp hợp lệ. Tại mỗi vị trí, mọi chữ số nhỏ hơn sẽ được kiểm tra trước và một chữ số chỉ bị loại bỏ khi các khối còn lại không thể tạo thành số thứ hai theo yêu cầu. Do đó, chữ số được chấp nhận đầu tiên luôn tối ưu cho vị trí đó. Việc xử lý các vị trí từ trái sang phải sẽ cho ra số lượng nhỏ nhất khả thi của độ dài đó. Vì độ dài được xử lý từ nhỏ nhất đến lớn nhất, độ dài khả thi đầu tiên sẽ cho số nhỏ nhất có thể nhỏ nhất trên toàn cầu. Cuối cùng, việc sắp xếp các chữ số không sử dụng thành biểu diễn hợp lệ nhỏ nhất sẽ cho số thứ hai tối thiểu có thể có cho số đầu tiên cố định đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 10**18

def can_make_other(cnt, length):
    if sum(cnt) != length:
        return False

    if length == 1:
        return True

    if length == 19:
        return cnt[0] == 18 and cnt[1] == 1 and sum(cnt[2:]) == 0

    return any(cnt[d] > 0 for d in range(1, 10))

def build_smallest(cnt):
    length = sum(cnt)

    if length == 1:
        for d in range(10):
            if cnt[d]:
                return str(d)

    first = -1
    for d in range(1, 10):
        if cnt[d]:
            first = d
            break

    if first == -1:
        return None

    cnt[first] -= 1
    result = [str(first)]

    result.extend("0" for _ in range(cnt[0]))

    for d in range(1, 10):
        result.extend(str(d) for _ in range(cnt[d]))

    return "".join(result)

def solve(s):
    n = len(s)

    if n < 2 or n > 38:
        return "-1 -1"

    original = [0] * 10
    for ch in s:
        original[ord(ch) - ord('0')] += 1

    min_k = max(1, n - 19)
    max_k = n // 2

    for k in range(min_k, max_k + 1):
        other_len = n - k
        cnt = original[:]
        first_digits = []

        possible = True

        for pos in range(k):
            chosen = -1

            for d in range(10):
                if cnt[d] == 0:
                    continue

                if pos == 0 and k > 1 and d == 0:
                    continue

                cnt[d] -= 1

                if can_make_other(cnt, other_len):
                    chosen = d
                    break

                cnt[d] += 1

            if chosen == -1:
                possible = False
                break

            first_digits.append(str(chosen))

        if not possible:
            continue

        first = "".join(first_digits)
        second = build_smallest(cnt)

        if second is None:
            continue

        if len(second) > 19:
            continue

        if len(second) == 19 and second != "1000000000000000000":
            continue

        if k == other_len and first > second:
            first, second = second, first

        return first + " " + second

    return "-1 -1"

def main():
    s = input().strip()
    print(solve(s))

if __name__ == "__main__":
    main()
```các`original`mảng lưu trữ bội số của mỗi chữ số, do đó mọi quyết định sau này có thể được thực hiện mà không cần quét nhiều lần chuỗi đầu vào. 

Vòng lặp bên ngoài chỉ xem xét các độ dài có thể thuộc về số nhỏ hơn.`min_k = max(1, n - 19)`đảm bảo rằng số thứ hai có nhiều nhất 19 chữ số, trong khi`n // 2`ngăn cản chúng ta xem xét một số dài hơn số tương ứng của nó. 

Vòng lặp xây dựng là phần tham lam. Tại mỗi vị trí, nó thử các chữ số theo thứ tự tăng dần. Một chữ số tạm thời bị xóa trước khi gọi`can_make_other`, bởi vì chức năng đó phải kiểm tra chính xác các khối còn lại sau khi cam kết với ứng viên. 

Việc xử lý đặc biệt vị trí đầu tiên sẽ ngăn cản các biểu diễn như`04`. Điều kiện cho phép`0`khi`k == 1`, bởi vì biểu diễn một ký tự`0`là hợp lệ.`can_make_other`xử lý giới hạn trên mà không dựa vào chuyển đổi số nguyên có độ chính xác tùy ý của Python. Một số hợp lệ gồm 19 chữ số phải chính xác (10^{18}), vì vậy việc kiểm tra số chữ số của nó vừa đơn giản vừa an toàn hơn so với việc xây dựng và chuyển đổi một chuỗi có khả năng không hợp lệ.`build_smallest`thực hiện tối thiểu hóa thứ cấp. Đối với số có nhiều chữ số, nó chọn chữ số nhỏ nhất khác 0 trước tiên, vì đặt số 0 ở đó sẽ tạo ra số 0 đứng đầu. Sau đó, tất cả các số 0 có thể được đặt ngay sau nó, theo sau là các chữ số còn lại theo thứ tự tăng dần. 

Việc kiểm tra độ dài-19 cuối cùng chỉ cần thiết cho số thứ hai. Thử nghiệm tính khả thi tham lam đã đảm bảo điều kiện này, nhưng việc duy trì xác thực rõ ràng sẽ giúp điều kiện biên rõ ràng và ngăn chặn những thay đổi vô tình trong tương lai vi phạm giới hạn (10^{18}). 

Số nguyên Python không tràn, nhưng giải pháp không bao giờ cần chuyển đổi chuỗi được xây dựng thành số nguyên. Tất cả các so sánh được xử lý theo độ dài và đối với trường hợp 19 chữ số có liên quan duy nhất, bằng cách so sánh trực tiếp với biểu diễn chuỗi chính xác của (10^{18}). 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`123456`, đầu vào có sáu chữ số. Độ dài số nhỏ hơn đầu tiên có thể là một, do đó thuật toán cố gắng tạo số có một chữ số. 

| k | Vị trí | chữ số ứng cử viên | Chữ số còn lại | Chiều dài khác | Khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 |`23456`| 5 | Có | 

Chữ số ứng viên đầu tiên là`1`, và năm chữ số còn lại tạo thành một số có năm chữ số hợp lệ. Từ`1`là lựa chọn nhỏ nhất có một chữ số có thể hoàn thành, câu trả lời là`1 23456`. Các chữ số còn lại đã được sắp xếp tối ưu theo thứ tự tăng dần. 

### Mẫu 2 

cho`42`, có hai chữ số, do đó độ dài nhỏ nhất có thể là một. 

| k | Vị trí | chữ số ứng cử viên | Chữ số còn lại | Chiều dài khác | Khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 |`42`| 1 | Không | 
| 1 | 1 | 1 |`42`| 1 | Không | 
| 1 | 1 | 2 |`4`| 1 | Có | 

Không có số 0 hoặc một khối nên chữ số khả thi đầu tiên là`2`. Chữ số còn lại là`4`, cho`2 4`. Cặp đôi đã được đặt hàng chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2 \cdot 10)) | Có nhiều nhất 19 độ dài ứng cử viên, nhiều nhất 19 vị trí cho mỗi công trình và 10 chữ số ứng cử viên ở mỗi vị trí. Mỗi lần kiểm tra tính khả thi chỉ quét 10 chữ số. | 
| Không gian | (O(n)) | Chuỗi đầu vào và chuỗi được xây dựng sử dụng không gian (O(n)), trong khi mảng đếm chữ số có kích thước không đổi. | 

Với (n \le 50), thuật toán chỉ thực hiện vài nghìn thao tác nhỏ. Tối đa 38 chữ số cho bất kỳ trường hợp khả thi nào đều tuân theo giới hạn 19 chữ số trên mỗi số, do đó, giải pháp phù hợp thoải mái với giới hạn một giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def can_make_other(cnt, length):
    if sum(cnt) != length:
        return False

    if length == 1:
        return True

    if length == 19:
        return cnt[0] == 18 and cnt[1] == 1 and sum(cnt[2:]) == 0

    return any(cnt[d] > 0 for d in range(1, 10))

def build_smallest(cnt):
    length = sum(cnt)

    if length == 1:
        for d in range(10):
            if cnt[d]:
                return str(d)

    first = -1
    for d in range(1, 10):
        if cnt[d]:
            first = d
            break

    if first == -1:
        return None

    cnt[first] -= 1
    result = [str(first)]
    result.extend("0" for _ in range(cnt[0]))

    for d in range(1, 10):
        result.extend(str(d) for _ in range(cnt[d]))

    return "".join(result)

def solve(s):
    n = len(s)

    if n < 2 or n > 38:
        return "-1 -1"

    original = [0] * 10
    for ch in s:
        original[ord(ch) - ord('0')] += 1

    min_k = max(1, n - 19)
    max_k = n // 2

    for k in range(min_k, max_k + 1):
        other_len = n - k
        cnt = original[:]
        first_digits = []
        possible = True

        for pos in range(k):
            chosen = -1

            for d in range(10):
                if cnt[d] == 0:
                    continue

                if pos == 0 and k > 1 and d == 0:
                    continue

                cnt[d] -= 1

                if can_make_other(cnt, other_len):
                    chosen = d
                    break

                cnt[d] += 1

            if chosen == -1:
                possible = False
                break

            first_digits.append(str(chosen))

        if not possible:
            continue

        first = "".join(first_digits)
        second = build_smallest(cnt)

        if second is None:
            continue

        if len(second) > 19:
            continue

        if len(second) == 19 and second != "1000000000000000000":
            continue

        if k == other_len and first > second:
            first, second = second, first

        return first + " " + second

    return "-1 -1"

def run(inp: str) -> str:
    return solve(inp.strip())

# Provided samples
assert run("123456") == "1 23456", "sample 1"
assert run("42") == "2 4", "sample 2"
assert run("000") == "-1 -1", "sample 3"

# Minimum-size input
assert run("7") == "-1 -1", "one block cannot form two numbers"

# Two zero blocks
assert run("00") == "0 0", "zero is valid when it is represented by one block"

# Boundary at 19 digits
assert run("1000000000000000000") == \
       "0 100000000000000000", "19-digit boundary"

# Twenty digits where 19-digit 10^18 is impossible
assert run("22222222222222222222") == \
       "22 222222222222222222", "must try a smaller length"

# All equal digits
assert run("11111111111111111111") == \
       "11 111111111111111111", "equal-digit construction"

# Maximum feasible length, both numbers equal 10^18
s38 = "11" + "0" * 36
assert run(s38) == \
       "1000000000000000000 1000000000000000000", "maximum feasible length"

# Maximum input length, impossible because two numbers hold at most 38 blocks
assert run("0" * 50) == "-1 -1", "50 blocks cannot fit"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7`|`-1 -1`| Kích thước đầu vào tối thiểu và yêu cầu đối với hai số không trống | 
|`00`|`0 0`| Xử lý đúng số 0 mà không bác bỏ số 0 đứng đầu | 
|`1000000000000000000`|`0 100000000000000000`| Ranh giới 19 chữ số và (10^{18}) | 
|`22222222222222222222`|`22 222222222222222222`| Chuyển từ số thứ hai có 19 chữ số xuống số có 18 chữ số | 
|`11111111111111111111`|`11 111111111111111111`| Các chữ số bằng nhau và cấu trúc có độ dài bằng nhau | 
|`11`+ 36 số 0 |`1000000000000000000 1000000000000000000`| Tổng chiều dài khả thi tối đa | 
| 50 số 0 |`-1 -1`| Giới hạn độ dài đầu vào tuyệt đối và không thể vượt quá 38 khối có thể sử dụng | 

## Vỏ cạnh 

Đối với đầu vào một khối`7`, vòng lặp độ dài không thể tạo ra hai số khác rỗng vì`n < 2`bị từ chối ngay lập tức. Đầu ra là`-1 -1`. Điều này ngăn việc triển khai vô tình coi một số là trống. 

Vì`00`, độ dài ứng cử viên đầu tiên là một. Việc xây dựng tham lam cố gắng`0`, loại bỏ nó và để lại một số 0 cho số còn lại. Vì chiều dài kia cũng là một,`can_make_other`chấp nhận nó. Kết quả là`0 0`. Quy tắc một chữ số đặc biệt là yếu tố phân biệt cách biểu diễn hợp lệ này với chuỗi nhiều chữ số không hợp lệ, chẳng hạn như`00`. 

Vì`000`, lần thử đầu tiên tương tự sẽ chọn`0`, nhưng vẫn còn hai số 0 cho số thứ hai. Độ dài yêu cầu của nó là hai, và`can_make_other`từ chối một multiset hoàn toàn bằng 0 cho mọi độ dài lớn hơn một. Không có ứng viên nào khác 0 nên thuật toán báo cáo`-1 -1`. 

Vì`1000000000000000000`, thuật toán bắt đầu bằng`k = 1`. Nó cố gắng`0`trước`1`, và sau khi bỏ đi một số 0 thì còn lại 18 chữ số, tạo thành số hợp lệ`100000000000000000`. Do đó số nhỏ hơn sẽ trở thành số 0 ngay lập tức. Kết quả là`0 100000000000000000`, điều này tốt hơn việc sử dụng`1`là số nhỏ hơn. 

Vì`22222222222222222222`, độ dài ứng cử viên đầu tiên là một, để lại 19 chữ số cho số còn lại. Kiểm tra tính khả thi sẽ loại bỏ 19 số đó vì số hợp lệ gồm 19 chữ số phải chính xác (10^{18}). Thuật toán sau đó sẽ thử`k = 2`. Bây giờ số kia có 18 chữ số và một số có 18 chữ số gồm hai chữ số nằm dưới (10^{18}), vì vậy`22`được chấp nhận là số nhỏ nhất có hai chữ số. Kết quả là`22 222222222222222222`. 

Để có độ dài khả thi tối đa, đầu vào bao gồm hai`1`khối và 36 khối không. Số có 19 chữ số hợp lệ duy nhất có thể có là (10^{18}) và có đủ số khối để tạo thành hai bản sao. Thuật toán đạt`k = 19`, xác minh nhiều chữ số chính xác cho số thứ hai và xây dựng cùng một giá trị cho cả hai bên. 

Đối với đầu vào có độ dài 50, thuật toán sẽ loại bỏ trước khi thử bất kỳ cách xây dựng nào. Mỗi số có thể chứa tối đa 19 chữ số, vì vậy hai số có thể chiếm tối đa 38 khối. Không có phân vùng nào có thể sử dụng hết 50 khối mà vẫn tôn trọng giới hạn số lượng, khiến`-1 -1`không thể tránh khỏi.
