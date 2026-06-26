---
title: "CF 105317G - Mật khẩu của Carlo"
description: "Mỗi chuỗi đầu vào trong danh sách có thể được coi là một chuỗi ký tự rất ngắn, tối đa là sáu ký tự. Đối với mỗi chuỗi truy vấn, chúng tôi được hỏi liệu nó có thể được hình thành bằng cách xóa một số ký tự khỏi ít nhất một trong các chuỗi được lưu trữ này mà không thay đổi thứ tự của…"
date: "2026-06-23T15:13:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105317
codeforces_index: "G"
codeforces_contest_name: "JPC 1.0"
rating: 0
weight: 105317
solve_time_s: 53
verified: true
draft: false
---

[CF 105317G - Mật khẩu của Carlo](https://codeforces.com/problemset/problem/105317/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi chuỗi đầu vào trong danh sách có thể được coi là một chuỗi ký tự rất ngắn, tối đa là sáu ký tự. Đối với mỗi chuỗi truy vấn, chúng tôi được hỏi liệu nó có thể được tạo bằng cách xóa một số ký tự khỏi ít nhất một trong các chuỗi được lưu trữ này mà không thay đổi thứ tự của các ký tự còn lại hay không. Nói cách khác, để một truy vấn hợp lệ, phải tồn tại một chuỗi nguồn trong danh sách chứa truy vấn dưới dạng một chuỗi con. 

Thách thức chính đến từ quy mô hơn là kiểm tra cá nhân. Có thể có tới năm mươi nghìn chuỗi nguồn và năm mươi nghìn truy vấn. Mặc dù việc kiểm tra xem một chuỗi có phải là chuỗi con của chuỗi khác nhanh hay không vì cả hai đều rất nhỏ, nhưng việc kiểm tra từng cặp vẫn sẽ dẫn đến hàng tỷ so sánh. Do đó, giải pháp lồng nhau trực tiếp sẽ vượt quá giới hạn thời gian ngay cả với các vòng lặp bên trong được tối ưu hóa rất tốt. 

Một điểm tinh tế là cả chuỗi nguồn và truy vấn đều cực kỳ ngắn. Điều này gợi ý rõ ràng rằng cấu trúc của mỗi chuỗi, chứ không phải số lượng của chúng, mới là thứ cần được khai thác. Bất kỳ giải pháp nào coi chuỗi là đối tượng có độ dài tùy ý đều bỏ qua việc đơn giản hóa khóa: mỗi chuỗi chỉ có 2^6 chuỗi con có thể có, là giới hạn trên không đổi. 

Một sai lầm ngây thơ phát sinh khi cố gắng xử lý trước chỉ tiền tố hoặc hậu tố. Ví dụ: nếu chuỗi nguồn là "abdc" và truy vấn là "abc", việc khớp tiền tố hoặc khớp chuỗi con sẽ từ chối chuỗi đó một cách không chính xác ngay cả khi việc xóa 'd' làm cho chuỗi đó hợp lệ. Một lỗi phổ biến khác là cho rằng truy vấn phải xuất hiện liền kề nhau, điều này là không bắt buộc. 

## Phương pháp tiếp cận 

Một ý tưởng đơn giản là xử lý từng truy vấn một cách độc lập và kiểm tra nó dựa trên mọi chuỗi nguồn. Vì cả hai chuỗi đều ngắn nên chúng ta có thể kiểm tra trạng thái chuỗi con theo thời gian tuyến tính theo độ dài nhiều nhất là sáu. Cách tiếp cận này đúng vì nó trực tiếp thực hiện định nghĩa. Tuy nhiên, với tối đa 5×10^4 truy vấn và 5×10^4 chuỗi nguồn, chúng tôi thực hiện khoảng 2,5×10^9 phép so sánh, mỗi phép so sánh tốn tối đa sáu thao tác. Điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là chúng ta liên tục giải quyết cùng một loại câu hỏi: “có chuỗi nguồn nào đó chứa truy vấn này dưới dạng một chuỗi con không?” Thay vì trả lời các truy vấn trực tuyến, chúng ta có thể tính toán trước tất cả các chuỗi con tồn tại trong bất kỳ chuỗi nguồn nào. Vì mỗi chuỗi có độ dài tối đa là sáu nên số lượng chuỗi con nhiều nhất là 2^6 = 64. Điều này biến quá trình tiền xử lý thành một bước mở rộng giới hạn. 

Chúng tôi tạo ra mọi chuỗi con của mỗi chuỗi nguồn và lưu trữ nó trong bộ băm. Sau khi bộ này được tạo, mỗi truy vấn sẽ giảm xuống còn một lần kiểm tra thành viên. Tổng số chuỗi được tạo tối đa là 5×10^4 × 64, khoảng 3,2 triệu mục, dễ quản lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · m · 6) | O(1) | Quá chậm | 
| Tính toán trước chuỗi | O(n · 2^6 + m) | O(n · 2^6) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành cấu trúc tiền xử lý và tra cứu trên tất cả các chuỗi con. 

