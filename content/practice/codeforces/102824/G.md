---
title: "CF 102824G - Đá quý"
description: "Chúng tôi có một bộ sưu tập các tảng đá, trong đó mỗi tảng đá được mô tả bằng một chuỗi các chữ cái viết thường. Một chữ cái tượng trưng cho loại khoáng chất xuất hiện bên trong tảng đá đó."
date: "2026-07-26T22:39:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102824
codeforces_index: "G"
codeforces_contest_name: "mBIT Advanced November 2020"
rating: 0
weight: 102824
solve_time_s: 33
verified: true
draft: false
---

[CF 102824G - Đá quý](https://codeforces.com/problemset/problem/102824/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các tảng đá, trong đó mỗi tảng đá được mô tả bằng một chuỗi các chữ cái viết thường. Một chữ cái tượng trưng cho loại khoáng chất xuất hiện bên trong tảng đá đó. Cùng một khoáng chất có thể xuất hiện nhiều lần trong một tảng đá, nhưng để một khoáng chất được coi là đá quý thì nó phải xuất hiện trong mọi tảng đá ít nhất một lần. 

Nhiệm vụ là đếm xem có bao nhiêu loại khoáng sản khác nhau thỏa mãn điều kiện này. Dữ liệu đầu vào đưa ra số lượng đá, theo sau là mô tả khoáng chất của từng loại đá. Đầu ra là số chữ cái viết thường chung cho tất cả các mô tả. 

Hạn chế chính là bảng chữ cái được cố định: chỉ có 26 loại khoáng sản có thể có. Ngay cả khi số lượng đá hoặc độ dài của phần mô tả tăng lên thì số lượng câu trả lời có thể vẫn không đổi. Điều này có nghĩa là chúng ta nên tránh so sánh từng cặp đá hoặc quét liên tục tất cả các ký tự khi phương pháp theo dõi hiện diện hoặc tần số đơn giản là đủ. Một giải pháp phụ thuộc vào bình phương của số lượng đá sẽ trở thành công việc không cần thiết, trong khi giải pháp xử lý từng ký tự một lần lại đủ nhanh. 

Những trường hợp vi tế xuất phát từ sự khác biệt giữa việc xuất hiện trong một tảng đá và việc xuất hiện nhiều lần trong một tảng đá. Ví dụ: nếu đầu vào là:```
3
aaa
a
aa
```câu trả lời là:```
1
```Khoáng sản`a`là một loại đá quý vì nó xuất hiện ở cả ba loại đá. Một giải pháp bất cẩn đếm tổng số lần xuất hiện thay vì kiểm tra sự hiện diện trong mỗi tảng đá có thể đếm quá mức. 

Một trường hợp khác là khi không có khoáng chất nào được chia sẻ bởi tất cả các loại đá. Ví dụ:```
2
abc
def
```đầu ra đúng là:```
0
```Giải pháp bắt đầu bằng tất cả các chữ cái là hợp lệ và chỉ loại bỏ các chữ cái bị thiếu phải xử lý trường hợp này một cách chính xác. 

Trường hợp ranh giới cuối cùng là một tảng đá:```
1
xyz
```Đầu ra là:```
3
```Mọi khoáng chất xuất hiện trong tảng đá duy nhất đều tự động có mặt trong mọi tảng đá. Các giải pháp giả sử có ít nhất hai tảng đá có thể trả về số 0 không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra mọi khoáng chất có thể có và kiểm tra xem nó có xuất hiện trong mọi tảng đá hay không. Vì chỉ có 26 chữ cái nên điều này đã tốt hơn nhiều so với việc so sánh từng cặp chuỗi. Đối với mỗi chữ cái, chúng tôi quét tất cả các loại đá và đánh dấu nó là không hợp lệ nếu bất kỳ loại đá nào không chứa chữ cái đó. Công việc tỉ lệ thuận với số lượng đá nhân với 26, cộng với chi phí kiểm tra tư cách thành viên bên trong mỗi chuỗi. Nếu tư cách thành viên được thực hiện bằng cách tìm kiếm chuỗi mọi lúc, tổng công việc có thể đạt tới khoảng`26 * n * m`, Ở đâu`m`là chiều dài trung bình của đá. 

Một giải pháp tự nhiên hơn đến từ thực tế là kích thước bảng chữ cái rất nhỏ. Thay vì liên tục hỏi liệu một chữ cái có tồn tại trong một tảng đá hay không, chúng ta tóm tắt từng hòn đá một lần. Đối với mỗi tảng đá, chúng tôi ghi lại tập hợp các chữ cái xuất hiện trong đó. Sau đó, chúng tôi giao nhau các bộ này. Sau khi xử lý tất cả các loại đá, chỉ những chữ cái còn sót lại sau mỗi lần giao nhau mới là đá quý. 

Phương pháp vũ phu hoạt động vì chỉ có một số loại khoáng sản có thể có, nhưng nó lặp lại việc kiểm tra sự tồn tại giống nhau nhiều lần. Quan sát quan trọng là một tảng đá chỉ cung cấp thông tin về những chữ cái hiện diện chứ không phải chúng xuất hiện bao nhiêu lần. Việc thể hiện mỗi tảng đá như một tập hợp các chữ cái hiện tại sẽ loại bỏ những công việc lặp đi lặp lại không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26 * n * m) | O(1) | Được chấp nhận đối với đầu vào nhỏ nhưng công việc không cần thiết | 
| Tối ưu | O(tổng chiều dài của tất cả các chuỗi + 26 * n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đá quý có thể có tất cả 26 chữ cái viết thường. Lúc đầu, mọi khoáng chất vẫn có thể hiện diện trong mọi tảng đá. 
2. Đọc từng hòn đá một và xây dựng bộ chữ cái xuất hiện trong hòn đá đó. Sự xuất hiện lặp đi lặp lại của cùng một chữ cái không làm thay đổi tập hợp vì chỉ sự tồn tại mới quan trọng. 
3. Giao bộ ứng cử viên hiện tại với các chữ cái tìm thấy trong tảng đá hiện tại. Bất kỳ chữ cái nào bị thiếu trên tảng đá này không bao giờ có thể là đá quý, vì vậy nó phải được loại bỏ. 
4. Sau khi tất cả các loại đá đã được xử lý xong, hãy đếm các chữ cái còn lại. Đây chính xác là những khoáng chất xuất hiện trong mỗi tảng đá. 

Tại sao nó hoạt động: 

Bộ được duy trì luôn đại diện cho các khoáng chất đã xuất hiện trong mỗi loại đá được xử lý từ trước đến nay. Ban đầu, trước khi nhìn thấy bất kỳ tảng đá nào, tất cả các chữ cái đều thỏa mãn điều kiện này. Khi một tảng đá mới được xử lý, chỉ các chữ cái có trong tảng đá đó mới có thể tiếp tục thỏa mãn điều kiện, do đó phép toán giao nhau giữ nguyên tính bất biến. Sau viên đá cuối cùng, bộ này chứa chính xác các chữ cái có trong tất cả các loại đá, đây là định nghĩa về đá quý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)

    common = set("abcdefghijklmnopqrstuvwxyz")

    for _ in range(n):
        rock = input().strip()
        common &= set(rock)

    print(len(common))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tạo ra tập ứng cử viên chứa mọi chữ cái viết thường. Điều này tương ứng với trạng thái ban đầu khi chưa có tảng đá nào được kiểm tra. 

