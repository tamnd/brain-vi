---
title: "CF 102203B - \u0421\u0440\u043e\u0447\u043d\u043e\u0435 \u0441\u043e\u043e\u0431\u0449\u0435\u043d\u0438\u0435"
description: "Chúng tôi có một chuỗi nhị phân mô tả những gì xảy ra trong mỗi nano giây nhận tin nhắn. Giá trị 0 có nghĩa là người nhận nhận được tin nhắn thành công tại thời điểm đó, trong khi giá trị 1 có nghĩa là hiện tượng nhiễu sẽ ngăn cản việc tiếp nhận."
date: "2026-08-18T00:36:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102203
codeforces_index: "B"
codeforces_contest_name: "\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e (8-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 102203
solve_time_s: 73
verified: true
draft: false
---

[CF 102203B - \u0421\u0440\u043e\u0447\u043d\u043e\u0435 \u0441\u043e\u043e\u0431\u0449\u0435\u043d\u0438\u0435](https://codeforces.com/problemset/problem/102203/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi nhị phân mô tả những gì xảy ra trong mỗi nano giây nhận tin nhắn. MỘT`0`có nghĩa là người nhận đã nhận được tin nhắn thành công tại thời điểm đó, trong khi`1`có nghĩa là nhiễu ngăn cản việc tiếp nhận. Vị trí đầu tiên và cuối cùng được đảm bảo`0`, do đó tin nhắn có phần đầu và phần cuối được nhận dạng. 

Đối với mỗi giá trị truy vấn`k`, máy thu được phép gặp nhiễu trong thời gian ít hơn`k`nano giây liên tiếp. Nếu một số khối nhiễu có chứa`k`hoặc nhiều hơn liên tiếp`1`s, tin nhắn không thể nhận được hoàn toàn. Chúng ta cần trả lời liệu toàn bộ chuỗi có thể thu được đối với từng giá trị đã cho của`k`. 

Đầu vào chứa độ dài chuỗi`n`, số lượng truy vấn`m`, chính chuỗi nhị phân, và sau đó`m`giá trị của`k`. Mỗi dòng đầu ra là`YES`khi mỗi khối liên tiếp của`1`s có độ dài nhỏ hơn`k`, Và`NO`nếu không thì. 

Chiều dài của chuỗi có thể đạt tới`10^6`, trong khi số lượng truy vấn có thể đạt tới`3 \cdot 10^5`. Điều này ngay lập tức loại trừ việc thực hiện quét toàn bộ chuỗi cho mọi truy vấn. Cách tiếp cận như vậy sẽ thực hiện được tới`10^6 * 3 * 10^5 = 3 * 10^11`kiểm tra ký tự, vượt xa những gì có thể phù hợp với giới hạn một giây. Về cơ bản, chúng ta cần trích xuất tất cả thông tin liên quan đến mọi truy vấn trong một lần truyền qua chuỗi. 

Các trường hợp cận biên xuất phát từ sự bất đẳng thức nghiêm ngặt trong điều kiện. Ví dụ, hãy xem xét```
2 2
00
1
2
```Không có vị trí can thiệp nào cả, vì vậy cả hai truy vấn phải tạo ra`YES`. Một giải pháp bất cẩn nhằm tìm kiếm một khoảng cách tích cực và coi việc chạy vắng mặt là độ dài mà người ta có thể từ chối một cách không chính xác`k = 1`. 

Một trường hợp ranh giới khác là```
4 3
0110
1
2
3
```Khối giao thoa dài nhất có chiều dài`2`, vì vậy đầu ra là```
NO
NO
YES
```Truy vấn thứ hai bị từ chối vì quy tắc yêu cầu thời lượng chạy phải nhỏ hơn`k`. Một giải pháp sử dụng`run <= k`sẽ in sai`YES`vì`k = 2`. 

Trường hợp hữu ích thứ ba là```
3 2
010
1
2
```Mỗi khối của`1`s có độ dài chính xác`1`, vậy câu trả lời là```
NO
YES
```Lại,`k = 1`không cho phép dù chỉ một nano giây ồn ào. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là xử lý mọi truy vấn một cách độc lập. Đối với một điều cụ thể`k`, quét chuỗi trong khi vẫn duy trì số chuỗi liên tiếp hiện tại`1`S. Nếu con số đó đạt tới`k`, câu trả lời là ngay lập tức`NO`; nếu quá trình quét kết thúc, câu trả lời là`YES`. Điều này đúng vì điều duy nhất có thể làm vô hiệu một truy vấn là một khối nhiễu liên tiếp đủ dài. 

Vấn đề là việc quét lặp đi lặp lại. Trong trường hợp xấu nhất, chuỗi có`10^6`nhân vật và có`3 \cdot 10^5`truy vấn. Nếu mọi truy vấn đều yêu cầu kiểm tra toàn bộ chuỗi thì trường hợp xấu nhất là khoảng`3 \cdot 10^11`hoạt động. Mặc dù mỗi lần quét riêng lẻ rất đơn giản nhưng việc lặp lại nhiều lần là không thể trong thời gian giới hạn. 

Điều quan trọng nhất là tất cả các truy vấn đều hỏi cùng một câu hỏi về cùng một chuỗi. Chúng tôi không cần biết độ dài của tất cả các lần chạy riêng biệt cho từng truy vấn. Cho phép`mx`là độ dài của khối liên tiếp dài nhất của`1`s trong toàn bộ chuỗi. Mỗi khối khác không dài hơn`mx`, vì vậy một truy vấn thành công chính xác khi`mx < k`. 

Do đó toàn bộ chuỗi có thể được tóm tắt bằng một số nguyên duy nhất. Chúng tôi tìm thấy`mx`một lần vào`O(n)`thời gian, thì mọi truy vấn sẽ được trả lời bằng một so sánh trong`O(1)`thời gian. 

Phương pháp vũ lực có hiệu quả vì nó trực tiếp kiểm tra mọi trở ngại có thể xảy ra đối với từng`k`, nhưng nó không thành công vì sự cản trở không thực sự phụ thuộc vào`k`. Quan sát cho thấy khối nhiễu dài nhất xác định hoàn toàn mọi truy vấn giúp giảm vấn đề từ việc quét liên tục chuỗi xuống một lần xử lý trước, sau đó là các truy vấn có thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nm)`|`O(1)`| Quá chậm | 
| Tối ưu |`O(n + m)`|`O(1)`bên cạnh việc lưu trữ đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, và chuỗi nhị phân`s`. Chuỗi chứa tất cả thông tin cần thiết để xác định khoảng thời gian nhiễu tồi tệ nhất. 
2. Quét`s`từ trái sang phải trong khi duy trì`cur`, độ dài của khối liên tiếp hiện tại của`1`S. Khi ký tự hiện tại là`1`, tăng`cur`. Khi đó`0`, khối nhiễu hiện tại đã kết thúc, vì vậy hãy đặt lại`cur`về không. 
3. Trong cùng một lần quét, hãy duy trì`mx`, giá trị lớn nhất`cur`đã từng đạt tới. Đây là khoảng thời gian dài nhất mà máy thu bị nhiễu liên tục. 
4. Đọc từng truy vấn`k`và so sánh nó với`mx`. In`YES`chính xác khi nào`mx < k`, bởi vì số lượng nano giây nhiễu liên tiếp được phép nhỏ hơn`k`. 
5. Thu thập các câu trả lời và in chúng lại với nhau. Xây dựng một chuỗi đầu ra sẽ tránh được các thao tác đầu ra lặp đi lặp lại không cần thiết khi có tới`3 \cdot 10^5`truy vấn. 

### Tại sao nó hoạt động 

giá trị`mx`là độ dài tối đa của bất kỳ khối liên tiếp nào của`1`S. Một truy vấn có giá trị`k`hợp lệ chính xác khi không có khối giao thoa nào có độ dài`k`hoặc hơn thế nữa. Từ`mx`là khối lớn nhất như vậy, tất cả các khối có chiều dài nhỏ hơn`k`chính xác khi nào`mx < k`. Quá trình tiền xử lý sẽ tính toán mức tối đa thực sự, do đó phép so sánh sẽ đưa ra câu trả lời chính xác cho mọi truy vấn một cách độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    s = input().strip()

    mx = 0
    cur = 0

    for ch in s:
        if ch == '1':
            cur += 1
            if cur > mx:
                mx = cur
        else:
            cur = 0

    ans = []

    for _ in range(m):
        k = int(input())
        if mx < k:
            ans.append("YES")
        else:
            ans.append("NO")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Quá trình quét sử dụng`cur`để thể hiện dòng nhiễu hiện tại. Chỉ tăng nó trên`1`và khởi động lại nó`0`làm cho mỗi lần chạy liên tiếp trở nên độc lập với những lần chạy khác. Bất cứ khi nào`cur`trở nên lớn hơn`mx`, chúng tôi cập nhật mức tối đa toàn cầu. 

Sự so sánh là có chủ ý`mx < k`, không`mx <= k`. Tuyên bố cho phép ít hơn`k`nano giây ồn ào liên tiếp. Để có chiều dài tối đa`3`,`k = 3`phải sản xuất`NO`, trong khi`k = 4`sản xuất`YES`. 

Các ký tự đầu tiên và cuối cùng được đảm bảo là`0`, nhưng thuật toán không dựa vào điều này để đảm bảo tính chính xác. Nó cũng sẽ xử lý chính xác một chuỗi chứa một chuỗi`1`s ở một trong hai ranh giới. 

Không có vấn đề tràn số nguyên vì bộ đếm lớn nhất chỉ`n`, nhiều nhất`10^6`, mà Python xử lý trực tiếp. Bản thân chuỗi đó được lưu trữ một lần và danh sách câu trả lời chứa nhiều nhất`3 \cdot 10^5`dây ngắn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Ví dụ về câu lệnh có một thông báo gồm bảy ký tự với ba vị trí nhiễu liên tiếp. Chúng ta có thể biểu diễn nó như`0111000`, với các truy vấn`1`,`4`, Và`3`. 

Quá trình quét hoạt động như sau. 

| Vị trí | Nhân vật |`cur`|`mx`| 
| --- | --- | --- | --- | 
| 1 |`0`| 0 | 0 | 
| 2 |`1`| 1 | 1 | 
| 3 |`1`| 2 | 2 | 
| 4 |`1`| 3 | 3 | 
| 5 |`0`| 0 | 3 | 
| 6 |`0`| 0 | 3 | 
| 7 |`0`| 0 | 3 | 

Lần chạy nhiễu tối đa cuối cùng là`mx = 3`. Vì`k = 1`,`3 < 1`là sai, vì vậy câu trả lời là`NO`. Vì`k = 4`,`3 < 4`là đúng, vậy câu trả lời là`YES`. Vì`k = 3`, bất đẳng thức lại sai, cho`NO`. 

Kết quả đầu ra là```
NO
YES
NO
```Ví dụ này chứng minh cụ thể tại sao việc so sánh phải nghiêm ngặt. 

### Ví dụ 2 

Hãy xem xét```
5 4
01010
1
2
3
100
```Có hai khối giao thoa riêng biệt, mỗi khối có chiều dài`1`. 

| Vị trí | Nhân vật |`cur`|`mx`| 
| --- | --- | --- | --- | 
| 1 |`0`| 0 | 0 | 
| 2 |`1`| 1 | 1 | 
| 3 |`0`| 0 | 1 | 
| 4 |`1`| 1 | 1 | 
| 5 |`0`| 0 | 1 | 

Giá trị cuối cùng là`mx = 1`. Các truy vấn được trả lời chỉ bằng giá trị này. 

|`k`| Tình trạng | Trả lời | 
| --- | --- | --- | 
| 1 |`1 < 1`|`NO`| 
| 2 |`1 < 2`|`YES`| 
| 3 |`1 < 3`|`YES`| 
| 100 |`1 < 100`|`YES`| 

Điều này chứng tỏ rằng số lượng khối nhiễu không quan trọng. Chỉ cái dài nhất mới ảnh hưởng đến câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + m)`| Chuỗi được quét một lần và mọi truy vấn đều mất thời gian không đổi. | 
| Không gian |`O(n + m)`| Chuỗi đầu vào và đầu ra thu thập được lưu trữ; bản thân thuật toán sử dụng`O(1)`trạng thái phụ trợ. | 

