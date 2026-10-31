---
title: "CF 106367A - Thế giới tràn ngập màu sắc"
description: "Chúng tôi được cung cấp một chuỗi các màu RGB được viết ở định dạng web thập lục phân tiêu chuẩn, trong đó mỗi màu bắt đầu bằng sáu ký tự thập lục phân viết thường."
date: "2026-06-19T08:25:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106367
codeforces_index: "A"
codeforces_contest_name: "Whalica Cup (Round 2)"
rating: 0
weight: 106367
solve_time_s: 44
verified: true
draft: false
---

[CF 106367A - Thế giới tràn ngập màu sắc](https://codeforces.com/problemset/problem/106367/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các màu RGB được viết ở định dạng web thập lục phân tiêu chuẩn, trong đó mỗi màu bắt đầu bằng`#`theo sau là sáu ký tự thập lục phân viết thường. Sáu ký tự này mã hóa ba số nguyên: đỏ, lục và lam, mỗi số nằm trong phạm vi từ 0 đến 255, được lưu dưới dạng hai chữ số hex trên mỗi kênh. 

Đối với mỗi màu đầu vào, chúng ta phải xây dựng màu bổ sung của nó. Hai màu bổ sung cho nhau khi mỗi thành phần RGB tương ứng có tổng bằng 255. Nói cách khác, nếu màu gốc có các thành phần`(R, G, B)`, màu đầu ra phải là`(255 − R, 255 − G, 255 − B)`, sau đó được chuyển đổi lại thành chuỗi thập lục phân được định dạng chính xác với các chữ cái viết thường và số 0 đứng đầu nếu cần. 

Kích thước đầu vào lớn đối với các trường hợp thử nghiệm, lên tới 10^4. Mỗi trường hợp thử nghiệm được xử lý độc lập và mỗi thao tác có thời gian không đổi cho mỗi màu. Điều này loại trừ mọi thứ bậc hai hoặc liên quan đến việc quét chuỗi lặp lại ngoài O(1) cho mỗi trường hợp thử nghiệm. Chuyển đổi trực tiếp trên mỗi ký tự là đủ. 

Một trường hợp thất bại tinh tế xuất phát từ việc định dạng. Mỗi thành phần RGB phải luôn được biểu diễn bằng chính xác hai chữ số thập lục phân. Nếu điều này bị bỏ qua, các giá trị như 0 sẽ trở thành`0`thay vì`00`, phá vỡ định dạng có độ dài cố định cần thiết. 

Ví dụ: nếu đầu vào là`#000000`, đầu ra đúng là`#ffffff`. Việc triển khai ngây thơ mà quên phần đệm có thể tạo ra`#fff`, không hợp lệ theo đặc điểm kỹ thuật. 

Một chi tiết cạnh khác là phân biệt chữ hoa chữ thường. Đầu ra phải ở dạng thập lục phân viết thường. Sử dụng chữ hoa như`#E0E1CC`sẽ bị từ chối mặc dù đúng về mặt số lượng. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là chuyển đổi từng chuỗi chữ số hex thành một giá trị màu nguyên bằng cách phân tích cú pháp từng ký tự theo cách thủ công, sau đó tính 255 trừ mỗi kênh và cuối cùng xây dựng lại chuỗi bằng cách liên tục chuyển đổi các số nguyên trở lại hex. Đây đã là O(1) cho mỗi trường hợp thử nghiệm, nhưng nếu được triển khai không hiệu quả, chẳng hạn như bằng cách nối chuỗi liên tục hoặc sử dụng chuyển đổi cơ sở thủ công chậm, nó có thể giảm chất lượng trong thực tế do các thao tác ký tự lặp đi lặp lại trên tối đa 10^4 đầu vào. 

Quan sát quan trọng là mỗi kênh độc lập và luôn bị giới hạn trong [0, 255], do đó, các hàm định dạng và phân tích cú pháp số nguyên tích hợp của Python là đủ và tối ưu. Chúng ta chỉ cần trích xuất các chuỗi con cố định, chuyển đổi chúng một lần, trừ đi 255 và định dạng lại bằng định dạng thập lục phân hai chữ số. Điều này tránh hoàn toàn việc xử lý từng chữ số một cách thủ công. 

Cấu trúc của đầu vào đảm bảo các chuỗi có kích thước không đổi, vì vậy giải pháp tối ưu chỉ đơn giản là chuyển đổi trực tiếp cho mỗi trường hợp thử nghiệm mà không có cấu trúc dữ liệu phụ trợ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân tích cú pháp brute thủ công + tái thiết | O(t) | O(1) | Đã chấp nhận | 
| Cắt trực tiếp + chuyển đổi int + định dạng | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc chuỗi màu`s`. Nó luôn có dạng`#RRGGBB`. Cấu trúc cố định này cho phép cắt trực tiếp mà không cần quét. 
2. Trích xuất thành phần màu đỏ bằng cách sử dụng`s[1:3]`, sử dụng màu xanh lá cây`s[3:5]`và màu xanh bằng cách sử dụng`s[5:7]`. Mỗi chuỗi con chính xác có hai ký tự hex, đảm bảo phân tích cú pháp hợp lệ. 
3. Chuyển đổi từng chuỗi con từ hệ thập lục phân sang số nguyên bằng cách sử dụng chuyển đổi cơ số 16. Điều này mang lại`(R, G, B)`ở dạng số. 
4. Tính phần bù cho mỗi kênh bằng cách trừ đi 255, tạo ra`(255 − R, 255 − G, 255 − B)`. Bước này độc lập với mỗi kênh, phản ánh định nghĩa về tính bổ sung của vấn đề. 
5. Chuyển đổi từng số nguyên thu được thành chuỗi thập lục phân viết thường gồm hai ký tự. Nếu giá trị nhỏ hơn 16 thì phải thêm số 0 đứng đầu. Định dạng có chiều rộng cố định này duy trì mã hóa RGB cần thiết. 
6. Ghép các kết quả theo thứ tự và thêm vào trước`#`để tạo thành chuỗi đầu ra cuối cùng. 

### Tại sao nó hoạt động 

Mỗi thành phần RGB được mã hóa độc lập trong phạm vi 8 bit cố định. Sự biến đổi`x → 255 − x`là một sự đảo ngược trên miền này, nghĩa là áp dụng nó hai lần sẽ trả về giá trị ban đầu. Vì biểu diễn thập lục phân là sự song ánh giữa`[0, 255]`Và`00`ĐẾN`ff`, chuyển đổi thành số nguyên, chuyển đổi và định dạng lại sẽ duy trì tính chính xác. Không tồn tại tương tác giữa các thành phần, do đó các kênh xử lý độc lập không thể tạo ra sự không nhất quán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        s = input().strip()

        r = int(s[1:3], 16)
        g = int(s[3:5], 16)
        b = int(s[5:7], 16)

        cr = 255 - r
        cg = 255 - g
        cb = 255 - b

        out.append("#" + format(cr, "02x") + format(cg, "02x") + format(cb, "02x"))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp dựa trên các vị trí cắt cố định, giúp tránh mọi chi phí vòng lặp trên mỗi ký tự. Mỗi`int(..., 16)`cuộc gọi xử lý chuyển đổi một cách an toàn ngay cả đối với số 0 đứng đầu. các`format(x, "02x")`đảm bảo cả đầu ra chữ thường và phần đệm 0 bắt buộc. 

Một lỗi triển khai phổ biến là sử dụng`hex(x)`trực tiếp, tạo ra các chuỗi như`0xa`thay vì`0a`. Điều đó sẽ phá vỡ định dạng trừ khi được xử lý hậu kỳ theo cách thủ công. Một vấn đề tế nhị khác là quên`.strip()`trên các dòng đầu vào, điều này có thể khiến các ký tự dòng mới cản trở việc cắt. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào. 

### Ví dụ 1:`#1f1e33`| Bước | R | G | B | Kết quả R | Kết quả G | Kết quả B | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Phân tích cú pháp hex | 31 | 30 | 51 | - | - | - | - | 
| Bổ sung | - | - | - | 224 | 225 | 204 | - | 
| Định dạng | - | - | - | e0 | e1 | cc |`#e0e1cc`| 

Điều này cho thấy sự đảo ngược kênh độc lập chính xác và định dạng hex thích hợp. 

### Ví dụ 2:`#0000ff`| Bước | R | G | B | Kết quả R | Kết quả G | Kết quả B | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Phân tích cú pháp hex | 0 | 0 | 255 | - | - | - | - | 
| Bổ sung | - | - | - | 255 | 255 | 0 | - | 
| Định dạng | - | - | - | ff | ff | 00 |`#ffff00`| 

Ví dụ này nhấn mạnh các giá trị biên, đảm bảo phần đệm 0 được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm xử lý các phép toán chuỗi có kích thước không đổi và số học | 
| Không gian | O(1) | Chỉ một số lượng biến cố định cho mỗi trường hợp thử nghiệm, đầu ra được lưu trữ tuyến tính | 

Các ràng buộc cho phép tối đa 10^4 trường hợp thử nghiệm và mỗi trường hợp được xử lý bằng cách cắt và chuyển đổi theo thời gian liên tục, giữ cho tổng thời gian chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()

    def solve():
        t = int(sys.stdin.readline())
        res = []
        for _ in range(t):
            s = sys.stdin.readline().strip()
            r = int(s[1:3], 16)
            g = int(s[3:5], 16)
            b = int(s[5:7], 16)
            res.append("#" + format(255-r, "02x") + format(255-g, "02x") + format(255-b, "02x"))
        print("\n".join(res))

    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("2\n#1f1e33\n#114514") == "#e0e1cc\n#eebaeb"

# minimum value edge
assert run("1\n#000000") == "#ffffff"

# maximum value edge
assert run("1\n#ffffff") == "#000000"

# mixed boundary
assert run("1\n#0f10f0") == "#f0ef0f"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| #000000 | #ffffff | đảo ngược ranh giới hoàn toàn bằng không | 
| #ffffff | #000000 | đảo ngược giá trị tối đa | 
| #0f10f0 | #f0ef0f | hỗn hợp độ chính xác kênh nhỏ/lớn | 

## Vỏ cạnh 

Màu hoàn toàn bằng không`#000000`thể hiện cách xử lý chính xác việc phân tích và định dạng hex số 0 đứng đầu. Mỗi kênh trở thành 255, phải được in dưới dạng`ff`, xác nhận rằng không thiếu logic đệm. 

Màu trắng toàn tập`#ffffff`kiểm tra thái cực ngược lại. Mỗi kênh trở thành 0, phải xuất hiện dưới dạng`00`. Một trình định dạng ngây thơ loại bỏ các số 0 đứng đầu sẽ xuất ra không chính xác`#0`, nhưng định dạng có chiều rộng cố định đảm bảo tính chính xác. 

Một trường hợp có giá trị hỗn hợp như`#0f10f0`đảm bảo rằng việc xử lý kênh độc lập không can thiệp vào các thành phần. Mỗi byte được đảo ngược riêng biệt, duy trì sự liên kết giữa các vị trí RGB và ngăn chặn các lỗi tính toán chia sẻ hoặc sắp xếp lại ngẫu nhiên.
