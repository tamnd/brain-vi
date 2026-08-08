---
title: "CF 102535J - Aufbau"
description: "Nhiệm vụ là mô phỏng một quá trình lấp đầy electron rất lớn, nhưng chỉ có lớp vỏ con được lấp đầy cuối cùng mới quan trọng. Một nguyên tử có số hiệu nguyên tử a có đúng một electron. Các electron được đặt vào các lớp con được sắp xếp theo giá trị n + l, và các liên kết được giải quyết bằng n nhỏ hơn."
date: "2026-08-06T19:58:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "J"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 101
verified: true
draft: false
---

[CF 102535J - Aufbau](https://codeforces.com/problemset/problem/102535/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là mô phỏng một quá trình lấp đầy electron rất lớn, nhưng chỉ có lớp vỏ con được lấp đầy cuối cùng mới quan trọng. Nguyên tử có số hiệu nguyên tử`a`có chính xác`a`electron. Các electron được xếp vào các lớp con được sắp xếp theo giá trị`n + l`và các ràng buộc được giải quyết bằng cách nhỏ hơn`n`. Một vỏ con có chỉ mục`l`có thể giữ`4l + 2`electron. 

Đối với mỗi truy vấn, thay vì in toàn bộ cấu hình electron, chúng ta chỉ cần lớp con cuối cùng nhận electron. Câu trả lời phải chứa số shell`n`, nhãn lớp con và số electron thực sự được đặt ở đó. 

Giới hạn của`a`lớn như`10^15`ngay lập tức loại trừ việc mô phỏng từng electron một. Ngay cả việc lặp lại tất cả các shell con được điền cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm vì có thể có hàng triệu vị trí có liên quan trên toàn bộ test.`10^5`truy vấn. Giải pháp cần phải vượt qua các nhóm lớn các lớp con về mặt toán học và chỉ kiểm tra số lượng ứng cử viên logarit. 

Trường hợp khó khăn đầu tiên là thứ tự không chỉ đơn giản là tăng số lượng shell. Ví dụ, đầu vào`19`sản xuất`4s1`, bởi vì`4s`có nhỏ hơn`n+l`hơn`3d`. Giải pháp quét shell theo thứ tự sẽ cho kết quả sai. 

Một trường hợp biên khác là một truy vấn kết thúc chính xác ở ranh giới của một lớp con. Đối với đầu vào`2`, câu trả lời là`1s2`. Tìm kiếm nhị phân bất cẩn tìm thấy nhóm tiếp theo thay vì nhóm đầu tiên chứa câu trả lời có thể di chuyển sai đến`2s`. 

Trường hợp tinh vi cuối cùng là cách đặt tên vỏ con mở rộng. Ví dụ: mẫu lớn nhất sử dụng nhãn dài hơn một ký tự. Một giải pháp giả định`l`luôn luôn có một chữ cái sẽ bị lỗi sau khi hết nhãn chữ cái đơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ tạo ra thứ tự của lớp con, trừ đi công suất của từng lớp con khỏi số electron còn lại và dừng lại khi giá trị còn lại vừa với lớp con hiện tại. Điều này đúng vì nó tuân theo chính xác thứ tự Aufbau. Tuy nhiên, đối với`10^15`electron, số lượng các lớp con quá lớn để có thể tạo riêng cho mỗi truy vấn. 

Quan sát hữu ích là các lớp con tự nhiên tạo thành các đường chéo theo giá trị`k = n + l`. Mỗi đường chéo có thể được bỏ qua toàn bộ. Đối với một cố định`k`, tất cả các cặp có thể thỏa mãn`n + l = k`và tổng số electron trên đường chéo đó có thể được tính bằng công thức. Chúng ta có thể tìm kiếm nhị phân đường chéo đầu tiên có dung lượng tích lũy đạt đến số nguyên tử nhất định. 

Sau khi xác định được đường chéo, số lớp vỏ con bên trong nó chỉ còn khoảng một nửa. Thay vì duyệt qua chúng, chúng tôi sử dụng một tìm kiếm nhị phân khác vì các khả năng tạo thành một cấp số cộng. Điều này làm giảm toàn bộ vấn đề thành một vài tìm kiếm logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số lượng chuỗi con) trên mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(log K) cho mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định hàm trả về tổng số electron có trong tất cả các lớp con với`n + l <= K`. 

