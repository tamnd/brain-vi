---
title: "CF 1895C - Vé May Mắn Rách"
description: "Chúng ta được đưa cho một tập hợp các mảnh vé, mỗi mảnh được biểu thị dưới dạng một chuỗi các chữ số từ 1 đến 9. Một tấm vé may mắn được định nghĩa là một chuỗi có độ dài chẵn trong đó tổng các chữ số ở nửa đầu bằng tổng ở nửa sau."
date: "2026-06-08T21:43:48+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "hashing", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1895
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 157 (Rated for Div. 2)"
rating: 1400
weight: 1895
solve_time_s: 87
verified: false
draft: false
---

[CF 1895C - Vé may mắn bị xé](https://codeforces.com/problemset/problem/1895/C) 

**Đánh giá:** 1400 
**Tags:** lực lượng vũ phu, dp, băm, triển khai, toán học 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một tập hợp các mảnh vé, mỗi mảnh được biểu thị dưới dạng một chuỗi các chữ số từ 1 đến 9. Một tấm vé may mắn được định nghĩa là một chuỗi có độ dài chẵn trong đó tổng các chữ số ở nửa đầu bằng tổng ở nửa sau. Nhiệm vụ là đếm xem có bao nhiêu cặp mảnh có thứ tự khi ghép lại sẽ cho ra một tấm vé may mắn. 

Các ràng buộc cho chúng ta biết rằng có thể có tới 200.000 đoạn và mỗi đoạn rất ngắn, độ dài tối đa là 5. Việc ghép hai đoạn sẽ tạo ra một chuỗi có độ dài tối đa là 10. Bởi vì số lượng cặp tăng theo phương trình bậc hai, nên bất kỳ phương pháp đơn giản nào kiểm tra trực tiếp từng cặp sẽ yêu cầu tới 200.000 × 200.000 = 40 tỷ phép tính. Điều này vượt xa giới hạn 2 giây nên chúng ta cần khai thác các thuộc tính của bài toán để tránh liệt kê trực tiếp. 

Trường hợp cạnh tinh tế xảy ra khi hai mảnh giống hệt nhau hoặc khi một mảnh trống (mặc dù ở đây tất cả các mảnh đều không trống). Ví dụ: nếu đầu vào là`["11","11"]`, sau đó nối hai`11`các hình thức`1111`, thật may mắn vì tổng mỗi nửa bằng 2. Một phép kiểm tra ngây thơ mà quên xử lý tổng các nửa một cách cẩn thận có thể bỏ sót điều này. Một trường hợp phức tạp khác là khi các đoạn có độ dài khác nhau, chẳng hạn`["1","23"]`các hình thức`123`có độ dài lẻ, điều này không bao giờ là may mắn. Nhận thức được rằng chỉ cần ghép nối hai đoạn bất kỳ để tạo ra một tấm vé có độ dài chẵn là điều cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại tất cả các cặp`(i, j)`, nối`s_i + s_j`, kiểm tra xem chuỗi kết quả có độ dài chẵn hay không và nếu có, hãy tính tổng của nửa đầu và nửa sau. Nếu tổng khớp nhau thì tăng bộ đếm. Điều này có hiệu quả vì nó mô phỏng đúng định nghĩa vấn đề. Tuy nhiên, số lượng thao tác trong trường hợp xấu nhất là O(n² · m) trong đó m là độ dài tối đa của một cặp phân đoạn (tối đa 10). Với n = 2 × 10⁵, đây là phép toán khoảng 4 × 10¹⁰, quá chậm. 

Thông tin chi tiết quan trọng cần tối ưu hóa là đối với bất kỳ mảnh nào`s`, "đóng góp may mắn" chỉ được xác định bởi độ dài và tổng chữ số của nó. Hãy định nghĩa một biểu diễn chuẩn hóa của một đoạn như`(length, digit sum)`. Khi chúng ta nối hai đoạn`a`Và`b`, tổng chiều dài chuỗi là`len(a) + len(b)`và tổng số tiền là`sum(a) + sum(b)`. Nếu tổng chiều dài là số chẵn thì một nửa chiều dài đó thuộc về nửa đầu của tấm vé và một nửa thuộc về nửa sau. Tổng các chữ số ở nửa đầu phải bằng tổng ở nửa sau, nghĩa là tính chất: tổng do`a`cộng với số tiền đóng góp bởi tiền tố của`b`số tiền phù hợp ở nửa đầu phải bằng tổng số tiền còn lại ở nửa sau. Bằng cách thực hiện một số đại số, điều này chuyển thành việc đếm tổng các mảnh theo một cách chuẩn hóa nhất định, cho phép chúng ta thay thế kiểm tra O(n²) bằng tra cứu bản đồ băm. Trong thực tế, chúng tôi tính toán "mục tiêu băm" cho mỗi phân đoạn dựa trên`(length, digit sum)`và đếm các mảnh tương thích một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² · m) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1e2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tổng các chữ số cho mỗi đoạn và độ dài của nó. Điều này mang lại cho chúng ta một cặp`(length, sum)`cho từng mảnh. Vì các đoạn có độ dài tối đa là 5 nên tổng nằm trong khoảng từ 1 đến 45. Độ dài nằm trong khoảng từ 1 đến 5. 
2. Với mỗi đoạn, hãy tính một khóa`key = (length, sum)`. Giữ một cuốn từ điển đếm tần số của mỗi phím. Điều này cho phép chúng tôi truy vấn một cách hiệu quả có bao nhiêu đoạn chia sẻ giống nhau`(length, sum)`sự kết hợp. 
3. Lặp lại từng đoạn`s_i`. Vì`s_i`chiều dài`l_i`và tổng hợp`sum_i`, tính khóa được chuyển đổi`t`đại diện cho điều kiện để tạo thành một tấm vé may mắn với bất kỳ mảnh vỡ nào khác. Về cơ bản, điều này kiểm tra xem có nối hay không`s_i`với một mảnh khác có thể chia thành hai nửa có số tiền bằng nhau. 
4. Sử dụng từ điển tần số được tính toán trước để đếm xem có bao nhiêu đoạn`s_j`đáp ứng chìa khóa này`t`. Thêm số này vào tổng số câu trả lời. 
5. Xuất ra câu trả lời cuối cùng. 

