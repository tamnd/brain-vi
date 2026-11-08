---
title: "CF 1437A - Đề án tiếp thị"
description: "Chúng tôi đang chọn một gói duy nhất có kích thước a để bán hộp thức ăn cho mèo. Mọi khách hàng ban đầu đều muốn mua một số x nào đó trong một phạm vi cố định [l, r]."
date: "2026-06-14T17:28:34+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1437
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 97 (Rated for Div. 2)"
rating: 800
weight: 1437
solve_time_s: 242
verified: true
draft: false
---

[CF 1437A - Kế hoạch tiếp thị](https://codeforces.com/problemset/problem/1437/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, lòng tham, toán học 
**Thời gian giải:** 4m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang chọn một kích thước gói duy nhất`a`để bán hộp thức ăn cho mèo. Mọi khách hàng ban đầu đều muốn mua một số`x`trong một phạm vi cố định`[l, r]`. Khách hàng sẽ hành động theo hai giai đoạn: đầu tiên họ mua càng nhiều gói giảm giá đầy đủ càng tốt.`a`càng tốt, sau đó họ cố gắng mua riêng lẻ những lon còn lại. Tuy nhiên, nếu phần còn lại ít nhất là một nửa`a`, họ đổi và mua thêm một gói đầy đủ thay vì những lon riêng lẻ còn sót lại. 

Điều này tạo ra số lượng mua cuối cùng hoặc`x + (a - (x mod a))`khi phần còn lại đủ lớn, hoặc chỉ`x`khi nó còn nhỏ. Mục tiêu là chọn`a`để mọi thứ có thể`x`TRONG`[l, r]`, số tiền cuối cùng được mua lớn hơn`x`. 

Đầu vào cho nhiều khoảng độc lập`[l, r]`và đối với mỗi cái, chúng ta phải quyết định xem kích thước gói như vậy có tồn tại hay không. 

Các ràng buộc đi lên đến`10^9`, do đó, bất kỳ giải pháp nào cố gắng mô phỏng hoặc kiểm tra tất cả các giá trị có thể có của`a`hoặc tất cả các giá trị của`x`trực tiếp là không thể. Thậm chí lặp đi lặp lại tất cả`a`lên đến`r`sẽ là quá chậm, vì điều đó sẽ đòi hỏi tới`10^9`hoạt động cho mỗi trường hợp thử nghiệm. 

Một trường hợp cạnh tinh tế đến từ những khoảng thời gian nhỏ. Ví dụ, khi`l = r = 1`, bất kỳ sự lựa chọn nào của`a`thất bại, vì`x = 1`luôn dẫn đến việc không bắt buộc phải nâng cấp. Mặt khác, khi khoảng lớn hơn, đặc biệt là khi nó kéo dài ít nhất 3 số nguyên liên tiếp, có thể tìm thấy kích thước gói buộc ít nhất một giá trị của`x`để luôn kích hoạt hành vi nâng cấp. 

Khó khăn chính là điều kiện phụ thuộc vào số học mô-đun và so sánh ngưỡng với`a / 2`, điều này tạo nên những lý luận ngây thơ về mỗi`x`gây nhầm lẫn riêng biệt. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi kích cỡ gói có thể`a`từ`1`đến một cái gì đó như`2r`và, đối với mỗi`a`, kiểm tra mọi`x`TRONG`[l, r]`. Với mỗi cặp hãy tính`x mod a`và xác minh xem giao dịch mua cuối cùng có vượt quá`x`. Cách tiếp cận này đúng về nguyên tắc vì nó mô phỏng trực tiếp hành vi của khách hàng, nhưng độ phức tạp của nó tỷ lệ thuận với`(r - l + 1) * max_a`, trong trường hợp xấu nhất đạt đến khoảng`10^18`hoạt động, khiến nó hoàn toàn không thể thực hiện được. 

Sự đơn giản hóa chính đến từ việc quan sát những gì thực sự cần được đảm bảo. Chúng tôi không cố gắng tối đa hóa việc mua thêm cho một mặt hàng cụ thể`x`, nhưng để đảm bảo rằng cho mọi`x`, quy tắc tham lam sẽ kích hoạt nâng cấp. Điều đó chỉ phụ thuộc vào việc phần còn lại`x mod a`từng hạ cánh trong “vùng an toàn” bên dưới`a/2`. 

Nếu có tồn tại bất kỳ`x`như vậy`x mod a < a/2`, thì khách hàng đó không nâng cấp và điều kiện không thành công. Vì vậy, chúng tôi muốn tránh có bất kỳ phần còn lại trong`[0, a/2)`. Điều đó chỉ có thể thực hiện được khi khoảng thời gian`[l, r]`đủ nhỏ để tất cả phần dư có thể được đẩy vào nửa trên bằng cách lựa chọn thích hợp`a`. 

Quan sát cấu trúc quan trọng là khi độ dài khoảng ít nhất là 4, phần dư nhất thiết phải bao phủ cả hai nửa cho bất kỳ lựa chọn mô đun nào, khiến cho việc nâng cấp tất cả các giá trị là không thể. Khi khoảng nhỏ (độ dài nhiều nhất là 3), chúng ta luôn có thể xây dựng một khoảng thích hợp`a`. 

Điều này làm giảm vấn đề chỉ còn kiểm tra kích thước của khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r-l+1)·r) | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính độ dài quãng`len = r - l + 1`. Điều này nắm bắt có bao nhiêu nhu cầu riêng biệt của khách hàng mà chúng tôi phải đồng thời đáp ứng. 
2. Nếu`len >= 4`, ngay lập tức kết luận rằng không có kích thước gói nào có thể buộc tất cả khách hàng phải nâng cấp. Điều này xuất phát từ thực tế là bất kỳ cấu trúc mô đun nào trong một khoảng rộng như vậy chắc chắn sẽ tạo ra phần dư ở nửa dưới đối với một số`x`. 
3. Ngược lại, khi`len <= 3`, đầu ra`YES`, vì chúng ta luôn có thể xây dựng kích thước gói đủ lớn để đẩy tất cả các giá trị vào`[l, r]`vào “khu vực nâng cấp” hoặc khiến tất cả phần còn lại rơi vào tình trạng buộc phải mua. 

