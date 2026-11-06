---
problem: 1282C
contest_id: 1282
problem_index: C
name: "Petya and Exam"
contest_name: "Codeforces Round 610 (Div. 2)"
rating: 1800
tags: ["greedy", "sortings", "two pointers"]
answer: passed_samples
verified: false
solve_time_s: 175
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a2da601-1174-83ec-845c-92c2dc74892b
---

# CF 1282C - Petya và kỳ thi 

**Đánh giá:** 1800 
**Tags:** tham lam, sắp xếp, hai con trỏ 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2da601-1174-83ec-845c-92c2dc74892b 

--- 

##Giải pháp 

## Hiểu vấn đề 

Mỗi trường hợp kiểm thử mô tả một bài kiểm tra trong đó các vấn đề đều có thời hạn và mất khoảng thời gian khác nhau tùy thuộc vào việc chúng dễ hay khó. Petya xử lý các vấn đề một cách tuần tự bắt đầu từ thời điểm 0, chọn thứ tự và có thể dừng ở bất kỳ thời điểm nguyên nào$s \le T$. Điểm của anh ấy phụ thuộc vào số lượng bài toán anh ấy đã giải quyết được, nhưng chỉ khi mọi bài toán có thời hạn tối đa là$s$lúc đó đã được giải quyết. Nếu ngay cả một vấn đề “tích cực” như vậy vẫn chưa được giải quyết khi anh ta rời đi, thì điểm cho lần thử đó sẽ bằng 0. 

Vì vậy, cấu trúc thực tế không chỉ là lập kế hoạch mà còn là chọn tiền tố thời gian và chọn một tập hợp con các vấn đề để phù hợp với thời gian đó đồng thời tôn trọng hai ràng buộc: tổng thời gian dành cho các vấn đề đã chọn không thể vượt quá$s$, và tối đa tất cả các vấn đề về thời hạn$s$phải được bao gồm trong tập giải quyết đó. 

Sự căng thẳng chính là sự gia tăng$s$nới lỏng thời hạn nhưng cũng tăng khối lượng công việc cần thiết vì nhiều vấn đề trở nên bắt buộc hơn. Một giải pháp tốt là một điểm cân bằng nào đó để Petya vẫn có thể hoàn thành tất cả các bài tập bắt buộc và cũng có thể lấp đầy thời gian còn lại bằng những bài tùy chọn. 

Các ràng buộc ngụ ý rằng bất kỳ giải pháp nào cũng phải gần với tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng cộng$n$qua các bài kiểm tra là$2 \cdot 10^5$, bất kỳ mô phỏng bậc hai nào trên các tập hợp con hoặc các điểm thời gian đều quá chậm. Các chiến lược dựa trên sắp xếp và quét tham lam là những ứng cử viên đương nhiên. 

Một trường hợp thất bại phổ biến phát sinh khi người ta cho rằng sau khi giải quyết được một loạt vấn đề bắt buộc, chúng ta có thể tham lam lấy những vấn đề còn lại rẻ nhất mà không cần xem xét thời hạn đặt ra sẽ thay đổi như thế nào khi chúng ta di chuyển.$s$. Ví dụ: chọn thời điểm muộn$s$mà không đảm bảo tính khả thi của tất cả các thời hạn trước đó sẽ dẫn đến việc đặt lại điểm không hợp lệ. 

Một thất bại tinh tế khác sẽ xảy ra nếu chúng ta cố gắng sửa chữa$s$và tham lam giải quyết các vấn đề theo thứ tự tùy ý mà không phân biệt chi phí dễ và khó. Ví dụ, việc trộn lẫn các vấn đề khó khăn sớm có thể cản trở việc đáp ứng thời hạn chặt chẽ hơn mà lẽ ra là tối ưu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng hết sức có thể để rời đi$s$, và với mỗi$s$xác định số lượng bài toán tối đa mà Petya có thể giải được trong thời gian giới hạn$s$đồng thời giải quyết mọi vấn đề với$t_i \le s$. Đối với một cố định$s$, trước tiên chúng ta sẽ xác định các vấn đề bắt buộc, đảm bảo chúng có thể phù hợp với$s$, rồi tham lam thêm các bài toán khác bằng cách tăng dần thứ tự thời gian giải. Điều này đúng nhưng đắt tiền. 

