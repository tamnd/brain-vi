---
title: "CF 102951C - LCS về hoán vị"
description: "Chúng ta có hai chuỗi chứa các phần tử giống nhau theo các thứ tự khác nhau, thường là hai hoán vị có độ dài $n$."
date: "2026-07-04T07:22:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102951
codeforces_index: "C"
codeforces_contest_name: "USACO Guide Problem Submission"
rating: 0
weight: 102951
solve_time_s: 44
verified: true
draft: false
---

[CF 102951C - LCS về Hoán vị](https://codeforces.com/problemset/problem/102951/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi chứa các phần tử giống nhau theo các thứ tự khác nhau, thường là hai hoán vị có độ dài$n$. Nhiệm vụ là xác định độ dài của chuỗi giá trị dài nhất xuất hiện trong cả hai hoán vị theo cùng một thứ tự tương đối mà không yêu cầu các giá trị phải liền kề nhau. 

Cụ thể, hãy tưởng tượng bạn có một danh sách các nhãn được sắp xếp theo thứ tự trên một giá và một giá khác có cùng nhãn nhưng được sắp xếp lại. Bạn được phép chọn một số nhãn từ kệ đầu tiên, giữ thứ tự từ trái sang phải và bạn muốn chuỗi dài nhất có thể cũng có thể được chọn theo thứ tự tương tự từ kệ thứ hai. 

Đầu ra là một số nguyên duy nhất, số phần tử tối đa có thể khớp theo cách này. 

Ràng buộc chính thúc đẩy giải pháp là đầu vào là các hoán vị, nghĩa là mọi giá trị xuất hiện chính xác một lần trong mỗi mảng. Với$n$lên đến khoảng$10^5$, bất kỳ phương pháp bậc hai nào so sánh tất cả các cặp vị trí sẽ yêu cầu theo thứ tự$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn khả thi trong thời gian chạy 2 giây. Điều này ngay lập tức loại trừ các giải pháp lập trình động tính toán rõ ràng LCS trong$O(n^2)$. 

Một số tình huống nguy hiểm rất dễ xử lý sai. 

Nếu hai hoán vị giống hệt nhau, ví dụ: 

đầu vào:$A = [1, 2, 3, 4]$,$B = [1, 2, 3, 4]$Câu trả lời đúng là 4, vì toàn bộ chuỗi đều trùng khớp. Một cách tiếp cận ngây thơ chỉ kiểm tra sự bình đẳng giữa các vị trí thay vì trật tự sẽ vẫn có tác dụng ở đây, điều này có thể trấn an một cách sai lầm một giải pháp sai lầm. 

Nếu một hoán vị bị đảo ngược: 

đầu vào:$A = [1, 2, 3, 4]$,$B = [4, 3, 2, 1]$Câu trả lời đúng là 1, vì không có hai phần tử nào giữ nguyên thứ tự tương đối. Các giải pháp giả định các chỉ số phù hợp hoặc quên các ràng buộc đặt hàng thường được tính quá mức ở đây. 

Một trường hợp lỗi tinh vi khác xuất hiện khi các giá trị đúng nhưng vị trí bị căn chỉnh sai: 

đầu vào:$A = [2, 3, 1, 4]$,$B = [1, 2, 3, 4]$Câu trả lời đúng là 2, không phải 3, vì chỉ một số chuỗi con giữ nguyên thứ tự trong cả hai mảng. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là thử tất cả các dãy con của hoán vị thứ nhất và kiểm tra xem mỗi dãy con có xuất hiện trong hoán vị thứ hai theo cùng một thứ tự hay không. Về nguyên tắc, điều này đúng vì nó kiểm tra rõ ràng mọi dãy ứng cử viên có thể là một phần của dãy con chung. Tuy nhiên, số lượng các dãy con của một$n$-mảng phần tử là$2^n$và thậm chí việc kiểm tra từng cái với mảng thứ hai cũng tốn thời gian tuyến tính, dẫn đến sự bùng nổ theo cấp số nhân mà thậm chí không thể thực hiện được ngay cả đối với mức độ vừa phải$n$. 

Bước đột phá về cấu trúc đến từ việc nhận ra rằng hoán vị thứ hai xác định một vị trí cố định cho mọi giá trị. Thay vì so sánh trực tiếp các giá trị, chúng ta có thể dịch hoán vị đầu tiên thành một chuỗi các chỉ số biểu thị vị trí mỗi phần tử xuất hiện trong hoán vị thứ hai. Khi phép biến đổi này được thực hiện, bài toán sẽ thay đổi hoàn toàn ký tự: chúng ta không còn khớp hai hoán vị nữa mà tìm chuỗi con tăng dài nhất trong một mảng chỉ số. 

Điều này có hiệu quả vì việc bảo toàn thứ tự tương đối trong cả hai hoán vị tương đương với việc bảo toàn thứ tự tăng dần của các vị trí trong hoán vị thứ hai. Bất kỳ dãy con nào của hoán vị thứ nhất đều tương ứng với một dãy các chỉ số trong dãy thứ hai, và dãy con đó hợp lệ khi và chỉ khi các chỉ số đó tăng nghiêm ngặt. 

Vấn đề LIS có thể được giải quyết trong$O(n \log n)$sử dụng cấu trúc tham lam với tìm kiếm nhị phân, đủ hiệu quả để$n = 10^5$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuỗi tiếp theo của Brute Force |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Bản đồ + LIS |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

## bước 

1. Xây dựng bản đồ vị trí cho hoán vị thứ hai, lưu trữ chỉ số của từng giá trị. Điều này cho phép tra cứu liên tục theo thời gian về vị trí của bất kỳ phần tử nào trong chuỗi thứ hai. 
2. Thay thế mỗi phần tử trong hoán vị thứ nhất bằng chỉ số tương ứng của nó từ hoán vị thứ hai, tạo ra một mảng vị trí mới. 
3. Tính độ dài của dãy con tăng nghiêm ngặt dài nhất của mảng được biến đổi này. 
4. Duy trì một mảng`dp`Ở đâu`dp[i]`lưu trữ giá trị kết thúc nhỏ nhất có thể có của chuỗi con có độ dài tăng dần`i + 1`. 
5. Đối với mỗi giá trị vị trí trong mảng được chuyển đổi, hãy sử dụng tìm kiếm nhị phân để tìm vị trí phù hợp`dp`. Nếu nó kéo dài chuỗi con dài nhất, hãy nối thêm nó; mặt khác, thay thế phần tử đầu tiên trong`dp`đó lớn hơn hoặc bằng nó. 
6. Chiều dài cuối cùng của`dp`là câu trả lời. 

Bước thay thế là ý tưởng then chốt giúp duy trì tính linh hoạt. Ngay cả khi chúng ta ghi đè lên các giá trị trong`dp`, chúng tôi chỉ giữ đuôi tốt nhất có thể cho các chuỗi con có độ dài nhất định, không cam kết với một chuỗi con cụ thể. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào,`dp[k]`đại diện cho vị trí kết thúc nhỏ nhất có thể có của một dãy con có độ dài tăng dần`k + 1`. Bất biến này đảm bảo rằng nếu tồn tại một dãy con tăng dài hơn thì nó luôn có thể được xây dựng dựa trên các kết thúc tối thiểu này. Vì chúng tôi luôn thay thế bằng các ứng cử viên hợp lệ nhỏ hơn nên chúng tôi không bao giờ mất khả năng mở rộng đến giải pháp tối ưu và chúng tôi không bao giờ tăng độ dài chuỗi con một cách giả tạo bằng cách sử dụng các phần tử không tương thích. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_left

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, v in enumerate(b):
        pos[v] = i

    seq = [pos[v] for v in a]

    dp = []
    for x in seq:
        i = bisect_left(dp, x)
        if i == len(dp):
            dp.append(x)
        else:
            dp[i] = x

    print(len(dp))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã xây dựng một bảng tra cứu trực tiếp để mỗi giá trị trong hoán vị đầu tiên có thể được ánh xạ tới vị trí của nó trong hoán vị thứ hai trong thời gian không đổi. Điều này tránh việc quét lặp đi lặp lại. 

Bước chuyển đổi tạo ra một chuỗi số duy nhất mã hóa các ràng buộc về thứ tự. Một khi điều này được thực hiện, cấu trúc vấn đề ban đầu sẽ biến mất và chúng ta chỉ suy luận về tính đơn điệu. 

Tính toán LIS sử dụng chiến lược tham lam với tìm kiếm nhị phân. các`dp`mảng không lưu trữ chuỗi con thực tế nhưng duy trì phần cuối tối thiểu có thể có cho mỗi độ dài, đủ để khôi phục độ dài chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$A = [2, 3, 1, 4]$,$B = [1, 2, 3, 4]$Vị trí trong$B$:$1 \to 0, 2 \to 1, 3 \to 2, 4 \to 3$Trình tự được chuyển đổi:$[1, 2, 0, 3]$| Bước | x | dp trước | dp sau | giải thích | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [] | [1] | bắt đầu dãy mới | 
| 2 | 2 | [1] | [1, 2] | tăng phần mở rộng | 
| 3 | 0 | [1, 2] | [0, 2] | thay thế phần tử đầu tiên | 
| 4 | 3 | [0, 2] | [0, 2, 3] | kéo dài lâu nhất | 

Câu trả lời cuối cùng là 3. 

Điều này chứng tỏ cách thay thế cho phép phục hồi từ các giá trị lớn trước đó và vẫn xây dựng một chuỗi con dài hơn sau này. 

### Ví dụ 2 

đầu vào:$A = [1, 2, 3]$,$B = [3, 2, 1]$Vị trí:$3 \to 0, 2 \to 1, 1 \to 2$Đã chuyển đổi:$[2, 1, 0]$| Bước | x | dp trước | dp sau | 
| --- | --- | --- | --- | 
| 1 | 2 | [] | [2] | 
| 2 | 1 | [2] | [1] | 
| 3 | 0 | [1] | [0] | 

Câu trả lời cuối cùng là 1. 

Điều này cho thấy thuật toán xử lý chính xác các thứ tự đảo ngược hoàn toàn trong đó không tồn tại dãy con tăng dần dài hơn 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| mỗi trong số$n$phần tử thực hiện tìm kiếm nhị phân trong cấu trúc LIS | 
| Không gian |$O(n)$| bản đồ vị trí và trình tự chuyển đổi | 

Phép biến đổi và tính toán LIS đều có quy mô tuyến tính hoặc gần tuyến tính, phù hợp thoải mái trong các ràng buộc lên đến$10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import log
    import sys
    input = sys.stdin.readline

    from bisect import bisect_left

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos = [0] * (n + 1)
        for i, v in enumerate(b):
            pos[v] = i

        seq = [pos[v] for v in a]

        dp = []
        for x in seq:
            i = bisect_left(dp, x)
            if i == len(dp):
                dp.append(x)
            else:
                dp[i] = x

        print(len(dp))

    solve()
    return sys.stdout.getvalue().strip()

# sample-like tests
assert run("4\n2 3 1 4\n1 2 3 4") == "3"
assert run("3\n1 2 3\n3 2 1") == "1"

# custom tests
assert run("1\n1\n1") == "1", "single element"
assert run("5\n1 2 3 4 5\n1 2 3 4 5") == "5", "identical permutations"
assert run("5\n5 4 3 2 1\n1 2 3 4 5") == "1", "fully reversed"
assert run("6\n2 4 6 1 3 5\n1 2 3 4 5 6") == "3", "mixed ordering"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp 1 phần tử | 1 | ranh giới tối thiểu | 
| hoán vị giống hệt nhau | n | khớp chính xác đầy đủ | 
| hoán vị ngược | 1 | lỗi đặt hàng nghiêm ngặt | 
| cấu trúc xen kẽ | 3 | hành vi LIS không tầm thường | 

## Vỏ cạnh 

Hoán vị một phần tử là cách kiểm tra độ chính xác đơn giản nhất. Thuật toán xây dựng bản đồ vị trí có kích thước một và tạo ra LIS một phần tử vì không có cấu trúc thay thế nào để so sánh. 

Các hoán vị giống hệt nhau kiểm tra xem việc triển khai LIS có bảo toàn được cấu trúc tăng dần mà không có sự thay thế không cần thiết hay không. Mọi phần tử đều mở rộng`dp`, do đó chiều dài tăng tuyến tính đến$n$, phù hợp với câu trả lời đúng. 

Hoán vị ngược nhấn mạnh yêu cầu đặt hàng nghiêm ngặt. Mỗi phần tử mới đều nhỏ hơn phần tử trước đó trong mảng được chuyển đổi, do đó cấu trúc LIS tiếp tục thu gọn về độ dài một, thể hiện khả năng xử lý chính xác các chuỗi không tăng. 

Các mẫu hỗn hợp xác nhận rằng việc giảm cục bộ không phá vỡ việc khám phá chuỗi con toàn cục. Ngay cả khi các phần tử nằm rải rác, chiến lược thay thế vẫn đảm bảo rằng các đuôi dưới mức tối ưu trước đó không chặn các phần mở rộng hợp lệ sau này.
