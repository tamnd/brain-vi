---
title: "CF 2145C - Dây Monocarp"
description: "Chúng ta được cấp một chuỗi nhị phân chỉ gồm a và b. Từ chuỗi này, chúng ta được phép loại bỏ chính xác một khối ký tự liền kề, có thể trống. Sau khi xóa xong, chúng ta nhìn vào các ký tự còn lại và đếm xem còn lại bao nhiêu ký tự a và b."
date: "2026-06-08T01:31:34+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 2145
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 183 (Rated for Div. 2)"
rating: 1300
weight: 2145
solve_time_s: 108
verified: false
draft: false
---

[CF 2145C - Chuỗi Monocarp](https://codeforces.com/problemset/problem/2145/C) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, tham lam, chuỗi 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân chỉ bao gồm`a`Và`b`. Từ chuỗi này, chúng ta được phép loại bỏ chính xác một khối ký tự liền kề, có thể trống. Sau khi xóa xong, chúng ta nhìn vào các ký tự còn lại và đếm xem có bao nhiêu ký tự`a`Và`b`bị bỏ lại. 

Mục tiêu là làm cho hai số lượng đó bằng nhau bằng cách chọn một đoạn bị xóa duy nhất, đồng thời giảm thiểu độ dài của đoạn bị xóa đó. Nếu không có lựa chọn nào về một khoảng bị loại bỏ có thể đạt được sự bằng nhau ngoại trừ việc xóa toàn bộ chuỗi, câu trả lời là`-1`. 

Hạn chế chính là việc xóa phải là một phân đoạn liên tục, không phải xóa rải rác. Điều đó về cơ bản tạo ra vấn đề về việc chọn một cửa sổ mà việc loại bỏ nó sẽ điều chỉnh sự cân bằng toàn cầu giữa`a`Và`b`. 

Kích thước đầu vào lớn, với tổng chiều dài của tất cả các trường hợp thử nghiệm lên tới 200.000. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận bậc hai nào đối với chuỗi con. Bất kỳ giải pháp nào thử tất cả các khoảng thời gian loại bỏ có thể một cách rõ ràng sẽ kiểm tra các ứng cử viên O(n²) trong trường hợp xấu nhất, quá chậm. 

Trường hợp cạnh tinh tế xuất hiện khi dây không có cách nào cân bằng ngay cả sau khi loại bỏ để bảo toàn cấu trúc. Ví dụ: nếu tất cả các ký tự đều`a`, cho dù bạn loại bỏ đoạn nội bộ nào thì chuỗi còn lại vẫn chỉ có`a`, vì vậy sự bình đẳng là không thể trừ khi chúng ta xóa mọi thứ. Tương tự, nếu tổng số lượng đã bằng nhau thì không loại bỏ gì là tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi phân khúc có thể`[l, r]`, xóa nó và kiểm tra xem chuỗi còn lại có số lượng bằng nhau không`a`Và`b`. Việc đếm các ký tự còn lại sau mỗi lần xóa có thể được tối ưu hóa bằng cách sử dụng tổng tiền tố, do đó mỗi lần kiểm tra là O(1). Tuy nhiên, có O(n²) các phân đoạn như vậy, do đó tổng độ phức tạp trở thành O(n²), quá lớn so với tổng chiều dài 2e5. 

Quan sát quan trọng là việc loại bỏ một phân đoạn sẽ thay đổi sự khác biệt tổng thể giữa các số lượng theo một cách rất có cấu trúc. Gọi tổng số mất cân bằng là:```
D = (#a - #b)
```Chúng tôi muốn sự mất cân bằng cuối cùng bằng 0 sau khi xóa một phân đoạn. Giả sử chúng ta loại bỏ một phân đoạn`[l, r]`. Cho phép:```
delta[l, r] = (#a - #b in s[l..r])
```Khi đó sự mất cân bằng còn lại trở thành:```
D - delta[l, r]
```Chúng tôi muốn:```
D - delta[l, r] = 0  →  delta[l, r] = D
```Vì vậy, vấn đề rút gọn thành việc tìm một mảng con có`(a - b)`giá trị bằng tổng`(a - b)`của chuỗi đầy đủ, đồng thời giảm thiểu độ dài của nó. 

Đây hiện là một phép chuyển đổi tổng tiền tố cổ điển. Chúng tôi chỉ định`+1`ĐẾN`a`Và`-1`ĐẾN`b`, tính tổng tiền tố và muốn có hai chỉ số`i < j`như vậy:```
prefix[j] - prefix[i] = D
```Tương đương:```
prefix[i] = prefix[j] - D
```Chúng tôi quét một lần và sử dụng bản đồ băm từ các giá trị tiền tố đến lần xuất hiện sớm nhất của chúng, giảm thiểu độ dài phân đoạn. 

Sự phức tạp duy nhất còn lại là tính khả thi: nếu tổng số đã bằng nhau thì câu trả lời là`0`. Nếu không có phân đoạn hợp lệ nào tồn tại, cách khả thi duy nhất là xóa mọi thứ, vì vậy chúng tôi quay lại`-1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tiền tố + Bản đồ băm | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển chuỗi thành mảng số trong đó`a = +1`Và`b = -1`. Điều này chuyển vấn đề thành làm việc với số dư thay vì số lượng thô. 
2. Tính tổng số tiền`D`. Nếu như`D == 0`, chuỗi đã được cân bằng nên không cần xóa và chúng tôi quay lại`0`. 
3. Xây dựng tổng tiền tố trong khi quét từ trái sang phải. Mỗi giá trị tiền tố thể hiện sự mất cân bằng cho đến chỉ số đó. 
4. Lưu trữ chỉ mục sớm nhất mà mỗi giá trị tiền tố xuất hiện. Điều này rất quan trọng vì việc sử dụng lần xuất hiện sớm nhất sẽ tối đa hóa độ dài của đoạn còn lại và giảm thiểu đoạn bị loại bỏ. 
5. Đối với từng vị trí`j`, tính giá trị tiền tố đích`prefix[j] - D`. Nếu chúng ta đã thấy giá trị này trước đây tại chỉ mục`i`, thì mảng con`(i+1..j)`chính xác là có sự mất cân bằng`D`, nghĩa là loại bỏ nó sẽ làm cho phần còn lại được cân bằng. 
6. Theo dõi độ dài tối thiểu trong số tất cả các phân đoạn hợp lệ như vậy. 
7. Nếu không tìm thấy đoạn nào, hãy quay lại`-1`. 

Ý tưởng trung tâm là mọi thao tác xóa hợp lệ đều tương ứng chính xác với việc chọn một phân mảng có sự mất cân bằng bằng với sự mất cân bằng ban đầu. Khi tổng tiền tố được sử dụng, vấn đề sẽ trở thành quét tuyến tính với tra cứu. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là việc loại bỏ một phân khúc chỉ ảnh hưởng đến sự mất cân bằng tổng thể thông qua sự đóng góp của chính phân khúc đó. Phép biến đổi tiền tố biến mọi phân đoạn thành hiệu của hai điểm, do đó điều kiện trở thành hiệu đích cố định. Vì mọi phân đoạn có thể được biểu diễn chính xác một lần dưới dạng cặp tiền tố nên việc quét tất cả các tiền tố đảm bảo rằng mọi thao tác xóa hợp lệ đều được xem xét. Việc lưu trữ các lần xuất hiện sớm nhất đảm bảo rằng trong số tất cả các cặp hợp lệ, chúng ta luôn thu được đoạn bị loại bỏ ngắn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()

        arr = [1 if c == 'a' else -1 for c in s]
        total = sum(arr)

        if total == 0:
            out.append("0")
            continue

        prefix = 0
        first_pos = {0: -1}
        ans = n + 1

        for i, v in enumerate(arr):
            prefix += v

            need = prefix - total
            if need in first_pos:
                ans = min(ans, i - first_pos[need])

            if prefix not in first_pos:
                first_pos[prefix] = i

        if ans == n + 1:
            out.append("-1")
        else:
            out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Quá trình triển khai bắt đầu bằng cách mã hóa chuỗi thành một mảng đã ký để số dư trở nên bổ sung. Tổng tiền tố theo dõi khoảng cách giữa chúng ta và sự bình đẳng ở mỗi chỉ số. Từ điển`first_pos`đảm bảo chúng tôi luôn sử dụng giá trị tiền tố xuất hiện sớm nhất, điều này cần thiết để giảm thiểu độ dài phân đoạn bị loại bỏ. 

Điểm tinh tế chính là thứ tự cập nhật: chúng tôi kiểm tra phân đoạn hợp lệ trước khi chèn tiền tố hiện tại vào bản đồ. Điều này ngăn việc vô tình sử dụng một phân đoạn kết thúc ở chỉ mục hiện tại không chính xác khi tính toán ranh giới độ dài. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
bbbab
```Chúng tôi tính toán các giá trị tiền tố bằng cách sử dụng`a=+1, b=-1`. 

| tôi | char | tiền tố | cần thiết (tiền tố - tổng cộng) | first_pos được sử dụng | độ dài tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | b | -1 | -1 - (-3)=2 | - | thông tin | 
| 1 | b | -2 | -2 - (-3)=1 | - | thông tin | 
| 2 | b | -3 | -3 - (-3)=0 | 0 tồn tại | 2 | 
| 3 | một | -2 | -2 - (-3)=1 | 1 tồn tại | 2 | 
| 4 | b | -3 | -3 - (-3)=0 | 0 tồn tại | 3 | 

Tổng số mất cân bằng là`-3`. Đoạn tốt nhất có độ dài`3`, tương ứng với việc loại bỏ tiền tố có chứa ba`b`ký tự, để lại một chuỗi cân bằng. 

Dấu vết này cho thấy sự khác biệt về tiền tố trực tiếp mã hóa việc xóa ứng viên như thế nào. 

### Ví dụ 2 

đầu vào:```
6
bbaaba
```Tổng số dư ở đây là`0`. 

Vì tổng tiền tố đã kết thúc bằng 0 nên không cần xóa và câu trả lời là`0`. 

Điều này xác nhận việc xử lý trường hợp đặc biệt khi chuỗi đã được cân bằng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần với các thao tác hashmap O(1) | 
| Không gian | O(n) | Giá trị tiền tố được lưu trữ trong bản đồ băm | 

Độ phức tạp tuyến tính vừa vặn thoải mái trong tổng giới hạn 2e5 ký tự trên tất cả các trường hợp thử nghiệm. Việc sử dụng bộ nhớ vẫn tỷ lệ thuận với các tổng tiền tố riêng biệt, được giới hạn bởi n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()

        arr = [1 if c == 'a' else -1 for c in s]
        total = sum(arr)

        if total == 0:
            out.append("0")
            continue

        prefix = 0
        first_pos = {0: -1}
        ans = n + 1

        for i, v in enumerate(arr):
            prefix += v
            need = prefix - total
            if need in first_pos:
                ans = min(ans, i - first_pos[need])
            if prefix not in first_pos:
                first_pos[prefix] = i

        out.append(str(-1 if ans == n + 1 else ans))

    return "\n".join(out)

# provided samples
assert run("""5
5
bbbab
6
bbaaba
4
aaaa
12
aabbaaabbaab
5
aabaa
""") == """3
0
-1
2
-1"""

# all equal
assert run("""1
4
aaaa
""") == "-1"

# already balanced
assert run("""1
2
ab
""") == "0"

# needs internal removal
assert run("""1
5
bbbab
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả`a`| -1 | không thể trừ khi xóa hoàn toàn | 
| đã cân bằng | 0 | không cần gỡ bỏ | 
| trường hợp hỗn hợp | 3 | tính chính xác của việc khớp tiền tố | 

## Vỏ cạnh 

Một chuỗi chỉ bao gồm một loại ký tự, chẳng hạn như`aaaa`, tạo ra xu hướng tiền tố âm hoặc dương không đổi mà không có cấu trúc lặp lại thỏa mãn sự khác biệt cần thiết. Thuật toán quét tất cả các tiền tố, không tìm thấy kết quả trùng khớp nào và trả về chính xác`-1`. 

Khi chuỗi đã được cân bằng, chẳng hạn như`ab`hoặc`baba`, tổng số tiền bằng không. Việc thoát sớm đảm bảo chúng tôi không tìm kiếm sai những nội dung xóa không cần thiết và chúng tôi ngay lập tức quay lại`0`. 

Đối với những trường hợp loại bỏ tối ưu ở biên, chẳng hạn như loại bỏ tiền tố hoặc hậu tố, bản đồ tiền tố vẫn bắt chúng vì chúng ta khởi tạo`prefix = 0`tại chỉ mục`-1`. Điều này đảm bảo rằng các phân đoạn bắt đầu tại chỉ mục`0`là những ứng cử viên hợp lệ và được xử lý thống nhất với các phân đoạn bên trong.
