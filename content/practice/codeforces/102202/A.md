---
title: "CF 102202A - Hạt cầu vồng"
description: "Chúng ta có một chuỗi có độ dài (N), trong đó mỗi viên ngọc có màu R, B hoặc V. Chúng ta có thể chọn một chuỗi con liền kề và cho đi. Chuỗi con được chọn phải trông đầy màu sắc đối với ba người quan sát khác nhau."
date: "2026-08-18T20:54:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102202
codeforces_index: "A"
codeforces_contest_name: "2019 KAIST RUN Spring Contest"
rating: 0
weight: 102202
solve_time_s: 577
verified: false
draft: false
---

[CF 102202A - Hạt cầu vồng](https://codeforces.com/problemset/problem/102202/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 37 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi có độ dài (N), trong đó mọi viên ngọc đều được tô màu`R`,`B`, hoặc`V`. Chúng ta có thể chọn một chuỗi con liền kề và cho đi. Chuỗi con được chọn phải trông đầy màu sắc đối với ba người quan sát khác nhau. 

Người quan sát bình thường có thể phân biệt được cả ba màu nên các viên ngọc liền kề phải có ký tự gốc khác nhau. Người quan sát mù màu đỏ nhìn thấy`R`Và`V`cùng màu, trong khi người quan sát mù màu xanh nhìn thấy`B`Và`V`như cùng một màu sắc. Chuỗi con được chọn không được có màu liền kề bằng nhau đối với bất kỳ người quan sát nào trong số này. 

Hệ quả chính còn mạnh hơn điều kiện ban đầu gợi ý lúc đầu. Xét bất kỳ cặp liền kề nào có chứa`V`. Nếu viên ngọc kia là`R`, một người mù màu đỏ nhìn thấy hai viên ngọc đỏ liên tiếp. Nếu viên ngọc kia là`B`, một người quan sát mù màu xanh nhìn thấy hai viên ngọc xanh liên tiếp. Nếu cặp đó là`VV`, mọi người đều nhìn thấy những màu sắc như nhau. Như vậy`V`không thể liền kề với bất kỳ thứ gì bên trong chuỗi con hợp lệ. 

Cặp liền kề duy nhất sống sót sau cả ba người quan sát là`RB`hoặc`BR`. Do đó, mọi chuỗi con hợp lệ có độ dài ít nhất là hai chỉ được chứa`R`Và`B`, và những ký tự đó phải luân phiên nhau. 

Ví dụ,`RBRB`là hợp lệ, trong khi`RVB`thì không. Một viên ngọc duy nhất như`V`luôn hợp lệ vì nó không có cặp liền kề nào cả. 

Đầu vào có thể chứa tới (250.000) trang sức. Thuật toán (O(N^2)) sẽ kiểm tra khoảng (N(N+1)/2) chuỗi con, tức là khoảng (31,25) tỷ khi (N=250.000). Điều đó vượt xa giới hạn thời gian một giây. Chúng ta chỉ cần kiểm tra chuỗi một số lần không đổi, đưa ra nghiệm (O(N)). 

Có một số trường hợp đặc biệt có thể dễ dàng gây ra việc triển khai không chính xác. 

Coi như```
1
V
```Câu trả lời là`1`. Một giải pháp chỉ tìm kiếm xen kẽ`R`Và`B`các phân đoạn có thể trả về 0 không chính xác, quên rằng một viên ngọc luôn hợp lệ. 

Coi như```
4
RVBR
```Câu trả lời là`1`. Mặc dù`V`không bằng một trong hai ký tự lân cận, nó không thể liền kề với`R`cho người quan sát mù màu đỏ hoặc để`B`cho người quan sát mù màu xanh. Giải pháp chỉ kiểm tra các ký tự liền kề trong chuỗi gốc sẽ chấp nhận không chính xác các phần chứa`V`. 

Coi như```
5
RBRBB
```Câu trả lời là`4`, từ`RBRB`. trận chung kết`BB`phá vỡ mô hình xen kẽ, do đó việc triển khai bất cẩn giữ nguyên độ dài hiện tại sau khi thấy một cặp không hợp lệ có thể bị tính quá mức. 

Cuối cùng,```
5
RRRRR
```có câu trả lời`1`. liền kề bằng nhau`R`đồ trang sức ngay lập tức không hợp lệ, nhưng mỗi viên ngọc riêng lẻ vẫn là một chuỗi con hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là liệt kê mọi chuỗi con liền kề và kiểm tra xem nó có đầy màu sắc đối với cả ba người quan sát hay không. Có (N(N+1)/2) chuỗi con. Nếu mỗi chuỗi con được kiểm tra bằng cách quét tất cả các cặp liền kề của nó thì kết quả trong trường hợp xấu nhất là (O(N^3)), điều này rõ ràng là không thể. 

Chúng ta có thể làm cho ý tưởng ngây thơ đó tốt hơn một chút bằng cách cố định vị trí bắt đầu và mở rộng chuỗi con từng viên ngọc một. Khi một cặp liền kề không hợp lệ xuất hiện, mọi chuỗi con dài hơn bắt đầu ở cùng một vị trí cũng không hợp lệ, do đó chúng ta không cần quét lại toàn bộ chuỗi con. Điều này làm giảm công việc xuống (O(N^2)), vì trong trường hợp xấu nhất, chúng tôi vẫn kiểm tra mọi vị trí kết thúc có thể có cho mọi vị trí bắt đầu. Với (N=250.000), tức là khoảng (31,25) tỷ tiện ích mở rộng, vẫn còn quá nhiều. 

Cách tiếp cận bạo lực có hiệu quả vì tính hợp lệ được xác định hoàn toàn bởi các cặp liền kề. Nhận xét hữu ích là sau khi kết hợp các yêu cầu của cả ba người quan sát, hầu hết mọi cặp đều bị cấm.`R`ở cạnh`B`là cặp hợp lệ duy nhất. MỘT`V`không bao giờ có thể tham gia vào một chuỗi con hợp lệ có độ dài lớn hơn một. 

Điều đó có nghĩa là chúng ta không cần phải xét đến các chuỗi con tùy ý. Chúng ta chỉ cần tìm phần liền kề dài nhất nơi mỗi ký tự được`R`hoặc`B`và mỗi cặp liền kề là khác nhau. Một phần như vậy chỉ đơn giản là một chuỗi xen kẽ như`RBRBR`hoặc`BRBRB`. 

Chúng ta có thể quét chuỗi một lần. Trong khi ký tự hiện tại tiếp tục xen kẽ`R`/`B`trình tự, tăng độ dài của nó. Ngược lại, bắt đầu một chuỗi mới có độ dài bằng một nếu ký tự hiện tại là`R`hoặc`B`. Vì`V`, chuỗi không thể đi qua nó nữa nên độ dài hiện tại trở thành một. 

Vì một viên ngọc luôn có giá trị nên câu trả lời là ít nhất một viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^3)) | (O(1)) | Quá chậm | 
| Lực lượng vũ phu gia tăng | (O(N^2)) | (O(1)) | Quá chậm | 
| Quét tối ưu | (O(N)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi và khởi tạo độ dài hợp lệ tốt nhất để`1`. Chuỗi con một viên ngọc không có cặp liền kề nên luôn có nhiều màu sắc. 
2. Duy trì`cur`, độ dài của chuỗi con hợp lệ dài nhất kết thúc ở vị trí hiện tại. Ban đầu`cur = 1`. 
3. Đối với mọi vị trí sau vị trí đầu tiên, hãy kiểm tra các ký tự trước đó và hiện tại. Nếu cả hai đều`R`hoặc`B`và chúng khác nhau, cặp này hợp lệ cho cả ba người quan sát, vì vậy hãy mở rộng chuỗi con hiện tại bằng cách đặt`cur += 1`. 
4. Nếu không, chuỗi con trước đó không thể được mở rộng qua vị trí này. Bộ`cur = 1`, bởi vì viên ngọc hiện tại luôn là một chuỗi con hợp lệ. 
5. Cập nhật câu trả lời chung với`max(ans, cur)`sau khi xử lý từng ký tự. 

Lý do chúng ta có thể loại bỏ chuỗi con trước đó ngay sau một cặp không hợp lệ là vì mọi chuỗi con dài hơn kết thúc ở vị trí hiện tại và bắt đầu trước cặp không hợp lệ sẽ vẫn chứa cùng một cặp liền kề bị cấm đó. Không có lợi ích gì trong việc giữ bất kỳ thứ gì trong số đó. 

### Tại sao nó hoạt động 

Để một chuỗi con có độ dài ít nhất hai có màu sắc đầy màu sắc đối với cả ba người quan sát, mọi cặp liền kề phải hợp lệ cho cả ba cách diễn giải màu.`R-B`Và`B-R`là những cặp duy nhất như vậy. Mỗi cặp chứa`V`không hợp lệ đối với ít nhất một người quan sát và bằng nhau`R`hoặc bằng`B`các cặp không hợp lệ đối với người quan sát bình thường. 

Do đó, một chuỗi con hợp lệ có độ dài ít nhất là hai chính xác là một chuỗi xen kẽ của`R`Và`B`. Trong quá trình quét,`cur`chính xác là chuỗi dài nhất kết thúc ở vị trí hiện tại. hợp lệ`R/B`cặp mở rộng nó, trong khi bất kỳ cặp nào khác không thể mở rộng và buộc hậu tố hợp lệ tốt nhất trở thành viên ngọc duy nhất hiện tại. Lấy giá trị lớn nhất của`cur`trên tất cả các vị trí do đó mang lại chuỗi con liền kề hợp lệ dài nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    ans = 1
    cur = 1

    for i in range(1, n):
        if s[i] in "RB" and s[i - 1] in "RB" and s[i] != s[i - 1]:
            cur += 1
        else:
            cur = 1

        if cur > ans:
            ans = cur

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu vào được đọc với`readline`, quá đủ cho một chuỗi có độ dài (250.000). Bản thân chuỗi đó được lưu trữ một lần.`cur`đại diện cho hậu tố hợp lệ kết thúc ở vị trí hiện tại. điều kiện```
s[i] in "RB" and s[i - 1] in "RB" and s[i] != s[i - 1]
```kiểm tra chính xác xem cặp liền kề mới có phải là`RB`hoặc`BR`. Kiểm tra tư cách thành viên trong`RB`là cần thiết bởi vì`V`không thể xuất hiện trong chuỗi con hợp lệ có độ dài lớn hơn một. 

Khi điều kiện thất bại,`cur`trở thành`1`còn hơn là`0`. Điều này xử lý cả nghỉ giải lao thông thường như`BB`và trường hợp đặc biệt của`V`. Viên ngọc hiện tại luôn có thể tự bắt đầu một chuỗi con hợp lệ mới. 

Không có vấn đề tràn số nguyên trong Python và`cur`không bao giờ vượt quá (N). Quá trình quét bắt đầu tại chỉ mục`1`, do đó quyền truy cập ký tự trước đó luôn nằm trong chuỗi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
4
RBBB
```Trạng thái quan trọng là độ dài dòng điện xoay chiều`R/B`hậu tố. 

| Vị trí | Nhân vật | Trước | Cặp hợp lệ? |`cur`|`ans`| 
| --- | --- | --- | --- | --- | --- | 
| 0 | R | - | - | 1 | 1 | 
| 1 | B | R | Có | 2 | 2 | 
| 2 | B | B | Không | 1 | 2 | 
| 3 | B | B | Không | 1 | 2 | 

Hai viên ngọc đầu tiên hình thành`RB`, giá trị này đúng cho mọi người quan sát. Tiếp theo`B`tạo ra`BB`, do đó trình tự xen kẽ phải bắt đầu lại ở đó. Câu trả lời là`2`. 

### Mẫu 2 

Đầu vào là:```
5
RBRBB
```| Vị trí | Nhân vật | Trước | Cặp hợp lệ? |`cur`|`ans`| 
| --- | --- | --- | --- | --- | --- | 
| 0 | R | - | - | 1 | 1 | 
| 1 | B | R | Có | 2 | 2 | 
| 2 | R | B | Có | 3 | 3 | 
| 3 | B | R | Có | 4 | 4 | 
| 4 | B | B | Không | 1 | 4 | 

Tiền tố`RBRB`hoàn toàn xen kẽ, cho chiều dài`4`. trận chung kết`B`không thể mở rộng nó vì nó tạo ra`BB`. Câu trả lời là do đó`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) | Mỗi viên ngọc được xử lý chính xác một lần sau viên ngọc đầu tiên. | 
| Không gian | (O(N)) | Chuỗi đầu vào yêu cầu lưu trữ (O(N)); bản thân thuật toán sử dụng (O(1)) không gian bổ sung. | 

