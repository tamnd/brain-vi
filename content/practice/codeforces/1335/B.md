---
title: "CF 1335B - Xây dựng chuỗi"
description: "Đối với mỗi trường hợp thử nghiệm, chúng ta phải xây dựng một chuỗi chữ thường có độ dài n. Điều kiện là mọi đoạn liền kề có độ dài a phải chứa đúng b chữ cái khác nhau. Bất kỳ chuỗi hợp lệ nào đều được chấp nhận. Các con số mô tả tình trạng cửa sổ trượt. Nếu chúng ta nhìn vào vị trí 1..."
date: "2026-06-11T15:56:55+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1335
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 634 (Div. 3)"
rating: 900
weight: 1335
solve_time_s: 124
verified: false
draft: false
---

[CF 1335B - Xây dựng chuỗi](https://codeforces.com/problemset/problem/1335/B) 

**Xếp hạng:** 900 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi phải xây dựng một chuỗi có độ dài chữ thường`n`. Điều kiện là mọi đoạn liền kề có độ dài`a`phải chứa chính xác`b`chữ cái khác nhau. Bất kỳ chuỗi hợp lệ nào đều được chấp nhận. 

Các con số mô tả tình trạng cửa sổ trượt. Nếu chúng ta nhìn vào vị trí`1...a`, sau đó`2...a+1`, sau đó`3...a+2`, mọi cửa sổ như vậy phải có cùng số lượng ký tự riêng biệt. Chúng ta được tự do lựa chọn các chữ cái. 

Tổng các giá trị của`n`nhiều nhất là 2000, vì vậy ngay cả các thuật toán bậc hai cũng có thể phù hợp một cách thoải mái. Thách thức không phải là hiệu suất mà là khám phá một cấu trúc đơn giản. Vì bảng chữ cái chỉ có 26 chữ cái viết thường và`b ≤ a`, câu trả lời hợp lệ luôn tồn tại. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả định`a = 1`Và`b = 1`.```
1
6 1 1
```Mỗi chuỗi con có độ dài một chứa chính xác một ký tự riêng biệt, do đó, bất kỳ chuỗi nào cũng hoạt động. Một đầu ra đúng là```
aaaaaa
```Một giải pháp bất cẩn là cố gắng duyệt qua một số chữ cái vẫn có tác dụng, nhưng nó bỏ lỡ thực tế là chỉ cần một chữ cái. 

Một trường hợp thú vị khác là khi`a = b`.```
1
5 5 5
```Chuỗi con duy nhất có độ dài năm phải chứa năm ký tự riêng biệt. Một câu trả lời có thể là```
abcde
```Sử dụng ít hơn năm chữ cái sẽ vi phạm yêu cầu. 

Một ví dụ tinh tế hơn là```
1
7 5 3
```Một chuỗi như```
abcabca
```hoạt động vì mọi cửa sổ có độ dài năm chỉ chứa các chữ cái`a`,`b`, Và`c`. Nếu thay vào đó chúng ta sử dụng```
abcdeab
```cửa sổ đầu tiên sẽ chứa năm chữ cái riêng biệt, điều này không chính xác. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng xây dựng chuỗi ký tự theo từng ký tự. Đối với mỗi vị trí mới, chúng tôi có thể kiểm tra tất cả 26 chữ cái và kiểm tra xem mọi chuỗi con có độ dài bị ảnh hưởng có`a`vẫn có chính xác`b`những ký tự riêng biệt. Vì mỗi lần xác minh sẽ kiểm tra tới`a`nhân vật, khối lượng công việc tăng lên nhanh chóng. Trong các phiên bản lớn hơn của bài toán, cách tiếp cận này trở nên kém hấp dẫn vì bản thân không gian tìm kiếm là rất lớn. 

Quan sát quan trọng là chúng ta không cần phải thỏa mãn mọi cửa sổ một cách độc lập. Nếu chúng ta tạo một mẫu lặp lại có bộ ký tự đã chứa chính xác`b`các chữ cái khác nhau, sau đó mỗi độ dài-`a`window sẽ kế thừa cùng một bộ. 

Lấy cái đầu tiên`b`các chữ cái trong bảng chữ cái và lặp lại chúng theo chu kỳ:```
abcabcabc...
```Từ`b ≤ a`, bất kỳ khối có độ dài liên tiếp nào`a`chỉ nhìn thấy những cái đó`b`các chữ cái và mỗi chữ cái xuất hiện ở đâu đó bên trong khối vì dấu chấm chính xác`b`. Cửa sổ trượt tự động thỏa mãn điều kiện. 

Brute-force hoạt động vì nó kiểm tra yêu cầu một cách trực tiếp nhưng không thành công vì nó tìm kiếm một cách không cần thiết. Nhận xét rằng câu trả lời có thể tuần hoàn sẽ làm giảm bài toán thành một cách xây dựng đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ trong n | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi test case, hãy chuẩn bị test đầu tiên`b`chữ thường:`"abc..."`. 
2. Xây dựng câu trả lời theo từng vị trí một. 
3. Tại vị trí`i`, đặt ký tự có chỉ mục là`i mod b`. 

Điều này tạo ra một mẫu độ dài lặp đi lặp lại`b`. 
4. Tiếp tục cho đến khi dây đạt độ dài`n`. 
5. Xuất chuỗi đã tạo. 

### Tại sao nó hoạt động 

Khối lặp lại chứa chính xác`b`chữ cái riêng biệt. Vì thời kỳ của nó cũng là`b`, mỗi nhóm`b`các vị trí liên tiếp chứa tất cả các chữ cái này. Cửa sổ có chiều dài`a`ít nhất là dài bằng`b`, do đó mỗi cửa sổ như vậy chứa mỗi`b`chữ ít nhất một lần. Không có chữ cái bổ sung nào xuất hiện, bởi vì chỉ những chữ cái đầu tiên`b`ký tự được sử dụng. Do đó mọi chuỗi con có độ dài`a`chứa chính xác`b`những ký tự riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
ans = []

for _ in range(t):
    n, a, b = map(int, input().split())
    cur = []

    for i in range(n):
        cur.append(chr(ord('a') + (i % b)))

    ans.append("".join(cur))

print("\n".join(ans))
```Giải pháp xử lý các trường hợp thử nghiệm một cách độc lập. 

biểu hiện`i % b`xác định ký tự nào của khối lặp sẽ được sử dụng. Chức vụ`0`được`'a'`, chức vụ`1`được`'b'`, vân vân. Sau khi đạt đến`b`-chữ cái thứ, mô hình bắt đầu lại. 

Biến`a`không xuất hiện trong mã. Điều đó thoạt nghe có vẻ đáng ngờ, nhưng bằng chứng ở trên giải thích tại sao yêu cầu duy nhất là`b ≤ a`. Một khi thời gian là`b`, mọi cửa sổ có độ dài`a`tự động chứa tất cả`b`các chữ cái. 

Sử dụng danh sách và`"".join()`tránh nối chuỗi lặp lại và giữ cho việc triển khai hiệu quả. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp mẫu```
7 5 3
```Khối lặp lại là`"abc"`. 

| Vị trí tôi | tôi mod b | Nhân vật | Chuỗi cho đến nay | 
| --- | --- | --- | --- | 
| 0 | 0 | một | một | 
| 1 | 1 | b | ab | 
| 2 | 2 | c | abc | 
| 3 | 0 | một | abca | 
| 4 | 1 | b | abcab | 
| 5 | 2 | c | abcabc | 
| 6 | 0 | một | abcabca | 

Câu trả lời cuối cùng là```
abcabca
```các cửa sổ```
abcab
bcabc
cabca
```tất cả đều chứa chính xác ba chữ cái riêng biệt. 

Bây giờ hãy xem xét```
5 5 1
```| Vị trí tôi | tôi mod b | Nhân vật | Chuỗi cho đến nay | 
| --- | --- | --- | --- | 
| 0 | 0 | một | một | 
| 1 | 0 | một | aa | 
| 2 | 0 | một | aaa | 
| 3 | 0 | một | aaa | 
| 4 | 0 | một | aaa | 

Đầu ra trở thành```
aaaaa
```Cửa sổ dài 5 phút duy nhất chứa một chữ cái riêng biệt phù hợp với yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được tạo một lần | 
| Không gian | O(n) | Chuỗi câu trả lời được lưu trữ rõ ràng | 

Vì tổng tất cả các giá trị của`n`không vượt quá 2000 thì thời gian chạy rất nhỏ so với giới hạn. Việc sử dụng bộ nhớ cũng không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n, a, b = map(int, input().split())
        cur = []

        for i in range(n):
            cur.append(chr(ord('a') + (i % b)))

        ans.append("".join(cur))

    return "\n".join(ans)

# provided sample
assert run(
"""4
7 5 3
6 1 1
6 6 1
5 2 2
"""
) == "abcabca\naaaaaa\naaaaaa\nababa"

# minimum size
assert run(
"""1
1 1 1
"""
) == "a"

# all values equal
assert run(
"""1
5 5 5
"""
) == "abcde"

# single distinct letter
assert run(
"""1
8 4 1
"""
) == "aaaaaaaa"

# maximum length pattern
assert run(
"""1
10 10 3
"""
) == "abcabcabca"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`a`| Hạn chế tối thiểu | 
|`5 5 5`|`abcde`| Trường hợp tất cả các chữ cái phải khác biệt | 
|`8 4 1`|`aaaaaaaa`| Thư lặp đi lặp lại duy nhất | 
|`10 10 3`|`abcabcabca`| Lặp lại nhiều vị trí | 

## Vỏ cạnh 

Hãy xem xét```
1
6 1 1
```Thuật toán sử dụng`i mod 1`, luôn luôn bằng không. Mỗi vị trí đều nhận được`'a'`, sản xuất```
aaaaaa
```Mỗi chuỗi con có độ dài bằng một chứa một chữ cái riêng biệt, do đó điều kiện được thỏa mãn. 

Bây giờ hãy xem xét```
1
5 5 5
```Các chỉ số modulo năm là```
0, 1, 2, 3, 4
```mang lại```
abcde
```Cửa sổ duy nhất có độ dài năm chứa chính xác năm chữ cái khác nhau. 

Cuối cùng, hãy xem xét```
1
7 5 3
```Chuỗi được tạo ra là```
abcabca
```Cửa sổ của nó là```
abcab
bcabc
cabca
```Mỗi cửa sổ chứa cùng một bộ`{a, b, c}`. Vì không có chữ cái nào khác xuất hiện và không có chữ cái nào trong số ba chữ cái này biến mất khỏi bất kỳ cửa sổ nào, nên yêu cầu này được giữ nguyên trong toàn bộ chuỗi.
