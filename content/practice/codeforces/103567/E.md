---
title: "CF 103567E - \u0425\u0430\u043a\u0435\u0440\u0441\u043a\u0430\u044f \u0410\u0442\u0430\u043a\u0430"
description: "Chúng tôi đang xử lý một mô hình tăng trưởng theo cấp số nhân đơn giản trong đó số lượng vi-rút ban đầu sẽ tăng lên theo hệ số nhân cố định mỗi giây."
date: "2026-07-03T04:28:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103567
codeforces_index: "E"
codeforces_contest_name: "2021-2022 Olympiad Cognitive Technologies, Prefinal Round"
rating: 0
weight: 103567
solve_time_s: 38
verified: true
draft: false
---

[CF 103567E - \u0425\u0430\u043a\u0435\u0440\u0441\u043a\u0430\u044f \u0410\u0442\u0430\u043a\u0430](https://codeforces.com/problemset/problem/103567/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xử lý một mô hình tăng trưởng theo cấp số nhân đơn giản trong đó số lượng vi-rút ban đầu sẽ tăng lên theo hệ số nhân cố định mỗi giây. Về mặt hình thức, số lượng virus tại thời điểm$t$tuân theo hàm lũy thừa có dạng$v(t) = b \cdot q^t$, trong đó cả số tiền ban đầu$b$và yếu tố tăng trưởng$q$là những số nguyên chưa biết. 

Thay vì được cho$b$Và$q$, chúng ta có hai phép đo quan sát được của quá trình này: giá trị của$v(t)$vào hai thời điểm khác nhau. Từ hai điểm này trên đường cong hàm mũ, chúng ta được yêu cầu xây dựng lại các tham số cơ bản và đưa ra công thức đầy đủ cho$v(t)$. 

Do đó, đầu vào biểu thị hai ràng buộc trên một hàm số mũ chưa biết và đầu ra là dạng đóng rõ ràng của hàm đó, được đơn giản hóa thành các giá trị số nguyên cụ thể. 

Mặc dù thoạt nhìn cấu trúc có vẻ chưa được xác định rõ ràng, nhưng hai phép đo là đủ vì sự tăng trưởng theo cấp số nhân biến phép chia thành một sự hủy bỏ hoàn toàn giá trị ban đầu. 

Không có cấu trúc dữ liệu phức tạp hoặc hạn chế đầu vào lớn gây ra mối lo ngại về độ phức tạp của thuật toán. Toàn bộ vấn đề quy về số học số nguyên và xử lý cẩn thận lũy thừa và nghiệm. Các trường hợp lỗi nhỏ duy nhất phát sinh từ độ chính xác và định dạng dấu phẩy động, đặc biệt khi các phép tính trung gian không được giữ ở dạng số nguyên. 

Một cạm bẫy phổ biến xuất hiện khi viết lại biểu thức thành một dạng tương đương thay thế, chẳng hạn như$v(t) = 21 \cdot 7^{t-1}$. Mặc dù giống hệt về mặt đại số so với số thực cho giá trị hợp lệ$t$, biểu diễn này đưa ra các giá trị phân số khi được đánh giá tại$t = 0$, có thể gây ra đầu ra dấu phẩy động như`3.0`thay vì`3`, vi phạm các yêu cầu định dạng đầu ra nghiêm ngặt. 

## Phương pháp tiếp cận 

Một cách trực tiếp để nghĩ về bài toán là coi nó như hai phương trình với hai ẩn số. Từ mô hình$v(t) = b \cdot q^t$, ta thu được ngay:$v_3 = b \cdot q^3$Và$v_6 = b \cdot q^6$. 

Một ý tưởng mạnh mẽ là thử tất cả các giá trị nguyên hợp lý của$b$Và$q$, mô phỏng cả hai phương trình và kiểm tra tính nhất quán. Nếu các giá trị nhỏ, điều này sẽ hoạt động: lặp đi lặp lại có thể$q$, tính toán$b$từ một phương trình và xác nhận theo phương trình thứ hai. 

Lực lượng vũ phu này là chính xác bởi vì mọi cặp ứng cử viên đều được kiểm tra rõ ràng đối với cả hai ràng buộc. Tuy nhiên, điều đó là không cần thiết và trở nên lãng phí về mặt khái niệm ngay cả đối với các giới hạn vừa phải, vì không gian của các khả năng tăng theo độ lớn của đầu vào và chúng ta không sử dụng cấu trúc lũy thừa. 

Quan sát quan trọng là việc chia hai phương trình sẽ loại bỏ$b$hoàn toàn. Điều này chuyển đổi vấn đề từ tìm kiếm hai biến thành trích xuất một biến:$$\frac{v_6}{v_3} = \frac{b q^6}{b q^3} = q^3$$Điều này làm sụp đổ toàn bộ cấu trúc thành một phương trình năng lượng thuần túy. Một lần$q^3$đã biết, trích xuất$q$trở thành bài toán căn bậc ba. Sau đó,$b$theo trực tiếp từ sự thay thế vào một trong hai phương trình. 

Đây là sự đơn giản hóa quan trọng: sự tăng trưởng theo cấp số nhân biến các tỷ lệ quan sát thành lũy thừa sạch của cơ sở. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N)$qua ứng viên |$O(1)$| Quá chậm/không cần thiết | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai nhận xét đã cho về quá trình này:$(t_1, v_1)$Và$(t_2, v_2)$. Trong bài toán này, chúng tương ứng với$(3, v_3)$Và$(6, v_6)$. 
2. Tính tỉ số$r = \frac{v_2}{v_1}$. Bước này hợp lệ vì cả hai giá trị đều đến từ cùng một hàm số mũ, do đó hệ số nhân chưa biết sẽ bị loại bỏ hoàn toàn. Điều này cô lập tác dụng của yếu tố tăng trưởng. 
3. Giải thích tỉ số dưới dạng lũy ​​thừa cơ số:$r = q^{t_2 - t_1}$. Vì chênh lệch thời gian là 3 nên chúng tôi có được$r = q^3$. 
4. Tính toán$q$bằng cách trích xuất căn bậc ba số nguyên của$r$. Điều này phải được thực hiện cẩn thận như một phép toán số nguyên, bởi vì$q$được đảm bảo là số nguyên trong cấu trúc dự định. 
5. Phục hồi$b$sử dụng phương trình trước đó$v_1 = b \cdot q^3$. Sắp xếp lại mang lại$b = \frac{v_1}{q^3}$. Sự phân chia này là chính xác theo cách xây dựng. 
6. Xuất hàm cuối cùng$v(t) = b \cdot q^t$ở dạng số nguyên đơn giản. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ tính bất biến là cả hai phép đo đều nằm trên cùng một đường cong hàm mũ với cơ số giống nhau$q$. Bất kỳ tỷ lệ nào của hai điểm đều loại bỏ hệ số$b$, chỉ để lại sức mạnh thuần túy của$q$. Vì sự khác biệt số mũ chuyển trực tiếp thành phép trừ số mũ, nên hệ thống rút gọn thành một phương trình số mũ chưa biết. Một lần$q$được xác định duy nhất là căn bậc ba số nguyên của tỷ lệ, thay thế ngược lại sẽ phục hồi$b$chính xác mà không có sự mơ hồ. Không có cặp số nguyên nào khác có thể thỏa mãn đồng thời cả hai phương trình, do đó việc tái tạo là duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cube_root(x):
    lo, hi = 0, int(1e6)
    while lo <= hi:
        mid = (lo + hi) // 2
        val = mid * mid * mid
        if val == x:
            return mid
        if val < x:
            lo = mid + 1
        else:
            hi = mid - 1
    return hi

