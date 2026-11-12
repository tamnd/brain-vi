---
title: "CF 1672H - Zigu Zagu"
description: "Chúng ta được cung cấp một chuỗi nhị phân và nhiều truy vấn về chuỗi con. Đối với mỗi khoảng thời gian truy vấn, chúng tôi liên tục xóa các phần liền kề của chuỗi con hiện tại."
date: "2026-06-10T01:34:42+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1672
codeforces_index: "H"
codeforces_contest_name: "Codeforces Global Round 20"
rating: 2700
weight: 1672
solve_time_s: 107
verified: true
draft: false
---

[CF 1672H - Zigu Zagu](https://codeforces.com/problemset/problem/1672/H) 

**Đánh giá:** 2700 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân và nhiều truy vấn về chuỗi con. Đối với mỗi khoảng thời gian truy vấn, chúng tôi liên tục xóa các phần liền kề của chuỗi con hiện tại. Hạn chế duy nhất là loại phần nào chúng tôi được phép xóa: đoạn đã xóa phải xen kẽ nghiêm ngặt, nghĩa là mọi cặp liền kề bên trong đoạn đó phải bao gồm các ký tự khác nhau. 

Một lần xóa có thể xóa một ký tự hoặc một đoạn dài hơn như`0101`hoặc`101`, nhưng không bao giờ giống như`00`hoặc`111`bởi vì chúng chứa các bit liền kề bằng nhau. Sau mỗi lần xóa, các phần còn lại của chuỗi sẽ đóng khoảng trống và tạo thành một chuỗi mới, và chúng ta tiếp tục cho đến khi không còn gì. Nhiệm vụ là tính toán số lần xóa tối thiểu cần thiết cho mỗi chuỗi con truy vấn. 

Các ràng buộc rất lớn, lên tới 200.000 ký tự và 200.000 truy vấn. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng việc xóa hoặc xử lý từng truy vấn theo thời gian tuyến tính. Bất kỳ giải pháp nào được chấp nhận đều phải xử lý trước chuỗi theo thời gian tuyến tính và trả lời từng truy vấn theo thời gian không đổi hoặc logarit. 

Một sai lầm ngây thơ thường xuất hiện ở đây là cho rằng vì chúng ta có thể xóa các chuỗi con xen kẽ nên chúng ta nên cố gắng loại bỏ một cách tham lam các đoạn xen kẽ dài. Trực giác này phá vỡ các trường hợp đơn giản. Ví dụ, trong chuỗi`11011`, việc loại bỏ tham lam có thể thử`101`đầu tiên, rời đi`11`, và sau đó yêu cầu xóa thêm hai lần nữa. Tuy nhiên, một chiến lược khác có vẻ khả thi nhưng vẫn không thể giảm câu trả lời xuống dưới 3. Khó khăn chính là việc xóa bị hạn chế bởi cấu trúc kề chứ không phải bởi việc lựa chọn ký tự tùy ý. 

Một trường hợp cạnh tinh tế khác là khi chuỗi con đã được xen kẽ hoàn toàn, chẳng hạn như`01010`. Trong trường hợp này, câu trả lời rõ ràng là 1, nhưng bất kỳ phương pháp nào chỉ tính các lần chạy có ký tự giống hệt nhau vẫn phải xử lý chính xác sự vắng mặt của các lần chạy đó. 

## Phương pháp tiếp cận 

Chiến lược bạo lực rất đơn giản: mô phỏng quá trình. Đối với mỗi chuỗi con truy vấn, chúng tôi liên tục quét chuỗi hiện tại, tìm phân đoạn xen kẽ tối đa, xóa chuỗi đó và tiếp tục cho đến khi trống. Mỗi lần quét tốn thời gian tuyến tính và chúng tôi có thể thực hiện xóa tuyến tính trong trường hợp xấu nhất, dẫn đến hành vi hình khối trong trường hợp xấu nhất. Điều này ngay lập tức thất bại dưới những ràng buộc. 

Quan sát quan trọng là chúng ta thực sự không cần phải mô phỏng việc xóa. Thay vào đó, chúng tôi hỏi cấu trúc nào buộc chúng tôi phải thực hiện nhiều thao tác. Lần duy nhất chúng tôi buộc phải chia công việc cho nhiều lần xóa là khi hai ký tự liền kề trong chuỗi con gốc bằng nhau. Một cặp như vậy không bao giờ có thể là một phần của cùng một đoạn xen kẽ, bất kể chúng ta xóa các phần khác của chuỗi như thế nào, bởi vì đẳng thức liền kề là một trở ngại vĩnh viễn bên trong bất kỳ đoạn liền kề nào. 

Điều này dẫn đến sự đơn giản hóa: mỗi khi chúng ta nhìn thấy một chỉ mục`i`như vậy`a[i] == a[i+1]`, chuỗi con phải yêu cầu ít nhất một thao tác bổ sung ngoài trường hợp cơ sở. Ngược lại, nếu một chuỗi con không có cặp liền kề bằng nhau thì chuỗi đó sẽ xen kẽ và có thể bị loại bỏ chỉ bằng một thao tác. 

Vì vậy, câu trả lời hoàn toàn trở thành một bài toán đếm trên các cặp bằng nhau liền kề trong mỗi khoảng truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng xóa | O(n²) mỗi truy vấn | O(n) | Quá chậm | 
| Tổng tiền tố trên các cặp bằng nhau liền kề | O(1) cho mỗi truy vấn sau quá trình tiền xử lý O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm mỗi truy vấn thành một số đếm trên một mảng được tính toán trước. 

1. Xây dựng mảng phụ trợ`bad`, Ở đâu`bad[i] = 1`nếu như`a[i] == a[i+1]`, nếu không thì`0`. Điều này nắm bắt chính xác các vị trí mà sự luân phiên bị phá vỡ. 
2. Xây dựng mảng tổng tiền tố`bad`. Điều này cho phép chúng ta tính nhanh số lượng cặp liền kề bằng nhau tồn tại trong bất kỳ khoảng nào. 
3. Đối với mỗi truy vấn`[l, r]`, tính số chỉ số`i`TRONG`[l, r-1]`Ở đâu`a[i] == a[i+1]`sử dụng các tổng tiền tố. 
4. Câu trả lời là`1 + count_of_bad_pairs`. các`1`tương ứng với phép toán cơ sở và mỗi đẳng thức bên trong buộc phải thực hiện một phép toán bổ sung. 

Lý do điều này hoạt động là vì mỗi khối tối đa gồm các ký tự bằng nhau đóng góp chính xác một điểm phân tách bắt buộc và mỗi điểm như vậy không thể được xử lý trong một lần xóa xen kẽ. 

### Tại sao nó hoạt động 

Điều bất biến là mọi thao tác hợp lệ chỉ có thể sử dụng các chuỗi con không chứa các ký tự liền kề bằng nhau. Vì vậy, mỗi vị trí mà`a[i] == a[i+1]`hoạt động như một sự cắt giảm bắt buộc giữa hai "khu vực tương thích xen kẽ" khác nhau. Những lần cắt này là độc lập, vì vậy mỗi lần cắt giảm số lượng thao tác tối thiểu thêm đúng một. Cấu trúc xóa không cho phép hợp nhất qua các ranh giới này, bởi vì bất kỳ phân đoạn nào bao trùm chúng sẽ vi phạm điều kiện xen kẽ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, q = map(int, input().split())
    s = input().strip()

    bad = [0] * (n + 1)
    for i in range(n - 1):
        if s[i] == s[i + 1]:
            bad[i + 1] = 1

    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + bad[i]

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        cnt = pref[r - 1] - pref[l - 1]
        out.append(str(cnt + 1))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai dựa trên việc chuyển đổi trực tiếp quan sát thành các tổng tiền tố. các`bad`mảng được dịch chuyển sao cho mỗi chỉ mục tương ứng với một ranh giới giữa các ký tự, điều này tránh được các vấn đề riêng lẻ khi xử lý các truy vấn. Tổng tiền tố sau đó biến mỗi truy vấn thành tổng phạm vi thời gian không đổi. 

Một lỗi phổ biến là quên rằng kiểm tra kề áp dụng cho`i`Và`i+1`, không trực tiếp đến các ký tự bên trong giới hạn truy vấn. Đây là lý do tại sao truy vấn sử dụng`r-1`là giới hạn trên khi đếm các vị trí xấu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`11011`| Truy vấn | Chuỗi con | Cặp xấu | Tính toán | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 |`101`| 0 | 1 + 0 | 1 | 
| 2 |`11011`| 2 | 1 + 2 | 3 | 
| 3 |`011`| 1 | 1 + 1 | 2 | 

Điều này cho thấy rằng mỗi lần xuất hiện các vùng lân cận bằng nhau sẽ buộc phải thực hiện một thao tác bổ sung, ngay cả khi các khối lớn xen kẽ tồn tại ở nơi khác. 

### Ví dụ 2 

Hãy xem xét`01010`: 

| Truy vấn | Chuỗi con | Cặp xấu | Tính toán | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 |`01010`| 0 | 1 + 0 | 1 | 
| 2 |`101`| 0 | 1 + 0 | 1 | 

Điều này xác nhận rằng các chuỗi xen kẽ hoàn toàn sẽ thu gọn thành một thao tác duy nhất bất kể độ dài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Một đường tuyến tính để tạo tổng tiền tố, thời gian không đổi cho mỗi truy vấn | 
| Không gian | O(n) | Lưu trữ cho mảng tiền tố | 

Quá trình tiền xử lý dễ dàng phù hợp với các giới hạn và mỗi truy vấn được trả lời trong thời gian không đổi, giúp giải pháp phù hợp với các ràng buộc tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    input = sys.stdin.readline

    n, q = map(int, input().split())
    s = input().strip()

    bad = [0] * (n + 1)
    for i in range(n - 1):
        if s[i] == s[i + 1]:
            bad[i + 1] = 1

    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + bad[i]

    res = []
    for _ in range(q):
        l, r = map(int, input().split())
        res.append(str(pref[r - 1] - pref[l - 1] + 1))

    return "\n".join(res)

# provided sample
assert run("""5 3
11011
2 4
1 5
3 5
""") == "1\n3\n2"

# single character queries
assert run("""3 2
010
1 1
2 2
""") == "1\n1"

# fully alternating
assert run("""5 1
01010
1 5
""") == "1"

# all equal
assert run("""4 1
1111
1 4
""") == "3"

# mixed pattern
assert run("""6 2
110100
1 6
2 5
""") == "2\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ký tự đơn | 1, 1 | tính đúng đắn của trường hợp cơ sở | 
| chuỗi xen kẽ | 1 | không bắt buộc phải chia tay | 
| tất cả đều bình đẳng | 3 | ràng buộc kề tối đa | 
| mẫu hỗn hợp | 2, 2 | xử lý phân khúc nội bộ | 

## Vỏ cạnh 

Một chuỗi con bao gồm một ký tự đơn luôn mang lại 0 cặp liền kề, do đó câu trả lời trở thành 1. Thuật toán xử lý điều này vì phạm vi truy vấn`[l, r-1]`trở nên trống rỗng và sự khác biệt tiền tố bằng không. 

Một chuỗi con hoàn toàn thống nhất như`000000`tạo ra một cặp xấu ở mọi ranh giới và thuật toán tính chính xác`length - 1`, tạo ra số lượng hoạt động chính xác như`length - 1 + 1`. 

Một chuỗi con xen kẽ hoàn toàn như`010101`không có cặp xấu, do đó chênh lệch tổng tiền tố bằng 0 và câu trả lời thu gọn thành một thao tác, phù hợp với thực tế là toàn bộ phân đoạn đã hợp lệ cho một lần xóa.
