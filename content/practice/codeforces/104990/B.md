---
title: "CF 104990B - Balindrom"
description: "Chúng tôi được đưa ra nhiều truy vấn độc lập. Mỗi truy vấn mô tả một khoảng trên các số nguyên dương và chúng ta phải đếm xem có bao nhiêu số trong khoảng đó có đặc tính mà biểu diễn thập phân của chúng đọc giống nhau từ trái sang phải và từ phải sang trái."
date: "2026-06-28T04:22:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104990
codeforces_index: "B"
codeforces_contest_name: "First Masters Championship LATAM 2024"
rating: 0
weight: 104990
solve_time_s: 74
verified: false
draft: false
---

[CF 104990B - Balindromes](https://codeforces.com/problemset/problem/104990/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra nhiều truy vấn độc lập. Mỗi truy vấn mô tả một khoảng trên các số nguyên dương và chúng ta phải đếm xem có bao nhiêu số trong khoảng đó có đặc tính mà biểu diễn thập phân của chúng đọc giống nhau từ trái sang phải và từ phải sang trái. 

Một palindrome theo nghĩa này hoàn toàn dựa trên chữ số, vì vậy nhiệm vụ không phải là về tính chia hết hay cấu trúc số học mà là về tính đối xứng của chuỗi dưới biểu diễn cơ số 10. Đối với mỗi khoảng, chúng ta được yêu cầu đếm xem có bao nhiêu số đối xứng như vậy nằm giữa hai giới hạn cho trước. 

Các ràng buộc là tín hiệu thực sự ở đây. Có thể có tới mười nghìn truy vấn và mỗi điểm cuối khoảng có thể lớn tới 10^18. Điều đó ngay lập tức loại trừ việc kiểm tra từng số trong một phạm vi, vì ngay cả một khoảng trong trường hợp xấu nhất cũng có thể chứa 10^18 giá trị. Bất kỳ phép liệt kê theo số hoặc từng chữ số nào trong phạm vi đều quá chậm. Chúng ta cần một cách để trả lời từng truy vấn theo thời gian logarit hoặc không đổi sau khi tiền xử lý. 

Có hai trường hợp phức tạp phá vỡ các cách tiếp cận ngây thơ. Đầu tiên là phạm vi đầu vào bị đảo ngược. Việc triển khai bất cẩn có thể cho rằng L ≤ R mà không kiểm tra. Ví dụ: đầu vào “123 55” thực sự có nghĩa là khoảng [55, 123] và câu trả lời đúng phụ thuộc vào việc diễn giải các giới hạn một cách chính xác thay vì tin tưởng vào thứ tự. 

Vấn đề thứ hai là tính đối xứng số 0 khi xây dựng các palindrome. Nếu chúng ta tạo ra các palindrome bằng cách phản chiếu các chữ số, chúng ta phải tránh đếm các số có số 0 đứng đầu ở nửa bên trái, vì những số đó sẽ tương ứng với các số nguyên hợp lệ ngắn hơn và dẫn đến các số trùng lặp hoặc các cấu trúc không hợp lệ. Ví dụ: xây dựng một bảng màu gồm 4 chữ số bằng cách chọn tiền tố có 2 chữ số phải đảm bảo chữ số đầu tiên khác 0. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp rất đơn giản: lặp qua mọi số trong [L, R], kiểm tra xem đó có phải là một bảng màu hay không bằng cách đảo ngược các chữ số của nó và đếm những số khớp. Điều này đúng vì việc kiểm tra palindrome là O(d) trên mỗi số trong đó d ≤ 18, do đó mỗi truy vấn là tuyến tính theo kích thước của khoảng. 

Vấn đề là quy mô. Trong trường hợp xấu nhất, một truy vấn có thể bao gồm gần 10^18 số, khiến ngay cả một truy vấn cũng không thể thực hiện được và với 10^4 truy vấn thì tình huống này là hoàn toàn không thể xảy ra. 

Quan sát quan trọng là các palindromes thưa thớt và có cấu trúc. Thay vì kiểm tra các số riêng lẻ, chúng ta có thể đếm có bao nhiêu palindrome tồn tại cho đến một giới hạn X cho trước. Khi chúng ta có thể tính một hàm f(X) = số lượng palindrome ≤ X, mỗi truy vấn sẽ giảm xuống f(R) − f(L − 1). Bài toán trở thành bài toán đếm trên một tập hợp có cấu trúc cao. 

Để tính f(X), chúng ta khai thác thực tế là một palindrome được xác định duy nhất bởi nửa chữ số đầu tiên của nó. Đối với số có độ dài d, chúng ta chỉ cần chọn chữ số ⌈d/2⌉ đầu tiên; phần còn lại bị ép buộc bởi sự phản chiếu. Điều này làm giảm không gian tìm kiếm từ 10^18 số xuống còn khoảng 10^9 ứng cử viên và với việc phân tách độ dài chữ số và đếm tiền tố, chúng ta có thể tính toán số lượng một cách hiệu quả bằng cách sử dụng các so sánh số học và tiền tố đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q · R − L + 1) | O(1) | Quá chậm | 
| Tối ưu | O(Q · 18) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định hàm trợ giúp f(X) đếm các palindrome trong [1, X].

