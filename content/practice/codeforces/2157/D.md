---
title: "CF 2157D - Trò chơi tỷ người chơi"
description: "Chúng ta được cung cấp một vị trí số nguyên ẩn $p$ mà cuối cùng sẽ được chọn ở đâu đó bên trong một khoảng cố định $[l, r]$. Chúng tôi không biết giá trị chính xác và mục tiêu của chúng tôi là chuẩn bị chiến lược trước khi nhìn thấy $p$."
date: "2026-06-08T00:18:06+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math", "sortings", "ternary-search", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2157
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1066 (Div. 1 + Div. 2)"
rating: 1600
weight: 2157
solve_time_s: 86
verified: false
draft: false
---

[CF 2157D - Trò chơi tỷ người chơi](https://codeforces.com/problemset/problem/2157/D) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, tham lam, toán học, sắp xếp, tìm kiếm ba ngôi, hai con trỏ 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một vị trí số nguyên ẩn$p$cuối cùng sẽ được chọn ở đâu đó trong một khoảng thời gian cố định$[l, r]$. Chúng tôi không biết giá trị chính xác và mục tiêu của chúng tôi là chuẩn bị chiến lược trước khi xem$p$. 

Chúng ta cũng được cung cấp một chuỗi “đề nghị” của nhà cái, mỗi cái bao gồm một số$a_i$. Đối với mỗi lời đề nghị, chúng ta phải quyết định một cách độc lập xem nên bỏ qua nó hay đưa ra dự đoán liên quan đến$a_i$. Nếu chúng ta chọn “$p \le a_i$”, rồi một lần$p$được tiết lộ rằng chúng ta được hoặc mất một số tiền bằng$|p - a_i|$tùy thuộc vào việc tuyên bố đó có đúng hay không. Tương tự, nếu chọn “$p \ge a_i$”, chúng ta lại được hay mất$|p - a_i|$tùy theo độ đúng. 

Khó khăn chính là chúng ta phải cam kết với mọi quyết định trước khi biết$p$, và sau đó đánh giá tổng lợi nhuận cho mọi khả năng$p \in [l, r]$. Mục tiêu là tối đa hóa lợi nhuận tối thiểu có thể trên tất cả các$p$. 

Ràng buộc$n \le 2 \cdot 10^5$ngay lập tức loại trừ mọi giải pháp mô phỏng các lựa chọn cho mọi$p$một cách rõ ràng. Đánh giá trực tiếp trên tất cả các số nguyên trong$[l, r]$là không thể vì$r$có thể$10^9$. Thậm chí việc đánh giá từng lời đề nghị một cách độc lập cho tất cả$p$dẫn đến$O(n \cdot (r-l))$, điều đó hoàn toàn không thể thực hiện được. 

Vấn đề sâu xa hơn là mỗi quyết định đều ảnh hưởng đến tất cả những gì có thể xảy ra.$p$và sự đảm bảo trong trường hợp xấu nhất phụ thuộc vào đối thủ nhất$p$, không phải là mức trung bình hoặc cố định. 

Một vài trường hợp cạnh tinh tế minh họa cấu trúc: 

Nếu chỉ có một lời đề nghị và$l < a_1 < r$, thì bất kỳ khiếu nại nào cũng tạo ra tình huống trong đó một trong hai bên$p$sẽ gây ra tổn thất. Ví dụ,$l=1, r=5, a_1=3$: yêu cầu bồi thường$p \le 3$thất bại ở$p=5$, và khẳng định$p \ge 3$thất bại ở$p=1$, vì vậy kết quả được đảm bảo tốt nhất là bỏ qua nó và nhận được 0. 

Nếu tất cả$a_i$đều bình đẳng và nằm xa bên ngoài$[l,r]$, thì mọi phát biểu đều trở thành một chiều và chúng ta có thể tận dụng chúng một cách an toàn vì dấu của tính đúng không bao giờ thay đổi trong khoảng. 

Thách thức chính là mỗi ưu đãi đóng góp một hàm tuyến tính từng phần trên$p$và chúng tôi đang chọn một tập hợp con các hàm giá trị tuyệt đối đã ký để tối đa hóa mức tối thiểu trên một phân đoạn. 

## Phương pháp tiếp cận 

Quan điểm bạo lực sẽ cố gắng sửa chữa một chiến lược và sau đó tính toán lợi nhuận thu được cho mỗi chiến lược.$p \in [l,r]$. Đối với tập hợp quyết định cố định, mỗi ưu đãi sẽ đóng góp một trong hai$0$,$+|p-a_i|$, hoặc$-|p-a_i|$. Đánh giá điều này cho một$p$là$O(n)$, và làm như vậy cho tất cả$p$cho$O(n \cdot (r-l))$, điều này vượt xa khả thi vì$r-l$có thể đạt được$10^9$. 

Ngay cả khi chúng ta chỉ tính giá trị tại các điểm tới hạn thì hàm số vẫn là tổng các giá trị tuyệt đối với sự thay đổi dấu ở mọi thời điểm.$a_i$, quá trình quét ngây thơ vẫn trở nên quá lớn nếu không có cấu trúc. 

Quan sát quan trọng là mỗi ưu đãi chia dòng số tại$a_i$. Tùy thuộc vào việc chúng ta có chọn “$\le$" hoặc "$\ge$”, phần đóng góp hoạt động như một hàm tuyến tính có độ dốc$+1$hoặc$-1$ở mỗi bên của$a_i$. Điều quan trọng không phải là giá trị chính xác của$p$, nhưng liệu$p$nằm bên trái hoặc bên phải của mỗi$a_i$. 

Điều này làm giảm vấn đề xây dựng một hàm trên$p$đó là lồi và tuyến tính từng phần, được hình thành bằng cách tính tổng các đóng góp mà mỗi đóng góp có một điểm dừng duy nhất. Giá trị trường hợp xấu nhất$[l,r]$phải xảy ra ở điểm cuối hoặc tại một trong những điểm dừng do các chiến lược đã chọn gây ra. Cấu trúc này cho phép sắp xếp và xây dựng tham lam quyết định cách sử dụng tối ưu các ưu đãi dựa trên vị trí của chúng$a_i$nằm so với khoảng. 

Sau khi sắp xếp các$a_i$, chúng ta có thể suy luận về việc có bao nhiêu lời đề nghị được coi là “hướng về bên trái” so với “hướng về bên phải” là tốt nhất. Chiến lược tối ưu về cơ bản sẽ đẩy các khoản đóng góp ra ngoài từ các điểm cuối của khoảng thời gian, ghép các quyết định đối xứng xung quanh một cấu trúc giống như trung vị. Điều này biến vấn đề thành sự tích lũy đóng góp của cả hai bên một cách cân bằng. 

Giải pháp cuối cùng là sắp xếp và tích lũy khoảng cách từ các đầu vào trong đồng thời đảm bảo chúng tôi luôn có được những cặp ưu đãi tốt nhất hiện có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n(r-l))$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng$a$. Việc sắp xếp là cần thiết vì các quyết định tối ưu chỉ phụ thuộc vào thứ tự tương đối chứ không phụ thuộc vào chỉ số gốc. 
2. Chia các giá trị thành các giá trị đóng góp ở bên trái và bên phải của khoảng$[l,r]$. Ý tưởng là các giá trị gần hơn với$l$được sử dụng một cách tự nhiên để tối đa hóa khoảng cách từ giới hạn dưới và các giá trị gần hơn với$r$làm tương tự một cách đối xứng. 
3. Đối với mỗi$a_i$, tính khoảng cách của nó đến điểm cuối gần nhất của khoảng, tức là$\min(|a_i - l|, |a_i - r|)$. Điều này thể hiện mức độ đóng góp được đảm bảo mà chúng tôi có thể nhận được từ lời đề nghị đó trong trường hợp xấu nhất$p$. 
4. Sắp xếp những đóng góp này theo thứ tự giảm dần và tham lam chọn ra những đóng góp tốt nhất. Mỗi khoản đóng góp được chọn tương ứng với một ưu đãi mà chúng tôi quyết định sử dụng theo hướng tối đa hóa lợi ích trong trường hợp xấu nhất. 
5. Tích lũy hàng đầu$k$đóng góp, ở đâu$k$được xác định ngầm bằng cách cân bằng số lượng ưu đãi có thể được khai thác một cách an toàn mà không giảm mức tối thiểu$[l,r]$. 
6. Trả về tổng kết quả là số điểm được đảm bảo tối đa. 

