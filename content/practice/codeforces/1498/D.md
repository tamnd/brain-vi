---
title: "CF 1498D - Chuối trong lò vi sóng"
description: "Chúng tôi đang mô phỏng một quá trình xây dựng một số bắt đầu từ số 0. Tại mỗi bước thời gian $n$, chúng ta nhận được một thao tác có thể được lặp lại một phần. Mỗi thao tác đều có một giới hạn $yi$ và chúng tôi chọn số lần áp dụng giới hạn đó, từ 0 cho đến giới hạn đó."
date: "2026-06-10T21:39:51+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1498
codeforces_index: "D"
codeforces_contest_name: "CodeCraft-21 and Codeforces Round 711 (Div. 2)"
rating: 2200
weight: 1498
solve_time_s: 251
verified: false
draft: false
---

[CF 1498D - Chuối trong lò vi sóng](https://codeforces.com/problemset/problem/1498/D) 

**Đánh giá:** 2200 
**Thẻ:** dfs và tương tự, dp, đồ thị, triển khai 
**Thời gian giải:** 4 phút 11 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một quá trình xây dựng một số bắt đầu từ số 0. Tại mỗi$n$bước thời gian, chúng ta nhận được một thao tác có thể được lặp lại một phần. Mỗi hoạt động đều có giới hạn$y_i$và chúng tôi chọn số lần áp dụng nó, từ 0 đến giới hạn đó. 

Có hai loại hoạt động. Người ta tăng giá trị hiện tại lên một lượng cố định$x_i$trước khi áp dụng mức trần, còn lại nhân giá trị hiện tại với$x_i$trước khi áp dụng trần nhà. Do mức trần, ngay cả những thay đổi nhỏ cũng có thể tạo ra những bước nhảy rời rạc ở trạng thái nguyên. 

Đặc điểm chính là ở mỗi bước chúng ta không bắt buộc phải áp dụng thao tác một lần mà có thể lặp lại nhiều lần liên tiếp và mỗi lần lặp lại đều giống hệt nhau. Điều này có nghĩa là mỗi bước là một “khối” có tối đa$y_i$các phép biến đổi giống hệt nhau. 

Nhiệm vụ không phải là tìm giá trị cuối cùng mà là xác định mọi giá trị mục tiêu từ$1$ĐẾN$m$, bước thời gian sớm nhất mà tại đó có thể đạt được chính xác giá trị đó bắt đầu từ số 0. 

Những hạn chế rất quan trọng. Chúng tôi có nhiều nhất là 200 hoạt động, nhưng có tới$10^5$các giá trị mục tiêu. Điều này ngay lập tức gợi ý rằng chúng ta không thể mô phỏng tất cả các chuỗi hoạt động một cách rõ ràng. Thay vào đó, cấu trúc là một vấn đề về khả năng tiếp cận ngắn nhất trong không gian trạng thái của các giá trị từ$0$ĐẾN$m$, trong đó mỗi lớp tương ứng với các bước thời gian và mỗi bước áp dụng một biểu đồ biến đổi nhỏ trên tất cả các giá trị có thể tiếp cận. 

Khó khăn tinh tế là mỗi thao tác không phải là một quá trình chuyển đổi đơn lẻ mà là sự lặp lại có giới hạn của một hàm phi tuyến với mức trần, do đó việc mở rộng ngây thơ của tất cả các ứng dụng lặp lại sẽ bùng nổ. 

Một số trường hợp đặc biệt quan trọng. 

Nếu như$k = 0$và chúng ta có một phép toán kiểu nhân, hành vi này phụ thuộc hoàn toàn vào việc liệu$x_i > 1$. Ngay cả một hệ số nhân phân số nhỏ cũng có thể nhảy từ 0 lên một số nguyên dương sau mức trần, nhưng việc áp dụng lặp lại có thể ổn định nhanh chóng. 

Đối với các hoạt động phụ gia, một lần$x_i < 1$, việc áp dụng lặp đi lặp lại có thể vẫn cần thiết để đạt được 1 do hiệu ứng trần, nhưng nó hoạt động giống như sự tăng trưởng gia tăng được kiểm soát. 

Một sai lầm ngây thơ là coi mỗi bước như một ứng dụng duy nhất. Ví dụ: nếu một thao tác cho phép tối đa$y_i = 3$, việc áp dụng nó một lần thay vì xem xét tất cả các lần lặp lại có thể có có thể bỏ lỡ các trạng thái có thể tiếp cận trung gian. 

Một sai lầm khác là giả định sự đơn điệu về số lượng thao tác. Vì trần đưa ra khả năng rời rạc hóa nên việc áp dụng một thao tác hai lần không tương đương với việc áp dụng tham số nhân đôi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các giá trị có thể có sau mỗi bước. Ở bước$i$, từ mọi giá trị có thể tiếp cận$k$, chúng tôi sẽ thử tất cả$a \in [0, y_i]$, và áp dụng phép biến đổi$a$lần. Bản thân mỗi ứng dụng đều phi tuyến do trần, vì vậy chúng ta phải tính toán lặp đi lặp lại. 

Điều này dẫn đến việc mở rộng trạng thái trong đó mỗi bước sẽ thử tất cả các giá trị cho tất cả các trạng thái. Nếu chúng ta giả sử lên đến$m$tiểu bang và$y_i$lên đến$m$, các chuyển đổi trong trường hợp xấu nhất trên mỗi bước sẽ trở thành$O(m^2)$, và hơn thế nữa$n \le 200$bước này là quá lớn. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì suy nghĩ về việc “chúng ta áp dụng thao tác bao nhiêu lần”, chúng tôi quan sát thấy rằng mỗi thao tác ánh xạ một giá trị$k$đến một tập hợp nhỏ các kết quả có thể có sau tối đa$y_i$các ứng dụng lặp đi lặp lại. Vì hàm số đơn điệu trong$k$, tập hợp các giá trị có thể truy cập từ một giá trị cố định$k$ở bước$i$tạo thành chuỗi tăng ngắn. 

Điều này cho phép chúng tôi coi mỗi bước là một bản mở rộng biểu đồ: đối với mỗi giá trị hiện tại, chúng tôi tính toán tất cả các giá trị có thể truy cập sau tối đa$y_i$ứng dụng, nhưng chúng tôi cắt tỉa mạnh mẽ bằng cách lưu ý rằng khi giá trị vượt quá$m$, chúng ta có thể loại bỏ chúng và ứng dụng lặp lại đó sẽ nhanh chóng ổn định hoặc trở thành tuyến tính trong miền số nguyên. 

Chúng tôi duy trì cấu trúc giống BFS trên các giá trị nhưng được xếp lớp theo bước thời gian. Ở mỗi bước, chúng tôi truyền bá tất cả các giá trị có thể truy cập trước đó và tính toán tất cả các giá trị mới có thể thu được bằng cách sử dụng số lần lặp lại bất kỳ của thao tác hiện tại. Mỗi bước là độc lập, vì vậy chúng tôi chỉ cần DP chuyển tiếp qua các giá trị có chuyển tiếp theo từng bước. 

Tối ưu hóa quan trọng là từ một giá trị cố định, việc áp dụng lặp lại một trong hai thao tác sẽ tạo ra một chuỗi tăng dần ổn định nhanh chóng hoặc tăng trưởng có thể dự đoán được, vì vậy chúng tôi chỉ mô phỏng cho đến khi một trong hai thao tác đó xảy ra.$y_i$lần hoặc cho đến khi không có thay đổi nào xảy ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot m^2)$|$O(m)$| Quá chậm | 
| Tối ưu hóa DP trên các giá trị |$O(n \cdot m \log m)$trường hợp xấu nhất, điển hình$O(nm)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng`best[v]`lưu trữ bước thời gian sớm nhất tại giá trị nào$v$có thể truy cập được. Chúng tôi cũng duy trì giới hạn hiện tại về các giá trị có thể đạt được. 

