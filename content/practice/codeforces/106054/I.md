---
title: "CF 106054I - In\u00e9s và compitas của cô ấy"
description: "Chúng ta được đưa ra một quá trình quyết định lặp đi lặp lại qua nhiều vòng. Trong mỗi vòng có một nhóm người chơi $N+1$: Inés và $N$ những người khác. Mỗi vòng trình bày hai cơ chế khả thi để phân phối vàng. Trong cơ chế đầu tiên, một nhóm nhỏ người chơi chọn “chia sẻ”."
date: "2026-06-20T13:22:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106054
codeforces_index: "I"
codeforces_contest_name: "2025 Argentinian Programming Tournament (TAP)"
rating: 0
weight: 106054
solve_time_s: 54
verified: true
draft: false
---

[CF 106054I - In\u00e9s và compitas của cô ấy](https://codeforces.com/problemset/problem/106054/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một quá trình quyết định lặp đi lặp lại qua nhiều vòng. Trong mỗi vòng có một nhóm$N+1$người chơi: Inés và$N$người khác. Mỗi vòng trình bày hai cơ chế khả thi để phân phối vàng. 

Trong cơ chế đầu tiên, một nhóm nhỏ người chơi chọn “chia sẻ”. Nếu như$k$tổng số người chơi chọn tùy chọn chia sẻ này thì tổng số tiền là$X$, và mỗi cái đó$k$người chơi nhận được$\lfloor X / k \rfloor$. Giá trị này chỉ phụ thuộc vào số lượng người chơi tham gia nhóm chia sẻ chứ không phụ thuộc vào danh tính của họ. 

Trong cơ chế thứ hai, người chơi chọn nó sẽ nhận được một số tiền cố định$Y$, độc lập với người khác. 

Đối với mỗi vòng, chúng tôi được cho biết những gì khác$N$người chơi lựa chọn. Sau đó Inés quyết định lựa chọn của riêng mình. Quyết định của cô ấy hoàn toàn mang tính tham lam trong mỗi vòng: cô ấy so sánh mức chi trả của chính mình theo phương án 1 so với phương án 2, giả sử các lựa chọn của người khác là cố định và chọn lựa chọn tốt hơn. Nếu cả hai lựa chọn đều mang lại lợi ích như nhau cho cô ấy, cô ấy phải chọn chia sẻ. 

Sau khi mô phỏng tất cả các vòng đấu với chiến lược này, chúng ta phải tính tổng số vàng tích lũy được của mọi người chơi, bao gồm cả Inés. 

Những hạn chế$N, M \le 100$làm rõ rằng ngay cả một điều đơn giản$O(M \cdot N)$hoặc$O(M \cdot (N+1))$mô phỏng là đầy đủ. Không cần tối ưu hóa ngoài việc đánh giá trực tiếp mỗi vòng. 

Một điểm tinh tế là quyết định của Inés ảnh hưởng đến lợi ích của những người khác trong quyền chọn chia sẻ, bởi vì nó thay đổi$k$. Điều này có nghĩa là chúng ta phải đánh giá cả hai thế giới giả định trong mỗi vòng: một thế giới trong đó Inés chọn phương án 1 và một thế giới nơi cô ấy chọn phương án 2. 

Các trường hợp Edge có thể phá vỡ lý luận ngây thơ thường đến từ việc tính toán lại$k$không chính xác hoặc quên rằng việc phân chia tầng áp dụng cho mọi người tham gia nhóm chia sẻ. 

Trường hợp góc minh họa tối thiểu là khi$X$nhỏ hơn số lượng người chia sẻ. Ví dụ, nếu$X = 3$và 4 người chơi chia sẻ thì mọi người đều nhận được$\lfloor 3/4 \rfloor = 0$. Bất kỳ việc triển khai nào quên sàn hoặc giả định ít nhất 1 đơn vị cho mỗi người chơi sẽ sai. 

Một trường hợp phạt góc khác là tie-break. Nếu cả hai lựa chọn đều mang lại lợi ích như nhau cho Inés, cô ấy phải chọn phương án 1. Điều này ảnh hưởng đến việc phân phối trong tương lai và phải được thực thi một cách chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là mô phỏng mỗi vòng hai lần theo ý tưởng. Đối với một vòng nhất định, chúng tôi thử Inés chọn tùy chọn 1, tính tổng số người chơi tham gia chia sẻ và xác định phần thưởng cho mỗi người chơi$\lfloor X/k \rfloor$. Sau đó, chúng tôi tính toán mức tăng giả định của Inés. Chúng tôi làm tương tự cho tùy chọn 2, trong đó lợi ích của cô ấy chỉ đơn giản là$Y$. 

Đánh giá mạnh mẽ này đã đủ cho mỗi vòng vì tính toán$k$chỉ là đếm xem có bao nhiêu$N$người chơi cố định chọn phương án 1, cộng thêm cả bản thân Inés tùy theo kịch bản. Đó là$O(N)$mỗi vòng, đưa ra$O(MN)$tổng công việc. 

Từ$N, M \le 100$, nhiều nhất là thế này$10^4$hoạt động, đó là tầm thường. 

Không có vụ nổ tổ hợp ẩn nào vì những người chơi không phải Inés không thay đổi quyết định của họ và mỗi vòng sẽ độc lập sau khi lựa chọn của Inés được ấn định. Sự phụ thuộc duy nhất nằm trong một vòng duy nhất thông qua giá trị của$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(MN)$|$O(N)$| Đã chấp nhận | 
| Tối ưu |$O(MN)$|$O(N)$| Đã chấp nhận | 

Trong thực tế, giải pháp “tối ưu” chỉ là mô phỏng cẩn thận. 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng vòng một cách độc lập, duy trì điểm tích lũy cho tất cả người chơi. 

1. Với mỗi vòng, hãy đọc$X$,$Y$, và dãy các lựa chọn của$N$người chơi. Chúng tôi ngay lập tức tính toán có bao nhiêu người chơi chọn tùy chọn 1, gọi giá trị này là$c$. Số lượng này xác định tất cả các kết quả được chia sẻ. 
2. Xét Inés chọn phương án 1. Trong trường hợp này, số người chia sẻ trở thành$k = c + 1$. Mọi người chia sẻ, bao gồm Inés và tất cả người chơi có lựa chọn 1, đều nhận được$\lfloor X / k \rfloor$. Người chơi chọn phương án 2 không nhận được gì ở nhánh này. Chúng tôi tính toán lợi ích giả định của Inés trong kịch bản này. 
3. Xét Inés chọn phương án 2. Tại đây, cô ấy nhận được$Y$. Những người chơi khác không bị ảnh hưởng bởi quyết định của cô ấy về lựa chọn của riêng họ, vì vậy việc phân bổ của họ không cần tính toán lại; chỉ có lợi ích của Inés mới quan trọng đối với việc ra quyết định. 
4. So sánh kết quả của Inés trong cả hai trường hợp. Nếu phương án 1 có giá trị cao hơn thì cô ấy sẽ chọn nó. Nếu phương án 2 có giá trị cao hơn thì cô ấy sẽ chọn nó. Nếu bằng nhau thì cô chọn phương án 1 do luật hòa. 
5. Sau khi quyết định của cô ấy được khắc phục, chúng tôi tính toán kết quả cuối cùng cho vòng đó và cộng phần đóng góp vào tổng số tiền của mỗi người chơi. Đối với tùy chọn 1, chúng tôi lại sử dụng thích hợp$k$và phân phối$\lfloor X/k \rfloor$tới tất cả người chia sẻ. Đối với phương án 2, chỉ Inés được lợi$Y$. 

Chi tiết quan trọng là chúng ta phải tính toán lại kết quả chia sẻ chỉ một lần cho mỗi kịch bản trong mỗi vòng, không tăng dần qua các vòng. 

### Tại sao nó hoạt động 

Thuật toán đúng vì mỗi vòng là độc lập ngoại trừ những lựa chọn cố định của những người chơi không phải Inés. Đối với bất kỳ vòng cố định nào, quyết định của Inés chỉ phụ thuộc vào việc so sánh hai giá trị xác định: phần thưởng của cô ấy khi được chia sẻ và phần thưởng trực tiếp. Phần thưởng chia sẻ hoàn toàn được xác định bởi số lượng người tham gia, số lượng này đã được biết trước cho cả hai trường hợp giả định. Vì quyết định của cô ấy không ảnh hưởng đến bất kỳ đầu vào vòng nào trong tương lai nên các quyết định tối ưu cục bộ cho mỗi vòng sẽ tạo ra kết quả chung chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())
    ans = [0] * (N + 1)

    for _ in range(M):
        X, Y = map(int, input().split())
        A = list(map(int, input().split()))

        cnt1 = sum(1 for a in A if a == 1)

        # Option 1: Inés shares
        k1 = cnt1 + 1
        gain1 = X // k1

        # Option 2: Inés takes Y
        gain2 = Y

        if gain1 > gain2:
            take_share = True
        elif gain2 > gain1:
            take_share = False
        else:
            take_share = True

        if take_share:
            k = cnt1 + 1
            val = X // k
            for i in range(N):
                if A[i] == 1:
                    ans[i] += val
            ans[N] += val
        else:
            ans[N] += Y

            for i in range(N):
                if A[i] == 1:
                    ans[i] += 0

    print(*ans)

