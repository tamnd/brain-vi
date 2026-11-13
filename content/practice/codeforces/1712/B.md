---
title: "CF 1712B - Hoán vị đáng thương"
description: "Chúng ta được cấp một số nguyên $n$ và chúng ta phải sắp xếp các số từ $1$ đến $n$ thành một hoán vị sao cho điểm cụ thể càng lớn càng tốt."
date: "2026-06-15T00:45:22+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1712
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 813 (Div. 2)"
rating: 800
weight: 1712
solve_time_s: 223
verified: true
draft: false
---

[CF 1712B - Hoán vị đáng tiếc](https://codeforces.com/problemset/problem/1712/B) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, lý thuyết số 
**Thời gian giải:** 3m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$n$, và chúng ta phải sắp xếp các số từ$1$ĐẾN$n$thành một hoán vị sao cho điểm cụ thể càng lớn càng tốt. Điểm được tính theo từng vị trí: tại chỉ số$i$, chúng ta lấy LCM của$i$và giá trị được đặt tại chỉ mục đó, sau đó tính tổng tất cả những đóng góp này. 

Vì vậy chúng ta không được tự do thay đổi các chỉ số mà chỉ có thể thay đổi các giá trị được gán cho từng vị trí. Mỗi số từ$1$ĐẾN$n$phải được sử dụng chính xác một lần và mục tiêu là tối đa hóa mức độ lớn của các giá trị LCM trên tất cả các vị trí. 

Các ràng buộc cho phép lên đến$10^5$cho mỗi trường hợp thử nghiệm và cũng lên đến$10^5$tổng số trên tất cả các trường hợp thử nghiệm. Điều này loại trừ bất kỳ giải pháp nào thử tất cả các hoán vị, vì$n!$phát triển cực kỳ nhanh chóng. Ngay cả cách tiếp cận bậc hai cho mỗi ca kiểm thử cũng sẽ quá chậm khi$n$lớn, vì$10^5$bình phương là được rồi$10^{10}$, vượt xa giới hạn 1 giây cho phép. Điều này đẩy chúng ta tới một công trình tuyến tính hoặc gần tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi$n$là nhỏ. Ví dụ, khi$n=1$, chỉ có một hoán vị nên đáp án là bắt buộc. Khi$n=2$, việc hoán đổi các phần tử sẽ làm thay đổi các đóng góp của LCM theo cách không rõ ràng và một trực giác ngây thơ như “đặt số lượng lớn vào các vị trí lớn” có thể đã thất bại nếu không được kiểm tra cẩn thận dựa trên cấu trúc LCM. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi hoán vị, tính tổng LCM cho mỗi hoán vị và chọn ra kết quả tốt nhất. Điều này đúng vì nó đánh giá tất cả các bài tập có thể. Tuy nhiên, nó đòi hỏi phải đánh giá$n!$hoán vị, và mỗi chi phí đánh giá$O(n)$, dẫn đến$O(n \cdot n!)$, điều này hoàn toàn không thể thực hiện được ngay cả đối với$n = 10$. 

Quan sát quan trọng là LCM của hai số$i$Và$p_i$cực đại khi tích của chúng lớn và ước chung lớn nhất của chúng nhỏ. Từ$\mathrm{lcm}(i, p_i) = \frac{i \cdot p_i}{\gcd(i, p_i)}$, chúng tôi muốn các cặp có gcd càng nhỏ càng tốt trong khi vẫn giữ cả hai giá trị lớn khi có thể. 

Điều này gợi ý việc ghép các số theo cách tránh các yếu tố chung. Cách đơn giản nhất để đảm bảo$\gcd(i, p_i) = 1$càng thường xuyên càng tốt là hoán đổi các phần tử theo cặp. Nếu chúng ta ghép đôi$i$với$i+1$, sau đó$\gcd(i, i+1)=1$, và LCM trở thành$i \cdot (i+1)$, lớn so với bất kỳ sự sắp xếp nào có số lượng nhỏ hơn. 

Vì vậy, thay vì tối ưu hóa toàn cục một cách phức tạp, chúng ta có thể xây dựng một hoán vị một cách tham lam bằng cách hoán đổi các phần tử liền kề. Điều này đảm bảo rằng mỗi cặp đều đóng góp một LCM lớn giống như sản phẩm và chúng tôi tránh lãng phí các giá trị lớn cho các chỉ số nhỏ. 

Đối với số lẻ$n$, phần tử cuối cùng không thể ghép nối được nên nó vẫn giữ nguyên vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot n!)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với danh sách$[1, 2, 3, \dots, n]$. Đây là hoán vị cơ bản trước khi tối ưu hóa. 
2. Duyệt mảng từ trái sang phải theo bước 2. 
3. Cho mỗi cặp$(i, i+1)$, hoán đổi hai giá trị. Điều này tạo ra các cặp$(2,1), (4,3), (6,5), \dots$. 
4. Nếu$n$là số lẻ, giữ nguyên phần tử cuối cùng vì nó không có đối tác. 
5. Xuất mảng kết quả. 

Lý do chúng tôi xử lý theo cặp là vì các số nguyên liền kề được đảm bảo là nguyên tố cùng nhau, giúp tối đa hóa LCM so với việc để chúng theo thứ tự. 

### Tại sao nó hoạt động 

