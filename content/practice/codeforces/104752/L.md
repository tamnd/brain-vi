---
title: "CF 104752L - Kho báu của Lucia"
description: "Chúng ta được cho một số nguyên dương $X$ và chúng ta được yêu cầu xây dựng một số thỏa mãn hai điều kiện cùng một lúc. Đầu tiên, nó phải chia hết cho $X$. Thứ hai, nó phải là một bình phương hoàn hảo, nghĩa là hệ số nguyên tố của nó có tất cả các số mũ chẵn."
date: "2026-06-28T23:00:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104752
codeforces_index: "L"
codeforces_contest_name: "Concurso de programaci\u00f3n ANIEI 2023"
rating: 0
weight: 104752
solve_time_s: 59
verified: true
draft: false
---

[CF 104752L - Kho báu của Lucia](https://codeforces.com/problemset/problem/104752/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương duy nhất$X$và chúng ta được yêu cầu xây dựng một số thỏa mãn hai điều kiện cùng một lúc. Đầu tiên nó phải chia hết cho$X$. Thứ hai, nó phải là một bình phương hoàn hảo, nghĩa là hệ số nguyên tố của nó có tất cả các số mũ chẵn. 

Một cách khác để suy nghĩ về nhiệm vụ là chúng ta được phép nhân$X$bởi một số nguyên dương nào đó$k$và chúng tôi muốn giá trị nhỏ nhất có thể của$X \cdot k$sao cho kết quả trở thành một hình vuông hoàn hảo. 

Ràng buộc$X \le 10^9$ngụ ý rằng chúng ta không thể mua được bất kỳ thuật toán nào phụ thuộc vào việc lặp qua bội số hoặc tìm kiếm các hình vuông hướng lên trên. Một lần quét ngây thơ như kiểm tra$X, 2X, 3X,\dots$và việc kiểm tra từng hình vuông sẽ quá chậm trong trường hợp xấu nhất, vì bội số bình phương nhỏ nhất có thể lớn hơn nhiều so với$X$, đặc biệt là khi$X$có nhiều thừa số nguyên tố riêng biệt. 

Một khó khăn ít rõ ràng hơn xuất hiện khi$X$không có hình vuông hoặc gần như không có hình vuông. Ví dụ, khi$X = 30$, câu trả lời là$900$, vốn đã lớn hơn 30 lần$X$. Nếu chúng ta cố gắng tìm kiếm tuyến tính, chúng ta sẽ nhanh chóng vượt quá bất kỳ giới hạn thời gian hợp lý nào. 

Cấu trúc trường hợp cạnh chính là câu trả lời không bắt nguồn từ các mẫu cộng hoặc chia hết, mà từ tính chẵn lẻ của số mũ trong hệ số nguyên tố. Bất kỳ phương pháp ngây thơ nào bỏ qua việc phân tích nhân tử sẽ thất bại trong các trường hợp như$X = 12$, đáp án ở đâu$36$, không$12$hoặc$24$và số nhân chính xác sẽ không hiển nhiên nếu không có lý luận về cấu trúc. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực bắt đầu từ$X$và kiểm tra$X \cdot 1, X \cdot 2, X \cdot 3,\dots$, kiểm tra xem mỗi giá trị có phải là một hình vuông hoàn hảo hay không. Mỗi lần kiểm tra yêu cầu tính căn bậc hai số nguyên hoặc xác minh xem bình phương của một số nguyên có bằng số đó hay không. Trong trường hợp xấu nhất, câu trả lời đúng có thể ở rất xa, do đó, điều này suy biến thành việc quét một phạm vi số nguyên lớn, dễ dàng vượt quá$10^9$hoạt động. 

Quan sát cấu trúc xuất phát từ hệ số nguyên tố. Một số là số chính phương khi và chỉ khi mọi số mũ nguyên tố trong hệ số của nó đều là số chẵn. Nếu chúng ta viết$X$là tích của các số nguyên tố có số mũ, chúng ta có thể thấy chính xác số nguyên tố nào vi phạm điều kiện này. Mỗi số nguyên tố có số mũ lẻ phải được ghép với một bản sao nữa của chính nó để tạo thành số mũ chẵn. Điều đó có nghĩa là chúng ta chỉ cần nhân$X$bởi một hệ số tối thiểu cố định tính chẵn lẻ của tất cả các số mũ. 

Điều này biến vấn đề thành một công trình xác định: nhân tử hóa$X$, tập hợp tất cả các số nguyên tố có số mũ lẻ và nhân chúng với nhau một lần. Tích đó chính xác là số nhỏ nhất biến tất cả các số mũ thành số chẵn, tạo ra bội số bình phương tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k \sqrt{X})$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Tối ưu |$O(\sqrt{X})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích nhân tử$X$bằng cách kiểm tra các ước số lên đến$\sqrt{X}$. Với mỗi số chia$p$, chia nhiều lần$X$qua$p$để đếm số mũ của nó. Điều này là cần thiết vì cấu trúc nguyên tố quyết định độ vuông góc. 
2. Duy trì kết quả đang chạy được khởi tạo thành 1. Biến này sẽ lưu trữ hệ số nhân nhỏ nhất cần thiết để sửa tính chẵn lẻ của số mũ. 
3. Với mỗi thừa số nguyên tố$p$, kiểm tra xem nó chia được bao nhiêu lần$X$. Nếu số mũ của nó là số lẻ thì nhân kết quả đó với$p$. Điều này khắc phục tính chẵn lẻ vì thêm một bản sao của$p$biến số mũ lẻ thành số chẵn. 
4. Sau khi xử lý tất cả các số nguyên tố đến$\sqrt{X}$, nếu còn lại$X$lớn hơn 1, nó là thừa số nguyên tố có số mũ 1. Đây luôn là số lẻ, vì vậy hãy nhân kết quả với số nguyên tố còn lại này. 
5. Trả lại bản gốc$X$nhân với hệ số hiệu chỉnh đã tính toán, đảm bảo là một bình phương hoàn hảo. 

