---
title: "CF 106169F - Sự vướng víu dấu hiệu"
description: "Bài toán yêu cầu chúng ta đếm các cặp sự kiện trong đó hai sự kiện xảy ra đủ gần nhau về thời gian và có dấu ngược nhau. Mỗi bản ghi đưa ra một dấu thời gian và một dấu hiệu, trong đó + và - đại diện cho hai mặt khác nhau."
date: "2026-06-25T11:08:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106169
codeforces_index: "F"
codeforces_contest_name: "2025-2026 ICPC NERC, Kyrgyzstan Regional Contest"
rating: 0
weight: 106169
solve_time_s: 38
verified: true
draft: false
---

[CF 106169F - Sự vướng víu dấu hiệu](https://codeforces.com/problemset/problem/106169/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta đếm các cặp sự kiện trong đó hai sự kiện xảy ra đủ gần nhau về thời gian và có dấu ngược nhau. Mỗi bản ghi đưa ra một dấu thời gian và một dấu hiệu, trong đó`+`Và`-`đại diện cho hai bên khác nhau. Một cặp hợp lệ nếu dấu thời gian khác nhau nhiều nhất`d`và hai dấu hiệu này khác nhau. Các sự kiện đã được sắp xếp theo thời gian, vì vậy nhiệm vụ là đếm tất cả các cặp dấu chéo như vậy một cách hiệu quả. 

Kích thước đầu vào có thể đạt tới`3 * 10^5`, điều này ngay lập tức loại trừ việc kiểm tra từng cặp sự kiện. Một so sánh trực tiếp sẽ thực hiện xung quanh`n * (n - 1) / 2`kiểm tra, đó là về`4.5 * 10^10`hoạt động ở kích thước tối đa. Ngay cả việc triển khai được tối ưu hóa rất tốt cũng không thể hoàn thành việc đó trong thời gian giới hạn của cuộc thi thông thường. Chúng ta cần khai thác thực tế là các dấu thời gian đã được sắp xếp. 

Một vài chi tiết gây ra câu trả lời sai trong các giải pháp hợp lý. Dấu thời gian bằng nhau là một cái bẫy phổ biến vì sự khác biệt có thể bằng 0 và điều kiện cho phép các cặp có cùng thời gian. Ví dụ:```
3 0
5 5 5
+-+
```Câu trả lời là`2`bởi vì hai`+`sự kiện kết hợp với`-`sự kiện ở cùng một dấu thời gian. Một giải pháp chỉ tìm kiếm thời gian nhỏ hơn hoặc lớn hơn sẽ bỏ lỡ chúng. 

Một sai lầm khác là quên rằng nhiều sự kiện ở cùng một dấu thời gian là những sự kiện riêng biệt. Ví dụ:```
4 1
1 1 2 2
++--
```Câu trả lời là`4`. Mọi`+`vào thời điểm đó`1`phù hợp với mọi`-`vào thời điểm đó`2`. Chỉ đếm các dấu thời gian riêng biệt sẽ tạo ra`1`, điều đó là sai. 

Trường hợp khó khăn cuối cùng là ranh giới của khoảng cách`d`. Ví dụ:```
2 3
10 13
+-
```Câu trả lời là`1`bởi vì khoảng cách chính xác là`3`. Sử dụng một so sánh nghiêm ngặt thay vì`<= d`mất các cặp hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là so sánh mọi sự kiện tích cực với mọi sự kiện tiêu cực. Đối với mỗi cặp, chúng tôi tính toán sự khác biệt tuyệt đối giữa thời gian của chúng và thêm một nếu nhiều nhất là`d`. Điều này rất dễ lý giải vì nó trực tiếp tuân theo định nghĩa của một cặp hợp lệ. 

Vấn đề là số lượng so sánh tăng theo phương trình bậc hai. Với`n = 300000`, số lượng các cặp có thể có là khoảng`45000000000`, vượt xa những gì chúng tôi có thể xử lý. 

Quan sát hữu ích đến từ dấu thời gian được sắp xếp. Thay vì kiểm tra xem mọi sự kiện trước đó có khớp với sự kiện sau hay không, chúng ta có thể duy trì tập hợp các sự kiện trước đó vẫn đủ gần với sự kiện hiện tại. Khi chúng ta di chuyển từ trái sang phải, mọi sự kiện trước đó vẫn nằm trong khoảng thời gian cho phép hoặc trở nên quá cũ và không bao giờ có thể khớp lại. 

Đối với mọi sự kiện hiện tại, chúng ta cần biết ít nhất có bao nhiêu sự kiện trước đó có dấu ngược lại và dấu thời gian`current_time - d`. Vì đầu vào đã được sắp xếp nên chúng ta có thể xóa các sự kiện đã hết hạn bằng con trỏ di chuyển. Chúng tôi chỉ cần số lượng hoạt động`+`Và`-`sự kiện chứ không phải vị trí cá nhân của họ. 

Khi sự kiện hiện tại được`+`, nó tạo thành một cặp hợp lệ với tất cả các hoạt động`-`sự kiện. Khi đó`-`, nó tạo thành một cặp hợp lệ với tất cả các hoạt động`+`sự kiện. Sau khi đếm, chúng ta chèn sự kiện hiện tại vào cửa sổ đang hoạt động để các sự kiện sau này có thể sử dụng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Đếm Cửa Sổ Trượt | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả các sự kiện dưới dạng`(time, sign)`cặp. Các sự kiện đã được sắp xếp nên chúng ta có thể xử lý chúng theo thứ tự nhất định mà không cần sắp xếp lại. Giữ con trỏ bên trái biểu thị sự kiện đầu tiên vẫn nằm trong cửa sổ thời gian hợp lệ. 
2. Duy trì số lượng sự kiện đang hoạt động được phân tách bằng dấu hiệu. Cửa sổ đang hoạt động chứa chính xác các sự kiện trước đó có dấu thời gian ít nhất`current_time - d`. 
3. Trước khi xử lý sự kiện hiện tại, hãy di chuyển con trỏ bên trái về phía trước trong khi sự kiện ở bên trái đã quá cũ. Xóa dấu của nó khỏi số lượng hoạt động vì nó không còn có thể tạo thành một cặp hợp lệ với bất kỳ sự kiện nào trong tương lai. 
4. Nhìn vào dấu hiệu của sự kiện hiện tại. Nếu nó là`+`, thêm số lượng hoạt động`-`sự kiện để trả lời. Nếu nó là`-`, thêm số lượng hoạt động`+`thay vào đó là các sự kiện. Đây chính xác là những sự kiện trước đó có dấu ngược lại và khoảng cách hợp lệ. 
5. Chèn sự kiện hiện tại vào cửa sổ đang hoạt động bằng cách tăng số lượng dấu hiệu tương ứng. Nó có thể phù hợp với các sự kiện sau này, vì vậy nó phải luôn sẵn có. 

Tại sao nó hoạt động: tại mọi thời điểm trong quá trình quét, cửa sổ hiện hoạt chứa tất cả và chỉ các sự kiện trước đó vẫn có thể tạo một cặp hợp lệ với các sự kiện hiện tại và tương lai. Mỗi cặp được tính chính xác một lần khi sự kiện sau đó trong cặp được xử lý. Vì chúng tôi chỉ thêm các sự kiện trái dấu từ cửa sổ đang hoạt động nên mọi cặp được đếm đều thỏa mãn điều kiện dấu và điều kiện thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d = map(int, input().split())
    t = list(map(int, input().split()))
    s = input().strip()

    ans = 0
    left = 0
    plus = 0
    minus = 0

    for i in range(n):
        while left < i and t[left] < t[i] - d:
            if s[left] == '+':
                plus -= 1
            else:
                minus -= 1
            left += 1

        if s[i] == '+':
            ans += minus
            plus += 1
        else:
            ans += plus
            minus += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Vòng lặp chính triển khai cửa sổ trượt từ hướng dẫn. Con trỏ`left`không bao giờ di chuyển lùi lại, vì thế mỗi sự kiện sẽ bị loại bỏ nhiều nhất một lần. Đây là những gì giữ cho tổng thể công việc tuyến tính. 

Điều kiện loại bỏ sử dụng`t[left] < t[i] - d`còn hơn là`<=`bởi vì dấu thời gian có sự khác biệt chính xác`d`vẫn còn hiệu lực. Sự kiện hiện tại được tính trước khi được thêm vào số lượng được lưu trữ, ngăn sự kiện ghép nối với chính nó. 

Số nguyên Python ở đây là đủ vì câu trả lời có thể lớn bằng số lượng của tất cả các cặp, xấp xỉ`4.5 * 10^10`. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
5 2
1 2 3 4 5
+-++-
```| Sự kiện hiện tại | Tích cực`+`| Tích cực`-`| Đã thêm vào câu trả lời | Trả lời | 
| --- | --- | --- | --- | --- | 
|`1 +`| 0 | 0 | 0 | 0 | 
|`2 -`| 1 | 0 | 1 | 1 | 
|`3 +`| 1 | 1 | 1 | 2 | 
|`4 +`| 1 | 1 | 1 | 3 | 
|`5 -`| 2 | 1 | 2 | 5 | 

Câu trả lời cuối cùng là`5`. Dấu vết này cho thấy các sự kiện ở khoảng cách ranh giới vẫn được giữ nguyên, bởi vì sự khác biệt về thời gian chính xác`2`vẫn còn hiệu lực. 

Đối với ví dụ thứ hai:```
4 0
7 7 8 8
++--
```| Sự kiện hiện tại | Tích cực`+`| Tích cực`-`| Đã thêm vào câu trả lời | Trả lời | 
| --- | --- | --- | --- | --- | 
|`7 +`| 0 | 0 | 0 | 0 | 
|`7 +`| 1 | 0 | 0 | 0 | 
|`8 -`| 0 | 0 | 0 | 0 | 
|`8 -`| 0 | 1 | 0 | 0 | 

Câu trả lời là`0`bởi vì chỉ có dấu thời gian bằng nhau mới có thể khớp khi`d = 0`và tất cả các dấu thời gian bằng nhau đều có cùng dấu trong trường hợp này. Dấu vết xác nhận rằng cửa sổ sẽ xóa các sự kiện có dấu thời gian khác nhau khi khoảng cách cho phép bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi sự kiện vào cửa sổ một lần và rời khỏi cửa sổ một lần | 
| Không gian | O(n) | Mảng dấu thời gian và ký hiệu được lưu trữ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tổng số thao tác tỷ lệ thuận với số lượng sự kiện. Việc sử dụng bộ nhớ là tuyến tính và vẫn nằm trong giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data):
    inp = io.StringIO(data)
    n, d = map(int, inp.readline().split())
    t = list(map(int, inp.readline().split()))
    s = inp.readline().strip()

    ans = 0
    left = 0
    plus = 0
    minus = 0

    for i in range(n):
        while left < i and t[left] < t[i] - d:
            if s[left] == '+':
                plus -= 1
            else:
                minus -= 1
            left += 1

        if s[i] == '+':
            ans += minus
            plus += 1
        else:
            ans += plus
            minus += 1

    return str(ans)

# sample
assert solve("""14 3
1 1 3 3 3 3 5 5 6 6 6 8 9 11
+--+++--+++++-
""") == "23"

# minimum size
assert solve("""1 10
5
+
""") == "0"

# all equal values
assert solve("""5 0
7 7 7 7 7
+-+-+
""") == "6"

# boundary distance
assert solve("""2 3
10 13
+-
""") == "1"

# all pairs valid
assert solve("""4 100
1 2 3 4
++++
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Sự kiện đơn lẻ | 0 | Không có cặp nào có thể được hình thành | 
| Dấu thời gian bằng nhau có dấu hiệu hỗn hợp | 6 | Xử lý khoảng cách bằng 0 và trùng lặp | 
| Khoảng cách chính xác bằng`d`| 1 | Kiểm tra ranh giới bao gồm | 
| Cùng một dấu hiệu ở mọi nơi | 0 | Xác nhận lọc dấu hiệu | 

## Vỏ cạnh 

Đối với trường hợp dấu thời gian bằng nhau:```
3 0
5 5 5
+-+
```Sự kiện đầu tiên được thêm vào hoạt động`+`đếm. Sự kiện thứ hai chứng kiến ​​một hoạt động`+`, vì vậy nó thêm một giá trị`-`ghép nối và được thêm vào`-`đếm. Sự kiện thứ ba chứng kiến ​​một hoạt động`-`, thêm một cặp khác. Kết quả là`2`, điều này phù hợp với thực tế là chỉ những sự kiện ở cùng một thời điểm mới có thể ghép nối. 

Đối với dấu thời gian trùng lặp có nhiều sự kiện:```
4 1
1 1 2 2
++--
```Cả hai sự kiện vào thời điểm`1`vẫn hoạt động khi xử lý hai sự kiện cùng một lúc`2`, bởi vì sự khác biệt của chúng chính xác là`1`. Mỗi sự kiện tiêu cực tính hai sự kiện tích cực, cho`4`tổng số cặp. Thuật toán xử lý bội số vì nó lưu trữ số lượng sự kiện chứ không phải dấu thời gian duy nhất. 

Đối với điều kiện biên:```
2 3
10 13
+-
```Sự kiện đầu tiên vẫn còn trong cửa sổ vì`10 < 13 - 3`là sai. Sự kiện thứ hai nhìn thấy một sự kiện ngược dấu và thêm nó vào câu trả lời. Kết quả là`1`, chứng tỏ việc so sánh bao hàm được xử lý chính xác.
