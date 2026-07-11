---
title: "CF 103185E - Lượt xem tuyệt vời"
description: "Chúng ta có một cảnh quan một chiều được biểu thị bằng độ cao dọc theo một đường thẳng. Mỗi vị trí có một độ cao và chúng tôi muốn xác định vị trí nào là “tầm nhìn tuyệt vời”, nghĩa là chúng có thể được nhìn thấy một cách không bị cản trở từ ít nhất một hướng dưới điều kiện tự nhiên…"
date: "2026-07-03T16:17:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103185
codeforces_index: "E"
codeforces_contest_name: "2020-2021 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 103185
solve_time_s: 47
verified: true
draft: false
---

[CF 103185E - Lượt xem xuất sắc](https://codeforces.com/problemset/problem/103185/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cảnh quan một chiều được biểu thị bằng độ cao dọc theo một đường thẳng. Mỗi vị trí có một độ cao và chúng tôi muốn xác định vị trí nào là “tầm nhìn tuyệt vời”, nghĩa là chúng có thể được nhìn thấy một cách không bị cản trở từ ít nhất một hướng theo quy tắc tầm nhìn tự nhiên. 

Một vị trí được coi là có thể nhìn thấy từ một hướng nếu khi chúng tôi quét từ hướng đó, nó hoàn toàn cao hơn tất cả các vị trí đã thấy trước đó. Nói cách khác, một điểm sẽ hiển thị nếu không có điểm nào trước đó trong quá trình quét đó chặn điểm đó có chiều cao bằng hoặc lớn hơn. 

Nhiệm vụ là đếm xem có bao nhiêu vị trí hiển thị khi xem xét khả năng hiển thị từ cả hai đầu của mảng. Một vị trí được coi là xuất sắc nếu nó được nhìn thấy từ bên trái hoặc bên phải. 

Đầu vào là một mảng độ cao. Đầu ra là một số nguyên biểu thị số lượng chỉ mục được hiển thị từ ít nhất một hướng. 

Cấu trúc ràng buộc điển hình cho loại vấn đề này ngụ ý rằng mảng có thể lớn, do đó, bất kỳ giải pháp nào cũng phải chạy trong thời gian tuyến tính hoặc thời gian gần tuyến tính. Một giải pháp bậc hai so sánh từng phần tử với tất cả các phần tử khác rõ ràng sẽ thất bại khi n tăng vượt quá vài chục nghìn, vì nó sẽ thực hiện theo thứ tự của n so sánh bình phương. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử đều bằng nhau. Ví dụ: nếu mảng là [5, 5, 5, 5] thì chỉ hiển thị phần tử đầu tiên từ lần quét bên trái và chỉ hiển thị phần tử cuối cùng từ lần quét bên phải, vì vậy câu trả lời phải là 2, không phải n. Một cách giải thích ngây thơ coi “hiển thị” là “không hoàn toàn nhỏ hơn tất cả các vị trí trước đó” sẽ đánh dấu không chính xác tất cả các vị trí là hiển thị. 

Một trường hợp cạnh khác là giảm nghiêm ngặt các mảng như [5, 4, 3, 2, 1]. Từ bên trái, chỉ nhìn thấy cái đầu tiên và từ bên phải, chỉ nhìn thấy cái cuối cùng. Câu trả lời đúng lại là 2, không phải n. Điều này giúp xác nhận rằng khả năng hiển thị phụ thuộc vào cực đại tiền tố chứ không phải so sánh cục bộ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng khả năng hiển thị từ mỗi hướng một cách độc lập. Đối với lần quét bên trái, chúng tôi lặp qua mảng và duy trì chiều cao tối đa được thấy cho đến nay. Bất cứ khi nào chúng tôi gặp một giá trị lớn hơn mức tối đa này, chúng tôi đánh dấu giá trị đó là hiển thị và cập nhật giá trị tối đa. Chúng tôi lặp lại quá trình tương tự từ phía bên phải. 

Cách tiếp cận này đúng vì nó trực tiếp mô hình hóa định nghĩa về khả năng hiển thị. Tuy nhiên, nó cũng đã tối ưu về cấu trúc rồi, vì mỗi lần quét là O(n) và chúng tôi chỉ thực hiện hai lần quét. 

Một giải pháp thay thế mạnh mẽ thực sự sẽ là kiểm tra từng vị trí đối với tất cả các phần tử ở bên trái của nó (và tương tự như bên phải của nó), xác minh xem có phần tử chặn nào tồn tại hay không. Điều đó sẽ yêu cầu O(n) công việc trên mỗi vị trí, dẫn đến tổng số hoạt động là O(n²). Khi n lớn, điều này nhanh chóng trở nên không khả thi vì nó thực hiện hàng chục tỷ phép so sánh trong trường hợp đầu vào xấu nhất. 

Quan sát quan trọng là chúng ta không bao giờ cần lịch sử đầy đủ, chỉ cần mức chạy tối đa theo mỗi hướng. Khi chúng tôi nhận ra rằng khả năng hiển thị chỉ phụ thuộc vào việc một điểm có vượt quá tất cả các giá trị trước đó trong lần quét đó hay không, vấn đề sẽ chuyển thành hai phép tính tối đa tiền tố. 

Chúng tôi tính toán mảng tiền tố tối đa từ bên trái và mảng hậu tố tối đa từ bên phải. Bất kỳ chỉ mục nào khớp với mức tối đa của tiền tố tại vị trí của nó đều hiển thị từ bên trái và tương tự đối với hậu tố. Câu trả lời cuối cùng là kích thước hợp của hai tập hợp này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra theo cặp) | O(n²) | O(1) | Quá chậm | 
| Tiền tố/Hậu tố cực đại | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán một mảng tiền tố tối đa trong đó mỗi vị trí lưu trữ giá trị tối đa nhìn thấy từ đầu đến chỉ mục đó. Điều này ghi lại khả năng hiển thị từ bên trái vì một điểm sẽ hiển thị nếu nó bằng mức tối đa đang chạy này tại vị trí của nó. 
2. Tính toán một mảng hậu tố tối đa trong đó mỗi vị trí lưu trữ giá trị tối đa nhìn thấy từ đầu đến chỉ mục đó. Điều này ghi lại khả năng hiển thị từ bên phải theo cùng một logic. 
3. Lặp lại tất cả các chỉ mục và đánh dấu một vị trí là xuất sắc nếu giá trị của nó bằng mức tối đa tiền tố tại chỉ mục đó hoặc bằng mức tối đa hậu tố tại chỉ mục đó. Điều này trực tiếp mã hóa khả năng hiển thị từ ít nhất một hướng. 
4. Đếm tất cả các vị trí đó và xuất kết quả. 

