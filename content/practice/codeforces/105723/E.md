---
title: "CF 105723E - Aloy và Bộ luật Cấm"
description: "Chúng ta được cấp một chuỗi chỉ gồm các ký tự a, b và c. Chuỗi được đảm bảo không có hai ký tự bằng nhau cạnh nhau và cả ba ký tự đều xuất hiện ít nhất một lần ở đâu đó trong chuỗi."
date: "2026-06-22T04:44:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105723
codeforces_index: "E"
codeforces_contest_name: "MTB Presents AUST Inter University Programming Contest 2025"
rating: 0
weight: 105723
solve_time_s: 47
verified: true
draft: false
---

[CF 105723E - Aloy và Bộ luật bị cấm](https://codeforces.com/problemset/problem/105723/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chỉ gồm các ký tự`a`,`b`, Và`c`. Chuỗi được đảm bảo không có hai ký tự bằng nhau cạnh nhau và cả ba ký tự đều xuất hiện ít nhất một lần ở đâu đó trong chuỗi. 

Nhiệm vụ là tìm đoạn liền kề ngắn nhất của chuỗi này chứa ít nhất một`a`, ít nhất một`b`, và ít nhất một`c`. Nói cách khác, trong số tất cả các chuỗi con, chúng ta muốn có độ dài tối thiểu của một cửa sổ “bao phủ” cả ba ký hiệu. 

Kích thước đầu vào có thể lên tới 100000 ký tự. Điều đó ngay lập tức loại trừ bất kỳ điều gì cố gắng kiểm tra tất cả các chuỗi con một cách rõ ràng, vì việc liệt kê tất cả các chuỗi con là bậc hai trong trường hợp xấu nhất và sẽ cần khoảng 10^10 thao tác. Ngay cả một vòng lặp lồng nhau trên các cửa sổ hợp lệ cũng sẽ quá chậm. 

Ràng buộc kề, tức là không có hai ký tự lân cận nào bằng nhau, có vai trò quan trọng về mặt cấu trúc nhưng không thực sự cần thiết để đảm bảo tính chính xác. Nó chủ yếu ngăn ngừa các trường hợp bệnh lý như tình trạng tương tự kéo dài. Tuy nhiên, ngay cả khi không có nó, vấn đề cốt lõi vẫn như cũ: chúng ta cần một cửa sổ tối thiểu chứa ba ký hiệu riêng biệt. 

Một số tình huống khó khăn đáng được nêu ra. 

Nếu chuỗi đã giống như`abc`, câu trả lời là 3, vì toàn bộ chuỗi là chuỗi con hợp lệ nhỏ nhất có thể. Một cách tiếp cận ngây thơ, đặt lại cửa sổ không chính xác quá mạnh có thể bỏ lỡ điều đó. 

Ví dụ: nếu các ký tự được trải ra`abacbc`, chuỗi con tối ưu có thể kết thúc ngay tại nơi xuất hiện ký tự bị thiếu cuối cùng. Quá trình quét tham lam ngây thơ khởi động lại quá sớm có thể bỏ lỡ các cửa sổ ngắn hơn bắt đầu bên trong các phân đoạn hợp lệ trước đó. 

Một trường hợp tinh tế khác là khi nhiều chuỗi con hợp lệ chồng chéo lên nhau. Ví dụ`abcbac`, một số cửa sổ chứa cả ba ký tự và ký tự tối thiểu không nhất thiết phải được căn chỉnh với các ranh giới rõ ràng. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ kiểm tra mọi chuỗi con có thể và đối với mỗi chuỗi con xem liệu`a`,`b`, Và`c`tất cả đều xuất hiện. Có các chuỗi con O(n^2) và mỗi lần kiểm tra có giá O(n) nếu được thực hiện trực tiếp hoặc O(1) với các bảng tần số được tính toán trước nhưng nhìn chung vẫn là O(n^2). Với n lên tới 100000, điều này vượt xa giới hạn khả thi. 

Chúng ta có thể cải thiện điều này bằng cách lưu ý rằng chúng ta không cần phải xem xét tất cả các chuỗi con một cách độc lập. Thay vào đó, chúng ta chỉ quan tâm đến các cửa sổ kết thúc ở một vị trí cụ thể. Nếu chúng ta sửa điểm cuối bên phải thì điểm cuối bên trái tốt nhất sẽ được xác định bởi vị trí mới nhất mà mỗi ký tự xuất hiện. 

Tại bất kỳ chỉ số nào`i`, giả sử chúng ta biết vị trí nhìn thấy lần cuối của`a`,`b`, Và`c`lên đến`i`. Nếu cả ba đều xuất hiện ít nhất một lần thì bất kỳ chuỗi con hợp lệ nào kết thúc tại`i`phải bắt đầu không muộn hơn vị trí nhỏ nhất trong số các vị trí được nhìn thấy lần cuối này. Bắt đầu sớm hơn sẽ bao gồm các ký tự không cần thiết và bắt đầu muộn hơn sẽ loại bỏ một trong các ký tự bắt buộc. Điều này đưa ra một cách trực tiếp để tính toán cửa sổ tốt nhất kết thúc ở mỗi vị trí trong O(1) và do đó đưa ra câu trả lời đầy đủ trong O(n). 

Điều này chuyển đổi vấn đề từ việc liệt kê các chuỗi con sang việc duy trì ba con trỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải trong khi vẫn duy trì chỉ mục gần đây nhất nơi mỗi ký tự xuất hiện. 

1. Khởi tạo ba biến`last_a`,`last_b`, Và`last_c`để chỉ ra rằng chưa có nhân vật nào được nhìn thấy. Chúng tôi cũng giữ một biến`ans`được khởi tạo với số lượng rất lớn. Điều này đại diện cho câu trả lời tốt nhất được tìm thấy cho đến nay. 
2. Lặp qua chuỗi có chỉ mục`i`. Ở mỗi bước, cập nhật vị trí nhìn thấy lần cuối tương ứng tùy thuộc vào việc`s[i]`là`a`,`b`, hoặc`c`. Điều này đảm bảo rằng tại bất kỳ thời điểm nào chúng ta cũng có được sự xuất hiện gần đây nhất của mỗi ký tự ở vị trí`i`. 
3. Sau khi cập nhật, hãy kiểm tra xem cả ba vị trí nhìn thấy lần cuối có hợp lệ hay không. Nếu bất kỳ chuỗi nào trong số chúng vẫn chưa được khởi tạo, chúng ta không thể tạo chuỗi con hợp lệ kết thúc tại`i`, vì vậy chúng ta bỏ qua phần còn lại của bước này. 
4. Nếu cả ba ký tự đã được nhìn thấy, hãy tính vị trí sớm nhất trong ba vị trí được nhìn thấy lần cuối. Vị trí này trở thành ranh giới bên trái của chuỗi con hợp lệ nhỏ nhất kết thúc tại`i`vẫn chứa cả ba ký tự. 
5. Tính độ dài của chuỗi con này là`i - min(last_a, last_b, last_c) + 1`, và cập nhật`ans`nếu độ dài này nhỏ hơn độ dài hiện tại là tốt nhất. 
6. Tiếp tục cho đến hết chuỗi. Câu trả lời cuối cùng là giá trị ghi tối thiểu. 

Tại sao đây là ranh giới bên trái chính xác là điểm mấu chốt. Bất kỳ chuỗi con nào kết thúc tại`i`phải bao gồm ít nhất một lần xuất hiện của mỗi ký tự. Lần xuất hiện gần nhất của mỗi ký tự trước hoặc tại`i`là ứng cử viên duy nhất đảm bảo được đưa vào mà không cần gia hạn một cách không cần thiết. Cửa sổ hợp lệ nhỏ nhất buộc phải bắt đầu sớm nhất trong số những lần xuất hiện cuối cùng bắt buộc này. 

### Tại sao nó hoạt động 

Tại mọi chỉ số`i`, thuật toán ngầm xem xét chuỗi con tốt nhất có thể kết thúc tại`i`. Tính bất biến đó là`last_a`,`last_b`, Và`last_c`luôn đại diện cho các vị trí gần đây nhất của các nhân vật tương ứng của họ cho đến`i`. Bất kỳ chuỗi con hợp lệ nào kết thúc tại`i`phải bao gồm ít nhất một lần xuất hiện của mỗi ký tự và do đó phải bắt đầu tại hoặc trước mỗi lần xuất hiện cuối cùng này. Sự khởi đầu chặt chẽ nhất có thể là mức tối thiểu trong số đó. Điều này đảm bảo rằng không có chuỗi con hợp lệ kết thúc tại`i`có thể ngắn hơn cái được xây dựng, do đó lấy mức tối thiểu trên tất cả`i`mang lại kết quả tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    
    last = {'a': -1, 'b': -1, 'c': -1}
    ans = 10**18
    
    for i, ch in enumerate(s):
        last[ch] = i
        
        if last['a'] != -1 and last['b'] != -1 and last['c'] != -1:
            start = min(last['a'], last['b'], last['c'])
            ans = min(ans, i - start + 1)
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện ý tưởng theo dõi các vị trí được nhìn thấy lần cuối. Từ điển`last`lưu trữ các chỉ số cho mỗi ký tự. Mỗi khi tiến về phía trước, chúng tôi cập nhật một mục nhập và ngay lập tức kiểm tra xem cả ba ký tự đã được nhìn thấy hay chưa. 

Chi tiết quan trọng là tính toán mức tối thiểu của lần xuất hiện cuối cùng. Bước này mã hóa thực tế là cửa sổ phải bao gồm hình thức bắt buộc gần đây nhất của mỗi ký tự. 

Việc xử lý từng cái một được hấp thụ bởi`+1`trong công thức độ dài, vì cả hai điểm cuối đều bao gồm. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`abac`| tôi | s[i] | cuối cùng | b cuối cùng | c cuối cùng | phút(cuối cùng) | cửa sổ bắt đầu | cuối cửa sổ | chiều dài | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | một | 0 | - | - | - | - | - | - | 
| 1 | b | 0 | 1 | - | - | - | - | - | 
| 2 | một | 2 | 1 | - | - | - | - | - | 
| 3 | c | 2 | 1 | 3 | 1 | 1 | 3 | 3 | 

Bảng cho thấy chỉ ở vị trí cuối cùng chúng ta mới có đủ ba ký tự. Lần xuất hiện cuối cùng tối thiểu là chỉ số 1, tạo ra chuỗi con`bac`. 

### Ví dụ 2:`cabac`| tôi | s[i] | cuối cùng | b cuối cùng | c cuối cùng | phút(cuối cùng) | cửa sổ bắt đầu | cuối cửa sổ | chiều dài | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | c | - | - | 0 | - | - | - | - | 
| 1 | một | 1 | - | 0 | - | - | - | - | 
| 2 | b | 1 | 2 | 0 | 0 | 0 | 2 | 3 | 
| 3 | một | 3 | 2 | 0 | 0 | 0 | 3 | 4 | 
| 4 | c | 3 | 2 | 4 | 2 | 2 | 4 | 3 | 

Ở đây câu trả lời tối ưu cũng xuất hiện ở cuối, nhưng một cửa sổ trước đó ở`i = 2`đã đạt được độ dài 3 (`cab`). Cửa sổ sau dài hơn và thuật toán tự nhiên giữ mức tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần với các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ có ba biến vị trí cuối cùng được lưu trữ | 

Quá trình quét tuyến tính nằm trong giới hạn n lên tới 100000. Việc sử dụng bộ nhớ là không đổi và không phụ thuộc vào kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    last = {'a': -1, 'b': -1, 'c': -1}
    ans = 10**18
    
    s = input().strip()
    for i, ch in enumerate(s):
        last[ch] = i
        if last['a'] != -1 and last['b'] != -1 and last['c'] != -1:
            start = min(last['a'], last['b'], last['c'])
            ans = min(ans, i - start + 1)
    
    return str(ans)

# provided sample
assert run("abac\n") == "3"

# minimum length case
assert run("abc\n") == "3"

# symmetric shuffle
assert run("cab\n") == "3"

# longer mixed case
assert run("abcbac\n") == "3"

# alternating spread
assert run("acbabc\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`abc`| 3 | cửa sổ hợp lệ tối thiểu | 
|`cab`| 3 | thứ tự hoán vị | 
|`abcbac`| 3 | chồng chéo các cửa sổ tối ưu | 
|`acbabc`| 3 | thiết lập lại và tái sử dụng nhiều lần | 

## Vỏ cạnh 

Đối với đầu vào rất ngắn như`abc`, thuật toán sẽ cập nhật ngay lập tức tất cả các vị trí cuối cùng và tạo ra một cửa sổ hợp lệ ở chỉ mục cuối cùng. Số lần xuất hiện cuối cùng tối thiểu trở thành 0, vì vậy câu trả lời là 3. 

Đối với các chuỗi có tính xen kẽ cao như`abcbac`, nhiều cửa sổ hợp lệ xuất hiện. Thuật toán không cố gắng chọn một cửa sổ “đầu tiên” duy nhất; nó đánh giá mọi điểm cuối một cách độc lập, đảm bảo rằng các cửa sổ sau này không thể ẩn điểm cuối tốt hơn trước đó. 

Đối với trường hợp một nhân vật xuất hiện muộn, chẳng hạn như`aaabbbccc`nếu hạn chế kề bị loại bỏ, phương thức vẫn theo dõi chính xác các lần xuất hiện cuối cùng và chỉ tạo một cửa sổ hợp lệ sau khi tất cả các ký tự đã được nhìn thấy, sau đó nó liên tục cập nhật câu trả lời bằng cách sử dụng các cửa sổ chặt chẽ hơn kết thúc ở mỗi lần xuất hiện mới.
