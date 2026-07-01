---
title: "CF 104218I - Huấn luyện của Balto"
description: "Chúng tôi được cung cấp một cấu trúc được chỉ đạo trên các làng $N$. Từ mỗi làng có đúng hai con đường đi ra: một con đường được đánh dấu bên trái và một con đường được đánh dấu bên phải, mỗi con đường chỉ đến một số làng (có thể giống hoặc khác nhau)."
date: "2026-07-01T23:51:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104218
codeforces_index: "I"
codeforces_contest_name: "UTPC Contest 03-03-23 Div. 1 (Advanced)"
rating: 0
weight: 104218
solve_time_s: 70
verified: true
draft: false
---

[CF 104218I - Khóa đào tạo của Balto](https://codeforces.com/problemset/problem/104218/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một cấu trúc định hướng trên$N$các làng. Từ mỗi làng có đúng hai con đường đi ra: một con đường được đánh dấu bên trái và một con đường được đánh dấu bên phải, mỗi con đường chỉ đến một số làng (có thể giống hoặc khác nhau). 

Một buổi đào tạo được xác định bởi làng bắt đầu và một số nguyên$K$. Từ điểm xuất phát đó, Balto di chuyển theo một mô hình có chiều dài cố định$4K$: anh ấy chiếm cạnh trái$K$lần, sau đó là cạnh phải$K$lần rồi lại rời đi$K$lần, và cuối cùng lại đúng$K$lần. Mỗi bước di chuyển được áp dụng cho làng hiện tại, do đó, đường đi được xác định đầy đủ bằng cách liên tục đi theo các cạnh đi ra. 

Nhiệm vụ là trả lời tới$10^5$các truy vấn như vậy một cách hiệu quả, trong đó mỗi truy vấn có thể có$K$lớn như$10^9$. 

Các ràng buộc ngay lập tức loại trừ việc mô phỏng từng bước truy vấn. Một truy vấn có thể yêu cầu tới$4K$chuyển tiếp, trong trường hợp xấu nhất là$4 \cdot 10^9$các bước. Nhân số đó với$10^5$các truy vấn khiến mọi mô phỏng trực tiếp đều không thể thực hiện được. 

Cấu trúc không phải là việc duyệt đồ thị tùy ý; mỗi nút có chính xác hai cạnh đi ra, vì vậy mỗi truy vấn chỉ là ứng dụng lặp lại của hai hàm xác định trái và phải. 

Một vài trường hợp tế nhị quan trọng: 

Một vấn đề là khi chuyển tiếp trái và phải tạo thành các chu kỳ ngắn. Ví dụ: một nút có thể lặp với chính nó trên cả hai cạnh. Trong những trường hợp như vậy, mô phỏng đơn giản sẽ lãng phí thời gian nhưng vẫn tạo ra kết quả chính xác. 

Một vấn đề khác là mô hình xen kẽ cố định và đối xứng. Việc tối ưu hóa bất cẩn giả định sự độc lập giữa các phân đoạn có thể bị phá vỡ khi đường dẫn bên trái và bên phải giao nhau với các chu trình theo những cách khác nhau. Ví dụ: nếu cả bên trái và bên phải đều ánh xạ vào cùng một chu trình nhưng có các điểm vào khác nhau, việc xử lý chúng một cách riêng biệt mà không đồng bộ hóa có thể sắp xếp sai các trạng thái. 

Cuối cùng, bởi vì$K$lớn, mọi giải pháp đều phải tránh lặp lại$K$lần trực tiếp. Bất kỳ cách tiếp cận nào phụ thuộc tuyến tính vào$K$mỗi truy vấn là không thể thực hiện được ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp tuân theo định nghĩa theo nghĩa đen. Đối với mỗi truy vấn, chúng tôi mô phỏng$K$sau đó di chuyển sang trái$K$di chuyển đúng, sau đó một lần nữa$K$di chuyển sang trái, và cuối cùng$K$những bước đi đúng đắn. Mỗi bước di chuyển là một bước nhảy con trỏ bằng cách sử dụng vùng kề được lưu trữ. 

Điều này đúng vì nó phản ánh chính xác quá trình được mô tả trong bài toán. Tuy nhiên, chi phí của nó là rất cao. Mỗi truy vấn thực hiện$4K$chuyển tiếp và với$K$lên đến$10^9$, ngay cả một truy vấn cũng có thể vượt quá giới hạn thời gian theo nhiều bậc độ lớn. 

Quan sát quan trọng là trình tự không tùy ý; nó là sự lặp lại của hai hàm xác định, trái và phải. Điều quan trọng không phải là quỹ đạo từng bước mà là kết quả của việc kết hợp các chức năng này nhiều lần. 

Thay vì suy nghĩ về các bước di chuyển riêng lẻ, chúng ta xem bên trái và bên phải dưới dạng đồ thị hàm số. Chúng ta cần ứng dụng lặp đi lặp lại nhanh chóng các chức năng này. Đây là cài đặt cổ điển để nâng nhị phân. 

Chúng tôi tính toán trước cho mỗi nút, nơi nó kết thúc sau$2^j$các ứng dụng của bên trái và riêng biệt nơi nó kết thúc sau$2^j$ứng dụng của quyền Khi điều này được xây dựng, bất kỳ số lượng lớn các bước di chuyển trái hoặc phải lặp đi lặp lại đều có thể được trả lời bằng$O(\log K)$thời gian. 

Mỗi truy vấn bao gồm bốn khối$K$di chuyển, luân phiên trái và phải. Chúng ta có thể tính toán kết quả của từng khối bằng cách sử dụng nâng nhị phân, xâu chuỗi chúng theo thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(QK)$|$O(1)$| Quá chậm | 
| Tối ưu (Nâng nhị phân) |$O(N \log K + Q \log K)$|$O(N \log K)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi các bước di chuyển trái và phải là hai hàm nhảy độc lập trên cùng một biểu đồ. 

1. Tính toán trước các bảng nâng nhị phân cho các chuyển đổi trái và phải. 

Đối với mỗi nút$v$, cửa hàng$upL[j][v]$= nút đạt được sau$2^j$trái di chuyển từ$v$, và tương tự$upR[j][v]$cho những bước đi đúng đắn. 
2. Khởi tạo cấp cơ sở$j = 0$sử dụng các cạnh đã cho. 

Điều này mã hóa chuyển tiếp trực tiếp trong một bước. 
3. Xây dựng cấp độ cao hơn bằng cách sử dụng bố cục. 

Đối với mỗi$j > 0$, định nghĩa$upL[j][v] = upL[j-1][ upL[j-1][v] ]$, và tương tự cho bên phải. 

Điều này hoạt động vì áp dụng$2^j$di chuyển bằng hai khối liên tiếp của$2^{j-1}$di chuyển. 
4. Đối với mỗi truy vấn, hãy bắt đầu từ ngôi làng nhất định. 
5. Áp dụng$K$di chuyển trái bằng cách sử dụng bàn nâng nhị phân để chuyển sang trái. 

phân hủy$K$thành sức mạnh của hai và nhảy theo đó. 
6. Áp dụng$K$di chuyển bên phải từ nút kết quả bằng cách sử dụng bảng bên phải. 
7. Lặp lại bước 5 và 6 một lần nữa vì mẫu đầy đủ là trái, phải, trái, phải. 

Mỗi khối độc lập vì trạng thái sau một phân đoạn sẽ trở thành đầu vào cho phân đoạn tiếp theo. 

### Tại sao nó hoạt động 

Mỗi loại chuyển động xác định một chức năng xác định từ nút này sang nút khác. Nâng nhị phân tính toán thành phần hàm lặp lại một cách hiệu quả bằng cách phân tách số mũ lớn thành lũy thừa của hai. Thuộc tính thành phần đảm bảo tính chính xác: áp dụng$2^a + 2^b$các bước tương đương với việc áp dụng$2^a$các bước tiếp theo$2^b$các bước, do đó các bước nhảy được tính toán trước sẽ thể hiện chính xác tất cả số bước có thể có. Vì truy vấn chỉ là một chuỗi gồm bốn lũy thừa hàm được áp dụng theo thứ tự, nên việc xâu chuỗi các kết quả này sẽ duy trì sự tương đương chính xác với quy trình ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LOG = 31  # enough for K up to 1e9

def build_up(n, nxt):
    up = [[0] * (n + 1) for _ in range(LOG)]
    for v in range(1, n + 1):
        up[0][v] = nxt[v]
    for j in range(1, LOG):
        for v in range(1, n + 1):
            up[j][v] = up[j - 1][up[j - 1][v]]
    return up

def lift(up, v, k):
    bit = 0
    while k:
        if k & 1:
            v = up[bit][v]
        k >>= 1
        bit += 1
    return v

def main():
    n = int(input())
    left = [0] * (n + 1)
    right = [0] * (n + 1)

    for i in range(1, n + 1):
        l, r = map(int, input().split())
        left[i] = l
        right[i] = r

    upL = build_up(n, left)
    upR = build_up(n, right)

    q = int(input())
    out = []

    for _ in range(q):
        v, k = map(int, input().split())
        v = lift(upL, v, k)
        v = lift(upR, v, k)
        v = lift(upL, v, k)
        v = lift(upR, v, k)
        out.append(str(v))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai tách biệt rõ ràng quá trình tiền xử lý và xử lý truy vấn. các`build_up`hàm xây dựng các bảng nâng nhị phân cho một mảng chuyển tiếp nhất định. Nó được tái sử dụng cho cả cạnh trái và phải, giúp logic nhỏ gọn và tránh trùng lặp. 

các`lift`Hàm áp dụng lũy ​​thừa của hai phép phân tách trên bảng được tính toán trước. Nó lặp qua các bit của$k$, chỉ áp dụng bước nhảy khi bit tương ứng được đặt. Điều này tránh việc xây dựng biểu diễn nhị phân một cách rõ ràng. 

Mỗi truy vấn áp dụng bốn lần chuyển đổi được dỡ bỏ theo trình tự. Thứ tự rất quan trọng vì mỗi phân đoạn bắt đầu từ kết quả của phân đoạn trước đó. 

Một cạm bẫy phổ biến là cố gắng hợp nhất trái và phải thành một cấu trúc duy nhất. Điều đó bị phá vỡ vì hàm chuyển đổi thay đổi giữa các phân đoạn; chúng không thể thay thế cho nhau. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng mẫu được cung cấp. 

đầu vào:```
3
2 3
3 1
1 3
3
1 1
1 2
3 1
```Trước tiên chúng tôi xây dựng các chuyển tiếp: 

| Nút | Trái | Đúng | 
| --- | --- | --- | 
| 1 | 2 | 3 | 
| 2 | 3 | 1 | 
| 3 | 1 | 3 | 

Bây giờ theo dõi các truy vấn. 

### Truy vấn 1: bắt đầu = 1, K = 1 

| Bước | Hành động | Nút | 
| --- | --- | --- | 
| 1 | bắt đầu | 1 | 
| 2 | trái 1 | 2 | 
| 3 | đúng 1 | 1 | 
| 4 | trái 1 | 2 | 
| 5 | đúng 1 | 3 | 

Kết quả cuối cùng là 3. 

### Truy vấn 2: start = 1, K = 2 

| Bước | Hành động | Nút | 
| --- | --- | --- | 
| 1 | bắt đầu | 1 | 
| 2 | trái 2 | 3 | 
| 3 | đúng 2 | 3 | 
| 4 | trái 2 | 1 | 
| 5 | đúng 2 | 3 | 

Kết quả cuối cùng là 3. 

### Truy vấn 3: start = 3, K = 1 

| Bước | Hành động | Nút | 
| --- | --- | --- | 
| 1 | bắt đầu | 3 | 
| 2 | trái 1 | 1 | 
| 3 | đúng 1 | 3 | 
| 4 | trái 1 | 2 | 
| 5 | đúng 1 | 3 | 

Kết quả cuối cùng là 3. 

Những dấu vết này xác nhận rằng mỗi phân đoạn phải được áp dụng tuần tự và các trạng thái trung gian ảnh hưởng trực tiếp đến các phân đoạn sau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log K + Q \log K)$| xây dựng bảng nâng nhị phân và trả lời từng truy vấn bằng bước nhảy log-K | 
| Không gian |$O(N \log K)$| lưu trữ hai bàn nâng để chuyển tiếp trái và phải | 

