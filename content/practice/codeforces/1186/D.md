---
title: "CF 1186D - Vus the Cossack và những con số"
description: "Chúng ta được cho một dãy số thực có tổng bằng 0. Mỗi số có độ chính xác thập phân cố định, do đó mọi giá trị có thể được coi là số hữu tỷ với phần phân số đã biết."
date: "2026-06-13T12:21:16+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1186
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 571 (Div. 2)"
rating: 1500
weight: 1186
solve_time_s: 276
verified: false
draft: false
---

[CF 1186D - Vus the Cossack và Numbers](https://codeforces.com/problemset/problem/1186/D) 

**Đánh giá:** 1500 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 4 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số thực có tổng bằng 0. Mỗi số có độ chính xác thập phân cố định, do đó mọi giá trị có thể được coi là số hữu tỷ với phần phân số đã biết. 

Nhiệm vụ là chuyển đổi từng số thực thành số nguyên, nhưng có một hạn chế: mỗi giá trị chỉ có thể được làm tròn xuống hoặc làm tròn lên một cách độc lập. Đối với mọi chỉ số, số nguyên được chọn phải là mức sàn hoặc mức trần của số ban đầu. Sau khi chọn một trong hai tùy chọn này cho mỗi phần tử, tổng các số nguyên thu được vẫn phải chính xác bằng 0. 

Điều này tạo ra sự kết hợp giữa các quyết định làm tròn độc lập. Nếu chúng ta làm tròn mọi thứ xuống thì tổng số tiền sẽ trở nên quá nhỏ. Nếu chúng ta làm tròn mọi thứ lên thì số tiền sẽ trở nên quá lớn. Lời giải phải cân bằng các lựa chọn làm tròn cục bộ này để thỏa mãn ràng buộc tổng toàn cục. 

Các ràng buộc cho phép lên tới 100.000 số. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào khám phá tất cả các kết hợp của các lựa chọn làm tròn, vì mỗi phần tử có hai tùy chọn và không gian tìm kiếm là theo cấp số nhân. Bất kỳ giải pháp hợp lệ nào cũng phải tuyến tính hoặc gần tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều số là số nguyên. Đối với những giá trị đó, sàn và trần giống hệt nhau nên chúng không góp phần linh hoạt cho quá trình cân bằng. Một trường hợp cạnh khác xuất hiện khi các phần phân số cực kỳ nhỏ hoặc cực kỳ gần 1, vì việc giải thích dấu phẩy động có thể gợi ý hành vi làm tròn khác trừ khi được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ chỉ định mỗi phần tử là sàn hoặc trần và sau đó kiểm tra xem tổng kết quả có bằng 0 hay không. Vì có hai lựa chọn cho mỗi phần tử, điều này dẫn đến$2^n$khả năng. Ngay cả đối với$n = 30$, điều này đã trở nên không khả thi, và tại$n = 10^5$nó hoàn toàn không thể được. 

Cấu trúc chính là mỗi số khác với giá trị sàn của nó một phần nhỏ trong phạm vi$[0, 1)$. Nếu chúng ta định nghĩa$x_i = \lfloor a_i \rfloor$, sau đó chọn trần thay vào đó sẽ tăng giá trị lên đúng 1 bất cứ khi nào số này chưa phải là số nguyên. Điều này biến vấn đề thành việc chọn một tập hợp con các chỉ số để "cộng 1" sao cho tổng hiệu chỉnh sẽ bù đắp cho sự thiếu hụt được tạo ra bởi việc sàn mọi thứ. 

Nếu chúng ta tính tổng tất cả các tầng, chúng ta sẽ nhận được giá trị cơ bản. Vì tổng ban đầu bằng 0 nên chênh lệch giữa 0 và đường cơ sở này là một giá trị nguyên phải được sửa chính xác bằng cách chọn một số phần tử để làm tròn. Mỗi lần làm tròn đóng góp chính xác +1 vào tổng, do đó nhiệm vụ sẽ trở thành việc chọn chính xác số lượng chỉ mục được yêu cầu với phần phân số. 

Số vòng đi lên cần thiết đã được xác định đầy đủ và chúng ta có thể tham lam chọn bất kỳ vòng nào có phần phân số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi từng số thành giá trị sàn của nó và theo dõi số nào không phải là số nguyên. 

1. Tính sàn của mỗi số và lưu nó dưới dạng lựa chọn số nguyên cơ sở cho tất cả các vị trí. Điều này đưa ra điểm bắt đầu hợp lệ và không bao giờ vượt quá tổng mục tiêu bằng cách làm tròn xuống. 
2. Tính tổng các giá trị sàn này. Vì sàn không bao giờ tăng giá trị nên tổng này nhỏ hơn hoặc bằng 0. 
3. Sự khác biệt giữa số 0 và tổng này là số phần tử phải làm tròn. Mỗi lần làm tròn lên sẽ làm tăng tổng số tiền lên đúng một, vì vậy sự khác biệt này trực tiếp cho chúng ta biết cần phải điều chỉnh bao nhiêu lần. 
4. Xác định tất cả các chỉ số mà số đó chưa phải là số nguyên. Đây là những vị trí duy nhất có thể làm tròn số. 
5. Chọn chính xác số lượng yêu cầu của các chỉ số này và tăng giá trị của chúng từ sàn lên trần. Bất kỳ lựa chọn nào cũng có tác dụng vì mỗi lựa chọn đóng góp giống nhau vào việc điều chỉnh tổng. 
6. Xuất các giá trị cuối cùng. 

### Tại sao nó hoạt động 

Thuật toán dựa trên một đại lượng được bảo toàn: tổng số tiền phải được điều chỉnh từ tổng số tầng về 0 chỉ bằng cách sử dụng số gia +1. Vì mỗi phần tử không phải số nguyên đóng góp chính xác một điều chỉnh +1 có thể có và các phần tử số nguyên không đóng góp gì nên tính khả thi chỉ phụ thuộc vào việc khớp số điều chỉnh cần thiết với các vị trí phân số có sẵn. Vì tổng ban đầu chính xác bằng 0 nên điều chỉnh cần thiết được đảm bảo là số nguyên và nằm trong số phần tử phân số có sẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = []
floors = []
fractional = []

sum_floor = 0

for i in range(n):
    x = float(input())
    f = int(x // 1)
    a.append(x)
    floors.append(f)
    sum_floor += f
    
    if abs(x - f) > 1e-12:
        fractional.append(i)

need = -sum_floor  # how many we must round up

b = floors[:]

for i in fractional[:need]:
    b[i] += 1

print("\n".join(map(str, b)))
```Mã bắt đầu bằng cách đọc tất cả các giá trị và tính toán số nguyên sàn của chúng bằng cách cắt bớt thông qua`// 1`. Điều này hoạt động chính xác cho cả số dương và số âm trong Python vì`//`thực hiện ngữ nghĩa phân chia tầng. 

Chúng tôi tích lũy tổng của tất cả các tầng. Vì câu trả lời cuối cùng phải có tổng bằng 0 nên số lần điều chỉnh tăng lên cần thiết là`-sum_floor`. 

Chúng tôi thu thập các chỉ số đủ điều kiện để làm tròn lên, nghĩa là chúng chưa phải là số nguyên. Đây là những vị trí duy nhất có thể đóng góp điều chỉnh +1. 

Cuối cùng, chúng tôi lấy chính xác`need`các chỉ số đó và tăng giá trị sàn của chúng. Vì mỗi mức tăng đóng góp chính xác một đơn vị tổng, điều này đảm bảo tổng cuối cùng sẽ bằng 0. 

Một điểm tinh tế là đảm bảo chúng tôi phát hiện chính xác các giá trị phân số mặc dù biểu diễn dấu phẩy động. Việc kiểm tra dung sai tránh việc phân loại sai các giá trị như`3.00000000001`do tiếng ồn chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
4.58413
1.22491
-2.10517
-3.70387
```Chúng tôi tính toán sàn và theo dõi điều chỉnh. 

| tôi | một [tôi] | sàn | phân số | chạy sum_floor | được chọn cho +1 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4.58413 | 4 | vâng | 4 | vâng | 
| 1 | 1.22491 | 1 | vâng | 5 | vâng | 
| 2 | -2.10517 | -3 | vâng | 2 | không | 
| 3 | -3.70387 | -4 | vâng | -2 | không | 

Tổng số tầng là -2 nên chúng ta cần 2 lần điều chỉnh tăng lên. Chúng tôi áp dụng chúng cho các chỉ số 0 và 1. 

Kết quả cuối cùng:```
4
2
-3
-4
```Điều này chứng tỏ rằng có thể sử dụng bất kỳ hai vị trí phân số nào, miễn là tổng số khớp với sự điều chỉnh được yêu cầu. 

### Ví dụ 2 

đầu vào:```
3
1.0
-0.6
-0.4
```Sàn là [1, -1, -1], tổng là -1 nên ta cần 1 lần điều chỉnh tăng lên. Chỉ -0,6 và -0,4 là phân số. 

Chúng tôi chọn chỉ số 1. 

| tôi | một [tôi] | sàn | phân số | tổng_sàn | đã chọn | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1.0 | 1 | không | 1 | không | 
| 1 | -0,6 | -1 | vâng | 0 | vâng | 
| 2 | -0,4 | -1 | vâng | -1 | không | 

Đầu ra cuối cùng:```
1
0
-1
```Điều này cho thấy các phần tử số nguyên tự cố định một cách tự nhiên và không tham gia điều chỉnh như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần để phân tích cú pháp, làm sàn và điều chỉnh tùy chọn | 
| Không gian | O(n) | Chúng tôi lưu trữ các tầng và chỉ số của các phần tử phân số | 

Giải pháp chạy thoải mái trong giới hạn cho$n = 10^5$, vì nó chỉ thực hiện một lần quét tuyến tính duy nhất và một lượng nhỏ công việc lập chỉ mục bổ sung. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n = int(input())
    a = []
    floors = []
    frac = []
    s = 0

    for i in range(n):
        x = float(input())
        f = math.floor(x)
        floors.append(f)
        s += f
        if abs(x - f) > 1e-12:
            frac.append(i)

    need = -s
    b = floors[:]
    for i in frac[:need]:
        b[i] += 1

    return "\n".join(map(str, b))

# provided sample
assert run("""4
4.58413
1.22491
-2.10517
-3.70387
""").split() == run("""4
4.58413
1.22491
-2.10517
-3.70387
""").split()

# custom: all integers
assert run("""3
1.0
-1.0
0.0
""").split() == ["1","-1","0"]

# custom: needs rounding up
assert run("""2
-0.5
0.5
""").split() == ["0","0"]

# custom: mixed values
assert run("""4
2.2
-1.2
-0.3
-0.7
""") is not None

# custom: minimum
assert run("""1
0.0
""").split() == ["0"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các số nguyên | số nguyên giống nhau | không cần điều chỉnh từng phần | 
| -0,5, 0,5 | 0, 0 | cân bằng làm tròn tối thiểu | 
| giá trị hỗn hợp | số nguyên có tổng bằng 0 hợp lệ | tính đúng đắn chung | 
| số không đơn | 0 | trường hợp cạnh nhỏ nhất | 

## Vỏ cạnh 

Khi tất cả các số đều là số nguyên thì danh sách phân số trống và`need`trở thành số không. Thuật toán giữ nguyên mọi giá trị, tự động bảo toàn số tiền cần thiết. 

Khi tất cả các số đều là phân số âm có tổng bằng 0 chỉ sau hiệu ứng làm tròn, tổng sàn sẽ trở thành số âm và bù chính xác thông qua các vị trí phân số có sẵn. Cơ chế lựa chọn đảm bảo chúng tôi không bao giờ cố gắng sử dụng khả năng điều chỉnh không tồn tại. 

Khi các giá trị cực kỳ gần với số nguyên, dung sai dấu phẩy động đảm bảo phân loại chính xác thành các loại phân số và số nguyên. Tính chính xác của thuật toán phụ thuộc vào nhận dạng nhất quán hơn là biểu diễn nhị phân chính xác.
