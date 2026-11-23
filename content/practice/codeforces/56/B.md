---
title: "CF 56B - Hoán vị hư hỏng"
description: "Chúng ta bắt đầu từ hoán vị được sắp xếp 1 2 3 ... n. Ai đó chọn chính xác một đoạn liền kề và đảo ngược nó. Chúng ta được cung cấp hoán vị cuối cùng và phải xác định xem liệu nó có thể được tạo ra bằng chính xác một lần đảo ngược như vậy hay không. Nhiệm vụ không phải là sắp xếp mảng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 56
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 52 (Div. 2)"
rating: 1300
weight: 56
solve_time_s: 102
verified: true
draft: false
---

[CF 56B - Hoán vị sai lầm](https://codeforces.com/problemset/problem/56/B) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu từ hoán vị được sắp xếp`1 2 3 ... n`. Ai đó chọn chính xác một đoạn liền kề và đảo ngược nó. Chúng ta được cung cấp hoán vị cuối cùng và phải xác định xem liệu nó có thể được tạo ra bằng chính xác một lần đảo ngược như vậy hay không. 

Nhiệm vụ không phải là sắp xếp mảng. Chúng ta chỉ cần xác định một đoạn`[l, r]`sao cho việc đảo ngược phân đoạn đó trong mảng được sắp xếp ban đầu sẽ tạo ra hoán vị đã cho. Nếu không có phân đoạn như vậy tồn tại, chúng tôi in`0 0`. 

Những hạn chế là nhỏ,`n ≤ 1000`, do đó ngay cả các thuật toán bậc hai cũng được chấp nhận. MỘT`O(n^2)`giải pháp thực hiện khoảng một triệu thao tác trong trường hợp xấu nhất, điều này không đáng kể trong giới hạn 2 giây. Điều này có nghĩa là chúng ta không cần cấu trúc dữ liệu phức tạp hoặc thuật toán nâng cao. Việc triển khai rõ ràng quan trọng hơn việc tối ưu hóa vi mô. 

Phần khó khăn không phải là hiệu quả mà là tính chính xác trong các trường hợp khó khăn. 

Một sai lầm dễ mắc phải là quên rằng sự đảo ngược phải đến từ mảng được sắp xếp ban đầu chứ không phải từ một số hoán vị trung gian. Coi như:```
5
1 3 2 5 4
```Một cách tiếp cận bất cẩn có thể nhận thấy hai đoạn đang giảm dần và cố gắng kết hợp chúng bằng cách nào đó. Nhưng một lần đảo ngược không thể đồng thời hoán đổi cả hai`(2,3)`Và`(4,5)`. Đầu ra đúng là:```
0 0
```Một trường hợp tinh tế khác xảy ra khi hoán vị đã được sắp xếp:```
4
1 2 3 4
```Tuyên bố yêu cầu đảo ngược một phân đoạn với các điểm cuối khác nhau. Không có sự đảo ngược nào làm thay đổi mảng thành chính nó trừ khi chúng ta đảo ngược đoạn có độ dài 1, điều này bị cấm vì`l < r`. Câu trả lời đúng là:```
0 0
```Một cách triển khai ngây thơ chỉ đơn giản là tìm kiếm những thông tin không khớp có thể in sai một cái gì đó như`1 1`. 

Trường hợp cạnh thứ ba là khi đoạn đảo ngược chạm vào ranh giới:```
5
5 4 3 2 1
```Ở đây toàn bộ mảng đã bị đảo ngược, vì vậy câu trả lời đúng là:```
1 5
```Ở đây thường xảy ra các lỗi khác nhau vì đoạn này bắt đầu ở phần tử đầu tiên và kết thúc ở phần tử cuối cùng. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Vì hoán vị ban đầu luôn là`1 2 3 ... n`, chúng ta có thể thử mọi cặp có thể`(l, r)`, đảo ngược phân đoạn đó và kiểm tra xem kết quả có khớp với hoán vị đầu vào hay không. 

có`O(n^2)`các phân đoạn có thể. Đảo ngược và so sánh chi phí mảng`O(n)`thời gian, do đó tổng độ phức tạp trở thành`O(n^3)`. Với`n = 1000`, điều này có nghĩa là khoảng một tỷ thao tác trong trường hợp xấu nhất, quá chậm trong Python. 

Cấu trúc của bài toán cho chúng ta khả năng quan sát tốt hơn nhiều. Nếu hoán vị được tạo bằng cách đảo ngược một đoạn thì ở mọi nơi bên ngoài đoạn đó, các số vẫn phải xuất hiện ở vị trí ban đầu. 

Điều đó có nghĩa là chỉ số đầu tiên trong đó`a[i] != i + 1`phải là điểm cuối bên trái của đoạn đảo ngược. Tương tự, khi chúng ta bắt đầu di chuyển qua đoạn đảo ngược, các giá trị phải giảm chính xác như sự đảo ngược sẽ tạo ra. 

Ví dụ:```
1 6 5 4 3 2 7 8
```Sự không khớp đầu tiên xảy ra ở vị trí`2`. Từ đó, số lượng giảm dần cho đến vị trí`6`. Đảo ngược khoảng đó trong mảng được sắp xếp ban đầu sẽ tái tạo lại hoán vị một cách chính xác. 

Điều này làm giảm nhiệm vụ xác định một phân khúc ứng cử viên và xác minh nó. Chúng tôi chỉ quét mảng một số lần không đổi, do đó độ phức tạp trở nên tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị. 
2. Tìm vị trí đầu tiên`l`Ở đâu`a[l] != l + 1`. 

Nếu không có vị trí như vậy tồn tại thì hoán vị đã được sắp xếp, do đó không có sự đảo ngược hợp lệ nào tồn tại. In`0 0`. 
3. Bắt đầu từ`l`, mở rộng`r`trong khi dãy đang giảm dần. 

Đoạn đảo ngược lấy từ mảng đã sắp xếp phải xuất hiện theo thứ tự giảm dần trong hoán vị cuối cùng. Bước này xác định phân khúc ứng cử viên tối đa. 
4. Đảo ngược mảng con từ`l`ĐẾN`r`. 

Chúng tôi thực hiện đảo ngược trực tiếp trên bản sao của hoán vị để kiểm tra xem thao tác này có khôi phục được thứ tự đã sắp xếp hay không. 
5. Kiểm tra xem mảng kết quả có bằng không`1 2 3 ... n`. 

Nếu có thì in`l + 1`Và`r + 1`sử dụng chỉ mục dựa trên 1. 

Ngược lại, in`0 0`. 

### Tại sao nó hoạt động 

Vị trí không chính xác đầu tiên phải thuộc về phân đoạn đảo ngược, bởi vì các vị trí trước nó đã khớp với hoán vị được sắp xếp ban đầu. 

Bên trong đoạn đảo ngược, các giá trị phải xuất hiện theo thứ tự giảm dần. Vì mảng ban đầu đang tăng dần nên việc đảo ngược bất kỳ phần liền kề nào sẽ lật thứ tự đó đúng một lần. Mở rộng`r`trong khi trình tự giảm dần xác định phân khúc ứng cử viên duy nhất có thể. 

Nếu việc đảo ngược phân đoạn đó sẽ khôi phục mảng đã được sắp xếp thì hoán vị được tạo ra bằng chính xác một lần đảo ngược. Nếu không, thì không phân đoạn nào khác có thể hoạt động được, bởi vì bất kỳ phân đoạn hợp lệ nào cũng phải bắt đầu ở điểm không khớp đầu tiên và tuân theo mô hình giảm dần được tạo bởi sự đảo ngược. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    l = -1

    for i in range(n):
        if a[i] != i + 1:
            l = i
            break

    if l == -1:
        print(0, 0)
        return

    r = l

    while r + 1 < n and a[r] > a[r + 1]:
        r += 1

    b = a[:]
    b[l:r + 1] = reversed(b[l:r + 1])

    if b == list(range(1, n + 1)):
        print(l + 1, r + 1)
    else:
        print(0, 0)

solve()
```Vòng lặp đầu tiên tìm kiếm sự không khớp sớm nhất giữa hoán vị và thứ tự được sắp xếp. Điều này xác định nơi đoạn đảo ngược phải bắt đầu. 

Nếu không có sự không khớp nào tồn tại thì hoán vị đã được sắp xếp. Vì vấn đề đòi hỏi`l < r`, chúng ta không thể sử dụng thao tác trống hoặc đảo ngược độ dài 1, vì vậy câu trả lời trở thành`0 0`. 

Vòng lặp thứ hai mở rộng ranh giới bên phải trong khi dãy tiếp tục giảm dần. Điều này ghi lại hình dạng chính xác được tạo ra bằng cách đảo ngược một đoạn tăng dần từ hoán vị ban đầu. 

Dòng:```
b[l:r + 1] = reversed(b[l:r + 1])
```tạo phiên bản đã sửa của mảng sau khi hoàn tác việc đảo ngược bị nghi ngờ. 

Cuối cùng, chúng tôi so sánh với hoán vị được sắp xếp. Bước xác minh này rất cần thiết vì chỉ phân khúc giảm dần là không đủ. Mảng như:```
1 5 4 3 2 6 8 7
```chứa nhiều vùng bị xáo trộn và không thể sửa chữa bằng một lần đảo ngược. 

Việc sử dụng mảng đã sao chép sẽ tránh vô tình sửa đổi dữ liệu gốc. Các ranh giới sử dụng các quy ước cắt lát Python một cách cẩn thận, trong đó`r + 1`là cần thiết vì điểm cuối phù hợp là độc quyền. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
1 6 5 4 3 2 7 8
```| Bước | tôi | r | Trạng thái mảng | 
| --- | --- | --- | --- | 
| Tìm sự không khớp đầu tiên | 1 | - | 1 6 5 4 3 2 7 8 | 
| Mở rộng phân khúc giảm | 1 | 5 | 1 6 5 4 3 2 7 8 | 
| Đoạn ngược | 1 | 5 | 1 2 3 4 5 6 7 8 | 
| Xác minh | 1 | 5 | Đã sắp xếp | 

Đầu ra:```
2 6
```Vùng giảm dần`6 5 4 3 2`khớp chính xác với những gì đảo ngược`[2,6]`sẽ tạo ra từ một mảng được sắp xếp. 

### Ví dụ 2 

đầu vào:```
5
1 3 2 5 4
```| Bước | tôi | r | Trạng thái mảng | 
| --- | --- | --- | --- | 
| Tìm sự không khớp đầu tiên | 1 | - | 1 3 2 5 4 | 
| Mở rộng phân khúc giảm | 1 | 2 | 1 3 2 5 4 | 
| Đoạn ngược | 1 | 2 | 1 2 3 5 4 | 
| Xác minh | 1 | 2 | Chưa được sắp xếp | 

Đầu ra:```
0 0
```Sự đảo ngược đầu tiên sửa chữa cặp`(3,2)`nhưng lá`(5,4)`vỡ. Điều này xác nhận rằng một lần đảo chiều là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét mảng một số lần không đổi | 
| Không gian | O(n) | Chúng tôi lưu trữ một bản sao hoán vị để xác minh | 

Với`n ≤ 1000`, ngay cả nghiệm bậc hai cũng có thể vượt qua một cách thoải mái. Cách tiếp cận tuyến tính đủ nhanh và dễ dàng và giúp việc triển khai đơn giản và đáng tin cậy. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))

        l = -1

        for i in range(n):
            if a[i] != i + 1:
                l = i
                break

        if l == -1:
            return "0 0"

        r = l

        while r + 1 < n and a[r] > a[r + 1]:
            r += 1

        b = a[:]
        b[l:r + 1] = reversed(b[l:r + 1])

        if b == list(range(1, n + 1)):
            return f"{l + 1} {r + 1}"

        return "0 0"

    return solve()

