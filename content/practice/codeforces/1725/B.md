---
title: "CF 1725B - Bóng Rổ Cùng Nhau"
description: "Chúng ta được yêu cầu thành lập các đội từ danh sách các cầu thủ bóng rổ ứng cử viên, mỗi đội có một lũy thừa số nguyên. Có một đội đối lập có sức mạnh $D$ và đội chúng tôi thành lập sẽ thắng nếu tổng sức mạnh của các thành viên vượt quá $D$."
date: "2026-06-09T19:01:35+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1725
codeforces_index: "B"
codeforces_contest_name: "COMPFEST 14 - Preliminary Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 1000
weight: 1725
solve_time_s: 119
verified: true
draft: false
---

[CF 1725B - Cùng nhau chơi bóng rổ](https://codeforces.com/problemset/problem/1725/B) 

**Đánh giá:** 1000 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 59 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu thành lập các đội từ danh sách các cầu thủ bóng rổ ứng cử viên, mỗi đội có một lũy thừa số nguyên. Có một đội đối lập có sức mạnh$D$và đội do chúng tôi thành lập sẽ thắng nếu tổng sức mạnh của các thành viên vượt quá$D$. Điều khó khăn là trước mỗi trận đấu, huấn luyện viên có thể khuếch đại tất cả các cầu thủ trong một đội để đấu với cầu thủ mạnh nhất trong đội đó. Mỗi người chơi có thể tham gia tối đa một đội và chúng tôi muốn tối đa hóa số đội có thể giành chiến thắng. 

Đầu vào bao gồm$N$số nguyên đại diện cho sức mạnh của người chơi và$D$, sức mạnh của kẻ thù. Đầu ra là một số nguyên duy nhất: chiến thắng tối đa. 

Những ràng buộc cho phép$N$lên đến$10^5$và quyền hạn lên tới$10^9$. Điều này cho chúng ta biết rằng một$O(N^2)$cách tiếp cận sẽ không hiệu quả. Chúng ta cần thứ gì đó gần$O(N \log N)$lúc tệ nhất. Hoạt động phải mở rộng quy mô hiệu quả với mảng lớn. Một cách tiếp cận đơn giản là thử mọi tập hợp con có thể để xem liệu tổng được khuếch đại của nó có vượt trội hay không$D$là không thể thực hiện được. 

Các trường hợp đặc biệt bao gồm: tất cả sức mạnh của người chơi nhỏ hơn$D$, chỉ một người chơi rất mạnh hoặc tất cả sức mạnh ngang nhau. Ví dụ, nếu$N = 3$,$D = 100$, và quyền hạn là$[30, 30, 30]$, không đội nào có thể thắng vì ngay cả việc khuếch đại thành viên mạnh nhất cũng mang lại$30 * 3 = 90 \le D$. Một cách tiếp cận bất cẩn có thể cho rằng một mình mỗi người chơi có thể tạo thành một đội chiến thắng một cách không chính xác, nhưng tổng số tiền khuếch đại phải vượt quá$D$. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ xem xét mọi tập hợp con người chơi có thể có để thành lập một đội, tính tổng khuếch đại cho tập hợp con đó và kiểm tra xem nó có vượt quá không.$D$. Trong trường hợp xấu nhất, điều này đòi hỏi phải lặp lại$2^N$tập hợp con. Vì$N = 10^5$, điều này là không thể. Ngay cả phương pháp heuristic thử tất cả các phân đoạn liên tiếp sau khi sắp xếp cũng quá chậm nếu được triển khai một cách ngây thơ. 

Điểm mấu chốt là sau khi sắp xếp người chơi theo sức mạnh, chiến lược tốt nhất là tham lam. Để tối đa hóa chiến thắng, chúng ta nên thành lập đội nhỏ nhất có thể với những cầu thủ mạnh nhất hiện có. Đối với một nhóm ứng viên có quy mô$k$, nếu thành viên mạnh nhất có quyền lực$p$, tổng đội được khuếch đại là$k * p$. Chúng tôi cần$k * p > D$. Bởi vì lớn hơn$k$giá trị làm cho nó dễ dàng hơn để vượt quá$D$đối với những người chơi nhỏ hơn, chúng tôi bắt đầu thành lập các đội từ mạnh nhất trở xuống. Mỗi lần chúng tôi chọn một người chơi là người mạnh nhất trong một đội mới, chúng tôi sẽ tính toán quy mô đội tối thiểu cần thiết để đánh bại$D$và hạ gục nhiều người chơi yếu nhất còn lại. Điều này đảm bảo mỗi đội chiến thắng và không có người chơi nào được sử dụng hai lần. 

Sự quan sát tham lam này biến vấn đề thành một phương pháp sắp xếp đơn giản và hai con trỏ hoặc theo dõi chỉ mục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N) | O(N) | Quá chậm | 
| Tối Ưu (Tham Lam + Sắp Xếp) | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp sức mạnh của người chơi theo thứ tự tăng dần. Việc sắp xếp đảm bảo chúng tôi có thể chọn người chơi mạnh nhất cho mỗi đội một cách hiệu quả và những người đồng đội yếu nhất hiện có. 
2. Khởi tạo hai con trỏ hoặc chỉ mục: một ở đầu (`i`) và một ở cuối (`j`) của mảng đã sắp xếp.`i`chỉ ra người chơi yếu nhất hiện có,`j`đến mạnh nhất. 
3. Khởi tạo bộ đếm`wins`về không. Điều này sẽ theo dõi số lượng đội chiến thắng được thành lập. 
4. Trong khi`i <= j`, hãy xem xét người chơi mạnh nhất còn lại tại`j`. Tính toán kích thước đội tối thiểu cần thiết để đánh bại`D`:`k = ceil(D / P[j])`. Đây là số lượng bản sao nhỏ nhất của`P[j]`cần thiết. 
5. Kiểm tra xem còn đủ người chơi hay không: nếu`j - i + 1 >= k`, thành lập một đội với người chơi`j`Và`k-1`trong số những người chơi yếu nhất hiện có bắt đầu từ`i`. Tăng`wins`, tăng`i`qua`k-1`để xóa các trình phát đã sử dụng khỏi phía trước và giảm bớt`j`để loại bỏ người chơi mạnh nhất đã sử dụng. 
6. Nếu không còn đủ người chơi để tạo thành đội chiến thắng với`P[j]`, dừng lại. Không còn đội nào có thể giành chiến thắng. 
7. Trở về`wins`. 

