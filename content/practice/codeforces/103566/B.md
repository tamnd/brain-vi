---
title: "CF 103566B - \u0411\u0430\u0440\u0438\u0441\u0442\u0430"
description: "Chúng ta được cho hai số nguyên, biểu thị số lượng $a$ và $b$, và chúng ta cần phân loại tỷ lệ của chúng thành một trong ba loại cà phê dựa trên mức độ lớn của $a$ so với $b$. Thay vì làm việc với các tỷ lệ dấu phẩy động, quyết định được đưa ra bằng cách sử dụng các bất đẳng thức."
date: "2026-07-03T04:57:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103566
codeforces_index: "B"
codeforces_contest_name: "2021-2022 Olympiad Cognitive Technologies, Final Round"
rating: 0
weight: 103566
solve_time_s: 42
verified: true
draft: false
---

[CF 103566B - \u0411\u0430\u0440\u0438\u0441\u0442\u0430](https://codeforces.com/problemset/problem/103566/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên biểu diễn số lượng$a$Và$b$, và chúng ta cần phân loại tỷ lệ của chúng thành một trong ba loại cà phê dựa trên mức độ lớn$a$được so sánh với$b$. 

Thay vì làm việc với các tỷ lệ dấu phẩy động, quyết định được đưa ra bằng cách sử dụng các bất đẳng thức. Nếu như$a$ít hơn ba lần$b$, đầu ra phải là thức uống đầu tiên. Nếu như$a$lớn hơn năm lần$b$, đầu ra sẽ là đồ uống thứ hai. Nếu không, nó rơi vào loại giữa. 

Do đó, đầu vào là một cặp số nguyên và đầu ra chính xác là một trong ba chuỗi cố định. 

Các ràng buộc không được hiển thị rõ ràng, nhưng cấu trúc của vấn đề làm rõ rằng giải pháp phải có thời gian không đổi cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào liên quan đến mô phỏng hoặc phép trừ lặp đi lặp lại sẽ không cần thiết và sẽ thất bại trong các giới hạn lập trình cạnh tranh điển hình khi$a$Và$b$có thể lớn. 

Các trường hợp cạnh tinh vi chính xuất phát từ đẳng thức biên. Ví dụ, khi$a = 3b$, điều kiện đầu tiên không được thỏa mãn nên đáp án phải chuyển sang bước kiểm tra tiếp theo. Tương tự, khi$a = 5b$, điều kiện thứ hai cũng không được thỏa mãn, điều này buộc phải áp dụng trường hợp mặc định. 

Việc triển khai bất cẩn thường thất bại do sử dụng phép chia dấu phẩy động chặt chẽ với các lỗi làm tròn hoặc bằng cách trộn lẫn các bất đẳng thức chặt chẽ và không chặt chẽ. 

Ví dụ, nếu$a = 3$Và$b = 1$, đầu ra đúng là loại giữa vì$a$không ít hơn$3b$và không lớn hơn$5b$. Bất kỳ triển khai nào sử dụng so sánh dấu phẩy động như$a / b < 3$có thể phân loại sai một cách không chính xác do các vấn đề về độ chính xác trong các cài đặt khác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tính tỷ lệ theo nghĩa đen$a / b$dưới dạng số dấu phẩy động và so sánh nó với 3 và 5. Về mặt lý thuyết, điều này đúng về mặt logic, nhưng nó đưa ra số học dấu phẩy động không cần thiết và có nguy cơ xảy ra lỗi chính xác trong các trường hợp gần ranh giới. Nó cũng yêu cầu phép chia, chậm hơn phép nhân số nguyên và kém rõ ràng hơn trong các bài toán nặng về số nguyên. 

Một cách tiếp cận mạnh mẽ hơn sẽ tránh được sự phân chia hoàn toàn bằng cách viết lại các bất đẳng thức. điều kiện$a / b < 3$tương đương với$a < 3b$, và tương tự$a / b > 5$trở thành$a > 5b$. Phép biến đổi này duy trì tính chính xác trong khi đảm bảo tất cả các phép tính đều ở dạng số học số nguyên, chính xác và an toàn. 

Thông tin chi tiết quan trọng là chúng ta thực sự không bao giờ cần tỷ lệ này, chỉ cần vị trí của nó so với hai ngưỡng. Khi điều đó được nhận ra, vấn đề sẽ giảm xuống còn hai trường hợp so sánh và một trường hợp mặc định. 

Cách giải thích bạo lực có hiệu quả vì quyết định chỉ phụ thuộc vào một so sánh duy nhất về độ lớn, nhưng nó trở nên không cần thiết khi chúng ta quan sát thấy rằng việc chia tỷ lệ cả hai vế theo$b$loại bỏ hoàn toàn sự phân chia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phép chia dấu phẩy động | O(1) | O(1) | Rủi ro | 
| So sánh số nguyên (tối ưu) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$a$Và$b$từ đầu vào. Chúng đại diện cho hai đại lượng có tỷ lệ xác định phân loại đầu ra. 
2. Kiểm tra xem$a < 3b$. Nếu điều này được giữ, hãy uống ngay ly đầu tiên. Việc kiểm tra này được đặt lên hàng đầu vì nó nắm bắt được vùng giá trị nhỏ nhất. 
3. Nếu điều kiện đầu tiên sai, hãy kiểm tra xem$a > 5b$. Nếu điều này được giữ, hãy uống ly thứ hai. Điều này xác định chế độ tỷ lệ lớn. 
4. Nếu cả hai điều kiện đều không được thỏa mãn, hãy cho ra ly giữa. Điều này tương ứng chính xác với khoảng$3b \le a \le 5b$. 

### Tại sao nó hoạt động 

Logic phân chia dòng số cho$a$thành ba vùng riêng biệt so với$b$: các giá trị hoàn toàn dưới đây$3b$, các giá trị ở trên$5b$, và các giá trị ở giữa. Mỗi cặp số nguyên$(a, b)$rơi vào đúng một trong các vùng này vì sự bất bình đẳng mang tính bổ sung và bao hàm mọi khả năng mà không bị chồng chéo. Vì mỗi vùng được ánh xạ tới một đầu ra duy nhất nên thuật toán không thể chỉ định các kết quả xung đột nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b = map(int, input().split())

if a < 3 * b:
    print("macchiato")
elif a > 5 * b:
    print("latte")
else:
    print("cappuccino")
```Việc thực hiện phản ánh trực tiếp sự bất bình đẳng dẫn xuất. Phép nhân thay thế phép chia, đảm bảo so sánh số nguyên chính xác mà không gặp vấn đề về độ chính xác. Thứ tự của các điều kiện rất quan trọng: ngưỡng nhỏ được kiểm tra trước, sau đó đến ngưỡng lớn và mọi thứ khác đều rơi vào loại giữa. 

Một điểm tinh tế là cả hai phép so sánh đều phải sử dụng các bất đẳng thức chặt chẽ một cách chính xác như đã suy ra. Thay đổi chúng thành$\le$hoặc$\ge$sẽ chuyển các giá trị biên vào loại sai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
```| Bước | một | b | Điều kiện a < 3b | Điều kiện a > 5b | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 2 | 1 | đúng | sai | macchiato | 

Đây$2 < 3\cdot1$, do đó điều kiện đầu tiên sẽ được kích hoạt ngay lập tức. Điều này xác nhận rằng các tỷ lệ nhỏ được phân loại chính xác mà không cần đánh giá các điều kiện sau này. 

### Ví dụ 2 

đầu vào:```
10 2
```| Bước | một | b | Điều kiện a < 3b | Điều kiện a > 5b | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 10 | 2 | sai | đúng | pha cà phê | 

Đây$10 > 5\cdot2 = 10$là sai, nên thực ra chúng ta phải cẩn thận: nó bằng 10, không lớn hơn. Vì vậy, cả hai điều kiện đều không đúng và kết quả là cappuccino. 

Dấu vết này nhấn mạnh tầm quan trọng của sự bất bình đẳng nghiêm ngặt. Các trường hợp đẳng thức nằm chính xác ở khoảng giữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng không đổi các phép tính số học và so sánh được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp này là tối ưu cho mọi kích thước đầu vào vì nó giảm quyết định xuống một số phép toán nguyên cố định, không phụ thuộc vào độ lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    a, b = map(int, input().split())

    if a < 3 * b:
        return "macchiato"
    elif a > 5 * b:
        return "latte"
    else:
        return "cappuccino"

# provided samples (constructed)
assert run("2 1\n") == "macchiato"
assert run("10 2\n") == "cappuccino"

# custom cases
assert run("0 10\n") == "macchiato", "minimum a"
assert run("100 1\n") == "latte", "large ratio"
assert run("3 1\n") == "cappuccino", "boundary 3b"
assert run("5 1\n") == "cappuccino", "boundary 5b"
assert run("15 3\n") == "cappuccino", "exact middle region"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 10 | macchiato | tỷ lệ cực trị thấp hơn | 
| 100 1 | pha cà phê | hành vi tỷ lệ lớn | 
| 3 1 | cà phê cappuccino | ranh giới dưới chính xác | 
| 5 1 | cà phê cappuccino | ranh giới trên chính xác | 
| 15 3 | cà phê cappuccino | trường hợp giữa điển hình | 

## Vỏ cạnh 

Khi nào$a = 3b$, bất đẳng thức thứ nhất thất bại vì nó nghiêm ngặt. Ví dụ, đầu vào$a=3, b=1$không mang lại điều kiện thứ nhất và thứ hai, do đó đầu ra trở thành cappuccino. Thuật toán xử lý việc này một cách chính xác vì đẳng thức tự nhiên rơi vào nhánh khác. 

Khi$a = 5b$, điều kiện thứ hai cũng thất bại. Ví dụ,$a=10, b=2$tạo ra sự bình đẳng và được phân loại chính xác vào loại trung bình. Điều này khẳng định rằng tính chặt chẽ của giới hạn trên là cần thiết. 

Khi$b$lớn, chẳng hạn như$a=0, b=10^9$, phép nhân$3b$Và$5b$vẫn vừa với phạm vi số nguyên 64 bit, do đó không có vấn đề tràn phát sinh trong Python. Điều kiện đầu tiên được kích hoạt ngay lập tức, tạo ra macchiato như mong đợi. 

Những trường hợp này xác nhận rằng thuật toán hoạt động nhất quán trên các đầu vào ranh giới và cực đoan mà không yêu cầu xử lý đặc biệt.
