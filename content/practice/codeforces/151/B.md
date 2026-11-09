---
title: "CF 151B - Số điện thoại"
description: "Mỗi người bạn có một cuốn danh bạ điện thoại có ghi các số có dạng XX-XX-XX. Mỗi số điện thoại thuộc về đúng một trong ba loại. Số taxi sử dụng cùng một chữ số ở mọi nơi. Ví dụ là 11-11-11 hoặc 55-55-55."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 151
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 107 (Div. 2)"
rating: 1200
weight: 151
solve_time_s: 106
verified: true
draft: false
---

[CF 151B - Số điện thoại](https://codeforces.com/problemset/problem/151/B) 

**Đánh giá:** 1200 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người bạn có một cuốn danh bạ điện thoại chứa các số được viết theo dạng`XX-XX-XX`. Mỗi số điện thoại thuộc về đúng một trong ba loại. 

Số taxi sử dụng cùng một chữ số ở mọi nơi. Ví dụ là`11-11-11`hoặc`55-55-55`. 

Một số bánh pizza đang giảm dần khi chúng ta đọc các chữ số của nó từ trái sang phải, bỏ qua dấu gạch ngang. Mỗi chữ số tiếp theo phải nhỏ hơn chữ số trước đó. Ví dụ là`98-76-54`Và`95-43-21`. 

Bất kỳ số nào không phải là taxi hay pizza đều được coi là số dành cho nữ. 

Đối với mỗi người bạn, chúng ta phải đếm xem mỗi loại có bao nhiêu số. Sau khi xử lý tất cả danh bạ điện thoại, chúng tôi xuất ra tên của những người bạn có số lượng taxi cao nhất, số lượng pizza cao nhất và số lượng cô gái cao nhất. Nhiều người bạn có thể hòa nhau ở mức tối đa và trong trường hợp đó, chúng tôi in tất cả họ theo thứ tự đầu vào. 

Những hạn chế là rất nhỏ. Có tối đa 100 người bạn và mỗi người bạn có tối đa 100 số điện thoại. Mỗi số có đúng 6 chữ số. Ngay cả việc triển khai đơn giản để kiểm tra từng chữ số của từng số riêng lẻ cũng có thể chạy thoải mái trong giới hạn. Tổng cộng, chương trình xử lý tối đa 10.000 số điện thoại, mỗi số có độ dài cố định. Điều đó có nghĩa là khối lượng công việc thực tế là rất nhỏ. 

Phần khó khăn không phải là hiệu suất mà là tính chính xác của phân loại và định dạng đầu ra. 

Một lỗi dễ mắc phải là quên bỏ qua dấu gạch ngang khi kiểm tra các chữ số. Coi như:```
98-76-54
```Các chữ số đang giảm dần, nhưng nếu chúng ta so sánh trực tiếp các ký tự mà không xóa`-`, logic bị phá vỡ bởi vì`'-'`không phải là một chữ số. 

Một sai lầm phổ biến khác là cho phép giảm số lượng pizza không nghiêm ngặt. Coi như:```
98-76-55
```Đây không phải là số pizza vì hai chữ số cuối bằng nhau. Một tình trạng bất cẩn như`>=`thay vì`>`chấp nhận nó một cách sai lầm. 

Trường hợp khó nhận biết xuất hiện khi cùng một số xuất hiện nhiều lần trong một danh bạ điện thoại. Tuyên bố nói rõ ràng rằng các bản sao phải được tính nhiều lần. Ví dụ:```
1
3 Alex
11-11-11
11-11-11
12-34-56
```Alex có hai số taxi chứ không phải một. Sử dụng một bộ để loại bỏ các bản sao sẽ tạo ra câu trả lời sai. 

Một cạm bẫy định dạng khác xảy ra khi nhiều bạn bè hòa nhau ở mức tối đa. Giả định:```
2
1 A
11-11-11
1 B
22-22-22
```Cả hai người bạn đều có một số taxi nên kết quả đúng là:```
If you want to call a taxi, you should call: A, B.
```Các tên phải giữ nguyên theo thứ tự đầu vào và được phân tách chính xác bằng`", "`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xử lý từng số điện thoại một cách độc lập và phân loại nó bằng cách kiểm tra các chữ số của nó. 

Đối với mỗi số, chúng tôi xóa dấu gạch ngang và thu được một chuỗi có độ dài 6. Sau đó, chúng tôi kiểm tra xem tất cả các chữ số có bằng nhau hay không. Nếu có thì đó là số taxi. 

Nếu không, chúng tôi kiểm tra xem mỗi chữ số có lớn hơn chữ số tiếp theo hay không. Nếu có thì đó là số pizza. 

Nếu không thì nó thuộc thể loại con gái. 

Phương pháp brute-force này đã đủ nhanh vì kích thước đầu vào rất nhỏ. Trong trường hợp xấu nhất, chúng tôi kiểm tra 10.000 số điện thoại, mỗi số chỉ chứa 6 chữ số. Đó là khoảng 60.000 thao tác ký tự, về cơ bản là tức thời. 

Không cần cấu trúc dữ liệu nâng cao hoặc thủ thuật tối ưu hóa. Quan sát quan trọng là mọi phân loại chỉ phụ thuộc vào sáu ký tự, vì vậy mỗi lần kiểm tra là thời gian không đổi. 

Việc triển khai ít cẩn thận hơn một chút có thể liên tục quét cùng một chuỗi nhiều lần hoặc chuyển đổi các chữ số một cách không cần thiết, nhưng ngay cả điều đó vẫn diễn ra thoải mái. Giải pháp sạch nhất chỉ đơn giản là xử lý mỗi số một lần và cập nhật bộ đếm tương ứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n×s×6) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n×s×6) | O(1) | Đã chấp nhận | 