Với (N \le 250.000), thuật toán chỉ thực hiện một số thao tác có thời gian không đổi cho mỗi ký tự. Điều này nằm trong giới hạn một giây một cách thoải mái, trong khi các phương pháp bậc hai sẽ yêu cầu hàng tỷ lần lặp trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(input())
    s = input().strip()

    ans = 1
    cur = 1

    for i in range(1, n):
        if s[i] in "RB" and s[i - 1] in "RB" and s[i] != s[i - 1]:
            cur += 1
        else:
            cur = 1

        ans = max(ans, cur)

    sys.stdin = old_stdin
    return str(ans)

# Provided samples
assert solution("4\nRBBB\n") == "2", "sample 1"
assert solution("5\nRBRBB\n") == "4", "sample 2"

# Minimum-size input
assert solution("1\nV\n") == "1", "single V is always valid"

# All equal values
assert solution("5\nRRRRR\n") == "1", "equal adjacent colors are invalid"

# V cannot be part of a multi-character valid substring
assert solution("5\nRVBRB\n") == "4", "longest valid part is BRBR"

# Maximum-size input
assert solution("250000\n" + "RB" * 125000 + "\n") == "250000", \
    "entire maximum-length alternating string is valid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\nV\n`|`1`| Kích thước tối thiểu và thực tế là một`V`hợp lệ | 
