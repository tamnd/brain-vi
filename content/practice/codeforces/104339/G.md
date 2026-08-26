---
title: "CF 104339G - Lừa bài"
description: "Chúng ta bắt đầu với một bộ bài $n$ khác nhau. Tham số cố định $m$ kiểm soát một thao tác lặp lại luôn hoạt động theo cùng một cách trên bộ bài, bất kể giá trị của lá bài. Mỗi hoạt động hoạt động theo hai giai đoạn."
date: "2026-07-01T18:39:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104339
codeforces_index: "G"
codeforces_contest_name: "FAMCS Olympiad for scholars, Qualification (copy)"
rating: 0
weight: 104339
solve_time_s: 59
verified: true
draft: false
---

[CF 104339G - Lừa bài](https://codeforces.com/problemset/problem/104339/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một bộ bài$n$thẻ riêng biệt. Một tham số cố định$m$kiểm soát một hoạt động lặp đi lặp lại luôn hoạt động theo cùng một cách trên bộ bài, bất kể giá trị của lá bài. 

Mỗi hoạt động hoạt động theo hai giai đoạn. Đầu tiên, chúng tôi phân phối bộ bài thành$m$cọc theo kiểu tuần hoàn từ trên xuống dưới. Sau đó, chúng ta kết hợp lại các cọc thành một bộ bài mới, nhưng thay vì xếp chồng 1 lên chồng 2, v.v., chúng ta sắp xếp lại các cọc sao cho cọc chứa thẻ khán giả được đặt lên trên, tiếp theo là cọc tiếp theo theo thứ tự tuần hoàn, quấn quanh ở cuối. 

Cấu trúc ẩn quan trọng là vị trí của mỗi thẻ sẽ tiến triển một cách xác định theo quy trình này. Lá bài được khán giả chọn không quan trọng đối với cơ chế chuyển đổi mà nó được dùng để quyết định cách quay khi ghép lại các cọc. 

Chúng tôi được yêu cầu một giá trị$k$, số lần lặp lại tối thiểu của thao tác này sao cho dù bộ bài ban đầu được sắp xếp như thế nào và bất kể lá bài nào được chọn, sau đó$k$số lần lặp lại lá bài đã chọn được đảm bảo ở vị trí cao nhất trong bộ bài. 

Bài toán đang yêu cầu thời gian hội tụ trong trường hợp xấu nhất của một phép biến đổi xác định trên các hoán vị, trong đó phép biến đổi phụ thuộc vào$n$Và$m$, nhưng không dựa trên hoán vị cụ thể khi chúng tôi xử lý trường hợp xấu nhất trên tất cả các trạng thái bắt đầu và các thẻ đã chọn. 

Ràng buộc$n, m \le 10^9$ngay lập tức loại trừ mọi mô phỏng qua thẻ hoặc trạng thái. Chúng ta thậm chí không thể xây dựng rõ ràng biểu đồ hoán vị theo các vị trí. Lời giải chỉ phải phụ thuộc vào đặc tính cấu trúc của phép biến đổi. 

Một sai lầm ngây thơ là cho rằng quá trình này phụ thuộc vào giá trị của các quân bài hoặc mô phỏng việc xáo trộn ngẫu nhiên. Ví dụ, trong một trường hợp nhỏ như$n=6, m=2$, người ta có thể theo dõi không chính xác các cọc thực tế và cho rằng sự hội tụ phụ thuộc vào thứ tự ban đầu. Tuy nhiên, hoạt động này hoàn toàn dựa trên vị trí nên chỉ có các chỉ số mới là quan trọng. 

Một vấn đề tế nhị khác là giả sử câu trả lời phụ thuộc vào lá bài đã chọn. Bước xoay loại bỏ sự phụ thuộc này trong trường hợp đảm bảo xấu nhất: chúng ta cần một$k$hoạt động cho tất cả các lựa chọn, do đó hệ thống hoạt động giống như sự hội tụ trong trường hợp xấu nhất của một quy trình được chỉ đạo trên các vị trí. 

## Phương pháp tiếp cận 

Điều quan trọng là diễn giải lại thủ thuật này như một sự chuyển đổi mang tính quyết định về các vị trí$1 \dots n$. 

Trong một lần thao tác, trước tiên chúng tôi chia bộ bài thành$m$trình tự xen kẽ: 

- cọc 1 chứa các vị trí$1, m+1, 2m+1, \dots$- cọc 2 chứa các vị trí$2, m+2, 2m+2, \dots$- vân vân 

Sau đó, chúng ta sắp xếp lại các cọc sao cho cọc chứa lá bài đã chọn trở thành cọc đầu tiên và chúng ta luân chuyển thứ tự cọc theo chu kỳ. 

Điều này có nghĩa là việc chuyển đổi không phụ thuộc vào danh tính thẻ mà chỉ phụ thuộc vào modulo lớp dư lượng nào.$m$lá bài đã chọn nằm trong đó, kết hợp với sự dịch chuyển theo chu kỳ. 

Quan sát quan trọng là chúng ta liên tục giảm bớt sự không chắc chắn về vị trí của lá bài đã chọn bằng cách nén cấu trúc của bộ bài thành các khối được tạo ra bởi modulo.$m$. Mỗi thao tác ánh xạ một cách hiệu quả một vị trí tới một vị trí mới được xác định bởi thương số và số dư của nó đối với$m$và việc xoay chỉ ảnh hưởng đến khối nào được coi là “đầu tiên” chứ không ảnh hưởng đến cấu trúc bên trong. 

Nếu chúng ta theo dõi vị trí của một thẻ cố định, chỉ số của nó sẽ tiến triển theo một hàm gần như hoạt động giống như việc áp dụng nhiều lần:$$x \mapsto \left\lceil \frac{x}{m} \right\rceil$$lên đến sự dịch chuyển theo chu kỳ mà không ảnh hưởng đến độ sâu tiệm cận. 

Quá trình kết thúc khi vị trí trở thành 1 và số bước trong trường hợp xấu nhất tương ứng với số lần chúng ta phải nén liên tục một chỉ mục trong cơ sở$m$cho đến khi nó sụp đổ thành một đơn vị duy nhất. Đây chính xác là số chữ số trong biểu diễn của$n$trong căn cứ$m$, hoặc tương đương là nhỏ nhất$k$như vậy:$$m^k \ge n$$Vì vậy, câu trả lời là:$$k = \lceil \log_m n \rceil$$Điều này xuất hiện vì mỗi thao tác làm giảm “quy mô” hiệu quả của không gian vị trí theo hệ số$m$và chúng tôi cần mức giảm đủ để thu hẹp bất kỳ vị trí bắt đầu nào xuống vị trí đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(kn)$|$O(n)$| Quá chậm | 
| Tính toán logarit |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$Và$m$. Quá trình chỉ phụ thuộc vào các tham số này vì sự hội tụ trong trường hợp xấu nhất sẽ bỏ qua các hoán vị ban đầu. 
2. Giải thích sự hình thành cọc lặp đi lặp lại như việc liên tục nhóm các chỉ số thành các khối có kích thước$m$. Mỗi nhóm làm giảm phạm vi hiệu quả của các vị trí. 
3. Nhận thấy rằng sau một thao tác, “khoảng cách” tối đa có thể có của bất kỳ thẻ nào từ trên xuống sẽ giảm khoảng từ$n$ĐẾN$\lceil n/m \rceil$. Đây là bước nén thúc đẩy sự hội tụ. 
4. Lặp lại lý do này: sau$t$hoạt động, độ sâu còn lại trong trường hợp xấu nhất là khoảng$\lceil n / m^t \rceil$. 
5. Chúng tôi muốn cái nhỏ nhất$t$sao cho giá trị này trở thành 1, nghĩa là mọi vị trí bắt đầu có thể có đều đã thu gọn lên trên cùng. 
6. Giải bất đẳng thức$m^t \ge n$. Câu trả lời là nhỏ nhất như vậy$t$, đó là trần của đế-$m$logarit của$n$. 

### Tại sao nó hoạt động 

Quá trình xác định sự phân chia xác định các vị trí thành$m$xen kẽ các chuỗi con, theo sau là sự sắp xếp lại theo chu kỳ của các chuỗi con này. Điều duy nhất quan trọng đối với sự hội tụ là số lần một vị trí tồn tại được “trải rộng” trên$m$nhóm trước khi nó trở thành phần tử đầu tiên trong chuỗi nhóm của nó. Mỗi thao tác làm giảm thang đo chỉ số hiệu quả theo hệ số$m$, vậy sau$t$bước bất kỳ vị trí ban đầu nào cũng phải nằm trong khối đầu tiên một lần$m^t$vượt quá$n$. Điều này đảm bảo lá bài đã chọn sẽ được đưa vào vị trí trên cùng bất kể thứ tự ban đầu hay việc xoay cọc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

# compute ceil(log_m(n)) without floating point
k = 0
cur = 1

while cur < n:
    cur *= m
    k += 1

print(k)
```Mã tránh các logarit dấu phẩy động, có thể không ổn định đối với các giá trị lớn lên tới$10^9$. Thay vào đó, nó liên tục nhân lên cho đến khi đạt hoặc vượt quá$n$, phản ánh trực tiếp tình trạng$m^k \ge n$. 

Biến`cur`thể hiện sự tăng trưởng của “khả năng nén” có thể đạt được sau mỗi hoạt động. Mỗi vòng lặp mô phỏng một ứng dụng của hiệu ứng tập hợp lại cọc về mặt giảm quy mô. 

Vòng lặp an toàn dưới các ràng buộc vì$m \ge 2$, vậy số lần lặp nhiều nhất là$\log_2(10^9)$, tức là khoảng 30. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 6, m = 2$Chúng tôi theo dõi ngưỡng phát triển như thế nào. 

| bước | cur | tình trạng | 
| --- | --- | --- | 
| 0 | 1 | 1 < 6 | 
| 1 | 2 | 2 < 6 | 
| 2 | 4 | 4 < 6 | 
| 3 | 8 | 8 ≥ 6 | 

Quá trình dừng ở bước 3, do đó đầu ra là 3. 

Điều này cho thấy rằng việc lặp đi lặp lại một nửa phạm vi hiệu quả cần ba bước trước khi bất kỳ vị trí bắt đầu nào sụp đổ lên trên cùng. 

### Ví dụ 2:$n = 21, m = 3$| bước | cur | tình trạng | 
| --- | --- | --- | 
| 0 | 1 | 1 < 21 | 
| 1 | 3 | 3 < 21 | 
| 2 | 9 | 9 < 21 | 
| 3 | 27 | 27 ≥ 21 | 

Câu trả lời là 3. 

Điều này xác nhận rằng nén bậc ba yêu cầu ba lần lặp trước khi toàn bộ không gian vừa khít với một khối hiệu quả duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log_m n)$| Mỗi lần lặp lại nhân phạm vi bảo hiểm với$m$, đạt$n$theo các bước logarit | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Sự ràng buộc$n \le 10^9$đảm bảo tối đa khoảng 30 lần lặp ngay cả trong trường hợp xấu nhất$m=2$, vậy nghiệm có hiệu quả là thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())

    k = 0
    cur = 1
    while cur < n:
        cur *= m
        k += 1

    return str(k)

# provided samples
assert run("6 2") == "3", "sample 1"
assert run("21 3") == "3", "sample 2"

# custom cases
assert run("2 2") == "1", "smallest growth"
assert run("10 10") == "1", "single jump covers all"
assert run("1000000000 2") == "30", "max depth binary growth"
assert run("9 3") == "2", "exact power boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 | 1 | trường hợp không tầm thường nhỏ nhất | 
| 10 10 | 1 | nén toàn bộ ngay lập tức | 
| 1e9 2 | 30 | độ sâu tối đa dưới những ràng buộc | 
| 9 3 | 2 | hành vi ranh giới sức mạnh chính xác của m | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$n = m$. Trong trường hợp này, một thao tác đã biến bất kỳ vị trí nào thành một kết cấu cọc đơn sẽ sụp đổ ngay sau khi được chỉ định lại. Thuật toán xử lý nó vì vòng lặp bắt đầu bằng`cur = 1`và nhân một lần để đạt được$m = n$, tạo ra đầu ra 1. 

Một trường hợp cạnh khác là khi$m = 2$Và$n$là rất lớn. Điều này tạo ra số lần lặp tối đa nhưng vẫn nằm trong khoảng 30 bước. Cấu trúc vòng lặp đảm bảo không có vấn đề tràn hoặc hiệu suất vì phép nhân vẫn nằm trong giới hạn số nguyên của Python. 

Trường hợp cạnh cuối cùng là$n = 1$, nhưng điều này bị loại trừ bởi các ràng buộc$n \ge 2$, do đó không cần xử lý đặc biệt.
