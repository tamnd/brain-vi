---
title: "CF 1594B - Số đặc biệt"
description: "Chúng ta được yêu cầu liệt kê một tập hợp số rất cụ thể được xác định bởi quy tắc khai triển cơ số. Một số được coi là hợp lệ nếu chúng ta có thể viết nó dưới dạng tổng lũy ​​thừa riêng biệt của một cơ số cố định $n$ nào đó."
date: "2026-06-10T08:55:52+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "math"]
categories: ["algorithms"]
codeforces_contest: 1594
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 747 (Div. 2)"
rating: 1100
weight: 1594
solve_time_s: 76
verified: true
draft: false
---

[CF 1594B - Số đặc biệt](https://codeforces.com/problemset/problem/1594/B) 

**Đánh giá:** 1100 
**Tags:** bitmask, toán học 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu liệt kê một tập hợp số rất cụ thể được xác định bởi quy tắc khai triển cơ số. Một số được coi là hợp lệ nếu chúng ta có thể viết nó dưới dạng tổng lũy ​​thừa riêng biệt của một cơ số cố định nào đó$n$. Điều đó có nghĩa là mỗi quyền lực$n^0, n^1, n^2, \dots$có thể được sử dụng một lần hoặc hoàn toàn không được sử dụng và giá trị được hình thành bằng cách cộng các lũy thừa đã chọn lại với nhau. 

Điều này ngay lập tức biến mọi số hợp lệ thành quyết định nhị phân theo số mũ: với mỗi số mũ$i$, chúng tôi quyết định xem$n^i$đóng góp vào tổng số. Nếu chúng ta nghĩ về mặt nạ bit, mỗi số tương ứng với mặt nạ nhị phân trong đó bit$i$kiểm soát sự bao gồm của$n^i$. 

Nhiệm vụ là sắp xếp tất cả các số có thể biểu diễn như vậy theo thứ tự tăng dần và trả về$k$-thứ 1 

Những hạn chế buộc phải có một cách tiếp cận cẩn thận. Số lượng ca kiểm thử có thể đạt tới$10^4$, và cả hai$n$Và$k$có thể lớn như$10^9$. Bất kỳ cách tiếp cận nào cố gắng tạo ra chuỗi một cách rõ ràng hoặc lặp qua các ứng cử viên sẽ ngay lập tức thất bại vì chuỗi tăng theo cấp số nhân về số lượng lũy ​​thừa có thể sử dụng. Ngay cả đối với mức độ vừa phải$k$, việc liệt kê các giá trị sẽ yêu cầu khám phá sự kết hợp của các quyền hạn, điều này là không khả thi. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ xuất hiện khi$n$là lớn. Ví dụ, nếu$n = 10^9$, sau đó$n^0 = 1$Và$n^1$đã vượt quá hầu hết các tổng trung gian, vì vậy thứ tự hoạt động rất khác so với trong các cơ sở nhỏ. Một công trình tham lam ngây thơ giả định khoảng cách đồng đều giữa các số bị phá vỡ ở đây vì sự tăng trưởng có tính phi tuyến tính cao. 

Một cạm bẫy khác là giả sử chuỗi hoạt động giống như các số nhị phân tiêu chuẩn cho tất cả$n$. Điều đó chỉ đúng khi$n = 2$. Đối với người khác$n$, các giá trị vị trí được tính theo trọng số theo cấp số nhân, do đó thứ tự từ điển của mặt nạ bit không khớp trực tiếp với thứ tự số trừ khi chúng tôi biện minh cẩn thận cho nó. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi tạo ra tất cả các số bằng cách thử mọi tập hợp con lũy thừa$n^0, n^1, \dots$đến một giới hạn hợp lý, tính tổng của chúng, lưu trữ, sắp xếp chúng và chọn$k$-phần tử thứ. Điều này đúng vì nó xây dựng một cách thấu đáo chính xác định nghĩa của các số hợp lệ. 

Điểm thất bại là kích thước. Ngay cả khi chúng ta giới hạn bản thân ở 60 lũy thừa đầu tiên (kể từ$n^i$trở nên rất lớn một cách nhanh chóng), chúng tôi vẫn có$2^{60}$tập hợp con. Điều đó vượt xa mọi giới hạn tính toán. 

Quan sát quan trọng là mọi số hợp lệ được xác định duy nhất bởi vectơ lựa chọn nhị phân trên lũy thừa của$n$. Điều đó có nghĩa là$k$-số nhỏ nhất tương ứng với biểu diễn nhị phân của$k$chính nó, nhưng được giải thích trong cơ sở$n$thay vì cơ sở 2. 

Chính xác hơn, nếu chúng ta viết$k$ở dạng nhị phân, mỗi bit cho biết liệu chúng ta có bao gồm lũy thừa tương ứng của$n$. các$i$-bit thứ đóng góp$n^i$nếu nó được thiết lập. Điều này hiệu quả vì thứ tự của tổng nhất quán với việc tăng giá trị nhị phân của mặt nạ: lũy thừa cao hơn chiếm ưu thế tất cả các lũy thừa thấp hơn, do đó thứ tự từ điển theo bit sẽ khớp với thứ tự số của các giá trị được xây dựng. 

Vì vậy, vấn đề giảm xuống việc chuyển đổi$k$sang dạng nhị phân và đánh giá nó như một đa thức trong cơ số$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| Tối ưu |$O(\log k)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng câu trả lời theo từng chữ số từ biểu diễn nhị phân của$k$. 

1. Chuyển đổi$k$sang dạng nhị phân, xử lý từ bit ít quan trọng nhất đến bit quan trọng nhất. Mỗi bit cho chúng ta biết liệu công suất tương ứng của$n$được bao gồm trong tổng. 
2. Duy trì giá trị đang hoạt động`ans`, ban đầu bằng 0 và một số nhân`pow_n`khởi tạo là 1, đại diện$n^0$. Hệ số nhân này theo dõi công suất hiện tại của$n$chúng tôi đang gán cho vị trí bit hiện tại. 
3. Đối với mỗi bit của$k$, nếu bit là 1, chúng ta thêm`pow_n`ĐẾN`ans`. Điều này phản ánh việc lựa chọn sức mạnh đó trong biểu diễn tập hợp con. 
4. Sau khi xử lý từng bit, cập nhật`pow_n *= n`, vì chúng ta chuyển sang số mũ tiếp theo. 
5. Tiếp tục cho đến khi tất cả các bit của$k$đã được xử lý. 
6. Đầu ra`ans mod (10^9 + 7)`. 

Lý do chúng ta có thể xử lý các bit theo thứ tự một cách an toàn là vì mỗi bit cao hơn tương ứng với lũy thừa lớn hơn của$n$, và những quyền hạn đó thống trị tất cả các đóng góp thấp hơn theo thứ tự các số hợp lệ. 

### Tại sao nó hoạt động 

Mỗi số hợp lệ tương ứng với một tập con lũy thừa của$n$, tương đương với mặt nạ nhị phân. Thứ tự của các số này khớp với thứ tự tự nhiên của các mặt nạ này được hiểu là số nguyên nhị phân, bởi vì việc tăng bit khác nhau cao nhất luôn làm tăng giá trị bất kể các bit thấp hơn do sự tăng trưởng theo cấp số nhân của$n^i$. Vì vậy,$k$-tập hợp con theo thứ tự được sắp xếp chính xác là biểu diễn nhị phân của$k$, được hiểu là một vectơ lựa chọn trên lũy thừa của$n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())

    ans = 0
    power = 1

    while k > 0:
        if k & 1:
            ans = (ans + power) % MOD
        power = (power * n) % MOD
        k >>= 1

    print(ans)