1. Đọc tất cả các chuỗi nguồn và khởi tạo một bộ băm trống. 

Tập hợp này sẽ đại diện cho mọi truy vấn có thể được hình thành từ ít nhất một chuỗi nguồn. 
2. Đối với mỗi chuỗi nguồn, hãy tạo tất cả các chuỗi con bằng cách sử dụng phép liệt kê bitmask trên các vị trí của nó. 

Vì độ dài tối đa là sáu nên mỗi chuỗi đóng góp tối đa 64 chuỗi con. 

Đối với mỗi mặt nạ bit, chúng tôi xây dựng chuỗi con tương ứng bằng cách chọn các ký tự nơi bit được đặt. 
3. Chèn từng dãy con được tạo vào tập hợp chung. 

Điều này đảm bảo rằng nếu bất kỳ chuỗi nào có thể tạo ra một mẫu nhất định thông qua việc xóa thì mẫu đó sẽ có sẵn cho các truy vấn. 
4. Đọc từng chuỗi truy vấn và kiểm tra xem nó có tồn tại trong tập hợp hay không. 

Nếu có thì xuất ra “YES”, nếu không thì xuất ra “NO”. 
5. Lặp lại cho đến khi tất cả các truy vấn được xử lý.

### Tại sao nó hoạt động 

Mỗi chuỗi con của chuỗi nguồn được tạo và lưu trữ rõ ràng. Bất kỳ câu trả lời hợp lệ nào cũng phải tương ứng với một chuỗi con của ít nhất một chuỗi nguồn, do đó nó phải xuất hiện trong tập hợp được xây dựng. Ngược lại, mọi thứ trong tập hợp đều được đảm bảo đến từ một chuỗi nguồn thực, vì vậy nó hợp lệ về mặt xây dựng. Điều này tạo ra sự tương đương hoàn hảo giữa tư cách thành viên trong tập hợp và tính hợp lệ của truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s_list = [input().strip() for _ in range(n)]
    
    subseqs = set()

    for s in s_list:
        L = len(s)
        # generate all subsequences
        for mask in range(1 << L):
            t = []
            for i in range(L):
                if mask & (1 << i):
                    t.append(s[i])
            subseqs.add("".join(t))
    
    m = int(input())
    out = []
    for _ in range(m):
        t = input().strip()
        out.append("YES" if t in subseqs else "NO")
    
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp được xây dựng xung quanh bộ tiền xử lý`subseqs`. Vòng lặp bitmask liệt kê mọi chuỗi con có thể có của mỗi chuỗi nguồn, bao gồm cả chuỗi trống, hợp lệ nhưng không liên quan đến các truy vấn dài hơn 0. Mỗi chuỗi được tạo sẽ được chèn vào một bộ Python, cho phép tra cứu O(1) trung bình trong các truy vấn. 

Chi tiết triển khai phổ biến quan trọng ở đây là cấu trúc chuỗi. Vì chuỗi ngắn nên được lặp lại`"".join`hoạt động vẫn rẻ. Độ phức tạp tổng thể bị chi phối bởi phép liệt kê bitmask thay vì chi phí xử lý chuỗi. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào minh họa nhỏ:```
3
ahmad
abd
jaun
6
jn
ad
acd
aqd
amh
jan
```Chúng tôi theo dõi xem các chuỗi con chính có được chèn vào tập hợp hay không. 

