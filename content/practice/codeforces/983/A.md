---
problem: 983A
contest_id: 983
problem_index: A
name: "Finite or not?"
contest_name: "Codeforces Round 483 (Div. 1) [Thanks, Botan Investments and Victor Shaburov!]"
rating: 1700
tags: ["implementation", "math"]
answer: passed_samples
verified: true
solve_time_s: 67
date: 2026-06-18
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33757e-78f4-83ec-b62c-055c2636f4e3
---

#CF 983A – Hữu hạn hay không? 

**Đánh giá:** 1700 
**Tags:** thực hiện, toán học 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33757e-78f4-83ec-b62c-055c2636f4e3 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một số hữu tỉ$p/q$và được yêu cầu xác định xem biểu diễn của nó có hữu hạn hay không khi viết dưới dạng cơ số$b$. Biểu diễn hữu hạn có nghĩa là sau một thời điểm nào đó, quá trình mở rộng dừng lại và không cần thêm chữ số nào nữa, tương tự như cách$1/2 = 0.5$ở cơ sở 10. 

Mỗi truy vấn cho ba số nguyên: một tử số$p$, mẫu số$q$, và một cơ sở$b$. Nhiệm vụ không phải là tính toán khai triển mà chỉ quyết định liệu nó có kết thúc hay không. 

Các ràng buộc rất lớn: lên tới$10^5$truy vấn và giá trị lên đến$10^{18}$. Điều này loại trừ bất kỳ cách tiếp cận nào mô phỏng phép chia hoặc xây dựng các biểu diễn từng chữ số. Ngay cả một phép chia dài cho mỗi truy vấn cũng sẽ không khả thi vì mẫu số và cơ số có thể đủ lớn để các trạng thái trung gian yêu cầu số học chính xác tùy ý và có khả năng lặp lại nhiều lần. 

Một điểm tinh tế là$p$không liên quan đến tính hữu hạn của biểu diễn. Ví dụ,$6/12$Và$1/2$có hành vi chấm dứt tương tự trong bất kỳ cơ sở nào. Vì vậy, vấn đề cơ bản là về$q$Và$b$, không phải về$p$. 

Các trường hợp cạnh xuất hiện khi phân số đã là số nguyên. Ví dụ, nếu$p = 12$Và$q = 3$, kết quả là$4$, luôn luôn chấm dứt bất kể cơ sở. Một cách tiếp cận ngây thơ vẫn có thể cố gắng phân tích các thừa số nguyên tố một cách không cần thiết, nhưng trường hợp này cần được phân loại ngay là hữu hạn. 

Một trường hợp cạnh khác phát sinh khi cơ sở chia sẻ tất cả các thừa số nguyên tố của mẫu số rút gọn. Ví dụ,$1/3$ở cơ số 3 là hữu hạn vì$3$chia căn cứ. Trong cơ số 10 nó là vô hạn vì$3$không chia$2 \cdot 5$. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng mô phỏng sự phân chia$p/q$trong căn cứ$b$, nhân số dư nhiều lần với$b$và trích xuất các chữ số. Về nguyên tắc, điều này có hiệu quả vì một phân số kết thúc chính xác khi phần còn lại bằng 0 ở một bước nào đó. Tuy nhiên, phần còn lại có thể quay vòng trong một thời gian dài, có thể lên tới$q$các trạng thái riêng biệt. Với$q$lớn như$10^{18}$, điều này trở nên không thể ngay cả đối với một truy vấn. 

Cái nhìn sâu sắc về cấu trúc quan trọng là tránh xa biểu diễn và thay vào đó nghiên cứu lý thuyết số. Một số hữu tỉ có độ giãn nở hữu hạn cơ số$b$chính xác khi mẫu số rút gọn không có thừa số nguyên tố nào khác ngoài những thừa số đã có trong$b$. Sau khi giảm$p/q$ở số hạng thấp nhất, điều kiện chỉ phụ thuộc vào việc mọi thừa số nguyên tố của$q$cũng chia$b$. 

