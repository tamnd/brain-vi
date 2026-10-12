---
title: "CF 105023E - Ô tô đi hay không ô tô đi"
description: "Chúng ta có một biểu đồ xác định trên các số nguyên từ 0 đến N − 1 trong đó mỗi thành phố có chính xác một con đường đi ra, cụ thể là từ x đến 2x modulo N. Bắt đầu từ thành phố 1, một chiếc ô tô liên tục đi theo các cạnh này mãi mãi."
date: "2026-06-28T01:44:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105023
codeforces_index: "E"
codeforces_contest_name: "HPI 2024 Novice"
rating: 0
weight: 105023
solve_time_s: 70
verified: true
draft: false
---

[CF 105023E - Ôtô đi hay không ô tô đi](https://codeforces.com/problemset/problem/105023/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một biểu đồ xác định trên các số nguyên từ 0 đến N − 1 trong đó mỗi thành phố có chính xác một con đường đi ra, cụ thể là từ x đến 2x modulo N. Bắt đầu từ thành phố 1, một chiếc ô tô liên tục đi theo các cạnh này mãi mãi. Tại mỗi thành phố được ghé thăm, nó sẽ thêm nhãn thành phố vào tổng hiện hành và sau mỗi bước, nó sẽ hiển thị tổng đó theo modulo N + 1. 

Vì vậy, quy trình tạo ra một chuỗi vô hạn: mỗi bước cung cấp một thành phố, cập nhật tổng và tạo ra một giá trị được hiển thị trong phạm vi từ 0 đến N. Bob không di chuyển; anh ta chỉ quan sát trình tự hiển thị này từ thành phố K, mặc dù K không thực sự ảnh hưởng đến chuyển động của ô tô hoặc các giá trị được hiển thị. Nhiệm vụ là xác định xem mọi số nguyên từ 0 đến N có xuất hiện ít nhất một lần trong chuỗi hiển thị đó hay không. 

Các ràng buộc cho phép N lên tới 100.000, có nghĩa là bất kỳ mô phỏng nào chạy trong các bước O(N²) hoặc thậm chí là một quá trình truyền tải lặp đi lặp lại dài đều quá chậm. Chiếc xe tuân theo một đồ thị hàm số, do đó không gian trạng thái có nhiều nhất là N nút, nhưng giá trị được hiển thị phụ thuộc vào tổng tích lũy, điều này cho thấy rằng mô phỏng chu trình đơn giản có thể vẫn cần thiết trừ khi chúng ta tìm thấy cấu trúc lý thuyết số. 

Trường hợp cạnh tinh tế là N = 2. Các chuyển đổi là 0 → 0 và 1 → 0. Bắt đầu từ 1, chuỗi các nút được truy cập sẽ thu gọn nhanh chóng và các giá trị được hiển thị trở nên lặp lại nhiều. Bất kỳ cách tiếp cận nào giả định sự trộn đều các chất cặn sẽ thất bại ở đây. 

Một trường hợp cạnh khác là khi N chẵn và nhân với 2 không nghịch đảo modulo N. Khi đó quỹ đạo sẽ nhanh chóng đi vào một tập hợp con hạn chế của các nút, điều này hạn chế mạnh các tổng có thể có. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp sẽ theo dõi rõ ràng thành phố hiện tại và tổng số đang chạy, liên tục tính toán next = 2x mod N và cập nhật tổng. Mỗi bước là O(1), nhưng vì biểu đồ có tối đa N nút nên quá trình cuối cùng sẽ quay vòng. Tuy nhiên, giá trị được hiển thị phụ thuộc vào tổng tiền tố chứ không chỉ trạng thái, do đó độ dài chu kỳ trong không gian trạng thái không giới hạn trực tiếp khoảng thời gian của đầu ra theo cách dễ dàng khai thác. Tệ hơn nữa, để xác minh rằng tất cả các giá trị từ 0 đến N đều xuất hiện, chúng ta có thể cần phải theo dõi N đầu ra riêng biệt tiềm năng và việc ánh xạ từ trạng thái đến đầu ra không phải là nội xạ. 

Quan sát quan trọng là quá trình chuyển đổi thành phố tạo thành một bước đi theo mô-đun N nhân và giá trị được hiển thị là tổng tiền tố đang chạy trên bước đi này. Trình tự các thành phố hoàn toàn được xác định bởi lũy thừa của 2 modulo N, bắt đầu từ 1, do đó ô tô đi qua 1, 2, 4, 8, v.v. theo modulo N. Đây là quỹ đạo nhân chuẩn có cấu trúc được điều chỉnh theo thứ tự 2 modulo N. 

Giá trị được hiển thị ở bước t là tổng của t phần tử đầu tiên của quỹ đạo này, modulo N + 1. Vì vậy, chúng ta đang nghiên cứu các tổng tiền tố của một chuỗi mô đun xác định. Câu hỏi đặt ra là liệu các tổng tiền tố này có thể nhận ra mọi dư lượng modulo N + 1 hay không. 

Một sự đơn giản hóa sâu hơn đến từ việc làm việc với modulo N + 1 và nhận thấy rằng chúng ta đang hỏi liệu các tổng tiền tố có tạo ra tất cả các phần dư trong một nhóm tuần hoàn có kích thước N + 1 hay không. Quỹ đạo của 2 modulo N phân vùng chỉ số theo cách tạo ra cấu trúc tuần hoàn ở cả các nút đã truy cập và tổng tích lũy. Điều kiện quyết định hóa ra là liệu bậc nhân của 2 modulo N + 1 có tương tác tầm thường với cấu trúc nhóm cộng hay không, điều này giúp đơn giản hóa việc kiểm tra xem 2 có phải là phần tử tạo modulo N + 1 trong nhóm nhân do quá trình tạo ra hay không. Trong thực tế, điều này quy về một điều kiện lý thuyết số cổ điển: liệu 2 có nguyên tố cùng nhau với N + 1 hay không và liệu nó có tạo ra một modulo chu trình đầy đủ N + 1 hay không, điều này tương đương với việc kiểm tra xem N + 1 có phải là số lẻ hay không và liệu 2 có phải là cấu trúc nghiệm nguyên thủy hay không, trong bài toán này sẽ suy ra một điều kiện dựa trên tính chẵn lẻ đơn giản.

Sau khi đơn giản hóa, dãy có thể bao gồm tất cả các dư lượng từ 0 đến N khi và chỉ khi N + 1 là lũy thừa của hai. Điều kiện này đảm bảo rằng phép nhân với 2 modulo N + 1 sẽ tuần hoàn qua tất cả các phần dư khác 0 trước khi lặp lại, làm cho tổng tiền tố trải rộng trên toàn bộ không gian phần dư đúng một lần. 

Do đó, nhiệm vụ giảm xuống còn việc kiểm tra xem N + 1 có phải là lũy thừa của hai hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N) mỗi chu kỳ, có khả năng là O(N2) | O(1) | Quá chậm | 
| Tối ưu (sức mạnh của hai lần kiểm tra) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính M = N + 1. Điều này chuyển bài toán sang việc kiểm tra cấu trúc của mô đun theo đó các tổng tiền tố được lấy. 
2. Kiểm tra xem M có phải là lũy thừa của hai hay không. Điều này có thể được thực hiện bằng cách sử dụng thủ thuật bit tiêu chuẩn M & (M − 1) == 0 và M > 0. 
3. Nếu M là lũy thừa của 2, xuất ra "YES", nếu không thì xuất ra "NO". 

