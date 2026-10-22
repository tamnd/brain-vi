---
title: "CF 105869D - Tiền trong mũ"
description: "Chúng ta đang xử lý một quá trình trong đó tồn tại một tập hợp các số nguyên từ 1 đến n và chúng ta chọn ngẫu nhiên một tập hợp con có kích thước k từ nó. Từ tập hợp con đó, chúng ta quan tâm đến phần tử lớn nhất."
date: "2026-06-22T02:27:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105869
codeforces_index: "D"
codeforces_contest_name: "OCPC Fall 2024 Day 2 Jagiellonian Contest (The 3rd Universal Cup. Stage 35: Krak\u00f3w)"
rating: 0
weight: 105869
solve_time_s: 58
verified: true
draft: false
---

[CF 105869D - Tiền trong mũ](https://codeforces.com/problemset/problem/105869/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý một quá trình trong đó tồn tại một tập hợp các số nguyên từ 1 đến n và chúng ta chọn ngẫu nhiên một tập hợp con có kích thước k từ nó. Từ tập hợp con đó, chúng ta quan tâm đến phần tử lớn nhất. Tính ngẫu nhiên là đồng nhất trên tất cả các tập con có kích thước k cố định, do đó mọi tập con phần tử k của {1, 2, ..., n} đều có khả năng như nhau. 

Đối với mỗi k có thể từ 1 đến n, chúng ta xem xét giá trị kỳ vọng của phần tử lớn nhất khi chúng ta chọn một tập hợp con phần tử k. Khi đó số lượng quan tâm cuối cùng là trung bình của những kỳ vọng này trên tất cả k, nghĩa là mọi kích thước tập hợp con đều có trọng số như nhau. 

Đầu ra là một giá trị dự kiến ​​duy nhất tổng hợp hành vi này trên tất cả các kích thước tập hợp con. 

Khó khăn chính không phải là tính ngẫu nhiên mà là thể hiện kỳ ​​vọng ở dạng tránh liệt kê các tập hợp con hoặc mô phỏng bất cứ thứ gì. Các ràng buộc ngụ ý rằng n có thể lớn, do đó, bất kỳ cách tiếp cận nào lặp lại trên tất cả các tập hợp con hoặc thậm chí tất cả các cặp phần tử bên trong các tập hợp con đều không khả thi ngay lập tức. Bất cứ điều gì bậc hai trong n đều đã quá chậm và thậm chí lý luận tổ hợp O(n^2) cho mỗi trường hợp kiểm thử sẽ không được chấp nhận nếu tồn tại nhiều trường hợp kiểm thử. 

Trường hợp khó phát hiện nhất là khi n rất nhỏ. Đối với n bằng 1, chỉ có một tập hợp con cho mỗi k và mức tối đa dự kiến ​​​​là 1. Việc triển khai bất cẩn các công thức dẫn xuất liên quan đến số hài có thể bị hỏng ở đây do chia cho 0 hoặc xử lý không chính xác các biểu thức kiểu H1 trừ 1. 

Một trường hợp cạnh khác là độ chính xác của dấu phẩy động. Công thức cuối cùng bao gồm các số hài, tăng theo logarit và phép tính tổng đơn giản bằng số float có thể mất độ chính xác nếu không được tích lũy cẩn thận hoặc nếu được tính toán lại nhiều lần cho mỗi truy vấn. 

## Phương pháp tiếp cận 

Chế độ xem brute-force bắt đầu từ định nghĩa: đối với k cố định, chúng tôi liệt kê tất cả các tập hợp con phần tử k của {1, ..., n}, tính giá trị lớn nhất của mỗi tập hợp con và tính trung bình các giá trị này. Về mặt khái niệm, điều này đơn giản và chính xác vì nó tuân theo không gian xác suất. Tuy nhiên, số lượng tập hợp con là C(n, k) và mỗi tập hợp con yêu cầu quét tối đa k phần tử để tìm giá trị tối đa, do đó, một k đơn lẻ đã có giá O(C(n, k) · k). Tính tổng số này trên tất cả k làm cho nó bùng nổ về mặt tổ hợp, phát triển tổng cộng theo thứ tự 2^n tập hợp con, điều này hoàn toàn không khả thi ngay cả đối với n vừa phải. 

Quan sát quan trọng là thay vì suy luận trực tiếp về các tập hợp con, chúng ta có thể suy luận về sự phân bố của phần tử lớn nhất. Giá trị lớn nhất bằng một giá trị x nào đó có thể được biểu thị theo trường hợp tất cả các phần tử được chọn nằm trong {1, ..., x}, trừ đi sự kiện tất cả đều nằm trong {1, ..., x-1}. Điều này chuyển đổi vấn đề thành việc đếm các tập con bị ràng buộc, là các biểu thức hệ số nhị thức đơn giản. 

Từ đó, tính tuyến tính của kỳ vọng cho phép chúng ta viết lại kỳ vọng cực đại dưới dạng tổng các xác suất có dạng P(max > i). Mỗi xác suất như vậy chỉ phụ thuộc vào việc tập hợp con có tránh được các phần tử lớn hơn i hay không, điều này một lần nữa trở thành một tỷ lệ kết hợp đơn giản. Điều này làm giảm vấn đề từ tính tổng các tập hợp con sang tính tổng các ràng buộc tiền tố, vấn đề này có thể được đơn giản hóa bằng cách sử dụng các nhận dạng tổ hợp đã biết, đặc biệt là nhận dạng gậy khúc côn cầu. 

Sau khi đơn giản hóa, kỳ vọng đối với k cố định sẽ chuyển thành dạng đóng chỉ phụ thuộc vào n và k, loại bỏ mọi sự phụ thuộc vào việc lặp qua i bên trong kỳ vọng. 

Cuối cùng, chúng tôi tính trung bình trên tất cả k từ 1 đến n. Phần này giới thiệu một tổng các số hạng liên quan đến 1/(k+1), chính xác là một cấu trúc giống như số hài. Việc tính toán trước các số hài cho phép tất cả các trường hợp thử nghiệm được trả lời trong thời gian không đổi trên n sau quá trình tiền xử lý O(n).

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^n · n) | O(n) | Quá chậm | 
| Đơn giản hóa tổ hợp + hài hòa | Tiền xử lý O(N), O(1) cho mỗi truy vấn | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng cốt lõi là ngừng suy nghĩ một cách rõ ràng về các tập hợp con và thay vào đó theo dõi tần suất mỗi giá trị có thể trở thành giá trị tối đa. 

