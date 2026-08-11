---
title: "CF 102409K - Tai ương cho vay"
description: "Các khoản vay ban đầu không còn quan trọng nữa khi chúng ta biết vị thế ròng của mỗi người. Với mỗi khoản vay a b c, người a đã cho đi c, trong khi người b nhận được c."
date: "2026-08-12T00:09:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102409
codeforces_index: "K"
codeforces_contest_name: "Semana i 2019"
rating: 0
weight: 102409
solve_time_s: 273
verified: true
draft: false
---

[CF 102409K - Tai ương cho vay](https://codeforces.com/problemset/problem/102409/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các khoản vay ban đầu không còn quan trọng nữa khi chúng ta biết vị thế ròng của mỗi người. Đối với mỗi khoản vay`a b c`, người`a`đã cho đi`c`, trong khi người`b`đã nhận được`c`. Khi tất cả các khoản vay được cộng lại, hãy xác định số dư của một người là số tiền họ sẽ nhận được trừ đi số tiền họ phải trả. Số dư dương có nghĩa là người đó phải nhận số tiền đó, số dư âm có nghĩa là người đó phải trả giá trị tuyệt đối của nó và số dư bằng 0 có nghĩa là người đó đã được thanh toán xong. 

Ví dụ, nếu`0`cho mượn`1`năm đơn vị và`1`cho mượn sau`2`năm đơn vị, số dư là`0 = +5`,`1 = 0`, Và`2 = -5`. Giao dịch trung gian ban đầu biến mất khỏi vấn đề, bởi vì người`0`có thể chỉ cần nhận năm đơn vị trực tiếp từ người`2`. 

Đầu ra là bất kỳ khoản thanh toán nào đưa mọi số dư về 0. Mục tiêu đầu tiên là giảm thiểu tổng số tiền được chuyển. Khi điều đó là tối ưu, mục tiêu thứ hai là giảm thiểu số lượng thanh toán. Định dạng đầu ra bắt buộc là số lần thanh toán, theo sau là người trả tiền, người nhận và số tiền. Báo cáo vấn đề xác nhận rằng biểu đồ khoản vay ban đầu có thể được thay thế bằng số dư ròng thu được. 

Mục tiêu đầu tiên có đặc tính đơn giản. Hãy để tổng số dư dương là`P`. Mỗi người có số dư dương phải nhận chính xác số dư của mình, vì vậy mọi khoản thanh toán hợp lệ đều chuyển ít nhất`P`đơn vị. Chúng ta có thể đạt được chính xác`P`bằng cách chỉ cho phép những người có số dư âm thanh toán cho những người có số dư dương. Do đó, tổng số tiền tối thiểu được cố định trước khi chúng tôi bắt đầu tối ưu hóa số lần thanh toán. 

Phần khó khăn là mục tiêu thứ hai. Có nhiều nhất 18 người, đây là hạn chế then chốt. Một thuật toán đa thức sẽ rất thú vị, nhưng vấn đề này chứa một lựa chọn giống như tập hợp con: một nhóm người có thể được giải quyết một cách độc lập một cách chính xác khi số dư của họ có tổng bằng 0. Việc tìm kiếm tập hợp tốt nhất của các nhóm như vậy nói chung là theo cấp số nhân, vì vậy`N <= 18`là điều làm cho chương trình động bitmask trở nên thực tế. Số lượng khoản vay có thể lên tới`100000`, nhưng những khoản vay đó chỉ phải được tích lũy vào`N`số dư, do đó sự đóng góp của họ là tuyến tính trong`K`. 

Có một số trường hợp đặc biệt mà việc triển khai trực tiếp có thể xử lý sai. 

Hãy xem xét hai người có số dư trái ngược nhau.```
2 1
1 0 1
```Người`0`có sự cân bằng`-1`và người`1`có sự cân bằng`+1`, vì vậy khoản thanh toán duy nhất là`0 1 1`. Việc thực hiện bất cẩn mà giữ đúng định hướng cho vay ban đầu sẽ dẫn đến kết quả ngược lại và không giải quyết được số dư. 

Bây giờ hãy xem xét một người có số dư bằng 0 mặc dù họ đã xuất hiện trong nhiều khoản vay.```
3 2
0 1 5
1 0 5
```Mọi số dư đều bằng 0, vì vậy đầu ra chính xác chỉ đơn giản là```
0
```Giữ những người ở trạng thái cân bằng bằng 0 trong DP theo cấp số nhân không làm thay đổi tính chính xác nhưng nó sẽ nhân đôi không gian trạng thái cho mỗi người bổ sung một cách không cần thiết. Loại bỏ chúng trước DP là một cách tối ưu hóa đáng kể khi nhiều khoản vay bị hủy. 

Trường hợp tinh vi hơn là khi tồn tại một số nhóm có tổng bằng 0 độc lập. Ví dụ,```
4 2
0 2 2
1 3 2
```cung cấp số dư`+2, +2, -2, -2`. Hai khoản thanh toán là đủ, một khoản trong mỗi cặp. Một thủ tục tham lam liên tục chọn con nợ và chủ nợ tùy tiện vẫn có thể giải quyết số tiền, nhưng có thể bỏ lỡ thực tế là những người tham gia đã chia thành các thành phần độc lập. Mục tiêu thứ hai chính xác là tìm kiếm càng nhiều thành phần độc lập như vậy càng tốt. 

Cuối cùng, cùng một cặp có thể xảy ra nhiều lần. đầu vào```
2 100000
0 1 1
0 1 1
...
0 1 1
```với dòng lặp lại`100000`thời gian tạo ra số dư`-100000`Và`+100000`. Bản gốc`100000`các khoản vay không liên quan sau khi tổng hợp và chỉ cần thanh toán một lần. Sử dụng trạng thái cho mỗi khoản vay thay vì số dư ròng sẽ hoàn toàn bỏ sót cấu trúc khiến vấn đề trở nên nhỏ bé. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu có thể liệt kê mọi phân vùng có thể có của những người khác 0 thành các nhóm. Đối với mỗi phân vùng, hãy kiểm tra xem mọi nhóm có tổng số dư bằng 0 hay không. Nếu có, một nhóm chứa`s`mọi người có thể giải quyết bằng cách sử dụng`s-1`thanh toán, do đó tổng số thanh toán được giảm thiểu bằng phân vùng có số lượng nhóm lớn nhất. Điều này đúng vì mọi thành phần được kết nối của một khoản thanh toán phải có tổng số dư bằng 0 và nhóm có tổng bằng 0`s`mọi người luôn có thể giải quyết được với`s-1`thanh toán. 

Vấn đề là số lượng phân vùng. Số phân vùng tập hợp của 18 phần tử được dán nhãn là số Chuông thứ 18,`682076806159`, đại khái`6.82 * 10^11`. Ngay cả việc kiểm tra lượng thông tin liên tục cho mỗi phân vùng cũng vượt xa giới hạn thời gian một giây. 

Lực lượng vũ phu hoạt động vì nó tìm kiếm rõ ràng các nhóm có tổng bằng 0 độc lập. Nó thất bại vì nó tìm kiếm các phân vùng hoàn chỉnh khi hầu hết thông tin đó đều dư thừa. Quan sát hữu ích là thuộc tính duy nhất của một nhóm quan trọng là liệu tổng số dư của nó có bằng không hay không. Điều đó cho phép chúng tôi thể hiện mọi tập hợp người bằng mặt nạ bit và sử dụng lại kết quả cho tất cả các mặt nạ nhỏ hơn. 

Đối với một mặt nạ`S`, cho phép`sum[S]`là tổng số dư của nó. Định nghĩa`dp[S]`là số lượng tối đa các tập con có tổng bằng 0 tách rời từng cặp có thể tìm thấy bên trong`S`. Đây là phân vùng có tổng bằng 0 tối đa, không nhất thiết là phân vùng của tất cả`S`. 

Chọn bất kỳ người nào`x`TRONG`S`. Nếu như`S`không có tổng bằng 0, một tập hợp tối ưu các nhóm có tổng bằng 0 không thể chứa mọi người trong`S`, bởi vì những nhóm đó gộp lại cũng sẽ có tổng bằng 0. Có thể loại bỏ ít nhất một người mà không làm giảm mức tối ưu. Vì vậy chúng ta có thể tận dụng tốt nhất`dp[S without x]`trên mọi lựa chọn của`x`. 

Nếu như`S`tổng của nó bằng 0, một phân vùng có tổng bằng 0 tối ưu của`S`chứa một số nhóm chứa`x`. Đang xóa`x`phá hủy chính xác nhóm đó, trong khi các nhóm còn lại tạo thành một phân vùng có tổng bằng 0 của`S without x`. Do đó, kết quả tối ưu là kết quả lớn hơn kết quả tốt nhất cho mặt nạ thu được bằng cách loại bỏ một phần tử. 

Điều này gây ra sự tái phát```
dp[S] = max dp[S without x]                         if sum[S] != 0
dp[S] = 1 + max dp[S without x]                    if sum[S] == 0
```tổng thể`x`TRONG`S`. Sự tái phát này là một tiêu chuẩn`O(n 2^n)`công thức lập trình động của phân vùng tổng bằng không tối đa. 

Toàn bộ số dư khác 0 có tổng bằng 0, vì vậy`dp[full]`chính xác là số lượng tối đa các nhóm có tổng bằng 0 độc lập. Nếu có`m`người khác không và`g`nhóm, mỗi nhóm quy mô`s`nhu cầu`s-1`thanh toán. Tổng hợp các nhóm đưa ra```
(s1 - 1) + (s2 - 1) + ... + (sg - 1)
= m - g
```tối đa hóa`g`hoàn toàn giống với việc giảm thiểu số lượng thanh toán. 

DP chỉ đưa ra số lượng nhóm tối ưu. Để khôi phục các nhóm thực tế, chúng tôi quay lại. Chọn một người còn lại`x`và liệt kê các mặt nạ con có chứa`x`cho đến khi tìm được tập con có tổng bằng 0`G`thỏa mãn```
dp[remaining without G] + 1 = dp[remaining]
```Một tập hợp con như vậy phải tồn tại theo định nghĩa của DP. Một lần`G`được tìm thấy, loại bỏ nó và lặp lại. 

Sau khi đã biết các nhóm, mỗi nhóm có thể được giải quyết độc lập. Trong một nhóm, liên tục chọn một con nợ và một chủ nợ và chuyển số tiền còn lại nhỏ hơn. Ít nhất một trong số chúng trở thành số 0 sau mỗi lần thanh toán. Vì nhóm bắt đầu với`s`số dư khác 0 và kết thúc với tất cả số dư bằng 0, chính xác`s-1`thanh toán được sản xuất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(B_n * n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(K + n 2^n)`|`O(2^n)`| Đã chấp nhận | 

Đây`B_n`là số Bell thứ n, trong khi phương pháp tối ưu chỉ sử dụng`2^n`nhiều nhất là đeo mặt nạ và kiểm tra`n`chuyển tiếp trên mỗi mặt nạ. 

## Hướng dẫn thuật toán 

1. Đọc từng khoản vay và tích lũy số dư ròng. Đối với một khoản vay`a b c`, thêm vào`c`ĐẾN`balance[a]`và trừ`c`từ`balance[b]`, bởi vì`a`có quyền nhận lại số tiền đó trong thời gian`b`nợ nó. 
2. Loại bỏ những người có số dư cuối cùng bằng 0. Một người như vậy không tham gia vào quá trình giải quyết bắt buộc và việc loại trừ họ sẽ làm giảm kích thước mặt nạ bit mà không thay đổi câu trả lời. 
3. Lưu trữ số dư còn lại theo một mảng có độ dài`m`. Vì mọi khoản vay ban đầu đều chuyển tiền từ người này sang người khác nên tổng của tất cả số dư bằng không. 
4. Tính toán`sum[mask]`cho mỗi tập hợp con. Xóa bit được đặt thấp nhất khỏi`mask`, sử dụng lại tổng đã tính toán của mặt nạ nhỏ hơn và thêm số dư tương ứng. Điều này tính toán tất cả các tổng tập hợp con trong`O(2^m)`thời gian. 
5. Khởi tạo`dp[0] = 0`. Đối với mọi mặt nạ không trống, hãy loại bỏ từng bit được đặt có thể và kế thừa kết quả tốt nhất từ ​​mặt nạ nhỏ hơn. Nếu mặt nạ hiện tại có tổng bằng 0, hãy thêm một vào ứng viên vì bản thân mặt nạ hiện tại có thể tạo thành một nhóm có tổng bằng 0. 
6. Đọc`dp[full]`là số lượng tối đa có thể có của các nhóm tổng bằng 0 độc lập. Vì một nhóm với`s`mọi người cần chính xác`s-1`thanh toán, số lần thanh toán tối thiểu là`m - dp[full]`. 
7. Xây dựng lại các nhóm. Lấy bit được đặt thấp nhất`x`của mặt nạ hiện tại và liệt kê các mặt nạ con của nó. Tìm một mặt nạ phụ`G`chứa đựng`x`tổng của nó bằng 0 và bằng cái nào`dp[current without G] + 1`bằng`dp[current]`. Điều này xác định một nhóm được sử dụng bởi một phân vùng tối ưu. 
8. Xóa`G`từ mặt nạ hiện tại và tiếp tục cho đến khi không còn người. Việc tái thiết xem xét nhiều nhất`m`các nhóm và mỗi giai đoạn tái thiết sẽ kiểm tra nhiều nhất`2^m`mặt nạ phụ, vẫn còn nhỏ đối với`m <= 18`. 
9. Đối với mỗi nhóm thu hồi được, chia thành viên thành nhóm nợ có số dư âm và chủ nợ có số dư dương. Ghép một con nợ với một chủ nợ và chuyển số dư tuyệt đối nhỏ hơn. 
10. Cập nhật cả số dư còn lại sau mỗi lần thanh toán. Nếu con nợ bằng 0, hãy chuyển sang con nợ tiếp theo. Nếu chủ nợ bằng 0, hãy chuyển sang chủ nợ tiếp theo. Ít nhất một con trỏ tiến lên sau mỗi lần lặp, vì vậy một nhóm có kích thước`s`tạo ra chính xác`s-1`thanh toán. 

### Tại sao nó hoạt động 

Bất biến quan trọng là`dp[mask]`bằng số lượng tối đa các nhóm tổng bằng 0 rời rạc có thể được chọn từ`mask`. Nếu như`mask`không phải là tổng bằng 0, mỗi phân vùng có tổng bằng 0 sẽ để lại ít nhất một người không được sử dụng, vì vậy việc loại bỏ một người phù hợp sẽ là điều tối ưu. Nếu như`mask`có tổng bằng 0, lấy nhóm chứa người được chọn bất kỳ. Việc loại bỏ người đó sẽ giữ nguyên tất cả các nhóm khác, vì vậy phương án tối ưu cho`mask`chính xác là nhiều hơn mức tối ưu cho mặt nạ nhỏ hơn tương ứng. Điều này chứng tỏ sự tái phát bằng quy nạp trên kích thước mặt nạ. 

Đối với tập hợp đầy đủ, bản thân tất cả những người khác 0 đều có tổng bằng 0, do đó, một phân vùng con tối ưu thực sự là một phân vùng của tất cả mọi người. Nếu nó chứa`g`nhóm và`m`mọi người, những nhóm đó yêu cầu`m-g`thanh toán. Tối đa hóa`g`do đó giảm thiểu số lượng thanh toán. 

Việc giải quyết tham lam cuối cùng là chính xác trong mỗi nhóm được chọn vì mỗi khoản thanh toán chỉ chuyển tiền từ con nợ sang chủ nợ, do đó tổng số tiền được chuyển chính xác bằng tổng số dư dương. Đó là tổng số tiền chuyển tối thiểu có thể. Mỗi khoản thanh toán tạo ra ít nhất một số dư còn lại bằng 0 và khoản thanh toán cuối cùng làm cho hai số dư cuối cùng bằng 0, cho kết quả chính xác`s-1`thanh toán cho một nhóm`s`mọi người. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(data: str) -> str:
    it = iter(map(int, data.split()))
    try:
        n = next(it)
        k = next(it)
    except StopIteration:
        return ""

    balance = [0] * n

    for _ in range(k):
        a = next(it)
        b = next(it)
        c = next(it)

        balance[a] += c
        balance[b] -= c

    people = []
    vals = []

    for person, value in enumerate(balance):
        if value != 0:
            people.append(person)
            vals.append(value)

    m = len(vals)

    if m == 0:
        return "0\n"

    size = 1 << m

    subset_sum = [0] * size

    for mask in range(1, size):
        bit = mask & -mask
        idx = bit.bit_length() - 1
        subset_sum[mask] = subset_sum[mask ^ bit] + vals[idx]

    dp = [0] * size

    for mask in range(1, size):
        best = 0
        bits = mask

        if subset_sum[mask] == 0:
            while bits:
                bit = bits & -bits
                candidate = dp[mask ^ bit] + 1
                if candidate > best:
                    best = candidate
                bits ^= bit
        else:
            while bits:
                bit = bits & -bits
                candidate = dp[mask ^ bit]
                if candidate > best:
                    best = candidate
                bits ^= bit

        dp[mask] = best

    groups = []
    mask = size - 1

    while mask:
        first = mask & -mask
        sub = mask

        while sub:
            if (sub & first) and subset_sum[sub] == 0:
                rest = mask ^ sub
                if dp[rest] + 1 == dp[mask]:
                    groups.append(sub)
                    mask = rest
                    break
            sub = (sub - 1) & mask

    answer = []

    for group in groups:
        debtors = []
        creditors = []

        bits = group
        while bits:
            bit = bits & -bits
            idx = bit.bit_length() - 1

            if vals[idx] < 0:
                debtors.append([idx, -vals[idx]])
            else:
                creditors.append([idx, vals[idx]])

            bits ^= bit

        i = 0
        j = 0

        while i < len(debtors) and j < len(creditors):
            debtor, owe = debtors[i]
            creditor, receive = creditors[j]

            amount = min(owe, receive)

            answer.append((people[debtor], people[creditor], amount))

            owe -= amount
            receive -= amount

            debtors[i][1] = owe
            creditors[j][1] = receive

            if owe == 0:
                i += 1
            if receive == 0:
                j += 1

    out = [str(len(answer))]
    out.extend(f"{a} {b} {c}" for a, b, c in answer)
    return "\n".join(out) + "\n"

def main():
    data = sys.stdin.buffer.read().decode()
    sys.stdout.write(solve(data))

if __name__ == "__main__":
    main()
```Phần đầu vào sử dụng`sys.stdin.buffer.read()`thay vì liên tục gọi`input()`. Với tối đa`100000`vay, việc đọc toàn bộ dữ liệu đầu vào cùng một lúc rất đơn giản và nhanh chóng, trong khi thuật toán thực tế vẫn chỉ có`O(N)`trạng thái tài chính. 

Cập nhật số dư sử dụng`balance[a] += c`Và`balance[b] -= c`. Việc đảo ngược các dấu hiệu này là nguyên nhân phổ biến dẫn đến các câu trả lời sai vì đầu vào mô tả hướng đi của khoản vay trước đây, trong khi đầu ra mô tả hướng trả nợ cuối cùng. 

Những người không cân bằng được lọc trước khi chế tạo mặt nạ. Do đó,`m`có thể nhỏ hơn`N`và mỗi bit mặt nạ tương ứng với số dư thực tế khác 0. Khi tất cả số dư bằng 0, hàm sẽ trả về ngay lập tức và không bao giờ tạo mảng DP có kích thước bằng 0 với logic tái thiết đặc biệt. 

Mảng tổng tập hợp con sử dụng bit được đặt thấp nhất. Nếu như`bit`vậy thì có chút đó`mask ^ bit`chứa mọi người khác, vì vậy`subset_sum[mask]`có thể được tính từ một mục được tính toán trước đó. Các giá trị có thể đạt tới khoảng`10^8`, vì vậy số nguyên Python có thể xử lý chúng một cách thoải mái mà không bị tràn. 

Quá trình chuyển đổi DP có chủ ý xem xét từng bit được đặt thay vì chỉ chọn một bit cố định. Bằng chứng đảm bảo rằng ít nhất một lựa chọn duy trì một phân vùng tối ưu và việc xem xét tất cả các lựa chọn cho phép phép truy toán phát hiện ra lựa chọn đó. 

Quá trình tái cấu trúc tách biệt với DP vì quá trình lặp lại chỉ lưu trữ số lượng tối ưu chứ không phải danh tính của mọi nhóm có tổng bằng 0. Việc liệt kê các mặt nạ con trong quá trình tái thiết là hợp lý vì có nhiều nhất 18 người khác 0. Nó cũng an toàn hơn việc cố gắng suy ra một nhóm từ bit bị loại bỏ bởi quá trình chuyển đổi DP, vì bit đó không tự xác định thành phần có tổng bằng 0 tương ứng. 

Việc giải quyết cuối cùng không làm thay đổi bản gốc`vals`mảng. Nó hoạt động trên biến đổi nhỏ`[person, amount]`cặp trong mỗi nhóm. Điều này rất hữu ích vì DP mô tả phân vùng tối ưu, trong khi giai đoạn quyết toán chỉ cần xây dựng một chuỗi thanh toán hợp lệ cho phân vùng đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 1
1 0 1
```Khoản vay khiến người ta`1`một chủ nợ và một người`0`một con nợ. 

| Tiểu bang | Giá trị | 
| --- | --- | 
|`balance[0]`|`-1`| 
|`balance[1]`|`+1`| 
| số dư khác 0 |`[-1, +1]`| 
|`sum[01]`|`0`| 
|`dp[01]`|`1`| 
| nhóm phục hồi |`{0, 1}`| 

Nhóm thu hồi gồm một con nợ và một chủ nợ. Người`0`trả tiền cho người`1`một đơn vị.```
1
0 1 1
```Tổng số tiền chuyển là một, điều này là không thể tránh khỏi vì người`1`phải nhận được một đơn vị. Nhóm có hai người, vì vậy một khoản thanh toán cũng là số lượng tối thiểu có thể có. 

### Mẫu 2 

Đầu vào là```
3 4
2 0 2
1 0 1
1 0 1
2 0 1
```Bốn khoản vay cho số dư ròng sau đây. 

| Người | Số dư | 
| --- | --- | 
|`0`|`-5`| 
|`1`|`+2`| 
|`2`|`+3`| 

Có ba người khác 0 nên không gian mặt nạ chỉ có 8 trạng thái. 

| Mặt nạ | Người | Tổng hợp |`dp`| 
| --- | --- | --- | --- | 
|`001`|`0`|`-5`|`0`| 
|`010`|`1`|`+2`|`0`| 
|`100`|`2`|`+3`|`0`| 
|`011`|`0,1`|`-3`|`0`| 
|`101`|`0,2`|`-2`|`0`| 
|`110`|`1,2`|`+5`|`0`| 
|`111`|`0,1,2`|`0`|`1`| 

Không có tập hợp con có tổng bằng 0 thích hợp nên cả ba người đều tạo thành một nhóm. Giai đoạn giải quyết phù hợp với con nợ`0`với chủ nợ`1`cho hai đơn vị, sau đó với chủ nợ`2`cho ba đơn vị.```
2
0 1 2
0 2 3
```Mẫu sử dụng hai khoản thanh toán giống nhau theo thứ tự chủ nợ ngược nhau, điều này tối ưu như nhau. 

Dấu vết chứng minh lý do tại sao DP phải phân biệt giữa nhóm có tổng bằng 0 và tập hợp người tùy ý. Tập hợp đầy đủ có tổng bằng 0, do đó nó đóng góp một thành phần độc lập, trong khi không có tập hợp con thích hợp nào có thể được giải quyết một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(K + m 2^m)`|`O(K)`để tính số dư,`O(2^m)`đối với các tập hợp con,`O(m 2^m)`cho DP, và`O(m 2^m)`trong trường hợp xấu nhất để tái thiết | 
| Không gian |`O(2^m + N)`| Tổng tập hợp con và DP mỗi tập chứa`2^m`các mục, trong khi mảng cân bằng chứa`O(N)`mục | 

Từ`m <= N <= 18`, có nhiều nhất`262144`mặt nạ. DP chính hoạt động ít hơn đại khái`18 * 262144`, hoặc`4.7 million`, chuyển tiếp bit. Việc tái thiết có quy mô theo cấp số nhân tương tự và vẫn mang tính thực tế ở giới hạn này. các`100000`các khoản vay đầu vào chỉ thêm một bước tiền xử lý tuyến tính, do đó giải pháp phù hợp một cách thoải mái trong phạm vi nhỏ dự kiến.`N`, lớn-`K`cấu trúc của vấn đề. 

## Trường hợp thử nghiệm 

Các thử nghiệm dưới đây sử dụng tương tự`solve`hoạt động như giải pháp được gửi. Vì vấn đề chấp nhận bất kỳ chuỗi thanh toán tối ưu nào nên việc so sánh kết quả xác định của việc triển khai này là đủ cho các thử nghiệm cố định này.```python
import io
import sys

def solve(data: str) -> str:
    it = iter(map(int, data.split()))

    n = next(it)
    k = next(it)

    balance = [0] * n

    for _ in range(k):
        a = next(it)
        b = next(it)
        c = next(it)
        balance[a] += c
        balance[b] -= c

    people = []
    vals = []

    for person, value in enumerate(balance):
        if value:
            people.append(person)
            vals.append(value)

    m = len(vals)

    if m == 0:
        return "0\n"

    size = 1 << m
    subset_sum = [0] * size

    for mask in range(1, size):
        bit = mask & -mask
        idx = bit.bit_length() - 1
        subset_sum[mask] = subset_sum[mask ^ bit] + vals[idx]

    dp = [0] * size

    for mask in range(1, size):
        best = 0
        bits = mask

        while bits:
            bit = bits & -bits
            candidate = dp[mask ^ bit]

            if subset_sum[mask] == 0:
                candidate += 1

            if candidate > best:
                best = candidate

            bits ^= bit

        dp[mask] = best

    groups = []
    mask = size - 1

    while mask:
        first = mask & -mask
        sub = mask

        while sub:
            if (sub & first) and subset_sum[sub] == 0:
                rest = mask ^ sub
                if dp[rest] + 1 == dp[mask]:
                    groups.append(sub)
                    mask = rest
                    break
            sub = (sub - 1) & mask

    answer = []

    for group in groups:
        debtors = []
        creditors = []

        bits = group
        while bits:
            bit = bits & -bits
            idx = bit.bit_length() - 1

            if vals[idx] < 0:
                debtors.append([idx, -vals[idx]])
            else:
                creditors.append([idx, vals[idx]])

            bits ^= bit

        i = 0
        j = 0

        while i < len(debtors) and j < len(creditors):
            debtor, owe = debtors[i]
            creditor, receive = creditors[j]

            amount = min(owe, receive)

            answer.append(
                (people[debtor], people[creditor], amount)
            )

            debtors[i][1] -= amount
            creditors[j][1] -= amount

            if debtors[i][1] == 0:
                i += 1
            if creditors[j][1] == 0:
                j += 1

    out = [str(len(answer))]
    out.extend(f"{a} {b} {c}" for a, b, c in answer)
    return "\n".join(out) + "\n"

def run(inp: str) -> str:
    return solve(inp)

assert run(
    """2 1
1 0 1
"""
) == """1
0 1 1
""", "sample 1"

assert run(
    """3 4
2 0 2
1 0 1
1 0 1
2 0 1
"""
) == """2
0 1 2
0 2 3
""", "sample 2"

assert run(
    """1 0
"""
) == """0
""", "minimum size with no loans"

assert run(
    """4 2
0 2 2
1 3 2
"""
) == """2
3 0 2
2 1 2
""", "two independent equal groups"

assert run(
    """3 1
0 1 1
"""
) == """1
1 0 1
""", "zero-balance participant"

large_input = "2 100000\n" + ("0 1 1\n" * 100000)

assert run(large_input) == """1
0 1 100000
""", "maximum K and large aggregated balance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 0 1`|`1 / 0 1 1`| Chỉ đạo trực tiếp con nợ-chủ nợ | 
|`3 4 / sample 2`|`2 / 0 1 2 / 0 2 3`| Thành phần tổng bằng 0 của ba người | 
|`1 0`|`0`| Độ lún trống và nhỏ nhất có thể`N`| 
|`4 2 / 0 2 2 / 1 3 2`|`2 / 3 0 2 / 2 1 2`| Nhiều nhóm tổng bằng 0 độc lập | 
|`3 1 / 0 1 1`|`1 / 1 0 1`| Một người có số dư cuối cùng bằng 0 sẽ bị loại bỏ | 
|`2 100000`với sự lặp đi lặp lại`0 1 1`|`1 / 0 1 100000`| Lớn`K`, tổng hợp các khoản vay lặp đi lặp lại và số dư lớn | 

## Vỏ cạnh 

Cặp đối diện trực tiếp là nhóm có tổng bằng 0 đơn giản nhất. Vì```
2 1
1 0 1
```số dư sau khi tổng hợp là`[-1, +1]`. Mặt nạ đầy đủ có tổng bằng 0, vì vậy`dp[full] = 1`. Tái thiết chọn cả hai người vào một nhóm. Giai đoạn giải quyết tìm ra con nợ`0`và chủ nợ`1`, chuyển một đơn vị và cả hai số dư đều bằng 0. Đầu ra là```
1
0 1 1
```Người có số dư bằng 0 không được tạo giao dịch giả mạo. Vì```
3 2
0 1 5
1 0 5
```hai khoản vay hủy bỏ chính xác, để lại số dư`[0, 0, 0]`. Bước lọc sẽ loại bỏ từng người, vì vậy`m = 0`và thuật toán ngay lập tức xuất ra```
0
```Đây cũng là lý do tại sao DP nên được xây dựng từ số dư khác 0 thay vì từ tất cả`N`mọi người. 

Các thành phần có tổng bằng 0 độc lập là trường hợp trung tâm đánh bại việc so khớp tham lam tùy ý. Vì```
4 2
0 2 2
1 3 2
```số dư là`[+2, +2, -2, -2]`. Phân vùng tối ưu có hai nhóm,`{0,3}`Và`{1,2}`. Mỗi nhóm cần một khoản thanh toán nên câu trả lời cuối cùng có hai giao dịch:```
2
3 0 2
2 1 2
```Tổng số tiền được chuyển là bốn, bằng tổng số dư dương và hai khoản thanh toán là tối ưu vì mỗi cặp độc lập đều đã cần một khoản thanh toán. 

Các khoản vay lặp lại thực hiện sự phân biệt giữa quy mô đầu vào và quy mô trạng thái. Với`100000`bản sao của`0 1 1`, số dư trở thành`[-100000, +100000]`. DP vẫn chỉ có bốn chiếc mặt nạ vì chỉ có hai người khác không. Tái thiết tạo ra một nhóm và giai đoạn thanh toán sẽ thực hiện một khoản thanh toán`100000`:```
1
0 1 100000
```Bản gốc`100000`các khoản vay được xử lý một lần, nhưng không có khoản vay nào tạo ra trạng thái DP riêng biệt. 

Trường hợp ranh giới cuối cùng là một cặp khác 0 ẩn giữa những người có số dư bằng 0. Vì```
3 1
0 1 1
```số dư là`[+1, -1, 0]`. Người`2`bị loại bỏ, để lại trạng thái DP hai phần tử`[+1, -1]`. Kết quả thanh toán là```
1
1 0 1
```Người tham gia có số dư bằng 0 không có vai trò trong việc giải quyết và các chỉ số bitmask chỉ đề cập đến hai người còn lại, vì vậy ID người ban đầu phải được bảo quản riêng trong quá trình xây dựng lại.
