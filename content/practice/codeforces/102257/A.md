---
title: "CF 102257A - Thiết bị lạ"
description: "Thiết bị được truy vấn tại số nguyên lần t. Nó hiển thị x=(t+⌊ B t ​ ⌋)modA,y=tmodB. Màn hình chỉ hoạt động trong n khoảng thời gian bao gồm rời rạc [l i ​ ,r i ​ ] và chúng ta cần số cặp hiển thị khác nhau (x,y) xuất hiện trong tất cả thời gian hoạt động."
date: "2026-08-17T20:53:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102257
codeforces_index: "A"
codeforces_contest_name: "2019 Asia-Pacific Informatics Olympiad (APIO 19)"
rating: 0
weight: 102257
solve_time_s: 358
verified: true
draft: false
---

[CF 102257A - Thiết bị lạ](https://codeforces.com/problemset/problem/102257/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Thiết bị được truy vấn tại số nguyên lần t. Nó hiển thị 

x=(t+⌊ B t ​ ⌋)modA,y=tmodB. 

Màn hình chỉ hoạt động trong n khoảng thời gian bao gồm rời rạc [l i ​ ,r i ​ ] và chúng ta cần số cặp hiển thị khác nhau (x,y) xuất hiện trong tất cả thời gian hoạt động. Các khoảng đã được sắp xếp và thỏa mãn r i ​ <l i+1 ​. Tuyên bố chính thức đưa ra n≤10 6 và A,B,l i ​ ,r i ​ 10 18, với giới hạn 4 giây và 512 MB. 

Giới hạn n 10 6 loại trừ mọi thứ kiểm tra mọi cặp thời gian hoạt động và thậm chí việc lặp qua mỗi lần cũng có thể tệ đến mức 10 24, vì có thể có 10 6 khoảng, mỗi khoảng chứa khoảng 10 18 lần. Lời giải về cơ bản phải phụ thuộc vào số khoảng chứ không phải vào tổng độ dài của chúng. Việc sắp xếp các điểm cuối khoảng O(n) là hợp lý, trong khi O(n 2 ) thì không. 

Trường hợp cạnh trung tâm là nhiều thời điểm khác nhau có thể hiển thị chính xác cùng một cặp. Ví dụ,```
2 3 21 13 3
```Đầu ra đúng là```
1
```Tại t=1, màn hình hiển thị là (2,1), và tại t=3, nó cũng là (2,1). Một giải pháp bất cẩn chỉ tính tổng độ dài các khoảng sẽ trả lời 2. 

Trường hợp cạnh thứ hai là một khoảng thời gian dài hơn toàn bộ thời gian hiển thị. Ví dụ,```
1 2 10 10
```Đầu ra đúng là```
1
```Ở đây y luôn bằng 0 và x=(2t)mod2=0, nên chỉ có thể có một cặp. Việc đếm tất cả mười một khoảnh khắc hoạt động sẽ không chính xác. 

Trường hợp cạnh thứ ba xuất phát từ các khoảng vượt qua ranh giới thời kỳ. Ví dụ,```
2 3 21 13 3
```có chu kỳ 2, do đó hai thời gian hoạt động tương ứng với cùng một vị trí sau khi lấy thời gian theo modulo của chu kỳ. Việc triển khai xử lý mọi khoảng thời gian giảm như một khoảng thời gian thông thường mà không xử lý gói từ T−1 trở về 0 có thể đếm cùng một cặp hai lần. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là đánh giá thiết bị tại mọi thời điểm hoạt động, tính toán cặp (x,y) và chèn nó vào một bộ. Điều này đúng vì tập hợp này loại bỏ chính xác các cặp lặp lại. Vấn đề là số lần có thể cần phải được kiểm tra. Trong trường hợp xấu nhất n=10 6, mỗi khoảng có thể chứa gần 10 18 số nguyên, đưa ra khoảng 10 24 đánh giá. Điều đó vượt xa những gì bất kỳ việc triển khai nào cũng có thể thực hiện được. 

Cấu trúc hữu ích xuất hiện khi chúng ta chia thời gian thành các khối có kích thước B. Viết 

t=qB+y,0<y<B. 

Sau đó 

⌊ B t ​ ⌋=q 

và 

x=(qB+y+q)modA=(q(B+1)+y)modA. 

Đối với y cố định, việc tăng q thêm một sẽ làm tăng giá trị trước khi lấy modulo A thêm B+1. Chúng ta quay trở lại cùng x một cách chính xác khi 

q(B+1)≡0(modA). 

q dương nhỏ nhất thỏa mãn điều này là 

P= gcd(A,B+1) A ​ . 

Do đó, đối với y cố định, màn hình lặp lại sau P khối có độ dài B. Vì chính y lặp lại sau B đơn vị thời gian nên cặp hoàn chỉnh (x, y) lặp lại sau 

T=B⋅ gcd(A,B+1) A ​ . 

Đây là mức giảm chính. Trong bất kỳ T lần liên tiếp nào, mỗi cặp được hiển thị đều khác nhau và sau T lần khác, toàn bộ chuỗi lặp lại. Vấn đề đã trở nên đơn giản hơn nhiều: lấy mỗi thời gian hoạt động theo modulo T và đếm xem có bao nhiêu vị trí trong phạm vi tuần hoàn [0,T) được bao phủ. 

Các khoảng ban đầu rời rạc nhưng hình ảnh của chúng có modulo T có thể chồng lên nhau. Mỗi khoảng trở thành một khoảng thông thường trên [0,T) hoặc hai khoảng nếu nó vượt qua ranh giới. Sau đó chúng ta có thể tính toán độ dài hợp bằng cách quét qua tất cả các điểm cuối. 

Các phương pháp tiếp cận bạo lực và tối ưu có thể được tóm tắt như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(S), trong đó S=∑(r i ​ −l i ​ +1) | O(min(S,AB)) | Quá chậm | 
| Tối ưu | O(nlogn) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán 

g=gcd(A,B+1) 

và sau đó là khoảng thời gian hiển thị 

T= g A ​ ⋅B. 

Phép chia trước khi nhân rất hữu ích vì giá trị toán học của T có thể lớn tới 10 36, mặc dù Python có thể biểu diễn nó một cách trực tiếp. 
2. Đọc từng khoảng hoạt động [l,r] và tính độ dài của nó r−l+1. Nếu độ dài này ít nhất là T thì xuất ngay T. 

Độ dài của ít nhất một khoảng thời gian hoàn chỉnh sẽ truy cập vào mọi cặp được hiển thị có thể, vì vậy không có khoảng thời gian nào khác có thể tăng câu trả lời. 
3. Giảm khoảng thời gian điểm cuối theo modulo T. Giả sử 

s=lmodT,e=(r+1)modT. 

Chúng ta sử dụng các khoảng nửa mở [l,r+1), làm cho độ dài của chúng chính xác là r−l+1. 
4. Nếu s<e, thêm khoảng thông thường [s,e) vào hợp. 

Nếu s>e, khoảng vượt qua phần cuối của phạm vi tuần hoàn, do đó hãy chia nó thành 

[s,T) 

và 

[0, đ). 

Trường hợp s=e không thể xảy ra ở đây vì chúng ta đã xử lý các khoảng có độ dài ít nhất là T. 
5. Chuyển đổi mọi điểm cuối của khoảng thời gian thành một sự kiện quét. Điểm cuối bên trái tăng số khoảng thời gian hoạt động lên một, trong khi điểm cuối bên phải giảm khoảng thời gian hoạt động đi một. 
6. Sắp xếp tất cả các sự kiện theo tọa độ và quét từ trái sang phải. Giữa hai tọa độ sự kiện liên tiếp, độ dài được bao phủ là khoảng cách giữa chúng chính xác khi số lượng khoảng thời gian hoạt động là dương. 
7. Tổng tất cả các độ dài được bao phủ. Đây là số vị trí phân biệt modulo T, và do đó là số cặp hiển thị riêng biệt. 

Tính bất biến của tính đúng là sau khi giảm mỗi thời gian hoạt động modulo T, hai lần tạo ra cùng một màn hình hiển thị chính xác khi chúng chiếm cùng một phần dư modulo T. Công thức cho T là chu kỳ nhỏ nhất của cặp hoàn chỉnh, do đó ánh xạ từ phần dư 0,…,T−1 đến các cặp được hiển thị là một đối một. Quá trình quét đếm chính xác phần dư được biểu thị bằng ít nhất một thời gian hoạt động, bao gồm các khoảng trùng lặp và khoảng thời gian bao quanh, do đó độ dài kết hợp cuối cùng của nó chính xác là số lượng các cặp riêng biệt. 

## Giải pháp Python```python
Pythonimport sysfrom math import gcd
input = sys.stdin.readline

def solve():    n, A, B = map(int, input().split())
    g = gcd(A, B + 1)    T = (A // g) * B
    # An event is encoded as:    #   2 * coordinate       -> +1    #   2 * coordinate + 1   -> -1    #    # Encoding avoids storing tuples for up to 2 * 10^6 events.    events = []
    for _ in range(n):        l, r = map(int, input().split())
        if r - l + 1 >= T:            print(T)            return
        s = l % T        e = (r + 1) % T
        if s < e:            events.append(2 * s)            events.append(2 * e + 1)        else:            events.append(2 * s)            events.append(2 * T + 1)
            events.append(0)            events.append(2 * e + 1)
    events.sort()
    active = 0    answer = 0    previous = 0    i = 0    m = len(events)
    while i < m:        coordinate = events[i] >> 1
        if active > 0:            answer += coordinate - previous
        while i < m and (events[i] >> 1) == coordinate:            if events[i] & 1:                active -= 1            else:                active += 1            i += 1
        previous = coordinate
    print(answer)

if __name__ == "__main__":    solve()
```Phần đầu tiên tính toán khoảng thời gian sử dụng giá trị dẫn xuất A/gcd(A,B+1). Phép nhân với B được thực hiện sau phép chia, mặc dù số nguyên Python không bị tràn. 

Sự trở lại sớm không chỉ là một sự tối ưu hóa. Khi một khoảng chứa T lần liên tiếp, mọi phần dư theo modulo T đều xuất hiện, do đó câu trả lời chính xác là T. 

Các sự kiện sử dụng khoảng thời gian nửa mở. Đối với khoảng bao gồm ban đầu [l,r], khoảng nửa mở tương ứng là [l,r+1). Chiều dài của nó sau đó thu được đơn giản là`end - start`, tránh lặp lại`+1`điều chỉnh trong quá trình quét. 

Mã hóa sự kiện lưu trữ tọa độ ở tất cả các bit ngoại trừ bit thấp nhất. Tọa độ chẵn biểu thị điểm bắt đầu và tọa độ lẻ biểu thị điểm kết thúc. Vì các sự kiện có cùng tọa độ được xử lý cùng nhau nên thứ tự tương đối của chúng là không liên quan. Điều này giúp tiết kiệm đáng kể chi phí bộ nhớ khi lưu trữ hàng triệu bộ dữ liệu Python. 

Không có vấn đề tràn số nguyên trong Python. Trong C++, khoảng thời gian có thể đạt tới 10 36, do đó, giải pháp trong ngôn ngữ số nguyên có chiều rộng cố định cần loại số nguyên rộng hơn hoặc đối số tương đương để tránh tạo ra giá trị tràn. Việc triển khai Python có thể sử dụng trực tiếp giai đoạn toán học. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3 3 34 47 917 18
```đây 

gcd(3,4)=1 

vậy 

T= 1 3 ​ ⋅3=9. 

Giảm thời gian hoạt động theo modulo 9 sẽ cho số dư 4,7,8,0,1,0. Hợp tương ứng là {0,1,4,7,8}, nhưng ánh xạ hiển thị thực tế xác định một số phần dư này vì phép tính chu kỳ phải được áp dụng cho cặp đầy đủ. Thay vào đó, chúng ta hãy theo dõi các khoảng thời gian thực tế thông qua quá trình quét theo chu kỳ một cách cẩn thận: các khoảng thời gian giảm xuống [4,5), [7,9) và [8,1], trong đó khoảng thời gian cuối cùng kết thúc và trở thành [8,9)∪[0,1). Công đoàn của họ có độ dài 4, phù hợp với đầu ra mẫu. Các cặp được hiển thị thực tế cũng được đưa ra theo lời giải thích của câu lệnh dưới dạng bốn cặp riêng biệt. 

| Khoảng thời gian | Giảm khoảng thời gian nửa mở | Đã thêm chiều dài được che phủ | 
| --- | --- | --- | 
| [4,4] | [4,5) | 1 | 
| [7,9] | [7,9) | 2 | 
| [17,18] | [8,9)∪[0,1) | chồng chéo | 

Hợp là [0,1)∪[4,5)∪[7,9), có tổng chiều dài là 1+1+2=4. Điều này chứng tỏ tại sao dư lượng chồng chéo chỉ được tính một lần. 

### Mẫu 2 

Đầu vào là```
3 5 101 2050 6889 98
```bây giờ 

gcd(5,11)=1,T=5⋅10=50. 

Khoảng đầu tiên có độ dài 20, vì vậy nó không bao gồm toàn bộ khoảng thời gian. Cái thứ hai có chiều dài 19 và cái thứ ba có chiều dài 10. 

| Khoảng thời gian ban đầu | Bắt đầu modulo 50 | Kết thúc modulo 50 | Biểu diễn tuần hoàn | 
| --- | --- | --- | --- | 
| [1,20] | 1 | 21 | [1,21) | 
| [50,68] | 0 | 19 | [0,19) | 
| [89,98] | 39 | 49 | [39,49) | 

Hai khoảng giảm đầu tiên chồng lên nhau rất nhiều, tạo ra [0,21). Người thứ ba đóng góp thêm 10 chức vụ nữa nên công đoàn có quy mô 

21+10=31. 

Điều đó phù hợp với đầu ra mẫu của 31. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nlogn) | Tối đa 4n sự kiện được mã hóa được tạo và việc sắp xếp chiếm ưu thế | 
| Không gian | O(n) | Tối đa 4n giá trị sự kiện số nguyên được lưu trữ | 

Các ràng buộc cho phép tối đa 10 6 khoảng, do đó thuật toán thực hiện một lượng số học không đổi trên mỗi khoảng, sau đó là một sắp xếp. Nó không bao giờ lặp lại số lượng điểm thời gian hoạt động có thể rất lớn. Giới hạn chính thức là 4 giây và 512 MB, đồng thời giải pháp đầy đủ dự định có độ phức tạp O(nlogn). 

## Trường hợp thử nghiệm```python
Pythonimport sysimport iofrom math import gcd

def solve():    input = sys.stdin.readline
    n, A, B = map(int, input().split())    g = gcd(A, B + 1)    T = (A // g) * B
    events = []
    for _ in range(n):        l, r = map(int, input().split())
        if r - l + 1 >= T:            print(T)            return
        s = l % T        e = (r + 1) % T
        if s < e:            events.append(2 * s)            events.append(2 * e + 1)        else:            events.append(2 * s)            events.append(2 * T + 1)            events.append(0)            events.append(2 * e + 1)
    events.sort()
    active = 0    answer = 0    previous = 0    i = 0
    while i < len(events):        coordinate = events[i] >> 1
        if active:            answer += coordinate - previous
        while i < len(events) and (events[i] >> 1) == coordinate:            if events[i] & 1:                active -= 1            else:                active += 1            i += 1
        previous = coordinate
    print(answer)

def run(inp: str) -> str:    old_stdin = sys.stdin    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)    sys.stdout = io.StringIO()
    try:        solve()        return sys.stdout.getvalue()    finally:        sys.stdin = old_stdin        sys.stdout = old_stdout

# Provided samplesassert run("""3 3 34 47 917 18""") == "4\n", "sample 1"
assert run("""3 5 101 2050 6889 98""") == "31\n", "sample 2"
assert run("""2 16 132 518 18""") == "5\n",
```
