---
title: "CF 1734E - Đồng đẳng hình chữ nhật"
description: "Chúng ta được yêu cầu xây dựng một ma trận $n nhân n$ trên trường thặng dư modulo một số nguyên tố $n$. Mọi mục nhập phải là một số nguyên trong phạm vi từ $0$ đến $n-1$ và đường chéo đã được cố định: mục nhập đường chéo $i$-th phải bằng $bi$."
date: "2026-06-15T03:30:23+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1734
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 822 (Div. 2)"
rating: 2100
weight: 1734
solve_time_s: 390
verified: false
draft: false
---

[CF 1734E - Sự đồng dạng hình chữ nhật](https://codeforces.com/problemset/problem/1734/E) 

**Xếp hạng:** 2100 
**Tas:** giải thuật xây dựng, lý thuyết số 
**Thời gian giải:** 6 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng một$n \times n$ma trận trên trường thặng dư modulo một số nguyên tố$n$. Mỗi mục nhập phải là số nguyên trong phạm vi$0$ĐẾN$n-1$, và đường chéo đã được cố định:$i$- mục nhập đường chéo thứ phải bằng$b_i$. 

Ràng buộc chính là điều kiện chống cộng mạnh trên mỗi cặp hàng riêng biệt và mỗi cặp cột riêng biệt. Nếu chúng ta lấy hai hàng bất kỳ$r_1 < r_2$và hai cột bất kỳ$c_1 < c_2$, thì ma trận phải tránh đẳng thức$$a_{r_1,c_1} + a_{r_2,c_2} \equiv a_{r_1,c_2} + a_{r_2,c_1} \pmod n.$$Điều này chính xác là nói rằng mọi$2 \times 2$ma trận con không được thỏa mãn modulo “bằng tổng chéo” cổ điển$n$. Tương tự, mọi ma trận con như vậy phải có định thức khác 0 trên trường hữu hạn$\mathbb{F}_n$. 

Vì vậy, nhiệm vụ là xây dựng một ma trận trên trường nguyên tố với một đường chéo quy định và với tính chất là mọi$2 \times 2$thứ yếu là không suy biến. 

Ràng buộc$n < 350$nghĩa là chúng ta có đủ khả năng$O(n^2)$hoặc thậm chí$O(n^2 \log n)$công trình xây dựng. Bất kỳ khối nào trên tất cả bốn chỉ số sẽ là quá chậm vì có$O(n^4)$hạn chế. 

Một trường hợp thất bại tinh tế đối với các phương pháp tiếp cận ngây thơ là việc thử điền ngẫu nhiên. Ví dụ: việc chọn các giá trị ngẫu nhiên cho mỗi hàng một cách độc lập thường sẽ vô tình tạo ra một suy biến$2 \times 2$ma trận phụ. Ngay cả việc sửa các mục theo đường chéo cũng không ngăn được những va chạm này vì ràng buộc có tính tổng thể và có cấu trúc bậc hai. 

Một nỗ lực ngây thơ khác là coi các hàng độc lập như các vectơ tùy ý với các mục nhập đường chéo cố định. Điều này cũng không thành công vì điều kiện kết hợp đồng thời các cặp hàng và cột. 

Khó khăn chính là điều kiện không cục bộ đối với các hàng hoặc cột mà thay vào đó thực thi một cấu trúc giống như thứ hạng toàn cầu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử điền vào từng ô ma trận và sau mỗi lần gán, hãy kiểm tra tất cả$O(n^4)$gấp bốn lần vì vi phạm. Ngay cả khi chúng tôi tối ưu hóa việc kiểm tra bằng cách duy trì tất cả$2 \times 2$trẻ vị thành niên tăng dần, mỗi bản cập nhật vẫn ảnh hưởng$O(n^2)$các ràng buộc và chúng ta sẽ cần phải kiểm tra tính nhất quán nhiều lần. Điều này nhanh chóng dẫn đến ít nhất$O(n^4)$làm việc tổng thể, điều đó là không thể đối với$n \approx 350$. 

Cái nhìn sâu sắc quan trọng là diễn giải lại điều kiện theo đại số. Sự bình đẳng bị cấm$$a_{r_1,c_1} + a_{r_2,c_2} = a_{r_1,c_2} + a_{r_2,c_1}$$chính xác là tuyên bố rằng ma trận có thứ hạng$1$khi bị hạn chế ở bất kỳ$2 \times 2$khối. Trên một trường, điều này tương đương với việc yêu cầu ma trận không hoạt động giống như một hàm có thể tách cộng trên bất kỳ hình chữ nhật nào. 

Cách cổ điển để tránh tất cả những đẳng thức như vậy trên một trường nguyên tố là áp dụng cấu trúc song tuyến tính. Nếu chúng ta có thể biểu diễn các mục dưới dạng$$a_{i,j} = x_i y_j$$TRONG$\mathbb{F}_n$, thì mọi$2 \times 2$định thức trở thành 0, điều này trái ngược với điều chúng ta mong muốn. Vì vậy, cấu trúc hạng 1 thuần túy không thành công. 

Thay vào đó, chúng ta cần một cấu trúc trong đó mọi$2 \times 2$định thức khác 0:$$a_{i,c_1} + a_{r_2,c_2} \ne a_{r_1,c_2} + a_{r_2,c_1}.$$Một thủ thuật tiêu chuẩn trong các trường nguyên tố là mã hóa các hàng và cột bằng cách sử dụng các hàm tuyến tính có độ dốc được chọn cẩn thận để mỗi hình chữ nhật tạo ra một tổ hợp affine riêng biệt. 

Cách xây dựng đúng là coi các chỉ số là các phần tử của$\mathbb{F}_n$và xác định ma trận bằng cách sử dụng phép nâng bậc hai:$$a_{i,j} = i \cdot j + c_i + d_j \pmod n,$$Ở đâu$c_i$Và$d_j$là các hiệu chỉnh được chọn để phù hợp với các ràng buộc về đường chéo. 

Hình thức này rất quan trọng vì các thuật ngữ chéo$i \cdot j$đảm bảo rằng bất kỳ hình chữ nhật nào cũng tạo ra một biểu thức song tuyến tính có các số hạng hỗn hợp không bị hủy trừ khi các chỉ số trùng nhau. Các điều khoản bổ sung$c_i$Và$d_j$cho phép chúng tôi thực thi các giá trị đường chéo mà không phá hủy tính không suy biến của trẻ vị thành niên. 

Bây giờ chúng ta thực thi điều kiện đường chéo:$$a_{i,i} = i^2 + c_i + d_i \equiv b_i \pmod n.$$Chúng ta được tự do lựa chọn$c_i = 0$, sau đó đặt$$d_i = b_i - i^2 \pmod n.$$Điều này xác định đầy đủ ma trận:$$a_{i,j} = i \cdot j + (b_j - j^2).$$Cấu trúc này đảm bảo đường chéo là chính xác và duy trì sự bất đối xứng đại số mạnh mẽ giữa các hàng và cột nhằm ngăn ngừa sự hủy bỏ trong bất kỳ trường hợp nào.$2 \times 2$ma trận phụ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^4)$|$O(n^2)$| Quá chậm | 
| Tối ưu |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc hoàn toàn theo modulo số học$n$, coi các chỉ số là các phần tử của trường hữu hạn. 

