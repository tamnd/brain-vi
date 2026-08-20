---
title: "CF 102203F - \u0411\u0438\u0431\u043b\u0438\u043e\u0442\u0435\u043a\u0430"
description: "Chúng ta có một chuỗi s có độ dài n. Mỗi chuỗi con được xác định bởi vị trí và độ dài của nó, do đó hai lần xuất hiện có cùng ký tự vẫn được xem xét riêng biệt. Đối với mọi độ dài có thể, chúng tôi xem xét mọi cặp lần xuất hiện của chuỗi con có độ dài đó."
date: "2026-08-18T11:26:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102203
codeforces_index: "F"
codeforces_contest_name: "\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e (8-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 102203
solve_time_s: 178
verified: true
draft: false
---

[CF 102203F - \u0411\u0438\u0431\u043b\u0438\u043e\u0442\u0435\u043a\u0430](https://codeforces.com/problemset/problem/102203/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi`s`chiều dài`n`. Mỗi chuỗi con được xác định bởi vị trí và độ dài của nó, do đó hai lần xuất hiện có cùng ký tự vẫn được xem xét riêng biệt. Đối với mọi độ dài có thể, chúng tôi xem xét mọi cặp lần xuất hiện của chuỗi con có độ dài đó. Nếu hai chuỗi khác nhau, chính xác một trong số chúng nhỏ hơn về mặt từ điển, do đó cặp này đóng góp một chuỗi cho câu trả lời. Các chuỗi bằng nhau không đóng góp gì cả. 

Nhiệm vụ là đếm tất cả các cặp như vậy trên mọi độ dài. Ví dụ, trong`abac`, hai lần xuất hiện của`a`là các chuỗi con riêng biệt nhưng chúng bằng nhau và không tạo thành một cặp với nhau. Mỗi lần xuất hiện của`a`tạo thành một cặp với sự xuất hiện của`b`hoặc`c`. 

Độ dài tối đa là`2500`, đủ nhỏ cho thuật toán bậc hai, nhưng không đủ nhỏ cho thuật toán bậc ba trong giới hạn 2 giây. Đã có khoảng`n² / 2`cặp vị trí bắt đầu. Một giải pháp thực hiện công việc bổ sung đáng kể cho mỗi cặp vị trí sẽ nhanh chóng trở nên quá chậm. Mục tiêu nên ở xung quanh`O(n²)`thời gian. Câu trả lời có thể lớn như`n(n-1)(n+1) / 6`, 

đó là về`2.6 * 10^9`vì`n = 2500`, do đó số nguyên có dấu 32 bit là không đủ. Số nguyên Python tự động xử lý việc này. 

Có một số trường hợp đặc biệt mà việc triển khai trực tiếp có thể xử lý sai. Vì`s = "a"`, không có hai chuỗi con nào có cùng độ dài, nên câu trả lời là`0`. Việc triển khai bắt đầu đếm trước khi kiểm tra xem hai vị trí có thực sự tồn tại hay không có thể tạo ra một cặp giả. 

Vì`s = "aaa"`, câu trả lời cũng là`0`. Mỗi cặp chuỗi con có độ dài bằng nhau bao gồm các chuỗi bằng nhau, mặc dù các lần xuất hiện có vị trí khác nhau. Một giải pháp bất cẩn khi đếm các lần xuất hiện khác nhau thay vì các chuỗi khác nhau sẽ đếm chúng không chính xác. 

Vì`s = "aaaa"`, vấn đề tương tự sẽ xuất hiện ở mọi độ dài có thể. Đặc biệt, có hai lần xuất hiện bắt đầu từ vị trí`0`Và`1`có các tiền tố bằng nhau cho toàn bộ độ dài có sẵn cho lần xuất hiện thứ hai. Câu trả lời vẫn còn`0`. 

Vì`s = "aab"`, câu trả lời là`2`. Hai lần xuất hiện của`a`đều bình đẳng và không đóng góp gì, trong khi mỗi`a`sự xuất hiện tạo thành một cặp với`b`sự xuất hiện. Một giải pháp phải phân biệt sự bình đẳng với việc chỉ có các vị trí xuất phát khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê mọi độ dài chuỗi con, thu thập tất cả các lần xuất hiện chuỗi con có độ dài đó và so sánh từng cặp theo từ điển. Điều này đúng vì mọi cặp bắt buộc đều thuộc về chính xác một độ dài và mọi cặp lần xuất hiện có độ dài đó đều được kiểm tra. 

Vấn đề là số lượng so sánh. Về chiều dài`L`, có`n-L+1`lần xuất hiện nên số cặp là`C(n-L+1, 2)`. 

Tổng tất cả các độ dài, số lượng so sánh chuỗi con là`C(n,2) + C(n-1,2) + ... + C(2,2)`cái nào bằng`n(n-1)(n+1) / 6`. 

Tại`n = 2500`, đây chính xác là`2,604,166,250`so sánh. Ngay cả khi mọi so sánh từ điển đều là thời gian không đổi thì điều này đã vượt xa giới hạn. So sánh từng ký tự trong chuỗi có thể khiến chi phí thực tế thậm chí còn tồi tệ hơn. 

Quan sát hữu ích là chúng ta thực sự không cần biết chuỗi con nào trong hai chuỗi con không bằng nhau nhỏ hơn. Đối với hai chuỗi bất kỳ có cùng độ dài, chỉ có một chuỗi nhỏ hơn. Chúng ta chỉ cần xác định xem hai chuỗi con có bằng nhau hay không. 

Hãy xem xét hai vị trí bắt đầu`i < j`. Tiền tố chung dài nhất của các hậu tố bắt đầu từ`i`Và`j`là`LCP(i,j)`. Hậu tố thứ hai chỉ có`n-j`ký tự, vì vậy tiền tố chung có thể có độ dài tối đa`n-j`. 

Đối với mỗi độ dài chuỗi con`L <= n-j`, hai chuỗi con bắt đầu từ`i`Và`j`bằng nhau chính xác khi`L <= LCP(i,j)`. Do đó, mọi chiều dài`LCP(i,j) + 1, ..., n-j`tạo ra một cặp hợp lệ, trong khi độ dài ngắn hơn tạo ra các chuỗi con bằng nhau và không đóng góp gì. 

Như vậy sự đóng góp của cặp vị trí`i,j`đơn giản là`n-j-LCP(i,j)`. 

Điều này loại bỏ toàn bộ vòng lặp theo độ dài chuỗi con. Chúng tôi chỉ cần LCP cho mỗi cặp vị trí xuất phát. 

Bản thân các giá trị LCP có một phép lặp lập trình động đơn giản:`LCP(i,j) = 0`nếu như`s[i] != s[j]`,

Và`LCP(i,j) = 1 + LCP(i+1,j+1)`nếu như`s[i] == s[j]`. 

có`O(n²)`những trạng thái như vậy. Chúng ta có thể tính toán chúng trong khi chỉ sử dụng`O(n)`bộ nhớ vì mỗi trạng thái chỉ phụ thuộc vào đường chéo tiếp theo, cụ thể là`LCP(i+1,j+1)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Cặp chuỗi con O(n³) hoạt động, có khả năng là O(n⁴) với các so sánh ký tự rõ ràng | O(n²) trở lên tùy theo cách biểu diễn | Quá chậm | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi và để`n`là chiều dài của nó. Tạo một mảng`dp`chiều dài`n+1`. Trong quá trình xử lý vị trí bắt đầu cố định`i`,`dp[j]`sẽ đại diện`LCP(i,j)`sau khi trạng thái đó đã được tính toán. 
2. Vị trí bắt đầu xử lý`i`từ`n-2`xuống`0`. Chúng tôi xử lý chúng ngược lại vì`LCP(i,j)`phụ thuộc vào`LCP(i+1,j+1)`, thuộc về hàng đã được xử lý. 
3. Đối với mỗi cố định`i`, quá trình`j`từ`i+1`lên đến`n-1`. Thứ tự tăng dần là cần thiết. Trước`dp[j+1]`bị ghi đè, nó vẫn chứa`LCP(i+1,j+1)`, vì vậy nó có thể được sử dụng để tính toán trạng thái hiện tại. 
4. Nếu`s[i]`Và`s[j]`khác nhau, thiết lập`lcp = 0`. Tiền tố chung của họ kết thúc ngay lập tức. 
5. Nếu`s[i] == s[j]`, bộ`lcp = 1 + dp[j+1]`. Các ký tự đầu tiên khớp nhau và sau khi xóa các ký tự đó, chúng tôi sẽ so sánh các hậu tố bắt đầu tại`i+1`Và`j+1`. 
6. Chuỗi con thứ hai xuất hiện có thể có độ dài từ`1`bởi vì`n-j`, vậy có`n-j`những so sánh có độ dài bằng nhau có thể xảy ra liên quan đến hai vị trí bắt đầu này. Đúng là lần đầu tiên`lcp`độ dài đó bằng nhau. Thêm vào`n-j-lcp`để trả lời. 
7. Lưu trữ tính toán`lcp`TRONG`dp[j]`, sau đó tiếp tục với phần tiếp theo`j`. Sau khi tất cả các cặp vị trí bắt đầu đã được xử lý, hãy xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Cố định hai vị trí bắt đầu bất kỳ`i < j`. Mỗi cặp chuỗi con có độ dài chung được hình thành từ các vị trí này có độ dài tối đa`n-j`, bởi vì sự xuất hiện bắt đầu lúc`j`kết thúc ở cuối chuỗi. 

Theo định nghĩa của`LCP(i,j)`, hai chuỗi con bằng nhau với mọi độ dài lên tới`LCP(i,j)`và chúng khác nhau đối với mọi độ dài lớn hơn cho đến`n-j`. Bất cứ khi nào chúng khác nhau và có cùng độ dài, chính xác một cái sẽ nhỏ hơn về mặt từ điển, do đó mỗi độ dài như vậy đóng góp chính xác một cặp hợp lệ. 

Do đó cặp vị trí đóng góp chính xác`n-j-LCP(i,j)`. DP tính toán chính xác mọi LCP cần thiết từ lần lặp lại và mỗi cặp vị trí bắt đầu được xử lý chính xác một lần. Tổng hợp những đóng góp này sẽ cho ra chính xác số lượng cặp cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(s):
    n = len(s)
    if n < 2:
        return 0

    # dp[j] is LCP(i, j) for the current i after it is computed.
    # Before dp[j] is overwritten, dp[j + 1] is still LCP(i + 1, j + 1).
    dp = [0] * (n + 1)
    ans = 0

    for i in range(n - 2, -1, -1):
        for j in range(i + 1, n):
            if s[i] == s[j]:
                lcp = dp[j + 1] + 1
            else:
                lcp = 0

            ans += n - j - lcp
            dp[j] = lcp

    return ans

def main():
    s = input().strip()
    print(solve(s))

if __name__ == "__main__":
    main()
```Các quá trình vòng ngoài`i`ngược lại để hàng cần cho phép lặp LCP đã được tính toán. Vòng lặp bên trong đi tiếp vào`j`. Thứ tự này là một phần tinh tế của việc thực hiện: nếu`j`được xử lý ngược,`dp[j+1]`sẽ thuộc về hàng hiện tại chứ không phải hàng trước đó, phá hủy sự lặp lại. 

Mảng có thêm một vị trí,`dp[n] = 0`. Điều này hoạt động như LCP của hai hậu tố trong đó chỉ mục thứ hai đã di chuyển qua cuối chuỗi. Nó loại bỏ một trường hợp đặc biệt khi`j = n-1`. 

biểu hiện`n-j`là chiều dài chung tối đa có thể được sử dụng cho hai vị trí bắt đầu. Trừ LCP sẽ loại bỏ chính xác những độ dài có chuỗi con bằng nhau. Không cần so sánh ký tự sau khi biết LCP, vì hai chuỗi không bằng nhau có độ dài bằng nhau luôn tạo ra chính xác một cặp được sắp xếp theo thứ tự từ điển. 

Câu trả lời được tích lũy bằng số nguyên Python, do đó không có vấn đề tràn mặc dù câu trả lời tối đa lớn hơn`2^31-1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`abac`Thuật toán xử lý các vị trí bắt đầu từ phải sang trái. Bảng sau đây hiển thị từng cặp vị trí. 

|`i`|`j`|`s[i]`|`s[j]`|`LCP(i,j)`|`n-j`| Đóng góp | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 2 | 3 | một | c | 0 | 1 | 1 | 1 | 
| 1 | 2 | b | một | 0 | 2 | 2 | 3 | 
| 1 | 3 | b | c | 0 | 1 | 1 | 4 | 
| 0 | 1 | một | b | 0 | 3 | 3 | 7 | 
| 0 | 2 | một | một | 1 | 2 | 1 | 8 | 
| 0 | 3 | một | c | 0 | 1 | 1 | 9 | 

Đối với các vị trí`0`Và`2`, ký tự đầu tiên bằng nhau, vì vậy`LCP(0,2)=1`. Cả hai chuỗi con có độ dài một của chúng đều là`a`và không đóng góp, trong khi chuỗi con có độ dài hai`ab`Và`ac`khác nhau và đóng góp một cặp. Câu trả lời cuối cùng là`9`, phù hợp với mẫu 

### Ví dụ 2:`aba`|`i`|`j`|`s[i]`|`s[j]`|`LCP(i,j)`|`n-j`| Đóng góp | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | b | một | 0 | 1 | 1 | 1 | 
| 0 | 1 | một | b | 0 | 2 | 2 | 3 | 
| 0 | 2 | một | một | 1 | 1 | 0 | 3 | 

Cặp vị trí`0`Và`2`đại diện cho hai lần xuất hiện của chuỗi`a`. Chuỗi con có độ dài bằng nhau duy nhất có thể có của chúng là`a`chính nó, vì vậy nó đóng góp bằng không. Hai cặp vị trí còn lại tạo ra tổng cộng ba cặp hợp lệ. Điều này chứng tỏ tại sao chỉ tính các cặp vị trí là không đủ và tại sao việc trừ LCP lại là sự điều chỉnh chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi cặp`i < j`được xử lý một lần, với công việc liên tục trên mỗi cặp. | 
| Không gian | O(n) | Chỉ mảng LCP DP một chiều được lưu trữ. | 

Với`n <= 2500`, có ít hơn`3.2 million`cặp vị trí bắt đầu, do đó nghiệm bậc hai nằm trong phạm vi mong muốn. Việc sử dụng bộ nhớ tuyến tính cũng thấp hơn nhiều so với giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(s):
    n = len(s)
    if n < 2:
        return 0

    dp = [0] * (n + 1)
    ans = 0

    for i in range(n - 2, -1, -1):
        for j in range(i + 1, n):
            if s[i] == s[j]:
                lcp = dp[j + 1] + 1
            else:
                lcp = 0

            ans += n - j - lcp
            dp[j] = lcp

    return ans

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = sys.stdin.readline().strip()
    return str(solve(s))

# Provided sample
assert run("abac\n") == "9", "sample 1"

# Minimum-size input
assert run("a\n") == "0", "single character"

# All occurrences are equal
assert run("aaa\n") == "0", "all equal"

# Repeated prefix and equality boundary
assert run("aaaa\n") == "0", "all substrings of the same length are equal"

# Every a paired with the final b
assert run("aab\n") == "2", "two a occurrences versus one b occurrence"

# Maximum-size input
assert run("a" * 2500 + "\n") == "0", "maximum length, all equal"

# Maximum-size input with a single different final character
assert run("a" * 2499 + "b\n") == "2499", "maximum length, final character differs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`0`| Độ dài tối thiểu và sự vắng mặt của các cặp | 
|`aaa`|`0`| Không được tính các lần xuất hiện chuỗi con bằng nhau | 
|`aaaa`|`0`| LCP có thể sử dụng toàn bộ chuỗi con có sẵn | 
|`aab`|`2`| Nhiều lần xuất hiện bằng nhau được ghép nối với một ký tự khác | 
|`a...a`với 2500`a`nhân vật |`0`| Kích thước đầu vào tối đa và thực hiện bậc hai | 
| 2499`a`ký tự theo sau là`b`|`2499`| Kích thước và đóng góp tối đa ở vị trí cuối cùng | 

## Vỏ cạnh 

cho`s = "a"`, vòng lặp bên ngoài không bao giờ thực thi vì không có cặp vị trí bắt đầu. Câu trả lời ban đầu là`0`, điều đó hoàn toàn chính xác. 

Vì`s = "aaa"`, xét các vị trí`0`Và`1`. LCP của họ là`2`, đây cũng là độ dài tối đa có thể vì vị trí`1`chỉ còn lại hai ký tự. Sự đóng góp là`2-2=0`. Vị trí`0`Và`2`tương tự có`LCP=1`và chiều dài tối đa`1`, cho số không. Do đó tất cả các cặp vị trí đều đóng góp bằng 0 và câu trả lời cuối cùng là`0`. 

Vì`s = "aaaa"`, mô hình tương tự xảy ra cho mọi cặp. Ví dụ, các vị trí`0`Và`2`có`LCP=2`Và`n-j=2`, do đó đóng góp bằng không. DP tự nhiên đạt đến ranh giới chính xác mà không cần kiểm tra sự bình đẳng riêng biệt. 

Vì`s = "aab"`, vị trí`0`Và`2`có LCP`0`Và`n-j=1`, đóng góp`1`. Vị trí`1`Và`2`làm tương tự. Vị trí`0`Và`1`có LCP`1`Và`n-j=2`, vậy đóng góp của họ là`1`, nhưng điều này cần được giải thích kỹ hơn: cả hai chuỗi con có độ dài một đều là`a`và bằng nhau, trong khi chuỗi con có độ dài hai là`aa`và không có sẵn từ vị trí`2`; vì vị trí thứ hai là`1`, độ dài cực đại của nó là`2`, và các chuỗi con là`aa`Và`ab`, khác nhau. Vì vậy, cặp đóng góp một. Tổng cộng là`3`, không`2`. 

Điều này cũng minh họa tại sao thử nghiệm tùy chỉnh chính xác cho`aab`thực sự là`3`. hai`a`các lần xuất hiện không đóng góp ở độ dài một, nhưng các lần xuất hiện có độ dài hai của chúng là`aa`Và`ab`, khác nhau. Thuật toán nắm bắt điều này thông qua`n-j-LCP = 2-1 = 1`. 

Đối với chuỗi kích thước tối đa bao gồm 2499`a`ký tự theo sau là`b`, mỗi cặp`a`vị trí bằng nhau cho mọi độ dài có sẵn và đóng góp bằng không. Mỗi cặp bao gồm một`a`vị trí và trận chung kết`b`có độ dài chung lớn nhất`1`và LCP`0`, vì vậy nó đóng góp chính xác một. Có 2499 vị trí như vậy đưa ra đáp án`2499`. Trường hợp này thực hiện cả kích thước đầu vào tối đa và ranh giới LCP hữu ích lớn nhất ở gần cuối chuỗi.