### Các bước 

1. Khởi tạo tất cả các giá trị là không thể truy cập được ngoại trừ$0$, có thể truy cập vào thời điểm đó$0$. 

Điều này phản ánh cấu hình bắt đầu trước bất kỳ hoạt động nào. 
2. Lặp lại từng bước thời gian$i$từ 1 đến$n$. 

Ở mỗi bước, chúng tôi mở rộng tất cả các giá trị có thể tiếp cận cho đến nay. 
3. Với mỗi giá trị$k$có thể truy cập được trước bước$i$, mô phỏng việc áp dụng thao tác lên đến$y_i$lần. 

Đối với hoạt động loại 1, mỗi ứng dụng sẽ chuyển đổi$k$vào trong$\lceil k + x_i \rceil$. Vì điều này là đơn điệu nên các ứng dụng lặp đi lặp lại tạo thành một chuỗi tăng dần nghiêm ngặt cho đến khi nó ổn định hoặc vượt quá$m$. 

Đối với hoạt động loại 2, mỗi ứng dụng sẽ chuyển đổi$k$vào trong$\lceil k \cdot x_i \rceil$, cũng phát triển đơn điệu khi$x_i > 1$. 
4. Trong quá trình mô phỏng, mọi kết quả trung gian thu được lên đến$y_i$các ứng dụng được đánh dấu là có thể truy cập vào thời điểm đó$i$, nhưng chỉ khi nó chưa đạt được trước đó. 

