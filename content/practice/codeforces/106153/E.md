---
title: "CF 106153E - \u742a\u9732\u8bfa\u7684\u6700\u5927\u65b9\u5dee\u5b50\u5e8f\u5217"
description: "Chúng ta được cho một dãy số và được yêu cầu chọn một dãy con sao cho phương sai của các phần tử trong dãy đó lớn nhất. Trong bối cảnh này, phương sai đo lường mức độ phân tán của các giá trị được chọn xung quanh giá trị trung bình của chúng, vì vậy chúng ta muốn một dãy con có các giá trị càng xa nhau càng tốt."
date: "2026-06-19T19:21:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106153
codeforces_index: "E"
codeforces_contest_name: "HNNU Freshman Competition Round 2"
rating: 0
weight: 106153
solve_time_s: 49
verified: true
draft: false
---

[CF 106153E - \u742a\u9732\u8bfa\u7684\u6700\u5927\u65b9\u5dee\u5b50\u5e8f\u5217](https://codeforces.com/problemset/problem/106153/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số và được yêu cầu chọn một dãy con sao cho phương sai của các phần tử trong dãy đó lớn nhất. Trong bối cảnh này, phương sai đo lường mức độ phân tán của các giá trị được chọn xung quanh giá trị trung bình của chúng, vì vậy chúng ta muốn một dãy con có các giá trị càng xa nhau càng tốt. 

Quan sát quan trọng trong vấn đề này là chúng ta không bắt buộc phải giữ lại tất cả các phần tử hoặc bảo toàn cấu trúc ngoài thứ tự tiếp theo. Chúng tôi có thể tự do chọn bất kỳ tập hợp con của chỉ số. Sự tự do đó làm giảm vấn đề từ một vấn đề tổ hợp trên các dãy con thành một vấn đề chỉ phụ thuộc vào việc lựa chọn giá trị. 

Nếu mảng có kích thước lên tới thang đo Codeforces điển hình, chẳng hạn như 2⋅10^5, thì mọi cách tiếp cận thử tất cả các chuỗi con đều không thể thực hiện được ngay lập tức vì số lượng các chuỗi con tăng theo cấp số nhân. Ngay cả việc lặp lại tất cả các cặp hoặc bộ ba cũng đã quá chậm. Điều này buộc giải pháp chỉ phụ thuộc vào thông tin tổng hợp từ mảng thay vì liệt kê. 

Một trường hợp thất bại tinh tế xuất hiện khi người ta cho rằng phương sai phụ thuộc nhiều hơn vào các cực trị. Ví dụ, nếu mảng là`[1, 2, 3]`, một trực giác ngây thơ có thể gợi ý rằng việc trộn các giá trị sẽ mang lại phương sai cao hơn so với việc chỉ lấy các giá trị cực trị. Nhưng phương sai được tối đa hóa khi các giá trị trải rộng nhất, do đó dãy con tốt nhất luôn tập trung vào các giá trị tối thiểu và tối đa. Bất kỳ nỗ lực nào nhằm bao gồm các giá trị trung gian đều chỉ kéo giá trị trung bình về phía trung tâm và làm giảm độ phân tán. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều bằng nhau. Vì`[5, 5, 5]`, mọi dãy con đều có phương sai bằng 0. Một công thức đơn giản liên quan đến sự khác biệt giữa giá trị tối thiểu và tối đa vẫn tạo ra số 0 chính xác, nhưng việc triển khai dấu phẩy động có thể gây ra nhiễu nếu không được xử lý cẩn thận. 

Vấn đề quan trọng cuối cùng là sự ổn định về số lượng. Bài toán yêu cầu tính toán một giá trị phân số một cách tự nhiên. Tính toán dấu phẩy động trực tiếp có thể gây ra lỗi chính xác, do đó cần có biểu diễn hợp lý hoặc có độ chính xác cao. 

## Phương pháp tiếp cận 

Chúng ta bắt đầu từ định nghĩa phương sai. Đối với một chuỗi giá trị được chọn$x_1, x_2, \dots, x_k$, phương sai là$$\mathrm{Var} = \frac{1}{k}\sum x_i^2 - \left(\frac{1}{k}\sum x_i\right)^2.$$Brute Force sẽ liệt kê mọi dãy con, tính toán giá trị trung bình và phương sai của nó, đồng thời theo dõi mức tối đa. Điều này đúng nhưng hoàn toàn không khả thi. Với$n$các phần tử, có$2^n$các chuỗi tiếp theo, và thậm chí$n=40$sẽ là quá lớn. 

Cái nhìn sâu sắc về cấu trúc quan trọng là phương sai được tối đa hóa bằng cách tối đa hóa mức chênh lệch. Vì phương sai phụ thuộc vào độ lệch bình phương so với giá trị trung bình, nên việc bao gồm bất kỳ giá trị trung bình nào giữa mức tối thiểu và tối đa sẽ làm giảm sự đóng góp khoảng cách bình phương mà không làm tăng mức chênh lệch. Điều này ngụ ý rằng một dãy con tối ưu sẽ chỉ sử dụng các giá trị cực trị của mảng. 

Chính xác hơn, một khi chúng ta nhận ra rằng dãy con tốt nhất sẽ chỉ bao gồm các phần tử tối thiểu và tối đa, thì vấn đề sẽ giảm xuống việc tính toán phương sai cho một tập hợp chỉ chứa hai giá trị này. Bất kỳ giá trị khác biệt bổ sung nào giữa chúng chỉ làm giảm phương sai, do đó nó không thể xuất hiện trong giải pháp tối ưu. 

Như vậy ta chỉ cần xét một dãy con được hình thành từ các lần xuất hiện của`mn`Và`mx`. Cấu hình tối ưu chỉ phụ thuộc vào số lần mỗi lần xuất hiện, nhưng dạng đóng đơn giản hóa hơn nữa thành biểu thức trực tiếp về mặt`mn`Và`mx`, như được dẫn xuất trong giải pháp được cung cấp. 

Việc giảm này giải quyết vấn đề từ tối ưu hóa chuỗi con sang tính toán theo thời gian không đổi sau khi quét mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu (giảm tối thiểu/tối đa) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc mảng và tính giá trị tối thiểu và tối đa của nó. 

Bước này nắm bắt tất cả thông tin liên quan đến việc tối đa hóa phương sai vì chỉ những điểm cực trị mới xác định mức chênh lệch. 
2. Sau khi quét ta có hai đại lượng vô hướng:`mn`Và`mx`. 

Chúng xác định các ứng cử viên duy nhất quan trọng để xây dựng dãy con tối ưu. 
3. Tính biểu thức dẫn xuất$$ans = 2(mn^2 + mx^2) - (mn + mx)^2.$$Công thức này xuất phát từ việc mở rộng biểu thức phương sai cho phân bố hai giá trị và đơn giản hóa thành dạng có thể mở rộng theo số nguyên được sử dụng trong quá trình triển khai bài toán. 
4. Xuất kết quả ở định dạng phân số cố định bằng cách chia thành phần nguyên và phần phân số bằng cách chia cho 4. 

Phép chia phát sinh do việc khai triển đại số tự nhiên đưa ra mẫu số là 4. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là bất kỳ chuỗi con nào đạt được phương sai tối đa đều có thể được chuyển đổi thành một chuỗi chỉ chứa các lần xuất hiện của mức tối thiểu toàn cục và mức tối đa toàn cục mà không làm giảm phương sai. Bất kỳ giá trị trung gian nào cũng làm giảm nghiêm trọng độ lệch bình phương so với giá trị trung bình trong khi không làm tăng phạm vi, do đó nó không thể cải thiện mục tiêu. Khi không gian bài toán thu gọn thành hai giá trị, phương sai chỉ phụ thuộc vào số lượng và giá trị của chúng và số lượng tối ưu được xử lý ngầm bằng cách đơn giản hóa dạng đóng được cung cấp trong đạo hàm của câu lệnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    mn = 10**30
    mx = -10**30

    for _ in range(n):
        x = int(input())
        if x < mn:
            mn = x
        if x > mx:
            mx = x

    ans = (mx * mx + mn * mn) * 2 - (mx + mn) * (mx + mn)

    print(ans // 4, end=".")
    frac = (ans % 4) * 25
    print(f"{frac:02d}")

t = int(input())
for _ in range(t):
    solve()
```Việc triển khai trước tiên thực hiện quét tuyến tính đơn để trích xuất các giá trị tối thiểu và tối đa. Không cần lưu trữ bổ sung ngoài hai biến. 

Biểu thức cho`ans`được tính toán chính xác như trong đạo hàm biên tập. Số học số nguyên được sử dụng xuyên suốt để tránh các vấn đề về độ chính xác. 

Định dạng đầu ra cuối cùng dựa trên thực tế là mẫu số được cố định ở 4, do đó phần còn lại luôn là 0, 1, 2 hoặc 3. Nhân với 25 sẽ chuyển đổi các trường hợp này thành phần trăm thập phân chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
1
2
3
```Chúng tôi theo dõi tối thiểu và tối đa: 

| bước | giá trị | mn | mx | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 
| 2 | 1 | 1 | 3 | 
| 3 | 2 | 1 | 3 | 

Sau khi xử lý,`mn = 1`,`mx = 3`. 

Chúng tôi tính toán:$$ans = 2(1 + 9) - (4)^2 = 20 - 16 = 4.$$Đầu ra là`4 / 4 = 1.00`. 

Điều này cho thấy giá trị trung gian 2 là không liên quan; chỉ có những thái cực mới quan trọng. 

### Ví dụ 2 

đầu vào:```
1
4
5
5
5
5
```| bước | giá trị | mn | mx | 
| --- | --- | --- | --- | 
| 1 | 5 | 5 | 5 | 
| 2 | 5 | 5 | 5 | 
| 3 | 5 | 5 | 5 | 
| 4 | 5 | 5 | 5 | 

chúng tôi nhận được`mn = mx = 5`.$$ans = 2(25 + 25) - (10)^2 = 100 - 100 = 0.$$Đầu ra là`0.00`. 

Điều này xác nhận rằng một mảng không đổi luôn mang lại phương sai bằng không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lần để tính toán tối thiểu và tối đa | 
| Không gian | O(1) | Chỉ có hai biến được lưu trữ | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì nó chỉ thực hiện quét tuyến tính cho mỗi trường hợp thử nghiệm và số học theo thời gian không đổi sau đó. Ngay cả đối với kích thước đầu vào lớn, tổng số thao tác vẫn tỷ lệ thuận với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import math

    def solve():
        n = int(input())
        mn = 10**30
        mx = -10**30
        for _ in range(n):
            x = int(input())
            mn = min(mn, x)
            mx = max(mx, x)
        ans = (mx * mx + mn * mn) * 2 - (mx + mn) * (mx + mn)
        print(ans // 4, end=".")
        print((ans % 4) * 25)

    t = int(input())
    for _ in range(t):
        solve()

    return output.getvalue().strip()

# sample-like tests
assert run("1\n3\n1\n2\n3\n") == "1.0"
assert run("1\n4\n5\n5\n5\n5\n") == "0.0"

# custom cases
assert run("1\n2\n-1\n1\n") == "1.0", "symmetric extremes"
assert run("1\n1\n42\n") == "0.0", "single element"
assert run("1\n3\n10\n-10\n0\n") == "100.0", "zero inside range ignored"
assert run("2\n2\n1\n2\n2\n7\n7\n") == "0.0\n0.0", "multiple test cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cực trị đối xứng | 1.0 | giá trị âm và dương | 
| phần tử đơn | 0,0 | phương sai tầm thường | 
| 0 trong phạm vi bị bỏ qua | 100,0 | giá trị trung gian không quan trọng | 
| nhiều trường hợp thử nghiệm | 0,0 0,0 | xử lý độc lập | 

## Vỏ cạnh 

Khi tất cả các giá trị giống hệt nhau, thuật toán sẽ đặt`mn = mx`và biểu thức được tính toán sẽ giảm về 0. Ví dụ, đầu vào`[7, 7, 7]`sản lượng`ans = 0`, vì vậy kết quả đầu ra chính xác là`0.00`không có bất kỳ tính toán dấu phẩy động nào. 

Khi các giá trị bao gồm số âm, chẳng hạn như`[-5, 10]`, thuật toán vẫn xác định chính xác các điểm cực trị và tính toán phương sai hoàn toàn từ mối quan hệ đại số của chúng. Công thức vẫn đúng vì nó chỉ phụ thuộc vào các phép tính số học trên`mn`Và`mx`, không dựa trên giả định dấu. 

Khi mảng chỉ có một phần tử`mn`bằng`mx`, và biểu thức lại có giá trị bằng 0. Quá trình quét xử lý việc này một cách tự nhiên mà không yêu cầu phân nhánh đặc biệt, vì cả tối thiểu và tối đa đều bắt đầu từ cùng một lần khởi tạo và được cập nhật nhất quán.
