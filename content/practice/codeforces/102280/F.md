---
title: "CF 102280F - \u041d\u0435\u043e\u0436\u0438\u0434\u0430\u043d\u043d\u0430\u044f \u0437\u0438\u043c\u0430"
description: "Mỗi dòng trong cuốn sổ chỉ có họ của người lái xe. Người lái xe viết họ một lần khi ra khỏi gara và một lần khi quay lại. Cuốn sổ không cho chúng ta biết sự kiện nào là sự ra đi và sự kiện nào là sự trở về."
date: "2026-08-13T15:59:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102280
codeforces_index: "F"
codeforces_contest_name: "2010, \u0422\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0430 \u0421\u0413\u0410\u0423 aka \u041a\u043e\u043d\u0442\u0435\u0441\u0442 \u043f\u0440\u043e \u043c\u0430\u0440\u0448\u0440\u0443\u0442\u043a\u0438"
rating: 0
weight: 102280
solve_time_s: 146
verified: true
draft: false
---

[CF 102280F - \u041d\u0435\u043e\u0436\u0438\u0434\u0430\u043d\u043d\u0430\u044f \u0437\u0438\u043c\u0430](https://codeforces.com/problemset/problem/102280/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi dòng trong cuốn sổ chỉ có họ của người lái xe. Người lái xe viết họ một lần khi ra khỏi gara và một lần khi quay lại. Cuốn sổ không cho chúng ta biết sự kiện nào là sự ra đi và sự kiện nào là sự trở về. 

Đối với người lái xe đã hoàn thành tất cả các chuyến đi, họ xuất hiện số lần chẵn. Người tài xế còn mắc kẹt ở đâu đó lại có một lần xuất phát không trùng khớp nên họ đó xuất hiện với số lần lẻ. Nhiệm vụ là tìm ra họ đó. Nếu không có họ phù hợp thì kết quả đầu ra được yêu cầu là`FAIL`. 

Ví dụ: nếu sổ ghi chép chứa```
Yakubov
Abramov
Yakubov
```sau đó`Yakubov`có hai bản ghi và`Abramov`có một cái, vậy`Abramov`là người lái xe đã không trở lại. 

Giá trị của`n`có thể đạt tới 150000 và họ có thể chứa tối đa 255 ký tự. Một thuật toán so sánh nhiều cặp bản ghi thì quá tốn kém. Với 150000 bản ghi, thuật toán bậc hai thực hiện gần như 

[ 
\frac{150000\cdot149999}{2}\khoảng 11,25\cdot10^9 
] 

so sánh, vượt xa những gì phù hợp với giới hạn hai giây. Chúng ta chỉ cần xử lý mỗi bản ghi một số lần không đổi, đưa ra giải pháp thời gian tuyến tính mong đợi. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai bất cẩn không thành công. Đầu vào nhỏ nhất có thể là một bản ghi:```
1
Petrov
```Câu trả lời là`Petrov`, bởi vì lần xuất hiện duy nhất đó không thể được ghép nối với một bản ghi khác. 

Họ lặp đi lặp lại cũng phải được xử lý chính xác. Vì```
5
Ivanov
Ivanov
Ivanov
Ivanov
Ivanov
```câu trả lời là`Ivanov`, bởi vì năm lần xuất hiện để lại một bản ghi chưa từng có. Việc triển khai chỉ tìm kiếm họ xuất hiện đúng một lần sẽ từ chối trường hợp này một cách không chính xác. 

Họ có thể dài tới 255 ký tự, do đó, họ phải được coi là một chuỗi tùy ý chứ không phải là một ký tự hoặc một giá trị số nhỏ. Ví dụ,```
1
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
```chứa một họ hợp lệ có độ dài tối đa và chuỗi chính xác đó phải được in. 

Cuối cùng, câu trả lời được xác định bằng tính chẵn lẻ chứ không phải theo thứ tự của các bản ghi. Trình tự```
Askerov
Shumacher
Askerov
Abalkin
Abalkin
```có ba cặp liên quan hoặc lần xuất hiện không trùng khớp bất kể những dòng đó xuất hiện ở đâu. Câu trả lời đúng là`Shumacher`. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là lấy từng họ và tìm kiếm một họ giống hệt khác có thể ghép với họ đó. Sau khi tìm thấy một cặp, cả hai bản ghi đều có thể bị xóa và bản ghi còn lại sẽ xác định người lái xe đã không quay lại. Điều này đúng vì mỗi chuyến đi hoàn thành đều đóng góp chính xác hai họ bằng nhau. 

Vấn đề là chi phí tìm kiếm. Trong trường hợp xấu nhất, chúng tôi có thể kiểm tra hầu hết mọi bản ghi khác đối với mọi bản ghi. Với`n = 150000`, điều này mang lại khoảng 11,25 tỷ so sánh. Mỗi sự so sánh tuy đơn giản nhưng khối lượng công việc đó lại vượt xa thời hạn. 

Quan sát hữu ích là thứ tự thực tế của các mục trong sổ ghi chép không quan trọng. Điều quan trọng là mỗi họ xuất hiện bao nhiêu lần. Mỗi trình điều khiển đã hoàn thành đều đóng góp một số chẵn, trong khi phần trả về bị thiếu sẽ thay đổi một số từ chẵn thành lẻ. 

Chúng ta có thể khai thác điều này trực tiếp bằng một bộ. Khi họ xuất hiện lần đầu tiên, hãy đặt họ vào bộ. Khi nó xuất hiện trở lại, hãy loại bỏ nó. Sau khi xử lý bất kỳ tiền tố nào của đầu vào, họ sẽ được đặt chính xác khi nó xuất hiện với số lần lẻ cho đến nay. Sau khi tất cả các bản ghi đã được xử lý, tập hợp này chứa chính xác các họ có tổng tần suất lẻ. 

Trong điều kiện đầu vào dự định, có một họ như vậy. Nếu tập hợp chứa chính xác một giá trị thì đó là câu trả lời. Nếu nó trống, tất cả các bản ghi đã được ghép nối và chúng tôi in`FAIL`. Nếu đầu vào không đúng định dạng tạo ra một số họ có tần suất lẻ, trả về`FAIL`cũng là hành vi an toàn vì không có trình điều khiển duy nhất để nhận dạng. 

Ý tưởng tương tự có thể được thực hiện với từ điển tần số, nhưng bộ chuyển đổi đơn giản hơn vì chúng ta không bao giờ cần số đếm chính xác. Chúng ta chỉ cần biết số hiện tại là số lẻ hay số chẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) dự kiến ​​| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và tạo một bộ trống gọi là`odd`. 
2. Xử lý từng họ một. Nếu họ không có trong`odd`, chèn nó. Nếu nó đã ở đó, hãy loại bỏ nó. Hai trường hợp tương ứng chính xác với việc thay đổi số lần xuất hiện của nó từ chẵn sang lẻ hoặc từ lẻ sang chẵn. 
3. Rốt cuộc`n`hồ sơ đã được xử lý, kiểm tra`odd`. Mỗi họ còn sót lại đều xuất hiện số lần lẻ. 
4. Nếu còn đúng một họ thì in ra. Họ đó có một lần xuất hiện chưa từng có nên tài xế của nó vẫn chưa quay lại. 
5. Nếu tập hợp trống, hãy in`FAIL`. Mỗi họ xuất hiện với số lần chẵn, vì vậy mỗi bản ghi sổ ghi chép có thể được ghép với một bản ghi giống hệt nhau. 
6. Nếu còn nhiều họ thì in thêm`FAIL`, vì đầu vào không xác định được trình điều khiển duy nhất chưa từng có. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bất kỳ số lượng bản ghi nào, họ sẽ thuộc về`odd`chính xác khi số lần xuất hiện được xử lý của nó là số lẻ. Ban đầu mọi số đếm đều bằng 0, do đó bất biến được giữ nguyên. Đọc họ sẽ thay đổi tính chẵn lẻ của nó một. Nếu số trước đó là số chẵn thì họ được chèn vào, nếu số lẻ thì họ sẽ bị xóa. Do đó, bất biến vẫn đúng sau mỗi bản ghi. 

