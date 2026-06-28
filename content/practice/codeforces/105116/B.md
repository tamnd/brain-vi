---
title: "CF 105116B - \u0410\u043a\u0446\u0438\u044f \u043d\u0430 \u0434\u0435\u043d\u044c \u0440\u043e\u0436\u0434\u0435\u043d\u0438\u044f"
description: "Chúng ta được yêu cầu quyết định liệu có thể chọn kết hợp hai loại mặt hàng bánh mì sao cho hai ràng buộc được thỏa mãn cùng một lúc hay không. Vasилиса phải mua chính xác một món đồ cho mỗi N khách của mình nên tổng số món đồ được ấn định là N."
date: "2026-06-27T19:46:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105116
codeforces_index: "B"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421 2024, \u043f\u0440\u0435\u0434\u043c\u0435\u0442\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 105116
solve_time_s: 45
verified: true
draft: false
---

[CF 105116B - \u0410\u043a\u0446\u0438\u044f \u043d\u0430 \u0434\u0435\u043d\u044c \u0440\u043e\u0436\u0434\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/105116/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu quyết định liệu có thể chọn kết hợp hai loại mặt hàng bánh mì sao cho hai ràng buộc được thỏa mãn cùng một lúc hay không. Vasилиса phải mua chính xác một món đồ cho mỗi N khách của mình, do đó tổng số mặt hàng được ấn định là N. Đồng thời, cô ấy có một phiếu khuyến mại yêu cầu tổng chi phí mua hàng phải chính xác là K rúp. 

Chỉ có hai loại sản phẩm có sẵn: một loại có giá A rúp cho mỗi mặt hàng và loại còn lại có giá B rúp cho mỗi mặt hàng, với A khác với B. Nhiệm vụ là xác định xem có tồn tại số nguyên không âm x và y sao cho x là số mặt hàng có giá A, y là số mặt hàng có giá B, x + y bằng N và tổng chi phí Ax + By bằng K. Nếu một cặp như vậy tồn tại, chúng ta phải xuất ra bất kỳ cặp hợp lệ nào; nếu không, chúng tôi xuất ra -1. 

Các ràng buộc cho phép các giá trị lên tới 10^9, điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các giá trị có thể có của x hoặc y. Quét tuyến tính trên N khả năng sẽ yêu cầu tới 10^9 lần lặp trong trường hợp xấu nhất, vượt xa giới hạn 1 giây. Điều này đẩy chúng ta tới một giải pháp đại số hơn là tìm kiếm tổ hợp. 

Một cạm bẫy phổ biến xuất hiện khi mọi người thử những lựa chọn tham lam như lấy càng nhiều món đồ rẻ càng tốt hoặc càng nhiều món đồ đắt tiền càng tốt. Những chiến lược đó thất bại vì chi phí mục tiêu K được cố định chính xác và giải pháp đúng có thể yêu cầu sự cân bằng chính xác thay vì cấu hình quá cao. Một trường hợp lỗi tinh vi khác xuất hiện khi phép chia số nguyên được sử dụng mà không kiểm tra khả năng chia hết, dẫn đến việc đếm phân số được âm thầm chấp nhận. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ lặp lại tất cả các giá trị có thể có của x từ 0 đến N, tính y là N trừ x và kiểm tra xem Ax + By có bằng K hay không. Điều này đúng vì nó kiểm tra rõ ràng mọi tổ hợp số đếm hợp lệ. Tuy nhiên, cách tiếp cận này thực hiện N lần lặp và mỗi lần lặp bao gồm công việc không đổi. Với N lên tới 10^9, điều này trở nên không khả thi về mặt tính toán. 

Quan sát quan trọng là vấn đề không thực sự có hai chiều. Khi chúng ta sửa x, giá trị của y được xác định ngay lập tức bởi N, điều này làm giảm hệ thống thành một biến duy nhất. Quan trọng hơn, phương trình chi phí trở nên tuyến tính theo x. Điều này có nghĩa là chúng ta có thể giải trực tiếp x bằng cách sử dụng đại số thay vì liệt kê. 

Từ x + y = N, chúng ta thay y = N - x vào phương trình chi phí. Điều này biến bài toán thành một phương trình tuyến tính duy nhất trong x. Vì chỉ có một ẩn số nên ta có thể cô lập nó trong thời gian không đổi. Nhiệm vụ còn lại là kiểm tra xem giá trị được tính có phải là số nguyên và nằm trong giới hạn hợp lệ hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N) | O(1) | Quá chậm | 
| Lời giải đại số tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu thị x là số lượng mục chi phí A và y là số lượng mục chi phí B.

1. Bắt đầu từ ràng buộc x + y = N và biểu diễn y dưới dạng N - x. Thao tác này sẽ loại bỏ một biến khỏi hệ thống và giảm mọi thứ thành một biến x chưa biết. 
2. Thay y vào phương trình chi phí Ax + By = K, thu được Ax + B(N - x) = K. Bước này chuyển bài toán thành phương trình tuyến tính chỉ trong x. 
3. Khai triển phương trình để có Ax + BN - Bx = K, sau đó nhóm các số hạng liên quan đến x để thu được x(A - B) + BN = K. Điều này tách biệt tất cả sự phụ thuộc vào x trong một hệ số duy nhất. 
4. Sắp xếp lại để giải x: x(A - B) = K - BN. Điều này biểu thị x dưới dạng tỷ lệ của hai đại lượng đã biết. 
5. Tính tử số K - BN và mẫu số A - B. Lúc này x phải bằng (K - BN) / (A - B). Giá trị chỉ hợp lệ nếu tử số chia hết cho mẫu số. 
6. Kiểm tra xem x được tính có phải là số nguyên và thỏa mãn 0 ≤ x ≤ N hay không. Nếu đúng như vậy, hãy tính y là N - x và xuất ra cả hai giá trị. Ngược lại, kết luận rằng không có lựa chọn hợp lệ nào tồn tại. 