Điều này làm giảm vấn đề về việc đơn giản hóa gcd lặp đi lặp lại và sau đó loại bỏ khỏi$q$tất cả các yếu tố được chia sẻ với$b$. Nếu có gì còn sót lại trong$q$, phần còn lại đó đưa ra một thừa số nguyên tố không tương thích, buộc phải mở rộng vô hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng phép chia) |$O(q)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Tối ưu (giảm hệ số nguyên tố thông qua việc loại bỏ gcd) |$O(\log \min(q,b))$khấu hao theo truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc với từng truy vấn một cách độc lập. 

1. Đầu tiên rút gọn phân số bằng cách tính$g = \gcd(p, q)$và thiết lập$q \leftarrow q/g$. 

Bước này loại bỏ tất cả các thừa số chung để chúng ta chỉ phân tích mẫu số tối giản. Bất kỳ yếu tố nào được chia sẻ trước đó đều không thể ảnh hưởng đến việc kết thúc vì nó bị loại bỏ trong phân số. 
2. Xóa khỏi$q$tất cả các thừa số nguyên tố cũng có mặt trong cơ sở$b$. 

Chúng tôi tính toán nhiều lần$g = \gcd(q, b)$. Nếu như$g > 1$, chúng tôi chia$q \leftarrow q/g$. 

Điều này hoạt động vì$\gcd(q, b)$trích xuất chính xác tích của các số nguyên tố chung, ngay cả khi không phân tích một trong hai số. 
3. Lặp lại bước trước cho đến khi$\gcd(q, b) = 1$. Khi đó, không còn yếu tố nào của$q$có thể được sản xuất bởi sức mạnh của$b$. 
4. Nếu vào chung kết$q$bằng 1, mẫu số đã được cơ số hấp thụ hoàn toàn và việc biểu diễn kết thúc. Ngược lại, ít nhất một thừa số nguyên tố của$q$không tương thích với$b$, làm cho sự khai triển là vô hạn. 

### Tại sao nó hoạt động 

Một số hữu tỉ có khai triển tận cùng cơ số$b$chính xác khi mẫu số rút gọn của nó chia một số lũy thừa của$b$. Điều kiện đó tương đương với việc nói mọi thừa số nguyên tố của mẫu số cũng phải xuất hiện trong$b$. Quá trình loại bỏ gcd lặp đi lặp lại sẽ loại bỏ một cách có hệ thống khỏi$q$mọi yếu tố đều phù hợp$b$. Nếu bất cứ thứ gì còn sót lại thì đó là thừa số nguyên tố không bao giờ có thể được đưa ra bằng cách nhân với$b$, điều này đảm bảo rằng phần còn lại trong khai triển cơ số không bao giờ biến mất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def solve():
    n = int(input())
    for _ in range(n):
        p, q, b = map(int, input().split())

        g = math.gcd(p, q)
        q //= g

        # remove all factors of q that are in b
        while True:
            g = math.gcd(q, b)
            if g == 1:
                break
            while q % g == 0:
                q //= g

        if q == 1:
            print("Finite")
        else:
            print("Infinite")

if __name__ == "__main__":
    solve()
```Bước đầu tiên giảm phân số về số hạng thấp nhất, đảm bảo chúng ta không mang các yếu tố không liên quan từ$p$. Sau đó, vòng lặp sẽ liên tục trích xuất bất kỳ sự trùng lặp nào giữa mẫu số và cơ số bằng cách sử dụng gcd, cách này hiệu quả ngay cả đối với$10^{18}$- số có kích thước. 

Vòng phân chia bên trong là cần thiết bởi vì$\gcd(q, b)$có thể chứa nhiều lần xuất hiện của một yếu tố và chúng tôi phải loại bỏ hoàn toàn yếu tố đó khỏi$q$trước khi tính toán lại. Điều này đảm bảo rằng mỗi lần lặp lại làm giảm nghiêm ngặt$q$, ngăn ngừa vòng lặp vô hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 12 10
```| Bước | p | q | b | gcd(q,b) | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 6 | 12 | 10 | - | giảm phân số | 
| giảm | 6 | 2 | 10 | - | chia cho gcd(6,12)=6 | 
| vòng lặp | 6 | 2 | 10 | 2 | loại bỏ yếu tố 2 | 
| cuối cùng | 6 | 1 | 10 | 1 | dừng lại | 