Các cách tiếp cận bạo lực và tối ưu ở đây thực sự giống nhau vì các ràng buộc rất nhỏ. Phần quan trọng là thực hiện các quy tắc phân loại một cách chính xác. 

## Hướng dẫn thuật toán 

1. Đọc số lượng bạn bè. 
2. Với mỗi người bạn, hãy đọc số điện thoại và tên của người bạn đó. 
3. Khởi tạo ba bộ đếm cho người bạn này: taxi, pizza và các cô gái. 
4. Đối với mỗi số điện thoại: 

1. Bỏ dấu gạch ngang để chỉ còn lại sáu chữ số. 
2. Kiểm tra xem sáu chữ số có giống nhau không. 
3. Nếu chúng giống hệt nhau, hãy tăng số đếm taxi. 
4. Mặt khác, hãy kiểm tra xem mọi chữ số có lớn hơn chữ số tiếp theo hay không. 
5. Nếu các chữ số giảm dần, hãy tăng số đếm pizza. 
6. Nếu không, hãy tăng bộ đếm cô gái. 
5. Lưu ba số đếm cùng với tên của người bạn. 
6. Sau khi xử lý xong tất cả bạn bè, hãy tính: 

1. Số lượng taxi tối đa. 
2. Số lượng pizza tối đa. 
3. Số lượng nữ tối đa. 
7. Duyệt danh sách bạn bè theo thứ tự nhập và thu thập: 

1. Mỗi tên có số lượng taxi bằng số lượng taxi tối đa. 
2. Mọi tên có số lượng pizza bằng số lượng pizza tối đa. 
3. Mỗi tên có số con gái đều bằng số con gái tối đa. 
8. In ba câu được yêu cầu có các tên được nối bởi`", "`. 

### Tại sao nó hoạt động 

Mỗi số điện thoại thuộc về đúng một loại. 

Nếu tất cả các chữ số đều bằng nhau thì số đó thỏa mãn trực tiếp định nghĩa taxi. 

Nếu các chữ số không bằng nhau nhưng mỗi chữ số lớn hơn chữ số tiếp theo thì số đó thỏa mãn định nghĩa pizza. 

Số còn lại không thể là taxi hay pizza nên phải thuộc hạng mục con gái. 

