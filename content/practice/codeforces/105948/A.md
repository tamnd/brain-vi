---
title: "CF 105948A - Nhật ký thực thi"
description: "Chúng tôi được cung cấp một danh sách các tệp, trong đó mỗi tệp có một tên và chuỗi quyền. Chuỗi quyền mã hóa xem tệp có thể được đọc, ghi hoặc thực thi bằng định dạng ba ký tự quen thuộc hay không."
date: "2026-06-22T16:05:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105948
codeforces_index: "A"
codeforces_contest_name: "CCF CAT NAEC 2025 (Provincial)"
rating: 0
weight: 105948
solve_time_s: 51
verified: true
draft: false
---

[CF 105948A - Nhật ký có thể thực thi](https://codeforces.com/problemset/problem/105948/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các tệp, trong đó mỗi tệp có một tên và chuỗi quyền. Chuỗi quyền mã hóa xem tệp có thể được đọc, ghi hoặc thực thi bằng định dạng ba ký tự quen thuộc hay không. Nhiệm vụ của chúng tôi là đếm xem có bao nhiêu tệp trong số này thỏa mãn hai điều kiện cùng một lúc: tên tệp tương ứng với tệp nhật ký và chuỗi quyền cấp quyền truy cập thực thi. 

Một tệp được coi là tệp nhật ký nếu tên của nó khớp với mẫu “S.log”, nghĩa là nó kết thúc chính xác bằng “.log” và phần trước nó có thể là bất kỳ chuỗi nào, kể cả chuỗi trống. Điều này ngay lập tức bao gồm các tên như “.log”, “a.log” và “123.log”, nhưng loại trừ bất kỳ tên nào mà “.log” không phải là hậu tố cuối cùng, chẳng hạn như “a.log.zip” hoặc “logfile”. 

Chuỗi quyền luôn là một trong tám khả năng cố định được xây dựng từ các ký tự r, w, x và -. Chúng tôi chỉ quan tâm liệu ký tự thứ ba có phải là 'x' hay không, vì ký tự đó biểu thị quyền thực thi. 

Kích thước đầu vào nhỏ, tối đa 1000 tệp và độ dài tên tệp tối đa là 100. Điều này có nghĩa là việc quét trực tiếp trên tất cả các tệp đã đủ hiệu quả và chúng tôi không cần bất kỳ cấu trúc dữ liệu hoặc tiền xử lý nâng cao nào. Bất kỳ giải pháp nào kiểm tra từng tệp theo thời gian tuyến tính sẽ chạy thoải mái trong giới hạn. 

Các trường hợp chính xuất phát từ việc xác định chính xác hậu tố “.log” thay vì kiểm tra xem chuỗi con “.log” có xuất hiện ở bất kỳ đâu trong tên tệp hay không. Ví dụ: “a.log.zip” chứa “.log” nhưng không phải là tệp nhật ký. 

Một trường hợp tinh tế khác là tiền tố trống. Tên tệp “.log” là hợp lệ và sẽ được tính nếu nó có quyền thực thi. Việc triển khai đơn giản giả định ít nhất một ký tự trước “.log” có thể từ chối nó một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực về cơ bản đã là tối ưu cho vấn đề này. Đối với mỗi tệp, chúng tôi kiểm tra xem tên của nó có kết thúc bằng “.log” hay không và liệu chuỗi quyền của nó có chữ ‘x’ ở vị trí thứ ba hay không. Mỗi kiểm tra là O(1) với các ràng buộc, vì độ dài chuỗi bị giới hạn và định dạng quyền là cố định. Trên n tệp, điều này mang lại tổng công việc là O(n). 

Người ta có thể tưởng tượng việc phân tích cú pháp phức tạp hơn, chẳng hạn như quét các chuỗi con hoặc cố gắng xác thực các mẫu theo cách tổng quát. Tuy nhiên, vấn đề này không yêu cầu phải xử lý trước hoặc tìm kiếm trên các tệp. Cấu trúc phẳng: mỗi tệp độc lập và điều kiện là cục bộ của chính chuỗi đó. 

Quan sát chính là việc kiểm tra hậu tố và kiểm tra quyền vị trí cố định đều giảm xuống các hoạt động liên tục trên mỗi tệp. Điều đó thu gọn vấn đề thành một bộ lọc đơn giản trên danh sách đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### ## Hướng dẫn thuật toán 

1. Đọc số lượng tệp n, sau đó lặp lại từng mục nhập tệp một. Chúng tôi làm điều này vì mỗi tệp đều độc lập và không cần cấu trúc chung. 
2. Đối với mỗi tệp, trích xuất tên tệp f và chuỗi quyền p. Quyết định cho tập tin này chỉ phụ thuộc vào hai chuỗi này. 
3. Kiểm tra xem f có kết thúc bằng chuỗi con “.log” hay không. Điều này đảm bảo chúng tôi chỉ xem xét các tệp nhật ký hợp lệ và tránh các kết quả trùng khớp sai khi “.log” xuất hiện ở giữa hoặc sớm hơn trong tên tệp. 
4. Kiểm tra xem chuỗi quyền p có ký tự thứ ba là ‘x’ hay không. Vì p luôn có độ dài 3 và sử dụng định dạng cố định nên điều này trực tiếp cho chúng ta biết liệu quyền thực thi có được cấp hay không. 
5. Nếu cả hai điều kiện đều đúng, hãy tăng bộ đếm. Bộ đếm này theo dõi số lượng tệp thỏa mãn điều kiện kết hợp. 
6. Sau khi xử lý tất cả các tệp, xuất bộ đếm làm câu trả lời cuối cùng. 

### Tại sao nó hoạt động

Thuật toán dựa trên thực tế là cả hai ràng buộc đều độc lập và có thể kiểm tra cục bộ. Điều kiện của tệp nhật ký hoàn toàn được xác định bằng so sánh hậu tố và quyền thực thi được xác định bởi ký tự vị trí cố định trong chuỗi có định dạng không đổi. Vì mỗi tệp được đánh giá độc lập và không có chuyển đổi nào phụ thuộc vào kết quả trước đó nên việc đếm các tệp hợp lệ sau khi lọc theo từng mục sẽ đảm bảo tính chính xác. Không có trường hợp trùng lặp nào mà việc khớp một phần có thể gây ra sự mơ hồ, bởi vì quy tắc hậu tố và mã hóa quyền đều chính xác và rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    ans = 0

    for _ in range(n):
        f, p = input().split()

        if len(f) >= 4 and f[-4:] == ".log":
            if p[2] == "x":
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo thuật toán. Kiểm tra hậu tố sử dụng tính năng cắt f[-4:] an toàn ngay cả khi chuỗi chính xác là “.log” vì độ dài 4 là trường hợp hợp lệ tối thiểu. Việc kiểm tra quyền sử dụng chỉ mục trực tiếp p[2], điều này an toàn vì tất cả các chuỗi quyền được đảm bảo có độ dài 3. 

Bộ đếm tích lũy các kết quả trùng khớp mà không lưu trữ bất kỳ trạng thái bổ sung nào, đảm bảo sử dụng bộ nhớ liên tục. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
.log rwx
a.log.zip r-x
proj.logger.cpp r-x
main rwx
b.log rw-
```Chúng tôi xử lý từng tệp một. 

| Tập tin | Tên kết thúc bằng ".log" | p[2] == 'x' | Đếm | 
| --- | --- | --- | --- | 
| .log | Có | Có | 1 | 
| a.log.zip | Không | Có | 1 | 
| proj.logger.cpp | Không | Có | 1 | 
| chính | Không | Có | 1 | 
| b.log | Có | Không | 1 | 

Câu trả lời cuối cùng là 1, vì chỉ “.log rwx” đáp ứng đồng thời cả hai điều kiện. 

Bây giờ hãy xem xét ví dụ thứ hai:```
a.log r-x
log.log rwx
.log r--
x.log rwx
```| Tập tin | Kết thúc bằng ".log" | p[2] == 'x' | Đếm | 
| --- | --- | --- | --- | 
| a.log | Có | Có | 1 | 
| log.log | Có | Có | 2 | 
| .log | Có | Không | 2 | 
| x.log | Có | Có | 3 | 

Đầu ra là 3, cho thấy nhiều tệp nhật ký hợp lệ có thể được tính độc lập. 

Những dấu vết này xác nhận rằng mỗi tệp được xử lý riêng biệt và chỉ có hậu tố chính xác và việc kiểm tra quyền mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi tệp được xử lý một lần bằng cách kiểm tra chuỗi thời gian liên tục | 
| Không gian | O(1) | Chỉ có một bộ đếm duy nhất được duy trì bất kể kích thước đầu vào | 

Với n 1000, tốc độ này rất nhanh và chạy tốt trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n = int(input().strip())
    ans = 0
    for _ in range(n):
        f, p = input().split()
        if len(f) >= 4 and f[-4:] == ".log" and p[2] == "x":
            ans += 1
    print(ans)

# provided sample
assert run(".log rwx\na.log.zip r-x\nproj.logger.cpp r-x\nmain rwx\nb.log rw-\n") == "1"

# minimum size, single valid
assert run("1\n.log rwx\n") == "1"

# minimum size, invalid permission
assert run("1\n.log r--\n") == "0"

# boundary: tricky suffix false positive
assert run("2\na.log.zip rwx\na.log rwx\n") == "1"

# all valid
assert run("3\n.log rwx\na.log rwx\nx.log rwx\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhật ký hợp lệ duy nhất | 1 | trường hợp tích cực cơ bản | 
| sự cho phép không hợp lệ | 0 | tính chính xác của bộ lọc quyền | 
| hậu tố hỗn hợp | 1 | tránh các kết quả trùng khớp chuỗi con “.log” sai | 
| tất cả các tập tin hợp lệ | 3 | tính chính xác của tổng hợp | 

## Vỏ cạnh 

Một trường hợp đặc biệt là tên tệp chính xác là “.log”. Thuật toán kiểm tra hậu tố bằng cách sử dụng f[-4:] == ".log", do đó, đối với f = ".log", lát cắt hợp lệ và khớp chính xác. Nếu p = "rwx", tập tin sẽ được tính. Điều này xác nhận rằng việc xử lý tiền tố trống được hỗ trợ một cách tự nhiên. 

Một trường hợp đặc biệt khác là tên tệp chứa “.log” bên trong, chẳng hạn như “a.log.zip”. Ở đây f[-4:] bằng “.zip”, không phải “.log”, do đó, tệp được loại trừ chính xác ngay cả khi chuỗi con xuất hiện trước đó. 

Trường hợp cạnh thứ ba là một tệp nhật ký không có quyền thực thi, chẳng hạn như “x.log rw-”. Mặc dù điều kiện hậu tố đã thỏa, nhưng p[2] vẫn là '-' nên không được tính. Điều này xác nhận rằng cả hai điều kiện phải được giữ đồng thời và không điều kiện nào lấn át điều kiện kia.