Đối với mỗi hòn đá,`set(rock)`chuyển đổi mô tả thành một tập hợp các khoáng chất xuất hiện ít nhất một lần. Thao tác này sẽ tự động loại bỏ các lần xuất hiện trùng lặp, đây chính xác là thông tin cần thiết cho sự cố này. Người điều hành giao lộ chỉ giữ lại các khoáng chất đã có sẵn và cũng có trong đá hiện tại. 

Kích thước cuối cùng của`common`là câu trả lời. Không có mối lo ngại nào về việc lập chỉ mục vì thuật toán hoạt động trực tiếp với các giá trị ký tự. Tràn số nguyên không phải là vấn đề đáng lo ngại vì giá trị lớn nhất được in ra chỉ là 26. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
3
abcdde
baccd
eeabg
```Dấu vết là: 

| Bước | Đá hiện tại | Chữ trong đá | Đá quý còn lại | 
| --- | --- | --- | --- | 
| Bắt đầu | không | tất cả các chữ cái | abcdefghijklmnopqrstuvwxyz | 
| 1 | abcdde | abcde | abcde | 
| 2 | baccd | abcd | abcd | 
| 3 | eeabg | abeg | ab | 

Câu trả lời là`2`. Dấu vết cho thấy chỉ còn lại những chữ cái còn sót lại ở mỗi giao lộ. 

Ví dụ khác:```
2
abc
def
```| Bước | Đá hiện tại | Chữ trong đá | Đá quý còn lại | 
| --- | --- | --- | --- | 
| Bắt đầu | không | tất cả các chữ cái | abcdefghijklmnopqrstuvwxyz | 
| 1 | abc | abc | abc | 
| 2 | chắc chắn | chắc chắn | trống | 

Câu trả lời là`0`. Điều này chứng tỏ trường hợp không có khoáng chất nào xuất hiện trong mọi tảng đá. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng chiều dài của tất cả các chuỗi) | Mỗi ký tự được xử lý một lần trong khi xây dựng các bộ | 
| Không gian | O(26) | Chỉ bộ đá quý có thể có hiện tại mới được lưu trữ | 

Thuật toán phụ thuộc tuyến tính vào tổng kích thước đầu vào và chỉ sử dụng bộ nhớ bổ sung không đổi. Vì bảng chữ cái không bao giờ vượt quá 26 chữ cái nên phương pháp này vẫn hiệu quả ngay cả khi mô tả về đá lớn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)
    common = set("abcdefghijklmnopqrstuvwxyz")
    for _ in range(n):
        common &= set(input().strip())
    print(len(common))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("3\nabcdde\nbaccd\neeabg\n") == "2\n", "sample 1"
assert run("2\nabc\ndef\n") == "0\n", "no common minerals"
assert run("1\nxyz\n") == "3\n", "single rock"
assert run("3\naaa\na\naa\n") == "1\n", "duplicate occurrences"
assert run("4\nabcdefghijklmnopqrstuvwxyz\nabcdefghijklmnopqrstuvwxyz\nabc\nabc\n") == "3\n", "large alphabet boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / abcdde / baccd / eeabg`|`2`| Trường hợp giao nhau tiêu chuẩn | 
|`2 / abc / def`|`0`| Không có khoáng sản chung | 
|`1 / xyz`|`3`| Hành vi đá đơn | 
|`3 / aaa / a / aa`|`1`| Chữ cái trùng lặp được tính một lần | 
| Bốn tảng đá chứa bảng chữ cái đầy đủ và các tập hợp con rút gọn |`3`| Xử lý chính xác các mô tả và ranh giới lớn hơn | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
3
aaa
a
aa
```Tảng đá đầu tiên chỉ rời đi`a`với tư cách là một ứng cử viên. Tảng đá thứ hai vẫn chứa`a`, và hòn đá thứ ba cũng chứa`a`, vậy tập cuối cùng có một phần tử. Đầu ra của thuật toán`1`bởi vì nó theo dõi sự hiện diện hơn là tần số. 

Đối với trường hợp cạnh thứ hai:```
2
abc
def
```Sau khi xử lý tảng đá đầu tiên, các ứng viên được`{a, b, c}`. Giao nhau với`{d, e, f}`loại bỏ mọi thứ, để lại một bộ trống. Thuật toán xuất ra chính xác`0`. 

Đối với trường hợp đá đơn:```
1
xyz
```Tập hợp ban đầu của tất cả các chữ cái được giao với`{x, y, z}`một lần. Tập hợp còn lại có ba phần tử, vì vậy đầu ra là`3`. Điều này có tác dụng vì mọi khoáng chất bên trong tảng đá duy nhất đều có mặt trong tất cả các loại đá. 

Đối với trường hợp các chữ cái lặp lại nhiều:```
3
aaaaab
bbbbba
ababab
```Tảng đá đầu tiên góp phần`{a, b}`, thứ hai cũng góp phần`{a, b}`, và đóng góp thứ ba`{a, b}`. Câu trả lời là`2`. Thuật toán bỏ qua các lần xuất hiện trùng lặp vì chúng không ảnh hưởng đến việc khoáng chất có tồn tại trong đá hay không.