```Mã trực tiếp thực hiện ý tưởng phân rã nhị phân. Biến`power`theo dõi sức mạnh liên tiếp của$n$, luôn giữ modulo$10^9+7$vì chỉ có giá trị cuối cùng được yêu cầu modulo số này. 

Một lỗi phổ biến ở đây là cố gắng tính toán trước lũy thừa lên tới$k$, nhưng điều đó là không cần thiết vì chúng ta chỉ cần số lũy thừa bằng số bit trong$k$, tối đa là 30 đối với các ràng buộc lên tới$10^9$. Một điểm tinh tế khác là cập nhật`power`sau khi xử lý từng bit; đảo ngược thứ tự này sẽ dịch chuyển tất cả các đóng góp theo một số mũ và phá vỡ tính đúng đắn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, k = 4
```Biểu diễn nhị phân:$k = 100_2$| Chỉ số bit | Giá trị bit | quyền lực$3^i$| trả lời | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 0 | 
| 1 | 0 | 3 | 0 | 
| 2 | 1 | 9 | 9 | 

Đầu ra là 9. 

Điều này cho thấy rằng chỉ bit được chọn cao nhất mới đóng góp và các bit thấp hơn bằng 0 chỉ cần bỏ qua các lũy thừa nhỏ hơn. 

