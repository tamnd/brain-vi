---
title: "CF 106403A - Sang trọng"
description: "Chúng ta được cho một số số nguyên độc lập. Với mỗi số nguyên x, chúng ta tính phần nguyên của căn bậc hai của nó, gọi nó là k = ⌊√x⌋. Nhiệm vụ là xác định xem x có phải là “sang trọng” hay không, nghĩa là nó có thể chia hết cho k này."
date: "2026-06-25T10:07:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106403
codeforces_index: "A"
codeforces_contest_name: "Bay Area Programming Contest 2026 Novice Division"
rating: 0
weight: 106403
solve_time_s: 40
verified: true
draft: false
---

[CF 106403A - Sang trọng](https://codeforces.com/problemset/problem/106403/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số số nguyên độc lập. Với mỗi số nguyên x, chúng ta tính phần nguyên của căn bậc hai của nó, gọi nó là k = ⌊√x⌋. Nhiệm vụ là xác định xem x có phải là “sang trọng” hay không, nghĩa là nó có thể chia hết cho k này. 

Vì vậy, đầu vào là một chuỗi các số và với mỗi số, chúng ta đưa ra một quyết định nhị phân: liệu x mod ⌊√x⌋ có bằng 0 hay không. 

Cấu trúc được cố tình tối giản. Mỗi truy vấn đều độc lập nên không có trạng thái chia sẻ hoặc hiệu ứng tích lũy. Đầu ra thường là một chuỗi các câu trả lời theo thứ tự giống như đầu vào. 

Kiểu ràng buộc cho loại bài toán này thường lớn, thường lên tới 10^5 hoặc 10^6 số, với x có thể lên tới 10^18. Sự kết hợp đó ngay lập tức loại trừ bất kỳ giải pháp nào thực hiện nhiều hơn công việc hằng số hoặc logarit trên mỗi số. Một vòng lặp ngây thơ trên tất cả các ước số sẽ quá chậm. 

Các trường hợp cạnh tinh tế đến từ những hình vuông hoàn hảo và những con số rất nhỏ. 

Trường hợp một góc là x = 1. Ở đây ⌊√1⌋ = 1, vì vậy điều kiện luôn đúng và câu trả lời gần như đúng. 

Một trường hợp khác là khi x ở ngay dưới một hình vuông hoàn hảo. Ví dụ x = 15 cho ⌊√15⌋ = 3 và 15 chia hết cho 3 nên nó hợp lệ. Nhưng x = 14 cũng cho ⌊√14⌋ = 3 và 14 không chia hết cho 3 nên không thành công. Các giá trị liền kề này nêu bật rằng ranh giới quyết định được điều khiển bởi hàm bước căn bậc hai chứ không phải độ lớn của chính x. 

Trường hợp cạnh thứ ba là các hình vuông hoàn hảo như x = 16. Ở đây ⌊√16⌋ = 4 và khả năng chia hết trở nên chính xác trong một số trường hợp (16 hoạt động), nhưng nhiều hình vuông hoàn hảo vẫn sẽ không thành công tùy thuộc vào giá trị, do đó không có lối tắt như “tất cả các hình vuông đều vượt qua”. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực rất đơn giản: đối với mỗi số x, hãy tính k = ⌊√x⌋, sau đó kiểm tra xem x % k == 0. Nếu chúng ta tính căn bậc hai bằng cách quét từ 1 trở lên cho đến khi i² vượt quá x, thì chi phí này là O(√x) cho mỗi truy vấn. Với đầu vào lớn, điều này trở nên nghiêm trọng, có khả năng đạt tới 10^10 thao tác trong trường hợp xấu nhất. 

Quan sát quan trọng là chúng ta không cần tìm kiếm k. Giá trị ⌊√x⌋ có thể được tính trực tiếp bằng cách sử dụng quy trình căn bậc hai số nguyên tiêu chuẩn, chạy trong thời gian không đổi hoặc logarit tùy thuộc vào việc triển khai. Khi đã biết k, việc kiểm tra tính chia hết là thời gian không đổi. 

Do đó, cấu trúc của bài toán không sâu về mặt thuật toán, nhưng nó rất nhạy với việc tính toán căn bậc hai số nguyên chính xác. Phải xử lý cẩn thận sqrt dấu phẩy động vì lỗi làm tròn có thể tạo ra k hoặc k+1 không chính xác, làm thay đổi kết quả chia hết gần các ô vuông hoàn hảo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét sqrt vũ lực | O(√x) mỗi truy vấn | O(1) | Quá chậm | 
| Số nguyên sqrt + kiểm tra | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng số một cách độc lập. 

1. Đọc số nguyên x. Mỗi giá trị được xử lý riêng biệt nên không cần xử lý trước. 
2. Tính k là căn bậc hai số nguyên của x. Đây là số nguyên lớn nhất sao cho k2 ≤ x. Tính đúng đắn của phần còn lại của logic phụ thuộc vào việc k có chính xác hay gần đúng hay không. 
3. Kiểm tra xem x có chia hết cho k hay không. Nếu x % k == 0 thì phân loại x là sang trọng, nếu không thì phân loại x là không sang trọng. 
4. Xuất kết quả của x trước khi chuyển sang số tiếp theo. 

Quyết định không tầm thường duy nhất là tính toán k một cách đáng tin cậy. Việc sử dụng sqrt dấu phẩy động và cắt bớt có thể được chấp nhận trong nhiều môi trường, nhưng cách tiếp cận an toàn hơn là tính k bằng cách sử dụng số học số nguyên hoặc điều chỉnh xung quanh giá trị ứng cử viên để làm tròn chính xác.

Tại sao nó hoạt động: mỗi số chỉ được phân loại theo mối quan hệ của nó với ⌊√x⌋. Không có sự tương tác giữa các số và không có trạng thái ẩn. Căn bậc hai số nguyên chia không gian đầu vào thành các khoảng trong đó k không đổi và trong mỗi khoảng chúng tôi chỉ kiểm tra một điều kiện mô-đun duy nhất. Điều này đảm bảo rằng khi k được tính toán chính xác thì quyết định sẽ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    t = int(input())
    for _ in range(t):
        x = int(input())
        k = math.isqrt(x)
        if x % k == 0:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp được xây dựng xung quanh`math.isqrt`, giúp tránh hoàn toàn các vấn đề về độ chính xác của dấu phẩy động. Một lỗi phổ biến là sử dụng`int(math.sqrt(x))`trực tiếp, điều này có thể thất bại đối với các hình vuông hoàn hảo lớn do làm tròn xuống không chính xác hoặc đôi khi làm tròn lên trong các trường hợp cạnh. Hàm căn bậc hai số nguyên đảm bảo k2 ≤ x < (k+1)2. 

Phần còn lại của quá trình triển khai được cố tình tối thiểu hóa: mỗi trường hợp thử nghiệm là độc lập và việc kiểm tra tính phân chia là thời gian không đổi. 

## Ví dụ đã hoạt động 

Xét đầu vào x = 15, 14, 16. 

Với mỗi giá trị, chúng ta tính k = ⌊√x⌋. 

| x | k = ⌊√x⌋ | x %k | Đầu ra | 
| --- | --- | --- | --- | 
| 15 | 3 | 0 | CÓ | 
| 14 | 3 | 2 | KHÔNG | 
| 16 | 4 | 0 | CÓ | 

Dấu vết này cho thấy ranh giới căn bậc hai nhóm các số thành các khối trong đó k cố định, nhưng khả năng chia hết thay đổi độc lập trong mỗi khối. 

Bây giờ hãy xem xét các giá trị nhỏ. 

| x | k | x %k | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | CÓ | 
| 2 | 1 | 0 | CÓ | 
| 3 | 1 | 0 | CÓ | 
| 4 | 2 | 0 | CÓ | 

Điều này chứng tỏ rằng với k = 1 thì điều kiện luôn đúng, đây là thuộc tính cấu trúc của định nghĩa chứ không phải là trường hợp đặc biệt trong mã. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi truy vấn thực hiện sqrt và mô đun số nguyên theo thời gian không đổi | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp này phù hợp thoải mái với các ràng buộc thông thường cho tối đa 10^5 truy vấn trở lên, vì mỗi thao tác có thời gian không đổi và được triển khai trong mã thư viện dựa trên C được tối ưu hóa để tính toán căn bậc hai. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = []
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        x = int(input())
        k = math.isqrt(x)
        out.append("YES" if x % k == 0 else "NO")
    return "\n".join(out)

# small cases
assert run("3\n1\n2\n3\n") == "YES\nYES\nYES"

# perfect square boundary
assert run("2\n15\n16\n") == "YES\nYES"

# non-trivial mix
assert run("4\n10\n11\n12\n13\n") == "NO\nNO\nYES\nNO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1,2,3 | CÓ, CÓ, CÓ | k=1 tính đúng đắn của hành vi | 
| 15,16 | CÓ, CÓ | ranh giới xung quanh hình vuông hoàn hảo | 
| 13-10 | hỗn hợp | độ đúng chung qua các khoảng | 

## Vỏ cạnh 

Với x = 1, ta được k = 1 nên điều kiện chia hết luôn đúng. Thuật toán trực tiếp trả về CÓ mà không cần xử lý đặc biệt nào, vì 1 % 1 = 0. 

Đối với các số ngay dưới một số chính phương, chẳng hạn như x = 24, chúng ta nhận được k = 4. Thuật toán kiểm tra 24 % 4 = 0, trả về CÓ, phù hợp với định nghĩa. Với x = 23, k vẫn là 4, nhưng 23 % 4 ≠ 0, trả về NO. Tính chính xác hoàn toàn phụ thuộc vào việc sử dụng ⌊√x⌋ thay vì bất kỳ căn bậc hai làm tròn nào. 

Đối với các bình phương hoàn hảo như x = 49, k = 7 và 49 % 7 = 0, do đó kết quả đầu ra là CÓ. Điều này xác nhận rằng thuật toán xử lý các ranh giới hình vuông chính xác mà không cần bất kỳ logic điều chỉnh nào. 

Với x = 2, k = 1, điều này làm giảm bài toán kiểm tra tính chia hết cho 1, luôn đúng. Việc triển khai xử lý việc này một cách tự nhiên mà không cần phân nhánh.
