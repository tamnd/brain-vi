---
title: "CF 104720A - Bánh mì Bonanza"
description: "Chúng ta có một số nguyên rất lớn được viết dưới dạng một chuỗi các chữ số liền kề nhau, không có dấu phân cách giữa các phép đo. Mỗi chữ số tương ứng với kết quả cân bánh mì riêng lẻ do Baker Sdozen sản xuất."
date: "2026-06-29T05:41:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104720
codeforces_index: "A"
codeforces_contest_name: "UTPC x WiCS Contest 10-06-23"
rating: 0
weight: 104720
solve_time_s: 51
verified: true
draft: false
---

[CF 104720A - Bánh mì Bonanza](https://codeforces.com/problemset/problem/104720/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số nguyên rất lớn được viết dưới dạng một chuỗi các chữ số liền kề nhau, không có dấu phân cách giữa các phép đo. Mỗi chữ số tương ứng với kết quả cân bánh mì riêng lẻ do Baker Sdozen sản xuất. Nhiệm vụ là tính tổng trọng lượng bằng cách tính tổng tất cả các chữ số trong chuỗi này. 

Vì vậy, thay vì diễn giải đầu vào dưới dạng một giá trị số duy nhất, chúng tôi coi nó như một chuỗi các phép đo một chữ số độc lập và tổng hợp chúng. 

Kích thước đầu vào lên tới 1000 chữ số. Giá trị này đủ nhỏ để chỉ cần quét tuyến tính trên chuỗi là đủ. Bất kỳ thuật toán nào kiểm tra từng ký tự đều chạy trong O(n), thuật toán này đủ nhanh trong giới hạn 1 giây. Bất cứ điều gì liên quan đến số học trên các số nguyên lớn ngoài việc trích xuất chữ số vẫn có thể sử dụng được trong Python, nhưng nó tốn chi phí không cần thiết so với xử lý ký tự trực tiếp. 

Sự tinh tế chính trong những vấn đề như thế này là cách biểu diễn. Nếu một người chuyển đổi nhầm dữ liệu đầu vào thành số nguyên rồi cố gắng xử lý nó thì không có gì sai sót ở đây, nhưng trong các biến thể tổng quát hơn, điều này có thể gây ra các vấn đề như mất các số 0 ở đầu hoặc chi phí chuyển đổi loại không cần thiết. Một cạm bẫy tiềm tàng khác là lặp lại trên số nguyên chứ không phải chuỗi, điều này là không thể nếu không chuyển đổi rõ ràng trở lại chuỗi. 

Trường hợp cạnh là tối thiểu nhưng vẫn đáng xem xét. Nếu đầu vào là một chữ số như`7`, câu trả lời là`7`. Nếu tất cả các chữ số đều bằng 0, chẳng hạn như`0000`, kết quả đúng là`0`. Việc triển khai bất cẩn nhằm cắt bớt các số 0 ở đầu và sau đó các quy trình có thể vẫn hoạt động ở đây, nhưng việc cắt bớt là không cần thiết và gây ra sự phức tạp có thể tránh được. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là trích xuất nhiều lần các chữ số từ một giá trị số bằng cách sử dụng mô đun và phép chia. Bắt đầu từ biểu diễn số nguyên, chúng ta có thể liên tục lấy`x % 10`, tích lũy và chia`x //= 10`. Điều này tính toán chính xác tổng chữ số theo thứ tự ngược lại. Chi phí là các phép toán O(d) trong đó d là số chữ số, vốn đã tuyến tính và tối ưu theo thuật ngữ tiệm cận. 

Tuy nhiên, việc chuyển đổi chuỗi đầu vào thành số nguyên là không cần thiết. Python có thể xử lý các số nguyên 1000 chữ số, nhưng việc phân tích cú pháp và số học sẽ tốn thêm chi phí và làm giảm độ rõ ràng. Quan trọng hơn, vấn đề đã cung cấp các chữ số ở dạng có thể lặp lại trực tiếp. Cấu trúc của đầu vào gợi ý rằng cách biểu diễn tự nhiên nhất là một chuỗi, do đó việc trích xuất chữ số trở thành một phép lặp ký tự đơn giản. 

Quan sát quan trọng là mỗi ký tự đã là một ký hiệu chữ số, do đó việc tính tổng chúng sẽ chuyển đổi từng ký tự thành giá trị số và tích lũy. Điều này tránh được việc phân tích cú pháp và giữ cho giải pháp ở mức tối thiểu và trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuyển đổi số nguyên + trích xuất chữ số | O(n) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Truyền chuỗi trực tiếp | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào dưới dạng chuỗi mà không chuyển đổi nó thành số nguyên. Điều này bảo tồn cấu trúc chữ số chính xác như đã cho. 
2. Khởi tạo biến tích lũy`total = 0`. Điều này sẽ lưu trữ tổng các chữ số đang chạy. 
3. Lặp lại từng ký tự`c`trong chuỗi. Mỗi ký tự được đảm bảo nằm giữa`'0'`Và`'9'`. 
4. Chuyển đổi từng ký tự thành giá trị số bằng cách sử dụng`ord(c) - ord('0')`hoặc`int(c)`, sau đó thêm nó vào`total`. 
5. Sau khi xử lý tất cả các ký tự, xuất ra`total`. 

### Tại sao nó hoạt động 

Đầu vào là biểu diễn cơ số 10 trong đó mỗi vị trí độc lập cho mục đích của nhiệm vụ này. Vì không cần diễn giải giá trị vị trí nên số này phân tích chính xác thành tổng các chữ số của nó. Thuật toán bảo toàn một bất biến đang chạy: sau khi xử lý ký tự i đầu tiên,`total`bằng tổng của các chữ số thứ i đó. Mỗi bước mở rộng bất biến này thêm chính xác một chữ số, đảm bảo tính chính xác khi vòng lặp kết thúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

total = 0
for c in s:
    total += ord(c) - ord('0')

print(total)
```Giải pháp đọc dữ liệu đầu vào dưới dạng chuỗi thô và loại bỏ ngay khoảng trắng. Điều này đảm bảo rằng các ký tự dòng mới không ảnh hưởng đến việc lặp lại. Sau đó, vòng lặp xử lý mỗi ký tự chính xác một lần, chuyển nó thành giá trị chữ số bằng cách sử dụng số học ASCII, nhanh hơn một chút so với gọi`int(c)`lặp đi lặp lại trong các vòng chặt chẽ. 

Bộ tích lũy`total`được cập nhật tại chỗ, tránh mọi cấu trúc dữ liệu phụ trợ. Không có nguy cơ tràn trong Python và không cần xử lý đặc biệt các số lớn vì kết quả bị giới hạn tối đa là 9000 (nếu tất cả 1000 chữ số là 9). 

## Ví dụ đã hoạt động 

### Ví dụ 1: Đầu vào`493`| Bước | Nhân vật | Giá trị chữ số | Tổng số chạy | 
| --- | --- | --- | --- | 
| 1 | '4' | 4 | 4 | 
| 2 | '9' | 9 | 13 | 
| 3 | '3' | 3 | 16 | 

Dấu vết này cho thấy mỗi chữ số đóng góp độc lập vào tổng cuối cùng, không có tương tác vị trí. 

### Ví dụ 2: Nhập liệu`9383`| Bước | Nhân vật | Giá trị chữ số | Tổng số chạy | 
| --- | --- | --- | --- | 
| 1 | '9' | 9 | 9 | 
| 2 | '3' | 3 | 12 | 
| 3 | '8' | 8 | 20 | 
| 4 | '3' | 3 | 23 | 

Điều này xác nhận rằng các chữ số lặp lại và cường độ hỗn hợp được xử lý thống nhất, không yêu cầu trường hợp đặc biệt nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự của chuỗi đầu vào được xử lý chính xác một lần | 
| Không gian | O(1) | Chỉ có một biến tích lũy duy nhất được sử dụng | 

Giới hạn đầu vào là 1000 chữ số làm cho giải pháp này có hiệu quả liên tục trong thực tế. Ngay cả trong trường hợp xấu nhất, 1000 lần lặp lại là chuyện nhỏ trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    s = input().strip()
    total = 0
    for c in s:
        total += ord(c) - ord('0')
    return str(total)

# provided samples
assert run("493\n") == "16", "sample 1"
assert run("9383\n") == "23", "sample 2"

# single digit
assert run("7\n") == "7", "single digit"

# all zeros
assert run("0000\n") == "0", "all zeros"

# maximum length simple case
assert run("9" * 1000 + "\n") == str(9000), "max size"

# alternating digits
assert run("101010\n") == "3", "alternating digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7`| 7 | đầu vào tối thiểu | 
|`0000`| 0 | xử lý số 0 hàng đầu | 
|`999...9 (1000x)`| 9000 | căng thẳng kích thước tối đa | 
|`101010`| 3 | mẫu chữ số hỗn hợp | 

## Vỏ cạnh 

Đầu vào có một chữ số như`5`thực hiện trường hợp vòng lặp tối thiểu. Thuật toán đọc một ký tự, chuyển đổi nó thành 5 và trả về ngay lập tức với tổng số 5, xác nhận rằng không cần giả định khởi tạo hoặc tích lũy nhiều bước. 

Một đầu vào hoàn toàn bằng không như`0000`đảm bảo rằng các chữ số 0 lặp lại không tạo ra sự tích lũy sai. Mỗi lần lặp thêm 0, giữ nguyên bất biến`total = 0`trong suốt quá trình thực hiện, tạo ra kết quả cuối cùng chính xác là 0. 

Một đầu vào có độ dài tối đa bao gồm toàn bộ`9`chữ số nhấn mạnh cả số lần lặp và tăng trưởng tích lũy. Thuật toán thực hiện 1000 phép cộng của 9, duy trì độ chính xác mà không gặp vấn đề về tràn hoặc độ chính xác và tạo ra 9000 như mong đợi.
