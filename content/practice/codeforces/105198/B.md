---
title: "CF 105198B - 21---0?"
description: "Chúng ta được cung cấp một chuỗi số lần giải quyết hàng ngày một cách gián tiếp. Mảng ban đầu chứa số lượng vấn đề đã được giải quyết mỗi ngày nhưng chỉ cung cấp tổng số ngày gần đây."
date: "2026-06-27T02:56:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105198
codeforces_index: "B"
codeforces_contest_name: "ShellBeeHaken Presents Intra SUST Programming Contest 2024 - Replay"
rating: 0
weight: 105198
solve_time_s: 57
verified: true
draft: false
---

[CF 105198B - 21---0?](https://codeforces.com/problemset/problem/105198/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi số lần giải quyết hàng ngày một cách gián tiếp. Mảng ban đầu chứa số lượng vấn đề đã được giải quyết mỗi ngày nhưng chỉ cung cấp tổng số ngày gần đây. Lần đầu tiên`k`ngày, giá trị biểu thị tổng số vấn đề được giải quyết từ đầu tháng cho đến ngày đó. Sau đó, mỗi giá trị đại diện cho tổng chính xác của giá trị trước đó.`k`ngày kết thúc vào ngày đó. 

Nhiệm vụ là khôi phục đủ thông tin về số lần đếm hàng ngày ban đầu để tìm ra khoảng thời gian liên tiếp dài nhất mà mỗi ngày đều có số lần giải quyết dương. 

Các ràng buộc buộc chúng ta hướng tới một giải pháp tuyến tính. Tổng số ngày của tất cả các trường hợp thử nghiệm nhiều nhất là`3 * 10^5`, vì vậy một`O(n)`hoặc`O(n log n)`phương pháp có tính thực tế. Một giải pháp kiểm tra mọi khoảng thời gian có thể hoặc tái tạo lại các khoản tiền nhiều lần sẽ yêu cầu khoảng`O(n^2)`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 2 giây cho phép. 

Một số trường hợp nguy hiểm có thể phá vỡ việc triển khai bất cẩn. Khi`k = n`, toàn bộ mảng chỉ là tổng tiền tố, vì vậy việc xử lý mọi giá trị như tổng cửa sổ trượt thông thường sẽ tạo ra sự khác biệt không chính xác. Ví dụ:```
Input:
1
3 3
1 2 2
```Những ngày ban đầu là`[1, 1, 0]`, vậy câu trả lời là`2`. Cách tiếp cận chỉ có cửa sổ trượt sai có thể thất bại vì không có cửa sổ đầy đủ sau lần đầu tiên`k`ngày. 

Một trường hợp phức tạp khác là khi tổng trượt giảm. Mức giảm không trực tiếp có nghĩa là ngày hiện tại không có giải pháp nào, bởi vì một ngày tích cực cũ có thể bị loại bỏ. Ví dụ:```
Input:
1
5 2
1 2 1 0 0
```Những ngày ban đầu là`[1, 1, 0, 0, 0]`, vậy câu trả lời là`2`. Chỉ nhìn vào việc các giá trị liên tiếp tăng hay giảm có thể phân loại ngày không chính xác. 

Trường hợp thứ ba là khi tất cả các ngày đều dương:```
Input:
1
5 2
1 2 2 2 2
```Câu trả lời là`5`. Một phương pháp tìm kiếm các thay đổi trong mảng đã cho thay vì xây dựng lại số lượng hàng ngày sẽ dừng chuỗi sớm một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng lại số liệu hàng ngày ban đầu. Đặt mảng ẩn là`x`. Lần đầu tiên`k`ngày, các giá trị đã cho là tổng tiền tố, do đó có thể tìm thấy mỗi ngày bằng cách trừ đi tổng tiền tố trước đó. Sau ngày`k`, giá trị hiện tại là tổng cửa sổ. Xóa cửa sổ trước đó và thêm ngày mới sẽ mang lại mối quan hệ:`a[i] - a[i-1] = x[i] - x[i-k]`Vì thế:`x[i] = a[i] - a[i-1] + x[i-k]`Điều này cho phép chúng tôi tính toán từng giá trị ẩn trong thời gian không đổi. 

Giải pháp brute-force trước tiên có thể xây dựng lại toàn bộ mảng và sau đó kiểm tra mọi chuỗi có thể bằng cách mở rộng từ mỗi ngày bắt đầu. Điều này đúng vì mọi khoảng thời gian có thể đều được kiểm tra, nhưng trong trường hợp xấu nhất nó thực hiện`O(n^2)`công việc. Với`n = 300000`, điều đó có thể có nghĩa là khoảng 90 tỷ hoạt động. 

Quan sát cho thấy mỗi ngày ẩn chỉ phụ thuộc vào giá trị`k`vị trí trước đó thay đổi hoàn toàn vấn đề. Chúng ta không cần phải tìm kiếm nhiều lần. Trong khi xây dựng lại ngày, chúng ta có thể duy trì độ dài chuỗi hiện tại. Bất cứ khi nào giá trị được phục hồi là dương, vệt sẽ tăng lên. Bất cứ khi nào nó bằng 0, chuỗi kết thúc và chúng tôi cập nhật câu trả lời tốt nhất. Vì mỗi ngày được xử lý một lần nên toàn bộ vấn đề trở thành tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`days`sẽ lưu trữ số lượng bài toán đã được giải quyết đã được phục hồi mỗi ngày. Sự lặp lại cho những ngày sau đó cần giá trị từ`k`ngày trước đó, vì vậy việc giữ nguyên các giá trị trước đó sẽ giúp quá trình chuyển đổi có thể thực hiện được. 
2. Khôi phục lần đầu`min(n, k)`ngày sử dụng tổng tiền tố. Ngày đầu tiên bằng`a[0]`. Mỗi ngày sau đó ở khu vực tiền tố này là`a[i] - a[i-1]`. Các giá trị này đại diện cho số lần giải thực tế hàng ngày vì các giá trị đã cho là tổng tích lũy. 
3. Cho mỗi ngày sau ngày đầu tiên`k`ngày, tính giá trị ẩn bằng cách sử dụng`a[i] - a[i-1] + days[i-k]`. Phép trừ thể hiện ngày xưa rời khỏi cửa sổ và thêm`days[i-k]`khôi phục phần đóng góp đã bị xóa. 
4. Trong khi phục hồi mỗi ngày, hãy cập nhật chuỗi hiện tại. Nếu giá trị được khôi phục lớn hơn 0, hãy tăng chuỗi hiện tại và cập nhật mức tối đa. Nếu nó bằng 0, hãy đặt lại chuỗi hiện tại vì ngày không giải được sẽ phá vỡ quá trình giải liên tiếp. 
5. In vệt tối đa được tìm thấy sau tất cả các ngày đã được xử lý. 

Tại sao nó hoạt động: công thức xây dựng lại tuân theo chính xác định nghĩa của từng cửa sổ trượt. Lần đầu tiên`k`ngày, sự khác biệt về tiền tố chỉ đưa ra các giá trị hàng ngày có thể có. Đối với những ngày sau đó, sự khác biệt giữa hai tổng cửa sổ liền kề cho chúng ta biết sự thay đổi do loại bỏ một ngày cũ và thêm một ngày mới. Vì mảng được đảm bảo là hợp lệ nên các giá trị được xây dựng lại chính xác là số lượng hàng ngày ban đầu. Sau đó, phép tính chuỗi sẽ đo trực tiếp chuỗi giá trị dương dài nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        days = [0] * n
        best = 0
        cur = 0

        for i in range(n):
            if i == 0:
                days[i] = a[i]
            elif i < k:
                days[i] = a[i] - a[i - 1]
            else:
                days[i] = a[i] - a[i - 1] + days[i - k]

            if days[i] > 0:
                cur += 1
                if cur > best:
                    best = cur
            else:
                cur = 0

        ans.append(str(best))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mảng`days`lưu trữ số lượng giải quyết hàng ngày được xây dựng lại. Nó được lập chỉ mục từ số 0, vì vậy`days[i-k]`là ngày rời khỏi cửa sổ trượt khi tính ngày`i`. 

Nhánh đầu tiên xử lý ngày 0 một cách riêng biệt vì không có tổng tiền tố trước đó. Nhánh thứ hai bao gồm phần tổng tiền tố của đầu vào, trong đó phép trừ các giá trị liền kề sẽ cho ra số đếm chính xác hàng ngày. Nhánh cuối cùng sử dụng phép truy toán cửa sổ trượt. 

Các số nguyên Python được sử dụng tự động, do đó các giá trị lớn có thể có của`a[i]`không tạo ra vấn đề tràn. Thứ tự của các thao tác trong phép toán lặp lại cũng rất quan trọng: sự khác biệt của khoảng thời gian trước đó phải được sửa bằng cách cộng lại phần đóng góp của ngày đi. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
n = 5, k = 2
a = [1, 2, 1, 4, 5]
```| Ngày | Công thức được sử dụng | Đã giải quyết được | Chuỗi hiện tại | Tối đa | 
| --- | --- | --- | --- | --- | 
| 1 |`a[0]`| 1 | 1 | 1 | 
| 2 |`a[1]-a[0]`| 1 | 2 | 2 | 
| 3 |`a[2]-a[1]+days[0]`| 0 | 0 | 2 | 
| 4 |`a[3]-a[2]+days[1]`| 4 | 1 | 2 | 
| 5 |`a[4]-a[3]+days[2]`| 1 | 2 | 2 | 

Việc tái thiết mang lại`[1, 1, 0, 4, 1]`. Số 0 ở ngày thứ ba phá vỡ chuỗi đầu tiên, để lại câu trả lời là`2`. 

Đối với mẫu thứ ba:```
n = 5, k = 2
a = [1, 2, 2, 2, 2]
```| Ngày | Công thức được sử dụng | Đã giải quyết được | Chuỗi hiện tại | Tối đa | 
| --- | --- | --- | --- | --- | 
| 1 |`a[0]`| 1 | 1 | 1 | 
| 2 |`a[1]-a[0]`| 1 | 2 | 2 | 
| 3 |`a[2]-a[1]+days[0]`| 1 | 3 | 3 | 
| 4 |`a[3]-a[2]+days[1]`| 1 | 4 | 4 | 
| 5 |`a[4]-a[3]+days[2]`| 1 | 5 | 5 | 

Mọi giá trị được phục hồi đều dương, do đó chuỗi kéo dài cả tháng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ngày được xây dựng lại và xử lý một lần. | 
| Không gian | O(n) | Mảng đã khôi phục được lưu trữ vì những ngày trong tương lai cần giá trị từ`k`các vị trí trước đó. | 

Tổng cộng`n`trên tất cả các trường hợp thử nghiệm được giới hạn bởi`3 * 10^5`, do đó, đường chuyền tuyến tính dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []

        for _ in range(t):
            n, k = map(int, input().split())
            a = list(map(int, input().split()))

            days = [0] * n
            cur = 0
            best = 0

            for i in range(n):
                if i == 0:
                    days[i] = a[i]
                elif i < k:
                    days[i] = a[i] - a[i - 1]
                else:
                    days[i] = a[i] - a[i - 1] + days[i - k]

                if days[i] > 0:
                    cur += 1
                    best = max(best, cur)
                else:
                    cur = 0

            out.append(str(best))

        return "\n".join(out)

    result = solve()
    sys.stdin = old_stdin
    return result

assert run("""3
2 2
1 2
5 2
1 2 1 4 5
5 2
1 2 2 2 2
""") == "2\n2\n5", "samples"

assert run("""1
2 1
5 0
""") == "1", "minimum k"

assert run("""1
3 3
1 2 2
""") == "2", "full prefix case"

assert run("""1
5 2
1 2 1 0 0
""") == "2", "decreasing windows"

assert run("""1
6 3
0 0 0 0 0 0
""") == "0", "all zero days"

assert run("""1
6 6
1 3 6 6 7 9
""") == "3", "large k boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 / 1 2`|`2`| Tháng nhỏ nhất và tái thiết tiền tố bình thường | 
|`3 3 / 1 2 2`|`2`| Trường hợp cả tháng là tổng tiền tố | 
|`5 2 / 1 2 1 0 0`|`2`| Cửa sổ trượt giảm dần | 
|`6 3 / 0 0 0 0 0 0`|`0`| Không có ngày tích cực tồn tại | 
|`6 6 / 1 3 6 6 7 9`|`3`| Lớn`k`và ranh giới tiền tố | 

## Vỏ cạnh 

Khi nào`k = n`, mọi giá trị đều là tổng tiền tố. Vì:```
1
3 3
1 2 2
```thuật toán chỉ sử dụng nhánh tái tạo tiền tố. Nó có được ngày`[1, 1, 0]`, cập nhật chuỗi thành`2`, sau đó đặt lại sau số 0. Đầu ra là`2`. 

Khi cửa sổ giảm đi, thuật toán không cho rằng ngày mới bằng 0. Vì:```
1
5 2
1 2 1 0 0
```số ngày hồi phục là`[1, 1, 0, 0, 0]`. Sự tái diễn giải thích đúng việc ngày xưa bỏ cửa sổ nên đáp án vẫn còn`2`. 

Khi mỗi ngày đều tích cực thì không cần thiết lập lại. Vì:```
1
5 2
1 2 2 2 2
```mỗi ngày được xây dựng lại bằng`1`, do đó chuỗi hiện tại phát triển từ`1`ĐẾN`5`và câu trả lời cuối cùng là`5`. Thuật toán xử lý việc này vì nó kiểm tra các giá trị hàng ngày được xây dựng lại thay vì các mẫu trong mảng đầu vào.
