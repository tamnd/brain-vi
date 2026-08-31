---
title: "CF 104442K - P = NP"
description: "Chúng tôi đang làm việc trong một mô hình máy trong đó tất cả số học được thực hiện bằng số nguyên không dấu 32 bit. Điều đó có nghĩa là mọi kết quả đều bị giảm modulo $2^{32}$ bất cứ khi nào nó vượt quá phạm vi có thể biểu thị."
date: "2026-06-30T18:08:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104442
codeforces_index: "K"
codeforces_contest_name: "AdaByron Regional Madrid 2023"
rating: 0
weight: 104442
solve_time_s: 53
verified: true
draft: false
---

[CF 104442K - P = NP](https://codeforces.com/problemset/problem/104442/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trong một mô hình máy trong đó tất cả số học được thực hiện bằng số nguyên không dấu 32 bit. Điều đó có nghĩa là mọi kết quả đều được giảm modulo$2^{32}$bất cứ khi nào nó vượt quá phạm vi đại diện. Nói cách khác, phép nhân không tạo ra số nguyên không giới hạn, nó bao quanh modulo$2^{32}$. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một giá trị$N$. Về mặt khái niệm, chúng tôi xem xét tất cả các giá trị không dấu 32 bit có thể có$P$. Đối với mỗi như vậy$P$, chúng tôi tính toán sản phẩm$N \cdot P$sử dụng số học tràn 32 bit và chúng tôi kiểm tra xem kết quả có bằng không$P$lại theo các quy tắc số học tương tự. Nhiệm vụ là đếm xem có bao nhiêu giá trị của$P$thỏa mãn điều kiện điểm cố định này. 

Vì vậy, câu hỏi cốt lõi trở thành: trong vành số nguyên modulo$2^{32}$, có bao nhiêu phần tử$P$thỏa mãn$$N \cdot P \equiv P \pmod{2^{32}}.$$Ràng buộc$t \le 10^5$có nghĩa là chúng ta phải trả lời từng truy vấn theo thời gian không đổi hoặc logarit. Bất kỳ cách tiếp cận nào lặp đi lặp lại trên tất cả$2^{32}$các giá trị có thể có của$P$là hoàn toàn không khả thi, vì điều đó sẽ cần khoảng bốn tỷ lần kiểm tra cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi$N = 1$. Trong trường hợp đó mọi$P$thỏa mãn phương trình vì phép nhân với 1 không có tác dụng gì, vì vậy câu trả lời phải là$2^{32}$. Một trường hợp quan trọng khác là$N = 0$, nơi phương trình trở thành$0 \equiv P \pmod{2^{32}}$, vậy chỉ$P = 0$hoạt động, đưa ra chính xác một giải pháp. 

Rủi ro chính trong việc triển khai đơn giản là cố gắng mô phỏng số học tràn cho mỗi$P$. Ngay cả khi được tối ưu hóa bằng C++ hoặc Python, việc lặp lại trên toàn bộ miền là không thể. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo kiểm tra trực tiếp mọi khả năng có thể$P$, tính toán$N \cdot P \bmod 2^{32}$và kiểm tra sự bình đẳng với$P$. Điều này đúng về mặt định nghĩa nhưng đòi hỏi$2^{32}$hoạt động cho mỗi trường hợp thử nghiệm, vượt xa mọi giới hạn thời gian. 

Sự đơn giản hóa chính là ngừng coi đây là số học trên các số nguyên của máy và thay vào đó hãy coi nó như một phương trình mô-đun:$$N \cdot P \equiv P \pmod{2^{32}}.$$Sắp xếp lại mang lại:$$(N - 1)\cdot P \equiv 0 \pmod{2^{32}}.$$Bây giờ bài toán trở thành một sự đồng đẳng tuyến tính cổ điển: chúng ta đang đếm xem có bao nhiêu dư lượng$P$làm cho sản phẩm chia hết cho$2^{32}$. Cho phép$M = 2^{32}$Và$a = N - 1$. Chúng tôi muốn số lượng giải pháp để:$$aP \equiv 0 \pmod{M}.$$Một kết quả lý thuyết số tiêu chuẩn cho chúng ta biết rằng số nghiệm modulo$M$ĐẾN$aP \equiv 0$chính xác là$\gcd(a, M)$. Trực giác cho thấy chỉ có bội số của$M / \gcd(a, M)$tồn tại, và chính xác là có$\gcd(a, M)$dư lượng như vậy trong phạm vi đầy đủ. 

Từ$M = 2^{32}$, câu trả lời đặc biệt đơn giản: đó là lũy thừa lớn nhất của hai phép chia$N - 1$, ngoại trừ khi$N = 1$, chúng tôi xử lý$N - 1 = 0$và có được tất cả$2^{32}$giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả P ​​|$O(2^{32})$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Giảm dựa trên GCD |$O(\log M)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đơn giản hóa vấn đề thành việc tính ước số chung lớn nhất cho mỗi trường hợp thử nghiệm. 

1. Đọc$N$cho một trường hợp thử nghiệm và giải thích modulo số học$M = 2^{32}$. Điều này đảm bảo tất cả hành vi tràn được mô hình hóa chính xác theo yêu cầu. 
2. Tính toán$a = N - 1$trong số học số nguyên. Chúng tôi xử lý giá trị này theo khái niệm modulo$M$, nhưng việc sử dụng số nguyên Python sẽ tránh được vấn đề tràn. 
3. Tính toán$g = \gcd(a, M)$. Giá trị này thể hiện sự chồng chéo về cấu trúc giữa hệ số nhân$a$và mô-đun. 
4. Đầu ra$g$. Điều này trực tiếp đếm có bao nhiêu dư lượng$P$thỏa mãn$aP \equiv 0 \pmod{M}$. 

Tính đúng đắn phụ thuộc vào việc diễn giải sự đồng dạng như một điều kiện chia hết. Chúng tôi đang tính toán các giải pháp để$M \mid aP$và gcd mô tả chính xác bao nhiêu$M$cơ cấu quyền lực chính của được chia sẻ với$a$, cái quyết định mức độ tự do$P$có thể khác nhau. 

### Tại sao nó hoạt động 

điều kiện$aP \equiv 0 \pmod{M}$tương đương với$M \mid aP$. Viết$g = \gcd(a, M)$, chúng tôi phân hủy$a = g a'$Và$M = g M'$Ở đâu$\gcd(a', M') = 1$. Điều kiện chia hết trở thành$M' \mid a'P$. Từ$a'$là modulo khả nghịch$M'$, lực lượng này$M' \mid P$. Như vậy$P$phải là bội số của$M'$, và có chính xác$g$các giá trị như vậy modulo$M$. Điều này đảm bảo số lượng là chính xác và đầy đủ. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

M = 1 << 32

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        N = int(input().strip())
        a = N - 1
        g = math.gcd(a, M)
        out.append(str(g))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên các số nguyên chính xác tùy ý của Python, do đó trừ đi 1 từ$N$không bao giờ gặp rủi ro về vấn đề tràn. mô-đun$2^{32}$được biểu diễn chính xác dưới dạng lũy ​​thừa của hai. Tính toán gcd hiệu quả vì một toán hạng là lũy thừa cố định của hai, do đó trong thực tế nó giảm xuống việc trích xuất bit được đặt thấp nhất của$N-1$, mặc dù sử dụng`math.gcd`giữ cho dung dịch sạch và an toàn. 

Một lỗi triển khai phổ biến là cố gắng mô phỏng rõ ràng phép nhân tràn 32 bit bên trong các vòng lặp. Điều đó là không cần thiết vì phép biến đổi đại số loại bỏ hoàn toàn phép nhân. 

## Ví dụ đã hoạt động 

Hãy xem xét hai trường hợp đại diện. 

Đầu tiên,$N = 1$. Sau đó$a = 0$Và$g = \gcd(0, 2^{32}) = 2^{32}$. Mọi$P$thỏa mãn phương trình. 

| Bước | Giá trị | 
| --- | --- | 
| N | 1 | 
| a = N - 1 | 0 | 
| gcd(a, 2^32) | 2^32 | 
| trả lời | 2^32 | 

Điều này xác nhận rằng phép nhân đồng nhất mang lại tất cả các điểm cố định có thể có. 

Thứ hai,$N = 5$. Sau đó$a = 4$, và kể từ đó$2^{32}$là lũy thừa của 2, gcd là lũy thừa lớn nhất của 2 chia 4, bằng 4. 

| Bước | Giá trị | 
| --- | --- | 
| N | 5 | 
| a = N - 1 | 4 | 
| gcd(a, 2^32) | 4 | 
| trả lời | 4 | 

Điều này cho thấy chỉ$P$chia hết cho$2^{30}$được chia tỷ lệ thích hợp trong cấu trúc modulo vẫn tồn tại, mang lại chính xác bốn dư lượng hợp lệ. 

Những ví dụ này nêu bật cách giải chỉ phụ thuộc vào cấu trúc nhị phân của$N - 1$, không phải về độ lớn của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \log 2^{32})$| Mỗi bài kiểm tra tính toán một gcd với mô-đun 32 bit cố định | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Sự phức tạp dễ dàng phù hợp trong giới hạn ngay cả đối với$t = 10^5$, vì gcd có lũy thừa bằng 2 cực kỳ nhanh trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io
import math