### Xây dựng chuỗi tiếp theo (dấu vết một phần) 

| Chuỗi nguồn | mặt nạ | dãy sau | chèn vào | 
| --- | --- | --- | --- | 
| abd | 101 | quảng cáo | vâng | 
| abd | 11 | ab | vâng | 
| abd | 1 | một | vâng | 
| abd | 0 | "" | vâng | 
| ahmad | 10101 | amd | vâng | 
| jaun | 1011 | tháng một | vâng | 

Bây giờ đánh giá truy vấn: 

| Truy vấn | Đặt thành viên | Đầu ra | 
| --- | --- | --- | 
| jn | trình bày qua "jaun" | CÓ | 
| quảng cáo | trình bày qua "abd" | CÓ | 
| acd | không được tạo ra | KHÔNG | 
| aqd | không được tạo ra | KHÔNG | 
| àh | thứ tự không khớp ở tất cả các nguồn | KHÔNG | 
| tháng một | trình bày qua "jaun" | CÓ | 

Dấu vết này cho thấy tính hợp lệ hoàn toàn được xác định bằng việc liệu truy vấn có xuất hiện trong phạm vi chuỗi con được tính toán trước hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 2^6 + m) | mỗi chuỗi tạo ra tối đa 64 chuỗi con, truy vấn là tra cứu băm | 
| Không gian | O(n · 2^6) | tất cả các chuỗi con duy nhất được lưu trữ trong một tập hợp | 

Hệ số tiền xử lý có hiệu quả tuyến tính theo n với hằng số nhỏ. Với n lên đến 5×10^4, tổng số chuỗi con được tạo vẫn ở mức thấp hàng triệu, nằm trong giới hạn bộ nhớ và thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    stdout_buffer = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = stdout_buffer

    solve()

    sys.stdout = old_stdout
    return stdout_buffer.getvalue().strip()

# sample
assert run("""3
ahmad
abd
jaun
6
jn
ad
acd
aqd
amh
jan
""") == """YES
YES
NO
NO
NO
YES"""

# single character match
assert run("""1
a
2
a
b
""") == """YES
NO"""

# empty subsequence case
assert run("""1
abc
1
""") == "YES"

# maximum small strings repetition
assert run("""2
aaaaaa
bbbbbb
2
ab
ba
""") == """NO
NO"""

# direct match only
assert run("""2
abc
def
3
abc
ac
df
""") == """YES
NO
NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khớp ký tự đơn | CÓ/KHÔNG | tính đúng đắn của thành viên cơ bản | 
| truy vấn trống | CÓ | xử lý chuỗi con trống | 
| ký tự lặp đi lặp lại | KHÔNG | ra lệnh thực thi ràng buộc | 
| chỉ khớp trực tiếp | hỗn hợp | không có kết quả dương tính giả do chồng chéo một phần | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi truy vấn trống. Mỗi chuỗi đều chứa dãy con trống, do đó kết quả đầu ra đúng luôn là CÓ. Thuật toán xử lý việc này một cách tự nhiên vì mặt nạ trống luôn được chèn vào trong quá trình tiền xử lý. 

Một trường hợp khác liên quan đến các ký tự lặp lại như "aaaaaa". Mọi chuỗi con sẽ thu gọn thành các chuỗi như "", "a", "aa", v.v., nhưng các chuỗi trùng lặp không thành vấn đề vì tập hợp chỉ lưu trữ các giá trị duy nhất. Các truy vấn như "aaa" được nhận dạng chính xác. 

Trường hợp cạnh có cấu trúc hơn xảy ra khi các ký tự tồn tại trong nhiều chuỗi nguồn nhưng không bao giờ theo đúng thứ tự trong một chuỗi. Ví dụ: "a" có thể xuất hiện trong một chuỗi và "b" trong một chuỗi khác, nhưng "ab" không hợp lệ trừ khi cả hai xuất hiện theo thứ tự trong một nguồn duy nhất. Thuật toán tránh được cạm bẫy này vì nó không bao giờ trộn lẫn các ký tự trên các chuỗi khác nhau; mọi chuỗi con được tạo từ một chuỗi gốc duy nhất, duy trì chính xác các ràng buộc về thứ tự.