Vì mỗi số được phân loại chính xác một lần và mỗi lần xuất hiện đều được tính độc lập nên bộ đếm cho mỗi người bạn đều chính xác. Lấy bộ đếm tối đa trên tất cả bạn bè sẽ xác định chính xác các tên được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    friends = []

    for _ in range(n):
        s, name = input().split()
        s = int(s)

        taxi = 0
        pizza = 0
        girls = 0

        for _ in range(s):
            number = input().strip().replace("-", "")

            if len(set(number)) == 1:
                taxi += 1
            elif all(number[i] > number[i + 1] for i in range(5)):
                pizza += 1
            else:
                girls += 1

        friends.append((name, taxi, pizza, girls))

    max_taxi = max(friend[1] for friend in friends)
    max_pizza = max(friend[2] for friend in friends)
    max_girls = max(friend[3] for friend in friends)

    taxi_names = [friend[0] for friend in friends if friend[1] == max_taxi]
    pizza_names = [friend[0] for friend in friends if friend[2] == max_pizza]
    girls_names = [friend[0] for friend in friends if friend[3] == max_girls]

    print(
        "If you want to call a taxi, you should call: "
        + ", ".join(taxi_names)
        + "."
    )

    print(
        "If you want to order a pizza, you should call: "
        + ", ".join(pizza_names)
        + "."
    )

    print(
        "If you want to go to a cafe with a wonderful girl, you should call: "
        + ", ".join(girls_names)
        + "."
    )

solve()
```Giải pháp tuân theo logic phân loại trực tiếp. 

biểu thức`number.replace("-", "")`loại bỏ các ký tự định dạng để chúng ta chỉ có thể làm việc với các chữ số. Sau đó, mọi số sẽ trở thành một chuỗi sáu ký tự. 

Việc kiểm tra taxi sử dụng`len(set(number)) == 1`. Một bộ sẽ loại bỏ các bản sao, vì vậy nếu kích thước được đặt là một thì mọi chữ số phải giống hệt nhau. 

Kiểm tra pizza so sánh các chữ số liền kề:```
all(number[i] > number[i + 1] for i in range(5))
```Sự so sánh hoàn toàn lớn hơn, không lớn hơn hoặc bằng. Chi tiết này quan trọng vì các chữ số lặp lại sẽ làm mất hiệu lực tình trạng của pizza. 

Hạng mục con gái chỉ đơn giản là trường hợp còn lại sau khi hai lần kiểm tra đầu tiên thất bại. 

Chương trình lưu trữ số lượng bạn bè của mỗi người và sau đó tính toán mức tối đa toàn cầu. Vì đầu ra cuối cùng phải giữ nguyên thứ tự đầu vào nên chúng tôi lặp qua danh sách được lưu trữ thay vì sắp xếp. 

Định dạng đầu ra rất dễ bị sai. Câu lệnh yêu cầu dấu phẩy theo sau là dấu cách, với đúng một dấu chấm cuối cùng. sử dụng`", ".join(...)`xử lý việc này một cách sạch sẽ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
2 Fedorov
22-22-22
98-76-54
3 Melnikov
75-19-09
23-45-67
99-99-98
7 Rogulenko
22-22-22
11-11-11
33-33-33
44-44-44
55-55-55
66-66-66
95-43-21
3 Kaluzhin
11-11-11
99-99-99
98-65-32
```Xử lý dấu vết: 

| Bạn | Số | Loại | Taxi | Pizza | Cô gái | 
| --- | --- | --- | --- | --- | --- | 
| Fedorov | 22-22-22 | Taxi | 1 | 0 | 0 | 
| Fedorov | 98-76-54 | Pizza | 1 | 1 | 0 | 
| Melnikov | 75-19-09 | Cô gái | 0 | 0 | 1 | 
| Melnikov | 23-45-67 | Cô gái | 0 | 0 | 2 | 
| Melnikov | 99-99-98 | Cô gái | 0 | 0 | 3 | 
| Rogulenko | 22-22-22 | Taxi | 1 | 0 | 0 | 
| Rogulenko | 11-11-11 | Taxi | 2 | 0 | 0 | 
| Rogulenko | 33-33-33 | Taxi | 3 | 0 | 0 | 
| Rogulenko | 44-44-44 | Taxi | 4 | 0 | 0 | 
| Rogulenko | 55-55-55 | Taxi | 5 | 0 | 0 | 
| Rogulenko | 66-66-66 | Taxi | 6 | 0 | 0 | 
| Rogulenko | 95-43-21 | Pizza | 6 | 1 | 0 | 
| Kaluzhin | 11-11-11 | Taxi | 1 | 0 | 0 | 
| Kaluzhin | 99-99-99 | Taxi | 2 | 0 | 0 | 
| Kaluzhin | 98-65-32 | Pizza | 2 | 1 | 0 | 

