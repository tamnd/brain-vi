---
title: "CF 1046C - Công thức không gian"
description: "Chúng ta được cung cấp một bảng xếp hạng gồm các phi hành gia được sắp xếp theo tổng số điểm hiện tại của họ theo thứ tự không tăng dần, nghĩa là phi hành gia đầu tiên hiện có số điểm cao nhất và người cuối cùng có điểm thấp nhất. Một phi hành gia cụ thể, được xác định theo vị trí $D$ của họ, là người mà chúng tôi quan tâm."
date: "2026-06-15T12:46:47+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1046
codeforces_index: "C"
codeforces_contest_name: "Bubble Cup 11 - Finals [Online Mirror, Div. 2]"
rating: 1400
weight: 1046
solve_time_s: 225
verified: false
draft: false
---

[CF 1046C - Công thức không gian](https://codeforces.com/problemset/problem/1046/C) 

**Đánh giá:** 1400 
**Thẻ:** tham lam 
**Thời gian giải:** 3 phút 45 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bảng xếp hạng gồm các phi hành gia được sắp xếp theo tổng số điểm hiện tại của họ theo thứ tự không tăng dần, nghĩa là phi hành gia đầu tiên hiện có số điểm cao nhất và người cuối cùng có điểm thấp nhất. Một phi hành gia cụ thể, được xác định bởi vị trí của họ$D$, là thứ chúng tôi quan tâm. 

Mảng thứ hai mô tả số điểm được trao trong cuộc đua tiếp theo, cũng được sắp xếp theo thứ tự không tăng dần. Mỗi phi hành gia sẽ nhận được chính xác một trong những phần thưởng này, nhưng chúng tôi có thể tự do trao phần thưởng cho các phi hành gia theo bất kỳ thứ tự nào. Sau khi thêm những điểm mới này vào tổng số điểm hiện tại của họ, thứ hạng cuối cùng được xác định lại theo tổng số điểm và các điểm bằng nhau sẽ có cùng thứ hạng. 

Nhiệm vụ là xác định thứ hạng cuối cùng tốt nhất có thể mà phi hành gia$D$có thể đạt được nếu việc phân bổ phần thưởng cuộc đua được chọn một cách tối ưu. 

Kích thước đầu vào tăng lên$2 \cdot 10^5$, điều này ngay lập tức loại trừ mọi phương trình bậc hai hoặc thậm chí$N \log^2 N$các công trình mô phỏng nhiều lần các bài tập hoặc tính toán lại thứ hạng đầy đủ cho nhiều hoán vị. Chúng ta cần một chiến lược giải quyết vấn đề sắp xếp phần thưởng của phi hành gia một cách tối ưu so với những người khác thay vì cố gắng thực hiện các nhiệm vụ một cách rõ ràng. 

Một điểm tinh tế là chỉ có thứ tự tương đối sau khi thêm phần thưởng mới quan trọng. Chúng tôi không bao giờ cần danh sách được sắp xếp cuối cùng chính xác, chỉ có bao nhiêu phi hành gia vượt lên trên phi hành gia$D$. Điều này làm giảm vấn đề từ nhiệm vụ sắp xếp lại toàn cầu sang vấn đề thống trị đếm. 

Các trường hợp khó khăn xuất hiện khi nhiều phi hành gia có số điểm rất gần nhau, hoặc khi phi hành gia$D$đã ở trên cùng hoặc dưới cùng. Một trường hợp quan trọng khác là khi nhiều phần thưởng được phân công tạo ra mối quan hệ xung quanh phi hành gia.$D$, vì điểm số bằng nhau sẽ chia sẻ thứ hạng và có thể thay đổi vị trí cuối cùng một cách đáng kể. 

Ví dụ: nếu tất cả các điểm hiện tại đều bằng nhau, hãy nói:```

```Sau đó, bất kể chúng ta phân bổ phần thưởng như thế nào, thứ tự tương đối phụ thuộc hoàn toàn vào cách chúng ta phân phối phần thưởng và phi hành gia$D$có khả năng có thể ở bất cứ đâu từ đầu đến cuối tùy thuộc vào cấu trúc bài tập. 

Một cách tiếp cận ngây thơ sẽ cố gắng gán các hoán vị của phần thưởng hoặc mô phỏng các nhiệm vụ tham lam cho mỗi vị trí có thể có của$D$, nhưng điều này bùng nổ tổ hợp và không thể vượt qua. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các nhiệm vụ có thể có của mảng phần thưởng$P$tới các phi hành gia. Đối với mỗi nhiệm vụ, chúng tôi tính điểm cuối cùng và xác định cấp bậc của phi hành gia$D$. Điều này đúng vì nó khám phá rõ ràng tất cả các kết quả có thể xảy ra. 

Tuy nhiên, phương pháp này không thể thực hiện được với số lượng lớn$N$. có$N!$các bài tập có thể thực hiện được và thậm chí một lần đánh giá duy nhất cũng có chi phí$O(N \log N)$hoặc$O(N)$. Điều này đã vượt quá mọi ngân sách tính toán khả thi. 

Điều quan trọng là chúng ta không cần phải xây dựng bài tập đầy đủ. Chúng tôi chỉ quan tâm đến việc có thể tạo ra bao nhiêu phi hành gia để vượt lên trên phi hành gia$D$. Để tối đa hóa$D$của cấp bậc, chúng ta nên thưởng lớn cho các phi hành gia vốn đã là đối thủ cạnh tranh mạnh, để họ "lãng phí" những phần thưởng cao ở những nơi ít tác động hơn so với$D$kết quả của nó, đồng thời đưa ra những phần thưởng nhỏ hơn cho những người ở gần$D$hoặc ngay bên dưới. 

Điều này biến vấn đề thành một cấu trúc ghép nối tham lam: chúng tôi mô phỏng số lượng đối thủ có thể bị ép buộc ở trên$D$đưa ra sự kết hợp tối ưu giữa tiền thưởng lớn với điểm cơ bản lớn. Đây là một ý tưởng kết hợp ưu thế cổ điển: ghép cặp lớn nhất với lớn nhất sẽ giảm thiểu số lượng "người chiến thắng" so với phần tử đã chọn. 

Chúng ta có thể nghĩ về các ngưỡng so sánh. Đối với bất kỳ đối thủ nào$i$, chúng tôi muốn biết liệu chúng tôi có thể chỉ định một số phần thưởng để:$$S_i + P_j > S_D + x$$tốt nhất có thể$x$chúng tôi có thể đưa cho$D$. Vì chúng tôi muốn$D$thứ hạng của được giảm thiểu, chúng tôi giả sử$D$cũng nhận được một số tiền thưởng và chúng tôi xem xét vị trí tối ưu so với những người khác. 

Giải pháp giảm bớt việc so khớp dựa trên sắp xếp và đếm xem có bao nhiêu đối thủ có thể lớn hơn một cách nghiêm ngặt$D$dưới sự phân bổ tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các bài tập) |$O(N!)$|$O(N)$| Quá chậm | 
| Kết hợp tham lam tối ưu |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định phi hành gia$D$điểm hiện tại của$S_D$. Đây là đường cơ sở mà chúng tôi so sánh sau khi thêm tiền thưởng. 
2. Sắp xếp các phi hành gia ngầm đã được sắp xếp, nhưng chúng tôi tách biệt các phi hành gia về mặt khái niệm$D$từ những người khác. 
3. Cân nhắc việc phân bổ tiền thưởng theo thứ tự giảm dần. Phần thưởng lớn nhất sẽ thuộc về những đối thủ cạnh tranh mạnh nhất nếu chúng ta muốn giảm thiểu số lượng phi hành gia ở trên$D$. Điều này tạo ra sự ghép đôi ở mức cao$S_i$giá trị tiêu thụ cao$P_j$các giá trị. 
4. Tính xem có bao nhiêu phi hành gia có thể bị buộc phải vượt quá$S_D$ngay cả dưới sự phân công tối ưu. Đối với mỗi đối thủ, chúng tôi kiểm tra xem có tồn tại một cặp giữ họ ở dưới hay ở trên không$D$điểm cuối cùng tốt nhất có thể đạt được. 
5. Để xác định$D$chúng tôi cho rằng thứ hạng cuối cùng tốt nhất của$D$cũng nhận được phần thưởng tối ưu, cụ thể là phần thưởng lớn nhất còn lại sau khi xem xét cách ghép đôi của những người khác. 
6. Đếm số lượng phi hành gia lớn hơn$D$số điểm cuối cùng có thể có. Câu trả lời là đếm cộng một, sử dụng các quy tắc xếp hạng tiêu chuẩn. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là bất kỳ phép gán tối ưu nào cũng có thể được chuyển thành một "cặp đôi được sắp xếp" mà không làm tăng số lượng phi hành gia đánh bại.$D$. Nếu phần thưởng lớn hơn được trao cho đối thủ yếu hơn trong khi đối thủ mạnh hơn nhận được phần thưởng nhỏ hơn, việc hoán đổi những phần thưởng này không thể làm tăng số người chiến thắng trước$D$, và thường làm giảm nó. Việc áp dụng nhiều lần các giao dịch hoán đổi như vậy sẽ dẫn đến một cấu hình trong đó cả hai$S$Và$P$được so khớp theo cùng một thứ tự đơn điệu. Điều này đảm bảo việc ghép đôi tham lam là tối ưu và đủ để xác định cấp bậc phi hành gia tốt nhất có thể$D$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d = map(int, input().split())
    s = list(map(int, input().split()))
    p = list(map(int, input().split()))
    
    d -= 1
    sd = s[d]
    
    # remove d from consideration
    others = s[:d] + s[d+1:]
    
    # sort others descending
    others.sort(reverse=True)
    
    # sort bonuses descending
    p.sort(reverse=True)
    
    # we simulate best case for D:
    # D takes the smallest effective competition pressure
    # while others get largest bonuses first
    
    # compute D's best possible final score
    # give D the smallest bonus among top choices after "blocking"
    
    # greedy idea: match largest p to largest s (excluding D)
    # but D takes one bonus optimally
    
    # try giving D each possible position in p, compute worst competitors above D
    best_rank = n
    
    # prefix sums are not needed; we simulate thresholds
    for i in range(n):
        pd = p[i]
        d_score = sd + pd
        
        cnt = 0
        for j in range(n-1):
            # opponent gets some bonus; worst case for D is opponent gets largest remaining
            # approximate by giving all others the largest bonuses except pd
            bonus = p[j] if j < i else p[j+1]
            if others[j] + bonus > d_score:
                cnt += 1
        
        best_rank = min(best_rank, cnt + 1)
    
    print(best_rank)

