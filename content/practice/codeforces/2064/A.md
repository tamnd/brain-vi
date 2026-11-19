---
title: "CF 2064A - Cuộc thi lập trình"
description: "Chúng ta bắt đầu với một chuỗi nhị phân s và một chuỗi trống t. Một bước di chuyển cho phép chúng ta lấy bất kỳ hậu tố nào của một chuỗi và nối nó vào chuỗi khác. Vì chỉ có thể di chuyển các hậu tố nên thứ tự tương đối của các ký tự không bao giờ thay đổi. Ký tự chỉ có thể vượt qua ranh giới giữa s và t."
date: "2026-06-08T07:23:08+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 2064
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1005 (Div. 2)"
rating: 800
weight: 2064
solve_time_s: 95
verified: true
draft: false
---

[CF 2064A - Cuộc thi lập trình web](https://codeforces.com/problemset/problem/2064/A) 

**Đánh giá:** 800 
**Tags:** tham lam, dây 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một chuỗi nhị phân`s`và một chuỗi trống`t`. 

Một bước di chuyển cho phép chúng ta lấy bất kỳ hậu tố nào của một chuỗi và nối nó vào chuỗi khác. Vì chỉ có thể di chuyển các hậu tố nên thứ tự tương đối của các ký tự không bao giờ thay đổi. Các nhân vật chỉ có thể vượt qua ranh giới giữa`s`Và`t`. 

Mục tiêu là kết thúc với mọi`0`bên trong`s`và mọi`1`bên trong`t`. Chúng tôi cần số lần chuyển hậu tố tối thiểu cần thiết để đạt được trạng thái đó. 

Độ dài của mỗi chuỗi tối đa là 1000 và tổng độ dài trên tất cả các trường hợp thử nghiệm cũng tối đa là 1000. Mặc dù các ràng buộc nhỏ nhưng giải pháp dự kiến ​​sẽ đơn giản hơn nhiều so với bất kỳ phương pháp dựa trên tìm kiếm nào. Câu trả lời có thể được tính bằng cách quét chuỗi một lần. 

Phần khó khăn là hiểu được một động thái thực sự thay đổi điều gì. Vì chỉ có thể chuyển hậu tố nên chúng ta không thể sắp xếp lại các ký tự một cách tùy tiện. Mỗi bước di chuyển chỉ làm dịch chuyển ranh giới giữa hai dây. Cấu trúc của các khối liên tiếp có ký tự bằng nhau trở thành quan sát chính. 

Hãy xem xét một số trường hợp cạnh dễ dàng. 

Nếu chuỗi chỉ chứa số 0, chẳng hạn như:```
s = "00000"
```sau đó`t`có thể vẫn trống. Câu trả lời là`0`. 

Một giải pháp bất cẩn luôn làm lay động tất cả`1`vào`t`có thể thực hiện sai các thao tác không cần thiết. 

Bây giờ hãy xem xét:```
s = "1111"
```Trạng thái cuối cùng hợp lệ duy nhất là:```
s = ""
t = "1111"
```Một bước di chuyển sẽ chuyển toàn bộ chuỗi. Câu trả lời là`1`. 

Một trường hợp tế nhị khác là:```
s = "001"
```Khối cuối cùng đã là khối của những cái đó. Chúng ta có thể di chuyển hậu tố đó trực tiếp đến`t`trong một động thái, vì vậy câu trả lời là`1`. 

Một lỗi phổ biến là đếm số khối và trả về giá trị đó. Ở đây có hai khối (`00`Và`1`), nhưng chỉ cần một động tác. 

Cuối cùng, hãy xem xét:```
s = "101"
```Câu trả lời là`3`. 

Cấu trúc xen kẽ buộc chúng ta phải di chuyển nhiều hậu tố qua lại. Bất kỳ cách tiếp cận nào chỉ đếm số lượng số một hoặc số không sẽ thất bại trong ví dụ này. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là coi mỗi trạng thái là một cặp chuỗi`(s, t)`và chạy BFS. Mỗi lần chuyển hậu tố có thể sẽ tạo ra một trạng thái mới. BFS cuối cùng sẽ tìm ra số bước di chuyển tối thiểu vì tất cả các bước di chuyển đều có chi phí như nhau. 

Vấn đề là số lượng các bang tăng lên một cách bùng nổ. Ngay cả đối với độ dài chuỗi vừa phải, vẫn có quá nhiều cặp chuỗi có thể khám phá. Cách tiếp cận này trở nên không thực tế gần như ngay lập tức. 

Để tìm ra giải pháp tốt hơn, chúng ta cần hiểu mục đích của một động thái. 

Xem chuỗi dưới dạng các khối ký tự bằng nhau liên tiếp. Ví dụ:```
001110011
```chứa các khối:```
00 | 111 | 00 | 11
```Mỗi khi nhân vật thay đổi giữa các vị trí liền kề, một khối mới sẽ bắt đầu. 

Giả sử chúng ta quét chuỗi từ trái sang phải. Bất cứ khi nào một khối`1`s xuất hiện, những cái đó cuối cùng phải kết thúc ở`t`. Bởi vì các bước di chuyển hoạt động dựa trên các hậu tố, mỗi lần chuyển đổi giữa các ký tự khác nhau sẽ tạo ra một trở ngại bổ sung đòi hỏi phải có một lần chuyển tiếp khác. 

Sau khi xem xét một số ví dụ, một mẫu đơn giản sẽ xuất hiện. 

Cho phép`changes`là số lượng vị trí`i`như vậy:```
s[i] != s[i - 1]
```Sau đó: 

Nếu chuỗi bắt đầu bằng`0`, đáp án chính xác đấy`changes`. 

Nếu chuỗi bắt đầu bằng`1`, câu trả lời là`changes + 1`. 

Tại sao ký tự bắt đầu lại quan trọng? 

Nếu chuỗi bắt đầu bằng`1`, khối đầu tiên cuối cùng phải được chuyển ra khỏi`s`. Điều đó đòi hỏi một hoạt động bổ sung. Nếu chuỗi bắt đầu bằng`0`, khối đầu tiên đã ở đúng phía và không yêu cầu di chuyển ban đầu. 

Điều này ngay lập tức mang lại một`O(n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Read the binary string`s`. 
2. Đếm xem các ký tự liền kề khác nhau bao nhiêu lần. 

Đối với mỗi chỉ số`i > 0`, nếu như`s[i] != s[i-1]`, tăng một bộ đếm`changes`. 

Mỗi vị trí như vậy đánh dấu sự bắt đầu của một khối mới. 
3. Nếu ký tự đầu tiên là`1`, thêm một câu nữa vào câu trả lời. 

Khối đầu tiên phải được chuyển ra khỏi`s`, tạo thêm một nước đi bắt buộc. 
4. Ngược lại, nếu ký tự đầu tiên là`0`, câu trả lời đơn giản là`changes`. 
5. Xuất kết quả. 

### Tại sao nó hoạt động 

Chuỗi có thể được xem như các khối số 0 và số 1 xen kẽ. 

Mọi ranh giới giữa hai khối lân cận phải được vượt qua bởi ranh giới hậu tố di chuyển tại một điểm nào đó. Mỗi thay đổi ký tự đóng góp chính xác một lần chuyển được yêu cầu. Điều này giải thích cho tất cả các chuyển tiếp trong chuỗi. 

Nếu chuỗi bắt đầu bằng một khối số 1, khối đó ban đầu nằm sai vị trí vì tất cả các khối phải kết thúc bên trong`t`. Việc di chuyển khối đầu tiên đó ra ngoài yêu cầu một thao tác bổ sung ngoài các chuyển đổi đã được tính. 

Do đó, số lần di chuyển tối thiểu bằng số lần thay đổi ký tự, cộng thêm một khi ký tự đầu tiên được thay đổi.`1`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()

        changes = 0
        for i in range(1, n):
            if s[i] != s[i - 1]:
                changes += 1

        answer = changes + (1 if s[0] == '1' else 0)
        print(answer)

solve()
```Việc thực hiện trực tiếp theo sau việc quan sát các khối. 

Vòng lặp đếm mọi vị trí mà ký tự hiện tại khác với ký tự trước đó. Đây chính xác là số lần chuyển tiếp giữa các khối liên tiếp. 

Sau đó, chúng tôi kiểm tra ký tự đầu tiên. Nếu nó là`1`, chúng ta thêm một nước đi nữa. Nếu không thì không cần thêm gì cả. 

Điều kiện biên duy nhất là một chuỗi có độ dài bằng một. Trong trường hợp đó vòng lặp không bao giờ thực thi,`changes`vẫn bằng 0 và công thức vẫn hoạt động chính xác.`"0"`sản xuất`0`, trong khi`"1"`sản xuất`1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = "00110"
```| Chỉ mục | Nhân vật | Thay đổi từ Trước | thay đổi | 
| --- | --- | --- | --- | 
| 0 | 0 | - | 0 | 
| 1 | 0 | Không | 0 | 
| 2 | 1 | Có | 1 | 
| 3 | 1 | Không | 1 | 
| 4 | 0 | Có | 2 | 

Chuỗi bắt đầu bằng`0`. 

| Biến | Giá trị | 
| --- | --- | 
| thay đổi | 2 | 
| bắt đầu bằng 1 | Không | 
| trả lời | 2 | 

Đầu ra:```
2
```Ví dụ này cho thấy câu trả lời bằng số lần chuyển khối khi khối đầu tiên đã bao gồm các số 0. 

### Ví dụ 2 

đầu vào:```
s = "101"
```| Chỉ mục | Nhân vật | Thay đổi từ Trước | thay đổi | 
| --- | --- | --- | --- | 
| 0 | 1 | - | 0 | 
| 1 | 0 | Có | 1 | 
| 2 | 1 | Có | 2 | 

Chuỗi bắt đầu bằng`1`. 

| Biến | Giá trị | 
| --- | --- | 
| thay đổi | 2 | 
| bắt đầu bằng 1 | Có | 
| trả lời | 3 | 

Đầu ra:```
3
```Ví dụ này minh họa sự di chuyển thêm do khối số 1 dẫn đầu gây ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét qua chuỗi | 
| Không gian | O(1) | Chỉ có một vài quầy được lưu trữ | 

Vì tổng độ dài của tất cả các trường hợp thử nghiệm tối đa là 1000 nên thuật toán chạy thoải mái trong giới hạn. Ngay cả đối với đầu vào lớn hơn nhiều, một lần quét tuyến tính duy nhất vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []

        for _ in range(t):
            n = int(input())
            s = input().strip()

            changes = 0
            for i in range(1, n):
                if s[i] != s[i - 1]:
                    changes += 1

            out.append(str(changes + (1 if s[0] == '1' else 0)))

        return "\n".join(out)

    return solve()

# provided sample
assert run(
"""5
5
00110
4
1111
3
001
5
00000
3
101
"""
) == """2
1
1
0
3"""

# minimum size, single zero
assert run(
"""1
1
0
"""
) == "0"

# minimum size, single one
assert run(
"""1
1
1
"""
) == "1"

# all zeros
assert run(
"""1
6
000000
"""
) == "0"

# alternating characters
assert run(
"""1
6
101010
"""
) == "6"

# single transition
assert run(
"""1
6
000111
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`0`| Chuỗi hoàn toàn hợp lệ nhỏ nhất | 
|`1`|`1`| Trường hợp đầu tiên hợp lệ nhỏ nhất | 
|`000000`|`0`| Tất cả các nhân vật đã thuộc về`s`| 
|`101010`|`6`| Số lần chuyển tiếp tối đa | 
|`000111`|`1`| Chính xác là ranh giới một khối | 

## Vỏ cạnh 

### Tất cả số không 

đầu vào:```
1
5
00000
```Quá trình quét không tìm thấy sự khác biệt liền kề.```
changes = 0
```Ký tự đầu tiên là`0`, vì vậy không có động thái bổ sung nào được thêm vào. 

Đầu ra:```
0
```Điều này thể hiện chính xác thực tế là trạng thái ban đầu đã đáp ứng được mục tiêu. 

### Tất cả những cái đó 

đầu vào:```
1
4
1111
```Việc quét lại không tìm thấy sự khác biệt liền kề.```
changes = 0
```Ký tự đầu tiên là`1`, vì vậy chúng tôi thêm một.```
answer = 1
```Một bước chuyển toàn bộ chuỗi vào`t`. 

### Chuyển đổi đơn 

đầu vào:```
1
6
001111
```Có chính xác một vị trí mà nhân vật thay đổi.```
changes = 1
```Chuỗi bắt đầu bằng`0`.```
answer = 1
```Di chuyển hậu tố`"1111"`ĐẾN`t`ngay lập tức đạt được mục tiêu. 

### Ký tự thay thế 

đầu vào:```
1
5
01010
```Mỗi cặp liền kề khác nhau.```
changes = 4
```Ký tự đầu tiên là`0`.```
answer = 4
```Đây là cấu trúc đòi hỏi khắt khe nhất vì mỗi nhân vật đều bắt đầu một khối mới. Công thức đếm chính xác tất cả các lần chuyển được yêu cầu.
