---
title: "CF 471A - MUH và Gậy"
description: "Chúng tôi được cung cấp chính xác sáu chiều dài thanh. Để tạo ra một con vật, bốn chiếc gậy phải được dùng làm chân, nghĩa là bốn chiếc gậy đó đều phải có cùng chiều dài. Sau khi chọn 4 gậy chân còn lại 2 gậy. Những điều này xác định loại động vật."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 471
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 269 (Div. 2)"
rating: 1100
weight: 471
solve_time_s: 81
verified: true
draft: false
---

[CF 471A - MUH và Gậy](https://codeforces.com/problemset/problem/471/A) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp chính xác sáu chiều dài thanh. Để tạo ra một con vật, bốn chiếc gậy phải được dùng làm chân, nghĩa là bốn chiếc gậy đó đều phải có cùng chiều dài. 

Sau khi chọn 4 gậy chân còn lại 2 gậy. Những điều này xác định loại động vật. 

Đối với con voi, hai chiếc gậy còn lại phải có chiều dài bằng nhau, vì đầu và thân có cùng kích thước. 

Đối với gấu, hai cây gậy còn lại phải có độ dài khác nhau, vì đầu phải ngắn hơn thân. 

Nhiệm vụ của chúng ta là xác định xem sáu cây gậy có thể tạo thành con gấu, con voi hay không. Tuyên bố đảm bảo rằng cả hai loài động vật đều không thể tồn tại cùng một lúc. 

Những hạn chế là rất nhỏ. Chỉ có sáu que và mỗi que có độ dài từ 1 đến 9. Mọi thuật toán hợp lý sẽ chạy ngay lập tức. Thử thách không phải là hiệu suất mà là xác định chính xác bốn cây gậy nào sẽ được chọn làm chân khi tồn tại nhiều chiều dài bằng nhau. 

Một lỗi phổ biến là chỉ tìm kiếm một giá trị xuất hiện đúng bốn lần. Các chân chỉ cần bốn que bằng nhau, do đó, giá trị xuất hiện năm hoặc sáu lần cũng có thể cung cấp các chân. 

Coi như:```
4 4 4 4 4 5
```Chúng ta có thể sử dụng bốn trong số năm thanh có chiều dài 4 làm chân. Các que còn lại là 4 và 5 khác nhau nên đáp án là`Bear`. Việc kiểm tra "chính xác bốn lần xuất hiện" sẽ từ chối trường hợp này một cách không chính xác. 

Một trường hợp tế nhị khác xảy ra khi sáu cây gậy giống hệt nhau:```
3 3 3 3 3 3
```Bốn que trở thành chân, để lại hai que có chiều dài 3 và 3. Vì các que còn lại bằng nhau nên đáp án đúng là`Elephant`. 

Tình huống khó khăn thứ ba là khi không có độ dài nào xuất hiện ít nhất bốn lần:```
1 2 3 4 5 6
```Không có bốn cây gậy bằng nhau nên không thể tạo thành con vật nào. Câu trả lời là`Alien`. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là thử mọi lựa chọn có thể về bốn cây gậy trong số sáu cây gậy. Chỉ có C(6,4) = 15 khả năng. Đối với mỗi lựa chọn, hãy kiểm tra xem các que đã chọn có độ dài bằng nhau hay không. Nếu có, hãy kiểm tra hai que còn lại. Chiều dài còn lại bằng nhau nghĩa là một con voi, chiều dài còn lại khác nhau nghĩa là một con gấu. 

Cách tiếp cận này đúng vì nó trực tiếp kiểm tra mọi cách hợp lệ để chọn chân. Chỉ với 15 sự kết hợp, nó đủ nhanh. 

Vấn đề càng trở nên đơn giản hơn khi chúng ta tập trung vào tần số. Điều quan trọng duy nhất là liệu độ dài nào đó có xuất hiện ít nhất bốn lần hay không. Nếu không có độ dài như vậy thì câu trả lời là ngay lập tức`Alien`. 

Khi chúng tôi tìm thấy một độ dài xuất hiện ít nhất bốn lần, chúng tôi sẽ loại bỏ chính xác bốn bản sao của nó. Chỉ còn lại hai cây gậy. Mối quan hệ của họ hoàn toàn quyết định câu trả lời. Độ dài bằng nhau cho`Elephant`, độ dài khác nhau cho`Bear`. 

Quan sát này loại bỏ sự cần thiết phải liệt kê các kết hợp và dẫn đến việc triển khai rất ngắn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(6,4)) = O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(6 log 6) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc sáu chiều dài thanh. 
2. Đếm xem mỗi độ dài xuất hiện bao nhiêu lần. 
3. Tìm độ dài có tần số ít nhất là 4. 