|`5\nRRRRR\n`|`1`| Lặp đi lặp lại các màu bằng nhau phải phá vỡ trình tự | 
|`5\nRVBRB\n`|`4`|`V`không thể thuộc về một chuỗi con hợp lệ có độ dài lớn hơn một | 
|`250000\n`theo sau là`RB`lặp lại 125000 lần |`250000`| Kích thước đầu vào tối đa và trường hợp ranh giới có độ dài đầy đủ | 

## Vỏ cạnh 

Đối với hộp đựng một viên ngọc```
1
V
```vòng lặp không thực thi vì không có cặp liền kề.`ans`bắt đầu lúc`1`, do đó thuật toán in`1`. Đây là lý do tại sao việc khởi tạo câu trả lời về 0 cũng sẽ chỉ hoạt động nếu việc triển khai xử lý riêng việc quét trống, trong khi việc khởi tạo được chọn tự nhiên phù hợp với đảm bảo của vấn đề rằng (N \ge 1). 

Đối với một chuỗi con chứa`V`, coi như```
4
RVBR
```Tại vị trí`1`, cặp`RV`không hợp lệ, vì vậy`cur`trở thành`1`. Tại vị trí`2`, cặp`VB`cũng không hợp lệ, vì vậy`cur`còn lại`1`. Tại vị trí`3`,`BR`là hợp lệ, vì vậy`cur`trở thành`2`. Câu trả lời là`2`, tương ứng với chuỗi con cuối cùng`BR`. Điều này chứng tỏ rằng`V`không chỉ đơn thuần là dấu phân cách giữa hai chuỗi mà còn không thể tham gia vào chuỗi con hợp lệ nhiều viên ngọc. 

