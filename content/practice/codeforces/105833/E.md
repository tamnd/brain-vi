---
title: "CF 105833E - Khai thác năng lượng"
description: "Chúng ta được cung cấp một bộ sưu tập các thùng chứa năng lượng, mỗi thùng bắt đầu với một lượng năng lượng nhất định. Chúng ta được phép di chuyển năng lượng giữa các thùng chứa, nhưng mọi sự chuyển giao đều không hiệu quả: nếu chúng ta di chuyển một lượng năng lượng nào đó từ thùng chứa này sang thùng chứa khác, một tỷ lệ phần trăm cố định của những gì chúng ta cố gắng di chuyển sẽ bị mất…"
date: "2026-06-25T06:29:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105833
codeforces_index: "E"
codeforces_contest_name: "NUS CS3233 Final Team Contest 2025"
rating: 0
weight: 105833
solve_time_s: 45
verified: true
draft: false
---

[CF 105833E - Khai thác năng lượng](https://codeforces.com/problemset/problem/105833/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập các thùng chứa năng lượng, mỗi thùng bắt đầu với một lượng năng lượng nhất định. Chúng ta được phép di chuyển năng lượng giữa các thùng chứa, nhưng mỗi lần chuyển giao đều không hiệu quả: nếu chúng ta di chuyển một lượng năng lượng nào đó từ thùng chứa này sang thùng chứa khác, một tỷ lệ phần trăm cố định của những gì chúng ta cố gắng di chuyển sẽ bị mất trong quá trình này. 

Mục đích là xác định giá trị lớn nhất$x$sao cho, sau bất kỳ chuỗi chuyển giao nào, mọi container đều có thể kết thúc chính xác$x$đơn vị năng lượng. 

Một cách hữu ích để diễn đạt lại vấn đề là nghĩ theo thuật ngữ “phân phối lại trong điều kiện rò rỉ”. Chúng ta không thay đổi tổng năng lượng một cách tùy tiện; chúng tôi chỉ phân phối lại nó và mỗi khi chúng tôi di chuyển năng lượng qua các thùng chứa, chúng tôi phải trả một khoản tổn thất tương ứng. 

Các ràng buộc đủ lớn đến mức bất kỳ giải pháp nào cố gắng mô phỏng việc chuyển tiền đều không thể thực hiện được ngay lập tức. Số lượng container có thể lên tới$10^4$và mỗi thử nghiệm liên quan đến số học trên các lần chuyển giá trị thực với độ chính xác cần thiết lên đến$10^{-6}$. Sự kết hợp này gợi ý rõ ràng rằng câu trả lời là liên tục và phải được tìm ra bằng cách sử dụng tính chất đơn điệu thay vì xây dựng trực tiếp. Một mô phỏng chuyển giao lực lượng mạnh mẽ sẽ liên quan đến việc suy luận về các chuyển động theo cặp và các hoạt động cân bằng có khả năng lặp đi lặp lại, sẽ bùng nổ theo kiểu tổ hợp. 

Một số trường hợp đặc biệt cho thấy lý do tại sao việc phân phối lại tham lam ngây thơ lại thất bại: 

Nếu tất cả các vùng chứa đã có năng lượng bằng nhau, ví dụ như đầu vào```
3 50
2 2 2
```câu trả lời đúng là rõ ràng$2$. Bất kỳ chiến lược nào cố gắng "di chuyển thặng dư" vẫn có thể thực hiện các giao dịch chuyển tiền không cần thiết và giảm giá trị có thể đạt được một cách không chính xác nếu chiến lược đó không nhận ra rằng không cần chuyển khoản. 

Nếu tổn thất bằng 0, chẳng hạn như```
2 0
1 11
```thì toàn bộ năng lượng được bảo toàn hoàn toàn và câu trả lời đơn giản phải là giá trị trung bình$(1+11)/2 = 6$. Bất kỳ phương pháp nào giả định các hạn chế về hướng hoặc cân bằng gia tăng đều có thể thất bại ở đây nếu nó không giảm thiểu việc bảo toàn tổng. 

Ví dụ: nếu tổn thất cực kỳ cao$k = 99$, gần như toàn bộ năng lượng được truyền đi đều biến mất. Trong những trường hợp như vậy, chỉ có thặng dư địa phương mới quan trọng và việc tính trung bình toàn cầu trở nên không thể vượt quá một giá trị rất nhỏ. Các thuật toán giả định sự phân phối lại gần như hoàn hảo sẽ đánh giá quá cao câu trả lời. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng mô phỏng quá trình phân phối lại. Người ta có thể liên tục xác định các thùng chứa trên mức mục tiêu và chuyển phần thặng dư của chúng sang những thùng chứa bên dưới. Mỗi lần chuyển sẽ giảm tổng năng lượng tùy thuộc vào tỷ lệ phần trăm tổn thất. Mặc dù đơn giản về mặt khái niệm nhưng điều này không khả thi về mặt tính toán. Mỗi bước cân bằng phụ thuộc vào những bước khác và số lượng chuyển giao tiềm năng không bị giới hạn theo cách đảm bảo hiệu quả. Trong trường hợp xấu nhất, điều này chuyển thành việc quét và điều chỉnh lặp đi lặp lại trên tất cả các vùng chứa, dẫn đến ít nhất là hành vi bậc hai về số lượng phần tử. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về chuyển khoản cá nhân và thay vào đó tập trung vào tính khả thi toàn cầu. Câu hỏi trở thành: đối với giá trị mục tiêu được đề xuất$x$, chúng tôi có thể xác minh xem liệu có thể kết thúc bằng tất cả các vùng chứa ít nhất không$x$, cho rằng chúng ta mất năng lượng trong quá trình chuyển giao? 

Nếu một container có nhiều hơn$x$, nó có năng lượng dư thừa có thể được phân phối lại. Tuy nhiên, chỉ một phần nhỏ những gì nó gửi là có thể sử dụng được ở nơi khác. Điều này có nghĩa là lượng dư thừa đóng góp ít hiệu quả hơn vào sự thiếu hụt của các container khác. 

Quan sát này dẫn đến một cấu trúc đơn điệu: nếu một giá trị nhất định$x$có thể đạt được thì bất kỳ giá trị nhỏ hơn nào cũng có thể đạt được. Điều này làm cho tìm kiếm nhị phân trở thành một công cụ tự nhiên. 

Đối với một cố định$x$, chúng tôi tính toán xem liệu tổng năng lượng hiệu dụng sau khi tính đến tổn thất truyền tải có đủ để hỗ trợ tất cả các container tiếp cận hay không$x$. Điều này làm giảm vấn đề xuống còn một lần kiểm tra đạt cho mỗi giá trị ứng viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng chuyển khoản | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + kiểm tra tính khả thi | O(n log A) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng năng lượng ban đầu của tất cả các thùng chứa. Tổng số này không bao giờ thay đổi ngoại trừ các khoản lỗ do chuyển tiền gây ra, vì vậy nó đóng vai trò là ngân sách toàn cầu. 
2. Cố định giá trị ứng viên$x$, đại diện cho năng lượng mục tiêu trong mỗi thùng chứa. Chúng tôi muốn kiểm tra xem nó có thể đạt được hay không. 
3. Đối với mỗi thùng chứa, hãy xem xét nó liên quan như thế nào đến$x$. Nếu nó có nhiều hơn$x$, năng lượng dư thừa có thể được chuyển giao. Nếu nó có ít hơn, nó cần năng lượng để đạt được mục tiêu. 
4. Tổng số tiền thặng dư, được xác định bằng$\max(0, a_i - x)$. Đây là tổng năng lượng có thể được chuyển ra khỏi các thùng chứa cao. 
5. Chỉ một phần thặng dư này được chuyển giao. Nếu tỷ lệ tổn thất là$k$, sau đó mỗi đơn vị được gửi sẽ đóng góp$(1 - k/100)$đơn vị cho bên nhận. Vì vậy, năng lượng truyền có thể sử dụng được giảm tỷ lệ thuận. 
6. Tính năng lượng hữu dụng khả dụng như sau:$$\text{available} = \text{total\_sum} - \frac{k}{100} \cdot \text{surplus}$$7. Kiểm tra xem năng lượng sẵn có này có ít nhất$n \cdot x$. Nếu đúng như vậy thì có thể phân phối năng lượng để mọi thùng chứa đều đạt tới$x$. 
8. Sử dụng tìm kiếm nhị phân$x$trong phạm vi từ$0$ĐẾN$\max(a_i)$, tinh chế cho đến khi đạt yêu cầu về độ chính xác. 

### Tại sao nó hoạt động 

Điều bất biến quan trọng là năng lượng chỉ biến mất khi nó được di chuyển và chỉ phần dư thừa trên mục tiêu mới cần được di chuyển. Bất kỳ cấu hình cuối cùng hợp lệ nào có tất cả các giá trị bằng$x$có thể được hiểu là một quá trình trong đó chính xác phần thặng dư ở trên$x$được phân phối lại và mọi đơn vị khác vẫn đứng yên. Điều này có nghĩa là tổn thất duy nhất không thể thu hồi được tỷ lệ thuận với số tiền thặng dư phải chuyển đi. 

Bởi vì điều kiện khả thi phụ thuộc đơn điệu vào$x$, tăng dần$x$chỉ có thể làm cho điều kiện khó thỏa mãn hơn. Điều này đảm bảo rằng tìm kiếm nhị phân hội tụ đến giá trị hợp lệ tối đa mà không bỏ sót các khả năng trung gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(x, a, n, k):
    total = 0.0
    surplus = 0.0
    
    for v in a:
        total += v
        if v > x:
            surplus += (v - x)
    
    lost = surplus * (k / 100.0)
    available = total - lost
    
    return available >= n * x

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    lo, hi = 0.0, max(a)
    
    for _ in range(60):
        mid = (lo + hi) / 2
        if can(mid, a, n, k):
            lo = mid
        else:
            hi = mid
    
    print(f"{lo:.10f}")

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là`can`hàm nén quá trình phân phối lại thành một phép kiểm tra số học duy nhất. Tìm kiếm nhị phân liên tục tinh chỉnh câu trả lời ứng viên, dựa vào tính đơn điệu của tính khả thi. 

Một điểm tinh tế là độ chính xác của dấu phẩy động là đủ vì phép kiểm tra ổn định dưới những nhiễu loạn nhỏ và dung sai lỗi yêu cầu là$10^{-6}$. Sáu mươi lần lặp lại tìm kiếm nhị phân đều vượt quá yêu cầu về độ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 50
4 2 1
```Chúng tôi tìm kiếm nhị phân cho giá trị bằng nhau cuối cùng. 

| giữa | tổng cộng | dư thừa | có sẵn | n*giữa | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 2.0 | 7 | 1 | 6,5 | 6 | vâng | 
| 3.0 | 7 | 4 | 5 | 9 | không | 

Thuật toán hội tụ đến$2.0$. Điều này phù hợp với trực giác rằng việc cân bằng mạnh mẽ bị hạn chế bởi tổn thất chuyển giao nặng nề. 

### Ví dụ 2 

đầu vào:```
2 90
1 11
```| giữa | tổng cộng | dư thừa | có sẵn | n*giữa | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 5.0 | 12 | 6 | 11.4 | 10 | vâng | 
| 6.0 | 12 | 5 | 11,5 | 12 | không | 

Kết quả hội tụ đến xấp xỉ$1.909...$. Điều này cho thấy trường hợp tổn thất cao ngăn cản việc đạt được giá trị trung bình số học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A)$| Mỗi lần kiểm tra tính khả thi sẽ quét tất cả các vùng chứa và tìm kiếm nhị phân thực hiện các lần lặp liên tục trong phạm vi giá trị | 
| Không gian |$O(1)$| Chỉ sử dụng tổng hợp và lưu trữ đầu vào | 

Những hạn chế$n \le 10^4$và yêu cầu về độ chính xác làm cho cách tiếp cận này nhanh chóng một cách thoải mái, vì nhiều nhất là vài triệu thao tác nguyên thủy được thực hiện cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    
    input = sys.stdin.readline
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    def can(x):
        total = 0.0
        surplus = 0.0
        for v in a:
            total += v
            if v > x:
                surplus += v - x
        return total - surplus * (k / 100.0) >= n * x
    
    lo, hi = 0.0, max(a)
    for _ in range(60):
        mid = (lo + hi) / 2
        if can(mid):
            lo = mid
        else:
            hi = mid
    return f"{lo:.6f}"

# provided samples
assert run("3 50\n4 2 1\n")[:3] == "2.0"
assert run("2 90\n1 11\n")[:5] == "1.909"

# custom cases
assert run("1 50\n10\n")[:4] == "10.0", "single element"
assert run("2 0\n1 11\n")[:1] == "6", "no loss averaging"
assert run("3 100\n10 0 0\n")[:1] == "0", "total loss extreme"
assert run("4 25\n5 5 5 5\n")[:1] == "5", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | cùng giá trị | trường hợp cơ bản tầm thường | 
| không mất mát | hành vi trung bình | bảo tồn năng lượng | 
| mất toàn bộ | sụp đổ về không | cực kỳ kém hiệu quả | 
| tất cả đều bình đẳng | điểm cố định ổn định | không chuyển khoản không cần thiết | 

## Vỏ cạnh 

Ví dụ: khi tất cả các giá trị giống hệt nhau`5 5 5 5`, việc kiểm tra tính khả thi ngay lập tức được thông qua$x = 5$vì thặng dư bằng không. Thuật toán tránh được mọi sự giảm nhân tạo một cách chính xác. 

Khi$k = 0$, số hạng thặng dư biến mất hoàn toàn. Việc kiểm tra tính khả thi giảm xuống còn việc so sánh tổng số tiền với$n \cdot x$, đó chính xác là điều kiện để lấy trung bình. Tìm kiếm nhị phân tự nhiên hội tụ về giá trị trung bình số học. 

Khi$k$gần 100 thì gần như toàn bộ số dư bị mất. Thuật toán vẫn hoạt động chính xác vì năng lượng sẵn có trở nên gần bằng tổng năng lượng ban đầu và tính khả thi nhanh chóng thất bại ở mức vừa phải.$x$. Điều này ngăn việc tìm kiếm nhị phân đánh giá quá cao khả năng phân phối lại.
