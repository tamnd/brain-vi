---
title: "CF 102911F - Văn học dân gian"
description: "Chúng ta được cấp một chuỗi các vật phẩm riêng biệt, mỗi vật phẩm ban đầu nằm ở một vị trí cố định từ 1 đến N. Chúng ta phải sắp xếp lại chúng theo một thứ tự mới."
date: "2026-07-04T08:05:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102911
codeforces_index: "F"
codeforces_contest_name: "2021 Ateneo de Manila Senior High School Dagitab Programming Contest (Mirror)"
rating: 0
weight: 102911
solve_time_s: 46
verified: true
draft: false
---

[CF 102911F - Văn học dân gian](https://codeforces.com/problemset/problem/102911/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các vật phẩm riêng biệt, mỗi vật phẩm ban đầu nằm ở một vị trí cố định từ 1 đến N. Chúng ta phải sắp xếp lại chúng theo một thứ tự mới. Ràng buộc mang tính vị trí: nếu một mục bắt đầu ở vị trí x và kết thúc ở vị trí y, thì khoảng cách tuyệt đối |x − y| ít nhất phải là K. Nói cách khác, không vật nào được phép ở gần vị trí ban đầu của nó mà phải di chuyển ra xa ít nhất K bước trong phép hoán vị cuối cùng. 

Nhiệm vụ là xây dựng bất kỳ hoán vị hợp lệ nào thỏa mãn quy tắc này hoặc xác định rằng không tồn tại hoán vị nào như vậy. 

Đầu vào là một chuỗi tên bài hát, nhưng những tên này chỉ là nhãn cho các phần tử riêng biệt. Cấu trúc thực là các vị trí chỉ mục từ 1 đến N. Đầu ra là một chuỗi được sắp xếp lại hợp lệ hoặc một câu lệnh không thể thực hiện được. 

Ràng buộc N lên tới 10^5 ngụ ý rằng chúng ta cần một cấu trúc tuyến tính hoặc gần tuyến tính. Bất kỳ giải pháp nào cố gắng kiểm tra hoán vị hoặc cấu hình tìm kiếm đều bị loại trừ ngay lập tức vì N! hoặc thậm chí N^2 cách tiếp cận vượt xa giới hạn khả thi. Các giải pháp khả thi duy nhất là hoán vị mang tính xây dựng trực tiếp hoặc dịch chuyển tham lam. 

Trường hợp cạnh khóa xuất phát từ N nhỏ hoặc K lớn. Nếu K lớn so với N thì không thể di chuyển được. 

Ví dụ: nếu N = 4 và K = 3, vị trí 1 sẽ cần phải di chuyển đến ít nhất là vị trí 4, nhưng vị trí 4 sẽ cần di chuyển đến nhiều nhất là vị trí 1, tạo ra xung đột không thể giải quyết đồng thời cho tất cả các phần tử. Một nỗ lực ngây thơ như dịch chuyển mọi thứ theo K vị trí sẽ thất bại vì việc bao bọc tạo ra khoảng cách nhỏ. 

Một trường hợp cạnh tinh vi khác là khi K = 0. Trong trường hợp đó, thứ tự ban đầu đã thỏa mãn điều kiện, vì |x − x| = 0 được cho phép. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ cố gắng tạo ra tất cả các hoán vị và kiểm tra xem mọi phần tử có thỏa mãn điều kiện khoảng cách hay không. Về nguyên tắc, điều này đúng vì nó trực tiếp thực thi quy tắc, nhưng nó yêu cầu kiểm tra N! hoán vị và mỗi lần kiểm tra có giá O(N), khiến nó hoàn toàn không khả thi nếu vượt quá N = 8 hoặc hơn. 

Một cải tiến mạnh mẽ có cấu trúc chặt chẽ hơn là cố gắng hoán đổi hoặc quay lại vị trí theo vị trí, đảm bảo mỗi phép gán đều tôn trọng giới hạn khoảng cách đối với các phần tử đã được đặt. Điều này vẫn bùng nổ vì các vị trí đầu hạn chế rất nhiều những vị trí sau và hệ số phân nhánh vẫn lớn. 

Quan sát quan trọng là ràng buộc hoàn toàn mang tính vị trí và đối xứng: mỗi chỉ số i cấm đặt phần tử của nó trong khoảng [i − (K − 1), i + (K − 1)]. Điều này gợi ý rằng thay vì tìm kiếm, chúng ta nên trực tiếp xây dựng một hoán vị dịch chuyển mọi chỉ số ra ngoài vùng cấm của nó một cách thống nhất. 

Một nỗ lực tự nhiên là một sự thay đổi theo chu kỳ. Nếu chúng ta di chuyển mọi phần tử từ i đến i + K, bao quanh thì mỗi phần tử sẽ di chuyển chính xác K vị trí về phía trước trong không gian chỉ mục. Tuy nhiên, việc bao bọc đưa ra một chuyển vị thứ hai có thể nhỏ hơn K. Lần duy nhất điều này không phá vỡ ràng buộc là khi khoảng cách bao quanh ít nhất là K, điều này xảy ra chính xác khi N − K ≥ K, hoặc tương đương N ≥ 2K. 

Khi N ≥ 2K, chúng ta có thể chia mảng thành hai phần có kích thước K và N − K một cách an toàn và xoay toàn bộ mảng theo K vị trí. Điều này đảm bảo mọi phần tử di chuyển tiến hoặc lùi ít nhất K. 

Nếu N < 2K, mọi nỗ lực đều phải thất bại vì các vị trí sẵn có được đóng gói quá chặt chẽ. Mọi phần tử phải tránh một cửa sổ trung tâm có kích thước 2K − 1 xung quanh nó, nhưng tổng số vị trí có sẵn không đủ để đáp ứng đồng thời tất cả các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(N! · N) | O(N) | Quá chậm | 
| Xây dựng ca tuần hoàn | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Việc xây dựng tối ưu phụ thuộc vào việc chúng ta có thể xoay mảng theo K vị trí một cách an toàn mà không vi phạm ràng buộc khoảng cách hay không. 

1. Kiểm tra giá trị của K. Nếu K bằng 0 thì thứ tự ban đầu đã thỏa mãn điều kiện nên chúng ta có thể xuất mảng trực tiếp. Điều này tránh được những biến đổi không cần thiết và xử lý trường hợp suy biến một cách rõ ràng. 
2. Kiểm tra xem N có nhỏ hơn 2K hay không. Nếu đúng như vậy, hãy kết luận ngay rằng không có hoán vị hợp lệ nào tồn tại. Điều này xuất phát từ thực tế là một vị trí hợp lệ sẽ yêu cầu mọi phần tử phải được ánh xạ bên ngoài một khoảng bị cấm có kích thước 2K − 1, khoảng này không thể chứa được trong một dòng có độ dài N. 
3. Khi N ít nhất là 2K, hãy xây dựng một hoán vị mới bằng cách dịch chuyển từng chỉ số tiến lên K vị trí theo modulo N. Cụ thể, với mỗi i từ 1 đến N, chúng ta gán nó cho vị trí (i + K), bao quanh điểm bắt đầu khi chúng ta vượt quá N. 
4. Xuất chuỗi kết quả theo thứ tự mới này. 

Lý do công trình này được chọn là vì nó tạo ra một mô hình dịch chuyển đồng đều. Mọi phần tử được di chuyển chính xác K vị trí về phía trước theo nghĩa vòng tròn và điều kiện kích thước N ≥ 2K đảm bảo rằng việc bao bọc không tạo ra khoảng cách phím tắt nhỏ hơn K. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là mỗi phần tử ban đầu ở vị trí i được di chuyển đến vị trí có khoảng cách tuần hoàn từ i chính xác là K về phía trước. Khi N ≥ 2K, chuyển vị tuần hoàn này tương ứng với chuyển vị tuyến tính của K hoặc N − K, và cả hai đều ít nhất là K. Do đó mọi phần tử đều thỏa mãn |i − p[i]| ≥ K, đảm bảo ràng buộc được duy trì trên toàn cầu. Vì mỗi vị trí được sử dụng chính xác một lần nên kết quả là một hoán vị hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    arr = input().split()

    if k == 0:
        print("YES")
        print(*arr)
        return

    if n < 2 * k:
        print("NO")
        return

    res = [None] * n

    for i in range(n):
        res[(i + k) % n] = arr[i]

    print("YES")
    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp mã hóa sự dịch chuyển theo chu kỳ được mô tả trong thuật toán. Mảng`res`đại diện cho thứ tự cuối cùng và mỗi chỉ mục gốc i được đặt ở vị trí (i + k) mod n. Hoạt động modulo là nơi duy nhất mà việc bao bọc được xử lý và tính chính xác phụ thuộc vào việc kiểm tra trước đó để đảm bảo rằng việc bao bọc này không tạo ra các chuyển vị nhỏ hơn K. 

Một lỗi phổ biến là quên rằng sự dịch chuyển theo chu kỳ tương tự không thành công khi N < 2K. Nếu không có lớp bảo vệ đó, các phần tử bao bọc từ đầu đến cuối có thể trở nên quá gần với vị trí ban đầu của chúng. 

## Ví dụ đã hoạt động 

Xét trường hợp N = 4 và K = 1 với đầu vào`[a, b, c, d]`. 

Chúng ta dịch chuyển mỗi phần tử 1 vị trí: 

| tôi | vị trí ban đầu | vị trí mới (i+1 mod 4) | 
| --- | --- | --- | 
| 1 | một | 2 | 
| 2 | b | 3 | 
| 3 | c | 4 | 
| 4 | d | 1 | 

Hoán vị kết quả là`[d, a, b, c]`. Mỗi phần tử di chuyển chính xác một vị trí theo chu kỳ và vì K = 1 nên điều này là hợp lệ. 

Bây giờ hãy xem xét N = 6 và K = 2 với đầu vào`[1, 2, 3, 4, 5, 6]`. 

| tôi | vị trí ban đầu | vị trí mới | 
| --- | --- | --- | 
| 1 | 1 | 3 | 
| 2 | 2 | 4 | 
| 3 | 3 | 5 | 
| 4 | 4 | 6 | 
| 5 | 5 | 1 | 
| 6 | 6 | 2 | 

Kết quả là`[5, 6, 1, 2, 3, 4]`. Mọi phần tử đều di chuyển 2 hoặc 4 vị trí so với chỉ mục ban đầu của nó và cả hai đều ít nhất là K. 

Những dấu vết này cho thấy sự dịch chuyển duy trì mô hình dịch chuyển đồng đều và tránh bất kỳ phần tử nào ở trong vùng lân cận bị cấm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử được đặt chính xác một lần trong quá trình xây dựng | 
| Không gian | O(N) | Chúng tôi lưu trữ mảng hoán vị kết quả | 

Giải pháp này chạy thoải mái trong các giới hạn vì nó chỉ thực hiện một lần truyền tuyến tính duy nhất qua các mảng đầu vào và đầu ra. Ngay cả đối với N = 10^5, điều này vẫn nằm trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

# K = 0 case
assert run("3 0\na b c\n") == "YES\na b c"

# small impossible case
assert run("4 3\na b c d\n") == "NO"

# basic valid shift
assert run("4 1\na b c d\n") == "YES\nd a b c"

# N = 6, K = 2
assert run("6 2\n1 2 3 4 5 6\n") == "YES\n5 6 1 2 3 4"

# boundary N = 1, K = 0
assert run("1 0\nx\n") == "YES\nx"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N=1, K=0 | CÓ x | trường hợp tối thiểu | 
| N=4, K=3 | KHÔNG | chế độ bất khả thi N < 2K | 
| N=4, K=1 | sự dịch chuyển theo chu kỳ | tính đúng đắn cơ bản | 
| N=6, K=2 | hoán vị dịch chuyển | hiệu lực nhiều bước | 

## Vỏ cạnh 

Khi K = 0, ràng buộc không áp đặt hạn chế nào. Thuật toán ngay lập tức trả về mảng ban đầu, thỏa mãn điều kiện một cách tầm thường vì mọi phần tử đều giữ khoảng cách 0 so với vị trí ban đầu của nó. 

Khi N < 2K, ví dụ N = 5, K = 3, bất kỳ sự dịch chuyển nào được thực hiện đều gây ra xung đột bao quanh. Một phần tử được di chuyển từ vị trí 4 đến vị trí 1 có khoảng cách là 3, nhưng các phần tử gần ranh giới không thể được đặt mà không vi phạm ràng buộc và việc xây dựng là không thể. Thuật toán phát hiện chính xác điều này và đưa ra NO trước khi thử bất kỳ hoán vị nào. 

Khi N ≥ 2K, sự dịch chuyển theo chu kỳ không bao giờ ánh xạ một phần tử vào khoảng cấm xung quanh vị trí ban đầu của nó và mọi phần tử đều kết thúc ở một khoảng cách an toàn, xác nhận tính đúng đắn của việc xây dựng trong mọi trường hợp khả thi.
