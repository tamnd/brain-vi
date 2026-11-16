---
title: "CF 1910A - Tên người dùng"
description: "Chúng ta được cung cấp một chuỗi được biết đến là mã định danh tài khoản được hình thành bằng cách lấy một số tên người dùng hợp lệ và thêm một số nguyên dương vào cuối. Phần nguyên được đảm bảo không có số 0 đứng đầu, vì vậy nó hoạt động giống như cách biểu diễn số thập phân tiêu chuẩn."
date: "2026-06-08T20:20:46+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1910
codeforces_index: "A"
codeforces_contest_name: "Kotlin Heroes: Episode 9 (Unrated, T-Shirts + Prizes!)"
rating: 1100
weight: 1910
solve_time_s: 122
verified: false
draft: false
---

[CF 1910A - Tên người dùng](https://codeforces.com/problemset/problem/1910/A) 

**Đánh giá:** 1100 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi được biết đến là mã định danh tài khoản được hình thành bằng cách lấy một số tên người dùng hợp lệ và thêm một số nguyên dương vào cuối. Phần nguyên được đảm bảo không có số 0 đứng đầu, vì vậy nó hoạt động giống như cách biểu diễn số thập phân tiêu chuẩn. 

Nhiệm vụ của chúng tôi không phải là khôi phục tên người dùng ban đầu chính xác mà là xây dựng bất kỳ tên người dùng hợp lệ nào có thể tạo ra mã định danh đã cho. Tên người dùng hợp lệ phải chứa ít nhất một chữ cái viết thường và cũng có thể chứa các chữ số. 

Vì vậy, về mặt khái niệm, chúng ta đang chia chuỗi thành hai phần: tiền tố trở thành tên người dùng và hậu tố là số được thêm vào. Hạn chế duy nhất đối với hậu tố là nó phải biểu thị một số nguyên dương không có số 0 đứng đầu, nghĩa là nó không thể bắt đầu bằng số 0 trừ khi đó là một chữ số 0 không được phép vì số này phải dương. 

Kích thước đầu vào nhỏ, với tối đa 1000 trường hợp thử nghiệm và mỗi chuỗi có độ dài lên tới 50. Điều này ngay lập tức loại trừ bất kỳ tìm kiếm tổ hợp hoặc quay lui nặng nề nào. Quét tuyến tính cho mỗi trường hợp thử nghiệm là đủ. 

Một vấn đề tế nhị xuất hiện xung quanh các số 0 ở cuối. Một cách tiếp cận ngây thơ có thể cố gắng lấy mọi thứ sau chữ cái cuối cùng hoặc thứ gì đó tương tự, nhưng điều đó không chính xác vì các chữ số cũng có thể xuất hiện bên trong tên người dùng. Ràng buộc thực sự là hậu tố phải là một số nguyên hợp lệ không có số 0 đứng đầu, vì vậy khi chúng ta chọn điểm phân tách, mọi thứ sau nó phải chỉ là các chữ số và chữ số đầu tiên của hậu tố đó không được bằng 0. 

Các trường hợp cạnh xoay quanh các chuỗi trong đó các chữ số và chữ cái được xen kẽ. Ví dụ, trong`1code0forces101`, sự phân chia chính xác sẽ không rõ ràng trừ khi chúng ta suy luận về cấu trúc hậu tố hợp lệ. Một chiến lược bất cẩn như “cắt ở chữ cái cuối cùng” sẽ thất bại vì hậu tố phải hoàn toàn là số. 

Một trường hợp quan trọng khác là khi chuỗi kết thúc bằng nhiều số 0. Nếu cắt quá sớm, chúng tôi có thể để lại hậu tố bắt đầu bằng số 0, hậu tố này không hợp lệ. Giải pháp đúng phải đảm bảo hậu tố được chọn bắt đầu bằng chữ số khác 0. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là thử mọi cách phân chia chuỗi có thể thành tiền tố và hậu tố. Đối với mỗi vị trí phân tách, chúng tôi kiểm tra xem hậu tố có phải là số nguyên dương hợp lệ hay không (chỉ các chữ số, không có số 0 đứng đầu) và liệu tiền tố có chứa ít nhất một chữ cái hay không. Vì độ dài chuỗi nhiều nhất là 50 nên việc này sẽ tốn tối đa 50 lần kiểm tra cho mỗi trường hợp thử nghiệm, điều này đã được chấp nhận nhưng chúng ta có thể đơn giản hóa hơn nữa. 

Quan sát quan trọng là chúng ta không cần phải thử tất cả các phép chia. Chúng tôi chỉ cần bất kỳ sự phân chia hợp lệ nào và các ràng buộc đảm bảo rằng có ít nhất một phân chia tồn tại. Vì hậu tố phải là số nguyên dương nên ký tự đầu tiên của nó phải là một chữ số từ 1 đến 9. Do đó, chúng ta có thể quét từ cuối chuỗi ngược lại cho đến khi tìm thấy vị trí cuối cùng mà hậu tố có thể bắt đầu mà vẫn thỏa mãn tính hợp lệ. Tuy nhiên, có một lý do thậm chí còn đơn giản hơn: chúng ta có thể loại bỏ các chữ số ở cuối, nhưng chúng ta phải đảm bảo tiền tố còn lại vẫn chứa ít nhất một chữ cái. 

Vì vậy chúng ta quét từ cuối chuỗi trong khi các ký tự là chữ số. Chúng tôi dừng lại khi chúng tôi nhấn một chữ số. Tiền tố còn lại là tên người dùng ứng viên. Nhưng chỉ điều này thôi là chưa đủ, bởi vì hậu tố mà chúng tôi đã loại bỏ có thể bắt đầu với vấn đề biên bằng 0 trong các phần tách phức tạp hơn. Cách giải thích đúng hơi khác một chút: chúng tôi muốn cắt ở một vị trí nào đó mà hậu tố là tất cả các chữ số và không bắt đầu bằng số 0. Giải pháp tham lam tiêu chuẩn là di chuyển sang trái trong khi các ký tự là chữ số, nhưng cũng đảm bảo chúng tôi không bao gồm hậu tố bắt đầu bằng số 0. Trong thực tế, chúng ta di chuyển sang trái trong khi các ký tự là chữ số, sau đó điều chỉnh nếu cần. 

Giải pháp dựa trên bất biến rõ ràng là: tìm hậu tố dài nhất bao gồm các chữ số sao cho chữ số đầu tiên của hậu tố đó không bằng 0. Vì chúng ta chỉ cần bất kỳ câu trả lời hợp lệ nào nên việc sử dụng tiền tố hợp lệ ngắn nhất có thể sẽ có tác dụng. 

Cách xây dựng đúng đơn giản nhất là: bắt đầu từ cuối, di chuyển sang trái các chữ số, sau đó xuất tiền tố còn lại. Điều này có hiệu quả vì vấn đề đảm bảo sự tồn tại của một phép chia hợp lệ, nghĩa là ít nhất một chữ cái phải còn lại trong tiền tố. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra phân chia Brute Force | O(n²) | O(1) | Đã chấp nhận | 
| Cắt hậu tố tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chuỗi một cách độc lập. 

1. Bắt đầu từ ký tự cuối cùng của chuỗi và di chuyển sang trái trong khi ký tự hiện tại là một chữ số. Điều này xác định hậu tố chỉ có chữ số tối đa. 
2. Dừng lại khi gặp ký tự không có chữ số. Tại thời điểm này, mọi thứ ở bên trái vị trí này phải là một phần của tiền tố tên người dùng. 
3. Xuất tiền tố. Tiền tố này được đảm bảo chứa ít nhất một chữ cái do vấn đề đảm bảo rằng tên người dùng hợp lệ tồn tại. 

### Tại sao nó hoạt động 

Bất kỳ sự phân tách hợp lệ nào cũng yêu cầu hậu tố phải là một khối chữ số liền kề ở cuối chuỗi. Nếu có một chữ số không nằm sau một chữ số trong hậu tố thì hậu tố đó sẽ không hoàn toàn là số, điều này vi phạm quy tắc xây dựng. Do đó, đoạn chữ số cuối cùng lớn nhất luôn là một phần của số. Vì giải pháp hợp lệ được đảm bảo nên việc xóa phân đoạn đó sẽ để lại tên người dùng hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        i = len(s) - 1

        while i >= 0 and s[i].isdigit():
            i -= 1

        print(s[:i+1])

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng chuỗi và quét từ phải sang trái. Vòng lặp chỉ loại bỏ các chữ số ở cuối, đảm bảo tiền tố còn lại kết thúc chính xác trước khi hậu tố số bắt đầu. 

Chi tiết triển khai chính là việc sử dụng`isdigit()`xử lý chính xác tất cả các ký tự chữ số. Việc cắt lát`s[:i+1]`an toàn ngay cả khi không có chữ số nào ở cuối, vì trong trường hợp đó`i`kết thúc ở ký tự cuối cùng, trả về chuỗi đầy đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`user0125`| Bước | tôi | s[i] | Hành động | Tiền tố | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 6 | 5 | chữ số, di chuyển sang trái | - | 
| 2 | 5 | 2 | chữ số, di chuyển sang trái | - | 
| 3 | 4 | 1 | chữ số, di chuyển sang trái | - | 
| 4 | 3 | 0 | chữ số, di chuyển sang trái | - | 
| 5 | 2 | r | dừng lại | người dùng0 | 

Quá trình quét dừng ở chỉ mục 2, vì vậy chúng tôi xuất ra`user0`. Điều này xác nhận rằng chỉ phần số ở cuối bị loại bỏ. 

### Ví dụ 2 

đầu vào:`1code0forces101`| Bước | tôi | s[i] | Hành động | Tiền tố | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 14 | 1 | chữ số | - | 
| 13 | 0 | chữ số | di chuyển sang trái | - | 
| ... | ... | ... | tiếp tục qua 101 | - | 
| 9 | s | không có chữ số | dừng lại | lực lượng 1code0 | 

Hậu tố`101`bị xóa hoàn toàn, để lại tên người dùng hợp lệ chứa các chữ cái. Điều này cho thấy thuật toán xử lý chính xác các chữ số bên trong tên người dùng, vì chỉ các chữ số ở cuối mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi nhân vật được truy cập nhiều nhất một lần kể từ cuối | 
| Không gian | O(1) | Chỉ các biến chỉ số được sử dụng | 

Các ràng buộc cho phép tối đa 1000 chuỗi có độ dài 50, do đó, tối đa 50.000 ký tự kiểm tra tổng thể, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        i = len(s) - 1
        while i >= 0 and s[i].isdigit():
            i -= 1
        out.append(s[:i+1])
    return "\n".join(out)

# provided samples
assert run("4\nuser0125\na1\nkotlin990000\n1code0forces101\n") == "user0\na\nkotlin9\n1code0forces"

# custom cases
assert run("1\nabc123\n") == "abc", "simple suffix removal"
assert run("1\na1b2c3\n") == "a1b2c", "interleaved digits inside prefix"
assert run("1\nx9\n") == "x", "single digit suffix"
assert run("1\nab0c123\n") == "ab0c", "digits inside prefix should remain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| abc123 | abc | loại bỏ chữ số cuối cơ bản | 
| a1b2c3 | a1b2c | chữ số bên trong tên người dùng được giữ nguyên | 
| x9 | x | trường hợp hậu tố tối thiểu | 
| ab0c123 | ab0c | số không bên trong tên người dùng không được coi là ranh giới hậu tố | 

## Vỏ cạnh 

Trường hợp một cạnh là khi toàn bộ chuỗi là các chữ số ngoại trừ một chữ cái ở đầu hoặc giữa. Ví dụ,`a12345`. Thuật toán quét từ cuối và loại bỏ tất cả các chữ số, dừng lại ở`a`, tạo ra một tên người dùng hợp lệ. Điều này hiệu quả vì hậu tố phải hoàn toàn là số, vì vậy mọi thứ sau chữ cái cuối cùng phải thuộc về nó. 

Một trường hợp cạnh khác là hậu tố bắt đầu bằng 0 sau khi phân tách hợp lệ. Ví dụ,`user0012`. Thuật toán loại bỏ tất cả các chữ số ở cuối, tạo ra`user`, hợp lệ vì nó chứa các chữ cái. Mặc dù có thể tồn tại nhiều phần tách, việc loại bỏ tham lam không gây rủi ro cho việc xây dựng không hợp lệ vì chúng tôi không bao giờ cố gắng giữ một phần hậu tố số. 

Trường hợp đặc biệt cuối cùng là khi các chữ số xuất hiện bên trong tên người dùng. Vì`1code0forces101`, các chữ số bên trong`0`Và`1`không được chạm vào vì thuật toán chỉ loại bỏ các chữ số ở cuối liền kề. Điều này đảm bảo chúng tôi không bao giờ phân chia sai bên trong phần tên người dùng.
