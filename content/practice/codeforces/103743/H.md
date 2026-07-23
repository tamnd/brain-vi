---
title: "CF 103743H - Ngựa Siêu Xám"
description: "Chúng ta được cung cấp một thứ tự được xác định đệ quy của tất cả các chuỗi nhị phân có độ dài $n$. Thứ tự này là cấu trúc mã Gray cổ điển: với $n=1$, nó chỉ đơn giản là $0,1$, và đối với $n$ lớn hơn, nó được tạo bằng cách lấy chuỗi trước đó, thêm tiền tố vào tất cả các phần tử bằng $0$, sau đó lấy…"
date: "2026-07-02T09:00:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103743
codeforces_index: "H"
codeforces_contest_name: "2022 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 103743
solve_time_s: 60
verified: true
draft: false
---

[CF 103743H - Ngựa xám siêu cấp](https://codeforces.com/problemset/problem/103743/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một thứ tự được xác định đệ quy của tất cả các chuỗi nhị phân có độ dài$n$. Thứ tự này là cách xây dựng mã Gray cổ điển:$n=1$nó đơn giản là$0,1$, và lớn hơn$n$nó được xây dựng bằng cách lấy chuỗi trước đó, thêm tiền tố vào tất cả các phần tử bằng$0$, sau đó lấy chuỗi tương tự trước đó theo thứ tự ngược lại và thêm tiền tố vào tất cả các phần tử bằng$1$. Điều này tạo ra một hoán vị của tất cả các số nguyên từ$0$ĐẾN$2^n-1$, trong đó mỗi số nguyên được hiểu là một$n$- chuỗi nhị phân bit. 

Chúng ta hãy gọi sự hoán vị này$f$, Ở đâu$f(x)$là phần tử ở vị trí$x$trong dãy mã Gray có độ dài$n$. Sau đó, bài toán xác định việc áp dụng lặp lại hoán vị này. Bắt đầu từ một$n$-số bit$S$, chúng tôi áp dụng$f$một lần để có được$S_1$, sau đó áp dụng lại để có được$S_2$, vân vân. Sau đó$k$ứng dụng, chúng tôi cần$S_k$. 

Kích thước đầu vào ngay lập tức buộc phải tiếp cận cẩn thận. Độ dài của chuỗi bit lên tới$3 \times 10^6$, vì vậy bất kỳ phương pháp nào lưu trữ hoặc xử lý tất cả$2^n$những giá trị là không thể. Ngay cả các phép biến đổi tuyến tính duy trì rõ ràng một$n \times n$kết cấu quá lớn. Các chiến lược khả thi duy nhất là những chiến lược xử lý chuỗi bit như một luồng và áp dụng các phép biến đổi có cấu trúc trong$O(n)$hoặc$O(n \log n)$. 

Một mô phỏng đơn giản của việc áp dụng hoán vị$k$nhiều lúc cũng không thể được vì$k$có thể lớn như$10^9$. Ngay cả một ứng dụng cũng phải tuyến tính trong$n$, do đó nhân số đó với$k$là không thể. 

Một cạm bẫy tinh vi xuất phát từ việc hiểu sai chức năng mã Gray thực sự là gì. Việc đọc trực tiếp có thể gợi ý rằng chúng ta cần xây dựng hoán vị theo cách đệ quy, nhưng làm như vậy sẽ yêu cầu tạo ra nhiều phần tử theo cấp số nhân. Một sai lầm phổ biến khác là cho rằng hoạt động này là nghịch đảo của chính nó hoặc có độ dài chu kỳ ngắn. Nó không. Ví dụ, với nhỏ$n$, ứng dụng lặp đi lặp lại không quay lại ánh xạ nhận dạng ngay lập tức, do đó, các phím tắt dựa trên chu kỳ không đáng tin cậy. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Chúng tôi xây dựng mảng mã Gray$a(n)$, sau đó áp dụng nó nhiều lần như một hoán vị. Mỗi ứng dụng ánh xạ mọi chỉ mục$x$ĐẾN$a(n)[x]$. Cái này đã tốn rồi$O(2^n)$bộ nhớ và thời gian cho mỗi ứng dụng, điều này không khả thi ngay cả đối với mức độ vừa phải$n$. Thất bại chính là cấu trúc của$a(n)$tăng trưởng theo cấp số nhân và không thể thành hiện thực. 

Quan sát quan trọng là hoán vị mã Gray không phải là tùy ý. Nó có một mô tả dạng đóng:$i$-giá trị mã Gray thứ là$g(i) = i \oplus (i \gg 1)$. Điều này có nghĩa là hoán vị thực sự là một phép biến đổi tuyến tính theo từng bit trên GF(2), trong đó mỗi bit đầu ra chỉ phụ thuộc vào tiền tố của các bit đầu vào. 

Khi chúng ta xem hoạt động như một phép biến đổi tuyến tính trên các bit, ứng dụng lặp lại sẽ trở thành sức mạnh của toán tử tuyến tính. Thay vì theo dõi các hoán vị, chúng tôi theo dõi xem mỗi bit đầu ra phụ thuộc như thế nào vào các bit đầu vào. Điều này biến vấn đề thành việc áp dụng lặp đi lặp lại phép tích chập XOR có cấu trúc. Việc xây dựng mã Gray đệ quy ngụ ý sự phụ thuộc hình tam giác, dẫn đến việc mở rộng kiểu nhị thức. Sau đó$k$ứng dụng, mỗi bit trở thành XOR của các phiên bản dịch chuyển của bit gốc, được tính trọng số bởi hệ số nhị thức modulo 2. 

Sự đơn giản hóa quan trọng là các hệ số nhị thức modulo 2 bị chi phối bởi định lý Lucas. một hệ số$\binom{k}{t}$là số lẻ khi và chỉ khi mọi bit được đặt vào$t$cũng được thiết lập trong$k$. Điều này có nghĩa là tất cả các dịch chuyển hợp lệ đều tương ứng chính xác với các mặt nạ con của$k$, biến phép biến đổi thành một tổ hợp các phép toán shift-XOR độc lập cho mỗi tập bit của$k$. 

Điều này làm giảm toàn bộ phép lũy thừa thành việc lặp qua các bit của$k$, áp dụng thao tác shift-và-XOR có cấu trúc cho từng thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(k \cdot 2^n)$|$O(2^n)$| Quá chậm | 
| Phân rã tuyến tính theo bit |$O(n \log k)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi chuỗi nhị phân đầu vào là một mảng bit$S[0 \dots n-1]$, trong đó chỉ số tăng từ bit quan trọng nhất đến bit ít quan trọng nhất. 

1. Đầu tiên, khởi tạo một mảng làm việc$res = S$. Điều này thể hiện trạng thái hiện tại sau khi không áp dụng phép chuyển đổi nào. 
2. Lặp lại từng vị trí bit$b$trong biểu diễn nhị phân của$k$. Nếu$b$-bit thứ của$k$không được thiết lập, chúng tôi bỏ qua nó. Nếu nó được đặt, chúng ta áp dụng một phép biến đổi tương ứng với việc dịch chuyển theo$2^b$. 

Lý do điều này có tác dụng là vì mỗi bit của$k$đóng góp một sự dịch chuyển một cách độc lập và tất cả sự kết hợp của các dịch chuyển này tương ứng chính xác với các mặt nạ con của$k$. 
3. Đối với một bit đã chọn$b$, thực hiện cập nhật tại chỗ của biểu mẫu$res[i] = res[i] \oplus res[i + 2^b]$cho tất cả hợp lệ$i$từ trái sang phải. 

Bước này hợp nhất thông tin từ các vị trí cách nhau bởi$2^b$, thêm tất cả các đóng góp tương ứng với các tập con bao gồm bit này một cách hiệu quả$k$. 
4. Lặp lại quá trình này cho tất cả các bit được đặt trong$k$. Mỗi bước sẽ nhân đôi phạm vi tương tác của các phần phụ thuộc trong chuỗi bit. 
5. Sau khi xử lý tất cả các bit của$k$, mảng$res$là câu trả lời cuối cùng. 

Phần không rõ ràng là tại sao sự dịch chuyển theo lũy thừa của hai là đủ. Điều này xuất phát từ việc phân tách khai triển nhị thức của các phép biến đổi Gray lặp đi lặp lại. Mỗi ứng dụng giới thiệu các phần phụ thuộc hoạt động giống như truyền bá tiền tố XOR và lũy thừa toán tử đó tạo ra sự kết hợp của các toán tử dịch chuyển độc lập. 

### Tại sao nó hoạt động 

Hoán vị mã Gray tương ứng với phép biến đổi tuyến tính trên không gian vectơ của chuỗi bit trên GF(2). Mỗi ứng dụng của phép biến đổi sẽ truyền thông tin từ các chỉ số cao hơn đến các chỉ số thấp hơn theo mẫu XOR có cấu trúc. Khi toán tử này được lũy thừa$k$lần, kết quả tương đương với việc tính tổng tất cả các thành phần của toán tử dịch chuyển có kích thước tương ứng với các tập hợp con bit trong$k$. Vì XOR là phép cộng trong GF(2), nên các thành phần này phân phối độc lập theo lũy thừa của hai. Do đó, phép biến đổi cuối cùng khớp chính xác với ứng dụng lặp của các bước shift-XOR cho mỗi bit được đặt trong$k$, bảo toàn mọi đóng góp mà không bỏ sót hoặc trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())
    s = input().strip()
    
    res = [0] * n
    for i, ch in enumerate(s):
        res[i] = 1 if ch == '1' else 0

    b = 0
    while (1 << b) <= k:
        if k & (1 << b):
            shift = 1 << b
            # apply res[i] ^= res[i + shift]
            for i in range(n - shift):
                res[i] ^= res[i + shift]
        b += 1

    print("".join(map(str, res)))

