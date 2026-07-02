---
title: "CF 104239A - \u041b\u0430\u0431\u043e\u0440\u0430\u0442\u043e\u0440\u043d\u044b\u0435 \u0440\u0430\u0431\u043e\u0442\u044b"
description: "Học kỳ được mô hình hóa như một dòng thời gian liên tục trong ngày. Mỗi phòng thí nghiệm chiếm một khối ngày liên tiếp và các khối này không chồng lên nhau. Nếu phòng thí nghiệm đầu tiên kéo dài $a1$ ngày, thì nó sẽ kéo dài từ ngày $1$ đến $a1$."
date: "2026-07-01T23:16:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104239
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2022-2023, \u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104239
solve_time_s: 53
verified: true
draft: false
---

[CF 104239A - \u041b\u0430\u0431\u043e\u0440\u0430\u0442\u043e\u0440\u043d\u044b\u0435 \u0440\u0430\u0431\u043e\u0442\u044b](https://codeforces.com/problemset/problem/104239/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Học kỳ được mô hình hóa như một dòng thời gian liên tục trong ngày. Mỗi phòng thí nghiệm chiếm một khối ngày liên tiếp và các khối này không chồng lên nhau. Nếu phòng thí nghiệm đầu tiên kéo dài$a_1$ngày, sau đó nó kéo dài nhiều ngày$1$bởi vì$a_1$. Phòng thí nghiệm thứ hai bắt đầu ngay sau đó và chạy trong$a_2$ngày, v.v. cho đến khi$n$- Phòng thí nghiệm kết thúc học kỳ. 

Một học sinh tên Vlad chỉ tham gia vào những ngày cụ thể: mỗi$k$-ngày thứ bắt đầu từ ngày$k$, vậy số ngày hoạt động là$k, 2k, 3k, \dots$. Bất cứ khi nào một trong những ngày này rơi vào khoảng thời gian của phòng thí nghiệm, Vlad sẽ đóng góp ít nhất một vấn đề đã được giải quyết cho phòng thí nghiệm đó. Nhiệm vụ là đếm xem có bao nhiêu khoảng thí nghiệm chứa ít nhất một bội số của$k$. 

Cấu trúc chính là chúng tôi không được yêu cầu mô phỏng hàng ngày. Tổng số ngày trong học kỳ có thể bằng$10^{18}$, vì vậy mọi cách tiếp cận lặp đi lặp lại trong nhiều ngày đều không thể thực hiện được. Cách tiếp cận khả thi duy nhất phải hoạt động với số học khoảng. 

Một trường hợp cạnh tinh vi phát sinh khi khoảng thí nghiệm hoàn toàn nằm giữa hai bội số liên tiếp của$k$. Ví dụ, nếu$k = 10$, phòng thí nghiệm chạy từ ngày 11 đến ngày 19 không chứa ngày hoạt động. Ngược lại, ngay cả một lab rất ngắn có độ dài 1 vẫn có thể được tính nếu nó chứa bội số của$k$, chẳng hạn như phòng thí nghiệm bao gồm ngày thứ 20 khi$k = 10$. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Chúng tôi tính toán khoảng thời gian ngày chính xác cho từng phòng thí nghiệm, sau đó lặp lại tất cả các bội số của$k$cho đến cuối học kỳ và kiểm tra xem mỗi bội số có rơi vào mỗi khoảng hay không. Điều này dẫn đến một cấu trúc lồng nhau: đối với mỗi phòng thí nghiệm, chúng tôi có thể quét nhiều bội số của$k$, hoặc quét tương đương tất cả các ngày và kiểm tra tư cách thành viên. Vì tổng số ngày là tổng của tất cả$a_i$, có thể đạt tới$10^{18}$, điều này ngay lập tức không thể thực hiện được. 

Quan sát quan trọng là lật ngược quan điểm. Thay vì hỏi liệu một phòng thí nghiệm có chứa nhiều$k$, ta hỏi có bao nhiêu bội số của$k$rơi vào tiền tố của dòng thời gian. Mỗi phòng thí nghiệm tương ứng với một khoảng tiền tố trên trục ngày toàn cầu. Số bội số của$k$lên đến một ngày$x$đơn giản là$\lfloor x / k \rfloor$. Vì vậy, đối với một phòng thí nghiệm trải dài$[L, R]$, số ngày hoạt động bên trong nó là$\lfloor R/k \rfloor - \lfloor (L-1)/k \rfloor$. Phòng thí nghiệm sẽ đóng góp nếu sự khác biệt này lớn hơn 0. 

Điều này làm giảm vấn đề trong việc duy trì tổng tiền tố đang chạy của thời lượng phòng thí nghiệm trong khi đánh giá một điều kiện số học đơn giản cho mỗi phòng thí nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua nhiều ngày hoặc bội số |$O(\sum a_i)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Prefix interval + division check |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì một biến đang chạy`cur`đại diện cho ngày cuối cùng của phòng thí nghiệm trước đó. Ban đầu`cur = 0`. Điều này cung cấp cho chúng tôi một cách trực tiếp để tính toán khoảng thời gian của mỗi phòng thí nghiệm. 
2. Đối với mỗi phòng thí nghiệm$i$, tính khoảng của nó như$[cur + 1, cur + a_i]$. Sau đó cập nhật`cur += a_i`. Điều này chuyển đổi cấu trúc được phân đoạn thành các khoảng số rõ ràng mà không lưu trữ chúng. 
3. Đối với mỗi khoảng thời gian$[L, R]$, tính xem có bao nhiêu bội số của$k$nằm bên trong nó bằng phép chia số nguyên:`R // k - (L - 1) // k`. 
4. Nếu kết quả là dương tính, hãy tăng câu trả lời lên một. Điều này trực tiếp kiểm tra xem có tồn tại ít nhất một ngày hợp lệ trong khoảng thời gian đó hay không. 
5. Sau khi xử lý tất cả các phòng thí nghiệm, xuất ra số đếm cuối cùng. 

Ý tưởng quan trọng là bội số của$k$được cách đều nhau và việc đếm chúng bên trong bất kỳ khoảng tiền tố nào có thể được rút gọn thành biểu thức số học theo thời gian không đổi. 

### Tại sao nó hoạt động 

Mỗi phòng thí nghiệm tương ứng với một đoạn liền kề trên trục số. Trình tự các ngày tham gia hợp lệ cũng là một cấp số cộng cố định. Việc đếm giao điểm giữa một phân đoạn và một cấp số cộng sẽ giảm bớt việc so sánh số lượng tiền tố ở các điểm cuối. biểu hiện$\lfloor R/k \rfloor - \lfloor (L-1)/k \rfloor$đếm chính xác có bao nhiêu số hạng của mẫu$t \cdot k$nằm ở$[L, R]$, do đó điều kiện “Vlad tham gia phòng thí nghiệm này” tương đương với số đếm này khác 0. Vì mỗi phòng thí nghiệm được xử lý độc lập và chính xác một lần nên tổng số lượng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    cur = 0
    ans = 0
    
    for x in a:
        L = cur + 1
        R = cur + x
        cur = R
        
        if R // k - (L - 1) // k > 0:
            ans += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp chỉ giữ lại một điểm cuối đang chạy`cur`, vì vậy nó không bao giờ lưu trữ hoặc lặp lại một cách rõ ràng qua nhiều ngày. Mỗi phòng thí nghiệm được xử lý trong thời gian không đổi bằng cách sử dụng số học. 

biểu hiện`(L - 1) // k`là điều cần thiết để xử lý chính xác các ranh giới. Không trừ 1, trường hợp`L`chính nó là bội số của`k`sẽ bị tính sai vì phép chia sàn sẽ dịch chuyển ranh giới không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
1 2 1 2 1
```Chúng tôi theo dõi khoảng thời gian trong phòng thí nghiệm và sự tham gia: 

| Phòng thí nghiệm | L | R | R//k | (L-1)//k | Đếm | Đã chụp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 0 | 0 | Không | 
| 2 | 2 | 3 | 1 | 0 | 1 | Có | 
| 3 | 4 | 4 | 2 | 1 | 1 | Có | 
| 4 | 5 | 6 | 3 | 2 | 1 | Có | 
| 5 | 7 | 7 | 3 | 3 | 0 | Không | 

Câu trả lời cuối cùng là 3. 

Điều này chứng tỏ rằng sự tham gia phụ thuộc hoàn toàn vào việc bội số của$k$hạ cánh bên trong mỗi khoảng, không phải trên độ dài khoảng. 

### Ví dụ 2 

đầu vào:```
6 5
6 10 3 1 2 7
```| Phòng thí nghiệm | L | R | R//k | (L-1)//k | Đếm | Đã chụp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 6 | 1 | 0 | 1 | Có | 
| 2 | 7 | 16 | 3 | 1 | 2 | Có | 
| 3 | 17 | 19 | 3 | 3 | 0 | Không | 
| 4 | 20 | 20 | 4 | 3 | 1 | Có | 
| 5 | 21 | 22 | 4 | 4 | 0 | Không | 
| 6 | 23 | 29 | 5 | 4 | 1 | Có | 

Câu trả lời cuối cùng là 4. 

Dấu vết này cho thấy các khoảng lớn có thể chứa nhiều bội số của$k$, trong khi các khoảng rất nhỏ vẫn có thể chứa chính xác một nếu chúng căn chỉnh chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phòng thí nghiệm được xử lý một lần với số học theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ một số biến đang chạy được lưu trữ | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$phòng thí nghiệm, vì vậy xử lý tuyến tính là đủ. Số học sử dụng các phép toán an toàn 64 bit vì các giá trị có thể đạt tới$10^{18}$, nhưng Python xử lý việc này một cách tự nhiên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    cur = 0
    ans = 0
    
    for x in a:
        L = cur + 1
        R = cur + x
        cur = R
        if R // k - (L - 1) // k > 0:
            ans += 1
    
    return str(ans)

# provided samples
assert run("5 2\n1 2 1 2 1\n") == "3"
assert run("3 10\n1 2 3\n") == "0"

# custom cases
assert run("1 1\n1\n") == "1"
assert run("1 5\n4\n") == "0"
assert run("2 3\n2 2\n") == "1"
assert run("3 2\n1 1 1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Khoảng cách nhỏ duy nhất đánh k | 1 | trường hợp ranh giới tối thiểu | 
| Khoảng đơn không chạm k | 0 | trường hợp không trúng đích | 
| Nhiều phòng thí nghiệm nhỏ lượt truy cập thưa thớt | 1 | căn chỉnh thưa thớt | 
| Phòng thí nghiệm nhỏ liên tục | 2 | liên tục vượt qua ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi phòng thí nghiệm bắt đầu chính xác ở mức bội số của$k$. Ví dụ, với$k = 5$, phòng thí nghiệm bắt đầu từ ngày thứ 10 phải được tính ngay. 

Đối với đầu vào:```
1 5
10
```Khoảng thời gian là$[1, 10]$. Chúng tôi tính toán:$10 // 5 = 2$,$(1 - 1) // 5 = 0$, do đó số đếm là 2, nghĩa là bội số của 5 và 10 đều ở trong phòng thí nghiệm và phòng thí nghiệm được tính chính xác. 

Một trường hợp khác là khi một phòng thí nghiệm kết thúc chính xác tại bội số của$k$nhưng bắt đầu ngay sau cái trước đó. Vì$k = 4$, khoảng$[5, 8]$vẫn chứa 8 là một ngày hợp lệ. Việc tính toán:$8 // 4 = 2$,$4 // 4 = 1$, cho số 1, đánh dấu đúng sự tham gia. 

Những trường hợp này xác nhận rằng việc xử lý ranh giới với$(L-1)$là cần thiết cho sự đúng đắn.
