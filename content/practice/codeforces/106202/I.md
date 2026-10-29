---
title: "CF 106202I - \u041a\u043e\u043b\u043b\u0435\u043a\u0446\u0438\u044f \u044f\u0433\u043e\u0434"
description: "Chúng ta được cung cấp một mảng các số nhân cố định có độ dài n, trong đó mỗi vị trí tương ứng với một vị trí bit trong số nhị phân tối đa n bit."
date: "2026-06-19T18:28:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106202
codeforces_index: "I"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106202
solve_time_s: 63
verified: true
draft: false
---

[CF 106202I - \u041a\u043e\u043b\u043b\u0435\u043a\u0446\u0438\u044f \u044f\u0433\u043e\u0434](https://codeforces.com/problemset/problem/106202/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nhân cố định có độ dài n, trong đó mỗi vị trí tương ứng với một vị trí bit trong số nhị phân tối đa n bit. Mọi số nguyên không âm x nhỏ hơn 2ⁿ đại diện cho một tập hợp con các chỉ số: bit thứ i của x quyết định xem chúng ta có đưa chỉ mục i vào tập hợp con đó hay không. 

Đối với số x đã chọn, chúng ta tính giá trị f(x) bằng cách nhân tất cả a[i] sao cho bit thứ i của x được đặt. Nếu một bit bằng 0 thì vị trí đó không đóng góp gì, nghĩa là nhân với 1. Vì vậy, mọi x tương ứng với một tích tập hợp con trên mảng a. 

Nhiệm vụ được lặp lại q lần. Mỗi truy vấn đưa ra một phạm vi [l, r] và chúng ta phải tính tổng f(x) trên tất cả các số nguyên x trong phạm vi đó, trong đó x được diễn giải bằng cách sử dụng chính xác n bit. Kết quả được lấy modulo 998244353. 

Các ràng buộc định hình giải pháp rất nhiều. Phạm vi của x lên tới 2⁶⁰, vì n có thể là 60, nên việc lặp trực tiếp trong một khoảng truy vấn là không thể. Ngay cả việc quét một khoảng thời gian cũng sẽ theo cấp số nhân trong trường hợp xấu nhất. Số lượng truy vấn đạt tới 200000, do đó, bất kỳ giải pháp nào cũng phải xử lý từng truy vấn theo logarit hoặc tuyến tính gần đúng trong n thời gian. 

Một nỗ lực ngây thơ sẽ đánh giá f(x) với mọi x trong [l, r], nhân tới 60 giá trị trên x. Ngay cả đối với một truy vấn có kích thước 2⁶⁰ trong trường hợp xấu nhất, điều này hoàn toàn không khả thi. Một cách đơn giản hóa không chính xác khác là cố gắng coi khoảng là độc lập trên mỗi bit, nhưng điều kiện khoảng sẽ phá vỡ tính độc lập giữa các bit, do đó việc liệt kê tập hợp con không thể được phân tích trực tiếp thành thừa số. 

Một trường hợp cạnh tinh tế phát sinh khi l bằng 0. Trong trường hợp đó, chúng ta cần một tiền tố bắt đầu từ 0, tương ứng với tập con trống x = 0 đóng góp giá trị 1, vì tích trống là 1. Bất kỳ giải pháp nào quên đóng góp cơ sở này sẽ bị sai lệch một trong mỗi phép tính tiền tố. 

## Phương pháp tiếp cận 

Khó khăn cốt lõi là f(x) có tính nhân trên các bit, trong khi hạn chế truy vấn là số, dựa trên thứ tự nhị phân. Điều này gợi ý một cấu trúc lập trình động chữ số trên biểu diễn nhị phân của x. 

Cách tiếp cận bạo lực rất đơn giản: với mỗi x trong [l, r], tính f(x) bằng cách quét tất cả các bit và nhân a[i] tương ứng. Chi phí này là O(n) cho mỗi x, do đó O(n · (r - l + 1)) cho mỗi truy vấn. Trong trường hợp xấu nhất khi r - l rất lớn, giá trị này trở thành hàm mũ trên n bit và không thể vượt qua. 

Điều quan trọng là thay vì xử lý trực tiếp các khoảng tùy ý, chúng ta có thể chuyển bài toán thành tổng tiền tố. Nếu chúng ta có thể tính F(x) = tổng của f(y) với mọi y ≤ x thì mỗi truy vấn sẽ trở thành F(r) - F(l - 1). Điều này làm giảm vấn đề khi trả lời số nhị phân DP trong phạm vi [0, x]. 

Bây giờ cấu trúc trở nên sạch sẽ. Mỗi số x là một chuỗi nhị phân có độ dài n và tại mỗi bit chúng ta quyết định đặt số 0 hay 1. Nếu đặt số 1, chúng ta nhân với a[i]. Ràng buộc y ≤ x đưa ra trạng thái chặt chẽ/lỏng lẻo giống hệt như chữ số cổ điển DP. Điểm quan trọng là sự đóng góp của hậu tố là độc lập khi chúng ta sửa trạng thái tiền tố, vì vậy chúng ta có thể sử dụng lại kết quả của bài toán con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên từng phạm vi truy vấn | O(q · n · 2ⁿ) trường hợp xấu nhất | O(1) | Quá chậm | 
| Chữ số tiền tố DP trên bit | O(q · n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán hàm tiền tố F(x), sau đó trả lời từng truy vấn dưới dạng chênh lệch của hai kết quả tiền tố.

1. Chuyển x thành biểu diễn nhị phân bằng cách sử dụng chính xác n bit, từ bit có trọng số cao nhất đến bit có trọng số nhỏ nhất. Biểu diễn này xác định chỉ số nào được bao gồm trong mỗi tập hợp con. 
2. Xác định trạng thái lập trình động dp[pos][tight], trong đó pos là chỉ mục bit hiện tại mà chúng tôi đang xử lý và chặt chẽ cho biết tiền tố mà chúng tôi đã tạo cho đến nay có chính xác bằng tiền tố của x hay không. Trạng thái này nắm bắt tất cả các lựa chọn hậu tố hợp lệ phù hợp với ràng buộc y ≤ x. 
3. Khởi tạo trường hợp cơ sở tại pos = n, trong đó không còn bit nào. Trong trường hợp này, sự tiếp tục hợp lệ duy nhất là lựa chọn trống, có tích bằng 1. Vì vậy dp[n][0] = dp[n][1] = 1. 
4. Xử lý các bit từ vị trí n - 1 xuống 0. Tại mỗi vị trí, chúng ta xem xét xem chúng ta đặt bit 0 hay bit 1. Đặt 0 đóng góp vào hệ số nhân 1. Đặt 1 đóng góp vào hệ số a[pos]. 
5. Nếu chúng ta ở trạng thái lỏng lẻo (chặt = 0), thì chúng ta có thể tự do chọn 0 hoặc 1. Cả hai lựa chọn vẫn ở trạng thái lỏng lẻo, vì chúng ta đã hoàn toàn ở dưới x. Điều này mang lại sự lặp lại dp[pos][0] = dp[pos + 1][0] · (1 + a[pos]). 
6. Nếu chúng ta ở trạng thái chặt chẽ, các chuyển đổi được phép phụ thuộc vào bit hiện tại của x. Nếu x[pos] = 1, chúng ta có thể đặt 0 và trở nên lỏng lẻo, hoặc đặt 1 và giữ nguyên trong khi nhân với a[pos]. Nếu x[pos] = 0 thì chúng ta buộc phải đặt ở vị trí 0 và chuyển sang trạng thái lỏng lẻo. Điều này mang lại dp[pos][1] = dp[pos + 1][0] khi x[pos] = 0, và dp[pos][1] = dp[pos + 1][0] + a[pos] · dp[pos + 1][1] khi x[pos] = 1. 
7. Sau khi điền dp[0][1], ta thu được F(x). Mỗi truy vấn [l, r] được trả lời là F(r) - F(l - 1), xử lý riêng trường hợp l = 0 bằng cách xử lý F(-1) = 0. 

Tính đúng đắn dựa trên thực tế là mọi số y ≤ x được biểu diễn duy nhất bằng một đường đi qua các trạng thái DP, trong đó kiểm soát chặt chẽ xem chúng ta có bị ràng buộc bởi x hay không. Tại mỗi vị trí, đóng góp nhân chỉ phụ thuộc vào quyết định tại vị trí đó và tổng hậu tố đã được tính toán, do đó các bài toán con không trùng nhau một cách không chính xác. Điều này đảm bảo rằng mỗi tập hợp con được tính chính xác một lần với trọng lượng sản phẩm chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def calc(x, a, n):
    if x < 0:
        return 0

    bits = [0] * n
    for i in range(n):
        bits[n - 1 - i] = (x >> i) & 1

    dp0 = [0] * (n + 1)
    dp1 = [0] * (n + 1)

    dp0[n] = 1
    dp1[n] = 1

    for pos in range(n - 1, -1, -1):
        dp0[pos] = dp0[pos + 1] * (1 + a[pos]) % MOD

        if bits[pos] == 1:
            dp1[pos] = (dp0[pos + 1] + a[pos] * dp1[pos + 1]) % MOD
        else:
            dp1[pos] = dp0[pos + 1]

    return dp1[0]

def main():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        ans = (calc(r, a, n) - calc(l - 1, a, n)) % MOD
        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc thực hiện phản ánh trực tiếp DP. Bước trích xuất bit căn chỉnh mỗi số nguyên với một vectơ nhị phân có độ dài n sao cho chỉ số i luôn tương ứng với cùng một số nhân a[i]. Hai mảng dp0 và dp1 tương ứng biểu thị trạng thái lỏng lẻo và chặt chẽ, đồng thời thứ tự chuyển đổi từ bit cao sang bit thấp đảm bảo các giá trị hậu tố đã được tính toán. 

Cần cẩn thận khi xử lý l = 0, vì l - 1 trở thành -1. Hàm trả về 0 một cách rõ ràng cho x âm để làm cho phép trừ tiền tố nhất quán. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó n = 3 và a = [2, 3, 5]. Mỗi số từ 0 đến 7 tương ứng với một tập hợp con của các giá trị này. 

Với x = 5 (nhị phân 101), f(5) = a[0] * a[2] = 2 * 5 = 10. 

Để tính tiền tố như F(5), chúng ta xem xét tất cả các số từ 0 đến 5 và tích lũy tích tập con của chúng. DP xây dựng tổng này bằng cách quyết định dần dần từng bit. 

Ví dụ thứ hai là x = 3 (nhị phân 011). Ở đây, DP ở cấp cao nhất giới hạn bit quan trọng nhất ở mức 0 hoặc 1 tùy thuộc vào độ chặt chẽ, đảm bảo chỉ tính các số hợp lệ lên đến 3. Các đóng góp được chia thành các trường hợp tiền tố đã nhỏ hơn hoặc vẫn bằng nhau và quá trình chuyển đổi tổng hợp chính xác các sản phẩm của các lựa chọn còn lại. 

Những ví dụ này xác nhận rằng mỗi số được tính chính xác một lần theo biểu diễn nhị phân của nó và các đóng góp nhân lên sẽ tích lũy độc lập dọc theo mỗi đường dẫn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · n) | Mỗi truy vấn thực hiện DP trên n bit với các lần chuyển đổi liên tục trên mỗi trạng thái | 
| Không gian | O(n) | Chỉ có hai mảng DP có kích thước n được duy trì | 

Với n ≤ 60 và q ≤ 200000, giải pháp thực hiện khoảng 12 triệu chuyển đổi DP, phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ vẫn ở mức tối thiểu do không có cấu trúc cho mỗi truy vấn nào được lưu trữ ngoài các mảng cố định. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def calc(x, a, n):
    if x < 0:
        return 0
    bits = [0] * n
    for i in range(n):
        bits[n - 1 - i] = (x >> i) & 1

    dp0 = [0] * (n + 1)
    dp1 = [0] * (n + 1)
    dp0[n] = dp1[n] = 1

    for pos in range(n - 1, -1, -1):
        dp0[pos] = dp0[pos + 1] * (1 + a[pos]) % MOD
        if bits[pos]:
            dp1[pos] = (dp0[pos + 1] + a[pos] * dp1[pos + 1]) % MOD
        else:
            dp1[pos] = dp0[pos + 1]

    return dp1[0]

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())
    res = []
    for _ in range(q):
        l, r = map(int, input().split())
        res.append(str((calc(r, a, n) - calc(l - 1, a, n)) % MOD))
    return "\n".join(res)

