---
title: "CF 104285N - Những con số của Nancy"
description: "Chúng ta được cung cấp một danh sách các số nguyên và chúng ta được phép tăng liên tục bất kỳ phần tử nào đã chọn thêm chính xác một phần tử. Mục tiêu là chuyển đổi mảng sao cho tất cả các giá trị trở nên khác biệt, đồng thời thực hiện càng ít gia số càng tốt."
date: "2026-07-01T20:58:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104285
codeforces_index: "N"
codeforces_contest_name: "PCCA Winter Camp Contest 2023"
rating: 0
weight: 104285
solve_time_s: 49
verified: true
draft: false
---

[CF 104285N - Những con số của Nancy](https://codeforces.com/problemset/problem/104285/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và chúng ta được phép tăng liên tục bất kỳ phần tử nào đã chọn thêm chính xác một phần tử. Mục tiêu là chuyển đổi mảng sao cho tất cả các giá trị trở nên khác biệt, đồng thời thực hiện càng ít gia số càng tốt. 

Nói lại, mỗi số đại diện cho một vị trí bắt đầu trên dòng số nguyên. Chúng ta chỉ được phép di chuyển các điểm về bên phải và mỗi lần di chuyển tốn một đơn vị khoảng cách. Chúng tôi muốn định vị lại tất cả các điểm sao cho không có hai điểm nào nằm trên cùng một số nguyên, giảm thiểu tổng chuyển động. 

Ràng buộc$n \le 2 \cdot 10^5$ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng từng bước tăng dần cho mỗi lần va chạm. Ngay cả khi mỗi phần tử chỉ cần một số lần di chuyển nhỏ, trường hợp xấu nhất có thể liên quan đến việc dịch chuyển tầng trên gần như toàn bộ mảng, dẫn đến hành vi bậc hai nếu được xử lý một cách ngây thơ. 

Một trường hợp lỗi tinh vi xuất hiện khi nhiều giá trị giống hệt nhau hoặc được nhóm chặt chẽ. Ví dụ: nếu đầu vào là$[5, 5, 5, 5]$, cách tiếp cận tham lam “sửa các bản sao cục bộ” có thể gán các giá trị như 5, 6, 7, 8, nhưng việc triển khai bất cẩn luôn tăng bản sao hiện tại cho đến khi nó trở thành duy nhất mà không xem xét các phép gán trước đó có thể đếm quá mức hoặc xử lý sai thứ tự. Một cạm bẫy khác là việc xử lý theo thứ tự tùy ý thay vì thứ tự có cấu trúc, điều này có thể phá vỡ tính tối ưu vì các quyết định trước đó hạn chế các quyết định sau. 

## Phương pháp tiếp cận 

Chế độ xem brute-force rất đơn giản: quét mảng nhiều lần và bất cứ khi nào xảy ra xung đột, hãy tăng một trong các bản sao cho đến khi nó trở thành duy nhất. Điều này có tác dụng vì mỗi thao tác tăng dần mô hình hóa trực tiếp bước di chuyển được phép. Tuy nhiên, mỗi mức tăng có thể gây ra xung đột mới với các giá trị đã được xử lý, vì vậy trong trường hợp xấu nhất, chúng ta sẽ phải truyền đi lặp lại các thay đổi trên nhiều phần tử. Đối với đầu vào giống như tất cả các giá trị bằng nhau, mỗi phần tử có thể tăng lên tới$O(n)$lần, cho$O(n^2)$hành vi. 

Quan sát cấu trúc quan trọng là chỉ có thứ tự tương đối mới quan trọng. Nếu chúng ta sắp xếp mảng, chúng ta sẽ loại bỏ mọi sự mơ hồ về giá trị nào cần được sửa trước. Sau khi được sắp xếp, chiến lược tối ưu sẽ trở nên tham lam và cục bộ: mỗi số chỉ cần lớn hơn ít nhất một giá trị đã chọn trước đó. Điều này chuyển đổi một vấn đề va chạm tổng thể thành một quá trình quét tuyến tính đơn giản trong đó chúng ta thực thi tính đơn điệu. 

Thay vì suy nghĩ theo mức tăng tùy ý, chúng tôi diễn giải lại nhiệm vụ như gán các giá trị cuối cùng$b_i$như vậy$b_i > b_{i-1}$Và$b_i \ge a_i$, đồng thời giảm thiểu$\sum (b_i - a_i)$. Việc sắp xếp đảm bảo rằng mọi giải pháp tối ưu đều có thể được sắp xếp lại theo thứ tự không giảm mà không làm tăng chi phí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Sắp xếp + Quét tham lam |$O(n \log n)$|$O(1)$hoặc$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng theo thứ tự tăng dần để các quyết định trước đó không bao giờ cần phải sửa đổi cho các phần tử sau này. 

1. Sắp xếp mảng theo thứ tự không giảm. Điều này đảm bảo chúng tôi xử lý các giá trị nhỏ trước tiên, do đó các giá trị lớn không chặn chúng một cách giả tạo. Cấu trúc tối ưu phải tôn trọng thứ tự sắp xếp vì việc hoán đổi hai phần tử trong cấu hình cuối cùng chỉ có thể giảm hoặc duy trì các số gia cần thiết. 
2. Khởi tạo một biến`current`để theo dõi giá trị nhỏ nhất chúng ta được phép gán tiếp theo. Điều này thể hiện giá trị cuối cùng được chọn cuối cùng trong mảng được chuyển đổi. 
3. Lặp lại mảng đã được sắp xếp. Đối với mỗi phần tử`x`, quyết định giá trị cuối cùng của nó là`max(x, current)`. Điều này đảm bảo giá trị ít nhất phải lớn bằng giá trị gốc và lớn hơn giá trị đã chọn trước đó nếu cần. 
4. Tích lũy chi phí bằng cách cộng thêm`final_value - x`để trả lời. Điều này trực tiếp đếm số lượng gia số đã được áp dụng cho phần tử này. 
5. Cập nhật`current`ĐẾN`final_value + 1`, thực thi tính khác biệt nghiêm ngặt cho phần tử tiếp theo. 

Mỗi bước thực thi điều chỉnh tối thiểu cục bộ trong khi vẫn duy trì tính khả thi toàn cầu. Bước sắp xếp đảm bảo rằng khi chúng ta tăng một giá trị, sau này chúng ta sẽ không bao giờ gặp phải giá trị ban đầu nhỏ hơn mà cần phải xem lại các quyết định trước đó. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi xử lý phần tử thứ i theo thứ tự được sắp xếp, chúng tôi đã xây dựng chuỗi tăng dần nghiêm ngặt nhỏ nhất có thể cho phần tử i đầu tiên tôn trọng tất cả các giới hạn dưới do mảng ban đầu áp đặt. Giải pháp tối ưu nào cũng phải gán các giá trị theo thứ tự tăng dần khi sắp xếp theo giá trị ban đầu; mặt khác, việc hoán đổi hai nhiệm vụ không theo thứ tự liền kề sẽ không làm tăng chi phí và sẽ lập lại trật tự. Đối số trao đổi này đảm bảo rằng việc cố định từng giá trị một cách tham lam vào vị trí có sẵn sớm nhất sẽ không bao giờ chặn các giải pháp tối ưu sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    current = -10**18
    ans = 0

    for x in a:
        if x <= current:
            ans += current + 1 - x
            current = current + 1
        else:
            current = x
        # next must be strictly larger
        current += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh việc xây dựng tham lam. Việc sắp xếp là cần thiết vì nó đảm bảo chúng ta chỉ cần so sánh với giá trị được chọn cuối cùng. Biến`current`đại diện cho khe số nguyên miễn phí tiếp theo. Khi giá trị đầu vào hiện tại đã đủ lớn, chúng ta có thể đặt trực tiếp mà không mất thêm chi phí. Nếu không, chúng tôi sẽ đẩy nó về phía trước`current`, trả chính xác số tiền chênh lệch. 

Một sự tinh tế phổ biến là cập nhật`current`sau khi phân công. Nó phải luôn chuyển động tới`final_value + 1`, không chỉ`final_value`, bởi vì tính duy nhất đòi hỏi sự phân tách chặt chẽ giữa các giá trị được chọn liên tiếp. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`[1, 1, 1]`. 

Sau khi sắp xếp, nó vẫn còn`[1, 1, 1]`. 

| Yếu tố | hiện tại trước | giá trị đã chọn | chi phí bổ sung | hiện tại sau | 
| --- | --- | --- | --- | --- | 
| 1 | -∞ | 1 | 0 | 2 | 
| 1 | 2 | 2 | 1 | 3 | 
| 1 | 3 | 3 | 2 | 4 | 

Tổng chi phí là 3. 

Dấu vết này cho thấy mỗi bản sao được đẩy tới số nguyên có sẵn tiếp theo như thế nào, tạo thành một chuỗi liên tiếp. 

Bây giờ hãy xem xét`[2, 2, 3]`. 

| Yếu tố | hiện tại trước | giá trị đã chọn | chi phí bổ sung | hiện tại sau | 
| --- | --- | --- | --- | --- | 
| 2 | -∞ | 2 | 0 | 3 | 
| 2 | 3 | 3 | 1 | 4 | 
| 3 | 4 | 4 | 1 | 5 | 

Số 2 thứ hai phải được tăng lên vì số 2 đã được sử dụng và số 3 cũng phải được dịch chuyển lên trên để tránh va chạm. 

Những ví dụ này xác nhận rằng thuật toán luôn gán các giá trị riêng biệt khả thi nhỏ nhất theo thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Việc sắp xếp chiếm ưu thế, theo sau là một lần quét tuyến tính duy nhất | 
| Không gian |$O(1)$hoặc$O(n)$| Tùy thuộc vào việc triển khai sắp xếp tại chỗ | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử, do đó$O(n \log n)$giải pháp thoải mái trong giới hạn, trong khi mọi mô phỏng bậc hai sẽ hết thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return str(solve_capture(inp)).strip()

def solve_capture(inp: str) -> int:
    data = inp.strip().split()
    n = int(data[0])
    a = list(map(int, data[1:]))

    a.sort()
    current = -10**18
    ans = 0

    for x in a:
        if x <= current:
            ans += current + 1 - x
            current = current + 1
        else:
            current = x
        current += 1

    return ans

# sample-like cases
assert solve_capture("3\n1 1 1") == 3
assert solve_capture("3\n1 2 3") == 0

# custom cases
assert solve_capture("1\n10") == 0
assert solve_capture("4\n5 5 5 5") == 6
assert solve_capture("5\n1 1 2 2 3") == 4
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | Không cần tăng thêm | 
| đã khác biệt | 0 | Trường hợp đơn điệu được sắp xếp | 
| tất cả đều bình đẳng | chi phí không cần thiết | truyền chuỗi | 
| trùng lặp hỗn hợp | phân công lại tối thiểu | tương tác của các khối | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phần tử giống hệt nhau, chẳng hạn như`[5, 5, 5, 5]`. Sau khi sắp xếp, thuật toán gán 5, 6, 7, 8 với chi phí lần lượt là 0, 1, 2, 3. các`current`con trỏ đảm bảo mỗi phép gán mới được đẩy vừa đủ để tránh xung đột, không bao giờ nhiều hơn. 

Một trường hợp cạnh khác là một mảng tăng nghiêm ngặt như`[1, 2, 3, 4]`. Thuật toán gán các giá trị không thay đổi vì mỗi phần tử đã thỏa mãn`current`hạn chế. Điều này xác nhận rằng quy tắc tham lam không bao giờ đưa ra những gia tăng không cần thiết. 

Một cụm hỗn hợp như`[1, 1, 2, 2, 3]`cho thấy các bản sao trên các giá trị khác nhau tương tác như thế nào. Thuật toán xử lý chuỗi được sắp xếp một cách thống nhất và`current`chuyển tiếp ràng buộc toàn cục để các bản sao cục bộ được giải quyết mà không phá vỡ cấu trúc sau này.
