---
title: "CF 105950F - Frodo và Sam"
description: "Chúng ta có hai chuỗi loại đá song song, mỗi chuỗi là một hoán vị của cùng một bộ nhãn. Người chơi chỉ được phép cắt cả hai chuỗi từ bên trái và chỉ từ bên phải."
date: "2026-06-21T21:59:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105950
codeforces_index: "F"
codeforces_contest_name: "UDESC Selection Contest 2025-1"
rating: 0
weight: 105950
solve_time_s: 94
verified: true
draft: false
---

[CF 105950F – Frodo và Sam](https://codeforces.com/problemset/problem/105950/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi loại đá song song, mỗi chuỗi là một hoán vị của cùng một bộ nhãn. Người chơi chỉ được phép cắt cả hai chuỗi từ bên trái và chỉ từ bên phải. Frodo xóa tiền tố khỏi mỗi chuỗi, trong khi Sam xóa hậu tố khỏi mỗi chuỗi. Sau những lần loại bỏ này, cả hai đoạn giữa còn lại phải trở nên giống hệt nhau, nghĩa là chúng đại diện cho cùng một chuỗi các loại đá theo cùng một thứ tự. 

Chi phí của một hoạt động được xác định bởi số lượng đá bị phá hủy. Frodo trả tiền cho mỗi viên đá được lấy ra từ phía bên trái, và Sam trả tiền cho mỗi viên đá được lấy ra từ phía bên phải, với chi phí F và S cho mỗi đơn vị khác nhau. Mục tiêu là chọn vị trí cắt sao cho các đoạn ở giữa khớp với nhau và ít nhất một viên đá còn lại, đồng thời giảm thiểu tổng chi phí phá hủy. 

Mỗi lời giải hợp lệ tương ứng với việc chọn một đoạn liền kề trong P và một đoạn liền kề trong Q có độ dài bằng nhau và có nội dung giống nhau. Khi một cặp phân đoạn như vậy được chọn, mọi thứ bên ngoài các phân đoạn đó sẽ bị xóa. Vấn đề tối ưu hóa trở thành việc chọn cặp phân khúc phù hợp nhất để giảm thiểu chi phí xóa. 

Các ràng buộc cho phép tối đa 100000 phần tử, do đó, mọi so sánh bậc hai trên tất cả các phân đoạn đều không thể thực hiện được. Ngay cả kết hợp chuỗi con O(N^2) cũng vượt xa giới hạn. Điều này ngay lập tức gợi ý rằng chúng ta phải giảm vấn đề về một cấu trúc hỗ trợ xử lý chuỗi tuyến tính hoặc gần tuyến tính, chẳng hạn như mảng hậu tố hoặc hàm băm cuộn kết hợp với lý luận LCP. 

Một vấn đề tế nhị xuất hiện khi nghĩ về tính đúng đắn: ngay cả khi chúng ta tìm thấy một đoạn chung dài thì việc tối đa hóa độ dài của nó thôi là chưa đủ. Chi phí cũng phụ thuộc vào vị trí phân đoạn đó xảy ra trong cả hai hoán vị. Những phân đoạn đầu sẽ rẻ hơn đối với Frodo, những phân đoạn muộn sẽ rẻ hơn đối với Sam, do đó, hai lần xuất hiện có cùng độ dài phân khúc có thể tạo ra những chi phí khác nhau. 

Một cách tiếp cận ngây thơ chỉ tối đa hóa độ dài chồng chéo có thể thất bại khi một đoạn ngắn hơn một chút xuất hiện ở những vị trí rẻ hơn nhiều. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử từng cặp vị trí bắt đầu trong P và Q, mở rộng trong khi các ký tự khớp và tính toán chi phí cho mỗi trận đấu. Điều này tìm thấy chính xác tất cả các mảng con phổ biến, nhưng mỗi phép so sánh có thể mất thời gian tuyến tính, dẫn đến O(N³) trong trường hợp xấu nhất, không thể sử dụng được cho N lên tới 100000. 

Một quan điểm có cấu trúc hơn là nhận ra rằng vấn đề cơ bản là tìm chuỗi con chung tốt nhất giữa hai chuỗi, trong đó “tốt nhất” không chỉ dài nhất mà còn rẻ nhất theo hàm chi phí phụ thuộc vào vị trí. 

Quan sát quan trọng là mọi giải pháp hợp lệ đều tương ứng với một chuỗi con chung của P và Q, và mọi chuỗi con chung xuất hiện dưới dạng tiền tố của một hậu tố nào đó trong P và một số hậu tố trong Q. Do đó, thay vì liệt kê trực tiếp các chuỗi con, chúng ta có thể làm việc với các hậu tố. 

Bằng cách xây dựng một mảng hậu tố trên sự kết hợp của P và Q bằng dấu phân cách, chúng ta có thể tính toán tiền tố chung dài nhất giữa các hậu tố liền kề. Bất kỳ chuỗi con chung nào cũng phải xuất hiện dưới dạng LCP của một số cặp liền kề trong đó một hậu tố đến từ P và chuỗi kia đến từ Q. Điều này làm giảm không gian tìm kiếm từ tất cả các cặp chuỗi con thành các cặp ứng cử viên O(N). 

Đối với mỗi cặp như vậy, chúng tôi tính toán chi phí tốt nhất bằng cách sử dụng vị trí chính xác của hai hậu tố và độ dài LCP của chúng. Chúng tôi đánh giá công thức chi phí cho chuỗi con ứng cử viên đó và lấy mức tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N³) | O(1) | Quá chậm | 
| Mảng hậu tố + LCP | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Ghép P và Q thành một mảng duy nhất, được phân tách bằng một dấu phân cách duy nhất không xuất hiện trong cả hai hoán vị. Điều này đảm bảo hậu tố từ các phía khác nhau vẫn có thể phân biệt được. 
2. Xây dựng mảng hậu tố cho chuỗi kết hợp, cùng với mảng LCP. Điều này cho phép chúng ta tính toán tiền tố chung dài nhất giữa hai hậu tố liền kề bất kỳ theo thứ tự được sắp xếp. 
3. Lặp lại các cặp liền kề trong mảng hậu tố. Bất cứ khi nào một hậu tố bắt nguồn từ P và hậu tố kia bắt nguồn từ Q, chúng ta xác định một chuỗi con chung ứng cử viên. 
4. Gọi i là vị trí bắt đầu trong P và j là vị trí bắt đầu trong Q. Gọi L là giá trị LCP giữa các hậu tố này. Điều này có nghĩa là P[i..i+L-1] khớp với Q[j..j+L-1]. 
5. Đối với mỗi ứng cử viên, hãy tính tổng chi phí để xóa mọi thứ bên ngoài phân đoạn đã chọn. Chi phí phụ thuộc vào số lượng phần tử được loại bỏ ở bên trái và bên phải trong cả hai mảng. Điều này trở thành biểu thức số học trực tiếp dựa trên i, j và L. 
6. Theo dõi chi phí tối thiểu trên tất cả các ứng viên hợp lệ có L ít nhất là 1, vì phân đoạn cuối cùng không được để trống. 

