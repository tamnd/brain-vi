---
title: "CF 105192B - Đây có phải là FFT không?"
description: "Chúng ta có một đa thức $c(x)$, được mô tả bởi các hệ số của nó từ bậc 0 đến bậc $l$. Đa thức này được biết là kết quả của việc nhân hai đa thức khác $a(x)$ và $b(x)$, nhưng các thừa số ban đầu đã bị mất."
date: "2026-06-27T04:11:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105192
codeforces_index: "B"
codeforces_contest_name: "Cupertino Informatics Tournament Online Mirror"
rating: 0
weight: 105192
solve_time_s: 78
verified: false
draft: false
---

[CF 105192B - Đây có phải là FFT không?](https://codeforces.com/problemset/problem/105192/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đa thức duy nhất$c(x)$, được mô tả bởi các hệ số của nó từ độ 0 đến độ$l$. Đa thức này được biết là kết quả của việc nhân hai đa thức khác$a(x)$Và$b(x)$, nhưng các yếu tố ban đầu đã bị mất. Nhiệm vụ không phải là khôi phục chúng một cách duy nhất mà là xây dựng bất kỳ cặp đa thức nào.$a$Và$b$tích chập của nó trả về chính xác đa thức đã cho$c$. 

Định dạng đầu ra yêu cầu chúng ta in rõ ràng bậc và hệ số của cả hai đa thức được xây dựng lại. Không có yêu cầu nào về mức độ tối thiểu, tính duy nhất hoặc bất kỳ ràng buộc cấu trúc nào ngoài tính đúng đắn của phép nhân. 

Những hạn chế là lớn, với$l \le 10^5$, điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng phân tích đa thức một cách có ý nghĩa hoặc giải hệ phương trình theo hệ số. Bất kỳ nỗ lực nào để xây dựng lại một hệ số hóa “thực” sẽ liên quan đến lý luận tích chập hoặc phân rã đại số, nặng hơn nhiều so với mức cần thiết. Quan sát quan trọng là tính đúng đắn chỉ phụ thuộc vào việc kết hợp sản phẩm$c$, chứ không phải ở mức độ “thú vị” của các yếu tố. 

Trường hợp cạnh nguy hiểm nhất là giả sử rằng cả hai đa thức phải không tầm thường hoặc cần có một phép chia cân bằng nào đó. Ví dụ, người ta có thể cố gắng chia đều các hệ số hoặc phân bố độ một cách không chính xác, nhưng bài toán không bao giờ yêu cầu những ràng buộc như vậy. 

Một trường hợp đơn giản minh họa điều này: 

đầu vào:```
2
6 5 1
```Đa thức là$c(x) = 6 + 5x + x^2$. Một ý tưởng hấp dẫn nhưng không cần thiết là tìm kiếm$(x+2)(x+3)$, nhưng điều đó không bắt buộc. Bất kỳ hệ số hợp lệ nào cũng hoạt động. 

Đầu ra chính xác cũng có thể là:```
2
6 5 1
0
1
```vì nhân với đa thức không đổi$1$bảo tồn mọi thứ. 

## Phương pháp tiếp cận 

Cách giải thích ngây thơ là chúng ta phải phục hồi hai đa thức có tích chập bằng đa thức đã cho. Nếu chúng ta thử làm điều này một cách trực tiếp, chúng ta sẽ giải được các hệ số$a_i, b_j$như vậy:$$c_k = \sum_{i+j=k} a_i b_j$$Đây là một hệ phương trình phi tuyến. Ngay cả đối với mức độ vừa phải$l$, cố gắng đoán hoặc giải một hệ thống như vậy sẽ bùng nổ về mặt tổ hợp, vì mỗi hệ số đưa ra các phụ thuộc trên nhiều cặp. 

Một nỗ lực có cấu trúc hơn có thể liên quan đến việc ấn định mức độ$n$Và$m$, sau đó cố gắng xây dựng lại các hệ số lặp đi lặp lại. Tuy nhiên, mỗi bước đều phụ thuộc vào các giá trị đã chọn trước đó và nhanh chóng trở nên không xác định được. Có vô số cách phân rã hợp lệ và việc tìm kiếm một cách có nguyên tắc là không cần thiết. 

Sự đơn giản hóa chính là nhận ra rằng vấn đề không yêu cầu phân tích nhân tử không tầm thường. Chúng ta có thể tự do lựa chọn bất kỳ cặp hợp lệ nào. Điều đó ngay lập tức gợi ý một đa thức không đổi là một thừa số. Nếu chúng ta đặt$b(x) = 1$, sau đó:$$a(x) \cdot 1 = a(x)$$Vì vậy việc lựa chọn$a(x) = c(x)$đáp ứng chính xác yêu cầu. 

Điều này làm giảm toàn bộ nhiệm vụ sao chép đa thức đầu vào thành một đa thức đầu ra và in đa thức không đổi$1$như thứ hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hệ số vũ phu | Hàm mũ | Cao | Quá chậm | 
| xây dựng tầm thường$a=c, b=1$|$O(l)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng một phân rã hợp lệ một cách trực tiếp mà không cần thực hiện bất kỳ phép tích chập hay giải đại số nào. 

## Hướng dẫn thuật toán 

1. Đọc bằng cấp$l$và các hệ số của$c(x)$. Các hệ số này sẽ được sử dụng lại trực tiếp làm hệ số của$a(x)$. Điều này hiệu quả vì chúng tôi dự định sử dụng đa thức nhận dạng nhân cho$b(x)$. 
2. Đặt$a(x) = c(x)$. Chúng tôi không sửa đổi các hệ số vì bất kỳ thay đổi nào cũng sẽ yêu cầu điều chỉnh bù đắp trong$b(x)$, đó là sự phức tạp không cần thiết. 
3. Xây dựng$b(x) = 1$, là đa thức bậc 0 với một hệ số duy nhất bằng 1. Điều này được chọn vì nó là đẳng thức nhân trong phép nhân đa thức. 
4. Xuất ra mức độ và hệ số của$a(x)$. 
5. Mức đầu ra 0 cho$b(x)$, theo sau là hệ số 1. 

### Tại sao nó hoạt động 

Phép nhân đa thức với một đa thức không đổi$1$giữ nguyên mọi hệ số. Kể từ khi tích chập với$[1]$không trộn lẫn hoặc dịch chuyển các số hạng thì đa thức thu được chính xác là đa thức ban đầu. Điều này đảm bảo rằng cặp được xây dựng luôn thỏa mãn$a(x) \cdot b(x) = c(x)$, bất kể các hệ số đầu vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    l = int(input().strip())
    c = list(map(int, input().split()))
    
    # a(x) = c(x)
    print(l)
    print(*c)
    
    # b(x) = 1
    print(0)
    print(1)

if __name__ == "__main__":
    main()
```Phần đầu tiên của mã đọc trực tiếp đa thức và in nó không thay đổi dưới dạng$a(x)$. Phần thứ hai xây dựng đa thức nhận dạng$b(x)$. Không cần tính toán ngoài việc xử lý đầu vào/đầu ra, đó là lý do tại sao giải pháp này phù hợp thoải mái với các ràng buộc. 

Một điểm tinh tế là đảm bảo định dạng độ phù hợp với định nghĩa: độ$l$tương ứng với$l+1$các hệ số và bậc 0 tương ứng với một hệ số duy nhất. Trộn lẫn điều này là lỗi triển khai phổ biến nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
6 5 1
```Chúng tôi giải thích điều này như$c(x) = 6 + 5x + x^2$. 

| Bước | một(x) | b(x) | Bình luận | 
| --- | --- | --- | --- | 
| Đọc đầu vào | - | - | Lưu trữ hệ số của$c$| 
| Chỉ định một | [6, 5, 1] ​​| - | Sao chép trực tiếp | 
| Chỉ định b | [6, 5, 1] ​​| [1] | Đa thức nhận dạng | 
| Đầu ra | [6, 5, 1] ​​| [1] | Phân hủy hợp lệ | 

Đầu ra được xây dựng lại$c(x)$chính xác vì nhân với 1 bảo toàn tất cả các hệ số. 

### Ví dụ 2 

đầu vào:```
0
7
```Điều này tương ứng với một đa thức không đổi$c(x) = 7$. 

| Bước | một(x) | b(x) | Bình luận | 
| --- | --- | --- | --- | 
| Đọc đầu vào | - | - | Hệ số đơn | 
| Chỉ định một | [7] | - | Sao chép | 
| Chỉ định b | [7] | [1] | Danh tính | 
| Đầu ra | [7] | [1] | hợp lệ | 

Điều này xác nhận rằng ngay cả đa thức nhỏ nhất có thể cũng được xử lý mà không cần viết vỏ đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(l)$| Chúng tôi chỉ đọc và in các hệ số một lần | 
| Không gian |$O(1)$thêm | Bỏ qua việc lưu trữ đầu vào, không sử dụng cấu trúc bổ sung | 

Các ràng buộc cho phép lên đến$10^5$các hệ số và giải pháp chỉ thực hiện quét và xuất tuyến tính, thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    l = int(input().strip())
    c = list(map(int, input().split()))

    out = []
    out.append(str(l))
    out.append(" ".join(map(str, c)))
    out.append("0")
    out.append("1")
    return "\n".join(out)

# provided sample
assert run("2\n6 5 1\n") == "2\n6 5 1\n0\n1"

# single constant polynomial
assert run("0\n7\n") == "0\n7\n0\n1"

# negative coefficients
assert run("1\n-3 4\n") == "1\n-3 4\n0\n1"

# larger case
assert run("3\n1 2 3 4\n") == "3\n1 2 3 4\n0\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đa thức không đổi | hành vi nhận dạng | trường hợp cạnh nhỏ nhất | 
| hệ số âm | bảo quản biển hiệu | không có giả định về tính tích cực | 
| độ nhỏ > 0 | tính đúng đắn chung | cấu trúc bình thường | 
| mảng lớn hơn | độ chính xác của tỷ lệ | xử lý tuyến tính | 

## Vỏ cạnh 

Trường hợp đa thức hằng$l = 0$là đầu vào có cấu trúc khác nhau duy nhất. Đối với đầu vào:```
0
5
```bộ thuật toán$a(x) = [5]$Và$b(x) = [1]$. Tích chập của một hằng số với 1 vẫn giữ nguyên không đổi, do đó đầu ra hợp lệ mà không cần bất kỳ sự phân nhánh đặc biệt nào. 

Cho dù các giá trị hệ số có lớn hay không đều thì đa thức đồng nhất vẫn đảm bảo tính chính xác mà không cần dựa vào bất kỳ số học nào ngoài việc sao chép các giá trị.