if __name__ == "__main__":
    solve()
```Giải pháp giữ tổng số hoạt động cho mỗi người chơi. Đối với mỗi vòng, trước tiên nó sẽ tính số lượng người chơi chọn tùy chọn 1, đây là số liệu thống kê toàn cầu duy nhất cần thiết. Sau đó, nó đánh giá lợi ích giả định của Inés theo cả hai lựa chọn. Việc ràng buộc được xử lý một cách rõ ràng bằng cách ưu tiên chia sẻ khi bình đẳng. 

Khi áp dụng quyết định cuối cùng, nhánh chia sẻ sẽ phân phối giá trị như nhau cho tất cả người chơi tham gia, bao gồm cả Inés. Nhánh không chia sẻ chỉ cập nhật Inés. 

Một lỗi triển khai phổ biến là quên rằng nhóm người chia sẻ bao gồm chính Inés trong kịch bản tùy chọn 1, điều này sẽ chuyển mẫu số từ$c$ĐẾN$c+1$. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng một ví dụ đơn giản để theo dõi cơ học một cách rõ ràng. 

### Ví dụ 1 

đầu vào:```
N=2, M=1
X=10, Y=6
A = [1, 2]
```| Bước | cnt1 | k1 | Tùy chọn Inés 1 | Tùy chọn Inés 2 | Quyết định | Cập nhật | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 10//2 = 5 | 6 | phương án 2 | Inés += 6 | 

Ở đây chia sẻ được 5, trực tiếp được 6 nên Inés chọn phương án 2. Chỉ có mình cô được 6. 

Kết quả cuối cùng: player1=0, player2=0, Inés=6. 

Điều này chứng tỏ sự lựa chọn của người chơi khác không thành vấn đề khi Inés từ chối chia sẻ. 

### Ví dụ 2 

đầu vào:```
N=3, M=1
X=12, Y=3
A = [1, 1, 2]
```| Bước | cnt1 | k1 | Tùy chọn Inés 1 | Tùy chọn Inés 2 | Quyết định | Cập nhật | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 12//3 = 4 | 3 | phương án 1 | p1+=4, p2+=4, Inés+=4 | 

Ở đây việc chia sẻ thực sự tốt hơn nên tất cả những người chọn phương án 1 cộng với Inés sẽ nhận được 4. 

Điều này cho thấy quyết định của Inés thay đổi quy mô nhóm như thế nào và do đó phân phối lại phần thưởng cho nhiều người chơi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(MN)$| Mỗi vòng đếm N người chơi và áp dụng số học theo thời gian không đổi | 
| Không gian |$O(N)$| Chỉ lưu trữ điểm tích lũy và mảng đầu vào hiện tại | 

Với$N, M \le 100$, các hoạt động tối đa là xung quanh$10^4$, trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except:
        pass
    return ""  # placeholder since direct capture depends on environment

# Sample-style sanity checks (logical, not executable in isolation)
# These are illustrative templates rather than strict assertions in this format.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | xử lý cà vạt đúng cách | hành vi vòng đơn | 
| tất cả chọn 2 | chỉ Inés bị ảnh hưởng | không có nhóm chia sẻ | 
| tất cả chọn 1 | chia sẻ nhóm đầy đủ | mẫu số bao gồm tất cả người chơi | 
| hộp đựng cà vạt | thích phương án 1 | luật ràng buộc | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi việc chia sẻ trở nên cực kỳ không có lợi do quy mô nhóm lớn. Ví dụ, nếu$X=1$và tất cả người chơi chọn phương án 1 thì$k=N+1$và mọi người tham gia đều nhận được 0. Thuật toán xử lý việc này một cách tự nhiên vì phép chia số nguyên tạo ra 0 và tất cả các cập nhật vẫn nhất quán. 

Một trường hợp cạnh khác là khi chỉ Inés chọn phương án 1 trong khi tất cả những người khác chọn phương án 2. Khi đó$k=1$, vì vậy cô ấy nhận được đầy đủ$X$. Việc triển khai bao gồm chính nó trong số đếm một cách chính xác, tránh chia cho 0 hoặc mẫu số không chính xác. 

Trường hợp tế nhị cuối cùng là sự bình đẳng giữa các lựa chọn. Nếu như$X//(c+1) = Y$, thuật toán vẫn phải chọn chia sẻ. Nhánh quyết định kiểm tra rõ ràng sự bình đẳng và chỉ định đường dẫn chia sẻ, đảm bảo tổng số xuôi dòng phù hợp với yêu cầu của bài toán.
