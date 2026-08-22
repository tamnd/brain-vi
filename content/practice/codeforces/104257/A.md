---
title: "CF 104257A - Câu trả lời chấp nhận được"
description: "Chúng tôi được cung cấp nhiều truy vấn độc lập. Mỗi truy vấn chứa hai số nguyên và với mỗi cặp, chúng ta được yêu cầu xuất ra tích số học của chúng. Kích thước đầu vào có thể lớn về số lượng truy vấn, lên tới một trăm nghìn."
date: "2026-07-01T21:44:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104257
codeforces_index: "A"
codeforces_contest_name: "2021 NTUIM Programming Design And Optimization (PDAO 2021)"
rating: 0
weight: 104257
solve_time_s: 52
verified: true
draft: false
---

[CF 104257A - Câu trả lời có thể chấp nhận được](https://codeforces.com/problemset/problem/104257/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều truy vấn độc lập. Mỗi truy vấn chứa hai số nguyên và với mỗi cặp, chúng ta được yêu cầu xuất ra tích số học của chúng. 

Kích thước đầu vào có thể lớn về số lượng truy vấn, lên tới một trăm nghìn. Mỗi số riêng lẻ đều nhỏ, được giới hạn trong khoảng từ âm một trăm đến một trăm, do đó bản thân phép nhân luôn an toàn với bất kỳ loại số nguyên tiêu chuẩn nào, kể cả các số nguyên chính xác tùy ý của Python. Điểm áp lực chính không phải là độ phức tạp số học mà là việc xử lý đầu vào và đầu ra. Một giải pháp đọc hoặc in không hiệu quả có thể dễ dàng trở nên quá chậm mặc dù việc tính toán cho mỗi trường hợp thử nghiệm là không đáng kể. 

Đầu ra phải giữ nguyên thứ tự: mỗi trường hợp thử nghiệm tạo ra chính xác một số nguyên và chúng được in từng dòng. 

Có một số trường hợp khó khăn dễ bị bỏ qua khi triển khai một cách khéo léo. Đầu tiên, số âm phải được xử lý chính xác, ví dụ như đầu vào`-3 92`phải nhường`-276`. Thứ hai, số không phải hoạt động bình thường, chẳng hạn như`94 0`sản xuất`0`. Thứ ba, số lượng lớn các trường hợp thử nghiệm lặp lại có nghĩa là bất kỳ chi phí trên mỗi dòng nào trong quá trình phân tích cú pháp hoặc in ấn đầu vào đều trở nên quan trọng. Một cách tiếp cận đơn giản sử dụng các phương thức nhập chậm hoặc lặp đi lặp lại việc xóa có thể vượt quá giới hạn thời gian mặc dù bản thân số học là thời gian không đổi. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo của bài toán sẽ là coi phép nhân như phép cộng lặp lại. Đối với mỗi trường hợp thử nghiệm, chúng tôi có thể thêm`a`với chính nó`b`lần, điều chỉnh dấu hiệu. Mặc dù đúng về mặt logic nhưng cách tiếp cận này thực hiện tới một trăm lần lặp cho mỗi trường hợp kiểm thử trong trường hợp xấu nhất và với tối đa một trăm nghìn trường hợp kiểm thử, nó dẫn đến khoảng mười triệu lần bổ sung. Đó vẫn là giới hạn có thể chấp nhận được trong một số môi trường, nhưng nó không cần thiết và đưa ra logic bổ sung để xử lý dấu hiệu và kiểm soát vòng lặp. 

Quan sát quan trọng là phép nhân là một phép toán liên tục được tích hợp sẵn trong tất cả các ngôn ngữ hiện đại, bao gồm cả Python. Vì cả hai đầu vào đều là số nguyên nên chúng ta có thể tính trực tiếp`a * b`mà không phân hủy hoạt động thêm. Điều này làm giảm mỗi trường hợp kiểm thử thành một phép toán số học và một lần ghi đầu ra. 

Do đó, toàn bộ vấn đề giảm xuống còn phân tích cú pháp đầu vào nhanh, nhân theo thời gian không đổi và định dạng đầu ra hiệu quả. Tính đúng đắn được rút ra ngay từ định nghĩa của phép nhân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bổ sung lặp đi lặp lại | O(t · | b | ) | 
| Phép nhân trực tiếp | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Thuật toán tối ưu 

1. Đọc số lượng test case`t`. Điều này xác định có bao nhiêu tính toán độc lập chúng tôi sẽ thực hiện. 
2. Với mỗi test, đọc hai số nguyên`a`Và`b`từ đầu vào. Chúng đại diện cho các giá trị được nhân lên. 
3. Tính tích`a * b`trực tiếp bằng cách sử dụng phép nhân số nguyên có sẵn của ngôn ngữ. Bước này đúng vì phép nhân được định nghĩa là một phép toán số học cơ bản trên các số nguyên. 
4. Xuất kết quả tính toán ngay lập tức hoặc thu thập để in hàng loạt. In hàng loạt thường được ưu tiên hơn để giảm chi phí I/O. 

### Tại sao nó hoạt động 

Mỗi trường hợp thử nghiệm độc lập và xác định một biểu thức số học duy nhất. Vì phép nhân trên số nguyên có tính xác định và đóng trong miền số nguyên nên việc tính toán`a * b`mang lại kết quả chính xác duy nhất cho cặp đầu vào đó. Không có sự phụ thuộc giữa các trường hợp thử nghiệm, do đó việc xử lý chúng một cách tuần tự sẽ duy trì tính chính xác mà không yêu cầu bất kỳ trạng thái chia sẻ hoặc tiền xử lý nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    out = []
    for _ in range(t):
        a, b = map(int, input().split())
        out.append(str(a * b))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách chuyển sang đầu vào nhanh bằng cách sử dụng`sys.stdin.readline`, điều này cần thiết khi xử lý tới một trăm nghìn dòng. Mỗi dòng được phân tích thành hai số nguyên và tích của chúng được tính theo thời gian không đổi. 

Thay vì in ngay lập tức, kết quả được tích lũy thành một danh sách và được in ngay lập tức. Điều này tránh các lệnh gọi I/O lặp đi lặp lại, vốn thường là điểm nghẽn trong Python đối với loại sự cố này. 

Bản thân phép nhân rất đơn giản và hoàn toàn dựa vào số học số nguyên tích hợp của Python. 

## Ví dụ đã hoạt động 

Chúng ta sẽ theo dõi hai trường hợp đơn giản để xem thuật toán hoạt động theo từng bước như thế nào. 

### Ví dụ 1 

đầu vào:`a = -3, b = 92`| Bước | một | b | a*b | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Đọc giá trị | -3 | 92 | -276 | | 
| Sản phẩm tính toán | -3 | 92 | -276 | -276 | 
| Lưu trữ kết quả | -3 | 92 | -276 | ["-276"] | 

Điều này xác nhận việc xử lý đúng toán hạng âm. Dấu nhân được bảo toàn chính xác bằng số học số nguyên. 

### Ví dụ 2 

đầu vào:`a = 94, b = 0`| Bước | một | b | a*b | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Đọc giá trị | 94 | 0 | 0 | | 
| Sản phẩm tính toán | 94 | 0 | 0 | 0 | 
| Lưu trữ kết quả | 94 | 0 | 0 | ["0"] | 

Điều này thể hiện hành vi số 0 đúng. Bất kỳ phép nhân nào liên quan đến số 0 đều mang lại số 0 và không cần logic trường hợp đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm thực hiện một phép nhân và phân tích cú pháp theo thời gian không đổi | 
| Không gian | O(t) | Lưu trữ chuỗi đầu ra trước khi in lần cuối | 

Các ràng buộc cho phép lên tới một trăm nghìn trường hợp thử nghiệm, phù hợp thoải mái trong thời gian tuyến tính. Việc sử dụng bộ nhớ cũng nhỏ vì mỗi kết quả chỉ là một chuỗi số nguyên ngắn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b = map(int, input().split())
        out.append(str(a * b))
    return "\n".join(out)

# provided-style cases
assert run("2\n2 6\n-3 92\n") == "12\n-276"
assert run("2\n97 38\n21 -67\n") == "3686\n-1407"

# custom cases
assert run("1\n0 0\n") == "0"
assert run("1\n-100 -100\n") == "10000"
assert run("3\n1 1\n-1 1\n1 -1\n") == "1\n-1\n-1"
assert run("2\n0 5\n5 0\n") == "0\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`0`| xử lý bằng không | 
|`-100 -100`|`10000`| tiêu cực × tiêu cực | 
| dấu hiệu hỗn hợp |`1, -1, -1`| ký đúng | 
| không có cặp nào |`0, 0`| đối xứng với số 0 | 

## Vỏ cạnh 

Các giá trị âm được xử lý hoàn toàn bằng phép nhân số nguyên của Python mà không yêu cầu bất kỳ logic ký hiệu thủ công nào. Ví dụ, đầu vào`-3 92`được đánh giá trực tiếp là`-276`bởi vì toán tử nhân bảo toàn các quy tắc ký hiệu số học bên trong. 

Zero là một trường hợp quan trọng khác vì nó thường cho thấy việc triển khai phép cộng lặp lại không chính xác. Nếu vòng lặp triển khai`b`lần thêm`a`, nó phải xử lý rõ ràng trường hợp`b = 0`, nếu không nó có thể tạo ra kết quả khác 0 không chính xác. Trong giải pháp này,`94 0`đánh giá trực tiếp`0`không cần xử lý đặc biệt. 

Số lượng lớn các trường hợp kiểm thử chủ yếu nhấn mạnh đến hiệu suất đầu vào và đầu ra. Vì mỗi phép tính đều không quan trọng nên mọi sự chậm lại sẽ xuất phát từ việc in lặp đi lặp lại. Việc sử dụng đầu ra được lưu vào bộ đệm đảm bảo rằng ngay cả một trăm nghìn kết quả cũng được ghi hiệu quả chỉ trong một thao tác.