Từ$q = 1$, đầu ra là hữu hạn. 

Điều này chứng tỏ rằng một khi mẫu số trở nên bao gồm đầy đủ các thừa số cơ bản thì việc khai triển sẽ chấm dứt. 

### Ví dụ 2 

đầu vào:```
4 3 10
```| Bước | p | q | b | gcd(q,b) | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 4 | 3 | 10 | - | giảm phân số | 
| giảm | 4 | 3 | 10 | - | đã giảm rồi | 
| vòng lặp | 4 | 3 | 10 | 1 | dừng lại ngay lập tức | 

Từ$q = 3 \neq 1$, đầu ra là vô hạn. 

Điều này cho thấy trường hợp mẫu số chứa một số nguyên tố không có trong cơ số, do đó không thể rút gọn được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log q \cdot \log b)$mỗi truy vấn | Mỗi bước gcd và bước chia làm giảm mẫu số và gcd chạy theo thời gian logarit | 
| Không gian |$O(1)$| Chỉ duy trì một số nguyên không đổi | 

Tổng công việc trên$10^5$các truy vấn vẫn hiệu quả vì mỗi mẫu số co lại nhanh chóng khi loại bỏ gcd lặp đi lặp lại và đầu vào lớn không dẫn đến chu kỳ dài. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        n = int(input())
        for _ in range(n):
            p, q, b = map(int, input().split())

            g = math.gcd(p, q)
            q //= g

            while True:
                g = math.gcd(q, b)
                if g == 1:
                    break
                while q % g == 0:
                    q //= g

            print("Finite" if q == 1 else "Infinite")

    from io import StringIO
    old_stdout = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("2\n6 12 10\n4 3 10\n") == "Finite\nInfinite"

# custom cases
assert run("1\n0 5 10\n") == "Finite"  # zero numerator
assert run("1\n10 1 2\n") == "Finite"  # integer result
assert run("1\n1 6 10\n") == "Infinite"  # 6 has factor 3
assert run("1\n1 8 2\n") == "Finite"  # denominator only 2-power
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0/5 | hữu hạn | trường hợp cạnh tử số bằng 0 | 
| 1/10 | hữu hạn | phân số nguyên | 
| 1/6 trong cơ số 10 | Vô hạn | thiếu thừa số nguyên tố | 
| 1/8 ở cơ sở 2 | hữu hạn | chỉ hệ số nguyên tố tương thích | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tử số bằng 0. Đối với đầu vào$0/q$, giá trị luôn bằng 0, có biểu diễn kết thúc ở bất kỳ cơ số nào. Sau khi giảm,$p=0$làm cho$\gcd(p,q)=q$, Vì thế$q$trở thành 1 ngay lập tức. Thuật toán xuất ra hữu hạn một cách chính xác mà không cần xử lý thêm. 

Một trường hợp khác là khi phân số đã là số nguyên. Ví dụ$10/2$giảm xuống$5/1$. Mẫu số trở thành 1 ở bước đầu tiên và thuật toán kết thúc ngay lập tức. Điều này tránh các vòng lặp gcd không cần thiết và xác nhận tính chính xác cho tất cả các phân số có giá trị nguyên. 

Trường hợp thứ ba là khi mẫu số chỉ chứa các thừa số có mặt ở cơ số nhưng có bội số cao, chẳng hạn như$1/8$trong cơ sở 2. Việc trích xuất gcd lặp đi lặp lại sẽ loại bỏ tất cả các yếu tố một cách rõ ràng vì gcd luôn trích xuất ít nhất một phần trùng lặp hoàn toàn với cấu trúc cơ sở và phép chia lặp lại đảm bảo loại bỏ hoàn toàn, cuối cùng giảm$q$đến 1.