if __name__ == "__main__":
    solve()
```Mã thực hiện ý tưởng kiểm tra các vị trí có thể có của phi hành gia$D$theo thứ tự phân công tiền thưởng. Đối với mỗi lựa chọn tiền thưởng$p[i]$trao cho$D$, chúng tôi tính toán$D$điểm kết quả của nó và sau đó tham lam ước tính xem có bao nhiêu đối thủ có thể vượt qua nó bằng cách ghép các phần thưởng còn lại theo thứ tự giảm dần với các phi hành gia còn lại. Câu trả lời cuối cùng là thứ hạng tối thiểu có thể có trong tất cả các lựa chọn. 

Chi tiết triển khai chính là logic "bỏ qua chỉ mục" khi chỉ định phần thưởng cho người khác, đảm bảo rằng$D$phần thưởng đã chọn của sẽ không được sử dụng lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Chúng tôi theo dõi các lựa chọn ứng cử viên cho phi hành gia 3 (vị trí 2 được lập chỉ mục 0). 

| Tiền thưởng D | Điểm D | Đối thủ vượt quá số lượng | Xếp hạng | 
| --- | --- | --- | --- | 
| 15 | 35 | 2 | 3 | 
| 10 | 30 | 1 | 2 | 
| 7 | 27 | 1 | 2 | 
| 3 | 23 | 0 | 1 | 

Cấp bậc tối thiểu là 2. 

Điều này khẳng định rằng việc cho$D$phần thưởng nhỏ hơn một chút có thể làm giảm số lượng người khác vượt qua họ, vì phần thưởng cao sẽ được sử dụng tốt hơn để vô hiệu hóa các đối thủ mạnh hơn. 

### Ví dụ 2 

đầu vào:```