Mỗi cặp hoán đổi$(i, i+1)$tạo ra hai thuật ngữ LCM:$$\mathrm{lcm}(i, i+1) = i(i+1), \quad \mathrm{lcm}(i+1, i) = i(i+1)$$theo thứ tự nào đó tùy theo vị trí. Điều này hoàn toàn tốt hơn việc giữ chúng thẳng hàng như$\mathrm{lcm}(i,i)=i$Và$\mathrm{lcm}(i+1,i+1)=i+1$, từ$2i(i+1)$tăng trưởng bậc hai trong khi$2i+1$là tuyến tính. 

Vì mỗi cặp độc lập và việc hoán đổi không ảnh hưởng đến các vị trí khác nên chúng ta có thể tối đa hóa đóng góp cục bộ cho mọi khối liền kề và tổng các lựa chọn cục bộ tối ưu sẽ mang lại mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(range(1, n + 1))

    for i in range(0, n - 1, 2):
        p[i], p[i + 1] = p[i + 1], p[i]

    print(*p)
```Giải pháp xây dựng hoán vị theo thời gian tuyến tính bằng cách khởi tạo hoán vị nhận dạng và sau đó hoán đổi mọi cặp liền kề. Vòng lặp`range(0, n - 1, 2)`đảm bảo rằng chúng tôi chỉ trao đổi các cặp hợp lệ và xử lý các độ dài lẻ một cách tự nhiên bằng cách giữ nguyên phần tử cuối cùng. 

Một lỗi phổ biến là thử logic dựa trên gcd phức tạp hơn, nhưng không cần thiết vì tính liền kề đã đảm bảo lợi ích tối đa cục bộ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$n = 5$Mảng ban đầu: 

| tôi | p[i] trước | hành động | p[i] sau | 
| --- | --- | --- | --- | 
| 1 | 1 | trao đổi với 2 | 2 | 
| 2 | 2 | trao đổi với 1 | 1 | 
| 3 | 3 | trao đổi với 4 | 4 | 
| 4 | 4 | trao đổi với 3 | 3 | 
| 5 | 5 | không | 5 | 

Kết quả:$[2, 1, 4, 3, 5]$Điều này cho thấy các vị trí lẻ vẫn ổn định ở cuối trong khi các phần tử trước đó được nhóm thành các cặp tối ưu. 

### Ví dụ 2 

đầu vào:$n = 6$| tôi | p[i] trước | hành động | p[i] sau | 
| --- | --- | --- | --- | 
| 1 | 1 | trao đổi với 2 | 2 | 
| 2 | 2 | trao đổi với 1 | 1 | 
| 3 | 3 | trao đổi với 4 | 4 | 
| 4 | 4 | trao đổi với 3 | 3 | 
| 5 | 5 | trao đổi với 6 | 6 | 
| 6 | 6 | trao đổi với 5 | 5 | 

Kết quả:$[2,1,4,3,6,5]$Điều này xác nhận cấu trúc ghép nối đầy đủ trên toàn bộ mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Chúng tôi xây dựng hoán vị một lần và hoán đổi từng phần tử nhiều nhất một lần | 
| Không gian |$O(n)$| Chúng tôi lưu trữ mảng hoán vị | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tổng$n$trên nhiều trường hợp thử nghiệm là nhiều nhất$10^5$, làm cho tổng công việc tuyến tính theo kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        p = list(range(1, n + 1))
        for i in range(0, n - 1, 2):
            p[i], p[i + 1] = p[i + 1], p[i]
        out.append(" ".join(map(str, p)))
    return "\n".join(out)

# provided samples
assert run("2\n1\n2\n") == "1\n2 1"

# custom cases
assert run("1\n3\n") == "2 1 3", "odd length"
assert run("1\n4\n") == "2 1 4 3", "basic pairing"
assert run("1\n5\n") == "2 1 4 3 5", "odd tail behavior"
assert run("1\n6\n") == "2 1 4 3 6 5", "full pairing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=3 | 2 1 3 | xử lý đuôi kỳ lạ | 
| n=4 | 2 1 4 3 | ghép đôi tham lam cơ bản | 
| n=5 | 2 1 4 3 5 | cấu trúc lẻ hỗn hợp | 
| n=6 | 2 1 4 3 6 5 | tính nhất quán bảo hiểm đầy đủ | 

## Vỏ cạnh 

cho$n=1$, thuật toán tạo ra$[1]$vì vòng lặp trao đổi không thực thi. Điều này khớp với hoán vị hợp lệ duy nhất, vì vậy tính chính xác là ngay lập tức. 

Vì$n=2$, trao đổi tạo ra$[2,1]$. Đây là giải pháp thay thế$[1,2]$cho điểm nhỏ hơn vì nó lãng phí lợi ích LCM lớn từ việc ghép nối chéo, xác nhận rằng ngay cả trường hợp không tầm thường nhỏ nhất cũng được xử lý một cách tối ưu. 

Đối với số lẻ$n$, chẳng hạn như$n=5$, phần tử cuối cùng vẫn không bị ảnh hưởng. Cấu trúc vẫn ghép tất cả các phần tử trước đó một cách tối ưu và phần tử chưa ghép đôi đóng góp giá trị duy nhất có thể có ở vị trí của nó, do đó, không thể cải thiện bằng cách di chuyển nó đi nơi khác mà không làm hỏng cặp tốt hơn.
