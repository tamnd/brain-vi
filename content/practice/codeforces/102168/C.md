---
title: "CF 102168C - \u0421\u043a\u043e\u0431\u043e\u0447\u043a\u0438"
description: "Chúng ta có một chuỗi dấu ngoặc đơn có độ dài chẵn. Số dấu ngoặc đơn mở và đóng là như nhau nên việc thay đổi vị trí bằng cách hoán đổi không bao giờ làm thay đổi tổng số của cả hai loại. Một thao tác chọn hai vị trí khác nhau và trao đổi ký tự của chúng."
date: "2026-08-19T15:12:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102168
codeforces_index: "C"
codeforces_contest_name: "\u041b\u0438\u0447\u043d\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u043e\u0433\u043e \u0443\u043d\u0438\u0432\u0435\u0440\u0441\u0438\u0442\u0435\u0442\u0430 \u0441\u0440\u0435\u0434\u0438 \u043d\u043e\u0432\u0438\u0447\u043a\u043e\u0432 2018-2019"
rating: 0
weight: 102168
solve_time_s: 232
verified: true
draft: false
---

[CF 102168C - \u0421\u043a\u043e\u0431\u043e\u0447\u043a\u0438](https://codeforces.com/problemset/problem/102168/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi dấu ngoặc đơn có độ dài chẵn. Số dấu ngoặc đơn mở và đóng là như nhau nên việc thay đổi vị trí bằng cách hoán đổi không bao giờ làm thay đổi tổng số của cả hai loại. Một thao tác chọn hai vị trí khác nhau và trao đổi ký tự của chúng. Nhiệm vụ là tìm số lượng hoán đổi tối thiểu cần thiết để biến chuỗi thành một chuỗi dấu ngoặc thông thường. 

Một chuỗi dấu ngoặc đơn là chính quy khi mỗi tiền tố chứa ít nhất bằng`(`BẰNG`)`và toàn bộ chuỗi chứa cùng một số cả hai. Điều kiện thứ hai đã được đảm bảo bởi đầu vào, vì vậy điều duy nhất có thể sai là một số tiền tố có quá nhiều dấu ngoặc đơn đóng. 

Chiều dài có thể đạt tới (10^6). Thuật toán bậc hai sẽ thực hiện khoảng (10^{12}) phép toán cơ bản trong trường hợp xấu nhất, vượt xa giới hạn hai giây có thể hỗ trợ. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính và đầu vào đủ lớn đến mức việc quét liên tục các hậu tố dài cũng nguy hiểm. 

Một trường hợp cạnh hữu ích là`)(`. Tổng số dư của nó bằng 0, nhưng tiền tố đầu tiên có số dư (-1), vì vậy câu trả lời là`1`. Giải pháp chỉ kiểm tra số dư cuối cùng sẽ trả về số 0 không chính xác. 

Một trường hợp cạnh khác là`))((`. Số dư tiền tố tối thiểu của nó là (-2), nhưng chỉ cần một lần hoán đổi: trao đổi số tiền đầu tiên`)`với ký tự thứ ba cho`()()`. Một giải pháp tính riêng từng đơn vị thâm hụt có thể trả về hai đơn vị thiếu hụt một cách không chính xác. 

Hướng làm tròn cũng có vấn đề. Vì`)))(((`, số dư tiền tố tối thiểu là (-3), trong khi câu trả lời là`2`, không`1`. Một lần hoán đổi sẽ thay đổi số dư tiền tố chính xác bằng hai, do đó, mức thâm hụt ba đòi hỏi hai lần hoán đổi. 

Các mẫu được cung cấp là`))((`, câu trả lời của ai`1`, Và`(())`, câu trả lời của ai`0`. 

## Phương pháp tiếp cận 

Việc triển khai tham lam trực tiếp có thể quét từ trái sang phải và bất cứ khi nào tiền tố hiện tại không hợp lệ, hãy tìm kiếm ở bên phải để tìm tiền tố không được sử dụng`(`và trao đổi nó với vi phạm`)`. Ý tưởng này đúng vì tiền tố phủ định phải nhận được dấu ngoặc đơn mở từ đâu đó sau đó và việc hoán đổi như vậy sẽ làm tăng số dư của mỗi tiền tố bị ảnh hưởng lên hai. 

Vấn đề là tìm kiếm tương lai đó`(`. Trong một chuỗi như`))))...((((...`, có thể có một tìm kiếm khoảng cách tuyến tính cho mỗi lần hiệu chỉnh. Với (n) ký tự, điều này có thể yêu cầu kiểm tra ký tự (\Theta(n^2)). Đối với (n=10^6), đó là thứ tự của các phép toán (10^{12}), quá chậm. 

Quan sát loại bỏ phần đắt tiền là chúng ta không bao giờ thực sự cần biết dấu ngoặc đơn mở nào trong tương lai sẽ được hoán đổi. Chúng ta chỉ cần biết tiền tố xấu dưới 0 là bao nhiêu. Đại diện`(`như (+1) và`)`là (-1). Giả sử một số tiền tố có số dư (-d). Hoán đổi một`)`từ tiền tố này với một`(`sau khi tiền tố tăng số dư tiền tố đó lên chính xác (2). Do đó, một lần hoán đổi có thể bù đắp được hai đơn vị thâm hụt. 

Gọi (m) là số dư tiền tố tối thiểu. Nếu (m \ge 0), dãy này đã chính quy và không cần hoán đổi. Nếu (m<0), thì ít nhất (\lceil -m/2\rceil) hoán đổi là cần thiết. Số lần hoán đổi như nhau luôn là đủ: bất cứ khi nào số dư hiện hành trở nên âm, về mặt khái niệm, chúng ta có thể trao đổi dấu ngoặc đơn đóng đó với dấu ngoặc đơn mở sau đó. Việc điều chỉnh sẽ làm tăng số dư bị ảnh hưởng lên gấp đôi và vì tổng số dư bằng 0 nên sau đó nhất thiết phải có dấu ngoặc đơn mở thích hợp. 

Có một cách thậm chí còn đơn giản hơn để thực hiện lý luận tương tự. Trong khi quét, hãy giữ số dư hiện tại và số dư nhỏ nhất nhìn thấy. Cuối cùng, câu trả lời là 

\frac{-m+1}{2} 
] 

