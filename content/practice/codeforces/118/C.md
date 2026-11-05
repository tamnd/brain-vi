---
title: "CF 118C - Số Fancy"
description: "Chúng ta được cấp một chuỗi các chữ số đại diện cho số xe. Số được coi là đẹp nếu có ít nhất k vị trí chứa cùng một chữ số. Chúng ta có thể thay đổi bất kỳ chữ số nào thành một chữ số khác và việc thay đổi chữ số a thành chữ số b sẽ có giá trị Nhiệm vụ có hai mục tiêu."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 118
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 89 (Div. 2)"
rating: 1900
weight: 118
solve_time_s: 171
verified: true
draft: false
---

[CF 118C - Số ưa thích](https://codeforces.com/problemset/problem/118/C) 

**Xếp hạng:** 1900 
**Tags:** vũ phu, tham lam, phân loại, dây 
**Thời gian giải:** 2m 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ số đại diện cho số xe. Con số được coi là đẹp nếu ít nhất`k`vị trí chứa cùng một chữ số. Chúng ta có thể đổi bất kỳ chữ số nào thành chữ số khác và thay đổi chữ số`a`thành chữ số`b`chi phí`|a - b|`. 

Nhiệm vụ có hai mục tiêu. Đầu tiên, giảm thiểu tổng chi phí sửa đổi. Thứ hai, trong số tất cả các câu trả lời có chi phí tối thiểu, trả về chuỗi kết quả nhỏ nhất về mặt từ điển. 

Kích thước đầu vào đủ nhỏ để chúng ta có thể đủ khả năng thử tất cả các chữ số mục tiêu từ`0`ĐẾN`9`. Độ dài chuỗi tối đa là`10^4`, vì vậy mọi giải pháp xung quanh`O(10 * n log n)`hoặc`O(10 * n)`là hoàn toàn an toàn. Cách tiếp cận bậc hai đối với tất cả các vị trí sẽ vẫn được áp dụng ở đây, nhưng chúng ta vẫn nên thiết kế logic một cách cẩn thận vì sự ràng buộc về mặt từ điển rất tinh tế. 

Phần khó khăn không phải là tính toán chi phí tối thiểu. Phần khó khăn là tạo ra câu trả lời nhỏ nhất về mặt từ điển trong số tất cả các giải pháp có chi phí tối thiểu. 

Hãy xem xét ví dụ này:```
4 3
9090
```Nếu chúng ta quyết định làm cho ba chữ số bằng`0`, chúng ta chỉ cần thay đổi một`9`vào trong`0`, trị giá`9`. Có hai sự lựa chọn:```
0090
9000
```Cả hai đều có cùng mức giá nhưng`0090`nhỏ hơn về mặt từ điển. Một chiến lược tham lam bất cẩn luôn thay đổi chữ số ngoài cùng bên trái trước tiên có thể thất bại đối với các mục tiêu khác. 

Một trường hợp tinh vi khác xuất hiện khi chuyển đổi các chữ số nhỏ hơn mục tiêu thành các chữ số lớn hơn mục tiêu. 

Ví dụ:```
5 4
12345
```Giả sử chữ số mục tiêu là`3`. 

Thay đổi`2 -> 3`Và`4 -> 3`mỗi chi phí`1`. 

Thay đổi`1 -> 3`Và`5 -> 3`mỗi chi phí`2`. 

Để có được kết quả nhỏ nhất về mặt từ điển trong số các giải pháp có chi phí bằng nhau, chúng ta nên: 

thay đổi các chữ số lớn hơn từ trái sang phải, vì việc giảm một chữ số trước đó sẽ làm cho chuỗi nhỏ hơn, 

nhưng thay đổi các chữ số nhỏ hơn từ phải sang trái, vì việc tăng chữ số trước đó sẽ làm cho chuỗi lớn hơn. 

Nhiều giải pháp không chính xác bỏ lỡ sự bất đối xứng này. 

Một trường hợp cạnh khác là khi chuỗi đã chứa ít nhất`k`chữ số bằng nhau. 

Ví dụ:```
6 4
777712
```Đáp án đúng là chi phí`0`và chính chuỗi gốc đó. Bất kỳ thuật toán nào tiếp tục sửa đổi các chữ số sau khi đạt`k`sự cố có thể vô tình làm câu trả lời trở nên tồi tệ hơn. 

## Phương pháp tiếp cận 

Một ý tưởng vũ phu rất đơn giản. Đối với mỗi chữ số mục tiêu`d`từ`0`ĐẾN`9`, hãy thử mọi tập hợp con vị trí có thể có để chuyển đổi thành`d`, tính tổng chi phí và giữ kết quả tốt nhất. 

Điều này hoạt động về mặt khái niệm vì số đẹp cuối cùng phải chọn một số chữ số xuất hiện ít nhất`k`lần. Khi chữ số đó được cố định, mọi vị trí thay đổi sẽ đóng góp độc lập vào chi phí. 

Vấn đề là số lượng tập hợp con. Ngay cả khi chúng ta chỉ xem xét các vị trí chưa bằng`d`, vẫn có thể có khoảng`2^n`khả năng. Với`n = 10^4`, điều này thật vô vọng. 

Quan sát quan trọng là chi phí chỉ phụ thuộc vào khoảng cách chữ số. Nếu chữ số mục tiêu của chúng tôi là`d`, sau đó chuyển đổi một chữ số có khoảng cách`1`luôn tốt hơn việc chuyển đổi một chữ số theo khoảng cách`2`. 

Điều đó có nghĩa là với mỗi chữ số mục tiêu, chúng ta nên tham lam đảm nhận các vị trí theo thứ tự chi phí tăng dần: 

đầu tiên tất cả các chữ số có khoảng cách`1`, 

sau đó khoảng cách`2`, 

và vân vân. 

Điều này ngay lập tức giảm thiểu tổng chi phí. 

Sau khi cố định chi phí tối thiểu có thể, chúng ta vẫn cần chuỗi nhỏ nhất về mặt từ điển. Điều này giới thiệu một lớp tham lam thứ hai. 

Giả sử chúng ta đang chuyển đổi các chữ số lớn hơn`d`. Việc thay thế chữ số lớn bằng chữ số nhỏ hơn sẽ làm giảm ký tự. Thực hiện việc này sớm hơn trong chuỗi sẽ cải thiện thứ tự từ điển, vì vậy chúng ta nên xử lý các vị trí đó từ trái sang phải. 

Đối với các chữ số nhỏ hơn`d`, sự thay thế sẽ làm tăng ký tự. Việc tăng ký tự sớm hơn sẽ làm ảnh hưởng đến trật tự từ điển, vì vậy chúng ta nên xử lý các vị trí đó từ phải sang trái. 

Điều này mang lại một cấu trúc tham lam rõ ràng cho từng chữ số mục tiêu. 

Chúng tôi thử tất cả mười chữ số mục tiêu, xây dựng chuỗi tốt nhất có thể cho mỗi chữ số, sau đó chọn: 

đầu tiên theo chi phí tối thiểu, 

thì nhỏ nhất về mặt từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10 · 2^n) | O(n) | Quá chậm | 
| Tối ưu | O(10 · n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem mỗi chữ số đã xuất hiện bao nhiêu lần trong chuỗi. 

Nếu chữ số`d`ít nhất đã xuất hiện`k`lần, chi phí cho mục tiêu này ngay lập tức`0`. 
2. Với mỗi chữ số mục tiêu`d`từ`0`ĐẾN`9`, tính xem cần thêm bao nhiêu bản sao. 

Cho phép:```
need = k - count[d]
```Nếu như`need <= 0`, chuỗi hiện tại đã thỏa mãn điều kiện cho chữ số này. 
3. Tạo một bản sao có thể thay đổi của chuỗi. 

Chúng tôi sẽ dần dần chuyển đổi các chữ số thành`d`. 
4. Khoảng cách xử lý từ`1`ĐẾN`9`. 

Cho mọi khoảng cách`dist`, có nhiều nhất hai chữ số nguồn hữu ích:`d - dist`Và`d + dist`. 

Đây chính xác là những chữ số có chi phí chuyển đổi bằng`dist`. 
5. Xử lý các chữ số đầu tiên lớn hơn`d`. 

Nếu như`d + dist <= 9`, quét chuỗi từ trái sang phải. 

Bất cứ khi nào chúng ta nhìn thấy chữ số`d + dist`và vẫn cần thêm bản sao của`d`, thay thế nó bằng`d`. 

Việc thay thế một chữ số lớn hơn bằng một chữ số nhỏ hơn sẽ làm cho chuỗi sớm hơn về mặt từ điển nhỏ hơn, do đó, từ trái sang phải là tối ưu. 
6. Sau đó xử lý các chữ số nhỏ hơn`d`. 

Nếu như`d - dist >= 0`, quét chuỗi từ phải sang trái. 

Bất cứ khi nào chúng ta nhìn thấy chữ số`d - dist`và vẫn cần thêm bản sao, hãy thay thế bằng`d`. 

Ở đây chúng tôi đang tăng chữ số. Trì hoãn những mức tăng này càng xa càng tốt sẽ giữ cho chuỗi nhỏ hơn về mặt từ điển. 
7. Mỗi lần thay thế đều bổ sung thêm`dist`vào tổng chi phí và giảm`need`bởi một. 

Dừng lại một lần`need`trở thành số không. 
8. So sánh ứng viên được xây dựng với ứng viên tốt nhất toàn cầu hiện tại. 

Thích tổng chi phí nhỏ hơn. 

Nếu chi phí bằng nhau, hãy ưu tiên các chuỗi nhỏ hơn về mặt từ điển. 

### Tại sao nó hoạt động 

Đối với một chữ số mục tiêu cố định`d`, chiến lược chi phí tối thiểu phải luôn chiếm các vị trí theo thứ tự tăng dần khoảng cách từ`d`. Bất kỳ giải pháp nào sử dụng khoảng cách lớn hơn trong khi có khoảng cách nhỏ hơn đều có thể được cải thiện ngay lập tức. 

Trong số tất cả các lựa chọn có chi phí tối thiểu, thứ tự từ điển chỉ phụ thuộc vào vị trí nào được thay đổi. Việc giảm các chữ số trước đó sẽ giúp ích cho trật tự từ điển, trong khi việc tăng các chữ số trước đó sẽ làm ảnh hưởng đến trật tự từ điển. Đó là lý do tại sao các chữ số lớn hơn được sửa đổi từ trái sang phải và các chữ số nhỏ hơn từ phải sang trái. 

Vì chúng tôi tính toán độc lập câu trả lời tối ưu cho mọi chữ số mục tiêu có thể có nên mức tối thiểu tổng thể trong số các ứng cử viên đó là câu trả lời cuối cùng tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    best_cost = float('inf')
    best_string = ""

    for target in range(10):
        cnt = s.count(str(target))
        need = k - cnt

        cur = list(s)
        cost = 0

        if need > 0:
            for dist in range(1, 10):
                if need == 0:
                    break

                bigger = target + dist
                if bigger <= 9:
                    for i in range(n):
                        if need == 0:
                            break
                        if int(cur[i]) == bigger:
                            cur[i] = str(target)
                            cost += dist
                            need -= 1

                smaller = target - dist
                if smaller >= 0:
                    for i in range(n - 1, -1, -1):
                        if need == 0:
                            break
                        if int(cur[i]) == smaller:
                            cur[i] = str(target)
                            cost += dist
                            need -= 1

        candidate = ''.join(cur)

        if cost < best_cost:
            best_cost = cost
            best_string = candidate
        elif cost == best_cost and candidate < best_string:
            best_string = candidate

    print(best_cost)
    print(best_string)

solve()
```Vòng lặp bên ngoài thử mọi chữ số có thể là chữ số lặp lại cuối cùng. Vì chỉ có mười chữ số nên phần này thực tế là không đổi. 

Biến`need`theo dõi xem chúng tôi vẫn cần thêm bao nhiêu bản sao của chữ số mục tiêu. Khi nó đạt đến 0, ứng cử viên hiện tại đã hoàn tất. 

Thứ tự sửa đổi là phần tế nhị nhất trong quá trình thực hiện. 

Khi chuyển đổi các chữ số lớn hơn thành mục tiêu, chúng tôi quét từ trái sang phải:```
for i in range(n):
```bởi vì việc giảm một chữ số trước đó sẽ cải thiện thứ tự từ điển. 

Khi chuyển đổi các chữ số nhỏ hơn thành mục tiêu, chúng tôi quét từ phải sang trái:```
for i in range(n - 1, -1, -1):
```bởi vì việc tăng chữ số sau sẽ ít gây hại hơn về mặt từ điển. 

Một chi tiết tinh tế khác là chúng tôi sửa đổi`cur`, không phải chuỗi gốc`s`. Số lượng ban đầu chỉ được sử dụng để xác định số lượng cần thay thế ban đầu. 

Bước so sánh phải kiểm tra chi phí trước tiên và thứ tự từ điển thứ hai. So sánh chuỗi Python đã triển khai chính xác thứ tự từ điển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 5
898196
```Giả sử chúng ta thử chữ số mục tiêu`8`. 

Số lượng ban đầu của`8`là`2`, Vì thế:```
need = 5 - 2 = 3
```| Khoảng cách | Hành động | Chuỗi | Chi phí | Cần | 
| --- | --- | --- | --- | --- | 
| 1 | Thay đổi đầu tiên`9 -> 8`| 888196 | 1 | 2 | 
| 1 | Thay đổi giây`9 -> 8`| 888186 | 2 | 1 | 
| 2 | Thay đổi`6 -> 8`| 888188 | 4 | 0 | 

Ứng cử viên cuối cùng:```
888188
```Dấu vết này cho thấy tại sao việc lấy khoảng cách nhỏ hơn trước là tối ưu. Bất kỳ giải pháp nào sử dụng chuyển đổi đắt tiền hơn trước khi sử dụng cả hai`9`s sẽ lãng phí chi phí. 

### Ví dụ 2 

đầu vào:```
5 4
12345
```Hãy thử chữ số mục tiêu`3`. 

Số lượng ban đầu của`3`là`1`, Vì thế:```
need = 3
```| Khoảng cách | Hành động | Chuỗi | Chi phí | Cần | 
| --- | --- | --- | --- | --- | 
| 1 | Thay đổi`4 -> 3`từ trái | 12335 | 1 | 2 | 
| 1 | Thay đổi`2 -> 3`từ bên phải | 13335 | 2 | 1 | 
| 2 | Thay đổi`5 -> 3`| 13333 | 4 | 0 | 

Ứng cử viên cuối cùng:```
13333
```Ví dụ này thể hiện thứ tự truyền tải không đối xứng. Việc tăng các chữ số nhỏ hơn từ bên phải sẽ giữ trật tự từ điển tốt hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10 · n) | Chúng tôi thử 10 chữ số mục tiêu và quét chuỗi một số lần không đổi | 
| Không gian | O(n) | Bản sao có thể thay đổi của chuỗi | 