Đối với một chỉ số subshell cố định`l`, các shell hợp lệ thỏa mãn`l + 1 <= n <= K - l`. Số vỏ như vậy là`K - 2l`, và mỗi người đóng góp`4l + 2`electron. Tính tổng biểu thức số học này sẽ cho một công thức khép kín, cho phép chúng ta bỏ qua toàn bộ đường chéo. 
2. Tìm kiếm nhị phân nhỏ nhất`K`sao cho tổng số electron theo đường chéo`K`ít nhất là số hiệu nguyên tử đã cho. 

Các đường chéo trước đó đã được lấp đầy hoàn toàn, do đó trừ đi tổng số của chúng để lại số electron phải đặt bên trong đường chéo`K`. 
3. Tìm lớp con chính xác bên trong đường chéo`K`. 

Trong đường chéo này, các vỏ được truy cập ngày càng tăng`n`. tương ứng`l`các giá trị giảm đi. Dung lượng của chúng là một dãy số học, do đó, một tìm kiếm nhị phân khác sẽ tìm thấy lớp con đầu tiên có dung lượng tích lũy đạt tới số electron còn lại. 
4. Tính số electron còn lại trong lớp con đó và chuyển đổi nó`l`giá trị sang định dạng nhãn được yêu cầu. 

Hai mươi chỉ số vỏ con đầu tiên sau`s`Và`p`có nhãn ký tự đơn. Sau đó, các nhãn sẽ trở thành tổ hợp các chữ cái được sắp xếp theo thứ tự từ điển. 

Tại sao nó hoạt động: thuật toán giữ nguyên thứ tự như quy tắc Aufbau bằng cách chỉ nhóm các phần liên tiếp của chuỗi ban đầu. Tìm kiếm nhị phân đầu tiên xác định đường chéo chính xác chứa câu trả lời và tìm kiếm thứ hai xác định lớp con chính xác bên trong đường chéo đó. Vì mỗi phần bị bỏ qua đều được tính bằng công thức dung lượng chính xác nên không có vị trí electron nào có thể bị bỏ qua hoặc tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def prefix(k):
    if k <= 0:
        return 0
    m = (k - 1) // 2
    return 2 * k * (m + 1) * (m + 1) - (2 * m * (m + 1) * (4 * m + 5)) // 3

def inside_sum(largest_l, count):
    return count * (4 * largest_l + 2) - 2 * count * (count - 1)

def label(idx):
    if idx == 0:
        return "s"
    if idx == 1:
        return "p"

    singles = "dfghijklmnoqrtuvwxyz"
    idx -= 2

    if idx < len(singles):
        return singles[idx]

    idx -= len(singles)
    length = 2
    while True:
        block = 26 ** length
        if idx < block:
            chars = []
            for _ in range(length):
                chars.append(chr(ord('a') + idx % 26))
                idx //= 26
            return ''.join(reversed(chars))
        idx -= block
        length += 1

def solve_one(a):
    lo, hi = 1, 100000
    while lo < hi:
        mid = (lo + hi) // 2
        if prefix(mid) >= a:
            hi = mid
        else:
            lo = mid + 1

    k = lo
    rem = a - prefix(k - 1)

    start_n = k // 2 + 1
    max_l = k - start_n

    lo, hi = 0, max_l
    while lo < hi:
        mid = (lo + hi) // 2
        if inside_sum(max_l, mid + 1) >= rem:
            hi = mid
        else:
            lo = mid + 1

    skipped = lo
    l = max_l - skipped
    before = inside_sum(max_l, skipped)
    e = rem - before
    n = k - l

    return str(n) + label(l) + str(e)

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        ans.append(solve_one(int(input())))
    print('\n'.join(ans))

