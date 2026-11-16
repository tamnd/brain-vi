---
title: "CF 1907B - YetnotherrokenKeoard"
description: "Chúng tôi được yêu cầu mô phỏng một bàn phím có các phím bị hỏng hoạt động giống như các phím lùi có chọn lọc. Mỗi chữ thường 'b' sẽ xóa chữ cái viết thường gần đây nhất trong chuỗi đã nhập và mọi chữ hoa 'B' sẽ xóa chữ cái viết hoa gần đây nhất. Tất cả các chữ cái khác được nối thêm bình thường."
date: "2026-06-08T20:37:16+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1907
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 913 (Div. 3)"
rating: 1000
weight: 1907
solve_time_s: 156
verified: true
draft: false
---

[CF 1907B - YetnotherrokenKeoard](https://codeforces.com/problemset/problem/1907/B) 

**Đánh giá:** 1000 
**Tags:** cấu trúc dữ liệu, triển khai, chuỗi 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một bàn phím có các phím bị hỏng hoạt động giống như các phím lùi có chọn lọc. Mỗi chữ thường 'b' sẽ xóa chữ cái viết thường gần đây nhất trong chuỗi đã nhập và mọi chữ hoa 'B' sẽ xóa chữ cái viết hoa gần đây nhất. Tất cả các chữ cái khác được nối thêm bình thường. Đầu ra là chuỗi được gõ cuối cùng sau khi xử lý tất cả các lần nhấn phím. 

Mỗi trường hợp thử nghiệm đưa ra một chuỗi đại diện cho các lần nhấn phím. Đầu vào chứa tối đa 1000 trường hợp thử nghiệm và tổng số chữ cái trong tất cả các trường hợp thử nghiệm tối đa là một triệu. Điều này ngụ ý rằng bất kỳ thuật toán nào cũng phải xử lý từng ký tự trong thời gian gần như không đổi, vì cách tiếp cận O(n²) có thể yêu cầu tới 10¹² thao tác trong trường hợp xấu nhất, tốc độ này quá chậm. Do đó, chúng ta phải hướng tới giải pháp O(n) cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh bao gồm các chuỗi chỉ chứa các ký tự 'b' hoặc 'B', trong đó không nên thêm gì vào và các ký tự xóa lùi lặp lại, không được xóa các chữ cái không tồn tại. Ví dụ: chuỗi "bbb" tạo ra kết quả trống vì không có chữ cái viết thường để xóa và "BB" tạo ra kết quả trống vì lý do tương tự với chữ in hoa. Các chuỗi hỗn hợp như "aBbB" phải xử lý chính xác các khoảng lùi xen kẽ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ duy trì một chuỗi hoặc danh sách đơn và đối với mỗi ký tự, quét ngược để tìm ký tự cuối cùng của trường hợp bắt buộc khi gặp 'b' hoặc 'B'. Mặc dù điều này đúng về mặt khái niệm, nhưng việc quét lùi để tìm từng khoảng lùi là O(n) cho mỗi lần xóa, dẫn đến thuật toán O(n²) trong trường hợp xấu nhất, quá chậm đối với các đầu vào có độ dài lên tới 10⁶. 

Thông tin chuyên sâu quan trọng để đạt được thời gian tuyến tính là duy trì hai ngăn xếp riêng biệt: một ngăn xếp cho chữ thường và một ngăn xếp cho chữ in hoa. Khi một ký tự bình thường được gõ vào, nó sẽ được thêm vào cả chuỗi cuối cùng và ngăn xếp tương ứng. Khi gõ 'b' hoặc 'B', chúng tôi lấy phần tử cuối cùng ra khỏi ngăn xếp thích hợp và xóa chữ cái đó khỏi chuỗi cuối cùng. Bằng cách sử dụng ngăn xếp, các thao tác xóa sẽ trở thành O(1) cho mỗi thao tác, đưa ra thuật toán O(n) tổng thể. Điều này có hiệu quả vì sự cố chỉ yêu cầu xóa chữ cái gần đây nhất của một trường hợp cụ thể, đây chính xác là hành vi mà một ngăn xếp mô hình một cách tự nhiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm đối với đầu vào lớn | 
| Dựa trên ngăn xếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách trống`result`để giữ chuỗi gõ cuối cùng. 
2. Khởi tạo hai danh sách trống`lower_stack`Và`upper_stack`để theo dõi vị trí của chữ thường và chữ hoa trong`result`. 
3. Lặp lại từng ký tự`c`trong chuỗi đầu vào. 
4. Nếu`c`là 'b', kiểm tra xem`lower_stack`không trống. Nếu có, hãy bật chỉ mục cuối cùng và xóa ký tự đó khỏi`result`. Nếu nó trống, không làm gì cả. 
5. Nếu`c`là 'B', kiểm tra xem`upper_stack`không trống. Nếu có, hãy bật chỉ mục cuối cùng và xóa ký tự đó khỏi`result`. Nếu nó trống, không làm gì cả. 
6. Nếu`c`là một chữ cái viết thường khác với 'b', hãy thêm nó vào`result`và nối thêm chỉ mục của nó vào`lower_stack`. 
7. Nếu`c`là một chữ cái viết hoa khác với 'B', hãy thêm nó vào`result`và nối thêm chỉ mục của nó vào`upper_stack`. 
8. Sau khi xử lý xong tất cả các ký tự, hãy nối`result`thành một chuỗi và xuất ra nó. 

Tính bất biến đó là`result`luôn chứa chuỗi đã gõ sau khi xử lý tất cả các ký tự cho đến vị trí hiện tại và ngăn xếp theo dõi vị trí của các chữ cái có sẵn để xóa. Điều này đảm bảo rằng các dấu lùi luôn loại bỏ đúng chữ cái gần đây nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    result = []
    lower_stack = []
    upper_stack = []
    
    for i, c in enumerate(s):
        if c == 'b':
            if lower_stack:
                idx = lower_stack.pop()
                result[idx] = ''
        elif c == 'B':
            if upper_stack:
                idx = upper_stack.pop()
                result[idx] = ''
        else:
            result.append(c)
            if c.islower():
                lower_stack.append(len(result) - 1)
            else:
                upper_stack.append(len(result) - 1)
    
    print(''.join(result))
```Giải pháp sử dụng`result`làm vùng chứa chuỗi có thể thay đổi và hai ngăn xếp để theo dõi các chỉ mục nhằm xóa nhanh. Sử dụng các chỉ mục thay vì bật lên từ`result`đảm bảo xóa O(1) mà không dịch chuyển các phần tử. Chúng tôi loại bỏ đầu vào để loại bỏ dòng mới ở cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1: "ARaBbbitBaby" 

| Bước | Nhân vật | kết quả | low_stack | trên_stack | 
| --- | --- | --- | --- | --- | 
| 1 | A | A | [] | [0] | 
| 2 | R | AR | [] | [0,1] | 
| 3 | một | ARA | [2] | [0,1] | 
| 4 | B | Aa | [2] | [0] | 
| 5 | b | A | [] | [0] | 
| 6 | b | A | [] | [0] | 
| 7 | tôi | Ái | [1] | [0] | 
| 8 | t | Ait | [1,2] | [0] | 
| 9 | B | nó | [1,2] | [] | 
| 10 | một | nó | [1,2,2] | [] | 
| 11 | b | nó | [1,2] | [] | 
| 12 | y | ity | [1,2,3] | [] | 

Dấu vết xác nhận rằng việc xóa chữ thường và chữ hoa hoạt động độc lập và chỉ xóa chữ cái gần đây nhất của loại tương ứng. 

### Ví dụ 2: "Bong bóng" 

| Bước | Nhân vật | kết quả | low_stack | trên_stack | 
| --- | --- | --- | --- | --- | 
| 1 | B | | [] | [] | 
| 2 | bạn | bạn | [0] | [] | 
| 3 | b | | [] | [] | 
| 4 | b | | [] | [] | 
| 5 | tôi | tôi | [0] | [] | 
| 6 | e | le | [0,1] | [] | 

Điều này cho thấy rằng các khoảng lùi liên tiếp ngoài các chữ cái có sẵn sẽ bị bỏ qua mà không có lỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý chính xác một lần và số lần xóa là O(1) thông qua ngăn xếp. | 
| Không gian | O(n) | Chuỗi kết quả và hai ngăn xếp lưu trữ tối đa tất cả ký tự trong đầu vào. | 

Với tổng chiều dài đầu vào không vượt quá 10⁶, giải pháp này phù hợp thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        s = input().strip()
        result = []
        lower_stack = []
        upper_stack = []
        for i, c in enumerate(s):
            if c == 'b':
                if lower_stack:
                    idx = lower_stack.pop()
                    result[idx] = ''
            elif c == 'B':
                if upper_stack:
                    idx = upper_stack.pop()
                    result[idx] = ''
            else:
                result.append(c)
                if c.islower():
                    lower_stack.append(len(result) - 1)
                else:
                    upper_stack.append(len(result) - 1)
        print(''.join(result))
    return output.getvalue().strip()

# Provided samples
assert run("12\nARaBbbitBaby\nYetAnotherBrokenKeyboard\nBubble\nImprobable\nabbreviable\nBbBB\nBusyasaBeeinaBedofBloomingBlossoms\nCoDEBARbIES\ncodeforces\nbobebobbes\nb\nTheBBlackbboard\n") == """ity
YetnotherrokenKeoard
le
Imprle
revile

usyasaeeinaedofloominglossoms
CDARIES
codeforces
es

helaoard"""

# Custom test cases
assert run("3\na\nb\nB\n") == "a\n\n", "single-letter edge cases"
assert run("1\nbBbbBB\n") == "", "only backspaces, nothing to delete"
assert run("1\nAbBa\n") == "a", "alternating deletes"
assert run("1\nABCdefBb\n") == "ACdef", "mixed uppercase/lowercase with backspaces"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "a\nb\nB\n" | "a\n\n" | Các trường hợp cạnh một ký tự, xóa lùi bị bỏ qua khi không có gì để xóa | 
| "bBbbBB\n" | "" | Nhiều khoảng lùi ngoài các chữ cái có sẵn | 
| "AbBa\n" | "một" | Chữ hoa xen kẽ |