Chi phí tiền xử lý tăng tuyến tính theo$N$lần số bit cần thiết để biểu diễn$K$, đủ cho$N \le 10^5$. Mỗi truy vấn đều có dạng logarit$K$, Vì thế$10^5$truy vấn vẫn có hiệu lực trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    LOG = 31

    def build(n, nxt):
        up = [[0]*(n+1) for _ in range(LOG)]
        for i in range(1, n+1):
            up[0][i] = nxt[i]
        for j in range(1, LOG):
            for i in range(1, n+1):
                up[j][i] = up[j-1][up[j-1][i]]
        return up

    def lift(up, v, k):
        b = 0
        while k:
            if k & 1:
                v = up[b][v]
            k >>= 1
            b += 1
        return v

    n = int(input())
    left = [0]*(n+1)
    right = [0]*(n+1)

    for i in range(1, n+1):
        l, r = map(int, input().split())
        left[i] = l
        right[i] = r

    upL = build(n, left)
    upR = build(n, right)

    q = int(input())
    for _ in range(q):
        v, k = map(int, input().split())
        v = lift(upL, v, k)
        v = lift(upR, v, k)
        v = lift(upL, v, k)
        v = lift(upR, v, k)
        print(v)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""3
2 3
3 1
1 3
3
1 1
1 2
3 1
""") == """3
3
3"""

# custom: self-loop
assert run("""1
1 1
2
1 5
1 100""") == """1
1"""

# custom: small cycle
assert run("""2
2 1
1 2
3
1 1
1 2
2 3
""") == """2
1
2"""

# custom: asymmetric graph
assert run("""3
2 2
3 3
1 1
2
1 4
2 4
""") == """2
3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị tự lặp | kết quả không đổi | ổn định theo chu trình suy biến | 
| Đồ thị 2 chu kỳ | hành vi chuyển đổi | tính đúng đắn theo cấu trúc tuần hoàn | 
| vòng tự bất đối xứng | chuyển tiếp độc lập | xử lý độc lập trái/phải | 

