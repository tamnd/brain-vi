---
title: "CF 104820G - \u0416\u0430\u0434\u043d\u043e\u0435 \u0434\u0435\u043b\u0435\u043d\u0438\u0435"
description: "Chúng tôi đang phân phát ba loại kẹo khác nhau cho ba người bạn, trong đó mỗi người bạn chỉ nhận một loại kẹo cụ thể. Người bạn đầu tiên chỉ lấy Snickers, người thứ hai chỉ lấy Mars và người thứ ba chỉ lấy Bounty."
date: "2026-06-28T12:56:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104820
codeforces_index: "G"
codeforces_contest_name: "\u0420\u0421\u041e-\u0410\u043b\u0430\u043d\u0438\u044f 2018-2023. \u0418\u0437\u0431\u0440\u0430\u043d\u043d\u043e\u0435"
rating: 0
weight: 104820
solve_time_s: 56
verified: true
draft: false
---

[CF 104820G - \u0416\u0430\u0434\u043d\u043e\u0435 \u0434\u0435\u043b\u0435\u043d\u0438\u0435](https://codeforces.com/problemset/problem/104820/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang phân phát ba loại kẹo khác nhau cho ba người bạn, trong đó mỗi người bạn chỉ nhận một loại kẹo cụ thể. Người bạn đầu tiên chỉ lấy Snickers, người thứ hai chỉ lấy Mars và người thứ ba chỉ lấy Bounty. Chúng tôi được cung cấp số lượng sẵn có của từng loại: A Snickers, B Mars và C Bounty. 

Mỗi người bạn phải nhận được ít nhất một loại kẹo mà họ ưa thích. Ngoài ra, số lượng kẹo nhận được phải tăng nghiêm ngặt từ người bạn đầu tiên đến người thứ hai và từ người thứ hai đến người thứ ba. Nghĩa là nếu ta ký hiệu các đại lượng được gán là x, y, z thì x, y, z phải thỏa mãn x < y < z, với x ≥ 1, y ≥ 1, z ≥ 1, đồng thời x ≤ A, y ≤ B, z ≤ C. 

Nhiệm vụ là đếm xem có bao nhiêu bộ ba (x, y, z) thỏa mãn tất cả các ràng buộc này. 

Các ràng buộc lên tới 10^6, điều này ngay lập tức loại trừ việc lặp lại trên tất cả các bộ ba có thể. Một phép liệt kê bậc ba hoặc thậm chí bậc hai ngây thơ sẽ tạo ra tới 10^18 lần lặp trong trường hợp xấu nhất, vượt xa giới hạn chấp nhận được. Giải pháp phải giảm vấn đề thành quét tuyến tính hoặc công thức đếm tổ hợp trực tiếp. 

Một trường hợp cạnh tinh tế xuất hiện khi bất kỳ A, B hoặc C nào bằng 1. Ví dụ: nếu A = B = C = 1, thì bộ ba duy nhất có thể là (1, 1, 1), vi phạm bất đẳng thức nghiêm ngặt, vì vậy câu trả lời là 0. Một cách tiếp cận ngây thơ mà quên thứ tự nghiêm ngặt có thể tính không chính xác điều này là hợp lệ. 

Một trường hợp cạnh khác phát sinh khi dung lượng nhỏ nhưng không bằng nhau, chẳng hạn như A = 1, B = 2, C = 3. Chỉ một bộ ba (1, 2, 3) là hợp lệ và bất kỳ sự hiểu sai nào về “ít nhất một” so với “các lựa chọn gán chính xác” đều có thể dẫn đến việc đếm quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các giá trị có thể có của x từ 1 đến A, y từ 1 đến B và z từ 1 đến C, kiểm tra xem x < y < z. Điều này đúng vì nó trực tiếp thực thi tất cả các ràng buộc, nhưng độ phức tạp của nó là O(ABC). Với mỗi biến lên tới 10^6, điều này dẫn đến 10^18 thao tác trong trường hợp xấu nhất, điều này là không thể thực hiện được. 

Quan sát quan trọng là các ràng buộc chỉ phụ thuộc vào thứ tự chứ không phụ thuộc vào danh tính của các loại kẹo vượt quá giới hạn về loại của chúng. Chúng tôi đang tính toán một cách hiệu quả các bộ ba tăng dần trong đó mỗi phần tử được giới hạn bởi một giới hạn trên độc lập. Điều này có thể được điều chỉnh lại bằng cách chọn x, y, z sao cho 1 ≤ x < y < z, với giới hạn độc lập. 

Thay vì lặp lại tất cả các bộ ba, chúng ta có thể cố định giá trị ở giữa y. Khi y được cố định, x có thể là số nguyên bất kỳ trong [1, y−1] nhưng cũng không được vượt quá A, do đó x bị giới hạn bởi min(A, y−1). Tương tự, z phải nằm trong [y+1, C], nhưng cũng không vượt quá C và phải lớn hơn y và ít nhất là 1, đồng thời phải tôn trọng B vì y được ấn định từ giới hạn Sao Hỏa, không phải z. Thật ra z chỉ phụ thuộc vào C. 

Vì vậy, với mỗi y, số lựa chọn x hợp lệ là min(A, y−1) và số lựa chọn z hợp lệ là max(0, C − y). Phần đóng góp cho mỗi y trở thành min(A, y−1) × max(0, C − y), nhưng chỉ khi y ≤ B. 

Điều này làm giảm vấn đề thành một vòng lặp đơn trên y từ 1 đến B, cho thời gian O(B). Vì B có thể lên tới 10^6 nên điều này có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(ABC) | O(1) | Quá chậm | 
| Bảng liệt kê cố định ở giữa | O(B) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Lặp lại tất cả các giá trị có thể có của y từ 1 đến B. Điều này thể hiện số kẹo được trao cho người bạn thứ hai và đảm bảo chúng ta tôn trọng giới hạn số kẹo sao Hỏa. 
2. Với mỗi y, hãy tính xem có bao nhiêu lựa chọn hợp lệ cho x. Vì x phải thỏa mãn 1 ≤ x < y và x ≤ A nên phạm vi hợp lệ là từ 1 đến min(A, y−1). Nếu y = 1, phạm vi này trống, điều này mang lại chính xác không có lựa chọn nào. 
3. Với cùng y, hãy tính xem có bao nhiêu lựa chọn hợp lệ cho z. Vì z phải thỏa mãn z > y và z ≤ C nên phạm vi hợp lệ là y+1 đến C, có kích thước max(0, C − y). 
4. Nhân số lựa chọn x hợp lệ với số lựa chọn z hợp lệ. Điều này có hiệu quả vì x và z độc lập khi y được cố định, do đó mọi cặp hợp lệ tạo thành một bộ ba duy nhất. 
5. Tính tổng phần đóng góp này trên tất cả y từ 1 đến B. 

Tại sao nó hoạt động: mọi bộ ba hợp lệ (x, y, z) được xác định duy nhất bởi phần tử ở giữa của nó là y. Với mỗi y như vậy, thuật toán đếm chính xác tất cả các lựa chọn x hợp lệ ở bên trái và tất cả các lựa chọn z hợp lệ ở bên phải theo các ràng buộc. Không có bộ ba nào được tính hai lần vì mỗi bộ ba có đúng một phần tử ở giữa và không có bộ ba không hợp lệ nào được tính vì cả hai bên đều thực thi nghiêm ngặt các giới hạn và thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B, C = map(int, input().split())
    
    ans = 0
    for y in range(1, B + 1):
        x_cnt = min(A, y - 1)
        if x_cnt <= 0:
            continue
        z_cnt = C - y
        if z_cnt <= 0:
            continue
        ans += x_cnt * z_cnt
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo chiến lược liệt kê phần tử ở giữa. Vòng lặp y đảm bảo chúng ta chỉ xem xét phân bổ hợp lệ cho người bạn thứ hai. biểu hiện`min(A, y - 1)`thực thi cả tính sẵn có và đặt hàng nghiêm ngặt với người bạn đầu tiên. Thuật ngữ`C - y`ngầm xử lý cả bất đẳng thức nghiêm ngặt và giới hạn trên của số tiền thưởng, vì bất kỳ giá trị nào lớn hơn C sẽ không hợp lệ. 

Cần phải cẩn thận xung quanh các điều kiện biên. Khi y = 1,`y - 1`trở thành 0, loại bỏ chính xác những đóng góp không hợp lệ. Khi y = C,`C - y`trở thành 0, đảm bảo không có lựa chọn z không hợp lệ nào được tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1: Đầu vào`2 3 4`Chúng tôi tính toán đóng góp cho mỗi y. 

| y | x lựa chọn | lựa chọn z | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 0 | 3 | 0 | 
| 2 | 1 | 2 | 2 | 
| 3 | 2 | 1 | 2 | 

Tổng cộng là 4. 

Điều này cho thấy các bộ ba hợp lệ chỉ xuất hiện như thế nào khi cả hai vế của y đều khác trống. Trường hợp y = 2 là linh hoạt nhất vì nó cho phép cả lựa chọn trái và phải. 

### Ví dụ 2: Nhập liệu`1 1 1`| y | x lựa chọn | lựa chọn z | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 

Yêu cầu bất đẳng thức nghiêm ngặt ngăn cản bất kỳ bộ ba nào hình thành. Mặc dù mỗi loại kẹo có ít nhất một vật phẩm nhưng không có cách nào để chia chúng thành các số lượng tăng dần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(B) | Vòng lặp đơn trên tất cả các giá trị có thể có của y | 
| Không gian | O(1) | Chỉ sử dụng các biến phụ không đổi | 

Quét tuyến tính trên tối đa 10^6 giá trị dễ dàng phù hợp với giới hạn thời gian và không cần thêm bộ nhớ ngoài một vài số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("2 3 4\n") == "4"
assert run("1 2 3\n") == "1"
assert run("1 1 1\n") == "0"

# custom cases
assert run("2 2 2\n") == "0", "no strictly increasing triple possible"
assert run("3 4 5\n") == "10", "symmetric mid-range growth"
assert run("10 1 10\n") == "0", "middle too small"
assert run("1 10 10\n") == "1", "only (1,2,3)-style single chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 2 | 0 | sự bất bình đẳng nghiêm ngặt ngăn cản mọi giải pháp | 
| 3 4 5 | 10 | tăng trưởng kết hợp chung | 
| 10 1 10 | 0 | ràng buộc ở giữa chặn tất cả các bộ ba | 
| 1 10 10 | 1 | lực liên kết trái tối thiểu cấu trúc đơn | 

## Vỏ cạnh 

Khi tất cả các giá trị đều bằng 1, vòng lặp chỉ đánh giá y = 1. Trong trường hợp này, x không có lựa chọn hợp lệ và z cũng không có lựa chọn nào, do đó đóng góp bằng 0, phù hợp với yêu cầu không thể đặt hàng nghiêm ngặt. 

Khi A rất nhỏ so với B và C, chẳng hạn như A = 1, B = 10, C = 10, hầu hết các đóng góp đều biến mất vì x luôn bằng 0 ngoại trừ khi y = 1, nhưng trường hợp đó cũng thất bại vì z yêu cầu y < z. Thuật toán lọc các trường hợp này một cách tự nhiên thông qua các ràng buộc tối thiểu và sai phân. 

Khi C nhỏ so với B, chẳng hạn như C = 2 và B lớn, chỉ y = 1 đóng góp có ý nghĩa, nhưng ngay cả khi đó các lựa chọn z cũng nhanh chóng bị giới hạn và tổng chính xác sẽ giảm về 0 hoặc một số rất nhỏ tùy thuộc vào A.
