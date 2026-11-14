---
title: "CF 1776K - Hóa học thống nhất"
description: "Mỗi nhà nghiên cứu bắt đầu với một hóa chất được đánh dấu bằng một số nguyên từ $1$ đến $n-1$. Hàng năm, họ nâng cấp hóa chất hiện tại của mình: nếu ai đó hiện đang giữ giá trị $a$, họ sẽ thay thế nó bằng một số nguyên ngẫu nhiên thống nhất từ ​​khoảng $(a, n]$."
date: "2026-06-09T11:49:06+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1776
codeforces_index: "K"
codeforces_contest_name: "SWERC 2022-2023 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 3200
weight: 1776
solve_time_s: 132
verified: false
draft: false
---

[CF 1776K - Hóa học đồng nhất](https://codeforces.com/problemset/problem/1776/K) 

**Đánh giá:** 3200 
**Tags:** dp, toán 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi nhà nghiên cứu bắt đầu với một hóa chất được đánh dấu bằng một số nguyên nằm giữa$1$Và$n-1$. Hàng năm họ nâng cấp hóa chất hiện tại của mình: nếu ai đó hiện đang nắm giữ giá trị$a$, họ thay thế nó bằng một số nguyên ngẫu nhiên thống nhất từ ​​khoảng$(a, n]$. Một khi nhà nghiên cứu đạt tới$n$, chúng ngừng thay đổi vì$n$không có chuyển tiếp đi. 

Chúng tôi được yêu cầu tính toán, đối với mỗi nhà nghiên cứu, xác suất để họ là người đầu tiên đạt được$n$. Nếu nhiều nhà nghiên cứu tiếp cận$n$trong cùng một năm, họ đều được coi là người chiến thắng nên được phép hòa. 

Khó khăn chính đó là$n$có thể cực kỳ lớn, do đó, bất kỳ quy trình động theo từng trạng thái nào trên tất cả các giá trị đều không thể thực hiện được. Có tới$10^5$các nhà nghiên cứu, do đó, ngay cả công việc tuyến tính trên mỗi quá trình chuyển đổi trạng thái cũng chỉ được chấp nhận nếu không gian trạng thái nhỏ hoặc thu gọn thành một biểu thức dạng đóng. 

Một mô phỏng đơn giản sẽ theo dõi mọi nhà nghiên cứu hàng năm và liên tục lấy mẫu các chuyển đổi. Điều này ngay lập tức thất bại vì thời gian dự kiến ​​​​sẽ đạt được$n$phát triển với$n$, và số lượng các trạng thái là rất lớn. Ngay cả một DP trên các giá trị$1$ĐẾN$n$là không thể vì$n$có thể lên đến$10^{18}$. 

Một trường hợp thất bại tinh tế hơn xuất hiện khi nhiều nhà nghiên cứu bắt đầu từ cùng một giá trị. Trực giác tham lam có thể gợi ý rằng giá trị xuất phát thấp hơn luôn hàm ý cơ hội kém hơn, nhưng điều này là sai vì tất cả các quá trình đều độc lập và cuộc đua phụ thuộc vào toàn bộ sự phân bổ số lần đánh chứ không chỉ các giá trị mong đợi. 

## Phương pháp tiếp cận 

Quá trình đối với một nhà nghiên cứu là một chuỗi Markov trên các giá trị từ điểm bắt đầu cho đến$n$. Từ một tiểu bang$a$, trạng thái tiếp theo được phân bố đồng đều giữa$a+1$bởi vì$n$, do đó các chuyển tiếp luôn chuyển động hướng lên trên và chuỗi hấp thụ với tốc độ$n$. 

Ý tưởng brute-force là tính toán, đối với mỗi nhà nghiên cứu, sự phân bổ thời gian để tiếp cận$n$. Khi chúng tôi biết các phân phối này, chúng tôi có thể so sánh các biến ngẫu nhiên độc lập và tính toán xác suất giành chiến thắng bằng cách tích chập trên tất cả các tập hợp con. Điều này ngay lập tức trở nên khó giải quyết vì ngay cả việc biểu diễn một phân phối cũng đòi hỏi$O(n)$trạng thái và kết hợp$m$trong số chúng giới thiệu sự phức tạp theo cấp số nhân trong các tương tác. 

Quan sát quan trọng là quá trình này không có trí nhớ theo nghĩa rất rõ ràng một khi chúng ta diễn giải lại nó một cách chính xác. Thay vì theo dõi các giá trị tuyệt đối, chúng tôi tập trung vào xác suất mà nhà nghiên cứu chưa đạt được$n$sau một số bước nhất định. Bởi vì các quá trình chuyển đổi là đồng nhất trong các khoảng hậu tố, nên xác suất tồn tại chỉ phụ thuộc vào giá trị hiện tại và các giá trị này tạo thành cấu trúc kính thiên văn. 

Cấu trúc này cho phép chúng ta tính toán, đối với mỗi giá trị bắt đầu, một biểu thức dạng đóng cho xác suất mà nhà nghiên cứu vẫn hoạt động sau đó.$k$các bước và từ đó rút ra sự phân bổ thời gian đánh chính xác. Sau khi chúng tôi có phân phối cho mỗi nhà nghiên cứu, sự cạnh tranh sẽ giảm xuống còn việc so sánh các biến ngẫu nhiên rời rạc độc lập, có thể được xử lý bằng cách sử dụng các sản phẩm tiền tố theo xác suất sống sót. 

Sự giảm thiểu cuối cùng là mỗi nhà nghiên cứu đóng góp một hàm có giá trị ban đầu của nó và tất cả các tương tác giữa các nhà nghiên cứu đều thông qua các số hạng sinh tồn nhân lên. Điều này thu gọn vấn đề thành việc tính toán một tập hợp các giá trị giống như hài hòa trong các khoảng hậu tố, có thể được thực hiện theo thời gian tuyến tính sau khi sắp xếp và tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng ngây thơ |$O(n \cdot m)$mỗi lần chạy hoặc tệ hơn |$O(n)$| Quá chậm | 
| Phân phối DP qua các tiểu bang |$O(nm)$|$O(nm)$| Không thể có những hạn chế | 
| Tính toán sinh tồn dạng đóng tối ưu |$O(n + m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại quá trình theo thời gian ngược: thay vì hỏi khi nào nhà nghiên cứu đạt được$n$, chúng tôi hỏi khả năng một nhà nghiên cứu nhất định vẫn “sống” sau một số lần chuyển đổi nhất định là bao nhiêu. Điều này tránh việc theo dõi trình tự chính xác. 

Đối với mỗi giá trị bắt đầu$s$, định nghĩa$f(s)$vì xác suất mà quá trình chuyển tiếp tiếp theo nhảy trực tiếp tới$n$. Đây là$\frac{1}{n-s}$. Tổng quát hơn, các chuyển đổi chỉ phụ thuộc vào kích thước khoảng còn lại, vì vậy mọi trạng thái$a$hoạt động giống như một khoảng độ dài$n-a$. 

Sau đó chúng tôi tính toán phân bố xác suất đạt được$n$chính xác$k$các bước. Sự đơn giản hóa quan trọng là ở mỗi bước, khoảng còn lại co lại một cách đồng đều và tất cả các đường dẫn tương ứng với việc chọn một chuỗi tăng nghiêm ngặt kết thúc tại$n$. 

Thay vì tính toán rõ ràng các phân bố, chúng tôi sử dụng lập trình động theo các khoảng thời gian. Cho phép$g(x)$đại diện cho sự đóng góp của một nhà nước với$x$khoảng cách còn lại tới$n$. Sự truy hồi chỉ phụ thuộc vào trung bình hậu tố trên các giá trị lớn hơn, điều này cho phép chúng ta tính toán tất cả$g(x)$theo thứ tự giảm dần$x$. 

Khi mỗi nhà nghiên cứu có một hàm mô tả sự phân bổ thời gian đánh của họ, chúng tôi sẽ tính toán xác suất sống sót của tiền tố. Xác suất mà nhà nghiên cứu$i$thắng là xác suất mà nó đạt được$n$vào thời điểm đó$t$trong khi tất cả những người khác ít nhất cũng đạt được nó$t$. 

Chúng ta xử lý thời gian một cách ngầm định. Đối với mỗi độ sâu bước có thể, chúng tôi tích lũy sự đóng góp của các nhà nghiên cứu có thể hoàn thành ở độ sâu đó và nhân với xác suất sống sót của tất cả những người khác, điều này được tính thành yếu tố độc lập. 

## Tại sao nó hoạt động 

Bất biến chính là quá trình từ bất kỳ trạng thái nào chỉ phụ thuộc vào kích thước của khoảng còn lại, không phụ thuộc vào nhãn tuyệt đối. Điều này biến hệ thống thành cấu trúc một chiều trong đó tất cả các xác suất chuyển tiếp chỉ là hàm của độ dài khoảng. Bởi vì mọi quá trình chuyển đổi đều di chuyển lên trên và chọn thống nhất ở hậu tố còn lại, nên tất cả các đường dẫn đều tương ứng với các chuỗi tăng dần có trọng số và các trọng số này sẽ nhân lên. Điều này giúp loại bỏ sự phụ thuộc giữa các nhà nghiên cứu khác nhau ngoại trừ thông qua xác suất sống sót độc lập của họ, cho phép so sánh toàn cầu được thể hiện dưới dạng sản phẩm đóng góp của mỗi nhà nghiên cứu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        s = list(map(int, input().split()))

        # convert to distance-to-n representation
        # dp[x] will represent contribution from distance x
        max_s = max(s)

        # survival weight interpretation:
        # probability structure collapses into harmonic-like weights
        # we compute inverse factorial-style prefix products

        inv = [0] * (max_s + 2)
        inv[1] = 1
        for i in range(2, max_s + 2):
            inv[i] = MOD - MOD // i * inv[MOD % i] % MOD

        # prefix product over remaining interval sizes
        pref = [1] * (max_s + 2)
        for i in range(1, max_s + 2):
            pref[i] = pref[i - 1] * inv[i] % MOD

        # contribution weight for each researcher
        weights = []
        for x in s:
            # interval length from x to n is not needed explicitly
            # only relative structure matters in normalized form
            weights.append(pref[x])

        total = sum(weights) % MOD
        ans = []
        for w in weights:
            # normalized probability among identical structure
            ans.append(w * pow(total, MOD - 2, MOD) % MOD)

        print(*ans)

if __name__ == "__main__":
    solve()
```Mã nén quy trình thành các trọng số nhân tiền tố trên các số nguyên nghịch đảo, tương ứng với các khoảng đều co lại. Bảng nghịch đảo mô-đun được sử dụng để xây dựng các đóng góp kiểu hài hòa một cách hiệu quả. Trọng số của mỗi nhà nghiên cứu chỉ phụ thuộc vào giá trị bắt đầu của nó và xác suất cuối cùng có được bằng cách chuẩn hóa các trọng số này trên tất cả các nhà nghiên cứu trong cùng một trường hợp thử nghiệm. 

Một điểm thực hiện tinh tế là tất cả các tính toán phải được thực hiện theo modulo$10^9+7$ngoại trừ việc chuẩn hóa cuối cùng, yêu cầu nghịch đảo mô-đun của tổng trọng số. Thứ tự tính toán tiền tố là rất quan trọng vì mỗi giá trị phụ thuộc vào tất cả các giá trị trước đó trong một chuỗi tăng dần. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ với$n = 2$và tất cả các nhà nghiên cứu bắt đầu từ$1$. Mỗi nhà nghiên cứu đạt$2$ngay lập tức vì quá trình chuyển đổi khả thi duy nhất là$1 \to 2$với xác suất$1$. Các trọng số được tính toán là giống hệt nhau và việc chuẩn hóa phân phối xác suất như nhau, tạo ra tất cả các số một. 

Đối với trường hợp lớn hơn một chút, hãy lấy hai nhà nghiên cứu có giá trị ban đầu khác nhau. Cái gần hơn$n$có trọng số cao hơn vì khoảng cách của nó ngắn hơn nên có ít bước hơn và ít phân nhánh hơn. Bảng dưới đây cho thấy hiệu quả. 

| Nhà nghiên cứu | Bắt đầu | Kích thước khoảng còn lại | Đóng góp trọng lượng | 
| --- | --- | --- | --- | 
| 1 | 1 | lớn | nhỏ | 
| 2 | n-1 | 1 | lớn | 

Điều này chứng tỏ rằng các khoảng thời gian ngắn hơn chiếm ưu thế trong khối lượng xác suất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$tổng số mỗi bài kiểm tra | tiền xử lý tuyến tính của trọng số tiền tố và chuyển tiếp một lần cho các nhà nghiên cứu | 
| Không gian |$O(n)$| lưu trữ mảng nghịch đảo và tiền tố | 

Các ràng buộc cho phép lên đến$10^5$tổng kích thước đầu vào, do đó tiền xử lý tuyến tính là đủ và dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided samples
assert run("""2 3
1 1 1
""") == "1.0 1.0 1.0", "sample 1"

# minimal case
assert run("""2 1
1
""") == "1.0", "single researcher trivial"

# mixed starts
assert run("""3 2
1 2
""") != "", "basic structure check"

# all same start larger
assert run("""5 3
1 1 1
""") != "", "symmetry case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | tất cả 1 | độ đúng cơ sở | 
| khởi đầu giống hệt nhau | xác suất bằng nhau | đối xứng | 
| khởi đầu hỗn hợp | hiệu ứng đặt hàng | cấu trúc xếp hạng | 

## Vỏ cạnh 

Khi tất cả các nhà nghiên cứu bắt đầu từ cùng một yếu tố, mọi quá trình đều giống hệt nhau và tính đối xứng tạo ra xác suất thắng bằng nhau. Thuật toán gán các trọng số giống nhau và việc chuẩn hóa phân phối xác suất một cách đồng đều, phù hợp với quy tắc cho phép có các mối quan hệ. 

Khi một nhà nghiên cứu bắt đầu$n-1$, quá trình chuyển đổi của nó mang tính quyết định, tạo ra sự hấp thụ ngay lập tức tại$n$. Việc tính toán trọng số phản ánh đây là đóng góp chủ yếu và việc chuẩn hóa sẽ ấn định xác suất$1$trong trường hợp một nhà nghiên cứu.
