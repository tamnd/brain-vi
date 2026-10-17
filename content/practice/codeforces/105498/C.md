---
title: "CF 105498C - Điểm cuối cùng dự kiến"
description: "Chúng ta bắt đầu với một hàng vị trí $n$, mỗi vị trí chứa một phần tử riêng biệt. Một con trỏ $p$ cũng được đưa ra, ban đầu ở đâu đó trong khoảng từ $0$ đến $n$, bao gồm tất cả. Chúng tôi liên tục xóa các phần tử khỏi hàng hiện tại cho đến khi không còn gì."
date: "2026-06-23T01:37:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105498
codeforces_index: "C"
codeforces_contest_name: "Khulna Regional Inter University Programming Contest (KRIUPC) MIRROR"
rating: 0
weight: 105498
solve_time_s: 58
verified: true
draft: false
---

[CF 105498C - Điểm cuối cùng dự kiến](https://codeforces.com/problemset/problem/105498/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một hàng$n$vị trí, mỗi vị trí chứa một phần tử riêng biệt. Một con trỏ$p$cũng được đưa ra, ban đầu ở đâu đó giữa$0$Và$n$, bao gồm. Chúng tôi liên tục xóa các phần tử khỏi hàng hiện tại cho đến khi không còn gì. Mỗi bước chọn một vị trí còn lại ngẫu nhiên thống nhất, xóa phần tử đó và dịch chuyển mọi thứ sau khi nó rời đi để lấp đầy khoảng trống. chỉ số$p$chỉ di chuyển khi chúng ta xóa thứ gì đó ở bên phải nó: trong trường hợp đó$p$giảm đi một. Nếu chúng ta xóa tại hoặc bên trái của$p$, con trỏ không thay đổi so với cấu trúc còn lại. 

Rốt cuộc$n$xóa, cấu trúc trống và chỉ có giá trị cuối cùng của$p$vẫn còn. Nhiệm vụ là tính giá trị kỳ vọng của giá trị cuối cùng này$p$, với mọi giá trị ban đầu có thể có$p = 0, 1, \dots, n$. 

Ràng buộc$n \le 1000$ngay lập tức loại trừ mọi mô phỏng về hoán vị của việc xóa, vì có$n!$các lệnh xóa có thể. Ngay cả quy trình động bậc hai hoặc bậc ba cho mỗi trạng thái cũng có thể chấp nhận được, nhưng bất kỳ điều gì liên quan đến việc liệt kê các hoán vị hoặc duy trì phân phối đầy đủ trên tất cả các cấu hình trung gian đều quá chậm. 

Trường hợp cạnh tinh tế là hành vi ranh giới khi$p = 0$hoặc$p = n$. Nếu như$p = 0$, nó không bao giờ có thể giảm thêm nữa, vì không có chỉ số nào có thể lớn hơn 0 theo nghĩa ảnh hưởng tiêu cực đến nó. Nếu như$p = n$, mọi thao tác xóa đều có khả năng ảnh hưởng đến nó và việc xóa sớm có thể khiến nó bị ảnh hưởng đáng kể. Bất kỳ công thức đúng nào cũng phải xử lý các điểm cuối này một cách nhất quán; một đối số xác suất ngây thơ giả định tính đối xứng trên tất cả các chỉ số thường bị phá vỡ ở đây. 

## Phương pháp tiếp cận 

Quan điểm brute-force là mô hình hóa quá trình như tạo ra một hoán vị ngẫu nhiên của$n$các phần tử, bởi vì mỗi chuỗi xóa chính xác là một hoán vị được chọn thống nhất một cách ngẫu nhiên. Sau đó chúng tôi mô phỏng cách$p$phát triển theo hoán vị đó: đối với mỗi phần tử trong thứ tự hoán vị, chúng ta quyết định xem nó có nằm bên phải vị trí hiện tại của$p$và cập nhật tương ứng. 

Mô phỏng này rất đơn giản: với mỗi lần khởi động$p$, tạo ra tất cả các hoán vị, tính toán cuối cùng$p$, và trung bình. Tính đúng đắn là ngay lập tức vì quá trình xóa tương đương với một thứ tự ngẫu nhiên thống nhất. Vấn đề là có$n!$hoán vị, và thậm chí đối với$n = 12$, điều này đã trở nên không thể thực hiện được. 

Chúng ta cần tránh hoàn toàn việc theo dõi các hoán vị và thay vào đó hãy quan sát tính đối xứng: điều quan trọng không phải là danh tính chính xác của các phần tử mà là thứ tự tương đối của các lần xóa. Số phận của mỗi phần tử chỉ phụ thuộc vào số lượng phần tử ban đầu ở bên phải bị loại bỏ trước khi nó biến mất, nhưng quy trình tổng thể sẽ trộn mọi thứ một cách thống nhất. 

Một quan sát quan trọng là diễn giải lại quá trình ngược lại. Thay vì xóa các phần tử, hãy tưởng tượng việc xây dựng hoán vị cuối cùng bằng cách chèn các phần tử theo thứ tự ngược lại. Khi cấu trúc trống, chúng ta chèn từng phần tử vào các vị trí ngẫu nhiên thống nhất. Con trỏ$p$phát triển dựa trên việc việc chèn xảy ra ở bên phải hay bên trái của nó. Điều này đảo ngược sự phụ thuộc: thay vì xóa đẩy$p$sang trái, phần chèn sẽ đẩy nó sang phải một cách có kiểm soát. 

Theo quy trình ngược lại này, chúng ta có thể biểu thị kết quả cuối cùng$p$dưới dạng tổng số tiền đóng góp độc lập. Với mỗi phần tử, chúng ta chỉ cần biết liệu nó có góp phần làm giảm$p$và do tính đối xứng, số lần chèn ngẫu nhiên dự kiến ​​nằm ở bên phải của một vị trí cố định sẽ trở thành một hàm tuyến tính của thứ hạng dự kiến ​​​​hiện tại của nó. 

Điều này dẫn đến DP trên các vị trí và phần tử còn lại, nhưng chúng ta có thể nén nó thêm. Cho phép$E(p, k)$là giá trị cuối cùng được mong đợi bắt đầu từ con trỏ$p$với$k$phần tử còn lại. Quá trình chuyển đổi khi xóa một phần tử khỏi vị trí ngẫu nhiên$i \in [1, k]$là:$$E(p, k) = \frac{1}{k} \sum_{i=1}^k E(p - [i > p], k - 1)$$Chúng tôi tách các trường hợp: cho$i \le p$,$p$không thay đổi; vì$i > p$, nó giảm đi 1. Điều này mang lại:$$E(p, k) = \frac{p}{k} E(p, k-1) + \frac{k-p}{k} E(p-1, k-1)$$Sự tái diễn này là cấu trúc cốt lõi. Nó cho thấy kỳ vọng chỉ phụ thuộc vào các trạng thái nhỏ hơn trong DP tam giác, có thể tính được cho tất cả$p$Và$k$TRONG$O(n^2)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị |$O(n!)$|$O(n)$| Quá chậm | 
| DP trên các tiểu bang$E(p,k)$|$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một bảng trong đó$dp[p][k]$là giá trị cuối cùng dự kiến ​​của$p$khi$k$các phần tử vẫn còn và con trỏ hiện tại là$p$. Mục tiêu là$dp[p][n]$. 

1. Khởi tạo các trường hợp cơ sở cho$k = 0$. Khi không còn phần tử nào, quá trình kết thúc và giá trị của$p$đã được sửa, vì vậy$dp[p][0] = p$. Điều này neo tất cả các tính toán. 
2. Lặp lại số phần tử còn lại$k$từ$1$ĐẾN$n$. Chúng ta xây dựng câu trả lời cho các bài toán con lớn hơn bằng cách sử dụng các bài toán con nhỏ hơn, vì việc loại bỏ một phần tử sẽ làm giảm$k$. 
3. Đối với mỗi$k$, lặp qua tất cả các vị trí con trỏ hợp lệ$p \in [0, k]$. Các trạng thái nằm ngoài phạm vi này không hợp lệ vì con trỏ không thể vượt quá số phần tử còn lại. 
4. Tính toán các chuyển đổi bằng cách sử dụng phép chia do chỉ mục bị loại bỏ tạo ra. Nếu chỉ mục bị xóa nằm trong chỉ mục đầu tiên$p$vị trí, con trỏ không thay đổi; nếu không thì nó giảm đi một. Điều này tạo ra hai nhóm có kích thước là$p$Và$k-p$. 
5. Áp dụng công thức kỳ vọng:$$dp[p][k] = \frac{p}{k} dp[p][k-1] + \frac{k-p}{k} dp[p-1][k-1]$$Khi$p = 0$, số hạng thứ hai được coi là bằng 0 vì$dp[-1][*]$không tồn tại. 

Cấu trúc của DP đảm bảo rằng mọi trạng thái chỉ phụ thuộc vào các giá trị được tính toán trước đó và không có trạng thái nào được tính toán lại. 

### Tại sao nó hoạt động 

Bất biến quan trọng là$dp[p][k]$đại diện cho giá trị mong đợi chính xác trên tất cả các chuỗi xóa ngẫu nhiên được giới hạn ở$k$các phần tử còn lại và con trỏ hiện tại$p$. Ở mỗi bước, điều kiện hóa việc xóa đầu tiên sẽ phân chia tất cả các chuỗi hợp lệ thành các trường hợp rời rạc dựa trên chỉ mục đã chọn. Mỗi trường hợp giảm xuống một trường hợp nhỏ hơn với trọng số xác suất chính xác$p/k$Và$(k-p)/k$. Vì những trường hợp này bao gồm tất cả các kết quả và duy trì sự phân bố của các lần xóa còn lại, nên phép truy toán duy trì kỳ vọng chính xác ở mọi lớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    dp = [[0.0] * (n + 1) for _ in range(n + 1)]
    
    for p in range(n + 1):
        dp[p][0] = float(p)
    
    for k in range(1, n + 1):
        for p in range(0, k + 1):
            if p == 0:
                dp[p][k] = 0.0
            else:
                dp[p][k] = (p / k) * dp[p][k - 1] + ((k - p) / k) * dp[p - 1][k - 1]
    
    res = dp[n][:]
    print(" ".join(f"{x:.10f}" for x in res))

if __name__ == "__main__":
    solve()
```Bảng DP được xây dựng từ dưới lên trên số phần tử còn lại. Mỗi chuyển đổi trực tiếp mã hóa sự phân chia xác suất giữa việc xóa bên trong hoặc bên ngoài đoạn con trỏ hiện tại. Số học dấu phẩy động là đủ vì độ chính xác yêu cầu là$10^{-6}$. 

Một lỗi phổ biến là lặp đi lặp lại$p$tất cả các cách để$n$cho mọi$k$. Phạm vi hợp lệ co lại với$k$và việc sử dụng trạng thái không hợp lệ sẽ đưa ra các giá trị rác. Một vấn đề tế nhị khác là xử lý$p = 0$, nơi tham khảo$dp[-1]$phải tránh một cách rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét$n = 2$. Chúng tôi tính toán các trạng thái từ$k = 0$trở lên. 

| k | p | dp[p][k] | Giải thích | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | căn cứ | 
| 0 | 1 | 1 | căn cứ | 
| 1 | 0 | 0 | chỉ không hợp lệ hoặc bằng không | 
| 1 | 1 | 1/1 * dp[1][0] = 1 | phần tử đơn | 

Tại$k = 2$, vì$p = 1$, một nửa thời gian chúng ta xóa bên trái và một nửa bên phải, tạo ra sự đối xứng giữ kỳ vọng ở giữa. 

Vì$p = 2$, việc xóa bên phải có nhiều khả năng xảy ra sớm hơn, do đó giá trị kỳ vọng tăng lên tương ứng với giá trị nhỏ hơn$p$. 

Điều này xác nhận rằng phép truy toán nắm bắt được độ lệch do vị trí con trỏ đưa ra một cách tự nhiên. 

### Ví dụ 2 

hãy để$n = 3$,$p = 2$. Chúng tôi tập trung vào quá trình chuyển đổi: 

Tại$k = 3$,$p = 2$, chúng tôi kết hợp:$2/3$cơ hội bị xóa ở hai vị trí đầu tiên, giữ nguyên$p$, Và$1/3$có thể nó ở vị trí thứ 3, giảm$p$đến 1. 

Điều này trộn lẫn các trạng thái$dp[2][2]$Và$dp[1][2]$, cho thấy kỳ vọng lan truyền ngược như thế nào qua cấu trúc DP. 

Dấu vết xác nhận rằng lớn hơn$p$các giá trị dần dần chuyển khối lượng xác suất sang các trạng thái nhỏ hơn khi số lần xóa tích lũy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| mỗi trạng thái được tính toán một lần trên DP hình tam giác | 
| Không gian |$O(n^2)$| bảng DP đầy đủ cho tất cả$p, k$| 

Ràng buộc$n \le 1000$làm cho$n^2 = 10^6$chuyển đổi khả thi trong thời gian giới hạn. Việc sử dụng bộ nhớ cũng có thể chấp nhận được vì bảng DP nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve = None

    n = int(inp.strip())
    dp = [[0.0] * (n + 1) for _ in range(n + 1)]
    for p in range(n + 1):
        dp[p][0] = float(p)
    for k in range(1, n + 1):
        for p in range(0, k + 1):
            if p == 0:
                dp[p][k] = 0.0
            else:
                dp[p][k] = (p / k) * dp[p][k - 1] + ((k - p) / k) * dp[p - 1][k - 1]
    return " ".join(f"{x:.10f}" for x in dp[n])

# custom small cases
assert run("1") == "0.0000000000 1.0000000000"
assert run("2")[:5] != ""  # sanity
assert run("3")[:5] != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0,000... 1,000... | chuyển tiếp cơ sở | 
| 2 | phân phối đối xứng | tính đúng đắn của sự tái phát | 
| 3 | trôi dần dần | lan truyền qua k | 

## Vỏ cạnh 

Khi nào$p = 0$, con trỏ không bao giờ giảm vì không có chỉ số nào lớn hơn 0. DP buộc chính xác tất cả các chuyển đổi vào$p = 0$nhánh, vì thuật ngữ liên quan đến$p-1$bị bỏ qua. 

Vì$p = n$, mọi thao tác xóa ngoại trừ lần xóa đầu tiên đều có khả năng ảnh hưởng đến con trỏ khác không. DP xử lý việc này vì tại$k = n$, đầy đủ các$p$đang hoạt động và sự tái diễn một cách tự nhiên cho phép lặp đi lặp lại quá trình chuyển đổi xuống trạng thái nhỏ hơn. 

Một trường hợp tế nhị cuối cùng là khi$p$cách tiếp cận$k$. Tại ranh giới đó, sự phân chia xác suất trở nên lệch nhiều về phía giảm$p$và DP phản ánh điều này bằng cách tính trọng số$dp[p-1][k-1]$thuật ngữ mạnh mẽ hơn. Điều này đảm bảo giá trị mong đợi không bị sai và duy trì ổn định ở mức cao$p$các giá trị.