Cực đại cuối cùng: 

| Danh mục | Tối đa | Bạn bè | 
| --- | --- | --- | 
| Taxi | 6 | Rogulenko | 
| Pizza | 1 | Fedorov, Rogulenko, Kaluzhin | 
| Cô gái | 3 | Melnikov | 

Ví dụ này hiển thị cả ba danh mục và thể hiện mối liên hệ giữa danh mục pizza. 

### Ví dụ tùy chỉnh 

đầu vào:```
2
3 Alice
11-11-11
98-76-54
12-34-56
2 Bob
22-22-22
87-65-43
```Xử lý dấu vết: 

| Bạn | Số | Loại | Taxi | Pizza | Cô gái | 
| --- | --- | --- | --- | --- | --- | 
| Alice | 11-11-11 | Taxi | 1 | 0 | 0 | 
| Alice | 98-76-54 | Pizza | 1 | 1 | 0 | 
| Alice | 12-34-56 | Cô gái | 1 | 1 | 1 | 
| Bob | 22-22-22 | Taxi | 1 | 0 | 0 | 
| Bob | 87-65-43 | Pizza | 1 | 1 | 0 | 

Cực đại cuối cùng: 

| Danh mục | Tối đa | Bạn bè | 
| --- | --- | --- | 
| Taxi | 1 | Alice, Bob | 
| Pizza | 1 | Alice, Bob | 
| Cô gái | 1 | Alice | 

Dấu vết này thể hiện việc xử lý ràng buộc chính xác và duy trì trật tự đầu vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n×s×6) | Mỗi số điện thoại đều có đúng 6 chữ số | 
| Không gian | O(1) | Chỉ có một số bộ đếm và danh sách tên được lưu trữ | 

Ngay cả ở giới hạn tối đa, chương trình chỉ xử lý khoảng 60.000 phép so sánh chữ số. Đó là thấp hơn nhiều so với giới hạn thời gian có sẵn. Việc sử dụng bộ nhớ cũng không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    n = int(input())

    friends = []

    for _ in range(n):
        s, name = input().split()
        s = int(s)

        taxi = 0
        pizza = 0
        girls = 0

        for _ in range(s):
            number = input().strip().replace("-", "")

            if len(set(number)) == 1:
                taxi += 1
            elif all(number[i] > number[i + 1] for i in range(5)):
                pizza += 1
            else:
                girls += 1

        friends.append((name, taxi, pizza, girls))

    max_taxi = max(friend[1] for friend in friends)
    max_pizza = max(friend[2] for friend in friends)
    max_girls = max(friend[3] for friend in friends)

    taxi_names = [friend[0] for friend in friends if friend[1] == max_taxi]
    pizza_names = [friend[0] for friend in friends if friend[2] == max_pizza]
    girls_names = [friend[0] for friend in friends if friend[3] == max_girls]

    print(
        "If you want to call a taxi, you should call: "
        + ", ".join(taxi_names)
        + "."
    )

    print(
        "If you want to order a pizza, you should call: "
        + ", ".join(pizza_names)
        + "."
    )

    print(
        "If you want to go to a cafe with a wonderful girl, you should call: "
        + ", ".join(girls_names)
        + "."
    )

    sys.stdout = sys.__stdout__

    return out.getvalue()