Lý do đằng sau bước 4 rất quan trọng: bất kỳ số nguyên tố nào lớn hơn$\sqrt{X}$không thể xuất hiện hai lần, do đó số mũ của nó phải chính xác bằng 1. Điều đó tự động làm cho nó trở thành số lẻ. 

### Tại sao nó hoạt động 

Mỗi số nguyên có một hệ số nguyên tố duy nhất. Một số là số chính phương khi và chỉ khi tất cả các số mũ trong hệ số này đều là số chẵn. Thuật toán xây dựng hệ số nhân tối thiểu làm cho tất cả các số mũ chẵn bằng cách sửa độc lập từng số mũ lẻ chính xác một lần. Không có sự tương tác nào tồn tại giữa các số nguyên tố khác nhau, do đó việc giảm thiểu hệ số nhân sẽ làm giảm các bản sửa lỗi chẵn lẻ cục bộ. Vì chúng ta chỉ cộng mỗi thừa số nguyên tố lẻ một lần nên tích thu được là hệ số hiệu chỉnh nhỏ nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x = int(input().strip())
    original = x
    res = 1

    i = 2
    while i * i <= x:
        if x % i == 0:
            cnt = 0
            while x % i == 0:
                x //= i
                cnt += 1
            if cnt % 2 == 1:
                res *= i
        i += 1

    if x > 1:
        res *= x

    print(original * res)

if __name__ == "__main__":
    solve()
```Giải pháp dựa trên vòng lặp nhân tử phân chia thử nghiệm tiêu chuẩn. Biến`res`tích lũy chính xác các số nguyên tố xuất hiện với số lần lẻ trong quá trình phân tích nhân tử của đầu vào ban đầu. 

Một điểm tinh tế là chúng ta phải bảo toàn giá trị ban đầu của$X$trước khi nhân tử giảm nó xuống 1. Phép nhân cuối cùng sử dụng giá trị ban đầu đó, bởi vì`x`trở thành số dư giảm trong quá trình nhân tử hóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$X = 12$Chúng tôi nhân tố hóa$12 = 2^2 \cdot 3^1$. 

| Bước | Thủ tướng | Số mũ | Số lẻ? | Nhân kết quả | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | - | 1 | 
| Quy trình 2 | 2 | 2 | Không | 1 | 
| Quy trình 3 | 3 | 1 | Có | 3 | 

Giá trị cuối cùng là$12 \cdot 3 = 36$. 

Điều này cho thấy chỉ những số nguyên tố có số mũ lẻ mới đóng góp vào số nhân. 

### Ví dụ 2:$X = 16$Chúng tôi nhân tố hóa$16 = 2^4$. 

| Bước | Thủ tướng | Số mũ | Số lẻ? | Nhân kết quả | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | - | 1 | 
| Quy trình 2 | 2 | 4 | Không | 1 | 

Giá trị cuối cùng là$16 \cdot 1 = 16$. 

Điều này xác nhận rằng các hình vuông vốn đã hoàn hảo vẫn không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{X})$| phép chia thử đến căn bậc hai của X | 
| Không gian |$O(1)$| chỉ một vài số nguyên được lưu trữ | 

Sự ràng buộc$X \le 10^9$làm cho$\sqrt{X} \le 31623$, đủ nhanh trong Python trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    x = int(input().strip())
    original = x
    res = 1

    i = 2
    while i * i <= x:
        if x % i == 0:
            cnt = 0
            while x % i == 0:
                x //= i
                cnt += 1
            if cnt % 2 == 1:
                res *= i
        i += 1

    if x > 1:
        res *= x

    return str(original * res)

# provided samples
assert run("3\n") == "9", "sample 1"
assert run("16\n") == "16", "sample 2"
assert run("12\n") == "36", "sample 3"

# custom cases
assert run("1\n") == "1", "minimum case"
assert run("2\n") == "4", "prime input"
assert run("18\n") == "36", "mixed exponents"
assert run("49\n") == "49", "already square"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | ranh giới nhỏ nhất | 
| 2 | 4 | hiệu chỉnh số mũ nguyên tố đơn | 
| 18 | 36 | nhiều thừa số với số mũ lẻ | 
| 49 | 49 | sự ổn định hình vuông đã hoàn hảo | 

## Vỏ cạnh 

cho$X = 1$, vòng lặp nhân tử không làm gì cả và`res`vẫn là 1, vì vậy đầu ra là$1$. Thuật toán xử lý chính xác 1 như một hình vuông hoàn hảo. 

Đối với một số nguyên tố như$X = 2$, nhân tử hóa mang lại số mũ 1, vì vậy`res`trở thành 2 và kết quả là$2 \cdot 2 = 4$, đó là bội số bình phương nhỏ nhất. 

Đối với một số như$X = 18 = 2 \cdot 3^2$, chỉ có số nguyên tố 2 có số mũ lẻ. Thuật toán nhân với 2, tạo ra$36$và không tồn tại bội số bình phương nhỏ hơn vì mọi bình phương đều phải chứa ít nhất$2^2$. 

Đối với một hình vuông hoàn hảo như$X = 49$, tất cả các số mũ đều chẵn, vì vậy`res = 1`và đầu ra không thay đổi.
