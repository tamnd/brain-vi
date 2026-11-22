---
title: "CF 27A - Thử nghiệm tiếp theo"
description: "Chúng tôi được cung cấp các chỉ số kiểm tra đã tồn tại trong hệ thống. Mỗi chỉ số là một số nguyên dương và tất cả các chỉ số đều khác biệt. Nhiệm vụ là tìm số nguyên dương nhỏ nhất không xuất hiện trong danh sách."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 27
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 27 (Codeforces format, Div. 2)"
rating: 1200
weight: 27
solve_time_s: 73
verified: true
draft: false
---
[CF 27A - Thử nghiệm tiếp theo](https://codeforces.com/problemset/problem/27/A) 

**Đánh giá:** 1200 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp các chỉ số kiểm tra đã tồn tại trong hệ thống. Mỗi chỉ số là một số nguyên dương và tất cả các chỉ số đều khác biệt. Nhiệm vụ là tìm số nguyên dương nhỏ nhất không xuất hiện trong danh sách. 

Đây là ý tưởng cổ điển "thiếu tích cực nhỏ nhất" nhưng các ràng buộc đủ nhỏ để chúng ta không cần bất kỳ cấu trúc dữ liệu nâng cao nào. Số lượng bài kiểm tra hiện có nhiều nhất là 3000 và mỗi chỉ số cũng nhiều nhất là 3000. Ngay cả một$O(n^2)$giải pháp thực hiện khoảng chín triệu thao tác trong trường hợp xấu nhất, dễ dàng phù hợp với giới hạn 2 giây trong Python. 

Khó khăn chính không phải là hiệu suất mà là việc xử lý tìm kiếm một cách chính xác. Câu trả lời không phải là giá trị lớn nhất cộng một trừ khi mọi số nguyên dương nhỏ hơn đều tồn tại. 

Hãy xem xét đầu vào này:```
3
1 7 2
```Câu trả lời đúng là`3`, không`8`. Chỉ số tiếp theo phải là số dương nhỏ nhất chưa được sử dụng. 

Một sai lầm dễ mắc phải khác là bắt đầu tìm kiếm từ`0`. Chỉ số kiểm tra chỉ là số nguyên dương. 

Ví dụ:```
4
2 3 4 5
```Câu trả lời đúng là`1`vì số nguyên dương nhỏ nhất bị thiếu ngay lập tức. 

Trường hợp cạnh thứ ba xuất hiện khi tất cả các giá trị từ`1`ĐẾN`n`đang có mặt. 

Ví dụ:```
5
1 2 3 4 5
```Câu trả lời trở thành`6`. Việc triển khai bất cẩn chỉ kiểm tra các số đã có trong mảng có thể không bao giờ in được gì. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Bắt đầu từ`1`và liên tục kiểm tra xem số hiện tại có tồn tại trong mảng hay không. Số đầu tiên không tìm thấy chính là đáp án. 

Để kiểm tra xem một giá trị có tồn tại hay không, chúng ta có thể quét tuyến tính toàn bộ mảng. Vì có thể có tối đa 3000 giá trị ứng viên và mỗi tìm kiếm có thể kiểm tra tất cả 3000 phần tử, nên độ phức tạp trong trường hợp xấu nhất trở thành$O(n^2)$. 

Đối với vấn đề này, thậm chí điều đó là đủ nhanh. Khoảng chín triệu so sánh được chấp nhận trong Python. 

Tuy nhiên, cấu trúc vấn đề mang lại cho chúng ta một giải pháp rõ ràng hơn. Chúng tôi chỉ quan tâm liệu một số có tồn tại hay không chứ không quan tâm đến việc nó xuất hiện ở đâu. Điều đó có nghĩa là truy vấn thành viên là hoạt động cốt lõi. Một bộ hỗ trợ kiểm tra thành viên ở mức trung bình$O(1)$thời gian. 

Quan sát rất đơn giản: khi các giá trị được lưu trữ trong một tập hợp, chúng ta có thể kiểm tra các số`1, 2, 3, ...`từng cái một cho đến khi chúng ta tìm thấy giá trị còn thiếu đầu tiên. 

Câu trả lời được đảm bảo là nhiều nhất`n + 1`. Nếu có`n`các số nguyên dương riêng biệt và tất cả các số từ`1`bởi vì`n`thì tồn tại`n + 1`bị thiếu. Điều này cho phép chúng tôi dừng lại sau khi kiểm tra nhiều nhất`n + 1`ứng viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Đã chấp nhận | 
| Tối ưu |$O(n)$trung bình |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`và danh sách các chỉ số kiểm tra hiện có. 
2. Lưu trữ tất cả các chỉ số trong một bộ. 

Một bộ cho phép kiểm tra tư cách thành viên nhanh chóng, đây chính xác là những gì chúng ta cần khi hỏi "số này đã tồn tại chưa?" 
3. Lặp qua các số nguyên từ`1`ĐẾN`n + 1`. 

Câu trả lời không thể vượt quá`n + 1`. Nếu mọi giá trị từ`1`bởi vì`n`có mặt thì số dương còn thiếu nhỏ nhất phải là`n + 1`. 
4. Với mỗi số nguyên`x`, kiểm tra xem nó có tồn tại trong tập hợp không. 

Nếu như`x`vắng mặt, in`x`ngay lập tức và dừng lại. 
5. Số bị thiếu đầu tiên gặp phải trong quá trình quét là chỉ số kiểm tra tiếp theo bắt buộc. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra các số nguyên dương theo thứ tự tăng dần. Trước khi đạt tới một giá trị nào đó`x`, mọi số nguyên dương nhỏ hơn đã được xác minh là tồn tại trong tập hợp. Khi`x`là giá trị đầu tiên không được tìm thấy, nó chính xác là số nguyên dương nhỏ nhất bị thiếu trong mảng. 

Vì quá trình quét tiến hành theo thứ tự và dừng ở khoảng trống đầu tiên nên thuật toán không thể bỏ qua câu trả lời hợp lệ nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# solution

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    used = set(arr)

    for x in range(1, n + 2):
        if x not in used:
            print(x)
            return

solve()
```Bước đầu tiên đọc mảng đầu vào và chuyển đổi nó thành một tập hợp có tên`used`. Sự chuyển đổi này là sự tối ưu hóa quan trọng vì việc kiểm tra tư cách thành viên diễn ra theo thời gian trung bình không đổi. 

Vòng lặp chạy từ`1`bởi vì`n + 1`bao gồm. Giới hạn trên là quan trọng. Nếu tất cả các số từ`1`ĐẾN`n`có mặt, câu trả lời phải là`n + 1`. sử dụng`range(1, n + 1)`sẽ bỏ lỡ trường hợp đó. 

Khi tìm thấy giá trị còn thiếu, hàm sẽ in giá trị đó và trả về ngay lập tức. Không cần phải tiếp tục quét vì chúng tôi xử lý số theo thứ tự tăng dần. 

Không có vấn đề tràn số nguyên nào tồn tại ở đây vì tất cả các giá trị đều rất nhỏ. Việc thực hiện chủ yếu là về ranh giới cẩn thận và điều kiện dừng chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 7 2
```| Hiện tại x | Tồn tại trong bộ? | Hành động | 
| --- | --- | --- | 
| 1 | Có | Tiếp tục | 
| 2 | Có | Tiếp tục | 
| 3 | Không | In 3 | 

Bộ này chứa`{1, 2, 7}`. Thuật toán kiểm tra các số theo thứ tự tăng dần và dừng ngay ở khoảng trống đầu tiên. Điều này thể hiện tính bất biến cốt lõi: mọi số nguyên dương nhỏ hơn đã được xác nhận trước khi câu trả lời được in ra. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
```| Hiện tại x | Tồn tại trong bộ? | Hành động | 
| --- | --- | --- | 
| 1 | Có | Tiếp tục | 
| 2 | Có | Tiếp tục | 
| 3 | Có | Tiếp tục | 
| 4 | Có | Tiếp tục | 
| 5 | Có | Tiếp tục | 
| 6 | Không | In 6 | 

Dấu vết này cho thấy tại sao vòng lặp phải tiếp tục cho đến khi`n + 1`. Tất cả các số nguyên dương nhỏ hơn đều tồn tại, do đó giá trị còn thiếu đầu tiên xuất hiện ngay sau tiền tố lớn nhất liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$trung bình | Xây dựng bộ và quét lên tới`n + 1`giá trị | 
| Không gian |$O(n)$| Bộ lưu trữ tất cả các chỉ số | 

Với`n ≤ 3000`, giải pháp này nằm trong giới hạn thoải mái. Ngay cả cách tiếp cận bậc hai cũng có thể vượt qua, nhưng giải pháp dựa trên tập hợp sẽ sạch hơn và có quy mô tốt hơn về mặt khái niệm. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    arr = list(map(int, input().split()))

    used = set(arr)

    for x in range(1, n + 2):
        if x not in used:
            print(x)
            return

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup_stdout
    return out.getvalue().strip()

# provided sample
assert run("3\n1 7 2\n") == "3", "sample 1"

# minimum size input
assert run("1\n1\n") == "2", "single existing value"

# missing 1
assert run("4\n2 3 4 5\n") == "1", "smallest positive missing immediately"

# consecutive sequence
assert run("5\n1 2 3 4 5\n") == "6", "answer is n + 1"

# unordered values
assert run("6\n4 2 1 6 3 7\n") == "5", "handles unsorted input"

# maximum-style boundary pattern
assert run("3\n3000 2999 1\n") == "2", "large values do not matter"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`2`| Đầu vào hợp lệ nhỏ nhất | 
|`4 / 2 3 4 5`|`1`| Thiếu dương nhỏ nhất | 
|`5 / 1 2 3 4 5`|`6`| Câu trả lời trở thành`n + 1`| 
|`6 / 4 2 1 6 3 7`|`5`| Thứ tự đầu vào không liên quan | 
|`3 / 3000 2999 1`|`2`| Giá trị lớn không liên quan không ảnh hưởng đến kết quả | 

## Vỏ cạnh 

Hãy xem xét trường hợp`1`bị thiếu ngay lập tức. 

đầu vào:```
4
2 3 4 5
```Tập hợp trở thành`{2, 3, 4, 5}`. Thuật toán bắt đầu kiểm tra từ`1`. Từ`1`không có mặt, nó in`1`ngay lập tức. Điều này xử lý chính xác câu trả lời nhỏ nhất có thể. 

Bây giờ hãy xem xét trường hợp tất cả các giá trị từ`1`bởi vì`n`hiện hữu. 

đầu vào:```
5
1 2 3 4 5
```Thuật toán kiểm tra`1`,`2`,`3`,`4`, Và`5`, tất cả đều tồn tại trong tập hợp. Sau đó nó sẽ kiểm tra`6`, cái bị thiếu nên nó in ra`6`. Đây là lý do tại sao vòng lặp phải chạy cho đến khi`n + 1`. 

Cuối cùng, hãy xem xét đầu vào không có thứ tự với các khoảng trống. 

đầu vào:```
6
7 3 1 2 6 4
```Tập hợp trở thành`{1, 2, 3, 4, 6, 7}`. Việc kiểm tra tiến hành như sau: 

-`1`tồn tại 
-`2`tồn tại 
-`3`tồn tại 
-`4`tồn tại 
-`5`không tồn tại 

Thuật toán in`5`. Thứ tự ban đầu không bao giờ quan trọng vì bộ chỉ lưu trữ thông tin thành viên.