Cuối cùng, mỗi người lái xe đã hoàn thành tất cả các chuyến đi đều có số bản ghi sổ chẵn và vắng mặt trong bộ. Người lái xe không quay lại có số bản ghi lẻ và vẫn còn trong bộ. Do đó, khi còn lại một họ thì đó chính xác là câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    odd = set()

    for _ in range(n):
        surname = input().strip()

        if surname in odd:
            odd.remove(surname)
        else:
            odd.add(surname)

    if len(odd) == 1:
        print(next(iter(odd)))
    else:
        print("FAIL")

if __name__ == "__main__":
    solve()
```bộ`odd`chỉ lưu trữ những họ có tần số hiện tại là số lẻ. Việc kiểm tra tư cách thành viên và việc chèn hoặc xóa dự kiến ​​là O(1), do đó việc xử lý một bản ghi sẽ mất thời gian dự kiến ​​không đổi. 

sử dụng`input().strip()`xóa dòng mới còn lại bởi`readline()`. Nó không làm thay đổi các chữ cái bên trong họ, vì vậy chữ hoa chữ thường vẫn có ý nghĩa quan trọng. Ví dụ,`Ivanov`Và`ivanov`là các chuỗi khác nhau và phải được coi là các trình điều khiển khác nhau. 

Không có số học số nguyên liên quan đến`n`ngoài bộ đếm vòng lặp, do đó việc tràn số nguyên không phải là vấn đề trong Python. Vòng lặp chạy chính xác`n`nhiều lần, điều này tránh được bất kỳ sự mơ hồ nào. 

điều kiện`len(odd) == 1`tốt hơn là chỉ đơn giản lấy một phần tử tùy ý. Cái sau sẽ ẩn đầu vào không đúng định dạng chứa nhiều họ có tần số lẻ. Trường hợp tập trống tương ứng trực tiếp với yêu cầu`FAIL`đầu ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên chứa ba bản ghi:```
3
Yakubov
Yakubov
Abramov
```Trạng thái của tập hợp thay đổi như sau. 

| Ghi lại | Họ | Hành động |`odd`sau khi ghi | 
| --- | --- | --- | --- | 
| 1 |`Yakubov`| chèn |`{Yakubov}`| 
| 2 |`Yakubov`| xóa |`{}`| 
| 3 |`Abramov`| chèn |`{Abramov}`|`Yakubov`xảy ra hai lần, vì vậy hai bản ghi của nó triệt tiêu lẫn nhau.`Abramov`xảy ra một lần và duy trì trong tập hợp, cho kết quả đầu ra`Abramov`. 

### Mẫu 2 

Mẫu thứ hai là```
7
Askerov
Shumacher
Askerov
Askerov
Shumacher
Abalkin
Abalkin
```Nhà nước phát triển như thế này. 

| Ghi lại | Họ | Hành động |`odd`sau khi ghi | 
| --- | --- | --- | --- | 
| 1 |`Askerov`| chèn |`{Askerov}`| 
| 2 |`Shumacher`| chèn |`{Askerov, Shumacher}`| 
| 3 |`Askerov`| xóa |`{Shumacher}`| 
| 4 |`Askerov`| chèn |`{Shumacher, Askerov}`| 
| 5 |`Shumacher`| xóa |`{Askerov}`| 
| 6 |`Abalkin`| chèn |`{Askerov, Abalkin}`| 
| 7 |`Abalkin`| xóa |`{Askerov}`|`Shumacher`xuất hiện hai lần và`Abalkin`xuất hiện hai lần.`Askerov`xuất hiện ba lần, do đó chính xác một lần xuất hiện vẫn không thể so sánh được. Đầu ra là`Askerov`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) dự kiến ​​| Mỗi họ được xử lý một lần, với các phép toán tập hợp O(1) dự kiến. | 
| Không gian | O(n) | Trong trường hợp xấu nhất, nhiều họ khác nhau có thể có tần số lẻ và vẫn nằm trong tập hợp. | 

Với`n`nhiều nhất là 150000, thuật toán thời gian tuyến tính dự kiến ​​chỉ thực hiện một số thao tác bảng băm trên mỗi dòng đầu vào. Điều đó nằm trong phạm vi độ phức tạp thuật toán dự định trong giới hạn hai giây. Việc sử dụng bộ nhớ tăng lên theo số lượng họ riêng biệt thay vì số lượng bản ghi được lưu trữ riêng biệt. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())
    odd = set()

    for _ in range(n):
        surname = input().strip()
        if surname in odd:
            odd.remove(surname)
        else:
            odd.add(surname)

    if len(odd) == 1:
        print(next(iter(odd)))
    else:
        print("FAIL")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """3
Yakubov
Yakubov
Abramov
"""
) == "Abramov", "sample 1"

# Provided sample 2
assert run(
    """7
Askerov
Shumacher
Askerov
Askerov
Shumacher
Abalkin
Abalkin
"""
) == "Askerov", "sample 2"

# Minimum-size input
assert run(
    """1
Petrov
"""
) == "Petrov", "minimum n"

# All records have the same surname, with an odd number of occurrences
assert run(
    """5
Ivanov
Ivanov
Ivanov
Ivanov
Ivanov
"""
) == "Ivanov", "all equal values"

# Maximum valid odd n, all records have the same surname
max_n = 149999
assert run(
    str(max_n) + "\n" + ("Z" * 255 + "\n") * max_n
) == "Z" * 255, "maximum n and maximum surname length"

# No unique unmatched surname, representing the FAIL case
assert run(
    """6
A
B
A
B
C
C
"""
) == "FAIL", "all records paired"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / Petrov`|`Petrov`| Đầu vào hợp lệ tối thiểu và một bản ghi chưa từng có | 
| Năm bản sao của`Ivanov`|`Ivanov`| Số lần xuất hiện lặp đi lặp lại và tính chẵn lẻ thay vì tần suất bằng một | 
| 149999 bản sao của họ 255 ký tự | Cùng họ | Số lẻ hợp lệ tối đa`n`và độ dài họ tối đa | 
|`A B A B C C`|`FAIL`| Mỗi lần xuất hiện đều được ghép nối và tập hợp trở nên trống | 

