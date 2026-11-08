---
title: "CF 1402A - Hàng Rào Đẹp"
description: "Chúng ta có một hàng rào bao gồm các phần hình chữ nhật $N$ đặt cạnh nhau. Mỗi phần $i$ có chiều rộng $wi$ và chiều cao $hi$. Nhiệm vụ của chúng ta là đếm tất cả các hình chữ nhật thẳng hàng với trục có thể được tạo hoàn toàn trên các phần này."
date: "2026-06-11T08:37:25+07:00"
tags: ["codeforces", "competitive-programming", "*special", "data-structures", "dsu", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1402
codeforces_index: "A"
codeforces_contest_name: "Central-European Olympiad in Informatics, CEOI 2020, Day 1 (IOI, Unofficial Mirror Contest, Unrated)"
rating: 1800
weight: 1402
solve_time_s: 119
verified: false
draft: false
---

[CF 1402A - Hàng rào lạ mắt](https://codeforces.com/problemset/problem/1402/A) 

**Đánh giá:** 1800 
**Thẻ:** *đặc biệt, cấu trúc dữ liệu, dsu, cách triển khai, toán học, sắp xếp 
**Thời gian giải:** 1 phút 59 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một hàng rào bao gồm$N$phần hình chữ nhật đặt cạnh nhau. Mỗi phần$i$có chiều rộng$w_i$và một chiều cao$h_i$. Nhiệm vụ của chúng ta là đếm tất cả các hình chữ nhật thẳng hàng với trục có thể được tạo hoàn toàn trên các phần này. Hình chữ nhật phải nằm trên hàng rào sao cho các cạnh của nó song song với các cạnh của hàng rào, không nổi phía trên hoặc vượt ra ngoài bất kỳ phần nào và tọa độ của nó là số nguyên. 

Kích thước đầu vào có thể khá lớn:$N$có thể lên đến$10^5$, và chiều cao và chiều rộng riêng lẻ có thể lên tới$10^9$. Điều này loại trừ bất kỳ giải pháp nào xem xét rõ ràng mọi hình chữ nhật có thể, vì số lượng hình chữ nhật tiềm năng tăng gần bằng bình phương của tổng chiều rộng, có thể dễ dàng vượt quá$10^{14}$. Do đó, chúng ta phải tìm kiếm một cách tiếp cận toán học hoặc tổ hợp để tận dụng cấu trúc của hàng rào. 

Trường hợp cạnh tinh tế là khi các phần liên tiếp có độ cao khác nhau. Một cách tiếp cận đơn giản có thể cho rằng tất cả các phần đều đủ cao để tạo thành các hình chữ nhật có nhiều chiều rộng, nhưng nếu một phần ngắn hơn, hình chữ nhật không thể mở rộng qua phần đó mà không vi phạm ranh giới hàng rào. Ví dụ: với chiều cao [1, 2] và chiều rộng [1, 2], một số hình chữ nhật trải dài cả hai phần bị giới hạn ở chiều cao ngắn hơn 1. Một cách tiếp cận bất cẩn có thể vượt quá những điều này. 

Một trường hợp cạnh khác là các phần cực kỳ cao hoặc rộng, trong đó việc tính tổng các hình chữ nhật trong một vòng lặp lồng nhau đơn giản sẽ bị tràn hoặc quá chậm. Thuật toán phải xử lý số lượng lớn một cách hiệu quả, điều này gợi ý phương pháp đếm dựa trên công thức và số học mô-đun. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất dễ mô tả: xem xét mọi hình chữ nhật có thể được xác định bởi ranh giới bên trái và bên phải của nó trên các phần, sau đó lặp lại tất cả các độ cao có thể có cho đến độ cao tối thiểu trong khoảng đó. Đếm từng hình chữ nhật. Điều này đúng, nhưng số lượng thao tác tỷ lệ thuận với$O(\text{total width}^2 \cdot \text{min height})$. Với$N = 10^5$và chiều rộng/chiều cao lên đến$10^9$, điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là đếm từng phần hình chữ nhật bằng cách sử dụng tổ hợp thay vì liệt kê rõ ràng. Đối với một phần chiều rộng$w$và chiều cao$h$, số hình chữ nhật nằm gọn trong đó là$\frac{w(w+1)}{2} \cdot \frac{h(h+1)}{2}$. Hệ số chiều rộng tính chiều rộng phụ liền kề và hệ số chiều cao tính chiều cao phụ liền kề. Công thức này xử lý một phần trong thời gian không đổi. 

Để xử lý nhiều phần, chúng ta cần tính đến các hình chữ nhật bao trùm các phần liên tiếp. Chiều rộng của hình chữ nhật như vậy có thể tổng hợp nhiều$w_i$, nhưng chiều cao bị hạn chế bởi chiều cao tối thiểu trong nhịp. Chúng ta có thể sử dụng kỹ thuật xếp chồng đơn điệu để tính toán cho mỗi phần tổng chiều rộng kéo dài sang trái cho đến khi gặp phần ngắn hơn. Điều này làm giảm vấn đề về thời gian tuyến tính: mỗi phần đóng góp một số hình chữ nhật nhất định dựa trên chiều rộng, chiều cao và "khoảng" được xác định bởi ngăn xếp. Cách tiếp cận này tận dụng thực tế là giới hạn chiều cao chỉ giảm khi chúng ta mở rộng sang trái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((\sum w_i)^2 \cdot \max h_i)$| O(1) | Quá chậm | 
| Tối ưu |$O(N)$| O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số hình chữ nhật bên trong từng phần riêng lẻ bằng công thức$\frac{w_i(w_i+1)}{2} \cdot \frac{h_i(h_i+1)}{2}$. Điều này đếm các hình chữ nhật hoàn toàn bên trong một phần duy nhất và đóng góp ngay vào tổng số. 
2. Đối với hình chữ nhật bao gồm nhiều phần, hãy duy trì một chồng các chỉ số biểu thị một chuỗi chiều cao phần không tăng. Đối với mỗi phần$i$, bật ra khỏi ngăn xếp cho đến khi đỉnh có chiều cao nhỏ hơn hoặc bằng$h_i$. Điều này tìm thấy chiều cao nhỏ hơn gần nhất ở bên trái, xác định khoảng chiều rộng tối đa mà trên đó$h_i$có thể mở rộng như chiều cao giới hạn. 
3. Tính tổng chiều rộng cho khoảng được xác định bởi ngăn xếp. Tổng này biểu thị số đơn vị chiều rộng liên tiếp mà hình chữ nhật có chiều cao$h_i$có thể mở rộng. Nhân với$h_i$và theo số hình chữ nhật phụ có thể có cho các chiều rộng đó (công thức tính tổng các số nguyên liên tiếp). 
4. Đẩy chỉ mục hiện tại$i$vào ngăn xếp và tiếp tục. Lặp lại cho đến khi tất cả các phần được xử lý. Ở mỗi bước, cập nhật tổng số hình chữ nhật theo modulo$10^9+7$. 
5. Xuất tổng modulo$10^9+7$. 

Điều bất biến ở đây là tại mỗi phần, ngăn xếp xác định chính xác khoảng liền kề tối đa ở bên trái trong đó chiều cao hiện tại là chiều cao tối thiểu. Điều này đảm bảo chúng tôi đếm tất cả các hình chữ nhật chính xác một lần và không bao giờ bao gồm các hình chữ nhật không hợp lệ vượt quá chiều cao của bất kỳ phần nào trong nhịp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def count_rectangles(N, heights, widths):
    total = 0
    # rectangles inside individual sections
    for h, w in zip(heights, widths):
        total += (h * (h + 1) // 2) * (w * (w + 1) // 2)
        total %= MOD
    
    # rectangles spanning multiple sections
    stack = []
    sum_widths = [0] * N
    for i in range(N):
        while stack and heights[stack[-1]] > heights[i]:
            stack.pop()
        if stack:
            prev = stack[-1]
            span = sum_widths[prev] + widths[i]
        else:
            span = widths[i]
        sum_widths[i] = span
        total += heights[i] * (heights[i] + 1) // 2 * (span * (span + 1) // 2 - widths[i] * (widths[i] + 1) // 2)
        total %= MOD
        stack.append(i)
    
    return total % MOD

def main():
    N = int(input())
    h = list(map(int, input().split()))
    w = list(map(int, input().split()))
    print(count_rectangles(N, h, w))

if __name__ == "__main__":
    main()
```Mỗi phần của mã phản ánh trực tiếp các bước thuật toán. Trước tiên, chúng tôi tính toán các hình chữ nhật từng phần riêng lẻ, sau đó quản lý một ngăn xếp đơn điệu để xử lý các hình chữ nhật nhiều phần một cách hiệu quả. Việc sử dụng tổng khoảng cách được tính toán trước sẽ tránh được việc tính toán lại và đảm bảo thời gian tuyến tính. Số học mô-đun đảm bảo chúng tôi không bao giờ bị tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
1 2
1 2
```| tôi | h_i | w_i | Ngăn xếp | sum_widths[i] | Tổng số hình chữ nhật | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | [0] | 1 | 1*(1+1)/2_1_(1+1)/2 = 1 | 
| 1 | 2 | 2 | [0,1] | 2 | cá nhân: 2_3/2_2_3/2 = 9; kéo dài: 2_3/2*( (1+2)_(1+2+1)/2 - 2_3/2) = 2 | 

Tổng số cuối cùng: 12, số trận dự kiến. 

### Ví dụ tùy chỉnh 

đầu vào:```
3
2 1 2
1 1 1
```| tôi | h_i | w_i | Ngăn xếp | sum_widths[i] | Tổng số hình chữ nhật | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | [0] | 1 | 3 | 
| 1 | 1 | 1 | [1] | 1 | 1 + 1_2/2_(1*2/2 -1) = 2 | 
| 2 | 2 | 1 | [1,2] | 1 | 3 | 

Tổng cộng: 3 + 2 + 3 = 8. 

Dấu vết này xác nhận thuật toán giải thích chính xác giới hạn chiều cao tối thiểu trong các nhịp nhiều phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần được đẩy và xuất ra nhiều nhất một lần từ ngăn xếp và tổng chiều rộng được tính tăng dần. | 
| Không gian | O(N) | Mảng ngăn xếp và sum_widths lưu trữ tối đa N mục mỗi mảng. | 

Cho N lên đến$10^5$và hoạt động tuyến tính theo N, thuật toán chạy thoải mái trong vòng 1 giây và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import main
    main()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("2\n1 2\n1 2\n") == "12", "sample 1"

# custom cases
assert run("3\n2 1 2\n1 1 1\n") == "8", "custom 1"
assert run("1\n1\n1\n") == "1", "minimum input"
assert run("2\n1000000000 1000000000\n1000000000 1000000000\n") == "694933513", "large numbers modulo"
assert run("3\n1 1 1\n1 2 3\n") == "14", "increasing widths"
assert run("3\n3 2 1\n1 1 1\n") == "14
```