cho âm (m). Tương tự, chúng ta có thể đếm số lần số dư sẽ trở nên âm và ngay lập tức đặt lại số dư từ (-1) thành (1). Công thức số dư tối thiểu rõ ràng hơn vì nó làm cho bằng chứng trở nên rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) để tìm kiếm và hoán đổi nhiều lần | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(1)) bên cạnh chuỗi đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc toàn bộ chuỗi dấu ngoặc đơn và khởi tạo`balance = 0`Và`minimum = 0`. Biến`balance`biểu thị sự khác biệt giữa số dấu ngoặc đơn mở và đóng trong tiền tố hiện tại. 
2. Quét chuỗi từ trái sang phải. Thêm vào`1`khi ký tự hiện tại là`(`và trừ`1`khi nào vậy`)`. 
3. Sau khi xử lý từng ký tự, cập nhật`minimum`với số dư nhỏ nhất đạt được cho đến nay. Giá trị âm có nghĩa là tiền tố tương ứng chứa nhiều dấu ngoặc đơn đóng hơn dấu ngoặc đơn mở. 
4. Sau khi quét, nếu`minimum`không âm, đầu ra`0`. Mọi tiền tố đều đã có số dư không âm và tổng số dư bằng 0, do đó chuỗi là đều đặn. 
5. Nếu`minimum`là âm, đầu ra`(-minimum + 1) // 2`. Mỗi lần hoán đổi có thể tăng số dư của tiền tố xấu lên tối đa là hai, trong khi việc sửa chữa tham lam có thể đạt được chính xác sự cải thiện đó, vì vậy đây là số lần hoán đổi tối thiểu. 

### Tại sao nó hoạt động 

Hãy xem xét một tiền tố có số dư tối thiểu toàn cầu (m<0). Mọi chuỗi hợp lệ phải nâng tiền tố này từ (m) lên ít nhất bằng 0. Một trao đổi có thể di chuyển một`(`từ sau tiền tố vào bên trong nó trong khi di chuyển một tiền tố`)`out, thay đổi số dư tiền tố chính xác (2). Do đó, cần phải có ít nhất (\lceil -m/2\rceil) hoán đổi. 

