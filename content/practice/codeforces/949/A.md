---
title: "CF 949A - Ngựa vằn"
description: "Chúng ta được cung cấp một chuỗi nhị phân biểu thị một chuỗi ngày theo thứ tự thời gian, trong đó mỗi ký tự là 0 hoặc 1."
date: "2026-06-17T02:22:09+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 949
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 469 (Div. 1)"
rating: 1600
weight: 949
solve_time_s: 92
verified: false
draft: false
---

[CF 949A - Ngựa vằn](https://codeforces.com/problemset/problem/949/A) 

**Đánh giá:** 1600 
**Thẻ:** tham lam 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân biểu thị một chuỗi ngày theo thứ tự thời gian, trong đó mỗi ký tự là 0 hoặc 1. Chúng ta phải chia các vị trí này thành nhiều nhóm sao cho mỗi vị trí thuộc về chính xác một nhóm, nhưng mỗi nhóm không cần phải bao gồm các chỉ số liên tiếp. 

Mỗi nhóm phải tạo thành một “chuỗi ngựa vằn”. Một chuỗi ngựa vằn hợp lệ có ba thuộc tính. Nó không trống, nó bắt đầu bằng 0, kết thúc bằng 0 và các phần tử liền kề trong dãy con phải xen kẽ giữa 0 và 1. 

Điều này ngay lập tức ngụ ý rằng mọi dãy con hợp lệ phải trông giống như 0, 01, 010, 0101, v.v., nhưng luôn kết thúc bằng 0, do đó độ dài của nó là số lẻ và nó chứa nhiều hơn một số 0. 

Chúng tôi được yêu cầu quyết định xem có thể phân vùng như vậy hay không và nếu có thì xuất ra bất kỳ phân vùng hợp lệ nào. 

Kích thước đầu vào lên tới 200.000 ký tự, loại trừ mọi thứ bậc hai hoặc thậm chí$O(n \log n)$với các hằng số nặng trừ khi nó có cấu trúc cực kỳ tốt. Dự kiến ​​sẽ có một công trình tham lam tuyến tính hoặc gần tuyến tính. 

Một ràng buộc cấu trúc quan trọng ẩn bên trong định nghĩa: mọi con ngựa vằn đều bắt đầu và kết thúc bằng 0, do đó, mỗi nhóm phải sử dụng các số 0 theo cách chúng đóng vai trò là cả điểm bắt đầu và kết thúc. Điều này đã gợi ý rằng số không sẽ kiểm soát tính khả thi. 

Một sai lầm ngây thơ là cố gắng xây dựng các chuỗi con một cách tham lam mà không theo dõi tính khả dụng của cấu trúc xen kẽ hoặc gán các số 0 một cách độc lập mà không ghép chúng với các số một một cách chính xác. Một thất bại phổ biến khác là bỏ qua thực tế là người ta phải luôn ngồi giữa hai số 0 trong dãy số sau của chúng; bất kỳ số 1 nào không thể ghép nối theo kiểu xen kẽ sẽ gây ra lỗi. 

Một trường hợp thất bại minh họa nhỏ là`111000`. Nhìn chung có đủ số 0 và số 1, nhưng mọi nỗ lực bắt đầu nhiều chuỗi đều thất bại vì không có mẫu xen kẽ nào có thể bắt đầu từ số 0 theo cách tiêu thụ tất cả các số một một cách chính xác. Đầu ra đúng là`-1`. 

Một trường hợp tế nhị khác là`0101`. Nó trông có vẻ xen kẽ, nhưng nó kết thúc bằng 1, vì vậy không có dãy con nào có thể bao gồm nó hoàn toàn; không thể gán nó vào các chuỗi ngựa vằn hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng phân chia các chỉ mục thành các nhóm và kiểm tra tính hợp lệ của từng nhóm. Điều này có nghĩa là gán mỗi chỉ mục cho một trong số các chỉ mục có thể$O(n)$các chuỗi tiếp theo và xác minh sự luân phiên của mỗi nhóm. Ngay cả khi chúng tôi hạn chế phân bổ tham lam, số cách phân phối các phần tử sẽ tăng theo cấp số nhân và bản thân việc xác minh là tuyến tính trên mỗi phân vùng ứng viên. Điều này hoàn toàn không thể thực hiện được ở$n = 2 \cdot 10^5$. 

Quan sát quan trọng là cấu trúc của mỗi con ngựa vằn rất cứng. Vì các chuỗi phải xen kẽ và bắt đầu bằng 0, nên mỗi khi chúng ta đặt số 1, nó phải tiếp tục một chuỗi đã bắt đầu trước đó hiện kết thúc bằng 0. Tương tự, mỗi số 0 sẽ bắt đầu một chuỗi mới hoặc đóng một chuỗi hiện có dự kiến ​​sẽ có số 0 tiếp theo. 

Điều này gợi ý nên duy trì hai tập hợp động “các chuỗi con mở”: những tập hợp hiện kết thúc bằng 0 và những tập hợp kết thúc bằng 1. Chúng ta luôn muốn so khớp các ký tự một cách tham lam: số 1 phải mở rộng chuỗi kết thúc bằng 0 và số 0 phải mở rộng chuỗi kết thúc bằng 1 hoặc bắt đầu một chuỗi mới. 

Cái nhìn sâu sắc quan trọng là chúng tôi không tự do lựa chọn phân vùng; thay vào đó, chúng ta xâu chuỗi các chuỗi một cách hiệu quả qua chuỗi. Mỗi chỉ mục gắn vào chính xác một chuỗi và các chuỗi phát triển theo kiểu xếp chồng dựa trên ký tự được mong đợi cuối cùng. Điều này làm giảm vấn đề thành một công trình tham lam với việc ghi sổ các chuỗi hoạt động. 

Cuối cùng, tất cả các chuỗi phải kết thúc bằng 0, nghĩa là không có chuỗi nào có thể tồn tại số 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Chuyển nhượng chuỗi tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai ngăn xếp (hoặc danh sách): một ngăn xếp cho các chuỗi hiện kết thúc bằng 0 và một ngăn xếp cho các chuỗi hiện kết thúc bằng 1. Mỗi chuỗi được biểu thị bằng các chỉ số được thu thập của nó. 

1. Lặp lại chuỗi từ trái sang phải. 
2. Khi chúng ta nhìn thấy một`0`, chúng ta phải đính kèm nó vào một chuỗi hiện kết thúc bằng`1`nếu một trình tự như vậy tồn tại, bởi vì điều đó bảo toàn sự luân phiên. Nếu không có chuỗi như vậy tồn tại, chúng ta bắt đầu một chuỗi mới với chỉ mục này. Bắt đầu các chuỗi mới bằng 0 là hợp lệ vì chúng bắt đầu chính xác. 
3. Khi chúng ta nhìn thấy một`1`, chúng ta phải đính kèm nó vào một chuỗi hiện kết thúc bằng`0`. Nếu không có thì việc xây dựng là không thể và chúng ta sẽ thất bại ngay lập tức. 
4. Sau khi đính kèm, chúng ta di chuyển chuỗi giữa các ngăn xếp vì ký tự kết thúc của nó bị lật. 
5. Sau khi xử lý tất cả các ký tự, chúng tôi xác minh rằng không có chuỗi nào kết thúc bằng`1`. Bất kỳ chuỗi nào như vậy sẽ vi phạm yêu cầu tất cả các chuỗi đều kết thúc bằng 0. 

Quyết định ở mỗi bước bị ép buộc bởi quy tắc luân phiên. Con số 1 không thể bắt đầu một con ngựa vằn và không thể theo sau con số 1 khác, vì vậy nó luôn tiêu thụ một “chuỗi không kết thúc mở” có sẵn. 

### Tại sao nó hoạt động 

Tại mỗi tiền tố của chuỗi, mỗi chuỗi hoạt động được đặc trưng duy nhất bởi phần tử cuối cùng của nó. Thuật toán đảm bảo rằng bất kỳ chuỗi nào kết thúc bằng 1 luôn chờ số 0 tiếp theo và bất kỳ chuỗi nào kết thúc bằng 0 đều đang chờ số 1 tiếp theo. Vì một con ngựa vằn hợp lệ phải luân phiên nghiêm ngặt nên bất kỳ sự sai lệch nào so với sự phân công tham lam sẽ chỉ làm trì hoãn việc tiêu thụ một nhân vật và làm giảm tính linh hoạt trong tương lai. Bởi vì 1 chỉ có một trạng thái tiền thân hợp lệ (các chuỗi 0 kết thúc) và 0 có một lựa chọn không bao giờ làm giảm tính khả thi khi tồn tại một chuỗi 1 kết thúc, nên lựa chọn tham lam sẽ bảo toàn tất cả các khả năng trong tương lai. Nếu tại bất kỳ thời điểm nào, số 1 không thể được chỉ định thì không có sự sắp xếp lại nào có thể khắc phục được nó vì mọi chuỗi có thể chấp nhận nó đều không tương thích về mặt cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    zero_end = []  # sequences ending with 0
    one_end = []   # sequences ending with 1
    res = []       # list of sequences

    # each sequence is a list of indices
    for i, ch in enumerate(s, 1):
        if ch == '0':
            if one_end:
                seq = one_end.pop()
                seq.append(i)
                zero_end.append(seq)
            else:
                seq = [i]
                zero_end.append(seq)
        else:  # ch == '1'
            if not zero_end:
                print(-1)
                return
            seq = zero_end.pop()
            seq.append(i)
            one_end.append(seq)

    # all sequences must end with 0
    if one_end:
        print(-1)
        return

    print(len(zero_end))
    for seq in zero_end:
        print(len(seq), *seq)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì các chuỗi rõ ràng và tự động di chuyển chúng giữa các trạng thái tùy thuộc vào ký tự cuối cùng được thêm vào. Chi tiết triển khai quan trọng là các chuỗi được lưu trữ trực tiếp, do đó không cần phải xây dựng lại ở cuối. 

Một điều tinh tế là việc xử lý xác thực cuối cùng: bất kỳ chuỗi nào còn lại trong nhóm “kết thúc bằng 1” đều không hợp lệ, vì ngựa vằn phải kết thúc bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`0010100`Chúng tôi theo dõi trình tự phát triển như thế nào. 

| Bước | Char | Trình tự không có kết thúc | Trình tự một đầu | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | [1] | [] | bắt đầu mới | 
| 2 | 0 | [1], [2] | [] | bắt đầu mới | 
| 3 | 1 | [1,3] | [2] | di chuyển tuần tự 1→1 | 
| 4 | 0 | [2,4] | [1] | mở rộng seq 2 | 
| 5 | 1 | [2,4,5] | [1] | mở rộng | 
| 6 | 0 | [1,6] | [2,4,5] | di chuyển thứ tự 1 | 
| 7 | 0 | [1,6], [2,4,5,7] | [] | di chuyển seq | 

Tất cả các chuỗi đều kết thúc bằng 0, vì vậy đầu ra hợp lệ. 

Dấu vết này cho thấy các số 0 bắt đầu các chuỗi mới hoặc sửa chữa các chuỗi hiện có, trong khi các số 0 tiêu thụ nghiêm ngặt các chuỗi mở không có kết thúc. 

### Ví dụ 2 

đầu vào:`0101`| Bước | Char | Không kết thúc | Một đầu | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | [1] | [] | bắt đầu | 
| 2 | 1 | [] | [1,2] | mở rộng | 
| 3 | 0 | [1,3,2] | [] | mở rộng | 
| 4 | 1 | [] | [1,3,2,4] | mở rộng | 

Chuỗi cuối cùng kết thúc bằng 1, do đó không tồn tại phân vùng hợp lệ. 

Điều này chứng tỏ sự cần thiết là mọi chuỗi phải kết thúc ở số 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi chỉ mục được đẩy và xuất hiện nhiều nhất một lần | 
| Không gian | O(n) | lưu trữ tất cả các chỉ số theo trình tự | 

Các hoạt động quét tuyến tính và xếp chồng theo thời gian không đổi đảm bảo giải pháp phù hợp thoải mái trong giới hạn cho$n = 200{,}000$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    s = sys.stdin.readline().strip()
    zero_end = []
    one_end = []

    for i, ch in enumerate(s, 1):
        if ch == '0':
            if one_end:
                seq = one_end.pop()
                seq.append(i)
                zero_end.append(seq)
            else:
                zero_end.append([i])
        else:
            if not zero_end:
                return "-1"
            seq = zero_end.pop()
            seq.append(i)
            one_end.append(seq)

    if one_end:
        return "-1"

    out = [str(len(zero_end))]
    for seq in zero_end:
        out.append(str(len(seq)) + " " + " ".join(map(str, seq)))
    return "\n".join(out)

# provided sample
assert run("0010100\n") != "-1"

# custom cases
assert run("0\n") == "1\n1 1", "single zero"
assert run("1\n") == "-1", "single one invalid"
assert run("0101\n") == "-1", "alternation ends wrong"
assert run("001100\n") != "-1", "balanced structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1 / 1 1`| trường hợp hợp lệ tối thiểu | 
|`1`|`-1`| sự khởi đầu không thể | 
|`0101`|`-1`| chấm dứt vi phạm ràng buộc | 
|`001100`| phân vùng hợp lệ | tương tác nhiều chuỗi | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi chuỗi bắt đầu bằng một chuỗi dài các chuỗi, chẳng hạn như`11100`. Ở ký tự đầu tiên, không có chuỗi kết thúc bằng 0 hiện có để đính kèm, do đó thuật toán ngay lập tức từ chối đầu vào. Điều này đúng vì không có con ngựa vằn nào có thể bắt đầu bằng 1. 

Một trường hợp cạnh khác là tiền tố xen kẽ hoàn hảo kết thúc bằng 1, chẳng hạn như`010`. Thuật toán sẽ xây dựng thành công một chuỗi kết thúc bằng 1, nhưng lần xác thực cuối cùng không thành công vì ngựa vằn phải kết thúc bằng 0. Điều này cho thấy tại sao việc kiểm tra trạng thái cuối cùng là cần thiết và không thể thay thế bằng kiểm tra cục bộ trong quá trình xây dựng. 

Trường hợp thứ ba là nhiều chuỗi cạnh tranh, ví dụ`000111000`. Thuật toán tạo ra nhiều chuỗi khi cần thiết và mỗi chuỗi tiêu thụ một chuỗi không có kết thúc mở khác nhau. Tính đúng đắn ở đây xuất phát từ thực tế là chúng ta không bao giờ ép buộc hai số 1 vào cùng một chuỗi trừ khi cấu trúc cho phép điều đó.
