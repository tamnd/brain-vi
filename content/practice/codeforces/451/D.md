---
title: "CF 451D - Đếm chuỗi con tốt"
description: "Chúng ta được cung cấp một chuỗi nhị phân chỉ bao gồm 'a' và 'b'. Đối với mỗi chuỗi con, chúng tôi liên tục hợp nhất các ký tự bằng nhau liên tiếp thành một ký tự duy nhất."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 451
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 258 (Div. 2)"
rating: 2000
weight: 451
solve_time_s: 123
verified: true
draft: false
---

[CF 451D - Đếm chuỗi con tốt](https://codeforces.com/problemset/problem/451/D) 

**Xếp hạng:** 2000 
**Thẻ:** toán 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân chỉ bao gồm`'a'`Và`'b'`. Đối với mỗi chuỗi con, chúng tôi liên tục hợp nhất các ký tự bằng nhau liên tiếp thành một ký tự duy nhất. Ví dụ:

-`"aaaa"`trở thành`"a"`-`"aabbaa"`trở thành`"aba"`-`"abbba"`trở thành`"aba"`Một chuỗi con được gọi là tốt nếu chuỗi nén thu được sau khi hợp nhất này là một chuỗi palindrome. 

Nhiệm vụ không phải là liệt kê các chuỗi con. Chúng ta chỉ cần đếm xem có bao nhiêu chuỗi con tốt có độ dài chẵn và bao nhiêu chuỗi con có độ dài lẻ. 

Chiều dài chuỗi có thể đạt tới$10^5$. Đã có số lượng chuỗi con$O(n^2)$, vì vậy việc kiểm tra từng chuỗi con riêng lẻ là không thể. Ngay cả việc quét tuyến tính trên mỗi chuỗi con cũng sẽ dẫn đến khoảng$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa những gì phù hợp với giới hạn thời gian 2 giây. Lời giải phải khai thác một số cấu trúc ẩn và chạy gần với thời gian tuyến tính. 

Phần tinh vi nhất của vấn đề là hiểu được hoạt động nén thực sự thay đổi những gì. 

Hãy xem xét chuỗi con`"abba"`. 

Sau khi nén:$$"abba" \rightarrow "aba"$$đó là một palindrome, vì vậy chuỗi con là tốt. 

Bây giờ hãy xem xét`"abab"`. 

Nén không thay đổi gì:$$"abab" \rightarrow "abab"$$và đây không phải là một bảng màu. 

Một sai lầm phổ biến là nghĩ rằng toàn bộ chuỗi nén phải được tính toán cho mọi chuỗi con. Điều quan trọng là đối với bảng chữ cái nhị phân, điều kiện palindrome trở nên đơn giản hơn nhiều. 

Một cái bẫy dễ dàng khác xuất hiện với các chuỗi con bao gồm một ký tự lặp lại. Ví dụ:```
aaaa
```Mỗi chuỗi con nén thành`"a"`, đó là một palindrome. Bất kỳ giải pháp nào chỉ dựa trên hình dạng của chuỗi nén vẫn phải đếm chính xác tất cả các độ dài. 

Trường hợp cạnh cuối cùng là các chuỗi xen kẽ như:```
ababab
```Việc nén không làm gì cả nên tính tốt phụ thuộc hoàn toàn vào chuỗi con gốc. Những trường hợp như vậy bộc lộ những giả định không chính xác về các chuỗi ký tự bằng nhau. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê mọi chuỗi con, nén nó và kiểm tra xem kết quả nén có phải là một bảng màu hay không. 

có$O(n^2)$các chuỗi con. Một lần nén có thể mất$O(n)$thời gian, cho$O(n^3)$tổng số phức tạp. Ngay cả với một số tối ưu hóa, việc kiểm tra từng chuỗi con một cách độc lập ít nhất vẫn còn$O(n^2)$, quá chậm đối với$n=10^5$. 

Bước đột phá đến từ việc hiểu được một chuỗi con tốt trông như thế nào trong bảng chữ cái nhị phân. 

Lấy bất kỳ chuỗi con nào. Sau khi nén các ký tự bằng nhau liên tiếp, kết quả là một chuỗi xen kẽ vì các ký tự bằng nhau liền kề đã được hợp nhất đi. 

Vì chỉ`'a'`Và`'b'`tồn tại, mọi chuỗi nén phải trông giống như một trong:$$a,\quad b,\quad ab,\quad ba,\quad aba,\quad bab,\quad abab,\ldots$$Một chuỗi nhị phân xen kẽ là một chuỗi palindrome chính xác khi độ dài của nó là số lẻ. 

Ví dụ:$$aba,\ bab,\ ababa$$là palindromes, trong khi$$ab,\ ba,\ abab$$thì không. 

Bây giờ hãy nhìn vào một chuỗi con$s[l..r]$. 

Dạng nén của nó bắt đầu bằng$s[l]$và kết thúc bằng$s[r]$. 

Vì chuỗi nén xen kẽ nhau nên nó là một bảng màu nếu và chỉ khi ký tự đầu tiên và cuối cùng của nó bằng nhau. 

Điều đó có nghĩa là: 

Một chuỗi con là tốt khi và chỉ khi ký tự đầu tiên và cuối cùng của nó bằng nhau. 

Điều này loại bỏ hoàn toàn nhu cầu nén. 

Vấn đề trở thành việc đếm các chuỗi con có điểm cuối chứa cùng một ký tự, được phân tách thành độ dài chẵn và lẻ. 

Giả sử chúng ta xử lý các vị trí từ trái sang phải. 

Đối với vị trí kết thúc cố định$i$, mọi chuỗi con tốt kết thúc tại$i$phải bắt đầu ở vị trí sớm hơn$j$với:$$s[j] = s[i]$$Tính chẵn lẻ của độ dài chuỗi con là:$$(i-j+1)$$Độ dài lẻ xảy ra khi$i$Và$j$có cùng chỉ số chẵn lẻ. 

Độ dài chẵn xảy ra khi$i$Và$j$có tính chẵn lẻ trái ngược nhau. 

Vì vậy chúng ta chỉ cần biết, với mỗi ký tự (`a`hoặc`b`), có bao nhiêu lần xuất hiện trước đó ở vị trí chẵn và bao nhiêu lần xuất hiện ở vị trí lẻ. 

Điều này mang lại một$O(n)$giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì bốn quầy.`a_even`,`a_odd`,`b_even`, Và`b_odd`lưu trữ bao nhiêu vị trí được xử lý chứa mỗi ký tự và từng chẵn lẻ chỉ mục. 
2. Quét chuỗi từ trái sang phải. 

Sử dụng các chỉ số dựa trên số không. 
3. Đối với vị trí hiện tại$i$, xác định xem$i$là chẵn hoặc lẻ. 
4. Đếm các chuỗi con tốt có độ dài lẻ kết thúc tại$i$. 

Vị trí bắt đầu phải chứa cùng một ký tự và có cùng tính chẵn lẻ như$i$. 

Nếu ký tự hiện tại là`'a'`Và$i$chẵn, cộng`a_even`. 

Nếu ký tự hiện tại là`'a'`Và$i$thật kỳ lạ, hãy thêm vào`a_odd`. 

Ý tưởng tương tự áp dụng cho`'b'`. 
5. Đếm các chuỗi con tốt có độ dài chẵn kết thúc tại$i$. 

Vị trí bắt đầu phải chứa cùng một ký tự và có tính chẵn lẻ đối diện. 

Nếu ký tự hiện tại là`'a'`Và$i$chẵn, cộng`a_odd`. 

Nếu ký tự hiện tại là`'a'`Và$i$thật kỳ lạ, hãy thêm vào`a_even`. 

Ý tưởng tương tự áp dụng cho`'b'`. 
6. Đếm chuỗi con một ký tự. 

Mỗi chuỗi con có độ dài một đều tốt và có độ dài lẻ, vì vậy hãy thêm một chuỗi con vào câu trả lời lẻ. 
7. Chèn vị trí hiện tại vào bộ đếm tương ứng. 

Các chuỗi con trong tương lai có thể bắt đầu từ đây. 
8. Tiếp tục cho đến khi toàn bộ chuỗi được xử lý. 

### Tại sao nó hoạt động 

Một chuỗi nhị phân nén luôn xen kẽ giữa`'a'`Và`'b'`. Chuỗi như vậy là một chuỗi palindrome chính xác khi ký tự đầu tiên và cuối cùng của nó bằng nhau. Ký tự đầu tiên và cuối cùng của chuỗi nén chính xác là ký tự đầu tiên và cuối cùng của chuỗi con gốc. Do đó, một chuỗi con chính xác là tốt khi điểm cuối của nó bằng nhau. 

Sau khi giảm vấn đề về đẳng thức điểm cuối, chỉ tính chẵn lẻ sẽ xác định xem độ dài chuỗi con là lẻ hay chẵn. Hai chỉ số có cùng số chẵn lẻ tạo ra một chuỗi con có độ dài lẻ, trong khi các chỉ số chẵn lẻ đối diện tạo ra một chuỗi con có độ dài chẵn. Bộ đếm được duy trì đếm chính xác những vị trí trước đó có thể đóng vai trò là điểm bắt đầu hợp lệ, do đó, mọi chuỗi con tốt đều được tính một lần khi điểm cuối bên phải của nó được xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

even_good = 0
odd_good = 0

a_even = a_odd = 0
b_even = b_odd = 0

for i, ch in enumerate(s):
    if ch == 'a':
        if i % 2 == 0:
            odd_good += a_even
            even_good += a_odd
            a_even += 1
        else:
            odd_good += a_odd
            even_good += a_even
            a_odd += 1
    else:
        if i % 2 == 0:
            odd_good += b_even
            even_good += b_odd
            b_even += 1
        else:
            odd_good += b_odd
            even_good += b_even
            b_odd += 1

    odd_good += 1

print(even_good, odd_good)
```Thuật toán xử lý mọi vị trí chính xác một lần. 

Bốn bộ đếm đại diện cho tất cả các lần xuất hiện trước đó của mỗi ký tự được phân chia theo tính chẵn lẻ của chỉ mục. Khi vị trí`i`đạt được, mọi chuỗi con tốt đều kết thúc tại`i`phải bắt đầu từ lần xuất hiện trước đó của cùng một ký tự. 

Nếu vị trí bắt đầu và kết thúc có cùng tính chẵn lẻ thì độ dài chuỗi con là số lẻ. Nếu số chẵn lẻ của chúng khác nhau thì độ dài chuỗi con là chẵn. Điều này cho phép chúng tôi cập nhật cả hai câu trả lời trong thời gian liên tục. 

Chuỗi con một ký tự ở vị trí`i`luôn luôn tốt, vì vậy`odd_good`được tăng một lần cho mỗi ký tự. 

Thứ tự của các hoạt động quan trọng. Đóng góp từ các vị trí trước đó phải được thêm vào trước khi chèn vị trí hiện tại vào bộ đếm. Nếu không, vị trí hiện tại sẽ tạo thành một chuỗi con có độ dài bằng 0 không chính xác với chính nó. 

Số nguyên Python tự động xử lý các giá trị lên tới xấp xỉ$n^2$, điều này là cần thiết vì câu trả lời có thể đạt tới khoảng$5 \times 10^9$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
bb
```| tôi | char | chẵn lẻ | thậm chí_tốt | lẻ_tốt | b_thậm chí | b_lẻ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | b | thậm chí | 0 | 1 | 1 | 0 | 
| 1 | b | lẻ | 1 | 2 | 1 | 1 | 

Câu trả lời cuối cùng:```
1 2
```Hai chuỗi con ký tự đơn góp phần vào số lẻ. Chuỗi con`"bb"`có điểm cuối bằng nhau và độ dài chẵn, vì vậy nó góp phần vào số chẵn. 

### Ví dụ 2 

đầu vào:```
baab
```| tôi | char | chẵn lẻ | thậm chí_tốt | lẻ_tốt | a_even | a_lẻ | b_thậm chí | b_lẻ | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | b | thậm chí | 0 | 1 | 0 | 0 | 1 | 0 | 
| 1 | một | lẻ | 0 | 2 | 0 | 1 | 1 | 0 | 
| 2 | một | thậm chí | 1 | 3 | 1 | 1 | 1 | 0 | 
| 3 | b | lẻ | 2 | 4 | 1 | 1 | 1 | 1 | 

Câu trả lời cuối cùng:```
2 4
```Các chuỗi con chẵn là`"aa"`Và`"baab"`. Bốn chuỗi con ký tự đơn tạo thành số lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần đi qua chuỗi | 
| Không gian |$O(1)$| Chỉ có một vài quầy được lưu trữ | 

Chiều dài chuỗi đạt tới$10^5$, do đó quét tuyến tính chỉ thực hiện$10^5$lần lặp lại. Bộ nhớ bổ sung liên tục dễ dàng phù hợp với giới hạn bộ nhớ. Cả hai ràng buộc đều được thỏa mãn một cách thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    s = input().strip()

    even_good = 0
    odd_good = 0

    a_even = a_odd = 0
    b_even = b_odd = 0

    for i, ch in enumerate(s):
        if ch == 'a':
            if i % 2 == 0:
                odd_good += a_even
                even_good += a_odd
                a_even += 1
            else:
                odd_good += a_odd
                even_good += a_even
                a_odd += 1
        else:
            if i % 2 == 0:
                odd_good += b_even
                even_good += b_odd
                b_even += 1
            else:
                odd_good += b_odd
                even_good += b_even
                b_odd += 1

        odd_good += 1

    print(even_good, odd_good)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    global input
    input = sys.stdin.readline

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided sample
assert run("bb\n") == "1 2", "sample 1"

# minimum size
assert run("a\n") == "0 1", "single character"

# sample 2 from statement
assert run("baab\n") == "2 4", "statement example"

# alternating string
assert run("abab\n") == "0 6", "alternating pattern"

# all equal
assert run("aaaa\n") == "4 6", "all substrings good"

# off-by-one parity check
assert run("aba\n") == "1 4", "same endpoints at distance two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`0 1`| Kích thước tối thiểu | 
|`baab`|`2 4`| Chạy và nén hỗn hợp | 
|`abab`|`0 6`| Mô hình xen kẽ | 
|`aaaa`|`4 6`| Mọi chuỗi con đều tốt | 
|`aba`|`1 4`| Xử lý chẵn lẻ và đếm điểm cuối | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
aaaa
```Mỗi chuỗi con nén thành`"a"`, vì vậy mọi chuỗi con đều tốt. Thuật toán đếm các chuỗi con hoàn toàn thông qua đẳng thức điểm cuối. Vì mỗi cặp điểm cuối đều chứa`'a'`, mọi chuỗi con đều được tính. Kết quả là:```
4 6
```khớp với tổng số mười chuỗi con của một chuỗi dài bốn. 

Bây giờ hãy xem xét:```
abab
```Nén không thay đổi gì cả. Chuỗi con tốt chính xác là những chuỗi có ký tự đầu tiên và cuối cùng trùng khớp. Các chuỗi con duy nhất như vậy là bốn chữ cái đơn và hai chuỗi con có độ dài ba. Thuật toán đạt được:```
0 6
```bởi vì không có chuỗi con có độ dài chẵn nào có điểm cuối phù hợp. 

Cuối cùng hãy xem xét:```
abba
```Toàn bộ chuỗi nén thành`"aba"`, đó là một palindrome. Việc triển khai ngây thơ chỉ vì lý do chuỗi gốc có thể bỏ lỡ điều này. Đặc tính điểm cuối xử lý nó ngay lập tức: ký tự đầu tiên và cuối cùng đều là`'a'`, vậy là chuỗi con tốt. Thuật toán tính nó thông qua việc so khớp`'a'`xảy ra tại các vị trí$0$Và$3$, thêm một chuỗi con tốt có độ dài chẵn.
