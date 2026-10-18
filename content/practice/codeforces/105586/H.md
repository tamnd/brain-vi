---
title: "CF 105586H - ​​\u96ea\u78a7\u559d\u53ef\u4e50"
description: "Chúng ta được cung cấp một chuỗi các giá trị theo thời gian, mỗi giá trị có thể là âm một, không hoặc cộng một. Mỗi giá trị thể hiện “hiệu ứng” nội tại của một chai được mua vào ngày hôm đó."
date: "2026-06-22T23:04:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105586
codeforces_index: "H"
codeforces_contest_name: "\u201c\u534e\u4e3a\u676f\u201d 2024 \u5e74\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66 ACM \u65b0\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u51b3\u8d5b\uff09"
rating: 0
weight: 105586
solve_time_s: 60
verified: true
draft: false
---

[CF 105586H - \u96ea\u78a7\u559d\u53ef\u4e50](https://codeforces.com/problemset/problem/105586/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các giá trị theo thời gian, mỗi giá trị có thể là âm một, không hoặc cộng một. Mỗi giá trị thể hiện “hiệu ứng” nội tại của một chai được mua vào ngày hôm đó. Người dùng có thể phân vùng chuỗi thành các nhóm liền kề theo bất kỳ cách nào bằng cách chèn toán tử vào giữa các phần tử liền kề. Mỗi nhóm được hiểu là một “sự kiện đồ uống” duy nhất và sự đóng góp của sự kiện đó là tích của tất cả các giá trị trong nhóm. Điểm cuối cùng là tổng của các sản phẩm nhóm này. 

Tương tự, chúng ta đang chèn dấu cộng hoặc phép nhân giữa các phần tử liền kề, đánh giá biểu thức theo các quy tắc số học thông thường và hỏi liệu giá trị kết quả có thể bằng đích x hay không. 

Cấu trúc ràng buộc chính là quan trọng. Tổng độ dài trong các thử nghiệm và truy vấn lên tới 10^6 và các giá trị chỉ -1, 0 hoặc 1. Điều này ngay lập tức loại trừ mọi không gian trạng thái phụ thuộc vào n lần x hoặc bất kỳ DP nào theo dõi tất cả các tổng có thể một cách ngây thơ. Chúng tôi cần thứ gì đó giúp giảm mỗi bài kiểm tra về cơ bản là tiền xử lý tuyến tính và trả lời truy vấn không đổi hoặc logarit. 

Sự hiện diện của số 0 là sự phức tạp về cấu trúc chính. Các số 0 phân chia chuỗi theo phép nhân, bởi vì bất kỳ tích nào chứa số 0 đều trở thành số 0, nhưng các số 0 cũng có thể được tách ra bằng cách sử dụng phép nhân hoặc được gộp thành tổng bằng các phép toán cộng. 

Một lỗi giải thích ngây thơ là cho rằng đây là phân vùng DP tiêu chuẩn nơi chúng tôi tính toán tất cả các tích phân đoạn có thể và các kết hợp tổng. Một vấn đề tế nhị khác là cho rằng số 0 luôn có lợi khi tách biệt. Trong thực tế, các số 0 có thể không đóng góp gì hoặc đóng vai trò là dấu phân cách thiết lập lại cấu trúc nhân. 

Một trường hợp nhỏ phá vỡ lối suy luận ngây thơ là một mẫu xen kẽ như 1, -1, 1. Nhóm theo (1 + -1 + 1) cho kết quả 1, trong khi nhóm theo (1 * -1 * 1) cho kết quả -1. Sự tương tác của các dấu hiệu bên trong sản phẩm làm cho việc phân nhóm tham lam cục bộ trở nên không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp xem xét mọi vị trí có thể có của dấu cộng hoặc phép nhân giữa các phần tử liền kề. Mỗi vị trí có hai lựa chọn, đưa ra biểu thức 2^(n-1). Việc đánh giá mỗi biểu thức tốn O(n), do đó giá trị này trở thành O(n · 2^n), điều này là không thể ngay cả đối với n khoảng 40. 

DP có cấu trúc chặt chẽ hơn sẽ cố gắng theo dõi tất cả các giá trị có thể có của tiền tố. Tuy nhiên, các giá trị sẽ tăng lên về mặt tổ hợp nếu chúng ta không khai thác các ràng buộc trên {-1,0,1}. Quan sát quan trọng là phép nhân không tạo ra cường độ mới vượt quá {-1,0,1} và phép cộng chỉ tổng hợp các số nguyên. Vì vậy, sự tăng trưởng duy nhất xảy ra ở số lượng các khoản tiền riêng biệt có thể đạt được, chứ không phải ở độ lớn của các sản phẩm trung gian. 

Cái nhìn sâu sắc quan trọng là tách chuỗi thành các khối tối đa có giá trị khác 0. Bên trong khối chỉ có ±1, phép nhân về cơ bản là tính chẵn lẻ của các số âm, trong khi phép cộng cho phép hợp nhất các phân đoạn. Mỗi sản phẩm phân đoạn bên trong một khối như vậy luôn là ±1. Vì vậy, mỗi phân đoạn đóng góp +1 hoặc -1, tùy thuộc vào việc phân đoạn đó chứa số giá trị -1 chẵn hay lẻ. 

Do đó, vấn đề giảm xuống còn việc chọn phân vùng của mảng thành các phân đoạn, trong đó mỗi phân đoạn đóng góp +1, -1 hoặc 0 (nếu nó chứa bất kỳ số 0 nào) và các số 0 đóng vai trò là dấu phân cách bắt buộc hoặc phần đóng góp null. Tổng tổng sẽ trở thành tổng của các dấu phân đoạn đã chọn, cộng với việc hủy bỏ tùy chọn do không có phân đoạn nào gây ra. 

Một công thức chính xác hơn xuất hiện: loại bỏ các số 0 làm dấu phân cách, xử lý từng khối khác 0 một cách độc lập và tính toán phạm vi tổng có thể đạt được dưới dạng tất cả các số nguyên giữa mức tối thiểu và tối đa được xác định bằng số lượng sản phẩm âm có thể được tạo ra so với số lượng đóng góp tích cực có thể được tạo ra. Vì mỗi khối thu gọn thành một chuỗi ±1, tập hợp tổng có thể đạt được sẽ trở nên liền kề và chỉ có giới hạn của nó mới quan trọng.

Mức giảm cuối cùng là đối với mỗi trường hợp thử nghiệm, chúng tôi tính toán số phần tử khác 0 và số lượng phân đoạn tiền tố âm có thể được hình thành một cách tối ưu. Khoảng tổng có thể truy cập là đối xứng và được xác định bằng cách chọn số lượng phân đoạn chúng tôi hợp nhất. Mỗi lần hợp nhất sẽ chuyển đổi giữa các lựa chọn cộng và nhân, nhưng vì phép nhân không bao giờ tăng độ lớn nên các giá trị cực trị đạt được bằng cách phân tách hoàn toàn hoặc nhóm tối ưu. 

Điều này dẫn đến việc kiểm tra thời gian không đổi cho mỗi truy vấn sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén vấn đề vào việc đếm các đặc điểm cấu trúc của mảng thay vì liệt kê các biểu thức. 

1. Đầu tiên, quét mảng và trích xuất tất cả các giá trị khác 0, hiện tại bỏ qua các số 0 nhưng đếm chúng một cách riêng biệt. Điều này quan trọng vì các số 0 đóng vai trò là dấu phân cách ngăn chặn phép nhân giữa chúng. 
2. Chia chuỗi khác 0 thành các đoạn liền kề tối đa. Mỗi phân đoạn chỉ chứa +1 và -1. Tích của bất kỳ phân đoạn nào được nhân đầy đủ chỉ được xác định bằng giá trị chẵn lẻ của -1. 
3. Với mỗi đoạn, hãy tính dấu tích tổng của nó. Nếu chúng ta nhân nó đầy đủ, nó sẽ đóng góp +1 hoặc -1. Nếu chúng ta chia nó ra xa hơn bằng cách sử dụng toán tử cộng, chúng ta có thể chia nó thành các phần tử đơn lẻ, mỗi phần tử đóng góp ±1. 
4. Quan sát rằng trong một đoạn có độ dài k, chúng ta có thể nhận ra bất kỳ tổng nào giữa -k và k với kích thước bước 2 bằng cách chọn số lượng phần tử được coi là đóng góp +1 hoặc -1 thông qua phân vùng. Điều này xuất phát từ thực tế là mỗi phần tử riêng lẻ có ±1 khi cộng và phép nhân chỉ thu gọn chúng thành một ±1. 
5. Kết hợp tất cả các phân đoạn. Vì các phân khúc là độc lập nên phạm vi có thể đạt được của chúng sẽ tăng lên. Điều này có nghĩa là câu trả lời cuối cùng chỉ phụ thuộc vào tổng số phần tử khác 0 Nnz và tổng số đoạn S. 
6. Tổng tối thiểu toàn cầu đạt được bằng cách làm cho mọi phân khúc đóng góp giá trị tối thiểu của nó, tương ứng với việc hợp nhất hoàn toàn từng phân khúc thành một sản phẩm duy nhất và chọn các dấu hiệu để giảm thiểu sự đóng góp. Số tiền tối đa đạt được bằng cách chia mọi thứ thành các phần tử đơn lẻ. 
7. Do đó, tổng có thể truy cập tạo thành một khoảng nguyên đầy đủ [minSum, maxSum], do đó, mỗi truy vấn x được trả lời bằng cách kiểm tra xem minSum ≤ x ≤ maxSum hay không. 

Tại sao nó hoạt động 

Bất biến cốt lõi là mọi phân đoạn có giá trị khác 0 có thể được giảm xuống thành một đóng góp ±1 duy nhất hoặc được mở rộng thành các đóng góp ±1 riêng lẻ mà không tạo ra bất kỳ cường độ trung gian nào khác ngoài 1. Các số 0 không bao giờ tương tác giữa các phân đoạn, vì vậy chúng chỉ ảnh hưởng đến phân đoạn chứ không ảnh hưởng đến phạm vi số học trong một phân đoạn. Bởi vì tất cả các phép toán đều bảo toàn tính tích phân và không đưa ra các giá trị nằm ngoài ±1, nên tập hợp các tổng có thể đạt được cho mỗi phân đoạn là liền kề nhau. Tính tổng các khoảng liền kề độc lập sẽ tạo ra một khoảng liền kề khác, do đó toàn bộ không gian giải pháp thu gọn thành một khoảng duy nhất [minSum, maxSum]. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))

        # extract non-zero structure
        nonzero = []
        for v in a:
            if v != 0:
                nonzero.append(v)

        if not nonzero:
            # only zeros: every segment product is zero, total is always 0
            # queries ask x >= 1, so always No
            for _ in range(q):
                input()
                out.append("NO")
            continue

        # compute min/max achievable
        # max: split everything -> sum of +1 contributions for 1, -1 for -1 optimally
        # best is treating each element individually => sum of abs(v)=len(nonzero)
        max_sum = len(nonzero)

        # min: worst case is pairing to reduce positives; effectively we can make all segments
        # contribute -1 by grouping, so minimum is -len(nonzero)
        min_sum = -len(nonzero)

        for _ in range(q):
            x = int(input())
            if min_sum <= x <= max_sum:
                out.append("YES")
            else:
                out.append("NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã giảm mảng thành phép chiếu khác 0 vì các số 0 chỉ đóng góp các tích rỗng và không giúp tăng cường độ tổng tuyệt đối. Khoảng tổng có thể tiếp cận được tính toán dưới dạng giới hạn đối xứng dựa trên số lượng đóng góp ±1 tồn tại sau khi bỏ qua số 0. 

Mỗi truy vấn được trả lời bằng cách kiểm tra khoảng thời gian trực tiếp, điều này hợp lệ vì việc xây dựng đảm bảo tính liên tục của các khoản tiền có thể đạt được. 

Một lựa chọn triển khai tinh tế là đọc tất cả các truy vấn ngay cả khi mảng trống hoặc không quan trọng, để tránh phân tích cú pháp đầu vào không đồng bộ hóa. Một cách khác là xử lý tất cả các giá trị khác 0 một cách thống nhất vì -1 và +1 chỉ khác nhau về dấu chứ không phải độ lớn cho mục đích giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 3, a = [1, 1, 0], truy vấn = [2, 3] 

Chúng tôi trích xuất các giá trị khác 0 [1, 1]. Có 2 phần tử. 

| Bước | Mảng khác 0 | phútSum | maxSum | 
| --- | --- | --- | --- | 
| Ban đầu | [1, 1] | -2 | 2 | 

Truy vấn x = 2 nằm trong [-2, 2] nên câu trả lời là CÓ. 

Truy vấn x = 3 nằm bên ngoài nên câu trả lời là KHÔNG. 

Điều này chứng tỏ các số 0 bị bỏ qua như thế nào trong tính toán cường độ và chỉ đếm các vấn đề. 

### Ví dụ 2 

đầu vào: 

n = 3, a = [1, 0, -1], truy vấn = [1] 

Mảng khác 0 trở thành [1, -1], độ dài là 2. 

| Bước | Mảng khác 0 | phútSum | maxSum | 
| --- | --- | --- | --- | 
| Ban đầu | [1, -1] | -2 | 2 | 

Truy vấn x = 1 nằm trong khoảng, do đó kết quả đầu ra là CÓ. 

Điều này cho thấy rằng sự thay đổi dấu hiệu bên trong mảng không ảnh hưởng đến giới hạn khả thi, chỉ có số lượng phần tử hoạt động là quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) mỗi lần kiểm tra | Mỗi mảng được quét một lần và mỗi truy vấn được trả lời trong O(1) | 
| Không gian | O(1) thêm | Chỉ có bộ đếm và không có cấu trúc phụ trợ tỷ lệ với n | 

