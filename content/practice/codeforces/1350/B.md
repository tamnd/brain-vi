---
title: "CF 1350B - Orac và mô hình"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi mô hình, chúng ta có một chuỗi các kích thước mô hình được lập chỉ mục từ 1 đến n. Chúng ta được phép chọn một tập con các chỉ số, nhưng các chỉ số được chọn phải được giữ theo thứ tự tăng dần, tương đương với việc chọn một dãy con của các chỉ số."
date: "2026-06-11T14:32:29+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1350
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 641 (Div. 2)"
rating: 1400
weight: 1350
solve_time_s: 96
verified: true
draft: false
---

[CF 1350B - Orac và Mô hình](https://codeforces.com/problemset/problem/1350/B) 

**Đánh giá:** 1400 
**Tags:** dp, toán, lý thuyết số 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi mô hình, chúng ta có một chuỗi các kích thước mô hình được lập chỉ mục từ 1 đến n. Chúng ta được phép chọn một tập con các chỉ số, nhưng các chỉ số được chọn phải được giữ theo thứ tự tăng dần, tương đương với việc chọn một dãy con của các chỉ số. 

Dãy con được chọn chỉ hợp lệ nếu mọi cặp chỉ số được chọn liên tiếp thỏa mãn hai ràng buộc cùng một lúc. Đầu tiên, chỉ mục tiếp theo phải là bội số của chỉ mục trước đó. Thứ hai, kích thước ở chỉ mục tiếp theo phải lớn hơn kích thước ở chỉ mục trước đó. Mục tiêu là tối đa hóa số lượng chỉ số có thể được chọn theo các quy tắc này. 

Các ràng buộc đủ lớn để n có thể đạt tới 100.000 cho mỗi trường hợp thử nghiệm, với tổng số trên tất cả các trường hợp thử nghiệm cũng lên tới 100.000. Điều này ngay lập tức loại trừ mọi cách tiếp cận bậc hai đối với tất cả các cặp chỉ số. Giải pháp kiểm tra sự chuyển đổi giữa tất cả các cặp hoặc cố gắng xây dựng các chuỗi con một cách rõ ràng sẽ quá chậm. Chúng ta cần một cái gì đó gần hơn với tuyến tính hoặc n log n cho mỗi trường hợp thử nghiệm. 

Một điểm tinh tế là cấu trúc không phải là một dãy con tổng quát DP trên các chỉ số. Ràng buộc chia hết buộc chỉ chuyển đổi từ i sang bội số của nó, điều này làm cho biểu đồ trở nên thưa thớt và có cấu trúc. Bỏ qua cấu trúc này sẽ dẫn đến DP O(n^2) sẽ thất bại. 

Các trường hợp cạnh thường phá vỡ các giải pháp ngây thơ bao gồm các mảng trong đó tất cả các giá trị đều bằng nhau, ví dụ n = 5, s = [7, 7, 7, 7, 7]. Ở đây, không tồn tại chuyển đổi hợp lệ nào vì không thể tăng nghiêm ngặt, vì vậy câu trả lời phải là 1. Một trường hợp đặc biệt khác là khi các giá trị tăng nghiêm ngặt nhưng các chỉ số không phù hợp với cấu trúc chia hết, chẳng hạn như n = 5, s = [1, 2, 3, 4, 5]. Mặc dù giá trị tăng lên, chỉ cho phép các chuỗi chỉ số nhân như 1 → 2 → 4, do đó câu trả lời bị giới hạn bởi cấu trúc chỉ mục thay vì thứ tự giá trị. 

## Phương pháp tiếp cận 

Chiến lược brute-force trực tiếp sẽ cố gắng tính toán chuỗi con hợp lệ dài nhất kết thúc ở mọi chỉ mục. Với mỗi chỉ số i, chúng ta sẽ kiểm tra tất cả các chỉ số trước đó j < i và xác minh xem j có chia hết i và s[j] < s[i] hay không. Nếu cả hai đều giữ nguyên, chúng tôi cập nhật dp[i] = max(dp[i], dp[j] + 1). Điều này đúng vì nó trực tiếp tuân theo định nghĩa của chuỗi hợp lệ. 

Tuy nhiên, cách tiếp cận này kiểm tra tối đa i phần tử trước đó cho mỗi i, dẫn đến khoảng n(n−1)/2 chuyển đổi trong trường hợp xấu nhất. Với n = 100.000, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là sự chuyển đổi không phải là tùy ý. Từ bất kỳ chỉ số i nào, chúng ta chỉ cần xét bội số i, 2i, 3i, v.v. của nó. Điều này làm đảo lộn hướng suy nghĩ: thay vì kiểm tra tất cả các trạng thái trước đó để tìm i, chúng ta truyền các giá trị dp theo bội số. Điều này hiệu quả vì số bội số của tất cả i được giới hạn bởi n log n. 

Chúng tôi tính toán dp[i] là chuỗi tốt nhất kết thúc tại i. Sau đó, với mọi i, chúng ta cố gắng mở rộng nó thành tất cả các bội số j = 2i, 3i, ..., cập nhật dp[j] nếu s[j] lớn hơn s[i]. Điều này chuyển đổi vấn đề thành DP có cấu trúc trên biểu đồ chia hết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP trên tất cả các cặp | O(n^2) | O(n) | Quá chậm | 
| DP trên bội số (lan truyền chia hết) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### ## Hướng dẫn thuật toán 

1. Khởi tạo một mảng dp trong đó dp[i] biểu thị độ dài tối đa của chuỗi hợp lệ kết thúc tại chỉ mục i. Bắt đầu với dp[i] = 1 với tất cả i vì mỗi phần tử riêng lẻ đã tạo thành một chuỗi hợp lệ. 
2. Lặp lại i từ 1 đến n theo thứ tự tăng dần. Đối với mỗi i, chúng tôi coi nó như điểm khởi đầu để mở rộng chuỗi về phía trước. 
3. Với mỗi i, lặp qua các bội số j = 2i, 3i, 4i của nó cho đến n. Với mỗi j như vậy, hãy kiểm tra xem s[j] có lớn hơn s[i] hay không. Nếu đúng như vậy, hãy cập nhật dp[j] = max(dp[j], dp[i] + 1).

Bước này có tác dụng vì mọi chuyển đổi hợp lệ đều phải thỏa mãn điều kiện chia hết chỉ số, do đó tất cả các phần tử kế tiếp có thể có của i chính xác là bội số của nó. 
4. Tiếp tục tuyên truyền những cải tiến về phía trước. Vì chúng ta xử lý i theo thứ tự tăng dần nên tất cả đóng góp từ các chỉ số nhỏ hơn đều đã được xem xét khi mở rộng từ i. 
5. Sau khi xử lý tất cả các chỉ số, câu trả lời là giá trị lớn nhất tính bằng dp. 

### Tại sao nó hoạt động 

Trạng thái dp nắm bắt chuỗi hợp lệ dài nhất kết thúc ở mỗi chỉ mục với ràng buộc là quá trình chuyển đổi chỉ xảy ra dọc theo các cạnh phân chia hợp lệ. Mọi chuỗi hợp lệ có thể được phân tách thành các bước liên tiếp trong đó mỗi chỉ mục tiếp theo là bội số của chỉ số trước đó, do đó, mỗi chuyển đổi được xem xét chính xác một lần khi xử lý điểm cuối nhỏ hơn. Vì chúng tôi chỉ cập nhật dp[j] khi s[j] hoàn toàn lớn hơn nên chúng tôi duy trì tính đơn điệu cần thiết trong các giá trị. Không có phần mở rộng hợp lệ nào bị bỏ sót vì mọi phần mở rộng hợp lệ trước đó của j phải là ước số của j và do đó được xử lý sớm hơn theo thứ tự lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = [0] + list(map(int, input().split()))

        dp = [1] * (n + 1)
        ans = 1

        for i in range(1, n + 1):
            val = dp[i]
            if val > ans:
                ans = val

            j = 2 * i
            while j <= n:
                if s[j] > s[i]:
                    if dp[j] < val + 1:
                        dp[j] = val + 1
                j += i

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng một mảng dp trong đó mỗi vị trí bắt đầu dưới dạng một chuỗi có độ dài một. Vòng lặp lồng nhau trên bội số đảm bảo rằng mọi chuyển đổi hợp lệ i → j trong đó j chia hết cho i được coi là chính xác một lần. Việc so sánh s[j] > s[i] thực thi điều kiện kích thước tăng dần. 

Một chi tiết triển khai tinh tế là chúng tôi lưu trữ dp[i] trong một biến cục bộ trước khi lặp qua bội số. Điều này ngăn chặn việc tra cứu mảng lặp đi lặp lại và đảm bảo sự truyền bá rõ ràng. Một điểm quan trọng khác là sử dụng chỉ mục dựa trên 1, điều này phù hợp trực tiếp với báo cáo vấn đề và tránh các lỗi sai lệch khi tạo bội số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 4, s = [5, 3, 4, 6] 

Chúng tôi tính toán dp từng bước. 

| tôi | dp[i] trước | bội số j đã được kiểm tra | cập nhật | dp sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2, 3, 4 | 3 và 4 được cập nhật | [1,2,2,3] | 
| 2 | 2 | 4 | 6 không áp dụng | [1,2,2,3] | 
| 3 | 2 | không | không | [1,2,2,3] | 
| 4 | 3 | không | không | [1,2,2,3] | 

Chuỗi dài nhất kết thúc ở chỉ số 4 với độ dài 3, tương ứng với 1 → 2 → 4. 

### Ví dụ 2 

đầu vào: 

n = 5, s = [5, 4, 3, 2, 1] 

Ở đây không tồn tại sự chuyển đổi giá trị tăng dần. 

| tôi | dp[i] | bội số j | cập nhật | 
| --- | --- | --- | --- | 
| 1 | 1 | 2,3,4,5 | không có (tất cả đều nhỏ hơn) | 
| 2 | 1 | 4 | không | 
| 3 | 1 | không | không | 
| 4 | 1 | không | không | 
| 5 | 1 | không | không | 

Mỗi dp giữ nguyên là 1, vì vậy câu trả lời là 1. Điều này xác nhận rằng chỉ riêng khả năng chia hết là không đủ nếu không có sự tăng trưởng giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi chỉ số i lặp qua các bội số của nó và tổng chuỗi bội số hài hòa trên tất cả i được giới hạn bởi n log n | 
| Không gian | O(n) | mảng dp có kích thước n | 

Độ phức tạp đủ nhanh vì tổng số lần thử cập nhật trong tất cả các trường hợp thử nghiệm tỷ lệ thuận với số cặp bội số chia, nằm trong khoảng 100.000 log 100.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = [0] + list(map(int, input().split()))

        dp = [1] * (n + 1)
        ans = 1

        for i in range(1, n + 1):
            ans = max(ans, dp[i])
            val = dp[i]
            for j in range(2 * i, n + 1, i):
                if s[j] > s[i]:
                    dp[j] = max(dp[j], val + 1)

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""4
4
5 3 4 6
7
1 4 2 3 6 4 9
5
5 4 3 2 1
1
9
""") == """2
3
1
1"""

# custom cases
assert run("""1
1
10
""") == "1", "single element"

assert run("""1
5
1 2 3 4 5
""") == "3", "chain 1->2->4 or 1->3->... limited by divisibility"

assert run("""1
6
1 1 1 1 1 1
""") == "1", "equal values block transitions"

assert run("""1
8
1 3 2 4 6 5 7 8
""") == "4", "mixed structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 đơn | 1 | trường hợp cạnh tối thiểu | 
| tăng giá trị | 3 | ràng buộc phân chia hạn chế tăng trưởng | 
| tất cả đều bình đẳng | 1 | sự bất bình đẳng nghiêm ngặt ngăn chặn mọi chuyển đổi | 
| mẫu hỗn hợp | 4 | tính đúng đắn khi xen kẽ | 

## Vỏ cạnh 

Đầu vào tối thiểu có một phần tử sẽ ngay lập tức trả về 1 vì không thể chuyển đổi. Thuật toán xử lý việc này một cách tự nhiên vì dp được khởi tạo bằng 1 và không có vòng lặp bội số nào tạo ra các bản cập nhật. 

Khi tất cả các giá trị đều bằng nhau, chẳng hạn như s = [7, 7, 7, 7], mọi phép so sánh s[j] > s[i] đều thất bại, do đó dp không đổi ở mức 1 cho tất cả các chỉ số. Do đó, mức tối đa cuối cùng là 1, khớp với đầu ra chính xác. 

Khi giá trị tăng nhưng chỉ số không phù hợp với cấu trúc nhân, chẳng hạn như s = [1, 2, 3, 4, 5, 6], chỉ có thể thực hiện được các chuỗi tuân theo chỉ số chia hết như 1 → 2 → 4 hoặc 1 → 3 → 6. Thuật toán tôn trọng điều này một cách chính xác vì nó chỉ lan truyền theo bội số, không bao giờ lan truyền giữa các chỉ số tùy ý.