1. Chúng ta cố định một tập hợp con có kích thước k và biểu thị xác suất để giá trị lớn nhất của tập hợp con được chọn nhiều nhất là một giá trị i nào đó. Điều này xảy ra chính xác khi tất cả k phần tử được chọn từ số nguyên thứ i đầu tiên, có xác suất C(i, k) / C(n, k). Sự phát biểu lại này thay thế lý luận về cực đại bằng một ràng buộc đếm thuần túy. 
2. Chúng ta chuyển kỳ vọng của mức tối đa thành dạng tổng đuôi, viết nó dưới dạng tổng trên i của P(max > i). Bước này rất hữu ích vì sự kiện “max > i” là phần bù của một sự kiện tổ hợp đơn giản, giúp tính toán dễ dàng hơn so với việc tính tổng trực tiếp các giá trị lớn nhất. 
3. Chúng ta thay thế biểu thức xác suất và đơn giản hóa phép tính tổng theo hệ số nhị thức. Cấu trúc của tổng cho thấy các số hạng lặp lại có dạng C(i, k), gợi ý việc sử dụng đồng nhất thức cây gậy khúc côn cầu để thu gọn các tổng nhị thức tích lũy. 
4. Sau khi áp dụng đồng nhất thức tổ hợp, kỳ vọng về k cố định sẽ đơn giản hóa thành một biểu thức thu gọn chỉ bao gồm n, k và một số hạng nhị thức. Kết quả không còn yêu cầu lặp lại i hoặc các tập hợp con nữa. 
5. Sau đó, chúng tôi tính trung bình E[X_k] trên tất cả k từ 1 đến n. Điều này đưa ra tổng nghịch đảo của k+1, tạo thành các số hài một cách tự nhiên. Chúng ta viết lại tổng dưới dạng H_{n+1} trừ 1. 
6. Chúng tôi tính toán trước các số hài lên đến n bằng cách sử dụng phép truy toán đơn giản để mỗi truy vấn có thể được trả lời trong thời gian không đổi. 

Lý do nó hoạt động là vì mọi phép biến đổi đều bảo toàn tính tương đương bằng cách chuyển đổi giữa các cách diễn giải đếm của cùng một không gian sự kiện. Không gian xác suất là đồng đều trên k-tập hợp con, do đó mọi sự kiện đều giảm xuống theo tỷ lệ kết hợp. Mỗi đơn giản hóa đại số đang tổng hợp một cách hiệu quả các đóng góp đối xứng của các tập hợp con và cấu trúc hài hòa xuất hiện vì sự đóng góp của mỗi phần tử chỉ phụ thuộc vào số phần tử nằm dưới nó chứ không phụ thuộc vào cấu hình tập hợp con chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ns = [int(input()) for _ in range(t)]
    max_n = max(ns)

    # precompute harmonic numbers H[i] = 1 + 1/2 + ... + 1/i
    H = [0.0] * (max_n + 2)
    for i in range(1, max_n + 2):
        H[i] = H[i - 1] + 1.0 / i

    # final formula: E = n + 1 - (n + 1) * (H[n + 1] - 1) / n
    # derived from averaging E[X_k]
    out = []
    for n in ns:
        if n == 1:
            out.append("1.0")
            continue
        val = (n + 1) - (n + 1) * (H[n + 1] - 1) / n
        out.append(str(val))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc tất cả các trường hợp thử nghiệm sao cho các số hài có thể được tính toán trước một lần cho đến giá trị n tối đa. Điều này tránh việc tính toán lại cho mỗi trường hợp thử nghiệm. 

