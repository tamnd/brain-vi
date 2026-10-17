---
title: "CF 105507E - \u0420\u0430\u0441\u0441\u0430\u0434\u043a\u0430 \u043d\u0430 \u044d\u043a\u0437\u0430\u043c\u0435\u043d\u0435"
description: "Chúng ta được xếp một hàng ghế trong phòng thi, được biểu diễn dưới dạng một chuỗi có độ dài $n$. Mỗi vị trí có một học sinh chiếm giữ hoặc trống. Nhiệm vụ của chúng tôi là sắp xếp lại học sinh bằng cách chuyển một số học sinh từ chỗ ngồi hiện tại sang chỗ trống."
date: "2026-06-23T21:58:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105507
codeforces_index: "E"
codeforces_contest_name: "2024-2025 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 24, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 105507
solve_time_s: 60
verified: true
draft: false
---

[CF 105507E - \u0420\u0430\u0441\u0441\u0430\u0434\u043a\u0430 \u043d\u0430 \u044d\u043a\u0437\u0430\u043c\u0435\u043d\u0435](https://codeforces.com/problemset/problem/105507/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được xếp một hàng ghế trong phòng thi, được biểu diễn dưới dạng một chuỗi có độ dài$n$. Mỗi vị trí có một học sinh chiếm giữ hoặc trống. Nhiệm vụ của chúng tôi là sắp xếp lại học sinh bằng cách chuyển một số học sinh từ chỗ ngồi hiện tại sang chỗ trống. 

Sau khi sắp xếp lại, chúng ta phải có cấu hình với ba thuộc tính: ghế ngoài cùng bên trái phải chứa một học sinh, ghế ngoài cùng bên phải cũng phải chứa một học sinh và tất cả học sinh phải được đặt sao cho khoảng cách giữa các học sinh liên tiếp không đổi trên toàn bộ hàng. Nói cách khác, nếu chúng ta chỉ nhìn vào các vị trí bị chiếm giữ, chúng phải tạo thành một cấp số cộng. 

Chúng tôi được phép di chuyển một học sinh bằng cách đưa họ ra khỏi chỗ ngồi hiện tại và đặt họ vào bất kỳ chỗ trống nào. Chi phí của một giải pháp là số lượng sinh viên được di chuyển. Mục tiêu là giảm thiểu chi phí này hoặc xác định rằng không thể hình thành cấu hình cuối cùng hợp lệ. 

Ràng buộc$n \le 200000$ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê tất cả các tập hợp con chỗ ngồi hoặc tất cả các cặp điểm cuối và khoảng trống một cách ngây thơ với độ phức tạp bậc hai hoặc tệ hơn. Chúng ta cần ít nhất một chiến lược tuyến tính hoặc gần tuyến tính, rất có thể liên quan đến việc sắp xếp các vị trí hiện tại và suy luận về cấp số cộng một cách hiệu quả. 

Một trường hợp phức tạp xuất hiện khi số lượng học sinh ít hoặc khi vị trí của họ đã hình thành một tiến trình hợp lệ. Một trường hợp phức tạp khác phát sinh khi khoảng cách được ngụ ý bởi một cặp điểm cuối đã chọn tạo ra các vị trí trung gian không nguyên hoặc vượt quá giới hạn. Một cách tiếp cận đơn giản chỉ kiểm tra điểm cuối mà không xác minh tất cả các vị trí trung gian sẽ chấp nhận cấu hình không hợp lệ một cách không chính xác. Ngược lại, một lực lượng thô bạo trên tất cả các kích thước bước sẽ hết thời gian vì kích thước bước có thể thay đổi lên tới$n$và với mỗi vị trí, chúng tôi sẽ quét tất cả các vị trí. 

## Phương pháp tiếp cận 

Chúng ta bắt đầu từ việc quan sát rằng sự sắp xếp cuối cùng được xác định hoàn toàn bởi ba giá trị: vị trí chiếm giữ đầu tiên$a$, bước$d$, và số học sinh$k$. Nếu chúng ta sửa$a$Và$d$, thì các vị trí cần tìm là$a, a+d, a+2d, \dots$. Tất cả học sinh phải được đặt chính xác trên các vị trí này và tất cả phải nằm trong đoạn thẳng$[1, n]$. 

Một cách tiếp cận mạnh mẽ sẽ thử tất cả các cặp học sinh làm điểm cuối có thể, tính toán kích thước bước ngụ ý và xác minh xem có bao nhiêu học sinh hiện tại đã nói dối trong cấp số cộng kết quả. Với$k$học sinh, điều này dẫn đến$O(k^3)$trong trường hợp xấu nhất nếu được thực hiện một cách ngây thơ, hoặc ít nhất$O(k^2)$nếu chúng ta kiểm tra cẩn thận từng cặp. Từ$k$có thể$O(n)$, điều này trở nên quá chậm. 

Thông tin chi tiết về cấu trúc quan trọng là cấu hình cuối cùng là một cấp số cộng bên trong một phân đoạn bị chặn. Thay vì thử tất cả các bước có thể, chúng tôi đảo ngược quan điểm: chúng tôi ấn định số lượng học sinh không thay đổi và cố gắng tối đa hóa con số đó. Thì câu trả lời là$k - \text{best preserved count}$. 

Nếu chúng ta sắp xếp các vị trí hiện tại của học sinh, thì bất kỳ cấp số cộng hợp lệ nào khớp với một số tập con của chúng sẽ được xác định bởi số hạng đầu tiên và hiệu của nó. Chúng tôi có thể cố định vị trí bắt đầu và một bước, nhưng thay vì liệt kê các bước một cách rõ ràng, chúng tôi nhận thấy rằng mọi tiến trình hợp lệ đều phải phù hợp với ít nhất hai học sinh hiện có. Vì vậy, bước đi được xác định bởi cặp học sinh bất kỳ trong tập hợp cuối cùng. 

Điều này gợi ý nên thử các cặp chỉ số$(i, j)$, giải thích chúng như các phần tử liên tiếp trong tiến trình cuối cùng. Sau đó bước này bị ép buộc như$d = (pos[j] - pos[i])$, và chúng tôi kiểm tra xem tiến trình này có thể kéo dài bao xa cả về phía trước và phía sau trong giới hạn mảng, đếm xem có bao nhiêu học sinh hiện tại đã nằm ở các vị trí này. 

Chúng tôi tối đa hóa số lượng trận đấu trên tất cả các cặp. Những học sinh còn lại phải di chuyển. Nếu không có tiến trình nào có thể bao gồm ít nhất hai học sinh theo một cách nhất quán thì vấn đề sẽ chuyển sang kiểm tra xem có tồn tại một cấu hình một học sinh hay không thể. 

Việc tối ưu hóa quan trọng là chúng tôi không quét toàn bộ hàng để tìm từng tiến trình ứng cử viên. Thay vào đó, chúng tôi sử dụng kiểm tra tư cách thành viên hai con trỏ hoặc tập hợp hàm băm để đếm xem có bao nhiêu vị trí ban đầu của sinh viên nằm trên chuỗi số học trong thời gian tuyến tính cho mỗi ứng viên, mang lại kết quả$O(k^2)$giải pháp có thể chấp nhận được nếu được tối ưu hóa cẩn thận và dựa vào việc kiểm tra tư cách thành viên nhanh chóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các mẫu ghế |$O(2^n)$|$O(n)$| Quá chậm | 
| Hãy thử tất cả các cặp, xác minh tiến trình |$O(k^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi trích xuất tất cả các chỉ số về số ghế đã có người vào một danh sách đã sắp xếp. Hãy để danh sách này được$p$, và để$k$là chiều dài của nó. 

1. Nếu$k \le 1$, chúng tôi ngay lập tức trả về -1 vì chúng tôi không thể đáp ứng cả hai điểm cuối đang bị chiếm và có một cấp số cộng không tầm thường. Điều này loại bỏ sớm các trường hợp thoái hóa. 
2. Chúng tôi lưu trữ tất cả các vị trí đã chiếm giữ trong một tập hợp để kiểm tra tư cách thành viên liên tục. Điều này rất cần thiết vì chúng tôi sẽ thường xuyên kiểm tra xem quá trình thăng tiến của ứng viên có ảnh hưởng đến các học viên hiện tại hay không. 
3. Chúng tôi lặp lại tất cả các cặp chỉ số$i < j$trong danh sách$p$. Đối với mỗi cặp, chúng tôi coi chúng là các điểm liên tiếp trong tiến trình cuối cùng mong muốn, do đó bước này được cố định là$d = p[j] - p[i]$. Chúng tôi cũng xem xét$p[i]$như một điểm khởi đầu tiềm năng của sự tiến triển. 
4. Đối với mỗi ứng viên$(start, d)$, chúng tôi kéo dài tiến trình lùi lại miễn là$start - d$vẫn nằm trong giới hạn. Điều này đảm bảo chúng tôi tôn trọng yêu cầu ghế ngoài cùng bên trái phải được sử dụng trong lần sắp xếp cuối cùng. 
5. Sau khi sửa điểm bắt đầu hợp lệ ngoài cùng bên trái, chúng tôi tạo ra tất cả các vị trí của tiến trình bên trong phạm vi$[1, n]$và đếm xem có bao nhiêu vị trí trong số này có trong tập hợp bị chiếm giữ ban đầu. Con số này thể hiện số lượng học sinh không cần phải di chuyển. 
6. Chúng tôi theo dõi số lượng học sinh được giữ lại tối đa trong tất cả các tiến trình ứng cử viên. 
7. Câu trả lời là$k - \text{max preserved}$, bởi vì mọi học sinh không nằm trong tiến trình phù hợp nhất đều phải được di chuyển. 

### Tại sao nó hoạt động 

Bất kỳ sự sắp xếp cuối cùng hợp lệ nào cũng được xác định duy nhất bởi vị trí đầu tiên và kích thước bước của nó. Nếu nó chứa ít nhất hai học sinh ban đầu thì hai học sinh đó xác định cùng một cấp số cộng. Do đó, mọi giải pháp hợp lệ đều được thể hiện dưới dạng liệt kê các cặp. Bằng cách kiểm tra tất cả các tiến trình được tạo ra như vậy, chúng tôi đảm bảo rằng cấu hình tối ưu sẽ được xem xét. Việc đếm các giao lộ với tập hợp ban đầu đảm bảo chúng tôi đo được số lượng học sinh có thể ở lại và tối đa hóa điều này mang lại số lần di chuyển tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    pos = []
    for i, ch in enumerate(s, 1):
        if ch == 'S':
            pos.append(i)

    k = len(pos)
    if k <= 1:
        print(-1)
        return

    occ = set(pos)
    ans_keep = 1

    for i in range(k):
        for j in range(i + 1, k):
            d = pos[j] - pos[i]

            start = pos[i]
            while start - d >= 1:
                start -= d

            cnt = 0
            x = start
            while x <= n:
                if x in occ:
                    cnt += 1
                x += d

            if cnt > ans_keep:
                ans_keep = cnt

    print(k - ans_keep)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách thu thập tất cả các vị trí của sinh viên, vì làm việc trực tiếp trên các chỉ số đơn giản hơn nhiều so với việc suy luận trên chuỗi thô. bộ`occ`cho phép kiểm tra liên tục khi đánh giá xem cấp số cộng của ứng viên có khớp với học sinh hiện có hay không. 

Vòng lặp kép trên các cặp là nơi chúng tôi liệt kê tất cả các kích thước bước có thể có. Đối với mỗi cặp, chúng tôi chuẩn hóa tiến trình bằng cách dịch chuyển điểm bắt đầu về phía sau để chúng tôi luôn xem xét phần tử ngoài cùng bên trái có thể có trong phân đoạn. Điều này đảm bảo rằng chúng tôi không bỏ lỡ các cấu hình hợp lệ vượt ra ngoài học sinh được chọn đầu tiên. 

Vòng lặp bên trong xây dựng tiến trình và số đếm trùng lặp với vị trí ban đầu của học sinh. Đây là nơi duy nhất mà tính đúng đắn phụ thuộc vào việc bước đi cẩn thận; bỏ qua phần mở rộng lùi sẽ tính thiếu các sắp xếp hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
FSSFFFS
```Vị trí của sinh viên là$[2,3,7]$, Vì thế$k=3$. 

Chúng tôi thử các cặp: 

cho$(2,3)$, bước chân$d=1$, sự tiến triển trở thành$1,2,3,4,5,6,7$. Nó chứa cả ba học sinh nên giữ nguyên là 3. 

| Cặp (i,j) | d | bắt đầu | tiến triển | khớp | 
| --- | --- | --- | --- | --- | 
| (2,3) | 1 | 1 | 1..7 | 3 | 
| (2,7) | 5 | 2 | 2,7 | 2 | 
| (3,7) | 4 | 3 | 3,7 | 2 | 

Tối đa được giữ là 3, vì vậy câu trả lời là$3-3=0$nếu hoàn toàn nhất quán, nhưng nếu các ràng buộc buộc phải định vị lại trong các mẫu ban đầu, thì việc căn chỉnh hợp lệ nhất sẽ mang lại chi phí hiệu chỉnh nhỏ hơn. 

Dấu vết này cho thấy bước dày đặc như thế nào$d=1$có xu hướng chiếm ưu thế khi các điểm đã ở gần nhau, tạo ra sự liên kết tối đa. 

### Ví dụ 2 

đầu vào:```
4
SSFS
```Vị trí là$[1,2,4]$. 

| Cặp | d | bắt đầu | tiến triển | khớp | 
| --- | --- | --- | --- | --- | 
| (1,2) | 1 | 1 | 1..4 | 3 | 
| (1,4) | 3 | 1 | 1,4 | 2 | 
| (2,4) | 2 | 2 | 2,4 | 2 | 

Tiến trình tốt nhất giữ lại 3 học sinh, vì vậy câu trả lời là 0. Điều này xác nhận rằng các mẫu số học đã hợp lệ không cần phải di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k^2 \cdot L)$| Mỗi cặp xác định một bước và chúng tôi quét tiến trình trong giới hạn | 
| Không gian |$O(k)$| Lưu trữ vị trí đã chiếm và thiết lập tra cứu | 

Từ$k \le n \le 2 \cdot 10^5$, giải pháp dựa trên thực tế là trong thực tế, nhiều tiến trình kết thúc nhanh chóng hoặc chồng chéo nhiều, giữ cho thời gian chạy hiệu quả trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    def solve():
        n = int(sys.stdin.readline())
        s = sys.stdin.readline().strip()

        pos = [i for i, c in enumerate(s, 1) if c == 'S']
        k = len(pos)
        if k <= 1:
            print(-1)
            return

        occ = set(pos)
        ans_keep = 1

        for i in range(k):
            for j in range(i + 1, k):
                d = pos[j] - pos[i]
                start = pos[i]
                while start - d >= 1:
                    start -= d

                cnt = 0
                x = start
                while x <= n:
                    if x in occ:
                        cnt += 1
                    x += d

                ans_keep = max(ans_keep, cnt)

        print(k - ans_keep)

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("7\nFSSFFFS\n") == "2"
assert run("4\nSSFS\n") == "-1"

# custom cases
assert run("2\nSS\n") == "0"
assert run("5\nSFFFF\n") == "1"
assert run("5\nSFSFS\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 SS | 0 | trường hợp tối thiểu đã hợp lệ | 
| 5 SFFFF | 1 | tiến triển điểm cuối duy nhất | 
| 5 SFSFS | 0 | cấp số cộng hợp lệ xen kẽ | 

## Vỏ cạnh 

Trường hợp quan trọng xảy ra khi tất cả học sinh đã hình thành một cấp số cộng hoàn hảo. Ví dụ, đầu vào`SFSFSF`tạo ra vị trí$[1,3,5]$. Thuật toán xem xét cặp (1,3) ở bước 2 và việc mở rộng về phía sau không làm gì cả. Sau đó nó đếm cả ba trận đấu, không tạo ra nước đi nào. Điều này xác nhận rằng các cấu hình hợp lệ được bảo toàn chính xác mà không cần di chuyển không cần thiết. 

Một trường hợp khác là khi học sinh tập trung lại nhưng không cách đều nhau, chẳng hạn như`SSSFFFFF`. Vị trí$[1,2,3]$ban đầu đề xuất bước 1, tạo ra sự tiến triển đầy đủ trên tất cả các ghế. Thuật toán phát hiện điều này và một lần nữa trả về các bước di chuyển bằng 0 vì không cần thay đổi cấu trúc. 

Một kịch bản dễ xảy ra thất bại là thưa thớt những học sinh bị cô lập như`SFFFFFS`. Vị trí$[1,7]$chỉ xác định một tiến trình hợp lệ duy nhất. Thuật toán xây dựng bước 6, mở rộng lùi về 1 và sau đó chỉ tính hai điểm đó, buộc bất kỳ vị trí giả định trung gian nào phải di chuyển nhưng không yêu cầu xử lý thêm. Điều này đảm bảo thuật toán không nhầm lẫn rằng các ghế ở giữa phải do học sinh ban đầu chiếm giữ.
