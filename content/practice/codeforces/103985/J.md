---
title: "CF 103985J - \u041a\u043e\u043c\u043f\u0430\u043d\u0438\u044f \u0438 \u043f\u043e\u0431\u0438\u0442\u043e\u0432\u043e\u0435 \u0418"
description: "Chúng ta được cung cấp một danh sách các số nguyên không âm, mỗi số đại diện cho một giá trị được gán cho một người trong nhóm. Nhiệm vụ là tính toán thước đo toàn cầu về “sự gắn kết nhóm”, được định nghĩa là tổng của tất cả các cặp người không có thứ tự theo bit AND của các giá trị của họ."
date: "2026-07-02T06:15:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103985
codeforces_index: "J"
codeforces_contest_name: "\u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 (\u041c\u041a\u041e\u0428\u041f) 2017, \u041b\u0438\u0433\u0430 \u0410"
rating: 0
weight: 103985
solve_time_s: 39
verified: true
draft: false
---

[CF 103985J - \u041a\u043e\u043c\u043f\u0430\u043d\u0438\u044f \u0438 \u043f\u043e\u0431\u0438\u0442\u043e\u0432\u043e\u0435 \u0418](https://codeforces.com/problemset/problem/103985/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên không âm, mỗi số đại diện cho một giá trị được gán cho một người trong nhóm. Nhiệm vụ là tính toán thước đo toàn cầu về “sự gắn kết nhóm”, được định nghĩa là tổng của tất cả các cặp người không có thứ tự theo bit AND của các giá trị của họ. 

Cụ thể, với mỗi cặp chỉ số phân biệt$i < j$, chúng tôi lấy$a_i \& a_j$và chúng tôi tính tổng các giá trị này trên tất cả các cặp. Đầu ra là một số nguyên duy nhất. 

Sự ràng buộc về$n$đạt tới 150.000 và mỗi giá trị có thể lớn bằng$10^8$. Việc tính toán theo cặp trực tiếp sẽ yêu cầu khoảng$n^2 / 2$hoạt động, ở quy mô này là theo thứ tự$10^{10}$, vượt xa mọi giới hạn thời gian hợp lý. Điều này ngay lập tức loại trừ bất kỳ giải pháp bậc hai nào. 

Các giá trị được giới hạn bởi$10^8$, phù hợp với 27 bit. Điều đó có nghĩa là bất kỳ phương pháp phân tách theo bit nào cũng có thể lặp lại một cách an toàn trên tối đa 30 vị trí bit, khiến cho chiến lược đếm từng bit trở nên hợp lý. 

Một dạng thất bại tinh tế đối với các phương pháp tiếp cận đơn giản là tràn số nguyên hoặc tính toán lại cùng một phần đóng góp nhiều lần không chính xác. Một lỗi phổ biến khác là cố gắng tối ưu hóa bằng cách sắp xếp, điều này không bảo toàn được mối quan hệ theo cặp bit. 

Ví dụ: nếu tất cả các số đều giống hệt nhau, hãy nói$[7,7,7]$, mỗi cặp đóng góp$7$, vậy câu trả lời là$3 \cdot 7 = 21$. Bất kỳ cách tiếp cận không chính xác nào cố gắng “nén các bản sao” mà không tính đến số lượng cặp đều có thể dễ dàng tính sai bội số. 

## Phương pháp tiếp cận 

Giải pháp brute-force lặp lại trên tất cả các cặp$(i, j)$và tính toán$a_i \& a_j$. Điều này đơn giản và chính xác vì nó tuân theo định nghĩa của vấn đề. Tuy nhiên, chi phí của nó tăng theo bậc hai với$n$và với 150.000 phần tử, điều này dẫn đến hơn mười tỷ phép tính, điều này là không khả thi. 

Quan sát quan trọng là hoạt động bitwise AND độc lập giữa các bit. Một bit chỉ đóng góp vào tổng cuối cùng nếu nó được đặt ở cả hai số của một cặp. Điều này gợi ý đảo ngược quan điểm: thay vì lặp lại từng cặp, chúng tôi đếm các đóng góp từng chút một. 

Sửa một vị trí bit$b$. Chúng tôi đếm có bao nhiêu số có tập hợp bit này, chẳng hạn$c_b$. Bất kỳ cặp số nào như vậy đều đóng góp$2^b$đến tổng cuối cùng và số cặp như vậy là$\binom{c_b}{2}$. Tổng hợp tất cả các bit sẽ cho câu trả lời. 

Điều này chuyển đổi vấn đề từ việc liệt kê cặp thành việc đếm tần số trên các bit, làm giảm đáng kể độ phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Đếm bit |$O(n \log A)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng trong khi duy trì việc đếm xem có bao nhiêu số chứa mỗi bit. 

1. Khởi tạo một mảng`cnt`có kích thước 31 (đủ cho các giá trị lên tới$10^8$). Mỗi vị trí lưu trữ bao nhiêu số được nhìn thấy cho đến nay đã đặt bit đó. Cấu trúc này cho phép chúng tôi theo dõi tần số bit tăng dần. 
2. Lặp lại từng số$x$trong đầu vào. Đối với mỗi vị trí bit$b$từ 0 đến 30, kiểm tra xem bit$b$được thiết lập trong$x$. Nếu có thì tăng`cnt[b]`. Điều này đảm bảo rằng sau khi xử lý tất cả các số,`cnt[b]`bằng tổng số phần tử đóng góp bit đó. 
3. Sau khi đếm, tính toán câu trả lời bằng cách lặp lại tất cả các vị trí bit. Đối với mỗi bit$b$, nếu như`cnt[b] >= 2`, thì có$\binom{cnt[b]}{2}$cặp đóng góp bit này. 
4. Thêm$\binom{cnt[b]}{2} \cdot 2^b$đến câu trả lời cuối cùng. Điều này xây dựng lại tổng đóng góp của tất cả các cặp chia sẻ bit đó. 
5. Xuất số tiền tích lũy. 

Bước quan trọng là phân tách các đóng góp theo bit, giúp tránh việc tính hai lần và đảm bảo mỗi cặp được xem xét chính xác một lần trên mỗi bit. 

### Tại sao nó hoạt động 

Mỗi cặp$(i, j)$đóng góp độc lập cho từng bit trong đó cả hai số đều có số 1. Đối với một bit cố định, sự đóng góp chỉ phụ thuộc vào số lượng số chứa bit đó chứ không phụ thuộc vào danh tính của chúng. Do đó, việc đếm các cặp chỉ số trong mỗi nhóm bit sẽ tái tạo lại chính xác tổng mà không cần liệt kê các cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    cnt = [0] * 31
    
    for x in a:
        b = 0
        while x:
            if x & 1:
                cnt[b] += 1
            x >>= 1
            b += 1
    
    ans = 0
    for b in range(31):
        c = cnt[b]
        if c >= 2:
            ans += (c * (c - 1) // 2) * (1 << b)
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng quét bit trực tiếp trên mỗi số thay vì kiểm tra tất cả 31 bit bằng một vòng lặp cố định. Điều này giữ cho các hoạt động được chặt chẽ và tránh việc kiểm tra bit không cần thiết đối với các đuôi bằng 0. 

Sự tinh tế chính là đảm bảo mỗi vị trí bit được căn chỉnh chính xác với chỉ mục của nó trong`cnt`. Biến`b`theo dõi chỉ số bit khi chúng ta dịch chuyển số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
3 5 2 3
```Chúng tôi theo dõi số lượng bit: 

| Số | Nhị phân | Bit được thêm vào | 
| --- | --- | --- | 
| 3 | 011 | 0, 1 | 
| 5 | 101 | 0, 2 | 
| 2 | 010 | 1 | 
| 3 | 011 | 0, 1 | 

Tính toán cuối cùng: 

- bit 0: 3 số 
- bit 1: 3 số 
- bit 2: 1 số 

Bây giờ đóng góp: 

- bit 0: C(3,2) * 1 = 3 
- bit 1: C(3,2) * 2 = 6 
- bit 2: 0 

Tổng cộng = 9 

Dấu vết này xác nhận rằng mỗi bit được xử lý độc lập và các đóng góp của cặp được tích lũy chính xác. 

### Ví dụ 2 

đầu vào:```
3
4 4 4
```| Số | Nhị phân | Bit được thêm vào | 
| --- | --- | --- | 
| 4 | 100 | 2 | 
| 4 | 100 | 2 | 
| 4 | 100 | 2 | 

Đếm: 

- bit 2: 3 

Đóng góp: 

- C(3,2) * 4 = 3 * 4 = 12 

Tất cả các cặp đều giống nhau và mỗi cặp đóng góp 4, vì vậy 3 cặp mang lại 12. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot B)$| Mỗi số được xử lý tối đa 31 bit | 
| Không gian |$O(B)$| Chỉ một mảng cố định để đếm bit | 

Giá trị của$B = 31$là hằng số so với các ràng buộc, vì vậy giải pháp là tuyến tính một cách hiệu quả$n$. Điều này dễ dàng phù hợp trong vòng 4 giây cho 150.000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import comb
    
    n_and_rest = inp.strip().split()
    n = int(n_and_rest[0])
    a = list(map(int, n_and_rest[1:]))

    cnt = [0] * 31
    for x in a:
        b = 0
        while x:
            if x & 1:
                cnt[b] += 1
            x >>= 1
            b += 1

    ans = 0
    for b in range(31):
        c = cnt[b]
        if c >= 2:
            ans += (c * (c - 1) // 2) * (1 << b)

    return str(ans)

# sample
assert run("4\n3 5 2 3") == "9"

# all equal
assert run("3\n7 7 7") == str(3 * 7)

# single element
assert run("1\n5") == "0"

# no shared bits
assert run("3\n1 2 4") == "0"

# max-ish simple
assert run("5\n1 1 1 1 1") == str(10)

print("OK")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 3 5 2 3 | 9 | độ chính xác của mẫu | 
| 3 7 7 7 | 21 | xử lý trùng lặp | 
| 1 5 | 0 | trường hợp cạnh phần tử đơn | 
| 3 1 2 4 | 0 | bit rời rạc | 
| 5 1 1 1 1 1 | 10 | đếm tổ hợp | 

## Vỏ cạnh 

Đối với một yếu tố đầu vào như`1\n5`, thuật toán đặt số lượng bit cho số nhưng không bao giờ đạt đến số lượng ít nhất là hai. Mọi`cnt[b]`nhiều nhất là 1, do đó tất cả đóng góp nhị thức đều biến mất, tạo ra 0, phù hợp với thực tế là không có cặp nào tồn tại. 

Đối với các yếu tố giống hệt nhau như`3\n7 7 7`, tất cả các bit của 7 được đếm ba lần. Mỗi bit đóng góp$\binom{3}{2} \cdot 2^b$. Tính tổng các bit sẽ tái tạo lại chính xác ba bản sao của 7 trên ba cặp, khớp trực tiếp với định nghĩa theo cặp. 

Đối với các số có bộ bit rời rạc như`1 2 4`, mỗi bit xuất hiện đúng một số, nên tất cả`cnt[b] ≤ 1`, nghĩa là không có cặp nào chia sẻ bất kỳ bit nào. Thuật toán trả về chính xác 0, phù hợp với thực tế là mọi AND đều bằng 0 trên tất cả các cặp.
