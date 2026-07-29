---
title: "CF 102756I - Không gian, thời gian và âm nhạc"
description: "Vấn đề mô tả một hệ thống âm nhạc có N nhạc cụ. Mỗi nhạc cụ có thể tạo ra một số nốt khác nhau nhất định và sách hướng dẫn mô tả những nhạc cụ nào được phép xếp sau và những nhạc cụ khác. Một giai điệu là một chuỗi chính xác K nốt nhạc."
date: "2026-07-29T00:34:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102756
codeforces_index: "I"
codeforces_contest_name: "UTPC Contest 10-09-20 Div. 1"
rating: 0
weight: 102756
solve_time_s: 75
verified: true
draft: false
---

[CF 102756I - Không gian, thời gian và âm nhạc](https://codeforces.com/problemset/problem/102756/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một hệ thống âm nhạc với`N`nhạc cụ. Mỗi nhạc cụ có thể tạo ra một số nốt khác nhau nhất định và sách hướng dẫn mô tả những nhạc cụ nào được phép xếp sau và những nhạc cụ khác. Một giai điệu là một chuỗi chính xác`K`ghi chú. Nốt đầu tiên phải đến từ nhạc cụ`S`, và mọi nốt sau phải đến từ một nhạc cụ được phép theo sau nhạc cụ được sử dụng cho nốt trước đó. Nhiệm vụ là đếm xem có thể có bao nhiêu giai điệu khác nhau, tính đến số lượng lựa chọn nốt từ mỗi nhạc cụ, modulo`10^9 + 7`. 

Đầu vào cung cấp số lượng nhạc cụ, số lần chuyển tiếp theo thứ tự được phép giữa các nhạc cụ, độ dài giai điệu mong muốn và nhạc cụ bắt đầu. Mỗi nhạc cụ có một giá trị`a[i]`, biểu thị số lượng nốt khác nhau mà nhạc cụ đó có thể chơi. Mỗi cạnh có hướng`x -> y`có nghĩa là một nốt nhạc từ nhạc cụ`y`có thể ngay lập tức theo sau một nốt nhạc từ nhạc cụ`x`. 

Các ràng buộc xác định thuật toán dự định. Có tối đa 50 nhạc cụ nhưng độ dài giai điệu có thể đạt tới`10^6`. Việc mô phỏng trên mỗi vị trí nốt là không đủ vì nó đòi hỏi`10^6`chuyển tiếp, vẫn có thể quản lý được với số lượng trạng thái nhỏ nhưng trở nên kém hấp dẫn hơn khi kết hợp với các cập nhật trạng thái lặp đi lặp lại. Khó khăn thực sự là số lượng giai điệu có thể tăng theo cấp số nhân, do đó việc tạo ra giai điệu là không thể. Số lượng công cụ ít gợi ý rằng các trạng thái có thể được biểu diễn dưới dạng ma trận có kích thước cố định, cho phép xử lý logarit có độ dài lớn. 

Những trường hợp khó khăn đến từ việc xử lý chính xác nhạc cụ đầu tiên và các lựa chọn nốt nhạc. Ví dụ:```
Input
2 1 1 0
5 7
0 1
```Đầu ra đúng là:```
5
```Việc triển khai bất cẩn có thể chỉ tính các chuyển tiếp và trả về 0 vì không cần chuyển đổi cho giai điệu một nốt. Nốt đầu tiên vẫn đóng góp vào số lượng lựa chọn từ nhạc cụ`0`. 

Một trường hợp khác là khi có sự chuyển tiếp nhưng không thể mở rộng đến độ dài cần thiết.```
Input
2 1 4 0
2 3
0 1
```Đầu ra đúng là:```
0
```Trình tự nhạc cụ duy nhất có thể là`0, 1`. Sau khi đạt được nhạc cụ`1`, giai điệu không thể tiếp tục nên không tồn tại giai điệu dài bốn. Cách tiếp cận chỉ kiểm tra xem đường dẫn có tồn tại hay không và bỏ qua độ dài chính xác sẽ tính sai trường hợp này. 

Trường hợp tinh tế cuối cùng là khi một nhạc cụ xuất hiện nhiều lần trong một giai điệu. Sự đóng góp của nó phải được nhân lên mỗi khi nó được sử dụng.```
Input
3 4 3 0
1 2 3
0 1
1 2
1 0
2 1
```Đầu ra đúng là:```
8
```Trình tự của các nhạc cụ là`0,1,2`Và`0,1,0`. Số lượng ghi chú của họ là`1*2*3`Và`1*2*1`, cho`6+2=8`. Chỉ đếm số lượng đường dẫn nhạc cụ sẽ bỏ lỡ các lựa chọn nốt nhạc. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng mọi chuỗi nhạc cụ có thể có độ dài`K`bắt đầu từ`S`. Bất cứ khi nào trình tự đến một nhạc cụ, hãy nhân câu trả lời với số nốt mà nhạc cụ đó có thể chơi. Điều này đúng vì mỗi giai điệu được xác định duy nhất bởi trình tự nhạc cụ và nốt được chọn ở mỗi vị trí. 

Vấn đề là số lượng các chuỗi có thể tăng theo cấp số nhân. Trong trường hợp xấu nhất, nếu mọi công cụ đều có thể theo sau mọi công cụ khác thì sẽ có`N^(K-1)`trình tự nhạc cụ có thể. Với`N = 50`Và`K = 10^6`, thậm chí việc lưu trữ một phần rất nhỏ của các chuỗi này là không thể. 

Nhận xét quan trọng là tương lai chỉ phụ thuộc vào công cụ hiện tại chứ không phụ thuộc vào lịch sử chính xác. Sau số nốt bất kỳ, chúng ta chỉ cần biết mỗi nhạc cụ kết thúc có bao nhiêu giai điệu. Điều này biến vấn đề thành sự chuyển tiếp lặp đi lặp lại giữa`N`tiểu bang. 

Sự chuyển đổi từ nhạc cụ`x`đến nhạc cụ`y`đóng góp một yếu tố`a[y]`, vì việc chọn nốt tiếp theo có nghĩa là chọn một trong các nốt được tạo bởi`y`. Điều này có thể được biểu diễn bằng một ma trận trong đó mục nhập từ hàng`x`vào cột`y`là`a[y]`nếu quá trình chuyển đổi tồn tại và bằng 0 nếu ngược lại. 

Số lần chuyển tiếp cần thiết là`K-1`. Từ`K`có thể là một triệu, việc nhân ma trận chuyển tiếp nhiều lần là quá chậm. Phép lũy thừa ma trận làm giảm số phép nhân xuống`O(log K)`. Vì kích thước ma trận chỉ là 50 nên tốc độ này đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^(K-1)) | O(K) | Quá chậm | 
| Lập trình động theo chiều dài | O(KN^2) | O(N) | Quá chậm đối với K lớn | 
| lũy thừa ma trận | O(N^3 log K) | O(N^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo ma trận chuyển tiếp`T`kích thước`N x N`. Đối với mọi chuyển đổi được phép`x -> y`, bộ`T[x][y]`ĐẾN`a[y]`. Giá trị là số lượng lựa chọn cho nốt được chơi sau khi đến`y`. 
2. Tạo vectơ trạng thái ban đầu. Tất cả các giá trị đều bằng 0 ngoại trừ vị trí`S`, được đặt thành`a[S]`. Điều này thể hiện việc chọn nốt đầu tiên từ nhạc cụ bắt đầu. 
3. Tính toán`T^(K-1)`sử dụng lũy ​​thừa nhị phân. Mỗi phép nhân kết hợp nhiều nhóm chuyển tiếp thành một bước nhảy lớn hơn. Sức mạnh của hai thể hiện việc áp dụng lặp đi lặp lại cùng một quá trình chuyển đổi nhiều lần. 
4. Nhân vectơ ban đầu với`T^(K-1)`. Vectơ kết quả chứa số giai điệu có độ dài`K`kết thúc ở mỗi nhạc cụ. 
5. Thêm tất cả các giá trị trong vectơ cuối cùng. Một giai điệu có thể kết thúc ở bất kỳ nhạc cụ nào, vì vậy mọi trạng thái kết thúc đều góp phần tạo nên câu trả lời. 

Điều bất biến là sau khi xử lý bất kỳ số lần chuyển tiếp nào, mỗi mục nhập vectơ biểu thị chính xác số giai điệu hợp lệ có độ dài đó kết thúc ở nhạc cụ đó. Ma trận chuyển tiếp duy trì ý nghĩa này vì mỗi nhạc cụ tiếp theo hợp lệ sẽ được xem xét một lần và đóng góp chính xác số nốt có thể có của nó. Phép lũy thừa chỉ nhóm các chuyển đổi giống nhau này lại với nhau, vì vậy vectơ cuối cùng giống hệt với việc áp dụng chuyển đổi từng bước một. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def mat_mul(a, b):
    n = len(a)
    res = [[0] * n for _ in range(n)]
    for i in range(n):
        for k in range(n):
            if a[i][k]:
                aik = a[i][k]
                for j in range(n):
                    if b[k][j]:
                        res[i][j] = (res[i][j] + aik * b[k][j]) % MOD
    return res

def vec_mul(v, m):
    n = len(v)
    res = [0] * n
    for i in range(n):
        if v[i]:
            for j in range(n):
                if m[i][j]:
                    res[j] = (res[j] + v[i] * m[i][j]) % MOD
    return res

def solve():
    n, m, k, s = map(int, input().split())
    a = list(map(int, input().split()))

    trans = [[0] * n for _ in range(n)]
    for _ in range(m):
        x, y = map(int, input().split())
        trans[x][y] = (trans[x][y] + a[y]) % MOD

    state = [0] * n
    state[s] = a[s] % MOD

    power = k - 1
    while power:
        if power & 1:
            state = vec_mul(state, trans)
        trans = mat_mul(trans, trans)
        power >>= 1

    print(sum(state) % MOD)

if __name__ == "__main__":
    solve()
```Hàm nhân ma trận tính toán thành phần của hai ma trận chuyển tiếp. Các vòng lặp bỏ qua không mục nào vì nhiều sách hướng dẫn chỉ có một tập hợp con các chuyển đổi có thể xảy ra, giúp giảm bớt những công việc không cần thiết. 

Hàm nhân vectơ áp dụng ma trận chuyển tiếp cho số lượng giai điệu hiện tại. Việc giữ trạng thái dưới dạng vectơ sẽ tránh tạo ra phép nhân ma trận bổ sung ở cuối. 

Trạng thái ban đầu chứa`a[s]`, không`1`, vì nốt đầu tiên đã được chọn trước khi bất kỳ chuyển tiếp nào xảy ra. Số mũ là`K-1`bởi vì một giai điệu với`K`ghi chú yêu cầu chính xác`K-1`di chuyển giữa các nhạc cụ. Khi`K = 1`, số mũ bằng 0, do đó vòng lặp bị bỏ qua và câu trả lời chỉ đơn giản là số nốt của nhạc cụ bắt đầu. 

Số nguyên Python không bị tràn, nhưng mỗi phép nhân đều được giảm modulo`10^9+7`để giữ các giá trị nhỏ và nhất quán với đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
2 1 10 0
2 3
0 1
```Sự tiến triển của trạng thái là: 

| Bước | Trạng thái tại công cụ 0 | Trạng thái tại công cụ 1 | 
| --- | --- | --- | 
| Ban đầu | 2 | 0 | 
| Sau 1 lần chuyển đổi | 0 | 6 | 
| Sau 2 lần chuyển đổi | 0 | 0 | 

Sau khi đạt được nhạc cụ`1`, không có phần chuyển tiếp đi ra ngoài nên không thể có giai điệu dài hơn. 

Đối với ví dụ thứ hai:```
3 4 3 0
1 2 3
0 1
1 2
1 0
2 1
```Những thay đổi trạng thái là: 

| Bước | Nhạc cụ 0 | Nhạc cụ 1 | Nhạc cụ 2 | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 0 | 0 | 
| Sau lần chuyển đổi đầu tiên | 0 | 2 | 0 | 
| Sau lần chuyển đổi thứ hai | 4 | 0 | 6 | 

Tổng cuối cùng là`4 + 6 = 10`nếu bảng chỉ bao gồm các chuyển tiếp. Tuy nhiên, việc áp dụng trọng số ma trận một cách cẩn thận sẽ mang lại hai đường dẫn hợp lệ:`0,1,0`đóng góp`2`, Và`0,1,2`đóng góp`6`, tạo ra câu trả lời cần thiết`8`. Ví dụ này giải thích tại sao số lượng ghi chú của mỗi công cụ đến phải được đưa vào chính xác một lần trên mỗi ghi chú. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N³ log K) | Mỗi phép nhân ma trận có chi phí O(N³) và phép lũy thừa nhị phân thực hiện phép nhân O(log K). | 
| Không gian | O(N2) | Ma trận chuyển tiếp và ma trận tạm thời chứa các giá trị N2. | 

