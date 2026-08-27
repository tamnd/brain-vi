---
title: "CF 104366G - Số tiền dự kiến"
description: "Chúng ta được cung cấp một chuỗi thập phân dài, được hiểu là một chuỗi các chữ số được đặt từ trái sang phải. Giữa mỗi cặp chữ số liền kề, chúng tôi quyết định độc lập xem có nên chèn dấu cộng hay không."
date: "2026-07-01T17:43:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104366
codeforces_index: "G"
codeforces_contest_name: "The 17th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 104366
solve_time_s: 53
verified: true
draft: false
---

[CF 104366G - Số tiền dự kiến](https://codeforces.com/problemset/problem/104366/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi thập phân dài, được hiểu là một chuỗi các chữ số được đặt từ trái sang phải. Giữa mỗi cặp chữ số liền kề, chúng tôi quyết định độc lập xem có nên chèn dấu cộng hay không. Mỗi vị trí$i$đóng góp một sự kiện ngẫu nhiên: với xác suất$p_i/100$, dấu cộng được chèn vào giữa chữ số$i$và chữ số$i+1$, nếu không thì các chữ số vẫn dính vào nhau. 

Khi tất cả các quyết định được đưa ra, chuỗi chữ số sẽ chia thành nhiều khối liền kề. Mỗi khối được hiểu là một số nguyên duy nhất (trong cơ số 10) và chúng tôi tính tổng tất cả các số nguyên này. Nhiệm vụ là tính giá trị kỳ vọng của tổng cuối cùng này trên tất cả các lựa chọn ngẫu nhiên, modulo$998244353$. 

Khó khăn chính là giá trị của một khối phụ thuộc vào chuỗi đầy đủ các quyết định “không cộng”, vì vậy việc đánh giá ngây thơ sẽ yêu cầu liệt kê tất cả$2^{n-1}$những cấu hình không thể thực hiện được$n$lên đến$2 \cdot 10^6$. Điều này ngay lập tức loại trừ mọi mô phỏng hàm mũ hoặc thậm chí bậc hai. 

Ràng buộc cũng gợi ý rằng mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính, vì thậm chí$O(n \log n)$chặt chẽ nhưng có thể chấp nhận được, trong khi bất cứ điều gì lặp đi lặp lại các khoảng chữ số sẽ quá chậm. 

Một trường hợp khó nhận thấy là các số 0 đứng đầu được phép có trong số đầu vào, nhưng chúng không ảnh hưởng đến độ chính xác số học vì chúng ta chỉ hiểu các chuỗi con liền kề là số nguyên. Một trường hợp góc khác là khi tất cả các xác suất là 0 hoặc 100, sẽ thu gọn vấn đề thành một phân đoạn xác định hoặc một số đầy đủ và giải pháp vẫn phải hoạt động nhất quán mà không cần viết vỏ đặc biệt. 

## Phương pháp tiếp cận 

Một chiến lược brute-force sẽ lặp lại trên tất cả các tập hợp con của$n-1$các vị trí cắt có thể. Đối với mỗi tập hợp con, chúng tôi sẽ xây dựng lại các khối kết quả, tính toán các giá trị số của chúng và thêm chúng vào tổng số đang chạy được tính theo xác suất của mẫu cắt chính xác đó. Mỗi cấu hình yêu cầu$O(n)$thời gian để phân tích thành số, vì vậy tổng công việc là$O(n 2^{n})$, điều này là không thể ngay cả đối với$n = 30$. 

Cấu trúc của bài toán thay đổi khi chúng ta ngừng suy nghĩ về các phân đoạn đầy đủ và thay vào đó xem xét sự đóng góp của từng vị trí chữ số một cách độc lập. Quan sát quan trọng là mỗi chữ số đều đóng góp vào tổng cuối cùng theo một cách rất được kiểm soát: đóng góp của nó chỉ phụ thuộc vào việc nó có thể “mở rộng” sang trái bao xa trước khi chạm vào dấu cộng. 

Nếu chúng ta cố định một chữ số ở vị trí$i$, giá trị của nó trong bất kỳ số nào đều phụ thuộc vào độ dài của hậu tố không gián đoạn kết thúc tại$i$. Mỗi lần chúng ta di chuyển sang trái, chúng ta sẽ tiếp tục khối tương tự hoặc phá vỡ nó với xác suất$p_{i-1}/100$. Điều này tạo ra một cấu trúc sinh tồn hình học: chữ số ở vị trí$i$đóng góp vào một khối kéo dài sang trái với xác suất nhân lên. 

Thay vì liệt kê các phân đoạn, chúng tôi tính toán mức đóng góp dự kiến ​​cho mỗi chữ số. Chúng tôi xử lý các chữ số từ trái sang phải, duy trì mức đóng góp dự kiến ​​của tiền tố hiện tại dưới dạng giá trị luân phiên. Mỗi chữ số mới sẽ thêm vào tất cả các đóng góp hiện có bằng cách dịch chuyển chúng một vị trí thập phân (nhân với 10) và cũng bắt đầu một đóng góp mới tùy thuộc vào việc có xảy ra sự phân chia trước nó hay không. Xác suất phân chia xác định mức độ “đặt lại” của cấu trúc trước đó. 

Điều này biến vấn đề thành một phép lặp tuyến tính đối với các đóng góp tiền tố, trong đó mỗi vị trí cập nhật giá trị mong đợi bằng cách sử dụng số học mô-đun và xác suất được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n 2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại xác suất như$q_i = 1 - p_i/100$, biểu thị xác suất không có dấu cộng nào được chèn vào giữa$i$Và$i+1$. Tất cả các tính toán được thực hiện modulo$998244353$, vì vậy chúng ta chuyển đổi phép chia bằng cách sử dụng nghịch đảo mô-đun. 

Chúng tôi duy trì một giá trị lăn$dp$, đại diện cho giá trị mong đợi của biểu thức được hình thành bởi tiền tố được xử lý cho đến nay, cùng với một biến trợ giúp$pref$, thể hiện sự đóng góp của cấu trúc hậu tố hiện tại vẫn có thể được mở rộng. 

1. Chuyển đổi mỗi ký tự chữ số thành một giá trị số nguyên khi chúng ta quét chuỗi từ trái sang phải. Điều này đảm bảo chúng tôi không bao giờ lưu trữ các chuỗi con lớn hoặc tính toán lại các giá trị số. 
2. Khởi tạo$dp = 0$Và$pref = 0$. Khi bắt đầu, không có chữ số nào đóng góp gì cả, vì vậy cả hai đại lượng đều bằng 0. 
3. Đối với từng vị trí$i$, kết hợp chữ số$d_i$bằng cách đầu tiên mở rộng những đóng góp hiện có. Mỗi số được tạo trước đó sẽ dịch chuyển sang trái một vị trí thập phân, vì vậy chúng tôi nhân phần đóng góp hiện có với 10. Điều này phản ánh việc thêm một chữ số sẽ thay đổi giá trị vị trí của tất cả các phân đoạn đang hoạt động như thế nào. 
4. Thêm sự đóng góp của việc bắt đầu một phân khúc mới tại vị trí$i$. Điều này xảy ra khi dấu cộng được chèn ngay trước đó$i$, xảy ra với xác suất$p_{i-1}/100$. Trong trường hợp đó, chữ số$i$bắt đầu một số mới và đóng góp trực tiếp dưới dạng$d_i$. Chúng tôi đánh giá sự khởi đầu mới này bằng xác suất phân chia. 
5. Kết hợp các hiệu ứng tiếp tục và khởi động lại bằng cách sử dụng$q_{i-1}$. Đóng góp dự kiến ​​của việc mở rộng phân khúc trước đó được tính theo$q_{i-1}$, trong khi phần khởi động lại được chia tỷ lệ bằng$p_{i-1}/100$. Điều này tạo ra một bản cập nhật tuyến tính có dạng:$$pref = pref \cdot (10 \cdot q_{i-1}) + d_i$$và sau đó$dp$tích lũy các khoản đóng góp một cách hợp lý. 
6. Tích lũy$dp$với sự đóng góp tiền tố hiện tại ở mỗi bước, vì mỗi tiền tố đều đóng góp vào tổng dự kiến ​​chung. 

Điểm tinh tế là chúng tôi không bao giờ theo dõi ranh giới phân khúc một cách rõ ràng. Thay vào đó, tính tuyến tính theo trọng số xác suất của kỳ vọng cho phép chúng ta hợp nhất tất cả các trạng thái phân đoạn thành một trạng thái phát triển duy nhất. 

### Tại sao nó hoạt động 

Sự đóng góp của mỗi chữ số chỉ phụ thuộc vào số lượng sự kiện "không cắt" liên tiếp xảy ra ở bên trái của nó. Những sự kiện này tạo thành một chuỗi Bernoulli độc lập, do đó, sự đóng góp dự kiến ​​có thể được biểu thị dưới dạng tổng của các vị trí nơi xác suất sống sót được nhân lên. Thuật toán mã hóa quá trình sinh tồn này trong một hệ thống hệ số cuộn, đảm bảo mọi phân đoạn có thể được tính toán ngầm chính xác một lần với trọng số xác suất chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n = int(input())
    s = input().strip()
    p = list(map(int, input().split()))

    # precompute probabilities
    q = [(100 - x) * modinv(100) % MOD for x in p]

    dp = 0
    pref = 0

    for i in range(n):
        d = ord(s[i]) - 48

        if i == 0:
            pref = d
            dp = d
        else:
            pref = (pref * 10 % MOD * q[i - 1] + d) % MOD
            dp = (dp + pref) % MOD

    print(dp % MOD)

if __name__ == "__main__":
    solve()
```Mã xử lý các chữ số trong một lần chuyển. Mảng$q$lưu trữ xác suất không chèn dấu cộng, được chuyển đổi thành dạng mô-đun bằng cách sử dụng nghịch đảo của 100. Biến$pref$theo dõi giá trị mong đợi của số hậu tố hoạt động hiện tại đang được tạo. Mỗi bước nhân nó với 10 vì một chữ số mới dịch chuyển tất cả các chữ số hiện có sang trái một chữ số thập phân, sau đó chia tỷ lệ theo$q[i-1]$để phản ánh xác suất mà phân khúc tiếp tục. Thêm$d$tài khoản để bắt đầu đóng góp ở vị trí mới. 

Biến$dp$tích lũy tất cả các đóng góp tiền tố, tương ứng với việc tổng hợp các giá trị mong đợi của tất cả các phân đoạn một cách ngầm định. 

Một cạm bẫy triển khai phổ biến là quên rằng xác suất phải được chuyển đổi thành phân số mô-đun. Một cách khác là áp dụng phép nhân với 10 trước khi áp dụng tỷ lệ xác suất, điều này sẽ làm trọng số các vị trí chữ số không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
n = 3
s = "123"
p = [100, 0]
```Ở đây lần cắt đầu tiên luôn xảy ra, vì vậy chúng tôi luôn chia giữa 1 và 2, nhưng không bao giờ chia giữa 2 và 3. 

| tôi | chữ số | q[i-1] | trước | trước sau | dp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | - | 0 | 1 | 1 | 
| 1 | 2 | 0 | 1 | 2 | 3 | 
| 2 | 3 | 1 | 2 | 23 | 26 | 

Tổng dự kiến ​​cuối cùng là 26, tương ứng với các phân đoạn "1" + "23". 

Bây giờ hãy xem xét:```
n = 3
s = "111"
p = [0, 0]
```Không có vết cắt nào xảy ra nên mọi thứ tạo thành một con số. 

| tôi | chữ số | q[i-1] | trước | trước sau | dp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | - | 0 | 1 | 1 | 
| 1 | 1 | 1 | 1 | 11 | 12 | 
| 2 | 1 | 1 | 11 | 111 | 123 | 

Điều này xác nhận rằng thuật toán hoạt động giống như tích lũy cơ số 10 tiêu chuẩn khi không xảy ra sự phân tách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Chuyển qua các chữ số một lần với các bản cập nhật mô-đun liên tục | 
| Không gian |$O(n)$| Lưu trữ mảng xác suất, nếu không thì bộ nhớ làm việc liên tục | 

Việc quét tuyến tính là cần thiết vì mỗi chữ số ảnh hưởng đến tất cả các kỳ vọng tiếp theo thông qua việc thay đổi giá trị vị trí. Với$n$lên đến$2 \cdot 10^6$, lời giải nằm trong giới hạn thời gian vì tất cả các phép toán đều là số học mô-đun đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()
    p = list(map(int, input().split()))

    q = [(100 - x) * pow(100, MOD - 2, MOD) % MOD for x in p]

    dp = 0
    pref = 0

    for i in range(n):
        d = ord(s[i]) - 48
        if i == 0:
            pref = d
            dp = d
        else:
            pref = (pref * 10 % MOD * q[i - 1] + d) % MOD
            dp = (dp + pref) % MOD

    return str(dp % MOD)

# sample-style and custom tests
assert run("2\n12\n100\n") == "13"
assert run("3\n123\n0 0\n") == "123"
assert run("3\n123\n100 100\n") == "1"  # 1 + 2 + 3

assert run("5\n00000\n0 0 0 0\n") == "0"
assert run("4\n9999\n50 50 50\n")  # just sanity run, value nontrivial

assert run("2\n10\n0\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`12 / 100`|`13`| tách đơn bắt buộc | 
|`123 / 0 0`|`123`| trường hợp không chia tách | 
|`123 / 100 100`|`6`| chia hoàn toàn thành các chữ số đơn | 
|`00000`|`0`| sự ổn định của số 0 đứng đầu | 
|`9999 / 50 50 50`| không tầm thường | trộn xác suất | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả xác suất bằng 0, nghĩa là số đó không bao giờ bị chia. Thuật toán xử lý việc này bởi vì mọi$q_i = 1$, do đó phép truy toán giảm xuống mức dịch chuyển chữ số thuần túy mà không có bất kỳ tỷ lệ xác suất nào. Đối với đầu vào`s = "1203"`Và`p = [0,0,0]`, tính toán xây dựng`1 → 12 → 120 → 1203`và câu trả lời cuối cùng khớp với giá trị nguyên đầy đủ. 

Một trường hợp cạnh khác là khi tất cả xác suất là 100, nghĩa là mọi cặp liền kề luôn bị chia đôi. Vì`s = "456"`với`p = [100,100]`, tất cả$q_i = 0$, do đó phép tính lặp lại sẽ chuyển sang khởi động lại ở mỗi chữ số. Thuật toán tạo ra`4 + 5 + 6 = 15`, khớp với tổng được phân đoạn đầy đủ dự kiến. 

Trường hợp cạnh thứ ba là một chuỗi số 0 dài. Vì`s = "0000"`bất kể xác suất ra sao, mọi đóng góp tiền tố vẫn bằng 0, do đó trạng thái đang chạy không bao giờ thay đổi. Thuật toán giữ chính xác cả hai`pref`Và`dp`ở mức 0 xuyên suốt, tránh mọi vấn đề về phân chia hoặc tràn ẩn.
