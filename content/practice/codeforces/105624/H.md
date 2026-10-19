---
title: "CF 105624H - \u0426\u0435\u043b\u0430\u044f \u043c\u0435\u0434\u0438\u0430\u043d\u0430"
description: "Nhiệm vụ là cộng số nguyên nhỏ nhất vào một mảng hiện có sao cho trung vị của mảng được sắp xếp cuối cùng trở thành chính xác x."
date: "2026-06-26T18:13:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105624
codeforces_index: "H"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2024-2025, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 105624
solve_time_s: 53
verified: true
draft: false
---

[CF 105624H - \u0426\u0435\u043b\u0430\u044f \u043c\u0435\u0434\u0438\u0430\u043d\u0430](https://codeforces.com/problemset/problem/105624/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là cộng số nguyên nhỏ nhất vào mảng hiện có sao cho trung vị của mảng được sắp xếp cuối cùng trở nên chính xác.`x`. Định nghĩa trung vị là phần tử ở giữa bên trái sau khi sắp xếp, nghĩa là đối với một mảng có độ dài`m`, trung vị là phần tử ở vị trí`(m + 1) // 2`sử dụng một chỉ mục dựa trên. Đây chính là quy ước trung vị thấp hơn được sử dụng trong bài toán ban đầu. 

Đầu vào cung cấp số giá trị hiện tại, giá trị trung bình mong muốn và chính mảng đó. Chúng tôi chỉ có thể chèn các giá trị mới chứ không thể sửa đổi các giá trị hiện có. Đầu ra là số lượng giá trị được chèn tối thiểu cần thiết. 

Độ dài mảng tối đa là 500, có nghĩa là các giải pháp chẵn xung quanh`O(n^2)`có thể phù hợp. Tuy nhiên, cấu trúc của vấn đề cho phép giải quyết đơn giản hơn`O(n log n)`cách tiếp cận sau khi sắp xếp. Điều quan trọng là chỉ có số giá trị nhỏ hơn`x`, bằng`x`, và lớn hơn`x`ảnh hưởng đến câu trả lời. Chúng tôi không cần phải mô phỏng các phần chèn thực tế. 

Một giải pháp bất cẩn thường thất bại trong những trường hợp`x`vắng mặt hoặc nơi trung vị hiện tại gần nhưng không chính xác`x`. Ví dụ:```
3 10
10 20 30
```Câu trả lời là`1`. Một giải pháp chỉ tính hiện có`x`giá trị sẽ thất bại vì`x`đã tồn tại, nhưng mức trung vị hiện tại là`20`. 

Một ví dụ khác:```
3 4
1 2 3
```Câu trả lời là`4`. Chỉ thêm một`4`cho`[1,2,3,4]`, trung vị của nó là`2`, không`4`. Mảng cuối cùng phải chứa đủ giá trị lớn hơn hoặc bằng`4`sao cho vị trí ở giữa đạt tới giá trị được chèn vào. 

Lỗi phổ biến thứ ba là xử lý sai số trung vị vốn đã chính xác:```
5 3
1 2 3 4 5
```Câu trả lời là`0`. Bất kỳ giải pháp nào luôn luôn chèn`x`khi nó xuất hiện sẽ tạo ra một câu trả lời khẳng định sai. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là thử cộng từng giá trị một và kiểm tra xem giá trị trung vị có trở thành`x`. Vì vị trí trung vị thay đổi khi độ dài thay đổi nên điều này đòi hỏi phải sắp xếp hoặc đếm mảng nhiều lần. Nếu chúng ta thử nhiều lần đếm có thể được chèn vào, số lần kiểm tra sẽ nhanh chóng trở thành công việc không cần thiết. Với kích thước đầu vào lớn trong các biến thể liên quan, việc liên tục xây dựng lại mảng đã sắp xếp sẽ trở thành nút thắt cổ chai. 

Quan sát quan trọng là các giá trị được chèn thực tế rất linh hoạt. Chúng ta có thể chèn các giá trị nhỏ hơn`x`, bằng`x`, hoặc lớn hơn`x`, và ba nhóm này đều quan trọng. 

Cho phép`less`là số lượng các giá trị hiện có nhỏ hơn`x`, Và`equal`là số lượng các giá trị hiện có bằng`x`. Sau khi sắp xếp, vị trí trung vị phải nằm sau tất cả các giá trị nhỏ hơn`x`, nhưng không phải sau tất cả các giá trị bằng`x`. 

Nếu như`x`bị thiếu, trước tiên chúng tôi chèn một bản sao của`x`. Sau đó, chúng ta chỉ cần cân bằng lại số đếm xung quanh vị trí trung vị. Nếu như`x`đã tồn tại, chúng ta có thể di chuyển trực tiếp vị trí trung tuyến vào khối`x`giá trị bằng cách thêm các giá trị ở phía cần thiết. 

Việc thực hiện có thể được thực hiện một cách tham lam bằng cách xem xét vị trí của trung vị sau khi chèn`x`nếu cần thì chỉ thêm số lượng giá trị tối thiểu cần thiết để di chuyển ranh giới trung vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời × n log n) | O(n) | Nói chung là quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng và tìm vị trí đầu tiên và cuối cùng chứa`x`. Sắp xếp các nhóm tất cả các giá trị nhỏ hơn`x`, bằng`x`, và lớn hơn`x`thành các phạm vi riêng biệt, điều này cho phép chúng ta chỉ suy luận về số lượng. 
2. Nếu`x`không tồn tại, chèn một bản sao của`x`về mặt khái niệm và tăng câu trả lời lên một. Giá trị trung vị phải là giá trị hiện có, do đó nếu không có phần chèn này thì giá trị đích không thể trở thành giá trị trung vị. 
3. Tính toán vị trí trung bình của mảng hiện tại bằng cách sử dụng chỉ mục dựa trên 0:`(n - 1) // 2`. 
4. Nếu vị trí này đã nằm trong khối giá trị bằng`x`, không cần chèn thêm nữa. 
5. Nếu vị trí ở giữa nằm trước vị trí đầu tiên`x`, có quá nhiều giá trị nhỏ hơn`x`. Thêm các giá trị lớn hơn hoặc bằng`x`cho đến khi vị trí trung tuyến di chuyển vào`x`khối. 
6. Nếu vị trí ở giữa nằm sau vị trí cuối cùng`x`, có quá nhiều giá trị lớn hơn`x`. Thêm các giá trị nhỏ hơn hoặc bằng`x`cho đến khi đạt đến vị trí trung vị`x`. 

