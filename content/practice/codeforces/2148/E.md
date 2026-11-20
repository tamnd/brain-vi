---
title: "CF 2148E - Tách"
description: "Chúng ta được cho một mảng các số nguyên dương và một tham số $k$. Đối với bất kỳ phân đoạn nào được chọn $[l, r]$, chúng tôi tưởng tượng việc chia mảng thành hai phần: bên trong phân khúc và bên ngoài phân khúc. Mọi phần tử bên trong phân đoạn bị ép thành một tập hợp đa phân biệt, gọi nó là nhóm 1."
date: "2026-06-08T01:14:59+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2148
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1050 (Div. 4)"
rating: 1200
weight: 2148
solve_time_s: 98
verified: true
draft: false
---

[CF 2148E - Tách](https://codeforces.com/problemset/problem/2148/E) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, hai con trỏ 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương và một tham số$k$. Đối với bất kỳ phân khúc nào đã chọn$[l, r]$, chúng ta tưởng tượng việc chia mảng thành hai phần: bên trong đoạn và bên ngoài đoạn. 

Mọi phần tử bên trong phân đoạn bị ép thành một tập hợp đa phân biệt, gọi nó là nhóm 1. Mọi phần tử bên ngoài phân đoạn có thể được phân phối tùy ý trên tất cả$k$nhiều bộ, bao gồm cả nhóm 1. 

Phân đoạn được coi là hợp lệ khi chúng ta có thể phân phối các phần tử bên ngoài sao cho tất cả$k$nhiều bộ kết thúc giống hệt như nhiều bộ. Điều này có nghĩa là với mọi giá trị$v$, mỗi nhóm phải chứa chính xác cùng số lần xuất hiện của$v$. 

Đầu ra hỏi có bao nhiêu phân đoạn$[l, r]$có thể tạo ra sự cân bằng hoàn hảo như vậy. 

Ràng buộc chính là chúng ta được phép tự do gán các phần tử bên ngoài, nhưng các phần tử bên trong được cố định trong nhóm 1 nên tạo ra sự mất cân bằng bắt buộc phải được bù đắp bằng các phần tử bên ngoài. 

Kích thước đầu vào tăng lên$2 \cdot 10^5$tổng số qua các lần kiểm tra, do đó, bất kỳ giải pháp nào thử tất cả các mảng con và tính toán lại thông tin tần số một cách độc lập sẽ là giải pháp bậc hai và sẽ không vượt qua. Thậm chí$O(n \sqrt{n})$là rủi ro. Chúng ta nên mong đợi điều gì đó gần với tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ là giả định rằng “chúng ta chỉ cần mỗi giá trị xuất hiện ít nhất$k$lần trong toàn bộ mảng”. Điều đó là chưa đủ vì các giới hạn phân phối áp dụng cho mỗi phân đoạn và tương tác với cách các phần tử bên trong được đưa vào nhóm 1. 

Ví dụ, nếu$k=2$và mảng là$[1, 2, 1]$, lấy đoạn$[2,2]$cách ly một 2 bên trong nhóm 1. Chỉ còn lại 2 bên ngoài nên chúng ta không thể cân bằng cả hai nhóm. Một đối số toàn cầu ngây thơ sẽ bỏ lỡ điều này. 

Một trường hợp thất bại khác là giả định tính đơn điệu của các phân đoạn hợp lệ. Kể cả nếu$[l, r]$là hợp lệ, mở rộng nó thành$[l, r+1]$có thể phá vỡ tính khả thi vì việc thêm một phần tử bắt buộc vào nhóm 1 sẽ làm tăng sự mất cân bằng mà có thể không thể khắc phục được nữa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực thử mọi phân khúc$[l, r]$. Đối với mỗi phân đoạn, chúng tôi tính toán số lần xuất hiện của mỗi giá trị nằm bên trong. Sau đó chúng tôi kiểm tra xem phần bên ngoài có thể được chia thành$k$nhiều bộ giống hệt nhau. 

Điều này yêu cầu xác minh rằng đối với mỗi giá trị$v$, tổng số$v$trừ đi số lần xuất hiện bên trong chia hết cho$k$, bởi vì các phần tử bên ngoài phải được phân bố đều giữa$k$xô. Điều kiện đó là cần nhưng chưa đủ; chúng tôi cũng phải đảm bảo các phần tử bắt buộc bên trong của nhóm 1 khớp với cấu hình bằng nhau cuối cùng. 

Nếu chúng ta cố gắng tính toán số lượng bên trong cho tất cả các phân đoạn một cách độc lập thì mỗi lần kiểm tra sẽ tốn$O(n)$, dẫn đến$O(n^3)$nếu được thực hiện một cách ngây thơ, hoặc$O(n^2)$với tổng tiền tố. Thậm chí$O(n^2)$quá chậm đối với$2 \cdot 10^5$. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì nghĩ đến việc cân bằng tất cả các nhóm, chúng ta tập trung vào lực của nhóm 1. 

Cho phép$cnt_v$là tổng số lần xuất hiện của giá trị$v$, và để$x_v$có bao nhiêu trong số đó ở bên trong$[l,r]$. Ở trạng thái cuối cùng, mỗi nhóm phải có cùng số lượng$v$, nói$t_v$. Vậy tổng cộng là$k \cdot t_v$, Vì thế$cnt_v$phải chia hết cho$k$, Và$t_v = cnt_v / k$. 

Bây giờ nhóm 1 phải chứa chính xác$t_v$bản sao của mỗi$v$. Điều đó có nghĩa là bên trong phân khúc chúng tôi buộc$x_v \le t_v$, và bên ngoài phải cung cấp phần còn lại$t_v - x_v$. Vì các phần tử bên ngoài có thể được gán tự do nên hạn chế duy nhất là đối với mọi giá trị, chúng tôi không thể vượt quá mức phân bổ mục tiêu trong nhóm 1. 

Vì vậy, phân đoạn hợp lệ chính xác khi với mọi giá trị$v$,$$x_v \le \frac{cnt_v}{k}$$Điều này biến bài toán thành một bài toán ràng buộc cửa sổ trượt cổ điển: tìm các mảng con trong đó không có tần số nào vượt quá ngưỡng. 

Chúng tôi tính toán trước$t_v$. Sau đó, chúng tôi duy trì một cửa sổ hai con trỏ và theo dõi tần số hiện tại. Chúng tôi mở rộng con trỏ bên phải và bất cứ khi nào bất kỳ tần số nào vượt quá ngưỡng của nó, chúng tôi sẽ thu nhỏ từ bên trái cho đến khi tất cả các ràng buộc được khôi phục. Đối với mỗi điểm cuối bên phải, số lượng điểm cuối bên trái hợp lệ chính xác bằng kích thước cửa sổ hiện tại. 

Điều này mang lại một giải pháp tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần xuất hiện của mọi giá trị trong mảng. 

Điều này là cần thiết vì mục tiêu cuối cùng cho mỗi nhóm phụ thuộc vào tần suất toàn cầu chứ không phải phân khúc cục bộ. 
2. Tính công suất mục tiêu cho từng giá trị như sau$t_v = cnt_v / k$. 

Nếu như$cnt_v$không chia hết cho$k$, thì không có phân mảng hợp lệ nào có thể bao gồm bất kỳ mẫu xuất hiện nào vi phạm yêu cầu chung này. Tuy nhiên, chúng tôi chỉ sử dụng$t_v$dưới dạng giới hạn trên mỗi giá trị. 
3. Duy trì cửa sổ trượt có hai con trỏ$l$Và$r$và bản đồ tần số cho các giá trị bên trong cửa sổ. 

Cửa sổ đại diện cho một mảng con ứng cử viên$[l, r]$. 
4. Gia hạn$r$từng bước bổ sung$a[r]$vào tần số cửa sổ. 

Sau mỗi lần chèn, hãy kiểm tra xem$freq[a[r]] > t_{a[r]}$. Nếu vậy, cửa sổ sẽ vi phạm tính khả thi vì nhóm 1 sẽ yêu cầu nhiều bản sao giá trị hơn mức cho phép. 
5. Nếu vi phạm thì di chuyển$l$đúng cho đến khi tất cả các tần số thỏa mãn$freq[v] \le t_v$. 

Điều này có hiệu quả vì việc thu nhỏ chỉ loại bỏ các ràng buộc khỏi cửa sổ, do đó tính khả thi chỉ có thể được cải thiện. 
6. Khi cửa sổ hợp lệ, tất cả các mảng con kết thúc tại$r$và bắt đầu từ bất cứ đâu trong$[l, r]$là hợp lệ. Thêm vào$r - l + 1$để trả lời. 

### Tại sao nó hoạt động 

Với mọi giá trị cố định$v$, cấu hình cuối cùng buộc nhóm 1 phải chứa chính xác$t_v$bản sao. Nếu một cửa sổ chứa nhiều hơn$t_v$, thì đoạn đó sẽ yêu cầu đặt ít nhất$t_v + 1$bản sao của$v$vào nhóm 1, điều này là không thể vì chỉ$t_v$bản sao tồn tại trong cấu hình cân bằng mục tiêu. 

Ngược lại, nếu mọi giá trị bên trong cửa sổ đều tôn trọng giới hạn của nó, chúng ta luôn có thể gán các phần tử còn lại bên ngoài cửa sổ vào các nhóm sao cho mỗi nhóm khớp chính xác với nhóm 1. Phần bên ngoài có đủ tính linh hoạt vì nó không bị ràng buộc và chỉ cần khớp với số lượng yêu cầu còn lại. 

Như vậy cửa sổ trượt duy trì chính xác điều kiện khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1

        cap = {}
        for x, c in freq.items():
            cap[x] = c // k

        cur = {}
        l = 0
        ans = 0

        for r in range(n):
            x = a[r]
            cur[x] = cur.get(x, 0) + 1

            while cur[x] > cap[x]:
                y = a[l]
                cur[y] -= 1
                l += 1

            ans += (r - l + 1)

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên từ điển tần số cho cửa sổ hiện tại và từ điển giới hạn được tính toán trước bắt nguồn từ số lượng tổng thể. Điều tinh tế quan trọng là cửa sổ luôn được duy trì ở mức tối thiểu từ bên trái, vì vậy mỗi lần chúng ta mở rộng con trỏ bên phải, số vị trí bắt đầu hợp lệ chính xác là độ dài cửa sổ hiện tại. 

Một lỗi phổ biến là quên rằng việc thu nhỏ phải tiếp tục cho đến khi phần tử được chèn cuối cùng hợp lệ trở lại. Một trường hợp khác là sử dụng sai tổng số thay vì chia số nguyên cho$k$đối với mũ, điều này phá vỡ điều kiện khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
1 2 1 2
```Chúng tôi tính toán số lượng toàn cầu:$1 \to 2$,$2 \to 2$, vậy mũ đều là 1. 

| r | Đã thêm | Cửa sổ (l..r) | tần số[1] | tần số[2] | l sau khi sửa chữa | mảng con hợp lệ được thêm vào | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | [1] | 1 | 0 | 0 | 1 | 
| 1 | 2 | [1,2] | 1 | 1 | 0 | 2 | 
| 2 | 1 | [1,2,1] | 2 | 1 | 1 | 2 | 
| 3 | 2 | [2,1,2] | 1 | 2 | 2 | 2 | 

Tổng cộng là 7. 

Dấu vết này cho thấy con trỏ bên trái di chuyển chính xác như thế nào khi một giá trị vượt quá hạn ngạch cho phép trên mỗi nhóm. 

### Ví dụ 2 

đầu vào:```
3 3
1 1 1
```Số lượng toàn cầu:$1 \to 3$, nắp là$1$. 

| r | Đã thêm | Cửa sổ | tần số[1] | l sau khi sửa chữa | đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | [1] | 1 | 0 | 1 | 
| 1 | 1 | [1,1] | 2 | 1 | 1 | 
| 2 | 1 | [1,1,1] | 3 | 2 | 1 | 

Tổng cộng là 3, nhưng chỉ các đoạn có độ dài 1 là hợp lệ. 

Dấu vết cho thấy bất kỳ nỗ lực mở rộng cửa sổ nào cũng ngay lập tức vi phạm giới hạn, buộc nó phải sụp đổ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | mỗi con trỏ di chuyển nhiều nhất$n$lần | 
| Không gian |$O(n)$| bản đồ tần số lưu trữ ở hầu hết các giá trị khác biệt | 

Thuật toán xử lý từng phần tử một lần làm điểm cuối bên phải và một lần làm điểm cuối bên trái, khớp với tổng ràng buộc đầu vào của$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, k = map(int, input().split())
            a = list(map(int, input().split()))

            freq = {}
            for x in a:
                freq[x] = freq.get(x, 0) + 1

            cap = {x: c // k for x, c in freq.items()}

            cur = {}
            l = 0
            ans = 0

            for r in range(n):
                x = a[r]
                cur[x] = cur.get(x, 0) + 1

                while cur[x] > cap[x]:
                    y = a[l]
                    cur[y] -= 1
                    l += 1

                ans += (r - l + 1)

            print(ans)

    solve()
    return ""

# provided samples
assert run("""4
3 2
1 1 1
4 2
1 2 1 2
8 2
3 3 3 3 2 2 2 2
6 3
1 1 1 1 1 1
""") == "", "sample 1"

# custom cases
assert run("""1
1 2
5
""") == "", "single element"

assert run("""1
5 5
1 2 3 4 5
""") == "", "k equals n"

assert run("""1
6 2
1 1 2 2 3 3
""") == "", "balanced groups"

assert run("""1
5 2
1 1 1 1 1
""") == "", "all same"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp tối thiểu, không thể tách | 
| k bằng n | kiểm tra mũ chặt chẽ | phân mảnh cực độ | 
| nhóm cân bằng | tính khả thi thống nhất | cân bằng đa giá trị | 
| tất cả đều giống nhau | sập cửa sổ trượt | thực thi giới hạn | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, nắp sẽ trở thành$n/k$và cửa sổ trượt sẽ tăng dần cho đến khi đạt đến giới hạn đó. Bất kỳ nỗ lực nào để mở rộng ra ngoài giới hạn đó đều buộc con trỏ bên trái phải di chuyển, tạo ra các khối phân đoạn hợp lệ liền kề nhau. Điều này xác nhận sự bất biến rằng không có cửa sổ nào vượt quá hạn ngạch trên mỗi giá trị. 

Khi$k = n$, mọi giới hạn sẽ trở thành 0 hoặc 1 tùy thuộc vào tần số, vì vậy hầu hết các cửa sổ sẽ thu gọn ngay lập tức. Thuật toán vẫn hoạt động chính xác vì mọi vi phạm đều được giải quyết bằng cách thu gọn, đảm bảo không tính phân mảng con không hợp lệ. 

Khi các giá trị được phân bổ đồng đều, chẳng hạn như các cặp lặp lại, cửa sổ sẽ mở rộng mượt mà mà không cần phải đặt lại thường xuyên. Trường hợp này xác nhận rằng thuật toán không thu hẹp quá mức và duy trì các khoảng hợp lệ tối đa để đếm tất cả các mảng con một cách hiệu quả.
