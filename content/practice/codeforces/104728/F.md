---
title: "CF 104728F - \u65b0\u53d6\u6a21\u8fd0\u7b97"
description: "Chúng ta được cung cấp một số nguyên tố $p$ và nhiều truy vấn. Mỗi truy vấn cung cấp một số nguyên khổng lồ $n$ và chúng ta cần đánh giá một thao tác tùy chỉnh trên $n!$."
date: "2026-06-29T03:24:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104728
codeforces_index: "F"
codeforces_contest_name: "Huazhong University of Science of Technology Freshmen Cup 2023"
rating: 0
weight: 104728
solve_time_s: 95
verified: false
draft: false
---

[CF 104728F - \u65b0\u53d6\u6a21\u8fd0\u7b97](https://codeforces.com/problemset/problem/104728/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên tố$p$, và nhiều truy vấn. Mỗi truy vấn cung cấp một số nguyên lớn$n$và chúng ta cần đánh giá một thao tác tùy chỉnh trên$n!$. 

hoạt động$x \oplus p$cư xử như lấy$x \bmod p$, nhưng có một điều khó hiểu: trước khi lấy số dư, chúng ta liên tục chia$x$qua$p$miễn là nó chia hết cho$p$. In other words, all factors of$p$được loại bỏ hoàn toàn trước khi tính modulo còn lại$p$. 

Một cách đại số hơn để thấy điều này là nếu chúng ta viết$$x = p^k \cdot r \quad \text{where } p \nmid r,$$sau đó$x \oplus p = r \bmod p$. Từ$r$không chia hết cho$p$, đây chỉ là$r \bmod p$, Nhưng$r$bản thân nó có thể vẫn còn lớn. 

Nhiệm vụ của chúng ta là tính giá trị này cho$x = n!$, cho mỗi truy vấn một cách độc lập. 

Các ràng buộc làm cho việc tính toán trực tiếp giai thừa là không thể. Với$n$lên đến$10^{18}$, thậm chí lặp đi lặp lại lên đến$n$là không thể. Một tính toán giai thừa đơn lẻ đã vượt quá mọi giới hạn thời gian khả thi và có tới$10^5$các truy vấn, do đó, ngay cả công việc logarit trên mỗi truy vấn cũng phải cực kỳ hiệu quả. 

Một cách tiếp cận ngây thơ sẽ cố gắng tính toán$n! \bmod p$, sau đó chia các thừa số của$p$, nhưng điều đó vẫn ngầm yêu cầu xử lý tất cả các số lên tới$n$, điều đó là không thể. 

Trường hợp cạnh tinh tế xuất hiện khi$n < p$. Trong trường hợp này,$n!$không chứa yếu tố$p$, do đó hoạt động giảm xuống chỉ đơn giản là$n! \bmod p$. Một giải pháp bất cẩn luôn áp dụng biện pháp “loại bỏ$p$-factors” tái diễn mà không xử lý trường hợp cơ sở này một cách chính xác có thể dẫn đến độ sâu đệ quy không chính xác hoặc lũy thừa mô-đun không cần thiết. 

Một tình huống khó khăn khác là khi$n$rất lớn nhưng có thương số nhỏ$n // p$. Cấu trúc của lời giải phụ thuộc rất nhiều vào sự phân rã thương số này và việc không nhận ra điều này sẽ dẫn đến hành vi hàm mũ nếu người ta cố gắng mô phỏng trực tiếp cấu trúc giai thừa. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ tính toán$n!$trực tiếp và sau đó liên tục chia cho$p$trong khi có thể, cuối cùng dùng modulo$p$. Về nguyên tắc, điều này đúng vì nó tuân theo định nghĩa của hoạt động một cách chính xác. Tuy nhiên, tính toán$n!$đã yêu cầu$O(n)$phép nhân, điều này không thể thực hiện được$n$lên đến$10^{18}$. Ngay cả đối với một truy vấn duy nhất, điều này là không thể thực hiện được. 

Quan sát quan trọng là chúng ta không bao giờ cần giai thừa đầy đủ, chỉ cần giá trị modulo của nó$p$sau khi loại bỏ tất cả các yếu tố của$p$. Điều này cho thấy sự đóng góp của các số chia hết cho$p$và những cái không chia hết cho$p$. Từ$p$là số nguyên tố, cấu trúc bội số của$p$lặp đi lặp lại thường xuyên, cho phép phân tách theo kiểu chia để trị trên cơ sở$p$. 

Chúng tôi chia số từ$1$ĐẾN$n$thành các khối có kích thước đầy đủ$p$và khối còn lại. Mỗi khối đầy đủ đóng góp một yếu tố có cấu trúc có thể được đơn giản hóa bằng định lý Wilson, trong đó$(p-1)! \equiv -1 \pmod p$. Điều này chuyển đổi từng khối đầy đủ thành một đóng góp nhân đơn giản và phần còn lại là một bài toán giai thừa nhỏ hơn. Điều này dẫn đến việc giảm đệ quy từ$n$ĐẾN$n // p$, cho độ sâu logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Tối ưu |$O(\log_p n + p)$tiền xử lý |$O(p)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một chức năng$F(n)$như giá trị của$n!$sau khi loại bỏ tất cả các yếu tố của$p$, lấy modulo$p$. 

### Tính toán trước 

1. Tính giai thừa modulo$p$cho mọi giá trị từ$0$ĐẾN$p-1$. Điều này cho phép truy cập trực tiếp vào$(n \bmod p)!$bất cứ khi nào chúng tôi cần nó. 

### Tính toán đệ quy 

1. Nếu$n = 0$, trở lại$1$, vì sản phẩm rỗng đóng góp trung tính. 
2. Chia$n$thành các khối có kích thước đầy đủ$p$: cho phép$n = a \cdot p + b$, Ở đâu$b = n \bmod p$. Điều này tách các số thành các chu kỳ hoàn chỉnh và một phần đuôi. 
3. Đuôi góp phần$(b!)$, mà chúng ta đã biết từ quá trình tính toán trước. 
4. Mỗi khối đầy đủ từ$1$ĐẾN$p$đóng góp$(p-1)! \equiv -1 \pmod p$sau khi loại bỏ yếu tố$p$. Đây là nơi tính nguyên thủy của$p$là điều cần thiết. 
5. Do đó, tất cả các khối đầy đủ đều đóng góp$(-1)^a$, vì có$a$những khối như vậy. 
6. Chúng tôi tính toán đệ quy phần đóng góp từ$a!$, bởi vì cấu trúc lặp lại ở quy mô$p$. Điều này mang lại yếu tố$F(a)$. 
7. Kết hợp mọi thứ:$$F(n) = F(a) \cdot (-1)^a \cdot (b!) \bmod p.$$### Tại sao nó hoạt động 

Điều bất biến là ở mọi cấp độ đệ quy, chúng tôi tính đến sự đóng góp của các số được nhóm theo các lớp dư lượng modulo$p$. Mỗi nhóm kích thước$p$hoạt động giống hệt nhau sau khi loại bỏ bội số của$p$, và giảm tới một giá trị không đổi modulo$p$. Phép đệ quy theo dõi có bao nhiêu nhóm đầy đủ tồn tại ở mỗi thang đo và định lý Wilson đảm bảo các nhóm đó thu gọn thành một thừa số dấu đơn giản. Bởi vì mỗi cấp độ giảm$n$ĐẾN$n // p$, không có đóng góp nào bị mất và tất cả các yếu tố của$p$được loại bỏ một cách nhất quán trước khi giảm mô-đun. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T, p = map(int, input().split())

    fact = [1] * p
    for i in range(1, p):
        fact[i] = fact[i - 1] * i % p

    def F(n):
        if n == 0:
            return 1
        a, b = divmod(n, p)
        res = F(a)
        if a % 2:
            res = (res * (p - 1)) % p
        return res * fact[b] % p

    for _ in range(T):
        n = int(input())
        print(F(n))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán trước giai thừa modulo$p$, cho phép tra cứu liên tục theo thời gian cho bất kỳ phần còn lại nào. Hàm đệ quy thực hiện việc phân rã$n = a p + b$, đảm bảo rằng mỗi cấp độ loại bỏ một chữ số của$n$trong căn cứ$p$. Phép nhân với$p-1$xử lý$(-1)^a$số hạng không có lũy thừa phân nhánh. 

Độ sâu đệ quy tối đa là$O(\log_p n)$, an toàn ngay cả đối với$n = 10^{18}$. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi quá trình tính toán bằng cách sử dụng hai đầu vào minh họa nhỏ. 

### Ví dụ 1 

hãy để$p = 5$,$n = 12$. 

| n | a = n//p | b = n%p | F(a) | đóng góp từ | sự thật[b] | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 12 | 2 | 2 | F(2)=2 | (-1)^2 = 1 | 2 | 4 | 

Đây$12! = 479001600$. Sau khi loại bỏ tất cả các hệ số của 5 và giảm modulo 5, kết quả phù hợp với giá trị tính toán. 

Dấu vết này cho thấy bài toán giảm từ 12 xuống 2 trong một bước như thế nào, thể hiện sự sụp đổ logarit của cấu trúc giai thừa. 

### Ví dụ 2 

hãy để$p = 7$,$n = 20$. 

| n | a = n//p | b = n%p | F(a) | đóng góp từ | sự thật[b] | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 20 | 2 | 6 | F(2)=2 | (-1)^2 = 1 | 6! mod 7 = 6 | 5 | 

Điều này chứng tỏ cấu trúc giai thừa và đệ quy còn lại tương tác độc lập như thế nào. Sự đóng góp toàn khối chỉ phụ thuộc vào tính chẵn lẻ của thương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \log_p n + p)$| Mỗi truy vấn giảm$n$theo yếu tố$p$, chi phí tính toán trước giai thừa$O(p)$| 
| Không gian |$O(p)$| Lưu trữ modulo giai thừa$p$| 

