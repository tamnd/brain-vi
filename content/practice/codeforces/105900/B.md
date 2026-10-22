---
title: "CF 105900B - Bộ bài vô biên"
description: "Chúng ta được cấp một bộ sưu tập vô hạn các thẻ trong đó mỗi thẻ được gắn nhãn bằng lũy ​​thừa của một số nguyên cố định $n$. Thẻ đầu tiên có giá trị $n^0$, thẻ thứ hai $n^1$, thẻ thứ ba $n^2$, v.v."
date: "2026-06-21T15:17:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105900
codeforces_index: "B"
codeforces_contest_name: "VI UnBalloon Contest Mirror"
rating: 0
weight: 105900
solve_time_s: 59
verified: true
draft: false
---

[CF 105900B - Bộ bài vô biên](https://codeforces.com/problemset/problem/105900/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ sưu tập vô hạn các thẻ trong đó mỗi thẻ được gắn nhãn bằng lũy thừa của một số nguyên cố định$n$. Thẻ đầu tiên có giá trị$n^0$, thứ hai$n^1$, thứ ba$n^2$, vân vân. Từ bộ bài này, một “nước đi” được hình thành bằng cách chọn bất kỳ tập hợp con quân bài hữu hạn nào và giá trị của nước đi chỉ đơn giản là tổng của các giá trị quân bài đã chọn. 

Vì vậy mỗi nước đi tương ứng với việc chọn một số chỉ số$i$, và tính tổng$n^i$. Vì mỗi quân bài có thể được chọn hoặc không được chọn nên mỗi nước đi về cơ bản là một tập hợp con của lũy thừa của$n$. 

Sau đó, tất cả các nước đi có thể được sắp xếp theo tổng kết quả của chúng và chúng ta được yêu cầu tính$k$- tổng nhỏ nhất trong thứ tự này. 

Các ràng buộc rất lớn: cả hai$n$Và$k$có thể lên đến$10^9$. Điều này ngay lập tức loại trừ mọi nỗ lực nhằm tạo ra các tập hợp con một cách rõ ràng hoặc thậm chí liệt kê các giá trị lên đến$k$bằng mô phỏng. Số lượng tập hợp con tăng theo cấp số nhân với số lượng thẻ được xem xét, do đó, một cách tiếp cận ngây thơ sẽ bùng nổ ngay cả đối với những mức giới hạn khiêm tốn. 

Một điểm tinh tế là hiểu liệu tập hợp con trống có được bao gồm hay không. Các ví dụ làm rõ rằng không phải vậy, vì chuỗi bắt đầu từ 1 chứ không phải 0. Điều đó có nghĩa là chúng ta chỉ xem xét các tập con không trống. 

Một trường hợp thất bại điển hình xuất phát từ việc cố gắng tạo ra số tiền một cách tham lam hoặc sử dụng hàng đợi ưu tiên trên các tập hợp con. Ví dụ, thậm chí chỉ với sức mạnh$1, n, n^2$, số lượng tập hợp con đã là 8 và việc mở rộng cấu trúc này để đạt tới$k = 10^9$là không thể. 

Khó khăn thực sự là nhận ra cấu trúc của các khoản tiền này theo thứ tự được sắp xếp. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các tập hợp con của các chỉ số và tính tổng của chúng, sau đó sắp xếp chúng. Ngay cả khi chúng tôi cố gắng tạo chúng tăng dần, số lượng tập hợp con vẫn tăng lên$2^m$Ở đâu$m$là số lượng quyền hạn được coi là. Để đạt được những giá trị xung quanh$k$, chúng ta sẽ cần$m$đủ lớn đó$2^m \ge k$, nghĩa$m \approx 30$đã đủ cho$k \le 10^9$. Tuy nhiên, việc liệt kê rõ ràng các tập hợp con và sắp xếp chúng vẫn yêu cầu lưu trữ khoảng một tỷ giá trị trong trường hợp xấu nhất, điều này là không thể. 

Cái nhìn sâu sắc quan trọng là ngừng coi các tập hợp con là các đối tượng tổ hợp và thay vào đó diễn giải lại cấu trúc tổng. Mỗi tập hợp con tương ứng với việc chọn có bao gồm từng lũy ​​thừa hay không$n^i$. Đây chính xác là một lựa chọn nhị phân cho mỗi chỉ mục, vì vậy mỗi bước di chuyển đều tương ứng với một chuỗi nhị phân và giá trị là đánh giá của chuỗi nhị phân đó trong cơ sở$n$chỉ sử dụng các chữ số 0 và 1. 

Bây giờ, quan sát quan trọng là việc sắp xếp các giá trị này theo giá trị số của chúng tương đương với việc sắp xếp các biểu diễn nhị phân của chúng được hiểu là các số có trọng số vị trí$n^i$. Vì quyền lực cao hơn thống trị quyền lực thấp hơn (vì$n \ge 2$), thứ tự này hoàn toàn phù hợp với thứ tự tự nhiên của các số nguyên nhị phân được diễn giải theo ý nghĩa vị trí cơ số 2 tiêu chuẩn. 

Điều này có nghĩa là$k$-giá trị nhỏ nhất thứ tương ứng chính xác với văn bản$k$ở dạng nhị phân và diễn giải từng bit như có bao gồm lũy thừa tương ứng của$n$. các$i$-bit thứ đóng góp$n^i$đến số tiền cuối cùng. 

Vì vậy, thay vì tạo ra các tập con, chúng tôi trực tiếp giải mã$k$thành nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(2^m \log 2^m)$|$O(2^m)$| Quá chậm | 
| Ánh xạ biểu diễn nhị phân |$O(\log k)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn tính giá trị được biểu thị bằng dạng nhị phân của$k$, trong đó mỗi bit tương ứng với việc chọn lũy thừa của$n$. 

1. Đọc$n$Và$k$. Chúng tôi đối xử$k$dưới dạng thứ hạng trong danh sách được sắp xếp của các tổng tập hợp con. 
2. Chuyển đổi$k$vào biểu diễn nhị phân của nó. Mỗi vị trí bit$i$cho chúng ta biết liệu$n^i$được bao gồm trong tập hợp con. 
3. Khởi tạo câu trả lời đang chạy là 0 và giá trị công suất hiện tại là 1, biểu thị$n^0$. 
4. Lặp lại các bit của$k$từ ít quan trọng nhất đến quan trọng nhất. Đối với mỗi bit: 

Nếu bit là 1, hãy cộng công suất hiện tại của$n$để trả lời. 
5. Sau khi xử lý từng bit, nhân công suất hiện tại với$n$(mô-đun$10^9+7$) để chuyển sang số mũ tiếp theo. 
6. Xuất ra modulo tổng tích lũy cuối cùng$10^9+7$. 

Lý do nhân với$n$có tác dụng là mỗi bước sẽ chuyển chúng ta từ$n^i$ĐẾN$n^{i+1}$, phù hợp với kết cấu của boong. 

### Tại sao nó hoạt động 

Mỗi tập hợp con tương ứng duy nhất với một chuỗi nhị phân và giá trị của nó là một hệ thống số vị trí có cơ số$n$và các chữ số bị giới hạn ở mức 0 hoặc 1. Vì$n \ge 2$, các chỉ số cao hơn luôn lấn át các chỉ số thấp hơn, đảm bảo rằng việc sắp xếp theo giá trị giống hệt với việc sắp xếp theo giá trị nguyên của mặt nạ nhị phân. Do đó thứ tự xếp hạng của các tập hợp con phù hợp với thứ tự tự nhiên của các số nguyên$k = 1, 2, 3, \dots$và biểu diễn nhị phân của$k$trực tiếp mã hóa tập hợp con được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, k = map(int, input().split())

    ans = 0
    power = 1

    while k > 0:
        if k & 1:
            ans = (ans + power) % MOD
        power = (power * n) % MOD
        k >>= 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện ý tưởng phân rã nhị phân. Biến`power`bài hát$n^i$khi chúng tôi quét các bit của$k$. Mỗi lần chúng ta gặp một bit đã đặt, chúng ta sẽ cộng lũy ​​thừa tương ứng vào kết quả. Modulo được áp dụng ở mọi bước để tránh tràn. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ tính toán trước tất cả các lũy thừa lên tới 30 hoặc 31 một cách rõ ràng. Thay vào đó, chúng tôi xây dựng chúng lặp đi lặp lại, thế là đủ vì$k \le 10^9$ngụ ý nhiều nhất là 30 bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 3, k = 3$Nhị phân của$k$là`11`. 

| Chỉ số bit | Giá trị bit | Nguồn điện hiện tại$3^i$| Đóng góp | Tổng chạy | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 
| 1 | 1 | 3 | 3 | 4 | 

Câu trả lời cuối cùng là 4. 

Điều này phù hợp với trình tự:$1, 3, 4, 9, 10, \dots$, trong đó số hạng thứ ba là 4. 

### Ví dụ 2:$n = 3, k = 4$Nhị phân của$k$là`100`. 

| Chỉ số bit | Giá trị bit | Nguồn điện hiện tại$3^i$| Đóng góp | Tổng chạy | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 0 | 0 | 
| 1 | 0 | 3 | 0 | 0 | 
| 2 | 1 | 9 | 9 | 9 | 

Câu trả lời cuối cùng là 9. 

Điều này tương ứng với việc chỉ chọn$3^2$, là tổng tập con nhỏ thứ tư. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log k)$| Chúng tôi xử lý một chút$k$, nhiều nhất là khoảng 30 thao tác | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được duy trì | 

