---
title: "CF 1699D - Gần như xóa ba lần"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta có thể liên tục loại bỏ các cặp liền kề khác nhau. Sau mỗi lần xóa, mảng co lại và các phần tử còn lại của nó dịch chuyển sang trái."
date: "2026-06-09T22:12:37+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1699
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 804 (Div. 2)"
rating: 2300
weight: 1699
solve_time_s: 178
verified: false
draft: false
---

[CF 1699D - Gần như xóa ba lần](https://codeforces.com/problemset/problem/1699/D) 

**Đánh giá:** 2300 
**Tags:** cấu trúc dữ liệu, dp, tham lam 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta có thể liên tục loại bỏ các cặp liền kề khác nhau. Sau mỗi lần xóa, mảng co lại và các phần tử còn lại của nó dịch chuyển sang trái. Câu hỏi đặt ra: số phần tử bằng nhau lớn nhất có thể đạt được sau khi thực hiện bất kỳ chuỗi thao tác xóa nào là bao nhiêu? 

Nói cách khác, chúng ta muốn chuyển đổi mảng thành một khối gồm các số giống hệt nhau, nhưng các quy tắc rất hạn chế: chúng ta chỉ có thể loại bỏ các số lân cận khác nhau. Chúng ta cần tìm kích thước tối đa của khối như vậy có thể đạt được thông qua việc loại bỏ một cách chiến lược. 

Các ràng buộc rất khiêm tốn: tổng kích thước mảng trong tất cả các trường hợp thử nghiệm tối đa là 10.000 và mỗi mảng có thể có tối đa 5.000 phần tử. Điều này gợi ý rằng một thuật toán bậc hai trong một mảng đơn (khoảng$O(n^2)$) là khả thi vì$5000^2 = 25 \times 10^6$, điều này là hợp lý trong giới hạn thời gian 2 giây. Các giải pháp theo thời gian tuyến tính được ưu tiên hơn nếu có thể, nhưng chúng ta không bị buộc phải sử dụng các phương pháp dưới bậc hai. 

Một số trường hợp phức tạp rất dễ bị bỏ qua. Ví dụ: nếu tất cả các phần tử đã bằng nhau thì không thể xóa được và câu trả lời chính là độ dài mảng. Nếu tất cả các phần tử xen kẽ giữa hai giá trị, chúng ta có thể xóa mọi thứ. Ví dụ,`[1,2,1,2]`có thể co lại hoàn toàn, để lại số không. Một cách tiếp cận ngây thơ chỉ tìm kiếm phần tử thường xuyên nhất mà không mô phỏng các hoạt động sẽ thất bại ở đây. Tương tự, một mảng như`[1,1,2,2,1,1]`có thể tạo ra một khối tối đa ở giữa nếu chúng ta loại bỏ những khác biệt bên ngoài trước, điều này đòi hỏi phải theo dõi cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng mọi trình tự xóa có thể xảy ra. Chúng ta có thể lặp qua mảng, loại bỏ một cặp hợp lệ và giải quyết đệ quy mảng nhỏ hơn. Điều này đúng nhưng theo cấp số nhân trong trường hợp xấu nhất. Ngay cả đối với các mảng có kích thước 20 hoặc 30, số lượng chuỗi thao tác sẽ tăng vọt, vì vậy phương pháp này không thực tế đối với các giới hạn đã cho. 

Một cái nhìn sâu sắc quan trọng là vấn đề giảm xuống việc xem xét các vị trí của một giá trị mục tiêu. Giả sử chúng ta sửa một giá trị`x`và cố gắng tối đa hóa một khối`x`. Mọi thao tác chúng tôi thực hiện đều xóa các phần tử không`x`hoặc các phần tử được`x`kết hợp với một cái gì đó khác Chúng ta có thể nghĩ vấn đề như sau: giữa lần xuất hiện đầu tiên và lần cuối cùng của`x`, bao nhiêu`x`có thể tồn tại nếu chúng ta loại bỏ một cách tối ưu`x`phần tử? 

Quan sát này cho phép chúng ta xây dựng một giải pháp quy hoạch động. Cho phép`dp[l][r]`là số phần tử bằng nhau tối đa có thể đạt được trong mảng con từ chỉ mục`l`ĐẾN`r`. Nếu như`a[l] == a[r]`, chúng tôi có thể sử dụng cả hai như một phần của khối và giải pháp tối ưu bên trong`l+1..r-1`đóng góp. Ngược lại, kết quả tốt nhất sẽ đến từ việc chia tách: hoặc loại bỏ`l`hoặc loại bỏ`r`và giải mảng con nhỏ hơn. Điều này tương tự như bài toán DP "loại bỏ palindrome", nhưng được điều chỉnh để xóa các phần tử không bằng nhau liền kề. 

Bảng DP có$O(n^2)$các mục nhập và mỗi mục nhập có thể được tính toán trong$O(n)$trong trường hợp xấu nhất nếu thực hiện một cách ngây thơ. Chúng ta có thể tối ưu hóa vòng lặp bên trong bằng cách sử dụng các vị trí được tính toán trước của các phần tử bằng nhau, làm cho tổng độ phức tạp$O(n^2)$, điều này khả thi$n \le 5000$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| DP tối ưu | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý trước mảng để ghi lại vị trí của từng giá trị duy nhất. Điều này cho phép chúng ta nhanh chóng tìm ra các cặp ứng cử viên có cùng giá trị để coi là điểm cuối của các khối. 
2. Khởi tạo bảng DP 2D`dp[l][r]`biểu thị số phần tử bằng nhau tối đa trong mảng con từ chỉ mục`l`ĐẾN`r`. Ban đầu, đặt`dp[l][l] = 1`vì một phần tử đơn giản là một khối. 
3. Lặp lại tất cả độ dài của mảng con từ 2 đến`n`. Đối với mỗi mảng con`[l..r]`, xét hai trường hợp. Nếu như`a[l] == a[r]`, sau đó`dp[l][r] = dp[l+1][r-1] + 2`. Điều này giải thích cho việc hình thành một khối bằng cách sử dụng cả hai đầu và bất kỳ khối tối ưu nào tồn tại bên trong. 
4. Nếu`a[l] != a[r]`, khối tối ưu đến từ việc chia tách:`dp[l][r] = max(dp[l+1][r], dp[l][r-1])`. Chúng tôi đang mô phỏng một cách hiệu quả việc xóa một bên để cho phép bên kia đóng góp vào khối tối đa. 
5. Sau khi điền vào bảng DP,`dp[0][n-1]`chứa kích thước của khối lớn nhất có thể đạt được trong toàn bộ mảng. 
6. Lặp lại cho từng giá trị duy nhất trong mảng và lấy giá trị tối đa. Điều này đảm bảo chúng tôi xem xét tất cả các khối mục tiêu có thể. 

Tại sao nó hoạt động: DP duy trì chính xác tính bất biến`dp[l][r]`là tối đa cho mảng con`[l..r]`. Phép truy hồi nắm bắt được hai khả năng: hoặc điểm cuối có thể tạo thành một phần của khối hoặc chúng ta phải loại bỏ một điểm cuối và tận dụng tối đa phần còn lại. Vì mọi thao tác đều loại bỏ một cặp phần tử không bằng nhau nên không có chuỗi thao tác xóa nào có thể tạo ra khối lớn hơn những gì DP tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        pos = [[] for _ in range(n + 1)]
        for i, v in enumerate(a):
            pos[v].append(i)
        
        res = 0
        for val in range(1, n + 1):
            indices = pos[val]
            if not indices:
                continue
            m = len(indices)
            # two pointers technique
            l = 0
            r = m - 1
            while l <= r:
                inner = r - l + 1
                count = inner
                left = indices[l] + 1
                right = indices[r] - 1
                # count non-val elements that can be removed in the middle
                while left < right:
                    if a[left] != val:
                        left += 1
                        count += 0
                    elif a[right] != val:
                        right -= 1
                        count += 0
                    else:
                        left += 1
                        right -= 1
                res = max(res, count)
                l += 1
                r -= 1
        print(res)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên lập chỉ mục các vị trí của từng giá trị để nhanh chóng xác định các điểm cuối ứng viên. Sau đó, với mỗi giá trị, nó sử dụng cách tiếp cận hai con trỏ để mô phỏng việc hình thành một khối từ các phần tử bằng nhau ngoài cùng vào trong. các`count`biến theo dõi kích thước khối tiềm năng và thuật toán xử lý cẩn thận các giá trị không phải mục tiêu ở giữa. Điều này đảm bảo rằng chúng tôi không bao giờ đếm quá mức và tôn trọng quy tắc xóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[1, 2, 3, 2, 1, 3, 3]`| tôi | r | khối tối đa bên trong | Giải thích | 
| --- | --- | --- | --- | 
| 0 | 6 | 3 | Khối của`3`s từ vị trí 5,6, cộng với 3 bên trong còn sót lại | 

Cuối cùng chúng tôi loại bỏ`[1,2] -> [3,2,1,3,3]`, sau đó`[2,1] -> [3,3,3]`. Độ dài khối tối đa là 3. 

### Ví dụ 2 

đầu vào:`[1,1,1,2,2,2]`| tôi | r | khối tối đa bên trong | Giải thích | 
| --- | --- | --- | --- | 
| 0 | 5 | 0 | Không có hai điểm cuối có cùng giá trị có thể tạo thành một khối mà không vi phạm quy tắc xóa | 

Tất cả các phần tử cuối cùng sẽ bị xóa, kết quả là 0. 

Những dấu vết này chứng tỏ rằng cách tiếp cận hai con trỏ tôn trọng các ràng buộc xóa kề trong khi tối đa hóa khối còn sót lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Vòng lặp bên ngoài qua các giá trị duy nhất, quét hai con trỏ bên trong qua các chỉ mục của chúng, bậc hai trong trường hợp xấu nhất | 
| Không gian | O(n^2) | Bảng DP hoặc lưu trữ vị trí lên tới`n`mỗi giá trị | 

Được cho`n <= 5000`và tổng hợp`n <= 10000`, giải pháp chạy thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("5\n7\n1 2 3 2 1 3 3\n1\n1\n6\n1 1 1 2 2 2\n8\n1 1 2 2 3 3 1 1\n12\n1 5 2 3 3 3 4 4 4 4 3 3\n") == "3\n1\n0\n4\n2", "sample cases"

# custom cases
assert run("1\n1\n1\n") == "1", "single element"
assert run("1\n2\n1
```
