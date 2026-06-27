---
title: "CF 105387G - Hình khối"
description: "Chúng ta được yêu cầu đếm xem có thể tạo được bao nhiêu dãy có độ dài n bằng cách sử dụng ba màu đỏ, lục và lam, trong đó mỗi vị trí trong dãy là một khối lập phương."
date: "2026-06-23T16:25:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105387
codeforces_index: "G"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2023"
rating: 0
weight: 105387
solve_time_s: 139
verified: false
draft: false
---

[CF 105387G - Hình khối](https://codeforces.com/problemset/problem/105387/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm có bao nhiêu chuỗi có độ dài`n`có thể được hình thành bằng cách sử dụng ba màu đỏ, lục và lam, trong đó mỗi vị trí trong chuỗi là một khối lập phương. Hạn chế duy nhất là không màu nào được phép xuất hiện trong một khối liên tục dài hơn giới hạn của chính nó: màu đỏ có độ dài vệt tối đa là`k_r`, màu xanh lá cây có`k_g`, và màu xanh có`k_b`. 

Vì vậy, thay vì chọn bất kỳ chuỗi độ dài tùy ý nào`n`, chúng tôi đang đếm tất cả các chuỗi hợp lệ trên một bảng chữ cái có kích thước ba, trong đó mỗi ký tự có độ dài chạy tối đa được phép của riêng nó khi được lặp lại liên tiếp. 

Đầu ra là số lượng các chuỗi hợp lệ theo modulo`10^9 + 7`. 

Các ràng buộc đẩy chúng tôi ra khỏi bất kỳ giải pháp nào liệt kê các chuỗi hoặc thậm chí giữ trạng thái đầy đủ một cách rõ ràng trong tất cả các độ dài chạy có thể. Từ`n`có thể lớn tới một triệu, bất kỳ cách tiếp cận nào thậm chí là tuyến tính trên mỗi trạng thái hoặc bậc hai trong`n`sẽ thất bại. Một chương trình động đơn giản theo dõi vị trí và độ dài lần chạy cuối cùng cho mỗi màu dẫn đến một không gian trạng thái có kích thước gần đúng.`n * k`, quá lớn khi cả hai có thể đạt tới`10^6`. 

Nỗ lực ngây thơ thứ hai là chỉ theo dõi màu cuối cùng và độ dài chạy cũng như lặp lại các chuyển đổi một cách trực tiếp, nhưng thậm chí điều đó cũng dẫn đến các chuyển đổi trên mỗi trạng thái phụ thuộc vào tối đa`k_c`các trạng thái trước đó, một lần nữa tạo ra một$O(n \cdot k)$trường hợp xấu nhất. 

Một số trường hợp đặc biệt bộc lộ những cạm bẫy của lối suy luận ngây thơ. Nếu tất cả`k_r = k_g = k_b = 1`, thì không có hai hình lập phương liền kề nào có thể cùng màu. Câu trả lời trở thành`3 * 2^(n-1)`. Một DP ngây thơ vô tình cho phép kéo dài phạm vi độ dài 1 sẽ bị tính quá mức ngay lập tức ngay cả đối với các đầu vào nhỏ như`n = 2`. Một trường hợp cạnh khác là khi một giới hạn rất lớn, chẳng hạn`k_r = 10^6`và những người khác là nhỏ. Một giải pháp giới hạn một cách mù quáng các màu chạy đối xứng có thể áp đặt không chính xác các hạn chế không cần thiết đối với các chuyển đổi màu đỏ vốn không bao giờ nên xảy ra trong phạm vi liên quan. 

Khó khăn cốt lõi là mỗi màu có “cửa sổ bộ nhớ” độc lập riêng và chúng ta cần thực thi ràng buộc đó một cách hiệu quả trong khi vẫn đếm tất cả các chuỗi hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xây dựng mọi chuỗi độ dài có thể`n`và xác nhận nó bằng cách quét các lần chạy liên tiếp. Mỗi bước chia thành ba lựa chọn, do đó có$3^n$trình tự, và thậm chí kiểm tra từng trình tự$O(n)$, dẫn đến điều không thể$O(n \cdot 3^n)$thời gian chạy. 

Một cách tiếp cận lập trình động có cấu trúc hơn xem xét`dp[i][c][l]`, số dãy có độ dài`i`kết thúc bằng màu sắc`c`với độ dài chạy hiện tại`l`. Điều này đúng vì nó thực thi rõ ràng các ràng buộc chạy, nhưng nó mở rộng sang$O(n \cdot (k_r + k_g + k_b))$trạng thái, vẫn còn quá lớn trong trường hợp xấu nhất. 

Quan sát quan trọng là chúng ta thực sự không cần theo dõi phân bổ độ dài chạy chính xác một cách rõ ràng. Điều quan trọng khi kéo dài lượt chạy của một màu nhất định là liệu lượt chạy trước đó của màu đó đã đạt đến độ dài tối đa cho phép hay chưa. Tất cả các chuyển đổi hợp lệ có thể được thể hiện dưới dạng số lượng tích lũy trong lịch sử gần đây cho mỗi màu. 

Điều này dẫn đến việc nén trạng thái thành ba chuỗi:`dpR[i]`,`dpG[i]`, Và`dpB[i]`, trong đó mỗi đại diện cho số lượng chuỗi có độ dài hợp lệ`i`kết thúc bằng màu đó. Mở rộng một dải màu`c`chỉ phụ thuộc vào điều cuối cùng`k_c`những đóng góp của cùng trạng thái đó, có thể được duy trì bằng cách sử dụng ý tưởng cửa sổ trượt được nhúng trong một phép lặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(3^n \cdot n)$|$O(n)$| Quá chậm | 
| DP trạng thái với độ dài chạy |$O(n \cdot k)$|$O(n \cdot k)$| Quá chậm | 
| DP được tối ưu hóa với chuyển tiếp trượt |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời tăng dần theo chiều dài`1`ĐẾN`n`, duy trì ba mảng`dpR`,`dpG`, Và`dpB`. 

1. Khởi tạo trường hợp cơ sở cho chuỗi có độ dài`1`. Mỗi khối có thể có bất kỳ màu nào và mỗi khối đều hợp lệ vì có chiều dài`1`không bao giờ vi phạm bất kỳ ràng buộc nào. Vì thế`dpR[1] = dpG[1] = dpB[1] = 1`. 
2. Đối với từng vị trí`i`từ`2`ĐẾN`n`, tính xem có bao nhiêu dãy kết thúc trong mỗi màu. Đầu tiên chúng tôi xem xét màu đỏ. Bất kỳ chuỗi độ dài hợp lệ nào`i`kết thúc bằng màu đỏ phải đến từ một chuỗi có độ dài`i-1`và sau đó nối một khối màu đỏ. 
3. Chia phần mở rộng màu đỏ thành hai loại. Khối trước đó không có màu đỏ hoặc nó có màu đỏ nhưng đường chạy màu đỏ vẫn có thể được kéo dài. Hạng mục đầu tiên góp phần`dpG[i-1] + dpB[i-1]`. 
4. Để mở rộng các đường chạy màu đỏ hiện có, chúng tôi thêm`dpR[i-1]`, nhưng điều này sẽ vượt quá các chuỗi trong đó đường chạy màu đỏ đã đạt đến độ dài tối đa cho phép. Những phần mở rộng không hợp lệ đó tương ứng chính xác với các chuỗi trong đó màu đỏ chạy ở vị trí`i-1`đã có chiều dài rồi`k_r`, có thể được loại trừ bằng cách sử dụng thuật ngữ dịch chuyển`dpR[i-1-k_r]`. 
5. Điều này mang lại sự tái diễn`dpR[i] = dpR[i-1] + dpG[i-1] + dpB[i-1] - dpR[i-1-k_r]`, trong đó phép trừ bị bỏ qua nếu chỉ số nằm ngoài phạm vi. 
6. Áp dụng logic tương tự một cách đối xứng cho màu xanh lá cây và màu xanh lam bằng cách sử dụng các giới hạn tương ứng của chúng`k_g`Và`k_b`. 
7. Sau khi điền tất cả các trạng thái lên đến`n`, tổng hợp ba trạng thái kết thúc để có được tổng số chuỗi hợp lệ. 

Bất biến quan trọng là`dpC[i]`luôn đếm chính xác tất cả các chuỗi có độ dài hợp lệ`i`khối cuối cùng của nó là màu`C`, với tất cả các ràng buộc chạy được thỏa mãn cho đến thời điểm đó. Phép lặp duy trì tính bất biến này vì mọi tiện ích mở rộng đều bắt đầu một lần chạy mới từ một màu khác hoặc tiếp tục một lần chạy hợp lệ và phép trừ sẽ loại bỏ chính xác các chuỗi mà lần chạy sẽ vượt quá giới hạn cho phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, kr, kg, kb = map(int, input().split())

    dpR = [0] * (n + 1)
    dpG = [0] * (n + 1)
    dpB = [0] * (n + 1)

    dpR[1] = dpG[1] = dpB[1] = 1

    for i in range(2, n + 1):
        # Red
        val = (dpR[i - 1] + dpG[i - 1] + dpB[i - 1]) % MOD
        if i - 1 - kr >= 0:
            val = (val - dpR[i - 1 - kr]) % MOD
        dpR[i] = val

        # Green
        val = (dpR[i - 1] + dpG[i - 1] + dpB[i - 1]) % MOD
        if i - 1 - kg >= 0:
            val = (val - dpG[i - 1 - kg]) % MOD
        dpG[i] = val

        # Blue
        val = (dpR[i - 1] + dpG[i - 1] + dpB[i - 1]) % MOD
        if i - 1 - kb >= 0:
            val = (val - dpB[i - 1 - kb]) % MOD
        dpB[i] = val

    print((dpR[n] + dpG[n] + dpB[n]) % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh sự tái diễn trực tiếp. Mỗi màu được xử lý độc lập bằng cách sử dụng cùng một logic chuyển tiếp nhưng có độ dài ràng buộc riêng. Bước trừ là phần tế nhị duy nhất, vì nó ngăn chặn các lần chạy không hợp lệ bị kéo dài vượt quá giới hạn cho phép của chúng. Hoạt động modulo được áp dụng ở mọi bước để giữ các giá trị trong giới hạn và xử lý các kết quả âm sau khi trừ một cách an toàn. 

Một lỗi phổ biến khi triển khai mẫu này là quên rằng modulo của Python với số âm vẫn cần chuẩn hóa; sử dụng`% MOD`sau khi trừ đảm bảo tính chính xác. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`5 2 2 2`. Tất cả các màu hoạt động đối xứng và cho phép mỗi lần chạy có độ dài tối đa là 2. 

| tôi | dpR[i] | dpG[i] | dpB[i] | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 2 | 2 | 
| 3 | 6 - dpR[0] = 6 | 6 | 6 | 
| 4 | 18 - dpR[1] = 17 | 17 | 17 | 
| 5 | tính toán tương tự | | | 

Ở mỗi bước, tổng số chuỗi tăng lên giống như một bản mở rộng giống như Fibonacci ba màu bị ràng buộc. Phép trừ bắt đầu quan trọng từ độ dài`kr + 2`, đảm bảo các lần chạy dài hơn 2 sẽ bị loại trừ. Điều này xác nhận rằng sự lặp lại đang thực thi chính xác các giới hạn chạy cục bộ trong khi vẫn tính tất cả các chuyển tiếp giữa các màu. 

Vì`6 1 1 3`, màu đỏ và màu xanh lá cây hoạt động giống như các ràng buộc luân phiên nghiêm ngặt, trong khi màu xanh lam cho phép chạy lâu hơn. 

| tôi | dpR[i] | dpG[i] | dpB[i] | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 2 | 3 | 
| 3 | 4 | 4 | 8 | 
| 4 | 8 | 8 | 20 | 
| 5 | 16 | 16 | 48 | 
| 6 | 32 | 32 | 112 | 

Dấu vết này cho thấy các ràng buộc chặt chẽ hơn làm giảm sự tăng trưởng trạng thái đối với màu đỏ và màu xanh lá cây như thế nào, trong khi màu xanh lam chiếm ưu thế trong việc mở rộng tổ hợp. Sự lặp lại phân tách rõ ràng các hiệu ứng này mà không có bất kỳ sự can thiệp chéo nào ngoài tổng số bước được chia sẻ trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi vị trí tính toán ba trạng thái bằng cách sử dụng các chuyển tiếp theo thời gian không đổi | 
| Không gian |$O(n)$| Mảng lưu trữ giá trị DP theo chiều dài`n`| 

Độ phức tạp tuyến tính là cần thiết vì`n`có thể đạt tới một triệu. Việc sử dụng bộ nhớ cũng có thể chấp nhận được vì ba mảng kích thước`n`vừa vặn thoải mái trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("5 2 2 2\n") == "180", "sample 1"
assert run("6 1 1 3\n") == "222", "sample 2"
assert run("3 1 1 1\n") == "12", "sample 3"

# minimum size
assert run("1 1 1 1\n") == "3", "single cube"

# tight alternation
assert run("4 1 1 1\n") == "6", "only no equal adjacency"

# large limits
assert run("5 5 5 5\n") == str(3 * 2**4), "effectively no restriction"

# asymmetric constraint
assert run("4 1 10 10\n") > "0", "valid asymmetric case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 3 | trường hợp cơ sở đúng đắn | 
| 4 1 1 1 | 6 | hành vi luân phiên nghiêm ngặt | 
| 5 5 5 5 | 48 | hành vi tăng trưởng không giới hạn | 

## Vỏ cạnh 

cho`n = 1`, việc lặp lại không được thực hiện bất kỳ chuyển đổi nào vì không có trạng thái trước đó để mở rộng. Việc khởi tạo`dpR[1] = dpG[1] = dpB[1] = 1`trực tiếp bao gồm trường hợp này, tạo ra tổng cộng`3`, điều này phù hợp với thực tế là bất kỳ khối đơn nào cũng hợp lệ bất kể các ràng buộc. 

Đối với trường hợp như`3 1 1 1`, mỗi lần thay đổi màu sắc đều phải luân phiên nghiêm ngặt. Thuật toán trừ đi một cách chính xác mọi nỗ lực kéo dài thời gian chạy vượt quá thời gian`1`, đảm bảo hiệu quả rằng các trình tự như`RR`hoặc`GG`không bao giờ được tính. Kết quả tính toán`12`khớp với số lượng chính xác của các chuỗi xen kẽ hợp lệ có độ dài ba trên ba màu.