Giải pháp này đủ nhanh để$k \le 10^9$, vì số lần lặp bị giới hạn bởi số bit trong$k$. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, sys.stdin.readline().split())

    ans = 0
    power = 1
    while k > 0:
        if k & 1:
            ans = (ans + power) % MOD
        power = (power * n) % MOD
        k >>= 1

    return str(ans)

# provided samples
assert run("3 3\n") == "4"
assert run("3 5\n") == "10"

# custom cases
assert run("2 1\n") == "1", "minimum k"
assert run("2 2\n") == "2", "single higher bit"
assert run("10 7\n") == str(1 + 10 + 100), "mixed binary structure"
assert run("3 1\n") == "1", "smallest non-empty subset"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | 1 | tập hợp con nhỏ nhất có thể | 
| 2 2 | 2 | lựa chọn bit cao đơn | 
| 10 7 | 111 | tính chính xác tích lũy nhiều bit | 
| 3 1 | 1 | tính đúng đắn của trường hợp cơ sở | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$k$là sức mạnh của hai. Ví dụ,$k = 8$tương ứng với nhị phân`1000`, nghĩa là chỉ$n^3$được chọn. Thuật toán xử lý điều này một cách tự nhiên vì chỉ có một bit duy nhất đóng góp và tất cả các lũy thừa thấp hơn đều bị bỏ qua. 

Một trường hợp khác là khi tất cả các bit thấp hơn được đặt, chẳng hạn như$k = 7$cho đi`111`. Ở đây tổng trở thành$1 + n + n^2$. Việc tích lũy lặp lại xây dựng chính xác điều này mà không cần bất kỳ xử lý đặc biệt nào. 

Trường hợp tinh tế cuối cùng là lớn$n$. Vì mọi số học đều là modulo$10^9+7$, lũy thừa trung gian được giảm đi một cách an toàn và phép nhân không bao giờ vượt quá giới hạn số nguyên điển hình trong Python, nhưng việc rút gọn theo mô-đun vẫn giữ cho các giá trị bị giới hạn và nhất quán.
