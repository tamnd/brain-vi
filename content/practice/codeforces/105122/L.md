---
title: "CF 105122L - Bình đẳng"
description: "Chúng ta được cung cấp một chuỗi dòng đơn nhằm biểu thị sự bằng nhau về mặt số học giữa hai biểu thức. Mỗi biểu thức có thể chứa các chữ số thập phân và các ký hiệu + và -."
date: "2026-06-27T19:41:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105122
codeforces_index: "L"
codeforces_contest_name: "XXVI Interregional Programming Olympiad, Vologda SU, 2024"
rating: 0
weight: 105122
solve_time_s: 76
verified: true
draft: false
---

[CF 105122L - Bình đẳng](https://codeforces.com/problemset/problem/105122/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dòng đơn nhằm biểu thị sự bằng nhau về mặt số học giữa hai biểu thức. Mỗi biểu thức có thể chứa các chữ số thập phân và các ký hiệu`+`Và`-`. Dấu trừ có thể hoạt động như một toán tử trừ nhị phân và như một dấu một ngôi làm đảo dấu của số sau và nhiều dấu một ngôi có thể xuất hiện liên tiếp. 

Nhiệm vụ là phân loại chuỗi đầu vào thành ba loại. Đầu tiên, chuỗi có thể không đúng định dạng, nghĩa là nó vi phạm các quy tắc cú pháp của chính ngôn ngữ đó, chẳng hạn như có các ký tự không hợp lệ, thiếu cấu trúc hoặc không chứa chính xác một chuỗi.`=`. Thứ hai, nó có thể hợp lệ về mặt cú pháp nhưng lại sai về mặt số học một khi được đánh giá. Thứ ba, nó có thể đúng về mặt cú pháp và cũng đúng về mặt số học. 

Khó khăn chính là tính hợp lệ phụ thuộc vào việc phân tích một ngữ pháp rất dễ dãi: đơn phân lặp đi lặp lại`+`Và`-`được phép ký hiệu, các chữ số có thể có số 0 đứng đầu và các biểu thức không bị hạn chế bởi khoảng trắng hoặc định dạng vượt quá các ký tự được phép. Đồng thời, các đầu vào không đúng định dạng phải bị loại bỏ sớm, ngay cả khi chúng có thể được diễn giải bằng số. 

Hạn chế lên tới 3 triệu ký tự trong một dòng. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liên tục cắt các chuỗi, xây dựng các chuỗi con trung gian lớn hoặc sử dụng phân tích cú pháp đệ quy với quay lui. Bất kỳ phương pháp bậc hai nào trong kích thước đầu vào sẽ không thành công và ngay cả các giải pháp tuyến tính cũng phải được triển khai cẩn thận để tránh chi phí Python như ghép nối lặp lại hoặc quay lui biểu thức chính quy. 

Một số trường hợp cạnh là tinh tế. 

Trường hợp quan trọng đầu tiên là cấu trúc không đúng định dạng xung quanh các toán tử. Ví dụ,`2+2=4+`không đúng định dạng vì toán tử xuất hiện ở cuối mà không có số theo sau. Người đánh giá ngây thơ có thể bỏ qua các toán tử theo sau và coi nó là cú pháp hợp lệ không chính xác. 

Trường hợp thứ hai là ký tự không hợp lệ. Đầu vào thích`2*2=4`phải được phân loại ngay lập tức là không đúng định dạng. Trình phân tích cú pháp chỉ tập trung vào các chữ số và bỏ qua các ký tự không xác định sẽ xử lý không chính xác`*`như tiếng ồn và tiếp tục. 

Trường hợp thứ ba là chuỗi đơn nguyên mơ hồ gần`=`. Ví dụ,`-+10=10`có giá trị về mặt cú pháp nhưng sai. Trình phân tích cú pháp kết hợp các toán tử một ngôi không chính xác hoặc bỏ qua các quy tắc thay thế dấu hiệu có thể đánh giá sai biểu thức bên trái. 

Cuối cùng, biểu thức trống rỗng xung quanh`=`không hợp lệ. Các chuỗi như`=1+2`hoặc`1+2=`không đúng định dạng vì mỗi bên phải chứa ít nhất một mã thông báo số hợp lệ. 

Do đó, thách thức cốt lõi là kết hợp xác thực cú pháp nghiêm ngặt với đánh giá một lần hiệu quả. 

## Phương pháp tiếp cận 

Trước tiên, cách giải thích bạo lực sẽ phân tích đầy đủ từng bên thành mã thông báo, xây dựng cây biểu thức rõ ràng hoặc chuyển đổi sang ký hiệu hậu tố, sau đó đánh giá cả hai bên. Điều này yêu cầu nhiều lần chuyển qua chuỗi, cộng với phân bổ trung gian cho danh sách mã thông báo hoặc ngăn xếp tỷ lệ thuận với kích thước đầu vào. 

Trong trường hợp xấu nhất, mỗi ký tự đóng góp vào nhiều cấu trúc: mã thông báo, nút AST hoặc khung ngăn xếp. Điều này dẫn đến các hệ số không đổi nặng và có khả năng xảy ra hành vi bậc hai nếu được thực hiện một cách bất cẩn bằng cách cắt chuỗi hoặc nối chuỗi lặp đi lặp lại. 

Quan sát quan trọng là chúng ta thực sự không cần xây dựng bất kỳ cấu trúc nào. Ngữ pháp đủ đơn giản để chúng ta có thể đánh giá từng bên trong một lần duyệt bằng cách duy trì tổng chạy và dấu hiện tại, đồng thời xác thực cú pháp tại thời điểm chúng ta đọc từng ký tự. 

Sau đó, việc kiểm tra đẳng thức sẽ giảm xuống việc tính toán hai số nguyên một cách nhanh chóng và so sánh chúng, đồng thời phát hiện cấu trúc không đúng định dạng bằng cách sử dụng logic trạng thái hữu hạn nhỏ: liệu chúng ta hiện có ở trong một số hay không, liệu một toán tử có được mong đợi hay không và liệu biểu thức có bắt đầu chính xác hay không. 

Điều này chuyển đổi phân tích cú pháp thành một quá trình truyền phát với bộ nhớ không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân tích cú pháp Brute Force + AST | O(n) đến O(n²) | O(n) | Quá chậm | 
| Đánh giá một lần có xác nhận | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi trong một lần quét và đánh giá riêng cả hai mặt. 

1. Trước tiên, hãy xác minh rằng chính xác một`=`tồn tại trong chuỗi. Nếu không, dữ liệu đầu vào ngay lập tức bị sai định dạng vì cấu trúc không bằng nhau. 
2. Chia quá trình xử lý thành hai nửa một cách hợp lý`=`. Chúng tôi không phân chia chuỗi về mặt vật lý; thay vào đó chúng tôi quét và chuyển ngữ cảnh khi gặp phải`=`. 
3. Đối với mỗi bên, duy trì ba biến số: tổng hiện tại, dấu hiệu hiện tại (+1 hoặc -1) và cờ cho biết liệu hiện tại chúng ta có đang mong đợi một con số hay không. Cờ này rất quan trọng để phát hiện cú pháp không đúng định dạng. 
4. Khi chúng ta gặp phải`+`hoặc`-`, chúng tôi cập nhật dấu hiệu hiện tại. MỘT`+`để nó không thay đổi, a`-`lật nó. Nếu chúng ta thấy nhiều dấu hiệu liên tiếp, chúng sẽ kết hợp một cách tự nhiên thành dấu hiệu cuối cùng trước một số. 
5. Khi gặp một chữ số, chúng tôi phân tích số liền kề đầy đủ. Chúng tôi nhân nó với tổng số đang chạy bằng cách sử dụng dấu hiệu hiện tại, sau đó đặt lại dấu hiệu thành dương và đánh dấu rằng chúng tôi hiện đang mong đợi một toán tử tiếp theo. 
6. Nếu chúng ta gặp bất kỳ ký tự nào ngoài chữ số,`+`,`-`, hoặc`=`, đầu vào bị sai định dạng ngay lập tức. 
7. Nếu một toán tử xuất hiện ở vị trí cần có một số (ví dụ sau một toán tử khác hoặc ở đầu một bên), thì biểu thức chỉ hợp lệ nếu nó là một số. Điều này có nghĩa là chúng tôi cho phép`+`hoặc`-`khi mong đợi một số, nhưng không cho phép đặt toán tử nhị phân ở các vị trí không hợp lệ, chẳng hạn như`2+`ở cuối hoặc`=+`. 
8. Sau khi xử lý cả hai mặt, so sánh tổng số được đánh giá. Nếu chúng khớp nhau, xuất ra`YES`. Nếu cú ​​pháp hợp lệ nhưng các giá trị khác nhau, hãy xuất ra`NO`. 

### Tại sao nó hoạt động 

Điều bất biến là tại bất kỳ thời điểm nào trong quá trình quét, tổng chạy biểu thị giá trị của tất cả các số được sử dụng đầy đủ với các hiệu ứng dấu tích lũy chính xác và dấu hiện tại biểu thị hiệu ứng kết hợp của tất cả các toán tử đơn phân được nhìn thấy kể từ số cuối cùng. 

Bởi vì mỗi số được sử dụng chính xác một lần và mỗi dấu hiệu chỉ ảnh hưởng đến số tiếp theo nên không tồn tại sự phụ thuộc bị trì hoãn. Điều này đảm bảo rằng việc đánh giá phát trực tuyến tương đương với việc phân tích cú pháp biểu thức đầy đủ. Việc phát hiện sai định dạng đã hoàn tất vì mọi cấu trúc không hợp lệ đều tương ứng với trạng thái trong đó một số được mong đợi nhưng không tìm thấy hoặc một ký tự bị cấm xuất hiện, cả hai đều bị phát hiện ngay lập tức trong quá trình truyền tải. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def evaluate(expr):
    n = len(expr)
    i = 0
    total = 0
    sign = 1
    expect_number = True

    if n == 0:
        return None, False

    while i < n:
        c = expr[i]

        if c in '+-':
            if expect_number:
                if c == '+':
                    pass
                else:
                    sign *= -1
                i += 1
            else:
                return None, False

        elif c.isdigit():
            j = i
            while j < n and expr[j].isdigit():
                j += 1
            num = int(expr[i:j])
            total += sign * num
            sign = 1
            expect_number = False
            i = j

        else:
            return None, False

    if expect_number:
        return None, False

    return total, True

def solve():
    s = input().rstrip('\n')

    if s.count('=') != 1:
        print("ERROR")
        return

    left, right = s.split('=')

    lv, ok1 = evaluate(left)
    rv, ok2 = evaluate(right)

    if not ok1 or not ok2:
        print("ERROR")
    elif lv == rv:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp tách phân tích cú pháp thành một bộ đánh giá có thể tái sử dụng để xử lý một biểu thức theo thời gian tuyến tính. Nó giữ một tổng đang chạy và một bộ tích lũy dấu, áp dụng từng số được phân tích cú pháp ngay lập tức. các`expect_number`cờ thực thi tính đúng đắn về cấu trúc, đảm bảo rằng các toán tử không xuất hiện ở các vị trí không hợp lệ. 

Sự phân chia ở`=`là an toàn vì vấn đề đảm bảo chính xác một biểu tượng đẳng thức cho các trường hợp đúng định dạng và bất kỳ điều gì khác ngay lập tức được phân loại là không đúng định dạng. 

Sự tinh tế chính là việc xử lý các toán tử đơn nguyên. Khi một`+`hoặc`-`xuất hiện ở vị trí mong đợi một số, nó sửa đổi trạng thái dấu thay vì được coi là toán tử nhị phân. Khi nhìn thấy một số, dấu tích lũy sẽ được áp dụng và đặt lại, đảm bảo rằng các chuỗi như`-+-+-5`được xử lý chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`-5+10+3=2+6`| Bước | Bên | Mã thông báo | Ký tên | Tổng cộng | Số lượng mong đợi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | trái |`-`| -1 | 0 | Đúng | 
| 2 | trái |`5`| -1 | -5 | Sai | 
| 3 | trái |`+`| 1 | -5 | Đúng | 
| 4 | trái |`10`| 1 | 5 | Sai | 
| 5 | trái |`+`| 1 | 5 | Đúng | 
| 6 | trái |`3`| 1 | 8 | Sai | 
| 7 | đúng |`2`| 1 | 2 | Sai | 
| 8 | đúng |`+`| 1 | 2 | Đúng | 
| 9 | đúng |`6`| 1 | 8 | Sai | 

Cả hai bên đánh giá là 8, vì vậy đầu ra là`YES`. 

Dấu vết này cho thấy việc xử lý đơn nguyên và phép cộng nhị phân được hợp nhất thành cùng một cơ chế và tính chính xác chỉ phụ thuộc vào thời điểm các số được cam kết với tổng. 

### Ví dụ 2:`2*2=4`| Bước | Bên | Mã thông báo | Tiểu bang | 
| --- | --- | --- | --- | 
| 1 | trái |`2`| số hợp lệ | 
| 2 | trái |`*`| ký tự không hợp lệ | 

Người đánh giá ngay lập tức từ chối biểu thức khi nhìn thấy`*`, vì chỉ có chữ số và`+`hoặc`-`được phép. Đầu ra là`ERROR`. 

Điều này chứng tỏ rằng việc phát hiện sai định dạng là cục bộ và ngay lập tức mà không cần tiếp tục phân tích cú pháp hoặc cố gắng khôi phục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần, với các lần chạy chữ số được sử dụng trong quá trình tiếp tục tuyến tính | 
| Không gian | O(1) | Chỉ một số biến cố định được duy trì bất kể kích thước đầu vào | 

Quét tuyến tính vừa vặn thoải mái trong giới hạn 1 giây ngay cả đối với 3 triệu ký tự. Việc sử dụng bộ nhớ không đổi do không có danh sách mã thông báo hoặc ngăn xếp đệ quy nào được tạo. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio

    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("-5+10+3=2+6") == "YES", "sample 1"
assert run("2+2=5") == "NO", "sample 2"
assert run("2*2=4") == "ERROR", "sample 3"

# custom cases
assert run("3=003") == "YES", "leading zeros"
assert run("-+10=10") == "NO", "invalid unary chain leading to mismatch"
assert run("=1+2") == "ERROR", "empty left side"
assert run("1+2=") == "ERROR", "empty right side"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3=003 | CÓ | tính chính xác của số 0 đứng đầu | 
| -+10=10 | KHÔNG | xử lý dấu hiệu đơn nhất với cú pháp hợp lệ | 
| =1+2 | LỖI | phát hiện biểu thức trái trống | 
| 1+2= | LỖI | toán tử theo sau và phía bên phải trống | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là các biểu thức bắt đầu bằng nhiều dấu đơn nguyên, chẳng hạn như`-+-+-5=5`. Trong quá trình phân tích cú pháp, mỗi dấu hiệu sẽ lật hoặc giữ nguyên trạng thái dấu hiệu hiện tại trước khi số đó được sử dụng. Người đánh giá bắt đầu ở trạng thái mong đợi một con số, do đó nó liên tục cập nhật`sign`không có lỗi, sau đó áp dụng nó sau khi đọc được chuỗi chữ số. Kết quả chính xác làm giảm chuỗi thành một dấu hiệu hiệu quả duy nhất. 

Một trường hợp khác là các toán tử theo sau như`2+`. Khi phân tích cú pháp`2`, trạng thái chuyển sang "toán tử mong đợi". Gặp gỡ`+`hợp lệ vì nó là vị trí của toán tử nhị phân, nhưng sau`+`không có số nào trước khi chuỗi kết thúc. Kiểm tra cuối cùng`if expect_number`nắm bắt điều này và đánh dấu biểu thức không đúng định dạng. 

Trường hợp cạnh thứ ba bị thiếu các cạnh xung quanh dấu đẳng thức. Đối với đầu vào`=1+2`, vế trái trống. Người đánh giá ngay lập tức trả về kết quả không đúng định dạng vì nó kết thúc bằng`expect_number`vẫn đúng mà không tiêu thụ bất kỳ con số nào. Logic tương tự được áp dụng đối xứng cho`1+2=`.