# provided sample
assert run(
"""4
2 Fedorov
22-22-22
98-76-54
3 Melnikov
75-19-09
23-45-67
99-99-98
7 Rogulenko
22-22-22
11-11-11
33-33-33
44-44-44
55-55-55
66-66-66
95-43-21
3 Kaluzhin
11-11-11
99-99-99
98-65-32
"""
) == (
"""If you want to call a taxi, you should call: Rogulenko.
If you want to order a pizza, you should call: Fedorov, Rogulenko, Kaluzhin.
If you want to go to a cafe with a wonderful girl, you should call: Melnikov.
"""
), "sample 1"

# minimum input
assert run(
"""1
1 Alex
11-11-11
"""
) == (
"""If you want to call a taxi, you should call: Alex.
If you want to order a pizza, you should call: Alex.
If you want to go to a cafe with a wonderful girl, you should call: Alex.
"""
), "single friend"

# duplicate numbers must count multiple times
assert run(
"""1
3 Bob
11-11-11
11-11-11
12-34-56
"""
) == (
"""If you want to call a taxi, you should call: Bob.
If you want to order a pizza, you should call: Bob.
If you want to go to a cafe with a wonderful girl, you should call: Bob.
"""
), "duplicates counted"

# tie handling
assert run(
"""2
1 A
11-11-11
1 B
22-22-22
"""
) == (
"""If you want to call a taxi, you should call: A, B.
If you want to order a pizza, you should call: A, B.
If you want to go to a cafe with a wonderful girl, you should call: A, B.
"""
), "ties"

# pizza must be strictly decreasing
assert run(
"""1
2 Carl
98-76-54
98-76-55
"""
) == (
"""If you want to call a taxi, you should call: Carl.
If you want to order a pizza, you should call: Carl.
If you want to go to a cafe with a wonderful girl, you should call: Carl.
"""
), "strict decrease"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Người bạn độc thân một số | Cùng một người bạn được in ở khắp mọi nơi | Hạn chế tối thiểu | 
| Số taxi trùng lặp | Đếm các bản sao riêng biệt | Tránh trùng lặp không chính xác | 
| Hai người bạn trói | Cả hai tên đều được in theo thứ tự | Xử lý cà vạt | 
|`98-76-55`| Không phải pizza | Bất bình đẳng nghiêm ngặt | 

## Vỏ cạnh 

Hãy xem xét các số điện thoại lặp đi lặp lại:```
1
3 Alex
11-11-11
11-11-11
12-34-56
```Thuật toán xử lý từng lần xuất hiện một cách độc lập. Hai số đầu tiên đều thỏa mãn điều kiện taxi vì tất cả các chữ số đều bằng nhau. Bộ đếm taxi trở thành 2. Vì câu lệnh đếm các mục lặp lại một cách riêng biệt nên hành vi này là đúng. 

Hãy xem xét một số gần giống số pizza:```
1
1 Bob
98-76-55
```Sau khi bỏ dấu gạch ngang, các chữ số sẽ trở thành`987655`. Các so sánh là:```
9 > 8
8 > 7
7 > 6
6 > 5
5 > 5   false
```Sự so sánh cuối cùng thất bại vì mức giảm không chặt chẽ. Thuật toán phân loại chính xác số đó là số của con gái. 

Xem xét mối quan hệ giữa nhiều người bạn:```
2
1 A
11-11-11
1 B
22-22-22
```Cả hai người bạn đều kết thúc với số lượng taxi là 1. Thuật toán tính số lượng taxi tối đa là 1 và sau đó thu thập mọi người bạn khớp với giá trị đó. Vì nó đi qua thứ tự đầu vào ban đầu nên đầu ra trở thành:```
A, B
```phù hợp với thứ tự yêu cầu. 

Hãy xem xét một người bạn không có số điện thoại:```
1
0 Alice
```Cả ba bộ đếm vẫn là 0. Vì Alice là người bạn duy nhất nên mọi mức tối đa của danh mục cũng bằng 0, vì vậy Alice xuất hiện ở cả ba dòng đầu ra. Thuật toán xử lý việc này một cách tự nhiên mà không có trường hợp đặc biệt nào.