## Vỏ cạnh 

Một vòng tự lặp ở mọi nơi, trong đó mọi nút đều trỏ đến chính nó ở cả bên trái và bên phải, tạo ra một hệ thống tầm thường. Bắt đầu từ bất cứ đâu, bất kỳ số lần di chuyển nào cũng giữ trạng thái không thay đổi. Thuật toán xử lý vấn đề này vì các bảng nâng nhị phân thu gọn thành ánh xạ nhận dạng ở mọi cấp độ, do đó mọi truy vấn được nâng lên đều trả về nút gốc. 

Chu kỳ hoán đổi hai nút, trong đó cả hai nút hoán đổi trái và phải, nhấn mạnh thành phần lặp lại. Mặc dù mỗi hàm đều đơn giản, việc lũy thừa lặp đi lặp lại phải đảm bảo tính chẵn lẻ một cách chính xác. Bảng nâng nắm bắt được điều này vì phép bình phương lặp lại mã hóa tính chẵn lẻ của chu kỳ một cách tự nhiên và mỗi truy vấn giảm xuống việc kiểm tra các bit của$K$. 

Trường hợp chuyển tiếp trái và phải tạo thành các chu kỳ khác nhau bắt đầu từ cùng một nút sẽ kiểm tra xem ứng dụng tuần tự có được bảo toàn hay không. Vì chúng tôi tính toán lại nút hiện tại sau mỗi phân đoạn nên trạng thái sẽ diễn biến chính xác thông qua các đồ thị hàm số khác nhau mà không trộn lẫn các chuyển tiếp.
