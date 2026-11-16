---
title: "CF 1919A - Trao đổi ví"
description: "Alice và Bob mỗi người bắt đầu với một chiếc ví chứa một số đồng xu. Ở mỗi lượt, người chơi hiện tại có thể giữ nguyên các ví hoặc hoán đổi hai ví. Sau lựa chọn đó, họ phải loại bỏ chính xác một đồng xu khỏi ví mà họ hiện đang giữ."
date: "2026-06-08T19:33:36+07:00"
tags: ["codeforces", "competitive-programming", "games", "math"]
categories: ["algorithms"]
codeforces_contest: 1919
codeforces_index: "A"
codeforces_contest_name: "Hello 2024"
rating: 800
weight: 1919
solve_time_s: 125
verified: true
draft: false
---

[CF 1919A - Trao đổi ví](https://codeforces.com/problemset/problem/1919/A) 

**Đánh giá:** 800 
**Tags:** trò chơi, toán học 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Alice và Bob mỗi người bắt đầu với một chiếc ví chứa một số đồng xu. Ở mỗi lượt, người chơi hiện tại có thể giữ nguyên các ví hoặc hoán đổi hai ví. Sau lựa chọn đó, họ phải loại bỏ chính xác một đồng xu khỏi ví mà họ hiện đang giữ. 

Một nước đi chỉ hợp pháp nếu ví mà người chơi cầm có chứa ít nhất một đồng xu trước khi lấy đồng xu đó ra. Người chơi đầu tiên không thể thực hiện một nước đi hợp pháp sẽ thua cuộc. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp số lượng xu ban đầu`a`Và`b`. Cả hai người chơi đều chơi hoàn hảo và chúng ta phải xác định xem người chiến thắng là Alice, người đi trước hay Bob. 

Những hạn chế lớn đến mức đáng ngạc nhiên. Mỗi ví có thể chứa tối đa`10^9`tiền xu và có thể có tới`1000`trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào mô phỏng trò chơi từng bước một đều không thể thực hiện được ngay lập tức. Một trò chơi có thể kéo dài khoảng`a + b`lượt, có thể là khoảng`2 · 10^9`di chuyển. Chúng ta cần một đặc tính toán học trực tiếp của các vị thế thắng và thua. 

Điều khó khăn là khả năng hoán đổi ví khiến trò chơi trông phức tạp hơn trò chơi bỏ cọc đơn giản. Một sai lầm phổ biến là tập trung vào việc người chơi sở hữu ví nào. Quyền sở hữu thay đổi liên tục và thực sự không liên quan. Một sai lầm khác là cho rằng chiếc ví lớn hơn luôn quan trọng hơn. Các ví dụ nhỏ cho thấy rằng chỉ có tổng số xu mới quyết định người chiến thắng. 

Hãy xem xét vị trí:```
a = 1, b = 1
```Tổng số đồng xu là 2. Alice loại bỏ một đồng xu, Bob loại bỏ một đồng xu và sau đó không còn đồng xu nào. Alice là người chơi tiếp theo và thua cuộc. Câu trả lời đúng là`Bob`. 

Bây giờ hãy xem xét:```
a = 1, b = 4
```Tổng số xu là 5. Vì có tổng cộng năm đồng xu có thể tháo rời nên trò chơi kéo dài đúng năm nước đi. Alice thực hiện các bước 1, 3 và 5 nên Bob không thể di chuyển sau đó. Câu trả lời đúng là`Alice`. 

Một cách tiếp cận bất cẩn cố gắng theo dõi quyền sở hữu ví có thể bỏ lỡ mô hình đơn giản hơn nhiều này. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô hình hóa trạng thái trò chơi như`(a, b, turn)`và xác định đệ quy xem mỗi vị trí đang thắng hay thua. Từ mỗi trạng thái, người chơi hiện tại có tối đa hai lựa chọn: hoán đổi hoặc không hoán đổi. Sau khi lựa chọn, một đồng xu sẽ bị xóa khỏi ví hiện tại. 

Đối với các giá trị nhỏ, điều này hoạt động. Chúng ta có thể liệt kê tất cả các vị trí có thể tiếp cận và quan sát cây trò chơi. Vấn đề là không gian trạng thái tăng lên theo kích thước ví. Từ`a`Và`b`có thể lớn như`10^9`, việc tìm kiếm như vậy là hoàn toàn không khả thi. 

Để tìm một mô hình, hãy kiểm tra những gì thực sự thay đổi trong quá trình di chuyển. 

Mỗi nước đi hợp pháp sẽ loại bỏ chính xác một đồng xu khỏi trò chơi. Không có động thái nào tạo ra tiền. Chỉ trao đổi những người hiện đang nắm giữ ví nào. Tổng số xu giảm đi đúng một xu mỗi lượt. 

Cho phép```
S = a + b
```Ban đầu có`S`tổng số xu. Vì mỗi nước đi lấy đi đúng một đồng xu nên có thể có chính xác`S`di chuyển trước khi tất cả tiền xu biến mất. 

Quan sát quan trọng là miễn là có ít nhất một đồng xu tồn tại ở đâu đó, người chơi hiện tại luôn có thể di chuyển. Nếu ví hiện tại của họ trống, họ chỉ cần trao đổi và lấy ví còn trống. Trường hợp duy nhất không thể di chuyển là khi cả hai ví đều không có đồng xu nào. 

Điều đó có nghĩa là độ dài trò chơi hoàn toàn cố định. Chính xác là như vậy`S`di chuyển, bất kể chiến lược. 

Một khi độ dài đã được cố định, trò chơi sẽ trở nên tầm thường. Alice thực hiện các bước đi được đánh số```
1, 3, 5, ...
```và Bob thực hiện các bước đi được đánh số```
2, 4, 6, ...
```Nếu như`S`kỳ quặc, Alice thực hiện nước đi cuối cùng và Bob đối mặt với vị trí trống. Alice thắng. 

Nếu như`S`chẵn, Bob thực hiện nước đi cuối cùng và Alice đối mặt với vị trí trống. Bob thắng. 

Toàn bộ trò chơi quy về việc kiểm tra tính chẵn lẻ của`a + b`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo độ dài trò chơi | Hàm mũ | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`a`Và`b`. 
2. Tính tổng số xu:```
S = a + b
```Mỗi nước đi sẽ loại bỏ chính xác một đồng xu, vì vậy`S`cũng là số nước đi chính xác trong trò chơi. 
3. Kiểm tra xem`S`là số lẻ hoặc số chẵn. 

Nếu như`S`thật kỳ lạ, Alice thực hiện nước đi cuối cùng và giành chiến thắng. 

Nếu như`S`chẵn, Bob thực hiện nước đi cuối cùng và thắng. 
4. Xuất kết quả trúng giải tương ứng. 

### Tại sao nó hoạt động 

Bất biến chính là mỗi nước đi hợp pháp sẽ làm giảm tổng số xu đi đúng một. Không có động thái nào có thể thay đổi điều này. Ngoài ra, bất cứ khi nào có ít nhất một đồng xu vẫn còn ở đâu đó, người chơi hiện tại có thể nhận được một ví không trống bằng cách hoán đổi nếu cần. Do đó, trò chơi không bao giờ kết thúc sớm. 

Trò chơi phải tiếp tục cho đến khi tất cả`a + b`đồng xu đã được gỡ bỏ. Vì tổng số nước đi là cố định và bằng nhau`a + b`, người chiến thắng chỉ được xác định bởi tính chẵn lẻ của số đó. Số nước đi lẻ có nghĩa là Alice thực hiện nước đi cuối cùng, trong khi số chẵn có nghĩa là Bob thực hiện nước đi cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    a, b = map(int, input().split())
    if (a + b) % 2:
        print("Alice")
    else:
        print("Bob")
```Việc thực hiện tuân theo đặc tính toán học một cách trực tiếp. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi tính toán`a + b`và kiểm tra tính chẵn lẻ của nó. Không cần mô phỏng vì chiến lược không ảnh hưởng đến thời lượng trò chơi. Mỗi lần di chuyển luôn tiêu tốn chính xác một xu và trò chơi luôn kéo dài cho đến khi hết xu. 

Không có mối lo ngại về tràn trong Python. Ngay cả số tiền tối đa có thể,```
10^9 + 10^9 = 2 · 10^9
```phù hợp thoải mái trong số học số nguyên thông thường. 

Điểm tinh tế duy nhất là nhận ra rằng việc hoán đổi không bao giờ làm thay đổi tổng số bước đi trong tương lai. Khi quan sát đó được thực hiện, việc thực hiện sẽ trở thành quyết định một dòng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = 1, b = 4
```| Biến | Giá trị | 
| --- | --- | 
| một | 1 | 
| b | 4 | 
| S = a + b | 5 | 
| Tính chẵn lẻ của S | lẻ | 
| Người chiến thắng | Alice | 

Trò chơi có đúng năm đồng xu nên sẽ có đúng năm nước đi. Alice thực hiện các bước 1, 3 và 5. Bob không di chuyển sau khi đồng xu thứ năm được lấy ra. 

### Ví dụ 2 

đầu vào:```
a = 5, b = 3
```| Biến | Giá trị | 
| --- | --- | 
| một | 5 | 
| b | 3 | 
| S = a + b | 8 | 
| Tính chẵn lẻ của S | Thậm chí | 
| Người chiến thắng | Bob | 

Có chính xác tám đồng tiền có thể tháo rời. Nước đi thứ tám thuộc về Bob nên Alice nhận vị trí trống và thua cuộc. 

Những ví dụ này minh họa tính bất biến trung tâm: chỉ có tổng số xu là quan trọng chứ không phải cách chúng được phân phối giữa các ví. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Một phép cộng và một phép kiểm tra chẵn lẻ | 
| Không gian | O(1) | Chỉ sử dụng một vài biến | 

Với tối đa 1000 trường hợp thử nghiệm, tổng công việc là rất nhỏ. Giải pháp dễ dàng phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    ans = []
    for _ in range(t):
        a, b = map(int, input().split())
        ans.append("Alice" if (a + b) % 2 else "Bob")
    print("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

# provided sample
assert run(
"""10
1 1
1 4
5 3
4 5
11 9
83 91
1032 9307
839204 7281
1000000000 1000000000
53110 2024
"""
) == (
"""Bob
Alice
Bob
Alice
Bob
Bob
Alice
Alice
Bob
Bob
"""
)

# minimum values
assert run(
"""1
1 1
"""
) == (
"""Bob
"""
), "minimum input"

# odd total
assert run(
"""1
2 3
"""
) == (
"""Alice
"""
), "odd total wins for Alice"

# even total
assert run(
"""1
7 9
"""
) == (
"""Bob
"""
), "even total wins for Bob"

# maximum values
assert run(
"""1
1000000000 1000000000
"""
) == (
"""Bob
"""
), "largest equal values"

# large odd sum
assert run(
"""1
1000000000 999999999
"""
) == (
"""Alice
"""
), "large odd total"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`Bob`| Vụ việc pháp lý nhỏ nhất | 
|`2 3`|`Alice`| Tổng số xu lẻ | 
|`7 9`|`Bob`| Tổng số xu chẵn | 
|`1000000000 1000000000`|`Bob`| Giá trị bằng nhau tối đa | 
|`1000000000 999999999`|`Alice`| Số tiền lẻ lớn gần giới hạn | 

## Vỏ cạnh 

### Cả hai ví đều bắt đầu bằng một đồng xu 

đầu vào:```
1
1 1
```Tổng số xu là:```
1 + 1 = 2
```Tổng cộng có hai động thái xảy ra. Bob thực hiện nước đi thứ hai mà không để lại đồng xu nào. Alice không có động thái hợp pháp nào sau đó. 

Đầu ra:```
Bob
```### Một ví lớn hơn nhiều so với ví kia 

đầu vào:```
1
1 1000000000
```Tổng số xu là:```
1000000001
```điều đó thật kỳ quặc. Việc phân phối không thành vấn đề. Trò chơi kéo dài chính xác`1000000001`di chuyển và Alice thực hiện bước cuối cùng. 

Đầu ra:```
Alice
```### Ví lớn bằng nhau 

đầu vào:```
1
1000000000 1000000000
```Tổng cộng là:```
2000000000
```đó là số chẵn. Bob luôn nhận được nước đi cuối cùng. 

Đầu ra:```
Bob
```### Ví hiện tại trống rỗng 

Giả sử tại một thời điểm nào đó trạng thái là:```
(0, 5)
```Một người chơi không bị mắc kẹt. Họ có thể trao đổi ví, lấy ví chứa năm đồng xu và loại bỏ một đồng xu. Trò chơi chỉ kết thúc khi trạng thái trở thành:```
(0, 0)
```Đây chính xác là lý do tại sao tổng số lượt di chuyển được cố định ở mức`a + b`, làm cho đối số chẵn lẻ có giá trị.