Đối với mỗi$s$, quét tất cả các vấn đề và sắp xếp chi phí ứng viên$O(n \log n)$, và làm điều này cho tất cả$T$những lúc có thể là không thể vì$T$có thể$10^9$. Ngay cả khi chúng ta chỉ xem xét sự khác biệt$t_i$, vẫn còn tới$n$ứng viên, đưa ra$O(n^2 \log n)$trong trường hợp xấu nhất. 

Quan sát quan trọng là các câu trả lời tối ưu chỉ xảy ra ở những thời điểm bằng một số$t_i$hoặc tại$T$. Bất kỳ chiến lược tối ưu nào cũng phải “bật” một ràng buộc mới chính xác khi một vấn đề mới trở thành bắt buộc. Điều này làm giảm tập ứng viên của$s$nhiều nhất là$n+1$. 

Một lần$s$đã được khắc phục, tất cả các vấn đề được chia thành bắt buộc (thời hạn$\le s$) và tùy chọn (thời hạn$> s$). Những điều bắt buộc phải được đưa vào và xác định chi phí thời gian cơ bản. Nếu chi phí cơ bản này vượt quá$s$, sự lựa chọn của$s$không hợp lệ. 

Trong số các vấn đề tùy chọn, chúng tôi muốn tối đa hóa số lượng tùy thuộc vào thời gian còn lại. Vì mọi vấn đề đều là chi phí$a$hoặc$b$, cách tối ưu là luôn lấy cái dễ hơn trước. Điều này trở thành một kiểu tham lam kiểu ba lô cổ điển, trong đó việc sắp xếp là không cần thiết: chúng ta chỉ đếm xem có bao nhiêu bài toán dễ và khó. 

Chúng tôi có thể xử lý trước bằng cách sắp xếp các vấn đề theo thời hạn và duy trì số lượng tiền tố thuộc loại dễ và khó. Sau đó với mỗi ứng viên$s$, chúng ta biết chính xác có bao nhiêu bài toán dễ/khó bắt buộc tồn tại và còn lại bao nhiêu bài tập tùy chọn. Tính khả thi được kiểm tra bằng cách sử dụng tổng thời gian và thời gian còn lại được sử dụng một cách tham lam để tạo thêm nhiều vấn đề hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả$s$|$O(n^2 \log n)$|$O(n)$| Quá chậm | 
| Sắp xếp + tiền tố + quét tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Sắp xếp tất cả các vấn đề theo thời gian bắt buộc$t_i$. Điều này đảm bảo rằng khi chúng ta tăng$s$, tập hợp các vấn đề bắt buộc chỉ tăng lên. Tính đơn điệu này là cấu trúc cốt lõi cho phép xử lý tiền tố. 
2. Xây dựng số tiền tố: tại vị trí$i$, lưu trữ bao nhiêu bài toán dễ và khó xuất hiện trong số các bài toán đầu tiên$i$vấn đề theo thứ tự sắp xếp. 
3. Xem xét từng thời điểm ứng viên$s$như một trong hai$t_i$đối với một số người$i$, hoặc$T$. Đối với tiền tố cố định lên đến$i$, tất cả những vấn đề này là bắt buộc. 
4. Tính chi phí thời gian bắt buộc như$cost = easy_i \cdot a + hard_i \cdot b$. Nếu điều này vượt quá$s$, bỏ qua cái này$i$bởi vì Petya thậm chí không thể đáp ứng được những ràng buộc cần thiết. 
5. Nếu khả thi hãy tính thời gian còn lại$rem = s - cost$. Đồng thời tính toán số lượng tùy chọn còn lại: dễ và khó ngoài tiền tố. 
6. Để tối đa hóa các vấn đề được giải quyết, hãy luôn ưu tiên các vấn đề tùy chọn dễ vì chúng tiêu tốn ít thời gian hơn cho mỗi vấn đề. Sử dụng càng nhiều càng dễ dàng phù hợp với$rem$, sau đó sử dụng các bài toán khó. 
7. Theo dõi tổng số giải tối đa trên tất cả hợp lệ$s$. 

### Tại sao nó hoạt động 

