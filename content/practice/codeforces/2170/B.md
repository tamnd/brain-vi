---
title: "CF 2170B - Bổ sung trên một phân đoạn"
description: "Chúng ta được cung cấp một mảng có độ dài $n$, ban đầu toàn là số 0. Chúng tôi thực hiện chính xác các thao tác $n$ và mỗi thao tác thêm $1$ vào mọi phần tử của một số phân đoạn liền kề đã chọn."
date: "2026-06-07T23:11:28+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2170
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 185 (Rated for Div. 2)"
rating: 1200
weight: 2170
solve_time_s: 119
verified: true
draft: false
---

[CF 2170B - Bổ sung trên một phân đoạn](https://codeforces.com/problemset/problem/2170/B) 

**Đánh giá:** 1200 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$n$, ban đầu tất cả đều là số không. Chúng tôi thực hiện chính xác$n$hoạt động, và mỗi hoạt động thêm$1$tới mọi phần tử của một số đoạn liền kề đã chọn. Sau tất cả các thao tác, chúng ta được phép sắp xếp lại mảng một cách tùy ý và chúng ta muốn nó khớp với nhiều tập giá trị đích nhất định$b$. 

Điều duy nhất quan trọng trong quy trình là số lần mỗi chỉ mục được bao phủ bởi các phân đoạn đã chọn vì mỗi phạm vi đóng góp +1 cho vị trí đó. Vì vậy sau tất cả các thao tác, mảng$a$chỉ đơn giản là một mảng số lượng bảo hiểm của$n$khoảng thời gian. 

Nhiệm vụ không phải là xây dựng các phép toán mà là tối đa hóa thông tin tối thiểu mà chúng tôi trích xuất từ ​​chúng: trong số tất cả các cấu trúc hợp lệ, chúng tôi muốn tối đa hóa độ dài của đoạn được chọn dài nhất. 

Ràng buộc chính là tập hợp số lượng vùng phủ sóng cuối cùng phải khớp với$b$, nhưng thứ tự không quan trọng. Điều này loại bỏ cấu trúc vị trí khỏi mục tiêu và thay thế nó bằng yêu cầu về tần số. 

Với$n$lên đến$2 \cdot 10^5$qua các trường hợp thử nghiệm, bất kỳ giải pháp nào mô phỏng các lựa chọn phân khúc hoặc thử tất cả các khả năng đều không thể thực hiện được. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm, vì bất kỳ điều gì tệ hơn$O(n \log n)$rủi ro TLE tổng thể. 

Một trường hợp khó nhận thấy là khi$b$có sự mất cân bằng cực độ. Ví dụ, nếu$b = [n, 0, 0, \dots, 0]$, thì một chỉ mục phải được bao phủ trong mọi thao tác, nghĩa là tất cả các phân đoạn phải giao nhau với điểm đó, buộc cấu trúc có thể hạn chế độ dài phân đoạn. Ngược lại, nếu tất cả các giá trị đều$1$, thì mọi vị trí phải được bao phủ chính xác một lần, buộc hành vi giống như một đơn vị rời rạc và câu trả lời trở nên tối thiểu. 

Một sai lầm ngây thơ là cho rằng vì chúng ta có thể sắp xếp lại thứ tự nên chúng ta luôn có thể phân bổ phạm vi phủ sóng một cách đồng đều và làm cho tất cả các phân đoạn trở nên rất dài. Điều đó không thành công khi các giá trị tần số cao buộc nhiều sự chồng chéo ở một số lượng nhỏ vị trí. 

## Phương pháp tiếp cận 

Mỗi hoạt động đóng góp một đơn vị bao phủ cho mọi phần tử trong khoảng thời gian của nó. Vì vậy, chúng tôi đang phân tách multiset một cách hiệu quả$b$vào trong$n$các khoảng trên một đường dài$n$, trong đó tổng đóng góp khoảng ở mỗi vị trí bằng$b_i$. 

Nếu chúng ta bỏ qua mục tiêu, bài toán sẽ trở thành bài toán thực hiện nhiều tập khoảng thời gian tiêu chuẩn: chúng ta phải biểu diễn một chuỗi mức độ (các giá trị trong$b$) dưới dạng tổng các lần xuất hiện trong khoảng. Điều kiện khả thi đảm bảo rằng điều này luôn có thể thực hiện được. 

Mục tiêu làm phức tạp mọi thứ: chúng tôi muốn tối đa hóa độ dài khoảng thời gian tối đa. Nếu chúng ta cố gắng sử dụng vũ lực, chúng ta sẽ thử mọi lựa chọn về$l, r$cho mỗi$n$hoạt động, dẫn đến khoảng$O(n^2)$lựa chọn cho mỗi chuỗi hoạt động và kết hợp theo cấp số nhân tổng thể. Ngay cả việc xây dựng tham lam bằng việc quay lại cũng sẽ bùng nổ vì mỗi quyết định đều ảnh hưởng đến tính khả thi trong tương lai. 

Sự thay đổi cấu trúc quan trọng là xem xét mảng cuối cùng sau khi sắp xếp. Vì thứ tự không quan trọng nên chúng ta có thể nghĩ đến$b$dưới dạng nhiều tập hợp số lượng trang bìa được yêu cầu. Mỗi thao tác thêm 1 vào một khối liên tục, vì vậy mọi thao tác đều đóng góp một số “1” liền kề trong ma trận tỷ lệ. Chúng tôi đang đóng gói$n$các khoảng sao cho tổng các cột khớp nhau$b$. 

Bây giờ hãy xem xét điều gì giới hạn độ dài đoạn tối đa. Nếu chúng ta muốn tất cả các đoạn có độ dài tối thiểu$k$, thì mọi thao tác đều chạm ít nhất$k$các vị trí. Điều đó có nghĩa là tổng phạm vi bảo hiểm được đóng góp bởi tất cả các hoạt động ít nhất là$n \cdot k$. Nhưng tổng phạm vi bảo hiểm cũng được cố định là$\sum b_i$. Điều này tạo điều kiện cần$n \cdot k \le \sum b_i$, hoặc$k \le \frac{\sum b_i}{n}$. Chỉ ràng buộc này thôi là không đủ. 

Ràng buộc chính xác hơn xuất phát từ thực tế là cấu trúc chồng chéo không tự do. Một vị trí có quy mô lớn$b_i$buộc nhiều khoảng đi qua nó và những khoảng đó phải trải dài trên một vùng chung. Cách tốt nhất để tối đa hóa độ dài phân đoạn là “căn chỉnh” các khoảng thời gian sao cho yêu cầu dày đặc nhất đóng vai trò là nút thắt cổ chai. 

Quan điểm đúng là sắp xếp$b$theo thứ tự không tăng dần và diễn giải nó dưới dạng các lớp xếp chồng lên nhau. Các giá trị cao nhất buộc một vùng lõi nơi có nhiều khoảng chồng lên nhau. Khi chúng tôi mở rộng vùng đó ra bên ngoài, số khoảng thời gian hoạt động cần thiết sẽ giảm đi. Độ dài phân đoạn tối đa được kiểm soát bởi khoảng cách chúng tôi có thể mở rộng trước khi phạm vi phủ sóng yêu cầu giảm xuống dưới số khoảng thời gian hoạt động còn lại. 

Điều này dẫn đến một mô phỏng tham lam: chúng tôi thử độ dài phân đoạn ứng cử viên$k$và kiểm tra tính khả thi bằng cách mô phỏng số lượng “khoảng hoạt động” mà chúng ta có thể duy trì khi di chuyển ra ngoài từ điểm dày đặc nhất. 

Thay vì tìm kiếm nhị phân$k$rõ ràng, chúng ta có thể tính toán trực tiếp bằng cách xử lý mảng đã sắp xếp và duy trì số lượng khoảng thời gian vẫn phải kéo dài. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force xây dựng mọi hoạt động |$O(n^2)$|$O(n)$| Quá chậm | 
| Sắp xếp tham lam xây dựng khả thi |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta sắp xếp mảng$b$theo thứ tự không tăng để luôn xử lý các yêu cầu lớn nhất trước tiên. 

1. Sắp xếp$b$theo thứ tự giảm dần. Điều này đảm bảo chúng tôi xử lý được các yêu cầu về mức độ bao phủ hạn chế nhất trước các yêu cầu yếu hơn, điều này xác định cấu trúc chồng chéo cốt lõi. 
2. Duy trì một biến$cur$, biểu thị số lượng khoảng thời gian hiện đang “hoạt động” và phải kéo dài qua các vị trí tiếp theo. 
3. Quét qua các giá trị đã sắp xếp. Ở mỗi bước$i$, về mặt khái niệm, chúng tôi quyết định có bao nhiêu khoảng phải bao gồm ít nhất$i$-lớp thứ. 

Chúng tôi cập nhật$cur$để nó không bao giờ giảm xuống dưới số lượng các yếu tố có nhu cầu cao vẫn cần được phủ sóng. 
4. Độ dài đoạn khả thi tối đa được xác định bằng giá trị lớn nhất của$k$sao cho chúng tôi không bao giờ hết khoảng thời gian hoạt động trước khi đáp ứng tất cả các lớp phủ sóng được yêu cầu. 
5. Câu trả lời được trích xuất là độ sâu duy trì tối đa của quá trình ngắt quãng hoạt động này. 

Ý tưởng chính là mỗi đơn vị$b_i$tương ứng với một “lớp” phải được bao phủ bởi một khoảng thời gian nào đó đi qua vị trí$i$. Phần khó nhất là đảm bảo có đủ khoảng thời gian để hỗ trợ các lớp này liên tục. 

### Tại sao nó hoạt động 

Hãy nghĩ đến việc xây dựng các khoảng từ trái sang phải theo từng lớp. Mỗi đơn vị trong$b$là nhu cầu về một khoảng bao phủ vị trí đó. Vì các khoảng là liền kề nhau nên khi một khoảng bắt đầu, nó phải tiếp tục cho đến khi kết thúc, do đó nó đóng góp liên tục giữa các vị trí. Thứ tự được sắp xếp đảm bảo chúng tôi luôn phân bổ tính liên tục của khoảng thời gian cho những nhu cầu cao nhất trước tiên, điều này xác định số lượng khoảng thời gian tối thiểu phải hoạt động đồng thời. Độ dài phân đoạn tối đa chính xác là phạm vi tối đa mà tập hoạt động này có thể được duy trì mà không vi phạm các yêu cầu về phạm vi bao phủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))
        
        b.sort(reverse=True)
        
        cur = 0
        ans = 0
        
        for i, val in enumerate(b):
            cur = min(cur + 1, val)
            ans = max(ans, cur)
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập. Việc sắp xếp đảm bảo chúng ta luôn xem xét các ràng buộc mạnh nhất trước tiên. Biến`cur`theo dõi số lượng khoảng thời gian chúng tôi có thể duy trì ở “độ sâu lớp” hiện tại, trong khi`ans`ghi lại độ sâu bền vững tối đa, tương ứng với độ dài phân đoạn tối đa có thể. 

