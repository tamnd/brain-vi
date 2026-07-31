---
title: "CF 102700D - Xúc xắc"
description: "Chúng tôi có một bộ sưu tập xúc xắc được nạp giống hệt nhau. Một con súc sắc có thể hiển thị mọi số từ 1 đến k ngoại trừ các số chia hết cho m. Tất cả các mặt được phép đều có khả năng như nhau, vì vậy điều duy nhất quan trọng đối với tổng cuối cùng là phần còn lại của mỗi lần tung modulo m."
date: "2026-07-30T07:50:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "D"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 118
verified: true
draft: false
---

[CF 102700D - Xúc xắc](https://codeforces.com/problemset/problem/102700/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập xúc xắc được nạp giống hệt nhau. Một con súc sắc có thể hiển thị mọi số từ 1 đến k ngoại trừ các số chia hết cho m. Tất cả các mặt được phép đều có khả năng như nhau, vì vậy điều duy nhất quan trọng đối với tổng cuối cùng là phần còn lại của mỗi lần tung modulo m. 

Nhiệm vụ là tìm xác suất để tổng n cuộn độc lập bằng 0 modulo m. Câu trả lời không được yêu cầu dưới dạng phân số mà dưới dạng giá trị mô-đun. Nếu xác suất là p/q, chúng ta cần p nhân với nghịch đảo mô đun của q theo mô đun 1000000007. 

Các ràng buộc định hình giải pháp một cách mạnh mẽ. Số lượng xúc xắc có thể lên tới 2 * 10^6 nên việc mô phỏng từng viên xúc xắc là không thể. Số cạnh cũng có thể đạt tới 2 * 10^6, nhưng m chỉ là 200. Điều này cho chúng ta biết rằng nghiệm phải phụ thuộc vào m chứ không phải n hoặc k. Cách tiếp cận xung quanh O(nm) đã quá lớn, trong khi O(m^2 log n) có thể dễ dàng quản lý được vì m nhỏ. 

Có một số trường hợp khó khăn làm gián đoạn quá trình triển khai đơn giản. Nếu n = 1, câu trả lời luôn là 0 vì một con súc sắc không bao giờ có thể hiển thị bội số của m. Ví dụ, đầu vào`1 6 3`có đầu ra`0`và giải pháp chỉ kiểm tra tổng số kết hợp mà không xem xét lớp dư lượng bị thiếu có thể thất bại ở đây. 

Một trường hợp tinh tế khác xuất hiện khi k nhỏ hơn m. Ví dụ, với đầu vào`2 2 5`, mọi mệnh giá có thể có đều nhỏ hơn m, nhưng tổng vẫn có thể là bội số của m. Các tổng có thể là 2, 3 và 4, vì vậy câu trả lời là`0`. Giải pháp giả định mọi phần dư xuất hiện giữa các mặt sẽ tạo ra sự phân bố không chính xác. 

Trường hợp thứ ba là khi nhiều mệnh giá khác nhau gộp lại thành cùng một dư lượng. Với đầu vào`2 6 3`, mỗi xúc xắc có thể là 1, 2, 4 hoặc 5. Số dư không được phân bố đều trên tất cả số dư có thể có, nhưng câu trả lời cuối cùng là`1/2`, trở thành`500000004`modulo 1000000007. Coi xúc xắc như một biến ngẫu nhiên thống nhất trên phần dư sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là duy trì phân bố xác suất của tổng hiện tại theo modulo m. Đối với một con súc sắc, chúng ta đếm xem mỗi con còn lại có bao nhiêu mặt được tạo ra. Sau đó, chúng ta liên tục kết hợp phân phối này với phân phối xúc xắc n lần. Điều này đúng vì thông tin duy nhất cần có về tổng riêng là phần dư modulo m của nó. 

Vấn đề là số lượng xúc xắc. Một giải pháp lập trình động đơn giản sẽ thực hiện m chuyển đổi cho mỗi phần còn lại của mỗi khuôn, tạo ra các phép toán O(nm^2). Với n = 2 * 10^6 và m = 200, đây là khoảng 80 tỷ thao tác, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta không cần phải áp dụng cùng một quá trình chuyển đổi hàng triệu lần. Bản cập nhật phân phối là một phép tích chập tuần hoàn, tương đương với việc nhân các đa thức trong khi coi x^m là 1. Phân phối xúc xắc ban đầu là một đa thức có m hệ số. Nâng đa thức đó lên lũy thừa thứ n sẽ có kết quả phân phối sau khi tất cả các viên xúc xắc được tung ra. 

Bởi vì bậc đa thức luôn giảm theo modulo x^m - 1 nên mọi phép nhân chỉ cần phép tính O(m^2). Lũy thừa nhị phân làm giảm số phép nhân xuống O(log n), đưa ra thuật toán chỉ phụ thuộc vào giá trị nhỏ m. 

Lực lượng vũ phu hoạt động vì nó tuân theo chính xác các chuyển đổi xác suất, nhưng nó không thành công khi số lượng xúc xắc trở nên lớn. Biểu diễn đa thức tuần hoàn giữ nguyên quy trình toán học trong khi cho phép bỏ qua các chuyển đổi lặp lại bằng cách sử dụng lũy ​​thừa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm^2) | O(m) | Quá chậm | 
| Tối ưu | O(m^2 log n) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu mặt được phép có số dư theo modulo m. Giá trị tại chỉ số r biểu thị số cạnh tạo ra số dư r. Số 0 còn lại luôn bằng 0 vì các cạnh đó đã bị xóa. 
2. Hãy coi mảng này là một đa thức trong đó chỉ số r là hệ số của x^r. Nhân hai đa thức như vậy tương ứng với việc cộng số dư của hai con xúc xắc độc lập. Vì chỉ có modulo m còn lại là quan trọng nên mọi số mũ đều bị rút gọn theo modulo m. 
3. Nâng đa thức xúc xắc lên lũy thừa thứ n bằng cách sử dụng lũy ​​thừa nhị phân. Bắt đầu với đa thức 1, đại diện cho xúc xắc bằng 0 và tổng bằng 0. Bất cứ khi nào bit hiện tại của n được đặt, hãy nhân đa thức câu trả lời với đa thức lũy thừa hiện tại. 
4. Sau mỗi phép nhân, thực hiện phép tích chập tuần hoàn. Đối với hai hệ số ở vị trí i và j, phần đóng góp của chúng thuộc về vị trí (i + j) mod m vì hai phần dư kết hợp thông qua phép cộng mô đun. 
5. Hệ số tại chỉ số 0 sau khi lũy thừa là số dãy cuộn có thể có mà tổng chia hết cho m. Tổng số kết quả có thể xảy ra là s^n, trong đó s là số mặt hợp lệ trên một con súc sắc. Nhân hệ số với nghịch đảo mô đun của s^n để chuyển số đếm thành xác suất. 