Nhiều lần hoán đổi cũng là đủ. Bất cứ khi nào tiền tố trở thành số âm thì phải có dấu ngoặc đơn mở sau đó vì toàn bộ chuỗi có số dấu ngoặc đơn mở và đóng bằng nhau. Hoán đổi một tương lai như vậy`(`với người vi phạm`)`tăng tất cả số dư giữa hai vị trí lên hai. Việc lặp lại thao tác này sẽ loại bỏ phần thiếu hụt và số lần chỉnh sửa cần thiết là chính xác (\lceil -m/2\rceil). Thuật toán tính toán chính xác giới hạn dưới đó, vì vậy câu trả lời của nó là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    balance = 0
    minimum = 0

    for ch in s:
        if ch == '(':
            balance += 1
        else:
            balance -= 1

        if balance < minimum:
            minimum = balance

    print((-minimum + 1) // 2)

if __name__ == "__main__":
    solve()
```Đầu vào được đọc dưới dạng một chuỗi vì có chính xác một trường hợp thử nghiệm. Chuỗi có thể chứa tối đa (10^6) ký tự, vì vậy`sys.stdin.readline`tránh được chi phí đầu vào không cần thiết. 

Quá trình quét duy trì số dư tiền tố chỉ bằng một số nguyên. Không cần phải sửa đổi chuỗi, vì bằng chứng cho thấy rằng các vị trí chính xác được sử dụng bởi các giao dịch hoán đổi không ảnh hưởng đến số lượng hoạt động, chỉ có sự thiếu hụt tiền tố tồi tệ nhất mới quan trọng.`minimum`bắt đầu bằng 0 thay vì ở giá trị dương lớn vì tiền tố trống có số dư bằng 0. Điều này cũng làm cho các chuỗi đã hợp lệ như`(())`Và`()()`tự nhiên tạo ra câu trả lời bằng 0. 

biểu hiện`(-minimum + 1) // 2`thực hiện phép chia trần số nguyên cho một giá trị dương. Ví dụ, thâm hụt một sẽ mang lại`1`, thâm hụt hai mang lại`1`, và thâm hụt ba cho`2`. Số nguyên Python không bị tràn, do đó không cần xử lý đặc biệt ngay cả ở độ dài (10^6). 

Bản thân số dư cuối cùng không cần phải kiểm tra. Câu lệnh đảm bảo số lượng dấu ngoặc đơn mở và đóng bằng nhau, vì vậy sau khi toàn bộ chuỗi được xử lý, số dư nhất thiết phải bằng 0. 

## Ví dụ đã hoạt động 

### Mẫu 1:`))((`Chuỗi bắt đầu bằng hai dấu ngoặc đơn đóng, do đó tiền tố đầu tiên trở nên không hợp lệ. Số dư tối thiểu là (-2), nghĩa là một lần hoán đổi có thể bù đắp toàn bộ khoản thâm hụt. 

| Vị trí | Nhân vật | Số dư | Tối thiểu | 
| --- | --- | --- | --- | 
| 0 |`)`| -1 | -1 | 
| 1 |`)`| -2 | -2 | 
| 2 |`(`| -1 | -2 | 
| 3 |`(`| 0 | -2 | 

Câu trả lời là 

[ 
\frac{-(-2)+1}{2} = \frac{3}{2} = 1 
] 

dùng phép chia số nguyên. Một sự hoán đổi có thể xảy ra là giữa vị trí 0 và 2, tạo ra`()()`. 

Dấu vết cho thấy tại sao chỉ riêng số dư cuối cùng là không đủ. Giá trị cuối cùng bằng 0, nhưng số dư tiền tố tối thiểu là âm, do đó chuỗi ban đầu không đều đặn. 

### Mẫu 2:`(())`Mọi tiền tố đều có số dư không âm nên số dư tối thiểu bằng 0. 

| Vị trí | Nhân vật | Số dư | Tối thiểu | 
| --- | --- | --- | --- | 
| 0 |`(`| 1 | 0 | 
| 1 |`(`| 2 | 0 | 
| 2 |`)`| 1 | 0 | 
| 3 |`)`| 0 | 0 | 

Câu trả lời là`0`. Trình tự đã đáp ứng điều kiện tiền tố bắt buộc và có tổng số dư bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi ký tự được xử lý chính xác một lần. | 
| Không gian | (O(1)) phụ trợ | Chỉ một`balance`Và`minimum`được duy trì sau khi đọc chuỗi. | 

Với (n\le 10^6), quá trình quét tuyến tính chỉ thực hiện khoảng một triệu lần lặp. Điều đó nằm trong phạm vi dự định của giới hạn hai giây, trong khi phương pháp bậc hai có thể yêu cầu các phép tính khoảng (10^{12}). 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_string(s: str) -> str:
    balance = 0
    minimum = 0

    for ch in s:
        if ch == '(':
            balance += 1
        else:
            balance -= 1

        minimum = min(minimum, balance)

    return str((-minimum + 1) // 2)

def run(inp: str) -> str:
    return solve_string(inp.strip()) + "\n"

# Provided samples
assert run("))((") == "1\n", "sample 1"
assert run("(())") == "0\n", "sample 2"

# Minimum-size valid input
assert run("()") == "0\n", "minimum size"

# A single unit of negative prefix balance
assert run(")(") == "1\n", "one swap needed"

# Odd-sized deficit, catches incorrect floor division
assert run(")))(((") == "2\n", "ceil division is required"

# Maximum-size input, already regular
assert run("()" * 500000) == "0\n", "maximum size"

# All possible symbols cannot be equal under the input guarantee,
# so the closest meaningful all-equal-style stress case is a
# repeated identical valid block.
assert run("()" * 10) == "0\n", "repeated blocks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`()`|`0`| Đầu vào tối thiểu không trống và một chuỗi đã hợp lệ | 
|`)(`|`1`| Tiền tố phủ định mặc dù tổng số dư bằng 0 | 
|`)))(((`|`2`| Đúng cách chia trần khi thâm hụt lẻ | 
|`()`lặp đi lặp lại 500000 lần |`0`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 
|`()`lặp lại 10 lần |`0`| Khối cân bằng lặp đi lặp lại và theo dõi tiền tố ổn định | 

Hạn chế đầu vào làm cho một chuỗi chữ hoàn toàn bằng nhau là không thể, bởi vì số lượng`(`Và`)`phải bằng nhau. Một chuỗi chỉ chứa`(`hoặc chỉ`)`sẽ vi phạm sự đảm bảo, do đó, bộ thử nghiệm sẽ sử dụng các trường hợp mẫu ký tự lặp lại có ý nghĩa lớn nhất để thay thế. 

## Vỏ cạnh 

cho`)(`, số dư là (-1) và (0). Tối thiểu là (-1), do đó công thức cho`(-(-1) + 1) // 2 = 1`. Hoán đổi hai vị trí tạo ra`()`. Giải pháp chỉ có số dư cuối cùng sẽ bỏ lỡ tiền tố đầu tiên không hợp lệ. 

Vì`))((`, số dư là (-1,-2,-1,0), nên mức tối thiểu là (-2). Công thức cho`1`. Điểm mấu chốt là một lần hoán đổi sẽ thay đổi số dư tiền tố thêm hai, do đó mức thâm hụt hai không yêu cầu hai lần điều chỉnh riêng biệt. 

Vì`)))(((`, số dư là (-1,-2,-3,-2,-1,0). Tối thiểu là (-3), cho`(-(-3) + 1) // 2 = 2`. Điều này mắc phải lỗi phổ biến khi sử dụng`-minimum // 2`, sẽ làm tròn xuống và trả về không chính xác`1`. 

Vì`()`lặp đi lặp lại 500000 lần, số dư xen kẽ giữa`1`Và`0`, Vì thế`minimum`vẫn bằng 0 trong toàn bộ đầu vào hàng triệu ký tự. Thuật toán thực hiện một lượng công việc không đổi cho mỗi ký tự và trả về`0`. Điều này thực hiện giới hạn đầu vào trên mà không yêu cầu bất kỳ sự hoán đổi nào. 

Vì`(())`, sự cân bằng tiến triển thông qua`1,2,1,0`, không bao giờ trở nên âm. Do đó, thuật toán trả về 0 mà không cố gắng tìm một hoán đổi, điều này xác nhận rằng các chuỗi đã hợp lệ được xử lý mà không cần thao tác trong trường hợp đặc biệt.