Với`n <= 10^6`Và`m <= 3 \cdot 10^5`, thuật toán thực hiện khoảng một triệu thao tác ký tự, sau đó là tối đa ba trăm nghìn phép so sánh. Điều này hoàn toàn thoải mái trong phạm vi dự định, không giống như`O(nm)`cách tiếp cận vũ phu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    s = input().strip()

    mx = 0
    cur = 0

    for ch in s:
        if ch == '1':
            cur += 1
            mx = max(mx, cur)
        else:
            cur = 0

    ans = []
    for _ in range(m):
        k = int(input())
        ans.append("YES" if mx < k else "NO")

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample, whose string is 0111000 according to the statement's description.
assert run(
    "7 3\n"
    "0111000\n"
    "1\n"
    "4\n"
    "3\n"
) == "NO\nYES\nNO", "sample 1"

# Minimum-size string, with no interference.
assert run(
    "2 3\n"
    "00\n"
    "1\n"
    "2\n"
    "100\n"
) == "YES\nYES\nYES", "minimum size and all zeros"

# One noisy position, testing the exact boundary mx == k.
assert run(
    "3 3\n"
    "010\n"
    "1\n"
    "2\n"
    "3\n"
) == "NO\nYES\nYES", "single noisy position"

# Several runs, with the longest one in the middle.
assert run(
    "9 4\n"
    "011011110\n"
    "1\n"
    "4\n"
    "5\n"
    "6\n"
) == "NO\nNO\nYES\nYES", "longest run is four"

