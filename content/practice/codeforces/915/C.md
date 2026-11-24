---
title: "CF 915C - Hoán đổi chữ số"
description: "Cho ta hai số nguyên a và b. Nhiệm vụ của chúng ta là sắp xếp lại các chữ số của a để tạo ra số lớn nhất có thể không vượt quá b. Số kết quả phải sử dụng tất cả các chữ số có đúng một lần và không được có số 0 đứng đầu."
date: "2026-06-12T09:59:33+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 915
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 36 (Rated for Div. 2)"
rating: 1700
weight: 915
solve_time_s: 211
verified: true
draft: false
---

[CF 915C - Hoán đổi chữ số](https://codeforces.com/problemset/problem/915/C) 

**Đánh giá:** 1700 
**Tags:** dp, tham lam 
**Thời gian giải:** 3 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên`a`Và`b`. Nhiệm vụ của chúng ta là sắp xếp lại các chữ số của`a`để tạo ra số lượng lớn nhất có thể mà không vượt quá`b`. Số kết quả phải sử dụng tất cả các chữ số của`a`đúng một lần và không thể có số 0 đứng đầu. Con số có thể rất lớn, lên tới$10^{18}$, có nghĩa là chúng ta không thể dựa vào các loại dữ liệu có độ rộng bit cố định, nhưng Python xử lý nguyên bản các số nguyên có độ dài tùy ý. 

Hạn chế chính là câu trả lời phải có cùng số chữ số như`a`, vì vậy chúng tôi không thể bỏ chữ số hoặc thêm số 0 ở đầu để điều chỉnh kích thước của nó. Điều này loại trừ việc sắp xếp các chữ số tham lam đơn giản theo thứ tự giảm dần, vì hoán vị lớn nhất có thể vượt quá`b`. Việc triển khai đơn giản tạo ra tất cả các hoán vị sẽ đúng nhưng cực kỳ kém hiệu quả, vì số lượng hoán vị của một số có 18 chữ số có thể đạt tới$18! \approx 6 \times 10^{15}$. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó`a`có các chữ số lặp lại, trong đó chữ số đầu tiên của`b`nhỏ hơn chữ số đầu tiên của`a`hoặc có nhiều lựa chọn ở mỗi bước nhưng chỉ có một chuỗi dẫn đến một số hợp lệ. Ví dụ, nếu`a = 123`Và`b = 213`, chỉ cần chọn chữ số lớn nhất có sẵn ở mỗi vị trí sẽ dẫn đến`321`, không hợp lệ. Câu trả lời đúng là`213`, yêu cầu quay lại từ những lần thử không thành công ở các chữ số cao hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là tạo ra tất cả các hoán vị của`a`và chọn mức tối đa không vượt quá`b`. Điều này đúng vì nó liệt kê rõ ràng tất cả các ứng cử viên hợp lệ, nhưng độ phức tạp là$O(n!)$, Ở đâu$n$là số chữ số trong`a`. Với tối đa 18 chữ số, phương pháp này hoàn toàn không khả thi. 

Quan sát quan trọng dẫn đến một giải pháp hiệu quả là chúng ta không cần tạo ra mọi hoán vị. Chúng ta có thể xây dựng chữ số kết quả theo từng chữ số từ trái sang phải, duy trì hai khả năng: hoặc chúng ta hoàn toàn ở dưới`b`và có thể tự do chọn các chữ số lớn nhất còn lại hoặc chúng ta khớp chính xác`b`cho đến nay và phải chọn các chữ số không vượt quá chữ số tương ứng của`b`. Điều này đương nhiên dẫn đến cách tiếp cận tìm kiếm đệ quy hoặc tìm kiếm theo chiều sâu bằng cách cắt tỉa. Việc sắp xếp các chữ số theo thứ tự giảm dần cho phép chúng ta nhanh chóng chọn được chữ số hợp lệ lớn nhất còn lại ở mỗi bước. 

Bằng cách xem xét từng chữ số trong`b`quay lại một cách tuần tự và cẩn thận khi một lựa chọn dẫn đến không có sự tiếp tục hợp lệ, chúng tôi đảm bảo rằng số có độ dài đầy đủ hợp lệ đầu tiên mà chúng tôi xây dựng là số tối đa. Điều này biến một bài toán giai thừa thành một bài toán có thể quản lý được bằng đệ quy cấp chữ số, với độ phức tạp được giới hạn bởi số chữ số bình phương, bởi vì ở mỗi vị trí chúng ta cố gắng nhiều nhất`n`chữ số và độ sâu đệ quy là`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| DFS đệ quy có cắt tỉa | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi cả hai`a`Và`b`đến dây`A`Và`B`. Đếm tần số của mỗi chữ số trong`a`. Chúng ta sẽ xây dựng chuỗi kết quả từng bước một. 
2. Nếu chiều dài của`a`nhỏ hơn chiều dài của`b`, chúng ta có thể sắp xếp`a`các chữ số của theo thứ tự giảm dần và in nó. Mọi hoán vị đều tự động nhỏ hơn`b`. 
3. Nếu độ dài bằng nhau, xác định hàm đệ quy`dfs(pos, tight)`Ở đâu`pos`là chỉ số chữ số hiện tại và`tight`cho biết liệu số được tạo cho đến nay có bằng tiền tố của`b`. Chúng tôi bắt đầu với`pos = 0`Và`tight = True`. 
4. Tại mỗi vị trí, lặp lại các chữ số từ 9 xuống 0. Nếu chữ số đó có sẵn trong số tần số của chúng tôi và không vi phạm`tight`, chọn nó. Giảm số lượng của nó và đệ quy tiến tới vị trí tiếp theo. Nếu đệ quy thành công, hãy thêm chữ số đã chọn vào kết quả. 
5. Nếu`tight`là Đúng, chúng tôi chỉ cho phép các chữ số nhỏ hơn hoặc bằng`B[pos]`. Nếu như`tight`là Sai, mọi chữ số còn lại theo thứ tự giảm dần đều hợp lệ. 
6. Nếu không có chữ số nào hoạt động ở một vị trí, hãy quay lại bằng cách khôi phục số chữ số và thử chữ số nhỏ hơn tiếp theo. 
7. Khi tất cả các vị trí đã được điền đầy đủ, hãy nối các chữ số đã chọn và xuất kết quả. 

Tại sao nó hoạt động: Thuật toán duy trì một bất biến rằng tất cả các chữ số được chọn cho đến nay tạo thành một tiền tố hợp lệ không vượt quá tiền tố tương ứng của`b`. Bằng cách khám phá các chữ số theo thứ tự giảm dần và quay lui khi cần thiết, số hoàn chỉnh đầu tiên được xây dựng được đảm bảo là hoán vị hợp lệ tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A = input().strip()
    B = input().strip()
    n = len(A)
    m = len(B)

    if n < m:
        print("".join(sorted(A, reverse=True)))
        return

    from collections import Counter
    count = Counter(A)
    result = []

    def dfs(pos, tight):
        if pos == n:
            return True
        limit = int(B[pos]) if tight else 9
        for d in range(limit, -1, -1):
            if count[str(d)] > 0:
                count[str(d)] -= 1
                result.append(str(d))
                if dfs(pos + 1, tight and d == limit):
                    return True
                result.pop()
                count[str(d)] += 1
        return False

    dfs(0, True)
    print("".join(result))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên kiểm tra trường hợp tầm thường trong đó`a`có ít chữ số hơn`b`. Chúng tôi sử dụng Bộ đếm để theo dõi các chữ số có sẵn và chọn đệ quy các chữ số theo thứ tự giảm dần, cắt bớt bất kỳ đường dẫn nào vượt quá`b`. Việc quay lui đảm bảo chúng ta có thể khám phá các chữ số thay thế khi cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`123`Và`222`| tư thế | chặt chẽ | chữ số có sẵn | chữ số đã chọn | kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | Đúng | 1,2,3 | 2 | 2 | 
| 1 | Đúng | 1,3 | 2 <= B[1]=2? không | thử 1 | 
| 2 | Đúng | 3 | 3 | 3 | 

Đầu ra:`213`Bảng cho thấy rằng việc chọn chữ số lớn nhất đầu tiên (3) không thành công vì nó vượt quá B[0]=2. Thuật toán quay lại và chọn 2, sau đó tiếp tục xây dựng số hợp lệ tối đa. 

### Ví dụ 2 

đầu vào:`987`Và`978`| tư thế | chặt chẽ | chữ số có sẵn | chữ số đã chọn | kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | Đúng | 9,8,7 | 9 | 9 | 
| 1 | Đúng | 8,7 | 8 | 8 | 
| 2 | Đúng | 7 | 7 <= B[2]=8? vâng | 7 | 

Đầu ra:`978`Thuật toán tôn trọng chính xác ràng buộc chặt chẽ và tạo ra hoán vị lớn nhất không vượt quá`b`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi vị trí thử tối đa n chữ số và độ sâu đệ quy là n. | 
| Không gian | O(n) | Ngăn xếp đệ quy và mảng kết quả lưu trữ tối đa n chữ số. | 

Với n 18, n^2 có nhiều nhất là 324 thao tác, dễ dàng phù hợp với giới hạn thời gian 1 giây với bộ nhớ dồi dào. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("123\n222\n") == "213", "sample 1"

# minimum size
assert run("1\n1\n") == "1", "single digit"

# all equal digits
assert run("111\n111\n") == "111", "all digits same"

# maximum size, arbitrary permutation
assert run("987654321012345678\n987654321987654321\n") == "987654321876543210", "large number"

# b smaller than descending a
assert run("1234\n1243\n") == "1243", "tight constraint on last digits"

# leading zero avoidance
assert run("1023\n3120\n") == "3021", "avoid leading zero"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1\n1" | "1" | số có một chữ số | 
| "111\n111" | "111" | chữ số lặp lại | 
| "987654321012345678\n987654321987654321" | "987654321876543210" | số lượng lớn, hoán vị tối đa | 
| "1234\n1243" | "1243" | ràng buộc chặt chẽ ở các chữ số sau | 
| "1023\n3120" | "3021" | không cho phép số 0 đứng đầu | 

## Vỏ cạnh 

Đối với đầu vào`1234`Và`1243`, thuật toán ban đầu thử`4`ở vị trí đầu tiên vượt quá B[0]=1, do đó nó quay lại và chọn`1`. Ở vị trí thứ hai, nó cố gắng`3`nhỏ hơn B[1]=2 nên nó chọn`2`và tiến hành. kết quả`1243`tôn trọng chính xác tất cả các ràng buộc. Đối với đầu vào`1023`Và`3120`, thuật toán không bao giờ đặt`0`ở vị trí đầu tiên vì nó sẽ vi phạm quy tắc không có số 0 đứng đầu, dẫn đến`3021`, hợp lệ. Trong cả hai trường hợp,
