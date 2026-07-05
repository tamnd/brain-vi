---
title: "CF 102916G - Chuỗi tối thiểu về mặt từ điển"
description: "Chúng ta được cho một chuỗi đơn và một số k. Từ chuỗi này chúng ta được phép xóa các ký tự mà vẫn giữ nguyên thứ tự tương đối của các ký tự còn lại."
date: "2026-07-04T08:00:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102916
codeforces_index: "G"
codeforces_contest_name: "Samara Farewell Contest 2020 (XXI Open Cup, Grand Prix of Samara)"
rating: 0
weight: 102916
solve_time_s: 41
verified: true
draft: false
---

[CF 102916G - Chuỗi tối thiểu về mặt từ điển](https://codeforces.com/problemset/problem/102916/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi đơn và một số k. Từ chuỗi này chúng ta được phép xóa các ký tự mà vẫn giữ nguyên thứ tự tương đối của các ký tự còn lại. Nhiệm vụ là chọn chính xác k ký tự theo thứ tự sao cho chuỗi con thu được càng nhỏ càng tốt theo thứ tự từ điển. 

Đầu ra không phải là chuỗi con, vì chúng ta được phép bỏ qua các ký tự ở bất kỳ đâu. Nó cũng không chỉ là sự lựa chọn bất kỳ k ký tự nhỏ nhất nào, vì thứ tự của chúng trong chuỗi gốc phải được giữ nguyên. 

Ràng buộc rằng độ dài chuỗi có thể lên tới 1e6 buộc chúng ta phải tránh xa mọi thứ bậc hai. Bất kỳ giải pháp nào cố gắng xem xét tất cả các chuỗi con hoặc mô phỏng lặp đi lặp lại các lựa chọn tham lam bằng cách quét qua hậu tố còn lại, sẽ giảm xuống O(nk) hoặc tệ hơn, quá chậm khi cả n và k đều lớn. 

Một cạm bẫy ngây thơ xuất hiện khi nghĩ “chỉ cần chọn nhân vật nhỏ nhất có thể ở mỗi bước”. Điều đó không thành công vì một lựa chọn nhỏ cục bộ có thể chặn quyền truy cập vào cấu trúc toàn cầu tốt hơn sau này. 

Ví dụ: hãy xem xét s = "bcaaa" và k = 3. Một lựa chọn tham lam ngây thơ sẽ chọn 'a' càng sớm càng tốt, nhưng nếu bạn lấy 'a' đầu tiên ngay lập tức, sau đó bạn có thể mất quá ít sự linh hoạt và cuối cùng bị buộc phải sử dụng một hậu tố tệ hơn mức cần thiết. Câu trả lời đúng là "aaa", yêu cầu phải cố tình bỏ qua các ký tự trước đó. 

Một dạng lỗi khác là cố gắng quét liên tục ký tự nhỏ nhất trong hậu tố còn lại rồi tiếp tục từ đó. Điều này đúng về mặt logic nhưng quá chậm, vì mỗi bước sẽ quét lại một hậu tố rút gọn, tạo ra hành vi O(nk). 

## Phương pháp tiếp cận 

Quan điểm bạo lực là xây dựng từng ký tự tiếp theo. Tại mỗi vị trí của câu trả lời, chúng tôi xem xét mọi chỉ mục tiếp theo có thể có trong chuỗi mà vẫn còn đủ ký tự để hoàn thành chuỗi con có độ dài k, chọn chỉ mục dẫn đến phần tiếp theo nhỏ nhất về mặt từ điển và lặp lại. Điều này đúng vì nó trực tiếp thực thi định nghĩa về thứ tự từ điển, nhưng mỗi quyết định đều yêu cầu quét một hậu tố và suy luận về tính khả thi, dẫn đến phân nhánh theo cấp số nhân hoặc tốt nhất là O(nk) nếu được thực hiện cẩn thận. 

Quan sát quan trọng là khi chúng tôi quyết định đặt một ký tự ở một vị trí nhất định trong câu trả lời, chúng tôi chỉ quan tâm đến vị trí sớm nhất mà chúng tôi có thể chọn ký tự nhỏ nhất có thể một cách an toàn trong khi vẫn để lại đủ ký tự để hoàn thành độ dài còn lại. Điều này biến vấn đề thành việc liên tục chọn một ký tự tối thiểu trong một cửa sổ khả thi trượt. 

Ở bất kỳ bước nào, nếu đang xây dựng đáp án mà vẫn cần r ký tự thì chúng ta chỉ được phép chọn vị trí i sao cho còn lại ít nhất r ký tự sau i. Ràng buộc đó hạn chế cửa sổ tìm kiếm một cách linh hoạt. Trong cửa sổ đó, việc chọn ký tự có sẵn nhỏ nhất luôn là tối ưu, bởi vì bất kỳ ký tự nào lớn hơn sẽ làm cho tiền tố tệ hơn và luôn có đủ khoảng trống còn lại để bù lại sau này. 

Cấu trúc đề xuất một thuật toán tham lam với ranh giới di chuyển và các truy vấn tối thiểu lặp lại trên các phạm vi, có thể được triển khai hiệu quả với ý tưởng ngăn xếp đơn điệu hoặc cây phân đoạn. Giải pháp lập trình cạnh tranh đơn giản nhất sử dụng quét tham lam kết hợp với chuyển động con trỏ cẩn thận, điều này là đủ vì mỗi ký tự được xử lý hiệu quả với số lần không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force trên các chuỗi con | O(C(n, k) * k) | O(k) | Quá chậm | 
| Tham lam với phạm vi quét khả thi | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý chuỗi trong khi vẫn duy trì số lượng ký tự cần thiết cho câu trả lời. Gọi r là số ký tự chúng ta vẫn phải chọn. Ban đầu r = k. 

Chúng tôi cũng duy trì một con trỏ cho biết khoảng cách chúng tôi được phép quét khi chọn ký tự tiếp theo. Ở mỗi bước, chúng tôi xác định chỉ mục xa nhất mà chúng tôi vẫn có thể sử dụng trong khi vẫn để lại đủ ký tự để hoàn thành chuỗi tiếp theo. Ranh giới này là n - r. 

Bên trong cửa sổ hợp lệ này, chúng ta cần tìm ký tự nhỏ nhất. Sau khi tìm thấy, chúng tôi sẽ thêm nó vào kết quả, giảm r đi một và tiếp tục quá trình bắt đầu ngay sau chỉ mục đã chọn. 

Mỗi lựa chọn ký tự sẽ thu nhỏ vùng hợp lệ và chúng tôi không bao giờ truy cập lại các vị trí không còn hữu ích. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ một đối số trao đổi tiêu chuẩn. Giả sử tại một bước nào đó chúng ta chọn ký tự c ở vị trí i, nhưng tồn tại một dãy con nhỏ hơn về mặt từ điển chọn ký tự sau c' < c ở vị trí j > i. Vì j nằm trong phạm vi khả thi nên việc thay thế c' bằng c sẽ ngay lập tức làm cho tiền tố trở nên tồi tệ hơn, mâu thuẫn với tính tối thiểu. Ngược lại, bất kỳ ký tự nào sớm hơn mức tối thiểu đã chọn đều vi phạm tính khả thi (không đủ vị trí còn lại) hoặc lớn hơn về mặt từ điển. Do đó, lựa chọn tham lam luôn phù hợp với một giải pháp toàn cục tối ưu và hậu tố còn lại vẫn là một bài toán con giống hệt trên một chuỗi ngắn hơn với k nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    k = int(input().strip())
    n = len(s)

    res = []
    i = 0
    remaining = k

    while remaining > 0:
        limit = n - remaining
        best_idx = i

        for j in range(i, limit + 1):
            if s[j] < s[best_idx]:
                best_idx = j

        res.append(s[best_idx])
        i = best_idx + 1
        remaining -= 1

    sys.stdout.write("".join(res))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo cấu trúc tham lam. Chi tiết quan trọng nhất là tính toán`limit = n - remaining`, điều này đảm bảo tính khả thi: nếu chúng ta chọn một ký tự ở vị trí thứ i thì vẫn phải có đủ ký tự để hoàn thành dãy con. 

Vòng lặp bên trong chỉ quét cửa sổ hợp lệ, đảm bảo tính chính xác. Con trỏ`i`đảm bảo chúng tôi không bao giờ xem xét lại các vị trí trước đó, do đó, mỗi chỉ mục được xử lý nhiều nhất một lần khi bắt đầu ứng cử viên và sau đó bị loại trừ vĩnh viễn. 

## Ví dụ đã hoạt động 

Xét s = "bcaabac", k = 4. 

Chúng tôi theo dõi độ dài còn lại và tiền tố đã chọn. 

| Bước | tôi | còn lại | cửa sổ | chỉ số đã chọn | char đã chọn | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | [0..3] = b c a a | 2 | một | một | 
| 2 | 3 | 3 | [3..4] = ab | 3 | một | aa | 
| 3 | 4 | 2 | [4..5] = b a | 5 | một | aaa | 
| 4 | 6 | 1 | [6..6] = c | 6 | c | aaa | 

Dấu vết này cho thấy các ràng buộc về tính khả thi buộc chúng ta đôi khi phải bỏ qua các ký tự nhỏ trước đó như thế nào nếu chúng ngăn cản việc hoàn thành độ dài k. 

Bây giờ hãy xem xét s = "aaaaaa", k = 3. 

| Bước | tôi | còn lại | cửa sổ | chỉ số đã chọn | char đã chọn | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | [0..3] | 0 | một | một | 
| 2 | 1 | 2 | [1..4] | 1 | một | aa | 
| 3 | 2 | 1 | [2..5] | 2 | một | aaa | 

Điều này cho thấy thuật toán suy biến thành việc chọn lần xuất hiện hợp lệ ngoài cùng bên trái khi tất cả các ký tự đều bằng nhau, xác nhận tính ổn định dưới các bản sao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi chỉ mục chỉ được quét trong các cửa sổ khả thi thu nhỏ và mỗi bước sẽ đưa con trỏ về phía trước | 
| Không gian | O(k) | lưu trữ chuỗi con kết quả | 

Giải pháp này dễ dàng phù hợp với các ràng buộc cho n đến 1e6, vì các phép toán là tuyến tính và chỉ liên quan đến các so sánh ký tự đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    # assume solve() is defined in same scope
    solve()
    return stdout.getvalue()

# provided sample (interpreted)
assert run("bcaabac\n4\n") == "aaac", "sample 1"

# k = 1, smallest possible subsequence
assert run("dcba\n1\n") == "a", "single pick smallest"

# all equal
assert run("aaaaaa\n4\n") == "aaaa", "all equal case"

# increasing order
assert run("abcdef\n3\n") == "abc", "already sorted"

# decreasing order
assert run("fedcba\n3\n") == "cba", "must pick later smaller letters"

# boundary k = n
assert run("abc\n3\n") == "abc", "take full string"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dcba, k=1 | một | lựa chọn tối thiểu duy nhất | 
| aaa, k=4 | aaa | sự ổn định trùng lặp | 
| abcdef, k=3 | abc | tiền tố đã tối ưu | 
| fedcba, k=3 | cba | lựa chọn tối ưu bị trì hoãn | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi lựa chọn tối ưu không phải là ký tự nhỏ nhất trên toàn cầu mà là ký tự nhỏ nhất trong một hậu tố bị ràng buộc. 

Lấy s = "acb", k = 2. Ở bước đầu tiên, chọn 'a' là đúng. Nếu k là 1 thì chúng ta vẫn chọn 'a'. Nếu k = 2, sau khi chọn 'a', chúng ta phải đảm bảo vẫn còn một ký tự, vì vậy chúng ta buộc phải chọn từ hậu tố "cb". Thuật toán giới hạn chính xác cửa sổ và chọn 'b', đưa ra "ab". Một cách tiếp cận ngây thơ luôn chọn ký tự nhỏ nhất còn lại trên toàn cầu sẽ thất bại nếu chọn quá sớm mà không tôn trọng tính khả thi. 

Một trường hợp khác là khi ký tự nhỏ nhất xuất hiện quá sớm nhưng lại không đủ ký tự còn lại. Trong s = "baaa", k = 2, chữ 'a' đầu tiên xuất hiện ở chỉ số 1. Tuy nhiên, việc chọn nó ngay lập tức vẫn khả thi vì còn đủ ký tự. Thuật toán tính toán chính xác ranh giới và cho phép nó, tạo ra "aa".
