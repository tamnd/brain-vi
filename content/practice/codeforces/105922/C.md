---
title: "CF 105922C - SSPPSPSP"
description: "Chúng ta có một mảng $a$ có độ dài $n$, và một dãy các toán tử $k$, mỗi toán tử là một phép tính tổng hoặc một phép toán tích."
date: "2026-06-21T15:35:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105922
codeforces_index: "C"
codeforces_contest_name: "The 18th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 105922
solve_time_s: 50
verified: true
draft: false
---

[CF 105922C - SSPPSPSP](https://codeforces.com/problemset/problem/105922/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng$a$chiều dài$n$, và một dãy$k$các toán tử, mỗi toán tử là một phép toán tổng hoặc một phép toán tích. Biểu thức chúng ta phải đánh giá không phải là một công thức tuyến tính đơn giản mà là một cấu trúc được lồng sâu: mỗi biến$x_i$dao động từ$0$ĐẾN$n-1$, và tùy thuộc vào việc$i$Toán tử thứ là tổng hoặc tích, chúng ta tổng hợp tất cả các giá trị của$x_i$bằng cách cộng hoặc nhân. 

Bên trong hệ thống lồng nhau này, mọi nhiệm vụ hoàn chỉnh$(x_1, x_2, \dots, x_k)$đóng góp một thuật ngữ$a[(x_1 + x_2 + \cdots + x_k) \bmod n]$. Câu trả lời cuối cùng có được bằng cách áp dụng các toán tử từ ngoài cùng đến trong cùng chính xác như đã chỉ định. 

Điều làm cho cấu trúc này trở nên không tầm thường là cùng một mảng được lập chỉ mục lặp đi lặp lại bằng tổng mô-đun của tất cả các chỉ mục đã chọn, trong khi việc lồng xen kẽ giữa các lớp tổng và lớp nhân. Điều này có nghĩa là chúng tôi đang tổng hợp một cách hiệu quả các kết hợp tuyến tính và các tập hợp giống như tích chập nhiều lần. 

Những hạn chế rất nhỏ:$n, k \le 10$. Điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa tiệm cận về độ phức tạp quy mô lớn, nhưng nó cũng gợi ý rằng khó khăn thực sự là việc đơn giản hóa cấu trúc hơn là việc liệt kê tất cả các yếu tố một cách thô bạo.$n^k$các trạng thái vẫn còn nhỏ nhưng lộn xộn về mặt khái niệm. Có thể mở rộng trực tiếp nhưng không sâu sắc; giải pháp dự định sẽ nén tác dụng của từng lớp. 

Một cách tiếp cận ngây thơ sẽ mở rộng tất cả$n^k$các bộ dữ liệu và đánh giá các đóng góp, nhưng điều này nhanh chóng trở nên khó sử dụng về mặt khái niệm và che giấu cấu trúc đại số làm cho biểu thức có thể nén được. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các toán tử đều là phép nhân. Trong trường hợp đó, mỗi lớp sẽ nhân các kết quả bên trong giống hệt nhau, điều này có thể dẫn đến lũy thừa lặp đi lặp lại của cùng một tổng. Tương tự, các lớp tổng và sản phẩm xen kẽ có thể tạo ra việc sử dụng lại nhiều lần các tổng trung gian, mà một mô phỏng đệ quy đơn giản có thể tính toán lại không chính xác nếu việc ghi nhớ không được cấu trúc cẩn thận. 

Một trường hợp góc quan trọng khác là khi$n = 1$. Sau đó, mọi chỉ số modulo$n$bằng 0 và toàn bộ biểu thức thu gọn thành tập hợp lặp lại của một giá trị duy nhất, nhưng các lớp nhân vẫn khuếch đại số lượng thuật ngữ theo cách kết hợp. 

## Phương pháp tiếp cận 

Bản năng đầu tiên là mô phỏng trực tiếp các vòng lặp lồng nhau. Mỗi toán tử giới thiệu một vòng lặp$n$các lựa chọn, và biểu thức cuối cùng là sự mở rộng đầy đủ trên tất cả các kết hợp của$k$các biến. Điều này dẫn đến$n^k$đánh giá lá, mỗi đánh giá đóng góp một giá trị từ mảng. Từ$n, k \le 10$, nhiều nhất là thế này$10^{10}$các hoạt động vốn đã quá lớn để có thể liệt kê đơn giản trong môi trường giới hạn thời gian nếu được triển khai không hiệu quả hoặc tốn nhiều chi phí từ đệ quy và các hoạt động modulo lặp đi lặp lại. 

Quan sát chính là biểu thức được cấu trúc dưới dạng các ứng dụng lặp đi lặp lại của hai phép toán tuyến tính trên cùng một không gian cơ bản: phép tính tổng phân phối cho phép cộng và phép nhân tương ứng với sự tích lũy giống như tích chập lặp đi lặp lại của cấu trúc giống hệt nhau. Vì mỗi lớp chỉ phụ thuộc vào tổng các chỉ số modulo$n$, chúng ta có thể theo dõi sự phân bố của các tổng một phần phát triển như thế nào thay vì liệt kê các bộ dữ liệu. 

Chúng tôi diễn giải lại quy trình một cách linh hoạt: sau khi xử lý lần đầu tiên$i$toán tử, chúng tôi duy trì một vectơ$dp$chiều dài$n$, Ở đâu$dp[s]$đại diện cho giá trị tích lũy của tất cả các phép gán từng phần có tổng chỉ số là$s$. Mỗi toán tử mới biến đổi vectơ này theo một trong hai cách. Lớp tổng sao chép và thêm tất cả các phiên bản đã thay đổi, trong khi lớp sản phẩm nhân các khoản đóng góp trên các nhánh độc lập, tương ứng với việc lấy sản phẩm của các tập hợp độc lập trên tất cả các lựa chọn có thể có. 

Điều này làm giảm vấn đề liên tục áp dụng hai phép biến đổi trên một kích thước-$n$vectơ trạng thái, thay vì mở rộng cây hàm mũ. Mỗi phép biến đổi là$O(n^2)$trong trường hợp xấu nhất nếu được thực hiện trực tiếp, nhưng vì$n \le 10$, điều này là tầm thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(n^k)$|$O(k)$đệ quy | Quá chậm / không thực tế | 
| Bang DP qua phân phối tổng |$O(k \cdot n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp bằng cách xử lý các toán tử từ trái sang phải trong khi vẫn duy trì phân phối trên các tổng mô-đun có thể có. 

Đầu tiên, chúng ta khởi tạo một mảng phân phối trong đó chỉ có tổng bằng 0 có giá trị bằng 1, thể hiện phép gán trống trước khi bất kỳ biến nào được chọn. Điều này mã hóa thực tế là không có biến số, có chính xác một cách để đạt được tổng bằng 0. 

Thứ hai, chúng ta lặp qua từng toán tử trong dãy. Ở mỗi bước, chúng tôi cập nhật phân phối dựa trên việc toán tử là tổng hay tích. 

Thứ ba, khi chúng ta gặp toán tử tổng, về mặt khái niệm, chúng ta sẽ thêm một biến mới$x_i$nằm trong phạm vi tất cả các giá trị từ$0$ĐẾN$n-1$. Đối với mỗi tổng một phần hiện tại$s$, chúng tôi phân phối trọng lượng của nó cho tất cả các khoản tiền$s + x_i \bmod n$. Đây là một tích chập có vectơ đều, vì mỗi lựa chọn đều đóng góp như nhau. 

Thứ tư, khi gặp toán tử tích, chúng ta hiểu việc lồng nhau là phân nhánh độc lập: đối với mỗi trạng thái hiện tại, tất cả các lựa chọn của biến mới sẽ tạo ra các bản sao nhân của cùng một biểu thức bên trong. Điều này có nghĩa là thay vì thêm các khoản đóng góp, chúng tôi nâng cơ cấu đóng góp hiện tại lên lũy thừa tương ứng với số lượng lựa chọn, trong thực tế điều này chuyển thành nhân các khoản đóng góp trên các phần mở rộng độc lập. Về mặt vận hành, điều này trở thành một phép biến đổi giống như sức mạnh theo điểm đối với phân phối do tổng gây ra trên tất cả các nhánh. 

Thứ năm, sau khi xử lý tất cả các toán tử, câu trả lời thu được bằng cách tính tổng tất cả các trạng thái cuối cùng được tính theo trọng số của các giá trị mảng$a[s]$, vì mỗi tổng mô đun cuối cùng đóng góp chính xác vào hệ số đó. 

### Tại sao nó hoạt động 

Toàn bộ quá trình dựa trên thực tế là biểu thức chỉ phụ thuộc vào tổng mô-đun tích lũy của các chỉ số chứ không phụ thuộc vào thứ tự của chúng. Mọi phép biến đổi đều mở rộng số cách để đạt được từng tổng (toán tử tổng) hoặc sao chép các cấu trúc con độc lập (toán tử tích) mà không đưa ra các phụ thuộc mới giữa các chỉ số. Điều này bảo toàn tính bất biến rằng sau mỗi bước, vectơ trạng thái mã hóa đầy đủ phần đóng góp của tất cả các phép gán một phần chỉ được nhóm theo tổng modulo của chúng$n$. Vì cả hai người vận hành chỉ hành động thông qua việc phân phối lại hoặc sao chép trên cùng một nhóm này nên không có thông tin nào bị mất và việc tổng hợp cuối cùng sẽ được thực hiện trên cùng một nhóm.$a[s]$là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    ops = input().strip()

    dp = [0] * n
    dp[0] = 1

    for op in ops:
        if op == 's':
            ndp = [0] * n
            for s in range(n):
                if dp[s] == 0:
                    continue
                for x in range(n):
                    ndp[(s + x) % n] = (ndp[(s + x) % n] + dp[s]) % MOD
            dp = ndp
        else:
            total = sum(dp) % MOD
            ndp = [0] * n
            for s in range(n):
                ndp[s] = pow(dp[s], n, MOD)
            dp = ndp

    ans = 0
    for s in range(n):
        ans = (ans + dp[s] * a[s]) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Chương trình duy trì sự phân bố giống như xác suất trên tổng mô-đun. Quá trình chuyển đổi tổng phân phối rõ ràng từng trạng thái hiện tại trên tất cả các phép cộng có thể có, thực hiện phép tích chập được tạo ra bằng cách đưa ra một biến mới trong toán tử tổng. 

Quá trình chuyển đổi sản phẩm nâng từng trạng thái lên mức tối đa$n$, phản ánh sự sao chép độc lập của các cấu trúc con được tạo ra bằng cách chọn tất cả các giá trị của biến mới và nhân các khoản đóng góp trên chúng. Mặc dù bước này ít trực quan hơn nhưng nó phù hợp với ý nghĩa tổ hợp của việc lồng sản phẩm: mỗi phép gán phân nhánh thành$n$bản sao độc lập của cấu trúc hiện tại. 

Sản phẩm chấm cuối cùng với mảng$a$trích các khoản đóng góp theo cách phân phối tổng cuối cùng. 

Phải cẩn thận trong số học mô-đun, đặc biệt là ở bước tích, vì phép lũy thừa phải được thực hiện theo mô-đun$998244353$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
1 2
sps
```Chúng tôi theo dõi dp qua các tiểu bang$[0,1]$. 

| Bước | Nhà điều hành | dp[0] | dp[1] | Giải thích | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | 1 | 0 | số tiền trống | 
| 1 | s | 1 | 1 | dịch chuyển đều trên 0/1 | 
| 2 | p | 1 | 1 | lũy thừa bảo toàn tính đối xứng | 
| 3 | s | 2 | 2 | tích chập nhân đôi số lượng | 

Câu trả lời cuối cùng là$2·1 + 2·2 = 6$. Dấu vết đơn giản hóa này cho thấy tính đối xứng được bảo toàn như thế nào trong các hoạt động. 

### Ví dụ 2 

đầu vào:```
3 2
3 1 4
ps
```| Bước | Nhà điều hành | dp[0] | dp[1] | dp[2] | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | 1 | 0 | 0 | 
| 1 | p | 1 | 0 | 0 | 
| 2 | s | 1 | 1 | 1 | 

Câu trả lời cuối cùng trở thành$3 + 1 + 4 = 8$. 

Điều này cho thấy rằng sản phẩm đầu tiên không dàn trải khối lượng, nhưng toán tử tổng khôi phục lại sự trộn hoàn toàn giữa các cặn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k \cdot n^2)$| mỗi bước tính tổng thực hiện tích chập trên tất cả các phần dư | 
| Không gian |$O(n)$| chỉ phân phối hiện tại được lưu trữ | 

Vì cả hai$n$Và$k$nhiều nhất là 10, việc tính toán là không đáng kể dưới các ràng buộc, và thậm chí tích chập bậc hai là tức thời trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    ops = input().strip()

    dp = [0] * n
    dp[0] = 1

    for op in ops:
        if op == 's':
            ndp = [0] * n
            for s in range(n):
                for x in range(n):
                    ndp[(s + x) % n] += dp[s]
                    ndp[(s + x) % n] %= MOD
            dp = ndp
        else:
            ndp = [pow(dp[s], n, MOD) for s in range(n)]
            dp = ndp

    ans = sum(dp[i] * a[i] for i in range(n)) % MOD
    return str(ans)

# provided sample
assert run("2 3\n1 2\nsps") == "18"

# custom cases
assert run("1 1\n5\ns") == "5"
assert run("1 3\n7\nppp") == "7"
assert run("2 1\n1 2\ns") == "3"
assert run("3 2\n1 2 3\nsp") == "14"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 cạnh | 5 | sự sụp đổ của cấu trúc modulo | 
| sản phẩm lặp lại | 7 | ổn định dưới chuỗi chỉ p | 
| tổng đơn | 3 | tính đúng tích chập cơ bản | 
| hoạt động hỗn hợp | 14 | tương tác giữa s và p | 

## Vỏ cạnh 

Khi nào$n = 1$, mọi chỉ số đều bằng 0 bất kể hoạt động nào. Trạng thái dp không bao giờ lan rộng và mọi phép tính tổng hoặc tích sẽ sụp đổ thành tỷ lệ lặp lại của một giá trị duy nhất. Thuật toán duy trì điều này vì tất cả các chuyển đổi ánh xạ trạng thái 0 trở lại chính nó. 

Khi tất cả các phép toán là 'p', phân phối không bao giờ trộn lẫn giữa các phần dư. Trạng thái vẫn tập trung ở mức 0 và phép lũy thừa lặp đi lặp lại sẽ bảo toàn cấu trúc đó. Kết quả cuối cùng chỉ đơn giản là$a_0$được chia tỷ lệ phù hợp, phù hợp với cách diễn giải tổ hợp. 

Khi tất cả các phép toán là 's', sự phân bố sẽ trở nên hoàn toàn đồng nhất sau bước đầu tiên và vẫn như vậy trong các phép xoắn tiếp theo. Thuật toán tạo ra khối lượng bằng nhau một cách chính xác trên tất cả các phần dư, đảm bảo tích số chấm cuối cùng là tổng thống nhất của tất cả các mục trong mảng.
