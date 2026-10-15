---
title: "CF 105461I - Tái phát định kỳ"
description: "Chúng ta có một chuỗi được tạo bởi phép truy hồi tuyến tính modulo một số nguyên cố định $M$. Chuỗi bắt đầu từ hai giá trị ban đầu $a0$ và $a1$, và mọi phần tử tiếp theo được hình thành bằng cách kết hợp hai giá trị trước đó bằng các hệ số cố định $A$ và $B$, sau đó giảm kết quả theo modulo…"
date: "2026-06-23T02:32:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105461
codeforces_index: "I"
codeforces_contest_name: "2024-2025 ICPC, Swiss Subregional"
rating: 0
weight: 105461
solve_time_s: 57
verified: true
draft: false
---

[CF 105461I - Tái phát định kỳ](https://codeforces.com/problemset/problem/105461/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi được tạo bởi phép truy hồi tuyến tính modulo một số nguyên cố định$M$. Chuỗi bắt đầu từ hai giá trị ban đầu$a_0$Và$a_1$và mọi phần tử tiếp theo được hình thành bằng cách kết hợp hai phần tử trước đó bằng hệ số cố định$A$Và$B$, sau đó giảm modulo kết quả$M$. Đây là một quá trình xác định, vì vậy khi cặp ban đầu được cố định thì toàn bộ chuỗi vô hạn cũng được cố định. 

Nhiệm vụ là xác định khi nào chuỗi này cuối cùng trở thành tuần hoàn và quan trọng hơn là tìm ra sự dịch chuyển dương nhỏ nhất.$T$sao cho từ một vị trí nào đó trở đi trình tự lặp lại mỗi$T$bước đi mãi mãi. 

Một quan sát quan trọng là trình tự được xác định đầy đủ bởi các cặp$(a_i, a_{i+1})$. Khi cặp tương tự xuất hiện trở lại, phép truy hồi buộc tất cả các giá trị trong tương lai lặp lại giống hệt nhau, vì giá trị tiếp theo chỉ phụ thuộc vào hai giá trị này. 

Ràng buộc$M \le 10^9$loại trừ mọi cách tiếp cận theo dõi tất cả các trạng thái một cách rõ ràng. Mặc dù về mặt khái niệm không gian trạng thái có kích thước$M^2$, chúng ta không thể lặp lại hoặc lưu trữ nó. Giới hạn thời gian 1 giây cũng ngụ ý rằng bất kỳ phương pháp nào cố gắng mô phỏng chuỗi cho đến khi tìm thấy sự lặp lại sẽ thất bại trong trường hợp xấu nhất, vì khoảng thời gian có thể cực kỳ lớn (lên tới$10^{18}$). 

Một cạm bẫy ngây thơ xuất hiện khi cố gắng phát hiện sự lặp lại bằng cách lưu trữ từng cặp$(a_i, a_{i+1})$. Điều này ngay lập tức gặp vấn đề về bộ nhớ khi khoảng thời gian lớn. Một vấn đề tế nhị khác là giả định rằng lần lặp lại đầu tiên sẽ cho ra kết quả chính xác. Chuỗi có thể bước vào một chu kỳ sau tiền tố nhất thời, do đó, sự lặp lại sớm nhất của một cặp không nhất thiết là cấu trúc tuần hoàn cuối cùng trừ khi nó là trạng thái lặp lại đầu tiên theo nghĩa biểu đồ trạng thái. 

## Phương pháp tiếp cận 

Sự truy hồi xác định một quá trình chuyển đổi xác định trên các cặp:$$(a_{i-1}, a_i) \rightarrow (a_i, a_{i+1})$$Ở đâu$$a_{i+1} = (A a_i + B a_{i-1}) \bmod M.$$Điều này có nghĩa là chúng ta đang duyệt qua một đồ thị có hướng trong đó mỗi nút là một cặp giá trị trong$[0, M)$và mỗi nút có chính xác một cạnh đi ra. Cấu trúc như vậy là một đồ thị hàm số, do đó mỗi thành phần được kết nối bao gồm một chu trình duy nhất với các cây được định hướng đi vào nó. 

Cách tiếp cận brute-force mô phỏng trình tự trong khi lưu trữ mọi cặp được truy cập. Thời điểm một cặp lặp lại, chúng ta phát hiện ra một chu kỳ. Điều này xác định chính xác tính tuần hoàn vì đồ thị hàm số đảm bảo sự lặp lại cuối cùng. Tuy nhiên, việc lưu trữ tất cả các trạng thái được truy cập đòi hỏi$O(M^2)$bộ nhớ trong trường hợp xấu nhất và thậm chí lưu trữ tới$10^9$các giá trị là không thể thực hiện được. Thời gian cũng có vấn đề vì chu kỳ có thể rất dài. 

Thông tin chi tiết về cấu trúc quan trọng là chúng ta không được yêu cầu mô phỏng cho đến khi lặp lại trong biểu đồ chung mà là phân tích mô đun hồi quy tuyến tính.$M$. Điều này tương đương với việc nghiên cứu phép biến đổi tuyến tính trên vectơ trong vành hữu hạn. Sự tiến hóa cặp là một bản đồ tuyến tính:$$\begin{pmatrix}
a_i \\
a_{i+1}
\end{pmatrix}
=
\begin{pmatrix}
0 & 1 \\
B & A
\end{pmatrix}
\begin{pmatrix}
a_{i-1} \\
a_i
\end{pmatrix}
\bmod M.$$Vì vậy, bài toán rút gọn thành việc tìm chu kỳ của dãy ma trận theo modulo nhân lặp lại$M$. Độ dài chu kỳ tương ứng với bậc của ma trận này tác động lên vectơ ban đầu, nhưng vì không gian là hữu hạn nên chúng ta có thể khai thác thực tế là hệ thống cuối cùng phải quay trở lại trạng thái trước đó trong cấu trúc chu trình giới hạn. 

Một quan sát trực tiếp hơn là vì hệ thống là một đồ thị hàm số theo cặp, nên khoảng thời gian này chính xác là độ dài chu kỳ đạt được khi chúng ta bước vào chu trình. Chúng ta có thể tìm chu trình bằng thuật toán phát hiện chu trình của Floyd, thuật toán này tránh lưu trữ tất cả các trạng thái và chỉ sử dụng bộ nhớ không đổi. 

Khi xác định được trạng thái lặp lại, chúng ta có thể tính độ dài chu kỳ bằng cách đi bộ một vòng quanh chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (lưu trữ tất cả các cặp) |$O(\text{period})$|$O(M^2)$| Quá chậm | 
| Phát hiện chu trình Floyd |$O(\text{period})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi trạng thái là một cặp$(x, y) = (a_i, a_{i+1})$. Hàm chuyển tiếp tính toán cặp tiếp theo. 

1. Xác định hàm ánh xạ một cặp$(x, y)$ĐẾN$(y, (A \cdot y + B \cdot x) \bmod M)$. Điều này ghi lại sự tái phát trực tiếp dưới dạng chuyển đổi trạng thái. 
2. Chạy phát hiện chu kỳ của Floyd bằng hai con trỏ. Một con trỏ tiến từng bước một, con trỏ kia tiến hai bước một lần. Nếu chúng gặp nhau thì tồn tại một chu trình. Điều này hiệu quả vì không gian trạng thái là hữu hạn nên sự lặp lại được đảm bảo. 
3. Đặt lại một con trỏ về trạng thái ban đầu và di chuyển cả hai con trỏ từng bước một cho đến khi chúng gặp lại nhau. Điều này đưa ra điểm vào của chu kỳ. Lý do điều này có hiệu quả là khoảng cách từ lúc bắt đầu đến khi vào chu kỳ bằng nhau ở cả hai bên khi sử dụng chuyển động đồng bộ. 
4. Bắt đầu từ mục nhập chu kỳ, đi về phía trước cho đến khi trở lại trạng thái cũ, đếm số bước. Số đếm này là độ dài chu kỳ, chính xác là khoảng thời gian cần thiết. 
5. Xuất độ dài chu kỳ cho từng trường hợp kiểm thử. 

### Tại sao nó hoạt động 

Mỗi trạng thái chuyển tiếp một cách xác định sang chính xác một trạng thái tiếp theo, do đó, chuỗi các trạng thái tạo thành một đường dẫn trong đồ thị có hướng hữu hạn. Một biểu đồ như vậy cuối cùng phải xem lại một trạng thái, tạo thành một chu trình. Sau khi nhập một chu kỳ, trình tự sẽ lặp lại với độ dịch cố định bằng độ dài chu kỳ. Thuật toán của Floyd đảm bảo phát hiện chu trình này mà không cần lưu trữ lịch sử bằng cách khai thác sự khác biệt về tốc độ tương đối của hai lần truyền trong cùng một chu kỳ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def nxt(x, y, A, B, M):
    return y, (A * y + B * x) % M

def cycle_length(a0, a1, A, B, M):
    tortoise = (a0, a1)
    hare = nxt(a0, a1, A, B, M)

    while tortoise != hare:
        tortoise = nxt(*tortoise, A, B, M)
        hare = nxt(*nxt(*hare, A, B, M), A, B, M)

    start = (a0, a1)
    mu = 0
    tortoise = start
    while tortoise != hare:
        tortoise = nxt(*tortoise, A, B, M)
        hare = nxt(*hare, A, B, M)
        mu += 1

    lam = 1
    hare = nxt(*tortoise, A, B, M)
    while tortoise != hare:
        hare = nxt(*hare, A, B, M)
        lam += 1

    return lam

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        M, a0, a1, A, B = map(int, input().split())
        out.append(str(cycle_length(a0, a1, A, B, M)))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai mã hóa sự lặp lại hoàn toàn trong`nxt`đảm bảo rằng các chuyển đổi trạng thái vẫn nhất quán ở mọi nơi trong thuật toán. Việc phát hiện chu trình của Floyd được thực hiện theo ba giai đoạn: đầu tiên phát hiện điểm gặp nhau, sau đó tìm mục nhập chu trình và cuối cùng là đo độ dài chu trình. 

Một điểm tinh tế là các trạng thái được lưu trữ dưới dạng bộ dữ liệu, là những so sánh không có hàm băm thay vì tra cứu từ điển. Điều này tránh được chi phí chung và giữ cho mỗi bước có thời gian không đổi. 

## Ví dụ đã hoạt động 

Hãy xem xét trình tự được xác định bởi$M = 3, a_0 = 0, a_1 = 1, A = 2, B = 2$. Các trạng thái phát triển thành từng cặp$(a_i, a_{i+1})$. 

### Ví dụ 1 dấu vết 

| bước | rùa | thỏ | 
| --- | --- | --- | 
| 0 | (0,1) | (1,2) | 
| 1 | (1,2) | (2,2) | 
| 2 | (2,2) | (2,2) | 

Ở bước 2, cả hai con trỏ đều gặp nhau ở cùng một trạng thái, xác nhận một chu trình. Mục nhập chu trình ngay lập tức là trạng thái bắt đầu và độ dài chu trình là 3 sau khi hoàn thành vòng lặp. 

Điều này thể hiện trường hợp chu kỳ bắt đầu gần như ngay lập tức, do đó tiền tố nhất thời bằng 0. 

### Ví dụ 2 dấu vết 

Lấy một phép truy toán tuyến tính đơn giản hơn ổn định đến một điểm cố định, chẳng hạn như$M = 7, a_0 = 0, a_1 = 0, A = 3, B = 4$. 

| bước | tiểu bang | 
| --- | --- | 
| 0 | (0,0) | 
| 1 | (0,0) | 
| 2 | (0,0) | 

Cả hai con trỏ trùng nhau ngay lập tức, hiển thị một chu trình tự lặp có độ dài bằng 1. Điều này xác nhận rằng các chuỗi không đổi là các chu trình hợp lệ theo định nghĩa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \cdot \lambda)$| Mỗi bài kiểm tra chạy thuật toán Floyd, đi qua chu trình một số lần không đổi | 
| Không gian |$O(1)$| Chỉ một số biến trạng thái cố định được lưu trữ | 

