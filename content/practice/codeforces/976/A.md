---
title: "CF 976A - Số nhị phân tối thiểu"
description: "Chúng ta được cung cấp một chuỗi nhị phân đã ở dạng chuẩn hợp lệ, nghĩa là nó biểu thị một số nguyên không âm ở dạng nhị phân mà không có các số 0 đứng đầu không cần thiết."
date: "2026-06-17T01:30:16+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 976
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 43 (Rated for Div. 2)"
rating: 800
weight: 976
solve_time_s: 106
verified: false
draft: false
---

[CF 976A - Số nhị phân tối thiểu](https://codeforces.com/problemset/problem/976/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân đã ở dạng chuẩn hợp lệ, nghĩa là nó biểu thị một số nguyên không âm ở dạng nhị phân mà không có các số 0 đứng đầu không cần thiết. Nhiệm vụ là chuyển đổi chuỗi này thành giá trị nhỏ nhất có thể theo quy tắc so sánh nhị phân, sử dụng hai thao tác được phép: tự do hoán đổi các ký tự liền kề và giảm bất kỳ sự xuất hiện nào của các ký tự liên tiếp bằng cách thay thế “11” bằng một “1”. 

Thao tác đầu tiên cho phép chúng ta hoàn toàn tự do sắp xếp lại các ký tự, vì bất kỳ hoán vị nào cũng có thể đạt được bằng cách hoán đổi liền kề lặp đi lặp lại. Thao tác thứ hai thay đổi cấu trúc của chuỗi bằng cách nén các cặp đơn vị, giảm số lượng đơn vị trong chuỗi một cách hiệu quả. 

Đầu ra không chỉ là bất kỳ chuỗi được sắp xếp lại nào mà còn là chuỗi nhị phân nhỏ nhất về mặt từ điển có thể thu được bằng các phép biến đổi này, trong khi vẫn duy trì một biểu diễn hợp lệ không có số 0 đứng đầu. 

Ràng buộc n 100 có nghĩa là ngay cả suy luận bậc hai hoặc mô phỏng nhỏ cũng có thể thực hiện được, nhưng cấu trúc của các phép toán cho thấy chúng ta không nên mô phỏng một cách mù quáng. Thay vào đó, chúng ta nên xác định các đại lượng bất biến, đặc biệt là có bao nhiêu số 0 và số 1 có thể được giảm bớt và sắp xếp lại một cách hiệu quả. 

Một cách tiếp cận ngây thơ sẽ thử tất cả các hoán vị và rút gọn, nhưng ngay cả với n = 100 thì điều này vẫn lớn về mặt thiên văn. Ngay cả một mô phỏng tham lam áp dụng nhiều lần các phép toán mà không có bất biến cấu trúc cũng có thể thất bại. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi chỉ chứa các chuỗi. Ví dụ: đầu vào "111" có thể thu gọn liên tục thành "1" và bất kỳ phương pháp nào xử lý việc giảm dưới dạng một lần chuyển sẽ dừng không chính xác ở "11". Một trường hợp khác là khi các số 0 tách biệt các số 0, chẳng hạn như "10101", trong đó việc sắp xếp lại tương tác với việc nén theo những cách không cục bộ. Một chiến lược hoán đổi tham lam ngây thơ có thể không nhận ra rằng việc tập hợp những cái lại với nhau trước tiên sẽ tối đa hóa khả năng nén. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là áp dụng liên tục hai thao tác theo mọi cách có thể, theo dõi tất cả các chuỗi có thể truy cập và chọn giá trị tối thiểu. Điều này hiệu quả vì không gian trạng thái là hữu hạn và tất cả các chuyển đổi đều hợp lệ. Tuy nhiên, mỗi lần hoán đổi sẽ mở rộng hệ số phân nhánh một cách đáng kể và mặc dù n nhỏ, số lượng chuỗi nhị phân riêng biệt có thể truy cập được thông qua việc xen kẽ các hoán đổi và nén vẫn tăng theo cấp số nhân. Điểm nghẽn là việc hoán đổi tạo ra tất cả các hoán vị trong khi quá trình nén thay đổi độ dài chuỗi một cách linh hoạt, khiến cho BFS hoặc DFS vượt qua các trạng thái không thể thực hiện được. 

Quan sát quan trọng là việc hoán đổi cho phép sắp xếp lại thứ tự tùy ý, do đó kết quả cuối cùng chỉ phụ thuộc vào số lượng số 0 và số 1 chứ không phụ thuộc vào vị trí ban đầu của chúng. Quy tắc nén “11 → 1” có nghĩa là nhiều số đơn vị thu gọn thành một số duy nhất, do đó, bất kỳ số dương nào của số đơn vị cuối cùng cũng hoạt động giống như một số đơn lẻ. Do đó, sự khác biệt có ý nghĩa duy nhất là liệu có ít nhất một số '1' hay không có số nào, kết hợp với cách các số 0 được sắp xếp so với số 0 cuối cùng. 

Nếu không có '1', câu trả lời chỉ đơn giản là "0". Nếu có ít nhất một số '1', tất cả những số đó có thể được hợp nhất thành một số '1' duy nhất và khi đó chúng ta chỉ cần đặt các số 0 một cách tối ưu. Vì không được phép sử dụng các số 0 đứng đầu nên cách sắp xếp tối thiểu là đặt số '1' ở xa bên trái nhất có thể trong khi vẫn giữ các số 0 ở sau nó, tạo ra một chuỗi có dạng "1" theo sau là tất cả các số 0. 

Do đó, vấn đề giảm xuống việc đếm xem số '1' có tồn tại hay không và đếm số không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Đếm Số Một và Số Không | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Quét chuỗi và đếm xem có bao nhiêu số 0 và liệu có tồn tại ít nhất một số '1' hay không. Điều này nắm bắt tất cả các thông tin có liên quan sau khi hoán đổi tùy ý. 
2. Nếu không tồn tại số '1', hãy trả về "0", vì bất kỳ chuỗi nào toàn số 0 đều biểu thị số 0 và không thể giảm thêm. 
3. Nếu tồn tại ít nhất một số '1', hãy giảm tất cả các số đó thành một số '1' duy nhất bằng cách áp dụng lặp lại thao tác "11 → 1". 
4. Xây dựng đầu ra bằng cách đặt số '1' đó theo sau là tất cả các số 0. Điều này đảm bảo không có số 0 đứng đầu xuất hiện và giá trị được giảm thiểu khi so sánh nhị phân. 

Bước lý luận quan trọng là một khi các giao dịch hoán đổi được cho phép tự do thì cấu trúc vị trí là vô nghĩa và một khi có sự nén, nhiều cấu trúc sẽ sụp đổ thành một đại diện duy nhất. Thông tin tối giản duy nhất là số đó bằng 0 hay dương và còn lại bao nhiêu số 0. 

### Tại sao nó hoạt động 

Điều bất biến là tập hợp nhiều ký tự giảm xuống tất cả các số 0 hoặc chính xác là một '1' cộng với các số 0. Hoán đổi duy trì số lượng và quy tắc nén làm giảm nghiêm ngặt số lượng mà không bao giờ tăng giá trị. Do đó, mọi trạng thái có thể truy cập đều tương đương với một cấu hình có nhiều nhất một '1' và trong số các cấu hình đó, việc đặt một '1' trước tất cả các số 0 sẽ mang lại giá trị nhị phân nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

if '1' not in s:
    print("0")
else:
    zeros = s.count('0')
    print("1" + "0" * zeros)
```Việc triển khai mã hóa trực tiếp việc rút gọn được mô tả trong thuật toán. Việc kiểm tra sự vắng mặt của '1' xử lý trường hợp suy biến số 0. Đếm các số 0 là đủ vì tất cả các số 1 đều thu gọn thành một đại diện duy nhất. Cấu trúc cuối cùng đảm bảo giá trị nhị phân nhỏ nhất có thể bằng cách đặt số ‘1’ duy nhất ở vị trí quan trọng cao nhất. 

Một cạm bẫy phổ biến là cố gắng duy trì trật tự tương đối sau khi hoán đổi, nhưng hoán đổi sẽ vô hiệu hóa mọi ràng buộc về vị trí. Một vấn đề tế nhị khác là quên rằng nhiều cái không còn là bội số ở dạng rút gọn cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1001
```| Bước | Những cái tồn tại | Số 0 | Chuỗi được xây dựng | 
| --- | --- | --- | --- | 
| Quét lần đầu | Có | 2 | - | 
| Giảm | Có → 1 cái | 2 | - | 
| Xây dựng | - | - | 100 | 

Điều này cho thấy cách hai số hợp nhất thành một số duy nhất và các số 0 không thay đổi, tạo ra biểu diễn nhỏ nhất có thể. 

### Ví dụ 2 

đầu vào:```
111
```| Bước | Những cái tồn tại | Số 0 | Chuỗi được xây dựng | 
| --- | --- | --- | --- | 
| Quét lần đầu | Có | 0 | - | 
| Giảm | 1 cái | 0 | - | 
| Xây dựng | - | - | 1 | 

Điều này thể hiện hiệu ứng nén hoàn toàn trong đó tất cả các số được thu gọn thành một chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét một lần để đếm số 0 và phát hiện số 0 | 
| Không gian | O(1) | Chỉ có bộ đếm được lưu trữ | 

Thuật toán phù hợp một cách thoải mái trong các ràng buộc vì n ≤ 100 khiến cho việc quét tuyến tính tầm thường trở nên tức thời. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = sys.stdin.readline().strip()
    if '1' not in s:
        return "0"
    return "1" + "0" * s.count('0')

# provided sample
assert run("4\n1001\n") == "100"

# all zeros (edge)
assert run("3\n000\n") == "0"

# single one
assert run("1\n1\n") == "1"

# all ones
assert run("5\n11111\n") == "1"

# mixed
assert run("5\n10101\n") == "1000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 000 | 0 | sụp đổ hoàn toàn bằng không | 
| 1 | 1 | trường hợp phần tử đơn | 
| 11111 | 1 | nén hoàn toàn | 
| 10101 | 1000 | chuẩn hóa cấu trúc hỗn hợp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chuỗi chỉ chứa số 0. Trong tình huống này, không có thao tác hoán đổi hoặc nén nào có thể thay đổi bất kỳ điều gì và kết quả đầu ra chính xác vẫn là “0”. Thuật toán xử lý vấn đề này một cách rõ ràng bằng cách kiểm tra sự vắng mặt của '1'. 

Một trường hợp cạnh khác là một chuỗi bao gồm toàn những chuỗi đơn lẻ. Việc áp dụng lặp đi lặp lại quy tắc nén sẽ giảm bất kỳ độ dài nào của các chuỗi liên tiếp thành một '1' duy nhất và việc hoán đổi là không liên quan. Việc triển khai tự nhiên tạo ra "1". 

Sự sắp xếp hỗn hợp chẳng hạn như “10101” có thể gợi ý rằng cần phải tối ưu hóa vị trí, nhưng vì hoán đổi cho phép hoán vị tùy ý nên trạng thái cuối cùng sẽ bỏ qua thứ tự ban đầu. Thuật toán giảm chính xác nó thành một số duy nhất theo sau là số 0, xác nhận rằng cấu trúc được xác định hoàn toàn bằng số lượng thay vì sắp xếp.
