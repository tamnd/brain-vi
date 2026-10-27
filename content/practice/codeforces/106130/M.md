---
title: "CF 106130M - \u5b57\u7b26\u6d88\u6d88\u4e50"
description: "Chúng tôi được cung cấp một chuỗi chữ thường. Trước khi trò chơi bắt đầu, chúng ta có thể sắp xếp lại các nhân vật theo bất kỳ thứ tự nào chúng ta muốn. Thao tác loại bỏ đơn sẽ chọn một khối liền kề có cùng ký tự và khối đó phải có độ dài ít nhất là 3."
date: "2026-06-19T19:52:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106130
codeforces_index: "M"
codeforces_contest_name: "GDUT 2025 Monthly competition"
rating: 0
weight: 106130
solve_time_s: 66
verified: true
draft: false
---

[CF 106130M - \u5b57\u7b26\u6d88\u6d88\u4e50](https://codeforces.com/problemset/problem/106130/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi chữ thường. Trước khi trò chơi bắt đầu, chúng ta có thể sắp xếp lại các nhân vật theo bất kỳ thứ tự nào chúng ta muốn. 

Thao tác loại bỏ đơn sẽ chọn một khối liền kề có cùng ký tự và khối đó phải có độ dài ít nhất là 3. Toàn bộ khối biến mất và các phần còn lại của chuỗi được nối lại. Chúng tôi có thể biểu diễn nhiều nhất`k`những hoạt động như vậy. 

Nhiệm vụ là tìm độ dài cuối cùng tối thiểu có thể có của chuỗi. 

Chi tiết quan trọng là quyền tự do sắp xếp lại chuỗi trước khi xảy ra bất kỳ sự loại bỏ nào. Khi thứ tự không còn cố định nữa thì vấn đề không còn nằm ở vị trí bên trong chuỗi gốc nữa. Điều quan trọng duy nhất là mỗi nhân vật xuất hiện bao nhiêu lần. 

Độ dài chuỗi có thể lớn bằng`10^5`, do đó, bất kỳ thuật toán nào cố gắng mô phỏng các chuỗi sắp xếp lại hoặc loại bỏ trên chính chuỗi đó đều không cần thiết. Bảng chữ cái chỉ chứa 26 chữ cái viết thường, điều này gợi ý rõ ràng rằng giải pháp nên dựa trên tần số ký tự thay vì cấu trúc chuỗi. 

Một vài trường hợp đặc biệt rất dễ bị đánh giá sai. 

Coi như:```
n = 2, k = 100
s = aa
```Số lượng của`'a'`chỉ là 2. Không thể hình thành khối nào có độ dài ít nhất bằng 3, vì vậy câu trả lời là`2`, không`0`. 

Coi như:```
n = 6, k = 1
s = aaabbb
```Một cách tiếp cận bất cẩn có thể cho rằng chỉ một trong hai nhóm có thể bị loại bỏ sau khi sắp xếp lại, điều này đúng. Chúng tôi loại bỏ một trong hai`"aaa"`hoặc`"bbb"`, để lại chiều dài`3`. 

Coi như:```
n = 7, k = 5
s = aaaaabc
```Mặc dù`k`lớn, nhân vật`'a'`có thể được loại bỏ hoàn toàn chỉ bằng một thao tác vì tất cả năm bản sao có thể được đặt vào một khối`"aaaaa"`. Câu trả lời là`2`, không phải thứ gì đó đòi hỏi nhiều thao tác. 

## Phương pháp tiếp cận 

Một tư duy vũ phu sẽ là nghĩ về tất cả những sự sắp xếp lại có thể xảy ra và tất cả những mệnh lệnh loại bỏ có thể xảy ra. Ngay cả đối với độ dài chuỗi vừa phải, số lượng hoán vị là vô cùng lớn và sau mỗi lần loại bỏ, chuỗi lại thay đổi. Điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là việc sắp xếp lại sẽ loại bỏ tất cả các ràng buộc về vị trí. 

Giả sử một nhân vật xuất hiện`c`lần. 

Nếu như`c < 3`, ký tự đó không bao giờ có thể tạo thành một khối di động. Cho dù chúng ta sắp xếp lại chuỗi như thế nào đi chăng nữa thì ít nhất những chuỗi đó`c`các ký tự phải được giữ lại. 

Nếu như`c ≥ 3`, chúng ta có thể đặt tất cả các bản sao lại với nhau thành một khối liền kề và xóa toàn bộ khối đó chỉ bằng một thao tác. Không bao giờ có bất kỳ lợi ích nào khi thực hiện nhiều hơn một thao tác trên cùng một ký tự, bởi vì một thao tác đã xóa mọi lần xuất hiện của ký tự đó. 

Điều này biến vấn đề thành một vấn đề phân bổ nguồn lực đơn giản. 

Mỗi ký tự có tần số ít nhất là 3 sẽ trở thành một mục: 

- Chi phí: 1 lần phẫu thuật. 
- Gain: tần số của nó. 

Chúng tôi có ngân sách tối đa là`k`hoạt động và muốn tối đa hóa số lượng ký tự bị xóa. Chiến lược tối ưu là loại bỏ các ký tự có tần số lớn nhất trước tiên. 

Vì chỉ có 26 chữ cái viết thường nên chúng ta chỉ cần đếm tần số, thu thập tất cả các tần số ít nhất là 3, sắp xếp chúng theo thứ tự giảm dần và lấy giá trị lớn nhất.`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n + 26 log 26) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của mỗi chữ cái viết thường. 
2. Đối với mọi tần số`c`, kiểm tra xem`c ≥ 3`. 

Tần số nhỏ hơn 3 không bao giờ có thể bị loại bỏ, do đó nó không đóng góp gì cho bể bơi di động. 
3. Lưu trữ mọi tần số có thể tháo rời vào một danh sách. 

Mỗi tần số như vậy đại diện cho một ký tự có thể bị xóa hoàn toàn bằng chính xác một thao tác. 
4. Sắp xếp danh sách theo thứ tự giảm dần. 

Vì mỗi ký tự di động đều có số lượng thao tác như nhau, cụ thể là một, nên trước tiên chúng ta nên thực hiện các thao tác trên tần số lớn nhất. 
5. Lấy cái đầu tiên`min(k, len(list))`tần số và tính tổng chúng. 

Đây là số lượng ký tự tối đa có thể được loại bỏ. 
6. Đầu ra:```
n - removed_characters
```### Tại sao nó hoạt động 

Sau khi sắp xếp lại, các lần xuất hiện của cùng một ký tự luôn có thể được nhóm lại với nhau. 

Đối với một nhân vật xuất hiện`c ≥ 3`lần, việc đặt tất cả các bản sao liên tiếp sẽ tạo ra một khối có chiều dài có thể tháo rời`c`, do đó toàn bộ lớp ký tự có thể bị xóa trong một thao tác. Đối với một ký tự xuất hiện ít hơn 3 lần, không thể hình thành khối di động nào cả. 

Như vậy, mỗi ký tự độc lập thuộc một trong hai loại: có thể tháo rời hoàn toàn với giá 1, hoặc không thể tháo rời. Vấn đề trở thành lựa chọn nhiều nhất`k`các ký tự có thể tháo rời có tổng tần số càng lớn càng tốt. Vì tất cả chi phí đều bằng nhau nên việc chọn tần số lớn nhất là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    freq = [0] * 26
    for ch in s:
        freq[ord(ch) - ord('a')] += 1

    removable = [c for c in freq if c >= 3]
    removable.sort(reverse=True)

    removed = sum(removable[:k])
    print(n - removed)

solve()
```Phần đầu tiên đếm tần số ký tự. 

Danh sách`removable`chứa chính xác những ký tự mà toàn bộ tập hợp xuất hiện có thể bị xóa trong một thao tác. Các tần số dưới 3 bị bỏ qua vì chúng không bao giờ có thể tham gia vào quá trình loại bỏ hợp lệ. 

Sau khi sắp xếp theo thứ tự giảm dần, đầu tiên`k`các mục tương ứng với các hoạt động có lợi nhất. Tổng hợp chúng cho số lượng ký tự bị xóa tối đa. 

Câu trả lời cuối cùng là độ dài ban đầu trừ đi số ký tự bị loại bỏ. 

Một điểm tinh tế là`k`có thể lớn hơn nhiều so với số lượng ký tự có thể tháo rời. Việc cắt Python xử lý việc này một cách tự nhiên, vì vậy`removable[:k]`vẫn an toàn ngay cả khi`k`vượt quá độ dài danh sách. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
9 3
aaccbbbba
```Tần số ký tự: 

| Nhân vật | Tần số | 
| --- | --- | 
| một | 3 | 
| b | 4 | 
| c | 2 | 

Tần số có thể tháo rời: 

| Bước | Danh sách có thể tháo rời | 
| --- | --- | 
| Trước khi sắp xếp | [3, 4] | 
| Sau khi sắp xếp | [4, 3] | 

Lựa chọn: 

| k | Tần số được chọn | Đã xóa | 
| --- | --- | --- | 
| 3 | [4, 3] | 7 | 

Câu trả lời cuối cùng: 

| n | Đã xóa | Kết quả | 
| --- | --- | --- | 
| 9 | 7 | 2 | 

Ví dụ này cho thấy rằng một khi việc sắp xếp lại được cho phép, chúng ta chỉ cần loại bỏ toàn bộ`'b'`nhóm và toàn bộ`'a'`nhóm. 

### Mẫu 2 

đầu vào:```
5 1
aaaaa
```Tần số ký tự: 

| Nhân vật | Tần số | 
| --- | --- | 
| một | 5 | 

Tần số có thể tháo rời: 

| Bước | Danh sách có thể tháo rời | 
| --- | --- | 
| Trước khi sắp xếp | [5] | 
| Sau khi sắp xếp | [5] | 

Lựa chọn: 

| k | Tần số được chọn | Đã xóa | 
| --- | --- | --- | 
| 1 | [5] | 5 | 

Câu trả lời cuối cùng: 

| n | Đã xóa | Kết quả | 
| --- | --- | --- | 
| 5 | 5 | 0 | 

Điều này chứng tỏ rằng tất cả các bản sao của một ký tự có thể bị xóa chỉ bằng một thao tác khi tần số của nó ít nhất là 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 26 log 26) | Đếm tần số, sau đó sắp xếp tối đa 26 giá trị | 
| Không gian | O(26) | Mảng tần số và danh sách di động | 

Vì kích thước bảng chữ cái là cố định nên chi phí sắp xếp là không đổi. Thời gian chạy bị chi phối bởi việc quét một chuỗi, dễ dàng phù hợp với các ràng buộc đối với`n = 10^5`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, k = map(int, input().split())
    s = input().strip()

    freq = [0] * 26
    for ch in s:
        freq[ord(ch) - ord('a')] += 1

    removable = [c for c in freq if c >= 3]
    removable.sort(reverse=True)

    return str(n - sum(removable[:k]))

# provided samples
assert run("9 3\naaccbbbba\n") == "2", "sample 1"
assert run("5 1\naaaaa\n") == "0", "sample 2"

# custom cases
assert run("1 0\na\n") == "1", "minimum size"
assert run("2 100\naa\n") == "2", "cannot remove length 2 block"
assert run("6 1\naaabbb\n") == "3", "only one group removable"
assert run("8 4\nabacadae\n") == "4", "one removable character"

# large all-equal case
assert run(f"100000 1\n{'a'*100000}\n") == "0", "maximum size style test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 / a`|`1`| Đầu vào nhỏ nhất có thể | 
|`2 100 / aa`|`2`| Tần số dưới 3 không bao giờ có thể tháo rời | 
|`6 1 / aaabbb`|`3`| Loại bỏ giới hạn ngân sách hoạt động | 
|`8 4 / abacadae`|`4`| Chỉ có một ký tự đủ điều kiện để xóa | 
|`100000 1 / a...a`|`0`| Tần số lớn được xử lý chính xác | 

## Vỏ cạnh 

Hãy xem xét:```
2 100
aa
```Tần số của`'a'`là 2. Danh sách di động trống vì 2 nhỏ hơn 3.```
removable = []
removed = 0
answer = 2
```Ngay cả ngân sách hoạt động không giới hạn cũng không thể giúp được vì không thể tạo khối hợp lệ. 

Coi như:```
6 1
aaabbb
```Các tần số là`[3, 3]`. 

Sau khi sắp xếp:```
[3, 3]
```Chỉ có một thao tác khả dụng, vì vậy chúng tôi lấy tần số đầu tiên:```
removed = 3
answer = 6 - 3 = 3
```Điều này xác nhận rằng số lượng hoạt động, chứ không chỉ khả năng loại bỏ, đều quan trọng. 

Coi như:```
7 5
aaaaabc
```Các tần số là:```
a -> 5
b -> 1
c -> 1
```Danh sách có thể tháo rời là:```
[5]
```Mặc dù có năm thao tác nhưng chỉ có một loại ký tự có thể tháo rời.```
removed = 5
answer = 2
```Thuật toán xử lý chính xác tần số 5 dưới dạng một khối có thể tháo rời thay vì yêu cầu nhiều thao tác.
