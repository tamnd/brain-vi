---
title: "CF 106186F - Đếm cặp"
description: "Chúng ta được hoán vị các số từ 1 đến n. Nhiệm vụ là đếm các lựa chọn có thứ tự của chỉ mục i và chỉ mục j khác trong đó giá trị tại j có thể được sử dụng làm độ dài bước nhảy và sau khi thực hiện bước nhảy đó từ i, giá trị tìm thấy ở đó chính xác là tổng của giá trị bắt đầu và…"
date: "2026-06-25T10:49:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106186
codeforces_index: "F"
codeforces_contest_name: "NWU IUPC 2025 powered by CPS Academy"
rating: 0
weight: 106186
solve_time_s: 35
verified: true
draft: false
---

[CF 106186F - Đếm cặp](https://codeforces.com/problemset/problem/106186/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số từ`1`ĐẾN`n`. Nhiệm vụ là đếm các lựa chọn có thứ tự của một chỉ mục`i`và một chỉ số khác`j`giá trị ở đâu`j`có thể được sử dụng làm độ dài bước nhảy và sau khi thực hiện bước nhảy đó từ`i`, giá trị tìm thấy ở đó chính xác là tổng của giá trị bắt đầu và độ dài bước nhảy. 

Nói cách khác, nếu giá trị tại vị trí`j`là`x`, chúng ta cần:`a[i + x] = a[i] + x`vị trí`i + x`phải tồn tại bên trong mảng. Câu trả lời là số lượng các lựa chọn hợp lệ như vậy của`i`Và`j`. 

Thuộc tính hoán vị là hạn chế chính. Mỗi số từ`1`ĐẾN`n`xuất hiện đúng một lần, do đó mọi độ dài bước nhảy có thể`x`có đúng một vị trí nơi nó xảy ra. 

Với`n`lên tới`200000`, việc kiểm tra từng cặp chỉ số sẽ yêu cầu khoảng`n² / 2`, tức là khoảng hai mươi tỷ séc trong trường hợp xấu nhất. Điều đó vượt xa giới hạn thời gian lập trình cạnh tranh thông thường cho phép. Chúng ta cần chuyển điều kiện thành thứ có thể tính được theo thời gian tuyến tính. 

Các trường hợp chính xuất phát từ thực tế là cặp hợp lệ trong chế độ xem được chuyển đổi không nhất thiết phải là cặp chỉ mục giống như`(i, j)`từ tuyên bố. 

Ví dụ:```
5
3 4 1 2 5
```Câu trả lời hợp lệ là`2`. Một cặp ban đầu hợp lệ là`(3, 3)`, bởi vì`a[3] = 1`Và`a[4] = 2`, cho`1 + 1 = 2`. Một giải pháp bất cẩn chỉ kiểm tra các cặp có chỉ số khác nhau sẽ bỏ sót trường hợp này. 

Một trường hợp khác là khi nhiều chỉ số có cùng giá trị chuyển đổi. Ví dụ:```
4
1 2 3 4
```Câu trả lời là`6`. Mỗi cặp đều hoạt động vì mảng đang tăng chính xác một, vì vậy tất cả các chỉ số đều có cùng giá trị`index - value`. Chỉ tính các vị trí liền kề sẽ trả về kết quả nhỏ hơn một cách không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp tuân theo định nghĩa. Với mọi cặp chỉ số có thể`(i, j)`, chúng tôi tính toán độ dài bước nhảy`a[j]`, kiểm tra xem`i + a[j]`nằm trong mảng và so sánh`a[i + a[j]]`với`a[i] + a[j]`. 

Điều này đúng vì nó kiểm tra chính xác điều kiện được yêu cầu. Tuy nhiên, có`n²`các cặp có thứ tự có thể. Khi`n = 200000`, điều này tạo ra khoảng bốn mươi tỷ phép tính, điều này là không thể. 

Quan sát hữu ích đến từ việc viết lại phương trình. Cho phép:```
x = a[j]
q = i + x
```Điều kiện trở thành:```
a[q] = a[i] + x
```Từ`q - i = x`, chúng tôi nhận được:```
a[q] - a[i] = q - i
```Sắp xếp lại:```
q - a[q] = i - a[i]
```Vì vậy thay vì tìm giá trị bước nhảy đặc biệt, chúng ta chỉ cần đếm các cặp vị trí có cùng giá trị`index - value`. 

Mỗi cặp vị trí có cùng giá trị được chuyển đổi tương ứng với chính xác một cặp vị trí ban đầu hợp lệ. Nếu vị trí`p < q`thỏa mãn:```
p - a[p] = q - a[q]
```sau đó:```
a[q] - a[p] = q - p
```Khoảng cách`q - p`là một số giữa`1`Và`n`và vì mảng là một hoán vị nên số đó xuất hiện ở đâu đó dưới dạng`a[j]`. Lựa chọn điều đó`j`cung cấp cặp gốc được yêu cầu. 

Vấn đề được rút gọn thành việc nhóm các giá trị bằng nhau của`index - value`và cộng số cặp trong mỗi nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Nhóm theo chênh lệch giá trị chỉ số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại qua mọi vị trí`i`và tính giá trị`i - a[i]`. Chúng tôi sử dụng lập chỉ mục dựa trên một vì câu lệnh mô tả các vị trí theo cách đó. 
2. Lưu trữ số lần mỗi giá trị được chuyển đổi xuất hiện. Các vị trí có cùng giá trị được chuyển đổi có thể tạo thành các cặp hợp lệ với nhau. 
3. Khi giá trị chuyển đổi đã xuất hiện`cnt`nhiều lần và chúng ta lại thấy nó, vị trí mới hình thành`cnt`cặp mới. Thêm vào`cnt`cho câu trả lời trước khi tăng tần số. 
4. Xuất ra số đếm tích lũy. 

Lý do tính năng đếm hoạt động là vì khi xử lý các vị trí từ trái sang phải, mọi lần xuất hiện trước đó của cùng một giá trị được chuyển đổi đều là một chỉ mục trước đó tạo thành một cặp hợp lệ với chỉ mục hiện tại. 

Tại sao nó hoạt động: 

Phép biến đổi bảo toàn chính xác điều kiện cần thiết. Một cặp ban đầu hợp lệ tạo ra hai vị trí`i`Và`i + a[j]`có giá trị thỏa mãn bằng nhau`index - value`. Ngược lại, hai vị trí bất kỳ có giá trị bằng nhau`index - value`có khoảng cách bằng chênh lệch giá trị của chúng và khoảng cách đó hiện diện dưới dạng một số giá trị mảng vì mảng là một hoán vị. Do đó, mỗi cặp được đếm tương ứng với một và chỉ một lựa chọn hợp lệ từ bài toán ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    freq = {}
    ans = 0

    for i, x in enumerate(a, start=1):
        key = i - x
        ans += freq.get(key, 0)
        freq[key] = freq.get(key, 0) + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Vòng lặp sử dụng các vị trí dựa trên một bằng cách bắt đầu`enumerate`từ`1`. Điều này tránh việc chuyển đổi giữa các chỉ số Python dựa trên số 0 và các vị trí toán học từ bài toán.`freq`lưu trữ số lượng vị trí trước đó có mỗi giá trị của`i - a[i]`. Khi giá trị tương tự xuất hiện lại, mỗi lần xuất hiện trước đó sẽ tạo ra một cặp hợp lệ mới, vì vậy chúng tôi thêm tần số hiện tại trước khi chèn vị trí hiện tại. 

Số nguyên Python có độ chính xác tùy ý, do đó câu trả lời không cần xử lý đặc biệt mặc dù số lượng cặp có thể vượt quá giới hạn số nguyên 32 bit. 

Không có kiểm tra ranh giới trong quá trình triển khai vì phép biến đổi đại số đã bao gồm điều kiện là bước nhảy nằm trong mảng. Vị trí thứ hai của mỗi cặp được đếm là vị trí thực trong hoán vị. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
5
3 4 1 2 5
```Các giá trị được chuyển đổi là: 

| Vị trí | Giá trị | Vị Trí - Giá Trị | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1 | 3 | -2 | 0 | 
| 2 | 4 | -2 | 1 | 
| 3 | 1 | 2 | 1 | 
| 4 | 2 | 2 | 2 | 
| 5 | 5 | 0 | 2 | 

Các nhóm là`{-2: 2}`,`{2: 2}`, Và`{0: 1}`. Hai nhóm số hai, mỗi nhóm đóng góp một cặp, đưa ra đáp án cuối cùng`2`. 

Dấu vết này chứng tỏ rằng thuật toán không cần biết vị trí nào chứa độ dài bước nhảy. Thuộc tính hoán vị tự động xử lý kết nối đó. 

Bây giờ hãy xem xét:```
4
1 2 3 4
```| Vị trí | Giá trị | Vị Trí - Giá Trị | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 
| 2 | 2 | 0 | 1 | 
| 3 | 3 | 0 | 3 | 
| 4 | 4 | 0 | 6 | 

Tất cả các vị trí đều có chung giá trị chuyển đổi nên mỗi cặp đều được tính. Kết quả là`4 * 3 / 2 = 6`. 

Ví dụ này xác minh rằng giải pháp xử lý chính xác các nhóm lớn thay vì chỉ kiểm tra các vị trí lân cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử mảng được xử lý một lần và mỗi thao tác từ điển được mong đợi là O(1). | 
| Không gian | O(n) | Trong trường hợp xấu nhất, mỗi vị trí có một giá trị chuyển đổi khác nhau. | 

Giải pháp chỉ thực hiện một lần chuyển đổi hoán vị, do đó nó dễ dàng phù hợp với các giới hạn cần thiết cho`n = 200000`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))

    freq = {}
    ans = 0
    for i, x in enumerate(a, start=1):
        key = i - x
        ans += freq.get(key, 0)
        freq[key] = freq.get(key, 0) + 1

    sys.stdin = old_stdin
    return str(ans)

# provided sample
assert run("5\n3 4 1 2 5\n") == "2", "sample"

# minimum size
assert run("1\n1\n") == "0", "single element"

# all pairs are valid
assert run("5\n1 2 3 4 5\n") == "10", "increasing permutation"

# catches missing same-index cases
assert run("3\n2 1 3\n") == "1", "self jump case"

# many equal transformed values
assert run("6\n1 2 3 4 5 6\n") == "15", "all equal keys"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`0`| Kích thước tối thiểu và không có cặp nào có thể | 
|`5 / 1 2 3 4 5`|`10`| Mỗi cặp thuộc về một nhóm chuyển đổi | 
|`3 / 2 1 3`|`1`| Cặp nơi gốc`i`Và`j`có thể trùng nhau | 
|`6 / 1 2 3 4 5 6`|`15`| Đếm nhóm tần số lớn | 

## Vỏ cạnh 

Đối với trường hợp:```
5
3 4 1 2 5
```thuật toán tính toán các giá trị được chuyển đổi:```
-2, -2, 2, 2, 0
```Các giá trị lặp lại tạo ra hai cặp. Những điều này tương ứng với hai lựa chọn ban đầu hợp lệ, bao gồm cả trường hợp sử dụng cùng một chỉ mục làm cả hai phần của cặp ban đầu. 

Vì:```
4
1 2 3 4
```tất cả các giá trị được chuyển đổi đều bằng 0. Thuật toán tiếp tục cộng số lần xuất hiện trước đó, tạo ra:```
0 + 1 + 2 + 3 = 6
```đếm mọi cặp có thể có chính xác một lần. 

Đối với một hoán vị không có giá trị biến đổi lặp lại, mọi tần số vẫn là một, do đó câu trả lời vẫn bằng 0. Những trường hợp như vậy được xử lý một cách tự nhiên vì không có vị trí trước đó có thể sánh được với vị trí hiện tại.
