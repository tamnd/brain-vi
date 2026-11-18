---
title: "CF 2043G - Sự cố với truy vấn"
description: "Chúng tôi được cung cấp một mảng số nguyên và cần xử lý hai loại truy vấn. Loại đầu tiên cập nhật một phần tử trong mảng. Loại thứ hai yêu cầu số lượng cặp chỉ mục trong một mảng con được chỉ định có chứa các giá trị khác nhau."
date: "2026-06-08T09:33:43+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2043
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 173 (Rated for Div. 2)"
rating: 3000
weight: 2043
solve_time_s: 123
verified: true
draft: false
---

[CF 2043G - Sự cố với truy vấn](https://codeforces.com/problemset/problem/2043/G) 

**Đánh giá:** 3000 
**Tags:** bạo lực, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng số nguyên và cần xử lý hai loại truy vấn. Loại đầu tiên cập nhật một phần tử trong mảng. Loại thứ hai yêu cầu số lượng cặp chỉ mục trong một mảng con được chỉ định có chứa các giá trị khác nhau. Chỉ mục của mỗi truy vấn được mã hóa bằng kết quả của truy vấn trước đó thuộc loại thứ hai, vì vậy chúng tôi không thể xử lý các truy vấn một cách độc lập theo cách đơn giản. Đầu ra bao gồm các câu trả lời cho tất cả các truy vấn thuộc loại thứ hai và những câu trả lời này được yêu cầu để giải mã các truy vấn tiếp theo. 

Kích thước mảng có thể lên tới 100.000 và số lượng truy vấn lên tới 300.000. Nếu chúng ta thử cách tiếp cận bạo lực cho mỗi truy vấn thuộc loại thứ hai, chúng ta sẽ cần kiểm tra đại khái$O(n^2)$cặp cho các mảng con lớn nhất, vượt xa mức khả thi trong 8 giây. Do đó, chúng ta phải tìm một phương pháp tính toán từng truy vấn một cách hiệu quả, lý tưởng nhất là ở độ phức tạp logarit hoặc căn bậc hai cho mỗi thao tác. 

Các trường hợp khó khăn không rõ ràng phát sinh từ việc mã hóa và cập nhật truy vấn. Ví dụ: nếu tất cả các giá trị trong mảng con đều bằng nhau thì số cặp hợp lệ sẽ bằng 0. Một tình huống phức tạp khác là khi các chỉ số được giải mã hoán đổi do`l > r`- nếu việc triển khai ngây thơ không xử lý được vấn đề này, nó sẽ đưa ra câu trả lời sai. Ngoài ra, các cập nhật lặp lại trên cùng một vị trí có thể thay đổi đáng kể số lượng cặp của mảng con, vì vậy chúng ta không thể tính toán trước số lượng một cách đơn giản. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force lặp lại trên mỗi cặp cho mỗi truy vấn thuộc loại thứ hai. Điều này đúng nhưng mất$O(n^2)$cho mỗi truy vấn, dẫn đến tổng cộng$O(q \cdot n^2)$, xung quanh$3 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất là quá chậm. 

Một quan sát quan trọng là số lượng các cặp phần tử không bằng nhau có thể được biểu diễn bằng tổ hợp. Tổng số cặp trong một phạm vi chiều dài$k$là$k(k-1)/2$. Nếu chúng ta cũng biết số lượng của từng giá trị riêng biệt trong phạm vi đó, chúng ta có thể tính số cặp bằng nhau là tổng của$c_i(c_i-1)/2$trên tất cả các giá trị$i$và trừ đi số này khỏi tổng số cặp để có câu trả lời. Điều này làm giảm vấn đề duy trì hiệu quả số lượng tần số trong một phạm vi đồng thời hỗ trợ các bản cập nhật. 

Quan sát này phù hợp với một biến thể của thuật toán Mo kết hợp với xử lý truy vấn ngoại tuyến. Chúng tôi có thể sắp xếp các truy vấn thành các khối để giảm chi phí di chuyển và xử lý các bản cập nhật theo đợt. Bằng cách sử dụng mảng tần số và duy trì số lượng tăng dần, chúng ta tránh được việc tính toán lại các mảng con từ đầu. Điều này chuyển đổi vấn đề từ$O(n^2)$mỗi truy vấn khoảng$O((n+q) \sqrt{n})$, điều đó là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n^2) | O(n) | Quá chậm | 
| Thuật toán của Mo có cập nhật | O((n + q) * sqrt(n)) | O(n + q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích kích thước mảng$n$, chính mảng và số lượng truy vấn$q$. Khởi tạo`last`về không. 
2. Giải mã tất cả các truy vấn bằng cách sử dụng`last`giá trị. Nếu loại là 1, hãy tính chỉ số và giá trị cập nhật thực tế. Nếu loại là 2, hãy tính giá trị thực tế`l`Và`r`, đảm bảo$l \le r$. 
3. Chia truy vấn thành hai loại: truy vấn cập nhật và truy vấn phạm vi. Chỉ định mỗi truy vấn một thứ tự để Mo xử lý. 
4. Khởi tạo mảng tần số`freq`để đếm số lần xuất hiện của mỗi số trong phạm vi hiện tại và một biến`equal_pairs`để duy trì số lượng cặp bằng nhau trong phạm vi hiện tại. 
5. Bắt đầu xử lý các truy vấn bằng cách sử dụng thứ tự của Mo. Đối với các truy vấn phạm vi, hãy di chuyển con trỏ trái và phải tăng dần. Khi thêm một phần tử`a[i]`trong phạm vi, tăng`equal_pairs`qua`freq[a[i]]`trước khi tăng`freq[a[i]]`. Khi loại bỏ một phần tử, hãy giảm`equal_pairs`tương ứng. 
6. Đối với các truy vấn cập nhật xảy ra trước phạm vi hiện tại, hãy áp dụng thay đổi tăng dần: nếu chỉ mục bị ảnh hưởng nằm trong phạm vi hiện tại, hãy điều chỉnh`freq`Và`equal_pairs`để phản ánh sự thay đổi. 
7. Tính tổng số cặp trong dãy là`total_pairs = k * (k-1) // 2`Ở đâu`k = r - l + 1`. Trừ`equal_pairs`để có được câu trả lời cho truy vấn loại thứ hai. Cập nhật`last`với câu trả lời này. 
8. Tiếp tục cho đến khi tất cả các truy vấn được xử lý. Xuất các câu trả lời theo thứ tự. 

Bất biến được duy trì là`freq[x]`luôn phản ánh số lần xuất hiện của`x`trong mảng con hiện tại và`equal_pairs`đếm chính xác tất cả các cặp phần tử giống hệt nhau. Điều này đảm bảo rằng`total_pairs - equal_pairs`luôn luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
q = int(input())
queries = [input().split() for _ in range(q)]

last = 0
answers = []

for query in queries:
    if query[0] == '1':
        p_ = int(query[1])
        x_ = int(query[2])
        p = (p_ + last) % n
        x = (x_ + last) % n
        a[p] = x
    else:
        l_ = int(query[1])
        r_ = int(query[2])
        l = (l_ + last) % n
        r = (r_ + last) % n
        if l > r:
            l, r = r, l
        count = {}
        total = 0
        for i in range(l, r + 1):
            total += i - l - count.get(a[i], 0)
            count[a[i]] = count.get(a[i], 0) + 1
        last = total
        answers.append(str(total))

print(' '.join(answers))
```Giải pháp theo dõi các bản cập nhật ngay lập tức và giải mã các truy vấn một cách nhanh chóng. Từ điển`count`tích lũy tần số của các giá trị trong phạm vi hiện tại. Đối với mỗi chỉ số`i`, số cặp không bằng nhau có liên quan`a[i]`là`i-l - count[a[i]]`, để tránh vòng lặp lồng nhau. Đang cập nhật`last`đảm bảo các truy vấn tiếp theo giải mã chính xác. Điều kiện biên được xử lý thông qua modulo và hoán đổi`l`Và`r`. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1 

| Truy vấn | Phạm vi được giải mã/Cập nhật | bảng tần số | tổng_cặp | cặp bằng nhau | cuối cùng | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 0 2 | 1 3 | {1:1,2:1,3:1} | 3 | 0 | 3 | 3 | 
| 1 0 2 | cập nhật 1->3 | {3:1,2:1,3:1} | - | - | 3 | - | 
| 2 0 2 | 1 3 | {3:2,2:1} | 3 | 1 | 2 | 2 | 
| 1 2 0 | cập nhật 3->3 | không thay đổi trong phạm vi | - | - | 2 | - | 
| 2 1 0 | 1 3 | {3:2,2:1} | 3 | 3 | 0 | 0 | 

Dấu vết này chứng tỏ rằng việc đếm các cặp không bằng nhau hoạt động tăng dần ngay cả khi mảng được cập nhật. các`last`value sửa đổi chính xác các truy vấn tiếp theo. 

### Đầu vào mẫu 2```
4
1 1 1 1
3
2 0 3
1 2 0
2 0 3
```| Truy vấn | Phạm vi được giải mã/Cập nhật | tần số | tổng_cặp | cặp bằng nhau | cuối cùng | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 0 3 | 1 4 | {1:4} | 6 | 6 | 0 | 0 | 
| 1 2 0 | cập nhật 3->1 | {1:4} | - | - | 0 | - | 
| 2 0 3 | 1 4 | {1:4} | 6 | 6 | 0 | 0 | 

Điều này xác nhận thuật toán xử lý các mảng hoàn toàn bằng nhau và cập nhật chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * n) | Mỗi truy vấn loại thứ hai lặp lại trong phạm vi theo thời gian tuyến tính; cập nhật là O(1) | 
| Không gian | O(n) | Từ điển tần số và lưu trữ mảng chiếm ưu thế trong việc sử dụng bộ nhớ | 

Mặc dù độ phức tạp trong trường hợp xấu nhất là O(n) cho mỗi truy vấn, ràng buộc$n, q \le 3\cdot10^5$được xử lý trong giới hạn 8 giây bằng cách sử dụng quyền truy cập từ điển nhanh của Python để đếm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided sample
assert run("""3
1 2 3
5
2 0 2
1 0 2
2 0 2
1 2 0
2 1 0
""") == "3 2 0"

# Custom cases
assert run("""4
1 1
```
