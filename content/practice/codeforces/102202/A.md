---
title: "CF 102202A - Hạt cầu vồng"
description: "Chúng ta có một chuỗi có độ dài (N), trong đó mỗi viên ngọc là một trong các R, B hoặc V. Chúng ta có thể chọn một chuỗi con liền kề và muốn nó có độ dài tối đa có thể. Chuỗi con được chọn phải trông đa dạng đối với ba người quan sát khác nhau."
date: "2026-08-24T05:07:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102202
codeforces_index: "A"
codeforces_contest_name: "2019 KAIST RUN Spring Contest"
rating: 0
weight: 102202
solve_time_s: 3406
verified: true
draft: false
---

[CF 102202A - Hạt cầu vồng](https://codeforces.com/problemset/problem/102202/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi có độ dài (N), trong đó mỗi viên ngọc là một trong`R`,`B`, hoặc`V`. Chúng tôi có thể chọn một chuỗi con liền kề và muốn độ dài tối đa có thể của nó. 

Chuỗi con được chọn phải trông đa dạng đối với ba người quan sát khác nhau. Người quan sát bình thường phân biệt được cả ba màu, người quan sát mù màu đỏ xử lý`V`BẰNG`R`, và một người quan sát mù màu xanh xử lý`V`BẰNG`B`. Đối với mỗi người quan sát, những viên ngọc lân cận phải có màu sắc cảm nhận khác nhau. 

Cách hữu ích để kết hợp ba yêu cầu này là kiểm tra từng cặp màu gốc lân cận có thể có. Hai màu giống nhau bị cấm đối với người quan sát bình thường. Cặp đôi`R,V`bị cấm đối với người quan sát mù màu đỏ, bởi vì nó trở thành`R,R`. Cặp đôi`B,V`bị cấm đối với người quan sát mù màu xanh, bởi vì nó trở thành`B,B`. Do đó, trong số tất cả các cặp khác biệt có thể có, cặp duy nhất còn tồn tại là`R,B`. 

Điều đó giúp phát biểu lại vấn đề đơn giản hơn nhiều: một chuỗi con hợp lệ có độ dài ít nhất là hai chỉ có thể chứa`R`Và`B`và mọi cặp lân cận phải luân phiên nhau. MỘT`V`chỉ có thể xuất hiện trong chuỗi con hợp lệ có độ dài bằng một. 

Giới hạn (N \le 250.000) loại trừ các thuật toán bậc hai hoặc kém hơn trong giải pháp dự định. Một lần quét tuyến tính duy nhất chỉ thực hiện một lượng công việc không đổi trên mỗi viên ngọc, khá thoải mái trong giới hạn 1 giây trong Python. Một thuật toán kiểm tra mọi cặp vị trí sẽ thực hiện khoảng (31) tỷ kiểm tra cặp ở kích thước đầu vào tối đa, do đó cấu trúc của chuỗi con hợp lệ cần được khai thác trực tiếp. 

Có một số trường hợp nhỏ trong đó việc triển khai chỉ dựa trên định nghĩa ban đầu có thể sai. Đối với đầu vào`1`theo sau là`V`, câu trả lời là`1`, bởi vì chuỗi con một viên ngọc không có cặp liền kề nào vi phạm bất kỳ điều kiện nào. Việc thực hiện bất cẩn chỉ nhằm mục đích thay thế`R`Và`B`có thể trả về 0 không chính xác. 

Đối với đầu vào`3`với`RVB`, câu trả lời là`1`. Mặc dù cả ba màu gốc đều khác nhau,`RV`trở thành`RR`cho một người quan sát mù màu đỏ và`VB`trở thành`BB`đối với người quan sát mù màu xanh. Việc chỉ kiểm tra xem các ký tự gốc lân cận có khác nhau hay không sẽ chấp nhận toàn bộ chuỗi một cách không chính xác. 

Đối với đầu vào`4`với`RBRB`, câu trả lời là`4`. Mọi cặp liền kề đều là`RB`hoặc`BR`, do đó cả ba người quan sát đều nhìn thấy các màu khác nhau ở mọi ranh giới. Việc triển khai xử lý sự hiện diện của nhiều màu quá lỏng lẻo có thể bỏ lỡ sự thay thế hoàn toàn đó`R/B`chuỗi là hợp lệ. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp có thể liệt kê mọi chuỗi con liền kề và kiểm tra xem nó có thỏa mãn cả ba người quan sát hay không. Đối với một chuỗi con, việc kiểm tra mọi cặp liền kề sẽ mất thời gian tỷ lệ thuận với độ dài của nó, do đó cách tiếp cận này đúng vì nó xác minh rõ ràng định nghĩa trước khi cập nhật câu trả lời tốt nhất. 

Vấn đề là số lượng chuỗi con. Có (N(N+1)/2) trong số chúng và nếu mỗi cái được kiểm tra từ đầu thì tổng số lần kiểm tra ký tự là 

\frac{N(N+1)(N+2)}{6}. 
] 

