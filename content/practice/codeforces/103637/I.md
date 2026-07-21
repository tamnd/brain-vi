---
title: "CF 103637I - Vật phẩm trong hộp"
description: "Chúng ta được cho hai số nguyên. Một trong số chúng, $n$, xác định có bao nhiêu hộp tồn tại, cụ thể là tổng cộng có $2n$. Giá trị thứ hai, $a$, mô tả có bao nhiêu mục riêng biệt bên trong mỗi hộp và mỗi hộp độc lập với các mục khác. Từ mỗi hộp chúng ta phải chọn đúng một vật phẩm."
date: "2026-07-02T22:21:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103637
codeforces_index: "I"
codeforces_contest_name: "2019-2020 10th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 103637
solve_time_s: 43
verified: true
draft: false
---

[CF 103637I - Vật phẩm trong hộp](https://codeforces.com/problemset/problem/103637/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên. Một trong số họ,$n$, xác định có bao nhiêu hộp tồn tại, cụ thể$2n$tổng số hộp. Giá trị thứ hai,$a$, mô tả có bao nhiêu mục riêng biệt bên trong mỗi hộp và mỗi hộp độc lập với các mục khác. 

Từ mỗi hộp chúng ta phải chọn đúng một vật phẩm. Vì mỗi hộp đều chứa$a$các lựa chọn và tất cả các ô đều độc lập, tổng số cách để thực hiện một lựa chọn hoàn chỉnh là tích của các lựa chọn trên tất cả các ô. Số lượng thô này sau đó được giảm modulo$2n + 2$và phần còn lại đó là đầu ra cần thiết. 

Khó khăn chính là cả hai$n$Và$a$có thể lớn như$10^9$, do đó tổng số hộp cũng lớn và giá trị trực tiếp$a^{2n}$lớn về mặt thiên văn và không thể tính toán được một cách rõ ràng. 

Quan sát quan trọng nhất là chúng ta không được yêu cầu giá trị đầy đủ, chỉ yêu cầu phần dư của nó theo modulo một số gắn chặt với cấu trúc số mũ, cụ thể là$2n + 2$. Điều này về cơ bản làm cho vấn đề về lũy thừa mô-đun với một mô-đun phụ thuộc vào chính số mũ. 

Một trường hợp thất bại đơn giản sẽ xuất hiện ngay lập tức nếu người ta cố gắng tính toán công suất một cách trực tiếp. Ví dụ, nếu$n = 10^9$Và$a = 2$, giá trị là$2^{2 \cdot 10^9}$, không thể tính toán hoặc thậm chí lưu trữ. Ngay cả cách tiếp cận lũy thừa mô-đun mà không chú ý đến cấu trúc vẫn có độ phức tạp tốt, nhưng chúng ta phải cẩn thận với mô-đun vì nó không cố định. 

Một vấn đề tế nhị khác nảy sinh khi$a$lớn hơn mô đun. Việc thực hiện bất cẩn có thể cho rằng việc giảm sớm là vô hại, nhưng vì mô đun phụ thuộc vào$n$, số mũ và mô đun tương tác theo cách không tầm thường và chúng ta phải duy trì tính chính xác theo các quy tắc lũy thừa mô đun. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản: đối với mỗi ô, hãy nhân số lượng lựa chọn$a$, lặp lại$2n$lần. Điều này mang lại$a^{2n}$. Sau khi tính toán giá trị này, chúng tôi lấy nó theo modulo$2n + 2$. 

Điều này có hiệu quả về mặt khái niệm vì mỗi hộp đóng góp một hệ số nhân độc lập. Tuy nhiên, bản thân việc tính toán là không khả thi vì việc lũy thừa bằng phép nhân lặp lại đòi hỏi$2n$phép nhân, có thể lên tới$2 \cdot 10^9$, vượt xa mọi giới hạn thời gian hợp lý. 

Một cải tiến tự nhiên là chuyển sang lũy ​​thừa nhanh. Điều này làm giảm chi phí lũy thừa từ tuyến tính sang logarit trong$n$. Tuy nhiên, cái nhìn sâu sắc hơn là mô đun không phải là tùy ý. Chính xác là như vậy$2n + 2$, luôn luôn chẵn và liên quan chặt chẽ với số mũ$2n$. Cấu trúc này gợi ý rằng việc bình phương lặp lại kết hợp với rút gọn mô đun là đủ và không cần thêm thủ thuật lý thuyết số nào như định lý Euler, vì mô đun không nhất thiết phải là số nguyên tố và$a$không được đảm bảo là nguyên tố cùng nhau. 

Do đó, cách tiếp cận tối ưu chỉ đơn giản là lũy thừa nhị phân với mô đun$2n + 2$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$phép nhân |$O(1)$| Quá chậm | 
| lũy thừa nhị phân |$O(\log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$Và$a$. Số hộp là$2n$và mô đun được định nghĩa là$m = 2n + 2$. 
2. Giảm chân đế$a$modulo$m$. Điều này không làm thay đổi kết quả cuối cùng vì phép nhân mô-đun bảo toàn tính tương đương và giữ giá trị trung gian ở mức nhỏ. 
3. Đặt số mũ$e = 2n$. Chúng tôi đang tính toán$a^e \bmod m$. 
4. Khởi tạo kết quả dưới dạng$res = 1$. Điều này đại diện cho một tích rỗng, là phần tử trung lập cho phép nhân. 
5. Trong khi$e > 0$, xử lý số mũ từng chút một. Nếu bit thấp nhất của$e$là 1, nhân kết quả hiện tại với$a$modulo$m$. Điều này đảm bảo chúng ta bao gồm chính xác các lũy thừa tương ứng với phân tách nhị phân của số mũ. 
6. Bình phương căn cứ$a = a^2 \bmod m$. Điều này chuyển chúng ta sang lũy ​​thừa tiếp theo của hai. 
7. Dịch số mũ sang phải một bit, chia số mũ cho 2 một cách hiệu quả. 

Mỗi lần lặp lại làm giảm kích thước số mũ theo cấp số nhân, đó là lý do tại sao phương pháp này hiệu quả ngay cả đối với đầu vào rất lớn. 

### Tại sao nó hoạt động 

Tại mỗi bước, thuật toán duy trì tính bất biến$res \cdot a^e$(Ở đâu$a$là cơ sở hiện tại) đồng dạng với ban đầu$a^{2n}$modulo$m$. Biểu diễn nhị phân của số mũ đảm bảo rằng mọi lũy thừa của hai đóng góp chính xác một lần khi bit tương ứng của nó được đặt. Vì phép nhân có tính kết hợp theo số học mô-đun và mỗi phép rút gọn đều bảo toàn sự đồng đẳng nên kết quả cuối cùng khớp với giá trị mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mod_pow(a, e, mod):
    res = 1
    a %= mod
    while e > 0:
        if e & 1:
            res = (res * a) % mod
        a = (a * a) % mod
        e >>= 1
    return res

def solve():
    n, a = map(int, input().split())
    mod = 2 * n + 2
    exp = 2 * n
    print(mod_pow(a, exp, mod))

if __name__ == "__main__":
    solve()
```chức năng`mod_pow`thực hiện phép lũy thừa nhị phân. Cơ sở được giảm modulo`mod`ngay lập tức để tránh sự tăng trưởng tràn trong quá trình bình phương. Vòng lặp xử lý số mũ theo từng bit, nhân thành kết quả bất cứ khi nào bit hiện tại góp phần phân tách số mũ. 

Hàm chính tính toán mô đun$2n + 2$, đặt số mũ thành$2n$và ủy quyền tính toán cho quy trình lũy thừa mô-đun. 

Một lỗi phổ biến là quên giảm modulo cơ sở`mod`trước khi bắt đầu vòng lặp. Mặc dù không được yêu cầu nghiêm ngặt về tính chính xác nhưng nó ngăn cản các giá trị trung gian tăng lớn một cách không cần thiết. Một vấn đề tinh tế khác là trộn lẫn số mũ và mô đun, vì cả hai đều phụ thuộc vào$n$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 1, a = 2
```Chúng tôi tính toán$2n = 2$và mô đun$m = 4$. Vì vậy chúng tôi đánh giá$2^2 \bmod 4$. 

| Bước | độ phân giải | cơ sở a | số mũ e | 
| --- | --- | --- | --- | 
| ban đầu | 1 | 2 | 2 | 
| sử dụng bit | 2 | 2 | 1 | 
| vuông | 2 | 0 | 1 | 
| bit cuối cùng | 0 | 0 | 0 | 

Kết quả là$0$. 

Điều này xác nhận rằng ngay cả những trường hợp nhỏ cũng có thể sụp đổ khi thu nhỏ mô đun, vì mô đun không phải là số nguyên tố và tương tác mạnh với đế. 

### Ví dụ 2 

đầu vào:```
n = 2, a = 3
```Chúng tôi tính toán$2n = 4$, mô đun$m = 6$, và đánh giá$3^4 \bmod 6$. 

| Bước | độ phân giải | cơ sở a | số mũ e | 
| --- | --- | --- | --- | 
| ban đầu | 1 | 3 | 4 | 
| sử dụng bit | 3 | 3 | 2 | 
| vuông | 3 | 3 | 2 | 
| sử dụng bit | 3 | 3 | 1 | 
| vuông | 3 | 3 | 1 | 
| bit cuối cùng | 3 | 3 | 0 | 

Kết quả cuối cùng là$3$. 

Dấu vết này cho thấy cách lũy thừa nhị phân chỉ tích lũy các đóng góp khi các bit lũy thừa đang hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$| Mỗi bước giảm một nửa số mũ | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Các ràng buộc cho phép lên đến$10^9$, vì vậy mọi cách tiếp cận tuyến tính đều không thể thực hiện được, nhưng phép lũy thừa logarit dễ dàng đủ nhanh trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, a = map(int, input().split())
    mod = 2 * n + 2

    def mod_pow(a, e, mod):
        res = 1
        a %= mod
        while e > 0:
            if e & 1:
                res = (res * a) % mod
            a = (a * a) % mod
            e >>= 1
        return res

    return str(mod_pow(a, 2 * n, mod))

# provided samples (from statement formatting)
assert run("1 2\n") == "0"
assert run("2 3\n") == "3"

# custom cases
assert run("1 1\n") == "1", "all ones"
assert run("2 2\n") == "4", "small power check"
assert run("5 10\n") == str(pow(10, 10, 12)), "mod consistency"
assert run("10 1\n") == "1", "identity base"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | hành vi nhận dạng cơ sở 1 | 
| 2 2 | 4 | tính đúng đắn của phép lũy thừa | 
| 5 10 | giá trị tính toán | tính nhất quán mô-đun với xác minh nhỏ | 
| 10 1 | 1 | số mũ không liên quan khi cơ số là 1 | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$a = 1$. Trong trường hợp đó, mọi phép nhân đều là trung tính và kết quả phải luôn là 1 bất kể$n$. Thuật toán khởi tạo`res = 1`và không bao giờ thay đổi nó, vì mỗi phép nhân bằng 1 modulo$m$. 

Một trường hợp cạnh khác là nhỏ$n$, đặc biệt$n = 1$, trong đó mô đun ở đâu$4$. Đối với bất kỳ$a$, thuật toán tính toán$a^2 \bmod 4$. Phép lũy thừa nhị phân xử lý chính xác điều này ngay cả khi bình phương trung gian gây ra sự rút gọn về 0, như đã thấy trong ví dụ ở đó$2^2 \equiv 0 \pmod 4$. 

Trường hợp thứ ba là khi$a$đã lớn hơn mô đun. Thuật toán ngay lập tức giảm nó bằng`a %= mod`, đảm bảo rằng các hoạt động tiếp theo vẫn bị giới hạn và chính xác.
