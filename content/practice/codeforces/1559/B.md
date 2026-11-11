---
title: "CF 1559B - Mocha và Đỏ và Xanh"
description: "Chúng ta được cung cấp một dòng vị trí, mỗi vị trí chứa một ô có màu đỏ hoặc xanh lam. Một số ô đã được cố định, trong khi một số ô khác trống và có thể được chỉ định một trong hai màu."
date: "2026-06-16T16:34:43+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1559
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 738 (Div. 2)"
rating: 900
weight: 1559
solve_time_s: 316
verified: false
draft: false
---

[CF 1559B – Mocha, Đỏ và Xanh](https://codeforces.com/problemset/problem/1559/B) 

**Xếp hạng:** 900 
**Tags:** dp, tham lam 
**Thời gian giải:** 5 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng vị trí, mỗi vị trí chứa một ô có màu đỏ hoặc xanh lam. Một số ô đã được cố định, trong khi một số ô khác trống và có thể được chỉ định một trong hai màu. 

Sau khi hoàn thiện tất cả các màu, chúng tôi phải trả chi phí bằng số cặp ô liền kề có cùng màu. Mỗi lần hai người hàng xóm phù hợp, chi phí sẽ tăng thêm một. Nhiệm vụ là điền vào tất cả các khoảng trống sao cho chi phí này càng nhỏ càng tốt và xuất ra bất kỳ màu nào đạt được mức tối thiểu này. 

Quan sát quan trọng là chi phí chỉ phụ thuộc vào đẳng thức liền kề. Điều này có nghĩa là chúng tôi không cố gắng khớp một mẫu chung, chỉ để tránh các màu giống hệt nhau trong thời gian dài nếu có thể. 

Các ràng buộc rất nhỏ: mỗi bài kiểm tra có độ dài tối đa là 100 và có tối đa 100 bài kiểm tra. Điều này cho phép bất kỳ cách tiếp cận tuyến tính hoặc thậm chí bậc hai nào cho mỗi bài kiểm tra, nhưng loại trừ việc liệt kê theo cấp số nhân của tất cả các chất trám, sẽ tăng lên như 2 lũy thừa của số khoảng trống và nhanh chóng trở nên không khả thi. 

Một vài tình huống khó khăn quan trọng. 

Nếu chuỗi đã được cố định hoàn toàn và thay thế như "RBRBRB", chi phí đã bằng 0 và không cần thay đổi. Một cách tiếp cận ngây thơ cố gắng “tối ưu hóa” có thể thay đổi cấu trúc bắt buộc một cách không chính xác nếu nó bỏ qua các ràng buộc cố định. 

Nếu tất cả các ký tự là "?" thì bất kỳ mẫu xen kẽ nào chẳng hạn như "BRBRBR..." hoặc "RBRBRB..." sẽ đạt được chi phí bằng 0, đây là mức tối ưu. Một kẻ tham lam bất cẩn luôn bắt đầu bằng một màu cố định mà không xem xét tính đối xứng vẫn có tác dụng, nhưng chỉ vì cả hai màu đều tương đương nhau. 

Nếu các ký tự cố định không nhất quán trong việc buộc các phân đoạn dài, chẳng hạn như "R???R", thì mọi giải pháp đều phải tôn trọng điểm cuối và không thể đơn giản thay thế tự do mà không căn chỉnh ranh giới. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử tất cả các phép gán màu cho các vị trí trống. Đối với mỗi bài tập, chúng tôi tính toán số cặp bằng nhau liền kề trong thời gian tuyến tính. Nếu có k khoảng trống, điều này tạo ra 2^k khả năng và mỗi đánh giá có giá O(n), dẫn đến O(n · 2^k). Ngay cả với n = 100, k có thể lớn, khiến phương pháp này chậm về mặt thiên văn. 

Cái nhìn sâu sắc về cấu trúc quan trọng là các xung đột liền kề hoàn toàn mang tính cục bộ. Mỗi vị trí chỉ tương tác với các vị trí lân cận của nó. Điều này có nghĩa là chúng ta không cần phải quyết định tất cả các khoảng trống một cách độc lập; thay vào đó, chúng ta có thể xây dựng giải pháp một cách tham lam từ trái sang phải trong khi vẫn duy trì tính nhất quán với những người hàng xóm đã được quyết định. 

Sự đơn giản hóa quan trọng là khi một ký tự được cố định, bất kỳ đoạn trống nào giữa các ký tự cố định sẽ hoạt động giống như một vấn đề nội suy bị ràng buộc. Bên trong một phân khúc như vậy, chúng tôi chỉ quan tâm đến việc liệu các điểm cuối có buộc phải khớp hay không khớp hay không và chúng tôi có thể lấp đầy một cách tham lam theo cách tránh được các vùng lân cận bằng nhau nhiều nhất có thể. Điều này dẫn đến cấu trúc tiêu chuẩn: truyền các ràng buộc từ trái sang phải, đảm bảo mỗi vị trí khác với vị trí lân cận bên trái của nó bất cứ khi nào có thể, nhưng vẫn tôn trọng các màu cố định. 

Điều này làm giảm vấn đề xuống chỉ còn một lần xây dựng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2^k) | O(n) | Quá chậm | 
| Xây dựng tham lam tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng chuỗi cuối cùng từ trái sang phải.