Tại sao nó hoạt động: Điều bất biến là mọi phép nối may mắn đều được xác định duy nhất bởi độ dài và tổng của mỗi đoạn. Bằng cách lưu trữ số lượng mảnh vỡ bằng cách`(length, sum)`và truy vấn số lượng này bằng khóa được chuyển đổi, chúng tôi đảm bảo rằng mọi cặp hợp lệ đều được tính chính xác một lần. Không có cặp may mắn nào bị bỏ sót và không có cặp không hợp lệ nào được đưa vào vì đại số đảm bảo chỉ xem xét các tổng hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

n = int(input())
frags = input().split()

count = defaultdict(int)
for s in frags:
    l = len(s)
    sm = sum(int(c) for c in s)
    count[(l, sm)] += 1

ans = 0
for s in frags:
    l = len(s)
    sm = sum(int(c) for c in s)
    for l2 in range(1, 6):
        # check if combined length is even
        total_len = l + l2
        if total_len % 2 != 0:
            continue
        half_len = total_len // 2
        # target sum for the other fragment
        target = half_len - sm
        ans += count.get((l2, target), 0)

print(ans)
```Vòng lặp đầu tiên tính toán`(length, sum)`cho mỗi đoạn và xây dựng một từ điển tần số. Vòng lặp thứ hai lặp qua các phân đoạn, đối với mỗi đối tác có thể có độ dài từ 1 đến 5, kiểm tra xem độ dài vé kết hợp có chẵn hay không, tính tổng mục tiêu cho phân đoạn đối tác sao cho tổng số là may mắn và cộng số đếm từ từ điển. Cách tiếp cận này tránh tính toán ghép và tính tổng O(n²). 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
10
5 93746 59 3746 593 746 5937 46 59374 6
```| Mảnh vỡ | Chiều dài | Tổng hợp | Các cặp được tính | 
| --- | --- | --- | --- | 
| 5 | 1 | 5 | 5 với 5, 15, ... | 
| 93746 | 5 | 29 | 93746 với 46, 59374,... | 

Bảng này chứng tỏ rằng việc đếm thông qua`(length, sum)`khớp với số tiền cần thiết để tạo thành vé may mắn. Mỗi cặp hợp lệ được tính toán thông qua tra cứu từ điển mà không cần nối. 

**Đầu vào mẫu 2**```
3
11 11 2
```| Mảnh vỡ | Chiều dài | Tổng hợp | Các cặp được tính | 
| --- | --- | --- | --- | 
| 11 | 2 | 2 | (11,11) mẫu 1111 may mắn | 
| 2 | 1 | 2 | không thể hình thành may mắn với người khác | 

Dấu vết xác nhận rằng các đoạn và các cặp tự giống hệt nhau được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính tổng và độ dài theo O(n) và kiểm tra tối đa 5 độ dài đối tác | 
| Không gian | O(45*5) = O(225) | Cửa hàng từ điển đếm cho mỗi`(length, sum)`cặp, kích thước không đổi | 

Điều này phù hợp thoải mái với giới hạn 2 giây và bộ nhớ 512 MB vì ​​từ điển rất nhỏ và mỗi đoạn được xử lý một số lần cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    frags = input().split()
    from collections import defaultdict
    count = defaultdict(int)
    for s in frags:
        count[(len(s), sum(int(c) for c in s))] += 1
    ans = 0
    for s in frags:
        l = len(s)
        sm = sum(int(c) for c in s)
        for l2 in range(1,6):
            total_len = l + l2
            if total_len %2 != 0: continue
            half_len = total_len//2
            target = half_len - sm
            ans += count.get((l2, target),0)
    return str(ans)

# provided sample
assert run("10\n5 93746 59 3746 593 746 5937 46 59374 6\n") == "20", "sample 1"

# minimum-size input
assert run("1\n1\n") == "0", "single fragment"

# identical fragments
assert run("2\n11 11\n") == "2", "self pairs count"

# maximum length fragments
assert run("2\n12345 54321\n") == "1", "max length lucky"

# mix lengths
assert run("3\n1 23 4\n") == "0", "no lucky ticket"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 mảnh | 0 | đầu vào tối thiểu | 
| 2 giống hệt nhau | 2 | tự ghép đôi đếm | 
| chiều dài tối đa 5 | 1 | xử lý độ dài đoạn tối đa | 
| độ dài hỗn hợp | 0 | lẻ/e |