Đối với (N=250.000), đây là khoảng (2,6\time10^{15}) hoạt động, vượt xa giới hạn thời gian. Ngay cả việc triển khai vũ lực được cải tiến nhằm mở rộng từng vị trí bắt đầu cho đến khi gặp ranh giới không hợp lệ vẫn mất (O(N^2)) trong trường hợp xấu nhất, bởi vì một chuỗi xen kẽ cho phép mọi tiện ích mở rộng tiếp tục. 

Quan sát quan trọng là ba điều kiện mù màu loại bỏ mọi cặp liền kề ngoại trừ`R,B`Và`B,R`. Khi điều này được nhận ra, chúng ta không cần phải kiểm tra các chuỗi con tùy ý nữa. Chúng ta chỉ cần lần chạy liền kề dài nhất trong đó mỗi cặp liền kề bao gồm các`R`Và`B`nhân vật. 

Thuộc tính này có thể được duy trì trong khi quét từ trái sang phải. Nếu ký tự hiện tại là`R`hoặc`B`và khác với ký tự trước đó, lần chạy xen kẽ hiện tại sẽ kéo dài thêm một. Nếu không, lần chạy hiện tại phải khởi động lại ở ký tự hiện tại. MỘT`V`luôn bắt đầu một chuỗi mới có độ dài một, vì không có chuỗi con hợp lệ nào có độ dài ít nhất hai có thể chứa nó. 

