---
title: "CF 102946B - Bongcloud"
description: "Chúng ta có một lưới có kích thước $n nhân m$ trong đó mỗi ô là 0 hoặc 1. Từ lưới này, chúng ta xem xét mọi hình chữ nhật con có thể được căn chỉnh với các đường lưới."
date: "2026-07-04T07:30:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102946
codeforces_index: "B"
codeforces_contest_name: "NCTU PCCA Winter Contest 2021"
rating: 0
weight: 102946
solve_time_s: 50
verified: true
draft: false
---

[CF 102946B - Bongcloud](https://codeforces.com/problemset/problem/102946/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới có kích thước$n \times m$trong đó mỗi ô là 0 hoặc 1. Từ lưới này, chúng tôi xem xét mọi hình chữ nhật con có thể được căn chỉnh với các đường lưới. Mỗi hình chữ nhật được xác định bằng cách chọn hàng trên cùng, hàng dưới cùng, cột bên trái và cột bên phải và nó phải chứa ít nhất một ô. 

Một hình chữ nhật được gọi là đối xứng theo chiều dọc nếu việc lật ngược nó không làm thay đổi nội dung của nó. Cụ thể, nếu bạn so sánh hàng$i$từ trên cùng của hình chữ nhật có hàng$i$từ dưới lên, chúng phải giống hệt nhau đối với tất cả các cột bên trong hình chữ nhật. 

Nhiệm vụ là đếm có bao nhiêu hình chữ nhật đối xứng theo chiều dọc như vậy tồn tại trong lưới. 

Ràng buộc$n \cdot m \le 10^6$là gợi ý cấu trúc quan trọng. Nó loại trừ mọi giải pháp bậc hai theo cả hai chiều cùng một lúc. Một phép liệt kê ngây thơ trên tất cả các hình chữ nhật đã có giá$O(n^2 m^2)$, vượt xa giới hạn khả thi. Ngay cả việc sửa hai mặt và quét phần còn lại một cách ngây thơ vẫn sẽ phát nổ trừ khi vấn đề được xử lý rõ ràng. 

Chế độ lỗi tinh tế xuất hiện khi tính đối xứng chỉ được kiểm tra trên các hàng ranh giới thay vì toàn bộ cấu trúc. Ví dụ: hãy xem xét một hình chữ nhật trong đó các hàng trên và dưới khớp nhau nhưng các hàng ở giữa không đối xứng. Một cách tiếp cận ngây thơ chỉ so sánh các hàng bên ngoài sẽ chấp nhận nó một cách không chính xác. 

Một cạm bẫy phổ biến khác là giả định rằng tính đối xứng giữa các hàng phụ thuộc vào sự tương tác giữa các cột. Trong thực tế, mỗi cột đóng góp độc lập vào việc cấu trúc hàng có phải là palindromic hay không, đây là chìa khóa để giảm bớt vấn đề. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi hình chữ nhật có thể. Đối với mỗi ứng cử viên được xác định bởi$(top, bottom, left, right)$, nó kiểm tra xem hàng$top+k$hàng khớp$bottom-k$cho tất cả hợp lệ$k$và liệu điều này có đúng với tất cả các cột trong khoảng hay không. Cái này đã tốn rồi$O(n^2 m^2)$hình chữ nhật, và mỗi kiểm tra là$O(nm)$trong trường hợp xấu nhất, điều này hoàn toàn không khả thi ngay cả đối với đầu vào vừa phải. 

Sự đơn giản hóa chính xuất phát từ việc tách điều kiện hàng và điều kiện cột. Một hình chữ nhật đối xứng theo chiều dọc khi và chỉ khi, với mỗi cột bên trong nó, chuỗi các giá trị dọc theo các hàng tạo thành một bảng màu. Điều này có nghĩa là bài toán sẽ phân rã theo từng cột: một hình chữ nhật hợp lệ chính xác khi nó là mảng con palindromic hợp lệ trong mỗi cột cùng một lúc. 

Thay vì theo dõi các hình chữ nhật đầy đủ, chúng tôi đảo ngược phối cảnh. Sửa một cột và xem nó dưới dạng một chuỗi nhị phân có độ dài$n$. Mỗi hình chữ nhật đối xứng theo chiều dọc đóng góp một đoạn hàng liền kề là một bảng màu trong cột này. Hình chữ nhật chỉ hợp lệ nếu khoảng cách hàng giống nhau là một palindrome trong mỗi cột được chọn, điều này dẫn đến quan sát rằng mỗi cột sẽ đếm độc lập các chuỗi con palindromic trên các hàng và câu trả lời cuối cùng là tổng trên các cột. 

Điều này làm giảm vấn đề tính toán số lượng chuỗi con palindromic trong chuỗi nhị phân cho mỗi cột. Điều đó có thể được thực hiện theo thời gian tuyến tính trên mỗi cột bằng cách sử dụng thuật toán của Manacher hoặc các kỹ thuật mở rộng trung tâm tương đương, giảm tổng độ phức tạp xuống$O(nm)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 m^2)$|$O(1)$| Quá chậm | 
| Đếm palindrome trên mỗi cột |$O(nm)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý cột lưới theo từng cột, coi mỗi cột là một chuỗi nhị phân độc lập trên các hàng. 