# Large input, checking that the solution remains linear.
n = 1000000
s = "0" + "1" * 999998 + "0"
large_input = (
    f"{n} 3\n"
    f"{s}\n"
    "999998\n"
    "999999\n"
    "1000000\n"
)
assert run(large_input) == "NO\nYES\nYES", "maximum string length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3 / 00 / 1, 2, 100`|`YES / YES / YES`| Độ dài tối thiểu và không có bất kỳ khối ồn ào nào | 
|`3 3 / 010 / 1, 2, 3`|`NO / YES / YES`| Ranh giới chính xác khi đường chạy dài nhất có chiều dài`1`| 
|`9 4 / 011011110 / 1, 4, 5, 6`|`NO / NO / YES / YES`| Nhiều lần chạy và sự bất bình đẳng nghiêm ngặt tại`k = 4`| 
|`1000000 3 / 0 + 999998 ones + 0 / 999998, 999999, 1000000`|`NO / YES / YES`| Kích thước chuỗi tối đa và lần chạy lớn nhất có thể | 

## Vỏ cạnh 

Trường hợp toàn 0 được xử lý bằng cách để lại`mx`bằng 0 trong suốt quá trình quét. Đối với đầu vào```
2 2
00
1
2
```thuật toán không bao giờ tăng`cur`, Vì thế`mx = 0`. Cả hai`0 < 1`Và`0 < 2`là đúng, tạo ra`YES`cho cả hai truy vấn. Điều này tránh việc phát minh ra một khối ồn ào không tồn tại. 