M = 1 << 32

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    res = []
    for _ in range(t):
        N = int(input().strip())
        res.append(str(math.gcd(N - 1, M)))
    return "\n".join(res)

# sample-style checks
assert solve("1\n1\n") == str(1 << 32)

# N = 2 => N-1 = 1 => gcd = 1
assert solve("1\n2\n") == "1"

# N = 5 => N-1 = 4 => gcd = 4
assert solve("1\n5\n") == "4"

# N = 0 => N-1 = -1 => gcd = 1
assert solve("1\n0\n") == "1"

# multiple cases
assert solve("3\n1\n2\n5\n") == f"{1<<32}\n1\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N = 1 |$2^{32}$| Tất cả các giá trị đều là điểm cố định | 
| N = 0 | 1 | Xử lý bao quanh tiêu cực | 
| N = 5 | 4 | Cấu trúc gcd không tầm thường | 
| Các trường hợp hỗn hợp | đa dạng | Tính chính xác của nhiều truy vấn | 

## Vỏ cạnh 

Khi nào$N = 1$, biểu thức trở thành$1 \cdot P = P$cho tất cả$P$, do đó thuật toán tính toán$a = 0$và trả về chính xác$\gcd(0, 2^{32}) = 2^{32}$. Điều này tránh bất kỳ sự phân nhánh đặc biệt nào trong mã. 

Khi$N = 0$, chúng tôi tính toán một cách hiệu quả$a = -1$. Trong modulo số học mô-đun$2^{32}$, điều này hoạt động giống như$2^{32} - 1$, nguyên tố cùng nhau với$2^{32}$, do đó gcd trở thành 1. Thuật toán xử lý việc này một cách tự nhiên mà không cần chuyển đổi không dấu. 

Khi$N$là lũy thừa lớn của hai,$N - 1$trở thành một chuỗi các bit được đặt và gcd trích xuất bit được đặt thấp nhất, đảm bảo câu trả lời luôn là lũy thừa của hai phù hợp với cấu trúc mô đun.
