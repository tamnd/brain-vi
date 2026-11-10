---
title: "CF 1537D - Xóa ước số"
description: "Chúng ta bắt đầu với số nguyên dương $n$. Hai người chơi luân phiên nhau, Alice đi trước. Trong mỗi lượt, người chơi hiện tại chọn một ước số thích hợp của số hiện tại, nghĩa là một ước số không phải là $1$ hay chính số đó và trừ nó khỏi số."
date: "2026-06-10T15:04:51+07:00"
tags: ["codeforces", "competitive-programming", "games", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1537
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 726 (Div. 2)"
rating: 1700
weight: 1537
solve_time_s: 150
verified: false
draft: false
---

[CF 1537D - Xóa ước số](https://codeforces.com/problemset/problem/1537/D) 

**Đánh giá:** 1700 
**Tags:** trò chơi, toán học, lý thuyết số 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một số nguyên dương$n$. Hai người chơi luân phiên nhau, Alice đi trước. Ở mỗi lượt, người chơi hiện tại chọn một ước số thích hợp cho số hiện tại, nghĩa là một ước số không$1$cũng không phải chính số đó và trừ nó khỏi số. 

Trò chơi tiếp tục với giá trị được cập nhật. Nếu một người chơi bắt đầu một lượt và không có số chia hợp lệ để trừ thì người chơi đó sẽ thua. 

Đối với mọi trường hợp thử nghiệm, chúng ta phải xác định người chơi nào thắng nếu cả hai đều chơi hoàn hảo. 

Đầu vào bao gồm tối đa$10^4$trò chơi độc lập. Giá trị khởi đầu$n$có thể lớn như$10^9$. Vì có nhiều trường hợp thử nghiệm nên bất kỳ giải pháp nào thực hiện khám phá trạng thái trò chơi rộng rãi cho từng giá trị đều bị nghi ngờ ngay lập tức. Một giải pháp xung quanh$O(\sqrt n)$mỗi trường hợp thử nghiệm là hoàn toàn an toàn vì$\sqrt{10^9} \approx 31623$, trong khi mọi thứ liên quan đến các bảng lập trình động lớn lên đến$n$là không thể. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Khi$n=1$, không có ước số nào cả. Alice thua ngay lập tức. 

đầu vào:```
1
1
```Đầu ra:```
Bob
```Một giải pháp giả định mọi số dương đều có ít nhất một nước đi sẽ thất bại ở đây. 

Số nguyên tố cũng là vị trí cuối cùng. Ví dụ: 

đầu vào:```
1
13
```Đầu ra:```
Bob
```Ước số duy nhất của số nguyên tố là$1$và chính nó, cả hai đều bị cấm. 

Sức mạnh của hai hành xử khác với các số chẵn khác. Ví dụ: 

đầu vào:```
1
8
```Đầu ra:```
Bob
```A naive rule such as "all even numbers are winning" gives the wrong answer.

 số$2$là một trường hợp đặc biệt khác: 

đầu vào:```
1
2
```Đầu ra:```
Bob
```Mặc dù nó là số chẵn nhưng nó không có nước đi hợp lệ vì các ước số duy nhất của nó là$1$Và$2$. 

## Phương pháp tiếp cận 

Ý tưởng tự nhiên đầu tiên là coi trò chơi như một trò chơi khách quan tiêu chuẩn. Với mọi giá trị$x$, chúng ta có thể tạo ra tất cả các nước đi hợp pháp$x \to x-d$, Ở đâu$d$là ước số thích hợp của$x$. Một thế cờ sẽ thắng nếu ít nhất một nước đi dẫn đến thế thua và sẽ thua nếu mọi nước đi đều dẫn đến thế thắng. 

Định nghĩa về lực lượng vũ phu này là chính xác vì nó phù hợp trực tiếp với cách chơi tối ưu. Thật không may, nó không thực tế. Từ$n$có thể đạt được$10^9$, chúng ta không thể xây dựng một bảng DP lên tới$n$. Ngay cả đối với các giới hạn nhỏ hơn nhiều, việc liệt kê liên tục các trạng thái trò chơi cũng trở nên quá tốn kém. 

Nhận xét quan trọng là trò chơi có cấu trúc rất cứng nhắc. Mỗi nước đi đều trừ đi một ước số thích hợp nên số đó luôn giảm. Điều này cho phép chúng ta phân tích các vị thế thắng và thua một cách toán học thay vì tính toán đệ quy. 

Hãy để chúng tôi kiểm tra các giá trị nhỏ: 

| n | Kết quả | 
| --- | --- | 
| 1 | Thua | 
| 2 | Thua | 
| 3 | Thua | 
| 4 | Thắng | 
| 5 | Thua | 
| 6 | Thắng | 
| 7 | Thua | 
| 8 | Thua | 
| 9 | Thua | 
| 10 | Thắng | 

Tất cả các số lẻ đều có vẻ thua. 

Đây không phải là một sự trùng hợp ngẫu nhiên. Nếu như$n$thật lẻ, mọi ước của$n$cũng thật kỳ quặc. Trừ một ước số lẻ từ một số lẻ sẽ tạo ra một số chẵn. Do đó, mọi nước đi từ vị trí lẻ đều đi đến vị trí chẵn. Nếu tất cả các vị trí chẵn có thể truy cập được từ số lẻ đều thắng thì mọi số lẻ đều thua. 

Tiếp theo, hãy xem xét các số chẵn. 

Nếu một số chẵn không phải là lũy thừa của hai thì nó chứa thừa số lẻ. Cho phép$d$là yếu tố kỳ lạ đó. Trừ$d$bảo toàn tính chẵn và tạo ra số lẻ. Vì số lẻ đang mất vị trí nên số chẵn như vậy sẽ thắng. 

Trường hợp khó khăn duy nhất là lũy thừa của hai. 

Cho phép$n=2^k$. 

Mọi ước số thực sự cũng là lũy thừa của hai. Bất kỳ động thái nào trông giống như$$2^k \to 2^k - 2^m$$với$m<k$. 

Bao thanh toán mang lại$$2^k - 2^m = 2^m(2^{k-m}-1).$$Thừa số thứ hai là số lẻ và lớn hơn$1$, do đó kết quả là một số chẵn không phải là lũy thừa của hai. 

Chúng ta đã xác định rằng mọi số chẵn không phải là lũy thừa của hai sẽ thắng. Do đó mọi chuyển động từ$2^k$dẫn đến thế thắng. 

Điều này có nghĩa là lũy thừa của hai xen kẽ giữa thắng và thua theo số mũ chẵn lẻ:$$2^1=2 \text{ is losing}$$

$$2^2=4 \text{ is winning}$$

$$2^3=8 \text{ is losing}$$

$$2^4=16 \text{ is winning}$$và vân vân. 

Đặc tính cuối cùng rất đơn giản: 

Nếu$n$thật kỳ quặc, Bob thắng. 

Nếu như$n$là số chẵn không phải là lũy thừa của hai, Alice thắng. 

Nếu như$n=2^k$, Alice thắng khi$k$chẵn và Bob thắng khi$k$thật kỳ quặc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lượng trạng thái có thể truy cập | Lưu trữ DP/trạng thái lớn | Quá chậm | 
| Tối ưu |$O(\log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị hiện tại$n$. 
2. Nếu$n$là số lẻ, đầu ra`"Bob"`. 

Mọi nước đi từ số lẻ sẽ chuyển sang số chẵn và vị trí lẻ sẽ thua. 
3. Kiểm tra xem$n$là sức mạnh của hai. 

Nhiều lần chia cho$2$trong khi nó vẫn chẵn và đếm xem có bao nhiêu phép chia được thực hiện. 
4. Nếu giá trị còn lại không$1$, sau đó$n$không phải là sức mạnh của hai. 

Những con số như vậy là số chẵn và chứa thừa số lẻ, cho phép di chuyển đến vị trí lẻ thua. đầu ra`"Alice"`. 
5. Nếu không$n=2^k$. 
6. Nếu$k$chẵn, đầu ra`"Alice"`. 
7. Nếu$k$là số lẻ, đầu ra`"Bob"`. 

### Tại sao nó hoạt động 

Toàn bộ giải pháp dựa trên việc phân loại các vị trí thành ba loại. 

Mọi số lẻ đều thua vì mọi nước đi hợp pháp đều dẫn đến số chẵn. 

Mọi số chẵn không phải là lũy thừa của 2 đều thắng vì nó chứa ước số lẻ. Trừ số chia đó sẽ tạo ra một số lẻ, số đó sẽ thua. 

Đối với lũy thừa hai, mỗi nước đi đều dẫn đến một số chẵn không phải là lũy thừa của hai. Vị trí đó là thắng nên lũy thừa của hai tự nó xen kẽ giữa thua và thắng theo tính chẵn lẻ của số mũ. Từ$2=2^1$đang thua, tất cả số mũ lẻ đều thua và tất cả số mũ chẵn đều thắng. 

Mọi giá trị có thể thuộc về chính xác một trong các loại này, vì vậy thuật toán luôn trả về giá trị chiến thắng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())

    if n % 2 == 1:
        print("Bob")
        continue

    x = n
    cnt = 0

    while x % 2 == 0:
        x //= 2
        cnt += 1

    if x != 1:
        print("Alice")
    else:
        print("Alice" if cnt % 2 == 0 else "Bob")
```Nhánh đầu tiên xử lý ngay tất cả các số lẻ vì chúng đang mất vị trí. 

Đối với các số chẵn, mã sẽ loại bỏ mọi thừa số của 2 và đếm số lượng đã bị loại bỏ. Nếu giá trị còn lại lớn hơn một thì số đó có thừa số lẻ và không phải là lũy thừa thuần túy của hai. Những vị trí đó luôn giành chiến thắng. 

Nếu giá trị còn lại đúng bằng 1 thì số ban đầu là$2^{\text{cnt}}$. Người chiến thắng chỉ phụ thuộc vào số mũ chẵn hay lẻ. 

Không cần mảng lớn hoặc đệ quy. Vòng lặp chạy nhiều nhất là khoảng ba mươi lần vì$2^{30} > 10^9$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1
4
12
69
```| n | Số lẻ? | Sức mạnh của hai? | Số mũ | Người chiến thắng | 
| --- | --- | --- | --- | --- | 
| 1 | Có | Không | - | Bob | 
| 4 | Không | Có | 2 | Alice | 
| 12 | Không | Không | - | Alice | 
| 69 | Có | Không | - | Bob | 

Đầu ra:```
Bob
Alice
Alice
Bob
```Ví dụ này hiển thị cả ba loại: số lẻ, lũy thừa của hai và số chẵn có thừa số lẻ. 

### Ví dụ 2 

đầu vào:```
3
2
8
16
```| n | Số lẻ? | Sức mạnh của hai? | Số mũ | Người chiến thắng | 
| --- | --- | --- | --- | --- | 
| 2 | Không | Có | 1 | Bob | 
| 8 | Không | Có | 3 | Bob | 
| 16 | Không | Có | 4 | Alice | 

Đầu ra:```
Bob
Bob
Alice
```Dấu vết này thể hiện hành vi xen kẽ của quyền hạn của hai. Số mũ lẻ thua, số mũ chẵn thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$| Nhiều nhất số thừa số của 2 trong$n$bị xóa | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Từ$n \le 10^9$, vòng lặp thực hiện tối đa khoảng 30 lần lặp cho mỗi ca kiểm thử. Ngay cả với$10^4$trường hợp thử nghiệm, thời gian chạy dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())

        if n % 2:
            ans.append("Bob")
            continue

        x = n
        cnt = 0

        while x % 2 == 0:
            x //= 2
            cnt += 1

        if x != 1:
            ans.append("Alice")
        else:
            ans.append("Alice" if cnt % 2 == 0 else "Bob")

    print("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

# provided sample
assert run("4\n1\n4\n12\n69\n") == "Bob\nAlice\nAlice\nBob\n", "sample 1"

# minimum value
assert run("1\n1\n") == "Bob\n", "n = 1"

# smallest power of two
assert run("1\n2\n") == "Bob\n", "2 is losing"

# even but not power of two
assert run("1\n6\n") == "Alice\n", "contains odd factor"

# largest power of two below 1e9
assert run("1\n536870912\n") == "Bob\n", "2^29"

# large odd value
assert run("1\n999999937\n") == "Bob\n", "large prime"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`Bob`| Trạng thái trò chơi trống | 
|`2`|`Bob`| Sức mạnh nhỏ nhất của hai | 
|`6`|`Alice`| Số chẵn có thừa số lẻ | 
|`536870912`|`Bob`| lũy thừa lớn của hai với số mũ lẻ | 
|`999999937`|`Bob`| Số nguyên tố lẻ lớn | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
1
```Thuật toán phát hiện ngay số lẻ và đưa ra`"Bob"`. Điều này phù hợp với trò chơi vì không có nước đi hợp pháp nào từ$1$. 

Coi như:```
1
2
```Vòng nhân tử tìm chính xác một thừa số của hai. Số lượng là$2^1$và số mũ là số lẻ nên thuật toán đưa ra`"Bob"`. Thực vậy,$2$không có ước số thích hợp và đang thua. 

Coi như:```
1
8
```Vòng lặp loại bỏ ba thừa số của hai, đưa ra số mũ$3$. Vì số mũ là số lẻ nên đáp án là`"Bob"`. Mỗi bước di chuyển từ$8$dẫn đến$6$hoặc vị trí chiến thắng khác nên người chơi đầu tiên thua cuộc. 

Coi như:```
1
18
```Loại bỏ các yếu tố của hai lá$9$, đó không phải là$1$. Số này là số chẵn không phải lũy thừa của 2, do đó thuật toán đưa ra`"Alice"`. Alice có thể trừ số chia lẻ$9$, đạt$9$, đó là một vị trí thua cuộc. 

Những trường hợp này bao gồm các tình huống có nhiều khả năng phá vỡ một phân tích không đầy đủ: vị trí cuối cùng, số nguyên tố, lũy thừa của hai và số chẵn chứa thừa số lẻ.
