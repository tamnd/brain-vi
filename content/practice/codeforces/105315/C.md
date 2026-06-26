---
title: "CF 105315C - Sinh Nhật Iyaad"
description: "Chúng tôi được đưa ra một số thí nghiệm độc lập. Trong mỗi thử nghiệm, có một số nguyên tham chiếu $D$ đại diện cho DNA của một con mèo cụ thể và danh sách các con mèo khác được biểu thị bằng số nguyên $ai$. Mỗi số nguyên mã hóa các đặc điểm di truyền dưới dạng bit."
date: "2026-06-23T15:05:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105315
codeforces_index: "C"
codeforces_contest_name: "JPC 4.0"
rating: 0
weight: 105315
solve_time_s: 48
verified: true
draft: false
---

[CF 105315C - Sinh nhật của Iyaad](https://codeforces.com/problemset/problem/105315/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số thí nghiệm độc lập. Trong mỗi thí nghiệm có một số nguyên tham chiếu$D$đại diện cho DNA của một con mèo cụ thể và danh sách những con mèo khác được biểu thị bằng số nguyên$a_i$. Mỗi số nguyên mã hóa các đặc điểm di truyền dưới dạng bit. 

Một con mèo được coi là có liên quan đến con mèo tham chiếu nếu tồn tại ít nhất một vị trí bit trong đó cả hai$a_i$Và$D$có 1. Theo thuật ngữ bitwise, điều này có nghĩa là AND bitwise của hai số khác 0. Nhiệm vụ chỉ đơn giản là đếm xem có bao nhiêu giá trị trong mỗi trường hợp thử nghiệm thỏa mãn điều kiện này. 

Mỗi trường hợp thử nghiệm có thể chứa tối đa$2 \cdot 10^5$số lượng và trên tất cả các trường hợp thử nghiệm lên đến$10^6$. Điều này ngay lập tức loại trừ bất cứ điều gì bậc hai trong$n$, vì ngay cả việc quét tuyến tính cho mỗi trường hợp thử nghiệm cũng đã là cấu trúc khả thi duy nhất. Hoạt động chúng tôi thực hiện cho mỗi phần tử phải có thời gian không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi$a_i = 0$. Từ$0 \& D = 0$, những phần tử như vậy không bao giờ được tính, bất kể$D$. Một trường hợp cạnh khác xảy ra khi$D = 0$. Trong trường hợp đó, không có số nào có thể chia sẻ một bit đã đặt với$D$, vì vậy câu trả lời luôn bằng 0, ngay cả khi tất cả$a_i$là khác không. Những trường hợp này rất dễ xử lý sai nếu người ta cho rằng mọi số dương đều tự động chia sẻ một chút. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra từng con mèo. Đối với mỗi$a_i$, tính toán$a_i \& D$và tăng bộ đếm nếu kết quả khác 0. Điều này đúng vì định nghĩa về sự liên quan chính xác là điều kiện đó. 

Chi phí của phương pháp này là tuyến tính theo tổng số số nguyên trên tất cả các trường hợp thử nghiệm. Mỗi bitwise AND là thời gian không đổi trên các số nguyên 32 bit hoặc 64 bit, do đó độ phức tạp tổng cộng là$O(\sum n)$, nhiều nhất là$10^6$. Điều này nhanh chóng một cách thoải mái trong giới hạn một giây trong Python nếu được triển khai với đầu vào nhanh. 

Không có cấu trúc sâu hơn để khai thác vì mỗi phần tử đều độc lập. Bất kỳ nỗ lực nào để xử lý trước hoặc nhóm các giá trị đều không làm giảm công việc hơn nữa, vì điều kiện phụ thuộc vào mặt nạ bit cố định$D$áp dụng riêng cho từng giá trị. Nhận thức chính là vấn đề đã ở mức tối thiểu: đó là bộ lọc phát trực tuyến theo điều kiện bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mỗi phần tử VÀ kiểm tra) | O(n) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng, I/O nhanh) | O(tổng n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. Mục tiêu trong mỗi trường hợp là đếm xem có bao nhiêu giá trị chia sẻ ít nhất một bit được đặt với$D$. 

1. Đọc$n$Và$D$cho trường hợp thử nghiệm. Điều này xác định bitmask cố định mà chúng tôi sẽ so sánh. 
2. Khởi tạo bộ đếm về 0. Điều này sẽ tích lũy tất cả những con mèo đủ điều kiện. 
3. Lặp lại qua từng giá trị$a_i$trong danh sách. 
4. Tính toán$a_i \& D$. Nếu kết quả khác 0 thì tăng bộ đếm. Kiểm tra này trực tiếp mã hóa xem hai mặt nạ bit có giao nhau hay không. 
5. Sau khi xử lý tất cả các giá trị, xuất bộ đếm. 

Mỗi bước đều cần thiết vì không có cách nào để suy ra kết quả của phần tử này từ phần tử khác; điều kiện hoàn toàn là cục bộ đối với mỗi cặp$(a_i, D)$. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là bitwise AND cô lập các bit tập hợp được chia sẻ. Nếu hai số có chung ít nhất một vị trí bit trong đó cả hai đều có số 1 thì AND của chúng phải khác 0. Ngược lại, nếu AND bằng 0 thì không có vị trí bit nào được chia sẻ, nghĩa là chúng không liên quan đến định nghĩa bài toán. Vì chúng tôi kiểm tra mọi phần tử chính xác một lần và điều kiện vừa cần vừa đủ nên số đếm cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, D = map(int, input().split())
        arr = list(map(int, input().split()))
        
        ans = 0
        for x in arr:
            if x & D:
                ans += 1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc như một vòng lặp phát trực tuyến đơn giản qua các trường hợp thử nghiệm. Nhập nhanh qua`sys.stdin.readline`là cần thiết vì tổng số số nguyên có thể lên tới một triệu. biểu hiện`x & D`là hoạt động cốt lõi; nó mã hóa trực tiếp điều kiện mối quan hệ mà không cần tính toán thêm. 

Không cần xử lý trước hoặc lưu trữ bổ sung ngoài trường hợp thử nghiệm hiện tại, điều này giúp duy trì mức sử dụng bộ nhớ không đổi so với kích thước đầu vào. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
1
4 12
1 6 8 5
```Đây$D = 12$, ở dạng nhị phân là`1100`. 

| tôi | a_i | a_i (nhị phân) | a_i & D | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0001 | 0000 | 0 | 
| 2 | 6 | 0110 | 0100 | 1 | 
| 3 | 8 | 1000 | 1000 | 2 | 
| 4 | 5 | 0101 | 0100 | 3 | 

Câu trả lời cuối cùng là 3. Điều này cho thấy rằng ngay cả việc chồng chéo một phần các bit cũng đủ chứ không phải ngăn chặn hoàn toàn. 

Bây giờ hãy xem xét trường hợp thứ hai:```
1
3 0
7 1 15
```| tôi | a_i | a_i & D | Đếm | 
| --- | --- | --- | --- | 
| 1 | 7 | 0 | 0 | 
| 2 | 1 | 0 | 0 | 
| 3 | 15 | 0 | 0 | 

Từ$D = 0$, không có số nào có thể chia sẻ một bit cố định với nó. Câu trả lời luôn là số không, mặc dù tất cả$a_i$là tích cực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(∑n) | Mỗi phần tử được kiểm tra một lần bằng thao tác bitwise theo thời gian không đổi | 
| Không gian | O(1) | Chỉ một bộ đếm được duy trì cho mỗi trường hợp thử nghiệm | 

Tổng giới hạn kích thước đầu vào của$10^6$số nguyên phù hợp dễ dàng trong phương pháp quét tuyến tính này. Mỗi hoạt động là một bit AND thân thiện với CPU, do đó giải pháp chạy thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import sys as _sys

    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue()

def solve():
    t = int(input())
    for _ in range(t):
        n, D = map(int, input().split())
        arr = list(map(int, input().split()))
        ans = 0
        for x in arr:
            if x & D:
                ans += 1
        print(ans)

# provided samples
assert run("1\n4 12\n1 6 8 5\n") == "3\n"
assert run("1\n3 0\n7 1 15\n") == "0\n"

# custom cases
assert run("1\n1 8\n0\n") == "0\n"
assert run("1\n5 7\n1 2 4 8 15\n") == "4\n"
assert run("1\n3 1\n2 4 8\n") == "0\n"
assert run("2\n2 3\n1 2\n3 4\n1 2 3\n") == "2\n0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số 0 đơn với khác 0 D | 0 | đảm bảo giá trị 0 không bao giờ được tính | 
| bit hỗn hợp với mặt nạ nhỏ | 4 | kiểm tra nhiều kết quả trùng lặp | 
| không có trường hợp chồng chéo | 0 | đảm bảo xử lý chính xác khi tất cả kết quả AND bằng 0 | 
| nhiều trường hợp thử nghiệm | 2 0 | xác minh tính độc lập của mỗi trường hợp thử nghiệm | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$D = 0$. Đối với đầu vào:```
1
4 0
1 2 3 4
```Thuật toán tính toán$x \& 0 = 0$cho mọi phần tử. Vòng lặp vẫn chạy trên tất cả các phần tử nhưng bộ đếm không bao giờ tăng. Đầu ra là 0, phù hợp với định nghĩa vì không thể có sự trùng lặp bit. 

Một trường hợp khác là khi tất cả$a_i = 0$:```
1
3 5
0 0 0
```Mỗi lần lặp đánh giá`0 & 5 = 0`, do đó bộ đếm vẫn bằng 0 trong suốt quá trình. Thuật toán xử lý việc này một cách tự nhiên mà không cần sử dụng cách viết đặc biệt. 

Một trường hợp tế nhị cuối cùng là khi$D$chỉ có một tập hợp bit, ví dụ$D = 1$. Chỉ tính số lẻ:```
1
5 1
2 3 4 5 6
```Thuật toán chỉ đếm chính xác 3 và 5, vì chúng chỉ có tập bit có ý nghĩa nhỏ nhất.
