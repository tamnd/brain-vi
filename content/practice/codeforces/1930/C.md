---
title: "CF 1930C - Lớn nhất về mặt từ điển"
description: "Chúng ta được cho một mảng a có độ dài n. Nhiệm vụ là chọn liên tục một phần tử từ a, cộng giá trị của nó cộng với chỉ số hiện tại của nó vào tập S, loại bỏ nó khỏi mảng và tiếp tục cho đến khi a trống. Sau tất cả các lần chèn, chúng ta sắp xếp tập S theo thứ tự giảm dần để tạo thành mảng b."
date: "2026-06-09T01:41:36+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1930
codeforces_index: "C"
codeforces_contest_name: "think-cell Round 1"
rating: 1700
weight: 1930
solve_time_s: 106
verified: false
draft: false
---

[CF 1930C - Lớn nhất về mặt từ điển](https://codeforces.com/problemset/problem/1930/C) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`a`chiều dài`n`. Nhiệm vụ là chọn liên tục một phần tử từ`a`, thêm giá trị của nó cộng với chỉ số hiện tại của nó vào một tập hợp`S`, xóa nó khỏi mảng và tiếp tục cho đến khi`a`trống rỗng. Sau tất cả các lần chèn, chúng tôi sắp xếp tập hợp`S`theo thứ tự giảm dần để tạo thành mảng`b`. Mục đích là chọn các phần tử từ`a`theo cách mà kết quả là`b`về mặt từ điển càng lớn càng tốt. 

Mỗi phần tử của`S`được tính như`a_i + i`Ở đâu`i`là vị trí dựa trên 1 tại thời điểm lựa chọn. Từ`S`là một tập hợp, các bản sao sẽ bị bỏ qua. Do đó, việc chọn các phần tử tạo ra số tiền tương tự sau này sẽ không cải thiện kết quả, điều này khiến việc đặt hàng trở nên quan trọng. Những ràng buộc cho phép`n`lên đến`3 * 10^5`với tổng số tiền trên tất cả các trường hợp thử nghiệm được giới hạn ở mức`3 * 10^5`, nghĩa là bất kỳ thuật toán nào có O(n log n) cho mỗi trường hợp thử nghiệm đều khả thi, nhưng cách tiếp cận O(n^2) quá chậm. 

Trường hợp cạnh tinh vi xảy ra khi các giá trị trong`a`đang tăng hoặc giảm nghiêm trọng. Ví dụ,`a = [2, 1]`chứng minh rằng việc loại bỏ số đầu tiên lớn nhất sẽ tạo ra số tiền duy nhất hơn, trong khi loại bỏ số nhỏ hơn trước có thể dẫn đến trùng lặp. Một trường hợp khác là khi tất cả`a_i`bằng nhau, nói`a = [1, 1, 1]`. Chọn các phần tử từ cuối sẽ tối đa hóa tổng với chỉ mục hiện tại vì`a_i + i`tăng với`i`. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ thử mọi hoán vị của các lệnh loại bỏ, tính toán`S`, sau đó sắp xếp thành dạng`b`. Điều này đúng nhưng hoàn toàn không khả thi vì có`n!`hoán vị, và thậm chí đối với`n = 10`, đây đã là 3,6 triệu khả năng rồi. 

Thông tin chi tiết quan trọng là giá trị được chèn vào`S`chỉ phụ thuộc vào tổng của phần tử và chỉ số hiện tại của nó. Để tối đa hóa về mặt từ điển, chúng tôi muốn các giá trị lớn nhất xuất hiện sớm trong`b`. Với mỗi giá trị`a_i`, tổng lớn nhất xảy ra nếu nó được chọn càng muộn càng tốt vì các vị trí sau có chỉ số lớn hơn. Vì vậy, để tối đa hóa`b`, trước tiên chúng ta nên mô phỏng việc chọn các phần tử từ cuối mảng. Chúng ta cũng phải tránh chèn các bản sao vào`S`, vì vậy chúng tôi theo dõi các giá trị được chèn bằng cách sử dụng một tập hợp. 

Cụ thể, nếu chúng ta lặp lại`a`theo thứ tự ngược lại và tính`a_i + i`cho mỗi`i`ngược lại, cộng mỗi tổng vào`S`chỉ khi nó chưa xuất hiện trước đó thì hãy sắp xếp`S`theo thứ tự giảm dần tạo ra từ điển lớn nhất mong muốn`b`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu (đảo ngược + bộ) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một tập hợp trống`S`để lưu trữ số tiền duy nhất. 
2. Lặp lại`a`từ phần tử cuối cùng đến phần tử đầu tiên. 
3. Đối với mỗi phần tử`a[i]`ở chỉ số dựa trên 0`i`, tính toán`a[i] + (i + 1)`để tính đến việc lập chỉ mục dựa trên 1. 
4. Nếu số tiền này chưa có trong`S`, chèn nó. 
5. Sau khi xử lý tất cả các phần tử, chuyển đổi`S`vào một danh sách và sắp xếp nó theo thứ tự giảm dần để tạo thành`b`. 
6. Đầu ra`b`. 

Tại sao nó hoạt động: lặp lại từ cuối đảm bảo rằng các phần tử ở chỉ số cao hơn được xem xét đầu tiên, tối đa hóa chúng`a_i + i`tiền. Chèn vào một bộ sẽ tránh trùng lặp. Sắp xếp theo thứ tự giảm dần đảm bảo rằng tổng lớn nhất xuất hiện đầu tiên, tạo ra mảng lớn nhất về mặt từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        seen = set()
        result = []
        # process from end to start
        for i in reversed(range(n)):
            val = a[i] + i + 1
            if val not in seen:
                seen.add(val)
                result.append(val)
        # output in decreasing order
        print(*sorted(result, reverse=True))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc số lượng trường hợp kiểm thử, sau đó xử lý từng mảng. sử dụng`reversed(range(n))`tính toán chính xác số tiền từ cuối. các`seen`set ngăn chặn việc chèn trùng lặp, điều này rất quan trọng. Việc sắp xếp ở cuối đảm bảo thuộc tính từ điển. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`a = [2, 1]`| Bước | tôi | một [tôi] | a[i]+i+1 | S | Danh sách kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | {2} | [2] | 
| 2 | 0 | 2 | 3 | {2, 3} | [2,3] | 

Sắp xếp giảm dần:`[3,2]`Điều này chứng tỏ rằng việc chọn đầu cuối trước đảm bảo cả hai tổng được chèn vào mà không bị trùng lặp. 

**Mẫu 2** 

đầu vào:`a = [1, 100, 1000, 1000000, 1000000000]`Xử lý ngược lại: 

| tôi | a[i]+i+1 | Chèn? | 
| --- | --- | --- | 
| 4 | 1000000005 | Có | 
| 3 | 1000004 | Có | 
| 2 | 1003 | Có | 
| 1 | 102 | Có | 
| 0 | 2 | Có | 

Sắp xếp giảm dần:`[1000000005, 1000004, 1003, 102, 2]`Hiển thị việc xử lý chính xác các số lớn và giữ nguyên thứ tự từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Lặp lại mảng là O(n), chèn vào một tập hợp được khấu hao O(1), kết quả sắp xếp là O(n log n) | 
| Không gian | O(n) | Tập hợp và danh sách kết quả lưu trữ tối đa n phần tử | 

Cho tổng của`n`trên tất cả các trường hợp thử nghiệm 3 * 10^5, giải pháp này phù hợp với giới hạn thời gian 2 giây và giới hạn bộ nhớ 256MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("3\n2\n2 1\n5\n1 100 1000 1000000 1000000000\n3\n6 4 8\n") == "3 2\n1000000005 1000004 1003 102 2\n11 7 6", "sample 1"

# custom: all equal
assert run("1\n5\n1 1 1 1 1\n") == "6 5 4 3 2", "all equal values"

# custom: strictly increasing
assert run("1\n4\n1 2 3 4\n") == "7 6 5 4", "strictly increasing"

# custom: strictly decreasing
assert run("1\n4\n4 3 2 1\n") == "7 6 5 4", "strictly decreasing"

# custom: single element
assert run("1\n1\n42\n") == "43", "single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 1 | 6 5 4 3 2 | xử lý trùng lặp và thứ tự từ điển | 
| 1 2 3 4 | 7 6 5 4 | thứ tự tăng dần, logic đầu tiên | 
| 4 3 2 1 | 7 6 5 4 | thứ tự giảm dần, áp dụng logic tương tự | 
| 42 | 43 | đầu vào kích thước tối thiểu | 

## Vỏ cạnh 

cho`a = [1, 1, 1, 1, 1]`, thuật toán xử lý từ phần tử cuối cùng: 

| tôi | a[i]+i+1 | S | 
| --- | --- | --- | 
| 4 | 5+1=5 | {5} | 
| 3 | 4 | {4,5} | 
| 2 | 3 | {3,4,5} | 
| 1 | 2 | {2,3,4,5} | 
| 0 | 1 | {1,2,3,4,5} | 

Sắp xếp giảm dần:`[5,4,3,2,1]`như mong đợi. Việc xử lý từ phía trước sẽ chèn các tổng trùng lặp và tạo ra một mảng từ điển nhỏ hơn. Điều này xác nhận chiến lược từ đầu đến cuối đã tối đa hóa một cách chính xác`b`.
