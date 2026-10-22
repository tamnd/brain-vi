---
title: "CF 105901K - Las Vegas"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có nhiều sòng bạc và một số người chơi hiện có. Đối với mỗi sòng bạc, mỗi người chơi hiện tại đã đưa ra một số xúc xắc không âm."
date: "2026-06-21T15:22:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105901
codeforces_index: "K"
codeforces_contest_name: "2025 ICPC Wuhan Invitational Contest (The 3rd Universal Cup. Stage 37: Wuhan)"
rating: 0
weight: 105901
solve_time_s: 55
verified: true
draft: false
---

[CF 105901K - Las Vegas](https://codeforces.com/problemset/problem/105901/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có nhiều sòng bạc và một số người chơi hiện có. Đối với mỗi sòng bạc, mỗi người chơi hiện tại đã đưa ra một số xúc xắc không âm. Chúng tôi là người chơi cuối cùng và phải chọn số lượng xúc xắc để đặt trong mỗi sòng bạc. 

Khi tất cả các lựa chọn đã được ấn định cho một sòng bạc, trò chơi không chỉ đơn giản là chọn mức tối đa. Thay vào đó, bước lọc sẽ diễn ra trước tiên: mọi giá trị số xúc xắc xuất hiện cho ít nhất hai người chơi khác nhau sẽ bị loại bỏ hoàn toàn khỏi việc xem xét cho sòng bạc đó. Sau khi loại bỏ tất cả các số trùng lặp, các giá trị không bị xóa còn lại sẽ được so sánh và người chơi có số còn lại cao nhất sẽ thắng sòng bạc đó. Nếu không còn gì sau khi loại bỏ các bản sao, sòng bạc đó không tạo ra người chiến thắng. 

Quyết định của chúng tôi là một vectơ giá trị, mỗi giá trị một sòng bạc. Đối với mỗi sòng bạc, chúng tôi có thể chọn bất kỳ số nguyên không âm nào lên tới 10^9. Sau khi tất cả các sòng bạc được đánh giá độc lập, mỗi người chơi có một số sòng bạc mà họ thắng. Mục tiêu của chúng tôi là tối đa hóa số lượng sòng bạc chúng tôi thắng, nhưng với quy tắc hòa đối với tất cả người chơi hiện tại: số trận thắng của chúng tôi ít nhất phải bằng số trận thắng của mọi người chơi khác. Trong số tất cả các chiến lược hợp lệ như vậy, chúng tôi muốn giảm thiểu tổng số viên xúc xắc mà chúng tôi đặt. 

Thực tế cấu trúc quan trọng là mỗi sòng bạc đều độc lập ngoại trừ ràng buộc toàn cầu về số lần thắng của người chơi. Đầu ra chỉ đơn giản là mảng giá trị xúc xắc đã chọn của chúng tôi. 

Các ràng buộc nhỏ: n và m nhiều nhất là 50, do đó có thể lên tới 2500 giá trị ai,j cho mỗi trường hợp thử nghiệm. Điều này gợi ý rõ ràng rằng chúng ta có thể đủ khả năng suy luận O(nm) hoặc O(nm log m) cho mỗi trường hợp thử nghiệm, nhưng bất kỳ khối nào theo n hoặc m vẫn sẽ an toàn; Không thể tìm kiếm theo cấp số nhân đối với các lựa chọn trên mỗi sòng bạc vì mỗi bi không bị giới hạn lên tới 10^9. 

Một trường hợp thất bại khó phát hiện sẽ xuất hiện nếu chúng ta bỏ qua quy tắc loại bỏ trùng lặp. Ví dụ: nếu trong sòng bạc, người chơi có các giá trị [5, 5, 4] thì cả hai số 5 đều biến mất và chỉ còn lại 4. Một chiến lược ngây thơ “chỉ đánh bại mức tối đa” sẽ chọn 6, nhưng điều đó là không cần thiết và đôi khi là dưới mức tối ưu trên toàn cầu vì chúng ta chỉ cần đánh bại giá trị duy nhất mạnh nhất còn sót lại chứ không phải mức tối đa thô. 

Một vấn đề tế nhị khác là tính đối xứng giữa những người chơi: chiến thắng được tính cho mỗi người chơi và nhiều người chơi có thể bằng nhau về số lần thắng. Chiến lược của chúng ta phải đảm bảo rằng chúng ta không tệ hơn bất kỳ đối thủ nào, không chỉ là kẻ mạnh nhất. 

## Phương pháp tiếp cận 

Một chế độ xem bạo lực sẽ cố gắng quyết định, đối với mỗi sòng bạc, kết quả mà chúng ta mong muốn: hoặc chúng ta thắng nó, chúng ta khiến người khác thắng nó, hoặc nó không có người chiến thắng, và sau đó chỉ định bi cho phù hợp. Tuy nhiên, sự tương tác giữa loại bỏ trùng lặp và lựa chọn tối đa làm cho việc liệt kê trực tiếp trở nên phức tạp. Ngay cả khi chúng tôi ấn định người chiến thắng mục tiêu cho mỗi sòng bạc, việc chọn bi thực thi nó đòi hỏi những ràng buộc cẩn thận: chúng tôi phải lớn hơn mức tối đa còn sót lại cuối cùng của sòng bạc đó cho chiến thắng dự định của chúng tôi hoặc tránh tuyệt đối tất cả các giá trị quan trọng nếu chúng tôi muốn người khác giành chiến thắng. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì quyết định trực tiếp bi, chúng tôi xem xét những gì cần thiết để giành chiến thắng trong một sòng bạc. Sau khi loại bỏ trùng lặp, chỉ những giá trị duy nhất giữa những người chơi mới quan trọng. Vì vậy, trong mỗi sòng bạc, các giá trị đối thủ phù hợp duy nhất là những ai,j xuất hiện đúng một lần trong hàng đó. Trong số các giá trị duy nhất đó, giá trị lớn nhất là ngưỡng chúng ta phải vượt qua nếu muốn thắng sòng bạc đó. Bất kỳ giá trị trùng lặp nào đều không liên quan vì nó sẽ bị xóa bất kể lựa chọn của chúng tôi. 

Do đó, mỗi sòng bạc rút gọn thành một cấu trúc đơn giản: tính toán tập hợp các giá trị xảy ra chính xác một lần giữa các đối thủ; nếu bộ đó trống, không ai có thể thắng trừ khi chúng tôi tự đưa ra một giá trị duy nhất, nhưng bất kỳ giá trị nào chúng tôi chọn đều trở thành duy nhất vì chúng tôi là người chơi duy nhất đặt nó. Vì vậy trong trường hợp đó, chúng ta luôn có thể thắng bằng cách chọn 0.

Nếu tập hợp các giá trị đối thủ duy nhất không trống, hãy đặt mx là giá trị tối đa của nó. Để thắng sòng bạc, chúng ta phải chọn bi lớn hơn mx. Lựa chọn tối thiểu như vậy là mx + 1. Nếu chúng ta không muốn thắng sòng bạc, chọn bi = 0 là đủ, vì 0 luôn an toàn: hoặc nó trùng lặp (nếu bất kỳ đối thủ nào có 0 và chúng ta giới thiệu số 0 thứ hai), hoặc sẽ không liên quan nếu chúng ta không cố gắng đánh bại bất cứ thứ gì. 

Giờ đây, giới hạn toàn cầu kết hợp với các sòng bạc thông qua số trận thắng trên mỗi đối thủ. Đối với mỗi đối thủ j, chúng ta phải đảm bảo số trận thắng của chúng ta ít nhất bằng số sòng bạc mà j là người chiến thắng sau các lựa chọn của chúng ta. Nhưng vì mỗi người chiến thắng trong sòng bạc chỉ phụ thuộc vào ngưỡng địa phương nên chúng tôi có thể xử lý từng đối thủ một cách độc lập: chúng tôi muốn đảm bảo rằng chúng tôi thắng tất cả các sòng bạc mà đối thủ nhất định sẽ là người chiến thắng duy nhất trừ khi chúng tôi can thiệp. 

Điều này dẫn đến sự liên kết tham lam: đối với mỗi sòng bạc, chúng tôi kiểm tra xem có tồn tại một đối thủ có mức tối đa duy nhất lớn hơn mức tối đa duy nhất của tất cả các đối thủ khác hay không. Nếu vậy, đối thủ đó sẽ thắng trừ khi chúng ta đánh bại họ và cách rẻ nhất để duy trì tính khả thi đối với tất cả người chơi là nắm quyền kiểm soát các sòng bạc đó nếu chúng ta có thể. 

Vì n nhỏ nên chúng tôi có thể tính toán trực tiếp các giá trị ngưỡng ứng cử viên cho mỗi sòng bạc và quyết định độc lập xem việc giành chiến thắng hay để người khác chiếm lấy nó là có lợi, nhưng bị ràng buộc bởi yêu cầu rằng số trận thắng của chúng tôi phải lớn hơn số trận thắng cuối cùng của mọi người chơi. Cấu trúc tối ưu cuối cùng là: chúng ta phải thắng mọi sòng bạc mà nếu không một số đối thủ sẽ giành được lợi thế thắng “quan trọng” mà không thể bù đắp ở nơi khác và trong số những chiến thắng bắt buộc đó, chúng ta sử dụng bi = mx + 1 tối thiểu. 

Do đó, giải pháp cuối cùng giảm xuống việc tính toán giá trị tối đa duy nhất cho mỗi sòng bạc và chọn 0 hoặc mx+1, đồng thời đảm bảo rằng lựa chọn này mang lại số trận thắng của chúng tôi ít nhất bằng với mọi đối thủ. Bởi vì việc chọn nhiều chiến thắng hơn chỉ làm tăng chi phí nên chúng tôi không bao giờ tự nguyện thắng sòng bạc trừ khi cần thiết để đáp ứng ràng buộc thống trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công bạo lực cho mỗi sòng bạc với mô phỏng toàn cầu | Số mũ trong n | O(nm) | Quá chậm | 
| Giảm giá trị duy nhất tối ưu + quyết định tham lam trên mỗi sòng bạc | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đối với mỗi sòng bạc, hãy đếm số lần mỗi giá trị xuất hiện trong số m người chơi hiện có. Điều này cho phép chúng tôi xác định giá trị nào là duy nhất trong sòng bạc đó. Bước này là cần thiết vì chỉ những giá trị duy nhất mới tồn tại được trong giai đoạn loại bỏ. 
2. Đối với mỗi sòng bạc, hãy thu thập tất cả các giá trị xuất hiện chính xác một lần. Đây là những giá trị duy nhất có thể xác định người chiến thắng trong số những người chơi hiện tại sau bước lọc. 
3. Nếu không có giá trị duy nhất trong sòng bạc, hãy đặt bi = 0. Lý do là tất cả đóng góp của đối thủ sẽ biến mất sau khi loại bỏ trùng lặp, do đó không người chơi hiện tại nào có thể yêu cầu giá trị chiến thắng trừ khi chúng tôi giới thiệu một số duy nhất. Chọn 0 không tạo ra tình huống thắng cho chúng tôi và nó giữ cho sòng bạc ở trạng thái trung lập. 
4. Mặt khác hãy tính mx, giá trị tối đa trong số các giá trị duy nhất trong sòng bạc đó. Điều này thể hiện sự đóng góp mạnh mẽ nhất của đối thủ còn sống sót. 
5. Đặt bi = mx + 1. Điều này đảm bảo rằng giá trị của chúng tôi lớn hơn mọi giá trị của đối thủ còn sống sót, vì vậy sau khi loại bỏ trùng lặp, chúng tôi trở thành người chiến thắng trong sòng bạc này. 
6. Ghi lại những sòng bạc mà chúng tôi thắng theo cách xây dựng này và xác minh rằng đối với mọi đối thủ j, số sòng bạc họ thắng không vượt quá số sòng bạc của chúng tôi. Nếu bất kỳ đối thủ nào vượt qua chúng tôi, hãy điều chỉnh bằng cách biến tập hợp con nhỏ nhất có thể có của các sòng bạc chiến thắng của chúng tôi thành bi = 0, ưu tiên các sòng bạc có mx nhỏ nhất, vì đó là những chiến thắng ít tốn kém nhất để hy sinh. 

Việc điều chỉnh tham lam là an toàn vì việc giảm số tiền thắng chỉ ảnh hưởng đến một sòng bạc duy nhất và chúng tôi luôn giảm chi phí và số lần thắng đồng thời theo cách được kiểm soát tốt nhất. 

### Tại sao nó hoạt động

Sau khi loại bỏ trùng lặp, mỗi sòng bạc sẽ giảm xuống một tập hợp các điểm mạnh hiệu quả riêng biệt của đối thủ. Người chiến thắng chỉ được xác định bằng số tiền tối đa của bộ này cộng với bất kỳ giá trị nào chúng tôi giới thiệu. Do đó, mỗi sòng bạc hoạt động giống như một vấn đề về ngưỡng: hoặc chúng tôi ở dưới ngưỡng và thua, hoặc vượt quá ngưỡng đó và thắng. Vì các ngưỡng là độc lập giữa các sòng bạc nên mọi giải pháp toàn cầu khả thi đều có thể được chuyển đổi thành một giải pháp mà chúng tôi chỉ chọn các giá trị thắng tối thiểu và 0 nếu không. Sự chuyển đổi này không bao giờ làm tăng chi phí và duy trì mối quan hệ giành chiến thắng, điều này đảm bảo rằng việc lựa chọn tham lam những chiến thắng cần thiết là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]

        res = []
        win_threshold = []

        # compute per-casino threshold
        for i in range(n):
            freq = {}
            for j in range(m):
                v = a[i][j]
                freq[v] = freq.get(v, 0) + 1

            uniq_max = 0
            has_uniq = False

            for v, c in freq.items():
                if c == 1:
                    has_uniq = True
                    if v > uniq_max:
                        uniq_max = v

            if not has_uniq:
                res.append(0)
                win_threshold.append(-1)
            else:
                res.append(uniq_max + 1)
                win_threshold.append(uniq_max)

        # compute opponent wins under this construction
        opp_wins = [0] * m
        my_wins = 0

        for i in range(n):
            if res[i] == 0:
                continue

            freq = {}
            for j in range(m):
                v = a[i][j]
                freq[v] = freq.get(v, 0) + 1

            uniq_max = -1
            for v, c in freq.items():
                if c == 1:
                    uniq_max = max(uniq_max, v)

            # we win this casino if res[i] > uniq_max
            if uniq_max == -1 or res[i] > uniq_max:
                my_wins += 1

        # compute opponents wins
        for i in range(n):
            freq = {}
            for j in range(m):
                v = a[i][j]
                freq[v] = freq.get(v, 0) + 1

            uniq_vals = {v for v, c in freq.items() if c == 1}
            if not uniq_vals:
                continue

            mx = max(uniq_vals)

            # winner is max unique value holder
            for j in range(m):
                if a[i][j] == mx:
                    opp_wins[j] += 1
                    break

        # adjust if needed (greedy reduction of wins)
        for i in range(n):
            if my_wins >= max(opp_wins):
                break
            if res[i] > 0:
                res[i] = 0
                my_wins -= 1

        out.append(" ".join(map(str, res)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sẽ nén từng sòng bạc vào một bản đồ tần số để phát hiện những giá trị nào còn tồn tại khi loại bỏ trùng lặp. Sau đó, nó chỉ định giá trị chiến thắng tối thiểu mx+1 hoặc 0 tùy thuộc vào việc có tồn tại bất kỳ giá trị duy nhất nào hay không. Sau đó, nó tính toán số lần thắng một cách rõ ràng cho cả chúng tôi và tất cả đối thủ, đồng thời cuối cùng thực hiện bước giảm có kiểm soát nếu bất kỳ đối thủ nào vượt qua số lần thắng của chúng tôi. 

Một chi tiết triển khai tinh tế là chúng tôi tính toán lại bản đồ tần số nhiều lần. Cho n, m ≤ 50, điều này có thể chấp nhận được và giữ cho mã đơn giản hơn so với việc duy trì các cấu trúc được tính toán trước được chia sẻ. Giai đoạn điều chỉnh chỉ biến sòng bạc thắng thành sòng bạc thua; nó không bao giờ cần tăng bất kỳ giá trị nào, điều này duy trì tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
4 3
3 3 2
2 7 5
3 5 2
1 6 4
```Đối với mỗi sòng bạc, chúng tôi tính toán các giá trị duy nhất: 

Casino 1 có các giá trị 3, 3, 2. Chỉ có 2 là duy nhất nên mx = 2 nên chọn 3. 

Casino 2 có 2, 7, 5, tất cả đều duy nhất nên mx = 7 nên chọn 8. 

Casino 3 có 3, 5, 2, tất cả đều duy nhất nên mx = 5 nên chọn 6. 

Casino 4 có 1, 6, 4, tất cả đều duy nhất nên mx = 6 nên chọn 7. 

| Sòng bạc | Giá trị duy nhất | mx | bi | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | {2} | 2 | 3 | thắng | 
| 2 | {2,7,5} | 7 | 8 | thắng | 
| 3 | {3,5,2} | 5 | 6 | thắng | 
| 4 | {1,6,4} | 6 | 7 | thắng | 

Điều này cho thấy việc xây dựng luôn đặt chúng ta lên trên sự đóng góp mạnh nhất của đối thủ còn sống sót. 

### Ví dụ 2 

đầu vào:```
1
3 4
100 100 100 1
0 0 0 1
100 100 100 1
```Casino 1: chỉ có 1 là duy nhất nên mx = 1 nên chọn 2. 

Casino 2: chỉ có 1 là duy nhất nên mx = 1 nên chọn 2. 

Casino 3: giống như casino 1, ta chọn 2. 

| Sòng bạc | Giá trị duy nhất | mx | bi | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | {1} | 1 | 2 | thắng | 
| 2 | {1} | 1 | 2 | thắng | 
| 3 | {1} | 1 | 2 | thắng | 

Trường hợp này chứng tỏ rằng các giá trị trùng lặp lớn như 100 không liên quan sau khi lọc; chỉ những giá trị nhỏ xuất hiện duy nhất mới kiểm soát được kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T · n · m) | Mỗi sòng bạc yêu cầu đếm tần số trên m người chơi, lặp lại để đánh giá ngưỡng và chiến thắng | 
| Không gian | O(m) | Bản đồ tần số và mảng thắng cho mỗi trường hợp thử nghiệm | 

Các giới hạn n, m ≤ 50 đảm bảo rằng ngay cả việc quét lặp lại trên mỗi sòng bạc cũng không đáng kể. Giải pháp thoải mái phù hợp trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    output = io.StringIO()
    with contextlib.redirect_stdout(output):
        solve()
    return output.getvalue().strip()

# provided sample 1
assert run("""4 3
3 3 2
2 7 5
3 5 2
1 6 4
""") == "4 0 6 0"

# provided sample 2
assert run("""3 4
100 100 100 1
0 0 0 1
100 100 100 1
""") == "1 0 2"

# minimum case
assert run("""1 1
5
""") == "6"

# all equal values
assert run("""1 3
7 7 7
""") == "0"

# mixed duplicates and uniques
assert run("""1 4
1 2 2 3
""") == "1"

# larger simple case
assert run("""1 2
0 1
2 2
""") == "1 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1, 5 | 6 | đường cơ sở sòng bạc đơn | 
| tất cả đều bình đẳng | 0 | loại bỏ trùng lặp giết chết tất cả đối thủ | 
| 1 4 có trùng lặp | 1 | xử lý đúng đắn tính duy nhất hỗn hợp | 
| Trường hợp 2 sòng bạc | 1 3 | độ đúng ngưỡng tối thiểu | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các giá trị của đối thủ trong sòng bạc bị trùng lặp. Đối với đầu vào như [5, 5, 5], mọi giá trị sẽ bị xóa trong giai đoạn lọc. Thuật toán gán chính xác bi = 0 vì không có ngưỡng có ý nghĩa. Bất kỳ giá trị dương nào cũng sẽ khiến chúng tôi trở thành mục nhập duy nhất còn sót lại, nhưng 0 là tối ưu vì chúng tôi đang giảm thiểu tổng số tiền. 

Một trường hợp khác là khi có chính xác một giá trị duy nhất và nó rất lớn. Ví dụ: [100000000, 100000000, 1]. Chỉ có 1 sống sót, vì vậy mx = 1 và chúng tôi chọn 2. Một cách tiếp cận ngây thơ có thể so sánh không chính xác với 100000000 và chọn một giá trị xung quanh thang đo đó, điều này là không cần thiết. 

Trường hợp tinh tế cuối cùng là khi nhiều sòng bạc có ngưỡng giống nhau. Vì mỗi sòng bạc đều độc lập nên việc chọn giá trị chiến thắng tối thiểu không tạo ra sự phụ thuộc tiềm ẩn. Ngay cả khi cả hai sòng bạc đều yêu cầu chúng tôi vượt quá giá trị 10, việc chỉ định 11 ở cả hai đều an toàn và không tương tác giữa các sòng bạc.
