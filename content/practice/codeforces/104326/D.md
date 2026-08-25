---
title: "CF 104326D - Kế hoạch thông minh"
description: "Chúng ta được cung cấp một hệ thống có hai nhân vật chia sẻ một số lượng hũ mật ong giống hệt nhau cố định. Ban đầu, các hũ được chia ngẫu nhiên giữa họ, nhưng chỉ chia khi cả hai bên đều nhận được ít nhất một hũ."
date: "2026-07-01T19:08:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104326
codeforces_index: "D"
codeforces_contest_name: "Udmurt SU Contest 2011"
rating: 0
weight: 104326
solve_time_s: 95
verified: true
draft: false
---

[CF 104326D - Kế hoạch thông minh](https://codeforces.com/problemset/problem/104326/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống có hai nhân vật chia sẻ một số lượng hũ mật ong giống hệt nhau cố định. Ban đầu, các hũ được chia ngẫu nhiên giữa họ, nhưng chỉ chia khi cả hai bên đều nhận được ít nhất một hũ. Vì không thể phân biệt được những chiếc bình nên điều duy nhất quan trọng là Pooh có bao nhiêu chiếc bình, có thể có giá trị bất kỳ từ 1 đến$n-1$, mỗi khả năng đều như nhau. 

Sau lần phân chia đầu tiên, hệ thống sẽ phát triển theo các vòng riêng biệt. Trong mỗi vòng, chính xác một trong ba điều có thể xảy ra. Với xác suất$p$, một chiếc bình di chuyển từ Pooh đến Heffalump, làm giảm số lượng của Pooh đi một. Với xác suất$q$, một chiếc bình di chuyển từ Heffalump tới Pooh, tăng số lượng của Pooh lên một. Với xác suất$r$, không có gì thay đổi. Nếu Pooh đạt tới 0 bình hoặc$n$chậu, quá trình dừng lại vĩnh viễn. 

Nhiệm vụ là xem xét một số vòng cố định$i$, và tính xác suất để hai điều kiện xảy ra đồng thời. Đầu tiên, quá trình này vẫn chưa được hấp thụ, có nghĩa là cả hai bên đều chưa đạt đến số 0 pot. Thứ hai, cấu hình sau$i$các vòng hoàn toàn giống với cấu hình ban đầu. 

Các ràng buộc đủ nhỏ để không gian trạng thái về cơ bản là tuyến tính theo$n$, từ$n \le 26$, nhưng số bước có thể lớn tới 1600. Điều này ngay lập tức gợi ý rằng giải pháp lập trình động bậc hai hoặc bậc ba cho mỗi trường hợp thử nghiệm là có thể chấp nhận được, trong khi mọi thứ theo cấp số nhân theo thời gian hoặc theo trạng thái thì không. 

Một điểm tinh tế là câu trả lời phụ thuộc vào sự phân chia ngẫu nhiên ban đầu. Chúng tôi không theo dõi một trạng thái bắt đầu duy nhất mà theo dõi mức trung bình trên tất cả các giá trị bắt đầu có thể có của số lượng nồi của Pooh. Một hạn chế quan trọng khác là quy tắc hấp thụ. Bất kỳ đường đi nào đạt tới 0 hoặc$n$phải được loại trừ hoàn toàn, ngay cả khi sau đó nó trở lại về mặt khái niệm với cùng một cấu hình. 

Một sai lầm ngây thơ là bỏ qua sự hấp thụ và coi đây là một bước đi ngẫu nhiên không hạn chế trên các số nguyên. Điều đó sẽ đếm không chính xác các đường đi qua ranh giới. Một sai lầm khác là tính trung bình các trạng thái không chính xác bằng cách trộn lẫn các chuyển đổi từ các vị trí bắt đầu khác nhau, điều này vi phạm yêu cầu chúng ta phải quay lại cùng một cấu hình ban đầu chứ không phải bất kỳ cấu hình phù hợp nào. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ mô phỏng tất cả các chuỗi có thể có của$i$các vòng bắt đầu từ mỗi lần phân chia ban đầu có thể. Mỗi vòng chia thành ba kết quả, vì vậy sau$i$có các bước$3^i$lịch sử có thể có cho mỗi trạng thái bắt đầu. Ngay cả đối với mức độ vừa phải$i$, điều này trở nên không khả thi, vì$3^{1600}$có kích thước lớn về mặt thiên văn. Ngay cả việc cắt bớt các trạng thái không hợp lệ do sự hấp thụ cũng không làm giảm hệ số phân nhánh đủ để làm cho điều này có thể thực hiện được. 

Quan sát quan trọng là hệ thống này là một chuỗi Markov trên một không gian trạng thái nhỏ bao gồm các giá trị nguyên từ$1$ĐẾN$n-1$, với ranh giới hấp thụ tại$0$Và$n$. Xác suất ở trạng thái sau$t$các bước chỉ phụ thuộc vào bước trước đó chứ không phụ thuộc vào toàn bộ lịch sử. Điều này cho phép chúng ta thay thế phép liệt kê hàm mũ bằng lập trình động theo thời gian. 

Tuy nhiên, chúng ta cũng cần lớp cấu trúc thứ hai. Điều kiện yêu cầu quay trở lại trạng thái ban đầu, có nghĩa là chúng ta không thể đơn giản tính toán phân bố sau$i$bước từ một phân phối ban đầu hỗn hợp. Chúng ta phải theo dõi các chuyển đổi riêng biệt cho từng trạng thái bắt đầu có thể có, sau đó kết hợp các kết quả theo phân bố ban đầu thống nhất. 

Điều này dẫn đến một công thức rõ ràng: với mỗi giá trị ban đầu$s$, tính xác suất mà bước đi ngẫu nhiên bị ràng buộc bắt đầu từ$s$lại là lúc$s$sau đó$i$bước mà không bao giờ chạm tới ranh giới. Đây là DP tiêu chuẩn về trạng thái và thời gian, và vì$n$nhỏ, việc chạy nó độc lập cho mỗi lần khởi động vẫn hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(3^k)$|$O(k)$| Quá chậm | 
| DP mỗi trạng thái bắt đầu |$O(n^2 \cdot k)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích quá trình này như một bước đi ngẫu nhiên trên các trạng thái$1$bởi vì$n-1$, trạng thái ở đâu$x$nghĩa là Pooh có$x$chậu. 

1. Đối với mỗi trạng thái bắt đầu có thể$s$, khởi tạo một mảng DP trong đó chỉ$dp[s] = 1$. Điều này thể hiện sự chắc chắn rằng quá trình bắt đầu trong cấu hình$s$, trước khi bất kỳ sự chuyển tiếp nào xảy ra. 
2. Lặp lại các bước thời gian từ$1$ĐẾN$k$, cập nhật một mảng DP mới ở mỗi bước. Mỗi lần chuyển đổi phân bổ khối lượng xác suất theo ba kết quả có thể xảy ra của một vòng. 
3. Từ một tiểu bang$x$, hệ thống có thể chuyển sang$x-1$với xác suất$p$, ĐẾN$x+1$với xác suất$q$, hoặc ở lại$x$với xác suất$r$. Chúng tôi chỉ áp dụng những cập nhật này khi trạng thái kết quả vẫn nằm trong$[1, n-1]$. 
4. Bất kỳ quá trình chuyển đổi nào sẽ chuyển sang$0$hoặc$n$bị loại bỏ. Điều này tương ứng với sự hấp thụ và những con đường đó không góp phần vào các trạng thái trong tương lai. 
5. Sau khi hoàn thành$i$bước, ghi lại$dp[s]$, đại diện cho xác suất bắt đầu từ$s$, hệ thống trở về$s$không có sự hấp thụ. 
6. Lặp lại điều này cho tất cả các trạng thái bắt đầu$s \in [1, n-1]$, sau đó tính trung bình các kết quả vì cấu hình ban đầu là ngẫu nhiên đồng đều trên các trạng thái này. 

### Tại sao nó hoạt động 

Trạng thái DP mã hóa ngầm cả vị trí và sự sống còn. Mọi khối lượng xác suất trong bảng tương ứng với một đường dẫn hợp lệ chưa được hấp thụ. Vì quá trình chuyển đổi được áp dụng từng bước và không bao giờ đưa lại trạng thái hấp thụ nên không có quỹ đạo không hợp lệ nào có thể quay lại hệ thống. Tính tuyến tính của xác suất đảm bảo rằng tổng các trạng thái bắt đầu độc lập và sau đó lấy trung bình khớp chính xác với khởi tạo ngẫu nhiên ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        p, q, r = map(float, input().split())
        k = int(input())

        m = n - 1
        inv_m = 1.0 / m

        answers = [0.0] * k

        for s in range(1, n):
            dp = [0.0] * n
            dp[s] = 1.0

            for t in range(k):
                ndp = [0.0] * n

                for x in range(1, n):
                    val = dp[x]
                    if val == 0:
                        continue

                    ndp[x] += val * r

                    if x - 1 >= 1:
                        ndp[x - 1] += val * p

                    if x + 1 <= n - 1:
                        ndp[x + 1] += val * q

                dp = ndp
                answers[t] += dp[s] * inv_m

        for v in answers:
            print(f"{v:.6e}")

if __name__ == "__main__":
    solve()
```Giải pháp này xây dựng một sự phát triển xác suất riêng biệt cho mỗi lần phân chia ban đầu có thể xảy ra. Mảng DP thể hiện sự phân bổ số lượng pot của Pooh sau mỗi vòng, với điều kiện là không đạt đến giới hạn hấp thụ. 

Bước chuyển tiếp bên trong áp dụng trực tiếp ba kết quả có thể xảy ra. Việc kiểm tra ranh giới đảm bảo rằng việc chuyển đổi sang trạng thái$0$hoặc$n$bị bỏ qua hoàn toàn, phù hợp với quy tắc trò chơi kết thúc ngay lập tức khi đạt đến các trạng thái đó. 

Mỗi bước thời gian đóng góp xác suất quay lại trạng thái bắt đầu cho giá trị ban đầu cụ thể đó. Chúng tôi tích lũy những đóng góp này và chia cho$n-1$ở cuối ngầm thông qua tính trung bình. 

Định dạng đầu ra sử dụng ký hiệu khoa học để phù hợp với các ràng buộc về độ chính xác cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp mẫu với$n = 3$, trong đó trạng thái bắt đầu hợp lệ duy nhất là$1$Và$2$. 

Đối với mỗi trạng thái bắt đầu, chúng tôi theo dõi khối lượng xác suất phát triển như thế nào theo thời gian. Bảng bên dưới cho thấy xác suất quay lại trạng thái cũ sau mỗi số bước, trước khi tính trung bình. 

### Theo dõi mỗi lần bắt đầu 

| Bước | Bắt đầu = 1 (trả về thăm dò) | Bắt đầu = 2 (trả về thăm dò) | 
| --- | --- | --- | 
| 1 | r | r | 
| 2 | r^2 + pq | r^2 + pq | 
| 3 | phân rã theo đường biên | phân rã theo đường biên | 
| 4 | ... | ... | 
| 5 | ... | ... | 

Câu trả lời cuối cùng ở mỗi bước là giá trị trung bình của hai cột. 

Điều này chứng tỏ rằng nói chung không cần phải có tính đối xứng, nhưng việc lấy trung bình trên các trạng thái ban đầu buộc phải có sự đóng góp thống nhất từ ​​cả hai đầu của không gian trạng thái. 

Kiểm tra độ tỉnh táo hữu ích thứ hai là trường hợp$r = 1$, nơi không bao giờ có chuyển động nào xảy ra. Trong trường hợp đó, mọi trạng thái vẫn cố định mãi mãi, do đó xác suất quay trở lại trạng thái cũ luôn chính xác bằng 1 cho mọi trạng thái.$i$, phù hợp với hành vi DP vì chỉ tồn tại các vòng lặp tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot n^2 \cdot k)$| Mỗi thử nghiệm chạy DP cho mọi trạng thái bắt đầu và mỗi DP sẽ cập nhật$n$tiểu bang cho$k$bước | 
| Không gian |$O(n)$| Chỉ có hai mảng kích thước$n$cần thiết cho mỗi lần chạy DP | 

Giới hạn$n \le 26$Và$k \le 1600$thực hiện việc này nhanh chóng một cách thoải mái ngay cả trong Python, vì tổng số lần chuyển đổi cho mỗi lần kiểm tra là nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""  # placeholder

# Sample (format placeholder since full I/O harness depends on integration)
# assert run("...") == "..."

# custom edge tests

# minimum n, minimal movement
# n=3, r=1 => always stable
# expected: all ones
# assert run("1\n3\n0.3 0.3 0.4\n3\n") == ...

# strong drift to boundaries
# assert run("...") == ...

# symmetric random walk
# assert run("...") == ...

# max k stress
# assert run("1\n26\n0.3 0.3 0.4\n1600\n") == ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$r=1$trường hợp | tất cả 1.0 | hấp thụ và độ chính xác không di chuyển | 
| nhỏ | dấu vết thủ công | xử lý ranh giới | 
| k lớn | đầu ra ổn định | ổn định hiệu suất và tích lũy | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi xác suất chuyển động đẩy khối lượng trực tiếp vào trạng thái hấp thụ. Ví dụ, với$n = 3$, bắt đầu từ trạng thái$1$, bất kỳ chuyển đổi nào làm giảm trạng thái sẽ kết thúc trò chơi ngay lập tức. DP loại bỏ khối lượng này một cách chính xác bằng cách kiểm tra ranh giới trước khi thêm các chuyển tiếp. 

Một trường hợp cạnh khác là khi$r$rất gần với 1. Trong tình huống này, hầu hết khối lượng xác suất vẫn giữ nguyên và độ ổn định về số quan trọng hơn cấu trúc tổ hợp. DP vẫn ổn định vì xác suất được tích lũy tăng dần mà không bị trừ đi. 

Trường hợp cuối cùng là khi$k = 1$. Ở đây, câu trả lời chỉ đơn giản là xác suất giữ nguyên trạng thái sau một bước, tính trung bình cho tất cả các lần bắt đầu. DP giảm xuống còn một lớp chuyển tiếp duy nhất và không có lịch sử sâu hơn nào liên quan.