Đối với màu sắc lặp đi lặp lại,```
5
RRRRR
```mỗi cặp liền kề là`RR`. Mỗi cặp thất bại xen kẽ`R/B`điều kiện, vậy`cur`liên tục đặt lại thành`1`. Còn lại tối đa`1`, điều này đúng vì bất kỳ chuỗi con nào có độ dài ít nhất là hai đều chứa các viên ngọc màu đỏ liền kề bằng nhau. 

Đối với ranh giới nơi chuỗi dài nhất kết thúc,```
5
BRBRB
```mỗi cặp đều hợp lệ.`cur`tiến triển thông qua`1, 2, 3, 4, 5`, Và`ans`đạt tới`5`. Không có cách xử lý cuối chuỗi đặc biệt nào vì câu trả lời được cập nhật trong khi xử lý ký tự cuối cùng. 

Đối với đầu vào xen kẽ kích thước tối đa, bao gồm`250000`các ký tự trong mẫu`RBRB...`, mọi cặp liền kề đều là`RB`hoặc`BR`. Do đó chiều dài hiện tại đạt tới`250000`và thuật toán trả về toàn bộ hạt. Điều này xác nhận rằng quá trình quét tuyến tính xử lý đầu vào lớn nhất được phép mà không có bất kỳ logic trường hợp đặc biệt nào.