Chiều dài như vậy có thể cung cấp bốn chân cần thiết. 
4. Nếu không có độ dài nào có tần số ít nhất là 4, hãy in`Alien`. 

Nếu không có bốn cây gậy bằng nhau thì không thể tạo ra một trong hai con vật. 
5. Loại bỏ chính xác bốn lần xuất hiện của độ dài đã chọn. 

Chúng tôi đang cố định bốn thanh đó làm chân. 
6. Thu thập hai que còn lại. 
7. Nếu hai độ dài còn lại bằng nhau thì in`Elephant`. 

Chiều dài đầu và thân bằng nhau phù hợp với định nghĩa của loài voi. 
8. Nếu không thì in`Bear`. 

Độ dài còn lại khác nhau phù hợp với định nghĩa về gấu. 

### Tại sao nó hoạt động 

Yêu cầu xác định đối với cả hai loài động vật là sự tồn tại của bốn chiếc gậy có chân có chiều dài bằng nhau. Bất kỳ cách xây dựng hợp lệ nào cũng phải sử dụng bốn thanh có cùng chiều dài, do đó một số chiều dài phải xảy ra ít nhất bốn lần. 

Sau khi chọn được bốn que như vậy thì còn lại đúng hai que. Loại động vật chỉ phụ thuộc vào hai cây gậy này. Các độ dài còn lại bằng nhau thỏa mãn điều kiện con voi, trong khi các độ dài còn lại khác nhau thỏa mãn điều kiện gấu. Vì tuyên bố đảm bảo câu trả lời là duy nhất nên việc kiểm tra bất kỳ độ dài nào có tần số ít nhất là bốn sẽ mang lại kết quả phân loại chính xác. 

## Giải pháp Python```python
import sys
from collections import Counter

input = sys.stdin.readline

sticks = list(map(int, input().split()))
cnt = Counter(sticks)

leg_length = None

for length, freq in cnt.items():
    if freq >= 4:
        leg_length = length
        break

if leg_length is None:
    print("Alien")
else:
    remaining = []
    removed = 0

    for x in sticks:
        if x == leg_length and removed < 4:
            removed += 1
        else:
            remaining.append(x)

    if remaining[0] == remaining[1]:
        print("Elephant")
    else:
        print("Bear")
```Phần đầu tiên đếm số lần xuất hiện của mỗi chiều dài thanh. Sau đó chúng tôi tìm kiếm bất kỳ độ dài nào xuất hiện ít nhất bốn lần. Nếu không tồn tại, chúng ta biết ngay rằng không có bộ chân hợp lệ nào có thể được hình thành. 

Bước loại bỏ đáng được chú ý. Chúng tôi loại bỏ chính xác bốn bản sao, không phải tất cả các bản sao. Những trường hợp như`4 4 4 4 4 5`phụ thuộc vào việc giữ cây gậy thứ năm. Việc loại bỏ tất cả các lần xuất hiện sẽ chỉ để lại một que còn lại và tạo ra kết quả không chính xác. 

Sau khi loại bỏ, vẫn còn đúng hai que. So sánh hai giá trị đó trực tiếp xác định loại động vật. 

Không cần sắp xếp, mặc dù giải pháp dựa trên sắp xếp cũng sẽ được chấp nhận. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2 5 4 4 4
```| Bước | Tiểu bang | 
| --- | --- | 
| Đếm tần số | {4:4, 2:1, 5:1} | 
| Chiều dài chân được tìm thấy | 4 | 
| Xóa bốn số 4 | Còn lại = [2, 5] | 
| So sánh các gậy còn lại | 2 ≠ 5 | 
| Đầu ra | Gấu | 