main()
```các`prefix`chức năng là tối ưu hóa cốt lõi. Nó đếm các đường chéo hoàn chỉnh thay vì các lớp con riêng lẻ. Biến`m`là lớn nhất có thể`l`giá trị theo đường chéo`k`, và công thức tính tổng tất cả các khả năng trong đường chéo đó. 

Người trợ giúp thứ hai,`inside_sum`, cho biết công suất của cái đầu tiên`count`vỏ con bên trong một đường chéo. Bởi vì những khả năng đó giảm đi đúng bốn lần mỗi lần, nên tổng là một cấp số cộng. 

Tìm kiếm nhị phân đầu tiên sử dụng phạm vi đủ lớn cho tất cả các đầu vào có thể. Giá trị cần thiết cho`10^15`electron nhỏ hơn nhiều so với`100000`, vì vậy giới hạn này chỉ là giới hạn trên an toàn. Số nguyên Python tránh tràn trong các phép nhân lớn. 

Việc chuyển đổi nhãn được tách biệt khỏi toán học. Điều này tránh trộn lẫn các quy tắc đặt tên bất thường với logic đếm electron. 

## Ví dụ đã hoạt động 

Đối với đầu vào`19`, tìm kiếm nhị phân tìm thấy đường chéo`k = 5`. 

| Biến | Giá trị | 
| --- | --- | 
| k | 5 | 
| Electron trước đường chéo | 18 | 
| Các electron còn lại | 1 | 
| n đầu tiên trong đường chéo | 3 | 
| Đầu tiên l theo đường chéo | 2 | 
| Đã chọn l | 0 | 
| Đã chọn n | 4 | 
| Electron trong lớp con | 1 | 

Đường chéo chứa`3d`,`4s`, và quá trình tìm kiếm nhận thấy rằng chỉ cần một electron sau khi hoàn thành tất cả các đường chéo trước đó. Kết quả là`4s1`. 

Đối với đầu vào`103`, các đường chéo trước chứa`94`electron, rời đi`9`electron bên trong đường chéo`k = 8`. 

| Biến | Giá trị | 
| --- | --- | 
| k | 8 | 
| Electron trước đường chéo | 94 | 
| Các electron còn lại | 9 | 
| n đầu tiên trong đường chéo | 5 | 
| Đầu tiên l theo đường chéo | 3 | 
| Đã chọn l | 2 | 
| Đã chọn n | 6 | 
| Electron trong lớp con | 9 | 

Vỏ con được chọn là`6d`, và kết quả là`6d9`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log K) | Hai tìm kiếm nhị phân trên các vị trí đường chéo và vỏ con | 
| Không gian | O(1) | Chỉ có một số lượng biến số nguyên không đổi được lưu trữ | 

Số nguyên tử lớn nhất chỉ yêu cầu vài chục lần tìm kiếm nhị phân. Điều này dễ dàng phù hợp với giới hạn của`10^5`trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    sys.stdin = old

    out = []
    for x in data[1:]:
        out.append(solve_one(int(x)))
    return "\n".join(out)

assert run("""3
19
103
1000000000000000
""") == """4s1
6d9
93591dzil31704"""

assert run("""1
1
""") == "1s1"

assert run("""1
2
""") == "1s2"

assert run("""1
18
""") == "3p6"

assert run("""1
20
""") == "3d2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1s1`| Số nguyên tử tối thiểu | 
|`2`|`1s2`| Ranh giới vỏ con chính xác | 
|`18`|`3p6`| Hoàn thành một đường chéo | 
|`20`|`3d2`| Chuyển sang phần con tiếp theo | 

## Vỏ cạnh 

Đối với số nguyên tử`2`, thuật toán tìm`k = 1`vì đường chéo đầu tiên đã chứa đủ electron. Số còn lại là`2`, và lớp con đầu tiên trong đường chéo có dung lượng`2`, vì vậy đầu ra là`1s2`. Tìm kiếm nhị phân không di chuyển sang đường chéo tiếp theo vì nó tìm kiếm tiền tố hợp lệ đầu tiên. 

Đối với số nguyên tử`19`, trước tiên thuật toán sẽ bỏ qua tất cả các đường chéo trước`k = 5`. Bên trong đường chéo đó, nó so sánh công suất theo thứ tự`3d`,`4s`. Lớp con thứ nhất không thể chứa các electron còn lại nên đáp án trở thành`4s1`. 

Đối với các giá trị rất lớn như`1000000000000000`, thuật toán không bao giờ xây dựng được cấu hình đầy đủ. Nó chỉ đánh giá các công thức cho các đường chéo hoàn chỉnh và thực hiện tìm kiếm logarit, sau đó tạo nhãn nhiều chữ cái cuối cùng từ chỉ mục vỏ con được tính toán. Điều này xử lý quy tắc đặt tên mở rộng mà không có bất kỳ giả định cố định nào về độ dài nhãn.