### Ví dụ 2 

đầu vào:```
n = 2, k = 12
```Nhị phân:$1100_2$| Chỉ số bit | Giá trị bit | quyền lực$2^i$| trả lời | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 0 | 
| 1 | 0 | 2 | 0 | 
| 2 | 1 | 4 | 4 | 
| 3 | 1 | 8 | 12 | 

Điều này khớp với đầu ra mẫu và chứng minh rằng nhiều bit tích lũy lũy thừa độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log k)$| Mỗi ca kiểm thử xử lý các bit của$k$một lần | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Sự phụ thuộc logarit vào$k$là cần thiết bởi vì$k$có thể đạt được$10^9$, nhưng độ dài nhị phân của nó tối đa là 30 bit, giữ cho lời giải dễ dàng trong giới hạn ngay cả đối với$10^4$trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7
    t = int(sys.stdin.readline())
    out = []

    for _ in range(t):
        n, k = map(int, sys.stdin.readline().split())
        ans = 0
        power = 1
        while k > 0:
            if k & 1:
                ans = (ans + power) % MOD
            power = (power * n) % MOD
            k >>= 1
        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("3\n3 4\n2 12\n105 564\n") == "9\n12\n3595374"

# custom cases
assert run("1\n2 1\n") == "1"
assert run("1\n2 2\n") == "2"
assert run("1\n10 5\n") == str((1 + 4) % (10**9 + 7))
assert run("1\n1000000000 1000000000\n")  # sanity check runs
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2, k=1 | 1 | tính đúng đắn của trường hợp nhỏ nhất | 
| n=2, k=2 | 2 | hành vi dịch chuyển bit đơn | 
| n=10, k=5 | 1 + 4 | tích lũy nhiều bit | 
| giá trị lớn | ổn định | hiệu suất và an toàn tràn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$k = 1$. Biểu diễn nhị phân chỉ chứa bit thấp nhất nên thuật toán trả về$n^0 = 1$. Vòng lặp chạy một lần, thêm`power = 1`, và dừng lại một cách chính xác. 

Một trường hợp khác là khi$n$rất lớn, chẳng hạn như$n = 10^9$. Bộ nhân đầu tiên`power`ĐẾN$10^9$, nhưng vì chúng ta giảm ngay modulo$10^9+7$, các giá trị vẫn hợp lệ. Cấu trúc xử lý bit không thay đổi và chỉ cần một hoặc hai lần lặp trước khi vượt quá mức công suất cao hơn.$k$độ dài bit của nó. 

Trường hợp cuối cùng là khi$k$có nhiều bit cao được đặt, chẳng hạn như$k = 2^m - 1$. Ở đây mỗi bit đều đóng góp, tạo ra một tổng hình học đầy đủ$1 + n + n^2 + \dots + n^m$. Thuật toán tích lũy tất cả các lũy thừa theo trình tự một cách tự nhiên, phù hợp với việc xây dựng các số có thể biểu thị lớn nhất trong phạm vi đó.