Thuật toán thoải mái phù hợp với các giới hạn. Ngay cả với`n = 10^4`, tổng khối lượng công việc chỉ khoảng vài trăm nghìn thao tác. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    s = input().strip()

    best_cost = float('inf')
    best_string = ""

    for target in range(10):
        cnt = s.count(str(target))
        need = k - cnt

        cur = list(s)
        cost = 0

        if need > 0:
            for dist in range(1, 10):
                if need == 0:
                    break

                bigger = target + dist
                if bigger <= 9:
                    for i in range(n):
                        if need == 0:
                            break
                        if int(cur[i]) == bigger:
                            cur[i] = str(target)
                            cost += dist
                            need -= 1

                smaller = target - dist
                if smaller >= 0:
                    for i in range(n - 1, -1, -1):
                        if need == 0:
                            break
                        if int(cur[i]) == smaller:
                            cur[i] = str(target)
                            cost += dist
                            need -= 1

        candidate = ''.join(cur)

        if cost < best_cost:
            best_cost = cost
            best_string = candidate
        elif cost == best_cost and candidate < best_string:
            best_string = candidate

    print(best_cost)
    print(best_string)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run(
"""6 5
898196
"""
) == "4\n888188\n", "sample 1"

# already beautiful
assert run(
"""6 4
777712
"""
) == "0\n777712\n", "already valid"

