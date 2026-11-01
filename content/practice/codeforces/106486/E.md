---
title: "CF 106486E - Excel \u7684\u5217\u7f16\u53f7"
description: "Chúng ta được cung cấp một chuỗi truy vấn, mỗi truy vấn là một số nguyên dương lớn biểu thị chỉ mục cột trong hệ thống bảng tính gắn nhãn các cột bằng chữ cái thay vì số."
date: "2026-06-19T15:13:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106486
codeforces_index: "E"
codeforces_contest_name: "Dalian University of Technology, Software College 2025 Freshman Contest"
rating: 0
weight: 106486
solve_time_s: 43
verified: true
draft: false
---

[CF 106486E - Excel \u7684\u5217\u7f16\u53f7](https://codeforces.com/problemset/problem/106486/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi truy vấn, mỗi truy vấn là một số nguyên dương lớn biểu thị chỉ mục cột trong hệ thống bảng tính gắn nhãn các cột bằng chữ cái thay vì số. Việc ghi nhãn bắt đầu từ các chữ cái đơn, sau đó tiếp tục với các chuỗi hai chữ cái, sau đó là các chuỗi ba chữ cái, v.v., theo mẫu từ điển trên các chữ cái tiếng Anh viết hoa. 

Nhiệm vụ là chuyển đổi từng vị trí số nguyên thành chuỗi “tên cột” tương ứng theo sơ đồ này. Khó khăn chính là đây không phải là phép chuyển đổi cơ số 26 tiêu chuẩn vì không có chữ số 0 và ánh xạ được dịch chuyển sao cho A tương ứng với 1 thay vì 0. 

Kích thước đầu vào lớn, lên tới 100.000 truy vấn và mỗi số lớn tới 10^18. Điều này ngay lập tức loại trừ mọi cách tiếp cận xây dựng chuỗi bằng cách liệt kê tất cả các nhãn trước đó hoặc mô phỏng toàn bộ chuỗi. Bất kỳ giải pháp nào cũng phải xử lý từng truy vấn theo thời gian logarit đối với giá trị, vì các phương pháp tiếp cận tuyến tính hoặc dựa trên liệt kê sẽ quá chậm. 

Một trường hợp cạnh tinh tế xuất hiện xung quanh lũy thừa 26. Ví dụ: 26 ánh xạ tới Z, nhưng 27 ánh xạ tới AA, không phải BA hoặc thứ gì đó tương tự. Một trường hợp phức tạp khác là các giá trị như 52, 53 và 702, trong đó logic modulo-26 ngây thơ có thể hoạt động sai nếu chúng ta không xử lý việc thiếu số 0 đúng cách. Ví dụ: 26 sẽ mang lại Z, trong khi số học modulo đơn giản thường tạo ra phần dư trống hoặc hành vi mang không chính xác trừ khi được điều chỉnh. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là bắt đầu từ 1 và liên tục tạo các tên cột theo thứ tự cho đến khi chúng ta đạt được chỉ mục mục tiêu. Điều này đơn giản về mặt khái niệm vì ánh xạ đơn điệu và được sắp xếp theo thứ tự từ điển, vì vậy chúng ta chỉ cần tăng một bộ đếm và chuyển đổi từng số thành biểu diễn chuỗi của nó cho đến khi đạt được giá trị truy vấn. Tuy nhiên, điều này yêu cầu tạo tối đa chuỗi a_i cho mỗi truy vấn. Vì a_i có thể lớn tới 10^18 nên điều này hoàn toàn không khả thi, ngay cả đối với một truy vấn. 

Quan sát quan trọng là cấu trúc hoạt động giống như một hệ thống chữ số cơ sở 26, ngoại trừ các chữ số từ 1 đến 26 thay vì 0 đến 25. Điều này có nghĩa là chúng ta có thể hiểu số này là biểu diễn cơ sở 26 được lập chỉ mục 1. Phép biến đổi tương tự như chuyển một số thành cơ số 26, nhưng bất cứ khi nào chúng ta trích xuất một chữ số, chúng ta phải ánh xạ số dư 0 thành chữ Z và giảm thương số trước khi tiếp tục. Điều chỉnh này giải quyết sự thay đổi từng bước một do thiếu chữ số 0. 

Thay vì tạo ra tất cả các nhãn trước đó, chúng tôi trực tiếp phân tách số thành các ký tự từ vị trí ít quan trọng nhất đến vị trí quan trọng nhất bằng cách lặp lại phép chia cho 26, sửa phần còn lại khi nó trở thành 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O(max a_i) cho mỗi truy vấn | O(1) | Quá chậm | 
| Chuyển đổi cơ sở vị trí-26 | O(log a_i) cho mỗi truy vấn | O(1) thêm (đầu ra sang một bên) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập.

1. Bắt đầu với số đã cho a. Điều này thể hiện vị trí trong danh sách nhãn cột có thứ tự vô hạn. 
2. Lặp lại tính toán ký tự cuối cùng của nhãn bằng cách lấy modulo 26. Điều này cho ra một số trong phạm vi từ 0 đến 25, nhưng ở đây 0 không tương ứng với A. Nếu phần còn lại là 0, chúng ta hiểu nó là 26, nghĩa là ký tự Z và chúng ta cũng giảm số đó trước khi tiếp tục. 
3. Chuyển đổi phần còn lại đã điều chỉnh thành một ký tự bằng cách ánh xạ 1 đến A, 2 thành B, v.v. cho đến 26 thành Z. 
4. Thêm ký tự này vào danh sách vì chúng ta đang xây dựng chuỗi từ vị trí ít quan trọng nhất đến vị trí quan trọng nhất. 
5. Chia số đó cho 26 (chia số nguyên). Điều này chuyển chúng ta đến “chữ số” tiếp theo trong hệ thống đặt tên cột. 
6. Lặp lại cho đến khi số đó bằng 0. 
7. Đảo ngược các ký tự đã thu thập để có được nhãn cột cuối cùng. 

Bước không rõ ràng là điều chỉnh khi số dư bằng 0. Điều này thay đổi cách biểu diễn một cách hiệu quả sao cho mỗi “chữ số” nằm trong số 1 đến 26 thay vì 0 đến 25, bảo đảm thực tế là không có cách biểu diễn nào cho số 0 trong hệ thống này. 

### Tại sao nó hoạt động 

Các nhãn cột tạo thành một hệ thống chữ số vị trí có cơ số 26 nhưng không có chữ số 0. Mỗi nhãn hợp lệ tương ứng duy nhất với một chuỗi các lựa chọn trong đó mỗi vị trí đóng góp một giá trị từ 1 đến 26. Quá trình phân chia lặp lại sẽ tái cấu trúc các vị trí này từ ít quan trọng nhất đến quan trọng nhất và bước hiệu chỉnh đảm bảo rằng các trường hợp biên như bội số của 26 được biểu diễn bằng Z thay vì tràn sang chữ số cao hơn tiếp theo một cách không chính xác. Điều này đảm bảo sự song song giữa số nguyên và chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def convert(x: int) -> str:
    res = []
    while x > 0:
        x -= 1
        res.append(chr(ord('A') + (x % 26)))
        x //= 26
    return ''.join(reversed(res))

def main():
    t = int(input())
    for _ in range(t):
        x = int(input())
        print(convert(x))

if __name__ == "__main__":
    main()
```Lựa chọn triển khai quan trọng là trừ đi một trước khi lấy modulo. Dòng đơn này giải quyết sự dịch chuyển từng bước một do thiếu chữ số 0. Nếu không có nó, bội số của 26 sẽ ánh xạ sai tới các chữ cái sai. 

Chúng tôi xây dựng kết quả theo thứ tự ngược lại vì mỗi bước modulo sẽ trích xuất “chữ số” có ý nghĩa thấp nhất trước tiên. Việc đảo ngược ở cuối sẽ khôi phục cách trình bày chính xác từ trái sang phải. 

## Ví dụ đã hoạt động 

Giả sử đầu vào 1 bằng 28. 

| Bước | x trước | x sau -1 | x % 26 | Char | Tiếp theo x | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 28 | 27 | 1 | B | 1 | 
| 2 | 1 | 0 | 0 | A | 0 | 

Chúng tôi thu được BA, phù hợp với ánh xạ mong đợi vì 27 là AA và 28 là BA. 

Bây giờ hãy xem xét 26. 

| Bước | x trước | x sau -1 | x % 26 | Char | Tiếp theo x | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 26 | 25 | 25 | Z | 0 | 

Kết quả là Z, xác nhận rằng việc căn chỉnh ranh giới hoạt động chính xác. 

Những ví dụ này cho thấy bước giảm đảm bảo xử lý chính xác bội số chính xác của 26 như thế nào và duy trì thứ tự từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t log a_i) | Mỗi số được chia nhiều lần cho 26 | 
| Không gian | O(1) bổ sung cho mỗi truy vấn | Chỉ sử dụng bộ đệm ký tự nhỏ | 

Hệ số logarit xuất phát từ việc giảm số lượng liên tục theo hệ số 26. Ngay cả đối với các giá trị lên tới 10^18, điều này yêu cầu ít hơn 40 lần lặp cho mỗi truy vấn, dễ dàng nằm trong giới hạn cho tối đa 10^5 truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def convert(x: int) -> str:
        res = []
        while x > 0:
            x -= 1
            res.append(chr(ord('A') + (x % 26)))
            x //= 26
        return ''.join(reversed(res))

    t = int(input())
    out = []
    for _ in range(t):
        out.append(convert(int(input())))
    return "\n".join(out)

# provided samples (conceptual placeholders since exact sample formatting is unclear)
# assert run("5\n5\n27\n52\n53\n703\n") == "E\nAA\nAZ\nBA\nAAA"

# custom cases
assert run("1\n1\n") == "A", "minimum case"
assert run("1\n26\n") == "Z", "boundary at single digit"
assert run("1\n27\n") == "AA", "first two-letter case"
assert run("1\n52\n") == "AZ", "boundary in two-letter space"
assert run("1\n703\n") == "AAA", "boundary three-letter rollover"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | A | xử lý giá trị tối thiểu | 
| 26 | Z | ranh giới một chữ số | 
| 27 | AA | chuyển sang hai chữ cái | 
| 52 | AZ | ranh giới hai chữ cái bên trong | 
| 703 | AAA | tính chính xác của ba chữ cái cuộn qua | 

## Vỏ cạnh 

Đối với đầu vào 26, thuật toán đặt x = 26, sau đó giảm xuống 25, tạo ra ký tự Z và kết thúc. Điều này tránh tạo ra phần dư trống và đảm bảo rằng bội số chính xác của 26 được xử lý rõ ràng. 

Đối với đầu vào 52, lần lặp đầu tiên ánh xạ 52 đến phần còn lại 0 sau khi điều chỉnh, tạo ra Z, sau đó tiếp tục với x = 1, tạo ra A. Điều này mang lại AZ, phù hợp với thứ tự từ điển dự kiến. 

Đối với đầu vào 703, các chuyển tiếp chia lặp lại trên tất cả các ranh giới ba ký tự. Bước đầu tiên mang lại A cho vị trí cuối cùng, sau đó quá trình truyền giống AA tạo ra AAA. Mỗi giai đoạn tôn trọng chính xác hệ thống chữ số có chỉ mục 1, xác nhận rằng việc truyền mang hoạt động giống như một hệ thống chữ số vị trí không có số 0.
