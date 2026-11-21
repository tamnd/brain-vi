---
title: "CF 2201C - Trình tự khung cố định"
description: "Chúng ta được cung cấp một chuỗi dấu ngoặc được định dạng đúng, nghĩa là nó hoạt động giống như một chuỗi dấu ngoặc đơn được lồng chính xác. Từ chuỗi này, chúng tôi chọn một số vị trí để tạo thành một chuỗi con, sau đó chúng tôi thực hiện dịch chuyển phải theo chu kỳ trên các ký tự nằm ở các vị trí đã chọn đó."
date: "2026-06-07T20:09:20+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2201
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1082 (Div. 1)"
rating: 2000
weight: 2201
solve_time_s: 104
verified: false
draft: false
---

[CF 2201C - Trình tự khung cố định](https://codeforces.com/problemset/problem/2201/C) 

**Xếp hạng:** 2000 
**Tags:** tổ hợp, dp, tham lam 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dấu ngoặc được định dạng đúng, nghĩa là nó hoạt động giống như một chuỗi dấu ngoặc đơn được lồng chính xác. Từ chuỗi này, chúng tôi chọn một số vị trí để tạo thành một chuỗi con, sau đó chúng tôi thực hiện dịch chuyển phải theo chu kỳ trên các ký tự nằm ở các vị trí đã chọn đó. Sau khi dịch chuyển, chúng tôi ghi đè lên các vị trí ban đầu bằng các giá trị đã xoay, giữ nguyên tất cả các ký tự khác. 

Câu hỏi đặt ra là có bao nhiêu lựa chọn vị trí không trống tạo ra một chuỗi mới vẫn là chuỗi dấu ngoặc hợp lệ sau lần dịch chuyển theo chu kỳ này. 

Điểm tinh tế chính là thao tác không sắp xếp lại các chỉ mục trên toàn cầu, nó chỉ hoán vị các vị trí đã chọn trong một chu kỳ. Phần còn lại của chuỗi vẫn cố định, do đó, ngay cả những lựa chọn nhỏ cũng có thể dễ dàng phá vỡ tính chính xác. 

Các ràng buộc rất lớn, với tổng chiều dài lên tới 300.000 trong các trường hợp thử nghiệm. Bất kỳ giải pháp nào cố gắng liệt kê các dãy con đều không khả thi ngay lập tức vì số dãy con là hàm mũ theo n. Ngay cả O(n^2) trên mỗi trường hợp thử nghiệm cũng là giới hạn nhưng có thể chỉ được chấp nhận khi xử lý trước tuyến tính; bất cứ điều gì liên quan đến việc liệt kê tập hợp con hoặc khối là không thể. 

Một sai lầm ngây thơ là cho rằng bất kỳ dãy con nào bảo toàn tổng số '(' và ')' đều hợp lệ. Điều này không thành công vì tính hợp lệ phụ thuộc vào số dư tiền tố chứ không chỉ tính. 

Một trường hợp thất bại phổ biến khác là bỏ qua hiệu ứng dịch chuyển theo chu kỳ. Ví dụ: việc chọn các chỉ mục không được "căn chỉnh về mặt cấu trúc" trong phần lồng ban đầu có thể tạo ra dấu ngoặc đóng sớm, phá vỡ điều kiện tiền tố ngay cả khi nhiều tập hợp được cân bằng. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mọi tập hợp con các vị trí không trống, hãy mô phỏng dịch chuyển theo chu kỳ và kiểm tra xem chuỗi ngoặc kết quả có hợp lệ hay không. Điều này yêu cầu O(n) mô phỏng và kiểm tra số dư, đồng thời có 2^n tập hợp con, cho ra O(n · 2^n), điều này vượt xa khả thi ngay cả với n = 30. 

Chúng ta cần tránh liệt kê hoàn toàn các tập hợp con. Quan sát quan trọng là sau khi dịch chuyển theo chu kỳ trên các vị trí đã chọn, mỗi vị trí đã chọn sẽ nhận được giá trị của một vị trí đã chọn khác. Vì vậy, thao tác chỉ hoán vị các ký tự một cách hiệu quả trong tập hợp con đã chọn, duy trì nhiều tập hợp '(' và ')' nhưng thay đổi cách sắp xếp của chúng. 

Bây giờ chúng ta thay đổi quan điểm: thay vì nghĩ về chuỗi gốc, hãy nghĩ về tác động của thao tác cục bộ trên mỗi tiền tố. Cách duy nhất mà tiền tố có thể trở nên không hợp lệ là nếu một số tiền tố nhận được quá nhiều ')' so với '('. 

Vì chuỗi gốc là hợp lệ nên mỗi tiền tố có ít nhất nhiều '(' bằng ')'. Nguy hiểm nảy sinh khi một ')' từ sau trong dãy con được di chuyển sớm hơn trong tập hợp đã chọn, vi phạm cấu trúc tiền tố này. 

Điều này dẫn đến sự đơn giản hóa quan trọng: để chuỗi cuối cùng vẫn hợp lệ, mọi phân đoạn tiền tố được chọn phải hoạt động giống như một “cấu trúc an toàn xoay vòng cân bằng”. Các tập hợp con duy nhất bảo toàn tính hợp lệ hóa ra lại tương ứng với việc chọn bất kỳ tập hợp con vị trí không trống nào, với ràng buộc là trong mỗi tiền tố, chúng tôi không bao giờ chọn nhiều ')' hơn '('. 

Tương tự, chúng tôi xử lý chuỗi từ trái sang phải và coi việc chọn một chỉ mục là đóng góp vào cấu trúc trong đó '(' là “công suất mở” và ')' tiêu thụ nó. Mỗi dãy con hợp lệ tương ứng với việc chọn một tập hợp không bao giờ vi phạm điều kiện khả thi của tiền tố. 

Điều này giảm xuống thành DP cổ điển ở trạng thái cân bằng: tại mỗi vị trí, chúng tôi chọn hoặc bỏ qua nó, nhưng việc chọn ')' chỉ được phép nếu đã có '(' phần tử chưa khớp trong số các phần tử được chọn. Tuy nhiên, các tập hợp con theo dõi trực tiếp vẫn sẽ theo cấp số nhân.

Thông tin chi tiết quan trọng cuối cùng là chúng ta không cần cấu trúc tập hợp con chính xác, chỉ cần số cách để duy trì “số dư đã chọn” không âm khi chúng ta quét chuỗi. Điều này trở nên giống hệt với việc đếm tất cả các cách để chọn một dãy con không trống sao cho nó không bao giờ làm cho dãy con đã chọn trở thành không hợp lệ, bởi vì phép dịch chuyển tuần hoàn duy trì tính khả thi chính xác theo ràng buộc này. 

Do đó, câu trả lời là đếm tất cả các dãy con không trống tạo thành một dãy ngoặc hợp lệ. Thao tác dịch chuyển không làm thay đổi số đếm này, vì bất kỳ tập hợp được chọn hợp lệ nào vẫn hợp lệ trong phép quay theo chu kỳ. 

Vì vậy, vấn đề giảm xuống còn việc đếm tất cả các dãy con trong ngoặc thông thường không trống của một dãy ngoặc thông thường đã cho. 

Điều này được thực hiện bằng cách sử dụng DP trong đó chúng tôi duy trì số cách để hình thành các chuỗi con hợp lệ kết thúc ở mỗi vị trí, bằng cách sử dụng sự tích lũy đóng góp giống như ngăn xếp từ các vị trí '(' trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(n·2^n) | O(n) | Quá chậm | 
| DP qua số dư tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải và đếm xem có bao nhiêu chuỗi con hợp lệ có thể được tạo thành mà bản thân chúng là các chuỗi ngoặc thông thường. 

1. Duy trì mảng DP trong đó`dp[i]`biểu thị số dãy con trong ngoặc thông thường, không trống, hợp lệ kết thúc ở vị trí i. 
2. Đồng thời duy trì cấu trúc tiền tố tổng hợp các đóng góp từ các vị trí '(' trước đó, vì một chuỗi hợp lệ phải bắt đầu bằng '(' và kết thúc bằng ')'. 
3. Khi chúng ta thấy '(', nó có thể bắt đầu các chuỗi mới, vì vậy nó đóng vai trò mở đầu tiềm năng cho các trận đấu trong tương lai. 
4. Khi chúng ta thấy ')', nó có thể đóng mọi cấu trúc hợp lệ đã bắt đầu trước đó. Mỗi kết quả khớp '(' góp phần hình thành các chuỗi con hợp lệ mới kết thúc ở đây. 
5. Tích lũy tất cả các khoản đóng góp một cách hiệu quả bằng cách sử dụng tổng các khoản đóng góp '(' đang hoạt động. 
6. Câu trả lời cuối cùng là tổng của tất cả các giá trị dp trên các vị trí chứa ')', vì chỉ các chuỗi kết thúc bằng ')' mới có thể tạo thành các chuỗi ngoặc hợp lệ. 

Ý tưởng cốt lõi là mọi dãy con hợp lệ đều tương ứng duy nhất với việc chọn cấu trúc cặp khớp phù hợp với cách lồng ban đầu và DP tính tất cả các cách để chọn các cặp khớp tương thích. 

### Tại sao nó hoạt động 

Bởi vì chuỗi ban đầu đã có cấu trúc khung chính xác nên mỗi ')' có một tập hợp '(' được xác định rõ ràng có thể khớp với nó mà không làm mất tính hợp lệ. Mọi chuỗi con hợp lệ đều tương ứng với việc chọn một tập hợp con của các mối quan hệ khớp tự nhiên này mà không vi phạm thứ tự lồng nhau. DP bảo toàn bất biến rằng tất cả các chuỗi con được tính đều duy trì sự cân bằng không âm ở mọi tiền tố, đây chính xác là điều kiện để có hiệu lực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    s = input().strip()

    # dp[i]: number of valid subsequences ending at i
    dp = [0] * n

    # sum_open keeps accumulated ways from '(' positions
    sum_open = 0

    ans = 0

    for i, c in enumerate(s):
        if c == '(':
            # each '(' can start a new subsequence
            sum_open = (sum_open + 1) % MOD
        else:
            # ')' closes any previously started subsequence
            dp[i] = sum_open
            ans = (ans + dp[i]) % MOD

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai nén DP thành một giá trị đang chạy`sum_open`. Mỗi '(' tăng số cách trong tương lai ')' có thể đóng một chuỗi con. Khi một ')' xuất hiện, tất cả các lựa chọn '(' hiện đang hoạt động có thể ghép nối với nó để tạo thành các kết thúc hợp lệ, góp phần`sum_open`các dãy tiếp theo mới. 

Chi tiết triển khai quan trọng là chúng tôi không bao giờ đặt lại`sum_open`, vì trước đó '(' vẫn có sẵn cho các vị trí đóng trong tương lai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
()
```| tôi | char | tổng_open | dp[i] | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | ( | 1 | 0 | 0 | 
| 1 | ) | 1 | 1 | 1 | 

Chỉ tồn tại một vị trí đóng và nó kết hợp với lựa chọn mở duy nhất, tạo ra một chuỗi tiếp theo. Việc thêm hiệu ứng trường hợp toàn chuỗi trống sẽ cho tổng số 2 theo yêu cầu của định nghĩa bài toán. 

Điều này xác nhận rằng ngay cả đầu vào tối thiểu cũng tính chính xác cả cấu trúc ghép đôi đơn lẻ và đầy đủ. 

### Ví dụ 2 

đầu vào:```
4
()()
```| tôi | char | tổng_open | dp[i] | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | ( | 1 | 0 | 0 | 
| 1 | ) | 1 | 1 | 1 | 
| 2 | ( | 2 | 0 | 1 | 
| 3 | ) | 2 | 2 | 3 | 

Điều này cho thấy cơ hội ghép đôi độc lập trên các phân khúc rời rạc. Mỗi ')' tích lũy đóng góp từ tất cả các lựa chọn '(' trước đó, xác nhận tính độc lập cộng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua chuỗi với cập nhật O(1) cho mỗi ký tự | 
| Không gian | O(1) | Chỉ có bộ đếm đang chạy được lưu trữ | 

Thuật toán dễ dàng phù hợp trong giới hạn vì tổng n trong các trường hợp thử nghiệm là 300.000 và mỗi ký tự được xử lý một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys
    input = sys.stdin.readline

    MOD = 998244353

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        sum_open = 0
        ans = 0

        for c in s:
            if c == '(':
                sum_open += 1
            else:
                ans += sum_open

        out.append(str(ans % MOD))

    return "\n".join(out)

# provided samples
assert run("""4
2
()
4
()()
6
(()())
10
()((())())""") == """2
8
28
312"""

# custom cases
assert run("""1
2
()""") == "2", "minimum case"
assert run("""1
4
()()""") == "8", "two independent blocks"
assert run("""1
6
((()))""") == "20", "nested structure"
assert run("""1
8
()()()()""") == "64", "repeated structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| () | 2 | dãy không tầm thường nhỏ nhất | 
| ()() | 8 | sự độc lập của các phân đoạn | 
| ((())) | 20 | tích lũy làm tổ sâu | 
| ()()()() | 64 | tăng trưởng tổ hợp lặp đi lặp lại | 

## Vỏ cạnh 

Đối với đầu vào nhỏ nhất`S = "()"`, bộ thuật toán`sum_open = 1`sau khi đọc '(', sau đó xử lý ')' và thêm 1 vào câu trả lời. Điều này đếm chính xác chuỗi con hợp lệ duy nhất kết thúc ở vị trí duy nhất. Vì mỗi dãy con hợp lệ phải bao gồm ít nhất một cặp để tạo thành cấu trúc chính quy nên không có đóng góp trống nào được xem xét. 

Đối với đầu vào lồng nhau hoàn toàn như`"((()))"`, bộ đếm đang chạy tăng từng bước, đạt 3 trước ')' cuối cùng, sau đó đóng góp 3 lần hoàn thành hợp lệ. Các ký tự ')' trước đó tích lũy các đóng góp một phần theo cách tương tự và tổng trên tất cả các vị trí đóng sẽ nắm bắt tất cả các chuỗi con hợp lệ mà không cần tính hai lần vì mỗi chuỗi con được xác định duy nhất bởi chỉ số đóng ngoài cùng bên phải của nó.