## Vỏ cạnh 

Trường hợp tối thiểu là```
1
Petrov
```Bộ này bắt đầu trống.`Petrov`được chèn vào, vì vậy sau bản ghi duy nhất, tập hợp là`{Petrov}`. Kích thước của nó là một và thuật toán in`Petrov`. 

Họ không nhất thiết phải xuất hiện chính xác một lần để trở thành câu trả lời. Coi như```
5
Ivanov
Ivanov
Ivanov
Ivanov
Ivanov
```Lần chèn đầu tiên xuất hiện`Ivanov`, người thứ hai loại bỏ nó, người thứ ba chèn lại, người thứ tư loại bỏ nó và người thứ năm chèn nó vào. Bộ cuối cùng chứa`Ivanov`, vậy câu trả lời là`Ivanov`. Đây là lý do tại sao việc theo dõi tính chẵn lẻ lại thích hợp hơn việc tìm kiếm cụ thể tần số của một. 

Trường hợp trả về tất cả có thể được biểu diễn bằng```
6
A
B
A
B
C
C
```

`A`được chèn vào và gỡ bỏ, sau đó`B`được chèn vào và gỡ bỏ, và cuối cùng`C`được chèn vào và loại bỏ. Tập cuối cùng trống nên chương trình sẽ in`FAIL`. 