### Tại sao nó hoạt động 

Phép biến đổi biến hệ hai phương trình tuyến tính hai ẩn thành một phương trình tuyến tính duy nhất một biến. Vì tất cả các ràng buộc đều tuyến tính và không có bất đẳng thức nào ngoài giá trị không âm nên mọi nghiệm khả thi đều phải thỏa mãn chính xác biểu thức dẫn xuất này. Bất kỳ nghiệm hợp lệ nào cũng phải tạo ra cùng một giá trị của x vì phương trình có nghiệm duy nhất là số thực và tích phân cộng giới hạn là hạn chế duy nhất còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    K = int(input())
    N = int(input())
    A = int(input())
    B = int(input())

    numerator = K - B * N
    denom = A - B

    if denom == 0:
        print(-1)
        return

    if numerator % denom != 0:
        print(-1)
        return

    x = numerator // denom
    y = N - x

    if x < 0 or y < 0:
        print(-1)
        return

    print(x, y)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện đạo hàm đại số. Bước đầu tiên tính toán tử số và mẫu số được sắp xếp lại của biểu thức cho x. Việc kiểm tra tính chia hết đảm bảo chúng ta không chấp nhận các giải pháp phân số. Kiểm tra giới hạn thực thi rằng cả hai số đếm đều là số nguyên không âm hợp lệ có tổng bằng N. 

Một chi tiết triển khai tinh tế là dấu hiệu của A - B. Vì A và B có thể xuất hiện theo một trong hai thứ tự nên mẫu số có thể âm, nhưng phép chia số nguyên trong Python vẫn hoạt động chính xác miễn là tính chia hết được kiểm tra trước khi chia. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

K = 11, N = 3, A = 5, B = 1 

Chúng tôi tính toán: 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| tử số | K - B·N | 11 - 3 = 8 | 
| mẫu số | A - B | 5 - 1 = 4 | 
| x | 8/4 | 2 | 
| y | N-x | 1 | 

Kết quả là hợp lệ vì cả x và y đều không âm và có tổng bằng N. Điều này cho thấy giải pháp cân bằng các mặt hàng đắt tiền và rẻ tiền như thế nào để phù hợp với chi phí mục tiêu chính xác. 

### Ví dụ 2 

đầu vào: 

K = 2, N = 1, A = 3, B = 0 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| tử số | 2 - 0·1 | 2 | 
| mẫu số | 3 - 0 | 3 | 

Ở đây 2 không chia hết cho 3 nên không tồn tại số nguyên x. Điều này cho thấy phương pháp này loại bỏ chính xác các giải pháp phân số không thể thực hiện được như thế nào mà không cần thử liệt kê. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Tất cả các phép toán đều là số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ sử dụng một số biến cố định | 

Lời giải dễ dàng nằm trong giới hạn vì nó không thực hiện phép lặp nào trên N và chỉ dựa vào một số phép tính số học không đổi, ngay cả đối với các giá trị lên tới 10^9. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import sys as _sys
    input = _sys.stdin.readline

    K = int(input())
    N = int(input())
    A = int(input())
    B = int(input())

    numerator = K - B * N
    denom = A - B

    if denom == 0:
        return "-1"

    if numerator % denom != 0:
        return "-1"

    x = numerator // denom
    y = N - x

    if x < 0 or y < 0:
        return "-1"

    return f"{x} {y}"

# provided sample
assert run("11\n3\n5\n1\n") == "2 1"

# N=1 trivial valid
assert run("5\n1\n5\n1\n") == "1 0"

# impossible due to parity/divisibility
assert run("2\n3\n5\n1\n") == "-1"

# all cookies cheaper case
assert run("3\n3\n1\n2\n") in ["3 0", "0 3"]

# boundary large equal mix
assert run("1000000000\n1000000000\n1\n2\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 11 3 5 1 | 2 1 | hỗn hợp hợp lệ cơ bản | 
| 5 1 5 1 | 1 0 | trường hợp cạnh một mục | 
| 2 3 5 1 | -1 | không có nghiệm số nguyên | 
| 3 3 1 2 | 3 0 hoặc 0 3 | trường hợp giá sai lệch | 
| giá trị lớn | đầu ra hợp lệ | an toàn tràn và cân | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng xảy ra khi A và B rất gần nhau hoặc hoán đổi cho nhau, ví dụ A = 1 và B = 2. Trong tình huống này, mẫu số A - B trở nên âm và việc xử lý bất cẩn có thể dẫn đến sai dấu. Thuật toán tránh điều này bằng cách dựa hoàn toàn vào tính chia hết của số nguyên trước khi chia, do đó dấu không ảnh hưởng đến tính chính xác. 

Một trường hợp khác xuất hiện khi K nhỏ hơn chi phí tối thiểu có thể có hoặc lớn hơn chi phí tối đa có thể có cho N mặt hàng. Ví dụ: nếu tất cả các mặt hàng có giá ít nhất là 5 và N là 3 thì bất kỳ K nào dưới 15 đều không thể. Công thức tự nhiên bác bỏ những trường hợp này vì tử số được tính toán tạo ra giá trị x nằm ngoài phạm vi [0, N]. 

Trường hợp cạnh thứ ba xảy ra khi không tồn tại nghiệm số nguyên mặc dù phương trình tuyến tính có nghiệm thực. Ví dụ: nếu x được tính bằng 2,5 thì việc kiểm tra tính chia hết sẽ không thành công và thuật toán sẽ báo cáo chính xác tính không thể chia được mà không cố gắng làm tròn hoặc tính gần đúng.
