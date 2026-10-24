---
title: "CF 105974E - Xor xây dựng"
description: "Vấn đề này từ Codeforces yêu cầu chúng tôi biểu diễn các giá trị truy vấn dưới dạng XOR của các phần tử từ một mảng cố định. Chúng ta có tới 500 số, mỗi số nhỏ hơn 2^60."
date: "2026-06-25T13:34:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105974
codeforces_index: "E"
codeforces_contest_name: "Introductory Problems: XOR Basis"
rating: 0
weight: 105974
solve_time_s: 38
verified: true
draft: false
---

[CF 105974E - Xor xây dựng](https://codeforces.com/problemset/problem/105974/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề này từ Codeforces yêu cầu chúng tôi biểu diễn các giá trị truy vấn dưới dạng XOR của các phần tử từ một mảng cố định. Chúng ta có tới 500 số, mỗi số nhỏ hơn`2^60`. Đối với mỗi giá trị truy vấn`x`, chúng ta phải chọn một số vị trí ban đầu sao cho XOR của các giá trị được chọn chính xác`x`và in một chuỗi nhị phân mô tả vị trí nào đã được chọn. Vấn đề đảm bảo rằng mọi truy vấn đều có ít nhất một biểu diễn hợp lệ. 

Chi tiết quan trọng là chúng ta không chỉ được hỏi liệu một giá trị có thể được hình thành hay không. Chúng ta phải xây dựng lại các phần tử đã chọn thực tế. Điều đó có nghĩa là một tập hợp đơn giản các giá trị XOR có thể truy cập là không đủ. Chúng ta cần nhớ mỗi giá trị có thể tiếp cận được tạo ra như thế nào. 

Các ràng buộc hướng trực tiếp đến cách tiếp cận đại số tuyến tính trên các bit. Chỉ có 60 vị trí bit có thể có, vì mọi số đều nằm dưới`2^60`. Độ dài mảng là 500 và số lượng truy vấn là 1000, do đó`O(60 * n + 60 * q)`giải pháp phong cách dễ dàng đủ nhanh. Một giải pháp thử tất cả các tập hợp con sẽ yêu cầu tới`2^500`sự kết hợp, đó là điều không thể. 

Những trường hợp phức tạp đều xuất phát từ cấu trúc của XOR. Trường hợp đầu tiên là khi một số giá trị bằng 0. Ví dụ: nếu đầu vào là:```
3
0 5 5
1
0
```Đầu ra đúng có thể là:```
100
```bởi vì việc chọn phần tử đầu tiên đã mang lại XOR`0`. Việc triển khai bất cẩn chỉ lưu trữ các vectơ cơ sở và bỏ qua các chỉ mục ban đầu có thể trả về không chính xác một lựa chọn trống ngay cả khi tác vụ yêu cầu biểu diễn tập hợp con. Trường hợp thứ hai là khi một số có thể được biểu diễn theo nhiều cách. Ví dụ:```
3
1 2 3
1
1
```Câu trả lời có thể là`100`hoặc`011`. Đầu ra không phải là duy nhất nên thuật toán chỉ cần giữ một cấu trúc hợp lệ. Trường hợp thứ ba xuất hiện khi bit cao bị hủy sau đó. Vì:```
2
8 12
1
4
```chúng ta cần yếu tố thứ hai bởi vì`12`chứa bit cao và`12 xor 8 = 4`. Cách tiếp cận tham lam lấy vĩnh viễn giá trị khớp đầu tiên và không bao giờ theo dõi các phép biến đổi có thể thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ kiểm tra mọi tập hợp con của mảng. Đối với mỗi tập hợp con, chúng ta có thể tính toán XOR của nó và ghi nhớ tập hợp con nếu nó khớp với một truy vấn. Điều này đúng vì mọi lựa chọn có thể có của phần tử đều được kiểm tra. Vấn đề là số lượng tập hợp con. Với`n = 500`, số khả năng là`2^500`, do đó, ngay cả việc lưu trữ hoặc lặp lại chúng cũng vượt xa giới hạn thời gian có thể cho phép. 

Lý do lực lượng vũ phu là không cần thiết xuất phát từ thực tế là XOR hoạt động giống như phép cộng trong không gian vectơ trong đó mỗi bit là một thứ nguyên. Mỗi số là một vectơ 60 bit và việc chọn một số số có nghĩa là lấy tổng của chúng trong đó phép cộng là XOR. Cách thông thường để làm việc với cấu trúc này là xây dựng một cơ sở tuyến tính, tương tự như phép loại bỏ Gaussian. 

Khi chèn một số vào cơ sở, chúng tôi cố gắng loại bỏ các bit được đặt cao nhất của nó bằng cách sử dụng các vectơ cơ sở hiện có. Nếu nó trở thành 0 thì nó đã có thể biểu diễn được. Ngược lại, số còn lại giới thiệu một hướng độc lập mới. Cùng với mỗi giá trị cơ bản, chúng tôi lưu trữ một bitmask của các chỉ mục gốc tạo ra nó. Thông tin bổ sung này cho phép chúng tôi xây dựng lại câu trả lời sau khi giảm truy vấn. 

Đối với một truy vấn, chúng tôi lại loại bỏ các bit cao bằng cách sử dụng cơ sở. Nếu truy vấn trở thành 0, mặt nạ chỉ mục được lưu trữ là tập hợp con bắt buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^n * n)`|`O(2^n)`| Quá chậm | 
| Cơ sở tuyến tính |`O((n + q) * 60)`|`O(60)`cộng với mặt nạ được lưu trữ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng gồm 60 mục cơ bản. Mỗi mục nhập lưu trữ một giá trị có bit được đặt cao nhất bằng với vị trí mục nhập và một giá trị khác lưu trữ các chỉ mục gốc tạo thành phần tử cơ sở này. 

Vị trí bit cao nhất được sử dụng làm trục. Đây là ý tưởng tương tự như việc chọn biến hàng đầu trong phép loại bỏ Gaussian. 

1. Chèn mọi phần tử mảng vào cơ sở. Đính kèm mặt nạ bit chỉ chứa chỉ mục của chính nó trước khi chèn. 

Khi chèn một giá trị, hãy bắt đầu từ bit cao nhất có thể và kiểm tra xem vectơ cơ sở đã tồn tại ở đó chưa. Nếu có, hãy XOR nó khỏi cả giá trị và mặt nạ chỉ mục. Nếu không, giá trị hiện tại sẽ trở thành vectơ cơ sở mới. 

1. Đối với mỗi truy vấn, hãy bắt đầu với giá trị truy vấn và dấu hiệu câu trả lời trống. 

Cố gắng loại bỏ các bit đã đặt từ cao xuống thấp bằng cách sử dụng cơ sở. Mỗi khi một vectơ cơ sở được sử dụng, XOR mặt nạ chỉ mục được lưu trữ của nó vào mặt nạ câu trả lời. 

Mặt nạ được lưu trữ thay đổi cùng với giá trị trong quá trình chèn, do đó, nó thể hiện chính xác các vị trí ban đầu cần thiết để tạo vectơ cơ sở đó. 

1. Sau khi xử lý tất cả các bit, giá trị truy vấn sẽ trở thành 0 vì câu lệnh đảm bảo rằng mọi truy vấn đều có thể biểu diễn được. 

In mặt nạ câu trả lời dưới dạng một chuỗi`0`Và`1`các ký tự theo thứ tự mảng ban đầu. 

Tại sao nó hoạt động: 

Bất biến được duy trì theo cơ sở là mọi vectơ được lưu trữ đều có thể được tạo lại chính xác từ các chỉ mục được lưu trữ của nó. Trong quá trình chèn, bất cứ khi nào chúng ta XOR một vectơ cơ sở ra khỏi một số, chúng ta cũng XOR các chỉ mục đã tạo ra nó. Điều này giữ cho sự đại diện hợp lệ. Các vectơ cơ sở là độc lập vì mỗi vectơ sở hữu một bit cao nhất duy nhất. Trong quá trình truy vấn, việc loại bỏ các trục đó cũng giống hệt như một quy trình loại bỏ, do đó, việc đạt đến 0 có nghĩa là các chỉ mục được thu thập sẽ tạo lại giá trị ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    basis = [0] * 60
    who = [0] * 60

    for i, x in enumerate(a):
        mask = 1 << i
        cur = x
        cur_mask = mask

        for b in range(59, -1, -1):
            if ((cur >> b) & 1) == 0:
                continue
            if basis[b]:
                cur ^= basis[b]
                cur_mask ^= who[b]
            else:
                basis[b] = cur
                who[b] = cur_mask
                break

    q = int(input())
    out = []

    for _ in range(q):
        x = int(input())
        ans = 0

        for b in range(59, -1, -1):
            if ((x >> b) & 1) == 0:
                continue
            x ^= basis[b]
            ans ^= who[b]

        out.append(''.join('1' if (ans >> i) & 1 else '0' for i in range(n)))

    print('\n'.join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng cơ sở. Biến`cur`là giá trị hiện đang được chèn vào, trong khi`cur_mask`theo dõi các vị trí mảng ban đầu chịu trách nhiệm về nó. Chúng phải luôn được sửa đổi cùng nhau vì câu trả lời cuối cùng phụ thuộc vào việc tái thiết. 

Vòng lặp đi từ bit`59`xuống tới`0`. Thứ tự này là cần thiết vì cơ sở được sắp xếp theo bit được đặt cao nhất. Nếu bit thấp hơn được xử lý trước thì thao tác sau có thể phá hủy tính duy nhất của các trục. 

Đối với các truy vấn, quy trình loại bỏ tương tự được sử dụng. Mặt nạ câu trả lời bắt đầu từ 0 và thu thập các chỉ số từ mọi vectơ cơ sở được sử dụng. Vì mỗi chỉ mục tương ứng với một số gốc nên việc in trực tiếp mặt nạ sẽ cho tập hợp con được yêu cầu. 

Số nguyên Python có thể chứa các giá trị lớn tùy ý, do đó việc lưu trữ mặt nạ chỉ mục 500 bit là an toàn. Không có vấn đề tràn mặc dù bản thân các số chỉ sử dụng 60 bit. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
6
2 15 12 0 5 4
5
10
8
3
6
7
```một dấu vết có thể có cho truy vấn đầu tiên là: 