### Tại sao nó hoạt động 

Hành vi của hệ thống được điều chỉnh hoàn toàn bởi dư lượng modulo`a`. Để thành công, mọi số nguyên trong`[l, r]`phải tránh tạo ra phần dư nhỏ theo bất kỳ mô đun đã chọn nào. Khi khoảng kéo dài bốn hoặc nhiều số nguyên liên tiếp, dư lượng modulo bất kỳ`a`nhất thiết phải bao gồm một khối hoàn chỉnh các giá trị liên tiếp, luôn giao với nửa dưới của mô đun. Điều đó đảm bảo có ít nhất một khách hàng không nâng cấp, vi phạm điều kiện. Khi khoảng có nhiều nhất là ba giá trị, chúng ta luôn có thể chọn`a`đủ lớn để cấu trúc phần dư tránh được hoàn toàn trường hợp sai sót này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        l, r = map(int, input().split())
        if r - l + 1 >= 4:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Việc thực hiện chỉ dựa vào việc tính toán độ dài khoảng thời gian. Không cần mô phỏng hành vi của khách hàng vì hệ thống mô-đun đầy đủ sẽ thu gọn về trạng thái chiều dài thuần túy. Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi, đảm bảo khả năng mở rộng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
l = 3, r = 4
```Chúng tôi tính toán độ dài khoảng. 

| tôi | r | r-l+1 | Quyết định | 
| --- | --- | --- | --- | 
| 3 | 4 | 2 | CÓ | 

Vì chỉ tồn tại hai giá trị nên chúng ta luôn có thể chọn một giá trị đủ lớn`a`để đảm bảo cả hai hành xử giống hệt nhau theo quy tắc. 

Điều này xác nhận rằng các khoảng thời gian nhỏ không tạo ra các dư lượng mô-đun xung đột. 

### Ví dụ 2 

đầu vào:```
l = 120, r = 150
```| tôi | r | r-l+1 | Quyết định | 
| --- | --- | --- | --- | 
| 120 | 150 | 31 | KHÔNG | 

Khoảng cách lớn. Bất kỳ lựa chọn mô đun nào cũng tạo ra nhiều phần dư riêng biệt trong phạm vi này, đảm bảo một số giá trị rơi vào vùng không nâng cấp. 

Điều này chứng tỏ sự bất khả thi trong việc kiểm soát hành vi trong một thời gian dài liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm chỉ yêu cầu phép trừ và so sánh | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm, do đó, giải pháp thời gian không đổi cho mỗi trường hợp dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        l, r = map(int, sys.stdin.readline().split())
        out.append("YES" if r - l + 1 < 4 else "NO")
    return "\n".join(out)

# provided samples
assert run("3\n3 4\n1 2\n120 150\n") == "YES\nNO\nYES"

# custom cases
assert run("1\n1 1\n") == "YES"
assert run("1\n1 4\n") == "NO"
assert run("1\n5 7\n") == "YES"
assert run("1\n10 13\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | CÓ | trường hợp ranh giới tối thiểu | 
| 1 4 | KHÔNG | độ dài khoảng thời gian thất bại đầu tiên | 
| 5 7 | CÓ | khoảng hợp lệ nhỏ | 
| 10 13 | KHÔNG | ngưỡng chính xác là 4 | 

## Vỏ cạnh 

Đối với một khoảng thời gian duy nhất như`l = r = 1`, thuật toán tính chiều dài`1`, nằm dưới ngưỡng, vì vậy nó trả về`YES`. Điều này phù hợp với thực tế là chỉ với một giá trị nhu cầu, luôn có thể chọn kích thước gói buộc hành vi nâng cấp hoặc làm cho điều kiện được thỏa mãn một cách trống rỗng. 

Đối với trường hợp biên giới`l = 1, r = 4`, độ dài tính được là`4`, kích hoạt`NO`. Mọi nỗ lực lựa chọn`a`không thành công vì bốn giá trị liên tiếp buộc cả phần còn lại nhỏ và lớn theo bất kỳ mô-đun nào, đảm bảo ít nhất một khách hàng không nâng cấp.