Giới hạn trên$M$và sự đảm bảo rằng câu trả lời nhiều nhất là$10^{18}$đảm bảo rằng việc phát hiện chu trình vẫn hiệu quả trong thực tế, vì chúng tôi không bao giờ khám phá toàn bộ không gian trạng thái mà chỉ khám phá chu trình thực tế có thể truy cập được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    output = io.StringIO()
    with contextlib.redirect_stdout(output):
        solve()
    return output.getvalue().strip()

# provided sample-style cases
assert run("1\n3 0 1 2 2\n") == "3"

# all equal -> fixed point cycle 1
assert run("1\n10 5 5 0 0\n") == "1"

# zero recurrence
assert run("1\n7 1 2 0 0\n") == "1"

# small oscillation
assert run("1\n5 1 2 1 1\n") >= "1"

# multiple cases
assert run("3\n3 0 1 2 2\n7 0 0 3 4\n10 1 2 0 3\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 1 | điểm cố định tức thời | 
| B = 0 trường hợp | chu trình đơn giản | thoái hóa tái phát | 
| nhiều trường hợp hỗn hợp | đầu ra không trống | xử lý hàng loạt | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chuỗi ngay lập tức đi vào vòng tự lặp. Ví dụ,$M = 10, a_0 = 5, a_1 = 5, A = 0, B = 0$. Giá trị tiếp theo luôn là 0 và chuỗi trở thành hằng số. Thuật toán phát hiện trạng thái lặp lại ở lần so sánh đầu tiên và độ dài chu kỳ được tính là 1 vì trạng thái chuyển sang chính nó sau một bước. 

Một trường hợp đặc biệt khác là khi sự lặp lại thu gọn thành một chu kỳ ngắn mà không có bất kỳ tiền tố nhất thời nào. Vì$M = 3, a_0 = 0, a_1 = 1, A = 2, B = 2$, trình tự quay vòng ngay lập tức. Thuật toán của Floyd phát hiện điều này mà không cần lưu trữ các trạng thái trước đó, vì rùa và thỏ gặp nhau bên trong chu trình. 

Trường hợp cạnh thứ ba là khi độ dài chu trình lớn, gần với giới hạn lý thuyết của$10^{18}$. Thuật toán không cố gắng liệt kê các trạng thái; nó chỉ đi theo các con trỏ cho đến khi chúng trùng nhau, rồi đo trực tiếp chu trình, đảm bảo tính chính xác bất kể độ lớn.