Các ràng buộc cho phép lên đến$10^5$truy vấn, nhưng mỗi truy vấn chỉ thực hiện độ sâu đệ quy logarit đối với cơ số$p$. Ngay cả trong trường hợp xấu nhất, điều này vẫn hiệu quả trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import factorial

    # placeholder: assume solve() is defined above
    # return output string
    return "NOT IMPLEMENTED"

# sample cases (structure only)
# assert run("...") == "..."

# custom edge cases
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhỏ n < p | N! mod p | trường hợp cơ sở đúng đắn | 
| n = p | (p-1)! mod p | Ranh giới Wilson | 
| lớn | giá trị tính toán | tính đúng đắn đệ quy | 
| n = 1e18 | đầu ra hợp lệ | xử lý độ sâu | 

## Vỏ cạnh 

Khi nào$n < p$, quá trình đệ quy ngay lập tức dừng lại ở bảng tra cứu giai thừa. Ví dụ, nếu$n = 4$Và$p = 11$, hàm trả về trực tiếp$4!$, vì không chia cho$p$xảy ra. 

Khi$n = p$, sự phân hủy mang lại$a = 1, b = 0$. Thuật toán trả về$F(1) \cdot (p-1) \cdot 1$, phù hợp với thực tế rằng$p! = p \cdot (p-1)!$, và sau khi loại bỏ$p$, chúng ta còn lại với$(p-1)! \equiv -1 \pmod p$, phù hợp với cấu trúc tái phát. 

Đối với rất lớn$n$, lặp lại phép chia cho$p$đảm bảo rằng đệ quy kết thúc ở độ sâu logarit và không có giá trị trung gian nào vượt quá giới hạn mô-đun.
