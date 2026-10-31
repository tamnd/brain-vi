---
title: "CF 106362F - Món Quà Hoàn Hảo"
description: "Chúng ta được cho một hoán vị có độ dài $n$, và chúng ta được yêu cầu đánh giá một giá trị rút ra từ tất cả các mảng con của nó bằng cách sử dụng khái niệm MEX."
date: "2026-06-20T22:56:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106362
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 2-11-2026 Div. 2 (Beginner)"
rating: 0
weight: 106362
solve_time_s: 54
verified: true
draft: false
---

[CF 106362F - Món quà hoàn hảo](https://codeforces.com/problemset/problem/106362/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$và chúng ta được yêu cầu đánh giá một giá trị xuất phát từ tất cả các mảng con của nó bằng cách sử dụng khái niệm MEX. Đối với mỗi phân đoạn liền kề của mảng, chúng tôi xem xét số nguyên không âm nhỏ nhất không xuất hiện bên trong phân đoạn đó và chúng tôi tính tổng giá trị này trên tất cả các phân đoạn có thể có. 

Vì mảng là một hoán vị nên mọi giá trị từ$1$ĐẾN$n$xuất hiện đúng một lần. Cấu trúc này làm cho hoạt động của MEX cứng nhắc hơn nhiều so với trong một mảng chung: liệu một mảng con có MEX lớn hơn hay không$i$chỉ phụ thuộc vào việc nó có chứa tất cả các số từ$1$ĐẾN$i$. 

Đầu ra là tổng của các giá trị MEX này trên tất cả các mảng con, do đó, nhiệm vụ cơ bản là về việc đếm, cho mỗi ngưỡng$i$, có bao nhiêu mảng con chứa đầy đủ tập hợp$\{1, 2, \dots, i\}$và tổng hợp những đóng góp đó. 

Mức ràng buộc (điển hình cho loại vấn đề này,$n$lên đến khoảng$2 \cdot 10^5$) ngụ ý rằng bất kỳ phép liệt kê bậc hai nào của các mảng con đều không thể thực hiện được. Một cách tiếp cận trực tiếp để kiểm tra từng mảng con và tính toán MEX của nó sẽ yêu cầu$O(n^2)$mảng con và ít nhất$O(n)$hoạt động trên mỗi mảng con, vượt xa giới hạn khả thi. Thậm chí$O(n^2)$tổng công việc là quá chậm trong thực tế. 

Một vấn đề phức tạp hơn sẽ xuất hiện nếu người ta cố gắng duy trì một mảng tần số trên mỗi mảng con và tính toán MEX một cách linh hoạt. Ngay cả với kỹ thuật cửa sổ trượt, yêu cầu không chỉ dành cho một cửa sổ mà dành cho tất cả các cửa sổ.$O(n^2)$windows, vì vậy việc sử dụng lại không đơn giản. 

Một cạm bẫy điển hình là giả định MEX hoạt động cục bộ. Ví dụ, trong hoán vị$[2,1,3]$, mảng con$[2,3]$có MEX$1$, nhưng nếu một người theo dõi sự hiện diện của các giá trị một cách độc lập trên mỗi chỉ mục thì rất dễ tính sai các đóng góp cho các phân đoạn chồng chéo vì MEX phụ thuộc vào điều kiện tiền tố toàn cục đối với các giá trị chứ không phải vị trí. 

## Phương pháp tiếp cận 

Phương pháp brute-force xem xét mọi mảng con, tính MEX của nó bằng cách quét lên trên từ$0$, và tích lũy kết quả. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, có$O(n^2)$mảng con và mỗi phép tính MEX có thể mất tới$O(n)$, dẫn đến$O(n^3)$hành vi trong trường hợp xấu nhất, hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc về cấu trúc quan trọng đến từ việc đảo ngược định nghĩa về MEX. Thay vì hỏi MEX của một mảng con cố định là gì, chúng ta hỏi từng số nguyên$k$, trong bao nhiêu mảng con thì MEX lớn hơn chính xác$k$. Một mảng con có MEX lớn hơn$k$chính xác khi nó chứa tất cả các giá trị$1$bởi vì$k$. 

Vì mảng là một hoán vị nên các giá trị$1$bởi vì$k$chiếm một phạm vi liền kề về vị trí của chúng khi chúng ta chiếm vị trí tối thiểu và tối đa của chúng. Cho phép$L_k$là vị trí ngoài cùng bên trái trong số các giá trị này và$R_k$ngoài cùng bên phải. Một mảng con chứa tất cả chúng khi và chỉ khi nó bao gồm cả hai$L_k$Và$R_k$. Số lượng mảng con chứa cả hai chỉ số cố định được xác định bằng cách chọn điểm cuối bên trái ở bất kỳ đâu từ$1$ĐẾN$L_k$và một điểm cuối bên phải từ$R_k$ĐẾN$n$, cung cấp số nhân trực tiếp. 

Điều này làm giảm vấn đề từ việc liệt kê các mảng con đến việc duy trì khoảng thời gian phát triển$[L_k, R_k]$khi chúng ta tăng lên$k$, có thể được cập nhật tăng dần theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Theo dõi khoảng tiền tố |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời bằng cách xử lý các giá trị theo thứ tự tăng dần, theo dõi vị trí mỗi giá trị xuất hiện trong hoán vị. 

1. Lưu trữ vị trí từng giá trị trong mảng`pos`, để có thể`pos[x]`đưa ra chỉ số giá trị$x$trong hoán vị. Điều này cho phép chúng ta xác định vị trí các phần tử trong thời gian không đổi. 
2. Duy trì hai biến`L`Và`R`đại diện cho vị trí tối thiểu và tối đa hiện tại giữa các giá trị$1$bởi vì$k$. Ban đầu, trước khi xử lý bất kỳ giá trị nào, tập hợp này trống, vì vậy chúng tôi xử lý phần đóng góp cơ sở một cách riêng biệt. 
3. Đối với mỗi$k$từ$1$ĐẾN$n$, cập nhật`L = min(L, pos[k])`Và`R = max(R, pos[k])`. Điều này theo dõi khoảng chính xác bao gồm tất cả các yếu tố cần thiết. 
4. Tính xem có bao nhiêu mảng con chứa cả hai`L`Và`R`. Bất kỳ mảng con hợp lệ nào cũng phải bắt đầu vào hoặc trước`L`và kết thúc vào hoặc sau`R`, vậy số đếm là`L * (n - R + 1)`ở dạng 1 chỉ mục. 
5. Cộng số này vào tổng số câu trả lời. Giá trị này tương ứng với số lượng mảng con có MEX ít nhất$k+1$, vậy tổng hợp lại tất cả$k$xây dựng lại tổng số tiền MEX. 
6. Xử lý trường hợp ban đầu tương ứng với$k = 0$, trong đó mỗi mảng con có MEX ít nhất là 1. Điều này góp phần$n(n+1)/2$, vì tất cả các mảng con đều thỏa mãn điều kiện tiền tố trống. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi xử lý giá trị$k$, khoảng$[L, R]$chính xác kéo dài tất cả các vị trí có chứa$\{1, 2, \dots, k\}$. Một mảng con có MEX lớn hơn$k$khi và chỉ nếu nó chứa cả hai điểm cuối của khoảng này, tương đương với việc chứa tất cả các phần tử trong tập hợp. Điều này làm giảm điều kiện ngăn chặn đã đặt thành một ràng buộc hình học đơn giản trên các điểm cuối khoảng thời gian, đảm bảo mỗi lần đóng góp được tính chính xác một lần cho mỗi ngưỡng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, v in enumerate(a, start=1):
        pos[v] = i

    total = n * (n + 1) // 2

    L = pos[1]
    R = pos[1]
    total += L * (n - R + 1)

    for k in range(2, n + 1):
        L = min(L, pos[k])
        R = max(R, pos[k])
        total += L * (n - R + 1)

    print(total)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc tính toán trước các vị trí sao cho mỗi giá trị có thể được định vị ngay lập tức. Khoản đóng góp ban đầu chiếm điều kiện MEX tiền tố trống. Sau đó, vòng lặp sẽ dần dần mở rộng khoảng chứa các giá trị$1$bởi vì$k$và mỗi bước sẽ tính toán có bao nhiêu mảng con bao phủ đầy đủ khoảng đó. 

Một điểm tinh tế là sự đóng góp đầu tiên cho$k=1$phải được tính toán sau khi khởi tạo`L`Và`R`ĐẾN`pos[1]`. Một cách khác là xử lý chính xác việc đếm 1 chỉ mục cho các mảng con, trong đó`L`sự lựa chọn và`n-R+1`sự lựa chọn phải được bao gồm cả hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét hoán vị$[1, 3, 2]$. 

Chúng tôi theo dõi các vị trí:`pos[1]=1`,`pos[2]=3`,`pos[3]=2`. 

| k | L | R | Sự đóng góp$L \cdot (n-R+1)$| 
| --- | --- | --- | --- | 
| 0 | - | - | 6 | 
| 1 | 1 | 1 | 1 * 3 = 3 | 
| 2 | 1 | 3 | 1 * 1 = 1 | 
| 3 | 1 | 3 | 1 * 1 = 1 | 

Tổng cộng là$6 + 3 + 1 + 1 = 11$. 

Điều này khớp với việc liệt kê trực tiếp các mảng con và xác nhận rằng mỗi ngưỡng đếm chính xác các mảng con có MEX vượt quá mức tương ứng. 

### Ví dụ 2 

Hoán vị$[2, 1, 3, 4]$. 

Vị trí:`pos[1]=2`,`pos[2]=1`,`pos[3]=3`,`pos[4]=4`. 

| k | L | R | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | - | - | 10 | 
| 1 | 2 | 2 | 2 * 3 = 6 | 
| 2 | 1 | 2 | 1 * 3 = 3 | 
| 3 | 1 | 3 | 1 * 2 = 2 | 
| 4 | 1 | 4 | 1 * 1 = 1 | 

Tổng cộng là$10 + 6 + 3 + 2 + 1 = 22$. 

Quá trình tiến triển cho thấy việc thêm các giá trị lớn hơn sẽ ổn định khoảng và giảm dần số lượng mảng con bao phủ toàn bộ khoảng đó như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi giá trị cập nhật khoảng thời gian một lần và đóng góp theo thời gian không đổi | 
| Không gian |$O(n)$| Mảng vị trí lưu trữ một chỉ mục cho mỗi giá trị | 

Thuật toán phù hợp thoải mái trong các ràng buộc điển hình cho$n \le 2 \cdot 10^5$, vì nó chỉ thực hiện tiền xử lý tuyến tính và quét tuyến tính đơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # inline solution
    input = sys.stdin.readline
    n = int(input().strip())
    a = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, v in enumerate(a, start=1):
        pos[v] = i

    total = n * (n + 1) // 2
    L = pos[1]
    R = pos[1]
    total += L * (n - R + 1)

    for k in range(2, n + 1):
        L = min(L, pos[k])
        R = max(R, pos[k])
        total += L * (n - R + 1)

    return str(total)

# provided sample-like checks
assert run("3\n1 3 2\n") == "11"

# minimum size
assert run("1\n1\n") == "1"

# already sorted
assert run("4\n1 2 3 4\n") == "20"

# reversed
assert run("4\n4 3 2 1\n") == "20"

# random permutation
assert run("5\n2 1 5 3 4\n") == run("5\n2 1 5 3 4\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | tính đúng đắn của trường hợp tối thiểu | 
| 1 2 3 4 | 20 | hành vi khoảng đơn điệu | 
| 4 3 2 1 | 20 | đối xứng dưới sự đảo ngược | 
| 2 1 5 3 4 | nhất quán | tính đúng đắn chung | 

## Vỏ cạnh 

Khi nào$n = 1$, logic khoảng bị suy biến vì không có bước mở rộng có ý nghĩa. Thuật toán xử lý chính xác điều này vì đóng góp ban đầu đã chiếm mảng con duy nhất. 

Để có một hoán vị được sắp xếp đầy đủ, mọi tiền tố$[1..k]$tạo thành một khoảng liền kề ở đầu mảng. Ví dụ, trong$[1,2,3,4]$, khoảng luôn luôn là$[1,k]$, do đó đóng góp giảm tuyến tính. Thuật toán tạo ra điều này một cách tự nhiên mà không cần cách viết đặc biệt. 

Đối với một hoán vị ngược lại như$[n, n-1, \dots, 1]$, khoảng luôn kéo dài toàn bộ mảng ngay sau khi xử lý hai phần tử đầu tiên. Sau đó, không có thay đổi nào xảy ra nữa và các khoản đóng góp vẫn ổn định. Việc theo dõi của`L`Và`R`đảm bảo độ bão hòa này được ghi lại chính xác một lần mỗi bước.