# provided sample
assert run("8\n1 6 5 4 3 2 7 8\n") == "2 6", "sample 1"

# already sorted
assert run("4\n1 2 3 4\n") == "0 0", "already sorted"

# reverse whole array
assert run("5\n5 4 3 2 1\n") == "1 5", "entire array reversed"

# impossible with one reversal
assert run("5\n1 3 2 5 4\n") == "0 0", "two separate disruptions"

# minimum valid reversal
assert run("2\n2 1\n") == "1 2", "smallest valid segment"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3 4`|`0 0`| Các hoán vị đã được sắp xếp không hợp lệ | 
|`5 4 3 2 1`|`1 5`| Phân đoạn có thể bao gồm toàn bộ mảng | 
|`1 3 2 5 4`|`0 0`| Một sự đảo ngược không thể khắc phục được nhiều vùng bị xáo trộn | 
|`2 1`|`1 2`| Trường hợp nhỏ nhất không tầm thường | 

## Vỏ cạnh 

Hãy xem xét hoán vị đã được sắp xếp:```
4
1 2 3 4
```Thuật toán quét tìm điểm không khớp đầu tiên và không tìm thấy gì, vì vậy`l`còn lại`-1`. Lúc này nó in ngay:```
0 0
```Điều này đúng vì bài toán yêu cầu đảo ngược với các điểm cuối khác biệt. 

Bây giờ hãy xem xét một sự đảo chiều chạm vào cả hai ranh giới:```
5
5 4 3 2 1
```Sự không khớp đầu tiên xảy ra ở chỉ mục`0`. Quá trình quét giảm dần tiếp tục cho đến khi lập chỉ mục`4`. Việc đảo ngược phân đoạn đó sẽ khôi phục chính xác thứ tự đã sắp xếp, do đó thuật toán sẽ đưa ra:```
1 5
```Điều này xác nhận rằng các chỉ số biên được xử lý chính xác. 

Cuối cùng, kiểm tra một hoán vị với hai vùng bị hỏng riêng biệt:```
5
1 3 2 5 4
```Sự không phù hợp đầu tiên xuất hiện ở vị trí`2`. Đoạn giảm dần chỉ trở thành`3 2`. Đảo ngược nó tạo ra:```
1 2 3 5 4
```Mảng vẫn chưa được sắp xếp nên việc xác minh không thành công và thuật toán in ra:```
0 0
```Bước xác minh cuối cùng này ngăn chặn các kết quả dương tính giả từ các phân đoạn giảm cục bộ không đại diện cho một sự đảo chiều toàn cầu duy nhất.