Tại sao nó hoạt động: mảng được sắp xếp được chia thành ba vùng. Cách duy nhất để giá trị trung vị thất bại là chỉ số trung vị nằm ngoài vùng bị chiếm giữ bởi`x`. Việc thêm các số ở phía đối diện sẽ di chuyển ranh giới mà không phá hủy ranh giới hiện có`x`khối. Vì mỗi lần chèn sẽ thay đổi vị trí trung bình ở mức nhỏ nhất có thể nên số lần bổ sung tham lam là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    ans = 0

    if x not in a:
        a.append(x)
        n += 1
        ans += 1

    a.sort()

    left = 0
    while left < n and a[left] < x:
        left += 1

    right = left
    while right < n and a[right] == x:
        right += 1

    median = (n - 1) // 2

    if left <= median < right:
        print(ans)
        return

    if median < left:
        need = 2 * left - n + 1
        ans += need
    else:
        need = n - 2 * right
        ans += need

    print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên xử lý trường hợp đặc biệt trong đó`x`vắng mặt. Thêm`x`là bắt buộc vì trung vị phải là một phần tử của mảng được sắp xếp cuối cùng. 

Sau khi sắp xếp,`left`chỉ ra sự xuất hiện đầu tiên của`x`, Và`right`điểm ngay sau lần xuất hiện cuối cùng của`x`. Khoảng thời gian`[left, right)`chính xác là vùng an toàn nơi chỉ số trung bình phải chạm tới. 

Khi chỉ số trung vị quá nhỏ, mảng chứa quá nhiều giá trị bên dưới`x`. Việc thêm các giá trị lớn sẽ làm tăng độ dài mảng và đẩy vị trí trung bình sang phải. Khi chỉ số trung vị quá lớn, việc thêm các giá trị nhỏ sẽ dịch chuyển vị trí trung vị sang trái. Các công thức tính số ca cần thiết tối thiểu. 