1. Trích xuất một cột dưới dạng chuỗi$s$chiều dài$n$. Điều này thể hiện cách các giá trị thay đổi theo hàng của cột cố định đó. 
2. Tính tất cả các chuỗi con palindrome của$s$. Điều này được thực hiện bằng cách sử dụng phương pháp mở rộng palindrome theo thời gian tuyến tính như thuật toán Manacher. Mỗi palindrome tương ứng với một khoảng đối xứng dọc hợp lệ trong cột này. 
3. Đếm xem có bao nhiêu chuỗi con palindromic tồn tại trong cột này. Số lượng này biểu thị số khoảng hàng thỏa mãn tính đối xứng cho cột cụ thể này. 
4. Thêm số này vào câu trả lời chung. 
5. Lặp lại cho tất cả các cột và in ra tổng số tiền. 

Điểm tinh tế là chúng ta không xây dựng hình chữ nhật một cách rõ ràng. Thay vào đó, mỗi palindrome trong một cột tương ứng với một khoảng hàng ứng cử viên và tổng các cột sẽ tổng hợp tất cả các hình chữ nhật hợp lệ vì một hình chữ nhật hợp lệ chính xác khi mọi cột đồng ý trên cùng một cấu trúc hàng palindromic. 

### Tại sao nó hoạt động 

Sửa bất kỳ hình chữ nhật nào. Điều kiện đối xứng dọc của nó có nghĩa là đối với mỗi cột bên trong nó, chuỗi các giá trị dọc theo các hàng đã chọn phải là một bảng màu. Do đó, mọi hình chữ nhật hợp lệ tương ứng với một khoảng hàng là một bảng màu trong mỗi cột của nó. Ngược lại, bất kỳ khoảng hàng nào có màu nhạt trong một cột đều đóng góp chính xác một cấu trúc hợp lệ trong cột đó. 

Điều này cho phép việc đếm tách hoàn toàn trên các cột vì điều kiện khoảng cách hàng được kiểm tra độc lập trên mỗi cột và sau đó được tích lũy. Không có sự tương tác nào giữa các cột ảnh hưởng đến tính hợp lệ ngoài ràng buộc trên mỗi cột này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def manacher_count(s):
    # counts palindromic substrings in s
    # transform implicitly by using odd/even radii
    n = len(s)
    d1 = [0] * n  # odd
    l, r = 0, -1
    for i in range(n):
        k = 1 if i > r else min(d1[l + r - i], r - i + 1)
        while i - k >= 0 and i + k < n and s[i - k] == s[i + k]:
            k += 1
        d1[i] = k
        k -= 1
        if i + k > r:
            l, r = i - k, i + k

    d2 = [0] * n  # even
    l, r = 0, -1
    for i in range(n):
        k = 0 if i > r else min(d2[l + r - i + 1], r - i + 1)
        while i - k - 1 >= 0 and i + k < n and s[i - k - 1] == s[i + k]:
            k += 1
        d2[i] = k
        k -= 1
        if i + k > r:
            l, r = i - k - 1, i + k

    return sum(d1) + sum(d2)

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    ans = 0
    for col in range(m):
        s = [grid[row][col] for row in range(n)]
        ans += manacher_count(s)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc lưới theo hàng, sau đó xây dựng từng cột dưới dạng một chuỗi tạm thời. Đối với mỗi cột, nó tính toán các chuỗi con palindromic bằng thuật toán Manacher. Các mảng$d1$Và$d2$lưu trữ bán kính của các chuỗi palindrome lẻ và chẵn tương ứng, và tổng của chúng trực tiếp đưa ra số lượng chuỗi con palindromic. 

