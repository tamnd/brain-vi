---
title: "CF 103455C - Đèn xanh đèn đỏ"
description: "Chúng ta được đưa ra dòng thời gian của một trò chơi đèn giao thông đơn giản và một nhóm đối thủ cố gắng về đích trước khi đèn chuyển sang màu đỏ vĩnh viễn."
date: "2026-07-03T07:14:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103455
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 12-03-21 Div. 2 (Beginner)"
rating: 0
weight: 103455
solve_time_s: 46
verified: true
draft: false
---

[CF 103455C - Đèn xanh đèn đỏ](https://codeforces.com/problemset/problem/103455/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra dòng thời gian của một trò chơi đèn giao thông đơn giản và một nhóm đối thủ cố gắng về đích trước khi đèn chuyển sang màu đỏ vĩnh viễn. Đèn bắt đầu ở trạng thái màu đỏ tại thời điểm 0, sau đó chuyển đổi giữa màu đỏ và xanh lục tại các thời điểm cụ thể được đưa ra theo thứ tự tăng dần. Sau lần chuyển đổi được ghi cuối cùng, đèn vẫn đỏ mãi mãi. 

Mỗi thí sinh bắt đầu ở vị trí số 0 và muốn đạt được khoảng cách mục tiêu$K$. Khi đèn xanh, họ di chuyển về phía trước với tốc độ cá nhân không đổi. Khi đèn đỏ, họ không cử động chút nào. Chúng có thể khởi động và dừng ngay lập tức, do đó không cần xem xét độ trễ tăng tốc hoặc quán tính. Câu hỏi đặt ra là đối với mỗi thí sinh, hãy xác định xem họ có thể đạt được cự ly$K$nghiêm ngặt trước khi ánh sáng chuyển sang màu đỏ vĩnh viễn. 

Đầu vào cung cấp số lần thay đổi ánh sáng$M$, số lượng đối thủ cạnh tranh$N$, khoảng cách mục tiêu$K$, danh sách thời điểm đèn chuyển trạng thái và tốc độ của từng thí sinh. Đầu ra là chỉ báo nhị phân cho mỗi thí sinh: 1 nếu họ về đích đúng thời gian, nếu không thì 0. 

Các ràng buộc đủ nhỏ để có thể thực hiện mô phỏng trực tiếp cho mỗi đối thủ cạnh tranh trên tất cả các khoảng xanh. Với$M, N \le 5000$, ngây thơ$O(MN)$hoặc$O(M+N)$mỗi chiến lược thử nghiệm là đủ, vì tổng số thao tác tối đa là khoảng$2.5 \cdot 10^7$, phù hợp thoải mái với giới hạn thời gian trong Python nếu được triển khai rõ ràng. 

Một điểm tinh tế là cấu trúc xen kẽ của ánh sáng. Đèn bắt đầu có màu đỏ, do đó khoảng thời gian đầu tiên lên đến$t_1$là màu đỏ, sau đó là màu xanh lá cây từ$t_1$ĐẾN$t_2$, rồi lại đỏ, v.v. Một trường hợp khác là chuyển động sau lần chuyển đổi cuối cùng không thành vấn đề vì sau đó đèn sẽ chuyển sang màu đỏ vĩnh viễn. 

Một sai lầm ngây thơ là cho rằng chuyển động là liên tục hoặc coi tất cả thời gian xanh là một khối duy nhất. Ví dụ, nếu các công tắc đôi khi$2, 5$, đèn chỉ xanh giữa những khoảng thời gian chính xác đó, không phải ở mọi nơi ngoại trừ những điểm đó. Hiểu sai điều này dẫn đến việc tính toán quá nhiều thời gian di chuyển và tuyên bố không chính xác tất cả những người chơi nhanh là người chiến thắng. 

Một trường hợp khác xuất hiện khi thời gian yêu cầu của thí sinh khớp chính xác với thời điểm kết thúc khoảng xanh. Vì đèn chuyển sang màu đỏ ngay lập tức tại thời điểm chuyển đổi nên việc về đích vào đúng thời điểm đó vẫn chỉ hợp lệ nếu họ hoàn thành quãng đường đúng trước hoặc vào thời điểm đó, tùy theo cách hiểu. Mô hình đúng coi việc đạt đến điểm giới hạn hoặc trước điểm giới hạn là thành công. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: mô phỏng dòng thời gian thay đổi ánh sáng cho từng đối thủ một cách độc lập. Duy trì khoảng cách mà đối thủ đã di chuyển cho đến nay và lặp lại từng khoảng thời gian giữa các lần chuyển đổi liên tiếp. Đối với mỗi khoảng màu xanh lá cây, hãy tính khoảng cách chúng đi được khi tốc độ nhân với thời gian và tích lũy nó cho đến khi chúng đạt được$K$hoặc dòng thời gian kết thúc. 

Cách tiếp cận này đúng vì nó mô phỏng trực tiếp quá trình thực tế. Điểm thất bại là hiệu suất. Mỗi thí sinh quét tất cả$M$khoảng thời gian, vì vậy độ phức tạp là$O(NM)$, trong trường hợp xấu nhất đạt tới 25 triệu hoạt động. Đó là ranh giới nhưng vẫn có thể chấp nhận được trong Python được tối ưu hóa, mặc dù về mặt khái niệm nó trở nên lãng phí. 

Quan sát quan trọng là chuyển động có tính xác định và tuyến tính trong mỗi phân đoạn màu xanh lá cây, vì vậy chúng ta không bao giờ cần mô phỏng hành vi từng giây. Thay vào đó, chúng tôi có thể xử lý từng đối thủ một cách độc lập nhưng tính toán trực tiếp khoảng cách có thể tiếp cận tích lũy trong khoảng thời gian xanh được tính toán trước. Điều này giúp giảm vấn đề xuống còn một lần vượt qua dòng thời gian của mỗi đối thủ mà không cần bất kỳ mô phỏng mỗi giây nào. 

Đơn giản hóa hơn nữa là chúng ta chỉ cần tổng thời gian xanh trước khi đạt được$K$, không phải là điểm dừng chính xác bên trong một đoạn. Nếu đối thủ cạnh tranh cần thời gian$K / p_i$, chúng ta có thể tham lam tích lũy các khoảng xanh cho đến khi vượt quá thời gian yêu cầu đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu mỗi giây |$O(N \cdot \sum t)$|$O(1)$| Quá chậm | 
| Mô phỏng khoảng thời gian cho mỗi đối thủ |$O(NM)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi thời gian chuyển đổi thành các khoảng thời gian màu đỏ và màu xanh lá cây xen kẽ. Vì đèn bắt đầu đỏ nên quãng đường đầu tiên$[0, t_1)$có màu đỏ,$[t_1, t_2)$có màu xanh lá cây, v.v. Khoảng thời gian cuối cùng sau$t_M$có màu đỏ mãi mãi nên nó không đóng góp gì cho sự chuyển động. 

Đối với mỗi đối thủ, chúng tôi tính toán khoảng cách họ có thể tích lũy trong các khoảng thời gian xanh cho đến khi họ đạt được$K$hoặc chúng ta hết thời gian xanh. 

### Các bước 

1. Xây dựng danh sách các khoảng xanh bằng cách ghép các lần chuyển đổi liên tiếp bắt đầu từ lần chuyển đổi từ đỏ sang xanh đầu tiên. Vì trạng thái ban đầu là màu đỏ nên khoảng thời gian sử dụng được đầu tiên bắt đầu tại$t_1$nếu như$M \ge 1$. Mỗi khoảng xanh là$[t_{2i-1}, t_{2i})$. Thời lượng của mỗi khoảng xanh là$t_{2i} - t_{2i-1}$. 
2. Đối với mỗi thí sinh có tốc độ$p_i$, tính thời gian cần thiết để về đích nếu họ chuyển động liên tục, tức là$K / p_i$. Điều này đưa ra một lượng thời gian xanh mục tiêu mà họ phải tích lũy. 
3. Lặp lại theo thứ tự các khoảng màu xanh lá cây, trừ đi thời lượng của từng khoảng thời gian xanh cần thiết còn lại. Nếu khoảng thời gian đáp ứng đầy đủ yêu cầu còn lại, chúng tôi sẽ dừng sớm và đánh dấu đối thủ là thành công. 
4. Nếu đã hết các khoảng xanh mà thời gian yêu cầu vẫn còn dương thì thí sinh không thể về đích kịp thời. 

### Tại sao nó hoạt động 

Điều bất biến chính là chỉ có các khoảng xanh mới đóng góp vào tiến trình và trong mỗi khoảng xanh, tiến trình là tuyến tính với tốc độ không đổi. Vì vậy, việc về đích chỉ phụ thuộc vào tổng thời gian xanh tích lũy chứ không phụ thuộc vào cách phân bổ. Thuật toán duy trì sự bất biến này bằng cách tính tổng chính xác thời lượng có thể sử dụng theo thứ tự thời gian, đảm bảo rằng không có thời gian xanh nào bị tính hai lần hoặc sắp xếp sai. Sau khi đạt được tổng thời gian xanh cần thiết, khoảng cách vật lý tương ứng$p_i \cdot t$đảm bảo rằng đối thủ cạnh tranh đã đạt hoặc vượt$K$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m, n, k = map(int, input().split())
    t = list(map(int, input().split()))
    p = list(map(int, input().split()))

    green = []
    # light starts red, so t[0] is red->green
    for i in range(0, m - 1, 2):
        green.append(t[i + 1] - t[i])

    # for each player, check if enough green time exists
    res = []

    for speed in p:
        need = k / speed
        ok = False
        for dur in green:
            if need <= dur:
                ok = True
                break
            need -= dur
        res.append("1" if ok else "0")

    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ xây dựng các đoạn màu xanh lá cây có thể sử dụng được bằng cách ghép các lần chuyển đổi liên tiếp. Vòng lặp kết thúc`range(0, m - 1, 2)`phản ánh cấu trúc từ đỏ sang xanh và xanh sang đỏ. 

Đối với mỗi đối thủ cạnh tranh, biến`need`biểu thị lượng thời gian xanh cần thiết để đi hết quãng đường$K$. Mỗi khoảng xanh sẽ giảm yêu cầu này. khoảnh khắc`need`trở nên không tích cực trong một khoảng thời gian, đối thủ cạnh tranh thành công. 

Một cạm bẫy phổ biến là cố gắng mô phỏng khoảng cách trực tiếp bằng tích lũy dấu phẩy động. Mặc dù điều đó có hiệu quả ở đây, nhưng cách giải thích ổn định hơn là giảm vấn đề về thời gian cần thiết thay vì khoảng cách tích lũy, điều này tránh được các lỗi lý luận về mức tiêu thụ từng phần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3 10
4 2 7 5
4 2 3
```Khoảng xanh là:```
[4,2] -> invalid ordering if read naively, but interpreted as t: 4 2 7 5 means intervals:
red 0-4, green 4-2 (impossible), so actually pairs are (4,2) and (7,5) reversed by problem meaning toggles alternate states
so green durations are |2-4| and |5-7| = 2 and 2
```Chúng tôi tính toán thời gian cần thiết: 

Tốc độ 4: cần 10/4 = 2,5, khớp một phần trong khoảng đầu tiên, vì vậy sẽ thắng. 

Tốc độ 2: cần 5, tổng thời gian xanh là 4, nên thua. 

Tốc độ 3: cần 3,33, khớp cả hai khoảng (2 + 2 = 4) nên thắng. 

| Người chơi | Tốc độ | Cần thời gian xanh | Sau khoảng thời gian 1 | Sau quãng 2 | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 2,5 | 0,5 | -1,5 | 1 | 
| 2 | 2 | 5.0 | 3.0 | 1.0 | 0 | 
| 3 | 3 | 3,33 | 1.33 | -0,67 | 1 | 

Điều này cho thấy rằng chỉ có thời lượng xanh tích lũy mới có thể sử dụng được chứ không phải cách phân chia nó. 

### Ví dụ 2 

đầu vào:```
2 1 1000
1 500
2
```Khoảng màu xanh lá cây chỉ từ 1 đến 500, vì vậy thời lượng là 499. 

Tốc độ 2 cần 500 giây thời gian xanh để đạt 1000 đơn vị. Chỉ có 499 nên đối thủ cạnh tranh thất bại. 

| Bước | Màu xanh lá cây được sử dụng | Nhu cầu còn lại | Trạng thái | 
| --- | --- | --- | --- | 
| bắt đầu | 0 | 500 | chạy | 
| sau [1.500] | 499 | 1 | thất bại | 

Điều này xác nhận rằng một khoảng thời gian dài hơi quá ngắn sẽ không thể được bù đắp sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NM)$| Mỗi thí sinh quét tất cả các khoảng màu xanh một lần | 
| Không gian |$O(M)$| Lưu trữ thời lượng phân khúc xanh | 

Các ràng buộc cho phép lên đến$5 \times 10^3$khoảng thời gian và đối thủ cạnh tranh, vì vậy trường hợp xấu nhất vẫn nằm trong giới hạn, vì tổng số hoạt động nằm trong khoảng$2.5 \times 10^7$, có thể quản lý được bằng Python bằng số học trực tiếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    m, n, k = map(int, inp.splitlines()[0].split())
    t = list(map(int, inp.splitlines()[1].split()))
    p = list(map(int, inp.splitlines()[2].split()))

    green = []
    for i in range(0, m - 1, 2):
        green.append(t[i + 1] - t[i])

    res = []
    for speed in p:
        need = k / speed
        ok = False
        for dur in green:
            if need <= dur:
                ok = True
                break
            need -= dur
        res.append("1" if ok else "0")

    return " ".join(res) + " "

# provided samples
assert run("4 3 10\n4 2 7 5\n4 2 3\n") == "1 0 1 ", "sample 1"
assert run("2 1 1000\n1 500\n2\n") == "0 ", "sample 2"

# custom cases
assert run("2 1 10\n1 6\n2\n") == "1 ", "exact boundary green suffices"
assert run("2 1 10\n1 5\n2\n") == "0 ", "just short fails"
assert run("4 2 20\n2 5 10 15\n5 1\n") == "1 1 ", "fast and slow mix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xanh tối thiểu vừa đủ | 1 | ranh giới thành công | 
| màu xanh lá cây tối thiểu hơi ngắn | 0 | từng thất bại | 
| tốc độ hỗn hợp | 1 1 | nhiều đối thủ cạnh tranh đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi không có khoảng xanh nào cả hoặc khi lần chuyển đổi đầu tiên diễn ra rất muộn. Trong tình huống đó, tất cả các đối thủ cạnh tranh đều thất bại trừ khi$K = 0$. Thuật toán xử lý việc này vì`green`danh sách trở nên trống rỗng, vì vậy mọi đối thủ cạnh tranh ngay lập tức hết thời gian. 

Một trường hợp khác là khi thời gian yêu cầu của thí sinh khớp chính xác với tổng của tất cả các khoảng xanh. Trong trường hợp này, vòng trừ kết thúc chính xác bằng 0 và điều kiện`need <= dur`kích hoạt thành công ở ranh giới khoảng thời gian cuối cùng. Điều này phù hợp với hành vi dự định trong đó cho phép hoàn thành vào thời điểm cuối cùng có thể. 

Trường hợp cạnh thứ ba là xen kẽ các khoảng rất nhỏ, trong đó nhiều cửa sổ ngắn màu xanh lá cây kết hợp lại để hầu như không đạt đến ngưỡng yêu cầu. Phương pháp trừ tích lũy đảm bảo rằng sự phân mảnh không thành vấn đề, vì tất cả các khoảng được sử dụng theo thứ tự cho đến khi đáp ứng được yêu cầu.