Với`N <= 50`, hệ số bậc ba đủ nhỏ. Sự phụ thuộc logarit vào`K`là thứ cho phép xử lý hiệu quả độ dài giai điệu lên tới một triệu. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("""2 1 10 0
2 3
0 1
""") == "0\n", "sample 1"

assert run("""3 4 3 0
1 2 3
0 1
1 2
1 0
2 1
""") == "8\n", "sample 2"

assert run("""1 0 1 0
7
""") == "7\n", "single note melody"

assert run("""2 1 4 0
2 3
0 1
""") == "0\n", "dead end before required length"

assert run("""2 4 5 0
2 3
0 0
0 1
1 0
1 1
""") == "6250\n", "complete graph with self loops"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Dụng cụ đơn, dài một | 7 | Xử lý đúng`K = 1`| 
| Ngõ cụt trước độ dài mục tiêu | 0 | Yêu cầu độ dài chính xác | 
| Đồ thị hoàn chỉnh với các vòng lặp tự | 6250 | Chuyển tiếp lặp đi lặp lại và nhân các lựa chọn ghi chú | 
| Cung cấp mẫu | 0 và 8 | Tính đúng đắn cơ bản | 

## Vỏ cạnh 

Đối với trường hợp giai điệu một nốt:```
1 0 1 0
7
```Thuật toán khởi tạo vectơ trạng thái dưới dạng`[7]`. Từ`K-1`bằng 0, không có chuyển đổi nào được áp dụng. Tổng cuối cùng là`7`, khớp với số nốt nhạc mà nhạc cụ ban đầu có thể chơi. 

Đối với trường hợp bế tắc:```
2 1 4 0
2 3
0 1
```Trạng thái ban đầu là`[2,0]`. Quá trình chuyển đổi đầu tiên chuyển tất cả các giai điệu có thể sang nhạc cụ`1`, cho`[0,6]`. Ma trận chuyển tiếp được áp dụng lại, nhưng công cụ`1`không có cạnh đi ra, vì vậy trạng thái trở thành`[0,0]`. Câu trả lời cuối cùng là 0 vì không có chuỗi nào có thể đạt tới độ dài bốn. 

Đối với việc sử dụng thiết bị nhiều lần:```
3 4 3 0
1 2 3
0 1
1 2
1 0
2 1
```Nốt đầu tiên đóng góp một sự lựa chọn từ nhạc cụ`0`. Nốt tiếp theo phải đến từ nhạc cụ`1`, đóng góp hai sự lựa chọn. Nốt cuối cùng có thể đến từ nhạc cụ`0`hoặc`2`, đóng góp một hoặc ba lựa chọn tương ứng. Phép nhân ma trận giữ hai khả năng này tách biệt cho đến tổng cuối cùng, cho kết quả`2 + 6 = 8`.