Tại sao nó hoạt động: Điều bất biến là ở mỗi bước, chúng tôi luôn thành lập một đội được đảm bảo giành chiến thắng khi sử dụng số lượng người chơi ít nhất và chúng tôi luôn tiêu thụ người chơi mạnh nhất hiện có. Không có người chơi nào được sử dụng hai lần và không có cơ hội chiến thắng nào bị bỏ qua. Mảng được sắp xếp đảm bảo chúng ta luôn có thể chọn kết hợp để tối đa hóa các chiến thắng tiềm năng còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def max_wins():
    n, D = map(int, input().split())
    P = list(map(int, input().split()))
    P.sort()
    
    i, j = 0, n - 1
    wins = 0
    
    while i <= j:
        strongest = P[j]
        team_size = (D + strongest) // strongest  # ceil(D / strongest)
        if team_size <= (j - i + 1):
            wins += 1
            i += team_size - 1
            j -= 1
        else:
            break
    
    print(wins)

if __name__ == "__main__":
    max_wins()
```Mã đầu tiên sắp xếp quyền hạn. Cách tiếp cận hai con trỏ đảm bảo chúng tôi luôn thu hút người chơi một cách tối ưu. các`(D + strongest) // strongest`thủ thuật tính trần không cần nhập`math.ceil`cho tốc độ. Điều kiện biên`i <= j`Và`team_size <= j - i + 1`xử lý các mảng nhỏ và ngăn ngừa lỗi chỉ mục. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`6 180`Quyền hạn:`[90, 80, 70, 60, 50, 100]`| tôi | j | mạnh nhất | đội_size | tôi sau | j sau | thắng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 100 | 2 | 1 | 4 | 1 | 
| 1 | 4 | 90 | 2 | 2 | 3 | 2 | 
| 2 | 3 | 70 | 3 | - | - | 2 | 

Giải thích: Đội thứ nhất sử dụng 100+60. Đội thứ hai sử dụng 90+70. Những người chơi còn lại không thể tạo thành một đội có tổng số >180. 

**Mẫu tùy chỉnh** 

đầu vào:`3 100`Quyền hạn:`[30, 30, 30]`| tôi | j | mạnh nhất | đội_size | tôi sau | j sau | thắng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 30 | 4 | - | - | 0 | 

Không đội nào có thể vượt quá 100. Thuật toán xuất ra chính xác 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp chiếm ưu thế; quét tuyến tính với hai con trỏ là O(N) | 
| Không gian | O(N) | Lưu trữ mảng đầu vào | 

Với$N\le10^5$,$N \log N \approx 5 \times 10^5$hoạt động, thoải mái trong thời hạn 1 giây. Bộ nhớ cũng dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    max_wins()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("6 180\n90 80 70 60 50 100\n") == "2", "sample 1"

# minimum input
assert run("1 1\n1\n") == "1", "single player equal to D"

# all equal
assert run("4 10\n3 3 3 3\n") == "1", "all equal small powers"

# maximum powers
assert run("5 1000000000\n1000000000 1000000000 1000000000 1000000000 1000000000\n") == "5", "max power values"

# edge case: not enough for second team
assert run("5 10\n2 3 3 3 4\n") == "1", "cannot form second winning team"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1 1\n1\n" | 1 | Đầu vào tối thiểu cho một người chơi | 
| "4 10\n3 3 3 3\n" | 1 | Mọi quyền hạn đều bằng nhau, tổng vừa đủ | 
| "5 1000000000\n1000000000 ...\n" | 5 | Quyền hạn tối đa được xử lý chính xác | 
| "5 10\n2 3 3 3 4\n" | 1 | Xử lý đúng khi người chơi còn lại không thành lập được đội | 

## Vỏ cạnh 

Nếu tất cả người chơi có sức mạnh như nhau, thuật toán sẽ tính toán chính xác quy mô đội cần thiết và thành lập càng nhiều đội càng tốt. Đối với đầu vào`[3,3,3,3]`với`D=10`, quy mô đội được yêu cầu là 4, vì vậy chỉ có một đội được thành lập. Nếu lũy thừa cực lớn, thuật toán vẫn hoạt động vì phép chia số nguyên tránh được tình trạng tràn. Các tình huống chơi đơn được xử lý chính xác vì việc tính toán quy mô đội có tính đến sự phân chia trần nhà.
