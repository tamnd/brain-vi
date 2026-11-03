---
title: "CF 11B - Jack Nhảy"
description: "Jack bắt đầu ở vị trí 0 trên trục số. Lần nhảy của anh ta có độ dài cố định: lần nhảy đầu tiên phải có độ dài 1, lần nhảy thứ hai phải có độ dài 2, lần nhảy thứ ba có độ dài 3, v.v. Đối với mỗi lần nhảy, anh ta có thể chọn một trong hai hướng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 11
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 11"
rating: 1600
weight: 11
solve_time_s: 86
verified: true
draft: false
---
[CF 11B - Jack nhảy](https://codeforces.com/problemset/problem/11/B) 

**Đánh giá:** 1600 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Jack bắt đầu ở vị trí`0`trên trục số. Lần nhảy của anh ta có độ dài cố định: lần nhảy đầu tiên phải có độ dài`1`, độ dài bước nhảy thứ hai`2`, độ dài bước nhảy thứ ba`3`, vân vân. Đối với mỗi lần nhảy, anh ta có thể chọn một trong hai hướng. Nhiệm vụ là tìm số lần nhảy tối thiểu cần thiết để tiếp đất chính xác tại vị trí`x`. 

Sự lựa chọn hướng đi là phần thú vị. Sau đó`k`nhảy, tổng quãng đường di chuyển luôn là:$1+2+3+\dots+k=\frac{k(k+1)}{2}$Điều duy nhất chúng tôi kiểm soát là bước nhảy nào là dương và bước nhảy nào là âm. 

Kích thước đầu vào tăng lên`10^9`, điều này ngay lập tức loại trừ mọi tìm kiếm trong không gian trạng thái đối với các vị trí. Tìm kiếm theo chiều rộng trên các tọa độ có thể truy cập sẽ bùng nổ vì sau`k`có những bước nhảy`2^k`các lựa chọn hướng đi. Thậm chí`k = 40`đã tạo ra hơn một nghìn tỷ khả năng. 

Bản thân câu trả lời vẫn nhỏ vì các số tam giác tăng theo phương trình bậc hai. Để đạt được khoảng cách xung quanh`10^9`, chúng ta chỉ cần đại khái:$\frac{k(k+1)}{2}\approx 10^9$mang lại`k ≈ 44721`. Điều đó có nghĩa là một`O(sqrt(x))`giải pháp hoàn toàn an toàn. 

Mục tiêu tiêu cực là nơi dễ mắc sai lầm. Vấn đề là đối xứng xung quanh số 0 vì mọi chuỗi bước nhảy trái và phải đều có thể được phản ánh. Tiếp cận`-5`thực hiện số lần nhảy tương đương với việc đạt được`5`. Việc triển khai bất cẩn xử lý các giá trị âm riêng biệt có thể gây ra sự phức tạp không cần thiết. 

Một trường hợp tinh vi khác xuất hiện khi tổng tích lũy đầu tiên vượt quá mục tiêu nhưng lại có tính chẵn lẻ sai. Ví dụ: 

đầu vào:```
2
```Sau 2 lần nhảy tổng quãng đường là`1 + 2 = 3`. Chúng tôi không thể làm`2`bởi vì việc thay đổi hướng nhảy sẽ thay đổi vị trí cuối cùng gấp đôi độ dài bước nhảy đó. Sự khác biệt`3 - 2 = 1`là số lẻ nên không có tập hợp con bước nhảy nào có thể khắc phục được. Chúng ta cần thêm một bước nhảy nữa, tính tổng`6`, và bây giờ`6 - 2 = 4`là số chẵn, vì vậy câu trả lời trở thành`3`. 

Bẫy chẵn lẻ thứ hai là: 

đầu vào:```
5
```Tổng số sau ba lần nhảy là`6`, vốn đã vượt quá`5`, Nhưng`6 - 5 = 1`thật kỳ quặc. Sau bốn lần nhảy tổng số sẽ trở thành`10`, Và`10 - 5 = 5`vẫn còn kỳ quặc. Chỉ sau năm lần nhảy chúng ta mới có được`15 - 5 = 10`, tức là chẵn. Câu trả lời đúng là`5`, không`3`hoặc`4`. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Lúc nhảy`i`, chọn trái hoặc phải và tiếp tục đệ quy. Sau đó`k`có những bước nhảy`2^k`những con đường có thể. Phương pháp này đúng vì nó khám phá mọi trình tự hướng dẫn pháp lý, nhưng nó gần như trở nên vô dụng ngay lập tức. Thậm chí chỉ với`40`nhảy vọt, không gian tìm kiếm vượt quá một nghìn tỷ trạng thái. 

Quan sát quan trọng là chỉ có tổng số tiền đã ký mới quan trọng. Giả sử sau`k`tổng quãng đường nhảy là:$S=1+2+\dots+k$Nếu ban đầu chúng ta tưởng tượng tất cả các bước nhảy về bên phải, thì chúng ta sẽ hạ cánh tại`S`. Lật một bước nhảy dài`d`theo hướng ngược lại thay đổi vị trí cuối cùng bằng cách`2d`, bởi vì bước nhảy đó góp phần`+d`thay vì`-d`. 

Vì vậy, mọi vị trí có thể tiếp cận sau`k`bước nhảy có dạng:$S-2t$Ở đâu`t`là tổng của một số tập hợp con độ dài bước nhảy. 

Điều này ngay lập tức đưa ra hai điều kiện cần thiết để đạt được`x`. 

Đầu tiên,`S`ít nhất phải có`|x|`, nếu không thì đơn giản là chúng ta không có đủ chuyển động tổng thể. 

Thứ hai,`S - |x|`phải chẵn, vì nó bằng`2t`. 

Điều tuyệt vời là những điều kiện này cũng đủ. Vì những con số`1,2,3,...,k`có thể hình thành mọi giá trị từ`0`ĐẾN`S`, bất kỳ sự khác biệt chẵn nào cũng có thể được sửa chữa bằng cách lật một tập hợp con các bước nhảy thích hợp. 

Điều đó làm giảm bài toán xuống việc tìm giá trị nhỏ nhất`k`như vậy:$\frac{k(k+1)}{2}\ge |x|$Và$\left(\frac{k(k+1)}{2}-|x|\right)\bmod 2=0$Chúng ta chỉ có thể tiếp tục tăng`k`cho đến khi cả hai điều kiện được giữ nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k) | O(k) | Quá chậm | 
| Tối ưu | O(sqrt(x)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc vị trí mục tiêu`x`. 
2. Thay thế`x`với`abs(x)`. Tiếp cận`-x`yêu cầu số lần nhảy tương đương với việc đạt được`x`bởi vì mọi hướng đều có thể được phản ánh. 
3. Khởi tạo`k = 0`Và`total = 0`. Đây`k`là số lần nhảy được sử dụng cho đến nay và`total`là tổng`1 + 2 + ... + k`. 
4. Tăng liên tục`k`bằng một và thêm nó vào`total`. 
5. Sau mỗi lần cập nhật, hãy kiểm tra hai điều kiện: 

1.`total >= x`2.`(total - x) % 2 == 0`6. Stop as soon as both conditions become true. Tại thời điểm này, chúng tôi biết một số bước nhảy có thể được đảo ngược để tạo ra chính xác`x`. 
7. Đầu ra`k`. 

Kiểm tra tính chẵn lẻ là cái nhìn sâu sắc cốt lõi. Lật một bước nhảy sẽ thay đổi tọa độ cuối cùng bằng một số chẵn, do đó sự khác biệt giữa`total`Và`x`phải chẵn. 

### Tại sao nó hoạt động 

Sau`k`nhảy, tọa độ tối đa có thể đạt được là`total = 1 + 2 + ... + k`. Bất kỳ tọa độ có thể truy cập nào khác đều có được bằng cách lật một số bước nhảy và mỗi lần nhảy sẽ thay đổi giá trị cuối cùng bằng cách`2d`. Điều đó có nghĩa là mọi tọa độ có thể tiếp cận đều khác với`total`bằng một số chẵn. 

Vì vậy, các vị trí có thể tiếp cận chính xác là tất cả các số nguyên có cùng tính chẵn lẻ như`total`bên trong khoảng`[-total, total]`. 

Thuật toán tìm kiếm đầu tiên`k`Ở đâu`x`nằm bên trong bộ có thể truy cập này. Kể từ khi chúng tôi kiểm tra`k`theo thứ tự tăng dần, câu trả lời hợp lệ đầu tiên là câu trả lời tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x = abs(int(input()))

k = 0
total = 0

while total < x or (total - x) % 2 != 0:
    k += 1
    total += k

print(k)
```Việc thực hiện phản ánh trực tiếp lý luận toán học. 

Dòng đầu tiên chuyển đổi mục tiêu thành giá trị tuyệt đối của nó. Điều này loại bỏ sự cần thiết phải suy nghĩ về trái và phải một cách riêng biệt. Câu trả lời cho`-17`giống hệt với câu trả lời cho`17`. 

Các biến`k`Và`total`theo dõi số lần nhảy hiện tại và tổng tam giác sau những lần nhảy đó. Mỗi lần lặp mô phỏng việc thêm độ dài bước nhảy bắt buộc tiếp theo. 

Điều kiện vòng lặp chứa cả hai thuộc tính bắt buộc. Phần đầu tiên kiểm tra xem chúng ta đã tích lũy đủ khoảng cách để đến được mục tiêu hay chưa. Phần thứ hai kiểm tra khả năng tương thích chẵn lẻ. 

Một sai lầm phổ biến là dừng lại ngay khi`total >= x`. Điều đó không thành công trong những trường hợp như`x = 2`, bởi vì`3`Và`2`có độ chẵn lẻ khác nhau. Một sai lầm khác là kiểm tra tính chẵn lẻ trước khi xác minh`total >= x`, có thể vô tình chấp nhận các trạng thái không thể. 

Số nguyên Python tự động xử lý các giá trị lớn, do đó không có rủi ro tràn. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, ở đây số nguyên có dấu 32 bit vẫn đủ vì tổng tam giác lớn nhất nằm ở gần`10^9`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```| Lặp lại | k | tổng cộng | tổng cộng >= x | (tổng - x) chẵn? | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | Không | Có | 
| 1 | 1 | 1 | Không | Không | 
| 2 | 2 | 3 | Có | Không | 
| 3 | 3 | 6 | Có | Có | 

Đầu ra:```
3
```Dấu vết này chứng minh tại sao vượt mục tiêu là chưa đủ. Sau hai lần nhảy chúng ta có thể đạt được`3`,`1`,`-1`, hoặc`-3`, nhưng không`2`. Sự không khớp chẵn lẻ chặn nó. 

### Ví dụ 2 

đầu vào:```
5
```| Lặp lại | k | tổng cộng | tổng cộng >= x | (tổng - x) chẵn? | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | Không | Không | 
| 1 | 1 | 1 | Không | Có | 
| 2 | 2 | 3 | Không | Có | 
| 3 | 3 | 6 | Có | Không | 
| 4 | 4 | 10 | Có | Không | 
| 5 | 5 | 15 | Có | Có | 

Đầu ra:```
5
```Ví dụ này cho thấy rằng một bước nhảy thêm không phải lúc nào cũng đủ để khắc phục tính chẵn lẻ. Thêm bước nhảy`4`đã thay đổi tổng số từ`6`ĐẾN`10`, vẫn khác với`5`bằng một số lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(x)) | Tổng tam giác tăng theo phương trình bậc hai, do đó vòng lặp chạy khoảng`sqrt(2x)`lần | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Vì`x = 10^9`, vòng lặp thực hiện đại khái`45000`số lần lặp lại rất nhỏ trong giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    import sys
    input = sys.stdin.readline

    x = abs(int(input()))

    k = 0
    total = 0

    while total < x or (total - x) % 2 != 0:
        k += 1
        total += k

    print(k)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("2\n") == "3\n", "sample 1"