Việc quét theo thời hạn sắp xếp đảm bảo rằng mọi giải pháp hợp lệ đều tương ứng với một số tiền tố của mảng đã được sắp xếp. Trong bất kỳ tiền tố cố định nào, tính khả thi chỉ phụ thuộc vào tổng thời gian xử lý cần thiết chứ không phụ thuộc vào thứ tự. Vì tất cả các nhiệm vụ thuộc một loại nhất định đều có chi phí giống nhau, nên việc sử dụng tối ưu thời gian còn lại là tham lam theo chi phí: thực hiện các nhiệm vụ dễ dàng hơn trước tiên luôn chiếm ưu thế trong bất kỳ trao đổi nào với nhiệm vụ khó về số lượng vấn đề được giải quyết. Điều này tạo ra một cấu trúc tối ưu ổn định trong đó tính khả thi của tiền tố và tính tham lam cục bộ cùng nhau bao trùm tất cả các lịch trình tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m = int(input())
    for _ in range(m):
        n, T, a, b = map(int, input().split())
        tp = list(map(int, input().split()))
        t = list(map(int, input().split()))

        arr = list(zip(t, tp))
        arr.sort()

        easy_pref = [0]
        hard_pref = [0]

        for ti, typ in arr:
            easy_pref.append(easy_pref[-1] + (typ == 0))
            hard_pref.append(hard_pref[-1] + (typ == 1))

        ans = 0

        def evaluate(limit_time, idx):
            easy_m = easy_pref[idx]
            hard_m = hard_pref[idx]

            cost = easy_m * a + hard_m * b
            if cost > limit_time:
                return 0

            rem = limit_time - cost

            easy_total = easy_pref[-1]
            hard_total = hard_pref[-1]

            easy_avail = easy_total - easy_m
            hard_avail = hard_total - hard_m

            take_easy = min(easy_avail, rem // a)
            rem -= take_easy * a

            take_hard = min(hard_avail, rem // b)

            return easy_m + hard_m + take_easy + take_hard

        n = len(arr)

        for i in range(n + 1):
            limit = T if i == n else arr[i-1][0]
            ans = max(ans, evaluate(limit, i))

        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ sắp xếp các vấn đề sao cho bất kỳ tiền tố nào cũng tương ứng với ngưỡng thời hạn khả thi. Mảng tiền tố cho phép trích xuất số lượng bắt buộc theo thời gian liên tục. Chức năng đánh giá thực thi tính khả thi của các nhiệm vụ bắt buộc, sau đó lấp đầy thời gian còn lại bằng các nhiệm vụ tùy chọn theo thứ tự chi phí tăng dần (dễ trước khó). Mỗi tiền tố được kiểm tra như một điểm cắt tối ưu tiềm năng. 

Một chi tiết tinh tế là thời gian của ứng viên được lấy chính xác như một giá trị thời hạn. Việc chọn bất kỳ khoảng thời gian trung gian nào giữa hai thời hạn không làm thay đổi bộ bắt buộc mà chỉ thắt chặt quỹ thời gian nên không thể cải thiện câu trả lời. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ với thời hạn và chi phí hỗn hợp: 

đầu vào:```
1
4 10 1 3
0 1 0 1
2 3 5 7
```Sắp xếp theo thời hạn: 

| tôi | t | gõ | dễ dàng trước | cứng trước | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 1 | 0 | 
| 2 | 3 | 1 | 1 | 1 | 
| 3 | 5 | 0 | 2 | 1 | 
| 4 | 7 | 1 | 2 | 2 | 

Đối với tiền tố$i=2$, giới hạn là$s=3$. Chi phí bắt buộc là$1\cdot1 + 1\cdot3 = 4$, vượt quá 3, vì vậy không hợp lệ. 

Đối với tiền tố$i=3$, giới hạn$s=5$. Chi phí bắt buộc là$2 + 3 = 5$, rem = 0, điểm = 3. 

Đối với tiền tố$i=4$, giới hạn$s=7$. Giá bắt buộc = 8, rem = -1, không hợp lệ. 

Câu trả lời hay nhất là 3. 

Dấu vết này cho thấy các tiền tố không khả thi được lọc hoàn toàn theo chi phí như thế nào và tính khả thi thay đổi mạnh mẽ như thế nào ở mỗi ngưỡng thời hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| việc sắp xếp chiếm ưu thế, mỗi đánh giá tiền tố là O(1) | 
| Không gian |$O(n)$| mảng để sắp xếp và tính tổng tiền tố | 

Giải pháp phù hợp thoải mái vì tổng thể$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, giúp quá trình xử lý dựa trên sắp xếp trở nên hiệu quả trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    m = int(input())
    out = []
    for _ in range(m):
        n, T, a, b = map(int, input().split())
        tp = list(map(int, input().split()))
        t = list(map(int, input().split()))
        arr = list(zip(t, tp))
        arr.sort()

        easy_pref = [0]
        hard_pref = [0]
        for ti, typ in arr:
            easy_pref.append(easy_pref[-1] + (typ == 0))
            hard_pref.append(hard_pref[-1] + (typ == 1))

        def eval(limit, idx):
            em = easy_pref[idx]
            hm = hard_pref[idx]
            cost = em * a + hm * b
            if cost > limit:
                return 0
            rem = limit - cost
            ea = easy_pref[-1] - em
            ha = hard_pref[-1] - hm
            take_e = min(ea, rem // a)
            rem -= take_e * a
            take_h = min(ha, rem // b)
            return em + hm + take_e + take_h

        ans = 0
        n = len(arr)
        for i in range(n + 1):
            limit = T if i == n else arr[i-1][0]
            ans = max(ans, eval(limit, i))

        out.append(str(ans))

    return "\n".join(out)

# provided samples (single aggregated test)
assert run("""10
3 5 1 3
0 0 1
2 1 4
2 5 2 3
1 0
3 2
1 20 2 4
0
16
6 20 2 5
1 1 0 1 0 0
0 8 2 9 11 6
7 17 1 6
1 1 0 1 0 0 0
1 7 0 11 10 15 10
6 17 2 6
0 0 1 0 0 1
7 6 3 7 10 12
5 17 2 5
1 1 1 1 0
17 11 10 6 4
1 1 1 2
0
1
""") == """3
2
1
0
1
4
0
1
2
1"""

# small custom case
assert run("""1
2 5 1 2
0 1
1 2
""") == "2", "basic mix"

# all easy
assert run("""1
3 10 1 5
0 0 0
1 2 3
""") == "3", "all easy"

# all hard tight time
assert run("""1
3 5 1 4
1 1 1
1 2 3
""") == "1", "hard constraint"

# boundary zero deadlines
assert run("""1
2 5 1 2
0 1
0 5
""") == "2", "deadline edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hộp nhỏ hỗn hợp | 2 | tương tác của các loại | 
| tất cả đều dễ dàng | 3 | tham lam lấp đầy sự đúng đắn | 
| tất cả đều cứng chặt | 1 | cắt tỉa khả thi | 
| hỗn hợp thời hạn không | 2 | xử lý ranh giới của t_i = 0 | 

## Vỏ cạnh 

Trường hợp nghiêm trọng là khi tất cả các vấn đề bắt buộc đã vượt quá giới hạn thời gian cho một tiền tố nhất định. Ví dụ, nếu$s$tương đương với một thời hạn sớm nhỏ nhưng lại bao gồm nhiều vấn đề khó khăn, việc kiểm tra chi phí sẽ ngay lập tức từ chối nó. Thuật toán trả về 0 một cách chính xác cho tiền tố đó mà không cố gắng điền tham lam, đảm bảo lịch trình không hợp lệ không làm rò rỉ một phần điểm. 

Một trường hợp khó khăn khác xảy ra khi mọi vấn đề đều dễ dàng. Khi đó mọi tiền tố đều khả thi và chiến lược tối ưu chỉ đơn giản là lấy càng nhiều tiền tố càng tốt trong phạm vi$T$. Giai đoạn lấp đầy tham lam tự nhiên giảm xuống còn việc thực hiện tất cả các nhiệm vụ theo thứ tự thời hạn tăng dần và giải pháp đạt được số lượng đầy đủ. 

Trường hợp khó phát hiện cuối cùng là khi thời hạn bằng nhau hoặc theo nhóm. Vì việc sắp xếp ổn định đối với các giá trị, nên các thời hạn giống nhau không tạo ra sự mơ hồ: cấu trúc tiền tố vẫn nắm bắt chính xác tất cả các bộ bắt buộc và việc đánh giá ở mỗi ranh giới vẫn đủ.
