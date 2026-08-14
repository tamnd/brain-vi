---
title: "CF 102323A - Đếm nguyên âm"
description: "Nhiệm vụ là kiểm tra một số tên và quyết định xem mỗi tên có chứa nhiều nguyên âm hơn phụ âm hay không. Các nguyên âm duy nhất là năm chữ cái viết thường a, e, i, o và u. Mỗi chữ cái viết thường khác là một phụ âm."
date: "2026-08-13T04:07:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102323
codeforces_index: "A"
codeforces_contest_name: "UCF Locals 2014"
rating: 0
weight: 102323
solve_time_s: 47
verified: true
draft: false
---

[CF 102323A - Số nguyên âm](https://codeforces.com/problemset/problem/102323/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là kiểm tra một số tên và quyết định xem mỗi tên có chứa nhiều nguyên âm hơn phụ âm hay không. Các nguyên âm duy nhất là năm chữ cái viết thường`a`,`e`,`i`,`o`, Và`u`. Mỗi chữ cái viết thường khác là một phụ âm. 

Đối với mỗi tên, chương trình phải sao chép tên chính xác như đã đọc, sau đó in`1`nếu số nguyên âm của nó lớn hơn số phụ âm của nó và`0`nếu không thì. Tuyên bố ban đầu của Cuộc thi lập trình địa phương UCF chỉ rõ rằng có`n`tên, mỗi tên có từ 1 đến 20 chữ cái viết thường và kết quả đầu ra bao gồm tên gốc theo sau là quyết định tương ứng. 

Độ dài tên tối đa chỉ là 20, do đó, ngay cả một lần quét đơn giản cũng đủ nhanh. Quét tuyến tính thực hiện tối đa 20 lần kiểm tra ký tự cho mỗi tên. Với cách thực hiện tự nhiên là kiểm tra một ký tự dựa trên tất cả năm nguyên âm có thể có, tức là tối đa 100 so sánh đơn giản cho mỗi tên. Ngay cả khi số lượng tên lớn, không có lý do gì để xem xét các thuật toán bậc hai hoặc hàm mũ ở đây. 

Sự so sánh phải chặt chẽ. Ví dụ, đầu vào```
1
ab
```chứa một nguyên âm và một phụ âm, vì vậy kết quả đúng là```
ab
0
```Việc thực hiện bất cẩn bằng cách sử dụng`vowels >= consonants`sẽ in sai`1`. 

Một trường hợp cạnh khác là tên không chứa nguyên âm. Vì```
1
bcdf
```số nguyên âm bằng 0 và số phụ âm là 4, do đó kết quả đầu ra là```
bcdf
0
```Một triển khai khởi tạo câu trả lời cho`1`và chỉ thay đổi nó sau khi tìm thấy nguyên âm có thể mắc sai lầm trong trường hợp này. 

Cực đoan ngược lại cũng có vấn đề. Vì```
1
aeiou
```cả năm ký tự đều là nguyên âm nên kết quả đúng là```
aeiou
1
```Chương trình sẽ đếm mọi lần xuất hiện chứ không chỉ xác định xem tên có chứa ít nhất một nguyên âm hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra từng ký tự trong mỗi tên và xác định xem nó có phải là một trong`a`,`e`,`i`,`o`, hoặc`u`. Chúng tôi duy trì bộ đếm nguyên âm và tăng nó bất cứ khi nào ký tự hiện tại là nguyên âm. Vì độ dài tên tối đa là 20 nên trường hợp xấu nhất khi so sánh rõ ràng mọi ký tự với tất cả năm nguyên âm là`5 * 20 = 100`so sánh ký tự cho một tên. Do đó tổng công việc là`O(5n)`, điều này đơn giản hóa thành`O(n)`cho một cái tên dài`n`. Cách tiếp cận này đã phù hợp với giới hạn một giây và giới hạn bộ nhớ 256 MB do cuộc thi chỉ định. 

Một phiên bản gọn gàng hơn một chút nhận thấy rằng tư cách thành viên trong bộ năm nguyên âm cố định có thể được biểu thị trực tiếp bằng một chuỗi chẳng hạn như`"aeiou"`. Sau đó`ch in "aeiou"`trả lời xem ký tự hiện tại có phải là nguyên âm hay không trong khi quá trình quét vẫn giữ nguyên tuyến tính. Cấu trúc quan trọng là mỗi nhân vật đều đóng góp độc lập cho câu trả lời. Không có sự tương tác giữa các ký tự lân cận, vì vậy không có lý do gì để xây dựng chuỗi con, sắp xếp tên hoặc thực hiện tìm kiếm lặp lại. 

Phiên bản brute-force và phiên bản tối ưu có cùng độ phức tạp tiệm cận vì bảng chữ cái của các nguyên âm có thể có kích thước không đổi. Việc tối ưu hóa chủ yếu là giảm bớt công việc liên tục không cần thiết và thể hiện rõ ràng hơn hoạt động cơ bản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(5n) = O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Thực sự không có thuật toán brute-force nào quá chậm theo các ràng buộc đã cho. Ngay cả phiên bản so sánh năm cũng chỉ thực hiện 100 so sánh nguyên âm cho tên dài nhất có thể. Gọi nó là quá chậm sẽ phóng đại những hạn chế. Việc kiểm tra tư cách thành viên một lần được ưu tiên hơn vì nó trực tiếp mô hình hóa vấn đề và tránh hệ số năm không cần thiết. 

## Hướng dẫn thuật toán 

1. Đọc số tên,`t`, vì đầu vào chứa một số trường hợp độc lập. 
2. Với mỗi tên, khởi tạo`vowels`về không. Bộ đếm thể hiện chính xác có bao nhiêu ký tự được xử lý cho đến nay thuộc về bộ nguyên âm. 
3. Quét tên từ trái sang phải. Với mỗi ký tự, hãy kiểm tra xem nó có thuộc về`"aeiou"`. Nếu có thì tăng`vowels`; nếu không thì giữ nguyên bộ đếm vì ký tự đó là phụ âm. 
4. Sau khi quét tên đầy đủ, số phụ âm của nó là`len(name) - vowels`. So sánh hai số này xác định kết quả cần thiết. 
5. Viết in tên gốc trước, sau đó là`1`khi`vowels > len(name) - vowels`, Và`0`nếu không thì. Sự nghiêm khắc`>`so sánh xử lý chính xác trường hợp đếm bằng nhau. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của tên,`vowels`chính xác là số ký tự nguyên âm trong tiền tố đó. Mỗi ký tự được phân loại một lần, vì vậy khi quá trình quét kết thúc,`vowels`bằng tổng số nguyên âm trong toàn bộ tên. Vì mỗi ký tự đều là nguyên âm hoặc phụ âm nên các ký tự còn lại`len(name) - vowels`các ký tự chính xác là phụ âm. Do đó, so sánh cuối cùng in`1`chính xác khi số lượng nguyên âm lớn hơn số lượng phụ âm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        name = input().strip()

        vowels = 0
        for ch in name:
            if ch in "aeiou":
                vowels += 1

        consonants = len(name) - vowels

        print(name)
        print(1 if vowels > consonants else 0)

if __name__ == "__main__":
    solve()
```Dòng đầu tiên đưa ra số lượng tên độc lập, do đó vòng lặp bên ngoài xử lý chính xác`t`trường hợp. Cuộc gọi tới`strip()`xóa dòng mới được tạo bởi`input()`trong khi vẫn giữ nguyên các chữ cái viết thường thực tế của tên. 

Vòng lặp bên trong thực hiện quét từ Hướng dẫn thuật toán bước 3. Biểu thức`ch in "aeiou"`là đủ vì câu lệnh giới hạn tên ở chữ cái viết thường. Không cần phải xử lý các nguyên âm viết hoa hoặc dấu câu. 

Sau khi đếm các nguyên âm, việc trừ độ dài tên sẽ cho ra số lượng phụ âm mà không cần quét lần thứ hai. Sự so sánh cuối cùng sử dụng`>`còn hơn là`>=`, đây là điều kiện biên giúp phân biệt rõ ràng nhiều nguyên âm hơn với một số bằng nhau. 

Không có vấn đề tràn số nguyên trong Python và thuật toán chỉ lưu trữ tên hiện tại và một bộ đếm số nguyên. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên chứa bốn tên:```
4
ali
arup
travis
orooji
```Vì`ali`, cuộc quét gặp phải`a`như một nguyên âm,`l`như một phụ âm và`i`như một nguyên âm. Số kết quả là hai nguyên âm và một phụ âm. 

| Nhân vật | Nguyên âm | Phụ âm | 
| --- | --- | --- | 
|`a`| 1 | 0 | 
|`l`| 1 | 1 | 
|`i`| 2 | 1 | 

Kết quả là`1`. Vì`arup`, các nguyên âm là`a`Và`u`, trong khi`r`Và`p`là phụ âm, chia đều và do đó`0`. Vì`travis`, có hai nguyên âm và bốn phụ âm, cho`0`. Vì`orooji`, có bốn nguyên âm và hai phụ âm, cho`1`. 

Đầu ra hoàn chỉnh là:```
ali
1
arup
0
travis
0
orooji
1
```Mẫu thứ hai có thể được xây dựng để thực hiện ranh giới đếm bằng nhau:```
3
a
bc
aeiou
```Dấu vết là: 

| Tên | Nguyên âm | Phụ âm | Kết quả | 
| --- | --- | --- | --- | 
|`a`| 1 | 0 | 1 | 
|`bc`| 0 | 2 | 0 | 
|`aeiou`| 5 | 0 | 1 | 

Trường hợp đầu tiên thể hiện tên pháp lý nhỏ nhất. Cái thứ hai thể hiện một tên không chứa nguyên âm. Phần thứ ba chứng minh rằng mọi lần xuất hiện của nguyên âm đều được tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) mỗi tên | Mỗi ký tự của một tên có độ dài`L`được kiểm tra một lần và bộ nguyên âm có kích thước không đổi. | 
| Không gian | O(1) phụ trợ | Chỉ cần bộ đếm nguyên âm và một vài biến vô hướng ngoài chính chuỗi đầu vào. | 

Với`L <= 20`, mỗi tên yêu cầu tối đa 20 lần lặp. Ngay cả việc so sánh nguyên âm năm chiều rõ ràng cũng chỉ yêu cầu so sánh 100 ký tự cho tên lớn nhất có thể, do đó giải pháp này thấp hơn nhiều so với giới hạn thời gian một giây có sẵn. Việc sử dụng bộ nhớ cũng không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_text(inp: str) -> str:
    data = io.StringIO(inp)
    t = int(data.readline())
    out = []

    for _ in range(t):
        name = data.readline().strip()

        vowels = 0
        for ch in name:
            if ch in "aeiou":
                vowels += 1

        consonants = len(name) - vowels

        out.append(name)
        out.append("1" if vowels > consonants else "0")

    return "\n".join(out) + "\n"

# Provided sample
assert solve_text(
    """4
ali
arup
travis
orooji
"""
) == """ali
1
arup
0
travis
0
orooji
1
""", "provided sample"

# Minimum-size input
assert solve_text(
    """1
a
"""
) == """a
1
""", "single vowel"

# No vowels
assert solve_text(
    """1
bcdf
"""
) == """bcdf
0
""", "no vowels"

# Equal number of vowels and consonants
assert solve_text(
    """1
ab
"""
) == """ab
0
""", "equal counts"

# Maximum-size name, all vowels
assert solve_text(
    """1
aaaaaaaaaaaaaaaaaaaa
"""
) == """aaaaaaaaaaaaaaaaaaaa
1
""", "maximum length"

# Several cases, including all consonants and mixed vowels
assert solve_text(
    """4
z
ae
baba
aeiouaeiou
"""
) == """z
0
ae
1
baba
1
aeiouaeiou
1
""", "mixed edge cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`a`theo sau là`1`| Đầu vào có kích thước tối thiểu và tên chỉ có nguyên âm | 
|`bcdf`|`bcdf`theo sau là`0`| Trường hợp nguyên âm không | 
|`ab`|`ab`theo sau là`0`| Số nguyên âm và phụ âm bằng nhau | 
|`aaaaaaaaaaaaaaaaaaaa`| Tên theo sau là`1`| Độ dài tối đa được phép và các giá trị hoàn toàn bằng nhau | 
| Nhiều tên hỗn hợp | tương ứng`1`hoặc`0`giá trị | Xử lý độc lập nhiều trường hợp thử nghiệm và đếm các nguyên âm lặp lại | 

## Vỏ cạnh 

Đối với ranh giới so sánh chặt chẽ, hãy xem xét đầu vào chính xác```
1
ab
```Thuật toán bắt đầu với`vowels = 0`. Nó đọc`a`, tăng bộ đếm lên`1`, sau đó đọc`b`và để nó ở`1`. Tên có độ dài hai, vì vậy`consonants = 2 - 1 = 1`. Từ`1 > 1`là sai, đầu ra là```
ab
0
```Điều này bộc lộ sai lầm phổ biến khi coi bình đẳng là một trường hợp thành công. 

Đối với một tên không có nguyên âm, hãy xem xét```
1
bcdf
```Mọi nhân vật đều thất bại trong bài kiểm tra tư cách thành viên, vì vậy`vowels`vẫn bằng không. Số lượng phụ âm trở thành`4 - 0 = 4`, và sự so sánh`0 > 4`là sai. Đầu ra là```
bcdf
0
```Bộ đếm không bao giờ cần hiệu chỉnh đặc biệt cho trường hợp này. 

Đối với tên chỉ chứa nguyên âm, hãy xem xét```
1
aeiou
```Mỗi ký tự trong số năm ký tự thuộc về bộ nguyên âm, do đó bộ đếm đạt tới năm ký tự. Số phụ âm là`5 - 5 = 0`, Và`5 > 0`là đúng. Đầu ra là```
aeiou
1
```Thuật toán đếm các lần xuất hiện lặp lại dưới dạng các nguyên âm riêng biệt, điều này là bắt buộc. Ví dụ: tên có độ dài tối đa```
1
aaaaaaaaaaaaaaaaaaaa
```chứa hai mươi lần xuất hiện nguyên âm. Bộ đếm đạt tới 20, số phụ âm bằng 0 và chương trình in`1`. 

Cuối cùng, đầu vào có thể chứa một số tên có đặc điểm hoàn toàn khác nhau. Mỗi lần lặp lại tạo ra một cái mới`vowels`truy cập, do đó thông tin từ một tên không thể rò rỉ sang tên tiếp theo. Đây là lý do tại sao một đầu vào như```
3
a
bc
ae
```sản xuất```
a
1
bc
0
ae
1
```Việc đặt lại tên cho mỗi tên là một phần của tính bất biến về tính chính xác: bộ đếm luôn chỉ mô tả tên hiện tại.
