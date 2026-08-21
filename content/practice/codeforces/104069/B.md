---
title: "CF 104069B - ID trường đại học tốt nhất"
description: "Chúng ta được cung cấp một danh sách sinh viên, trong đó mỗi sinh viên có một tên và một số nguyên liên quan. Đối với mỗi số nguyên, chúng ta có thể phân tích nó thành số nguyên tố. Trong số tất cả các thừa số nguyên tố của số nguyên đó, chúng ta quan tâm đến số nguyên tố lớn nhất chia hết số nguyên đó."
date: "2026-07-02T02:58:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104069
codeforces_index: "B"
codeforces_contest_name: "VII MaratonUSP Freshman Contest"
rating: 0
weight: 104069
solve_time_s: 46
verified: true
draft: false
---

[CF 104069B - ID trường đại học tốt nhất](https://codeforces.com/problemset/problem/104069/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách sinh viên, trong đó mỗi sinh viên có một tên và một số nguyên liên quan. Đối với mỗi số nguyên, chúng ta có thể phân tích nó thành số nguyên tố. Trong số tất cả các thừa số nguyên tố của số nguyên đó, chúng ta quan tâm đến số nguyên tố lớn nhất chia hết số nguyên đó. Đối với mỗi học sinh, chúng tôi tính toán “hệ số nguyên tố tối đa” này của số của họ. Nhiệm vụ là tìm học sinh có số có giá trị lớn nhất. Nếu nhiều học sinh có cùng thừa số nguyên tố lớn nhất, chúng tôi sẽ trả về thừa số nguyên tố xuất hiện sớm nhất trong thứ tự đầu vào. 

Kích thước đầu vào lên tới 100.000 sinh viên và mỗi con số tối đa là 100 triệu. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào phân tích các số một cách độc lập bằng cách sử dụng phép chia thử đơn giản lên đến n cho mỗi truy vấn. Ngay cả O(n √x) trên mỗi số cũng sẽ dẫn đến khoảng 10^5 × 10^4 thao tác trong trường hợp xấu nhất, vốn đã là ranh giới trong Python và còn tệ hơn khi triển khai ở mức độ nặng liên tục. 

Một trường hợp phức tạp xuất phát từ các số nguyên tố. Đối với một học sinh như vậy, câu trả lời chính là con số, vì nó là thừa số nguyên tố duy nhất và do đó lớn nhất. Một trường hợp khác là các số là tích của nhiều số nguyên tố trong đó thừa số lớn nhất không nhất thiết phải là số thường xuyên nhất hoặc nhỏ nhất; ví dụ: 2 × 3 × 97 phải được đánh giá dựa trên 97. Cuối cùng, các mối quan hệ phụ thuộc chặt chẽ vào thứ tự đầu vào, vì vậy chúng ta phải duy trì hành vi xuất hiện đầu tiên. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là phân tích từng số một cách độc lập. Đối với mỗi số nguyên x, chúng tôi thử chia nó cho mọi số nguyên từ 2 đến √x và theo dõi các thừa số nguyên tố. Điều này hoạt động hợp lý vì bất kỳ số tổng hợp nào cũng phải có hệ số không lớn hơn căn bậc hai của nó. Tuy nhiên, thực hiện điều này với 100.000 số khiến trường hợp xấu nhất phải thực hiện các phép toán 100.000 × 10.000, quá chậm trong Python và vẫn lãng phí ngay cả trong các ngôn ngữ được tối ưu hóa khi hằng số lớn. 

Quan sát quan trọng là chúng ta không cần nhân tử hóa đầy đủ. Chúng ta chỉ cần ước số nguyên tố lớn nhất. Điều này cho phép một cách tiếp cận tăng dần hơn: đối với mỗi số x, chúng tôi liên tục chia số đó cho các số nguyên tố hoặc thừa số nhỏ, nhưng thay vì lưu trữ tất cả các thừa số, chúng tôi chỉ theo dõi ước số lớn nhất gặp phải trong quá trình rút gọn. Khi chúng tôi loại bỏ tất cả các thừa số nhỏ, nếu giá trị còn lại lớn hơn 1 thì giá trị còn lại đó chính là thừa số nguyên tố và tự động là ứng cử viên lớn nhất có thể. 

Điều này làm giảm vấn đề đối với việc trích xuất hệ số hiệu quả trên mỗi số với việc kết thúc sớm và theo dõi liên tục hệ số tối đa được tìm thấy cho đến nay theo thời gian liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chia thử lên tới √x mỗi số | O(n √x) | O(1) | Quá chậm | 
| Loại bỏ hệ số gia tăng với tính năng theo dõi tối đa | O(n √x) trường hợp xấu nhất, nhanh trong thực tế do rút gọn sớm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng học sinh một trong khi vẫn duy trì câu trả lời tốt nhất cho đến nay.

1. Đọc tên sinh viên và số x rồi khởi tạo biến best_factor = 1 cho sinh viên này. Biến này theo dõi thừa số nguyên tố lớn nhất được phát hiện cho x. 
2. Thử chia x cho 2 nhiều lần. Mỗi lần chia thành công, chúng ta cập nhật best_factor = 2. Chúng ta làm điều này vì nếu 2 chia x dù chỉ một lần thì đó là thừa số nguyên tố và chúng ta muốn ghi lại ngay lập tức. 
3. Sau khi loại bỏ tất cả các thừa số của 2, chúng ta lặp lại các ứng cử viên lẻ p bắt đầu từ 3 đến √x. Đối với mỗi p, chúng tôi liên tục chia x cho p khi nó chia hết và bất cứ khi nào chúng tôi làm như vậy, chúng tôi sẽ cập nhật best_factor = p. Điều này đảm bảo rằng chúng tôi nắm bắt được tất cả các thừa số nguyên tố của x và vì chúng tôi chỉ ghi đè khi một ước số hợp lệ xuất hiện nên giá trị được lưu trữ cuối cùng là giá trị lớn nhất gặp phải. 
4. Nếu sau khi xử lý tất cả các ứng cử viên x > 1, thì bản thân x là số nguyên tố lớn hơn tất cả các thừa số được trích xuất trước đó, vì vậy chúng ta đặt best_factor = max(best_factor, x). 
5. So sánh best_factor với best_so_far tối đa toàn cầu. Nếu nó lớn hơn, chúng tôi cập nhật câu trả lời cho học sinh hiện tại. Nếu nó bằng nhau, chúng ta giữ cái trước đó bằng cách không làm gì cả. 
6. Sau khi xử lý tất cả học sinh, xuất ra tên đã lưu. 

Lý do điều này có hiệu quả là vì mọi hợp số đều có ít nhất một thừa số nguyên tố không vượt quá căn bậc hai của nó, vì vậy tất cả các số nguyên tố nhỏ hơn đều được đảm bảo tìm thấy trong quá trình lặp. Sau khi loại bỏ tất cả các thừa số nhỏ, mọi giá trị còn lại phải là số nguyên tố và vì nó không thể rút gọn được nữa nên nó phải là thừa số lớn nhất của số ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_prime_factor(x: int) -> int:
    best = 1

    while x % 2 == 0:
        best = 2
        x //= 2

    p = 3
    while p * p <= x:
        while x % p == 0:
            best = p
            x //= p
        p += 2

    if x > 1:
        best = max(best, x)

    return best

def main():
    n = int(input())
    best_name = ""
    best_value = -1

    for _ in range(n):
        parts = input().split()
        name = parts[0]
        x = int(parts[1])

        val = max_prime_factor(x)

        if val > best_value:
            best_value = val
            best_name = name

    print(best_name)

if __name__ == "__main__":
    main()
```Thủ tục phân tích nhân tử trước tiên sẽ loại bỏ tất cả các thừa số của 2 để đơn giản hóa việc lặp lại sau này. Đây là một tối ưu hóa tiêu chuẩn cho phép chỉ bước qua các số lẻ sau đó, giảm một nửa số lần lặp. 

Điều kiện vòng lặp`p * p <= x`là rất quan trọng vì x co lại khi chúng ta chia nó, do đó giới hạn vẫn chính xác một cách linh hoạt. Nếu chúng tôi sử dụng giá trị ban đầu, chúng tôi sẽ lặp lại quá mức. 

Kiểm tra cuối cùng`if x > 1`nắm bắt trường hợp thừa số còn lại là số nguyên tố lớn không bao giờ bị chia. 

Vòng lặp chính chỉ lưu trữ mức tối đa toàn cầu, đảm bảo rằng các mối quan hệ sẽ tự động giải quyết theo hướng xảy ra sớm nhất. 

## Ví dụ đã hoạt động 

Xem xét đầu vào:```
4
a 12
b 35
c 97
d 18
```Chúng tôi theo dõi yếu tố tốt nhất trên mỗi học sinh: 

| Sinh viên | x | Các yếu tố được tìm thấy | hệ số nguyên tố tối đa | tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | 
| một | 12 | 2,2,3 | 3 | một | 
| b | 35 | 5,7 | 7 | b | 
| c | 97 | 97 | 97 | c | 
| d | 18 | 2,3,3 | 3 | c | 

Sau khi xử lý sẽ có câu trả lời`c`bởi vì 97 là số lớn nhất trong số tất cả các thừa số nguyên tố tối đa. 

Bây giờ hãy xem xét một trường hợp cà vạt:```
3
x 14
y 21
z 10
```| Sinh viên | x | Các yếu tố được tìm thấy | hệ số nguyên tố tối đa | tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | 
| x | 14 | 2,7 | 7 | x | 
| y | 21 | 3,7 | 7 | x | 
| z | 10 | 2,5 | 5 | x | 

Cả x và y đều có hệ số lớn nhất là 7, nhưng x xuất hiện trước nên vẫn là đáp án. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n √x) trường hợp xấu nhất | Mỗi số được phân tích thành thừa số bằng cách chia thử cho đến căn bậc hai của nó, mặc dù các giá trị sẽ co lại trong quá trình chia | 
| Không gian | O(1) | Chỉ các biến bổ sung không đổi trong mỗi lần lặp | 

Cho n lên tới 100.000 và x lên tới 10^8, giới hạn căn bậc hai là khoảng 10.000. Trong thực tế, hầu hết các số đều giảm nhanh do thừa số nguyên tố nhỏ nên thuật toán chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    n = int(sys.stdin.readline())
    best_name = ""
    best_value = -1

    def max_prime_factor(x: int) -> int:
        best = 1
        while x % 2 == 0:
            best = 2
            x //= 2
        p = 3
        while p * p <= x:
            while x % p == 0:
                best = p
                x //= p
            p += 2
        if x > 1:
            best = max(best, x)
        return best

    for _ in range(n):
        name, x = sys.stdin.readline().split()
        x = int(x)
        val = max_prime_factor(x)
        if val > best_value:
            best_value = val
            best_name = name

    return best_name

assert run("1\nalice 2\n") == "alice"
assert run("3\na 10\nb 21\nc 14\n") == "b"
assert run("2\nx 49\ny 7\n") == "x"
assert run("4\na 6\nb 15\nc 10\nd 3\n") == "b"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | Alice | ranh giới tối thiểu | 
| vật liệu tổng hợp hỗn hợp | b | trích xuất số nguyên tố tối đa chính xác | 
| lặp đi lặp lại quyền lực | x | xử lý số nguyên tố bình phương | 
| nhiều mối quan hệ và đặt hàng | b | ràng buộc theo thứ tự đầu vào | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi số đó là số nguyên tố. Ví dụ, đầu vào`alice 97`. Vòng lặp phân tích nhân tử không tìm thấy ước số nào nên sau vòng lặp x vẫn là 97. Kiểm tra cuối cùng`if x > 1`đảm bảo rằng 97 được ghi là thừa số nguyên tố tối đa và thuật toán trả về alice một cách chính xác nếu không có học sinh nào sau đó vượt quá 97. 

Một trường hợp khác là sức mạnh hoàn hảo như`49`. Bắt đầu với 49, chúng ta chia cho 7 hai lần. Mỗi bộ phận cập nhật tốt nhất thành 7. Sau khi loại bỏ, x trở thành 1, do đó giá trị cuối cùng vẫn là 7. Điều này xác nhận rằng các yếu tố lặp lại không làm sai lệch logic theo dõi tối đa. 

Trường hợp thứ ba là các số có thừa số nguyên tố rất lớn ghép với thừa số nguyên tố nhỏ, chẳng hạn như 2 × 50000017. Thuật toán nhanh chóng loại bỏ thừa số 2 và sau đó nhận giá trị còn lại là số nguyên tố, đảm bảo rằng thừa số lớn không bị bỏ sót do giới hạn phép chia thử nghiệm nhỏ.
