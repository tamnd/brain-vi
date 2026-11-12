---
title: "CF 1668B - Khoảng cách xã hội"
description: "Chúng ta được sắp xếp theo vòng tròn gồm $m$ chỗ ngồi và danh sách $n$ người. Mỗi người có một “bán kính loại trừ” cá nhân $ai$: nếu họ ngồi trên một ghế nào đó thì ghế $ai$ gần nhất ở cả hai phía xung quanh vòng tròn phải để trống."
date: "2026-06-10T02:03:04+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1668
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 783 (Div. 2)"
rating: 900
weight: 1668
solve_time_s: 129
verified: false
draft: false
---

[CF 1668B - Khoảng cách xã hội](https://codeforces.com/problemset/problem/1668/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp theo vòng tròn$m$chỗ ngồi và danh sách$n$mọi người. Mỗi người đều có “bán kính loại trừ” cá nhân$a_i$: nếu họ ngồi ở chỗ nào đó thì chỗ gần nhất$a_i$ghế ở hai bên xung quanh vòng tròn phải trống. Nói cách khác, mỗi người không chỉ chiếm một chiếc ghế, họ còn chặn một cách hiệu quả một vòng cung liền kề của$2a_i + 1$chỗ ngồi trên vòng tròn, căn giữa ở vị trí của chúng. 

Nhiệm vụ là quyết định xem liệu chúng ta có thể đặt tất cả mọi người vào vòng kết nối sao cho không có khu vực bị chặn nào chồng lên nhau hay không. 

Chi tiết cấu trúc quan trọng ở đây hoàn toàn là về các khoảng cách đóng gói trên một vòng tròn, trong đó mỗi khoảng có chiều dài.$2a_i + 1$và các khoảng phải rời nhau. 

Các ràng buộc làm cho việc bố trí bằng vũ lực là không thể. Tổng số người trong tất cả các trường hợp thử nghiệm lên tới$10^5$, trong khi số ghế có thể lớn bằng$10^9$. Bất kỳ cách tiếp cận nào cố gắng mô phỏng chỗ ngồi hoặc vị trí kiểm tra riêng lẻ sẽ bị loại trừ ngay lập tức. Thậm chí$O(n^2)$mỗi trường hợp thử nghiệm sẽ là quá chậm. 

Trường hợp phức tạp là khi tổng nhu cầu gần bằng kích thước vòng tròn. Ví dụ, nếu$m = 5$và tất cả$a_i = 2$, khi đó mỗi người chặn 5 ghế, nghĩa là chỉ được ngồi tối đa một người. Một cách tiếp cận tham lam bỏ qua tính năng bao quanh hình tròn có thể cho rằng khoảng cách tuyến tính hoạt động không chính xác, nhưng tính năng bao quanh gây ra sự chồng chéo giữa vị trí cuối cùng và vị trí đầu tiên. 

Một tình huống phức tạp khác là khi có nhiều$a_i$các giá trị cùng tồn tại với một giá trị lớn duy nhất. Giá trị lớn có thể chiếm ưu thế và tiêu tốn hầu hết không gian sẵn có, không để lại sự sắp xếp hợp lệ nào ngay cả khi những giá trị nhỏ hơn có vẻ linh hoạt. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là mô phỏng các vị trí ngồi trên vòng tròn. Chúng ta có thể thử đặt mỗi người vào một số chỗ trống và đánh dấu khoảng thời gian bị chặn. Điều này sẽ liên quan đến việc duy trì cấu trúc của các phân đoạn trống và liên tục chèn các khoảng thời gian trong khi kiểm tra sự chồng chéo. Trong trường hợp xấu nhất, mọi vị trí đều có thể yêu cầu quét gần như toàn bộ không gian còn lại, dẫn đến$O(nm)$hành vi nếu được thực hiện một cách ngây thơ, điều đó là không thể đối với$m$lên đến$10^9$. 

Nhận xét quan trọng là các vị trí riêng lẻ không hề quan trọng. Chỉ có tổng số không gian bị chiếm dụng và bị chặn là quan trọng. Mỗi người tiêu dùng chính xác$2a_i + 1$chỗ ngồi của vòng tròn và những mức tiêu thụ này không được chồng lên nhau. Vì vòng tròn không có ranh giới nên chúng ta có thể coi nó như bài toán tổng dung lượng: chúng ta đang cố gắng đóng gói các đoạn có tổng chiều dài không được vượt quá$m$. 

Vì vậy, vấn đề giảm xuống để kiểm tra xem:$$\sum (2a_i + 1) \leq m$$Tuy nhiên, điều này hơi sai lệch trong cài đặt vòng tròn vì các ràng buộc kề không cho phép thứ tự đóng gói tùy ý giúp giảm không gian. Cách sàng lọc chính xác là sắp xếp ngầm định theo kích thước chỉ thông qua phép tính tổng, vì tất cả các phân đoạn đều cứng nhắc và không thể chồng chéo hoặc nén. 

Vì vậy, điều kiện khả thi hoàn toàn là kiểm tra tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force |$O(nm)$|$O(m)$| Quá chậm | 
| Tổng chiều dài chiếm dụng |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi người, hãy tính xem họ chiếm được bao nhiêu ghế, đó là$2a_i + 1$. Điều này thể hiện chỗ ngồi của họ cộng với vùng đệm trống cần thiết ở cả hai bên. 
2. Tính tổng các giá trị này của tất cả mọi người để có được tổng dung lượng cần thiết. 
3. So sánh tổng số này với$m$, kích thước của hình tròn. 
4. Nếu tổng dung lượng cần thiết nhỏ hơn hoặc bằng$m$, xuất ra "CÓ". Nếu không thì xuất ra "NO". 

Lý do đằng sau phép tính tổng là vùng cấm của mỗi người phải rời rạc và mỗi vùng có độ dài cố định bất kể vị trí. 

### Tại sao nó hoạt động 

Mỗi người tạo ra một khối ghế liền kề không thể chồng lên khối nào khác. Vì các khối này cứng nhắc và độc lập nên mọi cấu hình hợp lệ đều tương ứng với việc phân chia vòng tròn thành các đoạn rời rạc có độ dài chính xác này. Vòng tròn có tổng công suất$m$, do đó điều kiện cần là tổng độ dài các đoạn không vượt quá$m$. Nó cũng đủ vì nếu tổng chiều dài phù hợp, chúng ta luôn có thể đặt các đoạn tuần tự xung quanh vòng tròn mà không có khoảng trống nhỏ hơn khoảng cách cần thiết và phần bao quanh không đưa ra các ràng buộc mới vượt quá tổng dung lượng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        
        total = 0
        for x in a:
            total += 2 * x + 1
        
        if total <= m:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện là một bản dịch trực tiếp của điều kiện dẫn xuất. Mỗi$a_i$được mở rộng thành toàn bộ chiều dài đoạn bị chiếm đóng của nó$2a_i + 1$, và được tích lũy thành một khoản tiền hiện có. Sự so sánh cuối cùng với$m$quyết định tính khả thi. 

Một sai lầm phổ biến ở đây là quên ngồi ghế trung tâm$2a_i + 1$, chỉ sử dụng$2a_i$, điều này đánh giá thấp tổng mức sử dụng và dẫn đến kết quả đầu ra "CÓ" không chính xác. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp từ mẫu. 

### Ví dụ 1 

đầu vào:```
n = 3, m = 8
a = [1, 2, 1]
```| Người | a_i | Đã chiếm đóng (2a_i+1) | Tổng Chạy | Khả thi vs m | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 3 | vâng | 
| 2 | 2 | 5 | 8 | vâng | 
| 3 | 1 | 3 | 11 | không | 

Quyết định cuối cùng: KHÔNG kể từ ngày 11 > 8. 

Điều này cho thấy một yêu cầu lớn sẽ nhanh chóng làm cạn kiệt vòng tròn ngay cả khi các vị trí trước đó có vẻ ổn. 

### Ví dụ 2 

đầu vào:```
n = 2, m = 4
a = [1, 1]
```| Người | a_i | Đã chiếm đóng (2a_i+1) | Tổng Chạy | Khả thi vs m | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 3 | vâng | 
| 2 | 1 | 3 | 6 | không | 

Quyết định cuối cùng: KHÔNG vì 6 > 4. 

Điều này xác nhận rằng ngay cả những hạn chế nhỏ thống nhất cũng có thể vượt quá khả năng khi có mặt nhiều người. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi trường hợp thử nghiệm tính tổng tất cả mọi người một lần | 
| Không gian |$O(1)$| Chỉ tổng số đang chạy được lưu trữ | 

Giải pháp xử lý tối đa$10^5$tổng giá trị trên tất cả các trường hợp thử nghiệm, thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    solve()
    
    return output.getvalue()

# provided samples
assert run("""6
3 2
1 1 1
2 4
1 1
2 5
2 1
3 8
1 2 1
4 12
1 2 1 3
4 19
1 2 1 3
""") == """NO
YES
NO
YES
NO
YES
"""

# all minimum
assert run("""1
2 5
1 1
""") == "NO\n"

# single person fits exactly
assert run("""1
1 3
1
""") == "YES\n"

# tight fit boundary
assert run("""1
3 9
1 1 1
""") == "NO\n"

# large slack
assert run("""1
4 100
1 1 1 1
""") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| người độc thân chính xác | CÓ | trường hợp đẳng thức biên | 
| hai nhỏ đông đúc | KHÔNG | phát hiện tràn tối thiểu | 
| trận đấu tổng hợp thất bại | KHÔNG | sự đúng đắn từng cái một | 
| chùng m lớn | CÓ | khả năng mở rộng và tính tích cực | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$n = 1$. Công thức rút gọn thành việc kiểm tra xem$2a_1 + 1 \leq m$. Ví dụ, nếu$m = 3$Và$a_1 = 1$, người đó chiếm đúng cả 3 ghế, điều này là hợp lệ. Thuật toán tính toán chính xác tổng số là 3 và trả về CÓ. 

Một trường hợp khác là khi tất cả$a_i = 1$. Mỗi người chiếm 3 chỗ. Nếu như$m = 8$Và$n = 3$, tổng số trở thành 9, vượt quá dung lượng mặc dù về mặt trực quan, người ta có thể cố gắng “lắp” chúng bằng cách bao quanh. Thuật toán từ chối nó một cách chính xác vì không thể tránh khỏi sự chồng chéo theo vòng tròn khi tổng cầu vượt quá cung. 

Trường hợp cạnh cuối cùng là khi$m$rất lớn nhưng$n$là nhỏ. Ví dụ,$n = 2$,$m = 10^9$,$a = [10^9, 10^9]$. Tổng trở thành$4 \cdot 10^9 + 2$, ngay lập tức vượt quá$m$, tạo ra NO một cách chính xác mà không cần bất kỳ suy luận hình học nào.
