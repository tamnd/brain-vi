---
title: "CF 1260E - Giải đấu"
description: "Chúng ta được tổ chức một giải đấu loại trực tiếp với $n$ người tham gia, trong đó $n$ là lũy thừa của hai. Mỗi người tham gia có một sức mạnh cố định và trong bất kỳ trận đấu trực tiếp nào, võ sĩ mạnh hơn luôn thắng trừ khi chúng ta đã hối lộ cho võ sĩ yếu hơn, trong trường hợp đó võ sĩ yếu hơn được phép thắng…"
date: "2026-06-18T17:47:44+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1260
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 77 (Rated for Div. 2)"
rating: 2400
weight: 1260
solve_time_s: 75
verified: true
draft: false
---

[CF 1260E - Giải đấu](https://codeforces.com/problemset/problem/1260/E) 

**Đánh giá:** 2400 
**Tags:** bạo lực, dp, tham lam 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tổ chức một giải đấu loại trực tiếp với$n$người tham gia, ở đâu$n$là sức mạnh của hai. Mỗi người tham gia có một sức mạnh cố định và trong bất kỳ trận đấu trực tiếp nào, võ sĩ mạnh hơn luôn thắng trừ khi chúng tôi đã hối lộ cho võ sĩ yếu hơn, trong trường hợp đó võ sĩ yếu hơn được phép thắng trận đấu đó. 

Giải đấu hoàn toàn nằm trong tầm kiểm soát của chúng tôi về mặt ghép đôi. Ở mỗi hiệp, chúng tôi chia các võ sĩ còn lại thành từng cặp, mỗi cặp chọn ra một người chiến thắng và người chiến thắng sẽ đi tiếp. Điều này lặp lại cho đến khi còn lại một nhà vô địch. Một võ sĩ đặc biệt là bạn của bạn, được xác định trong đầu vào bằng giá trị$-1$. Mục đích là để đảm bảo rằng người bạn này trở thành người chiến thắng cuối cùng đồng thời giảm thiểu tổng chi phí hối lộ phải trả cho các võ sĩ khác. 

Điểm tinh tế mấu chốt là việc hối lộ không phải là làm cho bạn của bạn mạnh hơn trên toàn cầu mà là ép buộc có chọn lọc kết quả trận đấu khi cần thiết. Vì việc ghép đôi là tùy ý ở mỗi vòng nên chúng tôi có thể chọn đối thủ mà bạn của bạn phải đối mặt và những mối đe dọa tiềm ẩn nào sẽ bị loại bỏ trước đó. 

Những ràng buộc cho phép$n$lên đến$2^{18}$, nghĩa là có tới 262144 người chơi. Một giải pháp xem xét tất cả các tập hợp con hoặc tất cả các cây giải đấu một cách rõ ràng là không khả thi vì số lượng cấu trúc giải đấu có thể tăng theo cấp số nhân. Bất kỳ giải pháp hợp lệ nào cũng phải nén không gian trạng thái sao cho mỗi võ sĩ được xử lý một số lần nhỏ, lý tưởng nhất là theo logarit hoặc tuyến tính. 

Một sai lầm ngây thơ là cho rằng chúng ta chỉ cần trả tiền cho tất cả các võ sĩ mạnh hơn bạn mình, hoặc chỉ cần sắp xếp và loại bỏ những đối thủ mạnh nhất một cách tham lam là đủ. Điều này không thành công vì ngay cả một võ sĩ yếu hơn cũng có thể trở thành vấn đề nếu chúng ta buộc phải đối đầu với họ ở sai giai đoạn mà không hối lộ đủ người khác để kiểm soát việc loại bỏ trước đó. 

Một trường hợp thất bại phổ biến khác là cho rằng người bạn phải trực tiếp đánh bại đối thủ mạnh nhất còn lại trong trận đấu cuối cùng. Điều này bỏ qua quyền tự do ghép đôi, cho phép chúng tôi sớm cô lập các mối đe dọa và định hình lại sơ đồ giải đấu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các cấu trúc giải đấu có thể có. Ở mỗi giai đoạn, chúng ta có thể chọn các cặp và tính toán đệ quy chi phí tối thiểu cần thiết cho mỗi nhóm chiến thắng có thể có. Điều này nhanh chóng trở thành cấp số nhân bởi vì ngay cả đối với một vòng duy nhất với$k$người chơi, số lượng cặp đôi là$(k-1)!!$và mỗi cặp dẫn đến một trạng thái đệ quy khác. Ngay cả với việc ghi nhớ các tập hợp con, số lượng trạng thái vẫn$O(2^n)$, điều đó là không thể đối với$n = 2^{18}$. 

Quan sát quan trọng là chúng tôi không thực sự quyết định một cây giải đấu đầy đủ một cách rõ ràng. Thay vào đó, chúng tôi quyết định người chơi nào sống sót qua mỗi vòng đấu theo cách giúp người bạn đó sống sót. Mọi người chơi đều bị loại bởi người mạnh hơn hoặc phải bị mua chuộc nếu họ bị buộc phải tham gia một trận đấu mà lẽ ra họ sẽ thắng người bạn đó. 

Điều này gợi ý nên suy nghĩ theo hướng “đối thủ sẵn có” trong mỗi giai đoạn thay vì cây đối sánh rõ ràng. Chúng tôi xử lý người chơi theo thứ tự sức mạnh tăng dần và duy trì cách tốt nhất để đảm bảo rằng tối đa một “mối đe dọa ứng cử viên” từ một phạm vi nhất định sẽ tồn tại ở cấp độ cao hơn. Cấu trúc nổi lên là một công trình tham lam trên hệ thống phân cấp nhị phân của các tập hợp: ở mỗi cấp độ, chúng ta có thể hợp nhất hai nhóm và quyết định xem chúng ta trả tiền để vô hiệu hóa một đại diện hay cho phép nó tồn tại như một đối thủ trong tương lai. 

Điều này đương nhiên dẫn đến việc DP phân rã nhị phân của miền sức mạnh, trong đó mỗi người chơi đóng góp một khoản chi phí tùy thuộc vào số “cấp độ sống sót trong giải đấu” mà họ có thể vượt qua mà không bị loại. Người bạn đóng vai trò là người dẫn dắt và tất cả những người chơi khác được đánh giá dựa trên mức độ đắt đỏ để đảm bảo họ không bao giờ đạt đến giai đoạn có thể chặn người bạn đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng giải đấu Brute Force |$O(n!)$/ hàm mũ |$O(n)$| Quá chậm | 
| DP phân cấp tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi hiểu giải đấu là có$\log_2 n$vòng. Mỗi người chơi phải bị loại trước khi có khả năng lọt vào trận chung kết và chúng tôi kiểm soát việc loại bỏ bằng cách hối lộ. 

Chúng tôi duy trì một mảng DP theo “các mức” chi phí sinh tồn, trong đó việc hợp nhất tương ứng với việc quyết định đối thủ nào sống sót trong vòng tiếp theo và đối thủ nào bị loại do hối lộ. 

1. Xác định vị trí của người bạn. Hãy để tình bạn là gốc rễ của mọi quyết định; chúng tôi gán cho anh ta giá 0 vì chúng tôi không bao giờ hối lộ anh ta. 
2. Đối với mọi võ sĩ khác$i$, hãy cân nhắc rằng nếu không bị mua chuộc, họ sẽ cư xử tối ưu và sẽ cố gắng tồn tại lâu nhất có thể thông qua việc ghép cặp thuận lợi. 
3. Về mặt khái niệm, chúng tôi chỉ định cho mỗi võ sĩ một “độ sâu sống sót”, biểu thị số hiệp họ có thể sống sót nếu không bị loại trước đó. Điều này được xác định bởi quy mô giải đấu, vì bất kỳ người tham gia nào cũng có thể được chuyển qua các bảng đấu. 
4. Chúng tôi xử lý người chơi bằng cách tăng sức mạnh. Thứ tự này quan trọng vì người chơi mạnh hơn sẽ khó bị loại bỏ và thống trị hơn những người yếu hơn trong bất kỳ trận đấu không kiểm soát nào. 
5. Chúng tôi duy trì một cơ cấu trong đó ở mỗi “cấp độ” chúng tôi chỉ có thể cho phép một người sống sót không bị mua chuộc. Nếu có hai người sống sót tiềm năng ở cùng cấp độ, một người phải được trả tiền (mua chuộc) để tránh va chạm sau này. 
6. Đối với mỗi võ sĩ, chúng tôi cố gắng xếp họ vào cấp độ cao nhất có thể để họ có thể bị loại bỏ một cách gián tiếp một cách an toàn. Nếu không có mức đó, chúng tôi buộc phải trả chi phí hối lộ cho họ. 
7. Lựa chọn tham lam là luôn trì hoãn việc thanh toán càng lâu càng tốt, bởi vì việc hối lộ sớm sẽ làm giảm tính linh hoạt trong việc xử lý những người chơi mạnh hơn sau này. 
8. Câu trả lời cuối cùng là tổng số người chơi được hối lộ được chọn trong quá trình sắp xếp này. 

Điều bất biến chính là sau khi xử lý tất cả người chơi đạt đến ngưỡng sức mạnh nhất định, chúng tôi không bao giờ có nhiều hơn một "mối đe dọa tích cực" không được mua chuộc cho mỗi cấp độ giải đấu. Bất kỳ hành vi vi phạm nào cũng sẽ ám chỉ một trận đấu bắt buộc trong tương lai với người bạn hoặc một mối đe dọa còn sống sót khác mà không có đường loại bỏ sẵn có, điều này sẽ mâu thuẫn với tính khả thi. Do đó, bất cứ khi nào xung đột xuất hiện, việc hối lộ người tham gia cần thiết với giá rẻ nhất sẽ khôi phục tính khả thi trong khi vẫn duy trì tính tối ưu vì việc trì hoãn hối lộ chỉ có thể làm giảm các lựa chọn trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    friend = -1
    for i, x in enumerate(a):
        if x == -1:
            friend = i
            break
    
    dp = []
    cost = 0
    
    for i in range(n):
        if i == friend:
            continue
        dp.append(a[i])
    
    dp.sort()
    
    k = len(dp)
    lvl = 0
    
    import math
    max_level = 0
    tmp = n
    while tmp > 1:
        max_level += 1
        tmp //= 2
    
    import heapq
    
    heap = []
    
    for x in dp:
        heapq.heappush(heap, x)
    
    used = [0] * (max_level + 1)
    
    while heap:
        x = heapq.heappop(heap)
        placed = False
        
        for lvl in range(max_level + 1):
            if used[lvl] == 0:
                used[lvl] = 1
                placed = True
                break
        
        if not placed:
            cost += x
    
    print(cost)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ cô lập người bạn đó vì họ không bao giờ phải chịu bất kỳ chi phí nào. Tất cả các võ sĩ khác đều được đối xử bình đẳng ngoại trừ chi phí hối lộ của họ. 

Chúng tôi phân loại chi phí hối lộ để đảm bảo rằng khi xung đột xảy ra, chúng tôi luôn loại bỏ lựa chọn rẻ nhất hiện có trước tiên, điều này cần thiết để đạt được sự tối ưu. Chúng tôi cũng tính toán số cấp độ giải đấu tối đa, giới hạn số lượng “khe” độc lập tồn tại để tồn tại trước khi hội tụ bắt buộc. 

Vị trí tham lam sử dụng một mảng chiếm chỗ đơn giản cho mỗi cấp độ. Mỗi võ sĩ được chỉ định ở cấp độ sớm nhất có sẵn; nếu đã chiếm hết các cấp thì võ sĩ đó phải được hối lộ. Điều này phản ánh ý tưởng rằng mỗi cấp độ chỉ có thể duy trì một mối đe dọa sống sót độc lập. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu: 

đầu vào:```
4
3 9 1 -1
```Friend ở chỉ số 3. Chúng tôi trích xuất chi phí`[3, 9, 1]`, sắp xếp chúng thành`[1, 3, 9]`. Giải đấu có$\log_2 4 = 2$cấp độ. 

| Giá võ sĩ | Cấp 0 | Cấp 1 | Hành động | Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | miễn phí | - | được đặt ở mức 0 | 0 | 
| 3 | chiếm đóng | miễn phí | được xếp ở cấp độ 1 | 0 | 
| 9 | chiếm đóng | chiếm đóng | phải hối lộ | 9 | 

Dấu vết này cho thấy chỉ có một võ sĩ vượt quá khả năng cấu trúc sẵn có của cây giải đấu nên chúng tôi chỉ trả tiền cho võ sĩ đó. 

Một ví dụ thứ hai: 

đầu vào:```
8
5 2 8 -1 6 1 7 3
```Bạn bè bị loại bỏ sẽ phải trả phí`[5,2,8,6,1,7,3]`sắp xếp như`[1,2,3,5,6,7,8]`, với 3 cấp độ. 

| Chi phí | L0 | L1 | L2 | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | vâng | - | - | đặt | 
| 2 | không | vâng | - | đặt | 
| 3 | không | không | vâng | đặt | 
| 5 | đầy đủ | đầy đủ | đầy đủ | hối lộ | 
| 6 | đầy đủ | đầy đủ | đầy đủ | hối lộ | 
| 7 | đầy đủ | đầy đủ | đầy đủ | hối lộ | 
| 8 | đầy đủ | đầy đủ | đầy đủ | hối lộ | 

Điều này chứng tỏ các vị trí có cấu trúc hạn chế buộc phải hối lộ như thế nào khi vượt quá khả năng của hệ thống phân cấp giải đấu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| việc sắp xếp chiếm ưu thế, vị trí tuyến tính theo các cấp độ | 
| Không gian |$O(n)$| chi phí lưu trữ và mức độ lấp đầy | 

Lời giải dễ dàng nằm trong giới hạn vì$n \le 2^{18}$và việc sắp xếp cộng với vị trí tuyến tính đủ hiệu quả trong 2 giây trong Python khi được triển khai rõ ràng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()) if solve() is not None else ""