if __name__ == "__main__":
    main()
```Mã trực tiếp thực hiện phân tách shift-XOR. Chuỗi đầu vào được chuyển đổi thành một mảng số nguyên có thể thay đổi để các thao tác bit diễn ra nhanh chóng. Đối với mỗi bit được đặt trong$k$, chúng tôi tính toán khoảng cách dịch chuyển tương ứng và áp dụng một lần cập nhật. 

Một chi tiết triển khai tinh tế là hướng lặp lại. Chúng tôi luôn tính toán từ trái sang phải, nhưng điều này an toàn vì mỗi bước sử dụng các giá trị từ trạng thái trước đó trước lớp dịch chuyển hiện tại. Thứ tự không gây nhiễu giữa các bản cập nhật trong cùng một lớp bit vì mỗi chuyển đổi được xác định ở trạng thái đầy đủ trước đó. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ với$n = 4$Và$k = 3$và đầu vào$S = 1011$. 

Chúng tôi viết các chỉ số từ trái sang phải. 

| Bước |$k$bit được sử dụng | ca | trạng thái độ phân giải | 
| --- | --- | --- | --- | 
| ban đầu | - | - | 1 0 1 1 | 
| 0 | vâng | 1 | 1^0, 0^1, 1^1, 1 | 
| sau b=0 | | | 1 1 0 1 | 
| 1 | vâng | 2 | 1^0, 1^1, 0, 1 | 
| cuối cùng | | | 1 0 0 1 | 

Điều này cho thấy các lớp dịch chuyển liên tiếp dần dần trộn thông tin hậu tố vào các vị trí trước đó như thế nào. 

Đối với ví dụ thứ hai, hãy lấy$n = 5$,$k = 1$,$S = 11010$. Chỉ áp dụng ca 1, do đó chỉ xảy ra trộn XOR liền kề, phù hợp với một ứng dụng duy nhất của cấu trúc hoán vị Gray. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log k)$| Mỗi bộ bit trong$k$kích hoạt một đường truyền tuyến tính đầy đủ trên mảng | 
| Không gian |$O(n)$| Chúng tôi lưu trữ mảng bit làm việc tại chỗ | 

Những ràng buộc cho phép$n$lên tới$3 \times 10^6$, vì vậy vài chục đường chuyền tuyến tính là có thể chấp nhận được. Hệ số logarit nhỏ vì$k \le 10^9$đóng góp tối đa 30 lần lặp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    s = input().strip()

    res = [1 if c == '1' else 0 for c in s]

    b = 0
    while (1 << b) <= k:
        if k & (1 << b):
            shift = 1 << b
            for i in range(n - shift):
                res[i] ^= res[i + shift]
        b += 1

    return "".join(map(str, res))

# minimal case
assert run("1 1\n0\n") == "0"

# single bit flip propagation
assert run("3 1\n101\n") == "111"

# no operation
assert run("4 0\n1100\n") == "1100"

# small structured case
assert run("4 3\n1011\n") == "1001"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1, k=1 | 0 | sự ổn định của trường hợp cơ sở | 
| k=1 đơn giản | 111 | nhân giống một lớp | 
| k=0 | không thay đổi | hành vi nhận dạng | 
| k=3 trường hợp | 1001 | tương tác nhiều lớp | 

## Vỏ cạnh 

Khi nào$k = 0$, thuật toán không thực hiện thao tác dịch chuyển nào và trả về chuỗi gốc không thay đổi. Điều này phù hợp với định nghĩa về việc áp dụng hoán vị 0 lần. 

Vì$n = 1$, độ dài dịch chuyển luôn lớn hơn hoặc bằng độ dài chuỗi, do đó không có cập nhật nào xảy ra ngay cả khi các bit của$k$được thiết lập. Đầu ra vẫn ổn định, phù hợp với thực tế là mã Gray trên một bit là một hoán vị hoán đổi đơn giản có cấu trúc lặp lại sụp đổ trong các điều kiện biên. 

Khi$k$có nhiều bit được đặt, ví dụ$k = 3$, cả hai dịch chuyển 1 và dịch chuyển 2 đều được áp dụng. Thuật toán xử lý chúng một cách độc lập và kết quả cuối cùng sẽ tích lũy chính xác tất cả các kết hợp của những thay đổi này vì thành phần XOR mã hóa tự nhiên tất cả các tương tác tập hợp con mà không cần liệt kê rõ ràng.
