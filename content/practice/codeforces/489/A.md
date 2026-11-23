---
title: "CF 489A - Sắp xếp hoán đổi"
description: "Chúng ta được cấp một mảng các số nguyên và phải biến nó thành thứ tự không giảm bằng cách thực hiện hoán đổi vị trí mảng. Điều thú vị là chúng ta không cần số lần hoán đổi tối thiểu. Bất kỳ chuỗi hợp lệ nào chứa tối đa n lần hoán đổi đều được chấp nhận."
date: "2026-06-07T17:35:50+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 489
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 277.5 (Div. 2)"
rating: 1200
weight: 489
solve_time_s: 154
verified: false
draft: false
---

[CF 489A - SwapSort](https://codeforces.com/problemset/problem/489/A) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên và phải biến nó thành thứ tự không giảm bằng cách thực hiện hoán đổi vị trí mảng. Điều thú vị là chúng ta không cần số lần hoán đổi tối thiểu. Bất kỳ chuỗi hợp lệ nào chứa nhiều nhất`n`trao đổi được chấp nhận. 

Đầu ra bao gồm các hoạt động trao đổi. Sau khi áp dụng chúng theo thứ tự in, mảng phải được sắp xếp. 

Độ dài mảng nhiều nhất là 3000. Độ dài này đủ nhỏ để`O(n²)`giải pháp là hoàn toàn chấp nhận được vì`3000² = 9,000,000`hoạt động, dễ dàng phù hợp trong giới hạn. Không cần cấu trúc dữ liệu phức tạp hoặc`O(n log n)`kỹ thuật. 

Thách thức chính đến từ các giá trị trùng lặp. Nếu tất cả các giá trị đều khác biệt, chúng ta có thể chỉ cần ánh xạ từng giá trị tới vị trí đích của nó trong mảng được sắp xếp. Với các bản sao, một số vị trí có thể chứa cùng một giá trị, do đó việc ánh xạ bất cẩn có thể gán một lần xuất hiện cho nhiều vị trí mục tiêu. 

Coi như:```
3
2 1 2
```Mảng được sắp xếp là:```
1 2 2
```Nếu chúng ta chỉ ánh xạ giá trị`2`đến "vị trí của nó trong mảng được sắp xếp", có hai vị trí hợp lệ. Việc coi chúng là có thể hoán đổi cho nhau mà không theo dõi các lần xuất hiện riêng lẻ có thể tạo ra các giao dịch hoán đổi không chính xác. 

Một trường hợp cạnh khác là một mảng đã được sắp xếp:```
4
1 2 3 4
```Câu trả lời đúng là:```
0
```Một số triển khai thực hiện hoán đổi một cách không cần thiết mặc dù không cần thiết. 

Trường hợp thứ ba là khi tất cả các phần tử đều bằng nhau:```
5
7 7 7 7 7
```Một lần nữa, câu trả lời không được có giao dịch hoán đổi. Bất kỳ logic nào chỉ dựa trên so sánh giá trị đều phải tránh phát minh ra các chuyển động cho các phần tử giống hệt nhau. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là liên tục tìm phần tử nhỏ nhất sẽ xuất hiện ở mỗi vị trí và hoán đổi nó vào vị trí. Đối với vị trí`i`, chúng tôi quét hậu tố`i...n-1`, tìm phần tử chính xác và thực hiện một lần hoán đổi nếu cần. 

Cách làm này đúng vì sau khi cố định vị trí`i`, vị trí đó không bao giờ thay đổi nữa. Thật không may, nó đòi hỏi một`O(n)`quét từng cái`n`các vị trí, dẫn đến`O(n²)`thời gian. 

Điều thú vị là,`O(n²)`ở đây đã đủ nhanh rồi. Quan sát thực sự không phải là về việc cải thiện độ phức tạp tiệm cận mà là về việc tạo ra nhiều nhất`n`hoán đổi trong khi xử lý các giá trị trùng lặp một cách rõ ràng. 

Ý tưởng chính là sắp xếp một bản sao của mảng và duy trì vị trí hiện tại của mọi phần tử. Khi vị trí`i`chưa chứa giá trị cuối cùng của nó, chúng ta xác định vị trí chứa giá trị lẽ ra phải ở đây và hoán đổi hai vị trí. 

Mỗi trao đổi cố định vĩnh viễn một vị trí. Vì chỉ có`n`vị trí, tổng số lần hoán đổi nhiều nhất`n-1`. 

Để làm cho việc định vị các phần tử trở nên dễ dàng, chúng ta sắp xếp các cặp`(value, original_index)`. Điều này mang lại cho mỗi lần xuất hiện của một giá trị trùng lặp một danh tính duy nhất. Khi đó mỗi phần tử sẽ biết chính xác vị trí cuối cùng mà nó thuộc về. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi phần tử cùng với chỉ mục ban đầu của nó dưới dạng`(value, index)`. 
2. Sắp xếp các cặp này theo giá trị. Sau khi sắp xếp, xác định vị trí`i`của mảng này đại diện cho phần tử cuối cùng sẽ chiếm vị trí`i`theo thứ tự sắp xếp. 
3. Tạo một mảng`pos`Ở đâu`pos[original_index]`lưu trữ vị trí hiện tại của phần tử đó trong mảng làm việc. 
4. Tạo một mảng khác`cur`Ở đâu`cur[position]`lưu trữ phần tử gốc nào hiện đang chiếm giữ vị trí đó. 
5. Xử lý các vị trí từ trái qua phải. 
6. Tại vị trí`i`, kiểm tra xem phần tử chính xác đã có ở đó chưa. Phần tử đúng là phần tử có chỉ mục ban đầu xuất hiện trong danh sách cặp được sắp xếp tại vị trí`i`. 
7. Nếu phần tử chính xác đã ở vị trí`i`, Tiếp tục. 
8. Nếu không, hãy tìm vị trí hiện tại của nó bằng cách sử dụng`pos`. 
9. Hoán đổi hai vị trí trong`cur`, cập nhật các mục của họ trong`pos`, và ghi lại sự hoán đổi. 
10. Tiếp tục cho đến khi mỗi vị trí chứa phần tử được chỉ định. 

Mỗi lần hoán đổi sẽ đặt phần tử chính xác vào vị trí cuối cùng của nó. Vị trí đó không bao giờ thay đổi nữa. 

### Tại sao nó hoạt động 

Danh sách được sắp xếp của`(value, original_index)`các cặp chỉ định mỗi lần xuất hiện của mỗi giá trị một đích duy nhất. Ngay cả những bản sao cũng có thể phân biệt được vì chỉ số ban đầu của chúng khác nhau. 

Ở bước`i`, nếu phần tử chính xác chưa có mặt, chúng tôi sẽ di chuyển chính xác phần tử đó vào vị trí`i`. Sau khi hoán đổi, vị trí`i`chứa phần tử được chỉ định cho nó theo thứ tự được sắp xếp. 

Vì các thao tác sau này chỉ ảnh hưởng đến các vị trí lớn hơn hoặc bằng`i+1`, chức vụ`i`vẫn đúng mãi mãi. Việc lặp lại đối số này cho mọi vị trí cuối cùng sẽ tạo ra mảng được sắp xếp đầy đủ. 

Bởi vì mỗi lần hoán đổi cố định ít nhất một vị trí mới nên số lần hoán đổi không bao giờ vượt quá`n-1`, thỏa mãn yêu cầu bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    sorted_pairs = sorted((a[i], i) for i in range(n))

    cur = list(range(n))
    pos = list(range(n))

    swaps = []

    for i in range(n):
        target_id = sorted_pairs[i][1]

        if cur[i] == target_id:
            continue

        j = pos[target_id]

        swaps.append((i, j))

        x = cur[i]
        y = cur[j]

        cur[i], cur[j] = cur[j], cur[i]
        pos[x], pos[y] = pos[y], pos[x]

    print(len(swaps))
    for i, j in swaps:
        print(i, j)

solve()
```Giải pháp không bao giờ sắp xếp lại trực tiếp các giá trị. Thay vào đó, nó sắp xếp lại các mã định danh đại diện cho vị trí ban đầu của các phần tử. 

Mảng`sorted_pairs`xác định vị trí của mỗi phần tử gốc theo thứ tự sắp xếp cuối cùng. Mảng`cur`cho chúng ta biết phần tử gốc nào hiện đang chiếm giữ từng vị trí, trong khi`pos`cung cấp ánh xạ nghịch đảo. 

Khi hoán đổi xảy ra, cả hai cấu trúc phải được cập nhật một cách nhất quán. Cũng quên cập nhật`cur`hoặc`pos`là lỗi triển khai phổ biến nhất và ngay lập tức phá vỡ các lần tra cứu trong tương lai. 

Thuật toán cố định vị trí từ trái sang phải. Khi một vị trí đã được cố định, nó sẽ không bao giờ được chạm vào nữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
5 2 5 1 4
```Thứ tự sắp xếp của các cặp:```
(1,3) (2,1) (4,4) (5,0) (5,2)
```| Bước | tôi | Chỉ mục gốc bắt buộc | Vị trí hiện tại | Hoán đổi | cur sau khi trao đổi | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | - | - | [0,1,2,3,4] | 
| 1 | 0 | 3 | 3 | (0,3) | [3,1,2,0,4] | 
| 2 | 1 | 1 | 1 | không | [3,1,2,0,4] | 
| 3 | 2 | 4 | 4 | (2,4) | [3,1,4,0,2] | 
| 4 | 3 | 0 | 3 | không | [3,1,4,0,2] | 
| 5 | 4 | 2 | 4 | không | [3,1,4,0,2] | 

Đầu ra:```
2
0 3
2 4
```Ví dụ này cho thấy mỗi lần hoán đổi cố định vĩnh viễn một vị trí như thế nào. Sau lần hoán đổi đầu tiên, vị trí 0 đã chứa phần tử nhỏ nhất và không bao giờ thay đổi nữa. 

### Ví dụ 2 

đầu vào:```
4
4 3 2 1
```Thứ tự sắp xếp của các cặp:```
(1,3) (2,2) (3,1) (4,0)
```| Bước | tôi | Chỉ mục gốc bắt buộc | Vị trí hiện tại | Hoán đổi | cur sau khi trao đổi | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | - | - | [0,1,2,3] | 
| 1 | 0 | 3 | 3 | (0,3) | [3,1,2,0] | 
| 2 | 1 | 2 | 2 | (1,2) | [3,2,1,0] | 
| 3 | 2 | 1 | 2 | không | [3,2,1,0] | 
| 4 | 3 | 0 | 3 | không | [3,2,1,0] | 

Đầu ra:```
2
0 3
1 2
```Dấu vết này chứng tỏ rằng một lần hoán đổi có thể sửa chữa nhiều phần tử cùng một lúc, đó là lý do tại sao tổng số lần hoán đổi luôn ở mức dưới đây`n`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Chi phí sắp xếp O(n log n), vòng lặp cố định là O(n) và tất cả các hoạt động đều có thời gian không đổi. Phân loại biên tập được chấp nhận là O(n²) hoặc cao hơn. | 
| Không gian | O(n) | Mảng`cur`,`pos`và danh sách trao đổi lưu trữ thông tin tuyến tính. | 

Với`n ≤ 3000`, cả thời gian và mức sử dụng bộ nhớ đều nằm trong giới hạn thoải mái. Thậm chí vài triệu thao tác được thực thi nhanh chóng trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    def solve():
        input = sys.stdin.readline

        n = int(input())
        a = list(map(int, input().split()))

        sorted_pairs = sorted((a[i], i) for i in range(n))

        cur = list(range(n))
        pos = list(range(n))
        swaps = []

        for i in range(n):
            target_id = sorted_pairs[i][1]

            if cur[i] == target_id:
                continue

            j = pos[target_id]

            swaps.append((i, j))

            x = cur[i]
            y = cur[j]

            cur[i], cur[j] = cur[j], cur[i]
            pos[x], pos[y] = pos[y], pos[x]

        print(len(swaps))
        for x, y in swaps:
            print(x, y)

    solve()

    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided sample
assert run("5\n5 2 5 1 4\n").splitlines()[0] == "2"

# minimum size
assert run("1\n42\n") == "0"

# already sorted
assert run("4\n1 2 3 4\n") == "0"

# all equal
assert run("5\n7 7 7 7 7\n") == "0"

# duplicates
res = run("3\n2 1 2\n")
assert int(res.splitlines()[0]) <= 3

# reverse order
res = run("4\n4 3 2 1\n")
assert int(res.splitlines()[0]) == 2
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 42`|`0`hoán đổi | Kích thước mảng tối thiểu | 
|`1 2 3 4`|`0`hoán đổi | Đã sắp xếp đầu vào | 
|`7 7 7 7 7`|`0`hoán đổi | Xử lý trùng lặp | 
|`2 1 2`| Câu trả lời hợp lệ | Xử lý đúng các giá trị lặp lại | 
|`4 3 2 1`|`2`hoán đổi | Chu kỳ hoán vị điển hình | 

## Vỏ cạnh 

Hãy xem xét trường hợp giá trị trùng lặp:```
3
2 1 2
```Danh sách cặp được sắp xếp trở thành:```
(1,1) (2,0) (2,2)
```Hai lần xuất hiện của giá trị`2`vẫn có thể phân biệt được vì chỉ số ban đầu của chúng là khác nhau. Thuật toán gán một bản sao cho vị trí 1 và bản sao còn lại cho vị trí 2. Không có sự mơ hồ nào tồn tại, do đó mảng kết quả được sắp xếp chính xác. 

Hãy xem xét một mảng đã được sắp xếp:```
4
1 2 3 4
```Danh sách cặp được sắp xếp phù hợp với cách sắp xếp hiện tại. Đối với mọi vị trí`i`,`cur[i]`đã bằng chỉ số ban đầu được yêu cầu. Không có giao dịch hoán đổi nào được ghi lại và đầu ra chỉ đơn giản là:```
0
```Xem xét tất cả các yếu tố bằng nhau:```
5
7 7 7 7 7
```Sắp xếp`(value, index)`các cặp giữ nguyên thứ tự của chúng vì các chỉ số là khác nhau. Mọi vị trí đều đã chứa phần tử được chỉ định của nó, do đó, không có sự hoán đổi nào xảy ra. Thuật toán tự nhiên đưa ra các hoạt động bằng 0. 

Những trường hợp này minh họa tại sao việc gán cho mỗi lần xuất hiện một danh tính duy nhất thông qua chỉ mục ban đầu của nó là chi tiết quan trọng giúp giải pháp trở nên chính xác.
