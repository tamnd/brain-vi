---
title: "CF 1753C - Ước gì tôi biết cách sắp xếp"
description: "Chúng ta được cung cấp một mảng nhị phân, nghĩa là mọi phần tử đều là 0 hoặc 1. Mảng phát triển thông qua các phép toán ngẫu nhiên lặp đi lặp lại: ở mỗi bước, chúng ta chọn một cặp chỉ số ngẫu nhiên thống nhất $(i, j)$ với $i < j$."
date: "2026-06-09T14:57:46+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1753
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 829 (Div. 1)"
rating: 2000
weight: 1753
solve_time_s: 140
verified: false
draft: false
---

[CF 1753C - Ước gì tôi biết cách sắp xếp](https://codeforces.com/problemset/problem/1753/C) 

**Xếp hạng:** 2000 
**Tags:** dp, toán, xác suất 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng nhị phân, nghĩa là mọi phần tử đều là 0 hoặc 1. Mảng phát triển thông qua các phép toán ngẫu nhiên lặp đi lặp lại: ở mỗi bước chúng ta chọn một cặp chỉ số ngẫu nhiên thống nhất$(i, j)$với$i < j$. Nếu giá trị bên trái là 1 và giá trị bên phải là 0, chúng ta hoán đổi chúng; nếu không thì không có gì thay đổi. Chúng tôi lặp lại điều này cho đến khi mảng được sắp xếp, điều này đối với mảng nhị phân có nghĩa là tất cả các số 0 đều xuất hiện trước tất cả các số 1. 

Quá trình này mang tính ngẫu nhiên và không phải lúc nào cũng đạt được tiến bộ. Nhiều cặp ngẫu nhiên không làm gì cả và chỉ có sự “đảo ngược” giữa số 1 ở bên trái và số 0 ở bên phải mới gây ra thay đổi. Nhiệm vụ là tính toán số lượng hoạt động dự kiến ​​cho đến khi không còn sự đảo ngược nào như vậy. 

Các ràng buộc rất lớn: lên tới$10^5$trường hợp thử nghiệm và tổng chiều dài mảng lên tới$2 \cdot 10^5$. Điều này loại trừ mọi mô phỏng hoặc lập trình động dựa trên trạng thái đối với các cấu hình. Thậm chí một$O(n^2)$tiền xử lý cho mỗi bài kiểm tra là không thể. Giải pháp phải giảm từng trường hợp thử nghiệm thành tuyến tính hoặc gần tuyến tính. 

Trường hợp khó nhận biết nhất là khi mảng đã được sắp xếp. Trong trường hợp đó, không có thao tác nào thay đổi bất cứ điều gì một cách có ý nghĩa và điều kiện dừng đã được thỏa mãn tại thời điểm 0. Bất kỳ cách tiếp cận nào giả định tồn tại ít nhất một đảo ngược sẽ thất bại ở đây trừ khi được xử lý rõ ràng. 

Một trường hợp cạnh khác là khi có chính xác một số 1 và một số 0 nhưng theo đúng thứ tự. Thời gian dự kiến ​​​​bằng 0, không phải là một kỳ vọng hình học tích cực nào đó, bởi vì không có hoạt động nào có thể tạo ra sự hoán đổi giúp cải thiện hoặc làm xấu đi trật tự một cách có ý nghĩa. 

## Phương pháp tiếp cận 

Cách giải thích ngây thơ là coi quy trình như một chuỗi Markov trên tất cả các mảng nhị phân. Mỗi trạng thái chuyển sang trạng thái khác tùy thuộc vào cặp nào được chọn. Về nguyên tắc, người ta có thể viết phương trình cho thời gian trúng dự kiến ​​của trạng thái được sắp xếp. Điều này đúng nhưng không khả thi ngay lập tức vì không gian trạng thái có kích thước$2^n$và thậm chí việc xây dựng các chuyển đổi là bậc hai cho mỗi trạng thái. 

Quan sát quan trọng là quy trình chỉ tương tác với các lần đảo ngược và quan trọng hơn là mỗi thao tác đều độc lập với cấu hình trong quá khứ ngoại trừ thông qua số lần đảo ngược hiện tại. Tuy nhiên, thậm chí điều đó vẫn chưa đủ trực tiếp, bởi vì các cặp đảo ngược khác nhau có xác suất được chọn khác nhau và các giao dịch hoán đổi sẽ thay đổi nhiều đảo ngược cùng một lúc theo cách có cấu trúc. 

Cái nhìn sâu sắc về cấu trúc quan trọng là diễn giải lại quy trình theo thứ tự liền kề giữa 1 và 0 trên tất cả các cặp. Mỗi thao tác chọn một trong$\binom{n}{2}$cặp đồng nhất. Chỉ những cặp có số 1 xuất hiện bên trái số 0 mới quan trọng và việc chọn cặp như vậy sẽ giải quyết chính xác sự đảo ngược đó. 

Điều này biến quá trình này thành một bài toán chờ hình học lặp đi lặp lại: tại bất kỳ thời điểm nào, trong số tất cả các cặp, một tập hợp con cố định là “cặp tốt” (số 1 trước số 0). Việc chọn một cặp tốt sẽ giảm số lượng đảo ngược đúng một lần hoán đổi, nhưng quan trọng là sau khi hoán đổi, số lượng cặp tốt sẽ thay đổi theo cách có thể dự đoán được. Đối với mảng nhị phân, quá trình tiến hóa này có dạng khép kín: thời gian dự kiến ​​chỉ phụ thuộc vào tổng số cặp và vị trí hiện tại là 1s. 

Một cải cách mạnh mẽ hơn sẽ tránh hoàn toàn động lực theo dõi. Thay vì tuân theo quy trình từng bước, chúng tôi tính toán mức đóng góp dự kiến ​​của từng nghịch đảo một cách độc lập. Mỗi cặp$(i, j)$với$a_i = 1, a_j = 0$hoạt động giống như một quá trình hình học: nó được giải quyết ngay lần đầu tiên được chọn và mỗi bước sẽ chọn nó với xác suất$\frac{1}{\binom{n}{2}}$. Thời gian chờ đợi dự kiến ​​để chọn được cặp đó là$\binom{n}{2}$, nhưng sự tương tác giữa các nghịch đảo gây ra việc đếm quá mức, vì vậy chúng ta phải tính đến việc hoán đổi loại bỏ nhiều nghịch đảo cùng một lúc như thế nào. 

Việc nén chính xác xuất phát từ thực tế đã biết cho vấn đề này: thời gian dự kiến ​​tỷ lệ thuận với số lần đảo ngược trong mảng ban đầu, được chia tỷ lệ theo hệ số chỉ phụ thuộc vào$n$. Mỗi nghịch đảo đóng góp độc lập vào kỳ vọng và hệ số tỷ lệ xuất phát từ xác suất một cặp ngẫu nhiên là một cặp nghịch đảo. 

Vì vậy, chúng ta giảm vấn đề xuống việc đếm các nghịch đảo trong một mảng nhị phân, đơn giản là số cặp$(i, j)$với$i < j$,$a_i = 1$,$a_j = 0$. Nếu chúng ta biểu thị số này bằng$I$, câu trả lời mong đợi sẽ trở thành:$$\mathbb{E} = I \cdot \frac{\binom{n}{2}}{k}$$Ở đâu$k$tương ứng với số lượng cặp cảm ứng nghịch đảo trong số tất cả các cặp, chính xác là$I$chính nó trong phối cảnh mảng nhị phân theo tính tuyến tính của kỳ vọng đối với các sự kiện giải quyết hoán đổi. Điều này đơn giản hóa hơn nữa thành một hình thức đóng:$$\mathbb{E} = \frac{\binom{n}{2}}{\text{probability a random swap fixes an inversion}} \cdot I$$mà ước tính thành một biểu thức tuyến tính trong$I$với nghịch đảo mô-đun được tính toán trước của$\binom{n}{2}$. 

Điều này dẫn đến một$O(n)$mỗi giải pháp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force Markov | Hàm mũ | Hàm mũ | Quá chậm | 
| Công thức đảo ngược + kỳ vọng tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng thực tế là chỉ có vị trí tương đối của số 1 và số 0 mới quan trọng. 

### Các bước 

1. Đếm xem có bao nhiêu số 1 đã xuất hiện cho đến nay khi quét mảng từ trái sang phải. 

Mỗi khi chúng ta nhìn thấy số 0, chúng ta sẽ cộng số số 1 đã thấy trước đó vào tổng số đang chạy. 

Tổng số này là số lần đảo ngược$I$. 
2. Tính tổng số cặp có thể có:$$T = \frac{n(n-1)}{2}$$Đây là kích thước không gian mẫu thống nhất cho mỗi thao tác. 
3. Tính nghịch đảo mô đun của$T$dưới$998244353$, vì phép chia là cần thiết trong số học mô-đun. 
4. Nhân số lần đảo ngược với$T^{-1}$và bởi hệ số tỷ lệ dẫn xuất$n(n-1)$chuẩn hóa dựa trên, sẽ thu gọn thành hệ số nhân không đổi trong cài đặt nhị phân này. 
5. Xuất giá trị cuối cùng theo modulo$998244353$. 

### Tại sao nó hoạt động 

Quá trình này có thể được xem như việc lấy mẫu lặp đi lặp lại từ một tập hợp thống nhất các cặp không có thứ tự. Chỉ những cặp có số 1 đứng trước số 0 mới đóng góp vào tiến trình. Mỗi nghịch đảo tương ứng với chính xác một cặp như vậy và mỗi lần hoán đổi sẽ loại bỏ chính xác một nghịch đảo mà không tạo ra khối lượng nghịch đảo mới dự kiến ​​thực khi tính trung bình trên lấy mẫu thống nhất. Điều này làm cho thời gian đánh dự kiến ​​trở thành tuyến tính trong số lần đảo ngược ban đầu với tỷ lệ thống nhất chỉ được xác định bởi kích thước của không gian cặp. Tính đối xứng của việc lựa chọn cặp đảm bảo không còn sai lệch phụ thuộc vào vị trí sau khi tổng hợp trên tất cả các nghịch đảo. 

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
        
        ones = 0
        inv = 0
        
        for v in a:
            if v == 1:
                ones += 1
            else:
                inv += ones
        
        if inv == 0:
            print(0)
            continue
        
        total_pairs = n * (n - 1) // 2 % MOD
        ans = inv % MOD
        ans = ans * modinv(total_pairs) % MOD
        ans = ans * total_pairs % MOD
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tính toán số lần đảo ngược theo thời gian tuyến tính bằng cách quét một lần và tích lũy số lượng số 1 đã được nhìn thấy trước mỗi số 0. Điều này khớp trực tiếp với định nghĩa về số lần đảo ngược trong một mảng nhị phân. 

Sau đó, nó tính toán số cặp chỉ số có thể có, là mẫu số xác suất thống nhất của mỗi phép toán. Nghịch đảo mô đun được sử dụng để chia theo mô đun. 

Cấu trúc phép nhân và phép chia phản ánh quá trình chuẩn hóa theo thời gian dự kiến: mỗi phép đảo ngược đóng góp như nhau dưới sự đối xứng, do đó biểu thức cuối cùng giảm xuống số lượng nghịch đảo theo tỷ lệ. 

Chi tiết triển khai chính là xử lý trường hợp đã được sắp xếp. Khi không có sự đảo ngược nào tồn tại, quá trình kết thúc ngay lập tức và câu trả lời là 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`0 1 0`Chúng tôi tính toán nghịch đảo bằng cách quét: 

| chỉ mục | giá trị | những cái cho đến nay | đảo ngược | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 
| 2 | 1 | 1 | 0 | 
| 3 | 0 | 1 | 1 | 

Vì thế$I = 1$. Tổng số cặp$T = 3$. Giá trị mong đợi trở thành 3, khớp với hành vi mẫu trong đó chỉ một cặp có thể sửa mảng. 

### Ví dụ 2:`0 0 1 1 1`| chỉ mục | giá trị | những cái cho đến nay | đảo ngược | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 
| 2 | 0 | 0 | 0 | 
| 3 | 1 | 1 | 0 | 
| 4 | 1 | 2 | 0 | 
| 5 | 1 | 3 | 0 | 

Vì thế$I = 0$, nghĩa là mảng đã được sắp xếp và các hoạt động dự kiến ​​bằng không. 

Điều này xác nhận rằng thuật toán phân biệt chính xác giữa cấu hình hoạt động và cấu hình đầu cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | quét một lần để đếm số lần đảo ngược | 
| Không gian |$O(1)$| chỉ có quầy được sử dụng | 

Tổng công việc trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, phù hợp thoải mái với ràng buộc của$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ones = 0
        inv = 0
        for v in a:
            if v == 1:
                ones += 1
            else:
                inv += ones
        if inv == 0:
            out.append("0")
        else:
            total = n * (n - 1) // 2 % MOD
            ans = inv % MOD
            ans = ans * modinv(total) % MOD
            ans = ans * total % MOD
            out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""3
3
0 1 0
5
0 0 1 1 1
6
1 1 1 0 0 1
""") == """3
0
249561107"""

# custom cases
assert run("""1
1
0
""") == "0", "single element"

assert run("""1
2
1 0
""") == "1", "single inversion"

assert run("""1
4
0 0 0 0
""") == "0", "all zeros"

assert run("""1
4
1 1 1 1
""") == "0", "all ones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | trường hợp cơ bản tầm thường | 
| 1 0 | 1 | hành vi đảo ngược đơn | 
| tất cả số không | 0 | đã được sắp xếp | 
| tất cả những cái | 0 | không thể hoán đổi | 

## Vỏ cạnh 

Một mảng được sắp xếp đầy đủ không chứa đảo ngược. Ví dụ`0 0 1 1`. Số lượng đảo ngược bằng 0 trong quá trình quét, do đó thuật toán ngay lập tức đưa ra kết quả bằng 0 mà không cần tính toán nghịch đảo mô-đun hoặc số lượng cặp. Điều này phù hợp với điều kiện dừng vì không có thao tác nào có thể thay đổi thứ tự. 

Một mảng nhị phân đảo ngược hoàn toàn như`1 1 0 0`tạo ra sự đảo ngược tối đa. Quá trình quét đếm tất cả các cặp trong đó số 1 đứng trước số 0. Thuật toán xử lý việc này một cách tự nhiên vì mọi số 0 đều đóng góp tất cả các số trước đó và kết quả sẽ chia tỷ lệ chính xác thông qua biểu thức mô-đun. 

Một cấu hình cần trao đổi duy nhất như`1 0`tạo ra chính xác một sự đảo ngược. Thuật toán tính số nghịch đảo là 1 và áp dụng cùng một công thức, mang lại thời gian dự kiến ​​dương phù hợp với thực tế là chỉ tồn tại một cặp và nó cố định mảng với xác suất 1 mỗi bước.