# minimum size
assert run(
"""2 2
10
"""
) == "1\n00\n", "minimum size case"

# lexicographical tie handling
assert run(
"""4 3
9090
"""
) == "9\n0090\n", "lexicographically smallest among equal costs"

# all digits equal
assert run(
"""5 5
33333
"""
) == "0\n33333\n", "all equal"

# boundary conversion distances
assert run(
"""3 3
090
"""
) == "9\n000\n", "large digit distance handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6 4 / 777712`| trị giá`0`| Đầu vào đã đẹp | 
|`2 2 / 10`|`00`| Hạn chế tối thiểu | 
|`4 3 / 9090`|`0090`| Sự ràng buộc về mặt từ điển | 
|`5 5 / 33333`| chuỗi không thay đổi | Không có sửa đổi không cần thiết | 
|`3 3 / 090`|`000`| Xử lý chi phí chuyển đổi lớn | 

## Vỏ cạnh 

Hãy xem xét lại ví dụ về tie-break:```
4 3
9090
```Chữ số mục tiêu`0`đã xuất hiện hai lần nên chúng tôi cần thêm một lần nữa`0`. 

Cả hai`9`chi phí`9`để chuyển đổi. Thuật toán xử lý các chữ số lớn hơn từ trái sang phải, do đó nó thay đổi chữ số đầu tiên`9`. 

Kết quả:```
0090
```thay vì:```
9000
```Cả hai đều có chi phí như nhau, nhưng`0090`nhỏ hơn về mặt từ điển. 

Bây giờ hãy xem xét vấn đề truyền tải không đối xứng:```
5 4
12345
```Chữ số mục tiêu`3`. 

Khi xử lý các chữ số nhỏ hơn như`2`, thuật toán quét từ phải sang trái. Độ trễ này tăng dần về phía cuối chuỗi. 

Thay đổi ngoài cùng bên trái`2`quá sớm sẽ tạo ra tiền tố từ điển lớn hơn một cách không cần thiết. 

Cuối cùng, hãy xem xét một trường hợp đã thỏa mãn yêu cầu:```
6 4
777712
```Số chữ số`7`đã rồi`4`, Vì thế:```
need = 0
```Không có sửa đổi xảy ra. Chi phí vẫn bằng 0 và chuỗi gốc được giữ nguyên chính xác.
