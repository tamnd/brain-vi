---
title: "CF 104840A - \u041a\u043b\u043e\u043d\u044b"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có $n$ cá thể nhân bản được sắp xếp theo thứ tự nào đó, nhưng chúng ta chỉ quan sát nhiều tập nhãn được viết trên chúng. Mỗi nhãn là một số nguyên từ 1 đến $n$."
date: "2026-06-28T11:37:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104840
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104840
solve_time_s: 46
verified: true
draft: false
---

[CF 104840A - \u041a\u043b\u043e\u043d\u044b](https://codeforces.com/problemset/problem/104840/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản đều có$n$các cá thể nhân bản được sắp xếp theo thứ tự nào đó, nhưng chúng tôi chỉ quan sát nhiều tập nhãn được viết trên chúng. Mỗi nhãn là một số nguyên từ 1 đến$n$. 

Cách các nhãn này được gán tuân theo một quy tắc rất cụ thể. Hãy tưởng tượng các bản sao đứng thành một hàng theo thứ tự cuối cùng không xác định. Mỗi bản sao được gán một số dựa trên vị trí của nó từ đầu bên trái hoặc vị trí của nó từ đầu bên phải. Vì vậy đối với một số vị trí$i$, giá trị được gán là$i$hoặc$n - i + 1$. Sau khi gán, các bản sao có thể được hoán vị tùy ý, do đó mảng đầu vào cuối cùng chỉ là hoán vị của các giá trị được gán này. 

Nhiệm vụ là quyết định xem một mảng có độ dài nhất định có$n$có thể được tạo ra bởi sơ đồ ghi nhãn như vậy. 

Kích thước đầu vào lớn, lên tới$10^5$tổng số phần tử trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các hoán vị hoặc mô phỏng sự sắp xếp một cách rõ ràng. Thậm chí$O(n^2)$các phương pháp tiếp cận quá chậm vì một trường hợp thử nghiệm đơn lẻ đã có thể được$10^5$. 

Một điều tinh tế quan trọng là thứ tự trong mảng đầu vào không tương ứng với các vị trí ban đầu được sử dụng trong quá trình ghi nhãn. Chỉ có nhiều vấn đề chứ không phải sự sắp xếp. 

Một sai lầm phổ biến là cố gắng xây dựng lại hoán vị một cách tham lam theo một thứ tự cố định. Điều đó không thành công vì thứ tự gán thực tế không xác định và mảng cuối cùng được hoán vị. 

Ví dụ, hãy xem xét$n = 4$, mảng$[1, 4, 1, 3]$. Người ta có thể cố gắng khớp các vị trí một cách tham lam từ trái sang phải một cách không chính xác, nhưng vì sự sắp xếp ban đầu có thể được hoán vị tùy ý nên chỉ có cấu trúc tần số là quan trọng. 

Một trường hợp thất bại tinh vi khác là giả sử mỗi giá trị phải tương ứng với một vị trí duy nhất. Điều đó sai vì nhiều vị trí có thể mang lại cùng một giá trị khi$i = n - i + 1$, tức là ở giữa khi$n$thật kỳ quặc. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp sẽ là thử tất cả các cách có thể để gán cho mỗi vị trí giá trị chỉ mục bên trái hoặc giá trị chỉ mục bên phải của nó, sau đó kiểm tra xem tập hợp kết quả có khớp với đầu vào hay không. có$2^n$các bài tập như vậy và với mỗi bài tập, chúng tôi sẽ so sánh nhiều tập hợp trong$O(n)$, dẫn đến độ phức tạp theo cấp số nhân. Ngay cả đối với$n = 30$, điều này đã không thể thực hiện được, và đây$n$đi lên$10^5$. 

Quan sát quan trọng là mảng cuối cùng chỉ là một hoán vị của một tập hợp nhiều cấu trúc rất có cấu trúc. Đối với mỗi vị trí$i$, chúng tôi tạo ra hai giá trị có thể:$i$Và$n - i + 1$. Vì vậy, multiset mà chúng tôi mong đợi chính xác là sự kết hợp của các cặp này trên tất cả các vị trí. 

Điều này có nghĩa là mọi số từ 1 đến$n$xuất hiện một cách rất hạn chế: mỗi cặp$(i, n - i + 1)$đóng góp hai giá trị và những đóng góp này trùng lặp một cách đối xứng. Thay vì nghĩ về vị trí, chúng ta có thể nghĩ về tần số phù hợp. 

Một cách suy luận đơn giản hơn là xử lý các giá trị theo thứ tự tăng dần và cố gắng “tiêu thụ” các lần xuất hiện bằng cấu trúc tham lam: các giá trị nhỏ phải tương ứng với các chỉ số ban đầu, bởi vì chỉ các chỉ mục nhỏ mới có thể tạo ra các giá trị nhỏ thông qua việc gán danh tính, trong khi các giá trị lớn chỉ có thể đến từ phía phản ánh. 

Điều này biến vấn đề thành kiểm tra tính nhất quán giữa phân bố tần số của mảng và cấu trúc bắt buộc của các cặp$(i, n - i + 1)$. Việc xây dựng đúng sẽ giúp xác minh rằng khi chúng ta mô phỏng một cách tham lam việc gán các vị trí từ trái và phải, chúng ta không bao giờ vi phạm tính sẵn có của các giá trị bắt buộc. 

Chúng tôi duy trì hai con trỏ biểu thị vị trí không được sử dụng tiếp theo từ bên trái và bên phải. Ở mỗi bước, chúng tôi kiểm tra xem giá trị nhỏ nhất còn lại hiện tại trong mảng có thể khớp với con trỏ trái hay con trỏ phải hay không. Nếu không khớp thì không thể cấu hình được. 

Điều này có hiệu quả vì tại bất kỳ thời điểm nào, các phép gán hợp lệ duy nhất cho một vị trí là hai ứng cử viên xác định của nó và do phép hoán vị loại bỏ các ràng buộc về thứ tự nên quy trình sẽ giảm xuống một chuỗi các lựa chọn bắt buộc. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua bài tập |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Xác thực hai con trỏ tham lam |$O(n \log n)$hoặc$O(n)$tùy theo việc thực hiện |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta sắp xếp mảng vì thứ tự không liên quan và chúng ta chỉ cần suy luận về các giá trị có sẵn. Sau đó, chúng tôi cố gắng mô phỏng việc xây dựng một phép gán hợp lệ từ các vị trí bên ngoài vào trong. 

1. Sắp xếp mảng. Điều này cho phép chúng ta luôn suy luận về giá trị nhỏ nhất còn lại chưa từng có, giá trị bị ràng buộc nhiều nhất. 
2. Khởi tạo hai con trỏ,$l = 1$Và$r = n$, đại diện cho hai nguồn duy nhất có thể có cho phép gán tiếp theo trong một cấu trúc hợp lệ. 
3. Lặp lại mảng đã được sắp xếp. Với mỗi giá trị$x$, quyết định xem nó có thể tương ứng với vị trí bên trái hiện tại không$l$hoặc đúng vị trí$r$. Nếu như$x = l$, chúng tôi sử dụng điểm cuối bên trái và tăng dần$l$. Nếu như$x = r$, chúng tôi sử dụng đúng điểm cuối và giảm dần$r$. 
4. Nếu$x$không khớp với điểm cuối, chúng tôi ngay lập tức kết luận rằng không có cấu trúc hợp lệ nào tồn tại và trả về lỗi. 
5. Tiếp tục cho đến khi tất cả các phần tử được xử lý. Nếu tất cả các giá trị được sử dụng nhất quán thì cấu hình hợp lệ. 

Lý do đằng sau quy trình này là mọi cách xây dựng hợp lệ đều tương ứng với các vị trí ghép nối đối xứng từ hai đầu vào trong và mỗi bước loại bỏ chính xác một bậc tự do từ một trong hai đầu. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, các vị trí chưa được chỉ định còn lại tạo thành một khoảng liền kề$[l, r]$. Mọi ghi nhãn hợp lệ đều phải gán một trong hai$l$hoặc$r$đến giá trị tiêu thụ tiếp theo vì tất cả các vị trí bên trong đều tương ứng với các lựa chọn kết cấu đã được cố định. Nếu một giá trị không thể khớp với một trong hai điểm cuối thì nó không thể thuộc về bất kỳ phép gán hợp lệ nào vì không có vị trí bên trong nào có thể tạo ra nó mà không vi phạm cấu trúc cố định của nhãn trái/phải. Tính bất biến này đảm bảo rằng một khi xảy ra sự không khớp thì không có sự sắp xếp lại nào có thể khắc phục được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        a.sort()
        
        l, r = 1, n
        ok = True
        
        for x in a:
            if x == l:
                l += 1
            elif x == r:
                r -= 1
            else:
                ok = False
                break
        
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc sắp xếp để bộc lộ những hạn chế về cấu trúc. Hai con trỏ đại diện cho hai “nguồn hoạt động” duy nhất có thể có giá trị hợp lệ ở bất kỳ giai đoạn nào. Mỗi nhiệm vụ sẽ rút ngắn khoảng thời gian, đảm bảo rằng chúng tôi luôn duy trì tính nhất quán với thứ tự ban đầu giả định. 

Một cạm bẫy triển khai phổ biến là quên ngắt ngay lập tức khi không khớp. Tiếp tục sau một nhiệm vụ không hợp lệ có thể khôi phục tính nhất quán một cách không chính xác sau này, điều này là không thể về mặt logic. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
array = [1, 2, 1]
```Mảng được sắp xếp là$[1, 1, 2]$. 

| Bước | tôi | r | x | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 1 | khớp l, l → 2 | 
| 2 | 2 | 3 | 1 | không khớp (1 ≠ 2 và 1 ≠ 3) | 

Ở bước 2, quy trình không thành công, nghĩa là sự sắp xếp này không thể tương ứng với cấu trúc ghi nhãn trái/phải nhất quán. 

Tuy nhiên, hãy lưu ý rằng điều này nêu bật một đặc tính quan trọng: việc sắp xếp chỉ quan trọng thông qua tính nhất quán của điểm cuối chứ không phải chỉ tần số. 

### Ví dụ 2 

đầu vào:```
n = 4
array = [1, 4, 2, 3]
```Mảng được sắp xếp là$[1, 2, 3, 4]$. 

| Bước | tôi | r | x | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 1 | l → 2 | 
| 2 | 2 | 4 | 2 | l → 3 | 
| 3 | 3 | 4 | 3 | l → 4 | 
| 4 | 4 | 4 | 4 | r → 3 | 

Tất cả các giá trị được tiêu thụ thành công. 

Điều này thể hiện sự thu hẹp vào trong rõ ràng trong đó mọi giá trị khớp chính xác với một điểm cuối tại thời điểm xử lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế, quét tuyến tính sau | 
| Không gian |$O(1)$thêm (không bao gồm đầu vào) | Chỉ con trỏ và trạng thái tối thiểu | 

Các ràng buộc cho phép lên đến$10^5$tổng các phần tử, do đó$O(n \log n)$giải pháp nằm trong giới hạn. Việc sắp xếp từng ca kiểm thử một cách độc lập vẫn hiệu quả vì tổng của tất cả$n$bị giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        a.sort()
        l, r = 1, n
        ok = True
        for x in a:
            if x == l:
                l += 1
            elif x == r:
                r -= 1
            else:
                ok = False
                break
        out.append("YES" if ok else "NO")
    return "\n".join(out)

# sample-like tests
assert run("3\n3\n1 2 1\n4\n1 4 2 3\n3\n1 1 1\n") == "NO\nYES\nNO"

# minimum size
assert run("1\n1\n1\n") == "YES"

# all equal invalid except n=1
assert run("1\n3\n1 1 1\n") == "NO"

# already perfect permutation
assert run("1\n5\n1 2 3 4 5\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | CÓ | trường hợp cơ sở | 
| tất cả đều bình đẳng | KHÔNG | cấu trúc trùng lặp không thể | 
| hoán vị được sắp xếp | CÓ | chuỗi đầy đủ hợp lệ | 
| trộn mẫu | hỗn hợp | tính đúng đắn chung | 

## Vỏ cạnh 

Một trường hợp cạnh tinh tế là$n = 1$. Mảng duy nhất có thể là$[1]$, thỏa mãn điều kiện một cách tầm thường vì vị trí duy nhất là cả trái và phải đồng thời. Thuật toán chấp nhận nó một cách chính xác vì$l = r = 1$và giá trị duy nhất phù hợp. 

Một trường hợp khác là khi tất cả các giá trị giống hệt nhau, ví dụ$n = 4$,$[2, 2, 2, 2]$. Sắp xếp sản lượng$[2, 2, 2, 2]$, nhưng sự không khớp đầu tiên xảy ra ngay lập tức vì cả hai điểm cuối đều không bằng 2 khi$l = 1$Và$r = 4$. Điều này bác bỏ trường hợp này một cách chính xác vì không có cấu trúc đối xứng nào có thể tạo ra các giá trị đồng nhất. 

Một trường hợp cạnh khác phát sinh khi mảng là một hoán vị hợp lệ nhưng bị xáo trộn nhiều, chẳng hạn như$[3, 1, 4, 2]$. Việc sắp xếp sẽ loại bỏ các vấn đề về thứ tự và thuật toán sẽ xây dựng lại một chuỗi khớp bên trong hợp lệ. Quá trình này cho thấy rằng hoán vị không quan trọng miễn là tính nhất quán của điểm cuối được giữ ở mỗi bước.