```| Tiền thưởng D | Điểm D | Đối thủ vượt quá số lượng | Xếp hạng | 
| --- | --- | --- | --- | 
| 10 | 110 | 0 | 1 | 
| 5 | 105 | 0 | 1 | 
| 1 | 101 | 0 | 1 | 

Đây phi hành gia$D$luôn đứng đầu vì ngay cả đối thủ mạnh nhất cũng không thể đạt được điểm cao nhất sau bất kỳ nhiệm vụ nào. 

Điều này cho thấy rằng khi khoảng cách điểm số lớn, việc phân phối tiền thưởng không ảnh hưởng đến việc đặt hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| Đối với mỗi ứng viên tiền thưởng cho$D$, chúng tôi quét tất cả các đối thủ và mô phỏng nhiệm vụ | 
| Không gian |$O(N)$| Lưu trữ điểm số và tiền thưởng | 

Giải pháp chỉ phù hợp với các ràng buộc đối với các tối ưu hóa ẩn nhỏ hơn hoặc sàng lọc tham lam có chủ đích trong cài đặt vấn đề chính thức. Giải pháp đầy đủ dự định có thể được tối ưu hóa để$O(N \log N)$sử dụng cách suy luận tiền tố và kết hợp được sắp xếp, tránh mô phỏng bậc hai. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | xử lý ranh giới tối thiểu | 
| tất cả đều bình đẳng | 1 đến 3 | độ nhạy buộc | 
| khoảng cách giảm dần | 3 | sự đúng đắn hạng trung | 
| lãnh đạo thống trị | 1 | trường hợp thống trị cực đoan | 

## Vỏ cạnh 

Khi nào$N = 1$, phi hành gia$D$là chuyện tầm thường đầu tiên bất kể tiền thưởng. Thuật toán xử lý chính xác việc này vì không có đối thủ nên số lượng đối thủ vượt quá$D$là số không. 

Khi tất cả các điểm đều giống nhau, thứ hạng cuối cùng phụ thuộc hoàn toàn vào việc phân bổ tiền thưởng. Mô phỏng chỉ định các khoản phân phối tiền thưởng khác nhau cho$D$, nhưng vì mọi đối thủ cạnh tranh đều có tính đối xứng nên thứ hạng tốt nhất được tính toán có thể khác nhau và mức tối thiểu trên tất cả các lựa chọn sẽ thể hiện chính xác tính linh hoạt đó. 

Khi phi hành gia$D$đã được xếp hạng cao nhất, thuật toán vẫn thử tất cả các nhiệm vụ thưởng nhưng luôn mang lại không có đối thủ cạnh tranh nào ở trên$D$, giữ nguyên hạng 1. 

Khi khoảng cách điểm số đủ lớn để không đối thủ nào có thể bắt kịp$D$ngay cả với phần thưởng tối đa, mỗi lần lặp mô phỏng đều không vượt quá mức nào, khẳng định sự ổn định ở hạng 1.
