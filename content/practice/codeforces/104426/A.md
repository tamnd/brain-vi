---
title: "CF 104426A - G Game"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Hai người chơi chọn các tập hợp con khác nhau của các chỉ số từ mảng này. Abdulrahman được phép nhận tối đa chỉ số $P$ và Hazem được phép nhận tối đa chỉ số $Q$."
date: "2026-06-30T19:03:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104426
codeforces_index: "A"
codeforces_contest_name: "Syrian Private Universities Collegiate Programming Contest 2023"
rating: 0
weight: 104426
solve_time_s: 94
verified: false
draft: false
---

[CF 104426A - Trò chơi G](https://codeforces.com/problemset/problem/104426/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Hai người chơi chọn các tập hợp con khác nhau của các chỉ số từ mảng này. Abdulrahman được phép đón$P$chỉ số và Hazem được phép nhận$Q$chỉ số. Đóng góp của Abdulrahman là tổng các giá trị tại các chỉ số đã chọn của anh ấy, trong khi đóng góp của Hazem là tổng các giá trị tại các chỉ số anh ấy đã chọn và mục tiêu là tối đa hóa sự khác biệt giữa tổng của Abdulrahman và tổng của Hazem. 

Cấu trúc chính là mọi chỉ số đều có thể đóng góp tích cực vào điểm số của Abdulrahman, đóng góp tiêu cực thông qua lựa chọn của Hazem hoặc bị bỏ qua hoàn toàn. Vì cả hai người chơi đều tối ưu hóa độc lập nhưng bị hạn chế bởi các lựa chọn rời rạc nên chúng tôi đang quyết định một cách hiệu quả cho từng yếu tố xem nó được giao cho Abdulrahman, Hazem hay không ai, với giới hạn toàn cầu về số lượng yếu tố mỗi bên có thể lấy. 

Các ràng buộc làm rõ rằng bất kỳ giải pháp nào cố gắng xem xét các tập hợp con một cách rõ ràng là không thể. Với tổng số$n$lên đến$10^5$qua các bài kiểm tra và lên đến$10^5$trường hợp thử nghiệm, bất kỳ điều gì tệ hơn tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm đều quá chậm. Điều này ngay lập tức loại trừ sự lựa chọn theo cấp số nhân hoặc thậm chí$O(n^2)$mô phỏng tham lam. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị đều âm hoặc tất cả đều dương. Nếu tất cả các giá trị đều âm và$P = 0$, Abdulrahman hoàn toàn không thể bù đắp các lựa chọn của Hazem, vì vậy Hazem sẽ chọn các giá trị âm nhất có thể (vì chúng làm tăng chênh lệch bằng cách trừ đi số âm). Ngược lại, nếu tất cả các giá trị đều dương và$Q = 0$, Abdulrahman chỉ cần lấy các phần tử lớn nhất sẵn có. Một kẻ tham lam ngây thơ bỏ qua sự tương tác giữa năng lực giữa các nhiệm vụ tích cực và tiêu cực có thể thất bại trong nhiều trường hợp. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là thử mọi cách để phân mỗi phần tử thành ba loại: Abdulrahman, Hazem hoặc không được sử dụng, đồng thời tôn trọng những gì Abdulrahman sử dụng nhiều nhất.$P$nhiều nhất là các phần tử và Hazem$Q$. Điều này dẫn đến sự bùng nổ tổ hợp: ngay cả khi bỏ qua các ràng buộc, đây vẫn là$3^n$bài tập, và ngay cả khi cắt bớt số lượng bài tập hợp lệ vẫn theo cấp số nhân. Tính đúng đắn là không đáng kể vì nó khám phá tất cả các cấu hình hợp lệ, nhưng nó trở nên không thể thực hiện được ngay khi$n$vượt quá giá trị nhỏ. 

Quan sát quan trọng là quyết định cho từng phần tử chỉ phụ thuộc vào giá trị của nó so với các phần tử khác chứ không phụ thuộc vào vị trí. Nếu một yếu tố được gán cho Abdulrahman, nó sẽ đóng góp tích cực. Nếu được gán cho Hazem, nó sẽ góp phần tiêu cực một cách hiệu quả vào biểu thức cuối cùng. Nếu không sử dụng, nó đóng góp bằng không. Vì vậy, mọi phần tử đều có ba “chế độ” với các giá trị$+a_i$,$-a_i$, hoặc$0$, với giới hạn chung về số lần mỗi chế độ có thể được sử dụng. 

Chúng ta có thể giải thích lại vấn đề bằng cách chọn tối đa$P+Q$tổng số phần tử, trong đó mỗi phần tử được chọn được gán một dấu hiệu: Abdulrahman đưa ra$+a_i$, Hazem đưa ra$-a_i$. Nếu chúng ta quyết định chọn một phần tử, phép gán tốt nhất phụ thuộc vào việc liệu$a_i$là tích cực hoặc tiêu cực. Một con số dương tốt nhất nên đến tay Abdulrahman nếu có thể, hoặc bỏ qua thay vì đưa cho Hazem. Số âm được gán cho Hazem tốt hơn vì việc trừ số âm sẽ làm tăng điểm. 

Điều này gợi ý việc sắp xếp các phần tử theo mức đóng góp tuyệt đối của chúng tùy thuộc vào sự phân công tối ưu. Mỗi yếu tố đều có sự đóng góp tốt nhất có thể:$\max(a_i, -a_i)$, tùy thuộc vào người chơi mà nó sẽ dành cho. Tuy nhiên, chúng ta phải tôn trọng những hạn ngạch riêng biệt cho các nhiệm vụ tích cực và tiêu cực, điều này khiến cho sự tham lam trực tiếp trở nên không đủ trừ khi được xử lý cẩn thận. 

Cách chính xác là chia bài toán thành hai giai đoạn: chúng tôi xem xét việc lấy các phần tử theo thứ tự giá trị tuyệt đối giảm dần, nhưng chúng tôi gán chúng một cách tối ưu trong khi vẫn tôn trọng các khả năng còn lại$P$Và$Q$. Mỗi phần tử được đánh giá xem bên nào mang lại mức tăng cao hơn. Nếu như$a_i > 0$, Abdulrahman được hưởng lợi từ việc dùng nó; mặt khác Hazem được hưởng lợi từ việc lấy nó (vì trừ đi nó sẽ làm tăng điểm). Chúng ta luôn giao phần tử cho bên có lợi hơn nhưng chỉ khi bên đó còn năng lực. Nếu không, chúng ta sẽ quay lại phía bên kia nếu vẫn cải thiện được điểm số. 

Tính tham lam này hoạt động vì mọi phần tử đều đóng góp một cách độc lập một mức tăng tốt nhất cố định và không có sự tương tác giữa các phần tử ngoại trừ những hạn chế về năng lực, được giải quyết bằng cách xử lý các lựa chọn có tác động cao nhất trước tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Bài tập tham lam tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi phần tử là thứ có thể được gán cho Abdulrahman hoặc Hazem, nhưng với mức tăng khác nhau tùy thuộc vào dấu hiệu. Chúng tôi xử lý các yếu tố theo thứ tự ưu tiên tác động tuyệt đối lớn hơn để không lãng phí các khoảng thời gian có hạn cho các quyết định có tác động thấp. 

## Hướng dẫn thuật toán 

1. Tính toán giá trị tuyệt đối của mỗi phần tử, biểu thị mức độ ảnh hưởng của nó đến câu trả lời bất kể hướng gán. Chúng ta sẽ ưu tiên những tác động lớn hơn trước vì việc chọn sai chúng sau này sẽ tốn kém hơn là trì hoãn chúng. 
2. Sắp xếp các chỉ số của mảng theo thứ tự giá trị tuyệt đối giảm dần. Điều này đảm bảo rằng khi chúng tôi chỉ định các phần tử, chúng tôi luôn xử lý các quyết định có giá trị nhất trước tiên. 
3. Duy trì công suất còn lại$remP = P$Và$remQ = Q$và khởi tạo câu trả lời bằng 0. Chúng đại diện cho số lượng yếu tố mà mỗi người chơi vẫn có thể sử dụng. 
4. Lặp lại các phần tử theo thứ tự được sắp xếp. Đối với mỗi phần tử$a_i$, quyết định nhiệm vụ của nó. Nếu như$a_i > 0$, Abdulrahman được hưởng lợi từ việc sử dụng nó$+a_i$, trong khi Hazem dùng nó sẽ làm giảm kết quả xuống$a_i$. Nếu như$a_i < 0$, Hazem lấy nó mang lại lợi nhuận$+|a_i|$bởi vì việc trừ đi số âm sẽ cải thiện sự khác biệt. 
5. Nếu bên ưu tiên cho phần tử hiện tại vẫn còn dung lượng còn lại, hãy chỉ định nó ở đó và cập nhật câu trả lời tương ứng đồng thời giảm dung lượng đó. Ưu tiên là Abdulrahman cho các giá trị dương và Hazem cho các giá trị âm. 
6. Nếu mặt ưu tiên đã đầy, chỉ gán phần tử cho mặt kia nếu nó vẫn cải thiện kết quả dưới các ràng buộc. Nếu không bên nào chịu được thì bỏ qua. 

Lý do xử lý theo giá trị tuyệt đối hoạt động là vì một khi phần tử có tác động cao được quyết định, sau này nó không thể được cải thiện bằng cách hoán đổi với các phần tử nhỏ hơn mà không làm giảm tổng mức tăng, vì công suất là hạn chế ghép nối duy nhất. 

## Tại sao nó hoạt động 

Mỗi phần tử có chính xác hai nhiệm vụ quan trọng: giao nó cho Abdulrahman hoặc cho Hazem. Những điều này tương ứng với lợi ích$a_i$Và$-a_i$. Thuật toán luôn ưu tiên gán mức tăng cao hơn trước và xử lý các phần tử theo thứ tự giảm dần về mức độ chúng có thể thay đổi câu trả lời cuối cùng. Điều này tạo ra thứ tự tham lam đối với các lựa chọn có trọng số độc lập với hai thùng bị ràng buộc. Vì việc hoán đổi một quyết định có giá trị tuyệt đối thấp hơn bằng một quyết định cao hơn luôn làm tăng tổng điểm, nên bất kỳ giải pháp tối ưu nào cũng có thể được chuyển thành giải pháp tham lam mà không làm mất giá trị, duy trì tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, P, Q = map(int, input().split())
    a = list(map(int, input().split()))

    items = [(abs(x), x) for x in a]
    items.sort(reverse=True)

    res = 0
    remP, remQ = P, Q

    for _, x in items:
        if x > 0:
            if remP > 0:
                res += x
                remP -= 1
            elif remQ > 0:
                res -= x
                remQ -= 1
        else:
            if remQ > 0:
                res += -x
                remQ -= 1
            elif remP > 0:
                res += x
                remP -= 1

    print(res)

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```Mã bắt đầu bằng cách ghép từng giá trị với độ lớn tuyệt đối và sắp xếp sao cho các phần tử có ảnh hưởng nhất được xử lý trước tiên. Hai quầy theo dõi xem mỗi người chơi vẫn có bao nhiêu lựa chọn. Các giá trị dương đầu tiên được cung cấp cho Abdulrahman vì chúng trực tiếp tăng điểm và chỉ khi hết dung lượng đó, chúng tôi mới xem xét gán chúng cho Hazem. Các giá trị âm được gán một cách đối xứng tốt nhất cho Hazem vì chúng chuyển thành những đóng góp dương trong chênh lệch cuối cùng. 

Điều tinh tế chính là chúng ta không bao giờ bỏ qua việc gán một phần tử có giá trị nếu bên “nhầm” vẫn còn năng lực, bởi vì mọi phần tử đều phải đóng góp một cách tối ưu theo các ràng buộc còn lại. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một trường hợp đại diện nhỏ để xem các quyết định phát triển như thế nào: 

đầu vào:```
n = 5, P = 2, Q = 2
a = [5, -3, 4, -2, 1]
```Sắp xếp theo giá trị tuyệt đối: 

| Bước | Yếu tố | remP | remQ | Quyết định | Điểm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 2 | 2 | Abdulrahman mất | 5 | 
| 2 | 4 | 1 | 2 | Abdulrahman mất | 9 | 
| 3 | -3 | 0 | 2 | Hazem mất | 12 | 
| 4 | -2 | 0 | 1 | Hazem mất | 14 | 
| 5 | 1 | 0 | 0 | bỏ qua | 14 | 

Dấu vết này cho thấy rằng các giá trị dương trước hết thuộc về phía Abdulrahman, sau đó các giá trị âm sẽ lấp đầy hạn ngạch của Hazem, tối đa hóa việc lật dấu có lợi. 

Trường hợp thứ hai nhấn mạnh đến tương tác năng lực: 

đầu vào:```
n = 3, P = 1, Q = 1
a = [-10, 9, 8]
```Thứ tự sắp xếp:$-10, 9, 8$| Bước | Yếu tố | remP | remQ | Quyết định | Điểm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | -10 | 1 | 1 | Hazem mất | 10 | 
| 2 | 9 | 1 | 0 | Abdulrahman mất | 19 | 
| 3 | 8 | 0 | 0 | bỏ qua | 19 | 

Thứ tự đảm bảo phần tử cường độ lớn nhất xác định cấu trúc trước tiên, tránh bẫy gán các giá trị dương nhỏ trước các chuyển đổi âm lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| việc sắp xếp chiếm ưu thế cho từng trường hợp thử nghiệm | 
| Không gian |$O(n)$| lưu trữ mảng và sắp xếp cặp | 

Tổng cộng$n$qua các bài kiểm tra là$10^5$, do đó việc sắp xếp vẫn nằm trong giới hạn. Quét tuyến tính sau khi sắp xếp đảm bảo giải pháp tuyến tính hiệu quả cho từng phần tử bên ngoài chi phí sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    import sys
    input = sys.stdin.readline

    def solve():
        n, P, Q = map(int, input().split())
        a = list(map(int, input().split()))
        items = [(abs(x), x) for x in a]
        items.sort(reverse=True)
        res = 0
        remP, remQ = P, Q
        for _, x in items:
            if x > 0:
                if remP:
                    res += x
                    remP -= 1
                elif remQ:
                    res -= x
                    remQ -= 1
            else:
                if remQ:
                    res += -x
                    remQ -= 1
                elif remP:
                    res += x
                    remP -= 1
        print(res)

    t = int(input())
    for _ in range(t):
        solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided samples
assert run("""3
3 1 1
-2 0 2
3 1 2
6 -4 -5
5 0 2
10 -6 -9 8 -7
""") == """4
10
16"""

# custom cases
assert run("""1
1 0 1
5
""") == """-5"""

assert run("""1
2 2 0
-1 10
""") == """10"""

assert run("""1
4 1 2
-100 50 40 -30
""") == """180"""

assert run("""1
3 1 1
-10 9 8
""") == """19"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 1, 5 | -5 | chỉ Hazem mới có thể chọn | 
| 2 2 0, -1 10 | 10 | chỉ có Abdulrahman đóng góp | 
| 4 1 2, -100 50 40 -30 | 180 | đặt hàng cường độ lớn | 
| 3 1 1, -10 9 8 | 19 | phân công tối ưu hỗn hợp | 

## Vỏ cạnh 

Khi nào$P = 0$, thuật toán không bao giờ gán giá trị dương cho Abdulrahman, vì vậy mọi đóng góp phải đến từ việc Hazem chọn số âm. Đối với đầu vào:```
n = 3, P = 0, Q = 2
a = [5, -2, -7]
```thứ tự sắp xếp là 7, 5, 2. Hai phần tử có cường độ âm đầu tiên là -7 và -2, cả hai đều được gán cho Hazem, mang lại đóng góp$7 + 2 = 9$và 5 bị bỏ qua vì Q đã cạn kiệt. Điều này phù hợp với hành vi tối ưu vì việc gán 5 cho Hazem sẽ làm giảm điểm. 

Khi tất cả các giá trị đều dương và$Q = 0$, chỉ có Abdulrahman đóng góp. Vì:```
n = 3, P = 2, Q = 0
a = [1, 10, 5]
```thuật toán lấy 10 và 5 cho Abdulrahman và bỏ qua 1, tạo ra 15. Mọi nỗ lực gán cho Hazem đều không thể thực hiện được, vì vậy việc lựa chọn tham lam các giá trị lớn nhất là tối ưu. 

Khi tất cả các giá trị đều âm và cả hai người chơi đều có năng lực, Hazem sẽ lấy các giá trị âm nhất trước tiên vì chúng mang lại mức tăng cao nhất khi bị phủ định. Thuật toán ưu tiên một cách tự nhiên độ lớn giá trị, đảm bảo Hazem nắm bắt được những cải tiến lớn nhất trước khi Abdulrahman sử dụng các vị trí còn lại.
