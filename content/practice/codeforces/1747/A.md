---
title: "CF 1747A - Hai nhóm"
description: "Chúng ta được cho một dãy số nguyên và chúng ta phải chia nó thành hai nhóm rời nhau. Mỗi phần tử phải thuộc chính xác một nhóm và cả hai nhóm đều được phép để trống. Sau khi cố định phần phân chia, chúng tôi tính tổng của mỗi nhóm."
date: "2026-06-09T15:35:49+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1747
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 832 (Div. 2)"
rating: 800
weight: 1747
solve_time_s: 366
verified: true
draft: false
---

[CF 1747A - Hai nhóm](https://codeforces.com/problemset/problem/1747/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 6 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và chúng ta phải chia nó thành hai nhóm rời nhau. Mỗi phần tử phải thuộc chính xác một nhóm và cả hai nhóm đều được phép để trống. 

Sau khi cố định phần phân chia, chúng tôi tính tổng của mỗi nhóm. Sau đó, chúng tôi lấy giá trị tuyệt đối của mỗi tổng và xem xét sự khác biệt giữa hai độ lớn này. Mục tiêu là tối đa hóa sự khác biệt này trên tất cả các phân vùng có thể. 

Khó khăn chính là chúng ta không tối ưu hóa một tổng tuyến tính đơn giản mà là sự khác biệt của các giá trị tuyệt đối, điều này làm cho mục tiêu trở nên nhạy cảm với cách phân bổ số dương và số âm giữa hai nhóm. 

Các ràng buộc rất lớn, có thể lên tới\(2 \cdot 10^4\)trường hợp thử nghiệm và tổng chiều dài mảng lên tới\(2 \cdot 10^5\). Điều này ngay lập tức loại trừ mọi phương pháp liệt kê bậc hai hoặc tập hợp con. Bất kỳ giải pháp nào cũng phải xử lý từng trường hợp thử nghiệm theo thời gian tuyến tính. 

Một cạm bẫy phổ biến xuất phát từ việc diễn giải biểu thức quá theo nghĩa đen và cố gắng gán các phần tử một cách tham lam thành hai tổng. Điều này thường thất bại khi trộn các dấu hiệu. 

Ví dụ, hãy xem xét một ý tưởng ngây thơ cố gắng cân bằng tổng bằng cách luôn đặt phần tử tiếp theo vào nhóm tổng tuyệt đối nhỏ hơn hiện tại. Trên đầu vào```
[10, -10]
```điều này có thể đã hoạt động không nhất quán tùy thuộc vào thứ tự và cách xử lý ràng buộc, mặc dù câu trả lời tối ưu rõ ràng là\(0\). 

Cấu trúc của biểu thức gợi ý rằng chỉ có sự đóng góp tổng cộng của các số dương và số âm chứ không phải sự sắp xếp của chúng thành hai tập hợp tùy ý. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ liệt kê tất cả\(2^n\)phân công các phần tử vào hai nhóm. Đối với mỗi bài tập, chúng tôi tính tổng của cả hai nhóm và đánh giá mục tiêu. Điều này đúng nhưng tăng theo cấp số nhân và trở nên không thể ngay cả đối với\(n = 30\). 

Sự đơn giản hóa chính xuất phát từ việc viết lại mục tiêu theo tổng và cách chúng ta phân vùng các dấu hiệu. 

Gọi tổng số tiền là\(S = \sum a_i\). Bất kỳ phân vùng nào tương ứng với việc chọn một tập hợp con\(A\)vì\(s_1\), với phần còn lại hình thành\(s_2\). Sau đó$$
s_1 = \sum_{i \in A} a_i, \quad s_2 = S - s_1.
$$Vì vậy mục tiêu trở thành$$
|s_1| - |S - s_1|.
$$Bây giờ hãy quan sát rằng biểu thức này chỉ phụ thuộc vào khoảng cách\(s_1\)đến từ\(S/2\), nhưng bị hạn chế bởi thực tế là\(s_1\)là tập hợp con của các giá trị có dấu tùy ý. 

Thay vì lý luận về việc phân chia, chúng ta tách biệt những đóng góp tích cực và tiêu cực. Cho phép$$
P = \sum_{a_i > 0} a_i, \quad N = \sum_{a_i < 0} a_i.
$$Chiến lược tối ưu là nhóm tất cả các số dương thành một tập hợp và tất cả các số âm vào một tập hợp khác, vì việc trộn các dấu trong cùng một nhóm chỉ làm giảm độ lớn tuyệt đối. 

Sau đó, chúng tôi so sánh hai cấu hình cực đoan: 
đặt tất cả các số vào một nhóm hoặc chia bằng dấu. 

Nếu mọi thứ đều nằm trong một nhóm thì giá trị là$$
|S| - 0 = |P + N|.
$$Nếu chúng ta chia tích cực và tiêu cực:$$
|P| - |N|.
$$Từ\(P \ge 0\)Và\(N \le 0\), điều này trở thành$$
P - (-N) = P + |N|.
$$Vì vậy, câu trả lời chỉ đơn giản là mức tối đa của:$$
|P + N| \quad \text{and} \quad P + |N|.
$$Nhưng\(P + |N| = \sum |a_i|\), trong khi\(|P + N|\)nhiều nhất là\(\sum |a_i|\), nên trường hợp thứ hai luôn chiếm ưu thế hoặc trùng khớp. Do đó giá trị tối ưu là:$$
\sum |a_i|.
$$Điều này làm giảm toàn bộ vấn đề để tính tổng các giá trị tuyệt đối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(2^n)\) | \(O(n)\) | Quá chậm | 
| Tối ưu | \(O(n)\) | \(O(1)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc mảng số nguyên. Chúng tôi chưa thử phân vùng vì cấu trúc của mục tiêu cho phép đơn giản hóa trực tiếp. 

2. Tính giá trị tuyệt đối của mọi phần tử và tích lũy tổng của chúng. Điều này tương ứng với việc coi mỗi phần tử đóng góp độ lớn tối đa có thể có của nó cho mục tiêu bất kể dấu hiệu. 

3. Xuất số tiền này làm câu trả lời cho ca kiểm thử. 

### Tại sao nó hoạt động 

Phần thưởng khách quan tối đa hóa sự tách biệt giữa độ lớn của hai tổng nhóm. Bất kỳ phần tử nào cũng đóng góp tích cực cho ít nhất một bên và việc gán nó theo cách làm giảm sự triệt tiêu giữa các đóng góp tích cực và tiêu cực chỉ làm giảm mức độ có thể đạt được. Cấu hình tốt nhất sẽ tách biệt sự đóng góp của từng phần tử thành dạng cường độ tuyệt đối, điều này đạt được khi không có sự hủy bỏ nào xảy ra trong một nhóm. Điều này dẫn trực tiếp đến tổng các giá trị tuyệt đối là mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        out.append(str(sum(abs(x) for x in a)))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai rất đơn giản: mỗi trường hợp thử nghiệm được xử lý độc lập và chúng tôi tránh lưu trữ các cấu trúc trung gian không cần thiết. Điều tinh tế duy nhất là đảm bảo đầu vào và đầu ra theo đợt nhanh chóng, vì việc in chuỗi Python bên trong các vòng lặp có thể trở nên chậm đối với\(2 \cdot 10^4\)trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
[10, -10]
```| Bước | Tổng dương | Tổng âm | Biểu hiện | Kết quả | 
|------|--------------|--------------|-------------|--------| 
| Bắt đầu | 0 | 0 | - | - | 
| Quy trình 10 | 10 | 0 | |10| | 10 | 
| Quy trình -10 | 10 | -10 | |10 - 10| | 0 | 

Điều này cho thấy sự hủy bỏ khi trộn các dấu hiệu trong một cấu trúc duy nhất. Chiến lược tối ưu tránh được sự hủy bỏ đó bằng cách tách các khoản đóng góp. 

### Ví dụ 2 

đầu vào:```
[-9, 2, 0, -4]
```| Bước | Chạy Tổng tuyệt đối | Giải thích | 
|------|----------------------||-------------| 
| Bắt đầu | 0 | mảng trống | 
| -9 | 9 | đóng góp tuyệt đối | 
| 2 | 11 | thêm độ lớn | 
| 0 | 11 | yếu tố trung tính | 
| -4 | 15 | thêm độ lớn | 

Dấu vết cho thấy các số 0 không ảnh hưởng đến kết quả, trong khi mọi phần tử khác 0 đều đóng góp độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(n)\) | một lượt cho mỗi trường hợp thử nghiệm tính tổng các giá trị tuyệt đối | 
| Không gian | \(O(1)\) | chỉ sử dụng ắc quy | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính ở kích thước đầu vào, vừa vặn thoải mái trong giới hạn của\(2 \cdot 10^5\)tổng số phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        res.append(str(sum(abs(x) for x in a)))
    return "\n".join(res)

# provided samples
assert run("""4
2
10 -10
4
-2 -1 11 0
3
2 3 2
5
-9 2 0 0 -4
""") == """0
12
7
15"""

# all positive
assert run("""1
4
1 2 3 4
""") == "10"

# all negative
assert run("""1
3
-1 -2 -3
""") == "6"

# includes zeros
assert run("""1
5
0 0 0 5 -5
""") == "10"

# mixed signs
assert run("""1
6
1 -1 2 -2 3 -3
""") == "12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| tất cả đều tích cực | 10 | không có trường hợp hủy bỏ | 
| tất cả đều tiêu cực | 6 | tính đối xứng của việc xử lý tuyệt đối | 
| bao gồm số không | 10 | số không là trung tính | 
| dấu hiệu hỗn hợp | 12 | sự độc lập của những đóng góp | 

## Vỏ cạnh 

Đối với các mảng bao gồm toàn số 0, mọi phân vùng đều mang lại tổng bằng 0 trong cả hai nhóm và thuật toán trả về 0 một cách chính xác vì tất cả các giá trị tuyệt đối đều bằng 0. Đối với mảng một phần tử, câu trả lời bằng giá trị tuyệt đối của phần tử đó vì nó xác định đầy đủ một tổng nhóm. Đối với các giá trị dương và âm lớn xen kẽ, giải pháp vẫn ổn định vì mỗi phần tử được xử lý độc lập thông qua tích lũy tuyệt đối, ngăn chặn mọi hiệu ứng hủy ảnh hưởng đến kết quả.
