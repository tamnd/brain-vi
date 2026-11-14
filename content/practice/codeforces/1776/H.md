---
title: "CF 1776H - Beppa và SwerChat"
description: "Danh sách được hiển thị bởi SwerChat được sắp xếp theo thời gian gần đây. Thành viên trực tuyến gần đây nhất sẽ xuất hiện đầu tiên, thành viên gần đây thứ hai sẽ xuất hiện thứ hai, v.v. Lúc 9 giờ, Beppa ghi đơn đặt hàng a. 22h cô ghi lại đơn hàng khác b."
date: "2026-06-09T11:47:00+07:00"
tags: ["codeforces", "competitive-programming", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1776
codeforces_index: "H"
codeforces_contest_name: "SWERC 2022-2023 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 1300
weight: 1776
solve_time_s: 133
verified: true
draft: false
---

[CF 1776H - Beppa và SwerChat](https://codeforces.com/problemset/problem/1776/H) 

**Đánh giá:** 1300 
**Thẻ:** hai con trỏ 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Danh sách được hiển thị bởi SwerChat được sắp xếp theo thời gian gần đây. Thành viên trực tuyến gần đây nhất sẽ xuất hiện đầu tiên, thành viên gần đây thứ hai sẽ xuất hiện thứ hai, v.v. 

Lúc 9h, Beppa ghi đơn đặt hàng`a`. 22h chị ghi lại đơn hàng khác`b`. 

Giữa hai thời điểm đó, điều duy nhất có thể thay đổi thứ tự là một thành viên trực tuyến. Vì không có ai trực tuyến đồng thời nên mọi sự kiện trực tuyến sẽ được xử lý lần lượt. Khi một thành viên trực tuyến, họ sẽ trở thành thành viên được nhìn thấy gần đây nhất, vì vậy họ sẽ chuyển lên đầu danh sách. 

Chúng tôi phải xác định số lượng thành viên riêng biệt tối thiểu phải trực tuyến ít nhất một lần để chuyển đổi đơn đặt hàng từ`a`vào trong`b`. 

Tổng kích thước của tất cả các trường hợp thử nghiệm nhiều nhất là`10^5`, do đó, bất kỳ giải pháp nào so sánh tất cả các cặp thành viên hoặc mô phỏng lặp đi lặp lại các thao tác trên danh sách lớn sẽ quá chậm. MỘT`O(n)`hoặc`O(n log n)`giải pháp cho mỗi trường hợp thử nghiệm là đủ nhanh. 

Điều tinh tế là một thành viên có thể xuất hiện ở một vị trí khác mà không nhất thiết phải trực tuyến. Họ có thể bị đẩy lùi khi người khác lên mạng. Một cách tiếp cận ngây thơ đếm tất cả những người có quan điểm thay đổi sẽ đánh giá quá cao câu trả lời. 

Coi như:```
a = [1, 2, 3]
b = [2, 1, 3]
```Thành viên duy nhất`2`cần phải lên mạng. Thành viên`1`thay đổi vị trí, nhưng không bao giờ phải trực tuyến. 

Một sai lầm dễ mắc phải khác là cho rằng mọi đảo ngược giữa hai hoán vị đều tương ứng với một sự kiện trực tuyến. 

Ví dụ:```
a = [1, 2, 3, 4]
b = [3, 4, 1, 2]
```Thành viên`3`Và`4`có thể trực tuyến, tạo ra đơn hàng cuối cùng. Câu trả lời là`2`, không`4`. 

Một trường hợp đặc biệt khác xảy ra khi các danh sách đã giống hệt nhau:```
a = [1, 2, 3, 4]
b = [1, 2, 3, 4]
```Không ai cần phải lên mạng nên câu trả lời đúng là`0`. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là đoán xem thành viên nào đang trực tuyến và kiểm tra xem liệu một số chuỗi hoạt động đi trước có thể tạo ra`b`. 

Ý tưởng đó về nguyên tắc là đúng nhưng hoàn toàn không thực tế. có`2^n`các tập hợp con có thể có của các thành viên và thậm chí việc xác minh một tập hợp con ứng viên sẽ yêu cầu mô phỏng các hoạt động của danh sách. Với`n`lên tới`10^5`, điều này là không thể. 

Nhận xét quan trọng đến từ việc xem xét những thành viên **chưa bao giờ trực tuyến**. 

Giả sử một thành viên không bao giờ trực tuyến. Họ không bao giờ được chuyển lên phía trước. Điều duy nhất có thể xảy ra với họ là các thành viên khác vượt lên trước họ. 

Do đó, tất cả các thành viên chưa bao giờ trực tuyến đều giữ nguyên thứ tự tương đối so với hoán vị ban đầu`a`. 

Điều này có nghĩa là danh sách cuối cùng`b`có cấu trúc rất cụ thể:```
[ all members who were online ]
[ all members who were never online ]
```Phần thứ hai phải xuất hiện theo đúng thứ tự tương đối như trong`a`. 

Để giảm thiểu số lượng thành viên trực tuyến, chúng tôi muốn tối đa hóa số lượng thành viên không bao giờ trực tuyến. 

Cho phép`pos[x]`giữ chức vụ thành viên`x`TRONG`a`. 

Thay thế từng phần tử của`b`bởi vị trí của nó trong`a`. 

Ví dụ:```
a = [1, 4, 2, 5, 3]

pos:
1 -> 1
4 -> 2
2 -> 3
5 -> 4
3 -> 5

b = [4, 5, 1, 2, 3]

positions in a:
[2, 4, 1, 3, 5]
```Các thành viên không bao giờ trực tuyến phải tạo thành một hậu tố`b`, và vị trí của chúng trong`a`phải tăng lên một cách nghiêm túc. Đó chính xác là điều kiện để bảo toàn trật tự tương đối. 

Vì vậy, chúng ta cần hậu tố dài nhất của chuỗi biến đổi đang tăng nghiêm ngặt. 

Nếu hậu tố đó có độ dài`L`, sau đó`L`các thành viên có thể không bao giờ trực tuyến và câu trả lời là:```
n - L
```### So sánh phương pháp tiếp cận 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị`a`. 
2. Xây dựng một mảng`pos`Ở đâu`pos[x]`lưu trữ vị trí của thành viên`x`TRONG`a`. 
3. Đi ngang`b`từ phải sang trái. 
4. Duy trì bất biến mà hậu tố hiện đang được kiểm tra của`b`tương ứng với một chuỗi các vị trí tăng dần nghiêm ngặt trong`a`. 
5. Bắt đầu từ cuối, tiếp tục di chuyển sang trái trong khi:```
pos[b[i-1]] < pos[b[i]]
```Điều này có nghĩa là thứ tự tương đối của các thành viên đó khớp với thứ tự trong`a`, vì vậy tất cả chúng có thể không bao giờ trực tuyến. 
6. Vị trí đầu tiên mà điều kiện này không thành công là ranh giới giữa tiền tố trực tuyến và hậu tố không bao giờ trực tuyến. 
7. Nếu hậu tố tăng dần bắt đầu ở chỉ số`s`, thì độ dài của nó là`n - s`. 
8. Đầu ra:```
s
```bởi vì chính xác là lần đầu tiên`s`thành viên của`b`phải thuộc về bộ trực tuyến. 

### Tại sao nó hoạt động 

Mọi thành viên không bao giờ lên mạng sẽ giữ trật tự tương đối của họ từ`a`. Vì tất cả các thành viên trực tuyến đều kết thúc trước tất cả các thành viên không bao giờ trực tuyến nên các thành viên không bao giờ trực tuyến phải tạo thành một hậu tố`b`. 

Một hậu tố của`b`duy trì trật tự tương đối từ`a`chính xác khi các vị trí tương ứng trong`a`đang gia tăng nghiêm trọng. Bất kỳ hậu tố nào dài hơn sẽ vi phạm điều kiện này và không thể bao gồm toàn bộ các thành viên không bao giờ trực tuyến. 

Ngược lại, nếu một hậu tố tăng lên, chúng ta có thể nhận ra hoán vị bằng cách khai báo tất cả các thành viên trước hậu tố đó là trực tuyến. Xử lý các thành viên trực tuyến đó từ phải sang trái theo thứ tự của họ trong`b`, di chuyển từng cái về phía trước một lần. Điều này tạo ra chính xác tiền tố được yêu cầu trong khi vẫn để hậu tố theo thứ tự tương đối ban đầu. 

Do đó, hậu tố tăng dài nhất sẽ cho số lượng thành viên không bao giờ trực tuyến tối đa có thể, giúp giảm thiểu số lượng thành viên trực tuyến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    answers = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos = [0] * (n + 1)

        for i, x in enumerate(a):
            pos[x] = i

        s = n - 1

        while s > 0 and pos[b[s - 1]] < pos[b[s]]:
            s -= 1

        answers.append(str(s))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```các`pos`mảng chuyển đổi các câu hỏi có thứ tự tương đối thành các phép so sánh số nguyên đơn giản. Thay vì liên tục tìm kiếm các thành viên bên trong`a`, chúng tôi có thể kiểm tra việc bảo quản đơn hàng trong thời gian liên tục. 

Biến`s`theo dõi sự bắt đầu của hậu tố hợp lệ dài nhất. Chúng ta bắt đầu ở cuối vì mọi hậu tố có độ dài bằng 1 sẽ tự động tăng lên. 

điều kiện```
pos[b[s - 1]] < pos[b[s]]
```kiểm tra xem việc mở rộng hậu tố một vị trí sang trái có còn giữ nguyên thứ tự từ`a`. 

Khi điều kiện không thành công, chúng tôi đã tìm thấy hậu tố không bao giờ trực tuyến lớn nhất có thể. Câu trả lời chính xác là số phần tử đứng trước nó, tức là`s`. 

Không cần mô phỏng danh sách. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
a = [1, 4, 2, 5, 3]
b = [4, 5, 1, 2, 3]
```Vị trí trong`a`: 

| Thành viên | Vị trí | 
| --- | --- | 
| 1 | 0 | 
| 4 | 1 | 
| 2 | 2 | 
| 5 | 3 | 
| 3 | 4 | 

Trình tự được chuyển đổi:```
[1, 3, 0, 2, 4]
```| s | So sánh | Kết quả | 
| --- | --- | --- | 
| 4 | 2 < 4 | tiếp tục | 
| 3 | 0 < 2 | tiếp tục | 
| 2 | 3 < 0 | sai | 

Hậu tố tăng dài nhất bắt đầu tại chỉ mục`2`. 

| Số lượng | Giá trị | 
| --- | --- | 
| Độ dài hậu tố | 3 | 
| Trả lời | 2 | 

Đầu ra:```
2
```Điều này chứng tỏ các thành viên`1, 2, 3`có thể không bao giờ trực tuyến trong khi`4`Và`5`đủ để giải thích sự thay đổi 

### Ví dụ 2```
a = [1, 2, 3, 4, 5, 6]
b = [1, 2, 3, 4, 5, 6]
```Trình tự được chuyển đổi:```
[0, 1, 2, 3, 4, 5]
```| s | So sánh | Kết quả | 
| --- | --- | --- | 
| 5 | 4 < 5 | tiếp tục | 
| 4 | 3 < 4 | tiếp tục | 
| 3 | 2 < 3 | tiếp tục | 
| 2 | 1 < 2 | tiếp tục | 
| 1 | 0 < 1 | tiếp tục | 

Hậu tố mở rộng ra toàn bộ mảng. 

| Số lượng | Giá trị | 
| --- | --- | 
| Độ dài hậu tố | 6 | 
| Trả lời | 0 | 

Đầu ra:```
0
```Điều này xác nhận rằng không có sự kiện trực tuyến nào được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để xây dựng vị trí và một lượt quét ngược | 
| Không gian | O(n) | Mảng vị trí | 

Trên tất cả các trường hợp thử nghiệm, tổng của`n`nhiều nhất là`10^5`. Thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi phần tử, vì vậy nó phù hợp thoải mái trong giới hạn thời gian và sử dụng rất ít bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos = [0] * (n + 1)

        for i, x in enumerate(a):
            pos[x] = i

        s = n - 1
        while s > 0 and pos[b[s - 1]] < pos[b[s]]:
            s -= 1

        out.append(str(s))

    return "\n".join(out)

# provided sample
assert run(
"""4
5
1 4 2 5 3
4 5 1 2 3
6
1 2 3 4 5 6
1 2 3 4 5 6
8
8 2 4 7 1 6 5 3
5 6 1 4 8 2 7 3
1
1
1
"""
) == """2
0
4
0"""

# minimum size
assert run(
"""1
1
1
1
"""
) == "0"

# single move to front
assert run(
"""1
3
1 2 3
2 1 3
"""
) == "1"

# entire array reversed
assert run(
"""1
5
1 2 3 4 5
5 4 3 2 1
"""
) == "4"

# suffix of length one only
assert run(
"""1
4
1 2 3 4
3 4 2 1
"""
) == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n = 1`|`0`| Trường hợp nhỏ nhất có thể | 
|`[1,2,3] -> [2,1,3]`|`1`| Thay đổi vị trí không ngụ ý hoạt động trực tuyến | 
| Hoán vị ngược |`4`| Hậu tố tăng dài nhất có thể có độ dài bằng một | 
|`[1,2,3,4] -> [3,4,2,1]`|`3`| Phát hiện ranh giới khi quét ngược | 

## Vỏ cạnh 

Hãy xem xét trường hợp đã được sắp xếp:```
1
4
1 2 3 4
1 2 3 4
```Chuỗi vị trí được biến đổi là:```
[0, 1, 2, 3]
```Mọi so sánh liền kề đều tăng lên, do đó hậu tố sẽ mở rộng ra toàn bộ mảng. Thuật toán trả về`0`, xác định chính xác rằng không ai phải truy cập trực tuyến. 

Bây giờ hãy xem xét trường hợp chỉ có một thành viên trực tuyến:```
1
3
1 2 3
2 1 3
```Trình tự được biến đổi là:```
[1, 0, 2]
```Quét từ cuối:```
0 < 2   -> valid
1 < 0   -> invalid
```Hậu tố tăng dài nhất bắt đầu tại chỉ mục`1`, vậy câu trả lời là`1`. Thành viên`2`trực tuyến và di chuyển lên phía trước, trong khi`1`Và`3`vẫn ngoại tuyến. 

Cuối cùng, hãy xem xét một thứ tự xáo trộn cao:```
1
5
1 2 3 4 5
5 4 3 2 1
```Trình tự được biến đổi là:```
[4, 3, 2, 1, 0]
```Phép so sánh đầu tiên từ bên phải đã thất bại, vì vậy hậu tố tăng dài nhất có độ dài`1`. Thuật toán trả về`4`, nghĩa là bốn thành viên phải thuộc về nhóm trực tuyến. Đây là mức tối thiểu có thể vì không có hậu tố nào lớn hơn giữ nguyên thứ tự tương đối ban đầu.
