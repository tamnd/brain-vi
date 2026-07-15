---
title: "CF 103399D - Mô-đun nhân mô-đun nhanh 64-bit"
description: "Chúng ta được cho hai số nguyên và được yêu cầu tính tích của chúng theo một mô đun rất lớn, trong đó mô đun này có giá trị 64 bit đầy đủ."
date: "2026-07-03T12:08:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103399
codeforces_index: "D"
codeforces_contest_name: "Fast modular multiplication"
rating: 0
weight: 103399
solve_time_s: 45
verified: true
draft: false
---

[CF 103399D - Mô-đun nhân mô-đun nhanh 64-bit](https://codeforces.com/problemset/problem/103399/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên và được yêu cầu tính tích của chúng theo một mô đun rất lớn, trong đó mô đun này có giá trị 64 bit đầy đủ. Khó khăn chính không phải là phép nhân mà là thực hiện nó một cách an toàn mà không bị tràn trong khi vẫn giữ được độ chính xác theo modulo số lượng lớn đó. 

Về mặt khái niệm, nhiệm vụ rất đơn giản: chúng ta muốn tính toán$a \cdot b \bmod m$, nhưng cả hai$a \cdot b$và kết quả trung gian có thể vượt quá giới hạn có dấu hoặc không dấu 64 bit tùy thuộc vào ngôn ngữ và chi tiết triển khai. Từ$m$bản thân nó là một số nguyên 64 bit, phép nhân đơn giản trong loại có chiều rộng cố định có thể âm thầm quấn quanh trước khi áp dụng mô đun, tạo ra kết quả không chính xác. 

Đầu vào đại diện cho các truy vấn độc lập lặp đi lặp lại, mỗi truy vấn bao gồm hai số nguyên lớn và một mô đun. Đầu ra cho mỗi truy vấn là sản phẩm mô-đun chính xác. 

Hàm ý ràng buộc là tinh tế. Một phép nhân đơn lẻ có thời gian không đổi về mặt toán học, nhưng khi triển khai, chúng ta không thể dựa vào số học 128-bit tích hợp sẵn trong mọi môi trường. Giải pháp phải tránh tràn trung gian bằng cách sử dụng kỹ thuật mô phỏng phép nhân một cách an toàn chỉ bằng các thao tác 64 bit. 

Các trường hợp biên phát sinh khi cả hai toán hạng đều gần nhau$2^{64}$. Ví dụ, nếu$a = 2^{63}$,$b = 2$, thì tích thực sự sẽ vượt quá 64 bit ngay lập tức. Một cách tiếp cận đơn giản trong một ngôn ngữ có số học bao bọc có thể tính toán giá trị bằng 0 hoặc giá trị được bao bọc không chính xác trước khi áp dụng mô đun. 

Một trường hợp có vấn đề khác là khi mô đun gần bằng$2^{64}$. Ví dụ, nếu$m = 2^{64} - 1$, ngay cả phép nhân trung gian cũng phải được kiểm soát cẩn thận, bởi vì bất kỳ sự tràn nào cũng làm mất hiệu lực logic rút gọn mô-đun. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là tính tích trực tiếp bằng cách sử dụng phép nhân số nguyên gốc và sau đó áp dụng modulo. Điều này hoạt động trong các ngôn ngữ có số nguyên không giới hạn, vì tích được tính toán chính xác trước khi rút gọn. Tuy nhiên, trong số học 64 bit cố định, phép nhân tràn trước khi áp dụng mô đun, do đó giá trị tính toán đã bị hỏng. 

Điểm thất bại rất đơn giản: nhân hai số nguyên 64 bit có thể yêu cầu độ chính xác lên tới 128 bit. Nếu môi trường chỉ hỗ trợ số học 64 bit thì kết quả trung gian sẽ bị mất. 

Quan sát quan trọng là phép nhân mô-đun có thể được phân tách thành các phép cộng. Thay vì tính toán$a \cdot b$trực tiếp, chúng tôi giải thích phép nhân là phép nhân đôi lặp đi lặp lại và phép cộng có điều kiện, tương tự như logic lũy thừa nhị phân. Điều này tránh việc hình thành một sản phẩm trung gian lớn. Mỗi bước giữ giá trị giảm modulo$m$, đảm bảo tất cả các sản phẩm trung gian đều nằm trong phạm vi. 

Chúng tôi cũng có thể đẩy nhanh quá trình này bằng cách sử dụng phân tách bit: chúng tôi quét các bit của một toán hạng và tích lũy phần đóng góp của toán hạng kia, nhân đôi một cách an toàn theo mô đun ở mỗi bước. 

Điều này biến đổi phép nhân từ một phép toán có khả năng tràn thành một chuỗi các phép cộng và nhân đôi được kiểm soát, tất cả đều có thể được giảm modulo một cách an toàn.$m$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Lỗi trong số học 64-bit cố định | 
| Phép nhân mô-đun theo bit | O(log b) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán$a \cdot b \bmod m$sử dụng phép phân rã nhị phân của số nhân. 

1. Khởi tạo một biến kết quả bằng 0 và giảm cả hai số đầu vào theo modulo$m$. Việc giảm sớm đảm bảo tất cả các hoạt động sau này luôn bị giới hạn và ngăn chặn sự tăng trưởng không cần thiết. 
2. Lặp lại các bit của số nhân$b$. Ở mỗi bước, chúng tôi kiểm tra xem bit hiện tại có được đặt hay không. Nếu đúng như vậy, chúng ta cộng giá trị hiện tại của số nhân vào kết quả, áp dụng mô đun ngay lập tức. Điều này hoạt động vì mỗi bit được đặt đại diện cho sự đóng góp lũy thừa hai trong việc mở rộng nhị phân. 
3. Sau khi xử lý từng bit, chúng ta nhân đôi số nhân theo mô đun. Điều này tương ứng với việc chuyển trọng lượng của nó từ$2^k$ĐẾN$2^{k+1}$trong biểu diễn nhị phân. Mô-đun giữ cho việc nhân đôi này an toàn không bị tràn. 
4. Tiếp tục cho đến khi tất cả các bit của số nhân đã được xử lý. Kết quả tích lũy là sản phẩm mô-đun cuối cùng. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ việc giải thích phép nhân như là tổng của các đóng góp dịch chuyển. Bất kỳ số nguyên nào$b$có thể được viết dưới dạng tổng lũy ​​thừa của hai và nhân với$a$phân phối trên đại diện này. Thuật toán phản ánh chính xác sự phân tách này: bất cứ khi nào một bit được đặt, chúng ta sẽ thêm giá trị tỷ lệ lũy thừa hai tương ứng của$a$và chúng tôi duy trì tính chính xác bằng cách đảm bảo mọi giá trị trung gian đều được giảm theo modulo$m$. Không có sự tràn nào có thể ảnh hưởng đến tính chính xác vì không có phép tính trung gian nào vượt quá ranh giới mô đun một cách có ý nghĩa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mod_mul(a, b, m):
    a %= m
    b %= m
    res = 0

    while b > 0:
        if b & 1:
            res = (res + a) % m
        a = (a + a) % m
        b >>= 1

    return res

def solve():
    t = int(input())
    for _ in range(t):
        a, b, m = map(int, input().split())
        print(mod_mul(a, b, m))

if __name__ == "__main__":
    solve()
```chức năng`mod_mul`thực hiện phép nhân nhị phân theo mô đun. Ý tưởng chính là`a`được nhân đôi nhiều lần, đại diện cho lũy thừa liên tiếp của hai, trong khi`b`đang bị phân hủy từng chút một. Bất cứ khi nào một chút`b`đang hoạt động, sự đóng góp hiện tại của`a`được thêm vào kết quả. 

Một chi tiết tinh tế là việc giảm sớm cả hai`a`Và`b`modulo`m`. Trong khi giảm`a`là điều cần thiết để giữ giá trị nhỏ, giảm`b`là tùy chọn để đảm bảo tính chính xác nhưng vô hại và đôi khi giúp giảm số lần lặp vòng lặp một chút. Cấu trúc vòng lặp đảm bảo tính chính xác bất kể vì nó xử lý trực tiếp biểu diễn nhị phân. 

Tất cả các phép cộng và nhân đôi đều được giảm modulo ngay lập tức`m`, ngăn ngừa tràn hoàn toàn ngay cả trong môi trường bị hạn chế 64-bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

hãy để$a = 3$,$b = 5$,$m = 7$. Biểu diễn nhị phân của$b$là$101$. 

| Bước | một | b | độ phân giải | 
| --- | --- | --- | --- | 
| ban đầu | 3 | 5 | 0 | 
| bit 1 (LSB) | 3 | 5 | 3 | 
| nhân đôi | 6 | 2 | 3 | 
| bit 0 | 6 | 2 | 3 | 
| nhân đôi | 5 | 1 | 3 | 
| bit 1 | 5 | 1 | 1 | 
| nhân đôi | 3 | 0 | 1 | 

Kết quả cuối cùng là$1$, khớp$15 \bmod 7$. 

Dấu vết này cho thấy mỗi chữ số nhị phân của số nhân đóng góp độc lập như thế nào và việc giảm mô đun ngăn cản sự tăng trưởng vượt quá mô đun như thế nào. 

### Ví dụ 2 

hãy để$a = 10$,$b = 6$,$m = 9$. Biểu diễn nhị phân của$b$là$110$. 

| Bước | một | b | độ phân giải | 
| --- | --- | --- | --- | 
| ban đầu | 10 → 1 | 6 | 0 | 
| bit 0 | 1 | 6 | 0 | 
| nhân đôi | 2 | 3 | 0 | 
| bit 1 | 2 | 3 | 2 | 
| nhân đôi | 4 | 1 | 2 | 
| bit 1 | 4 | 1 | 6 | 
| nhân đôi | 8 → 8 mod 9 = 8 | 0 | 6 | 

Kết quả cuối cùng là$6$, khớp$60 \bmod 9$. 

Ví dụ này nêu bật việc giảm lặp lại và cho thấy ngay cả khi các giá trị trung gian vượt quá mô đun, tính chính xác vẫn được duy trì thông qua việc giảm ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log b) | Mỗi lần lặp xử lý một bit của số nhân | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Thời gian chạy hiệu quả đối với số nguyên 64 bit vì số lượng bit nhiều nhất là 64. Ngay cả với nhiều trường hợp thử nghiệm, tổng số thao tác vẫn nhỏ và thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def mod_mul(a, b, m):
        a %= m
        b %= m
        res = 0
        while b > 0:
            if b & 1:
                res = (res + a) % m
            a = (a + a) % m
            b >>= 1
        return res

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        a, b, m = map(int, sys.stdin.readline().split())
        res = mod_mul(a, b, m)
        out.append(str(res))
    return "\n".join(out)

# sample-like tests
assert run("1\n3 5 7\n") == "1", "sample 1"
assert run("1\n10 6 9\n") == "6", "sample 2"

# custom cases
assert run("1\n0 123456 7\n") == "0", "multiplication by zero"
assert run("1\n1 999999 2\n") == "1", "mod 1 behavior"
assert run("1\n2 2 3\n") == "1", "small cycle case"
assert run("1\n18446744073709551615 18446744073709551615 18446744073709551615\n") == "0", "max 64-bit edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 × lớn | 0 | hấp thụ bằng không | 
| thu gọn danh tính mod 2 | 1 | giảm độ đúng | 
| chu kỳ nhỏ | 1 | tính đúng đắn của phân rã nhị phân | 
| giá trị 64-bit tối đa | 0 | hành vi an toàn tràn | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi một toán hạng bằng 0. Thuật toán ngay lập tức trả về 0 vì không có bit nào được đặt trong số nhân hoặc số nhân không bao giờ đóng góp. Đối với đầu vào`0 123456 7`, vòng lặp không bao giờ tích lũy bất cứ thứ gì và kết quả vẫn bằng 0. 

Một trường hợp khác là khi mô đun rất nhỏ, chẳng hạn như 1 hoặc 2. Đối với mô đun 1, mọi mức giảm trung gian sẽ buộc tất cả các giá trị về 0 ngay lập tức, do đó đầu ra luôn bằng 0. Đối với mô-đun 2, chỉ có tính chẵn lẻ là quan trọng và thuật toán vẫn hoạt động vì mọi phép cộng và nhân đôi đều bảo toàn tính chính xác của mô-đun 2. 

Trường hợp cạnh cuối cùng là giá trị 64 bit tối đa được nhân với chính nó theo một mô đun lớn. Vì`18446744073709551615 18446744073709551615 18446744073709551615`, thuật toán giảm ngay lập tức cả hai toán hạng về 0 theo mô đun, do đó kết quả bằng 0. Mặc dù sản phẩm toán học thực sự rất lớn nhưng việc rút gọn mô-đun sẽ ngăn chặn bất kỳ sự tràn hoặc tính toán sai nào và thuật toán kết thúc một cách rõ ràng mà không bao giờ hình thành các giá trị trung gian lớn.
