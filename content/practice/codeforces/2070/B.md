---
title: "CF 2070B - Chương trình Robot"
description: "Chúng tôi đang mô phỏng một robot một chiều di chuyển dọc theo một đường số nguyên. Robot bắt đầu ở vị trí $x$, vị trí này có thể âm hoặc dương nhưng ban đầu không bao giờ bằng 0."
date: "2026-06-08T06:55:41+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2070
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 175 (Rated for Div. 2)"
rating: 1100
weight: 2070
solve_time_s: 97
verified: true
draft: false
---

[CF 2070B - Chương trình Robot](https://codeforces.com/problemset/problem/2070/B) 

**Đánh giá:** 1100 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một robot một chiều di chuyển dọc theo một đường số nguyên. Robot xuất phát tại vị trí$x$, có thể âm hoặc dương nhưng ban đầu không bao giờ bằng 0. Nó được đưa ra một chuỗi các lệnh có độ dài cố định$n$, trong đó mỗi lệnh dịch nó sang trái hoặc sang phải đúng một đơn vị. 

Robot thực hiện các lệnh theo thứ tự, mỗi lệnh một giây. Điều quan trọng là bất cứ khi nào nó đạt đến vị trí 0, con trỏ thực thi sẽ đặt lại về lệnh đầu tiên của chuỗi và robot sẽ tiếp tục lại từ đó. Nếu nó hoàn thành toàn bộ chuỗi lệnh mà không về 0 thì quá trình sẽ dừng hoàn toàn. 

Nhiệm vụ không phải là mô phỏng các vị trí trực tiếp trên$k$giây, kể từ khi$k$có thể cực kỳ lớn, nhưng thay vào đó, hãy đếm số lần robot hạ cánh chính xác về số 0 trong khoảng thời gian đó. 

Các ràng buộc ngay lập tức loại trừ mô phỏng ngây thơ. Mô phỏng từng bước trực tiếp là$O(k)$, và kể từ đó$k$có thể lên đến$10^{18}$, ngay cả một trường hợp thử nghiệm duy nhất cũng không thể thực hiện được. Thay vào đó, giải pháp phải hoạt động theo chu kỳ của chuỗi lệnh. 

Một khó khăn nhỏ là quá trình này không diễn ra theo chu kỳ một cách đơn giản vì việc đặt lại phụ thuộc vào việc đạt đến số 0 chứ không phụ thuộc vào việc hoàn thành chuỗi. 

Một vài trường hợp đặc biệt nêu bật lý do tại sao lý luận ngây thơ lại thất bại. Đầu tiên, hãy xem xét một dãy không bao giờ đạt đến 0 từ một vị trí bắt đầu nào đó. Trong trường hợp đó, robot có thể hoàn thành trình tự sớm và dừng lại, do đó chu kỳ đếm sẽ đánh giá quá cao. 

Thứ hai, hãy xem xét trình tự trong đó robot đạt đến số 0 nhiều lần trong một lần vượt qua. Mỗi lần truy cập như vậy sẽ đặt lại quá trình thực thi, nghĩa là chúng tôi không bao giờ thực sự hoàn thành chuỗi đầy đủ trong những trường hợp đó và lý do về mặt số lần chạy đầy đủ sẽ trở nên không hợp lệ. 

Thứ ba, vị trí bắt đầu là âm hay dương sẽ thay đổi cho dù lần truy cập đầu tiên xảy ra sớm hay muộn, vì vậy chúng ta không thể giả định tính đối xứng nếu không theo dõi hành vi tiền tố. 

## Phương pháp tiếp cận 

Một mô phỏng brute-force theo đúng nghĩa đen sẽ thực thi một lệnh mỗi giây, cập nhật vị trí, giảm dần$k$và đặt lại con trỏ lệnh bất cứ khi nào đạt đến số 0. Điều này đúng, nhưng mỗi lần đặt lại có thể đưa chúng ta quay lại phần đầu của chuỗi và trong trường hợp xấu nhất, chúng ta có thể duyệt đi duyệt lại các tiền tố của chuỗi. Với$k$lên đến$10^{18}$, phương pháp này hoàn toàn không khả thi. 

Quan sát quan trọng là quá trình này được xác định hoàn toàn bởi hai trạng thái: vị trí hiện tại và chỉ mục hiện tại trong chuỗi lệnh. Tuy nhiên, việc theo dõi tất cả các trạng thái có thể xảy ra theo thời gian là không cần thiết. Thay vào đó, chúng tôi chỉ quan tâm đến việc mất bao lâu để đạt đến số 0 bắt đầu từ một trạng thái nhất định và điều gì xảy ra ngay sau lần đặt lại đó. 

Từ vị trí bắt đầu cố định ở đầu chuỗi, chúng ta có thể tính toán trước khi robot lần đầu tiên đạt đến số 0. Nếu nó không bao giờ đạt đến 0 trước khi hết chuỗi thì quá trình sẽ kết thúc ngay lập tức. Ngược lại, giả sử lần đầu tiên nó đạt tới 0 là ở bước$t$. Sau đó chúng tôi đã tiêu thụ$t$giây và nhận được một sự kiện đặt lại. Sau khi đặt lại, robot khởi động lại ở vị trí 0 với toàn bộ chuỗi lần nữa. 

Điều này làm giảm vấn đề thành hai giai đoạn. Đầu tiên, tính lần đầu tiên bắt đầu từ$x$khi chúng ta chạm tới con số 0. Thứ hai, tính toán lần đầu tiên bắt đầu từ số 0 khi chúng ta chạm lại số 0 trong khi chạy chuỗi. Giai đoạn thứ hai trở thành một trình tạo chu trình: một khi ở mức 0, mỗi khi chúng ta trở về 0, chúng ta khởi động lại giống hệt nhau, do đó mô hình lặp lại. 

Do đó, giải pháp rút gọn thành việc tính toán hai giá trị bằng cách sử dụng mô phỏng tiền tố: 

một là lần trúng đầu tiên từ$x$và thời gian còn lại là thời điểm quay về 0 đầu tiên kể từ$0$. Sau đó, chúng tôi nhận được 0 lần xuất hiện, một lần xuất hiện hoặc số chu kỳ lặp lại bằng cách sử dụng số học trên$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(k)$|$O(1)$| Quá chậm | 
| Tiền tố + Phân tích chu kỳ |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi chuỗi lệnh thành mảng dịch chuyển tiền tố để có thể tính toán vị trí một cách nhanh chóng. 

1. Tính toán độ dịch chuyển tiền tố của chuỗi lệnh, trong đó mỗi L đóng góp -1 và mỗi R đóng góp +1. Điều này cho chúng tôi biết vị trí sau bất kỳ tiền tố nào của chuỗi mà không cần mô phỏng. 
2. Bắt đầu từ vị trí ban đầu$x$, quét qua các tổng tiền tố và tìm chỉ số sớm nhất$i$như vậy$x + prefix[i] = 0$. Điều này thể hiện khoảnh khắc đầu tiên robot chạm tới số 0 nếu nó từng chạm đến điểm 0 trong lần chạy đầu tiên. 
3. Nếu không có chỉ mục đó tồn tại trong$n$các bước, rô-bốt không bao giờ đặt lại và sẽ chỉ thực hiện cho đến khi hoàn thành hoặc hết thời gian$k$hết. Trong trường hợp này, câu trả lời là 0 hoặc 1 tùy thuộc vào việc đường dẫn bắt đầu có chạm đến 0 hay không, nhưng vì không có quá trình đặt lại nào xảy ra nên chúng tôi chỉ cần dừng đếm sau khi lần chạy đầu tiên kết thúc. 
4. Nếu cú ​​đánh đầu tiên xảy ra vào thời điểm$t_1$, chúng tôi tăng câu trả lời lên 1 và giảm$k$qua$t_1$. Hệ thống bây giờ khởi động lại từ vị trí 0 với chuỗi lệnh đầy đủ. 
5. Bây giờ mô phỏng từ vị trí 0 và tìm lại lần đầu tiên$t_2$sao cho tiền tố đạt tới số không. Nếu không như vậy$t_2$tồn tại, không có lần đặt lại nào xảy ra nữa, vì vậy chúng tôi trả về câu trả lời hiện tại. 
6. Nếu như vậy$t_2$tồn tại thì mọi$t_2$giây chúng tôi nhận được một thiết lập lại khác. Chúng tôi thêm$\lfloor k / t_2 \rfloor$để trả lời và kết thúc. 

Lý do mức giảm này hợp lệ là khi robot chạm đến 0, trạng thái sau khi đặt lại luôn giống hệt nhau: cùng vị trí bắt đầu, cùng con trỏ lệnh và cùng một trình tự. Điều này làm cho hành vi trong tương lai có tính tuần hoàn với khoảng thời gian cố định.$t_2$, do đó việc đếm trở thành một bài toán chia đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x, k = map(int, input().split())
        s = input().strip()

        # prefix displacement
        pref = []
        cur = 0
        first_hit_from_x = -1

        for i, c in enumerate(s):
            if c == 'L':
                cur -= 1
            else:
                cur += 1
            pref.append(cur)

            if first_hit_from_x == -1 and x + cur == 0:
                first_hit_from_x = i + 1

        if first_hit_from_x == -1 or first_hit_from_x > k:
            print(0)
            continue

        # first reset
        ans = 1
        k -= first_hit_from_x

        # now start from 0
        cycle = -1
        for i, v in enumerate(pref):
            if v == 0:
                cycle = i + 1
                break

        if cycle == -1:
            print(ans)
            continue

        ans += k // cycle
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào sự dịch chuyển tiền tố để tránh mô phỏng lặp lại. Biến`cur`duy trì chuyển động ròng sau mỗi tiền tố của chuỗi lệnh. Điều này cho phép chúng tôi kiểm tra khả năng tiếp cận bằng 0 trong thời gian không đổi trên mỗi bước. 

Vòng lặp đầu tiên tính toán cả mảng tiền tố và thời điểm sớm nhất robot đạt đến 0 bắt đầu từ$x$. Điều này rất quan trọng vì phân đoạn đầu tiên hoạt động khác với tất cả các phân đoạn sau do điểm bắt đầu khác 0. 

Sau lần đặt lại đầu tiên, hệ thống luôn khởi động lại từ vị trí 0. Đó là lý do tại sao vòng lặp thứ hai chỉ kiểm tra khi tiền tố trở thành 0 mà không thêm$x$. 

Một điểm tinh tế là chúng tôi chỉ tính số lần đặt lại đầy đủ trong thời gian còn lại. Nếu thời gian còn lại nhỏ hơn độ dài chu kỳ, phép chia số nguyên sẽ xử lý chính xác từng phần chu kỳ một cách tự nhiên. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

### Ví dụ 1 

đầu vào:```
3 2 6
LLR
```Giá trị tiền tố: 

| bước | di chuyển | vị trí từ x=2 | nhấn số 0 | 
| --- | --- | --- | --- | 
| 1 | L | 1 | không | 
| 2 | L | 0 | vâng | 
| 3 | R | 1 | không liên quan | 

Lần truy cập đầu tiên xảy ra ở thời điểm thứ 2, do đó một lần đặt lại xảy ra và$k = 4$. 

Bây giờ bắt đầu từ con số 0: 

| bước | tiền tố | nhấn số 0 | 
| --- | --- | --- | 
| 1 | -1 | không | 
| 2 | -2 | không | 
| 3 | -1 | không | 

Không đạt được số 0 nào nữa nên đáp án vẫn là 1. 

Điều này thể hiện trường hợp chỉ có phân đoạn ban đầu đóng góp. 

### Ví dụ 2 

đầu vào:```
2 -1 8
RL
```Từ$x=-1$: 

| bước | di chuyển | vị trí | 
| --- | --- | --- | 
| 1 | R | 0 | 
| 2 | L | -1 | 

Chúng tôi đạt 0 ở bước 1, vì vậy hãy đặt lại một lần. 

Từ số không: 

| bước | tiền tố | vị trí | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 0 | 0 | 

Độ dài chu kỳ là 2, vì vậy sau khi tiêu tốn 8 giây, chúng ta nhận được tổng cộng 4 lượt truy cập. 

Điều này cho thấy cấu trúc định kỳ sau lần đặt lại đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi bài kiểm tra xử lý tổng tiền tố một hoặc hai lần | 
| Không gian |$O(1)$thêm | Chỉ sử dụng bộ tích lũy tiền tố và bộ đếm | 

Tổng của$n$qua các bài kiểm tra được giới hạn bởi$2 \cdot 10^5$, do đó việc quét tuyến tính cho mỗi lần kiểm tra là đủ trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, x, k = map(int, input().split())
            s = input().strip()

            pref = []
            cur = 0
            first_hit = -1

            for i, c in enumerate(s):
                cur += -1 if c == 'L' else 1
                pref.append(cur)
                if first_hit == -1 and x + cur == 0:
                    first_hit = i + 1

            if first_hit == -1 or first_hit > k:
                print(0)
                continue

            ans = 1
            k -= first_hit

            cycle = -1
            for i, v in enumerate(pref):
                if v == 0:
                    cycle = i + 1
                    break

            if cycle == -1:
                print(ans)
            else:
                print(ans + k // cycle)

    solve()
    return ""

# provided samples (format placeholders omitted for brevity)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1 1 1\nL`|`1`| đánh ngay ở bước 1 | 
|`1\n2 1 10\nRR`|`0`| không bao giờ đạt tới số 0 | 
|`1\n3 1 100\nLRL`|`multiple or zero depending`| phát hiện chu kỳ sớm | 
|`1\n5 -2 1000000000000\nRRLLL`| số lượng lớn | đếm an toàn tràn | 

## Vỏ cạnh 

Trường hợp key edge là khi robot không bao giờ đạt tới 0 ngay cả khi bắt đầu từ vị trí bắt đầu. Trong tình huống đó, vòng lặp đầu tiên không bao giờ tìm thấy tiền tố hợp lệ, vì vậy câu trả lời ngay lập tức là 0. Ví dụ, nếu$x=3$và trình tự chỉ di chuyển sang phải, không bao giờ xảy ra việc thiết lập lại và quá trình thực thi kết thúc sớm. 

Một trường hợp khác là khi robot đạt tới 0 trong lần chạy một phần đầu tiên nhưng không bao giờ quay lại từ 0 nữa. Trong trường hợp đó, chúng tôi chỉ tính một lần đặt lại. Thuật toán xử lý việc này vì`cycle`vẫn còn -1, ngăn cản sự phân chia. 

Trường hợp thứ ba là khi độ dài chu kỳ là 1, nghĩa là tiền tố ngay lập tức trở về 0 từ 0 chỉ trong một bước. Thuật toán đếm chính xác mỗi giây còn lại là một lần đặt lại, vì phép chia số nguyên cho 1 sẽ tích lũy tất cả thời gian còn lại. 

Những trường hợp này xác nhận rằng cả logic lần truy cập đầu tiên và nén chu kỳ đều hoạt động chính xác trên các mẫu lệnh suy biến.