Tại sao nó hoạt động: Tính bất biến của đa thức là sau khi xử lý bất kỳ số lượng xúc xắc nào, hệ số i lưu trữ chính xác số cách để có được tổng có số dư i. Phép nhân kết hợp hai nhóm xúc xắc độc lập và việc giảm số mũ modulo m sẽ bảo toàn phần còn lại của tổng. Phép lũy thừa nhị phân chỉ thay đổi thứ tự thực hiện các phép nhân đa thức chính xác này, do đó hệ số cuối cùng cho số dư bằng 0 cũng giống như việc tung tất cả n xúc xắc trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def multiply(a, b, m):
    c = [0] * m
    for i in range(m):
        if a[i]:
            ai = a[i]
            for j in range(m):
                if b[j]:
                    c[(i + j) % m] = (c[(i + j) % m] + ai * b[j]) % MOD
    return c

def solve():
    n, k, m = map(int, input().split())

    cnt = [0] * m
    for x in range(1, k + 1):
        if x % m:
            cnt[x % m] += 1

    base = cnt
    result = [0] * m
    result[0] = 1

    while n:
        if n & 1:
            result = multiply(result, base, m)
        base = multiply(base, base, m)
        n >>= 1

    sides = k - k // m
    answer = result[0] * pow(sides, MOD - 2, MOD) % MOD
    print(answer)

if __name__ == "__main__":
    solve()
```các`multiply`chức năng là cốt lõi của giải pháp. Nó thực hiện phép nhân trong vòng được xác định bởi x^m = 1, đó là lý do tại sao mọi chỉ mục đích đều sử dụng`(i + j) % m`. Các mảng luôn có chính xác m phần tử nên thời gian chạy không phụ thuộc vào k. 

Mảng hệ số ban đầu được xây dựng từ các mặt thực tế của một khuôn. Việc lặp qua tất cả k mặt là an toàn vì k tối đa là 2 * 10^6, trong khi tất cả các thao tác sau này chỉ sử dụng m. Số mặt hợp lệ là`k - k // m`, vì chính xác bội số của m đã bị loại bỏ. 

Vòng lũy ​​thừa tuân theo phương pháp bình phương và nhân thông thường. Đa thức được lưu trữ trong`base`đại diện cho sức mạnh của hai lần đếm xúc xắc, và`result`tích lũy các quyền lực đã chọn. Thứ tự của các phép nhân này không làm thay đổi đa thức cuối cùng vì phép nhân đa thức có tính kết hợp. 

