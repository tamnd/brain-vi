---
title: "CF 104545J - Lễ Vui Vẻ Của Các Vị Thần"
description: "Câu chuyện mô tả một nhân vật gửi tiền vào tài khoản căng tin đúng một lần và sau đó liên tục chi tiêu vào các bữa ăn. Mỗi bữa ăn có chi phí cố định là hai đơn vị tiền tệ."
date: "2026-06-30T08:59:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104545
codeforces_index: "J"
codeforces_contest_name: "VIII MaratonUSP Freshman Contest"
rating: 0
weight: 104545
solve_time_s: 43
verified: true
draft: false
---

[CF 104545J - Lễ vui mừng của các vị thần](https://codeforces.com/problemset/problem/104545/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Câu chuyện mô tả một nhân vật gửi tiền vào tài khoản căng tin đúng một lần và sau đó liên tục chi tiêu vào các bữa ăn. Mỗi bữa ăn có chi phí cố định là hai đơn vị tiền tệ. Sau một số bữa ăn không xác định, chúng tôi quan sát thấy hai giá trị: số tiền đã được thêm vào trong một lần nạp tiền đó và số dư còn lại tại thời điểm chúng tôi kiểm tra. 

Nhiệm vụ là xây dựng lại số bữa ăn đã được thanh toán sau lần nạp tiền đó. Cấu trúc ẩn chính là cách duy nhất để số dư thay đổi sau khi nạp tiền là bằng cách trừ đi lặp lại chi phí không đổi cho mỗi bữa ăn, do đó, toàn bộ vấn đề giảm xuống còn việc tìm ra số tiền đã biến mất khỏi tài khoản và chia cho giá của một bữa ăn. 

Đầu vào cung cấp hai số nguyên, số tiền nạp lại và số dư hiện tại. Đầu ra là số bữa ăn được tiêu thụ sau khi nạp lại năng lượng. 

Các ràng buộc là cực kỳ nhỏ, với cả hai giá trị được giới hạn bởi 200 và được đảm bảo có cùng tính chẵn lẻ. Điều này ngay lập tức loại trừ mọi nhu cầu mô phỏng hoặc tìm kiếm. Bất kỳ giải pháp nào chạy trong thời gian không đổi hoặc thời gian tuyến tính trên phạm vi giá trị đều đã là quá đủ. 

Một sai lầm ngây thơ nhưng phổ biến là cố gắng xây dựng lại số dư ban đầu không xác định trước khi nạp tiền. Ví dụ: người ta có thể cho rằng tài khoản bắt đầu từ số 0 hoặc cố gắng đoán lịch sử nạp tiền trước. Điều đó là không cần thiết vì vấn đề nêu rõ chỉ có một lần sạc lại và mọi thứ liên quan sẽ xảy ra sau đó. 

Một sự nhầm lẫn tiềm ẩn khác đến từ ràng buộc chẵn lẻ. Nếu không để ý rằng mỗi bữa ăn có giá chính xác là hai bữa, người ta có thể nghĩ ngang giá là một con cá trích đỏ. Trong thực tế, nó đảm bảo rằng sự khác biệt giữa số tiền nạp và số dư cuối cùng chia hết cho hai, vì vậy câu trả lời luôn là số nguyên. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ mô phỏng từng bữa ăn một. Bắt đầu từ số tiền nạp lại, chúng tôi liên tục trừ đi hai cho đến khi đạt được số dư cuối cùng. Điều này hiệu quả vì mỗi thao tác tương ứng trực tiếp với một bữa ăn và chúng tôi dừng chính xác khi đạt đến mức cân bằng quan sát được. Tuy nhiên, mặc dù giới hạn ở đây rất nhỏ nhưng cách tiếp cận này lãng phí về mặt khái niệm vì nó thực hiện một lần lặp lại trong mỗi bữa ăn. 

Quan sát chính là tất cả các bữa ăn đều có chi phí giống nhau. Thay vì mô phỏng phép trừ lặp đi lặp lại, chúng ta có thể tính toán tổng số tiền chi tiêu một cách trực tiếp dưới dạng chênh lệch giữa số tiền được thêm vào và số tiền còn lại. Tổng số tiền chi tiêu đó phải bằng số bữa ăn nhân hai. Vì vậy, câu trả lời có được bằng một phép chia duy nhất. 

Điều này làm giảm vấn đề từ quy trình từng bước thành trích xuất số học trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(x) | O(1) | Có thể chấp nhận được nhưng không cần thiết | 
| Công thức trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán số tiền đã sử dụng sau khi nạp tiền và chuyển số tiền đó thành số bữa ăn. 

1. Đọc hai số nguyên, giá trị nạp lại và số dư hiện tại. Chúng thể hiện số tiền ban đầu sau khi nạp tiền và số tiền còn lại được quan sát. 
2. Tính tổng số tiền đã chi bằng cách lấy số tiền nạp trừ đi số dư cuối cùng. Sự khác biệt này tương ứng chính xác với tất cả các bữa ăn cộng lại vì không có hoạt động nào khác ảnh hưởng đến sự cân bằng. 
3. Chia số tiền chi tiêu cho hai để có được số bữa ăn, vì mỗi bữa ăn có giá đúng hai đơn vị. 
4. Xuất kết quả. 

### Tại sao nó hoạt động

Sau một lần nạp tiền, hoạt động duy nhất có thể ảnh hưởng đến số dư là thanh toán các bữa ăn, mỗi lần sẽ giảm số dư chính xác hai. Do đó, tổng mức giảm cân bằng chính xác gấp đôi số bữa ăn. Vì không có giao dịch nào khác xảy ra nên chênh lệch giữa số dư ban đầu sau khi nạp tiền và số dư cuối cùng sẽ xác định duy nhất số lượng bữa ăn. Điều kiện chẵn lẻ đảm bảo sự khác biệt này luôn là số chẵn, do đó phép chia số nguyên có giá trị không có số dư. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x, y = map(int, input().split())
    spent = x - y
    print(spent // 2)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo công thức dẫn xuất. Phép trừ`x - y`nắm bắt tổng chi tiêu sau khi nạp tiền. Không cần vòng lặp hay mô phỏng vì mỗi đơn vị tiền chi ra đều tương ứng với một số bữa ăn cố định. 

Sự tinh tế duy nhất là đảm bảo sử dụng phép chia số nguyên. Vì bài toán đảm bảo tính chẵn lẻ giống hệt nhau,`x - y`luôn luôn chẵn, do đó việc chia sàn sẽ tạo ra kết quả nguyên chính xác một cách an toàn. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi tính toán trên hai đầu vào. 

### Ví dụ 1 

đầu vào:```
50 34
```| nạp tiền x | cân bằng y | đã chi tiêu (x - y) | bữa ăn (đã chi / 2) | 
| --- | --- | --- | --- | 
| 50 | 34 | 16 | 8 | 

Chênh lệch giữa số tiền nạp và số dư còn lại là 16. Vì mỗi bữa ăn có giá 2 nên tương ứng với 8 bữa. Dấu vết khẳng định giải pháp chỉ phụ thuộc vào tổng chi tiêu chứ không phụ thuộc vào bất kỳ bước trung gian nào. 

### Ví dụ 2 

đầu vào:```
131 47
```| nạp tiền x | cân bằng y | đã chi tiêu (x - y) | bữa ăn (đã chi / 2) | 
| --- | --- | --- | --- | 
| 131 | 47 | 84 | 42 | 

Ở đây tổng số tiền chi tiêu là 84, chia thành 42 bữa ăn với giá 2 bữa mỗi bữa. Trường hợp này chứng tỏ rằng logic tương tự áp dụng cho các giá trị lớn hơn mà không có bất kỳ thay đổi nào về cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép giá trị lên tới 200, nhưng thuật toán không phụ thuộc vào kích thước của chúng. Nó thực hiện một chuỗi hoạt động cố định bất kể cường độ đầu vào, làm cho nó nằm trong giới hạn một cách tầm thường. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x, y = map(int, sys.stdin.readline().split())
    spent = x - y
    return str(spent // 2)

# provided samples
assert run("50 34\n") == "8", "sample 1"
assert run("131 47\n") == "42", "sample 2"

# custom cases
assert run("0 0\n") == "0", "no recharge spent"
assert run("2 0\n") == "1", "single meal"
assert run("200 200\n") == "0", "full balance untouched"
assert run("200 0\n") == "100", "maximum spending case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 0 | không chi tiêu sau khi nạp tiền | 
| 2 0 | 1 | số bữa ăn không tầm thường nhỏ nhất | 
| 200 200 | 0 | trường hợp cạnh mà không có gì được chi tiêu | 
| 200 0 | 100 | ranh giới tiêu thụ tối đa | 

## Vỏ cạnh 

Khi số tiền nạp bằng số dư hiện tại, chênh lệch bằng 0, do đó thuật toán không tạo ra bữa ăn nào. Điều này tương ứng với tình huống không có chi tiêu nào xảy ra sau khi nạp tiền và bước trừ ngay lập tức tạo ra số 0. 

Khi số dư giảm xuống 0, toàn bộ số tiền từ việc nạp tiền đã bị tiêu hết. Thuật toán tính toán`x - 0`, bằng với số tiền nạp đầy đủ và chia cho hai sẽ ra tổng số bữa ăn. Vì ràng buộc chẵn lẻ đảm bảo`x`ngay cả trong trường hợp này, phép chia luôn chính xác và tạo ra số nguyên hợp lệ.