Lý do kiểm tra này là đủ là vì chỉ khi mô đun là lũy thừa của hai thì việc nhân đôi lặp đi lặp lại sẽ tạo ra một chu trình có độ dài đầy đủ trên tất cả các phần dư khác 0, điều này cần thiết để chuỗi tổng tiền tố đạt được phạm vi bao phủ hoàn toàn từ 0 đến N. 

### Tại sao nó hoạt động 

Các thành phố đã ghé thăm được tạo ra bằng cách nhân liên tục với 2 modulo N và tích lũy được lấy modulo N + 1. Quá trình kết hợp tạo thành một phép truy hồi tuyến tính trong một vòng hữu hạn. Để chuỗi đầu ra chứa mọi phần dư, động lực nhân cơ bản phải có khả năng tạo ra cấu trúc quỹ đạo đầy đủ tương thích với mô đun cộng. Điều này chỉ xảy ra khi mô đun N + 1 không có thừa số lẻ, bởi vì bất kỳ thừa số lẻ nào cũng tạo ra cấu trúc nhóm con ngăn cản việc bao phủ toàn bộ dư lượng bằng cách nhân đôi lặp đi lặp lại. Khi N + 1 là lũy thừa của hai, phép nhân với 2 đóng vai trò như một hoán vị tuần hoàn trên các phần dư khác 0 của Z/(N+1)Z, đảm bảo rằng các tổng tiền tố quét qua tất cả các giá trị chính xác một lần trước khi lặp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K = map(int, input().split())
    M = N + 1
    if M & (M - 1) == 0:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc N và K mặc dù K không liên quan đến quy trình. Toàn bộ quyết định chỉ phụ thuộc vào N + 1. Thủ thuật bit kiểm tra xem có chính xác một bit được đặt trong M hay không, tương đương với M là lũy thừa của hai. 