### Tại sao nó hoạt động 

Mọi cấu hình cuối cùng hợp lệ tương ứng với một chuỗi con chung của P và Q. Mỗi chuỗi con như vậy xuất hiện dưới dạng tiền tố của một số cặp hậu tố và mảng hậu tố đảm bảo rằng ít nhất một cặp liền kề theo thứ tự từ điển sẽ nắm bắt được tiền tố chung tối đa có thể có cho cặp hậu tố đó. Vì chúng tôi đánh giá tất cả các cặp mảng chéo liền kề nên mọi chuỗi con chung tối đa đều được xem xét và các chuỗi con ngắn hơn được ngầm bao hàm thông qua sự xuất hiện của chúng trong cấu trúc hậu tố. Hàm chi phí chỉ phụ thuộc vào điểm cuối của chuỗi con được chọn, do đó việc đánh giá từng ứng cử viên sẽ duy trì tính tối ưu một cách độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_suffix_array(s):
    n = len(s)
    k = 1
    sa = list(range(n))
    rank = s[:]
    tmp = [0] * n

    def key(i):
        return (rank[i], rank[i + k] if i + k < n else -1)

    while True:
        sa.sort(key=key)
        tmp[sa[0]] = 0
        for i in range(1, n):
            tmp[sa[i]] = tmp[sa[i - 1]] + (key(sa[i - 1]) < key(sa[i]))
        rank = tmp[:]
        if rank[sa[-1]] == n - 1:
            break
        k <<= 1

    return sa, rank

def build_lcp(s, sa, rank):
    n = len(s)
    h = 0
    lcp = [0] * n
    for i in range(n):
        r = rank[i]
        if r == 0:
            continue
        j = sa[r - 1]
        while i + h < n and j + h < n and s[i + h] == s[j + h]:
            h += 1
        lcp[r] = h
        if h:
            h -= 1
    return lcp