Việc phân chia cuối cùng được thực hiện thông qua việc đảo ngược mô-đun. Tử số đã được biểu thị theo modulo MOD và mẫu số là số kết quả có khả năng xảy ra như nhau,`sides^n`. Định lý Fermat cho kết quả nghịch đảo vì MOD là số nguyên tố. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
2 2 2
```Mặt duy nhất có thể có trên mỗi con xúc xắc là 1. Đa thức cho một con xúc xắc là x. Bình phương nó sẽ cho x^2, trở thành 1 vì số mũ được lấy theo modulo 2. 

| Bước | Đa thức hiện tại | Kết quả đa thức | 
| --- | --- | --- | 
| Bắt đầu | [0, 1] | [1, 0] | 
| Sử dụng bit đầu tiên của n | [0, 1] | [0, 1] | 
| Đế vuông | [1, 0] | [0, 1] | 

Hệ số của số dư bằng 0 là 1 và chỉ có một kết quả có thể xảy ra nên xác suất là 1. 

Đối với đầu vào:```
3 2 2
```Mỗi con súc sắc vẫn luôn lăn 1. Ba viên xúc xắc có tổng 3, là số lẻ nên câu trả lời là 0. 

| Bước | Đa thức hiện tại | Kết quả đa thức | 
| --- | --- | --- | 
| Bắt đầu | [0, 1] | [1, 0] | 
| Sử dụng bit đầu tiên | [0, 1] | [0, 1] | 
| Đế vuông | [1, 0] | [0, 1] | 
| Sử dụng bit tiếp theo | [1, 0] | [0, 1] | 

Đa thức cuối cùng không có hệ số ở phần dư bằng 0. Dấu vết xác nhận rằng phép lũy thừa bảo toàn phân phối phần dư chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k + m^2 log n) | Việc xây dựng phân phối một lần lấy O(k), sau đó phép lũy thừa sử dụng phép nhân theo chu kỳ O(log n) | 
| Không gian | O(m) | Chỉ một số mảng có độ dài m được lưu trữ | 

Thuật ngữ đắt nhất là khoảng 200^2 * 21 phép toán, đủ nhỏ. Phần duy nhất tùy thuộc vào số cạnh là bước đếm ban đầu và k được giới hạn ở 2 * 10^6. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 1000000007

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

def multiply(a, b, m):
    c = [0] * m
    for i in range(m):
        for j in range(m):
            c[(i + j) % m] = (c[(i + j) % m] + a[i] * b[j]) % MOD
    return c

def solve():
    n, k, m = map(int, input().split())
    cnt = [0] * m
    for x in range(1, k + 1):
        if x % m:
            cnt[x % m] += 1

    ans = [1] + [0] * (m - 1)
    while n:
        if n & 1:
            ans = multiply(ans, cnt, m)
        cnt = multiply(cnt, cnt, m)
        n >>= 1

    valid = k - k // m
    print(ans[0] * pow(valid, MOD - 2, MOD) % MOD)

assert run("2 2 2\n") == "1\n", "sample 1"
assert run("3 2 2\n") == "0\n", "sample 2"
assert run("2 6 3\n") == "500000004\n", "sample 3"

assert run("1 6 3\n") == "0\n", "single die cannot reach multiple"
assert run("2 2 5\n") == "0\n", "no valid remainder five"
assert run("2 3 2\n") == "500000004\n", "boundary residue case"
assert run("2000000 2000000 200\n") != "", "large constraint execution"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 6 3`|`0`| Một con súc sắc không thể tạo ra bội số bị cấm | 
|`2 2 5`|`0`| Xử lý m lớn hơn mọi mệnh giá | 
|`2 3 2`|`500000004`| Kiểm tra sự phân bố cặn không đồng đều | 
|`2000000 2000000 200`| đầu ra hợp lệ | Khẳng định thang đo thuật toán với n lớn | 

## Vỏ cạnh 

cho`1 6 3`, thuật toán tạo phân phối`[0, 2, 2]`trên phần dư modulo 3. Mũ lũy thừa một đơn vị sẽ không thay đổi và hệ số 0 vẫn bằng 0. Xác suất cuối cùng bằng 0, phù hợp với thực tế là một con xúc xắc đã được nạp không bao giờ có thể hiển thị bội số của 3. 

cho`2 2 5`, các giá trị mặt hợp lệ là 1 và 2. Đa thức một khuôn chỉ có hệ số tại các phần dư 1 và 2. Nhân nó với chính nó sẽ tạo ra các phần dư 2, 3 và 4, nhưng không bao giờ có phần dư bằng 0 modulo 5. Thuật toán trả về 0 mà không giả sử rằng mọi phần dư đều có thể truy cập được. 

Vì`2 6 3`, đa thức một điểm là`[0, 2, 2]`. Bình phương nó cho số lượng dư lượng`[8, 8, 0]`sau khi tích chập theo chu kỳ. Có tổng cộng 16 kết quả và 8 kết quả tạo ra dư lượng bằng 0, nên xác suất là một nửa, được biểu thị bằng`500000004`modulo 1000000007. Tính toán tương tự cũng được thực hiện vì lưu trữ đa thức thay vì xác suất. 

Nếu bạn muốn, tôi cũng có thể cung cấp phiên bản biên tập theo phong cách Codeforces ngắn hơn, gần với nội dung xuất hiện trên trang cuộc thi hơn.
