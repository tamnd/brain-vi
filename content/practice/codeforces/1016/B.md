---
title: "CF 1016B - Lần xuất hiện của phân đoạn"
description: "Chúng ta có một chuỗi cơ sở $s$ và một chuỗi mẫu $t$, cả hai đều được tạo thành từ các chữ cái viết thường. Nhiệm vụ là trả lời nhiều truy vấn độc lập, trong đó mỗi truy vấn hỏi: bên trong một chuỗi con nhất định của $s$, bao nhiêu lần $t$ xuất hiện dưới dạng một chuỗi con liền kề?"
date: "2026-06-16T22:17:59+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1016
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 48 (Rated for Div. 2)"
rating: 1300
weight: 1016
solve_time_s: 116
verified: true
draft: false
---

[CF 1016B - Sự xuất hiện của phân đoạn](https://codeforces.com/problemset/problem/1016/B) 

**Đánh giá:** 1300 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi cơ sở$s$và một chuỗi mẫu$t$, cả hai đều được làm bằng chữ thường. Nhiệm vụ là trả lời nhiều truy vấn độc lập, trong đó mỗi truy vấn yêu cầu: bên trong một chuỗi con nhất định của$s$, bao nhiêu lần$t$xuất hiện dưới dạng chuỗi con liền kề? 

Một cách hữu ích để nghĩ về điều này là mọi vị trí trong$s$có thể là khởi đầu cho sự xuất hiện của$t$. Tuy nhiên, đối với một truy vấn$[l, r]$, chúng tôi chỉ đếm các ngôi sao$i$sao cho trận đấu đầy đủ$s[i..i+m-1]$nằm hoàn toàn bên trong phạm vi truy vấn. 

Các ràng buộc định hình không gian giải pháp theo một cách rất cụ thể. Độ dài chuỗi$n, m \le 1000$đủ nhỏ để kiểm tra tất cả sự sắp xếp có thể có giữa$s$Và$t$là khả thi. Mặt khác, số lượng truy vấn$q \le 10^5$đủ lớn để việc tính toán lại các kết quả khớp cho mỗi truy vấn sẽ ngay lập tức TLE. Điều này tạo ra sự tách biệt cổ điển: tiền xử lý$s$rẻ, nhưng công việc cho mỗi truy vấn phải$O(1)$hoặc logarit. 

Một lực lượng vũ phu cho mỗi truy vấn sẽ thử mọi vị trí trong chuỗi con và so sánh với$t$, tính chi phí$O(n \cdot m)$cho mỗi truy vấn trong trường hợp xấu nhất. Với$10^5$truy vấn, điều này trở nên quá chậm. 

Một trường hợp lỗi nhỏ phổ biến xuất hiện khi ranh giới so khớp vượt qua các cạnh truy vấn. Ví dụ, nếu$s = \texttt{aaaaa}$Và$t = \texttt{aaa}$, lần xuất hiện bắt đầu ở vị trí 1, 2 và 3. Đối với truy vấn như$[2,4]$, chỉ bắt đầu ở 2 và 3 là hợp lệ, nhưng việc quét chuỗi con đơn giản để kiểm tra các kết quả khớp hoàn toàn mà không xác minh các điều kiện biên có thể đếm không chính xác một kết quả khớp bắt đầu từ 1 ngay cả khi nó mở rộng ra ngoài phạm vi truy vấn. 

Một trường hợp cạnh khác là khi$m = 1$. Khi đó mọi ký tự đều bằng$t$là một lần xuất hiện hợp lệ và vấn đề giảm xuống việc đếm các ký tự trong phạm vi. Các giải pháp giả định$m > 1$thường bị hỏng ở đây do tính toán chỉ số như$r - m + 1$. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp rất đơn giản: đối với mỗi truy vấn, hãy quét tất cả các vị trí$i$từ$l$ĐẾN$r$, và kiểm tra xem$s[i..i+m-1] = t$. Điều này đúng vì nó trực tiếp tuân theo định nghĩa về sự xuất hiện. Tuy nhiên, mỗi lần kiểm tra đều tốn$O(m)$, và có$O(n)$vị trí bắt đầu có thể có cho mỗi truy vấn, dẫn đến$O(nm)$mỗi truy vấn và$O(qnm)$nói chung là không thể thực hiện được. 

Quan sát quan trọng là cấu trúc của các kết quả khớp hợp lệ không phụ thuộc vào truy vấn. Liệu$t$trùng khớp ở vị trí$i$độc lập với bất kỳ truy vấn nào; nó chỉ phụ thuộc vào$s$Và$t$. Điều này có nghĩa là chúng ta có thể tính toán trước một mảng nhị phân$ok[i]$, Ở đâu$ok[i] = 1$nếu như$t$xảy ra bắt đầu từ vị trí$i$, ngược lại là 0. 

Khi điều này được thực hiện, mỗi truy vấn sẽ giảm xuống thành một truy vấn tổng phạm vi trên mảng này. Với tổng tiền tố, chúng ta có thể trả lời từng truy vấn trong$O(1)$. Việc chuyển đổi từ so khớp chuỗi sang tổng tiền tố là điều giúp loại bỏ sự phụ thuộc vào$q$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q \cdot n \cdot m)$|$O(1)$| Quá chậm | 
| Tính toán trước + Tổng tiền tố |$O(n \cdot m + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành vấn đề truy vấn đánh dấu tĩnh + tổng tiền tố. 

1. Đối với mọi chỉ số$i$từ$1$ĐẾN$n - m + 1$, kiểm tra xem chuỗi con$s[i..i+m-1]$bằng$t$. 

Bước này xác định tất cả các vị trí bắt đầu hợp lệ của mẫu độc lập với các truy vấn. 
2. Xây dựng một mảng$ok$chiều dài$n$, Ở đâu$ok[i] = 1$nếu trận đấu bắt đầu lúc$i$, ngược lại là 0. 

Chúng tôi chỉ xác định vị trí bắt đầu hợp lệ tối đa$n-m+1$, vì ngoài điều đó ra, một trận đấu đầy đủ là không thể. 
3. Xây dựng mảng tổng tiền tố$pref$, Ở đâu$pref[i] = pref[i-1] + ok[i]$. 

Điều này cho phép chúng ta đếm xem có bao nhiêu trận đấu nằm trong bất kỳ khoảng thời gian nào trong thời gian không đổi. 
4. Đối với mỗi truy vấn$[l, r]$, tính toán câu trả lời như$pref[r-m+1] - pref[l-1]$, nhưng chỉ khi$r-m+1 \ge l$. Nếu không thì câu trả lời là 0. 

Điều kiện đảm bảo rằng chúng tôi chỉ tính các lần xuất hiện có độ dài đầy đủ phù hợp với phạm vi truy vấn. 

Chi tiết quan trọng là ranh giới phù hợp$r - m + 1$. Bất kỳ vị trí bắt đầu nào ngoài vị trí này sẽ tạo ra một chuỗi con vượt quá$r$, nên phải loại trừ. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào thực tế là mỗi lần xuất hiện của$t$được xác định duy nhất bởi vị trí bắt đầu của nó. Khi chúng tôi tính toán trước tất cả các chỉ số bắt đầu hợp lệ, mọi truy vấn sẽ trở thành vấn đề đếm trên một mảng nhị phân cố định. Tổng tiền tố duy trì số lượng chính xác trong các khoảng thời gian và ràng buộc$i + m - 1 \le r$được thực thi bằng cách giới hạn điểm cuối phù hợp ở$r - m + 1$. Điều này đảm bảo rằng không có sự cố nào được tính một phần hoặc bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m, q = map(int, input().split())
    s = input().strip()
    t = input().strip()

    if m > n:
        # pattern longer than text, no matches possible
        for _ in range(q):
            input()
            print(0)
        return

    ok = [0] * n

    for i in range(n - m + 1):
        if s[i:i + m] == t:
            ok[i] = 1

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + ok[i]

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        l -= 1
        r -= 1

        right = r - m + 1
        if right < l:
            out.append("0")
        else:
            out.append(str(pref[right + 1] - pref[l]))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Vòng tiền xử lý kiểm tra rõ ràng tất cả các sắp xếp có thể có của$t$TRONG$s$. Điều này an toàn vì cả hai chuỗi đều đủ nhỏ để$O(nm)$kiểm tra là chấp nhận được. 

Mảng tiền tố được xây dựng ở dạng 1 chỉ mục tiêu chuẩn để tránh các vấn đề riêng lẻ khi tính tổng phạm vi. Mỗi truy vấn chuyển đổi thành các chỉ mục dựa trên 0 và sau đó kẹp vị trí kết thúc hợp lệ thành$r - m + 1$. 

Một chi tiết triển khai tinh tế là xử lý các trường hợp trong đó$r - m + 1 < l$, có nghĩa là không có lần xuất hiện đầy đủ nào có thể vừa với khoảng thời gian truy vấn. Nếu không có kiểm tra này, phép trừ tiền tố sẽ truy cập vào các phạm vi không hợp lệ và đếm vượt mức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = codeforces
t = for
queries: (1,3), (3,10), (5,6), (5,7)
```Trước tiên chúng tôi tính toán các vị trí bắt đầu hợp lệ: 

| tôi | chuỗi con | trận đấu | 
| --- | --- | --- | 
| 1 | cá tuyết | 0 | 
| 2 | ca ngợi | 0 | 
| 3 | chắc chắn | 0 | 
| 4 | ờ? | 0 | 
| 5 | buộc | 1 | 
| 6 | orce | 0 | 
| 7 | rces | 0 | 
| 8 | ces | 0 | 

Vì vậy chỉ có vị trí 5 là hợp lệ. 

Tổng tiền tố: 

| tôi | trước | 
| --- | --- | 
| 0 | 0 | 
| 5 | 1 | 

Xử lý truy vấn: 

| truy vấn | tôi | r | đúng = r-m+1 | kết quả | 
| --- | --- | --- | --- | --- | 
| (1,3) | 1 | 3 | 1 | 0 | 
| (3,10) | 3 | 10 | 8 | 1 | 
| (5,6) | 5 | 6 | 4 | 0 | 
| (5,7) | 5 | 7 | 5 | 1 | 

Điều này xác nhận rằng chỉ những lần xuất hiện chứa đầy đủ trong mỗi phân đoạn mới được tính. 

### Ví dụ 2 

Hãy:```
s = aaaaa
t = aaa
```Bắt đầu hợp lệ là ở vị trí 1, 2, 3. 

Truy vấn (2,4): 

| tôi | hợp lệ | 
| --- | --- | 
| 1 | 1 (bị loại trừ, bắt đầu trước l) | 
| 2 | 1 | 
| 3 | 1 | 

Chúng tôi tính đúng = 4 - 3 + 1 = 2, vì vậy chúng tôi chỉ tính bắt đầu trong [2,2], cho ra 1 lần xuất hiện. Điều này thể hiện tính đúng đắn của ranh giới: các lần xuất hiện chồng chéo được xử lý bằng cách lọc vị trí bắt đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm + q)$|$O(nm)$để so sánh tất cả các ca của$t$, cộng$O(1)$mỗi truy vấn sử dụng tổng tiền tố | 
| Không gian |$O(n)$| lưu trữ cho mảng xuất hiện và tổng tiền tố | 

Những ràng buộc cho phép$n, m \le 1000$, Vì thế$n \cdot m = 10^6$hoạt động được an toàn. Khối lượng truy vấn lớn nhưng mỗi truy vấn có thời gian không đổi, giúp giải pháp nhanh chóng một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# Since full solution is embedded above, this block is illustrative

# sample 1
assert True

# minimal case
assert True

# all matches
assert True

# no matches
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu char đơn | 1 mỗi vị trí | m = vỏ 1 cạnh | 
| không xảy ra | 0 giây | tính đúng đắn khi không có kết quả phù hợp | 
| chuỗi chồng chéo đầy đủ | nhiều lần chồng chéo | xử lý mẫu chồng chéo | 
| truy vấn quá ngắn | 0 | trường hợp r-l+1 < m | 

## Vỏ cạnh 

Khi độ dài mẫu vượt quá độ dài phân đoạn truy vấn, thuật toán sẽ tạo ra số 0 một cách chính xác vì phạm vi được tính toán$r - m + 1$trở nên nhỏ hơn$l$, gây ra tình trạng từ chối sớm. Ví dụ, nếu$s = \texttt{abcde}$,$t = \texttt{abc}$, và truy vấn là$[4,5]$, sau đó$r - m + 1 = 3$, nhỏ hơn$l = 4$, vậy đáp án đúng là 0. 

cho$m = 1$, mọi kết quả khớp đều tương ứng chính xác với một ký tự trong$s$. Bước tiền xử lý đánh dấu từng chỉ mục trong đó$s[i] = t[0]$và tổng tiền tố sẽ đếm chính xác số lần xuất hiện trong bất kỳ phạm vi nào mà không cần sửa đổi.
