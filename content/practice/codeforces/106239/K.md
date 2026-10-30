---
title: "CF 106239K - \u7ebf\u6bb5\u8986\u76d6"
description: "Chúng ta được cho một danh sách được sắp xếp các điểm phân biệt trên trục số. Nhiệm vụ là bao phủ tất cả các điểm này bằng cách sử dụng tối đa $k$ các phân đoạn, trong đó một phân đoạn có thể là bất kỳ khoảng $[a,b]$ nào và chi phí của nó là độ dài hình học của nó $ Một cách hữu ích để điều chỉnh lại vấn đề là nghĩ đến việc nhóm các điểm."
date: "2026-06-20T09:06:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106239
codeforces_index: "K"
codeforces_contest_name: "2025\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66\u65b0\u751f\u8d5b(\u51b3\u8d5b)"
rating: 0
weight: 106239
solve_time_s: 46
verified: true
draft: false
---

[CF 106239K - \u7ebf\u6bb5\u8986\u76d6](https://codeforces.com/problemset/problem/106239/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một danh sách được sắp xếp các điểm phân biệt trên trục số. Nhiệm vụ là bao gồm tất cả các điểm này bằng cách sử dụng tối đa$k$các phân đoạn, trong đó một phân đoạn có thể là bất kỳ khoảng nào$[a,b]$và giá của nó là chiều dài hình học của nó$|a-b|$. Một đoạn bao phủ một điểm nếu điểm đó nằm giữa các điểm cuối của nó. Mục tiêu là giảm thiểu tổng chiều dài của tất cả các phân đoạn đã chọn và chúng tôi phải báo cáo giá trị tối ưu cho mỗi phân đoạn.$k$từ 1 đến$n$. 

Một cách hữu ích để điều chỉnh lại vấn đề là nghĩ đến việc nhóm các điểm lại. Nếu chúng ta quyết định bao gồm một nhóm điểm liên tiếp$x_l, x_{l+1}, \dots, x_r$với một phân khúc, phân khúc rẻ nhất có thể chính xác là$[x_l, x_r]$, và giá của nó là$x_r - x_l$. Bất kỳ giải pháp tối ưu nào cũng sẽ luôn sử dụng các phân đoạn bao phủ các khối liền kề của mảng đã sắp xếp, bởi vì bất kỳ phân đoạn nào bao trùm các điểm không liền kề đều có thể được rút ngắn mà không làm mất phạm vi bao phủ. 

Các ràng buộc cho phép lên đến$2 \times 10^5$điểm, ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các phân vùng hoặc sử dụng$O(n^2)$chuyển tiếp lập trình động trực tiếp. Chúng ta cần một cái gì đó gần hơn với tuyến tính hoặc$O(n \log n)$, và chúng ta cũng cần câu trả lời cho tất cả$k$, điều này gợi ý việc xây dựng cấu trúc toàn cầu một lần thay vì tính toán lại mỗi lần$k$. 

Một điểm tinh tế xuất hiện khi nghĩ về$k=1$. Đoạn duy nhất phải bao gồm tất cả các điểm, vì vậy câu trả lời là$x_n - x_1$. Đối với lớn hơn$k$, chúng tôi được phép chia mảng thành nhiều phân đoạn một cách hiệu quả, chỉ trả tiền cho các khoảng bên trong mỗi phân đoạn. 

Một cách tiếp cận ngây thơ có thể thử mọi cách để chia mảng thành$k$các nhóm cho mỗi$k$, nhưng điều đó dẫn đến sự bùng nổ tổ hợp. Ngay cả việc tính toán một$k$-phân vùng tối ưu với chi phí DP$O(n^2 k)$hoặc$O(n^2)$, quá lớn. 

## Phương pháp tiếp cận 

Quan sát quan trọng là việc chia nhỏ một phân khúc sẽ giúp giảm chi phí theo cách có cấu trúc chặt chẽ. Nếu chúng ta bao gồm tất cả các điểm bằng một đoạn thì chi phí là$x_n - x_1$. Giả sử chúng ta chèn một sự phân chia giữa$x_i$Và$x_{i+1}$. Thay vì một đoạn$[x_1, x_n]$, bây giờ chúng tôi sử dụng hai phân đoạn$[x_1, x_i]$Và$[x_{i+1}, x_n]$. Chi phí mới trở thành$$(x_i - x_1) + (x_n - x_{i+1})$$so với$$x_n - x_1.$$Sự khác biệt là$$(x_n - x_1) - [(x_i - x_1) + (x_n - x_{i+1})] = x_{i+1} - x_i.$$Điều này có nghĩa là mọi sự phân chia có thể có giữa các điểm liền kề đều có một mức “tiết kiệm” bằng khoảng cách giữa chúng. Việc phân chia sẽ loại bỏ khoảng cách đó khỏi tổng chi phí vì phân khúc không còn trải dài trên nó nữa. 

Vì vậy, vấn đề trở nên tương đương với điều này: chúng ta bắt đầu với chi phí$x_n - x_1$, và chúng tôi được phép chèn tối đa$k-1$chia tách. Mỗi lần chia ở vị trí$i$mang lại lợi ích$x_{i+1} - x_i$. Để giảm thiểu tổng chi phí, chúng tôi muốn tối đa hóa tổng lợi ích đã chọn, đạt được bằng cách chọn những khoảng trống lớn nhất. 

Như vậy, đối với mỗi$k$, chiến lược tối ưu là chọn$k-1$khoảng trống liền kề lớn nhất và trừ tổng của chúng từ$x_n - x_1$. 

Điều này làm giảm vấn đề sắp xếp các khoảng trống một lần và tính tổng tiền tố, sau đó mỗi câu trả lời là một phép trừ đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP trên các phân vùng |$O(n^2)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Sắp xếp các khoảng trống + tổng tiền tố |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tất cả các sai phân liền kề$d_i = x_{i+1} - x_i$vì$i = 1$ĐẾN$n-1$. 

Những khoản này thể hiện số tiền tiết kiệm được nếu chúng ta quyết định phân chia giữa hai điểm đó, vì việc phân chia sẽ loại bỏ khoảng cách đó khỏi bị bao phủ bởi một phân đoạn duy nhất. 
2. Sắp xếp mảng$d$theo thứ tự giảm dần. 

Chúng tôi muốn ưu tiên khoản tiết kiệm lớn nhất trước tiên vì mỗi phần phân chia đều độc lập và đóng góp bổ sung vào sự cải thiện tổng thể. 
3. Xây dựng một mảng tổng tiền tố trên các khoảng trống đã được sắp xếp, trong đó`pref[i]`là tổng lớn nhất$i$những khoảng trống. 

Điều này cho phép chúng tôi trả lời bất kỳ$k$một cách hiệu quả mà không cần tính toán lại số tiền. 
4. Khởi tạo chi phí cơ bản như$x_n - x_1$, tương ứng với việc bao gồm mọi thứ bằng một phân đoạn duy nhất. 
5. Đối với mỗi$k$từ 1 đến$n$, đầu ra:$$(x_n - x_1) - \text{pref}[k-1]$$bởi vì sử dụng$k$phân đoạn có nghĩa là chèn chính xác$k-1$chia tách. 

Tại sao nó hoạt động 

Mỗi phân đoạn hợp lệ tương ứng với việc chọn một tập hợp con các khoảng trống nơi đặt các vết cắt. Mỗi lần cắt sẽ loại bỏ chính xác một khoảng trống liền kề để không được đưa vào đoạn bao trùm. Vì các khoảng trống là độc lập và không tương tác nên sự cải thiện tổng thể có tính chất cộng thêm. Do đó, giải pháp tối ưu thu được bằng cách chọn giá trị lớn nhất$k-1$những cải tiến, đó chính xác là những gì việc sắp xếp đảm bảo. Không có cấu trúc thay thế nào có thể mang lại mức giảm lớn hơn bởi vì bất kỳ sự sắp xếp lại các vết cắt nào cũng không làm thay đổi nhiều khoảng trống đã chọn, chỉ những khoảng trống nào được bao gồm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
x = list(map(int, input().split()))

if n == 1:
    print(0)
    sys.exit()

gaps = [x[i+1] - x[i] for i in range(n - 1)]
gaps.sort(reverse=True)

pref = [0] * (n)
for i, v in enumerate(gaps, start=1):
    pref[i] = pref[i - 1] + v

base = x[-1] - x[0]

ans = []
for k in range(1, n + 1):
    ans.append(str(base - pref[k - 1]))

print(" ".join(ans))
```Việc triển khai phản ánh trực tiếp quá trình chuyển đổi từ phân vùng sang chọn khoảng trống. Việc chăm sóc duy nhất cần thiết là lập chỉ mục: có$n-1$khoảng trống nhưng câu trả lời yêu cầu$k-1$trong số đó, vì vậy mảng tiền tố được xây dựng với độ dài$n$để bao gồm trường hợp số 0 một cách an toàn. 

Chi phí cơ bản được tính một lần và mỗi truy vấn là một phép trừ theo thời gian không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Khoảng trống là$[1, 1]$, được sắp xếp như$[1, 1]$, tổng tiền tố$[0, 1, 2]$. Chi phí cơ bản là$3 - 1 = 2$. 

| k | khoảng trống đã chọn | tổng hợp | trả lời | 
| --- | --- | --- | --- | 
| 1 | không | 0 | 2 | 
| 2 | 1 khoảng cách | 1 | 1 | 
| 3 | 2 khoảng trống | 2 | 0 | 

Đầu ra:```
2 1 0
```Điều này cho thấy mỗi phân đoạn bổ sung sẽ loại bỏ một khoảng cách ranh giới, giảm dần tổng chi phí xuống 0 khi mọi điểm đều bị cô lập. 

### Ví dụ 2 

đầu vào:```
4
1 3 6 10
```Khoảng trống là$[2, 3, 4]$, được sắp xếp$[4, 3, 2]$, tổng tiền tố$[0, 4, 7, 9]$. Chi phí cơ bản là$9$. 

| k | khoảng trống đã chọn | tổng hợp | trả lời | 
| --- | --- | --- | --- | 
| 1 | không | 0 | 9 | 
| 2 | 4 | 4 | 5 | 
| 3 | 4,3 | 7 | 2 | 
| 4 | 4,3,2 | 9 | 0 | 

Dấu vết này nhấn mạnh rằng phân khúc tối ưu luôn cắt ở những khoảng cách lớn nhất trước tiên, tạo ra mức giảm chi phí cân bằng nhất cho mỗi phân khúc bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp$n-1$khoảng trống thống trị thời gian chạy | 
| Không gian |$O(n)$| Lưu trữ các khoảng trống và tổng tiền tố | 

Giải pháp thoải mái phù hợp với các ràng buộc vì$n \le 2 \times 10^5$và việc sắp xếp cộng với tính toán tiền tố tuyến tính cũng nằm trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n = int(input().strip())
    x = list(map(int, input().split()))

    if n == 1:
        return "0"

    gaps = [x[i+1] - x[i] for i in range(n - 1)]
    gaps.sort(reverse=True)

    pref = [0] * (n)
    for i, v in enumerate(gaps, start=1):
        pref[i] = pref[i - 1] + v

    base = x[-1] - x[0]

    ans = []
    for k in range(1, n + 1):
        ans.append(str(base - pref[k - 1]))

    return " ".join(ans)

# provided samples
assert run("3\n1 2 3\n") == "2 1 0"

# minimum size
assert run("1\n100\n") == "0"

# simple spaced points
assert run("2\n1 10\n") == "9 0"

# all equal gaps
assert run("5\n1 2 3 4 5\n") == "4 3 2 1 0"

# large uneven gaps
assert run("4\n1 2 100 101\n") == "100 1 0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 0 | xử lý trường hợp cơ bản | 
| hai điểm xa nhau | 9 0 | hành vi khoảng cách đơn | 
| khoảng cách đồng đều | 4 3 2 1 0 | giảm tham lam nhất quán | 
| khoảng cách lớn hỗn hợp | 100 1 0 0 | tính đúng đắn của việc sắp xếp các khoảng trống | 

## Vỏ cạnh 

Đối với một điểm duy nhất, thuật toán không tạo ra khoảng trống và trực tiếp đưa ra kết quả bằng 0, vì chi phí cơ bản bằng 0 và không cần phân chia. 

Ví dụ đầu vào:```
1
42
```Danh sách khoảng cách trống, tổng tiền tố đều bằng 0 và mọi$k$trả về số không. Thuật toán không bao giờ cố gắng truy cập các chỉ mục không hợp lệ vì mảng tiền tố có kích thước bằng$n$, Và$k-1$nằm trong khoảng an toàn từ 0 đến 0. 

Đối với các điểm được đóng gói chặt chẽ như:```
3
10 11 12
```khoảng trống là$[1,1]$và thuật toán chọn chúng theo thứ tự. Vì$k=2$, một khoảng trống sẽ bị loại bỏ, giảm chi phí từ 2 xuống 1, phù hợp với cách giải thích rằng một lần cắt sẽ loại bỏ chính xác một đơn vị nhịp. 

Đối với khoảng cách không cân bằng cao như:```
3
1 2 100
```khoảng trống là$[1, 98]$. Chiến lược tối ưu cho$k=2$chọn khoảng cách lớn trước tiên, loại bỏ 98 khỏi tổng khoảng cách, chỉ để lại 1, phù hợp với việc phân tách ở khoảng cách vượt trội.
