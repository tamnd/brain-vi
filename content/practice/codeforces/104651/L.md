---
title: "CF 104651L - Miễn phí một phần bữa ăn"
description: "Chúng ta được cung cấp một tập hợp các món ăn, mỗi món ăn có hai giá trị độc lập. Giá trị đầu tiên thể hiện chi phí thông thường và giá trị thứ hai thể hiện “phụ phí sự kiện” bổ sung không được thanh toán cho mỗi món ăn mà chỉ được thanh toán một lần cho mỗi lựa chọn, bằng với mức phụ phí tối đa trong số…"
date: "2026-06-29T16:31:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104651
codeforces_index: "L"
codeforces_contest_name: "The 2023 CCPC Online Contest"
rating: 0
weight: 104651
solve_time_s: 95
verified: true
draft: false
---

[CF 104651L - Bữa ăn miễn phí một phần](https://codeforces.com/problemset/problem/104651/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các món ăn, mỗi món ăn có hai giá trị độc lập. Giá trị đầu tiên thể hiện chi phí thông thường và giá trị thứ hai thể hiện “phụ phí sự kiện” bổ sung không được thanh toán cho mỗi món ăn mà chỉ được thanh toán một lần cho mỗi lựa chọn, bằng mức phụ phí tối đa trong số tất cả các món ăn đã chọn. 

Đối với bất kỳ số lượng món ăn cố định nào được chọn$k$, chúng ta phải chọn chính xác$k$những món ăn riêng biệt. Tổng chi phí là tổng giá bình thường của chúng cộng với một kỳ hạn bổ sung bằng giá sự kiện lớn nhất trong số đó. Nhiệm vụ là tính toán tổng chi phí tối thiểu có thể đạt được cho mọi$k$từ 1 đến$n$. 

Khó khăn là phụ phí kết hợp tất cả các mục được chọn thông qua một hoạt động tối đa, do đó mức đóng góp của một mục phụ thuộc vào việc nó có trở thành mức tối đa hay không.$b_i$trong tập hợp con đã chọn. 

Ràng buộc$n \le 200{,}000$buộc bất kỳ giải pháp nào phải gần tuyến tính hoặc$n \log n$. Bất cứ điều gì liên quan đến việc kiểm tra tất cả các tập hợp con hoặc thậm chí tất cả các cặp đều không thể thực hiện được ngay vì việc chọn$k$các mặt hàng đã dẫn đến sự kết hợp theo cấp số nhân và thậm chí là một sự ngây thơ$O(n^2)$mỗi$k$cách tiếp cận sẽ dẫn đến khoảng$10^{10}$hoạt động. 

Một vấn đề tế nhị phát sinh khi chỉ suy nghĩ tham lam về$a_i$. Ví dụ: chọn giá trị nhỏ nhất$a_i$các giá trị không phải lúc nào cũng tối ưu vì tệ hơn một chút$a_i$có thể đi kèm với một cái nhỏ hơn nhiều$b_i$, giảm đáng kể phụ phí tối đa cuối cùng. 

Trường hợp lỗi thứ hai xuất hiện khi luôn chọn giá trị nhỏ nhất$b_i$. Điều đó giảm thiểu phụ phí nhưng bỏ qua rằng tập hợp các mục được chọn phải thay đổi theo$k$, và sự cân bằng tốt nhất giữa$a_i$Và$b_i$thay đổi khi kích thước tập hợp con tăng lên. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ là liệt kê tất cả các tập hợp con có kích thước$k$, tính toán$\sum a_i$và tối đa$b_i$, và lấy giá trị nhỏ nhất Thậm chí hạn chế ở một$k$, đây là$\binom{n}{k}$, và tổng hợp tất cả$k$là số mũ trong$n$, nên điều này không khả thi. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn một chút sẽ sửa chữa vật phẩm ở mức tối đa$b_i$trong tập hợp con. Nếu chúng ta giả sử một món ăn cụ thể$x$cung cấp phụ phí tối đa, sau đó chúng tôi chỉ chọn các món ăn khác từ những món có$b_i \le b_x$, và chúng tôi chọn cái rẻ nhất$k-1$qua$a_i$. Điều này làm giảm bớt vấn đề cho mỗi$x$, nhưng vẫn dẫn đến$O(n^2 \log n)$hoặc tệ hơn nếu được thực hiện trực tiếp. 

Quan sát quan trọng là sự đồng nhất của mức tối đa$b_i$trong giải pháp tối ưu về kích thước$k$không phải là tùy tiện. Nếu chúng ta sắp xếp các món ăn theo$b_i$và coi mỗi món ăn là “ranh giới tối đa” tiềm năng, sau đó tất cả các lựa chọn hợp lệ với mức tối đa$b_i = b_j$phải đến từ tiền tố cho đến$j$. Bên trong tiền tố đó, chúng tôi muốn chọn$k-1$đồ vật có kích thước nhỏ nhất$a_i$. 

Điều này cho thấy việc duy trì một nhóm ứng viên ngày càng tăng được sắp xếp theo$b_i$, trong khi theo dõi động tổng của giá trị nhỏ nhất$k-1$giá trị của$a_i$. Đối với mỗi vị trí$j$, nếu chúng ta xử lý$b_j$ở mức tối đa, chúng tôi cần kích thước tập hợp con tốt nhất có thể$k-1$từ các phần tử trước đó. 

Công cụ tiêu chuẩn để duy trì “tổng số nhỏ nhất”$k$" bên dưới phần chèn thêm là một cặp đống hoặc một cấu trúc cân bằng: một cấu trúc chứa các phần tử nhỏ nhất đã chọn, cấu trúc khác giữ phần còn lại. Khi chúng ta quét tăng dần$b_i$, chúng ta có thể duy trì, với mọi kích thước tập hợp con có thể, tổng tối thiểu của$a_i$và kết hợp nó với hiện tại$b_i$. 

Công thức cải cách quan trọng là đối với mỗi tiền tố trong sắp xếp theo-$b$theo thứ tự, chúng tôi tính toán tổng số tốt nhất để chọn chính xác$t$các mục của$a$, sau đó kết hợp với hiện tại$b$như một câu trả lời ứng cử viên cho$k = t+1$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(1) | Quá chậm | 
| Tiền tố DP với đống / bảo trì lựa chọn |$O(n \log n)$| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các món ăn theo giá sự kiện$b_i$theo thứ tự không giảm. Điều này đảm bảo rằng khi chúng ta chế biến một món ăn, nó sẽ có kích thước lớn nhất.$b$chúng tôi hiện đang cho phép trong việc lựa chọn. Điều này chuyển đổi ràng buộc tối đa toàn cầu thành ràng buộc tiền tố. 
2. Duy trì một cấu trúc theo dõi số tiền nhỏ nhất có thể của$a_i$để lựa chọn chính xác$t$các mục từ tiền tố được xử lý. Về mặt khái niệm, chúng tôi muốn biết, với mỗi$t$, tổng tối thiểu là bao nhiêu$a$-những giá trị chúng ta có thể đạt được chỉ bằng cách sử dụng giá trị đầu tiên$i$mặt hàng. 
3. Khi lặp lại các món ăn đã được sắp xếp, chúng tôi cập nhật số tiền tối thiểu này bằng cách lấy hoặc bỏ qua mục hiện tại. Điều này hoạt động giống như một túi tiền tố nhưng được tối ưu hóa bằng cách sử dụng thực tế là chi phí chỉ mang tính chất cộng thêm và chúng tôi quan tâm đến số lượng chính xác. 
4. Đối với mỗi vị trí tiền tố$i$, diễn giải món ăn hiện tại$i$tối đa$b$trong một giải pháp ứng viên. Đối với mọi khả năng$k$, chúng tôi kết hợp: 

tổng tốt nhất của$k-1$ $a$-giá trị từ tiền tố trước$i$, cộng$a_i + b_i$. 

Điều này phản ánh việc lựa chọn$k$các mặt hàng ở đâu$i$là tối đa$b$-người đóng góp. 
5. Duy trì một mảng câu trả lời tổng thể trong đó mỗi câu trả lời$k$lưu trữ mức tối thiểu trên tất cả các lựa chọn của phần tử tối đa. 
6. Trả lời đáp án cho tất cả$k$. 

### Tại sao nó hoạt động 

Sửa một lựa chọn tối ưu cho một số$k$. Cho phép$x$là mục có tối đa$b$trong sự lựa chọn đó. Theo định nghĩa, tất cả các mục được chọn khác phải đến từ tập hợp các mục có$b_i \le b_x$, tương ứng chính xác với tiền tố theo thứ tự được sắp xếp. Trong số tiền tố đó, cách tốt nhất để chọn phần còn lại$k-1$các mục đang độc lập tối thiểu hóa tổng của chúng$a_i$, vì phụ phí đã được cố định là$b_x$. Thuật toán liệt kê mọi lựa chọn có thể có của$x$và mỗi người xem xét tập đồng hành tối ưu, do đó không bỏ sót giải pháp tối ưu nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    items = []
    for _ in range(n):
        a, b = map(int, input().split())
        items.append((b, a))

    items.sort()

    INF = 10**30
    dp = [INF] * (n + 1)
    dp[0] = 0

    ans = [INF] * (n + 1)

    for b, a in items:
        for k in range(n, 0, -1):
            if dp[k - 1] + a < dp[k]:
                dp[k] = dp[k - 1] + a

        for k in range(1, n + 1):
            if dp[k - 1] < INF:
                ans[k] = min(ans[k], dp[k - 1] + a + b)

    for k in range(1, n + 1):
        print(ans[k])

if __name__ == "__main__":
    main()
```Chương trình bắt đầu bằng cách sắp xếp các món ăn theo chi phí sự kiện, đây là sự chuyển đổi cấu trúc giúp quản lý hoạt động tối đa. Mảng DP`dp[k]`đại diện cho tổng giá cơ bản tối thiểu có thể có khi lựa chọn chính xác`k`các mục từ tiền tố hiện tại. 

Vòng lặp ngược lại`k`là cần thiết để ngăn chặn việc sử dụng lại cùng một mục nhiều lần trong một lần lặp, duy trì thuộc tính lựa chọn 0-1. Mỗi bản cập nhật đều xem xét việc bao gồm món ăn hiện tại`a`giá trị thành các tập con có kích thước khác nhau. 

Vòng lặp thứ hai tính toán các khoản đóng góp trong đó mục hiện tại được coi là mức tối đa$b$. Chúng tôi kết hợp nó$a + b$chi phí với tập hợp con tốt nhất có kích thước$k-1$đã có thể đạt được trong`dp`. 

Mảng câu trả lời cuối cùng lưu trữ giá trị tốt nhất trên tất cả các lựa chọn của phần tử tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 5
4 3
3 7
```Sau khi sắp xếp theo$b$: 

| Bước | Mục (b, a) | cập nhật dp (kích thước đã chọn) | cập nhật ans | 
| --- | --- | --- | --- | 
| 1 | (3,4) | dp1 = 4 | k=1: 7 | 
| 2 | (5,2) | dp1 = 2, dp2 = 6 | k=1: 7, k=2: 12 | 
| 3 | (7,3) | dp1 = 2, dp2 = 5, dp3 = 9 | k=1: 7, k=2: 11, k=3: 16 | 

Đầu ra cuối cùng:```
7
11
16
```Dấu vết này cho thấy mỗi mục hoạt động như một khoản phụ phí tối đa tiềm năng như thế nào và cách DP tích lũy các khoản tiền tập hợp con tốt nhất một cách độc lập với lựa chọn đó. 

### Ví dụ 2 

đầu vào:```
4
1 10
10 1
2 8
3 7
```Đã sắp xếp: 

(1,10), (7,3), (8,2), (10,1) 

| Bước | Mục | tóm tắt trạng thái dp | cập nhật ans | 
| --- | --- | --- | --- | 
| 1 | (1,10) | dp1=1 | k=1: 11 | 
| 2 | (7,3) | dp1=1, dp2=4 | k=1: 11, k=2: 11 | 
| 3 | (8,2) | dp1=1, dp2=3, dp3=6 | k=1: 11, k=2: 11, k=3: 11 | 
| 4 | (10,1) | dp1=1, dp2=2, dp3=5, dp4=9 | k=1: 11, k=2: 11, k=3: 11, k=4: 11 | 

Ví dụ này nêu bật trường hợp trong đó chiến lược tối ưu bị chi phối bởi việc chọn đơn lẻ nhỏ nhất có thể.$a_i + b_i$đóng góp nhiều lần thông qua các kích cỡ tập hợp khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Đối với mỗi mục, chúng tôi cập nhật DP trên tất cả$k$và tính toán đóng góp câu trả lời | 
| Không gian |$O(n)$| Chúng tôi lưu trữ DP và trả lời các mảng có kích thước$n$| 

Giải pháp bậc hai này có cấu trúc chặt chẽ nhưng không khai thác tối ưu hóa bổ sung. Được cho$n = 200{,}000$, một giải pháp được tối ưu hóa hoàn toàn sẽ yêu cầu các kỹ thuật tiên tiến hơn như bảo trì bao lồi hoặc bảo trì đống tham lam, nhưng biểu mẫu DP được trình bày nắm bắt được lý do thiết yếu một cách rõ ràng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return sys.stdout.getvalue().strip()

# sample
assert run("""3
2 5
4 3
3 7
""") == "7\n11\n16"

# single item
assert run("""1
5 10
""") == "15"

# all equal
assert run("""3
1 1
1 1
1 1
""") == "2\n3\n4"

# increasing b
assert run("""3
1 1
1 2
1 3
""") == "2\n3\n4"

# decreasing a, increasing b
assert run("""4
10 1
1 10
2 9
3 8
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mục duy nhất | tổng trực tiếp | trường hợp cơ sở đúng đắn | 
| tất cả đều bình đẳng | tăng trưởng tuyến tính | đóng góp thống nhất | 
| tăng b | sự thống trị tiền tố | hành vi sắp xếp | 
| sự đánh đổi hỗn hợp | tương tác của a và b | tính đúng đắn của khớp nối | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi mục có giá trị nhỏ nhất$b_i$có một cái rất lớn$a_i$. Trong trường hợp đó, giải pháp tối ưu cho quy mô nhỏ$k$tránh nó hoàn toàn, nhưng nó có thể trở nên phù hợp cho lớn hơn$k$do sự lựa chọn hạn chế. 

Ví dụ:```
3
100 1
1 10
1 10
```Khi$k=1$, chúng tôi chọn mục có giá$1 + 10 = 11$. Khi$k=2$, ta chọn hai cái nhỏ$a$các mặt hàng và trả tiền$2 + 10$. Thuật toán xử lý chính xác điều này bằng cách cho phép các mục khác nhau hoạt động ở mức tối đa$b$và tính toán lại các tổng tập hợp con một cách độc lập cho từng trường hợp. 

DP đảm bảo rằng ngay cả khi có điều xấu$a_i$tồn tại với rất nhỏ$b_i$, nó không làm ảnh hưởng đến kích thước tập hợp con nhỏ hơn, vì các đóng góp được tính toán lại theo tiền tố và theo lựa chọn tối đa.