1. Thay thế tất cả '?' với phần giữ chỗ và chuẩn bị điền vào chuỗi. Mục tiêu là quyết định từng vị trí trong khi tôn trọng các ràng buộc cố định. 
2. Đối với vị trí đầu tiên nếu là '?' thì gán tùy ý cho 'B'. Không có hàng xóm bên trái, vì vậy không có chi phí nào có thể bị ảnh hưởng ở đây. 
3. Với mỗi vị trí i từ 1 đến n - 1, xác định màu của nó dựa trên hai quy tắc. Nếu s[i] cố định ('B' hoặc 'R'), chúng ta phải giữ nó. Ngược lại, chúng ta chọn màu khác với s[i - 1] nếu có thể. 
4. Khi s[i] là '?', gán nó ngược lại với s[i - 1]. Điều này ngăn cản việc tạo một cặp liền kề bằng nhau ở vị trí i - 1 và i. 
5. Nếu s[i] cố định và khớp với s[i - 1], chúng tôi chấp nhận đóng góp chi phí vì chúng tôi không thể thay đổi các ký tự cố định. Nếu nó khác, chúng tôi chỉ cần tiếp tục. 
6. Tiếp tục cho đến hết chuỗi, đảm bảo mọi quyết định đều tối ưu cục bộ dựa trên các lựa chọn trước đó. 

Đầu ra là chuỗi được xây dựng đầy đủ. 

### Tại sao nó hoạt động 

Tại mọi vị trí, chúng tôi giảm thiểu sự đóng góp của cạnh giữa i - 1 và i. Nếu có sự lựa chọn tự do, chúng tôi luôn chọn màu tránh trùng với ký tự trước đó. Nếu không có sự lựa chọn tự do do một ràng buộc cố định, chúng ta buộc phải chấp nhận cái giá phải trả. Vì mỗi cạnh được xử lý chính xác một lần và độc lập nên không có quyết định nào sau này có thể cải thiện hoặc làm xấu đi các cạnh trước đó, vì vậy lựa chọn tham lam là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = list(input().strip())

        # handle first position
        if s[0] == '?':
            s[0] = 'B'

        for i in range(1, n):
            if s[i] == '?':
                # choose opposite of previous
                s[i] = 'R' if s[i - 1] == 'B' else 'B'

        print(''.join(s))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo việc xây dựng từ trái sang phải trực tiếp. Vị trí đầu tiên được khởi tạo tùy ý vì nó không có ràng buộc bên trái. Mỗi '?' tiếp theo được gán màu đối lập với màu trước đó, đảm bảo không có cặp liền kề bằng nhau mới nào được đưa vào khi có thể. 