Bốn que có chiều dài 4 trở thành chân. Những chiếc gậy còn lại có chiều dài khác nhau nên tượng trưng cho đầu và thân của một con gấu. 

### Ví dụ 2 

đầu vào:```
3 3 3 3 5 5
```| Bước | Tiểu bang | 
| --- | --- | 
| Đếm tần số | {3:4, 5:2} | 
| Chiều dài chân được tìm thấy | 3 | 
| Xóa bốn số 3 | Còn lại = [5, 5] | 
| So sánh các gậy còn lại | 5 = 5 | 
| Đầu ra | Voi | 

Hai que còn lại bằng nhau, phù hợp với yêu cầu của con voi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có sáu que được xử lý | 
| Không gian | O(1) | Bảng tần số lưu trữ tối đa chín độ dài | 

Kích thước đầu vào được cố định ở sáu phần tử, do đó thuật toán chỉ thực hiện một số thao tác. Nó dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import Counter

def solve():
    sticks = list(map(int, input().split()))
    cnt = Counter(sticks)

    leg_length = None

    for length, freq in cnt.items():
        if freq >= 4:
            leg_length = length
            break

    if leg_length is None:
        print("Alien")
        return

    remaining = []
    removed = 0

    for x in sticks:
        if x == leg_length and removed < 4:
            removed += 1
        else:
            remaining.append(x)

    print("Elephant" if remaining[0] == remaining[1] else "Bear")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("4 2 5 4 4 4\n") == "Bear", "sample 1"

# custom cases
assert run("3 3 3 3 5 5\n") == "Elephant", "equal remaining sticks"
assert run("1 2 3 4 5 6\n") == "Alien", "no four equal sticks"
assert run("4 4 4 4 4 5\n") == "Bear", "frequency greater than four"
assert run("9 9 9 9 9 9\n") == "Elephant", "all sticks equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 3 3 3 5 5`|`Elephant`| Các que còn lại bằng nhau | 
|`1 2 3 4 5 6`|`Alien`| Không có bộ chân hợp lệ nào tồn tại | 
|`4 4 4 4 4 5`|`Bear`| Phải xóa đúng bốn bản, không được hết | 
|`9 9 9 9 9 9`|`Elephant`| Tất cả các gậy giống hệt nhau | 

## Vỏ cạnh 

Hãy xem xét:```
4 4 4 4 4 5
```Tần số 4 là 5 vẫn đủ cho đôi chân. Thuật toán loại bỏ chính xác 4 bản sao của 4, để lại`[4, 5]`. Vì các que còn lại khác nhau nên nó xuất ra`Bear`. Bất kỳ giải pháp nào yêu cầu tần số chính xác là bốn sẽ thất bại ở đây. 

Coi như:```
3 3 3 3 3 3
```Thuật toán chọn 3 làm độ dài chân và loại bỏ bốn bản sao. Các gậy còn lại là`[3, 3]`. Vì chúng bằng nhau nên đầu ra là`Elephant`. Điều này xác nhận việc xử lý đúng khi tất cả sáu que có cùng chiều dài. 

Coi như:```
1 2 3 4 5 6
```Không có độ dài nào xuất hiện ít nhất bốn lần. Việc tìm kiếm độ dài chân không thành công nên thuật toán in ngay`Alien`. Điều này xử lý chính xác các đầu vào trong đó việc xây dựng bất kỳ con vật nào là không thể.