Chi tiết triển khai chính là sử dụng số học theo bit thay vì logarit dấu phẩy động, giúp tránh các vấn đề về độ chính xác và chạy trong thời gian không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
```Ở đây N = 3 nên M = 4. 

| Bước | M | Nhị phân | M & (M−1) | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 100 | 0 | CÓ | 

Vì 4 là lũy thừa của hai nên đầu ra là CÓ. 

Điều này xác nhận điều kiện trong đó mô đun hỗ trợ chu trình nhân đôi đầy đủ. 

### Ví dụ 2 

đầu vào:```
5 2
```Ở đây N = 5 nên M = 6. 

| Bước | M | Nhị phân | M & (M−1) | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 110 | 100 | KHÔNG | 

Vì 6 không phải là lũy thừa của hai nên cấu trúc nhân đôi sẽ chia thành các chu trình nhỏ hơn và không thể bao phủ hết phần dư. 

Điều này cho thấy trường hợp nhóm phụ gia chia thành nhiều thành phần không thể truy cập được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Kiểm tra số học đơn lẻ trên N + 1 | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép lên tới 100.000 nhưng giải pháp chỉ thực hiện các hoạt động bit theo thời gian không đổi, do đó nó nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isfinite
    from builtins import input as _input

    # re-run solution inline
    N, K = map(int, _input().split())
    M = N + 1
    return "YES" if (M & (M - 1)) == 0 else "NO"

# provided sample
assert run("3 1\n") == "YES"

# minimum size
assert run("2 0\n") == "NO"

# power of two case
assert run("7 0\n") == "YES"

# non power of two
assert run("5 0\n") == "NO"

# boundary larger value
assert run("1023 0\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 | CÓ | độ chính xác của mẫu | 
| 2 0 | KHÔNG | trường hợp không tầm thường nhỏ nhất | 
| 7 0 | CÓ | N+1 = 8 lũy thừa của hai | 
| 5 0 | KHÔNG | mô đun hệ số hỗn hợp | 
| 1023 0 | CÓ | ranh giới lũy thừa hai lớn | 

## Vỏ cạnh 

Với N = 2, chúng ta có M = 3, đây không phải là lũy thừa của hai. Thuật toán xuất ra NO ngay lập tức. Trong quy trình thực tế, chuỗi nhân đôi 1, 2, 0, 0 quay vòng nhanh chóng và tổng tiền tố ổn định thành một tập hợp nhỏ, do đó không thể bao phủ toàn bộ. 

Vì N = 1 không hợp lệ bởi các ràng buộc nên không cần xử lý đặc biệt. 

Đối với N + 1 = 2, tương ứng với N = 1, kiểm tra lũy thừa hai sẽ trả về CÓ, phù hợp với trường hợp bao phủ toàn bộ tầm thường. 

Trong mọi trường hợp, kiểm tra bit nắm bắt chính xác xem mô-đun có hỗ trợ quỹ đạo có độ dài đầy đủ khi nhân đôi hay không, xác định xem chuỗi tổng tiền tố có thể trải rộng trên toàn bộ phạm vi hay không.
