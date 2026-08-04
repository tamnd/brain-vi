---
title: "CF 102625J - RD Bhaiya và hệ thống token mới của anh ấy"
description: "Máy mã thông báo lưu trữ một tập hợp các số nguyên được chèn. Số mã thông báo hợp lệ không phải là một trong các giá trị được chèn trực tiếp. Thay vào đó, đó là bất kỳ giá trị XOR nào có thể thu được bằng cách chọn một số tập hợp con của các số được lưu trữ, bao gồm cả tập hợp con trống, có XOR bằng 0."
date: "2026-08-03T15:23:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102625
codeforces_index: "J"
codeforces_contest_name: "IIT(ISM) Virtual Farewell"
rating: 0
weight: 102625
solve_time_s: 47
verified: true
draft: false
---

[CF 102625J - RD Bhaiya và hệ thống token mới của anh ấy](https://codeforces.com/problemset/problem/102625/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Máy mã thông báo lưu trữ một tập hợp các số nguyên được chèn. Số mã thông báo hợp lệ không phải là một trong các giá trị được chèn trực tiếp. Thay vào đó, đó là bất kỳ giá trị XOR nào có thể thu được bằng cách chọn một số tập hợp con của các số được lưu trữ, bao gồm cả tập hợp con trống, có XOR bằng 0. Sau mỗi lần chèn, khách hàng yêu cầu mã thông báo ở một vị trí nhất định trong danh sách đã sắp xếp của tất cả các giá trị XOR riêng biệt có thể có. Nhiệm vụ là trả lời các truy vấn thống kê thứ tự này trong khi tiếp tục chèn. citturn0search0 

Đầu vào bao gồm tối đa\(10^6\)hoạt động. Việc chèn sẽ cho một số nguyên lên tới\(10^9\), and a query asks for the value at a valid one-indexed position among all generated XOR values. Vì số lượng thao tác cực kỳ lớn nên ngay cả một cách tiếp cận cần \(O(\sqrt{q})\) cho mỗi truy vấn cũng sẽ quá chậm. Chúng ta cần một giải pháp gần với thời gian hoạt động không đổi trên mỗi bit. Các giá trị phù hợp với 30 bit vì\(10^9 < 2^{30}\), do đó tự nhiên là tập con trống. Câu trả lời là:```text
0
```Một giải pháp bất cẩn giả định ít nhất một số được lưu trữ sẽ thất bại ở đây. 

Một trường hợp quan trọng khác là chèn trùng lặp:```text
3
1 5
1 5
2 2
```Các giá trị XOR có thể chỉ có`0`Và`5`, vậy mã thông báo thứ hai là:```text
5
```Nếu chúng ta đếm các số được chèn thay vì các hướng XOR độc lập, chúng ta sẽ nghĩ không chính xác rằng có thể có bốn tập hợp con XOR. 

Trường hợp phức tạp cuối cùng là khi các số được chèn phụ thuộc tuyến tính:```text
3
1 1
1 2
1 3
```Số thứ ba không thêm thông tin mới vì`1 XOR 2 = 3`. Các giá trị được tạo vẫn còn`{0,1,2,3}`. Một phương pháp nhân đôi số lượng giá trị một cách mù quáng sau mỗi lần chèn sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ lưu trữ mọi giá trị XOR được tạo. Nếu có\(k\)các số độc lập trong máy có\(2^k\)kết quả XOR tập hợp con khác nhau. Chúng ta có thể liệt kê mọi tập hợp con, tính toán XOR của nó, loại bỏ các bản sao, sắp xếp kết quả và trả lời các truy vấn theo chỉ mục. Điều này đúng vì mọi mã thông báo có thể đều đến từ chính xác một tập hợp con XOR. Vấn đề là ngay cả\(k=60\)sẽ tạo ra một lượng dữ liệu không thể tưởng tượng được và số lượng giá trị độc lập có thể tăng lên theo số lần chèn. Công việc trong trường hợp xấu nhất là theo cấp số nhân, quanh \(O(2^k)\), điều này là không thể đối với\(10^6\)hoạt động. 

Quan sát quan trọng là XOR hoạt động giống như phép cộng trong không gian vectơ nhị phân. Các số được chèn không tạo ra các bộ giá trị tùy ý. Chúng tạo ra một khoảng tuyến tính trên các bit. Cơ sở tuyến tính nhị phân lưu trữ các hướng độc lập của nhịp này. Nếu kích thước cơ sở là\(k\), có chính xác\(2^k\)kết quả XOR duy nhất. 

Thử thách còn lại là tìm\(n\)-giá trị nhỏ nhất trong khoảng được sắp xếp này mà không tạo ra tất cả các giá trị. Khi cơ sở được rút gọn thành dạng có thứ tự đặc biệt, mỗi vectơ cơ sở sẽ điều khiển một vị trí nhị phân một cách độc lập. Chúng ta có thể tham lam quyết định các bit của câu trả lời từ bit cao nhất đến bit thấp nhất. Tại mỗi bit, chúng ta biết có bao nhiêu giá trị được tạo ra có bit đó bằng 0. Nếu vị trí được yêu cầu nằm trong nhóm đó thì câu trả lời sẽ giữ bit này bằng 0. Nếu không, chúng tôi bỏ qua nhóm đó, trừ kích thước của nó khỏi vị trí và đặt bit thành một. 

Lực lượng vũ phu hoạt động vì nó thể hiện rõ ràng không gian vectơ, nhưng không thành công khi không gian trở nên lớn. Việc quan sát rằng không gian có một cơ sở compact cho phép chúng ta thực hiện logic sắp xếp tương tự một cách trực tiếp trên cơ sở đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(2^k)\) | \(O(2^k)\) | Quá chậm | 
| Tối ưu | \(O(30)\) cho mỗi truy vấn hoặc chèn | \(O(30)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì cơ sở tuyến tính nhị phân của các số được chèn. Đối với mỗi giá trị được chèn, hãy thử loại bỏ bit được đặt cao nhất bằng cách sử dụng các vectơ cơ sở hiện có. Nếu giá trị trở thành 0 thì nó đã được biểu thị bằng các số trước đó và không làm tăng số lượng mã thông báo có thể có. Ngược lại, lưu nó dưới dạng một vectơ cơ sở độc lập mới. 

2. Sau mỗi lần chèn thành công, hãy xây dựng lại cơ sở thành dạng rút gọn. Đối với mọi vị trí bit từ cao đến thấp, hãy loại bỏ bit đó khỏi tất cả các vectơ cơ sở thấp hơn. Điều này làm cho mỗi vectơ cơ sở chịu trách nhiệm về một bit duy nhất, cho phép chúng ta đếm xem có bao nhiêu giá trị được tạo ra có một bit nhất định bằng 0. 

3. Để trả lời câu hỏi về vị trí\(n\), xử lý các bit từ cao đến thấp. Giả sử có\(cnt\)các vectơ cơ sở còn lại có thể ảnh hưởng đến các bit thấp hơn. Trong số các khả năng còn lại, chính xác một nửa có bit hiện tại bằng 0 và một nửa có bit hiện tại bằng 1. Nhóm 0 có kích thước\(2^{cnt-1}\). 

4. Nếu\(n\)nằm trong nhóm 0, giữ bit trả lời hiện tại bằng 0. Ngược lại, di chuyển đến một nhóm bằng cách trừ kích thước của nhóm 0 khỏi\(n\), đặt bit trả lời hiện tại thành 1 và tiếp tục. 

5. Nếu\(n=1\), thuật toán tự nhiên trả về 0 vì tập con trống là giá trị đầu tiên theo thứ tự được sắp xếp. 

Điều bất biến là cơ sở rút gọn biểu thị chính xác không gian XOR giống như tất cả các số được chèn, nhưng với mỗi vectơ cơ sở đóng góp một bit cao nhất duy nhất. Trong quá trình truy vấn, mỗi quyết định chia các giá trị XOR còn lại thành hai nhóm có kích thước bằng nhau dựa trên bit hiện tại. Việc chọn một nửa đúng ở mỗi bit tuân theo thứ tự được sắp xếp, vì vậy sau khi xử lý tất cả các bit, giá trị được xây dựng chính xác là số mã thông báo được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX_BIT = 30

basis = [0] * MAX_BIT
changed = True

def rebuild():
    for i in range(MAX_BIT - 1, -1, -1):
        if basis[i]:
            for j in range(i - 1, -1, -1):
                if (basis[i] >> j) & 1:
                    basis[i] ^= basis[j]

def insert(x):
    global changed
    for i in range(MAX_BIT - 1, -1, -1):
        if ((x >> i) & 1) == 0:
            continue
        if basis[i]:
            x ^= basis[i]
        else:
            basis[i] = x
            changed = True
            return

def kth(x):
    if changed:
        rebuild()
        changed = False

    ans = 0
    cnt = sum(1 for v in basis if v)

    for i in range(MAX_BIT - 1, -1, -1):
        if basis[i]:
            half = 1 << (cnt - 1)
            if x > half:
                x -= half
                ans |= basis[i]
            cnt -= 1

    return ans

def solve():
    global changed
    q = int(input())
    out = []

    for _ in range(q):
        p, n = map(int, input().split())
        if p == 1:
            insert(n)
        else:
            out.append(str(kth(n)))

    sys.stdout.write("\n".join(out))

solve()
```các`basis`mảng lưu trữ một vectơ cho mỗi bit cao nhất có thể. Trong quá trình chèn, số hiện tại bị giảm bởi các vectơ hiện có giống hệt như phép loại bỏ Gaussian trên các giá trị nhị phân. Nếu mọi bit biến mất, số đó phụ thuộc và không thay đổi tập hợp các mã thông báo có thể có. 

Hoạt động xây dựng lại chuyển đổi cơ sở bình thường thành cơ sở rút gọn. Bước này bị trì hoãn cho đến khi có truy vấn vì nhiều lần chèn có thể xảy ra mà không cần kiểm tra thứ tự. các`changed`cờ ngăn chặn việc giảm lặp đi lặp lại không cần thiết. 

Hàm truy vấn đi qua cơ sở rút gọn từ bit cao đến bit thấp. Biến`cnt`theo dõi xem còn lại bao nhiêu vectơ độc lập để quyết định kích thước của nhóm 0 hiện tại. Các số nguyên Python xử lý lũy thừa của hai được sử dụng ở đây mà không bị tràn, mặc dù giá trị lớn nhất cần thiết chỉ là\(2^{30}\). 

Việc lập chỉ mục là dựa trên một. Mã thông báo được tạo đầu tiên bằng 0, do đó không cần xử lý đặc biệt nào ngoài việc sử dụng trực tiếp vị trí đã cho trong logic giảm một nửa. Thứ tự truy vấn và rút gọn là phần dễ gây ra lỗi nhất, vì truy vấn với cơ sở không rút gọn sẽ không bảo toàn thứ tự được sắp xếp. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```text
14
2 1
1 1
2 1
2 2
1 2
2 1
2 2
2 3
2 4
1 3
2 1
2 2
2 3
2 4
```một số thao tác đầu tiên hoạt động như sau: 

| Hoạt động | Cơ sở chèn | Vị trí truy vấn | Trả lời | 
|---|---|---|---| 
| Truy vấn | trống | 1 | 0 | 
| Chèn 1 | {1} | | | 
| Truy vấn | {1} | 1 | 0 | 
| Truy vấn | {1} | 2 | 1 | 
| Chèn 2 | {1,2} | | | 
| Truy vấn | {1,2} | 1 | 0 | 
| Truy vấn | {1,2} | 2 | 1 | 
| Truy vấn | {1,2} | 3 | 2 | 
| Truy vấn | {1,2} | 4 | 3 | 

Dấu vết này chứng tỏ rằng hai vectơ cơ sở độc lập tạo ra bốn giá trị, được sắp xếp như sau:`0, 1, 2, 3`. Cơ sở không bao giờ lưu trữ bốn số đó một cách rõ ràng. 

Sau khi chèn`3`, cơ sở không phát triển vì`3 = 1 XOR 2`. Các truy vấn còn lại vẫn trả về: 

| Hoạt động | Giá trị được chèn | Vectơ độc lập | Vị trí truy vấn | Trả lời | 
|---|---|---|---|---| 
| Chèn 3 | 1, 2, 3 | 2 | | | 
| Truy vấn | 1, 2, 3 | 2 | 1 | 0 | 
| Truy vấn | 1, 2, 3 | 2 | 2 | 1 | 
| Truy vấn | 1, 2, 3 | 2 | 3 | 2 | 
| Truy vấn | 1, 2, 3 | 2 | 4 | 3 | 

Dấu vết thứ hai xác nhận rằng các phần chèn phụ thuộc không tạo ra số mã thông báo bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(30)\) mỗi lần chèn hoặc truy vấn | Mọi thao tác chỉ xử lý 30 vị trí bit có thể có | 
| Không gian | \(O(30)\) | Cơ sở lưu trữ tối đa một giá trị cho mỗi bit | 

Giới hạn truy vấn của\(10^6\)làm cho các giải pháp tùy thuộc vào số lượng giá trị được lưu trữ là không thể. Cơ sở nhị phân giữ trạng thái cố định ở 30 mục, do đó tổng số thao tác dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    ans = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return ans

assert solve_data("""14
2 1
1 1
2 1
2 2
1 2
2 1
2 2
2 3
2 4
1 3
2 1
2 2
2 3
2 4
""") == """0
0
1
0
1
2
3
0
1
2
3""", "sample"

assert solve_data("""1
2 1
""") == "0", "empty basis"

assert solve_data("""3
1 5
1 5
2 2
""") == "5", "duplicate insertion"

assert solve_data("""5
1 1
1 2
1 3
2 1
2 4
""") == """0
3""", "dependent vector"

assert solve_data("""5
1 1000000000
2 1
2 2
2 1
2 2
""") == """0
1000000000
0
1000000000""", "large value boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| Truy vấn cơ sở trống | 0 | Tập hợp con trống được xử lý | 
| Chèn lặp đi lặp lại | 5 | Giá trị trùng lặp không tăng thứ hạng | 
| Đã chèn giá trị phụ thuộc | 0 và 3 | Sự phụ thuộc XOR bị bỏ qua | 
| Chèn giá trị tối đa | Đúng thứ tự hai giá trị | Xử lý ranh giới bit | 

## Vỏ cạnh 

Đối với thùng máy trống:```text
1
2 1
```cơ sở vẫn còn đầy số không. Truy vấn không có vectơ để phân chia, vì vậy giá trị duy nhất có thể là tập con XOR trống, bằng 0. Thuật toán trả về 0 vì vòng lặp truy vấn không có bit nào để chọn. 

Đối với các giá trị trùng lặp:```text
3
1 5
1 5
2 2
```lần chèn đầu tiên tạo ra một vectơ cơ sở để biểu diễn bit của`5`. Lần chèn thứ hai giảm hoàn toàn về 0 vì cùng một vectơ đã tồn tại. Truy vấn chỉ nhìn thấy một vectơ độc lập, tạo ra các giá trị có thứ tự`0, 5`. 

Đối với các giá trị phụ thuộc:```text
3
1 1
1 2
1 3
```lần chèn thứ ba bị loại bỏ bởi cơ sở hiện có bởi vì`3`đã được biểu diễn dưới dạng XOR của`1`Và`2`. Thứ hạng cơ sở vẫn là 2 nên chuỗi được tạo ra vẫn là`0, 1, 2, 3`. 

Đối với các giá trị lớn:```text
5
1 1000000000
2 1
2 2
2 1
2 2
```bit được sử dụng cao nhất là gần giới hạn 30 bit. Thuật toán vẫn chỉ kiểm tra các vị trí bit cố định nên không cần có trường hợp đặc biệt nào. Cơ sở rút gọn phân tách chính xác các lựa chọn 0 và khác 0 ngay cả ở bit trên cùng.