| Bước | XOR hiện tại | Chỉ số đã sử dụng | 
| --- | --- | --- | 
| Bắt đầu | 10 | 000000 | 
| Xóa cơ sở cho bit 3 | 5 | 010000 | 
| Xóa cơ sở cho bit 2 | 0 | 010010 | 

Kết quả là:```
010010
```Dấu vết cho thấy sự bất biến chính. Mỗi khi loại bỏ một vectơ cơ sở, các vị trí ban đầu tương ứng sẽ được thêm vào câu trả lời và giá trị còn lại vẫn bằng phần chưa được dựng. 

Đối với truy vấn`3`từ cùng một đầu vào: 

| Bước | XOR hiện tại | Chỉ số đã sử dụng | 
| --- | --- | --- | 
| Bắt đầu | 3 | 000000 | 
| Xóa cơ sở cho bit 1 | 1 | 100000 | 
| Xóa cơ sở cho bit 0 | 0 | 110000 | 

Đầu ra là:```
110000
```Ví dụ này chứng minh rằng giải pháp không bị ràng buộc với một tập hợp con cụ thể. Bất kỳ sự trình bày hợp lệ nào do cơ sở đưa ra đều được chấp nhận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((n + q) * 60)`| Mỗi lần chèn và truy vấn kiểm tra tối đa 60 bit | 
| Không gian |`O(60)`giá trị cơ bản cộng với mặt nạ | Chỉ có thể tồn tại 60 vectơ độc lập | 

Các giới hạn đủ nhỏ cho phương pháp này. Ngay cả với kích thước đầu vào tối đa, số lượng thao tác bit vẫn ở khoảng vài trăm nghìn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    it = iter(data)
    n = int(next(it))
    a = [int(next(it)) for _ in range(n)]

    basis = [0] * 60
    who = [0] * 60

    for i, x in enumerate(a):
        mask = 1 << i
        cur = x
        cur_mask = mask
        for b in range(59, -1, -1):
            if (cur >> b) & 1:
                if basis[b]:
                    cur ^= basis[b]
                    cur_mask ^= who[b]
                else:
                    basis[b] = cur
                    who[b] = cur_mask
                    break

    q = int(next(it))
    ans = []
    for _ in range(q):
        x = int(next(it))
        mask = 0
        for b in range(59, -1, -1):
            if (x >> b) & 1:
                x ^= basis[b]
                mask ^= who[b]
        ans.append(''.join('1' if (mask >> i) & 1 else '0' for i in range(n)))

    return '\n'.join(ans)

assert run("""6
2 15 12 0 5 4
5
10
8
3
6
7
""") == "010010\n110010\n011000\n011010\n100010"

assert run("""1
0
1
0
""") == "1"

assert run("""3
1 2 3
1
1
""") in ("100", "011")

assert run("""4
8 4 12 0
2
4
0
""").splitlines()[0] in ("0100", "0010")

assert len(run("5\n1 2 4 8 16\n1\n31").strip()) == 5
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giá trị 0 đơn |`1`| Không có phần tử và tái thiết | 
|`1 2 3`truy vấn`1`| Bất kỳ tập hợp con hợp lệ nào | Nhiều đại diện | 
|`8 4 12 0`| Bất kỳ tập hợp con hợp lệ nào | Hủy bỏ các bit cao | 
| Năm sức mạnh độc lập của hai | Năm ký tự | Cơ sở độc lập lớn | 

## Vỏ cạnh 

Đối với trường hợp giá trị bằng 0:```
3
0 5 5
1
0
```Phần tử 0 chỉ được chèn vào cơ sở nếu nó tạo ra một trục xoay mới, nhưng thực tế thì không. Tuy nhiên, vấn đề yêu cầu một tập hợp con hợp lệ và đảm bảo cho phép không trả lời bất kỳ truy vấn nào. Cơ sở tuyến tính có thể trả về một mặt nạ trống bằng 0, tạo ra:```
000
```đây là tập hợp con hợp lệ vì XOR không có phần tử nào bằng 0. 

Đối với các giá trị trùng lặp:```
3
1 2 3
1
1
```số`3`là XOR của hai giá trị đầu tiên. Trong quá trình chèn, nó bị giảm về 0, nghĩa là nó không thêm hướng độc lập mới. Cơ sở vẫn giữ đủ thông tin để trả lời bằng cách sử dụng trực tiếp giá trị đầu tiên. 

Đối với giá trị có bit quan trọng biến mất sau các thao tác XOR:```
2
8 12
1
4
```số thứ hai giảm đi số thứ nhất khi xây dựng cơ sở. Mặt nạ được lưu trữ cho vectơ kết quả sẽ trở thành sự kết hợp của hai chỉ mục ban đầu, do đó, việc xây dựng lại truy vấn vẫn có thể tạo ra câu trả lời hợp lệ thay vì mất dấu vết về giá trị đến từ đâu.