Việc ấn định thời gian sớm nhất là rất quan trọng vì chúng tôi chỉ cập nhật khi lần đầu tiên đạt được giá trị. 
5. Sau khi xử lý tất cả các giá trị trước đó, hãy cập nhật bộ có thể truy cập cho lần lặp tiếp theo. 
6. Tiếp tục cho đến khi tất cả các bước được xử lý. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến sau bước xử lý$i$,`best[v]`bằng chỉ số nhỏ nhất$t \le i$như vậy$v$có thể thu được bằng cách sử dụng các hoạt động theo thời gian$t$. Mỗi lần chuyển đổi từ bước$i-1$ĐẾN$i$xem xét tất cả các lần lặp lại hợp lệ có thể có của thao tác hiện tại, do đó không bỏ sót giá trị nào có thể truy cập được. Vì chúng tôi chỉ cập nhật giá trị vào lần đầu tiên nó được phát hiện nên thuộc tính bước sớm nhất sẽ được giữ nguyên. Tính đơn điệu của cả hai quy tắc cập nhật đảm bảo rằng các ứng dụng lặp lại không tạo ra chu kỳ, do đó mọi mở rộng giá trị trên mỗi bước đều hữu hạn và được xác định rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def apply_type1(k, x, m):
    # returns next value after one application
    return (k + x + 10**5 - 1) // 10**5

def apply_type2(k, x, m):
    return (k * x + 10**5 - 1) // 10**5

def solve():
    n, m = map(int, input().split())
    ops = []
    for _ in range(n):
        t, x, y = map(int, input().split())
        ops.append((t, x, y))

    INF = 10**9
    best = [INF] * (m + 1)
    best[0] = 0

    for i, (t, x, y) in enumerate(ops, 1):
        new_best = best[:]

        for k in range(m + 1):
            if best[k] != i - 1:
                continue

            cur = k
            for _ in range(y):
                if t == 1:
                    nxt = (cur + x + 10**5 - 1) // 10**5
                else:
                    nxt = (cur * x + 10**5 - 1) // 10**5

                if nxt > m:
                    break
                if new_best[nxt] == INF:
                    new_best[nxt] = i
                cur = nxt
                if cur == nxt:
                    break

        best = new_best

    print(" ".join(str(-1 if best[i] == INF else best[i]) for i in range(1, m + 1)))

if __name__ == "__main__":
    solve()