# minimum value
assert run("0\n") == "0\n", "already at origin"

# negative target
assert run("-2\n") == "3\n", "symmetry around zero"

# exact triangular number
assert run("6\n") == "3\n", "1+2+3 reaches target exactly"

# parity adjustment case
assert run("5\n") == "5\n", "needs extra jumps for parity"

# large boundary
assert run("1000000000\n") == "44723\n", "large input performance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`0`| Không cần nhảy | 
|`-2`|`3`| Đối xứng cho các mục tiêu tiêu cực | 
|`6`|`3`| Trường hợp tổng tam giác chính xác | 
|`5`|`5`| Nhiều hiệu chỉnh chẵn lẻ | 
|`1000000000`|`44723`| Hiệu suất gần giới hạn trên | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
0
```Thuật toán thành công ngay lập tức vì`total = 0`đã thỏa mãn cả hai điều kiện rồi. Vòng lặp không bao giờ chạy và câu trả lời là chính xác`0`. 

Bây giờ hãy xem xét:```
-2
```Thuật toán chuyển đổi điều này thành`2`. Các bước lặp trở nên giống hệt với ví dụ đã làm trước đó. Sau ba lần nhảy chúng ta có`total = 6`, Và`6 - 2 = 4`là số chẵn nên đáp án là`3`. Hướng phản ánh sẽ biến đổi bất kỳ giải pháp nào cho`2`thành giải pháp cho`-2`. 

Bẫy chẵn lẻ xuất hiện rõ ràng với:```
2
```Sau hai lần nhảy tổng số là`3`. Từ`3 - 2 = 1`thật kỳ quặc, không có tập hợp con bước nhảy nào có thể điều chỉnh vị trí chính xác`2`. Thuật toán tiếp tục đúng một bước nữa. 

Một trường hợp chẵn lẻ tinh tế hơn là:```
5
```Tổng số phát triển như`1, 3, 6, 10, 15`. Hai tổng đầu tiên quá nhỏ. Hai cái tiếp theo có sự khác biệt kỳ lạ so với`5`. Chỉ một`15`tạo ra sự khác biệt đồng đều. Thuật toán xuất ra chính xác`5`.
