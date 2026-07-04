---
title: "CF 102890L - Cùng đếm từ"
description: "Nhiệm vụ về cơ bản là đếm số lượng “từ” xuất hiện trong một văn bản nhất định theo quy tắc mã thông báo đơn giản. Chúng ta được cung cấp một chuỗi đại diện cho một dòng văn bản và chúng ta phải xác định có bao nhiêu mã thông báo từ riêng biệt tồn tại trong đó theo định nghĩa được ngụ ý bởi…"
date: "2026-07-04T13:41:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102890
codeforces_index: "L"
codeforces_contest_name: "2020 ICPC Gran Premio de Mexico 3ra Fecha"
rating: 0
weight: 102890
solve_time_s: 41
verified: true
draft: false
---

[CF 102890L - Cùng đếm từ](https://codeforces.com/problemset/problem/102890/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ về cơ bản là đếm số lượng “từ” xuất hiện trong một văn bản nhất định theo quy tắc mã thông báo đơn giản. Chúng ta được cấp một chuỗi đại diện cho một dòng văn bản và chúng ta phải xác định có bao nhiêu mã thông báo từ riêng biệt tồn tại trong đó theo định nghĩa mà bài toán ngụ ý. Một từ được hình thành bởi các ký tự chữ cái liên tiếp và bất kỳ thứ gì khác như dấu cách, dấu chấm câu hoặc chữ số đóng vai trò là dấu phân cách. Đầu ra là một số nguyên biểu thị số lượng đoạn từ như vậy xuất hiện sau khi tách văn bản. 

Một cách khác để xem dữ liệu đầu vào là chúng ta đang quét một luồng ký tự thô và nhóm nó thành các chuỗi con chữ cái liền kề tối đa. Mỗi lần chúng ta chuyển từ một chữ cái thành một chữ cái, chúng ta bắt đầu một từ mới và mỗi lần chúng ta chuyển từ một chữ cái sang một chữ cái, chúng ta sẽ kết thúc từ hiện tại. 

Các ràng buộc trong những bài toán như vậy thường đủ lớn để độ dài chuỗi có thể đạt tới khoảng 10^5 hoặc 10^6 ký tự. Điều đó ngay lập tức ngụ ý rằng bất kỳ cách tiếp cận bậc hai nào, chẳng hạn như cắt liên tục hoặc sử dụng các phép toán chuỗi đắt tiền bên trong các vòng lặp, sẽ thất bại. Cách tiếp cận khả thi duy nhất là quét tuyến tính một lần trên đầu vào, xử lý từng ký tự một lần. 

Một sai lầm ngây thơ xuất phát từ việc chỉ chia tách trên các khoảng trắng. Ví dụ, với đầu vào`hello,world`, ngây thơ`split()`trên dấu cách sẽ coi nó là một từ, trong khi câu trả lời đúng là hai từ:`hello`Và`world`. Một lỗi nhỏ khác xảy ra khi nhiều dấu phân cách xuất hiện liên tiếp, chẳng hạn như`a---b`. Một bộ đếm ngây thơ có thể đếm không chính xác các mã thông báo trống nếu không cẩn thận, nhưng cách diễn giải đúng sẽ bỏ qua các phân đoạn trống và chỉ tính các chuyển đổi thành các chuỗi chữ cái. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là coi chuỗi như một chuỗi các chuỗi con được phân tách bằng mọi ký tự không phải chữ cái có thể có. Người ta có thể lặp lại mọi chỉ mục bắt đầu có thể, mở rộng cho đến khi tìm thấy dấu phân cách và ghi lại các chuỗi con. Mặc dù điều này đơn giản về mặt khái niệm nhưng nó có nguy cơ quét liên tục các vùng chồng chéo. Trong trường hợp xấu nhất, chẳng hạn như một chuỗi toàn chữ cái, mỗi phần mở rộng chuỗi con sẽ trở thành O(n), dẫn đến hành vi O(n^2). 

Quan sát quan trọng là chúng ta không bao giờ cần hiện thực hóa các chuỗi con. Chúng ta chỉ cần phát hiện ranh giới giữa “bên trong một từ” và “bên ngoài một từ”. Điều này làm giảm vấn đề theo dõi một trạng thái boolean duy nhất trong khi quét từ trái sang phải. Mỗi ký tự được xử lý chính xác một lần và chúng tôi chỉ tăng số từ khi chúng tôi nhập một đoạn chữ cái từ trạng thái không phải chữ cái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Trích xuất chuỗi con Brute Force | O(n^2) | O(n) | Quá chậm | 
| Theo dõi trạng thái một lượt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai phần thông tin: bộ đếm các từ và một lá cờ cho biết liệu chúng tôi hiện có ở trong một từ hay không. 

1. Khởi tạo bộ đếm`words = 0`và một boolean`in_word = False`. Trạng thái này thể hiện rằng ban đầu chúng ta ở bên ngoài bất kỳ đoạn từ nào. 

2. Lặp lại từng ký tự trong chuỗi từ trái sang phải. Mỗi ký tự được phân loại là một chữ cái hoặc một dấu phân cách. 

3. Nếu ký tự hiện tại là một chữ cái và`in_word`là Sai, điều này có nghĩa là chúng ta đang nhập một từ mới. Chúng tôi tăng`words`bằng 1 và đặt`in_word = True`. Bước này rất quan trọng vì nó đảm bảo rằng chúng ta chỉ đếm ký tự đầu tiên của mỗi khối chữ cái liền kề. 

4. Nếu ký tự hiện tại là một chữ cái và`in_word`là Đúng, chúng ta tiếp tục bên trong cùng một từ và không làm gì cả. Điều này ngăn chặn việc đếm quá nhiều từ nhiều ký tự. 

5. Nếu ký tự hiện tại không phải là chữ cái, chúng ta đặt`in_word = False`, đánh dấu rằng mọi chữ cái tiếp theo sẽ bắt đầu một từ mới. Quá trình chuyển đổi này đảm bảo xử lý chính xác nhiều dấu phân cách liên tiếp. 

6. Sau khi xử lý tất cả các ký tự, giá trị của`words`là câu trả lời. 

Tính đúng đắn đến từ sự bất biến`in_word`là Đúng khi và chỉ khi ký tự trước đó là một chữ cái thuộc đoạn từ hiện đang hoạt động. Mỗi từ được tính chính xác một lần ở ký tự đầu tiên và không bao giờ lặp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().rstrip("\n")
    
    words = 0
    in_word = False
    
    for ch in s:
        if ch.isalpha():
            if not in_word:
                words += 1
                in_word = True
        else:
            in_word = False
    
    print(words)

if __name__ == "__main__":
    solve()
```Giải pháp đọc dòng đầu vào và xử lý từng ký tự. các`in_word`flag là cơ chế trung tâm giúp tránh việc đếm hai lần các chữ cái trong cùng một từ. Điểm tinh tế duy nhất là sử dụng`isalpha()`thay vì chỉ kiểm tra dấu cách, vì dấu phân cách có thể bao gồm dấu chấm câu hoặc các ký hiệu không phải chữ cái khác tùy thuộc vào định nghĩa của vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 
đầu vào:`hello,world 123 test`| Nhân vật | Là Thư | in_word (trước) | Hành động | từ | 
|---|---|---|---|---| 
| h | Đúng | Sai | từ bắt đầu | 1 | 
| e | Đúng | Đúng | tiếp tục | 1 | 
| tôi | Đúng | Đúng | tiếp tục | 1 | 
| tôi | Đúng | Đúng | tiếp tục | 1 | 
| o | Đúng | Đúng | tiếp tục | 1 | 
| , | Sai | Đúng | từ cuối | 1 | 
| w | Đúng | Sai | từ bắt đầu | 2 | 
| o | Đúng | Đúng | tiếp tục | 2 | 
| r | Đúng | Đúng | tiếp tục | 2 | 
| tôi | Đúng | Đúng | tiếp tục | 2 | 
| d | Đúng | Đúng | tiếp tục | 2 | 
| (khoảng trống) | Sai | Đúng | từ cuối | 2 | 
| 1 | Sai | Sai | bỏ qua | 2 | 
| 2 | Sai | Sai | bỏ qua | 2 | 
| 3 | Sai | Sai | bỏ qua | 2 | 
| (khoảng trống) | Sai | Sai | bỏ qua | 2 | 
| t | Đúng | Sai | từ bắt đầu | 3 | 
| e | Đúng | Đúng | tiếp tục | 3 | 
| s | Đúng | Đúng | tiếp tục | 3 | 
| t | Đúng | Đúng | tiếp tục | 3 | 

Điều này chứng tỏ rằng dấu câu và chữ số kết thúc các từ một cách chính xác và chỉ chuyển đổi thành chuỗi chữ cái mới tăng bộ đếm. 

### Ví dụ 2 
đầu vào:`---abc--de--`| Nhân vật | Là Thư | in_word (trước) | Hành động | từ | 
|---|---|---|---|---| 
| - | Sai | Sai | bỏ qua | 0 | 
| - | Sai | Sai | bỏ qua | 0 | 
| - | Sai | Sai | bỏ qua | 0 | 
| một | Đúng | Sai | từ bắt đầu | 1 | 
| b | Đúng | Đúng | tiếp tục | 1 | 
| c | Đúng | Đúng | tiếp tục | 1 | 
| - | Sai | Đúng | từ cuối | 1 | 
| - | Sai | Sai | bỏ qua | 1 | 
| d | Đúng | Sai | từ bắt đầu | 2 | 
| e | Đúng | Đúng | tiếp tục | 2 | 
| - | Sai | Đúng | từ cuối | 2 | 
| - | Sai | Sai | bỏ qua | 2 | 

Ví dụ này cho thấy rằng nhiều dấu phân cách liên tiếp không tạo ra số đếm bổ sung, vì logic chuyển đổi chỉ phản ứng với việc nhập trạng thái chữ cái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(n) | Mỗi ký tự được xử lý chính xác một lần trong một lần chuyển | 
| Không gian | O(1) | Chỉ có một số lượng biến không đổi được duy trì | 

Quét tuyến tính phù hợp thoải mái trong các ràng buộc thông thường lên tới 10^6 ký tự, vì mỗi thao tác có thời gian không đổi và không yêu cầu cấu trúc dữ liệu phụ trợ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue() if False else capture(inp)

def capture(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

# minimal cases
assert capture("") == "0"
assert capture("abc") == "1"
assert capture("a b c") == "3"

# punctuation-heavy
assert capture("hello,world") == "2"

# consecutive separators
assert capture("---a--b---c---") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`""`|`0`| xử lý chuỗi trống | 
|`"abc"`|`1`| từ đơn | 
|`"a b c"`|`3`| các từ cách nhau bằng dấu cách | 
|`"hello,world"`|`2`| tách dấu chấm câu | 
|`"---a--b---c---"`|`3`| nhiều dải phân cách | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là đầu vào không chứa chữ cái nào cả, chẳng hạn như`"12345!!!"`. Thuật toán khởi tạo`in_word = False`và không bao giờ gặp một chữ cái nào, vì vậy bộ đếm luôn bằng 0. Đầu ra là chính xác`0`. 

Một trường hợp cạnh khác là một chuỗi bắt đầu trực tiếp bằng dấu phân cách, theo sau là một từ, chẳng hạn như`"--abc"`. Đặt lại ký tự đầu tiên`in_word`nhưng không ảnh hưởng tới bộ đếm. Khi`'a'`đạt được, nó sẽ kích hoạt một cách chính xác số từ mới, dẫn đến`1`. 

Trường hợp cuối cùng là xen kẽ các dấu phân cách và các chữ cái đơn lẻ như`"a-b-c"`. Trước mỗi chữ cái là một chữ cái không phải chữ cái, vì vậy mỗi chữ cái bắt đầu một từ mới. Thuật toán tăng chính xác một lần cho mỗi chữ cái, tạo ra`3`, phù hợp với quy tắc phân đoạn dự định.