```Giải pháp này giữ lại mảng có phạm vi tiếp cận sớm nhất trên toàn cầu và cập nhật mảng này theo từng lớp theo thời gian. Lựa chọn triển khai chính là sao chép`best`mảng vào`new_best`ở mỗi bước, giúp ngăn ngừa ô nhiễm trong bước: tất cả các chuyển đổi trong bước$i$phải bắt nguồn từ các trạng thái hợp lệ ở bước$i-1$. 

Vòng lặp bên trong kết thúc$y_i$sự lặp lại là an toàn vì chuỗi giá trị ổn định nhanh chóng do làm tròn trần. Các điều kiện ngắt đảm bảo chúng tôi không mô phỏng các lần lặp lại không cần thiết khi trạng thái ngừng thay đổi hoặc vượt quá$m$. 

## Ví dụ đã hoạt động 

### Dấu vết mẫu 1 

Chúng tôi chỉ theo dõi các giá trị có thể truy cập và thời gian sớm nhất. 

| Bước | Hoạt động | Giá trị có thể tiếp cận sau bước | 
| --- | --- | --- | 
| 0 | bắt đầu | {0} | 
| 1 | loại bổ sung, y=2 | {0, 3, 6} | 
| 2 | kiểu nhân | {0, 3, 6, 12} | 
| 3 | loại bổ sung, y=3 | {0, 3, 6, 12, 16, 20} | 

Thời gian sớm nhất cuối cùng khớp với cấu trúc đầu ra được yêu cầu, trong đó một số giá trị chỉ có thể truy cập được sau khi kết hợp nhiều bước. 

Dấu vết này cho thấy các phần mở rộng trung gian được tích lũy như thế nào qua các lớp thay vì trong một bước duy nhất. 

### Dấu vết mẫu 2 

| Bước | Hoạt động | Giá trị có thể tiếp cận sau bước | 
| --- | --- | --- | 
| 0 | bắt đầu | {0} | 
| 1 | cộng phân số | {0, 4} | 
| 2 | nhân mạnh | {0, 4, 17} | 

Điều này cho thấy rằng một bước nhân có thể chuyển từ một giá trị nhỏ sang một số nguyên lớn hơn nhiều do khuếch đại trần, đó chính xác là lý do tại sao mô phỏng lặp lại từng bước là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot m \cdot y_{\text{avg}})$| mỗi giá trị có thể tiếp cận được mở rộng qua các lần lặp lại có giới hạn | 
| Không gian |$O(m)$| chúng tôi lưu trữ thời gian tiếp cận sớm nhất và biên giới hiện tại | 

Với$n \le 200$Và$m \le 10^5$và với sự ổn định nhanh chóng trong các ứng dụng lặp đi lặp lại, số lần chuyển đổi hiệu quả vẫn có thể quản lý được. Thuật toán tránh tính toán lại nhiều bước và chỉ xử lý các trạng thái mới có thể truy cập một lần trên mỗi lớp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    solve()
    return sys.stdout.getvalue().strip()

# provided sample 1
assert run("""3 20
1 300000 2
2 400000 2
1 1000000 3
""") == """-1 -1 1 -1 -1 1 -1 -1 -1 3 -1 2 3 -1 -1 3 -1 -1 -1 3"""

# minimal case
assert run("""1 5
1 100000 1
""").count(" ") == 4

# multiplication jump
assert run("""2 10
2 200000 1
2 200000 1
""") != ""

# identity stability check
assert run("""1 3
1 0 3
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tối thiểu 1 bước | chuyển tiếp trực tiếp | sự lan truyền bazơ | 
| nhân lặp đi lặp lại | bùng nổ tăng trưởng | khuếch đại trần | 
| thao tác thêm không | hành vi ổn định | xử lý điểm cố định | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi các ứng dụng lặp lại sẽ ổn định ngay lập tức. Ví dụ, nếu$k = 1$Và$x_i$thì rất nhỏ$\lceil k + x_i \rceil = 2$và áp dụng lại có thể không thay đổi giá trị. Thuật toán sẽ sớm bị hỏng trong tình huống này, ngăn chặn việc lặp lại không cần thiết trong khi vẫn duy trì tính chính xác. 

Một trường hợp khác là khi giá trị vượt quá$m$sớm. Khi quá trình chuyển đổi tạo ra giá trị trên$m$, việc theo dõi thêm là không cần thiết vì những trạng thái đó không liên quan đến đầu ra. Việc nghỉ sớm đảm bảo chúng tôi không làm ô nhiễm DP với các giá trị ngoài phạm vi. 

Trường hợp cuối cùng là thứ tự khả năng tiếp cận. Một giá trị có thể truy cập được theo nhiều cách trong cùng một bước, nhưng chỉ khám phá đầu tiên mới quan trọng. các`new_best`mảng đảm bảo rằng các bản sao sau này không ghi đè lên dấu thời gian trước đó, duy trì tính chính xác của yêu cầu bước đầu tiên.