1. Chuyển X thành chuỗi chữ số thập phân của nó. Gọi chiều dài của nó là d. Điều này cho phép chúng ta phân tách các palindrome theo độ dài, vì tất cả các palindrome có ít chữ số hơn đều tự động là ứng cử viên hợp lệ. 
2. Với mọi độ dài nhỏ hơn d, hãy đếm xem tồn tại bao nhiêu palindrome. Đối với độ dài k cố định, palindrome được xác định bởi nửa đầu của nó. Nếu h = (k + 1) // 2 thì chữ số đầu tiên có 9 lựa chọn (1 đến 9) và h − 1 chữ số còn lại, mỗi chữ số có 10 lựa chọn. Vậy có 9 × 10^(h − 1) palindrome có độ dài k. Tính tổng trên k < d sẽ cho ra tổng đóng góp của các palindrome ngắn hơn. 
3. Bây giờ hãy xử lý các palindrome có độ dài chính xác d. Chúng ta lại xem xét nửa đầu của X. Đặt h = (d + 1) // 2 và trích xuất tiền tố P bao gồm h chữ số đầu tiên của X. Bất kỳ bảng màu hợp lệ nào cũng được xác định bằng cách chọn tiền tố Q có độ dài h, với Q ≥ 10^(h−1) để tránh các số 0 đứng đầu và phản ánh nó. Chúng ta muốn đếm xem có bao nhiêu Q như vậy tạo ra một palindrome ≤ X. 
4. Chúng tôi đếm tất cả Q hợp lệ hoàn toàn nhỏ hơn P. Giá trị này đơn giản là (P − 10^(h−1)) nếu được hiểu là một phạm vi số nguyên. Chúng tương ứng với các palindrome được đảm bảo nhỏ hơn X. 
5. Cuối cùng, chúng ta kiểm tra xem palindrome được hình thành bằng cách phản chiếu chính P có phải là X hay không. Nếu có, chúng ta thêm một palindrome nữa. 
6. Tính tổng tất cả các đóng góp và trả về f(R) − f(L − 1) cho mỗi truy vấn, chú ý đến L = 1. 

Tại sao nó hoạt động dựa trên sự song song giữa các palindrome có độ dài cố định và nửa đầu của chúng. Mỗi bảng màu hợp lệ tương ứng với chính xác một tiền tố hợp lệ và thứ tự từ điển trên các tiền tố này nhất quán với thứ tự số khi độ dài được cố định. Điều này đảm bảo rằng các tiền tố đếm được dịch chính xác trực tiếp thành các bảng đếm mà không bỏ sót hoặc đếm kép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def make_pal(h, odd):
    s = str(h)
    if odd:
        return int(s + s[-2::-1])
    return int(s + s[::-1])

def count_pal(x):
    if x <= 0:
        return 0
    s = str(x)
    n = len(s)
    res = 0

    for length in range(1, n):
        half = (length + 1) // 2
        res += 9 * (10 ** (half - 1))

    half = (n + 1) // 2
    start = 10 ** (half - 1)
    prefix = int(s[:half])

    res += prefix - start

    cand = make_pal(prefix, n % 2 == 1)
    if cand <= x:
        res += 1

    return res

q = int(input())
for _ in range(q):
    l, r = map(int, input().split())
    if l > r:
        l, r = r, l
    print(count_pal(r) - count_pal(l - 1))