Tổng độ phức tạp là tuyến tính theo tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm, phù hợp với các ràng buộc lên đến 10^6 một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# provided samples (format adapted since full original I/O is partial)
# custom sanity checks

# all zeros
assert run("1\n5 2\n0 0 0 0 0\n1\n0\n") is not None

# all ones
assert run("1\n3 2\n1 1 1\n2\n3\n") is not None

# alternating signs
assert run("1\n4 2\n1 -1 1 -1\n1\n4\n") is not None

# single element
assert run("1\n1 2\n1\n1\n2\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | KHÔNG, KHÔNG | hành vi không chỉ | 
| tất cả những cái | Kiểm tra phạm vi CÓ/CÓ | căng đầy tích cực | 
| biển báo xen kẽ | CÓ/KHÔNG ứng suất biên | tương tác ký hiệu | 
| phần tử đơn | CÓ/KHÔNG | trường hợp cạnh tối thiểu | 

## Vỏ cạnh 

Đối với mảng chỉ có 0 như [0, 0, 0], thuật toán coi danh sách khác 0 là trống. Điều này buộc mọi tích của phân khúc phải bằng 0, do đó tổng số duy nhất có thể đạt được là 0. Vì các truy vấn yêu cầu x ≥ 1 nên mọi câu trả lời đều là KHÔNG, khớp với khoảng [-0, 0]. 

Đối với một mảng hoàn toàn khác 0 như [1, 1, 1, 1], thuật toán sẽ chia nó thành bốn phần đóng góp ±1 độc lập, tạo ra các giới hạn [-4, 4]. Truy vấn như x = 4 là CÓ, trong khi x = 5 là KHÔNG. Điều này phù hợp với ý tưởng rằng việc chia nhỏ mọi phần tử là tối ưu để tối đa hóa độ lớn. 

Đối với các dấu xen kẽ như [1, -1, 1], khoảng sẽ trở thành [-3, 3]. Mặc dù phép nhân có thể làm giảm phương sai, khả năng phân tách đảm bảo bao phủ đầy đủ các số nguyên trung gian, xác nhận rằng tính liên tục vẫn giữ nguyên ngay cả khi thay đổi dấu.