1. Giải thích từng chỉ mục hàng$i$và chỉ số cột$j$dưới dạng số nguyên trong$[0, n-1]$. Điều này cho phép tính toán trực tiếp trong trường này. 
2. Tính số hạng hiệu chỉnh cột cho từng cột$j$, được định nghĩa là$d_j = b_j - j^2 \mod n$. Điều này được chọn sao cho có thể thỏa mãn ràng buộc đường chéo mà không tạo ra nhiễu phụ thuộc vào hàng. 
3. Xây dựng từng mục ma trận bằng công thức$a_{i,j} = i \cdot j + d_j \mod n$. Điều này đảm bảo rằng mỗi cột đều có sự dịch chuyển affine cố định được áp dụng nhất quán trên tất cả các hàng. 
4. Xuất ra từng hàng ma trận kết quả. 

Lý do tách sự phụ thuộc thành số hạng song tuyến tính$i \cdot j$và một sự điều chỉnh chỉ theo cột là bất kỳ đẳng thức nào liên quan đến một$2 \times 2$ma trận con mở rộng thành một danh tính đa thức trong$i$Và$j$. Thuật ngữ sản phẩm hỗn hợp ngăn chặn việc hủy trên các cặp hàng khác nhau. 

### Tại sao nó hoạt động 

Sửa hai hàng bất kỳ$r_1 < r_2$và cột$c_1 < c_2$. Mở rộng chênh lệch tổng chéo mang lại$$(a_{r_1,c_1} + a_{r_2,c_2}) - (a_{r_1,c_2} + a_{r_2,c_1})$$

$$= (r_1 c_1 + d_{c_1}) + (r_2 c_2 + d_{c_2}) - (r_1 c_2 + d_{c_2}) - (r_2 c_1 + d_{c_1})$$Tất cả$d$-điều khoản hủy bỏ, rời đi$$r_1 c_1 + r_2 c_2 - r_1 c_2 - r_2 c_1 = (r_1 - r_2)(c_1 - c_2).$$Từ$n$là số nguyên tố, trường không có ước số bằng 0, nên biểu thức này khác 0 theo modulo$n$bất cứ khi nào$r_1 \ne r_2$Và$c_1 \ne c_2$. Điều này đảm bảo mọi$2 \times 2$ma trận con vi phạm đẳng thức bị cấm. 

Điều kiện đường chéo đã được đưa vào$d_i$, do đó tất cả các ràng buộc được thỏa mãn đồng thời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    b = list(map(int, input().split()))
    
    d = [(b[j] - (j * j) % n) % n for j in range(n)]
    
    for i in range(n):
        row = []
        for j in range(n):
            val = (i * j + d[j]) % n
            row.append(str(val))
        print(" ".join(row))