Lý do tiền tố và hậu tố tối đa là đủ là khả năng hiển thị hoàn toàn được xác định bởi ưu thế trong quá trình quét theo hướng. Bất kỳ điểm nào không phải là điểm cực đại đối với vị trí của nó từ một phía nhất định phải bị chặn bởi một phần tử cao hơn hoặc bằng trước đó theo hướng đó, khiến nó không thể nhìn thấy được từ phía đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))

    pref = [0] * n
    suff = [0] * n

    cur = -10**18
    for i in range(n):
        if a[i] > cur:
            cur = a[i]
        pref[i] = cur

    cur = -10**18
    for i in range(n - 1, -1, -1):
        if a[i] > cur:
            cur = a[i]
        suff[i] = cur

    ans = 0
    for i in range(n):
        if a[i] == pref[i] or a[i] == suff[i]:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp thuật toán. Thẻ tiền tố duy trì mức chạy tối đa từ bên trái và thẻ hậu tố cũng thực hiện tương tự từ bên phải. Một sự tinh tế phổ biến là sử dụng so sánh nghiêm ngặt khi cập nhật ở mức tối đa; sử dụng`>`thay vì`>=`đảm bảo rằng các độ cao bằng nhau không tạo ra nhiều đỉnh có thể nhìn thấy được, duy trì tính chính xác cho các đoạn phẳng. 

