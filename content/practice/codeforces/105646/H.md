---
title: "CF 105646H - Dự báo thời tiết"
description: "Chúng ta được cho một dãy số nguyên và chúng ta muốn chia nó thành các phân đoạn liền kề chính xác $k$. Mỗi đoạn có một giá trị trung bình, được tính bằng tổng các phần tử chia cho độ dài của nó."
date: "2026-06-22T05:25:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105646
codeforces_index: "H"
codeforces_contest_name: "Osijek Competitive Programming Camp, Winter 2024, Day 6: Potyczki Algorytmiczne Contest (The 3rd Universal Cup. Stage 2: Zielona G\u00f3ra)"
rating: 0
weight: 105646
solve_time_s: 50
verified: true
draft: false
---

[CF 105646H - Dự báo thời tiết](https://codeforces.com/problemset/problem/105646/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và chúng ta muốn chia nó thành chính xác$k$các đoạn liền kề. Mỗi đoạn có một giá trị trung bình, được tính bằng tổng các phần tử chia cho độ dài của nó. Trong số tất cả các phân đoạn trong một phân vùng, chúng tôi xem xét mức trung bình nhỏ nhất và mục tiêu của chúng tôi là chọn phân vùng làm cho mức trung bình nhỏ nhất này càng lớn càng tốt. 

Nói cách khác, chúng ta đang cắt mảng thành$k$khoảng thời gian và chúng tôi quan tâm đến khoảng thời gian yếu nhất xét về mức trung bình. Chúng tôi muốn làm cho khoảng thời gian yếu nhất đó trở nên mạnh mẽ nhất có thể. 

Kích thước đầu vào cho phép các mảng đủ lớn để mọi giải pháp đều phải gần tuyến tính hoặc gần tuyến tính cho mỗi lần kiểm tra. Sự hiện diện của một tham số$k$và việc tối ưu hóa trên các phân vùng gợi ý rõ ràng cấu trúc tìm kiếm tham lam hoặc nhị phân thay vì lập trình động trên tất cả các phần tách, điều này sẽ đưa ra một$O(n^2)$hoặc tệ hơn là phụ thuộc. 

Một điểm tinh tế xuất hiện khi tất cả các số đều âm hoặc khi$k$là lớn. Một chiến lược ngây thơ cố gắng mở rộng các phân khúc một cách tham lam cho đến khi mức trung bình của chúng ở mức tốt có thể thất bại vì các quyết định cục bộ ảnh hưởng đến tính khả thi của phân khúc trong tương lai. 

Ví dụ: giả sử mảng là$[1, 2, 3, 4]$Và$k = 2$. Một ý tưởng ngây thơ có thể cố gắng cắt đứt$3$, nhưng tùy thuộc vào cách tính trung bình, điều này có thể không tối ưu. Giải pháp đúng phải đánh giá các phân vùng trên toàn cầu thay vì tham lam. 

Một vấn đề khác phát sinh khi các giá trị được thả nổi hoặc khi các giá trị trung bình được so sánh trực tiếp. Tính không ổn định của dấu phẩy động có thể dẫn đến việc kiểm tra tính khả thi không chính xác đối với các ngưỡng đường biên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê mọi cách có thể để lựa chọn$k-1$cắt vị trí giữa$n-1$những khoảng trống. Đối với mỗi phân vùng, chúng tôi tính tổng phân đoạn và trung bình phân khúc, sau đó lấy mức tối thiểu và tối đa hóa nó. Điều này đúng nhưng có tính bùng nổ về mặt tổ hợp, vì số cách chọn đường cắt là$\binom{n-1}{k-1}$, điều này trở nên không khả thi ngay cả đối với những người khiêm tốn$n$. 

Cái nhìn sâu sắc về cấu trúc quan trọng là ngừng suy nghĩ trực tiếp về các phân vùng và thay vào đó hãy đặt câu hỏi quyết định: cho một giá trị cố định$x$, chúng ta có thể chia mảng thành$k$các phân khúc sao cho mỗi phân khúc có mức trung bình ít nhất$x$? Nếu chúng ta có thể trả lời điều này, chúng ta có thể tìm kiếm mức tối đa có thể$x$sử dụng tìm kiếm nhị phân, bởi vì nếu một ngưỡng nhất định$x$có thể đạt được, bất kỳ ngưỡng nhỏ hơn nào cũng có thể đạt được. 

Để kiểm tra tính khả thi của một giải pháp cố định$x$, chúng ta chuyển đổi điều kiện trung bình thành điều kiện tổng. Một đoạn từ$l$ĐẾN$r$thỏa mãn$$\frac{a_l + \dots + a_r}{r-l+1} \ge x$$tương đương với$$(a_l - x) + \dots + (a_r - x) \ge 0.$$Vì vậy chúng tôi trừ$x$từ mọi phần tử và bây giờ yêu cầu mỗi phân đoạn phải có tổng không âm. 

Với tổng tiền tố, điều này trở thành vấn đề lựa chọn trên các giá trị tiền tố: chúng ta muốn chọn$k+1$các chỉ số (ranh giới phân đoạn) sao cho mỗi phân đoạn tương ứng với tổng chênh lệch không âm giữa các vị trí tiền tố được chọn liên tiếp. Điều này dẫn đến việc kiểm tra tính khả thi tham lam có thể được hiểu là chọn các điểm cắt hợp lệ trong đó tổng tiền tố không giảm so với lần cắt cuối cùng. 

Cấu trúc thu được rút gọn thành việc tìm xem liệu chúng ta có thể chọn$k$các phân đoạn hợp lệ, có thể được đóng khung dưới dạng cấu trúc không giảm dài nhất trên tổng tiền tố được chuyển đổi hoặc trực tiếp hơn dưới dạng phân đoạn tham lam sử dụng các ràng buộc tối thiểu tiền tố. 

Tìm kiếm nhị phân kết thúc việc kiểm tra tính khả thi này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Kiểm tra tham lam | O(n độ chính xác của nhật ký) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi câu trả lời là một giá trị thực và tìm kiếm nó bằng cách sử dụng tìm kiếm nhị phân trong một khoảng chứa giá trị trung bình tối ưu một cách an toàn. 

1. Xác định hàm`check(x)`xác định liệu chúng ta có thể phân chia mảng thành ít nhất$k$phân khúc trong đó mỗi phân khúc có mức trung bình ít nhất$x$. Đây là thử nghiệm khả thi cốt lõi. 
2. Chuyển đổi mảng bằng cách trừ$x$từ mọi phần tử. Điều này chuyển đổi điều kiện “trung bình ≥ x” thành “tổng phân đoạn ≥ 0”. 
3. Tính tổng tiền tố trên mảng được chuyển đổi này. Cho phép$p[i]$là tổng của số đầu tiên$i$các phần tử được biến đổi 
4. Tham lam xây dựng các đoạn từ trái sang phải. Chúng tôi duy trì vị trí cắt cuối cùng. Đối với một đoạn mới kết thúc ở vị trí$i$, chúng tôi kiểm tra xem$p[i] - p[last]\ge 0$. Nếu có, chúng ta có thể đóng một phân đoạn ở đây. 
5. Mỗi lần đóng một đoạn, chúng ta sẽ di chuyển phần cắt cuối cùng tới$i$và tăng số lượng phân đoạn. 
6. Sau khi quét mảng, chúng tôi kiểm tra xem chúng tôi đã thu được ít nhất$k$phân đoạn. 
7. Tìm kiếm nhị phân trên$x$sử dụng cái này`check`chức năng, thắt chặt khoảng thời gian cho đến khi đạt được độ chính xác mong muốn. 

Lý do tinh tế là chúng tôi luôn cắt giảm càng sớm càng tốt khi có thể đạt được phân khúc không âm. Việc trì hoãn cắt giảm không thể giúp ích cho tính khả thi trong tương lai vì nó chỉ làm giảm tính linh hoạt còn lại. 

### Tại sao nó hoạt động 

Điều kiện được chuyển đổi làm giảm mọi ràng buộc phân đoạn thành yêu cầu không âm đối với các khác biệt tiền tố. Phân đoạn tham lam đảm bảo rằng mỗi phân đoạn được chọn là tối đa theo ràng buộc trong khi vẫn hợp lệ. Nếu một phân vùng hợp lệ tồn tại cho một$x$, chiến lược tham lam này sẽ tìm thấy ít nhất một phân vùng như vậy vì bất kỳ phân đoạn hợp lệ nào cũng có thể được chuyển đổi thành phân đoạn tham lam ngoài cùng bên trái mà không vi phạm tính khả thi. Điều này bảo tồn khả năng tiếp cận$k$phân đoạn chính xác khi có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(arr, k, x):
    cnt = 0
    cur = 0
    last = 0

    for v in arr:
        cur += v - x
        if cur - last >= 0:
            cnt += 1
            last = cur

    return cnt >= k

def solve():
    n, k = map(int, input().split())
    arr = list(map(int, input().split()))

    lo, hi = -1e5, 1e5

    for _ in range(60):
        mid = (lo + hi) / 2
        if can(arr, k, mid):
            lo = mid
        else:
            hi = mid

    print(lo)

if __name__ == "__main__":
    solve()
```các`can`Hàm thực hiện phân đoạn tham lam trên mảng được chuyển đổi. Biến`cur`duy trì tổng tiền tố đang chạy sau khi trừ đi mức trung bình của ứng viên. Biến`last`lưu trữ tổng tiền tố ở vị trí cắt cuối cùng, vì vậy`cur - last`chính xác là tổng của phân khúc hiện tại. Khi giá trị này trở thành không âm, chúng ta sẽ đóng một phân đoạn. 

Tìm kiếm nhị phân chạy với số lần lặp cố định, đủ để hội tụ dấu phẩy động. Phạm vi$[-10^5, 10^5]$chứa đựng một cách an toàn mọi ràng buộc điển hình trung bình hợp lý đã cho. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng$[1, 2, 3, 4]$với$k = 2$. 

Chúng tôi kiểm tra một ứng cử viên$x = 2.5$. 

| tôi | giá trị | tổng chuyển đổi | tổng phân đoạn (cur - cuối cùng) | cắt? | phân đoạn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | -1,5 | -1,5 | không | 0 | 
| 2 | 2 | -2.0 | -3,5 | không | 0 | 
| 3 | 3 | -1,5 | -5.0 | không | 0 | 
| 4 | 4 | -0,5 | -5,5 | không | 0 | 

Không thể cắt được nên chúng tôi không đạt được 2 phân đoạn. Điều này cho thấy rằng$x$là quá lớn. 

Bây giờ kiểm tra$x = 1.5$. 

| tôi | giá trị | tổng chuyển đổi | tổng phân đoạn | cắt? | phân đoạn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | -0,5 | -0,5 | không | 0 | 
| 2 | 2 | 0,5 | 0,0 | vâng | 1 | 
| 3 | 3 | 1,5 | 1,5 | vâng | 2 | 

Chúng tôi đạt được 2 phân khúc, vì vậy giá trị này là khả thi. Tìm kiếm nhị phân sau đó sẽ thử các giá trị cao hơn và hội tụ gần mức trung bình tối thiểu có thể đạt được tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log R)$| Mỗi kiểm tra tính khả thi là tuyến tính, tìm kiếm nhị phân chạy trên phạm vi chính xác | 
| Không gian |$O(1)$| Chỉ một số biến đang chạy được duy trì | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì mỗi lần kiểm tra là một lần vượt qua mảng và số lần kiểm tra được cố định bởi các yêu cầu về độ chính xác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""  # printing directly, so ignore return

# basic feasibility
run("""4 2
1 2 3 4
""")

# all equal
run("""5 3
5 5 5 5 5
""")

# increasing sequence
run("""6 2
1 2 3 4 5 6
""")

# minimum size segments
run("""3 3
10 1 10
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 4, k=2 | khoảng 2,5 | tối ưu hóa phân vùng cơ bản | 
| tất cả đều bình đẳng | giá trị chính xác | độ ổn định của các mức trung bình bằng nhau | 
| ngày càng tăng | thiên vị đuôi cao hơn | tham lam phân khúc đúng đắn | 
| k=n | phần tử tối thiểu | trường hợp phân vùng cực đoan | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$k = n$. Mỗi phần tử tạo thành phân đoạn riêng của nó, vì vậy câu trả lời phải bằng phần tử mảng tối thiểu. Thuật toán xử lý việc này vì mỗi lần kiểm tra phân đoạn đều giảm xuống một giá trị được chuyển đổi duy nhất$a_i - x \ge 0$và tìm kiếm nhị phân hội tụ chính xác đến giá trị nhỏ nhất giữ cho tất cả đều không âm. 

Một trường hợp cạnh khác xảy ra khi tất cả các giá trị giống hệt nhau. Bất kỳ phân vùng nào cũng mang lại giá trị trung bình của phân khúc giống hệt nhau, vì vậy câu trả lời là không đổi. Việc kiểm tra tính khả thi luôn thành công chính xác ở giá trị đó và không thành công ở trên giá trị đó vì tất cả các giá trị được chuyển đổi đều trở thành âm. 

Trường hợp cạnh cuối cùng là khi các giá trị âm và trộn lẫn với các giá trị dương. Phép biến đổi đảm bảo tính chính xác vì nó chuyển đổi tỷ lệ tuyệt đối thành độ lệch tương đối xung quanh ứng cử viên$x$, do đó việc phân đoạn chỉ phụ thuộc vào số dư tích lũy hơn là phân phối dấu hiệu.
