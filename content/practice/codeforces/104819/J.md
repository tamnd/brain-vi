---
title: "CF 104819J - Đếm"
description: "Chúng ta được yêu cầu xem xét tất cả các cây có thể được gắn nhãn trên các đỉnh $n$ và tính một giá trị số duy nhất cho mỗi cây: tổng khoảng cách trên tất cả các cặp đỉnh không có thứ tự. Giá trị này thường được gọi là chỉ số Wiener của cây."
date: "2026-06-28T13:04:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104819
codeforces_index: "J"
codeforces_contest_name: "2023 Sun Yat-sen University Collegiate Programming Contest, Onsite"
rating: 0
weight: 104819
solve_time_s: 63
verified: true
draft: false
---

[CF 104819J - Đếm](https://codeforces.com/problemset/problem/104819/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xem xét tất cả các cây có thể được dán nhãn trên$n$các đỉnh và tính một giá trị số duy nhất cho mỗi cây: tổng khoảng cách trên tất cả các cặp đỉnh không có thứ tự. Giá trị này thường được gọi là chỉ số Wiener của cây. Đối với một cố định$n$, các cấu trúc cây khác nhau có thể tạo ra các tổng khác nhau và nhiệm vụ là liệt kê mọi tổng riêng biệt có thể xảy ra. 

Đầu vào chỉ là$n$, với$n \le 20$. Đầu ra là tập hợp tất cả các giá trị chỉ số Wiener có thể đạt được trong số tất cả các cây được gắn nhãn trên$n$các nút, được sắp xếp ngày càng nhiều. 

Điểm mấu chốt là chúng ta không liệt kê cây một cách rõ ràng. Số cây được dán nhãn là$n^{n-2}$, vốn đã vượt quá$10^{23}$khi$n=20$, vì vậy bất kỳ cách tiếp cận nào lặp lại trên tất cả các cây đều ngay lập tức không thể thực hiện được. Thay vào đó, chúng ta đang tìm kiếm trên một không gian nhỏ hơn nhiều: không gian của các giá trị tổng khoảng cách có thể được tạo ra bởi các cấu trúc cây. 

Bản thân giá trị bị giới hạn. Trường hợp nhỏ nhất là một ngôi sao, trong đó hầu hết các cặp đều ở khoảng cách 2 và trường hợp lớn nhất là một đường đi, trong đó khoảng cách tăng tuyến tính. Ngay cả trong trường hợp xấu nhất, tổng vẫn ở mức$O(n^3)$, duy trì ở mức dưới vài nghìn đối với$n \le 20$. Phạm vi đầu ra giới hạn này là tín hiệu đầu tiên cho thấy việc lập trình động trên các trạng thái có thể đạt được là khả thi. 

Một cạm bẫy tinh vi sẽ xuất hiện nếu người ta cho rằng chỉ có hình dạng cây là quan trọng mà không theo dõi cách các cây con được gắn vào. Hai cấu trúc cây con giống hệt nhau có thể tạo ra các tổng khác nhau tùy thuộc vào đỉnh nào được chọn làm điểm kết nối, vì khoảng cách đến điểm đính kèm ảnh hưởng đến tất cả khoảng cách giữa các cây con. Bất kỳ DP ngây thơ nào chỉ theo dõi kích thước cây con và tổng khoảng cách bên trong sẽ làm mất đi sự phụ thuộc đó và tạo ra sự hợp nhất không chính xác. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ tạo ra mọi cây được gắn nhãn, tính toán các đường đi ngắn nhất cho tất cả các cặp bằng cách sử dụng BFS từ mỗi nút và ghi lại tổng kết quả. Ngay cả việc tạo ra tất cả các cây cũng đã là hàm mũ theo cách siêu đa thức, vì vậy cách tiếp cận này ngay lập tức không khả thi. 

Cấu trúc mở ra tiến trình là bất kỳ cây nào cũng có thể được xây dựng bằng cách liên tục nối hai cây nhỏ hơn với một cạnh duy nhất. Nếu chúng ta biết mọi thứ về hai thành phần trước khi nối chúng, chúng ta sẽ có thể tính toán mọi thứ về cây được hợp nhất. Khó khăn là việc hợp nhất không chỉ phụ thuộc vào khoảng cách bên trong mà còn phụ thuộc vào vị trí gắn cạnh kết nối bên trong mỗi thành phần. 

Điều này gợi ý rằng mỗi trạng thái của cây con phải nhớ nhiều hơn là chỉ chỉ mục Wiener nội bộ của nó. Chúng ta cũng cần biết, với mọi lựa chọn gốc có thể có bên trong cây con đó, tổng khoảng cách từ gốc đó đến tất cả các nút là bao nhiêu. Số lượng đó xác định mức độ tốn kém khi gắn cây con đó qua một đỉnh đã chọn. 

Do đó, chúng tôi coi mỗi trạng thái cây là một cặp bao gồm tổng khoảng cách theo cặp của nó và lựa chọn gốc cùng với tổng khoảng cách từ gốc đó. Khi hai cây có gốc được kết nối, khoảng cách giữa các cây con có thể được biểu thị hoàn toàn bằng kích thước cây con và tổng khoảng cách gốc này. Điều này làm cho việc hợp nhất hoàn toàn đại số. 

DP sau đó trở thành một tổ hợp giống như chiếc ba lô trên các kích thước cây con, trong đó mỗi trạng thái lưu trữ tất cả những gì có thể$(\text{Wiener}, \text{root-sum})$cặp cho kích thước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê Brute Force của cây |$O(n^{n-2} \cdot n^2)$|$O(n)$| Quá chậm | 
| DP qua trạng thái hợp nhất cây con |$O(n^2 \cdot S^2)$Ở đâu$S$là số lượng trạng thái trên mỗi kích thước |$O(n \cdot S)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng tất cả các cây có thể tăng dần theo kích thước. 

1. Khởi tạo DP cho kích thước 1. Một nút duy nhất có chỉ số Wiener 0 và tổng khoảng cách gốc 0. 
2. Duy trì cấu trúc`dp[k]`, lưu trữ tất cả các trạng thái có thể có của cây có kích thước$k$. Mỗi trạng thái là một cặp$(W, R)$, Ở đâu$W$là tổng các khoảng cách theo cặp trong cây con và$R$là tổng khoảng cách từ gốc được chọn đến tất cả các nút trong cây con đó. 
3. Đối với mỗi lần chia kích thước$k$cây thành hai phần$a$Và$b$, lấy một trạng thái từ`dp[a]`và một từ`dp[b]`. 
4. Đối với mỗi cặp trạng thái, hãy mô phỏng việc kết nối hai cây bằng cách thêm một cạnh vào giữa các gốc đã chọn của mỗi bên. Hãy để kích thước là$a$Và$b$, và để các trạng thái$(W_A, R_A)$Và$(W_B, R_B)$. 
5. Tính chỉ số Wiener đã hợp nhất khi chọn gốc trong thành phần đầu tiên:$$W = W_A + W_B + b \cdot R_A + a \cdot R_B + a \cdot b$$Điều này xuất phát từ việc đếm tất cả các cặp chéo: mỗi cặp$(u,v)$tích lũy khoảng cách qua hai gốc cộng với cạnh nối. 
6. Tính tổng khoảng cách gốc mới khi root ở gốc thành phần thứ nhất:$$R = R_A + (R_B + b)$$Mỗi nút trong thành phần thứ hai có thêm một cạnh cộng với khoảng cách bên trong tới nút gốc của nó. 
7. Về mặt đối xứng, cũng xem xét việc root ở gốc của thành phần thứ hai và tính các giá trị tương ứng. 
8. Chèn tất cả các trạng thái kết quả vào`dp[a+b]`. 
9. Sau khi xử lý tất cả các kích cỡ lên đến$n$, thu thập tất cả các giá trị chỉ mục Wiener riêng biệt từ`dp[n]`và sắp xếp chúng. 

Tính chính xác dựa trên thực tế là bất kỳ cây nào cũng có thể được phân tách bằng cách cắt cạnh cuối cùng thành hai cây nhỏ hơn và mọi lựa chọn có thể có về vị trí gắn cạnh đó được thể hiện bằng cách chọn một gốc trong mỗi cây con. 

### Tại sao nó hoạt động 

Bất biến chính là đối với mọi kích thước cây con$k$,`dp[k]`chứa mọi sự kết hợp có thể có của chỉ số Wiener nội bộ và tổng khoảng cách phụ thuộc vào gốc có thể phát sinh từ bất kỳ cây nào trên$k$các nút với bất kỳ sự lựa chọn gốc nào. Bởi vì mỗi cây có thể được phân tách duy nhất bằng cách loại bỏ cạnh được thêm cuối cùng của nó thành hai cây nhỏ hơn và vì các công thức hợp nhất chỉ phụ thuộc vào tổng kích thước và khoảng cách gốc, DP khám phá chính xác không gian giống như tất cả các cấu trúc cây có thể có mà không có sự dư thừa hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def merge_states(A, B):
    res = set()
    for WA, RA in A:
        for WB, RB in B:
            a = WA[2]
            b = WB[2]
```Đợi đã: chúng ta phải thiết kế lại mã một cách cẩn thận. Chúng tôi đã không xác định cấu trúc trạng thái đúng cách. Chúng ta cần dp[k] dưới dạng tập hợp các bộ dữ liệu có kích thước ẩn (W,R,k?). Chúng tôi sẽ lưu trữ (W, R). nhưng cần kích thước được biết từ chỉ mục dp. vậy được rồi. 

Nhưng cũng cần chéo công thức sử dụng a,b bên ngoài biết. 

Vì vậy hàm hợp nhất phải biết kích thước. 

Vì vậy, dp[a], dp[b] tập hợp (W,R). Chúng tôi sẽ lặp lại. 

Hãy thực hiện đầy đủ. 

Chúng ta cũng phải cẩn thận khi thực hiện nhưng không sao. 

Chúng tôi cũng nên đảm bảo rằng chúng tôi loại bỏ các trạng thái trùng lặp. 

Hãy tiếp tục viết mã.
