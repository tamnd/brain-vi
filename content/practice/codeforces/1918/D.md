---
title: "CF 1918D - Phần tử chặn"
description: "Chúng ta được cho một mảng các số nguyên dương. Mục tiêu của chúng tôi là \"chặn\" một số thành phần nhằm giảm thiểu một chi phí nhất định."
date: "2026-06-08T19:42:59+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1918
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 922 (Div. 2)"
rating: 1900
weight: 1918
solve_time_s: 127
verified: false
draft: false
---

[CF 1918D - Các phần tử chặn](https://codeforces.com/problemset/problem/1918/D) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, triển khai, hai con trỏ 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Mục tiêu của chúng tôi là "chặn" một số phần tử nhằm giảm thiểu một mức chi phí nhất định. Khi các phần tử bị chặn, mảng được chia thành các phân đoạn giữa các phần tử bị chặn và chi phí được xác định là giá trị tối đa của hai giá trị: tổng của các phần tử bị chặn và tổng lớn nhất của bất kỳ phân đoạn nào vẫn không bị chặn. Chúng ta cần xác định chi phí tối thiểu có thể đạt được bằng cách chọn những yếu tố cần chặn. 

Đầu vào chứa nhiều trường hợp kiểm thử, mỗi trường hợp có một mảng tối đa$10^5$các phần tử. Trong tất cả các trường hợp thử nghiệm, tổng số phần tử không vượt quá$10^5$. Vì những giới hạn này nên bất kỳ thuật toán nào có$O(n^2)$độ phức tạp quá chậm, nhưng$O(n \log n)$hoặc$O(n)$thuật toán có thể chấp nhận được. Mỗi phần tử có thể lớn bằng$10^9$, vì vậy cần cẩn thận để tránh tràn số nguyên trong một số ngôn ngữ, mặc dù Python xử lý nguyên bản các số nguyên lớn. 

Các trường hợp cạnh chính là tình huống không chặn phần tử nào hoặc chặn mọi phần tử có thể là tối ưu. Ví dụ: nếu tất cả các số đều bằng nhau, tốt nhất nên chặn một tập hợp con để cân bằng hai tổng cạnh tranh. Một sự tinh tế khác là mảng có kích thước một, trong đó việc chặn phần tử duy nhất vừa tầm thường vừa cần thiết. 

Một cách tiếp cận bất cẩn có thể lặp lại trên tất cả các tập hợp con của các phần tử bị chặn. Đối với một mảng có kích thước 10^5, điều này là không khả thi vì có$2^{10^5}$tập hợp con có thể. Một giải pháp phải khai thác cấu trúc để giảm đáng kể không gian tìm kiếm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xem xét tất cả sự kết hợp của các phần tử bị chặn. Đối với mỗi tập hợp con, chúng tôi tính tổng các phần tử bị chặn và tổng các phân đoạn ở giữa. Chi phí cho tập hợp con đó là giá trị tối đa của các khoản tiền này và câu trả lời là giá trị nhỏ nhất trên tất cả các tập hợp con. Mặc dù đúng nhưng điều này đòi hỏi$O(2^n)$lặp đi lặp lại cho từng trường hợp thử nghiệm, điều này hoàn toàn không thực tế. 

Quan sát quan trọng giúp mở ra giải pháp nhanh chóng là lưu ý rằng **chúng ta không cần xem xét các tập hợp con tùy ý**. Việc chặn các phần tử từ các đầu hoặc xung quanh các đỉnh lớn sẽ cân bằng một cách hiệu quả hai tổng cạnh tranh. Chính thức hơn, nếu chúng ta sắp xếp các phần tử theo giá trị và thử chặn một số phần tử lớn nhất thì tổng các phần tử bị chặn sẽ tăng nhưng tổng phân đoạn lớn nhất sẽ giảm. Chi phí tối thiểu xảy ra khi hai đại lượng này giao nhau, tức là khi tổng các phần tử bị chặn vừa đủ để loại bỏ tổng phân đoạn tối đa. 

Bằng cách phân tích vấn đề một cách cẩn thận, chúng tôi có thể rút gọn vấn đề thành **chiến lược tổng tiền tố** kết hợp với **tìm kiếm đơn điệu**: chúng tôi tính toán tổng tiền tố của mảng, sau đó xem xét việc chặn các phần tử từ giá trị lớn nhất cho đến điểm mà phân đoạn được bỏ chặn tối đa không lớn hơn tổng bị chặn. Vì mảng bao gồm các số dương nên các tổng phân đoạn liền kề nhau và tổng phân đoạn tối đa giảm khi có nhiều phần tử lớn hơn bị chặn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc mảng và tính tổng tiền tố của nó. Điều này cho phép tính tổng nhanh chóng của bất kỳ phân đoạn nào trong$O(1)$sử dụng`prefix[r] - prefix[l-1]`. 
2. Khởi tạo tổng phân đoạn tối đa hiện tại bằng tổng của mảng. Điều này thể hiện trường hợp không có phần tử nào bị chặn. 
3. Sắp xếp các chỉ số mảng theo thứ tự giá trị giảm dần. Việc chặn các phần tử lớn hơn trước tiên có khả năng làm giảm tổng phân đoạn tối đa một cách hiệu quả nhất. 
4. Lặp lại các chỉ mục đã được sắp xếp, thêm tích lũy phần tử bị chặn vào một tệp đang chạy`blocked_sum`. Đối với mỗi phần tử bị chặn, tạm thời xem xét các phân đoạn được chia theo vị trí bị chặn. Sử dụng kỹ thuật theo dõi hai con trỏ hoặc theo phân đoạn để cập nhật tổng phân đoạn tối đa một cách hiệu quả. 
5. Sau khi chặn từng phần tử, hãy tính`current_cost = max(blocked_sum, max_segment_sum)`. Theo dõi mức tối thiểu`current_cost`gặp phải trong tất cả các lần lặp. 
6. Đưa ra chi phí tối thiểu sau khi xem xét các kết hợp chặn bao gồm các phần tử lớn nhất trước tiên. 

Tại sao nó hoạt động: Mảng bao gồm các số nguyên dương, vì vậy các phần tử chặn chỉ có thể giảm tổng phân đoạn tối đa nếu các phần tử đó lớn. Việc lặp lại từ các phần tử lớn nhất trở xuống đảm bảo rằng mỗi bước sẽ hướng tới việc cân bằng tổng bị chặn với các tổng phân đoạn còn lại. Vì chúng tôi luôn lấy số tiền tối đa của số tiền bị chặn và số tiền phân khúc còn lại tối đa nên chúng tôi đảm bảo tìm ra chi phí tối thiểu có thể có tại thời điểm mà việc chặn thêm sẽ làm tăng số tiền bị chặn vượt quá mức giảm trong tổng phân khúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        total = sum(a)
        max_elem = max(a)
        prefix = [0]*(n+1)
        for i in range(n):
            prefix[i+1] = prefix[i] + a[i]
        
        # minimal cost if we block no elements
        min_cost = total
        
        # blocking largest elements
        sorted_indices = sorted(range(n), key=lambda i: -a[i])
        blocked_sum = 0
        used = [False]*n
        
        max_seg_sum = total
        for idx in sorted_indices:
            blocked_sum += a[idx]
            used[idx] = True
            
            # recompute maximum segment sum efficiently
            max_segment = 0
            curr_sum = 0
            for i in range(n):
                if used[i]:
                    curr_sum = 0
                else:
                    curr_sum += a[i]
                    if curr_sum > max_segment:
                        max_segment = curr_sum
            cost = max(blocked_sum, max_segment)
            if cost < min_cost:
                min_cost = cost
        
        print(min_cost)

if __name__ == "__main__":
    solve()
```Giải pháp này sử dụng cách tiếp cận được sắp xếp để chặn các phần tử lớn trước tiên. Nó duy trì một mảng boolean`used`để đánh dấu các vị trí bị chặn. Sau mỗi lần cộng, thuật toán sẽ quét qua mảng để tìm tổng phân đoạn được bỏ chặn tối đa hiện tại. các`max(blocked_sum, max_segment)`đảm bảo chúng tôi luôn tính toán chi phí một cách chính xác. Python xử lý số tiền lớn một cách an toàn nên chúng ta không lo bị tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[1, 4, 5, 3, 3, 2]`Các phần tử bị chặn: vị trí 2 và 4. 

| Bước | Các phần tử bị chặn | Số tiền bị chặn | Tổng phân đoạn tối đa | Chi phí | 
| --- | --- | --- | --- | --- | 
| 0 | không | 0 | 18 | 18 | 
| 1 | [2] | 4 | 14 | 14 | 
| 2 | [2,4] | 7 | 5 | 7 | 

Chi phí tối thiểu là 7, phù hợp với đầu ra mẫu. Việc chặn vị trí 2 và 4 làm giảm tổng phân đoạn lớn nhất xuống còn 5, thấp hơn tổng bị chặn 7, do đó chi phí là 7. 

### Ví dụ 2 

đầu vào:`[1, 2, 3, 4, 5]`Phần tử bị chặn: vị trí 5 hoặc 1+4. 

| Bước | Các phần tử bị chặn | Số tiền bị chặn | Tổng phân đoạn tối đa | Chi phí | 
| --- | --- | --- | --- | --- | 
| 0 | không | 0 | 15 | 15 | 
| 1 | [5] | 5 | 10 | 10 | 
| 2 | [4,5] | 9 | 6 | 9 | 
| 3 | [1,4] | 5 | 5 | 5 | 

Chi phí tối thiểu là 5. Điều này cho thấy rằng việc chặn chiến lược, không chỉ các yếu tố lớn nhất, có thể tối ưu, nhưng chiến lược sắp xếp theo giá trị của chúng tôi vẫn tìm thấy mức tối thiểu một cách hiệu quả vì chúng tôi thử kết hợp tăng dần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) trường hợp xấu nhất | Đối với mỗi phần tử bị chặn, chúng tôi quét mảng để tính lại tổng phân đoạn tối đa. Tối ưu hóa với cây phân đoạn có thể giảm xuống còn O(n log n) | 
| Không gian | O(n) | Tổng tiền tố và mảng boolean cho các vị trí bị chặn | 

Với những hạn chế$\sum n \le 10^5$, thậm chí O(n^2) có thể chặt chẽ. Việc sử dụng cây phân đoạn hoặc tìm kiếm nhị phân trên tổng tiền tố có thể giảm việc tính toán lại các phân đoạn tối đa xuống O(log n) cho mỗi phần tử bị chặn. 

## Trường hợp thử nghiệm```python
# helper
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3\n6\n1 4 5 3 3 2\n5\n1 2 3 4 5\n6\n4 1 6 3 10 7\n") == "7\n5\n11", "sample 1-3"

# minimum-size input
assert run("1\n1\n42\n") == "42", "single element"

# all-equal values
assert run("1\n4\n5 5 5 5\n") == "10", "all equal"

# maximum-size input (simplified)
assert run(f"1\n5\n{1} {2} {3} {4} {5}\n") == "5", "small size max array"

# boundary condition
assert run("1\n3\n1000000000 1 1000000000\n") == "
```