Brute-force hoạt động vì nó kiểm tra rõ ràng mọi khoảng thời gian có thể, nhưng không thành công vì có quá nhiều khoảng thời gian. Quan sát cho thấy các chuyển đổi cục bộ hợp lệ là chính xác`R -> B`Và`B -> R`biến vấn đề thành việc tìm một lần chạy xen kẽ dài nhất, có thể được thực hiện trong một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^3)) khi kiểm tra mọi chuỗi con từ đầu | (O(1)) | Quá chậm | 
| Brute Force dừng sớm | (O(N^2)) | (O(1)) | Quá chậm | 
| Quét tối ưu | (O(N)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi và khởi tạo độ dài lần chạy hợp lệ hiện tại bằng 0 và câu trả lời tối đa bằng 0. Một lần chạy biểu thị một hậu tố kết thúc ở vị trí hiện tại mà mọi cặp liền kề đều hợp lệ. 
2. Xử lý chuỗi từ trái sang phải. Đối với ký tự đầu tiên, hãy bắt đầu một chuỗi có độ dài một. Không có ký tự trước đó nên không có ranh giới để kiểm tra. 
3. Đối với mỗi ký tự sau, hãy kiểm tra xem cả ký tự hiện tại và ký tự trước đó có thuộc về không`{R, B}`và liệu chúng có khác nhau không. Đây chính xác là điều kiện mà cặp liền kề mới hoặc`RB`hoặc`BR`. 
4. Nếu điều kiện đó được giữ nguyên, hãy tăng thời lượng chạy hiện tại lên một. Ranh giới mới được thêm vào là hợp lệ và tất cả các ranh giới trước đó trong quá trình chạy đều hợp lệ. 
5. Nếu không, hãy bắt đầu một chuỗi độ dài mới ở ký tự hiện tại. Điều này bao gồm cả một`V`và một màu lặp đi lặp lại như`RR`hoặc`BB`. Không thể mở rộng chuỗi con hợp lệ trước đó. 
6. Sau khi xác định được thời lượng chạy hiện tại, hãy cập nhật câu trả lời tối đa. Lần chạy dài nhất gặp phải trong quá trình quét là chuỗi con hợp lệ dài nhất. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý vị trí (i),`current`chính xác là độ dài của chuỗi con hợp lệ dài nhất kết thúc ở vị trí (i). Nếu ranh giới mới là`RB`hoặc`BR`, việc thêm ký tự hiện tại sẽ duy trì tính hợp lệ, do đó lần chạy trước sẽ kéo dài thêm một ký tự. Đối với mọi ranh giới khác, không có chuỗi con hợp lệ nào có độ dài ít nhất là hai có thể vượt qua ranh giới đó, do đó, chuỗi con hợp lệ duy nhất kết thúc ở vị trí hiện tại có độ dài bằng một. Việc lấy mức tối đa của các độ dài kết thúc này sẽ xem xét mọi chuỗi con hợp lệ có thể có chính xác ở nơi nó kết thúc, do đó mức tối đa cuối cùng là mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    best = 1
    current = 1

    for i in range(1, n):
        if s[i] in "RB" and s[i - 1] in "RB" and s[i] != s[i - 1]:
            current += 1
        else:
            current = 1

        if current > best:
            best = current

    print(best)

if __name__ == "__main__":
    solve()
```Đầu vào được đọc bằng cách sử dụng`sys.stdin.readline`, đủ cho một chuỗi có độ dài (250.000) và tránh chi phí đầu vào không cần thiết.`current`lưu trữ độ dài của hậu tố xen kẽ hợp lệ kết thúc ở vị trí hiện tại. điều kiện```
s[i] in "RB" and s[i - 1] in "RB" and s[i] != s[i - 1]
```kiểm tra chính xác xem ranh giới giữa các vị trí`i - 1`Và`i`được cho phép. Cả hai ký tự phải không phải là`V`, và chúng phải khác nhau. Vì bảng chữ cái chỉ chứa`R`,`B`, Và`V`, điều này tương đương với việc nói rằng cặp này là`RB`hoặc`BR`. 

Khi điều kiện không thành công,`current`trở thành`1`, còn hơn là`0`. Bản thân viên ngọc hiện tại luôn là một hạt một viên ngọc hợp lệ, ngay cả khi nó`V`. 

Câu trả lời được khởi tạo thành`1`bởi vì (N \ge 1). Điều này cũng xử lý tất cả-`V`trường hợp không có chi nhánh đặc biệt. Không thể tràn số nguyên trong Python và đối tượng có kích thước đầu vào được lưu trữ duy nhất là chính chuỗi đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là`RBBB`. Sự chuyển đổi liền kề hợp lệ duy nhất là giữa các`R`Và`B`nhân vật. Sau lần đầu tiên`B`, tiếp theo`B`ngắt quãng chạy xen kẽ và phần còn lại`B`lại phá vỡ nó lần nữa. 

| Vị trí | Nhân vật | Trước | Chuyển đổi hợp lệ | Hiện tại | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | R | không | bắt đầu | 1 | 1 | 
| 1 | B | R | vâng | 2 | 2 | 
| 2 | B | B | không | 1 | 2 | 
| 3 | B | B | không | 1 | 2 | 

Chuỗi con`RB`có độ dài bằng hai và thỏa mãn mọi người quan sát. Chuỗi con không còn hoạt động vì mọi chuỗi con chứa hai chuỗi con liên tiếp`B`đồ trang sức vi phạm yêu cầu của người quan sát bình thường. Câu trả lời là`2`. 

### Mẫu 2 

Đầu vào là`RBRBB`. Bốn ký tự đầu tiên tạo thành một sự xen kẽ`R/B`sự liên tiếp. trận chung kết`B`liền kề với người khác`B`, vì vậy nó bắt đầu một lần chạy mới. 

| Vị trí | Nhân vật | Trước | Chuyển đổi hợp lệ | Hiện tại | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | R | không | bắt đầu | 1 | 1 | 
| 1 | B | R | vâng | 2 | 2 | 
| 2 | R | B | vâng | 3 | 3 | 
| 3 | B | R | vâng | 4 | 4 | 
| 4 | B | B | không | 1 | 4 | 

Chuỗi con`RBRB`có độ dài bằng bốn và mọi cặp liền kề đều bằng`RB`hoặc`BR`. trận chung kết`BB`ranh giới ngăn chặn câu trả lời dài năm, vì vậy kết quả là`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) | Mỗi viên ngọc được xử lý một lần với công việc liên tục. | 
| Không gian | (O(N)) | Chuỗi đầu vào yêu cầu bộ nhớ (O(N)), trong khi bản thân thuật toán sử dụng không gian bổ sung (O(1)). | 

