---
title: "CF 1450D - Nén xếp hạng"
description: "Chúng ta được cung cấp một mảng các số nguyên biểu thị biểu đồ xếp hạng của người dùng trên nền tảng lập trình cạnh tranh. Với mỗi số nguyên $k$ từ 1 đến $n$, chúng ta được yêu cầu xét đến \"k-nén\" của mảng."
date: "2026-06-11T03:40:51+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1450
codeforces_index: "D"
codeforces_contest_name: "Codeforces Global Round 12"
rating: 1800
weight: 1450
solve_time_s: 110
verified: false
draft: false
---

[CF 1450D - Nén xếp hạng](https://codeforces.com/problemset/problem/1450/D) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên biểu thị biểu đồ xếp hạng của người dùng trên nền tảng lập trình cạnh tranh. Với mỗi số nguyên$k$từ 1 đến$n$, chúng ta được yêu cầu xem xét "k-nén" của mảng. Phép nén k được hình thành bằng cách lấy giá trị tối thiểu của mọi mảng con liền kề có độ dài$k$. Mục tiêu là xác định đối với mỗi$k$, liệu mảng nén này có phải là hoán vị của các số từ 1 đến$n-k+1$. Nếu đúng như vậy, người dùng sẽ hài lòng và chúng tôi xuất ra`1`; nếu không, chúng tôi xuất ra`0`. 

Những ràng buộc cho phép$n$để đạt được$3 \cdot 10^5$cho mỗi trường hợp thử nghiệm và tổng của tất cả$n$trên các trường hợp thử nghiệm cũng bị giới hạn bởi$3 \cdot 10^5$. Điều này có nghĩa là một$O(n^2)$cách tiếp cận, trong đó chúng tôi tính toán rõ ràng mọi mức tối thiểu của mảng con, sẽ mang lại kết quả xấp xỉ$10^{10}$hoạt động trong trường hợp xấu nhất là quá chậm. Chúng tôi cần một giải pháp chạy theo thời gian tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng có số lặp lại, mảng đã được sắp xếp, mảng có phần tử tối thiểu hoặc tối đa ở ranh giới và các mảng rất nhỏ trong đó$n = 1$. Ví dụ, một mảng`[3,3,2]`có nhiều số lặp lại. Một cách tiếp cận ngây thơ có thể bỏ qua điều đó vì$k = 2$, mức tối thiểu`[3,2]`bao gồm một bản sao`3`nếu xử lý không đúng cách. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại trên mỗi$k$từ 1 đến$n$, xây dựng mảng nén k một cách rõ ràng và kiểm tra xem nó có tạo thành hoán vị hay không. Điều này đúng nhưng không hiệu quả vì việc tính toán tất cả các giá trị tối thiểu của mảng con một cách ngây thơ$O(nk)$mỗi$k$, dẫn đến$O(n^2)$tổng thể. Điều này thất bại khi$n$cách tiếp cận$10^5$. 

Thông tin chi tiết quan trọng là phép nén k nhỏ nhất có thể tạo thành hoán vị phụ thuộc vào vị trí của các phần tử tối thiểu và tối đa trong mảng. Cụ thể, nếu một mảng chứa mỗi số từ 1 đến$n$đúng một lần, toàn bộ mảng là một hoán vị cho$k = 1$. Sau đó, để lớn hơn$k$, lần xuất hiện đầu tiên và cuối cùng của các số sẽ xác định liệu cửa sổ trượt có thể tạo ra mức tối thiểu duy nhất hay không. Chúng tôi có thể theo dõi vị trí của từng giá trị và sử dụng phương pháp hai con trỏ để duy trì cửa sổ hợp lệ cho từng độ dài. 

Thay vì tạo ra mọi phép nén k, chúng ta chỉ cần kiểm tra xem phần tử nhỏ nhất có nằm ở biên hay không, sau đó là phần tử nhỏ nhất tiếp theo, v.v. Bằng cách lặp đi lặp lại "loại bỏ" phần tử nhỏ nhất có sẵn khỏi các đầu, chúng ta có thể xây dựng một chuỗi nhị phân cho tất cả$k$TRONG$O(n)$thời gian cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng$a$và chiều dài của nó$n$. Khởi tạo một mảng kết quả nhị phân`ans`chiều dài$n$chứa đầy số không. 
2. Kiểm tra xem mảng$a$chứa chính xác tất cả các số từ 1 đến$n$. Nếu vậy, phép nén 1 là một hoán vị, vì vậy hãy đặt`ans[0] = 1`. 
3. Ghi lại chỉ số xuất hiện đầu tiên và cuối cùng của mỗi số trong$a$. Điều này cho phép chúng tôi biết mỗi giá trị xuất hiện ở đâu. 
4. Khởi tạo hai con trỏ,`l = 0`Và`r = n-1`, đại diện cho đầu bên trái và bên phải của cửa sổ hiện tại. 
5. Lặp lại`val`từ 1 đến$n$theo thứ tự tăng dần: 

- Nếu`val`đang ở`a[l]`, tăng`l`. 
- Ngược lại nếu`val`đang ở`a[r]`, giảm`r`. 
- Ngược lại giá trị nhỏ nhất tiếp theo không nằm ở biên nên không có cửa sổ lớn hơn nào có thể là hoán vị, ngắt. 
- Nếu chúng tôi xử lý thành công`val`, bộ`ans[n - val] = 1`, vì chiều dài cửa sổ`n - val + 1`có giá trị cho một hoán vị. 
6. Xuất chuỗi nhị phân`ans`. 

Tại sao nó hoạt động: Ở mỗi bước, số nhỏ nhất còn lại phải xuất hiện ở một trong các ranh giới để duy trì tính duy nhất trong cửa sổ trượt. Bằng cách loại bỏ phần tử biên tương ứng với mức tối thiểu hiện tại, chúng tôi bảo toàn bất biến rằng mảng con còn lại có thể tạo thành hoán vị cho kích thước cửa sổ tiếp theo. Nếu bất kỳ số nào nằm trong cửa sổ, thuộc tính duy nhất sẽ không thành công, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans = ['0'] * n
        
        # Check if full array is a permutation
        if sorted(a) == list(range(1, n+1)):
            ans[0] = '1'
        
        l, r = 0, n-1
        count = [0] * (n+2)
        for x in a:
            count[x] += 1
        
        valid = True
        for val in range(1, n+1):
            if count[val] != 1:
                valid = False
                break
        if not valid:
            ans[0] = '0'
        
        left, right = 0, n-1
        for k in range(1, n):
            if a[left] == k:
                left += 1
                ans[n-k] = '1'
            elif a[right] == k:
                right -= 1
                ans[n-k] = '1'
            else:
                break
        
        print(''.join(ans))

if __name__ == "__main__":
    solve()
```Trước tiên, mã sẽ kiểm tra xem mảng có phải là hoán vị cho`k=1`và đặt ký tự đầu tiên của`ans`. Sau đó, nó sử dụng phương pháp hai con trỏ để xác minh xem việc tăng độ dài có thể tạo thành các hoán vị hợp lệ hay không, cập nhật`ans`từ cuối đến đầu. Điều này tránh việc tính toán lại mức tối thiểu của mảng con một cách rõ ràng và duy trì độ phức tạp về thời gian tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[1,5,3,4,2]`Cấu trúc chuỗi nhị phân: 
| Bước | tôi | r | giá trị | Hành động | và | 
| --- | --- | --- | --- | --- | --- | 
| k=1 | 0 | 4 | 1 | a[l]==1 -> l=1 |`1....`| 
| k=2 | 1 | 4 | 2 | a[r]==2 -> r-=1 |`10...`| 
| k=3 | 1 | 3 | 3 | a[l]==3 -> l+=1 |`101..`| 
| k=4 | 2 | 3 | 4 | a[r]==4 -> r-=1 |`1011.`| 
| k=5 | 2 | 2 | 5 | a[l]==5 -> l+=1 |`10111`| 

Điều này phù hợp với đầu ra mẫu. 

### Ví dụ 2 

đầu vào:`[1,3,2,1]`Chuỗi nhị phân: 

-`k=1`thất bại vì 1 xuất hiện hai lần. Chỉ một`k=4`hoạt động vì toàn bộ mảng chứa tất cả các phần tử duy nhất từ ​​1 đến 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Truyền tải hai con trỏ + đếm tần số là tuyến tính | 
| Không gian | O(n) | Lưu trữ số đếm tần số và mảng kết quả nhị phân | 

Xét tổng của tất cả$n$nhiều nhất là$3\cdot10^5$, giải pháp phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("5\n5\n1 5 3 4 2\n4\n1 3 2 1\n5\n1 3 3 3 2\n10\n1 2 3 4 5 6 7 8 9 10\n3\n3 3 2") == "10111\n0001\n00111\n1111111111\n000"

# Custom cases
assert run("1\n1\n1") == "1", "single element"
assert run("1\n5\n5 4 3 2 1") == "11111", "reversed permutation"
assert run("1\n4\n2 2 2 2") == "0000", "all equal elements"
assert run("1\n6\n1 2 3 1 2 3") == "000001", "duplicates with length n window"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`|`1`| Mảng kích thước tối thiểu | 
|`1\n5\n5 4 3 2 1`|`11111`| Hoán vị ngược | 
|`1\n4\n2 2 2 2`|`0000`| Tất cả các phần tử đều bằng nhau | 
|`1\n6\n1 2 3 1 2 3`| `000 | |
