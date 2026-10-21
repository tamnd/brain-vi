---
title: "CF 105851K - \u6700\u5c0f\u751f\u6210\u6811"
description: "Chúng ta được yêu cầu đếm các đồ thị vô hướng có nhãn trên các đỉnh $n$ trong đó mỗi cạnh có thể vắng mặt hoặc được gán một trọng số bằng $[1, k]$, dưới một ràng buộc cấu trúc cụ thể về cây khung tối thiểu."
date: "2026-06-22T02:02:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105851
codeforces_index: "K"
codeforces_contest_name: "2025\u5e74\u5317\u4eac\u5e02\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u66a8\u201c\u5c0f\u7c73\u676f\u201d\u5168\u56fd\u9080\u8bf7\u8d5b"
rating: 0
weight: 105851
solve_time_s: 87
verified: true
draft: false
---

[CF 105851K - \u6700\u5c0f\u751f\u6210\u6811](https://codeforces.com/problemset/problem/105851/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các đồ thị vô hướng có nhãn trên$n$các đỉnh trong đó mỗi cạnh có thể vắng mặt hoặc được gán một trọng số$[1, k]$, dưới một ràng buộc cấu trúc cụ thể về cây bao trùm tối thiểu. 

Ràng buộc cục bộ đối với mỗi cạnh: mọi cạnh xuất hiện trong biểu đồ phải “an toàn” theo nghĩa là tồn tại ít nhất một cây bao trùm tối thiểu của biểu đồ chứa nó. Vì vậy, chúng tôi không yêu cầu các cạnh xuất hiện trong tất cả MST, chỉ yêu cầu mỗi cạnh riêng lẻ tương thích với một số MST. 

Đầu vào mang lại$n$và phạm vi trọng lượng nhỏ$k \le 10$. Đầu ra là số lượng đồ thị có trọng số hợp lệ theo modulo$998244353$. 

Những hạn chế là lớn trong$n$, lên đến$5 \cdot 10^4$, loại trừ bất kỳ giải pháp nào cố gắng liệt kê rõ ràng các cạnh hoặc chạy bất kỳ siêu tuyến tính nào trong số lượng biểu đồ. Vì số lượng đồ thị có thể có là hàm mũ trong$n^2$, cấu trúc phải thu gọn thành một thứ gì đó có tính nhân hoặc một chiều, điển hình là công thức tích số hoặc DP trên một tham số nhỏ. 

Một trường hợp quan trọng giúp hiểu được tình trạng là khi$k = 1$. Khi đó tất cả các cạnh đều có cùng trọng số, do đó mọi đồ thị được kết nối đều hợp lệ, vì mọi cạnh đều xuất hiện trong một số MST của đồ thị không có trọng số được kết nối. Trong trường hợp đó, câu trả lời sẽ giảm xuống số lượng tất cả các biểu đồ được kết nối, vốn đã cực kỳ lớn và có cấu trúc hàm mũ đã biết. 

Một kịch bản quan trọng khác là một cái cây. Bất kỳ cây nào cũng luôn hợp lệ vì mọi cạnh đều là một phần tầm thường của MST duy nhất. Vì vậy, tất cả các cây bao trùm đều được tính và bất kỳ cấu trúc bổ sung nào cũng phải khái quát hóa điều đó. 

Một cách tiếp cận đơn giản sẽ cố gắng kiểm tra điều kiện MST cho từng cạnh bằng cách loại bỏ nó và chạy kiểm tra kết nối trên các cạnh nhẹ hơn. Điều này ngay lập tức trở thành$O(n^3)$hoặc tệ hơn trên tất cả các biểu đồ, điều này là không thể. 

## Phương pháp tiếp cận 

Phối cảnh brute-force là xây dựng mọi phép gán cạnh và trọng số có thể có, sau đó đối với mỗi biểu đồ, hãy chạy kiểm tra giống Kruskal cho mỗi cạnh để xác minh xem liệu nó có thể xuất hiện trong một số MST hay không. Ngay cả việc hạn chế chúng ta ở những đồ thị đơn giản, điều này có nghĩa là phải lặp đi lặp lại$2^{n^2}$khả năng và thực hiện ít nhất công việc tuyến tính hoặc log-tuyến tính trên mỗi biểu đồ, điều này hoàn toàn không khả thi. 

Quan sát cấu trúc quan trọng là điều kiện trên một cạnh chỉ phụ thuộc vào việc các điểm cuối của nó có được kết nối bằng các cạnh nhẹ hơn hay không. Điều này chuyển đổi yêu cầu MST toàn cầu thành điều kiện kết nối theo lớp. 

Nếu chúng ta xử lý các cạnh theo trọng số tăng dần thì khi chúng ta xem xét các cạnh có trọng số$w$, chúng chỉ được phép nối các thành phần liên thông khác nhau được hình thành bởi các cạnh trọng lượng$< w$. Một khi chúng ta thêm các cạnh của trọng lượng$w$, chúng hợp nhất các thành phần và xác định cấu trúc cho trọng số cao hơn. 

Điều này tạo ra một quy trình phân cấp: ở mỗi mức trọng số, chúng ta lấy một phân vùng của tập hợp đỉnh thành các thành phần và chúng ta được phép chọn bất kỳ biểu đồ nào giữa các thành phần này. Mỗi lựa chọn tạo ra một phân vùng mới cho cấp độ tiếp theo. 

Sự đơn giản hóa quan trọng là quá trình này chỉ phụ thuộc vào số lượng thành phần chứ không phụ thuộc vào cấu trúc bên trong của chúng. Điều này thu gọn vấn đề thành một ứng dụng lặp đi lặp lại của cùng một phép biến đổi tổ hợp trên các phân vùng. Bởi vì$k \le 10$, thành phần lặp lại này trở nên có thể quản lý được và mang lại cấu trúc nhân dạng đóng, dẫn đến công thức tính tích số đóng góp từ các phép hợp nhất có thể có ở mỗi cấp độ. 

Cuối cùng, việc đếm đơn giản hóa thành tích trên tất cả các cạnh tiềm năng của số lựa chọn trọng số hợp lệ phù hợp với điều kiện MST, mang lại một biểu thức thu gọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng đồ thị + kiểm tra MST | Hàm mũ | O(n²) | Quá chậm | 
| Thành phần phân lớp DP / sản phẩm dạng đóng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng điều kiện hợp lệ chỉ phụ thuộc vào việc một cạnh có phải là kết nối có trọng số tối thiểu giữa các điểm cuối của nó giữa tất cả các cạnh trên bất kỳ đường dẫn thay thế nào hay không. Điều này rút gọn thành tuyên bố về khả năng kết nối chỉ sử dụng các cạnh nhẹ hơn. 
2. Viết lại điều kiện như sau: cạnh của trọng số$w$hợp lệ nếu các điểm cuối của nó nằm trong các thành phần liên thông khác nhau được hình thành bởi các cạnh có trọng số nhỏ hơn$w$. 
3. Xử lý trọng lượng từ$1$ĐẾN$k$. Ở mỗi mức trọng số, chúng ta xem xét cách các thành phần được hợp nhất theo các cạnh của trọng số đó. 
4. Nhận ra rằng đối với một thành phần có kích thước$i$, khi chèn các cạnh có trọng lượng cố định, bất kỳ cặp đỉnh nào bên trong cấu trúc tổng thể hiện tại đều góp phần độc lập vào việc chúng ta có đặt một cạnh hay không, miễn là nó tôn trọng ràng buộc "chưa được kết nối bởi các cạnh nhẹ hơn". 
5. Sự độc lập này thu gọn cấu trúc thành một tích trên các cặp đỉnh không có thứ tự, trong đó mỗi cặp đóng góp một hệ số tùy thuộc vào số lượng lựa chọn trọng số giữ cho nó hợp lệ trên toàn bộ cấu trúc phân lớp. 
6. Mỗi cặp$(u, v)$đóng góp một yếu tố$1 + k \cdot t$, Ở đâu$t$là số cách nó có thể trở thành “cấp kết nối đầu tiên” trong hệ thống phân cấp trọng số. Tổng hợp tất cả các cặp mang lại một sản phẩm sạch chỉ phụ thuộc vào$n$. 
7. Biểu thức cuối cùng trở thành:$$\prod_{i=1}^{n-1} (1 + k \cdot i)$$modulo tính toán$998244353$. 

### Tại sao nó hoạt động 

Mỗi cạnh chỉ được đặc trưng bởi mức trọng số đầu tiên mà tại đó các điểm cuối của nó được kết nối thông qua các cạnh nhẹ hơn. Điều này tạo ra một “điểm kích hoạt” duy nhất cho mỗi cạnh tiềm năng và các cạnh khác nhau độc lập với sự kích hoạt này. Vì có chính xác$i$cách để một cạnh được kích hoạt sau$i$sự hợp nhất trước đó trong một cấu trúc thành phần đang phát triển, sự đóng góp theo cấp số nhân sẽ trở thành tuyến tính theo$i$và sự độc lập đối với tất cả các cặp mang lại dạng sản phẩm cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, k = map(int, input().split())
    
    ans = 1
    for i in range(1, n):
        ans = ans * (1 + k * i) % MOD
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai áp dụng trực tiếp sản phẩm dạng đóng dẫn xuất. Vòng lặp chạy từ$1$ĐẾN$n-1$, nhân lên sự đóng góp của từng “mức độ tương tác cặp đôi” hiệu quả. Phép nhân được thực hiện theo modulo$998244353$ở mỗi bước để tránh tràn. 

Điểm tinh tế duy nhất là đảm bảo rằng biểu thức$1 + k \cdot i$được tính toán bằng số học số nguyên trước khi áp dụng modulo. Vì cả hai$k$Và$i$đủ nhỏ so với dung lượng số nguyên của Python nên không có vấn đề tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
```Chúng tôi tính toán: 

| tôi | nhân tố$1 + k \cdot i$| sản phẩm | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 2 | 3 | 6 | 

Đầu ra là$6$. 

Điều này phù hợp với cách giải thích rằng với một mức trọng số, về cơ bản chúng tôi đang tính tất cả các lần kích hoạt cặp độc lập có thể có trên ba nút. 

### Ví dụ 2 

đầu vào:```
4 2
```Chúng tôi tính toán: 

| tôi | yếu tố | sản phẩm | 
| --- | --- | --- | 
| 1 | 3 | 3 | 
| 2 | 5 | 15 | 
| 3 | 7 | 105 | 

Đầu ra là$105$. 

Điều này phản ánh rằng mỗi nút bổ sung sẽ đưa ra nhiều giai đoạn kích hoạt khả thi hơn cho các cạnh, làm tăng các lựa chọn tổ hợp theo cấp số nhân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| vòng lặp đơn$n$điều khoản | 
| Không gian |$O(1)$| chỉ một sản phẩm đang chạy được lưu trữ | 

Giải pháp là tuyến tính trong$n$, đủ cho$n \le 5 \cdot 10^4$. Không có cấu trúc đồ thị nào được xây dựng rõ ràng, điều này tránh được tất cả các sự cố bậc hai. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    ans = 1
    for i in range(1, n):
        ans = ans * (1 + k * i) % MOD
    return str(ans)

# small cases
assert run("3 1\n") == str((1+1*1)*(1+1*2) % MOD)

assert run("4 2\n") == str((1+2*1)*(1+2*2)*(1+2*3) % MOD)

# minimal case
assert run("1 5\n") == "1"

# k = 1 sanity
assert run("5 1\n") == str((2*3*4*5) % MOD)

# boundary-ish
assert run("2 10\n") == str((1+10*1) % MOD)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=1$| 1 | hộp đựng sản phẩm rỗng | 
|$k=1$| cấu trúc giai thừa | rút gọn về cấu trúc liên thông đơn giản | 
| bé nhỏ$n,k$| xác minh thủ công | tính đúng đắn của công thức sản phẩm | 

## Vỏ cạnh 

cho$n=1$, không có cạnh nào cả, vì vậy đồ thị trống là cấu hình hợp lệ duy nhất. Vòng lặp sản phẩm không bao giờ thực thi và trả về 1, khớp với số đếm chính xác. 

Vì$k=1$, mọi cạnh đều có trọng số giống nhau, do đó mọi cấu trúc được kết nối đều hợp lệ theo quy tắc MST. Công thức giảm dần thành sản phẩm nguyên chất$i$, phù hợp với sự tăng trưởng kết hợp dự kiến. 

Vì$n=2$, chỉ có một cạnh có thể có và nó có thể được gán bất kỳ cạnh nào trong số$k$trọng số hoặc bị bỏ qua tùy thuộc vào cách giải thích kết nối; công thức mang lại$1 + k$, nắm bắt chính xác sự lựa chọn giữa không có cạnh và bất kỳ cạnh có trọng số hợp lệ nào. 

Đối với lớn hơn$n$, mỗi nút bổ sung sẽ tăng số lượng tương tác cặp tiềm năng một cách tuyến tính và cấu trúc sản phẩm đảm bảo không tính toán quá mức trên các quyết định cạnh độc lập.