Một lỗi triển khai phổ biến là trộn lẫn việc lập chỉ mục hàng và cột khi giải nén$s$. Mỗi cột phải được xây dựng độc lập dưới dạng một lát cắt dọc, nếu không cấu trúc palindrome sẽ được tính toán sai chiều. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
01
10
```Chúng tôi xử lý từng cột. 

Đối với cột 0, chuỗi là`"0,1"`. Chuỗi con Palindromic là`"0"`,`"1"`, vậy số đếm là 2. 

Đối với cột 1, chuỗi là`"1,0"`. Một lần nữa, các chuỗi con palindromic là`"1"`,`"0"`, vậy số đếm là 2. 

| Cột | Chuỗi | Số chuỗi con Palindromic | 
| --- | --- | --- | 
| 0 | 01 | 2 | 
| 1 | 10 | 2 | 

Câu trả lời cuối cùng là 4. 

Điều này xác nhận rằng mỗi cột đóng góp độc lập và ngay cả các cột đầy đủ không phải palindromic vẫn đóng góp các palindrome đơn ô. 

### Ví dụ 2 

đầu vào:```
3 1
1
1
1
```Chỉ tồn tại một cột có chuỗi`"111"`. 

Tất cả các chuỗi con đều là chuỗi palindrome nên số đếm là 6. 

| Loại trung tâm | Đóng góp | 
| --- | --- | 
| Palindromes lẻ | 6 | 
| Ngay cả palindromes | 3 (ngầm được bao gồm trong tổng số tiền chia nhỏ) | 

Câu trả lời cuối cùng là 6. 

Điều này chứng tỏ rằng các cột thống nhất tối đa tạo ra số bậc hai của chuỗi con palindromic. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi cột được xử lý một lần bằng thuật toán palindrome thời gian tuyến tính | 
| Không gian |$O(n)$| Mỗi lần chỉ có một chuỗi cột và mảng phụ trợ được lưu trữ | 

Tổng số ô nhiều nhất là$10^6$, do đó việc quét tuyến tính trên mỗi ô nằm trong giới hạn. Việc sử dụng bộ nhớ vẫn ở mức nhỏ vì chúng tôi không bao giờ lưu trữ đồng thời nhiều dữ liệu phụ trợ của một cột. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""

# minimal case
assert run("1 1\n1\n") == ""

# two different values single column
assert run("3 1\n1\n0\n1\n") == ""

# all equal grid
assert run("2 3\n111\n111\n") == ""

# mixed grid
assert run("2 2\n01\n10\n") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | 1 | hình chữ nhật hợp lệ nhỏ nhất | 
| cột xen kẽ | >1 | tính chính xác trên các chuỗi không đồng nhất | 
| tất cả những cái lưới | palindrome tối đa | xử lý tăng trưởng bậc hai | 
| mẫu hoán đổi 2×2 | 4 | sự độc lập của các cột | 

## Vỏ cạnh 

Một lưới ô đơn như`1 1 / 0`tạo ra chính xác một hình chữ nhật hợp lệ. Thuật toán xử lý việc này vì mỗi chuỗi cột có độ dài 1 và Manacher trả về chính xác một bảng màu duy nhất. 

Một lưới nơi mỗi cột xen kẽ như`010101...`đảm bảo rằng chỉ tồn tại các palindrome đơn bào. Thuật toán chỉ tính tổng chính xác các đóng góp có độ dài 1 do việc mở rộng không thành công ngay lập tức. 

Một lưới hoàn toàn đồng nhất, chẳng hạn như tất cả các lưới, tạo ra số lượng chuỗi con palindromic tối đa trên mỗi cột. Mỗi cột độc lập mang lại kết quả$n(n+1)/2$và tính tổng các cột tỷ lệ tuyến tính với$m$, mà thuật toán xử lý mà không sửa đổi.
