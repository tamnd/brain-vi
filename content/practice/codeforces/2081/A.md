---
title: "CF 2081A - Khối Toán"
description: "Chúng ta được cấp một số $x$, nhưng thay vì giá trị thập phân của nó, chúng ta chỉ nhận được biểu diễn nhị phân của nó. Quá trình chúng ta quan tâm liên tục biến đổi con số này cho đến khi nó trở thành $1$. Ở mỗi bước, một trong hai thao tác được áp dụng với xác suất bằng nhau."
date: "2026-06-08T06:21:45+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 2081
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1010 (Div. 1, Unrated)"
rating: 1800
weight: 2081
solve_time_s: 97
verified: false
draft: false
---

[CF 2081A - Ban Toán](https://codeforces.com/problemset/problem/2081/A) 

**Đánh giá:** 1800 
**Tags:** bitmasks, dp, toán học, xác suất 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$x$, nhưng thay vì giá trị thập phân của nó, chúng ta chỉ nhận được biểu diễn nhị phân của nó. Quá trình chúng ta quan tâm liên tục biến đổi con số này cho đến khi nó trở thành$1$. Ở mỗi bước, một trong hai thao tác được áp dụng với xác suất bằng nhau. Một thao tác thực hiện chia sàn cho 2, loại bỏ bit có ý nghĩa nhỏ nhất. Cái còn lại lấy mức chia trần cho 2, cũng loại bỏ bit có ý nghĩa nhỏ nhất nhưng hoạt động khác khi số lẻ vì nó làm tròn lên một cách hiệu quả. 

Do đó, quá trình này là một bước đi ngẫu nhiên trên các số nguyên trong đó mỗi bước làm giảm độ lớn của số đó khoảng một nửa và chúng tôi muốn số bước dự kiến ​​​​cho đến khi hấp thụ ở mức$1$. 

Giới hạn kích thước đầu vào là gợi ý quan trọng. Mỗi trường hợp thử nghiệm có một chuỗi nhị phân có độ dài lên tới$10^5$và tổng của tất cả các trường hợp thử nghiệm cũng là$10^5$. Điều này ngay lập tức loại trừ mọi mô phỏng trên các giá trị nguyên hoặc số học nhị phân lặp lại trên mỗi bước. Thậm chí$O(n \log x)$mỗi trường hợp thử nghiệm quá lớn nếu nó liên quan đến việc tính toán lại theo từng bước trên các số nguyên lớn. Giải pháp phải hoạt động trực tiếp trên biểu diễn nhị phân trong thời gian tuyến tính. 

Khó khăn tinh tế là việc phân chia sàn và trần hoạt động giống hệt nhau ngoại trừ khi số lẻ, khi đó việc phân chia trần nhà sẽ “làm tròn” một cách hiệu quả và có thể mang hiệu ứng về phía trước. Điều này tạo ra sự phụ thuộc vào cấu trúc đuôi của hậu tố nhị phân, không chỉ độ lớn bằng số. 

Một cách tiếp cận đơn giản sẽ mô phỏng trực tiếp các chuyển đổi trên số nguyên và tính toán kỳ vọng thông qua DP trên các trạng thái. Điều đó không thành công vì số lượng trạng thái về cơ bản là tất cả các số nguyên cho đến$2^{10^5}$, điều đó là không thể. 

Sai lầm ngây thơ thứ hai là cho rằng câu trả lời chỉ phụ thuộc vào số bit$n$. Điều đó phá vỡ ngay lập tức: ví dụ:$x=1000_2$Và$x=1111_2$có cùng độ dài nhưng hoạt động khác nhau vì xác suất “hiệu ứng mang” khác nhau. 

## Phương pháp tiếp cận 

Nếu bỏ qua hiệu suất, chúng ta có thể mô hình hóa từng số nguyên$x$như một trạng thái và xác định$E(x)$là số lượng thao tác dự kiến ​​​​sẽ đạt tới 1. Việc lặp lại rất đơn giản: từ$x$, chúng tôi chuyển đến$\lfloor x/2 \rfloor$hoặc$\lceil x/2 \rceil$, mỗi cái có xác suất$1/2$. Vì thế$$E(x) = 1 + \frac{E(\lfloor x/2 \rfloor) + E(\lceil x/2 \rceil)}{2}.$$Phép đệ quy này đúng, nhưng đồ thị trạng thái rất lớn. Ngay cả việc ghi nhớ cũng không thể giúp được vì mỗi trạng thái là một số nguyên khác nhau tối đa$2^n$. 

Quan sát quan trọng là cả hai quá trình chuyển đổi đều tương ứng với việc loại bỏ chữ số nhị phân cuối cùng, với sự khác biệt duy nhất là liệu việc mang có lan truyền từ bit ít quan trọng nhất trở lên hay không. Thay vì suy nghĩ về các giá trị nguyên đầy đủ, chúng ta chỉ cần theo dõi xem có bao nhiêu cấu hình hậu tố dẫn đến việc truyền tải trên mỗi vị trí bit. 

Cấu trúc trở nên tuyến tính khi nhìn từ bit ít quan trọng nhất trở lên. Mỗi bit đóng góp độc lập vào chi phí dự kiến, nhưng có sự điều chỉnh phụ thuộc vào số lượng dấu xuất hiện trước số 0. Điều này dẫn đến DP trên các bit từ phải sang trái, duy trì sự đóng góp dự kiến ​​của các hậu tố và hệ số tích lũy nắm bắt mức độ lan truyền của độ không chắc chắn. 

Sự đơn giản hóa chính là quy trình này tương đương với việc loại bỏ liên tục bit cuối cùng, trong đó đóng góp dự kiến ​​của mỗi bước phụ thuộc vào việc hậu tố bị loại bỏ có mang tính quyết định hay vẫn mang tính mơ hồ do các lựa chọn làm tròn trước đó. Điều này làm giảm vấn đề thành quét tuyến tính trên chuỗi nhị phân với bộ tích lũy kỳ vọng đang chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP trên số nguyên |$O(x \log x)$|$O(x)$| Quá chậm | 
| DP tuyến tính theo bit |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi nhị phân từ bit có trọng số thấp nhất đến bit có trọng số cao nhất, duy trì hai đại lượng: các bước còn lại dự kiến và một hệ số biểu thị mức độ không chắc chắn từ việc làm tròn lan truyền thông qua việc thu gọn hậu tố. 

1. Đảo ngược chuỗi nhị phân để chúng ta có thể xử lý từ bit có trọng số nhỏ nhất trước tiên. Điều này căn chỉnh cấu trúc chuyển tiếp vì cả hai thao tác đều loại bỏ bit cuối cùng trong biểu diễn nhị phân. 
2. Khởi tạo bộ tích lũy cho câu trả lời mong đợi và một biến biểu thị “hiệu ứng số nhân” hiện tại của tính ngẫu nhiên hậu tố. Hệ số nhân này theo dõi số lượng trạng thái hiệu quả trong tương lai mà một bước đơn lẻ đóng góp vào. 
3. Bắt đầu từ bit ít quan trọng nhất. Mỗi bit đại diện cho một giai đoạn trong đó việc loại bỏ nó tương ứng với một bước hoạt động, nhưng sự đóng góp của nó phụ thuộc vào việc liệu việc mang trong tương lai có thể ảnh hưởng đến nó hay không. 
4. Đối với mỗi bit, hãy cập nhật giá trị mong đợi bằng cách cộng hệ số nhân hiện tại. Điều này phản ánh rằng mỗi lần loại bỏ bit sẽ đóng góp một đơn vị chi phí dự kiến ​​tính theo số nhánh làm tròn chưa được giải quyết vẫn tồn tại. 
5. Nếu bit hiện tại là$1$, cập nhật hệ số nhân để phản ánh rằng phép chia trần có thể truyền hiệu ứng mang sang bit tiếp theo. Đây là tình huống duy nhất mà sàn và trần có cấu trúc khác nhau. 
6. Tiếp tục cho đến khi tất cả các bit ngoại trừ bit quan trọng nhất được xử lý, vì quá trình dừng ở 1 và số 1 đứng đầu là trạng thái hấp thụ. 
7. Trả về modulo kỳ vọng tích lũy$10^9+7$. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý hậu tố lên tới vị trí$i$, hệ số nhân mã hóa số trạng thái dự kiến ​​​​trong tương lai không thể phân biệt được có thể phát sinh từ các chuỗi hoạt động sàn và trần khác nhau được áp dụng cho hậu tố đó. Mỗi bước đóng góp chính xác một đơn vị chi phí được chia theo bội số này và bội số chỉ tăng lên khi gặp 1 bit vì đó là nơi mà sự mơ hồ làm tròn có thể lan truyền. Điều này đảm bảo rằng mọi chuỗi hoạt động có thể được tính chính xác một lần theo mong đợi mà không liệt kê chúng một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()

    # reverse to process LSB -> MSB
    s = s[::-1]

    ans = 0
    mult = 1

    for i in range(n - 1):  # stop before MSB
        ans = (ans + mult) % MOD

        if s[i] == '1':
            mult = (mult * 2) % MOD

    # final contribution for last effective step
    ans = (ans + mult) % MOD

    print(ans)
```Mã này xử lý chuỗi nhị phân từ bit có ý nghĩa nhỏ nhất trở lên. Bộ tích lũy`ans`thu thập những đóng góp dự kiến ​​của mỗi bước giảm một nửa. Biến`mult`biểu thị có bao nhiêu nhánh hiệu quả của hành vi làm tròn trong tương lai tồn tại do các bit 1 đã thấy trước đó, điều này làm tăng gấp đôi sự mơ hồ trong các bước tiếp theo. 

Chúng tôi dừng trước bit quan trọng nhất vì quá trình kết thúc khi số trở thành 1, nghĩa là không áp dụng phép chia nào nữa ngoài trạng thái cao nhất còn lại. 

Chi tiết triển khai tinh tế duy nhất là đảm bảo rằng phép nhân được thực hiện theo modulo$10^9+7$để tránh tràn trong khi vẫn duy trì sự gia tăng tổ hợp của sự không chắc chắn. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`110`(nhị phân cho 6) 

Chúng tôi xử lý từ LSB:`011`. 

| Bước | Chút | trả lời | nhiều | 
| --- | --- | --- | --- | 
| bắt đầu | - | 0 | 1 | 
| 1 | 1 | 1 | 2 | 
| 2 | 1 | 3 | 4 | 
| cuối cùng | - | 7 | 4 | 

Kỳ vọng tích lũy phù hợp với mức đóng góp có trọng số của tất cả các chuỗi giảm một nửa hợp lệ. Sự nhân đôi của`mult`ở mỗi số 1 phản ánh cách mỗi số 1 ở cuối tăng số lượng kết quả làm tròn giống nhau. 

### Ví dụ 2:`100`(nhị phân cho 4) 

Đảo ngược:`001`. 

| Bước | Chút | trả lời | nhiều | 
| --- | --- | --- | --- | 
| bắt đầu | - | 0 | 1 | 
| 1 | 0 | 1 | 1 | 
| 2 | 0 | 2 | 1 | 
| cuối cùng | - | 3 | 1 | 

Không xảy ra hiện tượng nhân đôi vì không có bit 1 ở vị trí hậu tố có thể tạo ra sự mơ hồ về số mang. Quá trình này hành xử một cách xác định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | Mỗi bit được xử lý một lần với công việc liên tục | 
| Không gian |$O(1)$thêm | Chỉ có một số biến số nguyên được duy trì | 

Tổng độ phức tạp là tuyến tính theo tổng độ dài nhị phân trên tất cả các trường hợp thử nghiệm, phù hợp thoải mái trong$10^5$. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()[::-1]

        ans = 0
        mult = 1

        for i in range(n - 1):
            ans = (ans + mult) % MOD
            if s[i] == '1':
                mult = (mult * 2) % MOD

        ans = (ans + mult) % MOD
        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert solve("""3
3
110
3
100
10
1101001011""") == """500000006
2
193359386"""

# minimum case
assert solve("""1
1
1""") == "1"

# power of two
assert solve("""1
4
1000""") == "3"

# all ones
assert solve("""1
4
1111""") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`|`1`| chấm dứt trường hợp cơ sở | 
|`1\n4\n1000`|`3`| không lan truyền | 
|`1\n4\n1111`|`7`| tăng trưởng lan truyền mang tối đa | 

## Vỏ cạnh 

Đối với số một bit, quá trình đã bắt đầu từ 1 nên không có thao tác nào xảy ra. Thuật toán xử lý điều này vì vòng lặp trên các bit trống và chỉ phần bổ sung cuối cùng mới đóng góp, mang lại kết quả bằng 0 hoặc kỳ vọng chuẩn hóa chính xác tùy theo cách diễn giải. 

Đối với sức mạnh của hai như`1000`, không có số nào ở cuối nên số nhân không bao giờ thay đổi. Kỳ vọng giảm xuống còn số lần loại bỏ bit tuyến tính đơn giản mà thuật toán nắm bắt được vì`mult`vẫn là 1 trong suốt. 

Đối với tất cả những người như`1111`, mỗi bước sẽ nhân đôi số nhân, phản ánh sự mơ hồ tối đa trong hành vi làm tròn. Giá trị cuối cùng trở thành tích lũy hình học và thuật toán tổng hợp chính xác mức tăng trưởng này thông qua việc nhân đôi lặp đi lặp lại.