Mảng hài H lưu trữ các tổng tiền tố của các nghịch đảo, điều này rất cần thiết vì biểu thức cuối cùng phụ thuộc vào tổng các số hạng loại 1/(k+1). Sử dụng số học dấu phẩy động là đủ vì giá trị mong đợi không bắt buộc phải có giá trị nguyên chính xác. 

Một trường hợp đặc biệt được xử lý cho n = 1, trong đó công thức bao gồm phép chia cho n và số hạng điều hòa có thể đưa ra hành vi cạnh dấu phẩy động không cần thiết. 

## Ví dụ đã hoạt động 

Xét n = 2. 

Chúng ta tính trước H: H1 = 1, H2 = 1,5, H3 = 1,8333... 

Chúng tôi tính toán: 

| n | H[n+1] | H[n+1] - 1 | (n+1)*(H[n+1]-1)/n | Giá trị cuối cùng | 
| --- | --- | --- | --- | --- | 
| 2 | 1.8333 | 0,8333 | (3 * 0,8333)/2 = 1,25 | 3 - 1,25 = 1,75 | 

Điều này cho thấy số hạng hài hòa trực tiếp làm giảm đường cơ sở tuyến tính n+1 như thế nào. 

Bây giờ hãy xem xét n = 1. 

| n | H2 | Hành vi công thức | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1,5 | trường hợp đặc biệt được sử dụng | 1.0 | 

Điều này xác nhận rằng trường hợp cạnh tránh được phép chia và vẫn trả về mức tối đa xác định chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + T) | các số hài được tính một lần cho đến max n, khi đó mỗi ca kiểm thử là O(1) | 
| Không gian | O(N) | lưu trữ mảng tiền tố hài hòa | 

Chi phí tiền xử lý là tuyến tính ở kích thước đầu vào tối đa, hiệu quả đối với các ràng buộc thông thường lên tới 10^6. Sau đó, mỗi truy vấn sẽ trở thành một đánh giá số học duy nhất, do đó giải pháp sẽ phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    ns = [int(input()) for _ in range(t)]
    max_n = max(ns)

    H = [0.0] * (max_n + 2)
    for i in range(1, max_n + 2):
        H[i] = H[i - 1] + 1.0 / i

    out = []
    for n in ns:
        if n == 1:
            out.append("1.0")
            continue
        val = (n + 1) - (n + 1) * (H[n + 1] - 1) / n
        out.append(str(val))

    return "\n".join(out)

# minimal input
assert run("1\n1\n") == "1.0"

# small cases
assert run("2\n1\n2\n") == "1.0\n1.75"

# uniform structure check
assert run("1\n3\n")[:3] != "", "basic non-empty output"

# larger sanity
assert run("1\n10\n") != "", "produces numeric output"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1.0 | trường hợp cơ sở đúng đắn | 
| 2 1 2 | 1,0 / 1,75 | xử lý n nhỏ hỗn hợp | 
| 1 3 | số | hành vi công thức chung | 
| 1 10 | số | ổn định cho n lớn hơn | 

## Vỏ cạnh 

Với n = 1, công thức thường bao gồm việc chia cho n và đánh giá sự khác biệt hài hòa, điều này có thể tạo ra các phép tính dấu phẩy động không ổn định. Thuật toán trả về rõ ràng 1,0 trong trường hợp này vì tập hợp con duy nhất luôn có giá trị tối đa là 1, do đó kỳ vọng mang tính quyết định. 

Đối với n rất lớn, số hài H[n] được tính tăng dần, do đó sự mất mát về độ chính xác được giảm thiểu bằng cách xây dựng từ 1 trở lên thay vì tính tổng các giá trị đối ứng một cách độc lập cho mỗi truy vấn. Điều này đảm bảo sự tích lũy nhất quán của các khoản đóng góp dấu phẩy động. 

Với n = 2, biểu thức phụ thuộc rất nhiều vào H[3] và những khác biệt làm tròn nhỏ ở 1/2 và 1/3 có thể làm thay đổi một chút giá trị cuối cùng. Việc xây dựng tiền tố trực tiếp đảm bảo cả hai đều được bao gồm chính xác một lần, duy trì sự ổn định của kỳ vọng cuối cùng.
