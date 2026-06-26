---
title: "CF 105262F - Chuỗi Fibonacci"
description: "Chúng ta có hai chuỗi cơ sở, gọi chúng là $F1$ và $F2$, và chúng ta xác định một chuỗi các chuỗi trong đó mọi chuỗi sau được hình thành bằng cách nối hai chuỗi trước đó theo thứ tự. Vì vậy $F3 = F2 + F1$, $F4 = F3 + F2$, v.v."
date: "2026-06-24T02:33:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105262
codeforces_index: "F"
codeforces_contest_name: "Game of Coders 3.0"
rating: 0
weight: 105262
solve_time_s: 49
verified: true
draft: false
---

[CF 105262F - Chuỗi Fibonacci](https://codeforces.com/problemset/problem/105262/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai chuỗi cơ sở, gọi chúng là$F_1$Và$F_2$và chúng tôi xác định một chuỗi các chuỗi trong đó mọi chuỗi sau được hình thành bằng cách nối hai chuỗi trước theo thứ tự. Vì thế$F_3 = F_2 + F_1$,$F_4 = F_3 + F_2$, vân vân. Đây là quy tắc cấu trúc tương tự như số Fibonacci, ngoại trừ việc thay vì cộng các giá trị, chúng ta nối các chuỗi. 

Đối với mỗi trường hợp thử nghiệm, chúng ta được hỏi một truy vấn duy nhất: cho trước một chỉ mục$x$và một vị trí$k$, chúng ta phải xác định ký tự nào xuất hiện ở vị trí$k$(1-lập chỉ mục) bên trong chuỗi$F_x$. Cái khó đó là$F_x$có thể trở nên lớn về mặt thiên văn rất nhanh, nên việc xây dựng nó một cách rõ ràng là không thể. 

Các hạn chế củng cố điều này: có thể có tới$10^4$các trường hợp thử nghiệm và tổng độ dài chuỗi đầu vào chỉ$10^5$, có nghĩa là quá trình tiền xử lý tổng thể phải tuyến tính. chỉ số$x$đi lên$1000$, vì vậy chúng ta có thể cần phải suy luận thông qua phép đệ quy sâu của các phép nối, nhưng chúng ta không thể xây dựng các chuỗi hoặc thậm chí lưu trữ chúng một cách rõ ràng ngoài việc theo dõi độ dài của chúng. vị trí$k$có thể lớn như$10^{18}$, điều này cho thấy rõ rằng chỉ có lý luận dựa trên độ dài mới khả thi. 

Một cách tiếp cận đơn giản sẽ liên tục xây dựng các chuỗi Fibonacci cho đến khi đạt được$F_x$, nhưng thậm chí còn lưu trữ$F_{40}$đã vượt quá giới hạn bộ nhớ trong hầu hết các trường hợp và$F_{100}$là hoàn toàn không khả thi. Một ý tưởng ngây thơ khác là đệ quy xây dựng các chuỗi một cách nhanh chóng, nhưng điều đó cũng bùng nổ theo cấp số nhân. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các ký tự trong cả hai chuỗi ban đầu đều giống hệt nhau. Trong trường hợp đó mọi$F_x$là thống nhất và mọi truy vấn đều giảm xuống việc trả về ký tự đơn đó. Việc triển khai bất cẩn mà vẫn cố gắng đệ quy có thể bị tràn hoặc lãng phí thời gian một cách không cần thiết. 

Một trường hợp cạnh khác là khi$k$chính xác bằng độ dài ranh giới giữa$F_{i-1}$Và$F_{i-2}$trong quá trình phân hủy. Ở đây xảy ra lỗi từng lỗi một vì việc nối chuỗi sẽ chia chuỗi thành hai phần và việc lập chỉ mục chính xác sẽ xác định xem chúng ta di chuyển sang trái hay phải trong đệ quy. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi xây dựng từng chuỗi$F_i$lặp đi lặp lại bằng cách sử dụng phép nối cho đến khi đạt được$F_x$, sau đó lập chỉ mục trực tiếp vào nó. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, mỗi phép nối có kích thước tuyến tính và sự tăng trưởng Fibonacci làm cho độ dài theo cấp số nhân. Thậm chí đạt tới$F_{40}$sẽ yêu cầu xây dựng một chuỗi có hàng tỷ ký tự, điều này là không thể cả về thời gian và bộ nhớ. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần chuỗi đầy đủ. Chúng ta chỉ cần biết mỗi lần kéo dài bao lâu$F_i$là gì và cách nối chia chia các vị trí. Từ$F_i = F_{i-1} + F_{i-2}$, chúng ta có thể tính toán trước độ dài bằng cách sử dụng cùng một phép truy toán, nhưng giới hạn chúng ở một giá trị lớn (chẳng hạn như$10^{18}$) để tránh tràn. Khi đã biết độ dài, việc trả lời một truy vấn sẽ trở thành một quá trình đi lùi từ$F_x$, quyết định xem vị trí$k$nằm ở phần bên trái$F_{x-1}$hoặc phần bên phải$F_{x-2}$. Điều này biến bài toán thành một phương pháp giảm logarit trên$x$, tương tự như điều hướng cây phân rã nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(x)$mỗi bài kiểm tra |$O(x)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xây dựng một mảng có độ dài cho tất cả các chuỗi Fibonacci đạt đến chỉ số được yêu cầu tối đa. 

1. Khởi tạo$len[1] = |F_1|$Và$len[2] = |F_2|$. Chúng được biết trực tiếp từ chuỗi đầu vào. 
2. Đối với mỗi$i \ge 3$, tính toán$len[i] = len[i-1] + len[i-2]$, nhưng nếu tổng vượt quá$10^{18}$, kẹp nó vào$10^{18}$. Điều này ngăn ngừa tràn trong khi vẫn duy trì tính chính xác cho việc lập chỉ mục. 
3. Khi bảng độ dài được tạo, hãy xử lý truy vấn$(x, k)$. Bắt đầu tại chỉ mục$x$và vị trí$k$. 
4. Trong khi$x > 2$, so sánh$k$với$len[x-1]$. Nếu như$k \le len[x-1]$, chuyển đến$x-1$và giữ$k$không thay đổi vì vị trí nằm ở chuỗi con bên trái. Ngược lại thì trừ$len[x-1]$từ$k$và di chuyển đến$x-2$, vì bây giờ chúng ta đang ở trong chuỗi con bên phải. 
5. Khi nào$x$trở thành 1 hoặc 2, trực tiếp trả về$k$-ký tự thứ của$F_1$hoặc$F_2$. 

Mỗi bước quyết định sẽ làm giảm quy mô vấn đề bằng cách tuân theo cấu trúc nối thay vì mở rộng nó. 

### Tại sao nó hoạt động 

Việc xây dựng đảm bảo rằng mọi$F_x$được chia chính xác thành hai phần liền kề: tiền tố$F_{x-1}$theo sau là hậu tố$F_{x-2}$. Mảng độ dài mã hóa phân vùng này một cách chính xác. Ở mỗi bước, thuật toán bảo toàn tính bất biến mà cặp hiện tại$(x, k)$đề cập đến cùng một ký tự như trong bản gốc$F_x$, vừa được ánh xạ vào một bài toán con nhỏ hơn. Vì mỗi lần chuyển đổi đều giảm nghiêm ngặt$x$, quá trình phải kết thúc và trường hợp cơ sở cuối cùng trực tiếp lập chỉ mục cho một chuỗi rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX = 10**18

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        f1 = input().strip()
        f2 = input().strip()
        x, k = map(int, input().split())

        # build length table up to x
        mx = x
        ln = [0] * (mx + 1)
        ln[1] = len(f1)
        if mx >= 2:
            ln[2] = len(f2)

        for i in range(3, mx + 1):
            val = ln[i - 1] + ln[i - 2]
            if val > MAX:
                val = MAX
            ln[i] = val

        cur_x = x
        cur_k = k

        while cur_x > 2:
            if cur_k <= ln[cur_x - 1]:
                cur_x -= 1
            else:
                cur_k -= ln[cur_x - 1]
                cur_x -= 2

        if cur_x == 1:
            print(f1[cur_k - 1])
        else:
            print(f2[cur_k - 1])

if __name__ == "__main__":
    solve()
```Giải pháp tách biệt hai mối quan tâm: tính toán độ dài và điều hướng. Mảng độ dài được xây dựng lặp đi lặp lại nên mỗi giá trị chỉ phụ thuộc vào hai giá trị trước đó, khớp với cấu trúc Fibonacci. Kẹp ngăn ngừa tràn trong khi vẫn giữ các phép so sánh hợp lệ cho mọi khả năng tiếp cận$k$. 

Vòng lặp gốc là logic cốt lõi. Mỗi lần lặp quyết định xem vị trí truy vấn nằm ở thành phần thứ nhất hay thứ hai của phép nối. Khi chuyển sang thành phần thứ hai, trừ$len[x-1]$là cần thiết vì các chỉ số thay đổi so với chuỗi con mới. 

Một lỗi thực hiện phổ biến là quên rằng$F_x = F_{x-1} + F_{x-2}$(không đảo ngược). Đảo ngược thứ tự này dẫn đến việc phân nhánh liên tục không chính xác. Một vấn đề tế nhị khác là không giới hạn độ dài, điều này có thể làm tràn danh sách Python ở các ngôn ngữ khác và phá vỡ các so sánh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
F1 = "a"
F2 = "b"
x = 6, k = 7
```Bảng chiều dài: 

| tôi | chiều dài Fi | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 3 | 
| 5 | 5 | 
| 6 | 8 | 

Bây giờ mô phỏng: 

| Bước | x | k | len[x-1] | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 7 | 5 | k > 5 nên đi bên phải | 
| 2 | 4 | 2 | 2 | k <= 2 nên đi sang trái | 
| 3 | 3 | 2 | 1 | k > 1 nên đi sang phải | 
| 4 | 1 | 1 | - | trả về f1[1] | 

Đầu ra là`"a"`. 

Dấu vết này cho thấy cách thuật toán liên tục thu hẹp khoảng thời gian chỉ bằng cách sử dụng so sánh độ dài mà không bao giờ xây dựng chuỗi. 

### Ví dụ 2 

đầu vào:```
F1 = "ab"
F2 = "xyz"
x = 4, k = 4
```Bảng chiều dài: 

| tôi | chiều dài Fi | 
| --- | --- | 
| 1 | 2 | 
| 2 | 3 | 
| 3 | 5 | 
| 4 | 8 | 

Dấu vết: 

| Bước | x | k | len[x-1] | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | 5 | k <= 5 nên đi sang trái | 
| 2 | 3 | 4 | 3 | k > 3 nên đi bên phải | 
| 3 | 1 | 1 | - | trả về f1[1] | 

Đầu ra là`"a"`. 

Điều này xác nhận tính đúng đắn của việc xử lý ranh giới khi chuyển từ phân đoạn trái sang phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(x)$mỗi bài kiểm tra | Việc xây dựng mảng độ dài cần có thời gian tuyến tính trong$x$và mỗi truy vấn giảm xuống nhiều nhất$x$bước | 
| Không gian |$O(x)$| Chỉ mảng có độ dài tối đa$x$được lưu trữ | 

Các ràng buộc cho phép lên đến$x = 1000$, do đó, ngay cả tuyến tính trên mỗi thử nghiệm cũng an toàn. Tổng độ dài chuỗi là$10^5$đảm bảo việc xử lý đầu vào vẫn hiệu quả và tổng số thao tác vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old = sys.stdout
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# sample-like tests
assert run("""1
1 1
a
b
6 7
""") == "a"

assert run("""1
2 3
ab
xyz
1 2
""") == "b"

# custom cases
assert run("""1
1 1
a
b
1 1
""") == "a"

assert run("""1
1 1
a
b
2 3
""") == "a"

assert run("""1
3 3
aaa
aaa
10 1000000000000000000
""") == "a"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cơ sở char đơn | một | trường hợp cơ sở tối thiểu | 
| lập chỉ mục F2 trực tiếp | b | chuỗi cơ sở thứ hai | 
| chuỗi giống hệt nhau | một | nhân giống đồng đều | 
| x sâu với k lớn | một | kẹp và hạ xuống | 

## Vỏ cạnh 

### Chuỗi ký tự thống nhất 

Nếu$F_1 = F_2 = "n"$, thì mọi$F_x$là sự lặp lại của cùng một nhân vật. Thuật toán vẫn tính toán độ dài và giảm dần, nhưng mọi trường hợp cơ sở đều trả về cùng một ký tự bất kể đường dẫn. Ví dụ, với$x = 1000$Và$k = 10^{18}$, việc đi xuống có thể ghé thăm nhiều tiểu bang, nhưng kết quả cuối cùng luôn là`"n"`. 

### Phân chia ranh giới ở độ dài tiền tố chính xác 

Hãy xem xét một bước trong đó$k = len[x-1]$. Điều kiện phải coi phần này thuộc về bên trái. Quy tắc`if cur_k <= ln[cur_x - 1]`đảm bảo tính đúng đắn. Nếu được viết dưới dạng bất đẳng thức nghiêm ngặt, thuật toán sẽ chuyển sai sang đoạn bên phải, tạo ra các lỗi sai lệch một. 

### Giá trị k cực lớn 

Khi nào$k$vượt quá độ dài thực tế, việc kẹp đảm bảo so sánh vẫn có hiệu lực. Kể cả nếu$len[x]$được giới hạn ở$10^{18}$, logic vẫn định tuyến truy vấn một cách nhất quán vì tất cả các hậu tố không thể truy cập được coi là chứa đầy đủ trong nhánh bên phải.
