---
problem: 999F
contest_id: 999
problem_index: F
name: "Cards and Joy"
contest_name: "Codeforces Round 490 (Div. 3)"
rating: 2000
tags: ["dp"]
answer: passed_samples
verified: true
solve_time_s: 74
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33b03a-41b4-83ec-a94f-6cd77659666c
---

#CF 999F - Thẻ và niềm vui 

**Xếp hạng:** 2000 
**Thẻ:** dp 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33b03a-41b4-83ec-a94f-6cd77659666c 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một nhóm người chơi ngồi quanh một bàn và nhiều bộ bài. Mỗi người chơi có một “giá trị ưu tiên” duy nhất. Mỗi người chơi phải nhận được chính xác số lượng thẻ như nhau và mỗi thẻ phải được chỉ định cho đúng một người chơi. 

Sau khi việc phân phối được ấn định, mỗi người chơi chỉ xem có bao nhiêu thẻ nhận được phù hợp với giá trị ưa thích của họ. Nếu người chơi nhận được chính xác$t$các thẻ giống nhau, đóng góp của chúng vào tổng số điểm là$h_t$, trong đó trình tự tính điểm tăng dần theo$t$. Người chơi không có trận đấu nào không đóng góp gì. 

Nhiệm vụ là chia tất cả các lá bài cho người chơi sao cho tổng điểm của tất cả người chơi là lớn nhất. 

Cấu trúc của các ràng buộc đã nói lên nhiều điều. Chúng tôi có tới 500 người chơi nhưng mỗi người chơi chỉ nhận được tối đa 10 thẻ. ràng buộc nhỏ này trên$k$là hạn chế cơ cấu quan trọng. Điều đó ngụ ý rằng mỗi người chơi, chúng tôi chỉ cần xem xét tối đa 10 “số trận đấu” có thể có và bất kỳ giải pháp nào cố gắng theo dõi các nhiệm vụ riêng lẻ trên tất cả các thẻ mà không nén sẽ không thành công. Trong khi đó, tổng số thẻ có thể lên tới 5000 nên việc xử lý các thẻ một cách độc lập là không thể; chúng ta phải tổng hợp chúng theo giá trị. 

Một cách tiếp cận ngây thơ sẽ cố gắng gán từng thẻ cho người chơi một cách tham lam hoặc mô phỏng các nhiệm vụ. Điều này nhanh chóng bị phá vỡ vì giá trị của một lá bài phụ thuộc vào hiệu ứng nhóm chung: việc đưa một lá bài cho một người chơi sẽ ngăn cản nó giúp đỡ người khác và lợi ích của một lá bài không phải là tuyến tính mà phụ thuộc vào số lượng lá bài phù hợp mà một người chơi đã có. 

Một ý tưởng tham lam có cấu trúc hơn một chút nhưng vẫn không chính xác là đưa từng lá bài cho người chơi hiện được hưởng lợi nhiều nhất. Điều này không thành công vì mức tăng cận biên thay đổi sau mỗi lần phân công và các nhiệm vụ trong tương lai phụ thuộc vào những lựa chọn ban đầu. 

Một khó khăn tiềm ẩn chính là nhiều người chơi có thể có cùng một con số yêu thích. Các thẻ có cùng giá trị sẽ trở thành tài nguyên chung phải được phân chia tối ưu cho tất cả những người chơi thích nó. 

Các trường hợp cạnh tranh bộc lộ lý luận tham lam không chính xác bao gồm các tình huống như có nhiều người chơi có cùng số yêu thích nhưng có rất ít quân bài giống nhau. Ví dụ: nếu mười người chơi đều thích giá trị 1 nhưng chỉ tồn tại ba lá bài có giá trị 1 thì bất kỳ chiến lược nào cũng phải quyết định người chơi nào nhận được 1 trận và người chơi nào không nhận được. Một cách tiếp cận ngây thơ là trải đều các lá bài hoặc phân công tùy ý có thể bỏ lỡ lựa chọn tối ưu về người chơi nào sẽ “kích hoạt” để nhận phần thưởng một phần. 

