---
title: "CF 1005A - Tanya và Cầu thang"
description: "Chúng ta được cung cấp một dãy số nguyên thể hiện những gì Tanya nói khi leo cầu thang trong một tòa nhà. Mỗi lần cô ấy bắt đầu một cầu thang mới, cô ấy luôn bắt đầu đếm từ 1 và tăng thêm 1 cho mỗi bước cho đến khi đến bước cuối cùng của cầu thang đó."
date: "2026-06-16T23:20:36+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1005
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 496 (Div. 3)"
rating: 800
weight: 1005
solve_time_s: 181
verified: true
draft: false
---

[CF 1005A - Tanya và Cầu thang](https://codeforces.com/problemset/problem/1005/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 3m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên thể hiện những gì Tanya nói khi leo cầu thang trong một tòa nhà. Mỗi lần cô ấy bắt đầu một cầu thang mới, cô ấy luôn bắt đầu đếm từ 1 và tăng thêm 1 cho mỗi bước cho đến khi đến bước cuối cùng của cầu thang đó. 

Vì vậy, trình tự không phải là tùy ý. Nó được hình thành bằng cách nối nhiều đoạn liên tiếp, trong đó mỗi đoạn là tiền tố của các số tự nhiên bắt đầu từ 1 và kết thúc ở một giá trị x nào đó. Nhiệm vụ của chúng tôi là chia chuỗi lại thành các phân đoạn đó và báo cáo có bao nhiêu phân đoạn cũng như độ dài của mỗi phân đoạn. 

Kích thước đầu vào tối đa là 1000 số. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào quét mảng với số lần không đổi hoặc thậm chí quét tuyến tính lồng nhau đều dễ dàng đủ nhanh. Không cần tiền xử lý, băm hoặc cấu trúc dữ liệu nâng cao. 

Sự tinh tế duy nhất nằm ở việc xác định chính xác nơi một cầu thang kết thúc và cầu thang tiếp theo bắt đầu. Một sai lầm ngây thơ là cho rằng mỗi khi giá trị chuỗi giảm hoặc mỗi khi chúng ta nhìn thấy số 1, chúng ta sẽ bắt đầu một phân đoạn mới mà không xử lý cẩn thận các ranh giới. Ví dụ: theo trình tự như`1 2 3 1 2 3 4`, việc khởi động lại ở mức 1 là hiển nhiên, nhưng trong các chuỗi tổng quát hơn, việc ngắt luôn xảy ra chính xác khi chuỗi ngừng tăng liên tiếp một đơn vị bắt đầu từ 1. 

Một cách tiếp cận không chính xác điển hình là chỉ kiểm tra`a[i] == 1`như một điểm đánh dấu ranh giới. Điều này có hiệu quả ở đây vì vấn đề đảm bảo tính hợp lệ, nhưng việc triển khai bất cẩn có thể bỏ sót rằng phần tử cuối cùng của phân đoạn không nhất thiết phải theo sau là 1; thay vào đó, phân đoạn kết thúc khi giá trị tiếp theo là 1 hoặc khi chúng ta đến cuối mảng. 

Giải thích đúng là mỗi bậc thang là một đoạn liền kề tối đa trong đó các giá trị chính xác`1, 2, 3, ..., k`. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để nghĩ về điều này là thử mọi phân vùng có thể có của chuỗi vào các bậc thang hợp lệ và xác minh từng bậc thang. Tại mỗi vị trí cắt tiềm năng, chúng tôi sẽ kiểm tra xem phân đoạn từ chỉ mục bắt đầu đó có tạo thành một chuỗi hợp lệ bắt đầu từ 1 và tăng dần lên 1 hay không. Trong trường hợp xấu nhất, chúng tôi có thể quét lại nhiều lần các phần của mảng để xác thực, dẫn đến hành vi bậc hai hoặc thậm chí bậc ba tùy thuộc vào cách cấu trúc kiểm tra. Mặc dù điều này vẫn nằm trong giới hạn cho n lên tới 1000, nhưng nó không cần thiết và nặng hơn mức cần thiết về mặt khái niệm. 

Quan sát quan trọng là chúng ta không cần phải đoán đoạn kết thúc ở đâu. Một đoạn phải kết thúc chính xác khi giá trị hiện tại bằng với độ dài của đoạn cho đến nay, bởi vì các bậc thang hợp lệ luôn trông giống như`1, 2, ..., length`. Vì vậy, chúng ta có thể tham lam mở rộng một phân đoạn cho đến khi vừa thấy số bằng với kích thước phân khúc hiện tại, sau đó đóng nó lại. 

Điều này làm giảm vấn đề xuống còn một đường tuyến tính duy nhất, duy trì giá trị dự kiến ​​hiện tại tiếp theo trong phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra phân vùng Brute Force | O(n²) | O(n) | Quá chậm/không cần thiết | 
| Đường chuyền đơn tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu quét mảng từ trái sang phải, coi mỗi phân đoạn mới là một cầu thang mới. 

Chúng tôi biết mọi bậc thang phải bắt đầu bằng 1, vì vậy chúng tôi kỳ vọng phần tử đầu tiên của bất kỳ đoạn nào cũng là 1. 
2. Khi chúng ta bắt đầu một phân đoạn mới, hãy đặt bộ đếm`expected = 1`và ghi lại rằng phân đoạn này đã bắt đầu. 
3. Di chuyển về phía trước trong mảng, kiểm tra từng giá trị. 

Nếu giá trị hiện tại khớp`expected`, chúng ta vẫn ở trong một cầu thang hợp lệ, nên chúng ta tăng dần`expected`bằng 1. 
4. Nếu tại bất kỳ thời điểm nào chúng ta đạt đến vị trí mà`expected`lại trở thành 1 sau khi hoàn thành một đoạn, điều đó cho biết điểm bắt đầu của một cầu thang mới. 
5. Một phân đoạn kết thúc chính xác khi chúng ta đã sử dụng đầy đủ tiền tố từ 1 cho đến một số k, tương ứng với thời điểm chúng ta chuẩn bị bắt đầu một phân đoạn 1 mới hoặc chúng ta đến cuối mảng. 
6. Lưu trữ độ dài của từng đoạn khi chúng ta hoàn thành nó. 

### Tại sao nó hoạt động 

Mỗi bậc thang hợp lệ được xác định duy nhất bởi điểm bắt đầu của nó và phải tuân thủ nghiêm ngặt các số nguyên liên tiếp tăng dần bắt đầu từ 1. Bởi vì đầu vào được đảm bảo hợp lệ nên không có sự mơ hồ trong phân đoạn: khi chúng ta thấy số 1, một phân đoạn mới phải bắt đầu và giữa hai số 1 liên tiếp, chuỗi phải tạo thành một chuỗi tăng dần liên tục. Cấu trúc này đảm bảo rằng phân đoạn tham lam không bao giờ cần phải quay lại hoặc đoán, vì bất kỳ sai lệch nào so với việc tăng từng đơn vị sẽ mâu thuẫn với tính hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

segments = []
i = 0

while i < n:
    expected = 1
    length = 0
    
    while i < n and a[i] == expected:
        length += 1
        expected += 1
        i += 1
    
    segments.append(length)

print(len(segments))
print(*segments)
```Mã quét mảng một lần. Vòng lặp bên ngoài tiến lên giữa các bậc thang và vòng lặp bên trong sử dụng chính xác một cầu thang hợp lệ bằng cách khớp với chuỗi tăng dần bắt đầu từ 1.`expected`biến thực thi cấu trúc của một cầu thang hợp lệ và`length`ghi lại bao nhiêu bước nó chứa. 

Một điểm tinh tế là chúng ta không bao giờ tìm kiếm số 1 tiếp theo một cách rõ ràng. Thay vào đó, vòng lặp bên trong tự nhiên dừng khi chuỗi bị ngắt, điều này chỉ có thể xảy ra ở ranh giới giữa các bậc thang vì đầu vào được đảm bảo hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
1 2 3 1 2 3 4
```| tôi | một [tôi] | dự kiến ​​| chiều dài | phân đoạn đã kết thúc | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | không | 
| 1 | 2 | 2 | 2 | không | 
| 2 | 3 | 3 | 3 | vâng | 
| 3 | 1 | 1 | 1 | không | 
| 4 | 2 | 2 | 2 | không | 
| 5 | 3 | 3 | 3 | không | 
| 6 | 4 | 4 | 4 | vâng | 

Chúng ta thu được hai đoạn có độ dài 3 và 4. Dấu vết cho thấy mỗi lần`expected`hoàn thành một lượt chạy, cầu thang sẽ kết thúc chính xác tại thời điểm đó. 

### Ví dụ 2 

đầu vào:```
5
1 2 1 1 2
```| tôi | một [tôi] | dự kiến ​​| chiều dài | phân đoạn đã kết thúc | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | không | 
| 1 | 2 | 2 | 2 | vâng | 
| 2 | 1 | 1 | 1 | vâng | 
| 3 | 1 | 1 | 1 | không | 
| 4 | 2 | 2 | 2 | vâng | 

Đầu ra là`3`các đoạn có độ dài`2 1 2`. 

Dấu vết này nêu bật rằng các cầu thang một thành phần là hợp lệ và được xử lý chính xác khi trình tự khởi động lại ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được truy cập chính xác một lần trong quá trình quét phân đoạn | 
| Không gian | O(1) | Chỉ các bộ đếm và một danh sách nhỏ độ dài đoạn được sử dụng | 

Kích thước đầu vào tối đa là 1000, do đó quá trình quét tuyến tính thấp hơn nhiều so với giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài bộ nhớ đầu ra, điều này là không thể tránh khỏi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    segments = []
    i = 0

    while i < n:
        expected = 1
        length = 0
        while i < n and a[i] == expected:
            length += 1
            expected += 1
            i += 1
        segments.append(length)

    return str(len(segments)) + "\n" + " ".join(map(str, segments))

# provided sample
assert run("7\n1 2 3 1 2 3 4\n") == "2\n3 4"

# single stairway
assert run("3\n1 2 3\n") == "1\n3"

# multiple single-step stairways
assert run("3\n1 1 1\n") == "3\n1 1 1"

# alternating short stairs
assert run("5\n1 2 1 2 3\n") == "2\n2 3"

# maximum length single staircase
assert run("1\n1\n") == "1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 | 1/3 | cầu thang đơn đầy đủ | 
| 1 1 1 | 3 / 1 1 1 | khởi động lại liên tiếp | 
| 1 2 1 2 3 | 2 / 2 3 | phân khúc hỗn hợp | 
| 1 | 1/1 | kích thước tối thiểu | 

## Vỏ cạnh 

Trường hợp một cạnh là khi toàn bộ chuỗi bao gồm nhiều bậc thang độc lập có độ dài 1, chẳng hạn như`1 1 1 1`. Thuật toán bắt đầu một phân đoạn tại mỗi`1`, khớp ngay lập tức`expected = 1`, tiêu thụ một phần tử và đóng phân khúc. Điều này tạo ra bốn đoạn có độ dài 1, phù hợp với cách diễn giải dự định. 

Một trường hợp khác là một cầu thang dài như`1 2 3 4 5`. Ở đây, vòng lặp bên trong không bao giờ bị đứt sớm,`expected`tăng dần cho đến hết và đúng một đoạn được ghi lại với độ dài đầy đủ chính xác.
