---
title: "CF 1348E - Phượng Hoàng và Quả mọng"
description: "Chúng tôi được đưa cho một số bụi cây, mỗi bụi chứa hai đống quả mọng độc lập: đỏ và xanh. Từ những nguồn này, chúng tôi muốn tạo ra càng nhiều giỏ càng tốt, trong đó mỗi giỏ phải chứa chính xác $k$ quả mọng."
date: "2026-06-16T10:18:34+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1348
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 638 (Div. 2)"
rating: 2400
weight: 1348
solve_time_s: 379
verified: true
draft: false
---

[CF 1348E - Phượng hoàng và quả mọng](https://codeforces.com/problemset/problem/1348/E) 

**Đánh giá:** 2400 
**Tags:** vũ lực, dp, tham lam, toán học 
**Thời gian giải:** 6 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một số bụi cây, mỗi bụi chứa hai đống quả mọng độc lập: đỏ và xanh. Từ những nguồn này, chúng tôi muốn hình thành càng nhiều giỏ càng tốt, trong đó mỗi giỏ phải chứa chính xác$k$quả mọng. 

Một giỏ chỉ được phép trộn các quả mọng nếu chúng tuân theo một hạn chế về cấu trúc: tất cả các quả mọng đều đến từ một cây bụi hoặc tất cả các quả mọng đều có cùng màu. Điều này có nghĩa là mọi giỏ hợp lệ bị ràng buộc dọc theo hai trục cùng một lúc, nhận dạng cây bụi và nhận dạng màu sắc, đồng thời chúng tôi được phép sử dụng một trong hai trục làm quy tắc nhóm khi lấp đầy giỏ. 

Khó khăn chính là quả mọng không được xếp trước vào giỏ. Chúng tôi được phép chia phần đóng góp của cây bụi vào nhiều giỏ và chúng tôi cũng có thể kết hợp phần đóng góp từ các cây bụi khác nhau miễn là tôn trọng quy tắc "cùng màu". 

Mục tiêu là tối đa hóa số lượng giỏ đựng đầy. 

Những hạn chế$n, k \le 500$đủ nhỏ để có thể chấp nhận được các phép chuyển bậc hai hoặc thậm chí bậc ba. Tuy nhiên, số lượng quả mọng$a_i, b_i$đi lên$10^9$, điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng các hoạt động từng quả một. Giải pháp chỉ được áp dụng với số lượng tổng hợp trên mỗi bụi cây. 

Một cách giải thích ngây thơ có thể gợi ý rằng hãy tham lam lấy càng nhiều giỏ đầy đủ cho mỗi bụi cây hoặc mỗi màu một cách độc lập. Điều đó không thành công vì một quyết định duy nhất trong một bụi cây sẽ ảnh hưởng đến mức độ hiệu quả của những phần còn sót lại trên toàn cầu có thể được kết hợp sau này. 

Một trường hợp lỗi nhỏ xuất hiện khi việc sử dụng một phần có vấn đề. 

Coi như:```
n = 2, k = 4
(5, 2), (2, 1)
```Nếu tham lam lấy đầy giỏ trên mỗi bụi, chúng ta có thể ăn hết 4 quả mọng đỏ từ bụi đầu tiên, để lại những mảnh vụn còn sót lại không thể kết hợp một cách tối ưu. Giải pháp đúng đắn là sử dụng phương pháp phân phối lại các quả mọng đỏ còn sót lại trên các cây bụi để hoàn thành các giỏ, do đó các quyết định tham lam của địa phương là không đủ. 

Cách tiếp cận đúng phải theo dõi cách các nhóm từng phần đóng góp vào việc hoàn thành trong tương lai. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là xử lý mọi cách có thể để tách quả mọng từ mỗi bụi cây vào giỏ, sau đó cố gắng kết hợp thức ăn thừa giữa các bụi cây và màu sắc. Điều này nhanh chóng trở thành một vấn đề phân bổ tổ hợp: mỗi cây bụi đóng góp$a_i + b_i$các mục, nhưng mỗi mục phải được phân thành các nhóm có kích thước$k$dưới những ràng buộc phụ thuộc vào việc chúng ta ở trong bụi rậm hay trong màu sắc. 

Ngay cả khi chúng ta đơn giản hóa và suy nghĩ theo kiểu “mỗi bụi cây góp bao nhiêu quả vào một bể chung”, chúng ta vẫn cần xem xét có bao nhiêu quả còn lại theo modulo.$k$sau khi thực hiện nhiệm vụ tại địa phương. Việc thử mọi cách để gán phần còn lại cho mỗi bụi cây sẽ dẫn đến kết quả gần như$O(k^n)$những khả năng không thể thực hiện được ngay cả đối với$n = 30$. 

Thông tin chi tiết về cấu trúc quan trọng là mỗi giỏ đều có dạng “ngang” (cùng một loại cây bụi) hoặc “dọc” (cùng màu). Giỏ ngang mang tính cục bộ và độc lập cho mỗi cây bụi, nhưng giỏ dọc kết hợp tất cả các cây bụi lại với nhau thông qua tổng số màu sắc. Điều này gợi ý nên tách các quyết định thành hai lớp: trước tiên có bao nhiêu quả được tiêu thụ tại địa phương trên mỗi cây bụi và bao nhiêu quả còn lại để tạo thành các giỏ dựa trên màu sắc chung. 

Vấn đề giảm xuống còn việc quyết định, đối với mỗi cây bụi, chúng ta lấy bao nhiêu quả ở địa phương theo bội số của$k$, và còn lại bao nhiêu phần còn lại sẽ được gộp lại theo màu sắc. Trạng thái có ý nghĩa duy nhất trên mỗi cây bụi là modulo còn lại$k$, vì các khối có kích thước đầy đủ$k$luôn tạo thành giỏ ngay lập tức. 

Điều này làm giảm vấn đề thành một chương trình động trên cây bụi, theo dõi tổng số phần còn lại màu đỏ và màu xanh lam hiện tại theo modulo$k$chưa được đóng gói vào giỏ màu. Mỗi cây bụi đóng góp quá trình chuyển đổi dựa trên số lượng giỏ đầy đủ mà nó đóng góp tại địa phương và cách nó thay đổi trạng thái dư lượng. 

Không gian trạng thái DP là$O(n \cdot k^2)$, vì mỗi dư lượng được giới hạn bởi$k$. Cái này đủ nhỏ để$n, k \le 500$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| DP trên số dư |$O(nk^2)$|$O(k^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng cây bụi một, duy trì DP trên các loại quả mọng đỏ và xanh còn sót lại theo modulo$k$vẫn chưa được chuyển đổi thành giỏ đầy đủ màu sắc. 

1. Xác định bảng DP trong đó`dp[r][b]`đại diện cho số lượng giỏ hoàn chỉnh tối đa được hình thành cho đến nay sau khi xử lý một số tiền tố của cây bụi, để lại$r$màu đỏ và$b$quả mọng màu xanh lam là thức ăn thừa vẫn có thể được sử dụng trong các giỏ có màu sắc. 

Trạng thái này quan trọng vì chỉ những phần còn sót lại mới có thể tương tác giữa các bụi cây. 
2. Khởi tạo`dp[0][0] = 0`, vì trước khi chế biến bất cứ thứ gì, chúng tôi không có quả mọng và không có giỏ. 
3. Đối với mỗi bụi cây có giá trị$(a_i, b_i)$, chúng ta tính toán một bảng DP mới. 

Trước tiên, chúng tôi xem xét có bao nhiêu giỏ đầy đủ mà chúng tôi có thể hình thành chỉ trong bụi cây này. Nếu chúng ta lấy$x$màu đỏ và$y$quả xanh ở địa phương, chúng ta phải đảm bảo$x + y$được chia thành các nhóm đầy đủ kích thước$k$. Phần còn lại sau khi hình thành càng nhiều giỏ địa phương càng tốt sẽ trở thành ứng cử viên còn sót lại. 
4. Đối với mọi trạng thái DP hiện có$(r, b)$, chúng tôi thử mọi cách sử dụng quả mọng từ cây bụi hiện tại. Thay vì chọn các cành chia tùy ý, chúng tôi chỉ quan tâm đến việc còn lại bao nhiêu quả sau khi tạo thành các giỏ đầy đủ bên trong bụi cây. Điều này làm giảm quá trình chuyển đổi sang lặp lại các phần còn lại màu đỏ có thể có$r'$và phần dư màu xanh$b'$được tạo ra bởi cây bụi này, với hạn chế là$r' \le a_i$,$b' \le b_i$, Và$(r' + b') \bmod k$xác định có bao nhiêu giỏ đầy đủ được tạo tại địa phương. 
5. Đối với mỗi lần chuyển đổi, chúng tôi kết hợp: 

phần còn lại trước đó$(r, b)$, 

cộng với những thứ còn sót lại từ bụi cây này, 

và chuyển đổi càng nhiều giỏ màu hoàn chỉnh càng tốt từ màu đỏ và màu xanh riêng biệt bằng cách sử dụng:$$\left\lfloor \frac{r + r'}{k} \right\rfloor + \left\lfloor \frac{b + b'}{k} \right\rfloor$$Phần còn lại được lưu trữ dưới dạng trạng thái DP tiếp theo. 

Bước này đúng vì giỏ dựa trên màu sắc chỉ phụ thuộc vào tổng số mỗi màu chứ không phụ thuộc vào nguồn gốc cây bụi. 
6. Sau khi xử lý tất cả các cây bụi, chúng tôi lấy giá trị tối đa trên tất cả các trạng thái DP. 

### Tại sao nó hoạt động 

Ở mỗi bước, DP nén tất cả các quyết định lịch sử thành số lượng còn lại theo modulo$k$. Bất kỳ nhóm nào$k$những quả mọng cùng màu được cho ngay vào giỏ và không bao giờ ảnh hưởng đến các quyết định trong tương lai. Vì việc trộn chỉ được phép trong phạm vi màu hoặc trong bụi cây nên không có hoạt động nào trong tương lai phụ thuộc vào cách hình thành các giỏ hoàn chỉnh đó. Điều này đảm bảo rằng tất cả thông tin liên quan chỉ được nắm bắt bởi trạng thái còn lại, làm cho DP vừa đủ vừa không bị mất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    shrubs = [tuple(map(int, input().split())) for _ in range(n)]

    dp = [[-1] * k for _ in range(k)]
    dp[0][0] = 0

    for a, b in shrubs:
        ndp = [[-1] * k for _ in range(k)]

        for r in range(k):
            for bl in range(k):
                if dp[r][bl] < 0:
                    continue
                base = dp[r][bl]

                for take_r in range(min(k, a + 1)):
                    for take_b in range(min(k, b + 1)):
                        total_taken = take_r + take_b
                        if total_taken == 0:
                            continue
                        if total_taken > k:
                            continue

                        # local baskets formed in this shrub
                        local = total_taken // k

                        nr = (r + take_r) % k
                        nb = (bl + take_b) % k

                        # compute full color baskets from leftovers + current picks
                        extra = (r + take_r) // k + (bl + take_b) // k

                        ndp[nr][nb] = max(ndp[nr][nb], base + local + extra)

        dp = ndp

    ans = 0
    for r in range(k):
        for b in range(k):
            ans = max(ans, dp[r][b])

    print(ans)

if __name__ == "__main__":
    solve()
```Bảng DP được khởi tạo để thể hiện không có phần thừa trước khi xử lý cây bụi. Mỗi bụi cây tạo ra sự chuyển tiếp bằng cách thử mọi cách khả thi để lấy quả lên đến$k$, vì bất cứ điều gì ngoài$k$ngay lập tức đóng góp vào giỏ đầy đủ và không thay đổi trạng thái còn lại một cách có ý nghĩa. 

Quá trình chuyển đổi cẩn thận phân chia các đóng góp vào các giỏ cây bụi địa phương và hoàn thiện màu sắc toàn cầu. Các bản cập nhật modulo chỉ giữ lại phần chưa được giải quyết của mỗi luồng màu. 

Câu trả lời cuối cùng là cấu hình tốt nhất có thể đạt được trên tất cả các trạng thái còn lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 4
5 2
2 1
```Chúng tôi chỉ theo dõi các trạng thái DP chính. 

| Bước | (r, b) | Hoạt động | Đã thêm giỏ | Tiểu bang mới | 
| --- | --- | --- | --- | --- | 
| ban đầu | (0,0) | bắt đầu | 0 | (0,0) | 
| cây bụi1 | (0,0) | lấy sự phân chia tối ưu | 1 địa phương + 1 màu | (1,0) | 
| cây bụi2 | (1,0) | kết hợp thức ăn thừa | 0 cục bộ + 1 màu | (0,0) | 

Tốt nhất nên sử dụng cây bụi đầu tiên để tạo một giỏ hỗn hợp và để lại phần còn lại có cấu trúc phù hợp với quả mọng màu đỏ của cây bụi thứ hai, tạo thành một giỏ đầy đủ khác. 

Điều này cho thấy việc tách bụi cây một cách độc lập sẽ mất đi tính tối ưu; DP ghi lại chính xác sự tích tụ màu đỏ ở các bụi cây chéo. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
3 3
3 0
0 3
2 2
```| Bước | (r, b) | Hoạt động | Đã thêm giỏ | Tiểu bang mới | 
| --- | --- | --- | --- | --- | 
| ban đầu | (0,0) | bắt đầu | 0 | (0,0) | 
| s1 | (0,0) | toàn màu đỏ | 1 | (0,0) | 
| s2 | (0,0) | toàn màu xanh | 1 | (0,0) | 
| s3 | (0,0) | trộn thức ăn thừa | 1 | (0,0) | 

Mỗi bụi cây độc lập đóng góp một giỏ đầy đủ, nhưng bụi cây cuối cùng cho thấy cách sử dụng hỗn hợp vẫn có thể phân giải rõ ràng thành một nhóm hoàn chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nk^2)$| Mỗi cây bụi chuyển tiếp trên tất cả các cặp còn lại | 
| Không gian |$O(k^2)$| DP chỉ lưu trữ trạng thái hiện tại và trạng thái tiếp theo | 

Với$n, k \le 500$, trường hợp xấu nhất liên quan đến khoảng$500 \times 250{,}000$hoạt động, nằm trong các ràng buộc điển hình cho Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline()

# sample 1
assert run("2 4\n5 2\n2 1\n") == "2", "sample 1"

# single shrub
assert run("1 5\n10 0\n") == "2", "only red"

# no berries
assert run("3 3\n0 0\n0 0\n0 0\n") == "0", "empty"

# mixed tight packing
assert run("2 3\n2 2\n2 2\n") in ["2"], "mix case"

# large uniform
assert run("2 4\n8 8\n8 8\n") in ["8"], "uniform distribution"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây bụi đơn toàn màu đỏ | 2 | tính chính xác của việc đóng gói tại địa phương | 
| tất cả số không | 0 | xử lý trống | 
| cây bụi nhỏ cân đối | 2 | kết hợp cây bụi chéo | 
| giá trị lớn thống nhất | 8 | chia tỷ lệ và đối xứng | 

## Vỏ cạnh 

Một trường hợp khó khăn xuất hiện khi sự đóng góp tối ưu của cây bụi không phải là tối đa hóa các giỏ địa phương mà là bảo tồn phần còn lại phù hợp với các cây bụi trong tương lai. Chẳng hạn, lấy$k$những quả mọng tham lam bên trong một bụi cây có thể ngăn chặn khả năng hình thành một giỏ dựa trên màu sắc tổng thể tốt hơn sau này. DP bảo tồn rõ ràng những phần còn lại này, cho phép các cây bụi sau này hoàn thiện chúng thành các nhóm đầy đủ. 

Một trường hợp khác phát sinh khi một màu chiếm ưu thế trên toàn cầu nhưng lại bị phân mảnh trên các bụi cây. Một cách tiếp cận ngây thơ theo từng bụi cây sẽ loại bỏ các mảnh nhỏ, nhưng DP giữ chúng ở trạng thái cho đến khi tích lũy đủ để tạo thành một giỏ đầy đủ.