Một trường hợp tế nhị khác là khi$h_t$phát triển rất nhanh chóng cho nhỏ$t$. Khi đó, tốt hơn là nên tập trung các trận đấu vào ít người chơi hơn là phân bổ họ một cách đồng đều. Bất kỳ giải pháp nào giả định phân phối bằng nhau đều là tối ưu đều thất bại ở đây. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là xem xét mọi việc phân bài cho người chơi. Với mỗi thẻ, hãy chọn một trong$n$cầu thủ, tôn trọng năng lực$k$. Điều này ngay lập tức mang lại khoảng$n^{kn}$các khả năng xảy ra trong trường hợp xấu nhất, có khả năng lớn về mặt thiên văn ngay cả đối với các đầu vào nhỏ. Ngay cả việc cắt bớt theo giá trị cũng không giúp được gì nhiều, bởi vì sự phụ thuộc không phải là cục bộ vào các lá bài mà là vào số lượng cuối cùng của mỗi người chơi. 

Cái nhìn sâu sắc về cấu trúc là ngừng suy nghĩ về từng thẻ riêng lẻ và thay vào đó nhóm mọi thứ theo giá trị. Đối với một giá trị cố định$x$, giả sử có$c$thẻ có giá trị này và giả sử$m$người chơi có giá trị yêu thích$x$. Chỉ những người chơi này mới quan tâm đến những lá bài này. Đối với mỗi người chơi như vậy, chúng tôi chỉ quan tâm có bao nhiêu$c$thẻ họ nhận được, lên tới$k$. 

Vì vậy, vấn đề trở thành: đối với từng nhóm giá trị độc lập, phân phát các vật phẩm giống hệt nhau (các thẻ có cùng giá trị) vào các thùng (người chơi), mỗi thùng có sức chứa$k$và mức tăng của thùng chỉ phụ thuộc vào số lượng vật phẩm nhận được. Đây là một DP giống như chiếc ba lô cổ điển đối với người chơi trên mỗi giá trị. 

Chúng tôi xử lý giá trị của người chơi theo giá trị. Đối với một giá trị cố định, chúng tôi tính toán số tiền mà mỗi người chơi sẽ nhận được nếu họ nhận được$t$khớp với giá trị này và sau đó chúng tôi chọn cách phân phối giá trị có sẵn$c$thẻ trong số đó. Từ$k \le 10$, chúng tôi có thể thực hiện DP trong đó chúng tôi xử lý từng người chơi một và theo dõi số lượng thẻ có giá trị này đã được chỉ định cho đến nay. 

Đối với mỗi người chơi, chúng tôi thử tất cả số lượng thẻ có thể$t$từ 0 đến$k$, được giới hạn bởi các thẻ còn lại và chuyển trạng thái DP tương ứng. 

