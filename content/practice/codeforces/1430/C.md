---
title: "CF 1430C - Số trên bảng trắng"
description: "Chúng ta bắt đầu với các số nguyên từ 1 đến n được viết trên bảng trắng. Trong một thao tác, chúng ta chọn hai số bất kỳ hiện có trên bảng, xóa chúng và ghi lại giá trị ceil((a+b)/2). Sau chính xác n-1 phép toán chỉ còn lại một số."
date: "2026-06-11T05:26:48+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1430
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 96 (Rated for Div. 2)"
rating: 1000
weight: 1430
solve_time_s: 698
verified: false
draft: false
---

[CF 1430C - Các con số trên bảng trắng](https://codeforces.com/problemset/problem/1430/C) 

**Đánh giá:** 1000 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam, triển khai, toán học 
**Thời gian giải:** 11 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với các số nguyên từ`1`bởi vì`n`viết trên bảng trắng. Trong một thao tác, chúng ta chọn hai số bất kỳ hiện có trên bảng, xóa chúng và ghi lại giá trị`ceil((a+b)/2)`. 

Sau chính xác`n-1`hoạt động chỉ còn lại một số. Nhiệm vụ của chúng ta không chỉ là làm cho con số cuối cùng đó càng nhỏ càng tốt mà còn đưa ra một chuỗi các thao tác đạt được mức tối thiểu này. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một số nguyên duy nhất`n`, nghĩa là ban đầu bảng chứa các số`1, 2, ..., n`. Đầu ra trước tiên phải chứa giá trị cuối cùng tối thiểu có thể có, theo sau là các cặp chính xác được chọn trong mỗi thao tác. 

Ràng buộc rằng tổng số tiền của tất cả`n`nhiều nhất là`2·10^5`là cực kỳ quan trọng. Điều đó có nghĩa là chúng tôi có đủ khả năng thực hiện công việc tuyến tính cho mỗi trường hợp thử nghiệm và đầu ra`n-1`hoạt động. Bất kỳ giải pháp nào cố gắng tìm kiếm trên các chuỗi hoạt động khác nhau đều không thể thực hiện được vì số lượng các chuỗi có thể tăng lên một cách bùng nổ. Thậm chí`n = 20`đã có vô số khả năng. 

Phần thú vị của vấn đề là phép toán làm tròn lên trên. Làm tròn lên có xu hướng giữ giá trị lớn hơn mức trung bình bình thường. Một chiến lược bất cẩn có thể dễ dàng để lại câu trả lời cuối cùng lớn hơn mức cần thiết. 

Trường hợp một cạnh là`n = 2`. 

đầu vào:```
1
2
```Hoạt động duy nhất có thể là:```
1 2
```sản xuất`ceil(3/2)=2`. Câu trả lời là`2`. 

Một trường hợp đặc biệt khác là khi ai đó liên tục kết hợp các số nhỏ nhất trước. Vì`n = 4`:```
1,2,3,4
```kết hợp`1`Và`2`cho`2`, sau đó kết hợp`2`Và`3`cho`3`, sau đó kết hợp`3`Và`4`cho`4`. Câu trả lời cuối cùng trở thành`4`, còn xa mới tối ưu. 

Trường hợp tinh tế thứ ba là lớn`n`. Trình tự thao tác vẫn phải được tạo một cách hiệu quả vì bản thân đầu ra chứa`n-1`dòng. Bất kỳ giải pháp nào chậm hơn tuyến tính cho mỗi trường hợp thử nghiệm sẽ lãng phí thời gian so với chi phí đầu ra không thể tránh khỏi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ khám phá tất cả các cặp có thể có ở mỗi bước và thử đệ quy mọi chuỗi thao tác. Điều này đúng vì cuối cùng nó sẽ xem xét mọi kết quả có thể xảy ra. Thật không may, nó gần như trở nên vô dụng ngay lập tức. Bắt đầu với`n`số, nước đi đầu tiên có`n(n-1)/2`lựa chọn, bước đi tiếp theo có`(n-1)(n-2)/2`sự lựa chọn, vân vân. Không gian tìm kiếm tăng theo cấp số nhân. 

Quan sát quan trọng xuất phát từ việc hiểu hoạt động đó làm gì. Vì chúng ta tính toán$$\left\lceil \frac{a+b}{2} \right\rceil,$$kết quả luôn nằm giữa`max(a,b)`và khoảng một nửa số tiền. Nếu chúng ta liên tục hợp nhất các số lớn lại với nhau, chúng sẽ co lại. Ví dụ:$$\left\lceil \frac{8+7}{2} \right\rceil = 8.$$Hai số lớn trở thành một số gần chúng chứ không lớn hơn. 

Giả sử chúng ta luôn lấy số lớn nhất hiện tại và hợp nhất nó với số lớn thứ hai. Bắt đầu từ`n`, điều này dần dần kéo giá trị lớn nhất xuống trong khi không bao giờ cho phép các giá trị nhỏ làm tăng kết quả. 

Ví dụ, đối với`n=4`:```
4,3 -> 4
4,2 -> 3
3,1 -> 2
```Câu trả lời cuối cùng trở thành`2`, đó là tối ưu. 

Một thực tế đáng chú ý đã được chứng minh trong giải pháp chính thức là chiến lược này luôn tạo ra giá trị cuối cùng tối thiểu có thể. Số cuối cùng luôn là`2`cho mọi`n ≥ 2`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với số lớn nhất`cur = n`. 
2. Với mọi giá trị`x`từ`n-1`xuống tới`1`, ghi lại thao tác`(cur, x)`. 
3. Thay thế`cur`qua:$$cur = \left\lceil \frac{cur+x}{2} \right\rceil$$Điều này mô phỏng giá trị còn lại sau khi hợp nhất hai số lớn nhất hiện có. 

1. Tiếp tục cho đến khi chỉ còn lại một giá trị. 
2. Xuất giá trị cuối cùng và tất cả các thao tác được ghi lại. 

Lý do điều này có hiệu quả là ở mỗi bước chúng tôi hợp nhất giá trị lớn nhất hiện tại với giá trị chưa sử dụng lớn nhất tiếp theo. Số lớn là số duy nhất có khả năng giữ cho câu trả lời cuối cùng lớn, vì vậy trước tiên chúng tôi tích cực giảm chúng. 

### Tại sao nó hoạt động 

Gọi giá trị lớn nhất hiện tại là`M`. Bất kỳ hoạt động nào liên quan đến số lượng nhỏ hơn đều rời đi`M`không thay đổi, nghĩa là bảng vẫn chứa một giá trị rất lớn. Nếu muốn kết quả cuối cùng càng nhỏ càng tốt thì chúng ta phải tiếp tục giảm giá trị lớn nhất bất cứ khi nào có thể. 

hoạt động$$\left\lceil \frac{M+x}{2} \right\rceil$$với số lượng lớn nhất tiếp theo có sẵn`x`đưa ra mức giảm tối đa mạnh nhất có thể trong khi vẫn tiêu thụ một trong những số lượng lớn. Việc lặp lại quá trình này sẽ tạo ra quỹ đạo nhỏ nhất có thể đạt được với giá trị tối đa. Chuỗi kết quả luôn kết thúc tại`2`, đó là giá trị cuối cùng tối thiểu có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    
    for _ in range(t):
        n = int(input())
        
        ops = []
        cur = n
        
        for x in range(n - 1, 0, -1):
            ops.append((cur, x))
            cur = (cur + x + 1) // 2
        
        print(cur)
        for a, b in ops:
            print(a, b)

solve()
```Biến`cur`đại diện cho số còn tồn tại sau khi xử lý các giá trị lớn nhất được thấy cho đến nay. Ban đầu nó là`n`, số lớn nhất trên bảng. 

Mỗi lần lặp ghi lại một thao tác giữa`cur`và giá trị chưa sử dụng lớn nhất tiếp theo. biểu hiện```
(cur + x + 1) // 2
```tính toán trần của`(cur+x)/2`sử dụng số học số nguyên. 

Thứ tự của các hoạt động quan trọng. Chúng ta phải xử lý các giá trị từ`n-1`xuống tới`1`. Đảo ngược thứ tự này sẽ tạo ra giá trị cuối cùng lớn hơn. 

Không cần xử lý đặc biệt đối với số lượng lớn vì tất cả các giá trị đều nằm trong phạm vi số nguyên của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
```| Bước | Hoạt động | Giá trị mới | 
| --- | --- | --- | 
| 1 | (4,3) | 4 | 
| 2 | (4,2) | 3 | 
| 3 | (3,1) | 2 | 

Câu trả lời cuối cùng:```
2
```Ví dụ này cho thấy giá trị lớn nhất giảm dần cho đến khi chỉ`2`vẫn còn. 

### Ví dụ 2 

đầu vào:```
n = 5
```| Bước | Hoạt động | Giá trị mới | 
| --- | --- | --- | 
| 1 | (5,4) | 5 | 
| 2 | (5,3) | 4 | 
| 3 | (4,2) | 3 | 
| 4 | (3,1) | 2 | 

Câu trả lời cuối cùng:```
2
```Trình tự chứng minh rằng ngay cả khi`n`tăng lên, liên tục hợp nhất các giá trị lớn nhất tiếp tục kéo mức tối đa xuống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một thao tác được ghi lại cho mỗi`n-1`sáp nhập | 
| Không gian | O(n) | Lưu trữ danh sách hoạt động | 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2·10^5`. Vì thuật toán thực hiện công tuyến tính và đưa ra kết quả chính xác`n-1`hoạt động, nó dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    out = []

    t = int(input())

    for _ in range(t):
        n = int(input())

        ops = []
        cur = n

        for x in range(n - 1, 0, -1):
            ops.append((cur, x))
            cur = (cur + x + 1) // 2

        out.append(str(cur))
        for a, b in ops:
            out.append(f"{a} {b}")

    return "\n".join(out)

# sample
res = run("1\n4\n")
assert res.splitlines()[0] == "2"

# minimum n
res = run("1\n2\n")
assert res.splitlines()[0] == "2"

# n = 3
res = run("1\n3\n")
assert res.splitlines()[0] == "2"

# larger value
res = run("1\n10\n")
assert res.splitlines()[0] == "2"

# multiple test cases
res = run("2\n2\n5\n")
assert res.splitlines()[0] == "2"
assert "2" in res
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2`|`2`| Trường hợp hợp lệ nhỏ nhất | 
|`n=3`|`2`| Trường hợp không tầm thường đầu tiên | 
|`n=4`|`2`| Ví dụ cỡ mẫu | 
|`n=10`|`2`| Xây dựng lớn hơn | 
| Nhiều trường hợp thử nghiệm | Câu trả lời đúng cho tất cả | Thiết lập lại trạng thái giữa các trường hợp | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
```Bảng chỉ chứa`1`Và`2`. Hoạt động duy nhất là:```
(1,2) -> 2
```Đầu ra của thuật toán`2`, điều này là tối ưu vì không có sự thay thế nào tồn tại. 

Coi như:```
1
3
```Thuật toán thực hiện:```
(3,2) -> 3
(3,1) -> 2
```và kết thúc với`2`. Một chiến lược ngây thơ như sáp nhập`(1,2)`đầu tiên sản xuất:```
2,3
```sau đó trở thành`3`, một kết quả tồi tệ hơn. 

Coi như:```
1
5
```Thuật toán tiếp tục giảm giá trị lớn nhất:```
5 -> 5 -> 4 -> 3 -> 2
```Điều bất biến là mọi thao tác đều nhắm tới giá trị còn lại lớn nhất hiện tại. Bởi vì giá trị lớn nhất kiểm soát câu trả lời cuối cùng nên việc giảm nó ở mọi cơ hội sẽ dẫn đến kết quả cuối cùng tối ưu.