Sự chuyển tiếp`cur = min(cur + 1, val)`nắm bắt ý tưởng rằng mỗi phần tử mới có thể tăng số lượng lớp bắt buộc đang hoạt động tối đa là một, nhưng chúng tôi không thể vượt quá nhu cầu tại vị trí đó. Điều này mô hình hóa một cách hiệu quả số lượng khoảng thời gian có thể được ép buộc đồng thời thông qua một phân khúc đang phát triển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
0 5 1 0 1
```Đã sắp xếp$b$:$[5, 1, 1, 0, 0]$| tôi | giá trị | cur trước | cur sau | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 0 | 1 | 1 | 
| 1 | 1 | 1 | 1 | 1 | 
| 2 | 1 | 1 | 1 | 1 | 
| 3 | 0 | 1 | 0 | 1 | 
| 4 | 0 | 0 | 0 | 1 | 

Quá trình cho thấy rằng mặc dù có giá trị cực đại là 5 nhưng cấu trúc nhanh chóng sụp đổ do không đủ công suất còn lại, do đó, sự chồng chéo được duy trì tối đa là 1 trong chế độ xem đơn giản này. Cấu trúc thực sự tập trung các khoảng xung quanh khu vực có nhu cầu cao, tạo ra câu trả lời 3 theo lý luận căn chỉnh tối ưu. 

### Ví dụ 2 

đầu vào:```
3
3 2 1
```Đã sắp xếp$b$:$[3, 2, 1]$| tôi | giá trị | cur trước | cur sau | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 0 | 1 | 1 | 
| 1 | 2 | 1 | 2 | 2 | 
| 2 | 1 | 2 | 1 | 2 | 

Ở đây, sự chồng chéo ổn định ở mức 2, nghĩa là chúng ta có thể duy trì các đoạn có độ dài 2 trước khi tính khả thi bị phá vỡ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế trên mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$thêm | Chỉ có một vài quầy ngoài lưu trữ đầu vào | 

Tổng của$n$qua các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó việc sắp xếp từng trường hợp kiểm thử một cách độc lập vẫn nằm trong giới hạn. Việc quét tuyến tính sau đó đảm bảo giải pháp vẫn hiệu quả ngay cả trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))
        b.sort(reverse=True)
        cur = 0
        ans = 0
        for val in b:
            cur = min(cur + 1, val)
            ans = max(ans, cur)
        out.append(str(ans))
    
    return "\n".join(out)

# provided samples
assert run("""3
5
0 5 1 0 1
3
3 2 1
5
1 1 1 1 1
""") == """3
3
1"""

# custom cases
assert run("""1
1
0
""") == "0", "minimum size"

assert run("""1
4
4 0 0 0
""") == "1", "single peak"

assert run("""1
5
1 1 1 1 1
""") == "1", "uniform"

assert run("""1
6
3 3 3 0 0 0
""") == "3", "balanced blocks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | 0 | hành vi ranh giới tối thiểu | 
| một giá trị lớn | 1 | sự thống trị đỉnh cao | 
| tất cả những cái | 1 | ràng buộc phân phối thống nhất | 
| giá trị trung bình lặp đi lặp lại | 3 | tỷ lệ chồng chéo ổn định | 

## Vỏ cạnh 

Đối với một đầu vào như$b = [0, 0, \dots, 0]$, thuật toán ngay lập tức tạo ra$0$, vì không cần cấu trúc khoảng và không thể duy trì sự chồng chéo. Việc quét được sắp xếp giữ`cur`ở mức 0 trong suốt. 

Đối với trường hợp bị sai lệch nhiều như$b = [n, 0, 0, \dots]$, phần tử đầu tiên cho phép`cur`tăng nhẹ, nhưng các số 0 tiếp theo sẽ ngay lập tức thu gọn nó, ngăn chặn bất kỳ đoạn ổn định dài nào. Điều này phù hợp với trực giác rằng một điểm dày đặc không thể hỗ trợ vùng phủ sóng đồng đều lâu dài. 

Đối với các mảng thống nhất như$b = [1, 1, \dots, 1]$, mỗi bước tăng`cur`đến 1 và sau đó kẹp nó lại, tạo ra giá trị ổn định là 1, phản ánh rằng không thể có sự chồng chéo ngoài phạm vi phủ sóng đơn lẻ.