Trường hợp ranh giới chính xác được xử lý bằng so sánh nghiêm ngặt. Vì```
4 3
0110
1
2
3
```quá trình quét tìm thấy`mx = 2`. Truy vấn`k = 1`thất bại,`k = 2`cũng thất bại vì`2`không thực sự nhỏ hơn`2`, Và`k = 3`thành công. Đầu ra là```
NO
NO
YES
```Trường hợp có một vị trí ồn ào duy nhất,```
3 2
010
1
2
```sản xuất`mx = 1`. Một giới hạn của`1`là không đủ vì điều kiện yêu cầu ít hơn một nano giây nhiễu liên tiếp, trong khi giới hạn`2`cho phép chạy dài một. Đầu ra là`NO`theo sau là`YES`. 

Nhiều khối nhiễu tách biệt không yêu cầu xử lý truy vấn riêng biệt. Vì```
9 4
011011110
1
4
5
6
```độ dài chạy là`2`,`1`, Và`4`, Vì thế`mx = 4`. Hai truy vấn đầu tiên không thành công, trong khi`4 < 5`Và`4 < 6`thành công. Thuật toán đạt được kết quả tương tự mà không lưu trữ độ dài chạy riêng lẻ, vì chỉ mức tối đa của chúng mới có thể ảnh hưởng đến bất kỳ truy vấn nào.