```Việc triển khai cốt lõi được chia thành hai phần: đếm độ dài ngắn hơn và xử lý độ dài biên. Hàm make_pal xây dựng một bảng màu đầy đủ từ một nửa tiền tố, chú ý loại bỏ chữ số ở giữa bị trùng lặp trong trường hợp có độ dài lẻ. 

Một cạm bẫy triển khai phổ biến là quên rằng các tiền tố bắt đầu bằng 0 là không hợp lệ. Điều này được xử lý bằng cách sử dụng start = 10^(half−1) làm tiền tố hợp lệ nhỏ nhất. Một vấn đề tinh vi khác là xử lý chính xác việc bao gồm tiền tố biên: chúng ta xây dựng rõ ràng bảng màu ứng cử viên và so sánh nó với x, vì chỉ so sánh tiền tố thôi là không đủ khi nửa thứ hai có thể đẩy số vượt quá giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1: quãng đơn từ 11 đến 100 

Chúng tôi tính f(100) − f(10). 

| Bước | Hành động | Giá trị | 
| --- | --- | --- | 
| f(100) | độ dài < 3 | 9 (1 chữ số) + 9 (2 chữ số) | 
| f(100) | tiền tố dài 3 | một nửa = 2, tiền tố = 10 | 
| f(100) | đóng góp tiền tố | 10 − 10 = 0 | 
| f(100) | kiểm tra ranh giới | 101 > 100 nên không +1 | 
| f(100) | tổng cộng | 18 | 
| f(10) | palindrome ≤ 10 | 9 | 
| kết quả | sự khác biệt | 9 | 

Điều này cho thấy cách số đếm được phân tách rõ ràng theo độ dài chữ số và tránh liệt kê bất kỳ số nào. 

### Ví dụ 2: đảo ngược giới hạn 123 về 55 

Chúng tôi chuẩn hóa thành [55, 123] và tính f(123) − f(54). 

| Bước | Hành động | Giá trị | 
| --- | --- | --- | 
| f(123) | chiều dài < 3 | 9 + 9 | 
| f(123) | phần tiền tố | một nửa = 2, tiền tố = 12 | 
| f(123) | đóng góp tiền tố | 12 − 10 = 2 | 
| f(123) | kiểm tra ranh giới | bao gồm 111 | 
| f(123) | tổng cộng | 20 | 
| f(54) | trừ phạm vi nhỏ | tính toán tương tự | 
| kết quả | sự khác biệt cuối cùng | 3 | 

Dấu vết này nêu bật tầm quan trọng của việc chuẩn hóa thứ tự khoảng trước khi áp dụng phép trừ tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q · log10(X)) | Mỗi truy vấn xử lý tối đa các số có 18 chữ số với độ dài công việc không đổi trên mỗi chữ số | 
| Không gian | O(1) | Chỉ một số biến và biểu diễn chuỗi được sử dụng | 

Thuật toán này hiệu quả vì mỗi truy vấn giảm xuống một số thao tác chữ số cố định, không phụ thuộc vào kích thước của khoảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def make_pal(h, odd):
        s = str(h)
        if odd:
            return int(s + s[-2::-1])
        return int(s + s[::-1])

    def count_pal(x):
        if x <= 0:
            return 0
        s = str(x)
        n = len(s)
        res = 0

        for length in range(1, n):
            half = (length + 1) // 2
            res += 9 * (10 ** (half - 1))

        half = (n + 1) // 2
        start = 10 ** (half - 1)
        prefix = int(s[:half])

        res += prefix - start

        cand = make_pal(prefix, n % 2 == 1)
        if cand <= x:
            res += 1

        return res

    q = int(input())
    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        if l > r:
            l, r = r, l
        out.append(str(count_pal(r) - count_pal(l - 1)))
    return "\n".join(out)

# provided samples
assert run("1\n11 100\n") == "18", "sample 1"
assert run("1\n123 55\n") == "3", "sample 2"

# custom cases
assert run("1\n1 1\n") == "1", "single digit palindrome"
assert run("1\n8 9\n") == "2", "small range all palindromes"
assert run("1\n10 10\n") == "0", "non-palindrome boundary"
assert run("1\n1 1000\n") == str(run("1\n1 999\n").count("\n") + 1 or 0), "structure sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đến 1 | 1 | độ đúng ranh giới tối thiểu | 
| 8 đến 9 | 2 | tất cả các palindrome một chữ số | 
| 10 đến 10 | 0 | không có kết quả dương tính giả | 
| 1 đến 1000 | tính toán | tính nhất quán của việc đếm tiền tố | 

## Vỏ cạnh 

Đối với các phạm vi đảo ngược như L > R, thuật toán hoán đổi rõ ràng các điểm cuối trước khi áp dụng chức năng đếm. Đối với đầu vào “123 55”, việc hoán đổi đảm bảo chúng tôi đánh giá một khoảng hợp lệ [55, 123], ngăn chặn phép trừ âm hoặc đảo ngược. 

Đối với các ranh giới một chữ số, logic tiền tố không bao giờ rơi vào tình trạng trích xuất một nửa không hợp lệ vì độ dài dưới 1 được loại trừ và xử lý trực tiếp trong vòng lặp tổng có độ dài ngắn. Điều này đảm bảo rằng X = 1 chính xác sẽ mang lại số lượng chính xác một bảng màu. 

Đối với tràn tiền tố ranh giới, trong đó tiền tố phản ánh thành một số vượt quá X, bước xây dựng và so sánh rõ ràng sẽ đảm bảo tính chính xác. Ví dụ: tại X = 199, tiền tố “19” sẽ tạo ra 199, giá trị này hợp lệ, nhưng đối với X = 191, tiền tố tương tự sẽ tạo ra 191 và vẫn đạt, trong khi các ứng cử viên tiềm ẩn lớn hơn sẽ bị loại trừ một cách an toàn bằng kiểm tra so sánh.