Với (N) nhiều nhất (250.000), quá trình quét chỉ thực hiện một số thao tác liên tục trong thời gian cho mỗi ký tự. Điều này dễ dàng tương thích với giới hạn thời gian 1 giây, trong khi mức sử dụng bộ nhớ thấp hơn nhiều so với giới hạn 1024 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())
    s = input().strip()

    best = 1
    current = 1

    for i in range(1, n):
        if s[i] in "RB" and s[i - 1] in "RB" and s[i] != s[i - 1]:
            current += 1
        else:
            current = 1

        best = max(best, current)

    print(best)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("4\nRBBB\n") == "2", "sample 1"
assert run("5\nRBRBB\n") == "4", "sample 2"

# Minimum-size input
assert run("1\nV\n") == "1", "single V"

# All equal values
assert run("5\nRRRRR\n") == "1", "all equal"

# V cannot participate in a valid substring of length > 1
assert run("3\nRVB\n") == "1", "V blocks both neighboring transitions"

# Maximum-size input
n = 250000
s = "".join("R" if i % 2 == 0 else "B" for i in range(n))
assert run(f"{n}\n{s}\n") == str(n), "maximum alternating string"

# Boundary and off-by-one case
assert run("6\nBRBBRB\n") == "3", "longest run is BRB"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / V`| 1 | Kích thước tối thiểu và thực tế là một`V`hợp lệ | 
|`5 / RRRRR`| 1 | Các màu lặp lại không thể tạo thành một cặp liền kề hợp lệ | 
|`3 / RVB`| 1 |`V`không thể ở cạnh một trong hai`R`hoặc`B`| 
|`250000 / RBRB...`| 250000 | Kích thước đầu vào tối đa và lần chạy đạt tới chuỗi đầy đủ | 
|`6 / BRBBRB`| 3 | Khởi động lại sau một ranh giới không hợp lệ và tránh từng lỗi một | 

## Vỏ cạnh 

Đối với một viên ngọc duy nhất, hãy xem xét đầu vào```
1
V
```Không có cặp liền kề nào cả, vì vậy mọi người quan sát đều coi hạt đó là hợp lệ. Thuật toán bắt đầu`current`Và`best`Tại`1`và không bao giờ đi vào vòng lặp, tạo ra`1`. 

Đối với một chuỗi con chứa`V`, coi như```
3
RVB
```Ở vị trí một, cặp`RV`không hợp lệ vì người mù màu đỏ coi nó như hai viên ngọc màu đỏ. Thuật toán đặt lại lần chạy thành`1`. Ở vị trí thứ hai,`VB`không hợp lệ vì lý do mù màu xanh tương tự, vì vậy việc chạy vẫn được giữ nguyên`1`. Kết quả là`1`. 

Đối với màu sắc lặp đi lặp lại, hãy xem xét```
5
RRRRR
```đầu tiên`R`tạo ra một chuỗi có chiều dài một. Mỗi lần tiếp theo`R`bằng ký tự trước đó, do đó mọi chuyển đổi đều không hợp lệ và quá trình chạy liên tục được đặt lại về một ký tự. Câu trả lời là`1`. 

Đối với một hạt xen kẽ hoàn toàn, hãy xem xét```
6
RBRBRB
```Mọi ranh giới đều là`RB`hoặc`BR`, Vì thế`current`tăng từ`1`bởi vì`6`. Tối đa trở thành`6`, cho thấy thuật toán không áp đặt bất kỳ hạn chế không cần thiết nào đối với độ dài của một đoạn xen kẽ`R/B`sự liên tiếp. 

Trường hợp ranh giới`BRBBRB`rất hữu ích để bắt từng lỗi một. Quá trình quét tạo ra độ dài chạy`1, 2, 1, 1, 2, 3`, vậy câu trả lời là`3`, được biểu thị bằng chuỗi con cuối cùng`BRB`. Việc triển khai bất cẩn cập nhật mức tối đa trước khi đặt lại hoặc so sánh sai cặp chỉ số có thể báo cáo không chính xác`2`hoặc`4`.