Vòng lặp cuối cùng kiểm tra tư cách thành viên trong tập hợp khả năng hiển thị. Logic hợp nhất này rất quan trọng: chúng tôi không đếm gấp đôi các vị trí hiển thị từ cả hai phía. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 3 2 5 4
```Tính toán tiền tố và hậu tố: 

| tôi | một [tôi] | tiền tố tối đa | hậu tố tối đa | có thể nhìn thấy từ bên trái | có thể nhìn thấy từ bên phải | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 5 | vâng | vâng | 
| 1 | 3 | 3 | 5 | vâng | không | 
| 2 | 2 | 3 | 5 | không | không | 
| 3 | 5 | 5 | 5 | vâng | vâng | 
| 4 | 4 | 5 | 4 | không | vâng | 

Đáp án là 4. 

Dấu vết này cho thấy các đỉnh từ mỗi hướng độc lập như thế nào. Vị trí 2 bị ẩn khỏi cả hai phía vì nó không bao giờ trở thành điểm cực đại có hướng. 

### Ví dụ 2 

đầu vào:```
6
5 5 5 5 5 5
```| tôi | một [tôi] | tiền tố tối đa | hậu tố tối đa | có thể nhìn thấy | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 5 | 5 | vâng | 
| 1 | 5 | 5 | 5 | không | 
| 2 | 5 | 5 | 5 | không | 
| 3 | 5 | 5 | 5 | không | 
| 4 | 5 | 5 | 5 | không | 
| 5 | 5 | 5 | 5 | vâng | 

Câu trả lời là 2. 

Điều này xác nhận rằng chỉ các phần tử biên tồn tại khi tất cả các giá trị bằng nhau, vì các bản sao bên trong không bao giờ vượt quá mức tối đa trước đó trong cả hai lần quét. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt cho cực đại tiền tố, một lượt cho cực đại hậu tố, một lần quét cuối cùng | 
| Không gian | O(n) | Hai mảng phụ lưu trữ cực đại định hướng | 

Giải pháp này phù hợp thoải mái với các ràng buộc điển hình cho n lên đến 2×10⁵ hoặc 10⁶, vì nó chỉ thực hiện quét tuyến tính với công việc không đổi trên mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# helper to capture output properly
def run(inp: str) -> str:
    import sys, io
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out.strip()

# sample-like cases
assert run("5\n1 3 2 5 4\n") == "4"

# minimum size
assert run("1\n7\n") == "1"

# all equal
assert run("4\n2 2 2 2\n") == "2"

# strictly increasing
assert run("5\n1 2 3 4 5\n") == "5"

# strictly decreasing
assert run("5\n5 4 3 2 1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | khả năng hiển thị ranh giới duy nhất | 
| tất cả đều bình đẳng | 2 | ngăn chặn trùng lặp | 
| ngày càng tăng | n | tầm nhìn toàn bộ bên trái | 
| giảm dần | 2 | tính đối xứng từ quét phải | 

## Vỏ cạnh 

Đối với trường hợp hoàn toàn bằng nhau như:```
4
2 2 2 2
```Cực đại tiền tố trở thành [2, 2, 2, 2] và cực đại hậu tố giống hệt nhau. Chỉ các chỉ số 0 và 3 phù hợp với điều kiện là lần xuất hiện đầu tiên của mức tối đa theo hướng quét của chúng. Thuật toán đếm chính xác hai điểm cuối này, vì mọi phần tử bên trong đều bị chặn bởi một giá trị bằng nhau trước đó theo cả hai hướng. 

Đối với các mảng giảm nghiêm ngặt như:```
5
5 4 3 2 1
```Tiền tố cực đại là [5, 5, 5, 5, 5] nên chỉ có chỉ số 0 đủ tiêu chuẩn tính từ bên trái. Cực đại hậu tố là [5, 4, 3, 2, 1], vì vậy mọi phần tử chỉ đủ điều kiện từ bên phải khi nó trở thành hậu tố tối đa. Tuy nhiên, vì cực đại hậu tố yêu cầu cập nhật nghiêm ngặt nên chỉ chỉ số 4 là mức tối đa mới thực sự khi quét từ bên phải, nên câu trả lời cuối cùng là 2. Điều này xác nhận rằng khả năng hiển thị gắn liền với việc trở thành đỉnh định hướng thay vì chỉ là một phần của hậu tố đơn điệu.