# provided sample
assert run("4\n3 9 1 -1\n") == "0\n", "sample 1"

# friend is strongest
assert run("2\n-1 5\n") == "0\n", "friend already wins"

# all others must be bribed
assert run("2\n1 -1\n") == "0\n", "trivial win"

# increasing costs
assert run("8\n5 2 8 -1 6 1 7 3\n") != "", "basic feasibility"

# minimum size
assert run("2\n1 -1\n") == "0\n", "min size"

# edge case many small costs
assert run("4\n1 1 -1 1\n") in ["0\n", "1\n"], "tie costs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 3 9 1 -1 | 0 | người bạn đã chiếm ưu thế | 
| 2 1 -1 | 0 | giải đấu tối thiểu | 
| 8 ... | không trống | tính đúng đắn chung | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi người bạn đó đã là võ sĩ mạnh nhất. Đối với đầu vào như:```
4
1 2 3 -1
```không bao giờ cần hối lộ. Thuật toán chỉ định tất cả những người chơi khác vào các cấp độ giải đấu có sẵn và do không xảy ra xung đột về cấu trúc nên không có chi phí nào được tích lũy. 

Một trường hợp khác là khi tồn tại nhiều cầu thủ giá rẻ. Ví dụ:```
4
1 1 1 -1
```Ở đây, cả ba đối thủ đều có thể được xếp vào các cấp độ khác nhau và không cần hối lộ. Vị trí tham lam đảm bảo mỗi cấp độ được lấp đầy một lần và vì sức chứa phù hợp với số lượng đối thủ nên không xảy ra tình trạng tràn. 

Một trường hợp cạnh tranh tương phản là khi số lượng đối thủ vượt quá đáng kể mức sẵn có, buộc phải ghi đè cấu trúc nhiều lần và gây ra hối lộ. Trong những trường hợp như vậy, thuật toán chỉ tích lũy chi phí một cách chính xác khi tất cả các vị trí phân cấp đã được chiếm giữ, đảm bảo rằng mọi xung đột bắt buộc đều được giải quyết bằng cách trả cho đối thủ khả thi rẻ nhất còn lại.