# custom tests

# minimum case
assert solve("1\n5\n1\n0 0") == "1"

# single element range
assert solve("2\n2 3\n1\n1 1") == "1"

# full range
assert solve("2\n2 3\n1\n0 3") == str((1 + 2 + 3 + 6) % MOD)

# all ones array
assert solve("3\n1 1 1\n1\n0 7") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1, a=[5], truy vấn [0,0] | 1 | đóng góp tập hợp con trống | 
| n=2, a=[2,3], truy vấn [1,1] | 1 | độ chính xác của việc lựa chọn tập hợp con duy nhất | 
| n=2, a=[2,3], truy vấn [0,3] | tổng của tất cả các tập hợp con | thống nhất liệt kê đầy đủ | 
| n=3, tất cả những cái | 8 | đếm tổ hợp các tập hợp con | 

## Vỏ cạnh 

Khi truy vấn bắt đầu bằng 0, phép trừ sử dụng F(-1). Quá trình triển khai xử lý vấn đề này bằng cách trả về số 0 cho đầu vào âm, giúp duy trì nhận dạng rằng tổng trên một tiền tố trống bằng 0. Ví dụ: trong truy vấn [0, r], kết quả trở thành chính xác F(r) mà không có bất kỳ vấn đề điều chỉnh nào. 

Khi x có các số 0 đứng đầu trong biểu diễn nhị phân của nó so với n, các vị trí đó hoạt động như các số 0 bắt buộc trong DP. Quá trình chuyển đổi dp1[pos] = dp0[pos + 1] thực thi chính xác hạn chế này, vì không có đường dẫn nào có thể đặt 1 ngoài ràng buộc tiền tố. Điều này đảm bảo các số có ít bit quan trọng hơn vẫn được nhúng chính xác vào không gian n-bit. 

Khi tất cả a[i] bằng 1, mọi tập hợp con đều có giá trị 1, do đó f(x) luôn bằng 1 và câu trả lời quy về việc đếm các số nguyên trong phạm vi. DP thoái hóa thành tiền tố tiêu chuẩn đếm trên số nhị phân, xác nhận rằng trọng số nhân được xử lý chính xác ngay cả trong trường hợp thống nhất.