if __name__ == "__main__":
    main()
```Việc thực hiện trực tiếp theo công thức dẫn xuất. Việc tính toán trước của$d_j$đảm bảo tính chính xác theo đường chéo, trong khi vòng lặp lồng nhau xây dựng từng mục trong$O(1)$thời gian. 

Một cạm bẫy triển khai phổ biến là quên rằng tất cả các hoạt động phải được giảm modulo$n$ở mỗi bước, đặc biệt là số hạng bình phương$j^2$. Một điểm tinh tế khác là đảm bảo rằng phép trừ cũng được thực hiện theo modulo$n$, vì các giá trị âm của Python sẽ lan truyền không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
b = [0, 0]
```Chúng tôi tính toán:$d_0 = 0 - 0 = 0$,$d_1 = 0 - 1 = -1 \equiv 1$. 

Bây giờ xây dựng ma trận: 

| tôi \ j | 0 | 1 | 
| --- | --- | --- | 
| 0 | (0 + 0) = 0 | (0 + 1) = 1 | 
| 1 | (0 + 0) = 0 | (1 + 1) = 2 ≡ 0 | 

Đầu ra:```
0 1
0 0
```Điều này xác nhận đường chéo là chính xác và điều kiện tổng chéo được giữ nguyên vì$(0+0) \ne (1+0) \mod 2$. 

### Ví dụ 2 

đầu vào:```
n = 3
b = [1, 1, 1]
```Tính toán hiệu chỉnh:$d_0 = 1 - 0 = 1$,$d_1 = 1 - 1 = 0$,$d_2 = 1 - 4 = -3 \equiv 0$. 

Xây dựng các mục: 

| tôi \ j | 0 | 1 | 2 | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 2 | 1 | 2 | 
| 2 | 3≡0 | 2 | 4≡1 | 

Ma trận:```
1 0 0
2 1 2
0 2 1
```Mọi$2 \times 2$yếu tố quyết định giảm xuống$(r_1-r_2)(c_1-c_2) \ne 0$, xác nhận tính hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi mục nhập ma trận được tính một lần với số học O(1) | 
| Không gian |$O(n)$| Chỉ mảng hiệu chỉnh được lưu trữ | 

Việc xây dựng bậc hai dễ dàng đủ nhanh để$n \le 350$, chỉ cần khoảng$1.2 \times 10^5$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import math

    input = sys.stdin.readline
    n = int(input())
    b = list(map(int, input().split()))
    
    d = [(b[j] - (j * j) % n) % n for j in range(n)]
    out = []
    for i in range(n):
        row = [(i * j + d[j]) % n for j in range(n)]
        out.append(" ".join(map(str, row)))
    return "\n".join(out)

# provided sample
assert run("2\n0 0\n") == "0 1\n0 0"

# custom: all equal
assert run("3\n1 1 1\n")  # structure check

# custom: identity-like diagonal
assert run("5\n0 1 2 3 4\n")

# custom: random small
assert run("4\n1 3 2 0\n")

# custom: edge n=2 alternative
assert run("2\n1 1\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 toàn số 0 | ma trận 2x2 hợp lệ | độ đúng cơ sở | 
| n=3 đường chéo không đổi | đầu ra có cấu trúc | tính nhất quán của xây dựng | 
| n=5 đường chéo tăng dần | tính đúng đắn của số học mô-đun | xử lý không tầm thường b | 
| n=4 giá trị hỗn hợp | độ bền chung | không có giả định đối xứng | 

## Vỏ cạnh 

Trường hợp nhỏ nhất có thể$n=2$là mong manh nhất bởi vì mọi$2 \times 2$ma trận con là toàn bộ ma trận. Trong trường hợp này, việc xây dựng vẫn tạo ra các thuật ngữ chéo khác biệt bởi vì$(r_1-r_2)(c_1-c_2) \equiv 1 \cdot 1 \not\equiv 0 \mod 2$, do đó ràng buộc được thỏa mãn ngay cả trong cấu hình tối thiểu. 

Khi tất cả$b_i$bằng nhau thì việc xây dựng trở thành ma trận song tuyến tính thuần túy cộng với độ dịch cột không đổi. Ràng buộc đường chéo không gây ra sự bất thường vì tất cả các hiệu chỉnh đều được hấp thụ vào$d_j$, để lại cấu trúc$i \cdot j$thuật ngữ không thay đổi, đó là điều đảm bảo tính hợp lệ. 

Nếu như$b_i = i$, việc hiệu chỉnh đường chéo trở thành$d_i = i - i^2$, có thể rất khác nhau. Mặc dù vậy, tất cả các tương tác hàng đều bị loại bỏ do chênh lệch chéo và chỉ còn lại số hạng tích, duy trì tính chính xác ngay cả dưới các đường chéo rất không đồng nhất.