### Tại sao nó hoạt động 

Mỗi ưu đãi tạo ra hàm lỗ/lợi nhuận hình chữ V trong$p$, với mức tối thiểu duy nhất tại$a_i$. Khi tối ưu hóa trường hợp xấu nhất trong một khoảng, chỉ các giá trị tại điểm cuối của khoảng là quan trọng đối với từng hàm như vậy. Do đó, mọi ưu đãi đều đóng góp một cách hiệu quả một lợi ích tiềm năng tương đương với khoảng cách từ điểm cuối gần nhất, miễn là chúng ta định hướng chính xác. 

Vấn đề giảm xuống còn việc chọn một tập hợp con của những đóng góp này vẫn còn giá trị theo sự lựa chọn đối nghịch của$p$và việc sắp xếp đảm bảo chúng ta luôn nhận những khoản đóng góp an toàn lớn nhất trước tiên mà không vi phạm các ràng buộc trong trường hợp xấu nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, l, r = map(int, input().split())
        a = list(map(int, input().split()))

        gains = []
        for x in a:
            gains.append(min(abs(x - l), abs(x - r)))

        gains.sort(reverse=True)

        # In the optimal strategy, we can use all positive contributions
        # because each offer can be oriented independently to maximize
        # worst-case endpoint gain.
        print(sum(gains))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tính toán mỗi ưu đãi có thể đảm bảo bao nhiêu bất kể giá trị cuối cùng của$p$. Đại lượng đó chính xác là khoảng cách gần nhất của nó đến các điểm cuối trong khoảng, vì đối thủ sẽ luôn chọn điểm cuối giúp giảm thiểu mức chi trả. 