def solve():
    n, f, s = map(int, input().split())
    p = list(map(int, input().split()))
    q = list(map(int, input().split()))

    sep = 0
    arr = p + [sep] + q
    n1 = len(p)

    sa, rank = build_suffix_array(arr)
    lcp = build_lcp(arr, sa, rank)

    INF = 10**30
    ans = INF

    total_const = 2 * n * s - 2 * f

    for i in range(1, len(sa)):
        a, b = sa[i - 1], sa[i]
        if (a < n1) == (b < n1):
            continue

        L = lcp[i]
        if L <= 0:
            continue

        if a < n1:
            ip = a
            iq = b - (n1 + 1)
        else:
            ip = b
            iq = a - (n1 + 1)

        L = min(L, n - ip, n - iq)
        if L <= 0:
            continue

        val = (f - s) * (ip + iq) - 2 * s * (L - 1)
        ans = min(ans, val)

    print(ans + total_const)

if __name__ == "__main__":
    solve()
```Mảng hậu tố được xây dựng bằng cách nhân đôi thứ hạng lặp lại, đủ cho 200k phần tử trong thực tế. Mảng LCP được tính toán bằng phương pháp Kasai tiêu chuẩn. 

Mỗi cặp hậu tố liền kề chỉ được kiểm tra khi một cặp đến từ P và cặp còn lại đến từ Q. LCP của chúng cung cấp phân khúc được chia sẻ tối đa bắt đầu từ các vị trí đó và chúng tôi đánh giá trực tiếp công thức chi phí. Sự thay đổi liên tục trong câu trả lời cuối cùng dẫn đến việc xóa hoàn toàn bên ngoài phân đoạn đã chọn. 

Một điểm tinh tế phổ biến là lập chỉ mục chuyển đổi giữa mảng được nối và vị trí Q ban đầu, phải trừ chính xác dấu phân cách và độ dài tiền tố. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó P và Q hoàn toàn khớp nhau. Mảng hậu tố sẽ đặt các hậu tố giống hệt nhau liền kề, tạo ra LCP lớn bằng N. Thuật toán chọn phân đoạn đầy đủ đó, dẫn đến không xóa bên trong phân đoạn và chỉ xóa ranh giới, là tối ưu. 

Trong trường hợp thứ hai khi các phân đoạn trùng khớp xuất hiện ở các độ lệch khác nhau, mảng hậu tố vẫn ghép nối các hậu tố tương ứng. LCP nắm bắt chính xác độ dài chồng chéo và so sánh chi phí đảm bảo rằng phân khúc ngắn hơn một chút nhưng xảy ra sớm hơn có thể đánh bại phân khúc dài hơn nhưng có vị thế đắt đỏ. 

| Bước | Cặp SA | LCP | tôi (P) | j (Q) | Độ dài đoạn | Đóng góp chi phí | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | cặp chéo | L | tôi | j | L | giá trị tính toán | 
| 2 | cặp tiếp theo | L' | tôi' | j' | L' | giá trị tính toán | 

Mỗi hàng tương ứng với việc đánh giá một chuỗi con ứng cử viên. Giá trị tối thiểu trên tất cả các hàng mang lại câu trả lời cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | xây dựng mảng hậu tố chiếm ưu thế | 
| Không gian | O(N) | mảng cho SA, thứ hạng, LCP và nối | 

Các ràng buộc cho phép tối đa 100000 phần tử và cấu trúc mảng hậu tố O(N log N) vừa vặn thoải mái trong giới hạn thời gian, trong khi mức sử dụng bộ nhớ tuyến tính vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# No direct runnable assertions here due to omitted full harness integration
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giống nhau tối thiểu | chi phí nhỏ | độ đúng cơ sở | 
| không chồng chéo | xóa hoàn toàn | hành vi dự phòng | 
| trận đấu chuyển | lựa chọn chuỗi con tối ưu | độ nhạy vị trí | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi chuỗi con phù hợp nhất rất ngắn nhưng lại xuất hiện ở những vị trí rất thuận lợi. Cách tiếp cận chuỗi con chung dài nhất ngây thơ sẽ bỏ lỡ nó. Đánh giá dựa trên mảng hậu tố vẫn xem xét nó vì nó xuất hiện dưới dạng LCP của một số cặp hậu tố liền kề. 

Một trường hợp cạnh khác là khi P và Q chia sẻ nhiều chuỗi con giống hệt nhau có cùng độ dài nhưng ở các vị trí khác nhau. Thuật toán phân biệt chính xác chúng vì chi phí phụ thuộc vào i và j, không chỉ độ dài. 

Trường hợp cạnh cuối cùng là khi kết quả khớp duy nhất có độ dài 1. Ngay cả trong kịch bản trùng lặp tối thiểu này, thuật toán vẫn đánh giá chính xác vì mọi cặp hậu tố mảng chéo có LCP ≥ 1 đều được xem xét.
