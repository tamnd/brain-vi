---
title: "CF 250B - Khôi phục IPv6"
description: "Địa chỉ IPv6 ở dạng đầy đủ là một cấu trúc cố định được tạo thành từ tám khối. Mỗi khối đại diện cho 16 bit và được viết chính xác bằng bốn ký tự thập lục phân, bao gồm cả số 0 đứng đầu khi cần thiết."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 250
codeforces_index: "B"
codeforces_contest_name: "CROC-MBTU 2012, Final Round (Online version, Div. 2)"
rating: 1500
weight: 250
solve_time_s: 65
verified: true
draft: false
---

[CF 250B - Khôi phục IPv6](https://codeforces.com/problemset/problem/250/B) 

**Đánh giá:** 1500 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Địa chỉ IPv6 ở dạng đầy đủ là một cấu trúc cố định được tạo thành từ tám khối. Mỗi khối đại diện cho 16 bit và được viết chính xác bằng bốn ký tự thập lục phân, bao gồm cả số 0 đứng đầu khi cần thiết. Các khối được phân tách bằng dấu hai chấm, do đó, một địa chỉ được mở rộng hoàn toàn luôn có cùng độ dài và luôn chứa bảy dấu hai chấm. 

Đầu vào chúng tôi nhận được là phiên bản rút gọn của địa chỉ đó. Hai lần nén độc lập có thể đã được áp dụng. Đầu tiên, trong mỗi khối, các số 0 đứng đầu có thể đã bị xóa, do đó, một khối như`00ea`có thể xuất hiện như`ea`. Thứ hai, một khối liền kề có giá trị bằng 0 có thể đã được thay thế bằng dấu hai chấm kép`::`, viết tắt của một hoặc nhiều khối 0 đầy đủ. Điều quan trọng là sự thay thế này chỉ có thể được sử dụng một lần cho mỗi địa chỉ. 

Nhiệm vụ là xây dựng lại biểu diễn tám khối đầy đủ ban đầu. Điều đó có nghĩa là chúng ta phải khôi phục cả hai khối 0 bị thiếu từ`::`và khôi phục từng khối về chính xác bốn chữ số thập lục phân bằng cách thêm các số 0 đứng đầu. 

Các ràng buộc rất nhỏ, tối đa là 100 địa chỉ, do đó, ngay cả cách tiếp cận phân tích cú pháp đơn giản cũng đủ nhanh. Khó khăn chính không phải là hiệu suất mà là tính chính xác trong việc xử lý cấu trúc mơ hồ của`::`, đặc biệt là phân biệt vị trí của các khối 0 bị bỏ qua và có bao nhiêu khối. 

Một số tình huống khó khăn có xu hướng phá vỡ các giải pháp ngây thơ. Nếu đầu vào chính xác`::`, nó đại diện cho tám khối 0 và nhiều cách triển khai coi nó là khối trống hoặc một khối đơn lẻ một cách không chính xác. Nếu như`::`xuất hiện ở đầu hoặc cuối, một bên của phần tách sẽ trống, điều này vẫn phải được diễn giải chính xác. Một trường hợp tế nhị khác là đảm bảo rằng khi mở rộng`::`, tổng số khối sẽ chính xác là tám, không nhiều hơn hoặc ít hơn, bất kể có bao nhiêu khối rõ ràng xuất hiện xung quanh nó. 

## Phương pháp tiếp cận 

Vấn đề mở rộng IPv6 đầy đủ chủ yếu là về việc đảo ngược mã hóa bị mất. Một ý tưởng mạnh mẽ là thử tất cả các vị trí có thể có của các khối 0 cho mỗi`::`và xác nhận việc tái cấu trúc nào mang lại chính xác tám khối và khớp với các phần hiển thị. Điều này sẽ liên quan đến việc chia chuỗi xung quanh`::`, đoán xem có bao nhiêu khối 0 đã được nén và kiểm tra tính nhất quán. Mặc dù đơn giản về mặt khái niệm, nhưng nó đưa ra lý luận tổ hợp không cần thiết mặc dù cấu trúc thực sự xác định câu trả lời một cách duy nhất. 

Quan sát quan trọng là không`::`các khối đã được cố định. Khi chúng tôi chia chuỗi tại`::`, số khối bị thiếu hoàn toàn được xác định bởi số lượng khối đã có. Vì địa chỉ cuối cùng phải chứa chính xác 8 khối nên số khối 0 được chèn vào chỉ đơn giản là`8 - (left_blocks + right_blocks)`. 

Điều này loại bỏ bất kỳ sự mơ hồ. Chúng tôi không còn tìm kiếm nữa, chúng tôi chỉ tính xem cần bao nhiêu khối 0 và chèn chúng vào đúng vị trí. 

Sau khi xây dựng lại danh sách đầy đủ tám khối, mỗi khối được mở rộng độc lập thành bốn chữ số thập lục phân bằng cách đệm các số 0 đứng đầu. Không có sự tương tác giữa các khối ở giai đoạn này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đoán vị trí bằng 0 | O(số mũ trên mỗi`::`) | O(1) | Quá chậm/không cần thiết | 
| Phân tích cú pháp và tái cấu trúc trực tiếp | O(1) mỗi địa chỉ | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng địa chỉ một cách độc lập. 

1. Kiểm tra xem chuỗi có chứa`::`. Nếu không, địa chỉ đó đã có tất cả tám khối được liệt kê rõ ràng. Chúng ta chỉ cần chia cho`:`và đệm mỗi khối thành bốn ký tự. 
2. Nếu`::`tồn tại, chia chuỗi thành hai phần: mọi thứ trước`::`và mọi thứ sau đó. Mỗi phần có thể trống. 
3. Chia cả hai phần cho`:`vào danh sách các khối. Các phần trống tạo ra danh sách trống, điều này quan trọng đối với các trường hợp như đầu hoặc cuối`::`. 
4. Đếm xem có bao nhiêu khối đã có trong cả hai danh sách cộng lại. Hãy để điều này được`k`. 
5. Tính xem phải chèn bao nhiêu khối 0 vào`z = 8 - k`. Giá trị này luôn không âm vì đầu vào được đảm bảo hợp lệ. 
6. Xây dựng danh sách khối đầy đủ dưới dạng: khối bên trái, sau đó`z`không có khối nào, sau đó là khối bên phải. Các khối 0 được biểu diễn dưới dạng`"0"`hoặc chuỗi trống nhưng sẽ được chuẩn hóa ở bước tiếp theo. 
7. Chuyển đổi mọi khối thành chuỗi thập lục phân gồm 4 ký tự bằng cách sử dụng phần đệm bằng 0. Việc này được thực hiện thống nhất cho cả khối gốc và khối được chèn vào. 
8. Nối tất cả tám khối bằng dấu hai chấm để tạo thành địa chỉ IPv6 mở rộng cuối cùng. 

Tính chính xác xuất phát từ thực tế là kích thước cấu trúc đầy đủ được cố định. Khi chúng tôi biết có bao nhiêu khối hiện diện rõ ràng, phần còn lại phải là 0 khối được giới thiệu bởi`::`, và không có sự tự do trong việc sắp xếp chúng ngoài khoảng cách duy nhất. 

Điều bất biến được duy trì là sau bước 6, danh sách khối được xây dựng luôn có chính xác tám vị trí logic được lấp đầy bởi các giá trị ban đầu hoặc các khối 0 được suy ra. Phần đệm ở bước 7 chỉ thay đổi định dạng chứ không thay đổi cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def expand_ipv6(s: str) -> str:
    if "::" in s:
        left, right = s.split("::")

        left_blocks = left.split(":") if left else []
        right_blocks = right.split(":") if right else []

        # remove empty strings caused by edge cases like ":" splitting artifacts
        left_blocks = [b for b in left_blocks if b]
        right_blocks = [b for b in right_blocks if b]

        k = len(left_blocks) + len(right_blocks)
        z = 8 - k

        blocks = left_blocks + (["0"] * z) + right_blocks
    else:
        blocks = s.split(":")

    # pad each block to 4 hex digits
    res = []
    for b in blocks:
        res.append(b.zfill(4))

    return ":".join(res)

def main():
    n = int(input())
    out = []
    for _ in range(n):
        s = input().strip()
        out.append(expand_ipv6(s))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai phản ánh trực tiếp logic tái thiết. Phần tế nhị nhất là xử lý`::`chính xác khi nó xuất hiện ở đầu hoặc cuối chuỗi, vì việc phân tách tạo ra các chuỗi trống không được coi là khối thực. Việc sử dụng tính năng lọc đảm bảo chúng tôi chỉ đếm mã thông báo thập lục phân thực tế khi xác định số lượng khối 0 bị thiếu. 

Bước đệm với`zfill(4)`là đủ vì mọi khối đều được đảm bảo là thập lục phân hợp lệ và nhiều nhất là bốn ký tự dài sau khi nén. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`a56f::0124:0001:0000:1234:0ff0`Chúng tôi chia nhau ra`::`, cho trái =`a56f`, phải =`0124:0001:0000:1234:0ff0`. 

| Bước | Khối bên trái | Khối bên phải | k | z | Khối cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| Phân tích | [a56f] | [0124, 0001, 0000, 1234, 0ff0] | 6 | 2 | [a56f, 0, 0, 0124, 0001, 0000, 1234, 0ff0] | 

Sau khi đệm,`a56f`trở thành`a56f`, số không trở thành`0000`, và phần còn lại vẫn được chuẩn hóa. Kết quả phù hợp với yêu cầu của tám khối. 

Điều này xác nhận rằng kích thước khoảng cách được tính toán sẽ khôi phục chính xác cấu trúc bị thiếu. 

### Ví dụ 2:`::`Ở đây toàn bộ địa chỉ được nén. 

| Bước | Khối bên trái | Khối bên phải | k | z | Khối cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| Phân tích | [] | [] | 0 | 8 | [0,0,0,0,0,0,0,0] | 

Sau khi đệm, mọi khối sẽ trở thành`0000`, tạo ra địa chỉ IPv6 hoàn toàn bằng không. 

Điều này chứng tỏ rằng các phần tách trống sẽ chuyển chính xác thành các danh sách có độ dài bằng 0 và thuật toán sẽ lấp đầy toàn bộ cấu trúc một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi địa chỉ được phân tích cú pháp và đệm theo thời gian không đổi vì nó luôn có tối đa 8 khối | 
| Không gian | O(1) | Chỉ một số khối cố định được lưu trữ trên mỗi địa chỉ | 

Kích thước đầu vào đủ nhỏ để ngay cả nhiều thao tác chuỗi cho mỗi trường hợp thử nghiệm cũng không đáng kể và giải pháp dễ dàng phù hợp với cả hai giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout

    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided samples
assert run("""6
a56f:d3:0:0124:01:f19a:1000:00
a56f:00d3:0000:0124:0001::
a56f::0124:0001:0000:1234:0ff0
a56f:0000::0000:0001:0000:1234:0ff0
::
0ea::4d:f4:6:0
""") == """a56f:00d3:0000:0124:0001:f19a:1000:0000
a56f:00d3:0000:0124:0001:0000:0000:0000
a56f:0000:0000:0124:0001:0000:1234:0ff0
a56f:0000:0000:0000:0001:0000:1234:0ff0
0000:0000:0000:0000:0000:0000:0000:0000
00ea:0000:0000:0000:004d:00f4:0006:0000"""

# all zeros edge
assert run("1\n::\n") == "0000:0000:0000:0000:0000:0000:0000:0000"

# no compression
assert run("1\na:b:c:d:e:f:1:2\n") == "000a:000b:000c:000d:000e:000f:0001:0002"

# leading ::
assert run("1\n::1:2:3:4:5:6:7\n") == "0000:0001:0002:0003:0004:0005:0006:0007"

# trailing ::
assert run("1\n1:2:3:4:5:6:7::\n") == "0001:0002:0003:0004:0005:0006:0007:0000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`::`| tất cả số không | nén đầy đủ | 
| KHÔNG`::`| khối đệm | định dạng cơ bản | 
| dẫn đầu`::`| chia trái trống | xử lý khoảng cách tiền tố | 
| theo sau`::`| chia phải-trống | xử lý khoảng cách hậu tố | 

## Vỏ cạnh 

Đối với đầu vào`::`, thuật toán chia thành các danh sách trống bên trái và bên phải. Số khối bị thiếu được tính toán trở thành 8, do đó 8 khối 0 được chèn vào. Sau khi đệm, tất cả trở thành`0000`, tạo ra một địa chỉ IPv6 đầy đủ hợp lệ. 

Vì`::1:2:3:4:5:6:7`, phía bên trái trống và phía bên phải chứa bảy khối. Thuật toán chèn một khối 0 vào đầu, đảm bảo tổng số đạt tới 8 khối. Phần đệm sau đó tạo ra biểu diễn đầy đủ chính xác. 

Vì`1:2:3:4:5:6:7::`, điều ngược lại xảy ra: phía bên phải trống và một khối 0 được thêm vào cuối. Điều này cho thấy việc bố trí`::`xác định một cách tự nhiên liệu các số 0 được chèn vào đầu hay cuối mà không có cách viết hoa đặc biệt.