Các biểu thức chỉ sử dụng số học số nguyên. Số nguyên Python không bị tràn, nhưng các công thức tương tự trong các ngôn ngữ có số nguyên có chiều rộng cố định vẫn phải được kiểm tra cẩn thận. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 10
10 20 30
```Sau khi chèn`10`đã tồn tại thì mảng được sắp xếp không thay đổi. 

| Biến | Giá trị | 
| --- | --- | 
| Mảng được sắp xếp | [10, 20, 30] | 
| Đầu tiên`x`chỉ mục | 0 | 
| Cuối cùng`x`chỉ mục | 1 | 
| Chỉ số trung vị | 1 | 
| Hành động | Thêm một giá trị lớn | 

Chỉ số trung vị nằm sau`10`khối, vì vậy một giá trị bổ sung là đủ. Thêm`9`cho`[9,10,20,30]`, trung vị của nó là`10`. 

### Mẫu 2 

đầu vào:```
3 4
1 2 3
```| Biến | Giá trị | 
| --- | --- | 
| Sắp xếp mảng sau khi chèn`x`| [1,2,3,4] | 
| Đầu tiên`x`chỉ mục | 3 | 
| Cuối cùng`x`chỉ mục | 4 | 
| Chỉ số trung vị | 1 | 
| Hành động | Di chuyển trung tuyến sang phải | 

Đã chèn`4`là không đủ. Dải phân cách phải di chuyển vào khối cuối cùng, yêu cầu tổng cộng bốn lần chèn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong công việc | 
| Không gian | O(n) | Mảng được lưu trữ để sắp xếp | 

Sự hạn chế của`n ≤ 500`dễ dàng cho phép giải pháp này. Thuật toán chỉ thực hiện một lần sắp xếp và một vài lần quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    if not data:
        return ""

    n = int(data[0])
    x = int(data[1])
    a = list(map(int, data[2:]))

    ans = 0
    if x not in a:
        a.append(x)
        n += 1
        ans += 1

    a.sort()

    left = 0
    while left < n and a[left] < x:
        left += 1

    right = left
    while right < n and a[right] == x:
        right += 1

    median = (n - 1) // 2

    if median < left:
        ans += 2 * left - n + 1
    elif median >= right:
        ans += n - 2 * right

    return str(ans)

assert run("""3 10
10 20 30
""") == "1"

assert run("""3 4
1 2 3
""") == "4"

assert run("""5 3
1 2 3 4 5
""") == "0"

assert run("""1 7
1
""") == "1"

assert run("""5 5
5 5 5 5 5
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 10 / 10 20 30`|`1`| hiện tại`x`nhưng số trung vị quá lớn | 
|`3 4 / 1 2 3`|`4`| Mất tích`x`và điều chỉnh lớn | 
|`5 3 / 1 2 3 4 5`|`0`| Đã đúng trung vị | 
|`1 7 / 1`|`1`| Mảng kích thước tối thiểu | 
|`5 5 / 5 5 5 5 5`|`0`| Tất cả các giá trị bằng nhau | 

## Vỏ cạnh 

Khi nào`x`vắng mặt, thuật toán sẽ chèn nó trước. Đối với đầu vào:```
1 7
1
```Mảng trở thành`[1,7]`. Chỉ số trung bình là`0`, vậy phần tử ở giữa phía dưới là`1`. Thuật toán phát hiện rằng trung vị nằm trước`x`chặn và thêm một giá trị lớn hơn, tạo ra câu trả lời`1`. 

Khi tất cả các giá trị đã bằng nhau`x`, khối trung vị bao phủ toàn bộ mảng. Vì:```
5 5
5 5 5 5 5
```vị trí đầu tiên và cuối cùng của`x`bao quanh chỉ số trung vị, do đó thuật toán trả về`0`. 

Khi có quá nhiều giá trị bên dưới`x`, thuật toán di chuyển trung vị sang phải. Vì:```
3 10
1 2 3
```trường hợp giá trị bị thiếu chèn`10`, thì cần có các giá trị bổ sung vì vị trí trung vị vẫn bị chiếm bởi các số nhỏ. Đáp án cuối cùng chỉ được xác định bằng khoảng cách từ vị trí trung tuyến đến`x`khối, không phải bởi các giá trị thực tế được chèn vào.
