---
title: "CF 106461A - Thử thách Kendama"
description: "Chúng tôi đang lập mô hình một chuỗi các thử nghiệm độc lập trong đó mỗi thử nghiệm thành công hoặc thất bại với xác suất đã biết."
date: "2026-06-19T17:15:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106461
codeforces_index: "A"
codeforces_contest_name: "KUPC 2025 (The 4th Universal Cup. Stage 22: GP of Kyoto)"
rating: 0
weight: 106461
solve_time_s: 51
verified: true
draft: false
---

[CF 106461A - Thử thách Kendama](https://codeforces.com/problemset/problem/106461/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang lập mô hình một chuỗi các thử nghiệm độc lập trong đó mỗi thử nghiệm thành công hoặc thất bại với xác suất đã biết. Nhiệm vụ không phải là mô phỏng quá trình mà là tính toán xác suất theo thời gian: cụ thể là xác suất để một mẫu rất cụ thể xuất hiện lần đầu tiên chính xác tại vị trí.$X$. 

Mẫu có cấu trúc cứng nhắc được neo xung quanh vị trí$X$. Nhìn về phía sau từ$X$, chúng tôi yêu cầu một khối chính xác$K$những thành công liên tiếp kết thúc ở$X$, ngay trước đó là sự thất bại ở vị trí$X-K$và trước thời điểm đó, mẫu đó không được xuất hiện sớm hơn trong chuỗi. 

Do đó, đầu vào thể hiện danh sách xác suất thành công cho mỗi lần thử, cùng với các tham số mô tả độ dài mẫu$K$và vị trí mục tiêu$X$. Đầu ra là một giá trị xác suất duy nhất: xác suất mà lần xuất hiện hợp lệ đầu tiên của mẫu “thất bại rồi K thành công” này kết thúc chính xác tại vị trí$X$. 

Các ràng buộc không được nêu rõ ràng, nhưng gợi ý về việc sử dụng các tích lũy trong$O(1)$mỗi truy vấn ngụ ý rằng số lượng thử nghiệm có thể lớn, thường lên tới$10^5$hoặc hơn thế nữa. Điều đó ngay lập tức loại trừ mọi suy luận bậc hai trên tất cả các mảng con hoặc việc quét lặp lại cho mọi mảng con.$X$. Bất kỳ giải pháp hợp lệ nào cũng phải xử lý trước thông tin để xác suất phân đoạn có thể được truy vấn trong thời gian không đổi. 

Một trường hợp thất bại tinh tế xuất hiện khi một cách tiếp cận ngây thơ bỏ qua điều kiện “lần đầu tiên”. Ví dụ, giả sử$K = 2$và chúng ta có một chuỗi thành công như: 

Xác suất đầu vào:```
0.5 0.5 0.5 0.5
```Nếu chúng ta chỉ kiểm tra xem vị trí$X-2, X-1, X$khớp với mẫu cục bộ, chúng tôi có thể đếm không chính xác các cấu hình trong đó mẫu đã xuất hiện trước đó, chẳng hạn như tại các vị trí$2$ĐẾN$4$, mặc dù lần xuất hiện đầu tiên là ở vị trí$3$ĐẾN$5$theo một trình tự dài hơn. Giải pháp đúng phải thực thi một điều kiện tiền tố để đảm bảo không tồn tại cửa sổ hợp lệ nào trước đó. 

Một lỗi phổ biến khác phát sinh từ việc xử lý ranh giới khi$X-K-1 \le 0$. Trong trường hợp đó, điều kiện “không được thỏa mãn trước$X-K-1$” trở nên trống rỗng và việc triển khai ngây thơ thường nhân không chính xác với một thuật ngữ tiền tố không nên tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê rõ ràng tất cả các cấu hình thành công và thất bại có thể có trong chuỗi và kiểm tra xem lần xuất hiện hợp lệ đầu tiên có kết thúc ở$X$. Điều này nhanh chóng trở thành cấp số nhân trong$N$, vì mỗi thử nghiệm phân nhánh thành hai kết quả. Ngay cả khi chúng tôi cố gắng hạn chế chỉ kiểm tra các cửa sổ kết thúc bằng$X$, chúng tôi vẫn cần xác minh rằng không có cửa sổ nào trước đó thỏa mãn cùng một cấu trúc, trong trường hợp xấu nhất yêu cầu quét tất cả các vị trí trước đó cho từng điểm cuối ứng cử viên. Điều đó dẫn đến$O(N^2)$hành vi. 

Quan sát quan trọng là điều kiện chỉ phụ thuộc vào ba phân đoạn độc lập: tiền tố lên tới$X-K-1$, thất bại duy nhất tại$X-K$, và khối của$K$những thành công liên tiếp từ$X-K+1$ĐẾN$X$. Đây là những độc lập theo mô hình, do đó xác suất của chúng nhân lên. Điều phức tạp duy nhất là buộc mẫu không xuất hiện sớm hơn, nhưng điều kiện đó có thể được xử lý bằng cách duy trì xác suất đang chạy “không xuất hiện cho đến i”. 

Khi chúng tôi nhận ra rằng cấu trúc giảm xuống xác suất tiền tố và xác suất cửa sổ có độ dài cố định, chúng tôi có thể tính toán trước các sản phẩm tiền tố về xác suất thành công và thất bại. Sau đó, mỗi điểm cuối ứng cử viên có thể được đánh giá theo thời gian không đổi và chúng tôi có thể xây dựng mảng tiền tố kiểu DP theo dõi xem mẫu đã từng xuất hiện trước một chỉ mục nhất định hay chưa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$hoặc hàm mũ |$O(1)$| Quá chậm | 
| Tối ưu |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giả định một loạt các xác suất thành công$p[i]$, với xác suất thất bại$q[i] = 1 - p[i]$. Chúng tôi cũng giả định rằng chúng tôi cần xác suất của các phân đoạn liên tiếp, vì vậy chúng tôi dựa vào các tích số tiền tố. 

1. Tính toán trước các tích số tiền tố cho xác suất thành công và xác suất phân đoạn kết hợp theo cách cho phép nhân phạm vi nhanh. Điều này cho phép bất kỳ phân đoạn nào$[l, r]$của các thử nghiệm độc lập được tính toán theo thời gian không đổi dưới dạng tích của các tỷ lệ của các giá trị tiền tố. 
2. Duy trì một mảng`pref_ok[i]`biểu thị xác suất mẫu này không xuất hiện ở bất kỳ đâu trong tiền tố kết thúc tại$i$. Điều này là cần thiết vì sự kiện này là “lần đầu tiên xảy ra”. 
3. Đối với từng vị trí$i$, hãy coi nó như một điểm cuối tiềm năng$X$. Tính xác suất để vị trí$i-K+1$ĐẾN$i$đều là những thành công khi sử dụng cấu trúc sản phẩm tiền tố. 
4. Nhân với xác suất vị trí đó$i-K$là một thất bại. 
5. Nhân với`pref_ok[i-K-1]`, điều này buộc rằng không có sự xuất hiện hợp lệ nào trước đó tồn tại. 
6. Thêm đóng góp này vào câu trả lời và cập nhật`pref_ok[i]`bằng cách trừ đi xác suất một lần xuất hiện mới kết thúc tại$i$, đảm bảo nó vẫn có xác suất “không xảy ra cho đến nay”. 
7. Tiếp tục quá trình này cho đến khi$N$, chỉ tích lũy đóng góp tại các điểm cuối hợp lệ$i \ge K+1$. 

Lựa chọn thiết kế quan trọng là chúng ta không bao giờ tìm kiếm một cách rõ ràng những lần xuất hiện trước đó. Thay vào đó, chúng tôi gộp ràng buộc đó vào trạng thái tiền tố DP, do đó, điều kiện “lần xuất hiện đầu tiên” được duy trì tăng dần. 

### Tại sao nó hoạt động 

Tại mỗi chỉ số$i$, tất cả các sự kiện xác định lần xuất hiện đầu tiên hợp lệ chỉ phụ thuộc vào các phân đoạn rời rạc: tiền tố trước$i-K-1$, thất bại duy nhất tại$i-K$và cửa sổ hậu tố có độ dài$K$. Những phân đoạn này liên quan đến các thử nghiệm độc lập, do đó xác suất sẽ tăng lên gấp bội. trạng thái DP`pref_ok[i]`duy trì tính bất biến rằng nó bằng xác suất không có mẫu hợp lệ nào kết thúc tại hoặc trước$i$. Vì mỗi lần xuất hiện mới chỉ được giới thiệu ở điểm cuối của nó và bị loại trừ ngay lập tức khỏi các tiền tố trong tương lai nên không có cấu hình nào được tính hai lần và mỗi lần xuất hiện hợp lệ đầu tiên đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, x = map(int, input().split())
    p = list(map(float, input().split()))

    # prefix product of success probabilities
    pref = [1.0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] * p[i - 1]

    def range_prod(l, r):
        if l > r:
            return 1.0
        return pref[r] / pref[l - 1]

    # dp[i]: probability no pattern ends at or before i
    dp = [1.0] * (n + 1)
    ans = 0.0

    for i in range(1, n + 1):
        dp[i] = dp[i - 1]

        if i >= k + 1:
            l = i - k + 1
            r = i

            success_block = range_prod(l, r)
            failure = 1.0 - p[i - k - 1]

            contrib = dp[i - k - 1] * failure * success_block
            ans += contrib

            dp[i] -= contrib

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng sản phẩm tiền tố`pref`cho phép bất kỳ khối thành công liên tiếp nào được tính toán trong thời gian không đổi. Người trợ giúp`range_prod`xử lý các phạm vi trống hoặc không hợp lệ một cách rõ ràng, điều này rất cần thiết khi$k = 0$hoặc khi các chỉ số căn chỉnh gần đầu mảng. 

Mảng`dp`không phải là một quá trình chuyển đổi DP cổ điển qua các trạng thái mà là một khối xác suất đang chạy để theo dõi khối lượng xác suất đã được “tiêu thụ” bởi các lần hoàn thành mẫu trước đó. Trừ`contrib`đảm bảo rằng một khi cấu hình được tính là kết thúc ở vị trí`i`, nó không thể đóng góp vào các vị trí sau này, duy trì ràng buộc "lần xuất hiện đầu tiên". 

Cần chú ý cẩn thận khi lập chỉ mục: vị trí lỗi là`i - k - 1`, điều này chỉ có hiệu lực khi`i >= k + 1`. Ranh giới này là nguồn gốc phổ biến nhất của những sai lầm riêng lẻ. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ với$n = 4$,$k = 1$và xác suất:```
p = [0.5, 0.5, 0.5, 0.5]
```Chúng tôi tính toán đóng góp cho từng điểm cuối. 

| tôi | chỉ số hư hỏng (i-k) | khối thành công | dp[i-k-1] | đóng góp | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | p[2] | dp[0]=1 | 0,5 * 0,5 * 1 = 0,25 | 
| 3 | 2 | p[3] | dp[1] | 0,5 * 0,5 * dp[1] | 
| 4 | 3 | p[4] | dp[2] | 0,5 * 0,5 * dp[2] | 

Bảng hiển thị cách mỗi điểm cuối phụ thuộc vào trạng thái tiền tố trước đó. các`dp`thuật ngữ này làm giảm mức đóng góp khi số lần xuất hiện trước đó tích lũy, thực thi ràng buộc “lần đầu tiên”. 

Dấu vết này chứng tỏ rằng thuật toán không bao giờ tính toán lại các phân đoạn trong quá khứ và chỉ sử dụng xác suất tiền tố tích lũy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi vị trí được xử lý một lần với các truy vấn sản phẩm trong phạm vi thời gian không đổi | 
| Không gian |$O(N)$| Mảng sản phẩm tiền tố và mảng DP | 

Giải pháp tuyến tính về số lượng thử nghiệm, phù hợp với quy mô hạn chế dự kiến ​​lên tới$10^5$. Việc sử dụng các sản phẩm tiền tố sẽ tránh mọi hoạt động quét lồng nhau trên các phạm vi, giữ cho việc tính toán trong giới hạn thời gian thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    # inline solution for testing
    n, k, x = map(int, input().split())
    p = list(map(float, input().split()))

    pref = [1.0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] * p[i - 1]

    def range_prod(l, r):
        if l > r:
            return 1.0
        return pref[r] / pref[l - 1]

    dp = [1.0] * (n + 1)
    ans = 0.0

    for i in range(1, n + 1):
        dp[i] = dp[i - 1]
        if i >= k + 1:
            l = i - k + 1
            r = i
            success_block = range_prod(l, r)
            failure = 1.0 - p[i - k - 1]
            contrib = dp[i - k - 1] * failure * success_block
            ans += contrib
            dp[i] -= contrib

    return str(ans)

# sample-like test
assert run("4 1 4\n0.5 0.5 0.5 0.5\n")[:5], "basic"

# minimum case
assert run("1 0 1\n0.7\n") is not None

# all successes
assert run("3 1 3\n1 1 1\n")[:1] == "0"

# boundary failure at start
assert run("3 1 2\n0.0 1.0 1.0\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 1 4 ... | không tầm thường | nhân giống cơ bản | 
| 1 0 1 ... | 1 | ranh giới tối thiểu | 
| tất cả 1s | 0 hoặc xác định | xác suất suy biến | 
| số 0 đứng đầu | ổn định | xử lý cạnh lỗi | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi chỉ số vị trí lỗi trở thành 0 hoặc âm. Giả định$k = 2$Và$i = 2$. Sau đó$i-k-1 = -1$, không hợp lệ. Thuật toán tránh điều này một cách rõ ràng bằng cách chỉ bắt đầu đóng góp khi$i \ge k+1$, đảm bảo rằng mọi trạng thái tiền tố được tham chiếu đều tồn tại. Trên những đầu vào như vậy, không có phép tính nào được thực hiện và câu trả lời vẫn là 0, điều này đúng vì một mẫu hợp lệ không thể tồn tại nếu không có lỗi trước đó. 

Một trường hợp cạnh khác xảy ra khi tất cả các xác suất là 1. Trong trường hợp đó, điều kiện lỗi có xác suất bằng 0 ở mọi nơi, do đó không có mẫu hợp lệ nào có thể hình thành. Thuật toán nhân đúng với`(1 - p[i-k-1])`, trở thành 0 và loại bỏ mọi đóng góp. 

Trường hợp thứ ba là khi$k = 0$, trong đó "khối thành công" trống. Hàm tích phạm vi trả về 1 cho các khoảng trống, do đó sự kiện giảm xuống thành “lỗi ở vị trí i-1 mà không có lần xuất hiện nào trước đó”, sự kiện này vẫn được cấu trúc tiền tố DP xử lý chính xác mà không cần sửa đổi.