Họ có độ dài tối đa được xử lý như một chuỗi Python thông thường. Ví dụ,```
1
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
```tạo ra toàn bộ họ không thay đổi. Thuật toán không bao giờ lập chỉ mục cho họ hoặc giả định bất kỳ điều gì về độ dài của nó, vì vậy ranh giới 255 ký tự không yêu cầu nhánh đặc biệt. 

Thứ tự của các bản ghi cũng không có hiệu lực. Vì```
5
Askerov
Abalkin
Askerov
Abalkin
Shumacher
```hai cặp hủy bỏ bất kể vị trí của họ. Sau bốn bản ghi đầu tiên, tập hợp này trống và`Shumacher`được chèn bởi bản ghi cuối cùng. Kết quả là`Shumacher`. 

Cuối cùng, kích thước đầu vào được khai báo là số lẻ. Theo điều kiện hợp lệ của câu chuyện, mỗi chuyến đi hoàn thành sẽ đóng góp hai bản ghi, trong khi đúng một chuyến đi chưa hoàn thành sẽ đóng góp thêm một bản ghi, do đó phải có đúng một họ tần số lẻ. các`FAIL`nhánh được giữ lại vì đặc tả đầu ra cho phép rõ ràng và vì việc kiểm tra kích thước tập hợp cuối cùng giúp việc triển khai trở nên mạnh mẽ nếu đầu vào không đáp ứng cấu trúc dự định đó.
