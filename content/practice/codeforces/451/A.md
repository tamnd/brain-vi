---
title: "CF 451A - Trò Chơi Với Gậy"
description: "Trò chơi được chơi trên một lưới gồm n gậy ngang và m gậy dọc. Mỗi thanh ngang giao nhau với mọi thanh dọc tạo nên n m giao điểm. Trong một lượt, người chơi chọn một điểm giao nhau còn lại."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 451
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 258 (Div. 2)"
rating: 900
weight: 451
solve_time_s: 79
verified: true
draft: false
---

[CF 451A - Trò chơi với gậy](https://codeforces.com/problemset/problem/451/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi được chơi trên một lưới được hình thành bởi`n`gậy ngang và`m`gậy dọc. Mọi thanh ngang giao nhau với mọi thanh dọc, tạo thành`n * m`các điểm giao nhau. 

Trong một lượt, người chơi chọn một điểm giao nhau còn lại. Việc chọn điểm đó sẽ loại bỏ thanh ngang và thanh dọc đi qua điểm đó. Khi hai cây gậy đó được loại bỏ, mọi giao điểm liên quan đến một trong hai cây gậy đó cũng biến mất. 

Chỉ có thể di chuyển khi còn lại ít nhất một điểm giao nhau. Người chơi đầu tiên là Akshat và người chơi thứ hai là Malvika. Cả hai đều chơi tối ưu và chúng ta phải xác định xem ai thắng. 

Đầu vào chỉ chứa hai số nguyên,`n`Và`m`. Đầu ra là tên của người chơi giành chiến thắng trong lối chơi tối ưu. 

Các ràng buộc rất nhỏ, cả hai chiều tối đa là 100. Ngay cả mô phỏng cũng sẽ vừa vặn thoải mái trong giới hạn. Thử thách thực sự là nhận ra cấu trúc trò chơi và biến nó thành một quan sát đơn giản. 

Một điểm không rõ ràng là giao điểm chính xác được chọn không quan trọng. Ví dụ, trong một`3 x 5`lưới, việc chọn bất kỳ giao điểm nào luôn loại bỏ chính xác một thanh ngang và một thanh dọc. Trạng thái trò chơi còn lại chỉ phụ thuộc vào số lượng gậy ngang và dọc còn lại chứ không phụ thuộc vào số gậy cụ thể còn lại. 

Hãy xem xét đầu vào:```
1 5
```Chỉ có một thanh ngang. Sau nước đi đầu tiên, cây gậy đó sẽ bị loại bỏ nên không còn giao lộ nào nữa. Akshat thắng ngay lập tức. Mô phỏng tập trung vào tọa độ giao điểm thay vì các hàng và cột còn lại có thể dễ dàng làm phức tạp trường hợp này. 

Một ví dụ hữu ích khác là:```
2 2
```Nước đi đầu tiên sẽ loại bỏ một hàng và một cột, để lại một`1 x 1`lưới. Nước đi thứ hai sẽ loại bỏ hàng và cột cuối cùng. Không còn nước đi nào nên Akshat thua và Malvika thắng. 

Ví dụ thứ ba là:```
3 4
```Tổng cộng có thể thực hiện được ba bước di chuyển. Sau nước đi thứ ba, tất cả các hàng hoặc tất cả các cột đều đã hết. Vì ba là số lẻ nên người chơi đầu tiên thực hiện nước đi cuối cùng và thắng. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ lập mô hình toàn bộ lưới và khám phá đệ quy tất cả các chuyển động có thể. Từ một tiểu bang có`r`các hàng còn lại và`c`các cột còn lại, mỗi lần di chuyển sẽ loại bỏ một hàng và một cột, tạo ra trạng thái`(r - 1, c - 1)`. Vì có nhiều lựa chọn ở mỗi bước nên cây trò chơi đơn giản sẽ phát triển nhanh chóng, mặc dù các ràng buộc là nhỏ. 

Điều quan trọng là mỗi nước đi luôn loại bỏ đúng một thanh ngang và một thanh dọc. Không có gì khác quan trọng. Sau một lần di chuyển, số hàng còn lại giảm đi một và số cột còn lại giảm đi một. 

Giả sử chúng ta bắt đầu với`n`hàng và`m`cột. Trò chơi chỉ có thể tiếp tục khi cả hai số đếm đều dương. Sau mỗi lần di chuyển, cả hai số lượng đều giảm đi một. Do đó, tổng số lần di chuyển chính xác là:```
min(n, m)
```Người chơi thực hiện nước đi cuối cùng sẽ thắng. Vì Akshat đi trước nên anh ta thắng khi số nước đi là số lẻ. Malvika thắng khi số nước đi là số chẵn. 

Toàn bộ trò chơi quy về việc kiểm tra tính chẵn lẻ của`min(n, m)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm và không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. 
2. Tính toán`moves = min(n, m)`. 

Mỗi lần di chuyển tiêu tốn chính xác một hàng và một cột. Khi hàng hoặc cột đạt đến 0, không còn điểm giao nhau nào và trò chơi kết thúc. 
3. Kiểm tra xem`moves`là số lẻ hoặc số chẵn. 

Nếu tổng số nước đi là số lẻ thì người chơi đầu tiên sẽ thực hiện nước đi cuối cùng. 

Nếu tổng số nước đi là số chẵn thì người chơi thứ hai sẽ thực hiện nước đi cuối cùng. 
4. In`"Akshat"`khi`moves`là số lẻ, nếu không thì in`"Malvika"`. 

### Tại sao nó hoạt động 

Sau mỗi lần di chuyển, chính xác một hàng và một cột sẽ biến mất. Trạng thái trò chơi hoàn toàn được xác định bởi số lượng hàng và cột còn lại. Bắt đầu từ`(n, m)`, sau đó`k`di chuyển nhà nước trở thành`(n - k, m - k)`. Trò chơi dừng khi một trong số này đạt đến số 0, điều này xảy ra sau đúng một thời gian.`min(n, m)`di chuyển. Vì người chơi luân phiên nhau nên người chiến thắng chỉ được xác định bằng số này là số lẻ hay số chẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

if min(n, m) % 2 == 1:
    print("Akshat")
else:
    print("Malvika")
```Việc thực hiện trực tiếp theo quan sát toán học. Đầu tiên, nó tính toán trò chơi phải có bao nhiêu nước đi. Giá trị đó là`min(n, m)`bởi vì mỗi lần di chuyển sẽ loại bỏ một hàng và một cột. 

Tính chẵn lẻ của giá trị này xác định ai là người thực hiện nước đi cuối cùng. Số nước đi lẻ nghĩa là người đầu tiên đi nước cuối cùng, còn số nước chẵn nghĩa là người chơi thứ hai đi nước cuối cùng. 

Không có điều kiện biên phức tạp nào vì cả hai chiều đều có ít nhất một và câu trả lời chỉ phụ thuộc vào một lần kiểm tra tính chẵn lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
```| Bước | Hàng còn lại | Cột Còn Lại | Người chơi | 
| --- | --- | --- | --- | 
| Bắt đầu | 2 | 2 | Akshat | 
| Di chuyển 1 | 1 | 1 | Akshat | 
| Di chuyển 2 | 0 | 0 | Malvika | 

Đây`min(2, 2) = 2`. Tổng số nước đi là chẵn nên Malvika thực hiện nước đi cuối cùng và giành chiến thắng. 

### Ví dụ 2 

đầu vào:```
3 4
```| Bước | Hàng còn lại | Cột Còn Lại | Người chơi | 
| --- | --- | --- | --- | 
| Bắt đầu | 3 | 4 | Akshat | 
| Di chuyển 1 | 2 | 3 | Akshat | 
| Di chuyển 2 | 1 | 2 | Malvika | 
| Di chuyển 3 | 0 | 1 | Akshat | 

Đây`min(3, 4) = 3`. Tổng số nước đi là số lẻ nên Akshat thực hiện nước đi cuối cùng và giành chiến thắng. 

Dấu vết này chứng tỏ rằng chỉ có số lượng hàng và cột còn lại mới quan trọng. Giao lộ thực tế được chọn không bao giờ thay đổi số lần di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một thao tác tối thiểu và một lần kiểm tra tính chẵn lẻ | 
| Không gian | O(1) | Chỉ sử dụng một vài biến | 

Giải pháp thực hiện một lượng công việc không đổi bất kể giá trị đầu vào. Nó dễ dàng đáp ứng các giới hạn về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import io
import sys

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, m = map(int, input().split())
    return "Akshat" if min(n, m) % 2 else "Malvika"

# provided sample
assert run("2 2\n") == "Malvika", "sample 1"

# minimum grid
assert run("1 1\n") == "Akshat", "single move"

# one row only
assert run("1 100\n") == "Akshat", "game ends after one move"

# odd minimum dimension
assert run("3 4\n") == "Akshat", "odd move count"

# even minimum dimension
assert run("4 4\n") == "Malvika", "even move count"

# maximum values
assert run("100 100\n") == "Malvika", "largest constraints"

# asymmetric large grid
assert run("99 100\n") == "Akshat", "odd minimum dimension"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`Akshat`| Trò chơi nhỏ nhất có thể | 
|`1 100`|`Akshat`| Di chuyển có sẵn duy nhất | 
|`3 4`|`Akshat`| Số lần di chuyển lẻ | 
|`4 4`|`Malvika`| Số lần di chuyển thậm chí | 
|`100 100`|`Malvika`| Hạn chế tối đa | 
|`99 100`|`Akshat`| Trường hợp bất đối xứng lớn | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1 1
```Có đúng một giao lộ. Akshat loại bỏ nó ở lượt đầu tiên, không để lại bước di chuyển nào cho Malvika. Thuật toán tính toán`min(1, 1) = 1`, số lẻ và trả về chính xác`Akshat`. 

Hãy xem xét đầu vào:```
1 5
```Một sai lầm phổ biến là nghĩ rằng có năm nước đi có thể xảy ra vì có năm điểm giao nhau. Trên thực tế, nước đi đầu tiên sẽ loại bỏ thanh ngang duy nhất, loại bỏ mọi giao lộ ngay lập tức. Thuật toán tính toán`min(1, 5) = 1`và trả về chính xác`Akshat`. 

Hãy xem xét đầu vào:```
100 100
```Giải pháp dựa trên mô phỏng có thể mô hình hóa toàn bộ bảng mạch một cách không cần thiết. Quan sát toán học cho thấy có đúng 100 nước đi. Vì 100 là số chẵn nên Malvika thực hiện nước đi cuối cùng. Thuật toán trả về`Malvika`trong thời gian không đổi. 

Hãy xem xét đầu vào:```
2 3
```Trò chơi kéo dài đúng hai nước đi. Sau lần di chuyển đầu tiên, một hàng và một cột sẽ biến mất. Sau nước đi thứ hai, không còn hàng nào nữa. Thuật toán tính toán`min(2, 3) = 2`, phát hiện tính chẵn lẻ và trả về chính xác`Malvika`. Trường hợp này xác nhận rằng kích thước lớn hơn không bao giờ ảnh hưởng đến người chiến thắng một khi đã biết kích thước nhỏ hơn.
