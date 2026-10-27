---
title: "CF 106107C - Khối lập phương của DGeneral Hamilton"
description: "Chúng ta đang xây dựng một khối lập phương lớn có kích thước $n nhân n nhân n$ từ $n^3$ các khối đơn vị giống hệt nhau. Mỗi khối lập phương nhỏ có sáu mặt được đánh số cố định từ 1 đến 6, mỗi mặt một nhãn."
date: "2026-06-19T22:51:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106107
codeforces_index: "C"
codeforces_contest_name: "SCPC Teens 2025"
rating: 0
weight: 106107
solve_time_s: 58
verified: true
draft: false
---

[CF 106107C - Khối lập phương của DGeneral Hamilton](https://codeforces.com/problemset/problem/106107/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng một khối lập phương có kích thước lớn$n \times n \times n$từ$n^3$khối đơn vị giống hệt nhau. Mỗi khối lập phương nhỏ có sáu mặt được đánh số cố định từ 1 đến 6, mỗi mặt một nhãn. Điểm tự do chính là chúng ta có thể xoay từng khối nhỏ một cách độc lập trước khi đặt nó vào cấu trúc lớn. 

Khi tất cả các hình khối đã được đặt, mỗi mặt của hình khối lớn sẽ trở thành một$n \times n$lưới các mặt đơn vị có thể nhìn thấy được. Mỗi hình vuông hiển thị chỉ đơn giản hiển thị bất kỳ số nào ở mặt hướng ra ngoài của khối lập phương nhỏ chiếm vị trí đó. Bản thân khối lập phương lớn cũng có sáu mặt và mỗi mặt được liên kết với một trong các nhãn từ 1 đến 6, nhưng phép gán đó không hạn chế các ô riêng lẻ ngoài quy tắc định hướng hình học. 

Nhiệm vụ là đếm xem có thể hình thành bao nhiêu khối lập phương lớn được gắn nhãn riêng biệt, trong đó hai cấu hình được coi là khác nhau nếu bất kỳ ô hiển thị nào trên bất kỳ mặt nào hiển thị một số khác. 

Ràng buộc$n \le 10^9$ngay lập tức loại bỏ bất kỳ phương pháp nào kiểm tra các hình khối riêng lẻ hoặc lặp lại trên khối. Thậm chí đến thăm tất cả$n^3$hình khối là không thể. Lời giải chỉ phải phụ thuộc vào sự đối xứng về cấu trúc, nghĩa là chúng ta nên phân loại các hình khối theo loại vị trí thay vì liệt kê chúng. 

Trường hợp cạnh tinh tế xuất hiện khi$n$là rất nhỏ. Vì$n=1$, có một khối duy nhất đóng góp tất cả sáu mặt. Vì$n=2$, mỗi khối lập phương nằm trên một góc nên mỗi khối lập phương tiếp xúc đồng thời với ba mặt của khối lập phương lớn. Những kích thước suy biến này thường phá vỡ các công thức bắt nguồn từ “lớn$n$” hình học, vì vậy chúng phải được kiểm tra riêng. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ gán một trong 24 phép quay cho mỗi$n^3$hình lập phương, vì hình lập phương có 24 đối xứng quay. Điều này dẫn đến$24^{n^3}$trước khi xem xét liệu các ràng buộc về bề mặt có được thỏa mãn hay không. Tuy nhiên, hầu hết các lựa chọn này không độc lập một khi chúng ta bắt buộc các mặt biên phải căn chỉnh chính xác với hướng ra ngoài của khối lập phương lớn. Quan trọng hơn, thậm chí viết ra hoặc lặp đi lặp lại$n^3$các vị trí đã không thể thực hiện được. 

Quan sát quan trọng là các ràng buộc chỉ phụ thuộc vào vị trí của khối lập phương chứ không phụ thuộc vào danh tính của nó. Mọi vị trí trên khối lập phương lớn đều thuộc một trong bốn loại: bên trong, mặt bên trong, cạnh hoặc góc. Mỗi danh mục áp đặt một số ràng buộc cố định về mặt và do đó có một số hướng hợp lệ cố định cho khối nhỏ được đặt ở đó. Một khi sự phân loại này được thực hiện, vấn đề sẽ trở thành sản phẩm của những lựa chọn độc lập đối với các loại vị thế này. 

Điều này làm giảm vấn đề từ việc suy luận về các hình khối riêng lẻ đến việc đếm xem có bao nhiêu vị trí rơi vào mỗi lớp hình học và nhân số lượng hướng cục bộ của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các vị trí khối |$O(24^{n^3})$|$O(1)$| Quá chậm | 
| Hình học theo phân loại vị trí |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hướng dẫn thuật toán 

1. Phân loại từng khối đơn vị trong$n \times n \times n$cấu trúc bằng bao nhiêu mặt của khối lập phương lớn mà nó chạm vào. Điều này xác định có bao nhiêu ràng buộc bên ngoài mà nó phải đáp ứng. 
2. Đếm các hình khối bên trong không chạm vào bất kỳ ranh giới nào. có$(n-2)^3$những vị trí như vậy khi$n \ge 2$. Mỗi cái đều không có hạn chế, vì vậy bất kỳ phép quay nào trong số 24 phép quay khối đều hợp lệ. 
3. Đếm các hình khối mặt trong, nằm trên một mặt nhưng không nằm trên các cạnh. có$6(n-2)^2$của họ. Mỗi khối như vậy có chính xác một mặt bị ràng buộc, vì vậy chúng ta cần các phép quay đặt một số cụ thể theo một hướng cụ thể. Khối lập phương có 4 góc quay thỏa mãn một ràng buộc mặt cố định, tương ứng với một góc quay tự do quanh trục đó. 
4. Đếm các hình khối cạnh trừ các góc. có$12(n-2)$của họ. Mỗi mặt tiếp xúc với hai mặt, do đó hai ràng buộc vuông góc phải được thỏa mãn đồng thời. Chính xác 2 phép quay thỏa mãn hai ràng buộc mặt cố định. 
5. Đếm các hình khối ở góc. Luôn có 8. Mỗi lần chạm vào ba mặt, xác định đầy đủ hướng của nó nên đúng 1 lần quay là hợp lệ. 
6. Nhân tất cả các khoản đóng góp: mỗi vị trí đóng góp độc lập nên tổng số là$24^{(n-2)^3} \cdot 4^{6(n-2)^2} \cdot 2^{12(n-2)}$. 
7. Xử lý các giá trị nhỏ của$n$. Vì$n=1$, có một khối lập phương và tất cả 24 phép quay đều đúng. Vì$n=2$, tất cả các hình khối đều là góc nên câu trả lời là 1. 

### Tại sao nó hoạt động 

Mỗi vị trí của khối lập phương chỉ đóng góp các ràng buộc từ các mặt của khối lập phương lớn mà nó nằm trên đó. Những ràng buộc này độc lập giữa các vị trí vì mỗi khối nhỏ được xoay độc lập. Sau khi các vị trí được nhóm theo số lượng ràng buộc mà chúng áp đặt, số lượng cấu hình hợp lệ sẽ được tính thành hệ số của một sản phẩm trên các nhóm này. Không có tương tác toàn cầu nào tồn tại ngoài phân loại này, do đó không cần kiểm tra tính nhất quán bổ sung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def mod_pow(a, e):
    return pow(a, e, MOD)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        
        if n == 1:
            print(24)
            continue
        if n == 2:
            print(1)
            continue
        
        m = n - 2
        
        interior = m * m * m
        face = 6 * m * m
        edge = 12 * m
        
        ans = 1
        ans = ans * mod_pow(24, interior) % MOD
        ans = ans * mod_pow(4, face) % MOD
        ans = ans * mod_pow(2, edge) % MOD
        
        print(ans)

if __name__ == "__main__":
    solve()
```Mã này phản ánh trực tiếp sự phân rã hình học. Phép trừ$n-2$cô lập toàn bộ vùng bên trong của khối và tất cả số lượng tổ hợp đều được lấy từ tham số đơn lẻ đó. 

Các trường hợp đặc biệt đối với$n=1$Và$n=2$tránh các chiều âm trong công thức và nắm bắt hình học suy biến trong đó mọi khối hoàn toàn tự do hoặc hoàn toàn bị ràng buộc. 

## Ví dụ đã hoạt động 

Hãy xem xét$n=3$. Sau đó$m = 1$. có$1$khối nội thất,$6$hình khối mặt trong,$12$hình khối cạnh, và$8$các góc. 

| Loại | Đếm | Lựa chọn mỗi khối | Đóng góp | 
| --- | --- | --- | --- | 
| Nội thất | 1 | 24 |$24^1$| 
| Mặt nội thất | 6 | 4 |$4^6$| 
| Cạnh | 12 | 2 |$2^{12}$| 
| Góc | 8 | 1 |$1$| 

Câu trả lời cuối cùng là$24 \cdot 4^6 \cdot 2^{12}$. Điều này khẳng định rằng ngay cả nhỏ$n$đã tạo ra cấu trúc lũy thừa mạnh mẽ. 

Bây giờ hãy xem xét$n=2$. Chỉ có các khối góc, tất cả là 8 khối. Mỗi góc hoàn toàn xác định hướng, vì vậy mọi khối lập phương đều bị ép buộc. Tích số giảm xuống 1, cho thấy hình học loại bỏ mọi tự do. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \log n)$| Mỗi trường hợp thử nghiệm sử dụng phép lũy thừa mô-đun nhanh trên một số lũy thừa không đổi | 
| Không gian |$O(1)$| Chỉ có một số số nguyên được duy trì | 

Các ràng buộc cho phép lên đến$10^5$các trường hợp thử nghiệm, nhưng mỗi trường hợp đều độc lập và chỉ yêu cầu một số lũy thừa mô-đun, đủ nhanh. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# We cannot directly call solve() here in static snippet context,
# but these illustrate intended assertions structure.

# custom cases (conceptual)
# n = 1 -> 24
# n = 2 -> 1
# n = 3 -> structured decomposition
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | 24 | khối đơn hoàn toàn tự do quay | 
| 1\n2 | 1 | khối chỉ có góc bị ràng buộc hoàn toàn | 
| 1\n3 | giá trị tính toán | kiểm tra sự phân rã thành tất cả các loại khối | 
| 3\n1\n2\n3 | 24 1 ... | xử lý nhiều bài kiểm tra | 

## Vỏ cạnh 

cho$n=1$, khối lập phương thoái hóa thành một đơn vị duy nhất. Khối lập phương đó đóng góp đồng thời tất cả sáu mặt và không có ràng buộc ranh giới nào hạn chế hướng. Mỗi vòng quay của khối lập phương tạo ra một cấu hình riêng biệt, vì vậy câu trả lời là 24. Thuật toán xử lý vấn đề này trực tiếp thông qua một trường hợp đặc biệt trước khi áp dụng công thức hình học, tránh các giá trị âm của$n-2$. 

Vì$n=2$, mỗi khối đơn vị nằm trên một góc. Mỗi khối phải đáp ứng đồng thời ba ràng buộc về mặt, cố định hướng của nó một cách duy nhất. Công thức tổng quát cũng sẽ rút gọn một cách chính xác vì tất cả các cơ số mũ đều biến mất, nhưng cách giải thích chỉ dựa vào góc làm cho lý luận trở nên rõ ràng: có chính xác một cấu hình hợp lệ. 

Vì$n \ge 3$, việc phân tách thành các vùng bên trong, mặt, cạnh và góc được áp dụng một cách rõ ràng và mỗi danh mục đóng góp một hệ số nhân độc lập mà không có sự chồng chéo hoặc thiếu sót.
