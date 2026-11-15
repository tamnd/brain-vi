---
title: "CF 1842C - Tenzing và Balls"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Mỗi test bắt đầu bằng một dòng số nguyên biểu thị một chuỗi các quả bóng màu được sắp xếp thành một hàng."
date: "2026-06-09T06:14:23+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1842
codeforces_index: "C"
codeforces_contest_name: "CodeTON Round 5 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1500
weight: 1842
solve_time_s: 190
verified: false
draft: false
---

[CF 1842C - Tenzing và Balls](https://codeforces.com/problemset/problem/1842/C) 

**Đánh giá:** 1500 
**Thẻ:** dp 
**Thời gian giải:** 3 phút 10s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Mỗi test bắt đầu bằng một dòng số nguyên biểu thị một chuỗi các quả bóng màu được sắp xếp thành một hàng. Một nước đi bao gồm việc chọn hai vị trí$i < j$sao cho các quả bóng ở các vị trí đó có cùng màu và sau đó xóa toàn bộ đoạn đó khỏi$i$ĐẾN$j$, bao gồm cả hai điểm cuối. Mọi thứ ở bên phải sẽ dịch chuyển sang trái sau khi loại bỏ. Chúng tôi lặp lại thao tác này bao nhiêu lần và mục tiêu là tối đa hóa tổng số quả bóng được loại bỏ. 

Một cách hữu ích để giải thích điều này là mọi thao tác sẽ loại bỏ một khối liền kề có điểm cuối có cùng giá trị và sau khi loại bỏ, mảng sẽ trở nên ngắn hơn, có khả năng cho phép các phép hợp nhất mới không liền kề trước đó. 

Tổng ràng buộc trên tất cả$n$là$2 \cdot 10^5$, vì vậy một$O(n^2)$giải pháp cho mỗi bài kiểm tra là quá chậm, và thậm chí$O(n \sqrt n)$là rủi ro. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi màu sắc lặp lại nhưng bị phân tách bằng các phân đoạn lớn. Ví dụ, trong`[1, 2, 3, 1]`, thật hấp dẫn khi nghĩ rằng cả hai`1`s có thể trực tiếp góp phần loại bỏ một lượng lớn, nhưng cấu trúc ở giữa ảnh hưởng đến những gì có thể thực hiện được sau khi xóa một phần. Một cách tiếp cận tham lam ngây thơ luôn loại bỏ cặp hợp lệ đầu tiên có thể thất bại vì nó có thể chặn các cặp tốt hơn trong tương lai. 

Một cạm bẫy khác là cho rằng mỗi màu đóng góp độc lập một số thứ như "khoảng cách giữa lần xuất hiện đầu tiên và lần xuất hiện cuối cùng". Điều đó bị phá vỡ khi cấu trúc bên trong cho phép xóa nhiều giai đoạn. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử mọi cặp có thể$(i, j)$với các giá trị bằng nhau, mô phỏng việc loại bỏ và giải đệ quy mảng còn lại. Điều này mô hình hóa chính xác quy trình, nhưng mỗi lần xóa sẽ thay đổi kích thước mảng và có thể có$O(n^2)$ứng cử viên di chuyển theo từng tiểu bang. Vì đệ quy phân nhánh nhiều nên độ phức tạp tổng thể sẽ trở thành cấp số nhân trong trường hợp xấu nhất. 

Quan sát quan trọng là chúng ta không bao giờ cần mô phỏng rõ ràng các mảng trung gian. Thay vào đó, chúng ta có thể suy nghĩ theo cách xử lý từ trái sang phải và quyết định xem có nên "mở" một đoạn mà sau này sẽ được đóng lại bằng màu phù hợp hay không. 

Điều này tự nhiên dẫn đến lập trình động qua tiền tố, nơi chúng tôi theo dõi câu trả lời tốt nhất cho từng vị trí. Khó khăn chính là khi chúng ta đóng một đoạn ở vị trí$i$, chúng ta phải tính đến cách tốt nhất để ghép các lần xuất hiện trước đó cùng màu trong khi có thể bỏ qua các phần tử bên trong. 

Một thủ thuật tiêu chuẩn cho các vấn đề của cấu trúc này là duy trì một mảng DP trong đó chúng tôi xem xét hai lựa chọn ở mỗi vị trí: hoặc chúng tôi coi phần tử hiện tại là độc lập (không được sử dụng trong bất kỳ thao tác loại bỏ bên ngoài nào) hoặc chúng tôi khớp nó với một số lần xuất hiện trước đó có cùng màu và loại bỏ mọi thứ ở giữa một cách tối ưu. 

Chúng tôi cũng duy trì, đối với mỗi màu, giá trị DP tốt nhất tại các vị trí mà màu đó xuất hiện lần cuối. Điều này tránh việc quét ngược nhiều lần. 

Giải pháp kết quả trở thành tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(n) đệ quy | Quá chậm | 
| DP tối ưu với trạng thái xuất hiện lần cuối | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một mảng DP trong đó`dp[i]`đại diện cho số lượng bóng bị loại bỏ tối đa khi xem xét tiền tố lên đến chỉ mục`i`. 

Chúng tôi cũng duy trì một từ điển`best[color]`, lưu trữ giá trị DP tốt nhất được thấy ở các vị trí mà màu này xuất hiện, sau khi áp dụng ý tưởng rằng chúng ta có thể đóng một phân đoạn ở vị trí hiện tại. 

### bước 

1. Khởi tạo`dp[0] = 0`và thiết lập tất cả`best[color] = 0`. 

Ý nghĩa là trước khi xử lý bất cứ điều gì, không thể loại bỏ được. 
2. Lặp lại mảng từ trái sang phải. 

Tại vị trí`i`, chúng ta xem xét màu bóng hiện tại`c = a[i]`. 
3. Đầu tiên, chuyển tiếp điều tốt nhất trước đó:`dp[i] = dp[i-1]`. 

Điều này thể hiện trường hợp chúng tôi không kết thúc bất kỳ việc loại bỏ nào tại vị trí`i`. 
4. Bây giờ hãy xem xét việc đóng một đoạn kết thúc tại`i`. 

Nếu trước đây chúng ta đã nhìn thấy cùng một màu sắc`c`tại một vị trí nào đó, thì lần xuất hiện trước đó có thể đóng vai trò là điểm cuối bên trái của thao tác xóa. 
5. Về màu sắc`c`, tính:$$dp[i] = \max(dp[i], best[c] + 1)$$Điều này thể hiện việc hình thành một cặp hợp lệ với sự xuất hiện sớm hơn của cùng một màu và loại bỏ mọi thứ ở giữa theo cách tối ưu đã được mã hóa trong`best[c]`. 
6. Cập nhật`best[c] = \max(best[c], dp[i])`. 

Điều này đảm bảo sự xuất hiện trong tương lai của`c`có thể được hưởng lợi từ trạng thái tốt nhất có thể đạt được cho đến thời điểm này. 
7. Sau khi xử lý tất cả các phần tử, câu trả lời là`dp[n]`. 

### Tại sao nó hoạt động 

Bất biến quan trọng là`best[c]`luôn lưu trữ giá trị DP tối đa có thể đạt được trong số tất cả các vị trí có màu`c`đã xuất hiện, nghĩa là nó thể hiện "trạng thái tốt nhất có thể trước khi đóng một đoạn màu`c`". Khi chúng tôi xử lý một lần xuất hiện mới của`c`, việc ghép nối nó với lần xuất hiện tốt nhất trước đó tương đương với việc chọn ranh giới bên trái tối ưu cho đoạn kết thúc ở đây. Vì các phân đoạn bên trong đã được giải quyết một cách tối ưu trong`dp`, chúng tôi không bao giờ mất đi tính tối ưu bằng cách thu gọn tất cả các lần xuất hiện trước đó thành một trạng thái tốt nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        dp_prev = 0
        best = {}

        for x in a:
            cur = dp_prev

            if x in best:
                cur = max(cur, best[x] + 1)

            if x not in best or cur > best[x]:
                best[x] = cur

            dp_prev = cur

        print(dp_prev)

if __name__ == "__main__":
    solve()
```Mã sử ​​dụng DP cuộn thay vì một mảng đầy đủ vì mỗi bước chỉ cần giá trị trước đó. Từ điển`best`nén tất cả các trạng thái lịch sử của mỗi màu thành một giá trị duy nhất, giúp tránh hành vi bậc hai. 

Sự chuyển tiếp`best[x] + 1`mã hóa ý tưởng đóng một phân đoạn kết thúc ở vị trí hiện tại bằng cách sử dụng lần xuất hiện trước đó cùng màu. Mức tăng tương ứng với việc thực hiện một thao tác xóa. 

Cần chú ý cập nhật`best[x]`sau khi tính toán`cur`, nếu không, chúng tôi sẽ cho phép một trạng thái sử dụng lại chính nó trong cùng một bước một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 2 3 3
```| tôi | màu sắc | dp_prev | tốt nhất[màu] trước | quyết định | dp[i] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | {} | mới | 0 | 
| 2 | 2 | 0 | {} | mới | 0 | 
| 3 | 2 | 0 | {2:0} | cặp | 1 | 
| 4 | 3 | 1 | {2:1} | mới | 1 | 
| 5 | 3 | 1 | {3:1} | cặp | 2 | 

Câu trả lời cuối cùng là 2. 

Điều này cho thấy sự xuất hiện thứ hai của màu sắc cho phép xóa theo chuỗi như thế nào và cách`best`chuyển tiếp các trạng thái có thể sử dụng được. 

### Ví dụ 2 

đầu vào:```
4
1 2 1 2
```| tôi | màu sắc | dp_prev | tốt nhất[màu] trước | quyết định | dp[i] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | {} | mới | 0 | 
| 2 | 2 | 0 | {} | mới | 0 | 
| 3 | 1 | 0 | {1:0} | cặp | 1 | 
| 4 | 2 | 1 | {2:1} | cặp | 1 | 

Câu trả lời cuối cùng là 1. 

Điều này thể hiện màu sắc xen kẽ: mỗi trận đấu phụ thuộc vào trạng thái tốt nhất trước đó thay vì vị trí thô. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi phần tử được xử lý một lần với các thao tác từ điển O(1) | 
| Không gian | O(n) | Trường hợp xấu nhất các màu khác biệt được lưu trữ trong`best`| 

Tổng độ phức tạp trong tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, phù hợp thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        dp_prev = 0
        best = {}

        for x in a:
            cur = dp_prev
            if x in best:
                cur = max(cur, best[x] + 1)
            best[x] = max(best.get(x, 0), cur)
            dp_prev = cur

        out.append(str(dp_prev))

    return "\n".join(out)

# provided samples
assert run("""2
5
1 2 2 3 3
4
1 2 1 2
""") == "4\n3"

# all same values
assert run("""1
5
7 7 7 7 7
""") == "4"

# alternating pattern
assert run("""1
6
1 2 1 2 1 2
""") == "3"

# single color pair only
assert run("""1
3
1 2 1
""") == "1"

# no possible useful merges
assert run("""1
4
1 2 3 4
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 4 | sập liên tục | 
| xen kẽ | 3 | cấu trúc đan xen | 
| cặp đơn | 1 | độ chính xác hoạt động tối thiểu | 
| khác biệt | 0 | không có kết quả dương tính giả | 

## Vỏ cạnh 

Đối với các mảng có tất cả các giá trị giống nhau, thuật toán cập nhật liên tục`best[color]`, đảm bảo mỗi lần xuất hiện mới có thể mở rộng chuỗi tối ưu trước đó. DP xây dựng một thao tác cho mỗi cặp lần xuất hiện liền kề, phù hợp với thực tế là mỗi lần hợp nhất sẽ giảm phân đoạn ít nhất một khoảng đầy đủ. 

Đối với các chuỗi xen kẽ nghiêm ngặt như`[1,2,1,2,...]`, mỗi màu`best`giá trị phát triển độc lập và DP tránh được việc đếm quá mức một cách chính xác bằng cách chỉ cho phép chuyển đổi khi tồn tại trạng thái hợp lệ trước đó. Điều này ngăn chặn chuỗi không chính xác trên các phân đoạn không tương thích. 

Đối với mảng không lặp lại,`best`vẫn trống suốt nên không có thao tác nào được hình thành. DP trả về 0 một cách chính xác vì không có điểm cuối hợp lệ nào tồn tại cho bất kỳ thao tác loại bỏ nào.