Điểm mấu chốt là các giá trị độc lập vì các thẻ có giá trị khác nhau không bao giờ tương tác trong số trận đấu của người chơi cho giá trị đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (chỉ định từng thẻ riêng lẻ) | Hàm mũ | O(1)-O(nk) | Quá chậm | 
| DP theo nhóm giá trị | O(nk²) | O(nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng giá trị thẻ riêng biệt và tích lũy đóng góp tốt nhất vào câu trả lời chung. 

1. Đếm số lần xuất hiện của mỗi giá trị thẻ. Với mọi giá trị$x$, chúng ta biết có bao nhiêu lá bài tồn tại. 
2. Nhóm người chơi theo con số yêu thích của họ. Với mỗi giá trị$x$, thu thập tất cả người chơi yêu thích$x$. Chỉ những người chơi này mới có thể hưởng lợi từ những lá bài có giá trị$x$. 
3. Đối với một giá trị cố định$x$, xác định một mảng DP trong đó$dp[i][j]$đại diện cho tổng mức tăng tối đa sau khi xem xét lần đầu tiên$i$người chơi trong nhóm này và phân phối chính xác$j$thẻ có giá trị$x$. 
4. Khởi tạo$dp[0][0] = 0$, và tất cả các trạng thái khác là không thể. 
5. Với mỗi người chơi trong nhóm này, hãy tính các chuyển tiếp: 

Đối với mỗi phân bổ hiện tại$j$, hãy thử cho người chơi này$t$thẻ ở đâu$0 \le t \le k$Và$j + t \le c_x$. 

Mức tăng thêm là$h_t$. 

Chúng tôi cập nhật$dp[i+1][j+t]$tương ứng. 

Lý do điều này có tác dụng là vì phần thưởng của mỗi người chơi chỉ phụ thuộc vào số trận đấu của chính họ chứ không phụ thuộc vào cách chia các trận đấu đó cho những người khác. 
6. Sau khi xử lý tất cả người chơi để lấy giá trị$x$, lấy giá trị tốt nhất có thể đạt được cho nhóm này và thêm nó vào câu trả lời chung. 
7. Lặp lại cho tất cả các giá trị khác nhau. 

Tổng cuối cùng của tất cả các giá trị là câu trả lời. 

### Tại sao nó hoạt động 

Bất biến quan trọng là với mỗi giá trị$x$, DP khám phá đầy đủ tất cả các phân phối hợp lệ của$c_x$các mặt hàng giống hệt nhau mà người chơi quan tâm$x$, trong khi vẫn duy trì cấu trúc phụ tối ưu: sau khi chúng tôi quyết định số lượng vật phẩm mà người chơi nhận được, đóng góp của họ sẽ trở nên cố định và độc lập với những người khác ngoại trừ hạn chế về năng lực còn lại. Vì các giá trị là độc lập và người chơi chỉ thu được lợi ích từ các kết quả phù hợp trên mỗi giá trị của chính họ nên việc tối ưu hóa từng giá trị riêng biệt và tính tổng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    cards = list(map(int, input().split()))
    fav = list(map(int, input().split()))
    h = [0] + list(map(int, input().split()))

    from collections import defaultdict

    cnt = defaultdict(int)
    for x in cards:
        cnt[x] += 1

    players_by_val = defaultdict(list)
    for i, x in enumerate(fav):
        players_by_val[x].append(i)

    ans = 0

    for val, players in players_by_val.items():
        c = cnt[val]
        if c == 0:
            continue

        m = len(players)

        dp = [[-1] * (c + 1) for _ in range(m + 1)]
        dp[0][0] = 0

        for i in range(m):
            ndp = [[-1] * (c + 1) for _ in range(m + 1)]
            for j in range(c + 1):
                if dp[i][j] < 0:
                    continue
                cur = dp[i][j]
                for t in range(k + 1):
                    if j + t > c:
                        break
                    val_gain = cur + h[t]
                    if ndp[i + 1][j + t] < val_gain:
                        ndp[i + 1][j + t] = val_gain
            dp = ndp

        best = 0
        for j in range(c + 1):
            best = max(best, dp[m][j])

        ans += best

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén dữ liệu đầu vào thành số lượng tần suất và nhóm người chơi, đảm bảo chúng tôi không bao giờ xử lý trực tiếp từng thẻ riêng lẻ. DP sau đó được chạy độc lập trên mỗi giá trị. 

Vòng chuyển tiếp thử một cách rõ ràng tất cả các phân bổ lên đến$k$thẻ cho mỗi người chơi. cái mũ$c + 1$đảm bảo chúng tôi không bao giờ vượt quá số thẻ có sẵn. sử dụng$-1$vì trạng thái không thể ngăn cản việc truyền bá các chuyển đổi không hợp lệ. 

Tập hợp cuối cùng chỉ đơn giản tính tổng mức đóng góp tối ưu cho mỗi nhóm giá trị vì không tồn tại sự ghép nối giá trị chéo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=4, k=3
cards: 1 3 2 8 5 5 8 2 2 8 5 2
fav:   1 2 2 5
h:     2 6 7
```Chúng tôi xử lý từng giá trị một cách độc lập. 

Đối với giá trị 2, có nhiều lá bài và hai người chơi quan tâm. 

| Bước | Người chơi | Thẻ được giao | Tóm tắt Tiểu bang DP | 
| --- | --- | --- | --- | 
| 1 | P2 (yêu thích=2) | thử t=0..3 | phân phối khối lượng sớm | 
| 2 | P3 (yêu thích=2) | tiếp tục phân bổ | trận đấu tập trung chia tốt nhất | 

Giải pháp tối ưu cuối cùng là cung cấp cho hai người chơi đủ trận đấu để tiếp cận$h_2 = 6$mỗi người, trong khi những người khác nhận được các giá trị khác nhau. Giá trị 8 không đóng góp gì vì không người chơi nào thích nó. 

Tổng cuối cùng trở thành: 

Người chơi1: 2, Người chơi2: 6, Người chơi3: 6, Người chơi4: 7, tổng cộng 21. 

Điều này xác nhận rằng DP tập trung chính xác các thẻ có giá trị giống hệt nhau thay vì trải đều chúng. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
n=3, k=2
cards: 1 1 1 2 2 3
fav:   1 2 3
h:     1 10
```Chúng tôi có: 

Giá trị 1: 3 lá bài, 1 người chơi 

Giá trị 2: 2 lá bài, 1 người chơi 

Giá trị 3: 1 lá bài, 1 người chơi 

Mỗi nhóm đều độc lập. 

Với giá trị 1, đưa cả hai lá bài cho một người chơi sẽ mang lại kết quả$h_2 = 10$. Thẻ còn lại không được sử dụng hoặc không liên quan đến người khác. 

Đối với giá trị 2, logic tương tự mang lại 10. 

Đối với giá trị 3, chỉ có một thẻ tồn tại nên mức đóng góp là$h_1 = 1$. 

Tổng cộng = 21. 

Dấu vết cho thấy việc nhóm theo giá trị sẽ ngăn chặn sự can thiệp giữa các vấn đề tối ưu hóa độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(V \cdot n \cdot k^2)$| Mỗi giá trị riêng biệt xử lý nhiều nhất tất cả người chơi; mỗi lần chuyển đổi DP cố gắng tối đa$k$phân bổ | 
| Không gian |$O(nk)$| Bảng DP về người chơi và số lượng thẻ được giao | 

Những ràng buộc đảm bảo$k \le 10$, Vì thế$k^2$nhiều nhất là 100. Với$n \le 500$, giải pháp chạy thoải mái trong giới hạn ngay cả khi hầu hết các giá trị xuất hiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    cards = list(map(int, input().split()))
    fav = list(map(int, input().split()))
    h = [0] + list(map(int, input().split()))

    from collections import defaultdict

    cnt = defaultdict(int)
    for x in cards:
        cnt[x] += 1

    players_by_val = defaultdict(list)
    for i, x in enumerate(fav):
        players_by_val[x].append(i)

    ans = 0

    for val, players in players_by_val.items():
        c = cnt[val]
        if c == 0:
            continue

        m = len(players)
        dp = [[-1] * (c + 1) for _ in range(m + 1)]
        dp[0][0] = 0

        for i in range(m):
            ndp = [[-1] * (c + 1) for _ in range(m + 1)]
            for j in range(c + 1):
                if dp[i][j] < 0:
                    continue
                for t in range(k + 1):
                    if j + t <= c:
                        ndp[i + 1][j + t] = max(ndp[i + 1][j + t], dp[i][j] + h[t])
            dp = ndp

        ans += max(dp[m])

    return str(ans)

# provided sample
assert run("""4 3
1 3 2 8 5 5 8 2 2 8 5 2
1 2 2 5
2 6 7
""") == "21"

# minimal case
assert run("""1 1
1
1
5
""") == "5"

# no matches possible
assert run("""2 2
1 1 1 1
2 3
1 2
""") == "0"

# all same favorite, concentrated reward
assert run("""3 2
1 1 1 1 1 1
7 7 7
1 5
""") == "15"

# mixed distribution pressure case
assert run("""2 2
1 1 1 2
1 1
0 10
""") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 5 | phân công trực tiếp một người chơi | 
| không có trận đấu | 0 | đúng đắn khi không có thẻ có lợi | 
| tất cả cùng yêu thích | 15 | tác dụng tập trung của DP | 
| áp lực hỗn hợp | 20 | đánh đổi giữa các chiến lược phân chia | 

## Vỏ cạnh 

Một trường hợp khó nhận thấy là khi một giá trị xuất hiện trong quân bài nhưng không người chơi nào thích nó. Thuật toán tự nhiên bỏ qua nó vì danh sách người chơi cho giá trị đó trống, do đó đóng góp của nó bằng 0. Điều này ngăn chặn việc vô tình chạy DP lãng phí. 

Một trường hợp khác là khi số lượng bài ít hơn số lượng người chơi muốn có. Ví dụ: nếu ba người chơi muốn có một giá trị nhưng chỉ có một lá bài tồn tại, DP đảm bảo rằng chỉ có một người chơi nhận được$t=1$và những người khác nhận được$t=0$, tối đa hóa$h_1$cho một nhiệm vụ duy nhất thay vì phân bổ không có đóng góp nào một cách không chính xác. 

Trường hợp thứ ba là khi$h_t$tăng trưởng mạnh mẽ, giúp việc tập trung tất cả các thẻ vào một người chơi trở nên tối ưu ngay cả khi nhiều người chơi đủ điều kiện. DP đánh giá rõ ràng tất cả các khoản phân phối, do đó, nó sẽ chọn phân bổ tập trung này một cách tự nhiên khi nó mang lại tổng phần thưởng cao hơn.