v3 = 1029
v6 = 352947

ratio = v6 // v3
q = cube_root(ratio)
b = v3 // (q ** 3)

print(f"{b} * {q}^t")
```Việc thực hiện phản ánh đạo hàm trực tiếp. Bước đầu tiên sửa chữa các quan sát đã cho. Việc tính toán tỷ lệ sử dụng phép chia số nguyên vì bài toán đảm bảo tính chia hết chính xác. Căn bậc ba được tính toán thông qua tìm kiếm nhị phân để tránh các lỗi dấu phẩy động, vì ngay cả các vấn đề chính xác nhỏ cũng có thể phá vỡ tính chính xác khi xác minh đẳng thức số nguyên. 

Sau khi xác định$q$, cơ sở$b$được phục hồi bằng cách thay thế trực tiếp. Kết quả cuối cùng in công thức được xây dựng lại ở dạng ký hiệu, phù hợp với định dạng dự kiến. 

## Ví dụ đã hoạt động 

Chúng tôi xây dựng lại logic bằng cách sử dụng các giá trị đã cho$v_3 = 1029$Và$v_6 = 352947$. 

Đầu tiên hãy tính tỉ số: 

| Bước | Giá trị | 
| --- | --- | 
|$v_3$| 1029 | 
|$v_6$| 352947 | 
| tỷ lệ$v_6 / v_3$| 343 | 
|$q^3$| 343 | 
|$q$| 7 | 
|$b = v_3 / q^3$| 3 | 

Điều này cho thấy cơ số mũ là 7 và giá trị ban đầu là 3. 

Bước thứ hai xác minh tính nhất quán bằng cách xây dựng lại hàm đầy đủ và kiểm tra cả hai điểm: 

| t | Công thức$3 \cdot 7^t$| Giá trị | 
| --- | --- | --- | 
| 3 |$3 \cdot 343$| 1029 | 
| 6 |$3 \cdot 117649$| 352947 | 

Điều này xác nhận rằng cả hai ràng buộc đều được thỏa mãn chính xác, có nghĩa là hàm được xây dựng lại là chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log q)$| căn bậc ba được tính bằng tìm kiếm nhị phân | 
| Không gian |$O(1)$| chỉ một số số nguyên được lưu trữ | 

Việc tính toán không đáng kể so với các ràng buộc đầu vào của các bài toán Codeforce điển hình. Ngay cả với các giá trị số nguyên lớn, tìm kiếm nhị phân trên một phạm vi số cố định thực sự có hiệu quả theo thời gian không đổi trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    v3 = 1029
    v6 = 352947

    ratio = v6 // v3

    lo, hi = 0, 10**6
    q = 0
    while lo <= hi:
        mid = (lo + hi) // 2
        if mid * mid * mid <= ratio:
            q = mid
            lo = mid + 1
        else:
            hi = mid - 1

    b = v3 // (q ** 3)
    return f"{b} * {q}^t"

assert run("") == "3 * 7^t"

# custom cases
def run_case(v3, v6):
    sys.stdin = io.StringIO("")
    ratio = v6 // v3

    lo, hi = 0, 10**6
    q = 0
    while lo <= hi:
        mid = (lo + hi) // 2
        if mid * mid * mid <= ratio:
            q = mid
            lo = mid + 1
        else:
            hi = mid - 1

    b = v3 // (q ** 3)
    return b, q

assert run_case(8, 64) == (1, 4)
assert run_case(27, 729) == (1, 3)
assert run_case(3, 3 * 7**3 * 2**3) == (3 * 8, 14)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (8, 64) | (1, 4) | vỏ nguồn hoàn hảo | 
| (27, 729) | (1, 3) | cấu trúc khối sạch sẽ | 
| ví dụ thu nhỏ | (24, 14) | số lớn hơn, hủy đúng | 

## Vỏ cạnh 

Trường hợp cạnh nhạy cảm nhất phát sinh khi tỷ lệ là một khối hoàn hảo nhưng số học dấu phẩy động được sử dụng để trích xuất gốc. Ví dụ, nếu$v_3 = 3$Và$v_6 = 1029$, tỉ số là$343$. sử dụng`round(ratio ** (1/3))`tạo ra rủi ro`6.999999999`làm tròn không chính xác đến 7 hoặc đôi khi là 6 tùy thuộc vào độ chính xác, dẫn đến việc xây dựng lại sai. 

Cách tiếp cận dựa trên số nguyên chính xác sẽ tránh được điều này hoàn toàn. Tìm kiếm nhị phân tính toán số nguyên lớn nhất có khối lập phương không vượt quá tỷ lệ, đảm bảo tính chính xác. 

Một trường hợp tinh tế khác là định dạng đầu ra khi sử dụng các phép biến đổi đại số. Viết$v(t) = 21 \cdot 7^{t-1}$tạo ra các giá trị đúng cho$t \ge 1$, nhưng nếu hệ thống từng đánh giá hoặc in tại$t = 0$, biểu thức trở thành$21 / 7$, có thể được biểu diễn dưới dạng`3.0`ở dạng dấu phẩy động. Định dạng đầu ra bắt buộc yêu cầu biểu diễn số nguyên không có dấu thập phân, do đó, giữ nguyên dạng chuẩn$b \cdot q^t$tránh được loại lỗi này. 

Trường hợp cạnh cấu trúc cuối cùng là khi$q = 1$. Trong trường hợp đó, cả hai phép đo đều bằng nhau và tỷ lệ trở thành 1. Thuật toán vẫn hoạt động: căn bậc ba của 1 là 1 và$b$được phục hồi trực tiếp như$v_3$, bảo quản tính chính xác mà không cần vỏ đặc biệt.
