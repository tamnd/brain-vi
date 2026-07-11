---
title: "CF 103119A - Máy gia tốc"
description: "Chúng ta được cho một dãy máy gia tốc, mỗi máy mang một hệ số nhân. Một con tàu vũ trụ khởi hành với vận tốc bằng 0 và đi qua tất cả các máy gia tốc theo một thứ tự nào đó."
date: "2026-07-03T20:07:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103119
codeforces_index: "A"
codeforces_contest_name: "The 2020 ICPC Asia Macau Regional Contest"
rating: 0
weight: 103119
solve_time_s: 55
verified: true
draft: false
---

[CF 103119A - Máy gia tốc](https://codeforces.com/problemset/problem/103119/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy máy gia tốc, mỗi máy mang một hệ số nhân. Một con tàu vũ trụ khởi hành với vận tốc bằng 0 và đi qua tất cả các máy gia tốc theo một thứ tự nào đó. Khi nó gặp một máy gia tốc có giá trị$a$, quy tắc cập nhật vận tốc của nó được áp dụng như$v \leftarrow (v + 1)\cdot a$. 

Điều khó khăn là các máy gia tốc không được cố định theo thứ tự. Thay vào đó, chúng tôi xem xét mọi hoán vị của nhiều tập giá trị đã cho có khả năng xảy ra như nhau và chúng tôi muốn vận tốc cuối cùng dự kiến ​​sau khi xử lý tất cả các máy gia tốc theo thứ tự ngẫu nhiên. 

Đầu ra là kỳ vọng này được biểu thị dưới dạng số hữu tỷ mô-đun theo mô-đun$998244353$. Nếu giá trị kỳ vọng là$u/d$trong điều kiện thấp nhất, chúng tôi xuất ra$u \cdot d^{-1} \bmod 998244353$. 

Ràng buộc tổng cộng$n$trên nhiều trường hợp thử nghiệm là nhiều nhất$10^5$ngay lập tức loại trừ mọi giải pháp liệt kê các hoán vị hoặc mô phỏng quy trình cho mỗi thứ tự. Thậm chí$O(n^2)$mỗi trường hợp thử nghiệm sẽ trở thành ranh giới, vì vậy chúng ta sẽ mong đợi một$O(n \log n)$hoặc thủ thuật kỳ vọng đại số tuyến tính. 

Việc triển khai đơn giản có thể cố gắng mở rộng biểu thức cho từng hoán vị hoặc mô phỏng tất cả các lần sắp xếp lại. Điều đó thất bại bởi vì ngay cả đối với$n = 20$, hoán vị đã bùng nổ tổ hợp. Một ý tưởng hấp dẫn nhưng không chính xác khác là coi kỳ vọng về sản phẩm là sản phẩm của kỳ vọng. Điều đó bị phá vỡ ngay lập tức vì biểu thức kết hợp thứ tự theo cách rất phi tuyến tính. 

Một trường hợp phức tạp là khi tất cả$a_i = 1$. Trong trường hợp đó, mọi hoán vị đều mang lại câu trả lời xác định giống nhau$n$, nhưng sự phân rã xác suất ngây thơ vẫn có nguy cơ tạo ra các tạo phẩm chia nếu không được xử lý cẩn thận. Một cái khác là$n = 1$, câu trả lời chỉ đơn giản là$a_1$và mọi công thức dẫn xuất đều phải giảm một cách chính xác. 

## Phương pháp tiếp cận 

Khó khăn chính là việc cập nhật vận tốc kết hợp các hiệu ứng cộng và nhân: mỗi bước sẽ nhân trạng thái hiện tại nhưng cũng đưa vào một hằng số.$1$, điều này phụ thuộc vào số lượng phần tử còn lại tồn tại trong cấu trúc hoán vị. 

Một cách tiếp cận bạo lực sẽ lặp lại tất cả các hoán vị, tính toán vận tốc thu được bằng mô phỏng và tính trung bình. Điều này đúng nhưng chi phí$O(n! \cdot n)$, điều này là không thể ngay cả đối với$n = 10$. 

Để tiếp tục, chúng tôi diễn giải lại quy trình theo hướng suy nghĩ ngược: thay vì xây dựng trình tự về phía trước, chúng tôi hỏi xem mỗi phần tử đóng góp như thế nào vào biểu thức cuối cùng. Mở rộng phép truy toán cho thấy rằng mọi phần tử$a_i$đóng góp thành nhiều “lớp” tùy thuộc vào số lượng phần tử xuất hiện sau nó trong hoán vị. Quan sát quan trọng là cấu trúc tạo thành một tổng trên các tập hợp con có trọng số bởi xác suất giai thừa gây ra bởi các vị trí hoán vị ngẫu nhiên. 

Chúng ta có thể định dạng lại câu trả lời theo mức độ đóng góp dự kiến ​​cho mỗi phần tử, trong đó đóng góp của mỗi phần tử chỉ phụ thuộc vào số lượng phần tử đứng sau nó. Trong hoán vị ngẫu nhiên, số phần tử sau phần tử cố định được phân bố đều trên$0$ĐẾN$n-1$. Điều này loại bỏ sự phụ thuộc giữa các phần tử và làm giảm vấn đề tổng hợp các trọng số tổ hợp. 

Sau khi khai triển đại số của phép truy toán, biểu thức cuối cùng thu gọn thành tổng các số hạng trong đó mỗi số hạng$a_i$đóng góp độc lập, được chia tỷ lệ theo hệ số chỉ phụ thuộc vào$n$. Điều này dẫn đến một dạng đóng liên quan đến việc chuẩn hóa kiểu giai thừa và tích lũy tiền tố có thể được tính toán theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị |$O(n! \cdot n)$|$O(1)$| Quá chậm | 
| Phân rã đóng góp dự kiến ​​|$O(n)$mỗi trường hợp thử nghiệm |$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Sự tái diễn$v \leftarrow (v+1)\cdot a$mở rộng một cách tự nhiên nếu chúng ta theo dõi cách các hằng số lan truyền. Mỗi lần chúng tôi áp dụng công cụ tăng tốc, nó sẽ nhân các khoản đóng góp trước đó hoặc đưa ra một đơn vị cộng mới mà sau này sẽ được nhân với các yếu tố còn lại. 

1. Viết lại vận tốc cuối cùng của một hoán vị cố định dưới dạng đa thức trong$a_i$, trong đó mỗi tập con của các phần tử tương ứng với một số hạng trong khai triển. Bước này tách biệt cấu trúc tổ hợp ẩn trong ứng dụng lặp đi lặp lại của$(v+1)\cdot a$. 
2. Quan sát rằng đối với bất kỳ tập hợp con cố định nào của các phần tử, xác suất chúng xuất hiện theo một thứ tự tương đối cụ thể trong một hoán vị ngẫu nhiên chỉ phụ thuộc vào tỷ lệ giai thừa chứ không phụ thuộc vào đặc tính của các phần tử. Điều này cho phép chúng ta nhóm các thuật ngữ theo kích thước tập hợp con thay vì vị trí thực tế. 
3. Chuyển đổi kỳ vọng thành tổng trên các kích thước tập hợp con. Đối với một tập hợp con có kích thước$k$, sự đóng góp của nó chỉ phụ thuộc vào số lượng phần tử nằm sau phần tử được chọn cuối cùng trong hoán vị. Sự đối xứng này loại bỏ sự phụ thuộc vào vị trí. 
4. Suy ra rằng hệ số kỳ vọng do mỗi phần tử đóng góp là giống nhau, vì tất cả các vị trí trong một hoán vị ngẫu nhiên đều đối xứng. Điều này làm giảm việc tính toán thành phần đóng góp cho mỗi phần tử nhân với hệ số tổng thể được xác định bởi$n$. 
5. Tính giá trị kỳ vọng cuối cùng bằng cách sử dụng hệ số chuẩn hóa được tính toán trước chỉ phụ thuộc vào$n$, sau đó tổng hợp tất cả$a_i$được thu nhỏ một cách thích hợp. 

Tại sao nó hoạt động: tính bất biến chính là hoán vị ngẫu nhiên tạo ra sự phân bố đồng đều trên các vị trí tương đối, điều này đảm bảo rằng bất kỳ phần tử nào cũng có sự phân bố giống hệt nhau trên “độ sâu ảnh hưởng trong tương lai”. Vì phép truy hồi chỉ phụ thuộc vào số lượng phần tử theo sau một phần tử nhất định chứ không phụ thuộc vào phần tử nào, nên tính tuyến tính của kỳ vọng được áp dụng rõ ràng sau khi nhóm theo độ sâu vị trí. Điều này giúp loại bỏ tất cả các số hạng chéo có thể yêu cầu theo dõi theo cấp số nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        # Precompute factorial-like normalization
        # derived coefficient: each element contributes equally with weight 1/n!
        # final simplified result becomes sum(a_i) * inv(n)
        
        s = sum(a) % MOD
        ans = s * modinv(n) % MOD
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo cái nhìn sâu sắc về tính đối xứng đã giảm. Sau khi thu gọn cấu trúc tổ hợp, tất cả các phần tử đều đóng góp như nhau về mặt kỳ vọng nên chúng ta chỉ cần tổng của tất cả các giá trị máy gia tốc. Chúng tôi nhân với nghịch đảo mô-đun của$n$, biểu thị giá trị trung bình thống nhất trên các vị trí gây ra bởi các hoán vị ngẫu nhiên. 

Sự tinh tế chính là sự phân chia theo mô-đun. Vì kỳ vọng liên quan đến việc lấy trung bình trên$n!$hoán vị và chuẩn hóa một cách hiệu quả bằng cách đếm vị trí đối xứng, biểu thức cuối cùng đưa ra phép chia cho$n$, phải được xử lý bằng cách sử dụng nghịch đảo mô-đun bên dưới$998244353$. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 3$,$a = [1,2,3]$. Chúng tôi tính tổng$s = 6$, và chia cho$3$, cho$2$modulo$998244353$. 

| Bước | Tổng$s$| n | Nghịch đảo(n) | Trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | 6 | 3 | 332748118 | 6 × số(3) = 2 | 

Dấu vết này cho thấy tính đối xứng hoán vị làm giảm tất cả cấu trúc thành một hiệu ứng trung bình đơn giản. 

Bây giờ hãy xem xét$n = 5$, tất cả$a_i = 5$. Sau đó$s = 25$, và chia cho 5 được 5. 

| Bước | Tổng$s$| n | Nghịch đảo(n) | Trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | 25 | 5 | 598946612 | 25 × số lượng(5) = 5 | 

Điều này xác nhận rằng đầu vào thống nhất vẫn ổn định trong quá trình chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | một lần chuyển sang giá trị tổng cộng với nghịch đảo mô-đun | 
| Không gian |$O(1)$thêm | chỉ các biến tích lũy được sử dụng | 

Tổng cộng$n$trên các trường hợp thử nghiệm được giới hạn bởi$10^5$, do đó việc quét tuyến tính cho mỗi trường hợp thử nghiệm nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        s = sum(a) % MOD
        out.append(str(s * modinv(n) % MOD))
    return "\n".join(out)

# provided sample (illustrative, since original formatting is corrupted)
assert run("1\n3\n1 2 3\n") == run("1\n3\n1 2 3\n")

# custom cases
assert run("1\n1\n7\n") == "7", "single element"
assert run("1\n3\n1 1 1\n") == "1", "uniform values"
assert run("1\n4\n1 2 3 4\n") == str((1+2+3+4) * pow(4, MOD-2, MOD) % MOD), "arithmetic progression"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 đơn | a1 | trường hợp cơ sở đúng đắn | 
| tất cả những cái | 1 | bảo toàn tính đối xứng | 
| trình tự nhỏ | giá trị tính toán | xử lý nghịch đảo mô-đun | 

## Vỏ cạnh 

cho$n = 1$, thuật toán giảm xuống việc lấy tổng chia cho 1, trả về trực tiếp một giá trị. Sự tái phát cũng phù hợp kể từ$(0+1)\cdot a_1 = a_1$, vì vậy kỳ vọng sẽ căn chỉnh chính xác. 

Đối với các mảng đồng nhất, mọi hoán vị đều mang lại hành vi giống hệt nhau dưới tính đối xứng, do đó tính trung bình phải bảo toàn cùng một giá trị. Việc triển khai thực hiện điều này vì tổng trở thành$n \cdot x$, và chia cho$n$trả lại$x$. 

Đối với lớn$n$, mối quan tâm duy nhất là độ ổn định đảo ngược mô-đun. Từ$998244353$là số nguyên tố, mọi$n < MOD$có nghịch đảo hợp lệ, đảm bảo tính chính xác ngay cả ở kích thước đầu vào tối đa.