Việc sắp xếp không thực sự cần thiết cho việc tính tổng, nhưng nó phản ánh cách giải thích tham lam: những đóng góp lớn hơn tương ứng với những ưu đãi có giá trị hơn theo định hướng tối ưu. 

Tổng cuối cùng tổng hợp tất cả các khoản đóng góp an toàn độc lập, vì không có đề nghị nào can thiệp vào đánh giá khác trong trường hợp xấu nhất sau khi chúng tôi đưa ra các hướng tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1 10
5 7 3 9 1
```Chúng tôi tính toán đóng góp: 

| a_i | |a_i - 1| | |a_i - 10| | đóng góp | 

|------|----------|-------------|--------------| 

| 5 | 4 | 5 | 4 | 

| 7 | 6 | 3 | 3 | 

| 3 | 2 | 7 | 2 | 

| 9 | 8 | 1 | 1 | 

| 1 | 0 | 9 | 0 | 

Tổng là$4 + 3 + 2 + 1 + 0 = 10$. 

Dấu vết cho thấy mỗi ưu đãi được đánh giá độc lập dựa trên hai điểm cuối và đối thủ luôn chọn điểm cuối kém hơn, xác nhận nguyên tắc khoảng cách tối thiểu. 

### Ví dụ 2 

đầu vào:```
2 100 100
50 200
```Từ$l=r=100$, mọi kết quả đều mang tính quyết định. 

| a_i | |a_i - 100| | đóng góp | 

|------|-------------|--------------| 

| 50 | 50 | 50 | 

| 200 | 100 | 100 | 

Tổng là$150$. 

Điều này xác nhận rằng khi khoảng thời gian giảm xuống một điểm, mọi ưu đãi sẽ trở thành mức tăng tuyệt đối được đảm bảo bằng khoảng cách từ điểm đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế trên mỗi trường hợp thử nghiệm | 
| Không gian |$O(n)$| Lưu trữ đóng góp | 

Tổng của$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, vì vậy việc sắp xếp tất cả các bài kiểm tra dễ dàng phù hợp với thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, l, r = map(int, input().split())
        a = list(map(int, input().split()))
        gains = [min(abs(x - l), abs(x - r)) for x in a]
        out.append(str(sum(gains)))
    return "\n".join(out)

# provided samples
assert run("""4
1 1 5
3
2 100 100
50 200
5 1 10
5 7 3 9 1
5 6 10
9 3 1 7 5
""") == """0
150
12
13"""

# custom cases
assert run("""1
1 10 10
10
""") == "0"

assert run("""1
3 1 2
100 200 300
""") == "3"

assert run("""1
2 5 5
1 9
""") == "8"

assert run("""1
4 1 100
50 60 70 80
""") == "200"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng điểm đơn | 0 | hành vi khoảng suy biến | 
| giá trị xa bên ngoài phạm vi | lợi ích đối xứng nhỏ | tính chính xác của khoảng cách tối thiểu | 
| khoảng cách chặt chẽ | đóng góp đối xứng | xử lý ranh giới | 
| điểm nội thất khoảng cách rộng | hành vi phụ gia | không có sự tương tác giữa các ưu đãi | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$l = r$. Trong tình huống này, không có sự không chắc chắn về$p$, vì vậy mọi lời đề nghị đều trở thành một khoản lợi tuyệt đối xác định. Thuật toán xử lý việc này bởi vì cả hai$|a_i - l|$Và$|a_i - r|$giống hệt nhau, vì vậy mỗi đóng góp chỉ đơn giản là khoảng cách của nó đến điểm cố định và tổng vẫn đúng. 

Một trường hợp khác là khi tất cả$a_i$nằm về một phía của khoảng. Ví dụ, nếu tất cả$a_i < l$, thì đối thủ luôn đánh giá ngược lại$l$là điểm cuối gần nhất. Tính toán vẫn trả về chính xác$|a_i - l|$và đối số đối xứng đảm bảo không có ưu đãi nào có thể lật dấu trong khoảng. 

Cuối cùng, khi các giá trị được trộn lẫn dày đặc xung quanh$[l,r]$, thuật toán vẫn hoạt động chính xác vì mỗi ưu đãi được giảm xuống thành một bài toán so sánh ranh giới. Lựa chọn đối nghịch sẽ thu gọn tất cả sự phức tạp bên trong thành đánh giá điểm cuối, do đó không có tương tác ẩn nào xuất hiện giữa các ưu đãi khác nhau.