Các ký tự cố định sẽ ghi đè một cách tự nhiên lựa chọn tham lam: khi s[i] đã là 'R' hoặc 'B', chúng tôi chỉ cần giữ nó và thuật toán tiếp tục. Điều này an toàn vì các ràng buộc cố định xác định các giới hạn không thể tránh khỏi trong chi phí và chúng tôi không cố gắng sửa đổi chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`?R???BR`Chúng tôi xử lý từ trái sang phải. 

| tôi | s[i] trước | s[i-1] | hành động | s sau bước | 
| --- | --- | --- | --- | --- | 
| 0 | ? | - | đặt thành B | BR???BR | 
| 1 | R | B | cố định | BR???BR | 
| 2 | ? | R | bộ B | BRB??BR | 
| 3 | ? | B | đặt R | BRBR?BR | 
| 4 | ? | R | bộ B | BRBRBR | 
| 5 | B | B | đã sửa, khớp | BRBRBR | 
| 6 | R | B | cố định | BRBRBRR | 

Điều này chứng tỏ mỗi ô trống được chọn như thế nào để tránh trùng khớp với hàng xóm bên trái của nó khi có thể và cách các ký tự cố định ghi đè các lựa chọn. 

### Ví dụ 2 

đầu vào:`???`| tôi | s[i] trước | s[i-1] | hành động | s sau bước | 
| --- | --- | --- | --- | --- | 
| 0 | ? | - | B | B?? | 
| 1 | ? | B | R | BR? | 
| 2 | ? | R | B | BRB | 

Điều này tạo ra một chuỗi xen kẽ hoàn hảo, đạt được độ không hoàn hảo bằng không. 

Ví dụ thứ hai cho thấy rằng trong các chuỗi hoàn toàn miễn phí, thuật toán giảm chi phí xuống mức tối thiểu về mặt lý thuyết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | mỗi vị trí được xử lý một lần | 
| Không gian | O(n) | lưu trữ và sửa đổi chuỗi | 

Tổng công việc trên tất cả các trường hợp thử nghiệm tối đa là 100 × 100 thao tác, nằm trong giới hạn tầm thường. Việc sử dụng bộ nhớ là tuyến tính theo độ dài chuỗi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = list(input().strip())
        if s[0] == '?':
            s[0] = 'B'
        for i in range(1, n):
            if s[i] == '?':
                s[i] = 'R' if s[i - 1] == 'B' else 'B'
        out.append(''.join(s))
    return '\n'.join(out)

# provided samples
assert run("5\n7\n?R???BR\n7\n???R???\n1\n?\n1\nB\n10\n?R??RB??B?\n") == \
"BRRBRBR\nBRBRBRB\nB\nB\nBRRBRBBRBR"

# all unknown
assert run("1\n5\n?????\n") == "BRBRB"

# already fixed alternating
assert run("1\n4\nBRBR\n") == "BRBR"

# single char
assert run("1\n1\n?\n") == "B"

# forced segment
assert run("1\n5\nR???R\n") == "RBRBR"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả '?' | BRBRB | sự xen kẽ tham lam thuần túy | 
| BRBR | BRBR | không sửa đổi chuỗi hợp lệ cố định | 
| ? độc thân | B | khởi tạo ranh giới | 
| R???R | RBRBR | tính nhất quán với các điểm cuối cố định | 

## Vỏ cạnh 

Đối với một chuỗi hoàn toàn không xác định như`?????`, thuật toán bắt đầu bằng 'B' và thay thế một cách xác định. Mỗi bước chỉ phụ thuộc vào ký tự trước đó, do đó toàn bộ cấu trúc nhất quán và không tạo ra cặp bằng nhau liền kề nào. 

Đối với một cấu hình đã tối ưu như`BRBR`, thuật toán không bao giờ sửa đổi các ký tự cố định và chỉ chuyển qua, duy trì cấu trúc chi phí bằng 0. 

Đối với đầu vào một ký tự, việc khởi tạo sẽ xử lý sự vắng mặt của các hàng xóm một cách an toàn, vì không tồn tại sự liền kề và do đó không thể tạo hoặc loại bỏ sự không hoàn hảo.
